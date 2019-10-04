---
theme: "none"
customTheme: "presentation_kit\\lib\\azusaish"
transition: "none"
separator: "^\n\r\n\r\n\r|^\n\n\n"
verticalSeparator: "^\n\r\n\r|^\n\n"

---

# SDのJenkins移行のBefore After



## きっかけ

- 2019年5月31日ごろから、SDのJenkinsサーバ(ci-01)のsmokeジョブがたびたび失敗していた
    - https://raccoon-co.slack.com/archives/C7Z2QQECE/p1559269909018800
- 原因はci-01のOSが古い



## 要件

- ci-01サーバのJenkinsのジョブを新sd-ci01サーバに移行する
  - 移行元 http://ci-01:8080/
  - 移行先 http://sd-ci01:8080/



## Before After



## インフラ、アプリ
| |ci-01|sd-ci01|
|--|--|--|
|OS|CentOS 6.3|Ubuntu 18.04.2|
|Jenkins|1.544|2.176.1|
|ブラウザ|Firefox|Chromium|
|ブラウザ操作|Selenium Web Driver|puppeteer|
|言語|Selenium IDE, HTML|Node.js v10.16|
|testing framework|Selenium Test Suite|Mocha|


- Ubuntuを採用したのは、CentOS6でpuppeteerでChromiumを起動できなかったから。



### Selenium IDE, HTML
```HTML
<tr>
	<td>open</td>
	<td>/</td>
	<td></td>
</tr>
<tr>
	<td>clickAndWait</td>
	<td>link=ログイン</td>
	<td></td>
</tr>
```



### Node.js
```JavaScript
    // ログイン前のTOPページ表示
    await page.goto(common.SD_URL, {timeout: 2*MINUTE, waitUntil: 'domcontentloaded'});

    // ログインリンクをクリックして画面遷移を待つ
    await Promise.all([
      page.click('a[href="/p/do/clickMemberLogin"]'),
      page.waitForNavigation({timeout: 2*MINUTE, waitUntil: "domcontentloaded"})
    ]);
```



## その他
| |ci-01|sd-ci01|
|--|--|--|
|ジョブ設定|Jenkinsの画面から設定|git(Jenkinsfile)|
|通知先|メール|メール、Slack|



## Jenkinsfile
```
#!groovy
pipeline {
    agent any
    tools{
        nodejs "nodejs10"
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '30'))
    }
    parameters {
        booleanParam(name: 'isProduction', defaultValue: true, description: 'オフにすると開発環境に対して実行')
    }
    triggers { cron('H 6,15,23 * * *') }
```



```
    stages {
        stage('Test') {
            options {
                retry(3) 
            }
            steps {
                dir('sd-ci/retailer_registration'){
                    sh "cp .env.${params.isProduction ? 'production' : 'dev'} .env"
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }
    }

```



```
   post{
        failure{
            script{
                if(params.isProduction){
                    emailext(
                        body: mailBody(),
                        subject: 'SD定期監視(会員登録)アラート／$DEFAULT_SUBJECT',
                        to: 'notification-sd-emergency@apps.raccoon.ne.jp',
                        replyTo: 'notification-sd-emergency@apps.raccoon.ne.jp'
                    )
                }
                slackSend attachments: attachments()
            }
        }
        always{
            publishHTML([allowMissing: true, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'sd-ci/retailer_registration/mochawesome-report', reportFiles: 'mochawesome.html', reportName: 'mochawesome-report', reportTitles: ''])
        }
    }
```



## ジョブ Before
- 全部で26個
- 最新の成功ビルドが2年前とか…



- 最新の成功ビルドが1年以内の旧ジョブ
  - brahma
  - sel.sd_smoke
  - sel.sd_memberregist
  - sel._delete_screen_shot → 不要
  - sel.encache1 → 1と2をまとめる
  - sel.encache2
  - sel.corec_smoke_4.0
  - sel.corec_smoke_2.0_at_your_machine



## ジョブ After
- 使われていそうなジョブ（＝最新の成功ビルドが1年以内のもの）を書き直した
  - brahma
  - corec_smoke
  - corec_smoke_at_your_machine
  - encache
  - retailer_registration
  - smoke



## retailer_registration
- 小売店会員登録ジョブ
  - 登録ページにメアドを入力して送信
  - 登録メールを受信して登録用URLを取得
  - 登録用URLを開き、会員情報を入力し、送信
  - 店舗情報を入力し、送信
  - 古いメールを削除



