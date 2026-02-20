# DB設計

## このファイルについて
データベースのテーブル構成・ER図・カラム定義をまとめる。
初期開発時に作成し、テーブル追加・変更時に更新すること。

---

## テーブル一覧

| No. | テーブル名 | 概要 | 備考 |
|-----|-----------|------|------|
| 1   | [table_name] | [概要] | |

---

## ER図

```mermaid
erDiagram
    users {
        id PK
        name string
        email string
        created_at timestamp
    }
    orders {
        id PK
        user_id FK
        total decimal
        created_at timestamp
    }
    users ||--o{ orders : "has"
```
※ 上記は記載例です。プロジェクトのテーブルに合わせて書き換えてください。

---

## テーブル定義

### [table_name]

| カラム名 | 型 | NULL許容 | デフォルト | 制約 | 説明 |
|---------|---|---------|-----------|------|------|
| id | [型] | NO | - | PK | 主キー |
| [column] | [型] | [YES/NO] | [値] | [FK/UNIQUE等] | [説明] |
| created_at | [型] | NO | CURRENT_TIMESTAMP | - | 作成日時 |
| updated_at | [型] | NO | CURRENT_TIMESTAMP | - | 更新日時 |

---

## インデックス設計

| テーブル | インデックス名 | カラム | 種類 | 用途 |
|---------|---------------|-------|------|------|
| [table] | [idx_name] | [columns] | [UNIQUE/INDEX] | [用途] |

---

## マイグレーション履歴

| バージョン | 日付 | 内容 |
|-----------|------|------|
| V1 | [日付] | 初期スキーマ作成 |
