# クライアントから API を呼び出す

このセクションでは、クライアント (React) から API (Express) を呼び出し、画面にデータを表示する方法を学びます。
最初はシンプルな API 呼び出しを実装し、順番に機能を拡張していきます。

---

## 1. クライアント（React）から API を呼び出す準備

クライアントからAPIを呼び出すためにここまでで作成したプロジェクトを使用します。

1. [Reactのセットアップ](./01-react-setup.md) で作成したReactプロジェクトを起動します。
   ```sh
   cd my-react-app
   npm run dev
   ```
2. ブラウザで `http://localhost:5173` を開いて、Reactアプリケーションが表示されることを確認します。
2. [Express プロジェクトの作成](./02-express-setup.md) で作成したAPIサーバーを起動します。
   ```sh
   cd my-express-api
   npm run dev
   ```
3. API が `http://localhost:3000` で動作していることを確認する。

---

## 2. fetch を使って API を呼び出す (データの取得)

まずは、ボタンを押したときに API からメッセージを取得する実装を行います。

### メッセージを取得するコンポーネントの作成

ボタンを押してサーバーから取得したメッセージを表示するコンポーネントを作成します。

`src/App.jsx`

```jsx
import { useState } from "react";

function App() {
  const [message, setMessage] = useState(null);

  const handleFetch = () => {
    const message = "サーバーから取得したメッセージ";
    setMessage(message);
  };

  return (
    <div>
      <h1>API からのメッセージ:</h1>
      <p>{message || "データなし"}</p>
      <button onClick={handleFetch}>メッセージを取得</button>
    </div>
  );
}

export default App;
```

### CORS

API サーバーとクライアントサーバーが異なる場合、CORS (Cross-Origin Resource Sharing) の設定が必要です。

CORS とは、異なるオリジン (ドメイン・ポート・プロトコル) からのリクエストを許可する仕組みです。

- C: Cross
- O: Origin
- R: Resource
- S: Sharing

例えば `http://localhost:3000` から `http://localhost:5173` へのリクエストは異なるオリジンです。
そのため、API サーバーで CORS を設定する必要があります。

APIのプロジェクトに `cors` パッケージをインストールします。

### CORS の設定

```sh
npm install cors
```

`server.js` に `cors` を追加します。

```js
import express from "express";
import cors from "cors";

const app = express();
const PORT = 3000;

app.use(cors({
  origin: "*"
}));

app.get("/", (req, res) => {
  res.json({ message: "Hello World" });
});

app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});

```

### メッセージを取得する処理の作成

`handleFetch` 関数内で `fetch` を使って API を呼び出し、メッセージを取得する処理を追加します。

```jsx
import { useState } from "react";

function App() {
  const [message, setMessage] = useState(null);

  const handleFetch = async () => {
    // NOTE: fetch を使って API を呼び出す
    const response = await fetch("http://localhost:3000")
    // NOTE: JSON データを取得
    const json = response.json();
    // NOTE: メッセージを取得
    const message = json.message;

    setMessage(message);
  };

  return (
    <div>
      <h1>API からのメッセージ:</h1>
      <p>{message || "データなし"}</p>
      <button onClick={handleFetch}>メッセージを取得</button>
    </div>
  );
}

export default App;

```

---

## 3. エラーハンドリングを追加する

次に、API のエラーを処理し、エラーが発生したときにメッセージを表示するようにします。

### `server.js` (Express 側)

ステータスコードが500のエラーを返すエンドポイントを追加します。

```js
import express from "express";
import cors from "cors";

const app = express();
const PORT = 3000;

app.use(cors({
  origin: "*"
}));

app.get("/", (req, res) => {
  res.json({ message: "Hello World" });
});

// エラーを返すエンドポイント
app.get("/error", (req, res) => {
  res.status(500).json({ error: "サーバーエラー発生" });
});

app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});

```

#### ステータスコード

