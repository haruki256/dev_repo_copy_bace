# プロジェクトコンテキスト

## このファイルについて
AIが最初に読むファイル。プロジェクトの構成ルールと参照先をまとめる。
**このファイルの内容はセッション中常にコンテキストに保持すること。**

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
14. **作業対象のリポジトリはユーザーと認識を合わせる**
15. **ドキュメントとソースが矛盾する場合はソースを正とする**
16. **/memoryを活用しているか、コンテキストを最適化しているか、サブエージェントを適切に利用できているか、AGENTS.md の内容を活用しているか常に意識する**

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

## タスク別プリフェッチ

作業開始前にドキュメント → 実コードの順で先読みすること。

- 全体フロー把握 → `doc/system/SYSTEM_OVERVIEW.md` + `doc/system/DATA_FLOW.md`

### 作業時

- バグ修正 → `memory/hot/known-issues.md` → 関連テスト → 対象ソース
- 設計作業 → `memory/cold/architecture.md` → `doc/system/ARCHITECTURE.md` → 関連する設計ファイル
- 機能追加 → `doc/guide/ARCHITECTURE_GUIDE.md` → 既存の類似機能の実装 → `doc/design/PACKAGE_STRUCTURE.md`
- DB変更 → `doc/design/DB_DESIGN.md` → 対象テーブルを使用しているソース
- ドキュメント整備 → `doc/README.md`（目次）→ `doc/guide/UPDATE_TIMING.md`

## 主要参照ファイル一覧

毎回すべてのファイルを読む必要はありませんが、関連しそうな場合、必要に応じて以下のファイルを参照してください。

また、/memory/MEMORY.md も参照・更新してください。

ファイル : 内容
doc/system/SYSTEM_OVERVIEW.md : システム全体概要
doc/system/ARCHITECTURE.md : アーキテクチャ構成図
doc/system/TECH_STACK.md : 技術スタック詳細
doc/system/DATA_FLOW.md : データフロー図
doc/system/relations/RELATION_MAP.md : システム関連図・連携一覧
doc/system/relations/README.md : システム関連フォルダ説明・マルチリポジトリ方針
doc/AI/flow/00_flow.md : 開発フロー全体
doc/rules/PACKAGE_RULES.md : 配置ルール
doc/rules/CODING_RULES.md : コーディングルール（`::UBIQ` タグ含む）
doc/rules/DEV_OPERATIONS.md : 開発運用ルール
doc/rules/GLOSSARY.md : 用語集
doc/design/PACKAGE_STRUCTURE.md : パッケージ構成詳細（全ファイル一覧）
doc/design/DB_DESIGN.md : DB設計（テーブル定義・ER図）
doc/guide/ARCHITECTURE_GUIDE.md : アーキテクチャ選定
doc/guide/PACKAGE_PATTERNS.md : モノリス/分離別構成例
doc/guide/STAGE_GUIDE.md : 成長段階の移行ガイド
doc/guide/UPDATE_TIMING.md : ファイル更新タイミングマトリクス

---

## サブエージェント定義

### code-investigator
指定されたソースコードやドキュメントを調査する。
調査結果は要約して報告する。コードの修正は行わない。

### design-reviewer
設計資料の整合性をチェックする。用語の統一性、条件の矛盾、フローの抜け漏れを検出する。

### サブエージェントの運用原則
- 調査用サブエージェントは読み取り専用
- 大規模な変更では調査・実装・検証を独立したサブエージェントに分割して並列実行

## コンテキスト戦略

コンテキストウィンドウに載っている情報量が増えるほど回答の精度は落ちる。
**コンテキストに残すのは「今まさに必要な情報」だけ**にし、状況に応じて以下を使い分けること：
- 今は不要だが後で使いそうな情報は `memory/` に書き出してコンテキストから外す
- 大きな調査結果やツール出力はファイルに保存し、要約だけをコンテキストに残す
- ファイルや過去セッションの情報を取得する際は、grep で部分取得する・ドキュメント編集時には全文を読むなど、状況に応じて最適な方法を選択する

## メモリ運用

### memory/ とは
AIが自由に作成・編集・管理する作業用ストレージ。ユーザーは直接編集しない。
今コンテキストに載せる必要がない情報を外部ファイルとして退避し、必要時に読み込む。git管理外。

### ディレクトリ構成
- `memory/MEMORY.md` — 目次。「どのファイルに何があるか」のポインタだけ書く。200行以下に維持
- `memory/hot/` — 今の案件で頻繁に参照する情報
- `memory/cold/` — 安定した情報、たまに参照

