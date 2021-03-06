# Widget Customize 初級: 演習問題
- レッスンの内容を参考にしながら、以下の要件を満たすように接客サービスをカスタマイズしましょう
- テスト配信で動作を確認し、挙動に問題がある場合はデバッグしてみましょう

## 演習1: シンプルな診断コンテンツ
### 想定シナリオ
<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner/_images/exercise_shindan_design.png" width="800px">

### 最低限のカスタマイズ
以下の要件を満たすように、アクションをカスタマイズします

- [ ] 元のテンプレートは、以下
    - ユーザーに見せる > 旧テンプレート > ポップアップ > 「画像+テキスト+複数リンク（2リンク）( ID: C000028 )」
        - 「 ID: C000020 」と名前が同じなので注意

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner/_images/c000028.png" width="300px">

- [ ] 「ベーシック」タブの「アクション設定」で以下の変数の値を変更
    - [ ] 見出し: `商品カテゴリー診断！`
    - [ ] 詳細文: `あなたにおすすめの商品カテゴリーを診断しますか？`
    - [ ] ボタンA
        - [ ] ボタンテキスト: `はい`
        - [ ] ボタン背景色: 赤っぽい色
    - [ ] ボタンB
        - [ ] ボタンテキスト: `いいえ`
        - [ ] ボタン背景色: 青っぽい色

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner/_images/exercise_state1.png" width="300px">

- [ ] ステート2を追加
    - [ ] 見出し: `自分磨きが好きだ`
    - [ ] 詳細文は不要なので、要素ごと削除

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner/_images/exercise_state2.png" width="300px">

- [ ] ステート1の「はい」を押したらステート2に遷移する
- [ ] ステート2と同様に、ステート3と4を追加
    - [ ] ステート3
        - [ ] 見出し: `健康が気になる`
    - [ ] ステート4
        - [ ] 見出し: `自分はインドア派だ`
- [ ] ステート2からの遷移を実装
    - [ ] 「はい」 → `ステート3`
    - [ ] 「いいえ」 → `ステート4`
- [ ] ステート5を追加
    - [ ] 見出し: `健康志向のあなたにはヘルスケアがおすすめ`
    - [ ] 詳細文: `あなたにぴったりのヘルスケア商品をチェック！`
    - [ ] ボタンは、リンク1つのみ（この時点では、スタイルが崩れる）
        - [ ] ボタンテキスト: `ヘルスケアカテゴリへ`
        - [ ] リンク先URL: `#healthcare` （適当なURLがある場合はそれを設定）

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/widget/beginner/_images/exercise_state5.png" width="300px">

- [ ] ステート5と同様に、ステート6〜8を追加
    - [ ] ステート6
        - [ ] 見出し: `おしゃれなあなたにはアクセサリーがおすすめ`
        - [ ] 詳細文: `あなたにぴったりのアクセサリーをチェック！`
        - [ ] ボタンテキスト: `アクセサリーカテゴリへ`
        - [ ] リンク先URL: `#accessories` （適当なURLがある場合はそれを設定）
    - [ ] ステート7
        - [ ] 見出し: `インドア派のあなたにはインテリアがおすすめ`
        - [ ] 詳細文: `あなたにぴったりのインテリア商品をチェック！`
        - [ ] ボタンテキスト: `インテリアカテゴリへ`
        - [ ] リンク先URL: `#interior` （適当なURLがある場合はそれを設定）
    - [ ] ステート8
        - [ ] 見出し: `アウトドア派のあなたにはアウトドアがおすすめ`
        - [ ] 詳細文: `あなたにぴったりのアウトドア商品をチェック！`
        - [ ] ボタンテキスト: `アウトドアカテゴリへ`
        - [ ] リンク先URL: `#outdoor` （適当なURLがある場合はそれを設定）
- [ ] 残り全てのステート遷移を実装
    - [ ] ステート3
        - [ ] 「はい」 → `ステート5`
        - [ ] 「いいえ」 → `ステート6`
    - [ ] ステート4
        - [ ] 「はい」 → `ステート7`
        - [ ] 「いいえ」 → `ステート8`
- [ ] ステート1の「いいえ」ボタンを押したら、widgetを閉じる

### 追加のカスタマイズ
最低限のカスタマイズができた場合は、以下のカスタマイズも試してみましょう

- [ ] ステート5〜8のボタンが左に寄っているのを直す
    - [ ] 既存のスタイルを壊さないように、ステート5〜8のボタン全てに、共通の新しいclassを付与します
        - 例) `result-link`
    - [ ] 追加したclassに適用するためのスタイルを、CSSの末尾に記載します
        - [ ] 上書きして適用すべきスタイルは、次の通り
            - [ ] `display`プロパティ
                - [ ] 値: `block`
            - [ ] `width`プロパティ
                - [ ] 値: `100%`
            - [ ] `margin`プロパティ
                - [ ] 値: `0`
- [ ] ステートそれぞれの質問や診断結果に適した画像を設定できるようにする
    - [ ] 実際に、それっぽい画像を設定してみましょう
- [ ] 最終結果が出たユーザーに対して、結果をイベント送信する
    - [ ] イベント名: `test`
    - [ ] フィールド名: `category_shindan_result`
    - [ ] 値: 文字列の`accessories` or `healthcare` or `interior` or `outdoor`
- [ ] 最終結果がすでに出ているユーザーに対しては、初期表示時にそのユーザーに対応する最終結果を再表示する
    - たとえば、診断結果が「アクセサリーカテゴリ」になったユーザーが再来訪したら、ステート5を初期表示する
    - カスタマイズ後は一度回答してしまうとステート1に戻せなくなるので、再度検証する場合は追加した処理を一度コメントアウトしましょう
- [ ] その他、気になる部分があれば直してみましょう

### 演習1のヒント
- ステートを増やす場合は、一つ前のステートのHTML要素をコピーしてから、差分だけを変更しましょう
- ステート毎に別々の見出しやリンク先を設定したい場合は、対応する「静的変数」を追加します
- `setState()`以外のメソッドは、基本的にはHTMLの`krt-on:click`などから直接呼び出すことはできません
- スクリプトを編集した場合は、「アクション再実行」ボタンを押してから動作検証します

# 補足: 受講後のサポート

- 受講後2週間限定で、指定のslackチャンネルにてカスタマイズのご質問をお受けいたします！
  - ぜひこの間に、上記課題や以前から実現したかったカスタマイズに挑戦し、どしどしご質問ください
  - なお、通常のKARTEチャットサポートでは引き続きカスタマイズに関するご質問は受け付けておりません。カスタマイズに関するご質問はslackにてお待ちしております。
- 質問する際は、slackチャンネルにて @homework へメンションしてください
  - 講師を含むEducation Team Memberが参加しています
- ご質問いただく際は、接客サービスのURLを貼り付けていただけるとよりスムーズにコミュニケーションが取れるのでご協力ください
  - なお、共有されたURLはPLAIDメンバーのみ閲覧でき、ほかのクラス参加者はURL先に遷移できず、中身を閲覧できませんのでご安心ください
  - PLAIDメンバーは、お客様のプロジェクトの閲覧のみ可能で編集等を行う権限を持っておりません。コメントの書き込み等もできないため、ご安心ください。
