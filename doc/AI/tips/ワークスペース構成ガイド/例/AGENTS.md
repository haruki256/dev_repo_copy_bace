=== WARNING — DO NOT PROCEED UNTIL REVIEWED ===
=== USER-PRIORITY INSTRUCTIONS ===

<user_priority_instructions>
<critical_execution_constraints>

The following constraints are explicitly provided by the user as highest-priority guidance for this task.
Treat them as mandatory constraints for all subsequent work unless they conflict with higher-level system or developer instructions.

<mandatory_rules>
1. Required-回答スタイル：日本語を主とし、カジュアルな表現を避け、丁寧な言葉遣いで正確に回答する。英語併記は可。要点から入り、前置き・繰り返し・つなぎ言葉を省く。1文で言えることに3文使わない。過剰な横文字を避け、関連資料がないか確認する
2. Required-ai-stash活用：<stash_operations> に沿って ai-stash を活用できるか積極的に検討・適切に実施する。コンテキストの S/N 比を維持するために能動的に書き出し・整理を行う
3. コード修正の基本姿勢：コード修正時は既存のスタイル・構成を尊重する。修正対象のコードを読んでから変更する。変更は依頼された範囲に留め、触っていないコードのdocstring・コメント・型注釈は変更しない。不要なコードは完全に削除する
4. 規約・ガイド参照：ユビキタス言語は GLOSSARY.md の定義に従い、新用語は GLOSSARY.md に追記する（分割禁止）。開発運用は DEV_OPERATIONS.md、アーキテクチャは ARCHITECTURE_GUIDE.md に従う。ソース作成・修正時はコーディング規約 CODING_RULES.md に従うとともに、::UBIQ 影響タグを維持・追加する
5. 連動更新：ファイル/フォルダの追加・削除・移動時は PACKAGE_STRUCTURE.md を更新する。doc/ 配下の追加・削除時は doc/README.md（目次）を更新する。テーブル追加・カラム変更時は DB_DESIGN.md を更新する。ファイル修正時は UPDATE_TIMING.md のタスク別早見表で連動更新を確認する
6. Lint & Test：コード修正後は Lint & Test を実行する
7. 情報の正本ルール：正本の優先順位はコード → doc → ai-stash。ドキュメントとソースが矛盾する場合はソースを正とする。ai-stash の内容は「ヒント」であり、行動前に必ず実コードで検証する。矛盾発見時は ai-stash を更新する
8. タスク別プリフェッチ：作業開始前にドキュメント → 実コードの順で先読みする。<ripo_cross_prefetch>も参照
9. サブエージェント運用：code-investigator はソースコード・ドキュメントの調査・要約（読み取り専用）。design-reviewer は設計資料の整合性チェック（読み取り専用）。大規模な変更では調査・実装・検証を独立したサブエージェントに分割して並列実行
10. ユーザーから特定の指示があった場合には、ai-instructions/INSTRUCTION-INDEX.md に該当指示がないか確認し、関連指示があればその手順によって進めていいかユーザーに確認する。関連指示がない場合は、ユーザーの指示内容をもとに適切な手順を判断して進める。また、必要によって、ai-instructions/ に新しい指示の手順を追加することも検討する。
</mandatory_rules>

<stash_operations>

STASH_INDEX.md はポインタと最小限のメタ情報（進行中タスクの優先度・締切など）のみを保持し、200行以下に維持する。  
ai-stash は AI の作業用コンテキストを整理するための補助記憶であり、正本ではない。  
hot/ は進行中タスクの作業メモ、cold/ は再利用価値があるが正式な doc にまだ反映していない情報の一時置き場とする。  
セッション開始時は STASH_INDEX.md → hot/ の順で復帰する。

## 基本方針
- ai-stash は AI の作業用コンテキストをきれいに保つための情報置き場であり、正本ではない
- 正本の優先順位は コード → doc → ai-stash とする
- 行動前に関連するコードと doc を確認し、ai-stash はヒントとして扱う
- ドキュメントとソースコードが矛盾する場合はソースコードを正とし、ai-stash を更新する
- 長いログ、詳細な調査経緯、却下案は別ファイルに分離する
- タスク、依頼、調査が完了して不要になったら ai-stash から削除する
- タスク自体は cold/ に保持しない。cold/ に残すのは、タスクから抽出した再利用価値のある情報だけとする
- 再利用価値が高く、まだ正式な doc に反映していない情報だけは一時的に cold/ に残してよい
- cold/ に残した情報も、正式な doc に反映後は削除を検討する

