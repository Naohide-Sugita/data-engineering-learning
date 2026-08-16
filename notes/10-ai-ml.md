# 10. AI・ML

## 目次

- [10-1. BigQuery ML](#10-1-bigquery-ml)
- [10-2. MLの基本プロセス](#10-2-mlの基本プロセス)
- [10-3. AutoML / Model Registry](#10-3-automl--model-registry)
- [10-4. LLM / Embeddings / RAGのデータ準備](#10-4-llm--embeddings--ragのデータ準備)

## 10-1. BigQuery ML

- BigQuery上のデータを移動せず、SQLで機械学習モデルの作成・評価・予測を行える
- インフラや学習環境を別途管理する必要がない
- 「BigQuery上の大規模データ + SQLでML + 低運用負荷」→ BigQuery ML

### データドリフト
- 過去と現在のサービングデータで特徴量の分布が変化すること
- ドリフトが大きい場合は再トレーニングを検討する
- BigQuery MLでは`ML.VALIDATE_DATA_DRIFT`で分布変化を評価できる
- 「過去データと現在データの分布比較」→ データドリフト

## 10-2. MLの基本プロセス

### BigQuery MLの基本操作
- `CREATE MODEL`：モデルを作成・学習
- `ML.EVALUATE`：モデル性能を評価
- `ML.PREDICT`：新しいデータを予測

### 基本用語
- 特徴量：予測の判断材料
- 目的変数：予測したい答え

### ロジスティック回帰
- 0 / 1のような二値分類に使う
- 例：クリックする / しない、購入する / しない
- BigQuery MLでは`MODEL_TYPE='LOGISTIC_REG'`
- 「Yes / No を予測」→ ロジスティック回帰

## 10-3. AutoML / Model Registry

### Vertex AI AutoMLによるテキスト分類
- 学習用テキストデータを整え、少ないコードでカスタム分類モデルを作成できる
- 顧客レビューなどのテキスト分類・センチメント分類に利用できる
- 「大量テキスト + カスタム分類 + ML開発を簡略化」→ Vertex AI AutoML

## 10-4. LLM / Embeddings / RAGのデータ準備

### BigQueryからGeminiを利用
- BigQueryからVertex AIのリモートモデルを利用してGeminiを呼び出せる
- Cloud resource connectionを使ってVertex AIと連携する
- SQLからテキスト生成・要約などを実行できる
- 「BigQuery上の大量テキスト + Gemini」→ Vertex AIリモートモデル
