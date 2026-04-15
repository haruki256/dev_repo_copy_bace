# プロジェクトコンテキスト

## このファイルについて
AIが最初に読むファイル。プロジェクトの構成ルールと参照先をまとめる。
このファイルの内容はセッション中常にコンテキストに保持すること。

---

## AI 必読ルール

以下のルールはすべてのコード修正・ドキュメント修正時に必ず守ること。

1. ユーザーは日本人。日本語を主とすること。英語併記は可
2. 回答は丁寧・正確に。過剰な横文字を避け、関連資料がないか確認する
3. コード修正時は既存のスタイル・構成を尊重。リファクタリング指示がある場合は可読性・保守性の向上可
4. ユビキタス言語: `doc/rules/GLOSSARY.md` の定義に従う。新用語は GLOSSARY.md に追記（分割禁止）
5. コーディング規約: `doc/rules/CODING_RULES.md` に従う（`::UBIQ` タグ含む）
6. 開発運用ルール: `doc/rules/DEV_OPERATIONS.md` に従う
7. アーキテクチャ: `doc/guide/ARCHITECTURE_GUIDE.md` に従う
8. `::UBIQ` 影響タグ: ソース作成・修正時にファイルが関与するユビキタス言語をタグとして維持・追加（CODING_RULES.md 参照）
9. ファイル/フォルダの追加・削除・移動時は `doc/design/PACKAGE_STRUCTURE.md` を更新
10. `doc/` 配下にドキュメントを追加・削除したら `doc/README.md`（目次）を更新
11. ファイル修正時は `doc/guide/UPDATE_TIMING.md` のタスク別早見表で連動更新を確認
12. テーブル追加・カラム変更時は `doc/design/DB_DESIGN.md` を更新
13. コード修正後は Lint & Test を実行（例: `npm run lint` / `npm run test`）
14. 作業対象のリポジトリはユーザーと認識を合わせる
15. ドキュメントとソースが矛盾する場合はソースを正とする
16. /ai-stash を活用しているか、コンテキストを最適化しているか、サブエージェントを適切に利用できているか、常に意識する
17. 特にai-stashはセッションに捉われないワークスペースにおける重要な知識管理・コンテキスト最適化手段なので、積極的に活用すること

---

## プロジェクト概要

- プロジェクト名：[Project Name]
- 使用言語・FW：[Programming Language] ([Framework])
- 概要：[Project Description] を提供するアプリケーション
- `src/` がアプリケーションルート
- 構成詳細 → `doc/design/PACKAGE_STRUCTURE.md`、配置ルール → `doc/rules/PACKAGE_RULES.md`
- Stage/構成方針 → `doc/guide/STAGE_GUIDE.md`

## 開発フロー

`doc/AI/flow/00_flow.md` に従う。各ステップは `doc/AI/flow/step/` 配下を参照。

## タスク別プリフェッチ

作業開始前にドキュメント → 実コードの順で先読みすること。

- 全体フロー把握 → `doc/system/SYSTEM_OVERVIEW.md` + `doc/system/DATA_FLOW.md`
- バグ修正 → `ai-stash/hot/known-issues.md` → 関連テスト → 対象ソース
- 設計作業 → `ai-stash/cold/architecture.md` → `doc/system/ARCHITECTURE.md` → 関連設計ファイル
- 機能追加 → `doc/guide/ARCHITECTURE_GUIDE.md` → 既存の類似機能 → `doc/design/PACKAGE_STRUCTURE.md`
- DB変更 → `doc/design/DB_DESIGN.md` → 対象テーブルを使用しているソース
- ドキュメント整備 → `doc/README.md`（目次）→ `doc/guide/UPDATE_TIMING.md`

## 参照ファイルの探し方

- `doc/README.md`（目次）を参照する
- grep / glob でプロジェクト内を検索する
- `ai-stash/STASH_INDEX.md` も適宜参照・更新すること

---

## サブエージェント定義

- code-investigator: ソースコード・ドキュメントの調査・要約（読み取り専用）
- design-reviewer: 設計資料の整合性チェック — 用語統一、条件矛盾、フロー漏れ（読み取り専用）
- 大規模な変更では調査・実装・検証を独立したサブエージェントに分割して並列実行

## コンテキスト戦略

コンテキストの情報量が増えるほど回答精度は落ちる。
溢れてから退避するのではなく、**情報の役割が変わった時点で能動的に整理する**。

- コンテキストには「What（何を決めた）」と「So What（だから今どうする）」だけを残す
- 「Why の詳細」と「How の詳細」は ai-stash/ に書き出す
- 大きな調査結果やツール出力はファイルに保存し、要約だけをコンテキストに残す
- 情報取得時は grep で部分取得、ファイル編集前は全文を読むなど、状況に応じて最適な方法を選択する

## ai-stash 運用

ai-stash/ はAIの作業用知識ストレージ（git管理外）。コンテキストの S/N 比を維持するために能動的に使う。
- `ai-stash/STASH_INDEX.md` — ポインタインデックス（常時参照）
- `ai-stash/hot/` — 頻繁に参照する情報、`ai-stash/cold/` — 安定した情報
- 正本はコード → doc → ai-stash 、ai-stashは作業用であり、使う、消す、移す、統合する
- ai-stash の内容は「ヒント」。行動前に必ず実コードで検証すること
- ai-stash とコードが矛盾する場合、常にコードを正とする

少なくとも以下のタイミングで ai-stash に書き出すこと：
- 判断が確定したとき — 検討過程・却下案を書き出し、結論と確定仕様をコンテキストに残す
- フェーズが切り替わったとき（調査→設計、設計→実装 等） — 前フェーズの作業詳細を書き出し、結果だけを残す
- 大きな作業単位が完了したとき — 経緯を書き出し、次に引き継ぐべき前提・制約だけを残す
- モデル/担当が変わるとき — 詳細な経緯を書き出し、現状の状態サマリを残す

→ 書き出し内容・コンテキスト管理の詳細は `ai-instructions/stash-guide.md` に従うこと

## セッション管理

以下のタイミングで内部セッションを切り替える：
- 大きなテーマが切り替わるとき
- AI の応答精度が落ちてきたとき（同じ質問への回答がブレる、存在しないファイルを参照する等）
- コンテキスト圧縮が発動したとき

→ 切替時は `ai-instructions/session-switch.md` の手順に従うこと

## エージェント手順書

詳細な手順が必要な場合は `ai-instructions/` 配下を参照すること。

- `ai-instructions/stash-guide.md` — ai-stash への書き込み・整理を行うとき
- `ai-instructions/stash-check.md` — ユーザーから「スタッシュチェック」と依頼があったとき
- `ai-instructions/session-switch.md` — セッションを切り替えるとき

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
