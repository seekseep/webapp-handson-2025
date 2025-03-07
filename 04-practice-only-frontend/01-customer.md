# 顧客を管理する

お客さんから顧客情報を管理したいという要望がありました。
あなたは顧客管理アプリを作ろうと思います。

プロジェクトの先行きも不透明なためある程度の確信を持ってから本格的な開発を行いたいと言われました。

そこでプロトタイプを作ってみることにしました。

# データを考える

顧客の情報には何があるかを整理しました。

営業先のお客さんの連絡先を調べるのに名刺フォルダから探し出すのが大変なのでシステムで一覧したいという要望があったので、顧客の名前、メールアドレス、電話番号、住所を管理したいと思います。

```mermaid
erDiagram
  customer["顧客"] {
    string id "顧客ID"
    string name "名前"
    string email "メールアドレス"
    string tel "電話番号"
    string address "住所"
  }

```

| 論理名 | 物理名 | 型 | 説明 | 例 |
| --- | --- | --- | --- | --- |
| ID | id | string | 顧客を一意に特定するためのID | 1 |
| 名前 | name | string | 顧客の名前 | 山田太郎 |
| メールアドレス | email | string | 顧客のメールアドレス |
| 電話番号 | tel | string | 顧客の電話番号 |
| 住所 | address | string | 顧客の住所 |

# 画面を考える

```mermaid
graph LR
  A[顧客一覧] --> B[顧客単一]
  A --> C[顧客登録]
```

| 画面名 | 説明 |
| --- | --- |
| 顧客一覧 | 顧客の一覧を表示する |
| 顧客単一 | 顧客の詳細を表示する |
| 顧客登録 | 顧客を登録する |

# 機能を考える

| 機能名 | 説明 |
| --- | --- |
| 顧客一覧表示 | 顧客の一覧を表示する |
| 顧客詳細表示 | 顧客の詳細を表示する |
| 顧客登録 | 顧客を登録する |
| 顧客編集 | 顧客を編集する |
| 顧客削除 | 顧客を削除する |

# プロジェクトの作成

Vite を使ってプロジェクトを作成しましょう。

```powershell
> npm create vite@latest my-crm -- --template react
```

CRM は Customer Relationship Management の略です。

プロジェクトの名前は好きなものをつけてください。

## 依存パッケージのインストール

画面遷移のライブラリをインストールします。

```powershell
> cd my-crm
> npm install react-router-dom
```

# 各画面の作成

## ディレクトリ構造

`src` の中を次のようにします。

今後各画面に対応するものは `src/routes` に配置します。

```
./src
├── App.jsx
├── main.jsx
└── routes
    ├── CustomerCollection.jsx
    ├── CustomerCreate.jsx
    └── CustomerSingle.jsx
```

## `main.jsx`

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)

```

## `App.jsx`

パスとコンポーネントの対応を設定します。

| パス | コンポーネント | 画面名 |
| --- | --- | --- |
| `/` | `CustomerCollection` | 顧客一覧 |
| `/:id` | `CustomerSingle` | 顧客単一 |
| `/new` | `CustomerCreate` | 顧客登録 |

```jsx
import { BrowserRouter, Route, Routes } from 'react-router-dom'
import CustomerCollection from './routes/CustomerCollection'
import CustomerCreate from './routes/CustomerCreate'
import CustomerSingle from './routes/CustomerSingle'

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<CustomerCollection />} />
        <Route path="/:id" element={<CustomerSingle />} />
        <Route path="/new" element={<CustomerCreate />} />
      </Routes>
    </BrowserRouter>
  )
}

export default App

```

## `src/routes/CustomerCollection.jsx`

```jsx
function CustomerCollection () {
  return (
    <div>
      <h1>顧客一覧</h1>
    </div>
  );
}

export default CustomerCollection;

```

## `src/routes/CustomerSingle.jsx`

```jsx
function CustomerSingle () {
  return (
    <div>
      <h1>顧客単一</h1>
    </div>
  );
}

export default CustomerSingle;

```

## `src/routes/CustomerCreate.jsx`

```jsx
function CustomerCreate () {
  return (
    <div>
      <h1>顧客登録</h1>
    </div>
  );
}

export default CustomerCreate;

