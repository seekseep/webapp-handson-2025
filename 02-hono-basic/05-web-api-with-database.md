# 注意

これは Express を使った Web API のサンプルコードです。
Honoをつかったものに随時書き換えていきます。

# ORM を介して取得した値を API から返す

このセクションでは、**Sequelize を使ってデータベースからデータを取得し、API を通じてクライアントに返す** 仕組みを実装します。

---

# プロジェクトの作成

まずは、プロジェクトを作成します。

```bash
$ mkdir sequelize-api
$ cd sequelize-api
$ npm init -y
```

## プロジェクトの設定

`"type"` を `"module"` に設定します。

```json
{
  "name": "sequelize-api",
  "version": "1.0.0",
  "type": "module",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

## 依存パッケージのインストール

```powershell
> npm i express sequelize sqlite3 cors
```

| パッケージ | 説明 |
| --- | --- |
| express | Web アプリケーションフレームワーク |
| sequelize | ORM ライブラリ |
| sqlite3 | データベース |
| cors | CORS 対策 |

## ディレクトリ構成

次のような状態です

```
.
├── node_modules
├── package-lock.json
└── package.json
```

## app.js

`app.js` を作成します。

```js
import express from 'express';
import cors from 'cors';

const app = express();

app.use(cors());

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(3000, () => {
  console.log("Server is running on http://localhost:3000");
})

```

`http://localhost:3000` にアクセスすると `Hello World!` が表示されます。

# データベースの用意

データベースには SQLite を使用します。

## データベースの作成

```bash
> sqlite3 database.sqlite
```

## テーブルの作成

会社、部署、従業員のテーブルを作成します。

```sql
CREATE TABLE company (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  address TEXT NOT NULL
);
```

```sql
CREATE TABLE team (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  company_id INTEGER NOT NULL
);
```

```sql
CREATE TABLE employee (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  email TEXT NOT NULL,
  seniority INTEGER NOT NULL,
  team_id INTEGER NOT NULL
);
```

## データの挿入

作成したテーブルにデータを挿入します。

```sql
INSERT INTO company (name, address) VALUES
  ("大阪商事","大阪府大阪市"),
  ("東京貿易","東京都港区"),
  ("福岡カンパニー","福岡県福岡市");
```

```sql
INSERT INTO team (name, company_id) VALUES
  ("営業", (SELECT id FROM company WHERE name = "東京貿易")),
  ("開発", (SELECT id FROM company WHERE name = "東京貿易")),
  ("人事", (SELECT id FROM company WHERE name = "東京貿易")),
  ("営業", (SELECT id FROM company WHERE name = "大阪商事")),
  ("開発", (SELECT id FROM company WHERE name = "大阪商事")),
  ("人事", (SELECT id FROM company WHERE name = "大阪商事")),
  ("営業", (SELECT id FROM company WHERE name = "福岡カンパニー")),
  ("開発", (SELECT id FROM company WHERE name = "福岡カンパニー")),
  ("人事", (SELECT id FROM company WHERE name = "福岡カンパニー"));
```

