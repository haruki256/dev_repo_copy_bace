# 成長段階ガイド

## このファイルについて
プロジェクトの成長に合わせて、いつ・どのように構成を変えるかのガイド。
各Stageの構成例は言語非依存で記載。言語固有の詳細は PACKAGE_PATTERNS.md を参照。

---

## Stage 1：立ち上げ期

### 目安
- モジュール（画面/API）数：1〜3
- 開発者：1〜2人
- DBテーブル数：少ない

### 構成例

```text
project-root/
├── AI_CONTEXT.md
├── doc/
│   ├── AI/
│   │   └── flow/
│   ├── project/
│   ├── guide/
│   └── rules/
├── src/ (or [Frontend]/, [Backend]/)
│   ├── app/
│   │   ├── [Endpoint 1]/             # モジュール：1
│   │   │   ├── controller
│   │   │   ├── service
│   │   │   └── data/
│   │   └── [Endpoint 2]/             # モジュール：2
│   │       ├── controller
│   │       ├── service
│   │       └── data/
│   ├── common/
│   │   ├── utils/
│   │   └── constants/
│   └── shared/
│       ├── dto/                     # この段階ではまだ空 or 最小限
│       └── persistence/
│           ├── entity/
│           │   ├── [Entity1]
│           │   └── [Entity2]
│           └── mapper(repository)/
│               ├── [Mapper1]
│               └── [Mapper2]
├── tests/                           # src構成をミラー
└── README.md
```

### この段階のポイント
- shared/dto/ はまだほぼ空。モジュールが少ないので共有する必要がない
- ビジネスロジックは各モジュール（入力・画面）のserviceに書く
- 同じDBテーブルを複数モジュールから使っても、shared/persistence/ のEntity/Mapperを共有するだけ
- 無理に共通化しない

### この段階でやらないこと
- shared/dto/ への積極的な切り出し
- ビジネスロジックの共通化
- モノレポ化
- DDD要素の追加

---

## Stage 2：成長期

### 移行トリガー
| 兆候 | アクション |
|---|---|
| 同じデータ構造を3モジュール以上から参照 | shared/dto/ に切り出す |
| 同じ変換処理を2箇所以上にコピペ | shared/dto/ にマッピング用DTOを追加 |
| パッケージ内のserviceが500行超え | パッケージ内でservice分割を検討 |
| commonが肥大化 | カテゴリ別にサブフォルダを追加 |
| DBテーブル数が増えてentity/が見づらい | persistence/entity/ 内をサブフォルダで分類 |
| 外部I/O（HTTP、ファイル等）がDB以外に増えた | shared/infra/ の新設を検討 |

### 目安
- モジュール（画面/API）数：4〜10
- 開発者：2〜5人
- 共通で使うデータ構造が出てきた

### 構成例（Stage 1からの差分を ★ で表示）

```text
project-root/
├── src/
│   ├── app/
│   │   ├── [Endpoint 1]/
│   │   ├── [Endpoint 2]/
│   │   ├── [Endpoint 3]/               # ★ モジュールが増える
│   │   ├── [Endpoint 4]/               # ★
│   │   └── [Endpoint 5]/               # ★
│   ├── common/
│   │   ├── utils/
│   │   │   ├── DateUtils
│   │   │   └── StringUtils          # ★ utilsが増える
│   │   └── constants/
│   └── shared/
│       ├── dto/                      # ★ 複数モジュールで使うDTOが出てくる
│       │   ├── [SharedDtoA]          # ★ 3モジュール以上から参照されるようになった
│       │   └── [SharedDtoB]          # ★
│       └── persistence/
│           ├── entity/
│           │   ├── [Entity1]
│           │   ├── [Entity2]
│           │   └── [Entity3]         # ★ テーブルが増える
│           └── mapper/
│               ├── [Mapper1]
│               ├── [Mapper2]
│               └── [Mapper3]         # ★
└── tests/
```

### この段階のポイント
- shared/dto/ に「3モジュール以上から参照されるデータ構造」を切り出し始める
- ただしDTOにロジックは持たせない（受け渡し用のデータ構造のみ）
- パッケージのserviceが大きくなったらパッケージ内でservice分割
- persistence/entity/ が見づらくなったらサブフォルダ化を検討

### 外部I/Oが増えた場合
- DB以外の外部連携（HTTPクライアント、ファイル操作、通信等）が増えてきたら shared/infra/ の新設を検討する
- persistence/ はDB特化のまま維持し、DB以外の外部I/Oを infra/ に分離する

### この段階でやらないこと
- shared/ にビジネスロジック（service）を置くこと
- DDDのAggregate、Domain Event等の導入
- 余分なモノレポ化

---

## Stage 3：成熟期（必要になった場合のみ）

### 移行トリガー
| 兆候 | アクション |
|---|---|
| フロント・バック間でAPI契約の齟齬が多発 | モノレポ化（ワークスペース等）を検討 |
| 同じビジネスルールが5パッケージ以上のserviceに散在 | DDD要素の追加導入を検討 |
| チームが複数に分かれた | パッケージ間の依存方向を厳密に管理 |
| モジュール数が10を大幅に超えた | app/ 内をカテゴリでグループ化 |

### 目安
- モジュール（画面/API）数：10超
- 開発者：5人超
- フロント・バック分離 or ビジネスルールの複雑化

### 構成例：DDD要素追加（ビジネスルール複雑化）

```text
project-root/
├── src/
│   ├── app/                          # 入力・画面単位（変更なし）
│   ├── common/
│   ├── shared/
│   │   ├── dto/
│   │   └── persistence/
│   └── domain/                       # ★ 新規追加：ドメイン層
│       ├── [Domain 1]/               # ★ ドメイン1
│       │   ├── [Entity]              # ★ DDD Entity（ロジックを持つ）
│       │   └── [DomainService]       # ★ ドメインサービス
│       └── [Domain 2]/               # ★ ドメイン2
```

### Stage 3 移行時の注意
- 一気に移行しない。1ドメイン or 1パッケージずつ段階的に進める
- 移行判断はDESIGN.mdに記録し、チームで合意を取る
- AI_CONTEXT.md を更新し、構成変更をAIに認識させる

---

## Stage移行チェックリスト

### Stage 1 → 2 への移行時
- [ ] shared/dto/ に切り出すデータ構造を特定した
- [ ] PACKAGE_RULES.md の判断基準に従って切り出しを行った
- [ ] 切り出したDTOにロジックを含めていないことを確認
- [ ] GLOSSARY.md に新しい用語を追加した

### Stage 2 → 3 への移行時
- [ ] 移行の必要性をDESIGN.mdに記録した
- [ ] 段階的移行の計画を立てた
- [ ] AI_CONTEXT.md を更新した
- [ ] 開発フローの変更が必要な場合、doc/AI/flow/ を更新した
