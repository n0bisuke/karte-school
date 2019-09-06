# Widget Customize 初級: 本編
## はじめに
### インストラクター紹介
- https://github.com/plaidev/karte-school/tree/master/_instructors

### 参加者自己紹介
- 企業名
- お名前
- 職種
    - ご自身がKARTEでどんなことをしているか
- 受講を決めたきっかけや理由

### このコースのゴール
- 簡単な「診断コンテンツ」を作れるようになる

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/shindan.gif" width="300px">

- それを通じて、Widgetのカスタマイズによってできることを知る
    - Web/App共通
        - 要素を複製する
        - 余白を調整する
        - ステートを増やして遷移する
        - KARTEにイベント送信する
        - ユーザー情報を埋め込んでアクションで活用する
    - Web
        - サイト内のボタンを自動でクリックさせる
        - サイト内の要素をクリックしたときに接客表示させる

### このコースの特徴
- このコースは、非エンジニアの方の受講を想定したものです
- Widgetのカスタマイズを実践する中で、HTML/CSSと、特にJavaScriptを学ぶことができます
- JavaScriptの文法解説よりも、「JavaScriptを書くと何ができるか」を体感することを重視しています
    - そのため、今回のコースを元に、カスタマイズの実践の中で長期的にJavaScriptを学んでいただくことを前提としています

### 事前課題の確認
- https://github.com/plaidev/karte-school/blob/master/widget/beginner_v2/pre_homework.md

### テスト配信環境の確認
- ご自身のKARTE環境で、自分だけに本番環境でテスト配信をすることができるかをご確認ください
    - 「テスト配信用セグメント」を使った配信制御が一般的です
    - セグメント設定例:
        - 「すべての期間 手動ラベル ラベル の 最新の値 が 〇〇 に等しい」

## 0. 診断コンテンツとは？
- 複数のステートをボタンによって複雑に分岐させ、人によって異なる最終結果を表示させるようなコンテンツ
- 高度な実装例
    - 「診断コンテンツの結果に合わせておすすめのページへ誘導」
        - https://admin.karte.io/store/svc/58192cb38349239572374202
        - ご自身のプロジェクトを選択すると、当該接客シナリオのページにリダイレクトします

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/shindan_store.gif" width="300px">

### 診断コンテンツの設計
- 今回は、シンプルなテンプレートをカスタマイズし、以下のようなポップアップを作成します

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/shindan.gif" width="300px">

- 診断シナリオは、以下とします
    - ユーザー毎に最適なキャンペーンをオススメする
    - 選択肢は、「はい」と「いいえ」のみ
    - 左上の数字が、Widgetのステートに対応

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/shindan_design.png" width="800px">

## 1. Widgetとは何か？
- KARTEでは、ブラウザやネイティブアプリにHTML/CSS/JavaScriptで記述されたコンテンツを配信できます
- Widgetとは、こうしたコンテンツの実装をより簡単にするためのKARTEの機能です
- HTML/CSS/JavaScriptをそのまま記述するよりも、以下の点が簡単になっています
    - HTML（表示）とJavaScript（状態や処理）との関連付け
        - 特に、コンテンツのステート（状態）管理

### 1-1. Widgetタイプの接客サービスかどうかを見分ける方法
- ブラウザやアプリ内で実行される接客サービスでだけ使用されます
- アクション編集画面にステートに関する次のような表示がある場合、Widgetが使われています

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/state_ui.png" width="300px">

## 2. 要素を増やす
- テンプレートにすでにある要素を複製して増やす方法を紹介します

### 2-1. ワーク: ステートを1つ増やす
<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/state2_check.png" width="400px">

- アクション編集画面を開きます
- 右サイドバーの「アクション設定」から、以下を設定します
    - 「メイン画像」
        - 適当な画像を設定
    - 「見出し」
        - `あなたに合ったキャンペーン診断！`
    - 「詳細文」
        - `診断を始めますか？`
    - 「ボタンテキスト」
        - `はい`
- 「ステート追加」ボタンを押してステート2を追加します

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/add_state.png" width="300px">

- 「カスタマイズ」タブを開きます
- HTMLのステート1に対応する要素を下にコピペして、ステート2用の要素を追加します
    - `krt-if`の値を`"state==2"`に書き換えます

```html
<div class="karte-temp-whole">
    <div class="karte-temp-state1" krt-if="state==1">
        <i class="karte-temp-close karte-close karte-temp-hover" krt-if="#{close.use}"></i>
        <!-- 中略 -->
    </div>
    <div class="karte-temp-state1" krt-if="state==2">
        <i class="karte-temp-close karte-close karte-temp-hover" krt-if="#{close.use}"></i>
        <!-- 中略 -->
    </div>
</div>
```

- 「変数」タブを開きます
- 別々の変数で各種文言を設定できるように、「+変数の追加」から以下の静的変数を追加します

変数名 | 型 | 値 | 表示名
-- | -- | -- | --
title2 | 文字列 | この〇〇を知っていますか？ | 見出し2
description2 | 文字列 | ''(空文字) | 詳細文2

- HTMLのステート2に対応する要素内の、対応する変数名を書き換えます

```html
    <div class="karte-temp-state1" krt-if="state==2">
        <!-- 中略 -->
                <div class="karte-temp-title">#{title2}</div>
                <div class="karte-temp-description">#{description2}</div>
        <!-- 中略 -->
    </div>
```

- 左上のステート選択ボタンから直接「ステート2」を選択し、ステート2が表示されることを確認します

- 右上の「保存」ボタンを押します
- ちなみに、「ベーシック > 表示設定 > ステート2」から、表示位置を右下などに変更できます

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/state2_display_setting.png" width="400px">

