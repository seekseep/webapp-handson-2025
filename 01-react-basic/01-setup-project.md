# Reactのプロジェクトの作成

この節では、React のプロジェクトの作り方について説明します。

# Viteを使ったプロジェクトの作成

Vite というツールを使って React のプロジェクトを作成します。

コマンドを実行した階層にプロジェクトができます。

```powershell
> npm create vite@latest my-react-app -- --template react

Scaffolding project in /path/to/my-react-app...

Done. Now run:

  cd my-react-app
  npm install
  npm run dev
```

## プロジェクトの中身

次のようなプロジェクトが作成されます。

```
./
├── README.md
├── eslint.config.js
├── index.html
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

# 作られたプロジェクトの起動

まずは作成されたプロジェクトを起動してみましょう。

## 依存パッケージのインストール

React を使ったプロジェクトでは自分が書いたコード以外にいくつかのパッケージを使います。

まずは利用するパッケージをインストールします。

作成したプロジェクトに移動して `npm install` を実行します。

```powershell
> cd my-react-app
> npm install
```

利用しているパッケージは言い換えればないと動かない必要なパッケージです。これらのパッケージのことを依存パッケージといいます。
場合によってはこれらのことをライブラリと呼ぶこともあります。様々な呼び方がありますが、混乱しないように気をつけてください。


## 起動

次のコマンドで開発用のサーバーが起動します。

```powershell
> npm run dev

  VITE v6.1.0  ready in 209 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

起動しない場合は依存パッケージがインストールされているか確認してください。

## 動作確認

ブラウザで `http://localhost:5173/` にアクセスしてみましょう。

ページが表示されれば成功です。

# 不要なファイルの削除

最低元の構成にしていきます。

## 不要なファイルの削除

次のファイルを削除します。

- `public/vite.svg`
- `src/assets/react.svg`
- `src/App.css`
- `src/index.css`

削除すると次のような構造になります。

```
./
├── README.md
├── eslint.config.js
├── index.html
├── package-lock.json
├── package.json
├── public
├── src
│   ├── App.jsx
│   ├── assets
│   └── main.jsx
└── vite.config.js
```

## ファイルの削除に関する修正

削除したファイルを参照している箇所を修正します。

各ファイルを次のように修正します。

### `src/App.jsx`

```jsx
function App () {
  return <h1>Hello World</h1>
}

export default App

```

### `src/main.jsx`

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

### `index.html`

削除した箇所以外に変更する場所は以下のとおりです。

- `<html lang="ja">`

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

## 動作確認

開発用のサーバーを起動して動作確認をします。

Hello World と表示されていれば成功です。

# まとめ

この節では、React のプロジェクトの作成方法について説明しました。
空のプロジェクトができたのでここから基礎を学んでいきましょう。

次の節では状態管理を学びます。

[状態管理](./02-state-management.md)
