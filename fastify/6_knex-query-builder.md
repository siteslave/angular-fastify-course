# Query builder

`knex` website https://knexjs.org/guide/

## Select

```javascript
db('users').select('username', 'password', 'first_name');
```

## Where

```javascript
db('users').select('*').where('username', 'satit').where('id', 1111);

// Like
const query = `%satit%`;
db('users')
	.select('*')
	.where('username', 'like', query)
	.where((w) => {
		w.where('first_name', 'like', query).orWhere(
			'last_name',
			'like',
			query
		);
	});

// Order by
db('users').select('*').orderBy('first_name', 'desc');

// Raw query
const sql = 'SELECT * FROM users WHERE id=1';
db.raw(sql);

db('users')
	.select(db.raw('count(*) as total'), 'first_name', 'last_name')
	.groupBy('first_name');
```
