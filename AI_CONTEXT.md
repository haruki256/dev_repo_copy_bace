# プロジェクトコンテキスト

## このファイルについて
AIが最初に読むファイル。プロジェクトの構成ルールと参照先をまとめる。

---

## AI 必読ルール

> **以下のルールはすべてのコード修正・ドキュメント修正時に必ず守ること。**

1. **ベース**：ユーザーは日本人です。コードコメントや、回答は必要に応じて英語も併記して構いませんが、**日本語を主とすること**。
2. **回答スタイル**：回答時には、過剰な横文字を避け、わかりやすく丁寧な説明を心がけると、ともに、関連する資料やドキュメントがないかを確認し正確な回答を行うこと。
3. **ソース修正時**：コード修正時には、指示がない場合は既存のコメント、処理を勝手に省略せずに、**既存のスタイルや構成を尊重して修正すること**。ただし、リファクタリングや、指示があった場合は、コードの可読性や保守性を向上させるために、必要に応じてスタイルの統一や構成の見直しを行うことができます。
4. **ユビキタス言語**: `doc/rules/GLOSSARY.md` で定義されたユビキタス言語による統一を徹底すること。新用語が発生したら分類して GLOSSARY.md に追記し、**用語ファイルを分割したり別ディレクトリに作成しないこと**。
5. **コーディング規約**: `doc/rules/CODING_RULES.md` に従うこと（`::UBIQ` タグの付与含む）
6. **開発運用ルール**: `doc/rules/DEV_OPERATIONS.md` に従うこと
7. **アーキテクチャ遵守**: `doc/guide/ARCHITECTURE_GUIDE.md` を読み、機能・画面単位パッケージ構成を守ること
8. **`::UBIQ` 影響タグ維持**: ソースコード作成・修正時にファイルが関与するユビキタス言語をタグとして維持・追加すること（CODING_RULES.md 参照）
9. **PACKAGE_STRUCTURE.md 更新**: 開発系ファイル/フォルダの追加・削除・移動時に `doc/design/PACKAGE_STRUCTURE.md` を更新すること
10. **ドキュメント目次更新**: `doc/` 配下にドキュメントを追加・削除した場合は、必ず `doc/README.md` (目次) を更新すること
11. **更新タイミング確認**: ファイル修正時、`doc/guide/UPDATE_TIMING.md` の **タスク別早見表** を参照し、連動して更新が必要なドキュメントを確認すること
12. **DB設計維持**: テーブル追加・カラム変更時は `doc/design/DB_DESIGN.md` を更新すること
13. **品質チェック**: コード修正後は Lint & Test を実行すること（例: `npm run lint` / `npm run test`）

---

## プロジェクト概要
- プロジェクト名：[Project Name]
- 使用言語・FW：[Programming Language] ([Framework])
- 概要：[Project Description] を提供するアプリケーション

## プロジェクト構成
本プロジェクトのベース構成は、`doc/guide/STAGE_GUIDE.md` の **Stage 1（立ち上げ期）** を標準としています。必要に応じてフロントエンドとバックエンドが分離したマルチレポ（Stage 3）構成にも対応します。プロジェクトの現在の Stage に応じた構成を採用してください。

- **src/** — アプリケーションのルートディレクトリ

Package 構成の詳細は doc/design/PACKAGE_STRUCTURE.md を参照。

### 基本方針 (Stage 1 ベース)
- [エンドポイント・画面単位]で `app/` 配下にパッケージを切る（controller, service, data 等）
- 共有リソース（エンティティ、DTOなど）は `shared/`、副作用なしユーティリティは `common/`
- 依存の方向：`app` → `shared` / `common` は可、逆方向は不可
- 詳細は doc/rules/PACKAGE_RULES.md を参照

## 開発フロー
- doc/AI/flow/00_flow.md に従って進めること
- 各ステップの詳細は doc/AI/flow/step/ 配下を参照

## ユビキタス言語
- doc/rules/GLOSSARY.md に従うこと

## 参照ファイル抜粋一覧

毎回すべてのファイルを読む必要はありませんが、関連しそうな場合、必要に応じて以下のファイルを参照してください。

| ファイル | 内容 |
|---|---|
| doc/system/SYSTEM_OVERVIEW.md | システム全体概要 |
| doc/system/ARCHITECTURE.md | アーキテクチャ構成図 |
| doc/system/TECH_STACK.md | 技術スタック詳細 |
| doc/system/DATA_FLOW.md | データフロー図 |
| doc/system/relations/RELATION_MAP.md | システム関連図・連携一覧 |
| doc/system/relations/README.md | システム関連フォルダ説明・マルチリポジトリ方針 |
| doc/AI/flow/00_flow.md | 開発フロー全体 |
| doc/rules/PACKAGE_RULES.md | 配置ルール |
| doc/rules/CODING_RULES.md | コーディングルール（`::UBIQ` タグ含む） |
| doc/rules/DEV_OPERATIONS.md | 開発運用ルール |
| doc/rules/GLOSSARY.md | 用語集 |
| doc/design/PACKAGE_STRUCTURE.md | パッケージ構成詳細（全ファイル一覧） |
| doc/design/DB_DESIGN.md | DB設計（テーブル定義・ER図） |
| doc/guide/ARCHITECTURE_GUIDE.md | アーキテクチャ選定 |
| doc/guide/PACKAGE_PATTERNS.md | モノリス/分離別構成例 |
| doc/guide/STAGE_GUIDE.md | 成長段階の移行ガイド |
| doc/guide/UPDATE_TIMING.md | ファイル更新タイミングマトリクス |
