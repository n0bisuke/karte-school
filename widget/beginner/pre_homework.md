# KARTE School - Tech Training: 事前に確認していただきたい事項・準備していただきたいこと

## はじめに

- [ ] KARTE School 共通の事前準備・確認事項がこちらにまとまっているので、こちらも必ずご確認・ご準備ください
    - [[KARTE School 全クラス共通] 事前に確認していただきたい事項・準備していただきたいこと](https://github.com/plaidev/karte-school/blob/master/pre_homework.md)
- [ ] 下記テンプレートを使用するので、このテンプレートを使ってテスト配信用接客サービスをご用意ください
    - 使用するテンプレート: ユーザーに見せる > 旧テンプレート > ポップアップ > 「上部画像+テキスト+ボタン (B000023)」
    - <img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner/_images/b000023.png" width="300px">
    - 留意点（再掲）
        - 必ず、配信対象をテスト配信用セグメントに絞り込みをしてください
        - 配信頻度は「アクセス毎」を指定
        - オプションの「同時配信を有効にする」にチェック


## Tech Training に参加される方に追加でご確認いただきたいこと

```
1. Web全般
2. HTML
3. CSS
4. JavaScript
  4-1. 変数
  4-2. 文字列
  4-3. オブジェクト
  4-4. 関数
  4-5. if文
  4-6. コメント
```

## 前提知識の確認
- このコースでは、次に紹介するWeb全般、HTML、CSS、JavaScriptに関する初歩的な知識を前提としています
    - 全て覚える必要は全くありませんが、用語のイメージをつけておくとコース内容の理解が早くなるかと思います

## 1. Web全般
- WebサイトやKARTEのポップアップなどのコンテンツは、HTML/CSS/JavaScriptといった技術によって実現されています
    - HTMLでは、コンテンツを構成する要素を記述します
    - CSSでは、コンテンツの装飾について記述します
    - JavaScriptでは、主にコンテンツの動きやデータの管理を担う処理を記述します

### 参考記事
- [【初心者向け】HTML、CSS、JavaScriptの違いと役割について | FASTCODING BLOG](https://fastcoding.jp/blog/all/jquery/html-css-javascript/)

#### 確認問題
- [ ] HTML/CSS/JavaScriptの役割の違いは？

## 2. HTML
- HTML要素は、「タグ」によって記述されます
    - タグには役割に応じていくつかの種類があります

```html
<p>こんにちは</p>
```

- HTML要素は、「属性」を持つことができます
    - 属性によって、要素に追加情報を持たせることができます

```html
<p class="greeting">こんにちは</p>
```

- HTML内では、`<!--`と`-->`で囲んだ部分は「コメント」と見なされます
    - コメントは、画面には表示されません

```html
<!-- ここはコメントです -->
<p class="greeting">こんにちは</p>
```

### 参考記事
- [HTML の基本 - Web 開発を学ぶ | MDN](https://developer.mozilla.org/ja/docs/Learn/Getting_started_with_the_web/HTML_basics)

#### 確認問題
- [ ] HTMLタグとは？
- [ ] HTML要素の属性とは？

## 3. CSS
- CSSでは、スタイルに関するルールを以下の形式に則って記述します

```css
CSSセレクタ {
  プロパティ名: プロパティ値;
}
```

- 各用語の意味は以下です
    - 「CSSセレクタ」
        - スタイルを適用するHTML要素を指定
    - 「CSSプロパティ」
        - どんなスタイルを設定するかを指定

- 実際の記述は、たとえば以下です

```css
p {
  color: red;
  width: 500px;
  border: 1px solid black;
}
```

- CSS内では、`/*`と`*/`で囲んだ部分は「コメント」と見なされます

```css
p {
  color: red; /* 文字色を赤く */
  width: 500px; /* 要素の幅を指定 */
  border: 1px solid black; /* 要素の境界線を1pxの黒色の実線に */
}
```

### 参考記事
- [CSS の基本 - Web 開発を学ぶ | MDN](https://developer.mozilla.org/ja/docs/Learn/Getting_started_with_the_web/CSS_basics)

#### 確認問題
- [ ] CSSセレクタとは？
- [ ] CSSプロパティとは？

## 4. JavaScript
### 4-1. 変数
- 「変数」とは、何らかの値を格納できる入れ物です
- 変数を新しく作るときは、以下のように宣言します
    - 変数の宣言には、`var`を使います
    - 値の代入には、`=`を使います

```js
var 変数名 = 変数の値;
```

- 実際の記述は、たとえば以下です

```js
var myName = 'Bob';
```

### 4-2. 文字列
- 変数に格納する値には、データ種類によっていくつかの型（Type）があります
- 最も代表的なデータ型は、「文字列」です
- 文字列を新しく作るときは、`'`や`"`で囲みます
- 複数の文字列を結合するには、`+`でつなぎます

```js
var myFullName = 'Bob' + ' ' + 'Dylan';
```

### 4-3. オブジェクト
- 「オブジェクト」とは、関連のあるデータ（「変数」）や処理（「関数」）のまとまりです
- 「プロパティ」とは、オブジェクトの中の変数です
- 「メソッド」とは、オブジェクトの中の関数です
- オブジェクトを新しく作成するときは、以下のように記述します

```js
var 変数名 = {
    プロパティ名またはメソッド名: 値,
    プロパティ名またはメソッド名: 値,
    ...
};
```

- 実際の記述は、たとえば以下です

```js
var person = {
  name: 'Bob',
  age: 32,
  gender: 'male',
  greeting: function() {
    console.log("Hi! I'm Bob.");
  }
};
```

- オブジェクトのプロパティは、以下のように参照することができます

```js
var myName = person.name;
```

- オブジェクトのメソッドは、以下のように実行することができます

```js
person.greenting();
```

- ブラウザやKARTEの仕組みによって予め定義されたオブジェクトも存在します
    - それらのオブジェクトは、明示的に宣言や作成をしなくても利用できます

### 4-4. 関数
- 「関数」とは、再利用したい処理をパッケージ化するための方法です
- オブジェクトのメソッドも「関数」でした
- 関数を新しく作成するには、以下のようにします

```js
function 関数名(引数名1, 引数名2,...) {
  処理
  return 戻り値;
}
```

- 実際の記述は、たとえば以下です

```js
function multiply(num1,num2) {
  var result = num1 * num2;
  return result;
}
```

- 「引数」とは、関数の実行に必要な値です
- 「戻り値」とは、関数の実行結果です
- 関数を実行するには、たとえば以下のように記述します

```js
var calcResult = multiply(7, 6);
console.log('答えは' + calcResult); // '答えは42'
```

- 関数の引数には、文字列、オブジェクトなど、さまざまなデータ型の値を渡すことができます
    - 以下のように、ある関数の引数として、別の関数を渡すこともできます

```js
function calc(num1, num2, func) {
    var result = func(num1, num2);
    return result;
}
var calcResult = calc(7, 6, function(num1, num2) {
  var result = num1 * num2;
  return result;
});
console.log('答えは' + calcResult); // '答えは42'
```

### 4-5. if文
- 「if文」は、指定した式がtrueであれば指定した処理を実行します

```js
if (条件式) {
    条件式がtrueの場合に実行する処理
}
```

- 実際の記述は、たとえば以下です
    - 値の比較には、`===`を使います
        - より曖昧な比較のために、`==`を使うこともあります

```js
var iceCream = 'チョコレート';
if (iceCream === 'チョコレート') {
  console.log('やった、チョコレートアイス大好き！');
}
```

- 指定した式がfalseの場合の処理も記述する場合は、「if-else文」を使います

```js
var iceCream = 'チョコレート';
if (iceCream === 'チョコレート') {
  console.log('やった、チョコレートアイス大好き！');
} else {
  console.log('あれれ、でもチョコレートは私のお気に入り......');
}
```

### 4-6. コメント

- JavaScript内では、`//`より後の部分は「コメント」と見なされます

```js
// 変数personを新規作成
var person = {
  name: 'Bob', // 名前を格納するプロパティ
  age: 32, // 年齢を格納するプロパティ
  gender: 'male', // 性別を格納するプロパティ
  greeting: function() { // 挨拶するメソッド
    console.log("Hi! I'm Bob.");
  }
};
// 挨拶させる
person.greenting();
```

### 参考記事
- [JavaScript の基本 - Web 開発を学ぶ | MDN](https://developer.mozilla.org/ja/docs/Learn/Getting_started_with_the_web/JavaScript_basics)

#### 確認問題
- [ ] 変数とは？
- [ ] 文字列の結合で使う記号は？
- [ ] オブジェクトのプロパティとは？
- [ ] オブジェクトのメソッドとは？
- [ ] 関数の引数とは？
