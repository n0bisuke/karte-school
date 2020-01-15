# もっとユーザーについて知るためのデータ分析SQL入門: 事前準備
## サンプルデータのダウンロード
- 下記のcsvファイルをダウンロードしてください

[KARTE School データ分析SQL入門.zip](https://github.com/plaidev/karte-school/files/4051746/KARTE.School.SQL.zip)

## Googleスプレッドシートへのデータアップロード
- スプレッドシートを新規作成します
    - `KARTE: もっとユーザーについて知るためのデータ分析入門` など適当な名前を付けます
- [ファイル > インポート > アップロード]から、2つのcsvファイルをそれぞれ読み込み、別のシートとして保存します
    - インポート場所には、[新しいシートを挿入する]を選んでください

## Datahubへのデータアップロード
- ご自身のアカウントにDatahubの「データの編集」権限があることを確認します
- Datahubのデータセット画面を開きます
- [作成 > データセットを作成]からサンプルデータ用のデータセットを作成します
    - データセット名
        - `karte_school`
- access_logsとusersのそれぞれについて、テーブルを新規作成します
    - サイドバーから作成したデータセットを選択し、[テーブルを作成 > テーブルを作成]をクリック
    - テーブル名とスキーマを、後述の表の通りに入力
    - CSVファイルをアップロードします

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/data_analysis/_images/inner_join.png" width="300px">

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/data_analysis/_images/csv_import.png" width="300px">

テーブル名 | CSVファイル名 | スキーマ
-- | -- | --
`access_logs` | `access_logs.csv` | `sync_date:TIMESTAMP,user_id:STRING,session_id:STRING,origin:STRING,path:STRING`
`users` | `users.csv` | `user_id:STRING,age:INT64,gender:STRING`

- [テーブル情報]から、正しくスキーマが設定されていることを確認します
    - access_logsの`sync_date`が`TIMESTAMP`であること
    - usersの`age`が`INTEGER`であること

<img src="https://raw.githubusercontent.com/plaidev/karte-school/master/data_analysis/_images/check_scheme.png" width="300px">