```

## 動作確認

`npm run dev` で起動して動作確認をします。

ブラウザに次のURLを入力して各画面が表示されるかを確認します。

| URL | 画面 |
| --- | --- |
| `http://localhost:3000/` | 顧客一覧 |
| `http://localhost:3000/1` | 顧客単一 |
| `http://localhost:3000/new` | 顧客登録 |

# 各画面の遷移

次の通り画面の遷移ができるようにしましょう

```mermaid
graph LR
  A[顧客一覧] --> B[顧客単一]
  A --> C[顧客登録]
```

## `src/routes/CustomerCollection.jsx`

```jsx
import { Link } from 'react-router-dom'

function CustomerCollection () {
  return (
    <div>
      <h1>顧客一覧</h1>
      <p>
        <Link to="/new">新規登録</Link>
      </p>
      <ul>
        <li>
          <Link to="/1">山田太郎</Link>
        </li>
      </ul>
    </div>
  );
}

export default CustomerCollection;

```

## `src/routes/CustomerSingle.jsx`

```jsx
import { Link } from "react-router-dom";

function CustomerSingle () {
  return (
    <div>
      <h1>顧客単一</h1>
      <hr />
      <Link to="/">顧客一覧に戻る</Link>
    </div>
  );
}

export default CustomerSingle;

```

## `src/routes/CustomerCreate.jsx`

```jsx
import { Link } from 'react-router-dom'

function CustomerCreate () {
  return (
    <div>
      <h1>顧客登録</h1>
      <hr />
      <Link to="/">顧客一覧に戻る</Link>
    </div>
  );
}

export default CustomerCreate;

```

# 顧客の登録機能

顧客の登録機能を作りましょう。

```mermaid
erDiagram
  customer["顧客"] {
    string id "顧客ID"
    string name "名前"
    string email "メールアドレス"
    string tel "電話番号"
    string address "住所"
  }

```

データに基づいたフォームを作ります。

## フォームの作成

顧客登録画面にフォームを追加します。

`src/routes/CustomerCreate.jsx`

```jsx
import { Link } from 'react-router-dom'

function CustomerCreate () {
  const handleSubmit = (event) => {
    event.preventDefault()
    const data = new FormData(event.target)
    const customer = {
      id: data.get('id'),
      name: data.get('name'),
      email: data.get('email'),
      tel: data.get('tel'),
      address: data.get('address'),
    }
    console.log(customer)
  }
  return (
    <div>
      <h1>顧客登録</h1>
      <form onSubmit={handleSubmit}>
        <div>
          <label>
            ID:
            <input type="text" name="id" />
          </label>
        </div>
        <div>
          <label>
            名前: <input type="text" name="name" />
          </label>
        </div>
        <div>
          <label>
            メールアドレス: <input type="text" name="email" />
          </label>
        </div>
        <div>
          <label>
            電話番号: <input type="text" name="tel" />
          </label>
        </div>
        <div>
          <label>
            住所:
            <textarea name="address" />
          </label>
        </div>
        <button type="submit">登録</button>
      </form>
      <hr />
      <Link to="/">顧客一覧に戻る</Link>
    </div>
  );
}

export default CustomerCreate;

```

## 顧客の登録

### `src/storage.js`

`src/storage.js` を作ります。

```js
const customers = {}

export function addCustomer (customer) {
  customers[customer.id] = customer
  console.log(customers)
}

```

### `src/routes/CustomerCreate.jsx`

`src/storage.js` を使ってデータを登録します。

```jsx
import { Link } from 'react-router-dom'
import { addCustomer } from '../storage'
```

`addCustomer()`にデータを渡します。

```js
  const handleSubmit = (event) => {
    event.preventDefault()
    const data = new FormData(event.target)
    const customer = {
      id: data.get('id'),
      name: data.get('name'),
      email: data.get('email'),
      tel: data.get('tel'),
      address: data.get('address'),
    }
    addCustomer(customer)
    event.target.reset()
    alert('登録しました')
  }
```

- `event.target.reset()` でフォームをリセットします。
- `alert('登録しました')` で登録完了を知らせます。

# 顧客の一覧

登録したデータを一覧で見る機能を作ります。

### 顧客一覧の取得

`getCustomers` を追加します。

