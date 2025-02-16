# データベースの用意

このセクションでは、SQLite をセットアップし、データベースを作成してデータを操作する方法を学びます。

---

## 1. SQLite のセットアップ

SQLite は軽量なデータベースで、シンプルな構成でアプリケーションに組み込むことができます。

Windows では [**Scoop** ](https://scoop.sh/) を使用してインストールできます。

その他にも [公式サイト](https://www.sqlite.org/) からのインストールの方法もあります。

Scoop はこの後も使うので、まずは Scoop のインストールから始めましょう。

### Scoop とは

Scoop は Windows 用のパッケージマネージャで、コマンドラインからアプリケーションをインストール・アップデート・アンインストールできます。

Linux の `apt` や macOS の `brew` に似た使い勝手を提供します。

### 1-1. Scoop のインストール

1. PowerShell を管理者権限で開き、以下のコマンドを実行します。

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
   ```

2. Scoop の `main` バケットが追加されていることを確認します。

   ```sh
   > scoop bucket list
   Name Source               Updated             Manifests
   ---- ------               -------             ---------
   main ~\scoop\buckets\main 2025/02/07 13:44:59      1357
   ```

   `main` バケットがない場合は、追加してください。

   ```sh
   scoop bucket add main
   ```

### 1-2. SQLite のインストール
Scoop を使って SQLite をインストールします。

```powershell
> scoop install sqlite
```

### 1-3. SQLiteBrowser のインストール

データベースを GUI で管理できる **DB Browser for SQLite** もインストールします。

```powershell
> scoop install sqlitebrowser
```

インストールのために他のプログラムのインストールが必要になる可能性があります。

エラーが出た場合は、エラーメッセージに従って必要なプログラムをインストールしてください。

インストールが完了したら、`sqlite3` コマンドと `sqlitebrowser` コマンドが使えることを確認します。

```sh
sqlite3 --version
sqlitebrowser
```

---

## 2. データベースの作成

1. SQLite のデータベースファイルを作成するために、ターミナルで以下のコマンドを実行します。

   ```sh
   sqlite3 mydatabase.db
   ```

   これにより、`mydatabase.db` というファイルが作成され、SQLite のコマンドラインが開きます。

2. `.tables` コマンドでテーブルがないことを確認します。

   ```sql
   .tables
   ```

   **出力:**
   ```
   (何も表示されない)
   ```

---

## 3. User テーブルの作成

次に、`User` テーブルを作成します。

```sql
CREATE TABLE User (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  email TEXT UNIQUE NOT NULL
);
```

作成したテーブルを確認するには、以下のコマンドを実行します。

```sql
.tables
```

**出力:**
```
User
```

カラム情報を確認するには、以下のコマンドを実行します。

```sql
PRAGMA table_info(User);
```

---

## 4. データの追加と SELECT 文の実行

### 4-1. データの追加
以下の SQL でデータを追加します。

```sql
INSERT INTO User (name, email) VALUES ("Alice", "alice@example.com");
INSERT INTO User (name, email) VALUES ("Bob", "bob@example.com");
```

### 4-2. データの取得
追加したデータを取得します。

```sql
SELECT * FROM User;
```

**出力:**
```
1|Alice|alice@example.com
2|Bob|bob@example.com
```

---

## 5. SQLite のデータを確認・更新・削除する

### 5-1. データの更新
`Alice` のメールアドレスを変更します。

```sql
UPDATE User SET email = "alice@newdomain.com" WHERE name = "Alice";
```

変更が反映されたか確認します。

```sql
SELECT * FROM User WHERE name = "Alice";
```

### 5-2. データの削除
`Bob` のデータを削除します。

```sql
DELETE FROM User WHERE name = "Bob";
```

削除が成功したか確認します。

```sql
SELECT * FROM User;
```

**出力:**
```
1|Alice|alice@newdomain.com
```

---

## 6. まとめと SQL の他の例

このセクションでは、以下の内容を学びました：
- SQLite のセットアップ（Scoop を使用）
- SQLiteBrowser のインストール
- データベースの作成
- `User` テーブルの作成
- データの追加 (`INSERT`)
- データの取得 (`SELECT`)
- データの更新 (`UPDATE`)
- データの削除 (`DELETE`)

### 他の SQL の例
#### 6-1. 条件検索 (`WHERE`)
特定の条件でデータを取得できます。

```sql
SELECT * FROM User WHERE email LIKE "%example.com";
```

#### 6-2. データのソート (`ORDER BY`)
名前の順で並び替えます。

```sql
SELECT * FROM User ORDER BY name ASC;
```

#### 6-3. 件数を取得 (`COUNT`)
データの数を取得します。

```sql
SELECT COUNT(*) FROM User;
```

#### 6-4. 重複を排除 (`DISTINCT`)
一意の値を取得します。

```sql
SELECT DISTINCT email FROM User;
```

---

## 次のステップ
次のセクションでは、**SQLite のデータを API から取得できるようにし、Express でデータを返す処理を実装** します。
