# Webアプリ開発の基本を学ぶハンズオン

この講義では、**SPA (React) + API (Express) + DB (SQLite) の構成でWebアプリを開発する基本** を学びます。
フロントエンドを「クライアント」、バックエンドを「API」、データベースを「DB」と呼び、それぞれの役割を明確にします。

---

## 目的

この講義の目的は、以下のスキルを習得することです：
1. React を用いたクライアントの構築
2. Express を用いた API の構築
3. クライアントと API の連携
4. データベースの利用と ORM の活用
5. API の設計（コントローラー、ユースケース、モデルの分割）
6. それらを組み合わせたシンプルな顧客管理アプリの作成

---

## ルール・制約

- **ESM (ECMAScript Modules) を使用する**
- **TypeScript は使用しない (JSのみ)**
- **UIライブラリは使用しない (純粋なHTML/CSS)**
- **Reduxなどの状態管理ライブラリは使用しない**
- **CSS は最低限**
- **DB は SQLite**
- **ORM は Sequelize**
- **代替案として Hono と Prisma も考慮するが、今回は扱わない**

---

# 目次

## 1. [React プロジェクトの作成](01-react-setup.md)
**目標: Hello World と useState を実装する**
- 1.1 Vite を使って React プロジェクトを作成する
- 1.2 プロジェクトのディレクトリ構成
- 1.3 Hello World を表示する
- 1.4 useState でカウンターを実装する

---

## 2. [Express プロジェクトの作成](02-express-setup.md)
**目標: JSON で Hello World を返す API を作成する**
- 2.1 Express プロジェクトを作成する
- 2.2 package.json で `type: "module"` を設定する
- 2.3 シンプルな GET API を作成する
- 2.4 サーバーを起動して動作確認する

---

## 3. [クライアントから API を呼び出す](03-client-api-communication.md)
**目標: API の値を画面に表示する (useEffect を活用)**
- 3.1 fetch を使って API を呼び出す
- 3.2 useEffect を使って非同期データを取得する
- 3.3 API のレスポンスを画面に表示する
- 3.4 エラーハンドリングを追加する

---

## 4. [DB の用意](04-database-setup.md)
**目標: SQLite に User テーブルを作成し、SELECT 文を実行する**
- 4.1 SQLite のセットアップ
- 4.2 User テーブルの作成
- 4.3 データの追加と SELECT 文の実行
- 4.4 SQLite のデータを確認する

---

## 5. [ORM を介して取得した値を API から返す](05-orm-api-integration.md)
**目標: Sequelize を使って User テーブルの値を API から返す**
- 5.1 Sequelize の導入
- 5.2 モデルの作成 (User)
- 5.3 ユースケース層の作成 (User の取得処理)
- 5.4 コントローラー層の作成 (API のエンドポイント)
- 5.5 クライアントから取得データを表示する

---

## 6. [顧客管理アプリの概要と構成](06-customer-management-overview.md)
**目標: 顧客管理システムの全体構成を整理し、開発の流れを明確にする**
- 6.1 アプリの機能概要
- 6.2 システム構成 (フロント・API・DB)
- 6.3 機能の開発手順

---

## 7. [顧客一覧の表示](07-customer-list.md)
**目標: データベースから顧客情報を取得し、React で表示する**
- 7.1 データベースのセットアップ
- 7.2 API (`GET /api/customers`) の実装
- 7.3 クライアントでデータを取得・表示
- 7.4 エラーハンドリングの追加

---

## 8. [顧客の追加](08-customer-add.md)
**目標: 顧客を新規登録できるようにする**
- 8.1 React Router DOM を導入し、ページ遷移を実装
- 8.2 API (`POST /api/customers`) の実装
- 8.3 フォームを作成し、バリデーションを追加

---

## 9. [顧客の詳細ページ](09-customer-detail.md)
**目標: 顧客の詳細情報を表示できるようにする**
- 9.1 API (`GET /api/customers/:id`) の実装
- 9.2 React で詳細ページを作成
- 9.3 ルーティングの設定

---

## 10. [顧客の編集・削除](10-customer-edit-delete.md)
**目標: 既存の顧客情報を編集・削除できるようにする**
- 10.1 API (`PUT /api/customers/:id`, `DELETE /api/customers/:id`) の実装
- 10.2 フォームを作成し、データを更新
- 10.3 削除ボタンを追加し、削除処理を実装

---

## 11. [顧客に紐づくノート機能](11-customer-notes.md)
**目標: 顧客ごとにメモを追加・表示できるようにする**
- 11.1 ノート用のデータベース作成
- 11.2 API (`POST /api/customers/:id/notes`, `GET /api/customers/:id/notes`) の実装
- 11.3 クライアントでノートの管理を実装

---

## 12. [グループ単位で顧客を管理](12-customer-groups.md)
**目標: グループを作成し、グループごとに顧客を管理できるようにする**
- 12.1 API (`POST /api/groups`, `GET /api/groups/:id/customers`) の実装
- 12.2 クライアントでグループごとの顧客を表示

---

## 13. [機能拡張と今後の展開](13-customer-advanced-features.md)
**目標: 検索機能・ページネーション・CSV エクスポートを実装**
- 13.1 検索 (`GET /api/customers?query=`) の追加
- 13.2 ページネーション (`GET /api/customers?page=1&limit=10`) の実装
- 13.3 CSV エクスポート (`GET /api/customers/export`) の実装

---

## まとめ

この講義では、**顧客管理アプリ** を構築しながら Web アプリ開発の基本を学びました。
