# もっとユーザーについて知るためのデータ分析SQL入門: レッスン
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
- SQLなどの簡単なデータ分析スキルがあるとどんな情報を集計することができるようになるか、理解する
- たとえば、KARTE Datahubで計測しているWebの閲覧データから、下記を抽出できるようになる
    - 「ある特定期間のアクセスについて、期間中に3セッション以上来ているユーザーの5PV以上閲覧したセッションの閲覧ページパスを時系列で抽出する」

### このコースの特徴
- このコースは、非エンジニアの方の受講を想定したものです
- SQLについては、細かい文法解説よりも、「SQLを書くと何ができるか」を体感することを重視しています

### 事前準備の確認
- https://github.com/plaidev/karte-school/blob/master/data_analysis/pre_homework.md

## 1. 「もっとユーザーについて知る」ために、なぜデータ分析スキルが必要か？
- KARTEの機能を使っても、決まった切り口であれば、かなりユーザーについて知ることができます
    - ユーザー詳細画面
        - 特定のユーザーが過去にどんな行動をしていたか？
    - セグメント機能
        - ユーザーデータ（過去に発生したイベントの統計値）が、ある条件に合致しているかどうか？
    - 接客サービス詳細画面
        - 特定の接客サービスに対してユーザーがどんな反応をするか？
    - ベーシックレポート
        - よく使われる軸で分析した結果のダッシュボード
- しかし、たとえば下記のようにより進んだ分析をする場合、独自にデータを集計する必要があります
    - 「ページA -> B -> C」の順番でページ遷移したユーザーを抽出したい
    - CVしたユーザーがその直前によく見ているページ群を抽出したい
    - 特定の接客サービスをクリックしたユーザーとしていないユーザーで、その10〜20日後のアクセス頻度を比較したい
- 自分の関心のある軸でユーザーを探したり、ユーザー行動の傾向を掴んだりしたい場合、SQLを中心とするデータ分析スキルが必要になる

## 2. Webサイトやネイティブアプリのユーザーを知るために使われるデータ
- Webサイトやネイティブアプリのユーザーを知るためには、ユーザーの行動ログ（イベントデータ、アクセスログ）が主に利用されています
    - 「Aさんのアクセス頻度が急に上がったので、サイトを好きになってくれたのかもしれない」
    - 「Aさんは猫好きの人がよく見るページを見ているので、同様に猫が好きかもしれない」
    - 「Aさんは毎月15日頃になると商品をよく購入するので、Aさんの給料日は15日かもしれない」
- また、ユーザーの属性情報が取得できている場合、行動ログに含まれるuser_idを使って名寄せすることで、行動ログと属性情報を合わせて分析することもできます。
- このコースでは、「事前準備」で使用した下記のサンプルデータを用いて、データ分析を体験します

csvファイル名 | 概要
-- | --
`access_logs.csv` | Webのアクセスログ
`users.csv` | ユーザーの属性情報

## 3. まずはGoogleスプレッドシートでアクセス集計をやってみる
- Webのアクセスログ自体に慣れるために、まずはGoogleスプレッドシートを使ってサンプルデータを集計してみます
- 事前準備で用意したGoogleスプレッドシートを開いてください

### 3-1. 演習: アクセスログの全体集計値を出してみる
- Googleスプレッドシートの`access_logs`にあるデータを使って、下記のデータを集計してみましょう（期間は全期間）
    1. ページビュー(PV)数
    2. セッション数
    3. ユニークユーザー(UU)数
    4. 1セッションあたりの平均PV数
    5. 1ユーザーあたりの平均PV数
    6. 1ユーザーあたりの平均セッション数
- ヒント
    - 元のデータを壊さないために、新しくシートを追加し、そこに結果を表示しましょう
    - ユニークな数を集計するために、スプレッドシートには`=COUNTUNIQUE()`という便利な関数が用意されています
- 追加の演習
    - pathも`=COUNTUNIQUE()`でCOUNTしてみましょう。その数にはどんな意味があるでしょうか？

### 3-2. 演習: アクセスログのユーザー毎の集計
- Googleスプレッドシートの`access_logs`にあるデータを使って、下記のデータを集計してみましょう（期間は全期間）
    1. ユーザー毎のPV数
    2. ユーザー毎のセッション数
- ヒント
    - ピボットテーブルを使うのは、1つの方法です
        - [データ > ピボットテーブル]
- 追加の演習
    - PV数が20以上のユーザーのuser_idの一覧を特定してみましょう
    - ユーザー毎のセッション数は、最大でいくつでしょうか？

### 3-3. 演習: アクセスログの性別毎の集計
- Googleスプレッドシートの`access_logs`と`users`にあるデータを組み合わせて、下記のデータを集計してみましょう（期間は全期間）
    1. 性別毎のPV数
    2. 性別毎のセッション数
    3. 性別毎のUU数
- ヒント
    - `access_logs`に`gender`列を追加し、`=VLOOKUP()`関数で`users`の値を
    - あとは、一つ前のワークと同様です
- 追加の演習
    - 年齢毎のPV数、セッション数、UU数も同様に集計してみましょう

## 4. 同じアクセス集計をSQLでやってみる
- Googleスプレッドシートで実施したのと同じ操作を、KARTE DatahubのSQLでやってみましょう
- 事前準備で用意したDatahubのデータセット画面を開きます

### 4-1. ワーク: access_logsテーブルをそのまま抽出する
- access_logsテーブルの内容を、全件そのままSQLで抽出してみます

