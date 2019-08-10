# KARTE School: Widget Customize 初級
## はじめに
### インストラクター紹介
- https://github.com/plaidev/karte-school/tree/master/_instructors

### このコースのゴール
- Widgetのカスタマイズによってできることを知る
- 簡単なWidgetのカスタマイズができるようになる

### このコースの特徴
- このコースは、非エンジニアの方の受講を想定したものです
- Widgetのカスタマイズを実践する中で、HTML/CSSと、特にJavaScriptを学ぶことができます
- JavaScriptの文法解説よりも、「JavaScriptを書くと何ができるか」を体感することを重視しています

### テスト配信環境の確認
- ご自身のKARTE環境で、自分だけに本番環境でテスト配信をすることができるかをご確認ください
    - 「テスト配信用セグメント」を使った配信制御が一般的です
    - セグメント設定例:
        - 「すべての期間 手動ラベル ラベル の 最新の値 が 〇〇 に等しい」

## Widgetとは何か？
- KARTEでは、ブラウザやネイティブアプリにHTML/CSS/JavaScriptで記述されたコンテンツを配信できます
- Widgetとは、こうしたコンテンツの実装をより簡単にするためのKARTEの機能です
- HTML/CSS/JavaScriptをそのまま記述するよりも、以下の点が簡単になっています
    - コンテンツのステート（状態）管理
    - HTML（表示）とJavaScript（データや処理）との関連付け

### Widgetタイプの接客サービスかどうかを見分ける方法
- ブラウザやアプリ内で実行される接客サービスでだけ使用されます
- ステートに関する表示がある場合、Widgetが使われています

## 要素を増やす
- カスタマイズする接客サービスを新規作成します
    - 使用するテンプレート
        - ユーザーに見せる > 旧テンプレート > ポップアップ > 「最小化-テキスト + ボタン（SC000018）」
    - 必ず、テスト配信用セグメントによる絞り込みをしてください
    - その他の配信設定
        - アクセス毎
        - 「同時配信を有効にする」にチェック

### ワーク: ボタンを3つに複製
- アクション編集画面のカスタマイズタブを開きます
- HTMLの以下の部分を、3行に複製します

```html
<a class="karte-temp-btn karte-temp-hover" href="#{state2.link}">#{state2.btn}</a>
<a class="karte-temp-btn karte-temp-hover" href="#{state2.link}">#{state2.btn}</a>
<a class="karte-temp-btn karte-temp-hover" href="#{state2.link}">#{state2.btn}</a>
```

- 別々の変数で文言を設定できるように、`state2.btn`という静的変数を3つに増やします
    - `state2.btn`の変数名を`state2.btn1`に、表示名を`ボタンテキスト1`に変更
    - 変数を2つ追加
        - `state2.btn2`
        - `state2.btn3`
- HTMLの変数参照側も変更します

```html
<a class="karte-temp-btn karte-temp-hover" href="#{state2.link}">#{state2.btn1}</a>
<a class="karte-temp-btn karte-temp-hover" href="#{state2.link}">#{state2.btn2}</a>
<a class="karte-temp-btn karte-temp-hover" href="#{state2.link}">#{state2.btn3}</a>
```

- 静的変数「ボタンテキスト1〜3」の値をベーシックタブから変更し、プレビューに反映されることを確認します

### 静的変数の使い方
- 「静的変数」というのは、KARTEの用語
- HTML/CSS/JavaScriptの中でよく変更する部分を静的変数にし、ベーシックタブから簡単にその値を変更できるようにする
- HTMLなどから参照する方法
    - `#{変数名}`
        - 例: `#{margin}`
    - `#{フォルダ名.変数名}`
        - 例: `#{state2.btn1}`

### 代表的なHTMLタグ
- HTMLタグはたくさんありますが、役割が決まっているタグ以外は、ほぼ同じです
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
- 役割が決まっているHTMLタグを除けば、divとspanだけを覚えれば大抵の表現は可能

### コラム: HTMLタグとセマンティクス
- なぜHTMLタグの種類はこんなにたくさんあるのか？
- HTMLタグの種類が多い理由
    1. 歴史的な経緯
        - もともと、HTML内でスタイルを指定するのが一般的だった
        - スタイルを指定するためのHTML要素があった
            - iタグ: イタリック
            - uタグ: アンダーライン
            - bタグ: 太字
        - その後、CSSの登場によりHTMLでスタイルを指定する必要が無くなった
    2. セマンティクスのため
        - 各要素のドキュメント上の意味を伝えてあげること
        - セマンティクスに配慮することで、人間には見た目が同じでも、プログラムにより良く意味を伝えられるようになる
            - SEO、アクセシビリティ向上
        - 主なセマンティック要素
            - h1〜6タグ: 見出し
            - pタグ: 段落
            - sectionタグ: セクション
            - headerタグ、footerタグ: ヘッダー、フッター

### チャレンジ: 詳細文も増やしてみる
- 詳細文を2つに増やしてみましょう
    - 内容を設定する静的変数も、2つにしてみます

## 余白を調整する
### ワーク: CSSによるmarginの調整
- 3つに増やしたボタンの間の余白を追加します
- ボタンに対応するHTML要素のclass属性を特定します
    - `karte-temp-btn karte-temp-hover`
- CSSタブの中から、`karte-temp-btn`に対応するスタイル指定を見つけて加筆します

```css
.karte-temp-btn {
    margin-top: 10px;
    /* 中略 */
}
```

### HTMLのclass属性とCSS
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

### CSSボックスモデル
- CSSで実現されるWeb上のレイアウトのルールは、「ボックスモデル」と呼ばれています
    - すべての要素が長方形のボックスとして表される

<img src="" width="300px">
出典: https://developer.mozilla.org/ja/docs/Learn/CSS/Introduction_to_CSS/Box_model

### 代表的なCSSプロパティ
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
- 文字の装飾に関するプロパティ
    - color
        - 文字色
    - font-size
        - 文字サイズ
    - text-decoration
        - 下線など

### チャレンジ: 詳細文に下線を引いてみる
- 詳細文に対応するHTML要素のclass名を特定します
- CSSを加筆し、詳細文に下線を表示します
    - 指定方法は、Googleで検索してください
