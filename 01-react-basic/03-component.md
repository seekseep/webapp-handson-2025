# コンポーネント

コンポーネントについて学びます。

コンポーネントは要素や部品という意味です。Reactでは表示する単位を切り分けてコンポーネントとして扱います。

今までのコードで扱ってきた　`App` はコンポーネントです。次のようにコンポーネントを定義します。

コンポーネントに分ける理由は様々なものがありますが、大きな理由は再利用性です。

コンポーネントを使うことで同じコードを何度も書くことなく再利用することができます。その他にも関心を分離することでコードの見通しを良くすることができます。

# コンポーネント分割の利用例

次のようなコードを考えてみましょう。

```jsx
function App () {
  return (
    <div>
      <div>
        <a href="/">トップに戻る</a>
      </div>
      <div>
        <h1>田中太郎</h1>
        <p>大きなパキラを育てるために貯金を頑張っている。</p>
        <dl>
          <dt>住所</dt>
          <dd>東京都</dd>
          <dt>年齢</dt>
          <dd>20歳</dd>
        </dl>
      </div>
      <div>
        <h1>山田花子</h1>
        <p>猫ときつねうどんが好き。茶色いものは美味しいと考えている。</p>
        <dl>
          <dt>住所</dt>
          <dd>大阪府</dd>
          <dt>年齢</dt>
          <dd>32歳</dd>
        </dl>
      </div>
      <div>
        <p>このプロフィールはフィクションです。</p>
      </div>
    </div>
  )
}
```

##　内容の整理

この App コンポーネントの内容を整理してみましょう

- トップに戻るリンク
- 田中太郎のプロフィール
- 山田花子のプロフィール
- フィクションである旨の表示

次のようにも言い換えられます

- ナビゲーション
- プロフィール(田中太郎)
- プロフィール(山田花子)
- 注意書き

## 各コンポーネントの作成

### ナビゲーション

ナビゲーションをコンポーネントとして切り出します。

```jsx
function Navigation () {
  return (
    <div>
      <a href="/">トップに戻る</a>
    </div>
  )
}
```

### プロフィール(田中太郎)

田中太郎のプロフィールをコンポーネントとして切り出します。

```jsx
function ProfileTanakaTaro () {
  return (
    <div>
      <h1>田中太郎</h1>
      <p>大きなパキラを育てるために貯金を頑張っている。</p>
      <dl>
        <dt>住所</dt>
        <dd>東京都</dd>
        <dt>年齢</dt>
        <dd>20歳</dd>
      </dl>
    </div>
  )
}
```

### プロフィール(山田花子)

山田花子のプロフィールをコンポーネントとして切り出します。

```jsx
function ProfileYamadaHanako () {
  return (
    <div>
      <h1>山田花子</h1>
      <p>猫ときつねうどんが好き。茶色いものは美味しいと考えている。</p>
      <dl>
        <dt>住所</dt>
        <dd>大阪府</dd>
        <dt>年齢</dt>
        <dd>32歳</dd>
      </dl>
    </div>
  )
}
```

### 注意書き

注意書きをコンポーネントとして切り出します。

```jsx
function Caution () {
  return (
    <div>
      <p>このプロフィールはフィクションです。</p>
    </div>
  )
}
```

### App コンポーネント

最後に　App コンポーネントを修正します。

```jsx
function App () {
  return (
    <div>
      <Navigation />
      <ProfileTanakaTaro />
      <ProfileYamadaHanako />
      <Caution />
    </div>
  )
}
```

Appの内容が整理されたと思います。

# Props の利用

２つのプロフィールを表すコンポーネントを見てみましょう。

```jsx
function ProfileTanakaTaro () {
  return (
    <div>
      <h1>田中太郎</h1>
      <p>大きなパキラを育てるために貯金を頑張っている。</p>
      <dl>
        <dt>住所</dt>
        <dd>東京都</dd>
        <dt>年齢</dt>
        <dd>20歳</dd>
      </dl>
    </div>
  )
}
```

```jsx
function ProfileYamadaHanako () {
  return (
    <div>
      <h1>山田花子</h1>
      <p>猫ときつねうどんが好き。茶色いものは美味しいと考えている。</p>
      <dl>
        <dt>住所</dt>
        <dd>大阪府</dd>
        <dt>年齢</dt>
        <dd>32歳</dd>
      </dl>
    </div>
  )
}
```

この２つはとても似ていることがわかります。

共通する箇所を一般化してみると次のようになります。

