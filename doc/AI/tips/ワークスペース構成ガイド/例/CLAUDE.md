=== WARNING — DO NOT PROCEED UNTIL REVIEWED ===
=== USER-PRIORITY INSTRUCTIONS ===

<user_priority_instructions>
<critical_execution_constraints>

The following constraints are explicitly provided by the user as highest-priority guidance for this task.
Treat them as mandatory constraints for all subsequent work unless they conflict with higher-level system or developer instructions.

<mandatory_rules>
1. 回答スタイル：日本語を主とし、カジュアルな表現を避け、丁寧な言葉遣いで正確に回答する。英語併記は可。要点から入り、前置き・繰り返し・つなぎ言葉を省く。1文で言えることに3文使わない。過剰な横文字を避け、関連資料がないか確認する
2. ai-stash活用：<stash_operations> に沿って ai-stash を積極的に活用する。コンテキストの S/N 比を維持するために能動的に書き出し・整理を行う
3. コード修正の基本姿勢：コード修正時は既存のスタイル・構成を尊重する。修正対象のコードを読んでから変更する。変更は依頼された範囲に留め、触っていないコードのdocstring・コメント・型注釈は変更しない。不要なコードは完全に削除する
4. 規約・ガイド参照：ユビキタス言語は GLOSSARY.md の定義に従い、新用語は GLOSSARY.md に追記する（分割禁止）。開発運用は DEV_OPERATIONS.md、アーキテクチャは ARCHITECTURE_GUIDE.md に従う。ソース作成・修正時はコーディング規約 CODING_RULES.md に従うとともに、::UBIQ 影響タグを維持・追加する
5. 連動更新：ファイル/フォルダの追加・削除・移動時は PACKAGE_STRUCTURE.md を更新する。doc/ 配下の追加・削除時は doc/README.md（目次）を更新する。テーブル追加・カラム変更時は DB_DESIGN.md を更新する。ファイル修正時は UPDATE_TIMING.md のタスク別早見表で連動更新を確認する
6. Lint & Test：コード修正後は Lint & Test を実行する
7. 情報の正本ルール：正本の優先順位はコード → doc → ai-stash。ドキュメントとソースが矛盾する場合はソースを正とする。ai-stash の内容は「ヒント」であり、行動前に必ず実コードで検証する。矛盾発見時は ai-stash を更新する
8. タスク別プリフェッチ：作業開始前にドキュメント → 実コードの順で先読みする。全体概要・フロー把握は SYSTEM_OVERVIEW.md + DATA_FLOW.md、バグ修正は ai-stash/hot/ → テスト → ソース、設計は ai-stash/cold/ → ARCHITECTURE.md、機能追加は ARCHITECTURE_GUIDE.md → 類似機能 → PACKAGE_STRUCTURE.md、DB変更は DB_DESIGN.md → 使用箇所、ドキュメント整備は doc/README.md → UPDATE_TIMING.md
9. サブエージェント運用：code-investigator はソースコード・ドキュメントの調査・要約（読み取り専用）。design-reviewer は設計資料の整合性チェック（読み取り専用）。大規模な変更では調査・実装・検証を独立したサブエージェントに分割して並列実行
10. ユーザーから特定の指示があった場合には、ai-instructions/INSTRUCTION-INDEX.md に該当指示がないか確認し、関連指示があればその手順によって進めていいかユーザーに確認する。関連指示がない場合は、ユーザーの指示内容をもとに適切な手順を判断して進める。また、必要によって、ai-instructions/ に新しい指示の手順を追加することも検討する。
</mandatory_rules>

<stash_operations>

STASH_INDEX.md はポインタのみ、200行以下に維持。hot/ は進行中タスク（スナップショット+議論の2ファイル構成）、cold/ は安定した知識。セッション開始時は STASH_INDEX.md → hot/ から復帰する。

