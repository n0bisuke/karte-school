# もっとユーザーについて知るためのデータ分析入門: レッスン
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

### 事前課題の確認
- https://github.com/plaidev/karte-school/blob/master/widget/beginner/pre_homework.md

## 「もっとユーザーについて知る」ために、なぜデータ分析スキルが必要か？
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

## Webサイトやネイティブアプリのユーザーを知るために使われるデータ
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

## まずはGoogleスプレッドシートでやってみる
- Webのアクセスログ自体に慣れるために、まずはGoogleスプレッドシートを使ってサンプルデータを集計してみます
- 事前準備で用意したGoogleスプレッドシートを開いてください

### ワーク: アクセスログの全体集計値を出してみる
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

### ワーク: アクセスログのユーザー毎の集計
- Googleスプレッドシートの`access_logs`にあるデータを使って、下記のデータを集計してみましょう（期間は全期間）
    1. ユーザー毎のPV数
    2. ユーザー毎のセッション数
- ヒント
    - ピボットテーブルを使うのは、1つの方法です
        - [データ > ピボットテーブル]
- 追加の演習
    - PV数が20以上のユーザーのuser_idの一覧を特定してみましょう
    - ユーザー毎のセッション数は、最大でいくつでしょうか？

### ワーク: アクセスログの性別毎の集計
- Googleスプレッドシートの`access_logs`と`users`にあるデータを組み合わせて、下記のデータを集計してみましょう（期間は全期間）
    1. 性別毎のPV数
    2. 性別毎のセッション数
    3. 性別毎のUU数
- ヒント
    - `access_logs`に`gender`列を追加し、`=VLOOKUP()`関数で`users`の値を
    - あとは、一つ前のワークと同様です
- 追加の演習
    - 年齢毎のPV数、セッション数、UU数も同様に集計してみましょう

## 同じことをSQLでやってみる
- Googleスプレッドシートで実施したのと同じ操作を、KARTE DatahubのSQLでやってみましょう
- 事前準備で用意したDatahubのデータセット画面を開きます

### ワーク: access_logsテーブルをそのまま抽出する
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
    - 例
        - `access_logs全件抽出`
- 右上の[保存]ボタンを押してクエリを保存します
- 左上の[フォルダを作成]ボタンを押します
    - フォルダ名を入力します
    - 例
        - `KARTE School: SQL練習用`
- クエリを作成したフォルダに移動します
    - これ以降、作成したクエリは適宜このフォルダに格納しましょう

### 演習: usersテーブルをそのまま抽出する
- 下記のクエリを作成します
    - usersテーブルから、下記の列だけを全件抽出
        - `user_id`
        - `age`
    - クエリ名
        - `users全件抽出`

### SELECT 列名 FROM テーブル名
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

### ワーク: access_logsテーブル全体を集計する
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

### COUNT()で数を集計する
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

### 演習: pathに関する集計
- 「全体access_logs集計」クエリの出力結果に下記の列を追加します
    - `path_count`
        - pathのユニークな種類数
    - `pv_per_path`
        - 1pathあたりの平均PV数

### ワーク: ユーザー毎にaccess_logsテーブルを集計する
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

### GROUP BYでグループ別に集計する
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

### 演習: path毎の集計
- 下記のクエリを作成します
    - access_logsテーブルから、下記の列を集計
        - `path`
        - `pv`
            - path毎のPV数
    - クエリ名
        - `path毎のaccess_logs集計`

### ワーク: 性別毎にaccess_logsテーブルを集計する
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

### JOINで複数テーブルを参照する
- 性別の情報はaccess_logsテーブルには存在していないため、usersテーブルも合わせて抽出する必要があります
- スプレッドシートでは`=VLOOPUP()`で外部テーブルを参照しました
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
-- | -- | -- | -- | --
2020-01-01T05:57:07.008Z | vis-MZPE | vis-MZPE_1 | https://cxclip.karte.io | / | 28 | woman
2020-01-01T11:49:59.719Z | vis-G6N3 | vis-G6N3_1 | https://cxclip.karte.io | / | 20 | woman
... | ... | ... | ... | ...

- JOINした結果の「仮想的なテーブル」は、通常のテーブルと同様にSELECT文で集計することができます

### 演習: 年齢毎にaccess_logsテーブルを集計する
- 下記のクエリを作成します
    - access_logsテーブルから、下記の列を集計
        - `age`
        - `pv`
            - 年齢毎のPV数
        - `session`
            - 年齢別のセッション数
    - クエリ名
        - `年齢毎のaccess_logs集計`
