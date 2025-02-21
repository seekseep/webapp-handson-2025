# フォームの作成

アプリケーションではユーザーが入力した内容を反映する機能が必要なことが多いです。

ログインやプロフィールの変更など普段からもさまざまな場面でフォームを使うことがあります。

ここでは　React を使ってフォームを作成する方法を学びます。

# フォームの作成

App コンポーネントにフォームを追加します。

`src/App.jsx` を次のように編集します。

```jsx
function App () {
  return (
    <form>
      <label>
        名前
        <input type="text" />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}

```

ボタンを押すとページがリロードされます。

これはHTMLのデフォルトの挙動です。従来のWebアプリケーションではフォームの内容をサーバーに送信するためにリクエストを送っていました。

次のように書くことで submit.php に対してフォームの内容を送信することができます。

```
<form action="/submit.php" method="post">
```

# ページの再読み込みををとめる

次のように書いてみましょう。

```jsx
function App () {
  function handleSubmit (e) {
    e.preventDefault()
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前
        <input type="text" />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}
```

先程のおさらいになりますが、form 要素に onSubmit イベントを追加しています。

フォームが送信されると handleSubmit 関数が呼ばれます。

`e.preventDefault()` はデフォルトの挙動をキャンセルするメソッドです。そのため、ページがリロードされることがなくなります。

## submitイベントの発火の条件

ボタンをクリックするとsubmitイベントが発火します。

それ以外に submitイベントを発火させる方法があります。
どのようにしたら submit イベントを発火させることができるでしょうか？

# フォームの内容を表示する

送信された内容を画面に表示するようにしましょう。

フォームを使って次のような画面を作ってみましょう。

1. ユーザーは名前を入力する
2. 送信ボタンを押す
3. 画面に「こんにちは、〇〇さん」と表示される

`src/App.jsx` を次のように編集します。

```jsx
function App () {
  function handleSubmit (e) {
    e.preventDefault()
    const data = new FormData(e.target)
    const name = data.get('name')
    alert("こんにちは、" + name + "さん")
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前
        <input type="text" name="name" />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}

```

## FormData オブジェクト

`FormData` オブジェクトはフォームのデータを扱うためのオブジェクトです。

`new FormData(e.target)` でフォームのデータを取得することができます。ここで `e.target` はイベントが発生した要素を指します。つまり、`<form>` 要素です。

`data.get('name')` でフォームの name 属性が `name` の値を取得することができます。

そのため、`name` という名前の input 要素に入力された値を取得することができます。

## 年齢を追加する

フォームが送信された後に `田中太郎さん(23)` のように年齢が表示されるように実装してみましょう。

### 実装例

```jsx
function App () {
  function handleSubmit (e) {
    e.preventDefault()
    const data = new FormData(e.target)
    const name = data.get('name')
    const age = data.get('age')
    alert(name + "さん(" + age + ")")
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前
        <input type="text" name="name" />
      </label>
      <label>
        年齢
        <input type="number" name="age" />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}

```

`<input type="number" name="age" />` で数値を入力するためのフォームを追加しました。

# 不正な動き

このままでは名前と年齢を入力しなくても送信ボタンを押すことができてしまいます。

実際に試してみてください。

## required 属性

`<input>` 要素に `required` 属性を追加することで入力必須にすることができます。

`required` 属性はフォームを `submit` する際に入力が必須であることを示します。

```jsx
function App () {
  function handleSubmit (e) {
    e.preventDefault()
    const data = new FormData(e.target)
    const name = data.get('name')
    const age = data.get('age')

    alert(name + "さん(" + age + ")")
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前
        <input type="text" name="name" required />
      </label>
      <label>
        年齢
        <input type="number" name="age" required />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}

```

この方法でも不正な動きを防ぐことができます。

# 入力中に不正な値であることを表示する

## 入力中に表示を変える

まずは入力中に表示を変える方法を学びましょう。

入力しながらパスワードに記号が含まれていないと表示されるようなフォームを見たことがあると思います。この場合は、入力中にパスワードの強度を表示しています。

そのように入力内容に応じて表示を変える方法を考えてみましょう。

まずは入力されている内容をそのまま画面に表示してみましょう。