## 最低限の書き出しタイミング
- 3ファイル以上をまたぐ検索・調査をしたとき
- ツール出力や調査ログが長くなったとき
- エラー修正が2回ループしたとき
- ツール呼び出しが10回連続したとき
- 1つのトピックで5往復以上したとき
- 判断が確定したとき
- 既知の問題や重要な事実を発見したとき
- フェーズ切替、または作業単位が完了したとき

## 運用ルール

### タイミング別の対応

| タイミング | 対応 |
|---|---|
| 新しいタスク開始時 | `hot/task_{task-name}_{YYYYMMDD}.md` と `hot/task_{task-name}_{YYYYMMDD}_discussion.md` を作成 |
| 大きな調査を行ったとき | `hot/investigation_{topic}_{YYYYMMDD}.md` を作成 |
| 問題・制約を発見したとき | `hot/issue_{topic}_{YYYYMMDD}.md` を作成、または既存ファイルを更新 |
| 判断が確定したとき | task ファイルの結論・合意事項を更新し、discussion に経緯を追記 |
| フェーズ切替・作業単位の完了時 | task ファイルの状態・結論・次にやることを更新 |
| タスク完了時 | hot/ の task ファイルと discussion ファイルは原則削除する。タスクから得られた再利用価値のある判断・既知問題・前提知識だけを必要に応じて cold/ に残す。STASH_INDEX.md を更新する |
| doc 反映完了時 | 対応する cold/ を削除し、STASH_INDEX.md を更新する |

### 更新時の原則
- task ファイルは短く保ち、復帰しやすさを優先する
- 長いログや詳細な経緯は discussion / investigation / issue に分離する
- 未確定情報は確定後に統合し、古い記述を残しすぎない
- 一時メモのまま放置せず、不要になったら削除する
- cold/ は恒久保管庫として使わない
- 不要になった情報は STASH_INDEX.md からも削除する

## ファイル命名規則と用途

| ファイル | 用途 |
|---|---|
| `STASH_INDEX.md` | 進行中タスク（優先度・締切付き）と、生きている参照用ファイルのポインタ |
| `hot/task_{task-name}_{YYYYMMDD}.md` | タスクの状態スナップショット。復帰時に最初に読む |
| `hot/task_{task-name}_{YYYYMMDD}_discussion.md` | 議論、却下案、判断経緯 |
| `hot/investigation_{topic}_{YYYYMMDD}.md` | 大きめの調査結果 |
| `hot/issue_{topic}_{YYYYMMDD}.md` | 進行中の不具合、制約、回避策 |
| `cold/decision_{topic}.md` | 確定済みで再利用価値があり、まだ doc 未反映の判断 |
| `cold/domain_{topic}.md` | 一時的に保持するドメイン知識、前提条件 |
| `cold/known-issues.md` | 再発しやすく、まだ正式な doc に反映していない既知問題 |

## STASH_INDEX.md の構成

STASH_INDEX.md は復帰時に最初に読むインデックスで、進行中タスクと参照用ファイルのポインタを保持する。

- 進行中タスクの **優先度・締切は STASH_INDEX.md 側を正本**とし、task ファイル側には書かない（二重管理を避けるため）
- 「参照用ファイル」セクションは**参照場面**で分類する。初期分類は下記3種とし、足りなければ場面カテゴリを追加してよい
- 1ファイルが複数場面にまたがる場合は**主な参照場面**に置き、他場面からは必要に応じてリンクで参照する

### テンプレート

```
# STASH_INDEX.md

AIが管理するポインタインデックス。情報そのものは書かない。

## 進行中タスク
- [P1 / 締切 YYYY-MM-DD] task-name — hot/task_{name}_{YYYYMMDD}.md
  - 関連: hot/investigation_xxx.md, hot/issue_xxx.md

## 参照用ファイル（タスク非紐付け）

### 機能追加・実装時に参照
- hot/investigation_xxx_YYYYMMDD.md — 一行説明
- cold/decision_xxx.md — 一行説明

### バグ修正・調査時に参照
- hot/issue_xxx_YYYYMMDD.md — 一行説明
- cold/known-issues.md — 一行説明

### 設計・方針検討時に参照
- cold/decision_xxx.md — 一行説明
- cold/domain_xxx.md — 一行説明
```

## hot/ の基本構成
タスクごとに最低限、以下の 2 ファイルを作成する。

- `hot/task_{task-name}_{YYYYMMDD}.md`
- `hot/task_{task-name}_{YYYYMMDD}_discussion.md`

必要に応じて investigation / issue を追加する。

### `hot/task_{task-name}_{YYYYMMDD}.md`