### 2-2. 静的変数の使い方
- 「静的変数」とは？（KARTEの用語）
    - ベーシックタブで値を設定できる変数
    - HTML/CSS/JavaScriptの中でよく変更する部分を静的変数にし、ベーシックタブから簡単にその値を変更できるようにする
- HTMLなどから参照する方法
    - `#{変数名}`
        - 例: `#{title}`
        - フォルダに入っている場合は、`#{フォルダ名.変数名}`
            - 例: `#{close.use}`

### 2-3. ワーク: ボタンを増やす
<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/state2_add_button.png" width="300px">

- HTMLのステート2に対応する要素内の以下の部分を、2行に複製します

```html
<a class="karte-temp-btn karte-temp-hover" href="#{link}">#{btn}</a>
<a class="karte-temp-btn karte-temp-hover" href="#{link}">#{btn}</a>
```

- 別々の変数で各種文言を設定できるように、「+変数の追加」から以下の静的変数を追加します

変数名 | 型 | 値 | 表示名
-- | -- | -- | --
btn2 | 文字列 | いいえ | ボタンテキスト2

- 先ほど複製したボタンについて、対応する変数名を書き換えます

```html
<a class="karte-temp-btn karte-temp-hover" href="#{link}">#{btn}</a>
<a class="karte-temp-btn karte-temp-hover" href="#{link}">#{btn2}</a>
```

- ステート2を表示して、プレビューに変数の値が反映されることを確認します
- 右上の「保存」ボタンを押します

### 2-4. 代表的なHTMLタグ
- HTMLタグの種類はたくさんありますが、そのほとんどは覚える必要はありません
    - 役割が決まっているHTMLタグを除けば、divとspanだけを覚えれば大抵の表現は可能です
- HTMLタグ毎に、指定できる属性が異なることがあります
- 主な「役割が決まっているHTMLタグ」
    - aタグ
        - リンクに使用
        - href属性
            - リンク先URLを指定
    - imgタグ
        - 画像表示に使用
    - ユーザーからのインプットを受け付けるタグ
        - inputタグ
            - type属性
                - "text": テキストボックス
                - "radio": ラジオボタン
                - "checkbox": チェックボックス
                - "file": ファイルのアップロード
        - selectタグ、optionタグ
            - セレクトボックス（プルダウン）
        - textareaタグ
            - テキストエリア
- その他の主なHTMLタグ
    - ブロック要素（主に他の要素を内包する）
        - div、h1、p、ul、li
    - インライン要素（主にテキスト以外の要素を内包しない）
        - span、i、label

### 2-5. コラム: HTMLタグとセマンティクス
- なぜHTMLタグの種類はこんなにたくさんあるのか？
    - 種類数でいうと、100以上
- HTMLタグの種類が多い理由
    1. 歴史的な経緯
        - もともと、HTML内でスタイルを指定するのが一般的だった
        - そのため、スタイルを指定するためのHTML要素があった
            - iタグ: イタリック
            - uタグ: アンダーライン
            - bタグ: 太字
        - その後、CSSの登場によりHTMLでスタイルを指定する必要が無くなった
    2. セマンティクスのため
        - 「セマンティクス」とは、プログラムが持つ「意味」のこと
            - HTMLでは、各要素のドキュメント上の意味
        - セマンティクスに配慮することで、人間には見た目が同じでも、プログラムにわかりやすく意味を伝えられるようになる
            - SEO、アクセシビリティ向上
        - 主なセマンティック要素
            - h1〜6タグ: 見出し
            - pタグ: 段落
            - sectionタグ: セクション
            - headerタグ、footerタグ: ヘッダー、フッター

## 3. CSSによるスタイル調整
- CSSやHTMLのclass属性を変更しスタイルを調整する方法を紹介します

### 3-1. ワーク: 余白を調整する
<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/set_margin.png" width="300px">

- 2つに増やしたボタンは、間が詰まっています
    - このボタンとボタンの間の余白を追加します
- まず、ボタンに対応するHTML要素のclass属性を特定します

```html
<a class="karte-temp-btn karte-temp-hover" href="#{link}">#{btn}</a>
<a class="karte-temp-btn karte-temp-hover" href="#{link}">#{btn2}</a>
```

- そのclass名は、`karte-temp-btn`と`karte-temp-hover`
    - ボタンとしてのスタイルは、名前から`karte-temp-btn`とわかる
- CSSタブの中から、`karte-temp-btn`に対応するスタイル指定を見つけて`margin-top`の指定を追加します

```css
.karte-temp-btn {
    margin-top: 10px; /* ボタンの上マージンを指定 */
    /* 中略 */
}
```

- プレビューで余白が広がったことを確認します
- 右上の「保存」ボタンを押します

### 3-2. HTMLのclass属性とCSS
- HTMLのclass属性は、CSSで指定したスタイルをHTMLの要素と対応づけるために使われます
- CSSの記述方法は以下です
    - 「CSSセレクタ」と呼ばれる文字列によって、対象の要素を特定します

```css
対象の要素を指すCSSセレクタ {
    プロパティ名: プロパティ値;
    プロパティ名: プロパティ値;
    ...
}
```

- CSSセレクタでは、主に以下のような手がかりを使ってHTML要素を指定します
    - HTMLタグ名
    - id属性
    - class属性
- class属性を使ってCSSセレクタを記述する方法は以下です
    - `.class名`
- つまり、`karte-temp-btn`というclassに対応するプロパティは以下のように記述します

```css
.karte-temp-btn {
    margin-top: 10px;
    /* 中略 */
}
```