`{名前}`と`{紹介}`、`{住所}`、`{年齢}` の箇所は変数となります。

```html
<div>
  <h1>{名前}</h1>
  <p>{紹介}</p>
  <dl>
    <dt>住所</dt>
    <dd>{住所}</dd>
    <dt>年齢</dt>
    <dd>{年齢}</dd>
  </dl>
</div>
```

## コンポーネントの定義

それではこれらの変数を受け取れるようにコンポーネントを作成しましょう。

```jsx
function Profile (props) {
  return (
    <div>
      <h1>{props.name}</h1>
      <p>{props.introduction}</p>
      <dl>
        <dt>住所</dt>
        <dd>{props.address}</dd>
        <dt>年齢</dt>
        <dd>{props.age}歳</dd>
      </dl>
    </div>
  )
}
```

変数の対応は次のとおりです。

|変数名|内容|例|
|---|---|---|
|name|名前|田中太郎|
|introduction|紹介|大きなパキラを育てるために貯金を頑張っている。|
|address|住所|東京都|
|age|年齢|20|

`function ProfileTanakaTaro()` と `function ProfileYamadaHanako()` は削除してください。

`Profile` コンポーネントを利用するように変更します。

## コンポーネントの利用

`App.jsx` を次のように書き換えましょう

```jsx
function App () {
  return (
    <div>
      <Navigation />
      <Profile
        name="田中太郎"
        introduction="大きなパキラを育てるために貯金を頑張っている。"
        address="東京都"
        age="20" />
      <Profile
        name="山田花子"
        introduction="猫ときつねうどんが好き。茶色いものは美味しいと考えている。"
        address="大阪府"
        age="32" />
      <Caution />
    </div>
  )
}

```

# 配列の利用

次のプロフィールの中に更にプロフィールを追加することを考えます。

```jsx
function App () {
  return (
    <div>
      <Navigation />
      <Profile
        name="田中太郎"
        introduction="大きなパキラを育てるために貯金を頑張っている。"
        address="東京都"
        age="20" />
      <Profile
        name="山田花子"
        introduction="猫ときつねうどんが好き。茶色いものは美味しいと考えている。"
        address="大阪府"
        age="32" />
      <Caution />
    </div>
  )
}
```

次のようにどんどん増えていくと項目を一つ追加しようとすると変更が大変になります。

```jsx
function App () {
  return (
    <div>
      <Navigation />
      <Profile
        name="田中太郎"
        introduction="大きなパキラを育てるために貯金を頑張っている。"
        address="東京都"
        age="20" />
      <Profile
        name="山田花子"
        introduction="猫ときつねうどんが好き。茶色いものは美味しいと考えている。"
        address="大阪府"
        age="32" />
      <Profile
        name="木村祐一"
        introduction="釣りが趣味で、週末は川に出かけることが多い。"
        address="北海道"
        age="29" />
      <Profile
        name="西岡美咲"
        introduction="アーモンドチップをたくさん買ったので消費するためにお菓子のレシピを探している。"
        address="奈良県"
        age="22" />
      <Profile
        name="町田章介"
        introduction="ゴルフクラブの形が好きで集めている"
        address="和歌山県"
        age="22" />
      <Profile
        name="中村美咲"
        introduction="お酒が好きで、週末は友達と飲み歩くことが多い。"
        address="大阪府"
        age="39" />
      <Caution />
    </div>
  )
}

```

そこで次のように書き換えます。

まずは人の情報を配列に入れます。

```js
const people = [{
    name: "田中太郎",
    introduction: "大きなパキラを育てるために貯金を頑張っている。",
    address: "東京都",
    age: "20"
  }, {
    name: "山田花子",
    introduction: "猫ときつねうどんが好き。茶色いものは美味しいと考えている。",
    address: "大阪府",
    age: "32"
  }, {
    name: "木村祐一",
    introduction: "釣りが趣味で、週末は川に出かけることが多い。",
    address: "北海道",
    age: "29"
  }, {
    name: "西岡美咲",
    introduction: "アーモンドチップをたくさん買ったので消費するためにお菓子のレシピを探している。",
    address: "奈良県",
    age: "22"
  }, {
    name: "町田章介",
    introduction: "ゴルフクラブの形が好きで集めている",
    address: "和歌山県",
    age: "22"
  }, {
    name: "中村美咲",
    introduction: "お酒が好きで、週末は友達と飲み歩くことが多い。",
    address: "大阪府",
    age: "39"
  }]
```

次に配列を使ってコンポーネントを作成します。

