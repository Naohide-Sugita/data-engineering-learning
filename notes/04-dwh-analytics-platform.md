# 4. DWH・分析基盤

## 目次

- [4-1. DWH](#4-1-dwh)
- [4-2. Data Lake](#4-2-data-lake)
- [4-3. Lakehouse](#4-3-lakehouse)
- [4-4. Raw / Staging / Mart](#4-4-raw--staging--mart)
- [4-5. OLTP / OLAP](#4-5-oltp--olap)
- [4-6. データアクセスパターン](#4-6-データアクセスパターン)

## 4-1. DWH

### BigQuery 外部テーブル
- Cloud Storageなどの外部データをBigQueryへロードせずSQLで参照できる
- CSV、JSON、Parquetなどを扱える
- 外部テーブルとBigQueryネイティブテーブルをJOINできる
- データの重複保存を避け、一時的・探索的な分析に向く
- 「GCS上のデータ + BigQueryへロードせず分析」→ 外部テーブル

### BigQuery オブジェクトテーブル
- Cloud Storage上の画像・音声・動画などの非構造化オブジェクトをBigQueryから扱う
- ファイルのURIやサイズ、更新日時などのメタデータをSQLで参照できる
- BigQuery MLやVertex AI連携による推論につなげられる
- 「Cloud Storage上の非構造化ファイル + BigQueryで分析/ML」→ オブジェクトテーブル

### BigQueryへの直接ロード
- `bq load`：ローカルファイルなどをBigQueryへロードする
- `LOAD DATA`：Cloud Storage上のCSVなどをBigQueryテーブルへ直接ロードできる
- 単純なバッチ取り込みではDataflowやDataprocなどの追加処理基盤は不要
- 「GCSのCSV + バッチ + 変換不要」→ BigQueryのロード機能

### Google Sheetsを外部テーブルとして参照
- Google SheetsをDrive URI経由でBigQuery外部テーブルとして参照できる
- BigQuery上のテーブルとJOINできる
- Connected Sheetsを使えばBigQueryの結果をGoogle Sheetsから参照できる
- 「Sheets + BigQuery JOIN + Sheetsで参照」→ 外部テーブル + Connected Sheets

## 4-2. Data Lake

## 4-3. Lakehouse

## 4-4. Raw / Staging / Mart

## 4-5. OLTP / OLAP

## 4-6. データアクセスパターン
