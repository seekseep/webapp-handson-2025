# Node.js のインストール

Node.js は JavaScript の実行環境です。

Node.js の登場前は、JavaScript はブラウザ上でしか動作しませんでした。
Node.js が登場することで、サーバーサイドの開発も JavaScript で行うことができるようになりました。
また、その他にもフロントエンドで利用することでより高度な開発を行うこともできるようになりました。

# CLI

Node.js を実行するためにはCLI（Command Line Interface）を使います。

## Windows

Windows では標準で Command Promp や PowerShell が利用できます。

公式のドキュメントに従ってインストールを行ってください。

[Windows への PowerShell のインストール](https://learn.microsoft.com/ja-jp/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.5)

すでにインストールされている場合は、プログラムの一覧に表示されます。

## macOS

macOS では標準で Terminal が利用できます。

Terminalは標準でインストールされているので、そのまま利用できます。

アプリケーションの一覧から `Terminal` を選択して起動します。

# Node.js のインストールの確認

Node.js がインストールされているか確認するために、CLI で次のコマンドを実行してください。

```bash
> node -v
```

バージョンが表示されればインストールされています。

```bash
> npm -v
v20.11.1
```

最新のバージョンは公式サイトを参照してください。

[Node.js](https://nodejs.org/ja/)

# Node.js のインストール方法

Node.jsのインストールには次の２つの方法があります。

- 公式が提供するインストーラーを利用する
- Node.jsのバージョン管理ツールを利用する

## 公式が提供するインストーラーを利用する

[ダウンロードページ](https://nodejs.org/ja/download)から自分のOSに合ったインストーラーをダウンロードしてインストールします。

## Node.jsのバージョン管理ツールを利用する

バージョン管理ツールを使うことで簡単に利用するNode.jsのバージョンを切り替えることができます。

これはプロジェクトによって利用するNode.jsのバージョンが異なる場合に便利です。

### Windows

Windows では `nvm-windows` というツールが利用できます。

また、 scoop というパッケージマネージャーを使ってインストールすることもできます。

- [scoop](https://scoop.sh/)
- [coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows)

#### インストールの実行例

環境によってことなりますが次のような手順でインストールすることができます

Scoopのインストール

```powershell
> iwr -useb get.scoop.sh | iex
```

Scoopを使ったnvmのインストール

```powershell
> scoop install nvm
```

nvm を使った Node.js のインストール

```powershell
> nvm install 23.8.0
```

利用するバージョンの指定

```powershell
> nvm use 23.8.0
```

インストールの確認
```
> node -v
v23.8.0
```


## macOS

macOS では `nvm` というツールが利用できます。

Homebrew というパッケージマネージャーを使ってインストールすることもできます。

- [Homebrew](https://brew.sh/)
- [nvm](https://github.com/nvm-sh/nvm)

### インストールの実行例

Homebrewのインストール

```bash
> /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Homebrewを使ったnvmのインストール

```bash
> brew install nvm
```

nvm を使った Node.js のインストール

```bash
> nvm install 23.8.0
```

利用するバージョンの指定

```bash
> nvm use 23.8.0
```

インストールの確認
```
> node -v
v23.8.0
```

# まとめ

JavaScript の実行環境である Node.js のインストール方法について学びました。

次の節でデータベースのインストールについて学びます。

- [SQLiteのインストール](./03-install-sqlite.md)