### 3-3. CSSボックスモデル
- CSSで実現されるWeb上のレイアウトのルールは、「ボックスモデル」と呼ばれています
    - すべての要素が長方形のボックスとして表される

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/box_model.png" width="600px">
出典: https://developer.mozilla.org/ja/docs/Learn/CSS/Introduction_to_CSS/Box_model

- 実際の要素の「ボックス」は、Chromeデベロッパーツールで確認することができます

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/margin_padding.png" width="800px">

### 3-4. 代表的なCSSプロパティ
- ボックスモデルに関するプロパティ
    - margin（-top、-right、-bottom、-left）
        - 周囲の要素との間の余白
    - border（-top、-right、-bottom、-left）
        - 境界線
        - 太さ、スタイル、色を設定可能
    - padding（-top、-right、-bottom、-left）
        - borderの内側の余白
    - width
        - コンテンツの幅
    - height
        - コンテンツの高さ
- ボックスの装飾に関するプロパティ
    - background
        - 背景色
    - border-radius
        - 角丸の度合い
- 文字の装飾に関するプロパティ
    - color
        - 文字色
    - font-size
        - 文字サイズ
    - text-decoration
        - 下線など

### 3-5. ワーク: 「はい」と「いいえ」で、ボタンの背景色を変える

- 「はい」ボタンと「いいえ」ボタンで別のスタイルを当てられるように、ボタンにclassを追加します
    - 「はい」
        - `yes`というclassを追加
    - 「いいえ」
        - `no`というclassを追加

- ステート1のボタンに`yes`というclassを付与します

```html
<a class="yes karte-temp-btn karte-temp-hover" href="#{link}">#{btn}</a>
```

- ステート2のボタンに`yes`と`no`というclassを付与します

```html
<a class="yes karte-temp-btn karte-temp-hover" href="#{link}">#{btn}</a>
<a class="no karte-temp-btn karte-temp-hover" href="#{link}">#{btn2}</a>
```

- `.karte-temp-btn`に対するスタイルから、`background`の指定を削除します
- ベーシックタブから、「ボタン背景色」を赤に設定します
- 別々の変数で各種文言を設定できるように、「+変数の追加」から以下の静的変数を追加します

変数名 | 型 | 値 | 表示名
-- | -- | -- | --
btn_background2 | カラー | (青) | ボタン背景色2

- 追加したclassに対応するスタイルを追加します

```css
.yes {
    background: #{btn_background}; /* 「はい」ボタンの背景色 */
}
.no {
    background: #{btn_background2}; /* 「いいえ」ボタンの背景色 */
}
```

- ステート2を表示し、ボタン色が変わったことを確認します
- 右上の「保存」ボタンを押します

## 4. ステートを切り替える
- UI操作やスクリプトによってステートを変更するための方法を紹介します

### 4-1. ワーク: ステート1のボタンでステート2に遷移できるようにする
- ステート1のボタンに対応するHTMLを、以下のように書き換えます
    - aタグをdivタグに変更
    - href属性を削除
    - krt-on:click属性を追加

```html
<!-- 変更前 -->
<a class="yes karte-temp-btn karte-temp-hover" href="#{link}">#{btn}</a>

<!-- 変更後 -->
<div class="yes karte-temp-btn karte-temp-hover" krt-on:click="setState(2)">#{btn}</div>
```

- ステート1のボタンを押すとステート2に遷移することを確認
- 右上の「保存」ボタンを押します