```jsx
const people = [{
  name: "田中太郎",
  introduction: "大きなパキラを育てるために貯金を頑張っている。",
  address: "東京都",
  age: "20"
}, {
  name: "山田花子",
  introduction: "猫ときつねうどんが好き。茶色いものは美味しいと考えている。",
  address: "大阪府",
  age: "32"
}, /* 省略 */]

function App () {
  return (
    <div>
      <Navigation />
      {people.map((person, index) => {
        return (
          <Profile
            key={index}
            name={person.name}
            introduction={person.introduction}
            address={person.address}
            age={person.age} />
        )
      })}
      <Caution />
    </div>
  )
}
```

これで動くようになっています。どのようにして動作しているのかを確認してみましょう。

## 配列の描画の動き方

### 1. 配列を使わない場合

```jsx
function App () {
  return (
    <div>
      <Navigation />
      <Profile
        name="田中太郎"
        introduction="大きなパキラを育てるために貯金を頑張っている。"
        address="東京都"
        age="20" />
      <Profile
        name="山田花子"
        introduction="猫ときつねうどんが好き。茶色いものは美味しいと考えている。"
        address="大阪府"
        age="32" />
      <Profile
        name="木村祐一"
        introduction="釣りが趣味で、週末は川に出かけることが多い。"
        address="北海道"
        age="29" />
      <Profile
        name="西岡美咲"
        introduction="アーモンドチップをたくさん買ったので消費するためにお菓子のレシピを探している。"
        address="奈良県"
        age="22" />
      <Profile
        name="町田章介"
        introduction="ゴルフクラブの形が好きで集めている"
        address="和歌山県"
        age="22" />
      <Profile
        name="中村美咲"
        introduction="お酒が好きで、週末は友達と飲み歩くことが多い。"
        address="大阪府"
        age="39" />
      <Caution />
    </div>
  )
}
```

### 2. 描画する内容を配列にいれる

次のようにコンポーネントは配列にいれることができます。

```jsx
function App () {
  const profiles = [
    <Profile
      key={0}
      name="田中太郎"
      introduction="大きなパキラを育てるために貯金を頑張っている。"
      address="東京都"
      age="20" />,
    <Profile
      key={1}
      name="山田花子"
      introduction="猫ときつねうどんが好き。茶色いものは美味しいと考えている。"
      address="大阪府"
      age="32" />,
    <Profile
      key={2}
      name="木村祐一"
      introduction="釣りが趣味で、週末は川に出かけることが多い。"
      address="北海道"
      age="29" />,
    <Profile
      key={3}
      name="西岡美咲"
      introduction="アーモンドチップをたくさん買ったので消費するためにお菓子のレシピを探している。"
      address="奈良県"
      age="22" />,
    <Profile
      key={4}
      name="町田章介"
      introduction="ゴルフクラブの形が好きで集めている"
      address="和歌山県"
      age="22" />,
    <Profile
      key={5}
      name="中村美咲"
      introduction="お酒が好きで、週末は友達と飲み歩くことが多い。"
      address="大阪府"
      age="39" />
  ]
  return (
    <div>
      <Navigation />
      {profiles}
      <Caution />
    </div>
  )
}
```

#### key について

`key` は React が要素を識別するための特別なプロパティです。

ここでは詳細の説明は避けますが、各要素に割り振るIDだと考えてください。
このIDを下に画面を描画するタイミングを最適化しています。

```jsx
  <Profile
    key={5}
    name="中村美咲"
    introduction="お酒が好きで、週末は友達と飲み歩くことが多い。"
    address="大阪府"
    age="39" />
```

### 3. 配列の内容をデータから作る

次のようにデータを配列に入れてコンポーネントを作成します。

```jsx
const people = [{
  name: "田中太郎",
  introduction: "大きなパキラを育てるために貯金を頑張っている。",
  address: "東京都",
  age: "20"
}, {
  name: "山田花子",
  introduction: "猫ときつねうどんが好き。茶色いものは美味しいと考えている。",
  address: "大阪府",
  age: "32"
}, /* 省略 */]

function App () {
  const profiles = []

  for (let i = 0; i < people.length; i++) {
    const person = people[i]
    profiles.push(
      <Profile
        key={i}
        name={person.name}
        introduction={person.introduction}
        address={person.address}
        age={person.age} />
    )
  }

  return (
    <div>
      <Navigation />
      {profiles}
      <Caution />
    </div>
  )
}


```

これは次のようにも書くことができます。

