# 成長段階ガイド

## このファイルについて
プロジェクトの成長に合わせて、いつ・どのように構成を変えるかのガイド。
各Stageの構成例は言語非依存で記載。言語固有の詳細は PACKAGE_PATTERNS.md を参照。

---

## Stage 1：立ち上げ期

### 目安
- 画面数：1〜3
- 開発者：1〜2人
- DBテーブル数：少ない

### 構成例

```text
project-root/
├── AI_CONTEXT.md
├── doc/
│   ├── flow/
│   │   ├── 00_flow.md
│   │   ├── step/
│   │   └── template/
│   ├── project/
│   │   └── feature-xxx/
│   │       ├── REQUIREMENTS.md
│   │       ├── DESIGN.md
│   │       ├── TESTLIST.md
│   │       └── IMPLEMENTATION_PLAN.md
│   ├── guide/
│   │   ├── ARCHITECTURE_GUIDE.md
│   │   ├── PACKAGE_PATTERNS.md
│   │   └── STAGE_GUIDE.md
│   └── rules/
│       ├── PACKAGE_RULES.md
│       └── GLOSSARY.md
├── src/
│   ├── app/
│   │   ├── userList/                # 画面：ユーザー一覧
│   │   │   ├── controller
│   │   │   ├── service
│   │   │   └── data/
│   │   │       ├── UserListRequest
│   │   │       └── UserListResponse
│   │   └── userDetail/              # 画面：ユーザー詳細
│   │       ├── controller
│   │       ├── service
│   │       └── data/
│   │           └── UserDetailResponse
│   ├── common/
│   │   ├── utils/
│   │   └── constants/
│   └── shared/
│       ├── dto/                     # この段階ではまだ空 or 最小限
│       └── persistence/
│           ├── entity/
│           │   ├── UserEntity
│           │   └── OrderEntity
│           └── mapper/
│               ├── UserMapper
│               └── OrderMapper
├── tests/                           # src構成をミラー
│   └── app/
│       ├── userList/
│       │   └── UserListServiceTest
│       └── userDetail/
│           └── UserDetailServiceTest
├── .editorconfig
├── .gitignore
└── README.md
```

### この段階のポイント
- shared/dto/ はまだほぼ空。画面が少ないので共有する必要がない
- ビジネスロジックは各画面のserviceに書く
- 同じDBテーブルを複数画面から使っても、shared/persistence/ のEntity/Mapperを共有するだけ
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
| 同じデータ構造を3画面以上から参照 | shared/dto/ に切り出す |
| 同じ変換処理を2箇所以上にコピペ | shared/dto/ にマッピング用DTOを追加 |
| 画面内のserviceが500行超え | 画面内でservice分割を検討 |
| commonが肥大化 | カテゴリ別にサブフォルダを追加 |
| DBテーブル数が増えてentity/が見づらい | persistence/entity/ 内をサブフォルダで分類 |
| 外部I/O（HTTP、ファイル、通知等）がDB以外に増えた | shared/infra/ の新設を検討 |

### 目安
- 画面数：4〜10
- 開発者：2〜5人
- 共通で使うデータ構造が出てきた

### 構成例（Stage 1からの差分を ★ で表示）

```text
project-root/
├── AI_CONTEXT.md
├── doc/
│   ├── flow/
│   ├── project/
│   │   ├── feature-user-management/
│   │   │   ├── REQUIREMENTS.md
│   │   │   ├── DESIGN.md
│   │   │   ├── TESTLIST.md
│   │   │   └── IMPLEMENTATION_PLAN.md
│   │   └── feature-order/           # ★ 機能要求単位で成果物が増える
│   │       ├── REQUIREMENTS.md
│   │       ├── DESIGN.md
│   │       ├── TESTLIST.md
│   │       └── IMPLEMENTATION_PLAN.md
│   ├── guide/
│   └── rules/
├── src/
│   ├── app/
│   │   ├── userList/
│   │   │   ├── controller
│   │   │   ├── service
│   │   │   └── data/
│   │   ├── userDetail/
│   │   │   ├── controller
│   │   │   ├── service
│   │   │   └── data/
│   │   ├── orderList/               # ★ 画面が増える
│   │   │   ├── controller
│   │   │   ├── service
│   │   │   └── data/
│   │   ├── orderDetail/             # ★
│   │   │   ├── controller
│   │   │   ├── service
│   │   │   └── data/
│   │   ├── orderConfirm/            # ★
│   │   │   ├── controller
│   │   │   ├── service
│   │   │   └── data/
│   │   └── dashboard/               # ★
│   │       ├── controller
│   │       ├── service
│   │       └── data/
│   ├── common/
│   │   ├── utils/
│   │   │   ├── DateUtils
│   │   │   └── StringUtils          # ★ utilsが増える
│   │   └── constants/
│   │       ├── AppConstants
│   │       └── MessageConstants      # ★ 定数が増える
│   └── shared/
│       ├── dto/                      # ★ 複数画面で使うDTOが出てくる
│       │   ├── UserDto               # ★ 3画面以上で参照されるようになった
│       │   └── OrderDto              # ★
│       └── persistence/
│           ├── entity/
│           │   ├── UserEntity
│           │   ├── OrderEntity
│           │   ├── OrderItemEntity   # ★ テーブルが増える
│           │   └── ProductEntity     # ★
│           └── mapper/
│               ├── UserMapper
│               ├── OrderMapper
│               ├── OrderItemMapper   # ★
│               └── ProductMapper     # ★
├── tests/
│   └── app/
│       ├── userList/
│       │   └── UserListServiceTest
│       ├── userDetail/
│       │   └── UserDetailServiceTest
│       ├── orderList/                # ★
│       │   └── OrderListServiceTest
│       ├── orderDetail/              # ★
│       │   └── OrderDetailServiceTest
│       ├── orderConfirm/            # ★
│       │   └── OrderConfirmServiceTest
│       └── dashboard/                # ★
│           └── DashboardServiceTest
├── .editorconfig
├── .gitignore
└── README.md
```