```jsx
const customers = {}

export function addCustomer (customer) {
  customers[customer.id] = customer
}

export function getCustomers () {
  return Object.values(customers)
}

```

### 顧客一覧の表示

`src/routes/CustomerCollection.jsx`

`getCustomers` を使ってデータを取得します。

```jsx
import { Link } from 'react-router-dom'
import { getCustomers } from '../storage';

function CustomerCollection () {
  const customers = getCustomers()
  return (
    <div>
      <h1>顧客一覧</h1>
      <p>
        <Link to="/new">新規登録</Link>
      </p>
      <ul>
        {customers.map((customer) => (
          <li key={customer.id}>
            <Link to={`/${customer.id}`}>
              {customer.name}
            </Link>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default CustomerCollection;

```

この実装にはいくつかの問題がありますが現段階では重要ではないのでこのまま進めます。

登録したデータが一覧で表示されることを確認します。

# 顧客の閲覧

登録したデータを詳細で見る機能を作ります。

## 顧客の取得

`getCustomer` を追加します。

IDを指定したらそのIDのデータを取得します。

```jsx
export function getCustomer (id) {
  return customer[id];
}

```

## 顧客単一の表示

顧客を取得して表示することができます。

```jsx
import { Link, useParams } from "react-router-dom";
import { getCustomer } from "../storage";

function CustomerSingle () {
  const param = useParams();
  const customer = getCustomer(param.id);

  return (
    <div>
      <h1>顧客単一</h1>
      <p>ID: {customer.id}</p>
      <p>名前: {customer.name}</p>
      <p>メールアドレス: {customer.email}</p>
      <p>電話番号: {customer.tel}</p>
      <p>住所: {customer.address}</p>
      <hr />
      <Link to="/">顧客一覧に戻る</Link>
    </div>
  );
}

export default CustomerSingle;

```

## 顧客がないとき

現段階では永続化していないので画面を再読込するとデータが見つからなくなります。

そのためエラーが発生します。顧客がない場合はエラーを表示するようにします。

```jsx
import { Link, useParams } from "react-router-dom";
import { getCustomer } from "../storage";

function CustomerSingle () {
  const param = useParams();
  const customer = getCustomer(param.id);

  return (
    <div>
      <h1>顧客単一</h1>
      {!customer && (
        <div>
          <p>顧客が見つかりません</p>
        </div>
      )}
      {customer && (
        <div>
          <p>ID: {customer.id}</p>
          <p>名前: {customer.name}</p>
          <p>メールアドレス: {customer.email}</p>
          <p>電話番号: {customer.tel}</p>
          <p>住所: {customer.address}</p>
        </div>
      )}
      <hr />
      <Link to="/">顧客一覧に戻る</Link>
    </div>
  );
}

export default CustomerSingle;
```


# 顧客の編集

登録したデータを編集する機能を作ります。

ここでは画面を増やさずにこの画面で編集できるようにします。

## フォームの作成

まずはフォームだけを表示します。
まだ保存処理は書いていません。

ID は変更できないようにします。

```jsx
import { Link, useParams } from "react-router-dom";
import { getCustomer } from "../storage";

function CustomerSingle () {
  const param = useParams();
  const customer = getCustomer(param.id);
  const handleSubmit = (event) => {
    event.preventDefault();
    const data = new FormData(event.target);
    const customer = {
      id: param.id,
      name: data.get("name"),
      email: data.get("email"),
      tel: data.get("tel"),
      address: data.get("address"),
    };
    console.log(customer);
    alert("保存しました");
  }

  return (
    <div>
      <h1>顧客単一</h1>
      {!customer && (
        <div>
          <p>顧客が見つかりません</p>
        </div>
      )}
      {customer && (
        <form onSubmit={handleSubmit}>
          <div>
            <label>
              ID: {customer.id}
            </label>
          </div>
          <div>
            <label>
              名前: <input type="text" name="name" defaultValue={customer.name} />
            </label>
          </div>
          <div>
            <label>
              メールアドレス: <input type="text" name="email" defaultValue={customer.email} />
            </label>
          </div>
          <div>
            <label>
              電話番号: <input type="text" name="tel" defaultValue={customer.tel} />
            </label>
          </div>
          <div>
            <label>
              住所:
              <textarea name="address" defaultValue={customer.address} />
            </label>
          </div>
          <button type="submit">保存</button>
        </form>
      )}
      <hr />
      <Link to="/">顧客一覧に戻る</Link>
    </div>
  );
}

export default CustomerSingle;

```

