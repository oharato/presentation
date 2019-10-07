---
theme: "none"
customTheme: "presentation_kit\\lib\\azusaish"
transition: "none"
separator: "^\n\r\n\r\n\r|^\n\n\n"
verticalSeparator: "^\n\r\n\r|^\n\n"

---

# 業務に役立つHTML5の小ネタ集



## きっかけ

- JSやHTML5を勉強する機会が多かった
    - 新卒向けJS研修
    - app2のJSをjQueryなしで書く
    - HTML5認定試験を勉強中

↓

JS書かなくてもHTML5の要素で<br>やりたいことはだいたい実現できる



## おことわり

- 「業務」＝「app2」(社内ツール)
    - ブラウザはChrome＆FF
    - 細かい見た目や挙動は気にしなくて良い



## 目次

- フォームの戻るボタン
- カレンダーで日付入力
- 開閉式のパーツ
- 入力候補サジェスト
- 二度押し不可ボタン
- ブラウザ対応状況



## フォームの戻るボタン



### フォームの戻るボタン(Before)
- 確認画面あるある
    - 決定ボタンをクリックするとexecuteなパスにPOSTする
    - 戻るボタンをクリックするとinputなパスにGETする
- 戻るボタンの挙動はJSで書く事が多い？
    - 戻るをクリックするとformのactionとmethodを書き換えて…



<div class="totsuzennoshi">
<p>楽に戻るボタンを実装したい</p>
</div>



<!-- .slide: data-background="html5tips/shuchusen.png" data-background-transition="none" -->
<div class="shuchusen">
input要素のformaction属性と<br>formmethod属性
</div>



- formaction: 指定した場合、そのボタンの属するフォームの action 属性より優先されます。
- formmethod: 指定した場合、そのボタンの属するフォームの method 属性より優先されます。
- IE10からサポート
- https://developer.mozilla.org/ja/docs/Web/HTML/Element/input



### フォームの戻るボタン(After)

```html
<form action="execute" method="post">
  <input type="hidden" name="user_name" value="ohara.tomoki">
  <input type="submit" value="決定">
  <input type="submit" value="戻る"
    formaction="input" formmethod="get">
</form>
```



#### 実装例

- http://st-app2/sd/foreign/class_weight_rate/bulk_register_index
    - `4082 120 2018-03-01`



## カレンダーで日付入力



### カレンダーで日付入力(Before)
- ユーザに日付を入力してもらいたい
    - jQuery UI Datepicker使う？



<div class="totsuzennoshi">
<p>楽にカレンダーを実装したい</p>
</div>



<!-- .slide: data-background="html5tips/shuchusen.png" data-background-transition="none" -->
<div class="shuchusen">
&lt;input type="date"&gt;
</div>



- 2017-11-28 FF57から使用可能。Chromeは2012年から。
- 日付を選択するとvalueにyyyy-mm-dd形式で日付の文字列がセットされる
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/date



### カレンダーで日付入力(After)

```html
<input type="date" name="execute_date" value="">
```



<p><input type="date" name="execute_date" value="" style="font-size:1.5rem;"></p>



## 開閉式のパーツ



### 開閉式のパーツ(Before)

- 詳細などに使われる、クリックしたら開いたり閉じたりするパーツ
- jQueryのslideToggle(), animate(), CSS3のtransition使う？



<div class="totsuzennoshi">
<p>楽に開閉式のパーツを実装したい</p>
</div>



<!-- .slide: data-background="html5tips/shuchusen.png" data-background-transition="none" -->
<div class="shuchusen">
details要素
</div>



- details要素の子要素にsummary要素を使い、詳細のタイトルをsummary要素の内容に書く。
- 詳細情報はsummary要素の兄弟要素として書く。
- https://developer.mozilla.org/ja/docs/Web/HTML/Element/details



### 開閉式のパーツ(After)

```html
<details>
  <summary>詳細</summary>
  <p>詳細な情報をここに書く</p>
</details>
```



#### 実装例

<details>
  <summary>詳細</summary>
  <p>詳細な情報をここに書く</p>
</details>

- http://10.100.252.10/sd/foreign/tt_next_claim/tt_next_claim_create_index



## 入力候補サジェスト



### 入力候補サジェスト(Before)
- 入力ボックスに文字を入力したら候補を表示したい
- JSでonkeyupイベントで文字を取得して、候補を正規表現とかで絞り込んで…



<div class="totsuzennoshi">
<p>楽にサジェスト機能を実装したい</p>
</div>



<!-- .slide: data-background="html5tips/shuchusen.png" data-background-transition="none" -->
<div class="shuchusen">
datalist要素
</div>



- datalist要素を作成する。idを設定する。子要素にoption要素を書く。
- input要素のlist属性にdatalist要素のidを指定する



### 入力候補サジェスト(After)

```html
<label>メール送信先:<input list="addresses" name="address" /></label>
<datalist id="addresses">
    <option value="ohara.tomoki@raccoon.ne.jp">
    <option value="shimoda.keitaro@raccoon.ne.jp">
    <option value="abe.hiroyuki@raccoon.ne.jp">
</datalist>
```



<label>メール送信先:<input list="addresses" name="address" style="font-size:1rem;"/></label>
<datalist id="addresses">
    <option value="ohara.tomoki@raccoon.ne.jp">
    <option value="shimoda.keitaro@raccoon.ne.jp">
    <option value="abe.hiroyuki@raccoon.ne.jp">
</datalist>



## 二度押し不可ボタン



### 二度押し不可ボタン(Before)
- 決定ボタンを連打しても実行は1回だけにしたい
- JSで実行したかどうかの変数を使って…



<div class="totsuzennoshi">
<p>楽に二度押し不可ボタンを実装したい</p>
</div>



<!-- .slide: data-background="html5tips/shuchusen.png" data-background-transition="none" -->
<div class="shuchusen">
addEventListener の第3引数
</div>



```javascript
target.addEventListener(type, listener, {
  once: true
});
```

- once: trueを指定するとlistener は一度実行された時に自動的に削除
- IE未サポート
- https://developer.mozilla.org/ja/docs/Web/API/EventTarget/addEventListener



### 二度押し不可ボタン(After)

```javascript
const form = document.forms.bulkRegisterForm;
form.execButton.addEventListener('click', function(){
    this.disabled = true;
    form.submit();
}, {once: true});
```



#### 実装例

- http://st-app2/sd/foreign/class_weight_rate/bulk_register_index
    - `4082 120 2018-03-01`



## ブラウザ対応状況

| |IE11|FF58|Chrome64|
|----|----|----|----|
|formaction<br>formmethod|o|o|o|
|type="date"|x|o|o|
|details|x|o|o|
|datalist|o|o|o|
|once|x|o|o|

- https://caniuse.com



## まとめ

- あなたがJSで実装しようとした機能はすでに実装済みかも
- 試験勉強や技術系の記事を通じて知識をブラッシュアップするのは大切
- vscodeのrevealjs拡張は最高



## More

- [HTML5認定試験](http://html5exam.jp/outline/lv1_v2.html)
- [MDN](https://developer.mozilla.org/ja/docs/Web/HTML/Element)
- [ブラウザ対応状況](https://caniuse.com/#index)
