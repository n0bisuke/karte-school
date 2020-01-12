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
- サイドバーからサンプルデータ用のデータセットを選択し、[テーブルを作成 > テーブルを作成]からテーブルを新規作成します

テーブル名 | スキーマ
-- | --
`access_logs` | `sync_date:TIMESTAMP,user_id:STRING,session_id:STRING,origin:STRING,path:STRING`
`users` | `user_id:STRING,age:INT64,gender:STRING`
