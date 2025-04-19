# 顧客管理アプリ

この章ではブラウザで完結する顧客管理アプリを作る演習をします。

データの永続化に関しては localStorage を使用します。そのため、実際に利用できるものではありません。
後に localStorageに書き込んでいる処理をWebAPIなどに置き換えることにより、実際に利用できるものになります。

この章では [Reactの基本](../01-react-basic/README.md) の知識を前提としています。

# 演習の流れ

段階を追って機能追加をしていきます。発注者が新しい要望を足してきたり、自分で開発している時に新しい要望を思いついたりすることがあると思います。

そのような場合を想定しています。

# 演習の方針

開発全体の流れに集中するために次の方針で進めます。

- TypeScript を使わない
- CSS は書かない
- 利用するライブラリは最低限

余力のある人は TypeScript や CSS を追加してみてください。

# UMLの利用

一部UMLを使って設計を説明します。ただし、厳密な使い方はしていません。

作るシステムの内容が理解できるように説明のために使っています。

## UMLの例

次のものはデータの関係を表す図です。

```mermaid
erDiagram
  company["会社"]
  team["部署"]
  employee["社員"]
  company ||--o{ team : has
  team ||--o{ employee : has
```

データ構造については [データベースについて](../02-hono-basic/04-database.md) のはじめに図解しています。

厳密に値を表すときもあります。

```mermaid
erDiagram
  company["会社"] {
    string id "会社ID"
    string name "会社名"
  }
  team["部署"] {
    string id "部署ID"
    string name "部署名"
    string companyId "会社ID"
  }
  employee["社員"] {
    string id "社員ID"
    string name "名前"
    string teamId "部署ID"
  }
  company ||--o{ team : has
  team ||--o{ employee : has
```

次のような画面遷移を説明する図も使います。

ここでは主な遷移方法に着目しています。

```mermaid
graph LR
  CompanyCollection[会社一覧画面] --> CompanySingle[会社詳細画面]
  CompanySingle --> TeamCreate[部署作成画面]
  CompanySingle --> TeamSingle[部署詳細画面]
  TeamSingle --> EmployeeSingle[社員詳細画面]
  TeamSingle --> EmployeeNew[社員詳細画面]
```

# Web API を使わずに localStorage を使う

本来は Web API を使ってデータの永続化を行いますが、この章では localStorage を使ってデータの永続化を行います。

```mermaid
---
title: 本来の形
---

graph LR
  subgraph ブラウザ
    Client[クライアント]
  end

  subgraph サーバー
    WebAPI[WebAPI]
    Database[データベース]
  end

  Client -->|非同期処理| WebAPI
  WebAPI --> Database

```

```mermaid
---
title: この章の形
---

graph LR
  subgraph ブラウザ
    direction LR
    Client[クライアント]
    localStorage[localStorage]
  end
  Client -->|非同期処理| localStorage
```

localStorage へのアクセスは同期処理で行えますが、　**WebAPIに置き換えられるように非同期処理で行います。**

# 画面の装飾について

この章では画面の装飾については触れません。

余裕がある人は CSS を使って装飾してみてください。

その他にも次のようなライブラリを使うこともできます。

- [MUI](https://mui.com/)
- [Bootstrap](https://getbootstrap.com/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Ant Design](https://ant.design/)
- [Chakra UI](https://chakra-ui.com/)

それでは始めていきましょう。

[はじめる](./01-cutomer.md)
