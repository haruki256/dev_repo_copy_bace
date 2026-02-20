# パッケージ構成例

## このファイルについて
本プロジェクトのパッケージ構成例。
フロントエンドとバックエンドで独立した構成を持つ場合の一般的な設計。

## プロジェクト全体構成

```text
project-root/
├── AI_CONTEXT.md
├── doc/                                # 共通ドキュメント
│   ├── flow/
│   ├── project/
│   ├── guide/
│   └── rules/
│
├── [Frontend Directory]/               # フロントエンド
│   ├── src/
│   │   ├── features/                   # ≒ app/（UI 画面単位）
│   │   ├── shared/                     # ≒ shared/（機能横断の共有）
│   │   └── common/                     # ≒ common/（純粋ユーティリティ）
│   └── [Config Files]
│
├── [Backend Directory]/                # バックエンド
│   ├── src/
│   │   ├── app/                        # 機能・API単位
│   │   ├── shared/                     # 共有リソース
│   │   └── common/                     # 純粋ユーティリティ
│   ├── tests/                          # インテグレーションテスト
│   └── [Config Files]
│
└── README.md
```

詳細は `doc/design/PACKAGE_STRUCTURE.md` 並びに各フレームワークのベストプラクティスを参照してください。
