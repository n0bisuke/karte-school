# もっとユーザーについて知るためのデータ分析SQL入門: 実践問題
- 事前準備でDatahub上に作成した、access_logsテーブルとusersテーブルを使ってください

## 問題1
- 全期間のアクセスログについて、20代男性が10PV以上閲覧しているpathを、PV数の多い順で抽出するクエリを作成してみましょう

### 想定される出力結果

path | pv
-- | --
/ | 81
/topic/customer-experience/ | 74
/topic/customer-satisfaction/ | 22
/practice/ | 19
/topic/cvr/ | 17
/interview/japantaxi/ | 16
/topic/data-driven/ | 14
/topic/personalize/ | 14
/topic/recommendation/ | 13
/interview/ | 11
/interview/mizkan/ | 11
/topic/d2c/ | 10
/topic/mau/ | 10

- 期間中に3セッション以上来ているユーザーについて、5PV以上あったセッションの閲覧ページpathを時系列で見たい

### ヒント
- 性別や年齢はusersテーブルにしか存在しないため、`JOIN`をする必要があります
- SQLの各命令が実行される順番に注意して記述しましょう

## 問題2
- 全期間のアクセスログについて、日付別のPV数、セッション数、UU数を集計するクエリを作成してみましょう

### 想定される出力結果

date | pv | session | uu
-- | -- | -- | --
2020/01/01 | 142 | 87 | 79
2020/01/02 | 160 | 102 | 92
2020/01/03 | 171 | 122 | 114
2020/01/04 | 275 | 136 | 131
2020/01/05 | 224 | 179 | 168
2020/01/06 | 649 | 468 | 448
2020/01/07 | 946 | 598 | 551
2020/01/08 | 859 | 565 | 526
2020/01/09 | 1004 | 593 | 563
2020/01/10 | 705 | 489 | 464
2020/01/11 | 78 | 59 | 59

### ヒント
- 下記のように`FORMAT_TIMESTAMP('%Y/%m/%d', sync_date)`と記述すると、sync_dateを`"2020/01/01"`のようなフォーマットに整形することができます

```sql
SELECT
    FORMAT_TIMESTAMP('%Y/%m/%d', sync_date) AS date
FROM `prd-karte-per-client.karte_school_3a2df8517d95013680fb7a93446fb92a.access_logs` AS logs
```

- 迷ったときは、`WITH`を使って複数のSELECT文に分けて考えてみましょう

## 問題3
- 全期間のアクセスログについて、下記の条件を満たすセッションに属するログを新しい順に抽出するクエリを作成してみましょう
    - `"/topic/mau/"`というpathを閲覧したセッション
    - 合計で3PV以上閲覧しているセッション

### 想定される出力結果

sync_date | session_id | path
-- | -- | --
2020-01-08T07:56:15.462Z | vis-QP01_3 | /topic/mau/
2020-01-08T08:00:32.369Z | vis-QP01_3 | /interview/
2020-01-08T08:00:36.277Z | vis-QP01_3 | /tag/industry/human-resources/
2020-01-08T08:01:13.663Z | vis-QP01_3 | /tag/industry/media/
2020-01-08T08:01:31.125Z | vis-QP01_3 | /tag/industry/human-resources/
2020-01-08T08:01:33.180Z | vis-QP01_3 | /interview/
2020-01-08T08:01:35.384Z | vis-QP01_3 | /tag/industry/it-service/
2020-01-08T08:01:47.292Z | vis-QP01_3 | /interview/4796/
2020-01-08T08:02:42.594Z | vis-QP01_3 | /interview/
2020-01-08T08:02:43.835Z | vis-QP01_3 | /tag/industry/it-service/
2020-01-08T08:02:44.422Z | vis-QP01_3 | /topic/mau/
2020-01-09T00:01:46.405Z | vis-QP01_4 | /topic/mau/
2020-01-09T00:03:49.701Z | vis-QP01_4 | /tag/popular-word/karte-for-app/
2020-01-09T00:04:09.990Z | vis-QP01_4 | /practice/creema-case03/
2020-01-09T00:04:58.233Z | vis-QP01_4 | /tag/popular-word/karte-for-app/
2020-01-09T00:05:11.804Z | vis-QP01_4 | /practice/theo-case01/
2020-01-09T00:05:19.715Z | vis-QP01_4 | /tag/popular-word/karte-for-app/
2020-01-09T00:05:23.278Z | vis-QP01_4 | /topic/mau/

### ヒント
- あるSELECT文の結果を使って別のSELECT文の結果を絞り込む場合、`INNER JOIN`を使いました
- 問題が複雑なときこそ、`WITH`を使って小さなSELECT文に分解して考えてみましょう
