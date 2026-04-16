# ドキュメント目次 (Documentation Index)

このプロジェクトのドキュメント一覧です。ドキュメントは目的別にディレクトリが分かれています。

---

## 👑 必読ドキュメント

AI および開発者がプロジェクトに参加する際、**一番最初**に読む必要があるファイルです。

- [AI_CONTEXT.md](../AI_CONTEXT.md) - プロジェクトコンテキストと AI 必読ルール
- [必読ガイド.md](guide/必読ガイド.md) - ドキュメントの読み進め方ガイド

---

## 📖 ルール・規約 (`rules/`)

開発時のルールや規約、ドメイン知識（ユビキタス言語）をまとめたディレクトリです。

- [DEV_OPERATIONS.md](rules/DEV_OPERATIONS.md) - 開発運用ルール（品質チェック、タグ維持などの行動指針）
- [CODING_RULES.md](rules/CODING_RULES.md) - コーディング規約（`::UBIQ` タグの使い方、言語別作法）
- [PACKAGE_RULES.md](rules/PACKAGE_RULES.md) - パッケージ配置ルール（app/ 等の分割方針、共通化の判断）
- [GLOSSARY.md](rules/GLOSSARY.md) - 用語集（ユビキタス言語の定義）

---

## 🏗️ システム設計 (`system/`)

システムの全体像や技術的な構成をまとめたディレクトリです。

- [SYSTEM_OVERVIEW.md](system/SYSTEM_OVERVIEW.md) - システムの全体概要と提供価値
- [ARCHITECTURE.md](system/ARCHITECTURE.md) - アーキテクチャ構成とパッケージ概略
- [DATA_FLOW.md](system/DATA_FLOW.md) - システム内のデータフローと状態管理
- [TECH_STACK.md](system/TECH_STACK.md) - 使用している技術スタックとその選定理由
- [relations/](system/relations/) - システム関連図・複数リポジトリ構成方針（関連図・個別システム説明）

---

## 📐 設計詳細・ガイド (`design/` & `guide/`)

より詳細な構成や、アーキテクチャパターンのリファレンスです。

- [PACKAGE_STRUCTURE.md](design/PACKAGE_STRUCTURE.md) - パッケージ構成詳細（全ファイル一覧）
- [DB_DESIGN.md](design/DB_DESIGN.md) - DB設計（テーブル定義・ER図・インデックス設計）
- [ARCHITECTURE_GUIDE.md](guide/ARCHITECTURE_GUIDE.md) - アーキテクチャの選定方針と設計の考え方
- [PACKAGE_PATTERNS.md](guide/PACKAGE_PATTERNS.md) - バックエンド・フロントエンドの実装パターンとテンプレート対応
- [STAGE_GUIDE.md](guide/STAGE_GUIDE.md) - プロジェクトの成長段階（Stage）ごとの移行ガイド
- [UPDATE_TIMING.md](guide/UPDATE_TIMING.md) - ファイル更新タイミングマトリクス（どのファイルをいつ更新するか）

---

## 🤖 AI開発フロー (`AI/`)

AIとの協調開発を進める際の手順とテンプレートです。

- [00_flow.md](AI/flow/00_flow.md) - 開発フロー全体の流れ
- `AI/flow/step/` - 各ステップ（要件定義〜実装〜テスト）の詳細ドキュメント
- `AI/flow/template/` - 各種ドキュメントのテンプレート（DESIGN.md, REQUIREMENTS.md 等）
- `AI/tips/`
  - [agent-context-entropy.md](AI/tips/agent-context-entropy.md) - AIエージェントにおけるコンテキストエントロピーと対策
  - [llm-instruction-degradation.md](AI/tips/llm-instruction-degradation.md) - AIモデルのコンテキスト限界と指示追従性の劣化

---

## 👤 ユーザー向けマニュアル (`usage/`)

アプリケーションのユーザー向けマニュアルやセットアップ手順です。

- [USER_GUIDE.md](usage/USER_GUIDE.md) - ユーザーガイド全体
- [SETUP_GUIDE.md](usage/SETUP_GUIDE.md) - 環境構築・セットアップガイド

---

## 📂 プロジェクト個別資料 (`project/`)

個別の開発時の要件定義や設計メモなどを格納するワーキングディレクトリです。

- 例: `project/feature-TEMPLATE/`
