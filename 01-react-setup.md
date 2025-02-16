# React プロジェクトの作成

このセクションでは、Vite を使って React プロジェクトを作成し、基本的なコンポーネントの動作を確認します。

---

## 1. Vite を使って React プロジェクトを作成する

Vite は、高速な開発環境を提供するビルドツールです。まずは Vite を使って React プロジェクトを作成します。

### 手順
1. ターミナルを開き、以下のコマンドを実行します。

   ```sh
   npm create vite@latest my-react-app -- --template react
   ```

2. プロジェクトディレクトリに移動し、依存関係をインストールします。

   ```sh
   cd my-react-app
   npm install
   ```

3. 開発サーバーを起動します。

   ```sh
   npm run dev
   ```

### ビルドツールが必要な理由

フロントエンド開発では、開発の効率化 や パフォーマンスの最適化 のためにビルドツールが必要になります。モダンなJavaScriptの開発環境では、ブラウザが直接解釈できない形式のコードを使用することが多く、それを適切に処理するためにビルドツールが役立ちます。

### モジュールシステムの利用

現代のJavaScriptでは、コードをモジュール化して管理することが一般的です。しかし、ブラウザがサポートしているモジュールの仕様（ES Modules）と、開発時に使用するモジュールバンドラーの仕様（CommonJSやECMAScript Modulesなど）には違いがあります。

ビルドツールを使うことで、異なるモジュールシステムを適切に変換し、一つのファイルまたは効率的にロードされる形にまとめることができます。例えば、以下のような import/export を利用したコードを、ブラウザが理解できる形に変換できます。

```js
// ES Modules
import { fetchData } from './utils.js';

fetchData();
```

これをバンドルすると、すべてのモジュールが一つのファイルに統合され、ブラウザで効率的に読み込めるようになります。

#### JSXの利用
Reactなどのライブラリでは、JSX（JavaScript XML）という構文を利用します。しかし、JSXはブラウザが直接解釈できるJavaScriptではないため、そのままでは動作しません。

例えば、以下のようなJSXのコードがあります。

```jsx
const element = <h1>Hello, world!</h1>;
```

これをビルドツールが適切なJavaScriptに変換することで、ブラウザで実行できるようになります。例えば、Babelを使用すると次のようなコードに変換されます。

```js
const element = React.createElement('h1', null, 'Hello, world!');
```

この変換を手作業で行うのは現実的ではないため、ビルドツールを利用することで開発がスムーズになります。

---

## 2. プロジェクトのディレクトリ構成

プロジェクトのディレクトリ構成は以下のようになります。

```
./
├── README.md
├── eslint.config.js
├── index.html
├── package-lock.json
├── package.json
├── public
│   └── vite.svg
├── src
│   ├── App.css
│   ├── App.jsx
│   ├── assets
│   │   └── react.svg
│   ├── index.css
│   └── main.jsx
└── vite.config.js

```

| ファイル | 説明 |
| --- | --- |
| `README.md` | プロジェクトの説明や使い方を記述したファイル |
| `eslint.config.js` | ESLint の設定ファイル |
| `index.html` | メインの HTML ファイル |
| `package-lock.json` | パッケージの依存関係を固定したファイル |
| `package.json` | プロジェクトの設定や依存関係を管理するファイル |
| `public/` | 静的ファイルを配置するディレクトリ |
| `src/` | ソースコードを配置するディレクトリ |
| `src/App.css` | アプリケーションのスタイルを記述するファイル |
| `src/App.jsx` | メインのコンポーネントを記述するファイル |
| `src/assets/` | 画像やフォントなどのリソースを配置するディレクトリ |
| `src/index.css` | メインのスタイルを記述するファイル |
| `src/main.jsx` | アプリケーションのエントリーポイントを記述するファイル |
| `vite.config.js` | Vite の設定ファイル |

- ESLintとは、コードの品質を保つためのツールです。コードのスタイルや構文エラーを検出し、修正することができます。


### React Dev Tools のインストール