## 顧客の更新

`updateCustomer` を追加します。

```js
export function updateCustomer (customer) {
  customers[customer.id] = customer
}

```

## フォームの処理から更新処理を呼び出す

`updateCustomer` を呼び出します。

```jsx
import { Link, useParams } from "react-router-dom";
import { getCustomer, updateCustomer } from "../storage";
```

```js
  const handleSubmit = (event) => {
    event.preventDefault();
    const data = new FormData(event.target);
    const customer = {
      id: param.id,
      name: data.get("name"),
      email: data.get("email"),
      tel: data.get("tel"),
      address: data.get("address"),
    };
    updateCustomer(customer);
    alert("保存しました");
  }
```

## 動作確認

一度編集して、保存してください。

一覧に戻って再度単一画面に戻って内容が変更されていることを確認してください。

# 顧客の削除

登録したデータを削除する機能を作ります。

## 顧客の削除

`deleteCustomer` を追加します。

次の２つの処理の結果は同じです。

```js
export function deleteCustomer (id) {
  delete customers[id]
}

```

## 削除ボタンの追加

`src/routes/CustomerSingle.jsx`

```jsx
import { Link, useParams } from "react-router-dom";
import { deleteCustomer, getCustomer, updateCustomer } from "../storage";

function CustomerSingle () {
  const param = useParams();
  const customer = getCustomer(param.id);
  const handleSubmit = (event) => {
    /* 省略 */
  }

  const handleDelete = () => {
    alert("削除しました");
    deleteCustomer(param.id);
  }

  return (
    <div>
      <h1>顧客単一</h1>
      {!customer && (
        <div>
          <p>顧客が見つかりません</p>
        </div>
      )}
      {customer && (
        <form onSubmit={handleSubmit}>
          {/* 省略 */}
          <button type="submit">保存</button>
          <button type="button" onClick={handleDelete}>削除</button>
        </form>
      )}
      <hr />
      <Link to="/">顧客一覧に戻る</Link>
    </div>
  );
}

export default CustomerSingle;

```

## 動作確認


顧客登録後に削除して一覧表示に戻ってください。
一覧から削除されています。

## 削除時に画面遷移

削除後に表示されるのは混乱を招くので一覧画面に戻るようにします。

`useNavigate` をインポートします。

```js
import { Link, useNavigate, useParams } from "react-router-dom";
```

```js
function CustomerSingle () {
  const param = useParams();
  const navigate = useNavigate();
```

削除時に遷移します。

```js
const handleDelete = () => {
  alert("削除しました");
  deleteCustomer(param.id);
  navigate("/");
}
```

# データの永続化

データを永続化するために localStorage に書き込みます。

`src/storage.js`

次の処理で localStorage に書き込みます。

```js
function save () {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(customers))
}
```

次の処理で localStorage から読み込みます。

```js
export function load() {
  const data = localStorage.getItem(STORAGE_KEY)
  if (data) {
    Object.assign(customers, JSON.parse(data))
  }
}
```

## データの書き込み時に保存

`addCustomer`, `updateCustomer`, `deleteCustomer` で保存処理を呼び出します。

```js
export function addCustomer (customer) {
  customers[customer.id] = customer
  save()
}
```

```js
export function updateCustomer (customer) {
  customers[customer.id] = customer
  save()
}
```

```js
export function deleteCustomer (id) {
  delete customers[id]
  save()
}
```

## 画面起動時に読み込み

`src/main.js`

`load` を呼び出します。

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App.jsx'
import { load } from './storage'

load()

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

## 動作確認

ブラウザをリロードしてもデータが残っていることを確認してください。

# まとめ

ここまでで顧客の情報の作成、表示、編集、削除を行いました。

この４つのデータの操作は CRUD と呼ばれて基本的な操作です。今後のこの組み合わせで様々な機能追加を進めていきます。

- Create: 作成
- Read: 読み込み
- Update: 更新
- Delete: 削除

次の章ではここに機能追加をします。

[次の機能追加](./02-team.md)