```
# task-name — YYYY-MM-DD

## 目的
このタスクで何を達成しようとしているか

## 状態
進行中 / ブロック中

## ブロック理由
（ブロック中の場合）なぜ止まっているか、何が解決すれば進められるか

## 合意事項
ユーザーと確定した方針・判断の一覧

## 重要な判断と理由
何を、なぜそう決めたか

## 結論
現時点で決まっていること

## 次にやること
次のアクション

## 関連ファイル
- path/to/file — 説明

## 関連 ai-stash/
- hot/xxx.md — 説明
- cold/xxx.md — 説明
```

### `hot/task_{task-name}_{YYYYMMDD}_discussion.md`

```
# task-name — 議論の記録（YYYY-MM-DD）

## 議論の流れ
1. ...
2. ...

## 経緯
判断の背景、却下した案とその理由、方針変更の経緯など。
議論の流れでは残しきれない文脈を要約して記録する。
```

### hot/cold の判断基準

| | hot/ | cold/ |
|---|---|---|
| 内容 | 今の案件で頻繁に参照する情報 | 再利用価値があるが、まだ正式な doc に反映していない情報 |
| 例 | 進行中タスクの判断、直近の調査結果、未解決の問題 | 確定済みの判断、再発しやすい問題、整理済みの前提知識 |
| 移動 | タスク完了後は原則削除 | doc 反映まで一時保持し、反映後は削除 |

</stash_operations>

<check_protocol>
Before sending the final user-facing response, check whether that response complies with each rule in the order the rules are written.
If a rule is non-compliant, revise the response.

As the check result, prepend exactly one line to the final user-facing response in the format `:chk:<status>:`.

`<status>` must be a character string corresponding to the rules in written order, like `:chk:...~.:`
- `.` = compliant
- `x` = non-compliant
- `-` = additional context required
- `~` = not applicable

Constraints:
- `-` may be used only when context required for evaluation or revision is missing.
- `~` may be used only for a non-applicable rule whose name does not start with `Required`.
- For rules that can be verified directly from the final user-facing response itself, use only `.` or `x`.

If `-` appears, request additional context from the supervising AI agent or calling agent, not from the user.

Do not include the `:chk:` line in code changes, file contents, logs, generated documents, or other artifacts unless explicitly instructed to do so.
</check_protocol>

</critical_execution_constraints>
</user_priority_instructions>

<operational_reference>
<workspace_context>

## リポジトリ構成

| リポジトリ | 役割 | 主な参照場面 |
|---|---|---|
| dev_repo_copy_bace/ | 役割を記載する | 参照場面を記載する |

## リポ横断プリフェッチ

<ripo_cross_prefetch>
- まず対象リポジトリの `AGENTS.md` → `doc/README.md` → `doc/guide/必読ガイド.md` を読む
- 全体像把握 → `doc/system/SYSTEM_OVERVIEW.md` → `doc/system/ARCHITECTURE.md` → `doc/system/DATA_FLOW.md` → `doc/system/TECH_STACK.md`
- 外部連携・他リポ依存の把握 → `doc/system/relations/RELATION_MAP.md` → `doc/system/relations/README.md` → `doc/system/relations/[SystemName].md` → 必要なら相手リポジトリの同系統 doc
- 機能追加・改修 → `doc/guide/ARCHITECTURE_GUIDE.md` → `doc/guide/PACKAGE_PATTERNS.md` → 既存の類似機能ソース → `doc/design/PACKAGE_STRUCTURE.md`
- DB 定義・永続化変更 → `doc/design/DB_DESIGN.md` → `doc/system/DATA_FLOW.md` → `src/shared/persistence/` など実装箇所
- 配置・責務の確認 → `doc/rules/PACKAGE_RULES.md` → `doc/design/PACKAGE_STRUCTURE.md` → 対象 `src/` 配下
- 用語・規約確認 → `doc/rules/GLOSSARY.md` → `doc/rules/CODING_RULES.md` → `doc/rules/DEV_OPERATIONS.md`
- 要件定義・設計フローを回すとき → `doc/AI/flow/00_flow.md` → `doc/AI/flow/step/` → `doc/project/`
- セットアップ・利用導線確認 → `doc/usage/SETUP_GUIDE.md` → `doc/usage/USER_GUIDE.md`
- doc 更新時 → `doc/README.md` → `doc/guide/UPDATE_TIMING.md` → 更新対象 doc
- バグ調査時 → `ai-stash/STASH_INDEX.md` → `ai-stash/hot/` → 関連テスト → 関連ソース → 必要な doc

同じ構成の別リポジトリでも、原則として「対象リポジトリ内の同名ファイルを同じ順序で読む」。
</ripo_cross_prefetch>

</workspace_context>
</operational_reference>

=== END OF USER-PRIORITY INSTRUCTIONS ===
