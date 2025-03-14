# フォーム

ユーザー情報を送信するフォームを考えます。

# HTMLの機能を中心にフォームを作る

## 基本的なフォームの形

HTMLには `<form>` を始めとするフォームを作成するための要素が用意されています。

``` jsx
function App () {
  return (
    <form>
      <label>
        名前:
        <input type="text" name="name" />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}
```

## フォーム送信時の処理

これで送信するとアラートが表示されます。

しかし、その後に画面が再読込されていることを確認してください。

また、submit は input にカーソルがある状態でエンターキーを押しても送信されます。

``` jsx
function App () {

  function handleSubmit (event) {
    window.alert('送信されました')
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}
```

## 値の取得

送信時に値を取得してみましょう。

`new FormData(formElement)` でフォームの値を取得できます。

フォーム内の `name` 属性をキーとして値を取得できます。

```jsx
function App () {
  function handleSubmit (event) {
    const data = new FormData(event.target)
    const name = data.get('name')
    window.alert(`${name}さん`)
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}
```

## イベントのキャンセル

今のままでは画面が再読込されているので次のコードを追加して再読み込みをやめる。

`event.preventDefault()` はイベントのデフォルトの動作をキャンセルするメソッドです。

フォームの送信時のデフォルトのイベントは画面遷移です。現在は遷移先を指定していないので、再読み込みが行われます。

```jsx
function App () {
  function handleSubmit (event) {
    event.preventDefault()
    const data = new FormData(event.target)
    const name = data.get('name')
    window.alert(`${name}さん`)
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}
```

## 値の追加

値を追加してみましょう。

```jsx

function App () {
  function handleSubmit (event) {
    event.preventDefault()
    const data = new FormData(event.target)
    const name = data.get('name')
    const age = data.get('age')
    const email = data.get('email')
    window.alert(`${name}さんは${age}歳です。メールアドレスは${email}です`)
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" />
      </label>
      <label>
        年齢:
        <input type="text" name="age" />
      </label>
      <label>
        メールアドレス:
        <input type="text" name="email" />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}
```

## 初期値の設定

`defaultValue` 属性を追加することで初期値を設定できます。

```jsx
function App () {
  function handleSubmit (event) {
    event.preventDefault()
    const data = new FormData(event.target)
    const name = data.get('name')
    const age = data.get('age')
    const email = data.get('email')
    window.alert(`${name}さんは${age}歳です。メールアドレスは${email}です`)
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" />
      </label>
      <label>
        年齢:
        <input type="text" name="age" defaultValue="20" />
      </label>
      <label>
        メールアドレス:
        <input type="text" name="email" />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}

```

## バリデーション

`required` 属性を追加することで必須項目にすることができます。

```jsx
function App () {
  function handleSubmit (event) {
    event.preventDefault()
    const data = new FormData(event.target)
    const name = data.get('name')
    const age = data.get('age')
    const email = data.get('email')
    window.alert(`${name}さんは${age}歳です。メールアドレスは${email}です`)
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" required />
      </label>
      <label>
        年齢:
        <input type="text" name="age" defaultValue="20" required />
      </label>
      <label>
        メールアドレス:
        <input type="text" name="email" required />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}
```

この状態でも次のような問題が残っています。

- 年齢に数値以外を入力できる
- 名前に文字を大量に入力できる
- 多大くないメールアドレスを入力できる

### その他のバリデーション

次のコードでは次のバリデーションを追加しています。

- 名前は入力必須、20文字以内
- 年齢は入力必須、0以上200以下
- メールアドレスは入力必須、メールアドレス形式

```jsx
function App () {
  function handleSubmit (event) {
    event.preventDefault()
    const data = new FormData(event.target)
    const name = data.get('name')
    const age = data.get('age')
    const email = data.get('email')
    window.alert(`${name}さんは${age}歳です`)
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" required maxLength="20" />
      </label>
      <label>
        年齢:
        <input type="number" name="age" defaultValue="20" required min="0" max="200" />
      </label>
      <label>
        メールアドレス:
        <input type="email" name="email" required />
      </label>
      <button type="submit">送信</button>
    </form>
  )
}
```

他にもバリデーションの方法はいくつかある。