[React Developer Tools](https://ja.react.dev/learn/react-developer-tools) は、React アプリケーションのデバッグやパフォーマンスの分析を行うためのツールです。

---

## 3. Hello World を表示する

Vite のセットアップが完了したら、`App.jsx` を編集して `Hello World` を表示します。

### 不要なファイルの削除

まず、不要なファイルを削除します。

- `src/App.css`
- `src/index.css`
- `src/assets/react.svg`
- `public/vite.svg`

次のようなディレクトリ構成になります。

```
./
├── README.md
├── eslint.config.js
├── index.html
├── package-lock.json
├── package.json
├── src
│   ├── App.jsx
│   └── main.jsx
└── vite.config.js
```


### `index.html`

- `<html>` の `lang` 属性を `ja` に設定します。
- `<link rel="icon" type="image/svg+xml" href="/vite.svg" />` を削除します。

```html
<!doctype html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite + React</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

### `src/main.js`

- `import './index.css'` を削除します。

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

### `src/App.jsx`

`Hello World` を表示するように書き換えます。

```jsx
function App() {
  return <h1>Hello World</h1>;
}

export default App;
```

### 動作確認

開発サーバーを起動します。

```sh
npm run dev
```

ブラウザで `http://localhost:5173` にアクセスすると、`Hello World` が表示されます。

これがReactが動作する最小構成の状態です。

---

## 4. useState でカウンターを実装する

次に、`useState` を使って簡単なカウンターを作成します。

この後のコードはすべて `src/App.jsx` に記述します。

### 数字を表示する

次のように `h1` タグ内に数字を表示するようにします。

```jsx
function App() {
  return <h1>1</h1>;
}

export default App;
```

### 変数を表示する

表示していた数字を変数に置き換えます。

```jsx
function App() {
  let count = 1;

  return <h1>{count}</h1>;
}

export default App;
```

### ボタンを表示する

ボタンを追加します。

コンポーネントは常に一つの要素を返す必要があります。

そのため、ボタンを `div` タグで囲むようにします。

```jsx
function App() {
  let count = 1;

  return (
    <div>
      <h1>{count}</h1>
      <button>Click me</button>
    </div>
  );
}
```

#### Fragment を使う

`div` タグで囲む代わりに、`Fragment` を使って複数の要素を返すことができます。

```jsx
import { Fragment } from 'react';

function App() {
  let count = 1;

  return (
    <Fragment>
      <h1>{count}</h1>
      <button>Click me</button>
    </Fragment>
  );
}
```

または次のようにも記述できます。

```jsx
function App() {
  let count = 1;

  return (
    <>
      <h1>{count}</h1>
      <button>Click me</button>
    </>
  );
}
```

それぞれの動きを開発者ツールで確認してみましょう。

### ボタンのイベントハンドラーを設定する

ボタンをクリックしたときの動作する処理を設定します。

この処理のことをイベントハンドラー(Event Handler)と呼びます。

ここではイベントはクリックのことを指します。他にも様々なイベントがあります。

何かが起きた(=イベント)ら何かをする(=ハンドル)という考え方です。

ここでは `window.alert` を使ってアラートを表示するようにします。

```jsx
function App() {
  let count = 1;

  function handler () {
    window.alert("クリックされました")
  }

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={handler}>Click me</button>
    </div>
  );
}

export default App;
```

ボタンを押すとアラートが表示されることを確認してみましょう。


#### イベント

イベントには次のようなものがあります。

- `onClick` イベントは、要素がクリックされたときに発生します。
- `onChange` イベントは、入力要素の値が変更されたときに発生します。
- `onSubmit` イベントは、フォームが送信されたときに発生します。

#### 関数の定義

今回の実装では次の２つは同じ意味を持ちます。

```js
function handler () {
  window.alert("クリックされました")
}
```

```js
const handler = () => {
  window.alert("クリックされました")
}
```

ただし、JavaScriptの厳密な意味では違いがあります。

React ではその違いを活用する場面はほとんどありません。

### 状態を定義する

クリックされたときの処理を書けるようになったので表示していた数字を変更してみましょう。

```jsx
function App() {
  let count = 1;

  function handler () {
    count = count + 1;
    window.alert("countが" + count + "になりました")
  }

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={handler}>Click me</button>
    </div>
  );
}

export default App;

```

アラートには更新された値が表示されますが、画面には反映されません。

これは React が変更を検知できないためです。

次のように書くことで、React が変更を検知し、画面を更新します。

```jsx
import { useState } from 'react';

function App() {
  const [count, setCount] = useState(1);

  function handler () {
    setCount(count + 1);
    window.alert("countが" + count + "になりました")
  }

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={handler}>Click me</button>
    </div>
  );
}

```

#### `const` と `let` の違い

`let` は再代入が可能な変数を宣言します。

```js
let count = 1;
count = count + 1; // OK
```

`const` は再代入ができない変数を宣言します。

```js
const count = 1;
count = count + 1; // Error
```

再代入可能な場合、その変数に入っている値が他のものになる可能性があるので、各場合にたいして処理を書く必要があります。

可能な限り `const` を使い、再代入が必要な場合にのみ `let` を使うようにしましょう。


## 5. カウンターの仕上げ

最後に理解を確認するために、カウンターを完成させます。

次のような動きにするカウンターを作ってみましょう。

### 基本的な動作

- カウンターの初期値は `0` からスタート
- `＋` ボタンをクリックするとカウンターが `1` ずつ増加
- `ー` ボタンをクリックするとカウンターが `1` ずつ減少

### 応用的な動作

- カウンターの値が 0 未満にならないようにする
- カウンターの値が 10 を超えないようにする
- カウンターの値が 0 以下の場合は `ー` ボタンを無効にする
- カウンターの値が 10 以上の場合は `＋` ボタンを無効にする

ボタンの無効については[HTML 属性: disabled](https://developer.mozilla.org/ja/docs/Web/HTML/Attributes/disabled) を参照

### 基本的な動作の実装例

```jsx
import { useState } from 'react';

function App () {
  const [count, setCount] = useState(0);

  const plusHandler = () => {
    setCount(count + 1);
  }

  const minusHandler = () => {
    setCount(count - 1);
  }

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={plusHandler}>＋</button>
      <button onClick={minusHandler}>ー</button>
    </div>
  );
}

export default App;
```

### 応用的な動作の実装例

```jsx
import { useState } from 'react';

const MAX = 10;
const MIN = 0;

function App () {
  const [count, setCount] = useState(0);

  const plusHandler = () => {
    if (count >= MAX) return;
    setCount(count + 1);
  }

  const minusHandler = () => {
    if (count <= MIN) return;
    setCount(count - 1);
  }

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={plusHandler} disabled={count >= MAX}>＋</button>
      <button onClick={minusHandler} disabled={count <= MIN}>ー</button>
    </div>
  );
}

export default App;
```

---

## まとめ

このセクションでは、以下の内容を学びました：
- Vite を使って React プロジェクトを作成する
- `Hello World` を表示する
- `useState` を使ってカウンターを実装する
- 開発サーバーを起動して動作を確認する

次のステップでは、Express を使って API を作成し、フロントエンドと連携させます。