```sql
INSERT INTO employee (name, email, seniority, team_id) VALUES
  ("田中太郎", "taro@tanaka.jp", 5, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "東京貿易" AND team.name = "営業" LIMIT 1)),
  ("山田花子", "hanako@yamada.jp", 11, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "東京貿易" AND team.name = "営業" LIMIT 1)),
  ("鈴木慶一", "keichi@suzuki.jp", 15, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "東京貿易" AND team.name = "開発" LIMIT 1)),
  ("中村恵", "megumi@nakamura.jp", 2, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "大阪商事" AND team.name = "開発" LIMIT 1)),
  ("木下祐介", "yusuke@kinoshita.jp", 1, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "大阪商事" AND team.name = "人事" LIMIT 1)),
  ("ジョーンフガート", "doe@fugate.com", 1, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "福岡カンパニー" AND team.name = "人事" LIMIT 1)),
  ("左雨泽", "yuze@zuo.ch", 11, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "福岡カンパニー" AND team.name = "営業" LIMIT 1)),
  ("佐藤健", "sato@tokyo.co.jp", 3, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "東京貿易" AND team.name = "開発" LIMIT 1)),
  ("高橋愛", "ai@tokyo.co.jp", 7, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "東京貿易" AND team.name = "営業" LIMIT 1)),
  ("伊藤和也", "kazuya@tokyo.co.jp", 10, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "東京貿易" AND team.name = "人事" LIMIT 1)),
  ("藤井誠", "fujii@osaka.co.jp", 6, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "大阪商事" AND team.name = "営業" LIMIT 1)),
  ("三浦亮", "miura@osaka.co.jp", 9, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "大阪商事" AND team.name = "開発" LIMIT 1)),
  ("大塚佳奈", "kana@osaka.co.jp", 4, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "大阪商事" AND team.name = "人事" LIMIT 1)),
  ("村上翔", "murakami@fukuoka.co.jp", 2, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "福岡カンパニー" AND team.name = "営業" LIMIT 1)),
  ("橋本悠", "hashimoto@fukuoka.co.jp", 8, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "福岡カンパニー" AND team.name = "開発" LIMIT 1)),
  ("松井玲奈", "rena@fukuoka.co.jp", 5, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "福岡カンパニー" AND team.name = "人事" LIMIT 1)),
  ("加藤直樹", "kato@tokyo.co.jp", 12, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "東京貿易" AND team.name = "営業" LIMIT 1)),
  ("石田健一", "ishida@osaka.co.jp", 14, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "大阪商事" AND team.name = "開発" LIMIT 1)),
  ("小林優", "kobayashi@fukuoka.co.jp", 3, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "福岡カンパニー" AND team.name = "営業" LIMIT 1)),
  ("清水健", "shimizu@fukuoka.co.jp", 7, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = "福岡カンパニー" AND team.name = "開発" LIMIT 1));
```

## データの確認

```sql
SELECT * FROM company;
1|大阪商事|大阪府大阪市
2|東京貿易|東京都港区
3|福岡カンパニー|福岡県福岡市
```

```sql
SELECT * FROM team;
1|営業|2
2|開発|2
3|人事|2
4|営業|1
5|開発|1
6|人事|1
7|営業|3
8|開発|3
9|人事|3
```

```sql
SELECT * FROM employee;
1|田中太郎|taro@tanaka.jp|5|1
2|山田花子|hanako@yamada.jp|11|1
3|鈴木慶一|keichi@suzuki.jp|15|2
4|中村恵|megumi@nakamura.jp|2|5
5|木下祐介|yusuke@kinoshita.jp|1|6
6|ジョーンフガート|doe@fugate.com|1|9
7|左雨泽|yuze@zuo.ch|11|7
8|佐藤健|sato@tokyo.co.jp|3|2
9|高橋愛|ai@tokyo.co.jp|7|1
10|伊藤和也|kazuya@tokyo.co.jp|10|3
11|藤井誠|fujii@osaka.co.jp|6|4
12|三浦亮|miura@osaka.co.jp|9|5
13|大塚佳奈|kana@osaka.co.jp|4|6
14|村上翔|murakami@fukuoka.co.jp|2|7
15|橋本悠|hashimoto@fukuoka.co.jp|8|8
16|松井玲奈|rena@fukuoka.co.jp|5|9
17|加藤直樹|kato@tokyo.co.jp|12|1
18|石田健一|ishida@osaka.co.jp|14|5
19|小林優|kobayashi@fukuoka.co.jp|3|7
20|清水健|shimizu@fukuoka.co.jp|7|8
```

# 会社の一覧の取得

まずは、会社の一覧を取得するエンドポイントを作成します。

最初はSQLを直接実行してデータを取得します。

## エンドポイントの作成

```js
app.get('/companies', async (req, res) => {
  const companies = []

  companies.push({
    id: 1,
    name: "大阪商事",
    address: "大阪府大阪市"
  })

  companies.push({
    id: 2,
    name: "東京貿易",
    address: "東京都港区"
  })

  companies.push({
    id: 3,
    name: "福岡カンパニー",
    address: "福岡県福岡市"
  })

  res.json(companies)
})
```

## curl を使った動作確認

```bash
$ curl http://localhost:3000/companies
```

## ブラウザの開発者ツールを使った動作確認

ブラウザの開発者ツールを使って、会社の一覧を取得します。

```js
const response = await fetch('http://localhost:3000/companies')
const data = await response.json()
console.log(data)
```

## データベースからの取得

SQLを実行してデータを取得します。

```js
import express from 'express';
import cors from 'cors';
import sqlite from 'sqlite3';
```