[クライアント側のフォーム検証](https://developer.mozilla.org/ja/docs/Learn/Forms/Form_validation) について詳しく知りたい場合はこちらを参照してください。


## HTMLの機能について

ここまで見てきたようにHTMLのAPIは非常に強力で、フォームの作成には十分な機能が揃っている。

しかし、これでは実現したい値が複雑な場合には対応できない場合がある。

その場合は useState を使ってフォームの値を管理する方法を考える。

# Reactの機能を使ってフォームを作る

## 入力中の値を表示したい

入力中の表示をするためには、useStateを使ってフォームの値を管理する。

```jsx
import { useState } from 'react'

function App () {
  const [name, setName] = useState('')

  function handleChange (event) {
    setName(event.target.value)
  }

  function handleSubmit (event) {
    event.preventDefault()
    window.alert(`${name}さん`)
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" value={name} onChange={handleChange} />
      </label>
      <dl>
        <dt>入力中の名前</dt>
        <dd>{name}</dd>
      </dl>
      <button type="submit">送信</button>
    </form>
  )
}
```

## 入力欄のヒントを出す

名前が短すぎる場合にはエラーメッセージを表示する

```jsx
import { useState } from 'react'

function App () {
  const [name, setName] = useState('')

  function handleChange (event) {
    setName(event.target.value)
  }

  function handleSubmit (event) {
    event.preventDefault()
    window.alert(`${name}さん`)
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" value={name} onChange={handleChange} />
      </label>
      {name.length < 3 && <p>短すぎる</p>}
      {3 <= name.length && name.length <= 10  && <p>適切な長さ</p>}
      {name.length > 10 && <p>長過ぎる</p>}
      <button type="submit">送信</button>
    </form>
  )
}

```

## 送信ボタンを無効にする

条件に応じて送信ボタンを無効にする

```jsx
import { useState } from 'react'

function App () {
  const [name, setName] = useState('')

  function handleChange (event) {
    setName(event.target.value)
  }

  function handleSubmit (event) {
    event.preventDefault()
    window.alert(`${name}さん`)
  }

  const isTooShort = name.length < 3
  const isTooLong = name.length > 10
  const isValid = !isTooShort && !isTooLong

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" value={name} onChange={handleChange} />
      </label>
      {isTooShort && <p>短すぎる</p>}
      {isValid  && <p>適切な長さ</p>}
      {isTooLong && <p>長過ぎる</p>}
      <button type="submit" disabled={!isValid}>
        {isValid ? '送信' : '入力してください'}
      </button>
    </form>
  )
}
```

## 入力欄ごとにエラーを表示する

```jsx
import { useState } from 'react'

function App () {
  const [name, setName] = useState('')
  const [age, setAge] = useState('')

  function handleChangeName (event) {
    setName(event.target.value)
  }

  function handleChangeAge (event) {
    setAge(event.target.value)
  }

  function handleSubmit (event) {
    event.preventDefault()
    window.alert(`${name}さんは${age}歳です`)
  }

  let nameError = null
  if (name.length < 3) {
    nameError = '短すぎる'
  } else if (name.length > 10) {
    nameError = '長過ぎる'
  }

  let ageError = null
  if (age < 0 || age > 200) {
    ageError = '不正な値'
  }

  const isValid = !nameError && !ageError

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" value={name} onChange={handleChangeName} />
      </label>
        {nameError && <p>{nameError}</p>}
      <label>
        年齢:
        <input type="number" name="age" value={age} onChange={handleChangeAge} />
      </label>
      {ageError && <p>{ageError}</p>}
      <button type="submit" disabled={!isValid}>
        {!isValid ? '入力してください' : '送信'}
      </button>
    </form>
  )
}
```

## リファクタリング

- バリデーション関数を作成する
- 値を一つのオブジェクトにまとめる

```jsx
import { useState } from 'react'

function validateName (name) {
  if (name.length < 3) {
    return '短すぎる'
  } else if (name.length > 10) {
    return '長過ぎる'
  }
  return null
}

function validateAge (age) {
  if (age < 0 || age > 200) {
    return '不正な値'
  }
  return null
}

function App () {
  const [values, setValues] = useState({
    name: '',
    age: ''
  })

  function handleChangeName (event) {
    // const nextValues ={
    //   age: values.age,
    //   name: values.name
    // }
    // nextValues.name = event.target.value
    // setValues(nextValues)
    setValues({
      ...values,
      name: event.target.value
    })
  }

  function handleChangeAge (event) {
    setValues({
      ...values,
      age: event.target.value
    })
  }

  function handleSubmit (event) {
    event.preventDefault()
    window.alert(`${values.name}さんは${values.age}歳です`)
  }

  const nameError = validateName(values.name)
  const ageError = validateAge(values.age)
  const isValid = !nameError && !ageError

  return (
    <form onSubmit={handleSubmit}>
      <label>
        名前:
        <input type="text" name="name" value={values.name} onChange={handleChangeName} />
        {nameError && <p>{nameError}</p>}
      </label>
      <label>
        年齢:
        <input type="number" name="age" value={values.age} onChange={handleChangeAge} />
        {ageError && <p>{ageError}</p>}
      </label>
      <button type="submit" disabled={!isValid}>
        {!isValid ? '入力してください' : '送信'}
      </button>
    </form>
  )
}
```

### 初期値

次の箇所で初期値を設定できます。

```js
  const [values, setValues] = useState({
    name: '',
    age: ''
  })
```

# 比較

HTMLの機能を使ったフォームの実装はコード量も少なく実装も簡単でした。しかし、自由度は低くなってしまいます。

郵便番号を下に住所を特定するような処理はHTMLの標準の機能では実現できません。

それに対して React の機能を使ったフォームの実装は自由度が高く、複雑な処理も実現できます。コード量が増えてしまうことは難点です。

# 一般的なプロジェクト

一般的なプロジェクトではこれらの煩雑な処理をライブラリを使って簡単に実装することができます。

有名なライブラリには次のようなものがあります。

- [Formik](https://formik.org/)
- [React Hook Form](https://react-hook-form.com/jp/)

この他にもいくつかありますが、これらのライブラリを使うことでフォームの実装を簡単にすることができます。

# まとめ

フォームを作成するときには次の要素が重要になります。

- 入力欄の値を管理する
- バリデーション
- 初期値を指定する
- 送信(Submit)時の処理

要件ごとに実装方法は変わりますが、Reactの機能を使うことで柔軟に対応できることがわかりました。

[非同期処理](05-async.md) に進みましょう。