```jsx
function App () {
  const [values, setValues] = React.useState({
    name: '',
    age: ''
  })

  function handleChange (e) {
    const data = new FormData(e.target.form)
    const name = data.get('name')
    const age = data.get('age')
    setValues({
      name: name,
      age: age
    })
  }

  function handleSubmit (e) {
    const data = new FormData(e.target)
    const name = data.get('name')
    const age = data.get('age')
    alert(`${data.name}さん(${data.age})`)
  }

  return (
    <form
      onChange={handleChange}
      onSubmit={handleSubmit}>
      <label>
        名前
        <input
          type="text" name="name" />
      </label>
      <label>
        年齢
        <input
          type="number" name="age" />
      </label>
      <div>
        <p>入力中の内容</p>
        <p>名前: {values.name}</p>
        <p>年齢: {values.age}</p>
      </div>
      <button type="submit">送信</button>
    </form>
  )
}

```

入力欄に値を入力してから、フォーカスを外してください。

入力中の内容が表示されることが確認できると思います。


## 問題がある入力欄の表示を変える

入力が正しくない場合にその旨を表示するようにしましょう。

```jsx
function App () {
  const [errors, setErrors] = React.useState({
    name: '',
    age: ''
  })
  const [values, setValues] = React.useState({
    name: '',
    age: ''
  })

  function handleChange (e) {
    const data = new FormData(e.target.form)
    const name = data.get('name')
    const age = data.get('age')

    const values = {
      name: name,
      age: age
    }

    const errors = {
      name: '',
      age: ''
    }

    if (values.name === '') {
      errors.name = '名前を入力してください'
    }

    if (values.age === '') {
      errors.age = '年齢を入力してください'
    }

    setValues(values)
    setErrors(errors)
  }

  let nameError = null
  if (errors.name) {
    nameError = <p>{errors.name}</p>
  }

  let ageError = null
  if (errors.age) {
    ageError = <p>{errors.age}</p>
  }

  return (
    <form onChange={handleChange}>
      <label>
        名前
        <input type="text" name="name" />
      </label>
      {nameError}
      <label>
        年齢
        <input type="number" name="age" />
      </label>
      {ageError}
      <div>
        <p>入力中の内容</p>
        <p>名前: {name}</p>
        <p>年齢: {age}</p>
        <p>{error}</p>
      </div>
      <button type="submit">送信</button>
    </form>
  )
}

```

一度値を入力してからフォーカスを外してください。
その時はエラーが表示されません。

次の入力した値を削除してからフォーカスを外してください。
その時はエラーが表示されることが確認できると思います。

## エラーの表示方法

前のエラーの表示方法では次のように表示していました。

```jsx
let nameError = null
if (errors.name) {
  nameError = <p>{errors.name}</p>
}

return (
  {nameError}
)
```

こうすることでエラーがあるときだけ表示できます。

これは次のようにも書き換えることができます。

```jsx
const nameError = errors.name ? <p>{errors.name}</p> : false

return (
  {nameError}
)
```

React では `false` や `null` などを返すとその要素は表示されません。

更に次のようにも書き換えることができます。

```jsx

return (
  {errors.name && <p>{errors.name}</p>}
)
```

ここでみた３つの書き方は同じ意味です。

Reactを使った開発ではHTMLをどのように表示するのかに対して関心が強いです。

HTMLの構造が視覚的にわかりやすいように書くことができるというメリットがあります。好みの問題ではありますが、一般的には次のように書くことが多いです。

```jsx
function App () {
  const [errors, setErrors] = React.useState({
    name: '',
    age: ''
  })

  function handleChange (e) {
    const data = new FormData(e.target.form)
    const name = data.get('name')
    const age = data.get('age')

    const errors = {
      name: '',
      age: ''
    }

    if (name === '') {
      errors.name = '名前を入力してください'
    }

    if (age === '') {
      errors.age = '年齢を入力してください'
    }

    setErrors(errors)
  }

  return (
    <form onChange={handleChange}>
      <label>
        名前
        <input type="text" name="name" />
      </label>
      {errors.name && <p>{errors.name}</p>}
      <label>
        年齢
        <input type="number" name="age" />
      </label>
      {errors.age && <p>{errors.age}</p>}
      <button type="submit">送信</button>
    </form>
  )
}

```

# 理解の確認

フォームを次のように変えてください。

## 基本的な課題

1. 入力が必須なメールアドレスの入力欄を追加してください。
2. 年齢は0から200までの数値であることを確認してください。
3. 自己紹介を `<textarea>` 要素を使って入力できるようにしてください。

## 応用的な課題

1. メールアドレスの値に `@` が含まれていない場合はエラーを表示してください。
2. 一つでもエラーがある場合は送信ボタンを押せなくしてください。


# まとめ

前の節で学んだ状態管理を使いながらフォームの実装を学びました。

フォームの場合は入力と送信という２つのイベントがあります。

また、エラーと値も管理してきました。エラーではユーザーが入力した内容から表示する内容を作成しました。

次の節では非同期処理を学びます。

- [非同期処理](./04-async.md)
