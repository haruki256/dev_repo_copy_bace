# 複数関連プロジェクトでの AI 活用メソッド

## 背景：関連リポジトリが多いプロジェクト

単独リポジトリで完結するシステムではなく、複数の関連リポジトリで構成されている場合がある

## 解決：関連リポジトリを1つのワークスペースにまとめて AI に読ませる

例：関連リポジトリ、xx,yy 親フォルダ：xx-service/

### 手順

1. **親フォルダを作成** する（例：`xx-service/`）
2. その配下に **関連リポジトリをすべてクローン** する
3. 親フォルダのルートに **AGENTS.md と CLAUDE.md を配置**する（CLAUDE.md は AGENTS.md へのポインタ。実質的な指示はすべて AGENTS.md に書く）
4. AI ツール（Claude Code 等）を **親フォルダをワーキングディレクトリとして起動** する

```
xx-service/ ← ここで AI を起動
├── AGENTS.md ← ワークスペース全体の設定
├── CLAUDE.md ← Claude Code → AGENTS.md へのポインタのみ
├── ai-instructions/ ← エージェント向け手順書（オンデマンド読み込み）
│   ├── stash-check.md ← ai-stash 棚卸し手順
│   └── ... ← 今後追加可能
├── ai-stash/ ← AI 作業用ナレッジストア（git 管理外・個人ごと）
│   ├── STASH_INDEX.md ← ポインタインデックス（常時参照）
│   ├── hot/ ← 今の案件で頻繁に参照
│   └── cold/ ← 安定した情報、たまに参照
├── xx/ ← メインリポジトリ
│   ├── AGENTS.md ← プロジェクト詳細はここ
│   ├── CLAUDE.md ← → AGENTS.md へのポインタ
│   ├── doc/
│   └── src/
├── xx-batch/
├── yy/
```

### 効果

- AI が必要に応じて **実際の関連リポジトリ内のソースコードを直接調査** できる
- ドキュメントの記載とソースコードの実装を **突き合わせて検証** できる
- 「ドキュメントにはこう書いてあるが、実際のコードではこうなっている」という **乖離を発見** できる

### 注意点

- **プロジェクトのルール・規約**は `xx/AGENTS.md` に集約されている。xx-service の AGENTS.md はそこへの誘導に加えて、ワークスペース固有の設定（ai-stash 運用、サブエージェント、プリフェッチ、セッション管理等）を持つ
- **作業対象のリポジトリはユーザーの指示に従う**。特に指定がなければ xx が対象
- AI に複数リポジトリを見せると **コンテキストが広がる** ため、作業対象を明確に指示すること

### ai-stash の配置

AI のナレッジストア（ai-stash）は `xx-service/ai-stash/` に配置する。`xx/` 配下ではなく xx-service ルートに置く理由：
- **git 管理不要**：ai-stash は個人の作業知識であり、push する必要がない。xx-service ルートは git 管理外なので `.gitignore` の設定も不要
- **個人ごとに最適化**：同じプロジェクトでも担当領域や作業文脈は人によって異なる。ai-stash は各自が育てるもの
- **リポジトリ横断**：ai-stash には複数リポジトリにまたがる知識も含まれるため、特定リポジトリの配下に置くより親フォルダに置く方が自然

### STASH_INDEX.md の設計

ポインタインデックスをアクセスパターン別に構造化する。AIがタスクの種類に応じて適切なファイルを見つけやすくなる。

```markdown
# STASH_INDEX.md

## 機能追加・実装時に参照
- アーキテクチャ判断 → ai-stash/architecture.md
- コーディング規約 → ai-stash/conventions.md

## バグ修正・調査時に参照
- 既知の問題 → ai-stash/known-issues.md
- 過去のバグパターン → ai-stash/bug-patterns.md

## 設計・方針検討時に参照
- 技術選定の経緯 → ai-stash/dependencies.md
- 案件ごとの設計判断 → ai-stash/案件名-design.md
```

### hot/cold 分離

ナレッジファイルを「今の案件で頻繁に参照するもの（hot）」と「安定した情報でたまに参照するもの（cold）」に分離する。

```
ai-stash/
├── STASH_INDEX.md
├── hot/ ← 今の案件で頻繁に参照
│   ├── current-task.md
│   └── active-decisions.md
└── cold/ ← 安定した情報
    ├── architecture.md
    ├── conventions.md
    └── dependencies.md
```

案件完了後の情報は hot/ → cold/ に移動。新案件で必要になった情報は cold/ → hot/ に昇格する。

### ai-stash の棚卸し

xx-service は git 管理外なので気軽にファイルを作れる分、肥大化しやすい。定期的な棚卸しが必要。

棚卸し手順の詳細は `ai-instructions/stash-check.md` に記載。AGENTS.md に「スタッシュチェックと依頼があったら instructions/stash-check.md を読んで実行」と指示しておくことで、ユーザーが「スタッシュチェックして」と言うだけで実行される。

### ai-instructions/ ディレクトリ

ai-stash/（知識データ）とは別に、エージェント向けの手順書を置くディレクトリ。
- AGENTS.md はポインタだけ持ち、詳細手順は ai-instructions/ に置く
- 必要時に AGENTS.md から参照して読み込む形
- ファイルベースなので **Claude Code 以外のAIツールでも同じ手順が使える**
- Claude Code にはカスタム指示の機能があるが、それに依存しない設計

## AGENTS.md の設計思想

### AGENTS.md は毎ターン再注入される

AGENTS.md はユーザーが新しいメッセージを送るたびに再注入される。会話が長くなっても指示が有効であり続ける。

この仕組みを踏まえた設計：
- **AGENTS.md は小さく保つ**（大きいと毎ターンのコストが膨らむ）
- 「何を知っているか」ではなく **「何をどこで調べるか」を書く地図** として設計する
- 行動規範（必ず守るルール）は AGENTS.md に書く → 常に有効
- 詳細情報は ai-stash/ や ai-instructions/ に退避 → 必要時のみ読み込み

## コンテキストファイルの例

親フォルダに配置する AGENTS.md / CLAUDE.md / ai-instructions のサンプルは [`例/`](例/) 配下を参照。

- [AGENTS.md](例/AGENTS.md) — ワークスペースルートに置く AGENTS.md のサンプル
- [CLAUDE.md](例/CLAUDE.md) — AGENTS.md へのポインタとして置く CLAUDE.md のサンプル
- [ai-instructions/](例/ai-instructions/) — エージェント向け手順書のサンプル
  - [stash-check.md](例/ai-instructions/stash-check.md) — スタッシュチェック手順
  - [stash-guide.md](例/ai-instructions/stash-guide.md) — ai-stash 書き出し運用ガイド
  - [session-switch.md](例/ai-instructions/session-switch.md) — セッション切り替え手順