sync_date | user_id | session_id | origin | path
-- | -- | -- | -- | --
2020-01-01T05:57:07.008Z | vis-MZPE | vis-MZPE_1 | https://cxclip.karte.io | /
2020-01-01T11:49:59.719Z | vis-G6N3 | vis-G6N3_1 | https://cxclip.karte.io | /
... | ... | ... | ... | ...


- [データセット > karte_school_{{api_key}} > access_logsテーブル]の画面を開きます
- 右上の[クエリを作成]ボタンを押します
- 末尾の`LIMIT 1000`を削除します

```sql
SELECT * FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
```

- 右上の[クエリを実行]ボタンを押します
    - access_logsテーブルを全件抽出できていることを確認
- `SELECT`と`FROM`の間を書き換え、下記のようにします

```sql
SELECT
  sync_date
  , user_id
  , session_id
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
```

- クエリを再度実行し、今度は指定した3つの列だけが出力されていることを確認します

sync_date | user_id | session_id
-- | -- | --
2020-01-01T05:57:07.008Z | vis-MZPE | vis-MZPE_1
2020-01-01T11:49:59.719Z | vis-G6N3 | vis-G6N3_1
... | ... | ...

- クエリの名称を変更し、名称を保存します
    - クエリ名
        - `access_logs抽出`
- 右上の[保存]ボタンを押してクエリを保存します
- 左上の[フォルダを作成]ボタンを押します
    - フォルダ名を入力します
    - 例
        - `KARTE School: SQL練習用`
- クエリを作成したフォルダに移動します
    - これ以降、作成したクエリは適宜このフォルダに格納しましょう

### 4-2. 演習: usersテーブルをそのまま抽出する
- 下記のクエリを作成します
    - usersテーブルから、下記の列だけを全件抽出
        - `user_id`
        - `age`
    - クエリ名
        - `users抽出`

### 4-3. SELECT 列名 FROM テーブル名
- SELECT文
    - SQLの中でも、データを特定の条件で抽出するときに実行する命令です
    - 元のデータに変更を加えずにデータ分析ができます
    - 今回のコースでは、このSELECT文しか使いません
- 列名
    - `SELECT`の直後にカンマ区切りで列の名前を書くことで、その列だけを結果に含めることができます
