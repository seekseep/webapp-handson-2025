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
    alert(`こんにちは、${name}さん`)
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
    alert(`${name}さん(${age})`)
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

```jsx
function App () {
  function handleSubmit (e) {
    e.preventDefault()
    const data = new FormData(e.target)
    const name = data.get('name')
    const age = data.get('age')
    alert(`${name}さん(${age})`)
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

# 入力中に表示を変える

ユーザーが入力中に表示を変えることができます。

入力しながらパスワードに記号が含まれていないと表示されるようなフォームを見たことがあると思います。この場合は、入力中にパスワードの強度を表示しています。

そのように入力内容に応じて表示を変える方法を考えてみましょう。

まずは入力されている内容をそのまま画面に表示してみましょう。

```jsx
function App () {
  const [name, setName] = React.useState('')
  const [age, setAge] = React.useState('')

  function handleChange (e) {
    const data = new FormData(e.target)
    const name = data.get('name')
    const age = data.get('age')
    alert(`${name}さん(${age})`)
  }

  function handleChangeName (e) {
    setName(e.target.value)
  }

  function handleChangeAge (e) {
    setAge(e.target.value)
  }

  return (
    <form onChange={handleChange}>
      <label>
        名前
        <input
          type="text" name="name"
          onChange={handleChangeName}
          required />
      </label>
      <label>
        年齢
        <input
          type="number" name="age"
          onChange={handleChangeAge}
          required />
      </label>
      <div>
        <p>入力中の内容</p>
        <p>名前: {name}</p>
        <p>年齢: {age}</p>
      </div>
      <button type="submit">送信</button>
    </form>
  )
}

```

こうすると入力中の内容が画面に表示されます。

それぞれの値が変更されたときのイベントハンドラを定義しています。対応する要素の onChange イベントにそれぞれの関数を設定しています。


# 問題がある入力欄の表示を変える

入力が正しくない場合にその旨を表示するようにしましょう。

```jsx
function App () {
  const [name, setName] = React.useState('')
  const [nameError, setNameError] = React.useState('')
  const [age, setAge] = React.useState('')


  function handleChange (e) {
    const data = new FormData(e.target)
    const name = data.get('name')
    const age = data.get('age')
    setName(name)
    setAge(age)
    if (name === '' || age === '') {
      setError('名前と年齢を入力してください')
    } else {
      setError('')
    }
  }

  return (
    <form onChange={handleChange}>
      <label>
        名前
        <input type="text" name="name" required />
      </label>
      <label>
        年齢
        <input type="number" name="age" required />
      </label>
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
