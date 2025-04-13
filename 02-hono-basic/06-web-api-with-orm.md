# 注意

これは Express と Sequelize を使った Web API のサンプルコードです。
HonoとPrismaのものには随時書き換えていきます。

# ORM

ORM (Object-Relational Mapping) は、オブジェクトとリレーショナルデータベースのデータをマッピングするためのツールです。

# データベースの準備

動作確認のためにデータの変更があったので元の状態に戻します。

## 初期化用SQLの準備

次のSQLを`init.sql`として保存します。

```sql
CREATE TABLE company (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  address TEXT NOT NULL
);

CREATE TABLE team (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  company_id INTEGER NOT NULL
);

CREATE TABLE employee (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  email TEXT NOT NULL,
  seniority INTEGER NOT NULL,
  team_id INTEGER NOT NULL
);

INSERT INTO company (name, address) VALUES
  ('大阪商事','大阪府大阪市'),
  ('東京貿易','東京都港区'),
  ('福岡カンパニー','福岡県福岡市');

INSERT INTO team (name, company_id) VALUES
  ('営業', (SELECT id FROM company WHERE name = '東京貿易')),
  ('開発', (SELECT id FROM company WHERE name = '東京貿易')),
  ('人事', (SELECT id FROM company WHERE name = '東京貿易')),
  ('営業', (SELECT id FROM company WHERE name = '大阪商事')),
  ('開発', (SELECT id FROM company WHERE name = '大阪商事')),
  ('人事', (SELECT id FROM company WHERE name = '大阪商事')),
  ('営業', (SELECT id FROM company WHERE name = '福岡カンパニー')),
  ('開発', (SELECT id FROM company WHERE name = '福岡カンパニー')),
  ('人事', (SELECT id FROM company WHERE name = '福岡カンパニー'));

INSERT INTO employee (name, email, seniority, team_id) VALUES
  ('田中太郎', 'taro@tanaka.jp', 5, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '東京貿易' AND team.name = '営業' LIMIT 1)),
  ('山田花子', 'hanako@yamada.jp', 11, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '東京貿易' AND team.name = '営業' LIMIT 1)),
  ('鈴木慶一', 'keichi@suzuki.jp', 15, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '東京貿易' AND team.name = '開発' LIMIT 1)),
  ('中村恵', 'megumi@nakamura.jp', 2, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '大阪商事' AND team.name = '開発' LIMIT 1)),
  ('木下祐介', 'yusuke@kinoshita.jp', 1, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '大阪商事' AND team.name = '人事' LIMIT 1)),
  ('ジョーンフガート', 'doe@fugate.com', 1, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '福岡カンパニー' AND team.name = '人事' LIMIT 1)),
  ('左雨泽', 'yuze@zuo.ch', 11, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '福岡カンパニー' AND team.name = '営業' LIMIT 1)),
  ('佐藤健', 'sato@tokyo.co.jp', 3, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '東京貿易' AND team.name = '開発' LIMIT 1)),
  ('高橋愛', 'ai@tokyo.co.jp', 7, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '東京貿易' AND team.name = '営業' LIMIT 1)),
  ('伊藤和也', 'kazuya@tokyo.co.jp', 10, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '東京貿易' AND team.name = '人事' LIMIT 1)),
  ('藤井誠', 'fujii@osaka.co.jp', 6, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '大阪商事' AND team.name = '営業' LIMIT 1)),
  ('三浦亮', 'miura@osaka.co.jp', 9, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '大阪商事' AND team.name = '開発' LIMIT 1)),
  ('大塚佳奈', 'kana@osaka.co.jp', 4, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '大阪商事' AND team.name = '人事' LIMIT 1)),
  ('村上翔', 'murakami@fukuoka.co.jp', 2, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '福岡カンパニー' AND team.name = '営業' LIMIT 1)),
  ('橋本悠', 'hashimoto@fukuoka.co.jp', 8, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '福岡カンパニー' AND team.name = '開発' LIMIT 1)),
  ('松井玲奈', 'rena@fukuoka.co.jp', 5, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '福岡カンパニー' AND team.name = '人事' LIMIT 1)),
  ('加藤直樹', 'kato@tokyo.co.jp', 12, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '東京貿易' AND team.name = '営業' LIMIT 1)),
  ('石田健一', 'ishida@osaka.co.jp', 14, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '大阪商事' AND team.name = '開発' LIMIT 1)),
  ('小林優', 'kobayashi@fukuoka.co.jp', 3, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '福岡カンパニー' AND team.name = '営業' LIMIT 1)),
  ('清水健', 'shimizu@fukuoka.co.jp', 7, (SELECT team.id FROM team JOIN company ON team.company_id = company.id WHERE company.name = '福岡カンパニー' AND team.name = '開発' LIMIT 1));

```

