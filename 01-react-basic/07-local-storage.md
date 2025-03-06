# ローカルストレージ

ローカルストレージはブラウザにデータを保存するための仕組みです。ローカルストレージに保存されたデータは、ページを閉じても消えません。そのため、次回ページを開いたときにも保存されたデータを取得することができます。

# カウンターと再読み込み

`App.jsx` を書き換えてカウンターが動作するようにしましょう。

```jsx
import { useState } from 'react'

function App () {
  const [count, setCount] = useState(0)

  function handleClick () {
    setCount(count + 1)
  }

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={handleClick}>増やす</button>
    </div>
  )
}

```

## 再読み込み

カウンターを10まで増やしてから画面を再読み込みしてください。

カウンターが0に戻ってしまいます。これを再読込してもカウンターが維持されるようにしましょう。

# ローカルストレージに保存

ローカルストレージに保存するためには、`localStorage` オブジェクトを使います。

```jsx
import { useState } from 'react'

function App () {
  const [count, setCount] = useState(0)

  function handleClick () {
    setCount(count + 1)
    localStorage.setItem('count', count + 1)
  }

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={handleClick}>増やす</button>
    </div>
  )
}

```

これでカウンターの値がローカルストレージに保存されるようになりました。

画面を再読み込みしてみましょう。それでも、カウンターの値は維持されていません。

# ローカルストレージから取得

`useState` に関数を渡すと初期値を設定することができます。この関数は初回のみ実行されます。

```jsx
import { useState } from 'react'

function App () {
  const [count, setCount] = useState(() => {
    const count = localStorage.getItem('count')
    return count ? parseInt(count, 10) : 0
  })

  function handleClick () {
    setCount(count + 1)
    localStorage.setItem('count', count + 1)
  }

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={handleClick}>増やす</button>
    </div>
  )
}

```

これでカウンターの値が再読み込みしても維持されるようになりました。

# ローカルストレージの値の削除

ローカルストレージの値を削除するには、`localStorage.removeItem` メソッドを使います。

```jsx
import { useState } from 'react'

function App () {
  const [count, setCount] = useState(() => {
    const count = localStorage.getItem('count')
    return count ? parseInt(count, 10) : 0
  })

  function handleClick () {
    setCount(count + 1)
    localStorage.setItem('count', count + 1)
  }

  function handleClear () {
    setCount(0)
    localStorage.removeItem('count')
  }

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={handleClick}>増やす</button>
      <button onClick={handleClear}>リセット</button>
    </div>
  )
}

```

ある程度カウンターを増やした後にリセットボタンを押すと、カウンターの値が0に戻ります。
さらに再読込してもカウンターの値は維持されません。

# localStorage の各機能の確認

## key

`localStorage` オブジェクトは、キーと値のペアを保存します。キーは文字列で、値は文字列に変換されます。

```jsx
localStorage.setItem('key', 'value')
```

保存できる値はすべて文字列に変換されます。そのため、数値やオブジェクトを保存する場合は文字列に変換して保存する必要があります。

## localStorage.setItem

`localStorage.setItem` メソッドは、指定したキーと値をローカルストレージに保存します。

```jsx
localStorage.setItem('key', 'value')
```

## localStorage.getItem

`localStorage.getItem` メソッドは、指定したキーに対応する値を取得します。

```jsx
const value = localStorage.getItem('key')
```

値がない場合は `null` が返ります。

## localStorage.removeItem

`localStorage.removeItem` メソッドは、指定したキーに対応する値を削除します。

```jsx
localStorage.removeItem('key')
```

## localStorage.clear

`localStorage.clear` メソッドは、すべてのキーと値を削除します。

```jsx
localStorage.clear()
```

# JSON の保存

`localStorage` には文字列しか保存できないため、オブジェクトを保存する場合は `JSON.stringify` メソッドを使って文字列に変換します。

```jsx
const obj = { key: 'value' }
localStorage.setItem('key', JSON.stringify(obj))
```

取得する場合は `JSON.parse` メソッドを使ってオブジェクトに変換します。

```jsx
const obj = JSON.parse(localStorage.getItem('key'))
```

## プロフィールのフォーム

プロフィールを保存するフォームを作成して、ローカルストレージに保存してみましょう。

`App.jsx` を次のように書き換えます。

```jsx
import { useState } from 'react'

function App () {
  const [name, setName] = useState('')
  const [bio, setBio] = useState('')

  function handleChangeName (event) {
    setName(event.target.value)
  }

  function handleChangeBio (event) {
    setBio(event.target.value)
  }

  return (
    <div>
      <div>
        <label>
          名前:
          <input
            type='text'
            name='name'
            value={name}
            onChange={handleChangeName} />
        </label>
      </div>
      <div>
        <label>
          自己紹介:
          <textarea
            value={bio}
            onChange={handleChangeBio} />
        </label>
      </div>
    </div>
  )
}

```