```jsx
function App () {
  const profiles = people.map((person, index) => {
    return (
      <Profile
        key={index}
        name={person.name}
        introduction={person.introduction}
        address={person.address}
        age={person.age} />
    )
  })

  return (
    <div>
      <Navigation />
      {profiles}
      <Caution />
    </div>
  )
}
```

さらに次のように書くこともできます。

```jsx
function App () {
  return (
    <div>
      <Navigation />
      {people.map((person, index) => {
        return (
          <Profile
            key={index}
            name={person.name}
            introduction={person.introduction}
            address={person.address}
            age={person.age} />
        )
      })}
      <Caution />
    </div>
  )
}
```
これが最初に提示した内容です。

# ファイル分割

現在の App.jsx の内容を確認してみましょう。

```jsx

function Navigation () {
  return (
    <div>
      <a href="/">トップに戻る</a>
    </div>
  )
}

function Profile (props) {
  return (
    <div>
      <h1>{props.name}</h1>
      <p>{props.introduction}</p>
      <dl>
        <dt>住所</dt>
        <dd>{props.address}</dd>
        <dt>年齢</dt>
        <dd>{props.age}歳</dd>
      </dl>
    </div>
  )
}

function Caution () {
  return (
    <div>
      <p>このプロフィールはフィクションです。</p>
    </div>
  )
}

const people = [{
  name: "田中太郎",
  introduction: "大きなパキラを育てるために貯金を頑張っている。",
  address: "東京都",
  age: "20"
}, {
  name: "山田花子",
  introduction: "猫ときつねうどんが好き。茶色いものは美味しいと考えている。",
  address: "大阪府",
  age: "32"
}, /* 省略 */]


function App () {
  return (
    <div>
      <Navigation />
      {people.map((person, index) => {
        return (
          <Profile
            key={index}
            name={person.name}
            introduction={person.introduction}
            address={person.address}
            age={person.age} />
        )
      })}
      <Caution />
    </div>
  )
}

```

App.jsxの内容が大きくなってしまっているのでファイルに分けましょう。

## ディレクトリ構造

`src` の中を次のようにします。

```
./src
├── App.jsx
├── Caution.jsx
├── Navigation.jsx
├── Profile.jsx
├── data.js
└── main.jsx

```

## `App.jsx`

```jsx
import Navigation from "./Navigation"
import Profile from "./Profile"
import Caution from "./Caution"
import { people } from "./data"

function App () {
  return (
    <div>
      <Navigation />
      {people.map((person, index) => {
        return (
          <Profile
            key={index}
            name={person.name}
            introduction={person.introduction}
            address={person.address}
            age={person.age} />
        )
      })}
      <Caution />
    </div>
  )
}
export default App

```

## `Navigation.jsx`

```jsx
function Navigation () {
  return (
    <div>
      <a href="/">トップに戻る</a>
    </div>
  )
}

export default Navigation

```


## `Profile.jsx`

```jsx
function Profile (props) {
  return (
    <div>
      <h1>{props.name}</h1>
      <p>{props.introduction}</p>
      <dl>
        <dt>住所</dt>
        <dd>{props.address}</dd>
        <dt>年齢</dt>
        <dd>{props.age}歳</dd>
      </dl>
    </div>
  )
}

export default Profile

```

## `Caution.jsx`

```jsx
function Caution () {
  return (
    <div>
      <p>このプロフィールはフィクションです。</p>
    </div>
  )
}

export default Caution

```

## `data.js`

人の情報は任意のものを入れてください。

```js
export const people = [{
  name: "田中太郎",
  introduction: "大きなパキラを育てるために貯金を頑張っている。",
  address: "東京都",
  age: "20"
}, {
  name: "山田花子",
  introduction: "猫ときつねうどんが好き。茶色いものは美味しいと考えている。",
  address: "大阪府",
  age: "32"
}, /* 省略 */]

```

## 動作確認

ブラウザで動作確認をしてみましょう。

各ファイルの中が整理された状態でも問題なく動作することがわかります。


# まとめ

この章では次のことを学びました。

- コンポーネントの切り出し
- 配列を利用したコンポーネントの描画
- ファイル分割

Reactの中心的な考えの一つのコンポーネントに触れました。

コンポーネントの利用の目的の中の大きなものに再利用性があります。

配列を利用したコンポーネントの描画は、同じ構造のコンポーネントを繰り返し描画する際に便利です。

最後にコンポーネントごとにファイル分割することで、コードの見通しを良くするこことができました。

次は具体的にフォームを作ってみましょう。

[フォーム](../04-form.md)
