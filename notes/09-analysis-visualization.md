# 9. 分析・可視化

## 目次

- [9-1. Looker / Looker Studio](#9-1-looker--looker-studio)
- [9-2. LookML基礎](#9-2-lookml基礎)

## 9-1. Looker / Looker Studio

### Looker Scheduler + User Attribute
- Looker Scheduler：ダッシュボードやレポートを定期配信する
- User Attribute：ユーザーやグループごとに異なる属性値を設定する
- User Attributeをフィルターに使うと、同じダッシュボードでも受信者ごとに表示内容を変えられる
- 「ユーザー別フィルター + 定期配信」→ Scheduler + User Attribute

### Looker カスタムフィールド
- LookMLを編集せず、Explore上で新しいフィールドを作成できる
- Develop権限がなくても利用できる
- テーブル計算は取得済みのクエリ結果に対する計算
- 「Develop権限なし + 新しい分析フィールド」→ カスタムフィールド

### Looker Studio データブレンド
- 複数のデータソースをLooker Studio側で結合し、1つのグラフやダッシュボードで利用する
- BigQuery側に新しい統合テーブルを作らず、手軽に複数ソースを組み合わせられる
- 「複数データソース + 1つのダッシュボード」→ データブレンド

## 9-2. LookML基礎

### 行レベルアクセス制御
- User Attribute：ユーザーごとの属性値
- `access_filter`：User Attributeなどを使って閲覧可能な行を制御する
- 同じExploreやダッシュボードで、ユーザーごとに表示データを変えられる

### measure
- 集計・計算した指標をExploreやダッシュボードで利用するためのフィールド
- 収益・コストから利益率などの指標を計算できる
- 「既存フィールドから指標を作る」→ measure

### Explore設計
- LookMLで定義した複数テーブルをJOINし、分析用フィールドをまとめて提供する
- 関連するメトリクスやディメンションを単一Exploreにまとめると、フィルターやドリルダウンを統一しやすい
- 「複数テーブル + 共通フィルター + ドリルダウン」→ 単一Explore