ステータスコードはレスポンスの状態を示すコードです。

200 は OK で、リクエストが成功したことを示します。
500 は Internal Server Error で、サーバー側でエラーが発生したことを示します。

その他にもよく使うステータスコードには次のようなものがあります。

| ステータスコード | 意味 | 説明 |
| --- | --- | --- |
| 200 | OK | リクエストが成功 |
| 201 | Created | リソースが作成された |
| 400 | Bad Request | リクエストが不正 |
| 401 | Unauthorized | 認証が必要 |
| 403 | Forbidden | アクセスが拒否された |
| 404 | Not Found | リソースが見つからない |
| 500 | Internal Server Error | サーバー側でエラーが発生 |

### `src/App.jsx`（更新）

リクエストに失敗するとfetch関数の内部では例外が発生します。

その例外をキャッチしてエラーメッセージを表示するようにします。

```jsx
import { useState } from "react";

function App() {
  const [message, setMessage] = useState(null);
  const [error, setError] = useState(null);

  const handleFetch = async (url) => {
    setError(null);
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`エラー: ${response.status}`);
      }
      const data = await response.json();
      setMessage(data.message);
    } catch (err) {
      setError(err.message);
    }
  };

  return (
    <div>
      <h1>API からのメッセージ:</h1>
      {error ? <p style={{ color: "red" }}>{error}</p> : <p>{message || "データなし"}</p>}
      <button onClick={() => handleFetch("http://localhost:3000")}>正常なメッセージを取得</button>
      <button onClick={() => handleFetch("http://localhost:3000/error")}>エラーを取得</button>
    </div>
  );
}

export default App;
```

---

## 4. ローディング状態を追加する

API 呼び出し中に「読み込み中...」と表示するようにします。

### `src/App.jsx`（更新）
```jsx
import { useState } from "react";

function App() {
  const [message, setMessage] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(false);

  const handleFetch = async (url) => {
    setError(null);
    setLoading(true);
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`エラー: ${response.status}`);
      }
      const data = await response.json();
      setMessage(data.message);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div>
      <h1>API からのメッセージ:</h1>
      {loading ? (
        <p>読み込み中...</p>
      ) : error ? (
        <p style={{ color: "red" }}>{error}</p>
      ) : (
        <p>{message || "データなし"}</p>
      )}
      <button onClick={() => handleFetch("http://localhost:3000")}>正常なメッセージを取得</button>
      <button onClick={() => handleFetch("http://localhost:3000/error")}>エラーを取得</button>
    </div>
  );
}

export default App;
```

---

## 5. 関数を整理する（リファクタリング）

最後に、非同期処理を専用の関数として分けてコードを整理します。

### `src/api.js`
```js
export async function fetchMessage(url) {
  const response = await fetch(url);
  if (!response.ok) {
    throw new Error(`エラー: ${response.status}`);
  }
  const data = await response.json();
  return data.message;
}
```

### `src/App.jsx`（リファクタリング）
```jsx
import { useState } from "react";
import { fetchMessage } from "./api";

function App() {
  const [message, setMessage] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(false);

  const handleFetch = async (url) => {
    setError(null);
    setLoading(true);
    try {
      const data = await fetchMessage(url);
      setMessage(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div>
      <h1>API からのメッセージ:</h1>
      {loading ? (
        <p>読み込み中...</p>
      ) : error ? (
        <p style={{ color: "red" }}>{error}</p>
      ) : (
        <p>{message || "データなし"}</p>
      )}
      <button onClick={() => handleFetch("http://localhost:3000")}>正常なメッセージを取得</button>
      <button onClick={() => handleFetch("http://localhost:3000/error")}>エラーを取得</button>
    </div>
  );
}

export default App;
```

---

## まとめ

このセクションでは、順番に機能を追加しながら、API の非同期データ取得を実装しました。
次のステップでは、データベース (SQLite) をセットアップし、API でデータを取得する準備を行います。
