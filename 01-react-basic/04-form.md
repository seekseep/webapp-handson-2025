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

### テンプレートリテラル

`window.alert(`${name}さん`)` のように文字列の中に変数を埋め込むことをテンプレートリテラルといいます。

```js
`文字列 ${変数} 文字列`
```

は次のコードと同じ意味を持ちます。

```js
"文字列" + 変数 + "文字列"
```

また、改行コードもそのまま使えます。

```js
`文字列
文字列`
```

```js
"文字列\n文字列"
```

この２つも同じ文字列になります。

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

## `&&` と　`||` の使い方

### truthy と falsy

JavaScript では次の値は falsy として扱われます。

- `false`
- `null`
- `undefined`
- `0`
- `NaN`
- `''`

それ以外の値は truthy として扱われます。

### `&&`

次のコードはそれぞれ同じ結果を返します。

name に値が入っているときに表示する。

```jsx
function App () {
  const name = 'Alice'
  let nameNode = null
  if (name) {
    nameNode = <p>{name}</p>
  }

  return (
    <div>
      {nameNode}
    </div>
  )
}

```

```jsx
function App () {
  const name = 'Alice'
  const nameNode = name && <p>{name}</p>
  return (
    <div>
      {nameNode}
    </div>
  )
}
```

```jsx
function App () {
  const name = 'Alice'
  return (
    <div>
      {name && <p>{name}</p>}
    </div>
  )
}
```

### `||`

次のコードはそれぞれ同じ結果を返します。

name が falsy のときに表示する。

```jsx
function App () {
  const name = ''
  let nameNode = null
  if (!name) {
    nameNode = <p>名前がありません</p>
  }

  return (
    <div>
      {nameNode}
    </div>
  )
}
```

```jsx
function App () {
  const name = ''
  const nameNode = name || <p>名前がありません</p>
  return (
    <div>
      {nameNode}
    </div>
  )
}
```

```jsx
function App () {
  const name = ''
  return (
    <div>
      {name || <p>名前がありません</p>}
    </div>
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
  if (Number.isNaN(+age) || age < 0 || age > 200) {
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

#### スプレッド構文

`...` はスプレッド構文と呼ばれ、オブジェクトや配列を展開するために使われます。

```js
const obj = { a: 1, b: 2 }
const obj2 = { ...obj, c: 3 }
console.log(obj2) // { a: 1, b: 2, c: 3 }
```

```js
const arr = [1, 2]
const arr2 = [...arr, 3]
console.log(arr2) // [1, 2, 3]
```

詳しくは、[スプレッド構文](スプレッド構文) を参照してください。

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

## Formik

Formik はフォームの状態管理、バリデーション、送信処理を簡単に実装できるライブラリです。

yup というライブラリと組み合わせることでバリデーションの仕組みと表示の仕組みを分けて作ることもできます。

```jsx
import { Formik, Form, Field, ErrorMessage } from 'formik'
import * as Yup from 'yup'

const validationSchema = Yup.object().shape({
  name: Yup.string()
    .required("必須項目です")
    .max(20, "20文字以内で入力してください"),
  age: Yup.number()
    .required("必須項目です")
    .min(0, "0以上の数値を入力してください")
    .max(200, "200以下の数値を入力してください")
    .typeError("数値を入力してください"),
  email: Yup.string()
    .email("不正なメールアドレスです")
    .required("必須項目です")
})

function App () {
  const handleSubmit = (values) => {
    console.log(values)
  }
  return (
    <Formik
      initialValues={{ name: '', age: 20, email: '' }}
      onSubmit={handleSubmit}
      validationSchema={validationSchema}>
        {({ isValid }) => (
          <Form>
            <label>
              名前:
              <Field type="text" name="name" />
              <ErrorMessage name="name" component="div" />
            </label>
            <label>
              年齢:
              <Field type="number" name="age" />
              <ErrorMessage name="age" component="div" />
            </label>
            <label>
              メールアドレス:
              <Field type="email" name="email" />
              <ErrorMessage name="email" component="div" />
            </label>
            <button type="submit" disabled={!isValid}>
              送信
            </button>
          </Form>
        )}
    </Formik>
  )
}

export default App
```

ただし、カスタマイズが難しかったりパフォーマンスの調整が難しいという欠点もあります。

## React Hook Form

React Hook Form を使う場合は次のように書きます。

React Hook Form はパフォーマンスが高く、カスタマイズがしやすいという特徴があります。
カスタマイズのしやすさはコードの量が増えることにも繋がります。

```jsx
import { useForm } from "react-hook-form";

function App() {
  const {
    register,
    handleSubmit,
    formState: { errors, isValid },
  } = useForm({
    mode: "onChange",
    defaultValues: {
      name: "",
      age: 20,
      email: "",
    },
  });

  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label>
        名前:
        <input
          {...register("name", {
            required: "必須項目です",
            maxLength: { value: 20, message: "20文字以内で入力してください" },
          })}
        />
        {errors.name && <div>{errors.name.message}</div>}
      </label>

      <label>
        年齢:
        <input
          type="number"
          {...register("age", {
            required: "必須項目です",
            min: { value: 0, message: "0以上の数値を入力してください" },
            max: { value: 200, message: "200以下の数値を入力してください" },
            validate: (value) =>
              isNaN(value) ? "数値を入力してください" : true,
          })}
        />
        {errors.age && <div>{errors.age.message}</div>}
      </label>

      <label>
        メールアドレス:
        <input
          type="email"
          {...register("email", {
            required: "必須項目です",
            pattern: {
              value: /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/,
              message: "不正なメールアドレスです",
            },
          })}
        />
        {errors.email && <div>{errors.email.message}</div>}
      </label>

      <button type="submit" disabled={!isValid}>
        送信
      </button>
    </form>
  );
}

export default App;
```

# まとめ

フォームを作成するときには次の要素が重要になります。

- 入力欄の値を管理する
- バリデーション
- 初期値を指定する
- 送信(Submit)時の処理

要件ごとに実装方法は変わりますが、Reactの機能を使うことで柔軟に対応できることがわかりました。

ライブラリを活用する例も見ました。

一番良いライブラリがあるというわけではなく、プロジェクトの要件に応じて選択することが重要です。

ライブラリを使うのか、使わないのか、使う場合はどのライブラリを使うのか、それぞれのメリットとデメリットを理解して選択することが重要です。

また、最終的には自身のプロジェクトの要件に応じてカスタマイズすることを求められることも多いです。
その場合はそれぞれの選んだ方法を正しく理解することで保守性の高いコードになります。

[非同期処理](05-async.md) に進みましょう。