次の処理では、データベースを開いて、`company` テーブルからデータを取得しています。

```js
app.get('/companies', async (req, res) => {
  const db = new sqlite.Database('./database.sqlite');

  const companies = []

  await new Promise((resolve, reject) => {
    db.serialize(() => {
      db.all('SELECT * FROM company', (err, rows) => {
        if (err) {
          reject(err);
        } else {
          for (const row of rows) {
            companies.push({
              id: row.id,
              name: row.name,
              address: row.address
            })
          }
        }
        resolve();
      })
    })
  })

  db.close();

  res.json(companies)
})
```

## curl を使った動作確認

```bash
$ curl http://localhost:3000/companies
```

## ブラウザの開発者ツールを使った動作確認

ブラウザの開発者ツールを使って、会社の一覧を取得します。

```js
const response = await fetch('http://localhost:3000/companies')
const data = await response.json()
console.log(data)
```

# 会社の取得

会社のIDを指定して、会社の情報を取得するエンドポイントを作成します。

```js
app.get('/companies/:id', async (req, res) => {
  const db = new sqlite.Database('./database.sqlite');

  const id = req.params.id;

  let company = null;

  await new Promise((resolve, reject) => {
    db.serialize(() => {
      db.get('SELECT * FROM company WHERE id = ?', [id], (err, row) => {
        if (err) {
          reject(err);
        } else {
          company = {
            id: row.id,
            name: row.name,
            address: row.address
          }
        }
        resolve();
      })
    })
  })

  db.close();

  if (company) {
    res.json(company)
  } else {
    res.status(404).json({ message: "Not Found" })
  }
})
```

## curl を使った動作確認

```bash
$ curl http://localhost:3000/companies/1
```

## ブラウザの開発者ツールを使った動作確認

ブラウザの開発者ツールを使って、会社の情報を取得します。

```js
const response = await fetch('http://localhost:3000/companies/1')
const data = await response.json()
console.log(data)
```

# 会社の作成

新しい会社を作成するエンドポイントを作成します。

```js
app.post('/companies', express.json(), async (req, res) => {
  const db = new sqlite.Database('./database.sqlite');

  const name = req.body.name;
  const address = req.body.address;

  let id = null;

  await new Promise((resolve, reject) => {
    db.serialize(() => {
      db.run('INSERT INTO company (name, address) VALUES (?, ?)', [name, address], function(err) {
        if (err) {
          reject(err);
        } else {
          id = this.lastID;
        }
        resolve();
      })
    })
  })

  db.close();

  res.json({ id })
})
```

## curl  を使ったテスト

curl を使って、新しい会社を作成します。

```bash
$ curl -X POST -H "Content-Type: application/json" -d '{"name":"札幌商事","address":"北海道札幌市"}' http://localhost:3000/companies
```

追加されていることを確認する。

```bash
$ curl http://localhost:3000/companies
```

## 開発者ツールを使ったテスト

ブラウザで開発者ツールを使って、新しい会社を作成します。

```js
const response = await fetch('http://localhost:3000/companies', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: '名古屋商事',
    address: '愛知県名古屋市'
  })
})
const data = await response.json()
console.log(data)
```

追加されていることを確認する

```js
const response = await fetch('http://localhost:3000/companies')
const data = await response.json()
console.log(data)
```

## 会社の更新

会社の情報を更新するエンドポイントを作成します。

```js
app.put('/companies/:id', express.json(), async (req, res) => {
  const db = new sqlite.Database('./database.sqlite');

  const id = req.params.id;
  const name = req.body.name;
  const address = req.body.address;

  let result = null;

  await new Promise((resolve, reject) => {
    db.serialize(() => {
      db.run('UPDATE company SET name = ?, address = ? WHERE id = ?', [name, address, id], function(err) {
        if (err) {
          reject(err);
        } else {
          result = this.changes;
        }
        resolve();
      })
    })
  })

  db.close();

  if (result === 1) {
    res.json({ id })
  } else {
    res.status(404).json({ message: "Not Found" })
  }
})
```

## curl を使ったテスト

curl を使って、会社の情報を更新します。

```bash
$ curl -X PUT -H "Content-Type: application/json" -d '{"name":"アメリカ商事","address":"ヒューストン"}' http://localhost:3000/companies/1
```

更新されていることを確認する。

