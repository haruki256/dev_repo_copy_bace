# パッケージ配置ルール

## 基本構成

### src/app/（画面単位）
- 1画面 = 1パッケージ
- 画面内に controller, service, data/ を配置
- ビジネスロジックは service に書く
- data/ にはリクエスト・レスポンス・その他モデルなど画面固有のデータ構造をすべて置く

### src/common/（技術的共通処理）
- utils/：汎用ユーティリティ（日付変換、文字列処理等）
- constants/：全体で使用する定数

### src/shared/（共有データ構造・DB永続化）
- dto/：複数画面で共有するデータ構造（ロジックは持たない）
- persistence/record/：DBテーブルと1対1のデータ構造
- persistence/mapper/：DBアクセス処理

## 画面内 data/ と shared/dto/ の使い分け

| | 画面内 data/（app/画面X/data/） | shared/dto/ |
|---|---|---|
| 目的 | その画面で使うデータ構造全般 | 複数画面で共有するデータ構造 |
| 中身 | Request, Response, その他モデル | 画面横断で使うDTO |
| 命名 | XxxRequest, XxxResponse, XxxData | XxxDto |
| ロジック | 持たない | 持たない |
| 判断 | 1画面でしか使わないもの全部 | 3画面以上から参照されるもの |

## DB Record と DTO の区別

| | DB Record（shared/persistence/record/） | DTO（shared/dto/） |
|---|---|---|
| 目的 | テーブルの行を表す | 必要な項目だけ抽出・整形した受け渡し用データ |
| 内容 | カラムと1対1対応 | ビジネス上必要な項目のみ |
| ロジック | 持たない | 持たない |
| 管理 | テーブルごとに1つ（増殖させない） | 用途に応じて作成 |
| 配置 | shared/persistence/record/ に集約 | shared/dto/ |

### なぜDB Recordを集約するか
同じテーブルを複数画面から参照する場合、画面ごとにDB用データ構造を作ると増殖して「どれが正？」になる。
shared/persistence/record/ に1テーブル1Recordで集約し、画面側は必要な項目だけDTOやdata/のResponseに変換して返す。

## shared/ に追加する際の判断基準
1. 3画面以上から同じデータ構造を参照する → shared/dto/ に追加
2. データ変換が2箇所以上で必要 → shared/dto/ にマッピング用DTOを追加
3. 上記以外のビジネスロジック → 画面内の service に書く（共通化しない）
4. 判断に迷ったら画面内に置く

## 共通化の判断
- 「同じコードが3箇所以上」かつ「今後も増える見込み」→ 初めて共通化を検討
- 共通化する場合は DESIGN.md に影響範囲を明記すること
- 早すぎる共通化はしない

## DDD要素について
- 本構成では簡易DDDとして「ユビキタス言語（GLOSSARY.md）」と「境界の判断基準（本ファイル）」のみ導入
- shared/dto/ はDDD Entityではない（ロジックを持たない受け渡し用データ構造）
- フルDDDの導入が必要になった場合は STAGE_GUIDE.md の Stage 3 を参照