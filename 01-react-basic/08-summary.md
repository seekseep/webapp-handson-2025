# 振り返り

ここまで学んできたことを簡単に振り返りましょう

# プロジェクトの作成

Vite を使って React プロジェクト作成しました。

```
> npm create vite@latest my-react-app -- --template react

Scaffolding project in /path/to/my-react-app...

Done. Now run:

  cd my-react-app
  npm install
  npm run dev
```

# 状態管理

`useState` を使って状態管理を行いました。

カウンターアプリを作りました。

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

# コンポーネント

## 配列からコンポーネントを描画

```jsx
function App () {
  const items = ['りんご', 'ばなな', 'みかん']

  return (
    <ul>
      {items.map(item => (
        <li key={item}>{item}</li>
      ))}
    </ul>
  )
}
```

## ファイル分割

```
src/
  components/
    Header.jsx
    Footer.jsx
  App.jsx
```

```jsx
function Header () {
  return <header>ヘッダー</header>
}

export default Header
```

```jsx
import Header from './components/Header'
import Footer from './components/Footer'

function App () {
  return (
    <div>
      <Header />
      <main>メイン</main>
      <Footer />
    </div>
  )
}
```

# フォーム

状態管理を使いながらフォームのバリデーションを行いました。

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

# 非同期処理

非同期処理を行いました。

```jsx
import { useState } from 'react'

// fetchZipcodeByAddress は省略

function App () {
  const [address, setAddress] = useState({
    zipcode: '',
    address1: '',
    address2: '',
    address3: ''
  })
  const [error, setError] = useState('')
  const [loading, setLoading] = useState(false)

  // handleSubmit は省略

  return (
    <div>
      <h1>住所を表示する</h1>
      <form onSubmit={handleSubmit}>
        <label>
          郵便番号
          <input name="zipcode" />
        </label>
        <button type="submit">検索</button>
      </form>
      {loading && <p>ローディング中...</p>}
      <dl>
        <dt>郵便番号</dt>
        <dd>{address.zipcode}</dd>
        <dt>住所1</dt>
        <dd>{address.address1}</dd>
        <dt>住所2</dt>
        <dd>{address.address2}</dd>
        <dt>住所3</dt>
        <dd>{address.address3}</dd>
      </dl>
    </div>
  )
}
```

# ローカルストレージ

ローカルストレージを使ってデータを保存しました。

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

# まとめ

React の基本的な使い方を学びました。

次はここまで学んだことを使って実際に顧客管理システムを作ってみましょう。

- [顧客管理システム](../04-practice-only-frontend/01-introduction.md)
