# プロジェクトコンテキスト

## このファイルについて
AIが最初に読むファイル。プロジェクトの構成ルールと参照先をまとめる。

## プロジェクト概要
<!-- プロジェクト開始時に記入 -->
- プロジェクト名：
- 使用言語・FW：
- 概要：

## プロジェクト初期セットアップ（AI確認事項）
プロジェクト開始時、AIは以下をユーザーに確認し、構成を調整すること：
1. 使用言語・フレームワーク
2. テストコードの配置方針（src横置き or 別ディレクトリ）
3. 画面単位の命名規則
4. PACKAGE_PATTERNS.md を参照し、言語に合った構成を提案すること

## 構成の基本方針
- 入口（entrypoint）単位で src/app/ 配下にパッケージを切る（画面、APIエンドポイント、コマンド等）
- ビジネスロジックは各画面の service に書く（共通化は慎重に）
- 画面固有のデータ構造（Request/Response/その他モデル）は各画面の data/ に置く
- 複数画面で共有するデータ構造は shared/dto/ に置く（ロジックは持たせない）
- DBテーブルと1対1のデータ構造は shared/persistence/entity/ に集約する
- DBアクセス処理は shared/persistence/mapper/ に集約する
- 技術的な共通処理は common/ に置く（副作用なしのユーティリティのみ）
- 依存の方向：app → shared/common は可、逆方向は不可
- 詳細は doc/rules/PACKAGE_RULES.md を参照

## 開発フロー
- doc/flow/00_flow.md に従って進めること
- 各ステップの詳細は doc/flow/step/ 配下を参照

## 用語
- doc/rules/GLOSSARY.md に従うこと

## 参照ファイル一覧
| ファイル | 内容 |
|---|---|
| doc/flow/00_flow.md | 開発フロー全体 |
| doc/rules/PACKAGE_RULES.md | 配置ルール |
| doc/rules/GLOSSARY.md | 用語集 |
| doc/guide/ARCHITECTURE_GUIDE.md | アーキテクチャ選定 |
| doc/guide/PACKAGE_PATTERNS.md | 言語別構成例 |
| doc/guide/STAGE_GUIDE.md | 成長段階の移行ガイド |