## データベースの削除

```powershell
$ rm database.sqlite
```

## SQLの実行

```powershell
$ sqlite3 database.sqlite < init.sql
```

## データの確認

```powershell
$ sqlite3 database.sqlite
sqlite> .tables
company   employee  team
sqlite> SELECT * FROM company;
1|大阪商事|大阪府大阪市
2|東京貿易|東京都港区
3|福岡カンパニー|福岡県福岡市
```

# モデルの準備

## Sequilize のセットアップ

`sequelize.js` を作成

```js
import { Sequelize } from 'sequelize';

export const sequelize = new Sequelize({
  dialect: 'sqlite',
  storage: './database.sqlite'
});

```

## モデルの作成

`models` ディレクトリを作成して、`Company.js` を作成します。

`models/Company.js` に次の内容を記述します。

```js
import { DataTypes } from 'sequelize';
import { sequelize } from '../sequelize.js';

const Company = sequelize.define('Company', {
  id: {
    type: DataTypes.INTEGER,
    autoIncrement: true,
    primaryKey: true,
  },
  name: {
    type: DataTypes.STRING,
    allowNull: false,
  },
  address: {
    type: DataTypes.STRING,
    allowNull: true,
  },
}, {
  tableName: 'company',
  timestamps: false,
});

export default Company;

```

# 会社の一覧の取得

会社の一覧をSequlizeを使って取得するエンドポイントを作成します。

```js
import Company from './models/Company.js';
```

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

を次のように書き換えます

```js
app.get('/companies', async (req, res) => {
  const companies = await Company.findAll();
  res.json(companies);
})
```

`http://localhost:3000/companies` にアクセスして、データが取得できることを確認します。

# 会社の取得

一度会社だけの取得として書き換えます。

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

このコードは次のようなコードに書き換わります。

```js
app.get('/companies/:id', async (req, res) => {
  const id = req.params.id;
  const company = await Company.findByPk(id);

  if (company) {
    res.json(company);
  } else {
    res.status(404).json({ message: "Not Found" });
  }
})
```

`http://localhost:3000/companies/1` にアクセスして、データが取得できることを確認します。

# 会社の作成

会社の作成をSequlizeを使って書き換えます。

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

を次のように書き換えます。

```js
app.post('/companies', express.json(), async (req, res) => {
  const name = req.body.name;
  const address = req.body.address;

  const company = await Company.create({ name, address });

  res.json({ id: company.id });
})
```

`http://localhost:3000/companies` にPOSTリクエストを送信して、データが追加されることを確認します。

# 会社の更新

会社の更新をSequlizeを使って書き換えます。

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

を次のように書き換えます。

```js
app.put('/companies/:id', express.json(), async (req, res) => {
  const id = req.params.id;
  const name = req.body.name;
  const address = req.body.address;

  const result = await Company.update({ name, address }, { where: { id } });

  if (result[0] === 1) {
    res.json({ id });
  } else {
    res.status(404).json({ message: "Not Found" });
  }
})
```

# 会社の削除

会社の削除をSequlizeを使って書き換えます。

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

を次のように書き換えます。

```js
app.delete('/companies/:id', async (req, res) => {
  const id = req.params.id;

  const result = await Company.destroy({ where: { id } });

  if (result === 1) {
    res.json({ id });
  } else {
    res.status(404).json({ message: "Not Found" });
  }
})
```

