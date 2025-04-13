# Hono のプロジェクトのセットアップ

この節では Hono を使ったWebAPIを作成するためのプロジェクトの準備を行います。

ローカル環境で WebAPI を起動して結果が表示されるところまでを確認します。

# 事前準備

この節では、以下の事前ができていることを確認します。

- エディタ
- Node.js, npm

## エディタ

コードを編集するためのエディタが用意できていることを確認してください。

## Node.js

Node.js と npm がインストールされていることを確認してください。

次のコマンドを実行してバージョンが表示されることを確認してください。

バージョンその時点での最新にすることを推奨します。

```powershell
> node -v
v18.16.0
> npm -v
9.5.0
```

- [Node.js](https://nodejs.org/ja/)

# Hono のプロジェクトのセットアップ

プロジェクトの作成を行います。

```powershell
> npm create hono@latest
Need to install the following packages:
create-hono@0.17.0
Ok to proceed? (y) y
create-hono version 0.17.0
✔ Target directory my-app
✔ Which template do you want to use? nodejs
✔ Do you want to install project dependencies? Yes
✔ Which package manager do you want to use? npm
✔ Cloning the template
✔ Installing project dependencies
🎉 Copied project files
Get started with: cd my-app

```

## `Target directory`

プロジェクトを作成するディレクトリ名を指定します。

## `Which template do you want to use?`

作成するプロジェクトのテンプレートを選択します。
`nodejs` を選択します。

## `Do you want to install project dependencies?`

プロジェクトで利用するパッケージをインストールするかどうかを選択します。
`Yes` を選択します。

## `Which package manager do you want to use?`
プロジェクトで利用するパッケージマネージャーを選択します。
`npm` を選択します。

# プロジェクトの確認

作られたディレクトリの中は次のようになっています。

```
.
├── node_modules
├── package-lock.json
├── package.json
├── README.md
├── src
│   └── index.ts
└── tsconfig.json
```

- `node_modules` : プロジェクトで利用するパッケージがインストールされます。
- `package-lock.json` : プロジェクトで利用するパッケージのバージョンが記録されます。
- `package.json` : プロジェクトの情報が記載されています。
- `README.md` : プロジェクトの説明が記載されています。
- `src` : プロジェクトのソースコードが格納されます。
- `src/index.ts` : プロジェクトのエントリーポイントです
- `tsconfig.json` : TypeScript の設定ファイルです。

# プロジェクトの起動

プロジェクトを起動してみます。

```powershell
> cd my-app
> npm run dev
```

## 動作確認

ブラウザで `http://localhost:3000` にアクセスします。
`Hello Hono!` と表示されていれば成功です。

# まとめ

この節では、Hono を使った WebAPI のプロジェクトを作成しました。

次の節では　WebAPIについて確認します。

- [WebAPI について](./02-webapi.md)
