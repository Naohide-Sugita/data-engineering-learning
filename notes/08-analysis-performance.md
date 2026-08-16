# 8. 分析・性能

## 目次

- [8-1. BigQuery / SQLによる分析](#8-1-bigquery--sqlによる分析)
- [8-2. Jupyter Notebook / Colab](#8-2-jupyter-notebook--colab)
- [8-3. パーティショニング](#8-3-パーティショニング)
- [8-4. クラスタリング](#8-4-クラスタリング)
- [8-5. クエリ性能・最適化](#8-5-クエリ性能最適化)
- [8-6. BI Engine / マテリアライズドビュー](#8-6-bi-engine--マテリアライズドビュー)

## 8-1. BigQuery / SQLによる分析

### ウィンドウ関数と移動平均
- `AVG() OVER()`：元の行を残したまま平均値などを計算する
- `PARTITION BY store_id`：店舗などのグループごとに計算範囲を分ける
- `ORDER BY date`：ウィンドウ内の順序を定義する
- `ROWS BETWEEN 6 PRECEDING AND CURRENT ROW`：現在行を含む直近7行を計算対象にする
- 移動平均：一定範囲の平均を、対象期間をずらしながら計算する

### BI向け事前集計
- BIツールから大量明細を毎回集計すると、クエリ量や表示負荷が大きくなる
- よく使う集計結果をBigQuery側に事前作成し、BIから参照する構成にできる
- 定期更新にはScheduled Queryなどを使える

## 8-2. Jupyter Notebook / Colab

### Colab Enterprise
- Google Cloud上の管理されたJupyterノートブック環境
- BigQueryへ接続し、SQL・Python・Markdown・可視化を1つのノートブックにまとめられる
- ノートブックを再実行して最新データから結果を再生成できる
- チーム共有やPythonパッケージを使った分析にも向く
- 「BigQuery + Python + Notebook + 共有/再現性」→ Colab Enterprise

## 8-3. パーティショニング

- 1つのテーブルを日付などのキーで内部的に分割する
- 取り込み時間パーティションでは`_PARTITIONDATE` / `_PARTITIONTIME`を利用できる
- 列パーティションでは`order_date`などの実データ列を利用する
- WHERE句で対象パーティションだけ絞るとスキャン量とコストを削減できる
- require partition filterを使うと、パーティション条件なしのクエリを防げる

### 古い分析データの保管
- 頻繁に利用する期間はBigQueryに保持する
- 古いデータは用途に応じてCloud Storageなどへ移行する
- Cloud StorageではLifecycleによる自動削除・ストレージクラス変更を利用できる

## 8-4. クラスタリング

## 8-5. クエリ性能・最適化

## 8-6. BI Engine / マテリアライズドビュー

### BigQuery マテリアライズドビュー
- クエリ結果を事前計算・保持して、繰り返しクエリを高速化する
- 元データの変更に応じて自動更新される
- 「繰り返す集計 + 高速化 + 鮮度維持」→ マテリアライズドビュー

### Pub/Sub → BigQuery + マテリアライズドビュー
- BigQuery subscriptionでPub/SubメッセージをBigQueryへ直接書き込める
- 生データをBigQueryに保持しつつ、マテリアライズドビューで集計結果を保持できる
- 「生データを残す + 集計ダッシュボードを高速化」→ BigQuery subscription + マテリアライズドビュー

### マテリアライズドビューとランキング
- マテリアライズドビューの結果を、別クエリで`RANK()`などを使って順位付けできる
- 「自動更新される集計 + ランキング」→ マテリアライズドビュー + ランキングクエリ