#　現在のコードの確認

ライブラリを導入してコードが簡潔になりました。

```js
import express from 'express';
import cors from 'cors';
import Company from './models/Company.js';

const app = express();

app.use(cors());

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.get('/companies', async (req, res) => {
  const companies = await Company.findAll();
  res.json(companies);
})

app.get('/companies/:id', async (req, res) => {
  const id = req.params.id;
  const company = await Company.findByPk(id);

  if (company) {
    res.json(company);
  } else {
    res.status(404).json({ message: "Not Found" });
  }
})

app.post('/companies', express.json(), async (req, res) => {
  const name = req.body.name;
  const address = req.body.address;

  const company = await Company.create({ name, address });

  res.json({ id: company.id });
})

app.put('/companies/:id', express.json(), async (req, res) => {
  const id = req.params.id;
  const name = req.body.name;
  const address = req.body.address;

  const result = await Company.update({ name, address }, { where: { id } });

  if (result[0] === 1) {
    res.json({ id });
  } else {
    res.status(404).json({ message: "Not Found" });
  }
})

app.delete('/companies/:id', async (req, res) => {
  const id = req.params.id;

  const result = await Company.destroy({ where: { id } });

  if (result === 1) {
    res.json({ id });
  } else {
    res.status(404).json({ message: "Not Found" });
  }
})

app.listen(3000, () => {
  console.log("Server is running on http://localhost:3000");
})

```

# 参照したデータの取り方

会社を取得する時に、部署の情報も取得するようにします。

## Team モデルの作成

`models/Team.js` を作成します。

```js
import { DataTypes } from 'sequelize';
import { sequelize } from '../sequelize.js';
import Company from './Company.js';

const Team = sequelize.define('Team', {
  id: {
    type: DataTypes.INTEGER,
    autoIncrement: true,
    primaryKey: true,
  },
  name: {
    type: DataTypes.STRING,
    allowNull: false,
  },
  company_id: {
    type: DataTypes.INTEGER,
    allowNull: false,
    references: {
      model: Company,
      key: 'id',
    },
  },
}, {
  tableName: 'team',
  timestamps: false,
});

export default Team;

```

`company_id` は `Company` モデルの `id` と関連付けられていることを示すために、`references` を使っています。

```js
company_id: {
  type: DataTypes.INTEGER,
  allowNull: false,
  references: {
    model: Company,
    key: 'id',
  },
},
```

## リレーションの定義

`models/index.js` を作成します。

それぞれのモデルをインポートして、リレーションを定義します。

```js
import Company from './Company.js';
import Team from './Team.js';

Team.belongsTo(Company, { foreignKey: 'company_id', as: 'company' });
Company.hasMany(Team, { foreignKey: 'company_id', as: 'teams' });

export { Company, Team };
```

## エンドポイントの修正

リレーションを定義したCompnayをインポートする。

```js
import { Company } from './models/index.js';
```

実行時に teams を含めて取得するように修正します。

```js
app.get('/companies/:id', async (req, res) => {
  const id = req.params.id;
  const company = await Company.findByPk(id, { include: 'teams' });

  if (company) {
    res.json(company);
  } else {
    res.status(404).json({ message: "Not Found" });
  }
})
```

`http://localhost:3000/companies/1` にアクセスして、データが取得できることを確認します。

# 追加課題

- 部署の作成、取得、更新、削除のエンドポイントを作成してください。
- 従業員の作成、取得、更新、削除のエンドポイントを作成してください。
- ここまでできたコードを読みやすいようにファイル分割してください。

# まとめ

この章でははじめにSQLを使ってWebAPIからデータベースにアクセスしました。

次にORMを使ってデータベースにアクセスする方法を学びました。

それぞれで同じものを実装しようとしたときにコード量が異なることがわかります。

また、ORMの場合は事前準備が必要でした。このことから実現したい目的に応じて使い分けることが重要であることがわかります。
