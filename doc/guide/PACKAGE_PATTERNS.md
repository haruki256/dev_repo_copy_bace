# 言語別パッケージ構成例

## このファイルについて
ベースリポジトリの構成を、各言語・フレームワークに適用する際の具体例。
プロジェクト開始時にAIがこのファイルを参照し、言語に合った構成を提案する。

## Java（Spring Boot）

### src構成
```text
src/
  main/
    java/com/example/projectname/
      app/
        userList/                    # 画面：ユーザー一覧
          UserListController.java
          UserListService.java
          data/
            UserListRequest.java
            UserListResponse.java
        userDetail/                  # 画面：ユーザー詳細
          UserDetailController.java
          UserDetailService.java
          data/
            UserDetailResponse.java
      common/
        utils/
          DateUtils.java
        constants/
          AppConstants.java
      shared/
        dto/
          UserDto.java
          OrderDto.java
        persistence/
          record/
            UserRecord.java
            OrderRecord.java
          mapper/
            UserMapper.java
            OrderMapper.java
      config/                        # Spring設定クラス
      Application.java
    resources/
      mapper/                        # MyBatis XMLマッパー
      config/
        application.yml
      static/
      templates/
  test/
    java/com/example/projectname/
      app/
        userList/
          UserListServiceTest.java
        userDetail/
          UserDetailServiceTest.java
      shared/
        persistence/
          mapper/
            UserMapperTest.java
```

### 補足
- テストは src/test/ に src/main/ のミラー構成で配置
- Spring Boot固有のconfig/はapp/の外に配置
- MyBatis XMLマッパーは resources/mapper/ に配置

## TypeScript（Next.js / App Router）

### src構成
```text
src/
  app/                               # Next.js App Router
    user-list/
      page.tsx                       # 画面コンポーネント
      actions.ts                     # Server Actions（controller相当）
      service.ts                     # ビジネスロジック
      data/
        types.ts                     # Request/Response型定義
    user-detail/
      page.tsx
      actions.ts
      service.ts
      data/
        types.ts
  common/
    utils/
      date.ts
    constants/
      index.ts
  shared/
    dto/
      user.ts
      order.ts
    persistence/
      record/
        user.ts
        order.ts
      repository/                    # DB操作（Prisma等）
        userRepository.ts
        orderRepository.ts
  __tests__/                         # テスト（src横置きも可）
    user-list/
      service.test.ts
    user-detail/
      service.test.ts
```

### 補足
- テスト配置は __tests__/ またはファイル横置き（xxx.test.ts）を選択
- persistence/mapper/ の代わりに repository/ を使用（ORM前提）

## Python（FastAPI）

### src構成
```text
src/
  app/
    user_list/
      router.py                      # エンドポイント（controller相当）
      service.py
      data/
        schemas.py                   # Request/Response（Pydantic）
    user_detail/
      router.py
      service.py
      data/
        schemas.py
  common/
    utils/
      date_utils.py
    constants.py
  shared/
    dto/
      user_dto.py
      order_dto.py
    persistence/
      record/
        user_record.py               # SQLAlchemy Model等
        order_record.py
      repository/
        user_repository.py
        order_repository.py
  main.py
  tests/
    app/
      test_user_list_service.py
      test_user_detail_service.py
    shared/
      persistence/
        test_user_repository.py
```

### 補足
- テストは tests/ ディレクトリにミラー構成
- Pydanticのスキーマは各画面の data/schemas.py に配置

## Flutter（モバイルアプリ）

### lib構成
```text
lib/
  app/
    user_list/
      user_list_screen.dart          # 画面Widget
      user_list_controller.dart      # 状態管理
      user_list_service.dart
      data/
        user_list_state.dart
    user_detail/
      user_detail_screen.dart
      user_detail_controller.dart
      user_detail_service.dart
      data/
        user_detail_state.dart
  common/
    utils/
    constants/
    widgets/                         # 共通Widget
  shared/
    dto/
      user_dto.dart
      order_dto.dart
    persistence/
      record/
      repository/
test/
  app/
    user_list/
      user_list_service_test.dart
```

### 補足
- 状態管理はRiverpod/Bloc等に合わせてcontrollerの実装を変更
- 共通Widgetは common/widgets/ に配置

## 構成選択後のチェックリスト
プロジェクト開始時、言語を選んだら以下を確認：
- [ ] 画面パッケージの命名規則を決めた（GLOSSARY.mdに記載）
- [ ] テスト配置方針を決めた
- [ ] DB/ORMの選定と persistence/ 内の構成を決めた
- [ ] AI_CONTEXT.md にプロジェクト概要を記入した