- FROM テーブル名
    - 対象のテーブルを指定するには、`FROM`の直後にテーブル名を書きます
    - Datahubのテーブルは下記のような記述をバッククォート囲みで指定します
        - `prd-karte-per-client.データセット名.テーブル名`
        - 例
            - `prd-karte-per-client.karte_school_3a2df8517d95013680fb7a93446fb92a.access_logs`
        - データセット名は競合を避けるために自動でapi_keyが入ります
            - その部分を`{{api_key}}`と書くこともできます
            - 例
                - `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
    - テーブル名指定を手打ちするのは難しいので、テーブル画面の[クエリを作成]ボタンから作成するのがおすすめです

### 4-4. ワーク: access_logsテーブル全体を集計する
- 下記を集計します
    1. ページビュー(PV)数
    2. セッション数
    3. ユニークユーザー(UU)数
    4. 1セッションあたりの平均PV数
    5. 1ユーザーあたりの平均PV数
    6. 1ユーザーあたりの平均セッション数

pv | session | uu | pv_per_session | pv_per_uu | session_per_uu
-- | -- | -- | -- | -- | --
5213 | 3397 | 2973 | 1.5345893435384161 | 1.753447695930037 | 1.1426168853010428

- 下記の内容でクエリを新規作成し、実行します

```sql
SELECT
  COUNT(*) AS pv
  , COUNT(DISTINCT session_id) AS session
  , COUNT(DISTINCT user_id) AS uu
  , COUNT(*) / COUNT(DISTINCT session_id) AS pv_per_session
  , COUNT(*) / COUNT(DISTINCT user_id) AS pv_per_uu
  , COUNT(DISTINCT session_id) / COUNT(DISTINCT user_id) AS session_per_uu
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
```

- クエリ名を変更します
    - `全体access_logs集計`

- 作成したクエリはこのコース用のクエリフォルダに格納します

### 4-5. COUNT()で数を集計する
- 表示する列名の代わりにただ`COUNT()`と書くと、全レコードを集計して1行を出力してくれます
- `COUNT()`の括弧の中で、集計方法を指定できます
    - `COUNT(*)`
        - レコード数を集計
    - `COUNT(DISTINCT 列名)`
        - 列名に指定した列の値について、重複を削除したユニークな種類数を集計
        - スプレッドシートの`=COUNTUNIQUE()`関数と同じ
        - 例
            - `COUNT(DISTINCT session_id)`
            - `COUNT(DISTINCT user_id)`
- `AS`で出力結果に表示するカラム名を指定できます
    - 指定しない場合、`f0_`などの適当な値が使われることがあるので、なるべくわかりやすい名前を指定しましょう
- 数値や結果が数値になる式の場合、SQLの中で四則演算することもできます
    - 例
        - `3 + 7 AS ten`
        - `COUNT(DISTINCT session_id) / COUNT(DISTINCT user_id) AS session_per_uu`

### 4-6. 演習: pathに関する集計
- 「全体access_logs集計」クエリの出力結果に下記の列を追加します
    - `path_count`
        - pathのユニークな種類数
    - `pv_per_path`
        - 1pathあたりの平均PV数

### 4-7. ワーク: ユーザー毎にaccess_logsテーブルを集計する
- 下記を集計します
    1. ユーザー毎のPV数
    2. ユーザー毎のセッション数

user_id | pv | session
-- | -- | --
vis-0D5F | 1 | 1
vis-4ZM0 | 1 | 1
vis-992H | 8 | 7
... | ... | ...

- 下記の内容でクエリを新規作成し、実行します

```sql
SELECT
  user_id
  , COUNT(*) AS pv
  , COUNT(DISTINCT session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
GROUP BY user_id
```

- クエリ名を変更します
    - `ユーザー毎のaccess_logs集計`
- 作成したクエリはこのコース用のクエリフォルダに格納します

### 4-8. GROUP BYでグループ別に集計する
- `GROUP BY 列名`を使うと、指定した列の値が同じレコードを「グループ」として集約してくれます
    - グループ毎に1行が出力されます
    - 例
        - `GROUP BY user_id`
            - user_idが同じレコードを「グループ」として束ねて、それぞれ1レコードに集約する
- `COUNT()`と`GROUP BY`を組み合わせると、全体ではなく、グループ毎の集計結果を出力できます


sync_date | user_id | session_id
-- | -- | --
2020-01-06T05:17:58.922Z | vis-5O1N | vis-5O1N_1
2020-01-06T14:17:22.520Z | vis-5O1N | vis-5O1N_4
2020-01-08T02:47:57.148Z | vis-5O1N | vis-5O1N_7
2020-01-08T06:44:32.736Z | vis-E30K | vis-E30K_4
2020-01-09T05:09:14.669Z | vis-E30K | vis-E30K_5
2020-01-09T05:09:21.634Z | vis-E30K | vis-E30K_5
2020-01-09T06:54:38.593Z | vis-E30K | vis-E30K_6
2020-01-07T15:26:22.100Z | vis-Q8QF | vis-Q8QF_1
2020-01-07T15:27:39.862Z | vis-Q8QF | vis-Q8QF_1

- ↓`COUNT() & GROUP BY user_id`↓

user_id | pv | session
-- | -- | --
vis-5O1N | 3 | 3
vis-E30K | 4 | 3
vis-Q8QF | 2 | 1

### 4-9. 演習: path毎の集計
- 下記のクエリを作成します
    - access_logsテーブルから、下記の列を集計
        - `path`
        - `pv`
            - path毎のPV数
    - クエリ名
        - `path毎のaccess_logs集計`

### 4-10. ワーク: 性別毎にaccess_logsテーブルを集計する
- 下記を集計します
    1. 性別毎のPV数
    2. 性別毎のセッション数
    3. 性別毎のUU数

gender | pv | session
-- | -- | --
man | 2136 | 1359
woman | 3077 | 2038

- 下記の内容でクエリを新規作成し、実行します

```sql
SELECT
  users.gender AS gender
  , COUNT(*) AS pv
  , COUNT(DISTINCT logs.session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs` AS logs
INNER JOIN `prd-karte-per-client.karte_school_{{api_key}}.users` AS users
  ON logs.user_id = users.user_id
GROUP BY users.gender
```

- クエリ名を変更します
    - `性別毎のaccess_logs集計`
- 作成したクエリはこのコース用のクエリフォルダに格納します

### 4-11. JOINで複数テーブルを参照する
- 性別の情報はaccess_logsテーブルには存在していないため、usersテーブルも合わせて抽出する必要があります
- スプレッドシートでは`=VLOOKUP()`で外部テーブルを参照しました
- SQLでは、`JOIN`を使用することで、複数テーブルを結合した結果を「仮想的なテーブル」として利用できます
    - JOINにはいくつか種類がありますが、ここでは`INNER JOIN`を使用します
- `JOIN`の直後には、`FROM`の直後と同様に、テーブル名を指定します
    - `FROM メインのテーブル名 INNER JOIN 結合するテーブル名`
    - `AS`を使うと、テーブルに一時的な通称を付けることができます
        - 例
            - `AS logs`
            - `AS users`
- `JOIN`を使う場合、名寄せの方法を`ON`の後に指定します
    - それぞれのテーブルで共通している列をイコールでつなぎます
    - 例
        - `ON logs.user_id = users.user_id`
- logsテーブル

sync_date | user_id | session_id | origin | path
-- | -- | -- | -- | --
2020-01-01T05:57:07.008Z | vis-MZPE | vis-MZPE_1 | https://cxclip.karte.io | /
2020-01-01T11:49:59.719Z | vis-G6N3 | vis-G6N3_1 | https://cxclip.karte.io | /
... | ... | ... | ... | ...

- usersテーブル

user_id | age | gender
-- | -- | --
vis-MZPE | 28 | woman
vis-G6N3 | 20 | woman
... | ... | ...

- INNER JOIN後の「仮想的なテーブル」
    - 列名が被る可能性があるので、内部的に`テーブル名.列名`という列名に変換されます

logs.sync_date | logs.user_id | logs.session_id | logs.origin | logs.path | users.age | users.gender
-- | -- | -- | -- | -- | -- | --
2020-01-01T05:57:07.008Z | vis-MZPE | vis-MZPE_1 | https://cxclip.karte.io | / | 28 | woman
2020-01-01T11:49:59.719Z | vis-G6N3 | vis-G6N3_1 | https://cxclip.karte.io | / | 20 | woman
... | ... | ... | ... | ... | ... | ...

- JOINした結果の「仮想的なテーブル」は、通常のテーブルと同様にSELECT文で集計することができます
- `INNER JOIN`で名寄せのキーとして利用した列の値が片方のテーブルにしかない場合、そのキーに対応するレコードは出力されません
    - これを利用して、テーブルAのレコードを、テーブルBに存在しているuser_idだけに絞り込んで出力する、といったことができます

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/data_analysis/_images/inner_join.png" width="300px">

### 4-12. 演習: 年齢毎にaccess_logsテーブルを集計する
- 下記のクエリを作成します
    - access_logsテーブルから、下記の列を集計
        - `age`
        - `pv`
            - 年齢毎のPV数
        - `session`
            - 年齢別のセッション数
        - `uu`
            - 年齢別のUU数
    - クエリ名
        - `年齢毎のaccess_logs集計`

### 4-13. SQLでデータを加工することの価値
- SQLを使わなくても、生データをCSVファイルでダウンロードできれば、スプレッドシートでデータ分析ができます
- 一方、SQLを使うことで、下記のような恩恵を受けられます
    - データの抽出条件や加工方法をSQLでコード化できる
        - ロジックを調整しながら、何度でも実行できる
        - 他の人と簡単に共有できる
    - データベースが変わっても、ほぼ同様のSQLを実行することができる
        - スプレッドシートの独自関数を覚えなくていい
        - 汎用的なスキルとしてのSQL

## 5. SQLの抽出結果を加工して見やすくする
- 「ユーザー毎のaccess_logs集計」のクエリ結果をより見やすくするために、下記のように加工してみます
    - PV数が多い順に並び替える
    - 上位5位のみ出力する
    - 特定のユーザーの結果だけを抽出する
    - 5セッション以上閲覧しているユーザーの結果だけを抽出する

### 5-1. ワーク: 出力結果を並び替える
- 「ユーザー毎のaccess_logs集計」のクエリを編集し、PV数の降順で並び替えます

user_id | pv | session
-- | -- | --
vis-PS3T | 61 | 11
vis-6SBU | 39 | 2
vis-5JAW | 35 | 2
... | ... | ...

- 末尾に`ORDER BY pv DESC`を追加し、実行します

```sql
SELECT
  user_id
  , COUNT(*) AS pv
  , COUNT(DISTINCT session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
GROUP BY user_id
ORDER BY pv DESC
```

- 次に、セッション数の降順で並び替えます
    - `ORDER BY pv DESC`の前に`-- `と書いてコメントアウトします
    - 代わりに`ORDER BY session DESC`を追加し、実行します

```sql
SELECT
  user_id
  , COUNT(*) AS pv
  , COUNT(DISTINCT session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
GROUP BY user_id
-- ORDER BY pv DESC
ORDER BY session DESC
```

- クエリを保存します

### 5-2. ORDER BYによる並び替え
- `ORDER BY 列名`を使うことで、指定した列を用いて出力結果を並び替えることができます
    - スプレッドシートのフィルタ&並び替え機能と同様
- デフォルトでは昇順になりますが、`ORDER BY 列名 DESC`と書くことで降順を指定できます
    - `ASC`
        - 昇順（デフォルト）
        - ascending orderの略
    - `DESC`
        - 降順
        - descending orderの略

### 5-3. SQLのコメントアウト
- コメント
    - 実行時に特に意味を持たないメモのこと
    - SQLでは、下記のように記述します
        - `-- コメント`
    - SQLに限らず、HTML/CSS/JavaScriptなどでもそれぞれの記述方法があります
- コメントアウト
    - プログラムの一部をコメント化して一時的に除外すること
    - 後でまた使うかもしれない部分をとっておくために使いましょう
    - 例
        - `-- ORDER BY pv DESC`
- コメントアウトのショートカットキー
    - コメントアウトしたい行にカーソルを置くか、複数行を選択した状態で、下記を同時押しします
        - Windows
            - `Ctrl + /`
        - Mac
            - `Cmd + /`

### 5-4. ワーク: 出力結果に表示する行数を絞って、上位5位のみ表示する
- 「ユーザー毎のaccess_logs集計」のクエリを編集し、PV数の上位5位だけを出力するようにします

user_id | pv | session
-- | -- | --
vis-PS3T | 61 | 11
vis-6SBU | 39 | 2
vis-5JAW | 35 | 2
vis-6TGK | 33 | 1
vis-O2OP | 32 | 6

- `ORDER BY session DESC`をコメントアウトします
- `ORDER BY pv DESC`のコメントアウトを戻します
- 末尾に`LIMIT 5`を追加し、実行します

```sql
SELECT
  user_id
  , COUNT(*) AS pv
  , COUNT(DISTINCT session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
GROUP BY user_id
ORDER BY pv DESC
-- ORDER BY session DESC
LIMIT 5
```

- クエリを保存します

### 5-5. LIMITで出力する行数を指定する
- `LIMIT`を使うことで、出力結果に含める行数を指定することができます
- `ORDER BY`と`LIMIT`を組み合わせて使うことで、「上位N件」などの表示が可能です
    - 例
        - `ORDER BY pv DESC LIMIT 5`
- ちなみに、Datahubのテーブル画面で[クエリを作成]ボタンを押した場合に自動で作成されるクエリにも、末尾に`LIMIT 1000`と記述されていました
    - 巨大なテーブルに対して不用意に重いクエリを実行してしまうことを避けるために、自動で`LIMIT`が付与されます

### 5-6. 演習: アクセスログの最新の10行を出力する
- 「access_logs抽出」のクエリを編集し、sync_dateが最も新しい10行だけを出力するようにしましょう
- 今度は、最も古い10行だけを出力してみましょう
- 編集したクエリは保存します

### 5-7. ワーク: 特定のユーザーの集計結果だけに絞り込む
- まず、特定のユーザーのaccess_logsをそのまま抽出してみます
    - ここでは、user_idが`vis-2JKE`の行を抽出します

user_id | sync_date | session_id | path
-- | -- | -- | --
vis-2JKE | 2020-01-02T12:40:54.747Z | vis-2JKE_27 | /
vis-2JKE | 2020-01-02T14:29:18.812Z | vis-2JKE_28 | /
vis-2JKE | 2020-01-03T13:33:12.252Z | vis-2JKE_29 | /
... | ... | ... | ...

- 下記の内容でクエリを新規作成し、実行します

```sql
SELECT
  user_id
  , sync_date
  , session_id
  , path
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
WHERE user_id = "vis-2JKE"
ORDER BY sync_date
```

- クエリ名を変更します
    - `特定ユーザーの抽出・集計`
- 作成したクエリはこのコース用のクエリフォルダに格納します
- 次に、「ユーザー毎のaccess_logs集計」のクエリを参考に、user_idが`vis-2JKE`の行だけを集計してみます

user_id | pv | session
-- | -- | --
vis-2JKE | 23 | 17

- 元のクエリを全てコメントアウトし、集計用のクエリを追記します

```sql
-- SELECT
--   user_id
--   , sync_date
--   , session_id
--   , path
-- FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
-- WHERE user_id = "vis-2JKE"
-- ORDER BY sync_date

SELECT
  user_id
　, COUNT(*) AS pv
  , COUNT(DISTINCT session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
WHERE user_id = "vis-2JKE"
GROUP BY user_id
```

- クエリを実行し、結果を確認します
- クエリを保存します

### 5-8. WHEREで抽出する対象を絞り込む
- `WHERE`を使うことで、抽出・集計する対象の行を絞り込むことができます
- `WHERE`の直後には、結果がTRUEかFALSEになるような条件式を記述します
    - 例
        - `WHERE user_id = "vis-2JKE"`
        - `WHERE age < 30`
        - `WHERE age >= 20`
- 条件式は、`AND`や`OR`でつなげることもできます
    - 例
        - `WHERE age >= 20 AND age < 30`
        - `WHERE user_id = "vis-2JKE" OR user_id = "vis-NALM"`
- 条件式は、`NOT`でTRUEとFALSEを反転させることができます
    - 例
        - `WHERE NOT user_id = "vis-2JKE"`

### 5-9. 演習: 年齢や性別でusersを絞り込む
- 「users抽出」のクエリを順次編集して実行し、下記の条件で絞り込んで表示してみましょう
    - 「22歳未満」のユーザー
    - 「50歳以上」かつ「女性」のユーザー
    - 「user_idがvis-PS3T, vis-6SBU, vis-5JAWのいずれか」のユーザー

### 5-10. ワーク: 5セッション以上閲覧しているユーザーの集計結果だけを出力する
- 「ユーザー毎のaccess_logs集計」のクエリを編集し、「5セッション以上閲覧しているユーザー」に限定して結果を出力することを考えます
    - `LIMIT 5`は使わないのでコメントアウトします
    - `ORDER BY pv DESC`をコメントアウトし、`ORDER BY session DESC`のコメントアウトを戻します


```sql
SELECT
  user_id
  , COUNT(*) AS pv
  , COUNT(DISTINCT session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
GROUP BY user_id
-- ORDER BY pv DESC
ORDER BY session DESC
-- LIMIT 5
```

- まず、下記のようにWHEREを足して実行し、エラーになることを確認します

```sql
SELECT
  user_id
  , COUNT(*) AS pv
  , COUNT(DISTINCT session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
WHERE session >= 5
GROUP BY user_id
-- ORDER BY pv DESC
ORDER BY session DESC
-- LIMIT 5
```

- エラー内容
    - `Unrecognized name: session; Did you mean session_id?`
- 今度は、WHEREをGROUP BYの後に移動して実行し、再びエラーになることを確認します

```sql
SELECT
  user_id
  , COUNT(*) AS pv
  , COUNT(DISTINCT session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
GROUP BY user_id
WHERE session >= 5
-- ORDER BY pv DESC
ORDER BY session DESC
-- LIMIT 5
```

- エラー内容
    - `Syntax error: Unexpected keyword WHERE`
- `WHERE`を`HAVING`に書き換えて実行し、期待した結果が得られることを確認します

```sql
SELECT
  user_id
  , COUNT(*) AS pv
  , COUNT(DISTINCT session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
GROUP BY user_id
HAVING session >= 5
-- ORDER BY pv DESC
ORDER BY session DESC
-- LIMIT 5
```

- クエリを保存します

### 5-11. 集計結果に対する絞り込みは、WHEREではなくHAVINGを使う
- WHEREは、抽出・集計する前のテーブルに対して、「事前」に絞り込みをするために使います
    - 条件式には、「抽出・集計する前のテーブル」の列名だけを使うことができます
- 集計結果に対する「事後」の絞り込みは、WHEREではなく`HAVING`を使います
- `HAVING`の直後には、WHEREと同様に条件式を指定します
    - 条件式には、「抽出・集計した結果」の列名だけを使うことができます
    - 例
        - `HAVING user_id = "vis-2JKE"`
        - `HAVING session >= 5`
        - `HAVING session >= 5 AND pv >= 10`

### 5-12. 演習: PV数の多いページpathを特定する
- 「path毎のaccess_logs集計」のクエリを編集し、下記の条件を付け加えましょう
    - originが"https://cxclip.karte.io"に等しいアクセスのみ集計
    - PV数が30以上のpathのPV数のみ出力
- クエリを保存します

### 5-13. コラム: 実行順序は、FROM → JOIN → WHERE → GROUP BY → HAVING → ORDER → LIMITの順番
- SQLの命令が増えてくると、実際に実行される順番を意識しないと書き方がわからなくなってしまいます
- 下記のクエリの実行順序を考えます
    - 「PVが20以上の女性ユーザーについて、最大3行をPV数の降順で出力する」

```sql
SELECT
  logs.user_id AS user_id
  , COUNT(*) AS pv
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs` AS logs
INNER JOIN `prd-karte-per-client.karte_school_{{api_key}}.users` AS users
  ON logs.user_id = users.user_id
WHERE users.gender = "woman"
GROUP BY logs.user_id
HAVING pv >= 20
ORDER BY pv DESC
LIMIT 3
```

- 実行順序は、主に記述された順番（上から下）となっています
    - SELECTだけは例外
- 特に、`GROUP BY`は集約される前後で使用できる列が変わるので重要です
- 上記クエリの実行イメージは、下記の通りです
- `FROM`で、access_logsテーブルを全件取ってくる

sync_date | user_id | session_id | origin | path
-- | -- | -- | -- | --
2020-01-04T12:20:44.051Z | vis-PS3T | vis-PS3T_267 | https://cxclip.karte.io | /
... | ... | ... | ... | ...

- `JOIN`で、usresテーブルを結合される

logs.sync_date | logs.user_id | logs.session_id | logs.origin | logs.path | users.age | users.gender
-- | -- | -- | -- | -- | -- | --
2020-01-04T12:20:44.051Z | vis-PS3T | vis-PS3T_267 | https://cxclip.karte.io | / | 50 | woman
... | ... | ... | ... | ... | ... | ...

- `WHERE`で、users.genderが"woman"の行だけに絞り込まれる
- `GROUP BY`で、user_idが共通のグループ毎に集約される
    - ここで、`SELECT`で指定された集計方法が使われる

user_id | pv
-- | --
vis-PS3T | 61
... | ...

- `HAVING`で、pvが20以上の行だけに絞り込まれる
- `ORDER BY`で、pvの降順に並び替えられる
- `LIMIT`で、もし3行以上あったら3行目まで出力される

### 5-14. ワーク: GROUP BYにまつわるよくあるエラーを体験する
- 直前のコラムで見た下記の内容でクエリを新規作成し、実行します

```sql
SELECT
  logs.user_id AS user_id
  , COUNT(*) AS pv
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs` AS logs
INNER JOIN `prd-karte-per-client.karte_school_{{api_key}}.users` AS users
  ON logs.user_id = users.user_id
WHERE users.gender = "woman"
GROUP BY logs.user_id
HAVING pv >= 20
ORDER BY pv DESC
LIMIT 3
```

- クエリ名を変更します
    - `SQLの実行順序確認用の全部盛りクエリ`
- 作成したクエリはこのコース用のクエリフォルダに格納します
- クエリを順次編集して実行し、エラーが出ることを確認します
- `GROUP BY`していないusers.genderを、`SELECT`で指定します
    - `SELECT list expression references users.gender which is neither grouped nor aggregated`

```sql
SELECT
  logs.user_id AS user_id
  , users.gender AS gender
  , COUNT(*) AS pv
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs` AS logs
INNER JOIN `prd-karte-per-client.karte_school_{{api_key}}.users` AS users
  ON logs.user_id = users.user_id
WHERE users.gender = "woman"
GROUP BY logs.user_id
HAVING pv >= 20
ORDER BY pv DESC
LIMIT 3
```

- `, users.gender AS gender`を削除します
- 今度は、`GROUP BY`と`HAVING`をコメントアウトして実行し、エラーが出ることを確認します

```sql
SELECT
  logs.user_id AS user_id
  , COUNT(*) AS pv
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs` AS logs
INNER JOIN `prd-karte-per-client.karte_school_{{api_key}}.users` AS users
  ON logs.user_id = users.user_id
WHERE users.gender = "woman"
-- GROUP BY logs.user_id
-- HAVING pv >= 20
ORDER BY pv DESC
LIMIT 3
```

- 今度は、`GROUP BY`と`HAVING`のコメントアウトを戻して、クエリを保存します

## 6. 小さなSELECT文の組み合わせで複雑なSQLを作る
- すぐに記述方法が思いつかないような複雑なSQLでも、小さなSELECT文の組み合わせで作成することができます
- ここでは、下記のようなクエリを作成します
    - 「期間中に3セッション以上来ているユーザーについて、5PV以上あったセッションの閲覧ページpathを時系列で見たい」
- 一般的に、大きな課題は小さな課題に分解してから考えた方が解きやすくなります
- このクエリも、下記の3つのクエリを組み合わせれば作成できそうです
    - 期間中に3セッション以上来ているユーザーのuser_idを抽出するクエリ
    - 5PV以上あったセッションのsession_idを抽出するクエリ
    - アクセスログをsync_dateの昇順で表示するクエリ

### 6-1. WITHでSELECT文の結果を一時的なテーブルとして再利用する
- SQLでは、SELECT文の結果をさらに別のSELECT文のFROMに指定することができます
- `WITH`を使うと、SELECT文の結果に名前を付けることができます
    - 名前を付けたSELECT文の結果も、一時的なテーブルとして別のSELECT文のFROMに指定できます
- 2つ目以降は、`WITH`の部分にカンマを書きます

```sql
WITH logs AS (
  SELECT
    sync_date
    , user_id
    , session_id
  FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
)
, users AS (
  SELECT
    user_id
    , gender
  FROM `prd-karte-per-client.karte_school_{{api_key}}.users`
)
SELECT
    logs.session_id AS session_id
    , users.gender AS gender
FROM logs
INNER JOIN users
  ON logs.user_id = usres.user_id
```

### 6-2. ワーク: WITHを組み合わせて30行以上の長いクエリを書いてみる
- 前述したクエリを作成していきます
    - 「期間中に3セッション以上来ているユーザーについて、5PV以上あったセッションの閲覧ページpathを時系列で見たい」
- 空のクエリを新規作成し、クエリ名を変更します
    - `WITHを連結して複雑な条件で絞り込み`
- 作成したクエリはこのコース用のクエリフォルダに格納します
- まず、「期間中に3セッション以上来ているユーザーのuser_idを抽出するクエリ」を作成します

```sql
SELECT
  user_id
  , COUNT(DISTINCT session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
GROUP BY user_id
HAVING session >= 3
```

- `WITH`を使って、実行結果に`often_visited_users`という名前をつけます
    - そのままSELECT文で抽出して、結果を確認します

```sql
WITH often_visited_users AS (
  SELECT
    user_id
    , COUNT(DISTINCT session_id) AS session
  FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
  GROUP BY user_id
  HAVING session >= 3
)
SELECT * FROM often_visited_users
```

- 同様に、「5PV以上あったセッションのsession_idを抽出するクエリ」を追加します
    - `sessions_with_many_pv`という名前を付けます
    - そのままSELECT文で抽出して、結果を確認します

```sql
WITH often_visited_users AS (
  SELECT
    user_id
    , COUNT(DISTINCT session_id) AS session
  FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
  GROUP BY user_id
  HAVING session >= 3
)
, sessions_with_many_pv AS (
  SELECT
    session_id
    , COUNT(*) AS pv
  FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
  GROUP BY session_id
  HAVING pv >= 3
)
SELECT * FROM sessions_with_many_pv
```

- これで、抽出対象のuser_idとsession_idの用意ができました
- 次に、まずは抽出対象のuser_idにマッチするアクセスログだけに絞り込みます
    - `INNER JOIN`は両方のテーブルに含まれる結果しか出力しないので、これを利用して絞り込みます
    - `logs_filtered_by_user`という名前を付けます
    - 結果は、session_id, sync_dateの順番に昇順で表示します

```sql
WITH often_visited_users AS (
  SELECT
    user_id
    , COUNT(DISTINCT session_id) AS session
  FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
  GROUP BY user_id
  HAVING session >= 3
)
, sessions_with_many_pv AS (
  SELECT
    session_id
    , COUNT(*) AS pv
  FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
  GROUP BY session_id
  HAVING pv >= 3
)
, logs_filtered_by_user AS (
  SELECT
    logs.sync_date AS sync_date
    , logs.session_id AS session_id
    , logs.path AS path
  FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs` AS logs
  INNER JOIN often_visited_users AS target_users
  ON logs.user_id = target_users.user_id
)
SELECT * FROM logs_filtered_by_user ORDER BY session_id, sync_date
```

- 最後に、抽出対象のsession_idにマッチするアクセスログだけに絞り込みます
    - `logs_filtered_by_user_and_session`という名前を付けます
    - 結果は、session_id, sync_dateの順番に昇順で表示します

```sql
WITH often_visited_users AS (
  SELECT
    user_id
    , COUNT(DISTINCT session_id) AS session
  FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
  GROUP BY user_id
  HAVING session >= 3
)
, sessions_with_many_pv AS (
  SELECT
    session_id
    , COUNT(*) AS pv
  FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
  GROUP BY session_id
  HAVING pv >= 5
)
, logs_filtered_by_user AS (
  SELECT
    logs.sync_date AS sync_date
    , logs.session_id AS session_id
    , logs.path AS path
  FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs` AS logs
  INNER JOIN often_visited_users AS target_users
  ON logs.user_id = target_users.user_id
)
, logs_filtered_by_user_and_session AS (
  SELECT
    logs.sync_date AS sync_date
    , logs.session_id AS session_id
    , logs.path AS path
  FROM logs_filtered_by_user AS logs
  INNER JOIN sessions_with_many_pv AS target_sessions
  ON logs.session_id = target_sessions.session_id
)

SELECT
  sync_date
  , session_id
  , path
FROM logs_filtered_by_user_and_session ORDER BY session_id, sync_date
```

- 出力結果を眺めて、どんなことがわかるか考えてみましょう
- クエリを保存します

## 7. SQLでできる他のこと
- 以上でワークは終了です、お疲れ様でした！
- 今後、SQLを実践していくために、SQLでできることと関連する関数や演算子を簡単に紹介します
    - 詳細の解説はしないので、興味がある方は調べてみてください
        - 参考: [標準 SQL 関数と演算子  |  BigQuery  |  Google Cloud](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators)

### 7-1. 文字列操作
- 複数の文字列を結合する
    - `CONCAT(origin, path)`
- 正規表現に一致する部分だけを抽出する
    - `REGEXP_EXTRACT(email, r"^[a-zA-Z0-9_.+-]+")`
- JSONの中身を参照する
    - `JSON_EXTRACT_SCALAR()`

### 7-2. 日時の操作
- クエリ実行時点の時刻を生成する
    - `CURRENT_TIMESTAMP()`
- 時刻の計算をする
    - `TIMESTAMP_ADD(TIMESTAMP "2008-12-25 15:30:00 UTC", INTERVAL 10 MINUTE)`

### 7-3. 集計
- 条件に合致する行だけCOUNTする
    - `COUNTIF(age >= 20)`
- 主に数値を集計する
    - `MAX(age)`
    - `MIN(age)`
    - `SUM(pv)`
    - `AVG(age)`

### 7-4. その他
- 行に番号を振る
    - `ROW_NUMBER()`
    - `RANK()`
- 文字列を配列化する
    - `SPLIT("a,b,c,d")`
- 配列を別々の行にバラす
    - `UNNEST`

## 8. BigQueryとDatahub
- Datahubでは、裏側でBigQueryというデータベース製品を使っています

### 8-1. BigQueryのStandard SQL
- [BigQuery](https://cloud.google.com/bigquery/?hl=ja)
    - クラウドサービスとしてGoogleが提供しているデータベースです
    - 数億行規模のテーブルであっても、高速に集計することができます
- Datahubで記述するSQLは、BigQueryの「標準SQL」と呼ばれるSQLです
- 実は、データベース製品の機能の違いなどによって、SQLには微妙に方言があります
    - あるデータベースに対するSELECT文が、別のデータベースでは動かないケースがあります
- ただし、アメリカで学んだ英語がイギリスでも使えるように、BigQueryの標準SQLを学ぶことで、他のデータベースのSQLもすぐに書くことができるようになります

### 8-2. Datahubでできること
- Datahubでは、BigQueryを拡張していくつかの機能を追加しています
- 最大の特徴は、KARTEのプロジェクトで発生したイベントログが全て格納されたテーブルが用意されていることです
    - 参考: [karte_eventテーブルへのクエリを作成する](https://developers.karte.io/docs/datahub-karte_event-table)
    - 注意
        - karte_eventテーブルはとても大規模なテーブルなので、長期間のクエリを
- クエリの一部を「パラメータ化」し、GUIで設定できる機能もあります
- 「特定ユーザーの抽出・集計」のクエリで、検索対象のuser_idをパラメータ化してみます

```sql
-- SELECT
--   user_id
--   , sync_date
--   , session_id
--   , path
-- FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
-- WHERE user_id = "{{user_id}}"
-- ORDER BY sync_date

SELECT
  user_id
  , COUNT(*) AS pv
  , COUNT(DISTINCT session_id) AS session
FROM `prd-karte-per-client.karte_school_{{api_key}}.access_logs`
WHERE user_id = "{{user_id}}"
GROUP BY user_id
```

- パラメータエディタには、パラメータの設定をyamlと呼ばれる記法で記述します
    - 参考: [クエリパラメータを利用する](https://developers.karte.io/docs/use-datahub-query-parameter)

```yml
user_id:
  type: text
  label: 抽出対象のuser_id
  value: vis-2JKE
```

- また、KARTEでよく使われるクエリは「クエリコレクション」からダウンロードして使うことができます
    - 参考: [クエリコレクションを活用する](https://developers.karte.io/docs/datahub-query-collection)
- その他、クエリをKARTEで利用するための機能も多数用意されています
    - クエリ結果を使って、メールをリスト配信する（ターゲット配信）
    - クエリ結果をユーザーに紐付けて、セグメントの条件に使う（紐付けテーブル）
    - クエリ結果を、アクションから参照する（アクションテーブル）
    - クエリ結果をテーブルに保存し、外部のBIで可視化する

### 8-3. ワーク: karte_eventテーブルからアクセスログを抽出する
- データセット画面から、下記のテーブルを開きます
    - データセット
        - `karte_stream_{{api_key}}`
    - テーブル
        - `karte_event_*`
- [クエリを作成]ボタンを押します
- 作成したクエリを、下記のように書き換えます
    - `karte_event('YYYYMMDD', 'YYYYMMDD')`で抽出期間を指定できるので、日付を編集して今日だけを抽出するようにします
        - 例
            - `{{ karte_event('20200112', '20200112') }}`
        - 記述を失敗すると、全期間のイベントを抽出する非常に重いクエリが実行されてしまうので、注意してください
    - `LIMIT`を10件にします

```sql
SELECT * FROM {{ karte_event('YYYYMMDD', 'YYYYMMDD') }} LIMIT 10 -- YYYYMMDDには実際の日付が入る
```

- `SELECT`と`WHERE`の指定を追加し、下記のように書き換えて実行します

```sql
SELECT
  sync_date
  , user_id
  , session_id
  , JSON_EXTRACT_SCALAR(values, '$.view.access.uri.path') AS path
FROM {{ karte_event('YYYYMMDD', 'YYYYMMDD') }} -- YYYYMMDDには実際の日付が入る
WHERE
  event_name = 'view'
LIMIT 10
```

- これで、このコースで使ったaccess_logsテーブルとほぼ同じデータを、実際のKARTEのイベントから抽出することができます
- クエリ名を変更します
    - `karte_eventテーブルからアクセスログを抽出`
- 作成したクエリはこのコース用のクエリフォルダに格納します

### 8-4. クエリリソース上限について
- Datahubでは、「月間使用クエリリソース」の上限が設定されています
    - プランによって、上限値は異なります
    - 使用状況や上限値は、[Datahub設定]画面から確認可能です
- クエリリソースを消費しないためには、下記を意識しましょう
    - `karte_event`テーブルに対する実行クエリは、必要以上に長い期間のデータを抽出しない
    - クエリ実行前に表示される`このクエリは実行時に xx GB のデータを処理します。`を確認してから実行する
        - 過度に大きい場合は、不要なデータを参照していないか見直す

## おわりに
### 事後学習の確認
- (TBD)
- ぜひ、実践問題を通じてこのコースの内容を復習してください

### 今後の学習
- SQLを使ったデータ分析に慣れるためのコツは、失敗を恐れずにひたすら「try! try! try!」すること
    - 書籍やオンライン学習コンテンツで学ぶのは、後からでも遅くない
    - 「やってみる → 不明点にぶつかる → 調べる → わかる」のサイクルを回す
    - 学習効率を上げるには、わかる人にとにかく質問すること