### 4-2. Widgetが提供するカスタムディレクティブ
- [Widgetのカスタムディレクティブ](https://developers.karte.io/reference#template)とは？（KARTEの用語）
    - WidgetのHTMLで使用できるカスタムの属性
        - 通常のHTMLでは使えない
    - 「HTML要素」と「Widgetの状態やメソッド」とを紐付ける
    - ディレクティブの値は、文字列ではなくJavaScriptとして解釈される
        - つまり、HTML属性の中に条件文や処理を直接書ける
- このコースで使うディレクティブ
    - `krt-if="条件文"`
        - 条件文が成立するときだけ、この要素を表示する
        - 主にステート毎の表示切替のために利用されている
    - `krt-on:event名="処理"`
        - event名で指定したイベントが発生した場合に、指定した処理を実行
            - eventについては後述
        - 主に要素のクリック時の処理を設定するために利用されている
- HTMLの中で、ディレクティブが使われている部分を改めて見てみましょう

```html
<div class="karte-temp-whole">
    <div class="karte-temp-state1" krt-if="state==1">
        <i class="karte-temp-close karte-close karte-temp-hover" krt-if="#{close.use}"></i>
        <!-- 中略 -->
                <div class="yes karte-temp-btn karte-temp-hover" krt-on:click="setState(2)">#{btn}</div>
        <!-- 中略 -->
    </div>
    <div class="karte-temp-state1" krt-if="state==2">
        <i class="karte-temp-close karte-close karte-temp-hover" krt-if="#{close.use}"></i>
        <!-- 中略 -->
    </div>
</div>
```

### 4-3. Widgetが提供する、ステートに関するメソッド
- ステートとは？（KARTEの用語）
    - Widgetが管理する、表示状態を指定するための番号
    - コンテンツ内での表示切替に利用する
- メソッドとは？（JavaScriptの用語）
    - JavaScriptのオブジェクトに登録された関数（=処理）
    - メソッド呼び出しの記述方法
        - `オブジェクト名.メソッド名(引数)`
        - 例
            - `widget.setState(2)`
- Widgetに対する一部の操作は、メソッドとして登録されている
- ステートに関するWidgetのメソッド
    - `widget.setState(n)`
        - ステートをnに変更する
        - HTMLの`krt-on:click=""`から直接呼び出す場合、`widget.`は不要
    - `widget.show()`
        - ステートを1に変更する
        - `widget.setState(1)`と同じ
    - `widget.hide()`
        - ステートを0に変更する
        - ステート0は、「非表示時」に対応している
        - `widget.setState(0)`と同じ
- 参考: [リファレンス - widget | developers.karte.io](https://developers.karte.io/reference#widget)

### 4-4. ワーク: 残りのステートを追加する
- 以下の診断シナリオを参考に、残りのステートを追加します

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/shindan_design.png" width="800px">

- 「ステート追加」ボタンを3回押し、ステート3〜5を追加します

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/add_state.png" width="300px">

- 別々の変数で各種文言を設定できるように、「+変数の追加」から以下の静的変数を追加します

変数名 | 型 | 値 | 表示名
-- | -- | -- | --
title3 | 文字列 | この〇〇を買いましたか？ | 見出し3
title4 | 文字列 | あなたには「友達紹介キャンペーン」がオススメ！ | 見出し4
title5 | 文字列 | あなたには「プレゼントキャンペーン」がオススメ！ | 見出し5
description3 | 文字列 | ''(空文字) | 詳細文3
description4 | 文字列 | 友達に紹介してクーポンをGETしよう！ | 詳細文4
description5 | 文字列 | アンケートに答えて抽選に応募しよう！ | 詳細文5
btn4 | 文字列 | 友達に紹介する | ボタンテキスト4
btn5 | 文字列 | アンケートに答える | ボタンテキスト5
link4 | URL | (任意のURL) | リンク先URL4
link5 | URL | (任意のURL) | リンク先URL5

- ステート2のボタンを修正し、クリック時にステート変更できるようにします
    - 「はい」
        - ステート3へ
    - 「いいえ」
        - ステート5へ

```html
<div class="yes karte-temp-btn karte-temp-hover" krt-on:click="setState(3)">#{btn}</div>
<div class="no karte-temp-btn karte-temp-hover" krt-on:click="setState(5)">#{btn2}</div>
```

- ステート2に対応するHTMLをコピーし、ステート3に対応するHTMLを追加します
    - 以下に対応する部分を修正
        - `krt-if`の値
        - `title`の変数名
        - `description`の変数名
    - ボタンクリック時の遷移先ステートを修正
        - 「はい」
            - ステート4へ
        - 「いいえ」
            - ステート5へ

```html
<div class="karte-temp-state1" krt-if="state==3">
    <i class="karte-temp-close karte-close karte-temp-hover" krt-if="#{close.use}"></i>
    <div class="karte-temp-card">
        <div class="karte-temp-mainvisual"><img src="#{mainvisual}" alt="#{mainvisual_alt}" /></div>
        <div class="karte-temp-wrap">
            <div class="karte-temp-title">#{title3}</div>
            <div class="karte-temp-description">#{description3}</div>
            <div class="yes karte-temp-btn karte-temp-hover" krt-on:click="setState(4)">#{btn}</div>
            <div class="no karte-temp-btn karte-temp-hover" krt-on:click="setState(5)">#{btn2}</div>
        </div>
    </div>
</div>
```

- ステート3に対応するHTMLをコピーし、ステート4に対応するHTMLを追加します
    - 以下に対応する部分を修正
        - `krt-if`の値
        - `title`の変数名
        - `description`の変数名
        - `btn`の変数名
    - ボタンを1つにし、リンクにする
        - aタグに変更
        - `krt-on:click`を削除
        - href属性を追加
            - 値は`#{link4}`

```html
<div class="karte-temp-state1" krt-if="state==4">
    <i class="karte-temp-close karte-close karte-temp-hover" krt-if="#{close.use}"></i>
    <div class="karte-temp-card">
        <div class="karte-temp-mainvisual"><img src="#{mainvisual}" alt="#{mainvisual_alt}" /></div>
        <div class="karte-temp-wrap">
            <div class="karte-temp-title">#{title4}</div>
            <div class="karte-temp-description">#{description4}</div>
            <a class="yes karte-temp-btn karte-temp-hover" href="#{link4}">#{btn4}</a>
        </div>
    </div>
</div>
```

- ステート4に対応するHTMLをコピーし、ステート5に対応するHTMLを追加します
    - 以下に対応する部分を修正
        - `krt-if`の値
        - `title`の変数名
        - `description`の変数名
        - `link`の変数名
        - `btn`の変数名

```html
<div class="karte-temp-state1" krt-if="state==5">
    <i class="karte-temp-close karte-close karte-temp-hover" krt-if="#{close.use}"></i>
    <div class="karte-temp-card">
        <div class="karte-temp-mainvisual"><img src="#{mainvisual}" alt="#{mainvisual_alt}" /></div>
        <div class="karte-temp-wrap">
            <div class="karte-temp-title">#{title5}</div>
            <div class="karte-temp-description">#{description5}</div>
            <a class="yes karte-temp-btn karte-temp-hover" href="#{link5}">#{btn5}</a>
        </div>
    </div>
</div>
```

- 最後に動作を確認し、問題なければ「保存」ボタンを押します
- ここまでで、診断コンテンツとしてのカスタマイズは終了です
    - これ以降は、Widgetでよくあるカスタマイズについて紹介します

## 5. KARTEへのイベント送信とユーザー情報変数
- WidgetからKARTEにイベント送信をすることで、ユーザーデータを更新することができます
- アクションの中にユーザーデータを埋め込んで配信することで、ユーザー毎にWidgetの挙動を変更することができます

### 5-1. ワーク: ステートの状態が変わったらKARTEにイベント送信する
- Scriptの末尾に、以下を追記
    - ステートが変更される度に、testイベントを発生させる
    - testイベントの中に、現在のステートを格納

```js
// 動的変数stateが変化したときに実行される関数を登録
widget.onChangeVal('state', function(values) {
    // 発生させるイベント名を指定（問題ある場合は変更してください）
    var eventName = 'test';
    // イベント名と値を指定して、イベントを発生させる
    tracker.track(eventName, {
        // イベントの値に接客サービスIDを含める
        '#{campaign_id}': {
            // イベントの値に最新のステートを含める
            state: values.newVal
        }
    });
});
```

- テスト配信を実施し、ステートを変更してみる
- 自分のユーザー詳細画面から、testイベントが発生しているか確認
    - 画面に反映されるまで、最大数十秒ほどタイムラグがある場合がある

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/track_event.png" width="800px">

- testイベントの`接客サービスID.state`に最新のステートが格納されていることを確認

### 5-2. ステート変更検知
- Widgetの動的変数とは？（KARTEの用語）
    - Widgetに登録された変数
    - 静的変数とは異なり、ユーザーの操作によって動的に変わることがある
    - ステートやユーザーの入力値などを管理するために使う
- ステートの値は、`state`という名称でWidgetの「動的変数」として登録されています
- Widgetの動的変数の値が変更したときに任意の処理を実行する記述方法は以下です

```js
widget.onChangeVal('動的変数の名前', function(values) {
    // 動的変数が変化したときに実行したい処理を記述
});
```

### 5-3. KARTEへのイベント送信
- KARTEのユーザーデータは、イベントと呼ばれる単位で管理されています
    - イベントには、イベント名と複数の値を含めることができます
- 接客サービスの表示やクリックなどに関しては、自動で以下のようなイベントが発生します
    - 参考: [主要なイベント | KARTEサポートサイト](https://support.karte.io/post/67LppL36ssbFztyOjUzaqZ)

イベント名 | 発生タイミング
-- | --
message_open | 接客サービスを表示
message_click | 接客サービスのリンクをクリック
message_close | 接客サービスを閉じる
_message_state_changed | 接客サービスのステート変更（設定時のみ）

- WidgetのScript内からも、KARTEに任意のイベントを送信することができます
    - `tracker`というイベントトラッキング用のオブジェクトを利用します
    - イベント名には、自由に名前を設定できます
        - 半角小文字英字(a-z)、半角数字(0-9)と'_'のみ使用できます
        - 参考: [アクションでカスタムイベントを送信する | KARTEサポートサイト](https://support.karte.io/post/2DmFyL2Dd929nRYO4wSej8#1-0)
    - ちなみにWeb版のKARTEでも、計測タグが設置されたページ上で`tracker`が使えるようになります
    - 参考: [リファレンス - tracker | KARTE developers portal](https://developers.karte.io/reference#jssdk-tracker)

```js
tracker.track('イベント名', {
    フィールド名1: 値1,
    フィールド名2: 値2,
    ...
});
```

- 値が不要な場合は、イベント名だけを渡して実行することもできます

```js
tracker.track('イベント名');
```

- ワークの例では、接客サービス毎に別のフィールドに値を設定できるように、接客サービスのIDをフィールド名に含めています
    - 静的変数`#{campaign_id}`を使うと、KARTEが自動で接客サービスIDを設定してくれます

```js
// 変数eventNameに、発生させるイベント名を格納（問題ある場合は変更してください）
var eventName = 'test';
// イベント名と値を指定して、イベントを発生させる
tracker.track(eventName, {
    // イベントの値に接客サービスIDを含める
    '#{campaign_id}': {
        // イベントの値に最新のステートを含める
        state: values.newVal
    }
});
```

- KARTEにイベント送信することで、接客アクションの状態などをユーザー情報に保持できます
    - イベントによって更新されたユーザー情報は、主に以下に使えます
        - セグメントへの利用
        - ユーザー情報変数を使ったアクションへの埋め込み

### 5-4. ワーク: 前回のステートに応じて、初期表示時のステートを切り替える
- 「ベーシック > データ管理 > ユーザー情報変数 > 追加」をクリック
- 以下の内容で変数を作成
    - 変数名
        - `lastState`
    - 条件
        - すべての期間
        - `test`（先ほどの手順で送信したイベント名）
        - `接客サービスID.state`
            - 接客サービスIDは、URLの`/service/`の直後から確認できる
        - 最新の値
    - デフォルト値
        - `1`
    - プレビューの値
        - `3`を選択し、Enter

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/user_data_variable.png" width="500px">

- Scriptの`widget.show()`の部分を、以下のように書き換えます

```js
// 変数lastStateに前回のステート値を格納
var lastState = [[lastState]];
// もしlastStateの値が'0'だったら
if (lastState === '0') {
    // ステート1を初期表示
    widget.setState(1);
// もしlastStateの値が'2'だったら
} else if (lastState === '2') {
    // ステート1を初期表示
    widget.setState(1);
} else {
    // 前回の最後のステートを初期表示
    widget.setState(lastState);
}
```

- プレビュー画面での動作確認
    - 「アクション再実行」ボタンを押し、ステート3から表示されることを確認します
- テスト配信での動作確認
    - 離脱時の最後のステートに応じて、再読み込み時の初期表示ステートが変化することを確認します
        - 素早く操作しすぎるとユーザー情報の更新が間に合わないことがあります
- アクションを「保存」します

### 5-5. ユーザー情報変数
- 接客アクションに、配信先ユーザーのユーザー情報を埋め込んで配信することができます
    - セグメントと同様の設定で参照するユーザー情報を指定
- 参照できる情報は、「[ユーザー詳細画面 > ユーザーデータ](https://support2.karte.io/userbh/userbh-userdetail/1588/#index)」から確認できます

### 5-6. 事例: ユーザー情報変数を利用した閲覧履歴一覧やお気に入り一覧の表示
- 閲覧履歴
    - テンプレート > スクリプト実行 > 「閲覧アイテム情報取得スクリプト」
    - テンプレート > ユーザーに見せる > 「直近閲覧アイテムリスト」
    - [サイト上に簡易に閲覧履歴機能を導入](https://admin.karte.io/store/svc/58219c257a7ed7cf3b2266f8)
    - [スマホで閲覧履歴を表示して回遊性を高めるとともにコンバージョン率アップ（サンプル百貨店）｜KARTE CX Clip｜KARTE CX Clip](https://cxclip.karte.io/practice/sample-case03/)
- お気に入り
    - テンプレート > ユーザーに見せる > 「お気に入りボタン」
    - テンプレート > ユーザーに見せる > 「お気に入りアイテムリスト」
    - [サイトに「お気に入り機能」をKARTEで簡単実装（LUXA）｜KARTE CX Clip｜KARTE CX Clip](https://cxclip.karte.io/practice/luxa-case01/)

### 5-7. 事例: チャット終了後にアンケートを表示する
- チャット表示用の接客サービスを閉じたときに、別のアンケート用接客サービスを表示する
    - チャット接客をカスタマイズし、「チャットパネルを閉じたらイベントを発生させる」という処理を追加する
    - そのイベントをトリガーに、アンケート用接客サービスを配信する
    - [チャット終了後にアンケートを実施し満足度を調査](https://admin.karte.io/store/svc/5c11d66f516e5108a2bd5a66)

### 5-8. [for Web]その他のデータ保存領域
- Webの場合は、KARTEのユーザー情報以外にも、JavaScriptからデータを読み書きできる領域があります
    - ブラウザのcookie
    - ブラウザのlocalStorage

## 6. JavaScriptのデバッグについて
- デバッグとは？（一般用語）
    - プログラムの誤り（=バグ）を見つけて、それを直すこと
- アクションを修正した場合の結果の検証をどうやるか？
    - 細かい修正の結果はプレビューで確認
        - Scriptを修正した場合は、Scriptタブ最下部の「アクション再実行」ボタンを押す
    - ある程度の修正がまとまったらテスト配信で確認
        - 実際に配信したときしか発生しない不具合もある
        - 以下に該当するアクションは、特にテスト配信で確認した方が良い
            - サイト側のページ遷移をともなう
            - KARTEへのイベント送信をすることがある
            - ユーザー情報変数を使っている

### 6-1. ChromeデベロッパーツールのConsoleタブ
- バグの原因を特定する方法
    - プログラムを熟読する
    - [Chromeデベロッパーツール](https://developers.google.com/web/tools/chrome-devtools/?hl=ja)のConsoleタブを活用する

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/console_tab.png" width="800px">

- JavaScriptが思った通りに動作しないとき
    - Consoleにエラーが出ていないか確認する
        - 出ている場合は、その内容をヒントにバグ原因のあたりをつける
    - WidgetのScriptに`console.log("ログの内容")`を足す
        - プログラム中に分岐がある場合、どの分岐に入ったか調べる
        - 変数がある場合、その値を調べる

### 6-2. ワーク: バグのあるプログラムをデバッグする
- 初期表示ステートを指定した以下の部分に、バグを含めてみます
    - ifの条件内を以下のように修正します

```js
var lastState = [[lastState]];
if (lastState === '0') {
    widget.setState(1);
} else if (lastState === '2') {
    widget.setState(1);
} else {
    widget.setState(lastStata); // lastState -> lastStataにしてみる
```

- 「アクション再実行」ボタンを押しても接客が表示されないことを確認します
    - つまり、バグがある状態です
- [Chromeデベロッパーツール](https://developers.google.com/web/tools/chrome-devtools/?hl=ja)のConsoleタブを開きます
- 左上の🚫Clear Consoleボタンを押してConsoleをキレイにします
- 再び「アクション再実行」ボタンを押します
- 以下のように、エラーメッセージが表示されます

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/console_error.png" width="800px">

- エラーメッセージで最も大事なのは、最初の行です

```
karte.tracker error: ReferenceError: lastStata is not defined
```

- 「`lastStata`は定義されていません」というエラーから、変数名を間違えていたことがわかります
- 変数名を`lastState`に修正して、エラーが出なくなることを確認しましょう

### 6-3. ワーク: デバッグ用のログを出力する
- 今度は、以下のようにScriptに3つのログ出力処理を追加してみます

```js
var lastState = [[lastState]];
// 変数lastStateの値をログに出力
console.log("lastState: " + lastState);
if (lastState === '0') {
    // 分岐1に入ったことを知るためのログを出力
    console.log("分岐1です！");
    widget.setState(1);
} else if (lastState === '2') {
    // 分岐2に入ったことを知るためのログを出力
    console.log("分岐2です！");
    widget.setState(1);
} else {
    // 分岐3に入ったことを知るためのログを出力
    console.log("分岐3です！");
    widget.setState(lastState);
}
```

- 左上の🚫Clear Consoleボタンを押してConsoleをキレイにします
- 「アクション再実行」ボタンを押します
- 以下のように、ログが表示されます
    - 変数lastStateには、ユーザー情報変数の「プレビューの値」で指定した3が格納されていることがわかります
    - また、if文による分岐は「分岐3」に進んでいることがわかります

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/console_log.png" width="600px">

- このように、`console.log()`で動作を確かめたい処理の中にログをたくさん書くことで、プログラムの挙動をより詳細に知ることができます
- 最後に、デバッグに使ったログを削除します
    - エンドユーザーにログが見えてしまったり、開発者が見たいログに紛れてしまったりするため

---

ここから先は、Web版のKARTEのみで使える内容になります。

---

## 7. [for Web]サイト内ボタンの自動クリック
### 7-1. ワーク: ステート4のボタンをクリックしたとき、サイト内のある要素を自動でクリックさせる
- 配信対象のページを開きます
- ページ内から、リンクなどクリック時に処理が発生する要素を見つけます
- その要素を指定するCSSセレクタを特定します
    - 要素の上で右クリックし、「検証」を選択します
    - [Chromeデベロッパーツール](https://developers.google.com/web/tools/chrome-devtools/?hl=ja)が開きます
        - Elementsタブの中で、該当の要素に対応するHTMLタグがハイライトされます
    - ハイライトされたHTMLタグを右クリックし、「Copy > Copy selector」を選択します
    - クリップボードにその要素を指定するCSSセレクタがコピーされます

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner_v2/_images/copy_selector.png" width="600px">

- WidgetのScriptの末尾に、以下を追加します

```js
// 変数selectorに、CSSセレクタを格納
var selector = 'コピーしたCSSセレクタ';
// 変数elementに、対象のHTML要素を格納
var element = document.querySelector(selector);
// widgetのメソッドにautoClickを新規追加
widget.method('autoClick', function() {
    // 対象のHTML要素を自動クリック
    element.click();
    // widgetを閉じる
    widget.hide();
});
```

- HTMLを編集します
    - ステート4のボタンを、以下のHTMLで置き換えます

```html
<div class="yes karte-temp-btn karte-temp-hover" krt-on:click="autoClick()">#{btn4}</div>
```

- アクションを保存し、テスト配信で動作確認します
    - うまくいけば、ステート4のボタンを押すと、ページ内の指定した要素が自動クリックされます

### 7-2. サイト内要素の取得について
- documentオブジェクトを使うことで、ページそのものの状態を取得したり、ページを操作したりできる
    - KARTEの仕様ではなく、JavaScriptの仕様
- `document.querySelector('CSSセレクタ')`
    - CSSセレクタを指定して、ページ内の要素を取得する
    - 取得した要素は、JavaScriptの変数に格納しておき、後から使うことができる
- `要素.click()`
    - 取得した要素をプログラムからクリックさせる

```js
// 変数selectorに、CSSセレクタを格納
var selector = 'コピーしたCSSセレクタ';
// 変数elementに、対象のHTML要素を格納
var element = document.querySelector(selector);
// 対象のHTML要素を自動クリック
element.click();
```

- サイト内要素に依存したカスタマイズをすることには、一定のリスクがあります
    - サイト側の現在の仕様や今後の仕様変更によって不具合が発生する可能性がある
        - 常にあると思っていた要素が、一定の条件では表示されない仕様だった可能性
        - サイトが改修され、サイト内要素が取得できなくなる可能性

### 7-3. Widgetにメソッドを新規追加する
- `setState(n)`などの予めセットされたメソッド以外に、独自のメソッドをWidgetに追加することができます
- メソッドを追加するための記述方法
    - `widget.method('メソッド名', function() { 処理 })`

```js
// widgetのメソッドにautoClickを新規追加
widget.method('autoClick', function() {
    // 対象のHTML要素を自動クリック
    element.click();
    // widgetを閉じる
    widget.hide();
});
```

- 追加したメソッドは、`setState(n)`などと同様、WidgetのHTML要素と関連づけることができます
    - `krt-on:click=""`の値に指定します

```html
<div class="karte-temp-btn karte-temp-hover" krt-on:click="autoClick()">#{state2.btn3}</div>
```

## 8. [for Web]サイト内要素のクリックで接客を表示
- サイト内の既存の要素に対するUI操作をしたタイミングで、接客を表示することができます

### 8-1. ワーク: サイト内のある要素をクリックしたときにwidget.show()する
- Scriptの以下の部分をコメントアウトします
    - 対象範囲を選択して、以下のショートカット
        - Windows: `[Ctrl] + [/]`
        - Mac: `[Cmd] + [/]`

```js
// var lastState = [[lastState]];
// if (lastState === '0') {
//     widget.setState(1);
// } else if (lastState === '2') {
//     widget.setState(1);
// } else {
//     widget.setState(lastState);
// }
```

- 配信対象のページを開きます
- ページ内から、クリック時に処理が発生しない要素を見つけます
    - 実際には、接客表示用のボタンをサイト側に設置することを想定しています
- その要素を指定するCSSセレクタを特定します
    - 手順は、先ほどと同様です
- Scriptの冒頭に以下を追記します

```js
// 変数selector2に、CSSセレクタを格納
var selector2 = 'コピーしたCSSセレクタ';
// 変数element2に、対象のHTML要素を格納
var element2 = document.querySelector(selector2);

// 対象のHTML要素に、click時に実行される関数を登録
element2.addEventListener('click', function() {
    // widgetを表示
    widget.show();
});
```

- アクションを保存し、テスト配信で動作確認します
    - うまくいけば、サイト内の要素をクリックすると、接客が表示されます

### 8-2. HTMLとeventとイベントリスナー
- eventとは？(HTML/JavaScriptの仕様)
    - Webページ上でユーザーがある要素に対してクリックやスワイプなどの操作をした場合に、その要素に対して発生するもの
    - KARTEの「イベント」とは異なります
- 主なevent
    - click
        - クリック、タップされた
    - mouseover
        - マウスポインタが要素に重なった（PCのみ）
    - keydown
        - キーボードが押された
    - change
        - input要素の値が変更された
    - scroll
        - スクロールした
- イベントリスナーとは？(HTML/JavaScriptの仕様)
    - eventが発生した場合の処理を、要素に後から追加することができる
- イベントリスナの追加方法

```js
// 変数elementに、対象のHTML要素を格納
var element = document.querySelector('CSSセレクタ名');
// 対象のHTML要素に、event発生時に実行される関数を登録
element.addEventListener('event名', function() {
    // event発生時に実行される処理
});
```

### 8-3. 事例: ユーザーのページ操作に応じた接客表示
- スクロール率が一定を超えたら接客表示
  - [上へ戻るボタンをスクロールナビで表示し、快適な操作体験を](https://admin.karte.io/store/svc/58059584e6472d851ddf7904)
  - [記事ページ閲覧ユーザーにメディアの価値を伝えてスムーズに会員登録に誘導（ビズヒント）｜KARTE CX Clip｜KARTE CX Clip](https://cxclip.karte.io/practice/bizhint-case01/)
- マウスポインタがページ表示エリアから離脱したら接客表示
  - [離脱しそうなユーザーにオススメ商品をご紹介](https://admin.karte.io/store/svc/58192c6d8349239572374200)

### 8-4. 事例: 特定の操作をした人だけにKARTEでイベント送信
- スクリプト配信し、ページを下までスクロールした人だけに、読了を示すイベントを発火
    - セグメントなどで利用
    - [記事コンテンツ読了後にアンケートを表示し、データを行動分析に活用（一番搾りブランドサイト）｜KARTE CX Clip｜KARTE CX Clip](https://cxclip.karte.io/practice/kirin-case01/)
- ページ内の特定要素をクリックした人だけに、クリックイベントを発火
    - ただし、ページ遷移をともなう場合はイベント送信が間に合わないことがある

## おわりに
- 今回は、ステートの追加と変更によって、簡単な診断コンテンツを作成できることを紹介しました

### 今回の内容のおさらい
- 表示要素を増やす場合、HTMLのそれに対応する部分を複製します
    - 文言などを別々に設定するためには、静的変数も新たに作成する
    - 余白が崩れた場合は、CSSでmarginやpaddingを設定する
- ステートを増やす場合も、HTMLの既存のステート全体に対応する部分を複製します
    - `krt-if="state==n"`を指定した要素内は、ステートがnのときしか表示されません
    - `krt-on:click="setState(n)"`を指定した要素をクリックすると、ステートがnに遷移します
- ユーザー毎に表示を切り替える場合、KARTEにイベント送信しその値をユーザー情報変数として利用します
    - イベント送信するには、`tracker.track('イベント名', 値を示すオブジェクト)`を利用します
- JavaScriptのデバッグをするには、ChromeデベロッパーツールのConsoleタブを駆使します
    - 文法エラーがある場合は、Consoleにその旨が表示されます
    - `console.log('ログの内容')`をプログラム中に追加することで、プログラム実行時の変数の値や分岐の様子を知ることができます
- [for Web]サイト内要素と接客サービスを関連づけるには、ChromeデベロッパーツールでCSSセレクタを特定し、要素をJavaScriptで取得します
    - 要素の取得には、`document.querySelector('CSSセレクタ')`を使います
    - 要素の自動クリックは、`要素.click()`で実現できます
    - 要素に対してユーザーが操作をすると、対応するeventが発生します
        - event発生時の処理は、`要素.addEventListener('event名', function() { 処理 })`と書くことで追加できます

### 事後課題の確認
- https://github.com/plaidev/karte-school/blob/master/widget/beginner/post_homework.md

### 今後の学習
- プログラミング学習のコツは、失敗を恐れずにひたすら「try! try! try!」すること
    - 書籍やオンライン学習コンテンツで学ぶのは、後からでも遅くない
    - 「やってみる → 不明点にぶつかる → 調べる → わかる」のサイクルを回す
    - 学習効率を上げるには、わかる人にとにかく質問すること
        - KARTEのカスタマイズに関する質問は、Slackへ
- 信頼できる情報源を知る
    - [MDN](https://developer.mozilla.org/ja/docs/Web)
        - Firefoxブラウザを開発するMozillaが運営するサイト
        - HTML/CSS/JavaScriptを中心に、Webブラウザの標準仕様が紹介されている
    - [Qiita](https://qiita.com/)
        - プログラマ向けの技術情報共有サービス
        - ユーザー投稿型なので玉石混交だが、より実践的な調べ物に役に立つことがある
- おすすめ学習コンテンツ
    - 書籍
        - [『非エンジニアのためのJavaScript』](https://booth.pm/ja/items/1311888)
          - 講義終了後に slack にてPDFをお配りいたします。ぜひご一読ください。
        - [スラスラ読める JavaScript ふりがなプログラミング (ふりがなプログラミングシリーズ）](https://www.amazon.co.jp/dp/4295003859)
          - JavaScriptを順を追って見やすく解説しているドリルのように練習しながら学ぶ本です。非エンジニアにとても読みやすいテイストになっています。
        - [HTML&CSS&JavaScript辞典 第7版](https://www.amazon.co.jp/dp/4798050229/)
          - 講義中に参考図書としてご利用いただいた本です。HTML,CSS,JSを索引から調べることもできるので、今後も1冊お手元にあるとよいかも知れません。
    - オンラインコンテンツ
        - [ドットインストール](https://dotinstall.com/)
            - 3分動画で手軽に学べるプログラミング学習サービス
        - [Progate](https://prog-8.com/)
            - 初心者向けプログラミング学習サイト