| |ci-01|sd-ci01|
|--|--|--|
|メールサーバ|gmail|securemx|
|会員登録用メール|sd.regist.selenium.yyyyMMddhhmmss@catchall.raccoon.ne.jp|sd_e2e@raccoon.ne.jp|
|メール受信|gmailにブラウザログインして、メールを検索、該当メールを開く|IMAPでメールサーバにログイン、メールを検索、該当メールを取得|



## デモ
- vscodeでmiscリポジトリの sd-ci/retailer_registration を開く
- 環境変数が開発環境になっていることを確認
  - .env の中身
  - デモ用に HEADLESS=0
- コンソールを大きくし、ブラウザと並べて見られるようにする
- コンソールで npm test
- mochawesome.html Open with Live Server



## コード チラ見



## package.json
env-cmd: .envファイルを読んで環境変数に追加
```
  "dependencies": {
    "env-cmd": "^9.0.3",
    "imap-simple": "^4.3.0",
    "mailparser": "^2.7.1",
    "mocha": "^6.1.4",
    "mochawesome": "^4.0.1",
    "puppeteer": "^1.18.0"
  },
  "scripts": {
    "test": "env-cmd mocha test/index.js --reporter mochawesome"
  },
```



## index.js
分割したテストをまとめている
```JavaScript
require('./submit_email.js');
require('./find_entry_url.js');
require('./submit_retailer_info.js');
require('./submit_shop_info.js');
require('./delete_old_mails.js');
```



## submit_email.js
メールアドレスを入力して登録ボタンクリック
```
const email = 'sd_e2e@raccoon.ne.jp';
await page.type('form[action="/p/entry/mail/send.do"] input[name="mailAddress"]', email);
await Promise.all([
    page.click('form[action="/p/entry/mail/send.do"] input[type="button"]'),
    page.waitForNavigation({timeout: 2*MINUTE, waitUntil: "domcontentloaded"})
]);
```



完了画面になったことを確認
```
const textContent = await page.$eval('body', el =>  el.textContent);
assert.ok(/メールを送信しました/.test(textContent), "メールを送信しました、という文言が含まれていません。");
```



エラーが起きたらスクショを撮ってレポートHTMLに表示
```
try{
  // 省略
}catch(e){
  await common.addScreenshotToReport(this, e);
throw e;
}
```



## find_entry_url.js
imap-simpleというライブラリでメールサーバに接続
```
const imaps = require('imap-simple');
const config = {
  imap: {
    user: common.IMAP_USER,
    password: common.IMAP_PASSWORD,
    host: common.IMAP_HOST,
    port: 993,
    tls: true,
    tlsOptions: { rejectUnauthorized: !common.IMAP_IGNORE_TLS_ERROR },
    authTimeout: 30000,
    debug: (common.IMAP_DEBUG ? function(o){console.log(o)} : null),
  }
};
connection = await imaps.connect(config);
```



受信箱を開いて対象メールを検索
```
await connection.openBox('INBOX');
const searchCriteria = [
  ['FROM', common.MAIL_FROM],
  ['SINCE', yesterday],
  ['SUBJECT', common.MAIL_SUBJECT]
];
const fetchOptions = {bodies: ['']};
const messages = await connection.search(searchCriteria, fetchOptions);
```



mailparserというライブラリでパースしてメールオブジェクトを作成
```
const simpleParser = require('mailparser').simpleParser;
const mails = await Promise.all(messages.map(async message => {
  const part = message.parts.find(part => part.which === '');
  const mail = await simpleParser(`Imap-Id: ${message.attributes.uid}\r\n${part.body}`);
  return mail;
}));
```



送信日時が最新のメールから OneTimePassword を取得
```
const lastMail = mails.sort((a, b) => b.date - a.date)[0];
const otpMatch = /otp=(.+?)\n/.exec(lastMail.text);
const otp = otpMatch ? otpMatch[1] : null;
```



## まとめ
- Jenkinsサーバ起因による監視アラートの誤検知が減った（はず）
- テストをローカルで実行しやすくなった and テストが書きやすくなった
  - DX(Developer Experience)が向上！



## 残件
- ci-01サーバは社内mavenリポジトリとしても使われているのでサーバ破棄できず
  - → 社内mavenリポジトリ移行
- 会員登録テストのユーザを不通過にしているのは審査担当者
  - (信田さん) 自動で不通過になってくれると楽。長期連休明けに出展企業閲覧用アカウントも混ざってきたので作業が大変だった
  - → 会員登録テストのユーザを不通過にするバッチ
