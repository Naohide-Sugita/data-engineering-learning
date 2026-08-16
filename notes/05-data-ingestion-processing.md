# 5. データ取り込み・処理

## 目次

- [5-1. ETL / ELT](#5-1-etl--elt)
- [5-2. データ形式（CSV / JSON / Parquet / Avro）](#5-2-データ形式csv--json--parquet--avro)
- [5-3. データ転送・移行方式](#5-3-データ転送--移行方式)
- [5-4. バッチ処理](#5-4-バッチ処理)
- [5-5. ストリーミング処理](#5-5-ストリーミング処理)
- [5-6. Pub/Sub](#5-6-pubsub)
- [5-7. Dataflow / Apache Beam](#5-7-dataflow--apache-beam)
- [5-8. Dataproc / Spark](#5-8-dataproc--spark)
- [5-9. Cloud Data Fusion](#5-9-cloud-data-fusion)
- [5-10. データソース / シンク](#5-10-データソース--シンク)
- [5-11. ウィンドウ処理 / 遅延データ](#5-11-ウィンドウ処理--遅延データ)

## 5-1. ETL / ELT

- ETL：ロード前に変換・クレンジングする
- ELT：先にDWHなどへロードし、その後に変換する
- 「不正データを保存前に除去したい」→ ETL

## 5-2. データ形式（CSV / JSON / Parquet / Avro）

## 5-3. データ転送・移行方式

### Storage Transfer Service
- オンプレミスや他クラウドからCloud Storageへデータを転送するマネージドサービス
- オンプレミスのNFSからの移行にも利用できる
- 転送帯域制御や進捗・失敗状況の確認ができる
- S3などの転送対象をinclude/excludeプレフィックスで分割し、複数ジョブを並列化できる
- 「ネットワーク経由の大容量転送」→ Storage Transfer Service
- 「大量の小ファイル + 時間短縮」→ プレフィックス分割 + 並列転送

### Transfer Appliance
- 物理アプライアンスを使って大容量データをオフライン転送する
- ネットワーク帯域が細く、数百TB〜PB級を短期間で移行したい場合に向く
- 「大容量 + 低帯域 + 短期間」→ Transfer Appliance

### BigQuery Data Transfer Service
- Google Adsなどの外部サービスからBigQueryへ定期的にデータを取り込む
- 過去データのバックフィルと定期転送を同じ仕組みで実行できる
- サーバーレスで追加オーケストレーターが不要
- 「対応サービス → BigQuery + 定期取り込み」→ BigQuery Data Transfer Service

### Database Migration Service
- オンプレミスなどのDBをGoogle CloudのDBへ移行するマネージドサービス
- MySQL → Cloud SQL for MySQL などに対応する
- 継続レプリケーションによりダウンタイムを抑えた移行ができる
- 「DB移行 + 整合性維持 + ダウンタイム最小化」→ Database Migration Service

### 既存パイプラインを再利用する移行
- 既存ツールにBigQueryコネクタがある場合は、再構築より既存パイプラインの再利用を優先できる
- データマッピングをBigQuery向けに調整する
- 「既存ツールがBigQuery対応 + 移行速度優先」→ 既存コネクタを再利用

## 5-4. バッチ処理

### Cloud Storage到着をトリガーしたBigQueryロード
- 小規模ファイルを到着直後に処理するならCloud Run functionsなどのイベント駆動処理が向く
- サーバーレスなので常時稼働環境が不要
- 「小規模ファイル + 到着時に即実行 + 低運用負荷」→ Cloud Run functions

## 5-5. ストリーミング処理

## 5-6. Pub/Sub

- メッセージやイベントを受け取り、後続処理へ渡すサービス
- ストリーミングパイプラインの入口として使う
- 例：`デバイス → Pub/Sub → Dataflow → BigQuery`

### フロー制御
- サブスクライバーが一度に受け取るメッセージ数・データ量を制限する
- サブスクライバーの処理能力を超える受信を防ぐ
- 「Pub/Sub + スパイク + サブスクライバーが処理しきれない」→ フロー制御

## 5-7. Dataflow / Apache Beam

- Dataflow：バッチ・ストリーミングのデータ処理を実行するマネージドサービス
- Apache Beam：Dataflowなどで実行するパイプラインを記述するSDK

### リアルタイム検証・クレンジング
- BigQueryへロードする前に検証・クレンジング・変換を実行できる
- 自動スケーリングにより大規模ストリームにも対応しやすい
- 「リアルタイム + 大量データ + ロード前変換」→ Dataflow

### 大規模バッチ変換
- Cloud Storage上の大規模データを変換してBigQueryへロードする用途にも向く
- 「GCS上の大規模データ + 変換 + BigQuery」→ Dataflow

### Dataflowテンプレート / 外部API
- テンプレートを使うと定型パイプラインの開発負荷を抑えられる
- パイプラインからCloud Translation APIなどの外部APIを利用できる
- 「GCS + 外部API変換 + BigQuery + 低運用負荷」→ Dataflow

## 5-8. Dataproc / Spark

### Dataproc Serverless
- Apache Sparkなどの処理をクラスタ管理なしで実行できる
- 「Spark + クラスタ管理不要」→ Dataproc Serverless

### Dataproc Workflow Templates
- Dataproc上の複数Spark/Hadoopジョブをワークフローとして定義・実行する
- Dataproc中心の定期バッチを、Cloud Composerより単純な構成で管理できる場合がある
- 「Dataproc + 定期Sparkジョブ + 構成を簡素化」→ Workflow Templates

## 5-9. Cloud Data Fusion

- GUI中心でETLパイプラインを構築するマネージドサービス
- Cloud Storage → BigQueryなどの典型的なデータ統合をローコードで構築できる
- 「GUIで素早くETL」→ Cloud Data Fusion
- 「大規模ストリーミングや細かな処理ロジック」→ Dataflow

## 5-10. データソース / シンク

- ソース：データの入力元
- シンク：データの出力先
- 例：`Pub/Sub（ソース）→ Dataflow → BigQuery（シンク）`

## 5-11. ウィンドウ処理 / 遅延データ