```bash
$ curl http://localhost:3000/companies/1
```

## 開発者ツールを使ったテスト

ブラウザで開発者ツールを使って、会社の情報を更新します。

```js
const response = await fetch('http://localhost:3000/companies/1', {
  method: 'PUT',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'ブラジル商事',
    address: 'リオデジャネイロ'
  })
})
const data = await response.json()
console.log(data)
```

更新されていることを確認する。

```js
const response = await fetch('http://localhost:3000/companies/1')
const data = await response.json()
console.log(data)
```

## 会社の削除

会社を削除するエンドポイントを作成します。

```js
app.delete('/companies/:id', async (req, res) => {
  const db = new sqlite.Database('./database.sqlite');

  const id = req.params.id;

  let result = null;

  await new Promise((resolve, reject) => {
    db.serialize(() => {
      db.run('DELETE FROM company WHERE id = ?', [id], function(err) {
        if (err) {
          reject(err);
        } else {
          result = this.changes;
        }
        resolve();
      })
    })
  })

  db.close();

  if (result === 1) {
    res.json({ id })
  } else {
    res.status(404).json({ message: "Not Found" })
  }
})
```

## curl を使ったテスト

curl を使って、会社を削除します。

```bash
$ curl -X DELETE http://localhost:3000/companies/1
```

削除されていることを確認する。

```bash
$ curl http://localhost:3000/companies
```

## 開発者ツールを使ったテスト

ブラウザで開発者ツールを使って、会社を削除します。

```js
const response = await fetch('http://localhost:3000/companies/1', {
  method: 'DELETE'
})
const data = await response.json()
console.log(data)
```

削除されていることを確認する。

```js
const response = await fetch('http://localhost:3000/companies')
const data = await response.json()
console.log(data)
```

# 会社の取得の機能追加

会社を取得した時に部署の内容も一緒に取得できるようにしましょう。

## データ構造

次のようなデータを返すとします。

```json
{
  "id": 1,
  "name": "大阪商事",
  "address": "大阪府大阪市",
  "teams": [
    {
      "id": 4,
      "name": "営業"
    },
    {
      "id": 5,
      "name": "開発"
    },
    {
      "id": 6,
      "name": "人事"
    }
  ]
}
```

## SQL

company.id が 1 の会社の部署を取得する SQL は次のようになります。

```sql
SELECT
  company.id AS company_id,
  company.name AS company_name,
  company.address AS
  company_address,
  team.id AS team_id,
  team.name AS team_name
FROM
  company
JOIN
  team
ON
  company.id = team.company_id
WHERE
  company.id = 1
```

## エンドポイント

```js
app.get('/companies/:id', async (req, res) => {
  const db = new sqlite.Database('./database.sqlite');

  const id = req.params.id;

  let company = null;

  await new Promise((resolve, reject) => {
    db.serialize(() => {
      db.get(`
        SELECT
          company.id AS company_id,
          company.name AS company_name,
          company.address AS
          company_address,
          team.id AS team_id,
          team.name AS team_name
        FROM
          company
        JOIN
          team
        ON
          company.id = team.company_id
        WHERE
          company.id = ?
      `, [id], (err, row) => {
        if (err) {
          reject(err);
        } else {

          if (company === null) {
            company = {
              id: row.company_id,
              name: row.company_name,
              address: row.company_address,
              teams: []
            }
          }

          company.teams.push({
            id: row.team_id,
            name: row.team_name
          })
        }
        resolve();
      })
    })
  })

  db.close();

  if (company) {
    res.json(company)
  } else {
    res.status(404).json({ message: "Not Found" })
  }
})
```

各行は次のようなデータが入っています。

```json
{
  "company_id": 1,
  "company_name": "大阪商事",
  "company_address": "大阪府大阪市",
  "team_id": 4,
  "team_name": "営業"
}
```

```json
{
  "company_id": 1,
  "company_name": "大阪商事",
  "company_address": "大阪府大阪市",
  "team_id": 5,
  "team_name": "開発"
}
```

```json
{
  "company_id": 1,
  "company_name": "大阪商事",
  "company_address": "大阪府大阪市",
  "team_id": 6,
  "team_name": "人事"
}
```

なので最初の行の処理のときは、company が null なので、company を初期化します。

すべての行ごとに、company.teams に部署の情報を追加します。