### この段階のポイント
- shared/dto/ に「3画面以上から参照されるデータ構造」を切り出し始める
- ただしDTOにロジックは持たせない（受け渡し用のデータ構造のみ）
- 画面のserviceが大きくなったら画面内でservice分割（例：OrderConfirmService → OrderValidationService + OrderExecutionService）
- commonも必要に応じてサブフォルダやファイルが増える
- persistence/entity/ が見づらくなったらサブフォルダ化を検討

### 外部I/Oが増えた場合
- DB以外の外部連携（HTTPクライアント、ファイル操作、通知、キャッシュ等）が増えてきたら shared/infra/ の新設を検討する
- persistence/ はDB特化のまま維持し、DB以外の外部I/Oを infra/ に分離する
- 今すぐ作る必要はなく、2つ以上の異なる外部I/Oが出てきた時点で検討する

### この段階でやらないこと
- shared/ にビジネスロジック（service）を置くこと
- DDDのAggregate、Domain Event等の導入
- モノレポ化

---

## Stage 3：成熟期（必要になった場合のみ）

### 移行トリガー
| 兆候 | アクション |
|---|---|
| フロント・バック間でAPI契約の齟齬が多発 | モノレポ化を検討 |
| 同じビジネスルールが5画面以上のserviceに散在 | DDD要素の追加導入を検討 |
| チームが複数に分かれた | パッケージ間の依存方向を厳密に管理 |
| 画面数が10を大幅に超えた | app/ 内をカテゴリでグループ化 |

### 目安
- 画面数：10超
- 開発者：5人超
- フロント・バック分離 or ビジネスルールの複雑化

### 構成例A：モノレポ化（フロント・バック分離）

```text
project-root/
├── AI_CONTEXT.md
├── doc/                              # ★ docは共通のまま
│   ├── flow/
│   ├── project/
│   ├── guide/
│   └── rules/
├── packages/                         # ★ 新規追加
│   ├── frontend/                     # ★ フロントエンド
│   │   ├── src/
│   │   │   ├── app/                  # 画面単位（構成はStage 2と同じ）
│   │   │   │   ├── userList/
│   │   │   │   ├── userDetail/
│   │   │   │   ├── orderList/
│   │   │   │   └── ...
│   │   │   ├── common/
│   │   │   │   ├── utils/
│   │   │   │   └── constants/
│   │   │   └── shared/
│   │   │       └── dto/
│   │   └── tests/
│   ├── backend/                      # ★ バックエンド
│   │   ├── src/
│   │   │   ├── app/                  # 画面（API）単位
│   │   │   │   ├── userApi/
│   │   │   │   │   ├── controller
│   │   │   │   │   ├── service
│   │   │   │   │   └── data/
│   │   │   │   ├── orderApi/
│   │   │   │   │   ├── controller
│   │   │   │   │   ├── service
│   │   │   │   │   └── data/
│   │   │   │   └── ...
│   │   │   ├── common/
│   │   │   └── shared/
│   │   │       ├── dto/
│   │   │       └── persistence/
│   │   │           ├── entity/
│   │   │           └── mapper/
│   │   └── tests/
│   └── shared/                       # ★ フロント・バック共通
│       └── api-contract/             # ★ API契約（型定義・スキーマ）
│           ├── UserApi
│           └── OrderApi
├── .editorconfig
├── .gitignore
└── README.md
```

### 構成例B：DDD要素追加（ビジネスルール複雑化）

```text
project-root/
├── AI_CONTEXT.md
├── doc/
├── src/
│   ├── app/                          # 画面単位（変更なし）
│   │   ├── userList/
│   │   ├── orderList/
│   │   ├── orderDetail/
│   │   ├── orderConfirm/
│   │   └── ...
│   ├── common/
│   ├── shared/
│   │   ├── dto/
│   │   └── persistence/
│   │       ├── entity/
│   │       └── mapper/
│   └── domain/                       # ★ 新規追加：ドメイン層
│       ├── order/                    # ★ 注文ドメイン
│       │   ├── Order                 # ★ DDD Entity（ロジックを持つ）
│       │   ├── OrderItem             # ★
│       │   └── OrderService          # ★ ドメインサービス
│       └── user/                     # ★ ユーザードメイン
│           ├── User                  # ★
│           └── UserService           # ★
├── tests/
│   ├── app/
│   └── domain/                       # ★ ドメインのテスト追加
│       ├── order/
│       │   └── OrderServiceTest
│       └── user/
│           └── UserServiceTest
├── .editorconfig
├── .gitignore
└── README.md
```

### 構成例Bの補足
- domain/ のEntityはロジック（振る舞い）を持つ（DDD Entity）
- persistence/entity/ はDBテーブル用、domain/ のEntityはビジネスロジック用で別物
- 画面のserviceは domain/ のEntityやServiceを呼び出す形に変わる
- 画面service → domain service → persistence の依存方向になる
- この移行には開発フロー自体の見直しが伴う（画面単位 → ドメイン単位の設計検討）
- **Stage 2までの構成と共存可能**：一部のドメインだけ domain/ に切り出して、他は画面serviceのままにできる

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
- [ ] モノレポ化 or DDD導入のどちらかを選択した
- [ ] 段階的移行の計画を立てた
- [ ] AI_CONTEXT.md を更新した
- [ ] 開発フローの変更が必要な場合、doc/flow/ を更新した