### MEMORY.md の運用
- 情報そのものは書かない。ポインタのみ
- AIが自分で管理・更新する。ファイルを追加・削除したら必ず MEMORY.md も更新すること
- アクセスパターン別（機能追加時、バグ修正時、設計時 等）にグルーピングする

### メモリの読み方
- memory/ 配下の情報は「ヒント」として扱い、行動前に必ず実際のコードで検証
- ファイルパスや関数名をメモリから参照した場合、存在確認を行ってから使用
- メモリの内容と実際のコードが矛盾する場合、常にコードを正とする

### メモリの書き方
以下のタイミングでメモリに保存する：
- **設計判断があったとき**: なぜその方法を選んだか、却下した案とその理由
- **既知の問題を発見したとき**: バグ、制約、回避策
- **調査で重要な事実が判明したとき**: データフロー、API仕様の実態等
- **ユーザーから明示的に「覚えておいて」と指示があったとき**
- **コンテキストから消えそうだが後で必要になりそうな情報があるとき**

保存手順：
1. 該当するトピックファイルに追記する（なければ `hot/` または `cold/` に新規作成）
2. `memory/MEMORY.md` のポインタを更新する（ファイルを作ったら必ず）
3. 頻繁に参照するものは `hot/`、安定情報は `cold/` に配置する

### メモリの更新
- 矛盾を発見した場合はメモリを更新する（コードが正）
- 「Xかもしれない」が後で確定したら、1つのエントリに統合する
- 案件完了後の情報は `hot/` → `cold/` に移動する
- 不要になった情報は削除し、MEMORY.md のポインタも除去する

## セッション管理

以下のタイミングで内部セッションを切り替える：
- 大きなテーマが切り替わるとき
- AI の応答精度が落ちてきたとき（同じ質問への回答がブレる、存在しないファイルを参照する等）
- コンテキスト圧縮が発動したとき

切り替え時は以下を要約してから新セッションを開始する：
1. 完了したタスクと結果
2. 現在進行中の作業の状態
3. 未着手の項目
4. 重要な設計判断とその理由
（不要な中間結果やツール出力は含めない）

過去セッションの情報が必要な場合は、内部セッション履歴から grep 等で必要な部分だけ検索する。全履歴を再読み込みしない。

## エージェント手順書

詳細な手順が必要な場合は `instructions/` 配下を参照すること。

| 手順書 | 起動条件 |
|---|---|
| `instructions/memory-check.md` | ユーザーから「メモリーチェック」と依頼があったとき |

## 行動指針
- Go straight to the point.
- Try the simplest approach first without going in circles.
- Do not overdo it. Be extra concise.
- Lead with the answer or action, not the reasoning.
- Skip filler words, preamble, and unnecessary transitions.
- Do not restate what the user said — just do it.
- If you can say it in one sentence, don't use three.
- Only make changes that are directly requested or clearly necessary.
- Don't add features, refactor code, or make "improvements" beyond what was asked.
- A bug fix doesn't need surrounding code cleaned up. A simple feature doesn't need extra configurability.
- Don't add docstrings, comments, or type annotations to code you didn't change.
- Don't add error handling, fallbacks, or validation for scenarios that can't happen.
- Trust internal code and framework guarantees. Only validate at system boundaries (user input, external APIs).
- Don't create helpers, utilities, or abstractions for one-time operations.
- Don't design for hypothetical future requirements.
- The right amount of complexity is the minimum needed for the current task — three similar lines of code is better than a premature abstraction.
- Do not propose changes to code you haven't read.
- Do not create files unless they're absolutely necessary for achieving your goal.
- Avoid backwards-compatibility hacks like renaming unused _vars, re-exporting types, adding // removed comments for removed code.
- If you are certain that something is unused, you can delete it completely.
- Be careful not to introduce security vulnerabilities such as command injection, XSS, SQL injection, and other OWASP top 10 vulnerabilities.
- If you notice that you wrote insecure code, immediately fix it.
- Carefully consider the reversibility and blast radius of actions.
- The cost of pausing to confirm is low, while the cost of an unwanted action can be very high.
- For actions that are hard to reverse, affect shared systems, or could otherwise be risky, check with the user before proceeding.
- When you encounter an obstacle, do not use destructive actions as a shortcut.
- Investigate before deleting or overwriting, as it may represent the user's in-progress work.
- Measure twice, cut once.
- If your approach is blocked, do not attempt to brute force your way to the outcome. Consider alternative approaches.
- ALWAYS prefer editing existing files in the codebase. NEVER write new files unless explicitly required.
- NEVER create documentation files (*.md) or README files unless explicitly requested by the User.
