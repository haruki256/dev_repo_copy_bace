# パッケージ配置ルール

## 基本構成

### src/app/（画面単位）
- 1画面 = 1パッケージ
- 画面内に controller, service, data/ を配置
- ビジネスロジックは service に書く
- data/ にはリクエスト・レスポンス・その他モデルなど画面固有のデータ構造をすべて置く

### src/app/ の「画面」の定義
- app/ 配下の単位は「UI画面」に限らず「ユーザー/外部からの入口（entrypoint）単位」で切る
- バックエンドAPIのみの場合：エンドポイント単位（userApi/, orderApi/）
- CLIの場合：コマンド単位（importCmd/, exportCmd/）
- バッチの場合：ジョブ単位（dailyReport/, dataSync/）
- 本ドキュメントでは便宜上「画面」と表記するが、上記を含む

### src/common/（技術的共通処理）
- utils/：汎用ユーティリティ（日付変換、文字列処理等）
- constants/：全体で使用する定数
- **commonに置けるもの：** 副作用なしの技術的な薄いユーティリティのみ
- **commonに置かないもの：** I/O（DB、HTTP、ファイル等）や状態を持つ処理

### src/shared/（共有データ構造・DB永続化）
- dto/：複数画面で共有するデータ構造（ロジックは持たない）
- persistence/entity/：DBテーブルと1対1のデータ構造
- persistence/mapper/：DBアクセス処理

## 画面内 data/ と shared/dto/ の使い分け

| | 画面内 data/（app/画面X/data/） | shared/dto/ |
|---|---|---|
| 目的 | その画面で使うデータ構造全般 | 複数画面で共有するデータ構造 |
| 中身 | Request, Response, その他モデル | 画面横断で使うDTO |
| 命名 | XxxRequest, XxxResponse, XxxData | XxxDto |
| ロジック | 持たない | 持たない |
| 判断 | 1画面でしか使わないもの全部 | 3画面以上から参照されるもの |

## DB Entity（persistence/entity/）と DTO（shared/dto/）の区別

| | DB Entity（shared/persistence/entity/） | DTO（shared/dto/） |
|---|---|---|
| 目的 | テーブルの行を表す | 必要な項目だけ抽出・整形した受け渡し用データ |
| 内容 | カラムと1対1対応 | ビジネス上必要な項目のみ |
| ロジック | 持たない | 持たない |
| 管理 | テーブルごとに1つ（増殖させない） | 用途に応じて作成 |
| 配置 | shared/persistence/entity/ に集約 | shared/dto/ |

### なぜDB Entityを集約するか
同じテーブルを複数画面から参照する場合、画面ごとにDB用データ構造を作ると増殖して「どれが正？」になる。
shared/persistence/entity/ に1テーブル1Entityで集約し、画面側は必要な項目だけDTOやdata/のResponseに変換して返す。

### 注意：persistence/entity/ はDDD Entityではない
- persistence/entity/ はDBテーブルと1対1のデータ構造（ロジックを持たない）
- DDD Entityは「振る舞い（ロジック）を持つオブジェクト」であり、別物
- persistence/ というフォルダ配下にあることで「DB用」と判断できる

## shared/ に追加する際の判断基準
1. 3画面以上から同じデータ構造を参照する → shared/dto/ に追加
2. データ変換が2箇所以上で必要 → shared/dto/ にマッピング用DTOを追加
3. 上記以外のビジネスロジック → 画面内の service に書く（共通化しない）
4. 判断に迷ったら画面内に置く

## 共通化の判断
- 「同じコードが3箇所以上」かつ「今後も増える見込み」→ 初めて共通化を検討
- 共通化する場合は DESIGN.md に影響範囲を明記すること
- 早すぎる共通化はしない

## 依存の方向ルール
- app/* → shared/* ：参照してよい
- app/* → common/* ：参照してよい
- shared/* → app/* ：**参照しない**
- common/* → app/* ：**参照しない**
- common/* → shared/* ：**参照しない**
- persistence/entity/ を画面のcontrollerやレスポンスにそのまま露出しない。app側でdata/やdtoに変換して返す

## 画面内が複雑な場合の分割ルール
- 1URLの画面内にステップ・タブ・モーダル等の複数ユースケースがある場合、serviceをフォルダ化し、ユースケースごとにserviceファイルを分割する
- パッケージ（画面）の単位は変えない
- 分割の目安：service内に明確に異なる操作（入力/確認/検索等）が2つ以上ある場合
- 命名：画面名 + ユースケース名 + Service（例：OrderEntryConfirmService）

### 分割例
```text
app/orderEntry/
  controller
  service/                               # serviceをフォルダ化
    OrderEntryInputService               # 入力ステップのロジック
    OrderEntryConfirmService             # 確認ステップのロジック
    OrderEntryCompleteService            # 完了ステップのロジック
    OrderProductSearchService            # 商品検索モーダルのロジック
  data/
    OrderEntryInputRequest
    OrderEntryConfirmRequest
    OrderEntryCompleteResponse
    OrderProductSearchData
```

## DDD要素について
- 本構成では簡易DDDとして「ユビキタス言語（GLOSSARY.md）」と「境界の判断基準（本ファイル）」のみ導入
- shared/dto/ はDDD Entityではない（ロジックを持たない受け渡し用データ構造）
- shared/persistence/entity/ もDDD Entityではない（DBテーブルと1対1のデータ構造）
- フルDDDの導入が必要になった場合は STAGE_GUIDE.md の Stage 3 を参照
