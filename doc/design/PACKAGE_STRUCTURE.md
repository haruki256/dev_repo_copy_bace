# パッケージ構成

> 本ファイルは全ファイル・フォルダの詳細一覧です。システム構成の概要図は `doc/system/ARCHITECTURE.md` を参照してください。

## 全体構成

本プロジェクトの標準構成（Stage 1）のディレクトリツリーです。
※ フロントエンド・バックエンド分離（Stage 3）を取る場合は、本構成を各リポジトリの `src/` 配下で適用してください。

```text
[Project Root]/
├── AI_CONTEXT.md                           # AI向けコンテキスト定義
├── doc/                                    # 共通ドキュメント
│   ├── flow/                               # 開発フロー（要件定義〜実装サイクル）
│   ├── project/                            # プロジェクト管理・各モジュールの設計資料
│   ├── guide/                              # アーキテクチャやステージ移行設計ガイド
│   └── rules/                              # コーディング・運用ルール
│
├── src/                                    # メインソースコード（[Programming Language]）
│   ├── app/                                # 入力・画面単位（Controller, Service, Data）
│   │   ├── [Endpoint 1]/                   # [Endpoint 1] モジュール
│   │   │   ├── controller (handler)        # エンドポイント, UIイベント受付
│   │   │   ├── service                     # ビジネスロジック
│   │   │   └── data                        # 対象モジュール固有のDTO・型定義
│   │   └── settings/                       # 設定画面
│   │
│   ├── common/                             # 純粋ユーティリティ（副作用なし）
│   │   └── utils/
│   │
│   ├── shared/                             # 複数モジュール間での共有リソース
│   │   ├── dto/                            # 共有型定義（3パッケージ以上から参照されるもの）
│   │   ├── persistence/                    # データベースアクセス/外部保存層 (DB使用時)
│   │   │   ├── entity/                     # DBテーブル等と1:1のデータモデル
│   │   │   └── repository (mapper)/        # データ永続化処理
│   │   └── config/                         # アプリ全体設定管理
│   │
│   └── [Entrypoint]                        # アプリケーションエントリポイント (main, index 等)
│
├── tests/                                  # テストコード
└── [Root Config Files]                     # Linter, Formatter 等の共通設定 (package.json, Cargo.toml 等)
```

---

## 依存方向のルール

```text
app/ ──→ shared/ ──→ common/
  │         │
  │         ├── persistence/
  │         └── config/
  │
  └──→ common/
```

- `app/*` → `shared/*` ：✅ 参照可
- `app/*` → `common/*` ：✅ 参照可
- `shared/*` → `app/*` ：❌ 禁止 (共有リソースが特定の入力に依存してはいけない)
- `shared/*` → `common/*` ：✅ 参照可
- `common/*` → `app/*`, `shared/*` ：❌ 禁止 (ユーティリティは純粋でなければいけない)

---

## レイヤーと役割の対応表

| パッケージ | 役割 | 責務の範囲 |
|---|---|---|
| `app/[Endpoint]/controller` | コントローラ / ハンドラ | ユーザー入力・APIリクエストの受付と、Service呼び出し。 |
| `app/[Endpoint]/service` | サービス | 当該パッケージ群のビジネスロジック。 |
| `app/[Endpoint]/data` | データ | 当該パッケージ内でのみ使用する型・構造体定義。 |
| `shared/dto/` | 共有DTO | 3つ以上のパッケージから参照される、データ受け渡し専用の型・構造体。 |
| `shared/persistence/entity/` | データモデル (DB用) | DBのテーブルと1:1で対応する状態。ロジックは持たない。 |
| `shared/persistence/repository/`| リポジトリ (DB用) | データの保存・復元処理（`entity/` を読み書きし、返す）。 |

---

## テスト配置

| テスト種類 | 配置例 |
|---|---|
| ユニットテスト | ソースコードの隣 (`*.test.*`, `*.spec.*`) または言語標準の作法 |
| インテグレーションテスト | `tests/` 等のプレフィックス・ディレクトリ |
