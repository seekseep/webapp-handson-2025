# SQLite　のインストール

SQLiteは、軽量なデータベース管理システムです。

この節では、SQLiteのインストール方法について説明します。

# なぜSQLiteを使うのか

SQLiteは、以下のような特徴があります。

- サーバーを必要としない
- 軽量
- シンプルなSQL

今回の学習においては実行速度やスケーラビリティは重要ではありません。
そのため簡単に構築できるSQLiteを利用します。

# Windows

SQLiteのインストール方法には次の２つの方法があります。

- 公式が提供するインストーラーを利用する
- パッケージマネージャーを使ってインストールする

## 公式が提供するインストーラーを利用する

[ダウンロードページ](https://www.sqlite.org/download.html)から自分のOSに合ったインストーラーをダウンロードしてインストールします。

## パッケージマネージャーを使ってインストールする

Scoopを使ってインストールすることができます。

Scoopを使う場合の手順は次のとおりです。

Scoopのインストール

```bash
> iwr -useb get.scoop.sh | iex
```

SQLiteのインストール

```bash
> scoop install sqlite
```

SQLiteのバージョンを確認

```bash
> sqlite3 --version
```

バージョンが表示されていたらインストール完了です。

# macOS

SQLite のインストール方法には次の２つの方法があります。

- 公式が提供するインストーラーを利用する
- Homebrewを使ってインストールする

## 公式が提供するインストーラーを利用する

[ダウンロードページ](https://www.sqlite.org/download.html)から自分のOSに合ったインストーラーをダウンロードしてインストールします。

## Homebrewを使ってインストールする

Homebrewを使ってインストールすることができます。

Homebrewを使う場合の手順は次のとおりです。

Homebrewのインストール

```bash
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

SQLiteのインストール

```bash
$ brew install sqlite
```

SQLiteのバージョンを確認

```bash
$ sqlite3 --version
```

バージョンが表示されていたらインストール完了です。

# まとめ

この節では、SQLiteのインストール方法について説明しました。

これで必要な環境構築は終了です。

次からReactの基礎を学んでいきましょう。

- [Reactの基礎](../01-react-basic/README.md)
