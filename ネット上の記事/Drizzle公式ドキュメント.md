## これはなに
- Drizzle公式ドキュメントの読書メモ
- 重要概念やよく使いそうなAPIについての理解を書いていく

## MEET DRIZZLE
### Why Drizzle
- SQLを知っていればDrizzleもわかる
	- SQLライクな構文や機能を提供する
- SQLライクなだけではなく、便利で効率的なクエリのためのQueries APIも提供している
- サーバーレス対応を前提に設計されたORMらしい

## FUNDAMENTALS
### Schema
- DrizzleはTypeScriptでスキーマを定義することができる
- マイグレーションでDrizzle-Kitを使う場合は、スキーマファイルで定義したすべてのモデルをエクスポートする
	- Drizzle-Kitがそれらをインポートし、マイグレーション差分の処理に利用するため
- すべてのモデル定義を1つの `schema.ts`にまとめてもいいし、複数ファイルに分割してもいい
- Drizzleにおけるテーブルは、DBと同様に少なくとも1つのカラムを持って定義する必要がある
- Drizzleには共通のテーブルオブジェクトというものは存在しないので、使用するDBを選んで定義する必要がある
	- PostgreSQLでもMySQLでも使えるような定義方法、というものは存在しない
- Drizzle DB宣言時にcasingオプションを指定することで、カラム定義時のキーと実際のDBカラムのマッピングを行える
```ts
import { drizzle } from "drizzle-orm/node-postgres";
import { integer, pgTable, varchar } from "drizzle-orm/pg-core";

export const users = pgTable('users', {
	id: integer(),
	firstName: varchar()
})

const db = drizzle({ connection: process.env.DATABASE_URL, casing: 'snake_case' })

await db.select().from(users);
```
```sql
SELECT "id", "first_name" from users;
```
- カラム定義を別ファイルに分けて再利用することができる
	- updated_at, created_at, deleted_atなどの多くのモデル/テーブルで使われるであろうカラムを別で定義して利用する
```ts
const timestamps = {
	updated_at: timestamp(),
	created_at: timestamp().defaultNow().notNull(),
	deleted_at: timestamp(),
}

export const users = pgTable('users', {
	id: integer(),
	...timestamps
})

export const posts = pgTable('posts', {
	id: integer(),
	...timestamps
})
```

### Database connection
- Drizzleは、設計上すべてのエッジ環境やサーバーレスランタイムとネイティブに互換性がある

### Query data
- DrizzleでのSQLライクなDBアクセスの例
```ts
await db
	.select()
	.from(posts)
	.leftJoin(comments, eq(posts.id, comments.post_id))
	.where(eq(posts.id, 10))
```
```sql
SELECT *
FROM posts
LEFT JOIN comments ON posts.id = comments.post_id
WHERE posts.id = 10
```
```ts
await db.insert(users).values({ email: 'user@gmail.com' })
```
```sql
INSERT INTO users (email) VALUES ('user@gmail.com')
```
