# 基本的なWebAPI

それでは[準備したプロジェクト](./01-setup-project.md)を使ってWebAPIの実装をしていきます。

## 動作確認

次のコマンドを実行することでプロジェクトを起動します。

```powershell
> npm run dev
```

ブラウザで `http://localhost:3000` にアクセスします。

この状態でファイルを変更すると自動で再起動します。

# Hello Hono の確認

`./src/index.ts` の中身は次のとおりになっています。

```ts
import { serve } from '@hono/node-server'
import { Hono } from 'hono'

const app = new Hono()

app.get('/', (c) => {
  return c.text('Hello Hono!')
})

serve({
  fetch: app.fetch,
  port: 3000
}, (info) => {
  console.log(`Server is running on http://localhost:${info.port}`)
})

```

## Hello Hono! を変えてみる

次のコードを

```ts
app.get('/', (c) => {
  return c.text('Hello Hono!')
})
```

次のように変更しましょう。

```ts
app.get('/', (c) => {
  return c.text('こんにちは Hono!')
})
```

## 動作確認

ブラウザで `http://localhost:3000` にアクセスします。

`こんにちは Hono!` と表示されていれば成功です。

表示されない場合はサーバーが起動しているかを確認しましょう。

# ユーザーの表示

次のコードを追加してください。

```ts
app.get('/user', (c) => {
  return c.json({
    name: '田中太郎',
    age: 18
  })
})
```

## コード全体

コード全体は次のようになっています。

コードの階層構造が崩れていないことに注意してください。

```ts
import { serve } from '@hono/node-server'
import { Hono } from 'hono'

const app = new Hono()

app.get('/', (c) => {
  return c.text('こんいちは Hono!')
})

app.get('/user', (c) => {
  return c.json({
    name: '田中太郎',
    age: 18
  })
})

serve({
  fetch: app.fetch,
  port: 3000
}, (info) => {
  console.log(`Server is running on http://localhost:${info.port}`)
})

```

## 動作確認

ブラウザで `http://localhost:3000/user` にアクセスします。

`{"name":"田中太郎","age":18}` と表示されていれば成功です。

## 課題

- `name` と `age` を自分の名前と年齢に変更してください。

# JSON

JSONとはJavaScript Object Notationの略で、データを表現するためのフォーマットです。

プログラムがデータ構造を扱うためのフォーマットとして広く使われています。JSON自体は文字列ですがそれらに対して規約を設けることで構造的なデータを表現することができます。

次のような構造を文字列で表すことができます。

```json
{
  "name": "田中太郎",
  "age": 18,
  "isStudent": true,
  "hobby": [
    "サッカー",
    "バスケットボール"
  ]
}
```

次のように1行の文字列にしても同じ意味になります。

```json
{"name": "田中太郎","age": 18,"isStudent": true,"hobby": ["サッカー","バスケットボール"]}
```

JSON はWebAPI以外でもAWSの設定などにも使われます。

# パスパラメータ

IDを指定してユーザーを取得するようなAPIを作成してみましょう。

## 仕様

- `/user/1` にアクセスすると田中太郎の情報を取得できる
- `/user/2` にアクセスすると山田花子の情報を取得できる
- `/user/3` にアクセスすると佐藤次郎の情報を取得できる

## データ

### 田中太郎

名前は `田中太郎` で年齢は `18` です。
住所は `東京都` です。
趣味は `サッカー` と `バスケットボール` です。

### 山田花子

名前は `山田花子` で年齢は `20` です。
住所は `大阪府` です。
趣味は `バレーボール` と `テニス` です。

### 佐藤次郎

名前は `佐藤次郎` で年齢は `22` です。
住所は `北海道` です。
趣味は `野球` と `サッカー` です。

## コード

```ts
app.get('/user/:id', (c) => {
  const id = c.req.param('id')

  if (id == '1') {
    return c.json({
      name: '田中太郎', age: 18,
      address: '東京都',
      hobbies: ['サッカー', 'バスケットボール']
    })
  }

  if (id == '2') {
    return c.json({
      name: '山田花子', age: 20,
      address: '大阪府',
      hobbies: ['バレーボール', 'テニス']
    })
  }

  if (id == '3') {
    return c.json({
      name: '佐藤次郎', age: 22,
      address: '北海道',
      hobbies: ['野球', 'サッカー']
    })
  }

  return c.json({
      message: 'User not found'
  }, 404)
})
```

## 説明

`/user/:id` とすることでパスから `id` を取得することができます。

プログラムの中では次のようにして取り出しています。

```ts
const id = c.req.param('id')
```

if 文を使って `id` の値によって返すデータを変更しています。

## 動作確認

ブラウザで次のURLにアクセスしてみましょう。
- `http://localhost:3000/user/1`
- `http://localhost:3000/user/2`
- `http://localhost:3000/user/3`

それぞれに対応したJSONが表示されていれば成功です。

# ユーザーの一覧

次のようなAPIを作成してみましょう。

## 仕様

- `/users` にアクセスするとユーザーの一覧を取得できる

## コード

```ts
const users = [
  {
    id: 1,
    name: '田中太郎',
    age: 18,
    address: '東京都',
    hobbies: ['サッカー', 'バスケットボール']
  },
  {
    id: 2,
    name: '山田花子',
    age: 20,
    address: '大阪府',
    hobbies: ['バレーボール', 'テニス']
  },
  {
    id: 3,
    name: '佐藤次郎',
    age: 22,
    address: '北海道',
    hobbies: ['野球', 'サッカー']
  }
]

app.get('/users', (c) => {
  return c.json(users)
})
```

# まとめ

この節では、Honoを使ったWebAPIの実装を行いました。

次はデータベースの基本的な操作を確認していきます。

- [データベースの基本](./04-basic-database.md)