これは次のように書くことができます。


```jsx
import { useState } from 'react'

function App () {
  const [values, setValues] = useState({
    name: '',
    bio: ''
  })

  function handleChangeName (event) {
    setValues({
      name: event.target.value,
      bio: values.bio
    })
  }

  function handleChangeBio (event) {
    setValues({
      name: values.name,
      bio: event.target.value
    })
  }

  return (
    <div>
      <div>
        <label>
          名前:
          <input
            type='text'
            name='name'
            value={values.name}
            onChange={handleChange} />
        </label>
      </div>
      <div>
        <label>
          自己紹介:
          <textarea
            value={values.bio}
            onChange={handleChangeBio} />
        </label>
      </div>
    </div>
  )
}

```

## JSONの保存

`localStorage` に保存するためには、オブジェクトを文字列に変換する必要があります。

次のように書くことで、プロフィールを保存することができます。

```jsx
import { useState } from 'react'

function App () {
  const [values, setValues] = useState(() => {
    const profileJson = localStorage.getItem('profile')
    if (!profileJson) {
      return {
        name: '',
        bio: ''
      }
    }

    var profile = JSON.parse(profileJson)

    return {
      name: profile.name,
      bio: profile.bio
    }
  })

  function handleChangeName (event) {
    setValues({
      name: event.target.value,
      bio: values.bio
    })
    localStorage.setItem('profile', JSON.stringify(values))
  }

  function handleChangeBio (event) {
    setValues({
      name: values.name,
      bio: event.target.value
    })
    localStorage.setItem('profile', JSON.stringify(values))
  }

  return (
    <div>
      <div>
        <label>
          名前:
          <input
            type='text'
            name='name'
            value={values.name}
            onChange={handleChangeName} />
        </label>
      </div>
      <div>
        <label>
          自己紹介:
          <textarea
            value={values.bio}
            onChange={handleChangeBio} />
        </label>
      </div>
    </div>
  )
}

```

# ブラウザごとでの保存？

localStorage はブラウザごとに保存されます。そのため、同じページを開いても別のブラウザで開いた場合とは別のデータが保存されます。

## プライベートブラウズでの保存

プライベートブラウズで開いた場合は、ローカルストレージに保存されたデータは保存されません。

値が保存された状態でブラウザのプライベートモードを開いてみてください。保存されたデータが消えていることが確認できます。

## 他のブラウザ

Edge を利用している場合は、Chromeで同じURLを開いてください。

値が保存されていないことがわかります。

## ローカルストレージの使い道

XのポストやYouTubeのコメントのようなものは localStorage に保存されていません。
これらはサーバー上に保存されています。

ローカルストレージは一般的な使い方は、ユーザーの一時的なデータを保存することです。ログインしている認証情報や、フォームの入力内容などが保存されます。


# 理解の確認

## 保存タイミングの修正

次のフォームの保存ボタンを押したときだけ、ローカルストレージに保存するように修正してください。

きれいに書ける所があれば修正してください。

```jsx
import { useState } from 'react'

function App () {
  const [values, setValues] = useState(() => {
    const profileJson = localStorage.getItem('profile')
    if (!profileJson) {
      return {
        name: '',
        bio: ''
      }
    }

    var profile = JSON.parse(profileJson)

    return {
      name: profile.name,
      bio: profile.bio
    }
  })

  function handleChangeName (event) {
    setValues({
      name: event.target.value,
      bio: values.bio
    })
  }

  function handleChangeBio (event) {
    setValues({
      name: values.name,
      bio: event.target.value
    })
  }

  function handleSubmit (event) {
    event.preventDefault()
    console.log(values)
  }

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>
          名前:
          <input
            type='text'
            name='name'
            value={values.name}
            onChange={handleChangeName} />
        </label>
      </div>
      <div>
        <label>
          自己紹介:
          <textarea
            value={values.bio}
            onChange={handleChangeBio} />
        </label>
      </div>
      <button type='submit'>保存</button>
    </form>
  )
}

```

## ファイル分割

上で作ったコードをファイル分割してください。

# まとめ

この章では、ローカルストレージについて学びました。

ローカルストレージはブラウザにデータを保存するための仕組みです。ローカルストレージに保存されたデータは、ページを閉じても消えません。そのため、次回ページを開いたときにも保存されたデータを取得することができます。

最後に振り返りましょう。

[振り返り](./08-summary.md)
