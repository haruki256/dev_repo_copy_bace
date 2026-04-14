# xx-service ワークスペース

## このファイルについて

複数リポジトリをまとめたワークスペースの設定ファイル。

**このファイルの内容はセッション中常にコンテキストに保持すること。**

>**`xx/AGENTS.md`** を最初に読むこと。プロジェクトのルール・規約・参照先はそちらに記載。

## リポジトリ構成

| リポジトリ | 役割 | 主な参照場面 |
|---|---|---|
| xx/ | エンドユーザー向け Web アプリ（**サービスメイン**） | 常時 |
| xx-batch/ |  |  |
| yyy/ |  |  |

## 作業ルール

1. **作業対象のリポジトリはユーザーと認識を合わせる**
2. **xx/AGENTS.md のルールに従う**
3. **ドキュメントとソースが矛盾する場合はソースを正とする**
4. **/ai-stashを活用しているか、コンテキストを最適化しているか、サブエージェントを適切に利用できているか、AGENTS.md の内容を活用しているか常に意識する**

## 開発フロー

- `xx/doc/AI/flow/00_flow.md` に従って進めること
- 各ステップの詳細は `xx/doc/AI/flow/step/` 配下を参照

## タスク別プリフェッチ

作業開始前にドキュメント → 実コードの順で先読みすること。

- xxx関連 → `xx/doc/dev-doc/システム情報/xxx.md` → `xxx/zzz/`
- yyy関連 → `yy/doc/dev-doc/システム情報/yyy資料.md` → `yy/zzz/`
- 全体フロー把握 → `xx/doc/dev-doc/システム情報/全体像.md` + `aaa.md`

### 作業時

- バグ修正 → `ai-stash/known-issues.md` → 関連テスト → 対象ソース
- 設計作業 → `ai-stash/architecture.md` → 関連する案件設計ファイル
- 機能追加 → `xx/doc/design/architecture.md` → 既存の類似機能

## サブエージェント定義

### repo-investigator
指定されたリポジトリのソースコードを調査する。
調査結果は要約して報告する。コードの修正は行わない。

### design-reviewer
設計資料の整合性をチェックする。用語の統一性、条件の矛盾、フローの抜け漏れを検出する。

### サブエージェントの運用原則
- 調査用サブエージェントは読み取り専用
- 大規模な変更では調査・実装・検証を独立したサブエージェントに分割して並列実行

## コンテキスト戦略

コンテキストウィンドウに載っている情報量が増えるほど回答の精度は落ちる。
**コンテキストに残すのは「今まさに必要な情報」だけ**にし、状況に応じて以下を使い分けること：
- 今は不要だが後で使いそうな情報は `ai-stash/` に書き出してコンテキストから外す
- 大きな調査結果やツール出力はファイルに保存し、要約だけをコンテキストに残す
- ファイルや過去セッションの情報を取得する際は、grep で部分取得する・ドキュメント編集時には全文を読むなど、状況に応じて最適な方法を選択する

## ai-stash 運用

### ai-stash/ とは
AIが自由に作成・編集・管理する作業用ストレージ。ユーザーは直接編集しない。
今コンテキストに載せる必要がない情報を外部ファイルとして退避し、必要時に読み込む。git管理外。

### ディレクトリ構成
- `ai-stash/STASH_INDEX.md` — 目次。「どのファイルに何があるか」のポインタだけ書く。200行以下に維持
- `ai-stash/hot/` — 今の案件で頻繁に参照する情報
- `ai-stash/cold/` — 安定した情報、たまに参照

### STASH_INDEX.md の運用
- 情報そのものは書かない。ポインタのみ
- AIが自分で管理・更新する。ファイルを追加・削除したら必ず STASH_INDEX.md も更新すること
- アクセスパターン別（機能追加時、バグ修正時、設計時 等）にグルーピングする

### ai-stash の読み方
- ai-stash/ 配下の情報は「ヒント」として扱い、行動前に必ず実際のコードで検証
- ファイルパスや関数名を ai-stash から参照した場合、存在確認を行ってから使用
- ai-stash の内容と実際のコードが矛盾する場合、常にコードを正とする

### ai-stash の書き方
以下のタイミングで ai-stash に保存する：
- **設計判断があったとき**: なぜその方法を選んだか、却下した案とその理由
- **既知の問題を発見したとき**: バグ、制約、回避策
- **調査で重要な事実が判明したとき**: リポジトリ横断のデータフロー、API仕様の実態等
- **ユーザーから明示的に「覚えておいて」と指示があったとき**
- **コンテキストから消えそうだが後で必要になりそうな情報があるとき**

保存手順：
1. 該当するトピックファイルに追記する（なければ `hot/` または `cold/` に新規作成）
2. `ai-stash/STASH_INDEX.md` のポインタを更新する（ファイルを作ったら必ず）
3. 頻繁に参照するものは `hot/`、安定情報は `cold/` に配置する

### ai-stash の更新
- 矛盾を発見した場合は ai-stash を更新する（コードが正）
- 「Xかもしれない」が後で確定したら、1つのエントリに統合する
- 案件完了後の情報は `hot/` → `cold/` に移動する
- 不要になった情報は削除し、STASH_INDEX.md のポインタも除去する

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
| `instructions/stash-check.md` | ユーザーから「スタッシュチェック」と依頼があったとき |

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