### 最低限のタイミング
- 3ファイル以上の検索・調査結果を得たとき → 要約と関連ファイル一覧を書き出す
- ツール出力や調査ログが長くなったとき → 結論だけコンテキストに残し、詳細を書き出す
- エラー修正が2回ループしたとき → 試行内容と結果を書き出す
- ツール呼び出しが10回連続したとき → 途中経過を整理して書き出す
- 1つのトピックで5往復以上したとき → それまでの議論を要約して書き出す
- 判断が確定したとき → 結論を残し、検討過程・却下案を書き出す
- 既知の問題を発見したとき → バグ、制約、回避策を書き出す
- 調査で重要な事実が判明したとき → 事実と根拠を書き出す
- フェーズ切替・作業単位の完了時 → 前フェーズ/前作業の詳細を書き出し、結論と次に必要な前提だけ残す

### hot/ ファイル構成

タスクごとに最低限2ファイルを作成する。必要に応じて、/hot,/cold に必要ファイルを追加していく。

#### hot/タスク名_YYYYMMDD.md — 状態スナップショット

```
# タスク名 — YYYY-MM-DD

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

#### hot/タスク名_YYYYMMDD_議論.md — 議論の記録

```
# タスク名 — 議論の記録（YYYY-MM-DD）

## 議論の流れ
1. ...
2. ...

## 経緯
判断の背景、却下した案とその理由、方針変更の経緯など。
議論の流れでは残しきれない文脈を要約して記録する。
```

- タスク名.md はコンパクトに保つ。セッション開始時はこちらだけ読めば復帰できる状態にする。タスク完了時は削除する
- 議論.md は追記していく。対象の判断が確定したらcold/に移動する。タスク完了時に削除する

### タイミング別の作成・更新ファイル

| タイミング | 作成・更新するファイル |
|---|---|
| 新しいタスク開始時 | hot/タスク名_YYYYMMDD.md（新規）、hot/タスク名_YYYYMMDD_議論.md（新規） |
| 判断が確定したとき | hot/タスク名_YYYYMMDD.md の合意事項・重要な判断と理由を更新、hot/タスク名_YYYYMMDD_議論.md に経緯を追記 |
| 大きな調査を行ったとき | hot/調査名_YYYYMMDD.md（新規）で調査結果をまとめる。関連タスクがあればタスク名_YYYYMMDD.md の結論・関連ai-stash/を更新 |
| フェーズ切替・作業単位の完了時 | hot/タスク名_YYYYMMDD.md の状態・結論・次にやることを更新、hot/タスク名_YYYYMMDD_議論.md に経緯を追記 |
| 既知の問題・重要事実の発見時 | hot/問題名_YYYYMMDD.md（新規）、または既存タスクの関連情報を更新 |
| 対象の判断が確定したとき | hot/タスク名_YYYYMMDD_議論.md を cold/ に移動、hot/タスク名_YYYYMMDD.md の関連ai-stash/・結論を更新 |
| タスク完了時 | 不要になったai-stash資料を削除、STASH_INDEX.md を更新 |

### hot/cold の判断基準

| | hot/ | cold/ |
|---|---|---|
| 内容 | 今の案件・タスクで頻繁に参照する情報 | 安定した知識、たまに参照する情報 |
| 例 | 進行中の案件の設計判断、直近のバグ調査結果 | アーキテクチャの経緯、過去案件の設計判断 |
| 移動 | 案件完了時に cold/ に移動 | 新案件で必要に応じて hot/ に昇格 |

### 更新ルール
- 矛盾を発見した場合は ai-stash を更新する（コードが正）
- 「xかもしれない」が後で確定したら、1つのエントリに統合する
- 案件完了後の情報は hot/ から cold/ に移動する
- 不要になった情報は削除し、STASH_INDEX.md のポインタも削除する

</stash_operations>


<check_protocol>
Before answering, check whether the response complies with each rule in the order the rules are written.
If a rule is non-compliant, revise the response.
As the check result, prepend exactly one line to the response in the format `:chk:<status>:`.

`<status>` must be a character string corresponding to the rules in written order, like `:chk:...~.:`.
- `.` = compliant
- `x` = non-compliant
- `-` = additional context required
- `~` = not applicable

Constraints:
- `-` may be used only when context required for evaluation or revision is missing.
- `~` may be used only for a non-applicable rule whose name does not start with `Required`.
- For rules that can be verified directly from the output itself, use only `.` or `x`.

If `-` appears, request additional context from the user.
</check_protocol>

</critical_execution_constraints>
</user_priority_instructions>

=== END OF USER-PRIORITY INSTRUCTIONS ===