```js
if (company === null) {
  company = {
    id: row.company_id,
    name: row.company_name,
    address: row.company_address,
    teams: []
  }
}

company.teams.push({
  id: row.team_id,
  name: row.team_name
})
```

## curl を使った動作確認

```bash
$ curl http://localhost:3000/companies/2
```

## ブラウザの開発者ツールを使った動作確認

ブラウザの開発者ツールを使って、会社の情報を取得します。

```js
const response = await fetch('http://localhost:3000/companies/2')
const data = await response.json()
console.log(data)
```

# コードの確認

ここまで直接SQLを操作しながらWebAPIを開発してきました。

コード全体を確認してみましょう。

```js
import express from 'express';
import cors from 'cors';
import sqlite from 'sqlite3';

const app = express();

app.use(cors());

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.get('/companies', async (req, res) => {
  const db = new sqlite.Database('./database.sqlite');

  const companies = []

  await new Promise((resolve, reject) => {
    db.serialize(() => {
      db.all('SELECT * FROM company', (err, rows) => {
        if (err) {
          reject(err);
        } else {
          for (const row of rows) {
            companies.push({
              id: row.id,
              name: row.name,
              address: row.address
            })
          }
        }
        resolve();
      })
    })
  })

  db.close();

  res.json(companies)
})

app.get('/companies/:id', async (req, res) => {
  const db = new sqlite.Database('./database.sqlite');

  const id = req.params.id;

  let company = null;

  await new Promise((resolve, reject) => {
    db.serialize(() => {
      db.get(`
        SELECT
          company.id AS company_id,
          company.name AS company_name,
          company.address AS
          company_address,
          team.id AS team_id,
          team.name AS team_name
        FROM
          company
        JOIN
          team
        ON
          company.id = team.company_id
        WHERE
          company.id = ?
      `, [id], (err, row) => {
        if (err) {
          reject(err);
        } else {

          if (company === null) {
            company = {
              id: row.company_id,
              name: row.company_name,
              address: row.company_address,
              teams: []
            }
          }

          company.teams.push({
            id: row.team_id,
            name: row.team_name
          })
        }
        resolve();
      })
    })
  })

  db.close();

  if (company) {
    res.json(company)
  } else {
    res.status(404).json({ message: "Not Found" })
  }
})

app.post('/companies', express.json(), async (req, res) => {
  const db = new sqlite.Database('./database.sqlite');

  const name = req.body.name;
  const address = req.body.address;

  let id = null;

  await new Promise((resolve, reject) => {
    db.serialize(() => {
      db.run('INSERT INTO company (name, address) VALUES (?, ?)', [name, address], function(err) {
        if (err) {
          reject(err);
        } else {
          id = this.lastID;
        }
        resolve();
      })
    })
  })

  db.close();

  res.json({ id })
})

app.put('/companies/:id', express.json(), async (req, res) => {
  const db = new sqlite.Database('./database.sqlite');

  const id = req.params.id;
  const name = req.body.name;
  const address = req.body.address;

  let result = null;

  await new Promise((resolve, reject) => {
    db.serialize(() => {
      db.run('UPDATE company SET name = ?, address = ? WHERE id = ?', [name, address, id], function(err) {
        if (err) {
          reject(err);
        } else {
          result = this.changes;
        }
        resolve();
      })
    })
  })

  db.close();

  if (result === 1) {
    res.json({ id })
  } else {
    res.status(404).json({ message: "Not Found" })
  }
})

app.delete('/companies/:id', async (req, res) => {
  const db = new sqlite.Database('./database.sqlite');

  const id = req.params.id;

  let result = null;

  await new Promise((resolve, reject) => {
    db.serialize(() => {
      db.run('DELETE FROM company WHERE id = ?', [id], function(err) {
        if (err) {
          reject(err);
        } else {
          result = this.changes;
        }
        resolve();
      })
    })
  })

  db.close();

  if (result === 1) {
    res.json({ id })
  } else {
    res.status(404).json({ message: "Not Found" })
  }
})

app.listen(3000, () => {
  console.log("Server is running on http://localhost:3000");
})

```

コネクションの作成とクローズが各エンドポイントで行われているため、コードが冗長になっています。
その他にもSQLを直接操作しているため、エンドポイントごとにSQLが記述されているため、コードが複雑になっています。

これらの課題を解決するために、ORMを使って整理していきます。
