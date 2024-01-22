# Database connection

## Install package

```shell
npm i knex mysql2 @fastify/env -S
```

## Add plugin

Create file `plugins/config.js`

```javascript
'use strict';

const fp = require('fastify-plugin');

module.exports = fp(async function (fastify, opts) {
	fastify.register(require('@fastify/env'), {
		dotenv: {
			path: `./config/.env`,
			debug: true,
		},
		schema: {
			type: 'object',
			properties: {
				NODE_ENV: {
					type: 'string',
					default: 'development',
				},
				JWT_SECRET: {
					type: 'string',
					default: 'secret',
				},
				DB_HOST: {
					type: 'string',
					default: 'localhost',
				},
				DB_PORT: {
					type: 'number',
					default: 3306,
				},
				DB_USER: {
					type: 'string',
					default: 'root',
				},
				DB_PASSWORD: {
					type: 'string',
					default: '123456',
				},
				DB_NAME: {
					type: 'string',
					default: 'test',
				},
			},
		},
	});
});
```

Create file `config/.env`

```environment
NODE_ENV=development
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=xxxxx
DB_PORT=3306
DB_NAME=dbname
JWT_SECRET=xx.xxxx
```

## Create plugin

Create file `plugins/db.js`

```javascript
'use strict';

const fp = require('fastify-plugin');
const Knex = require('knex');

module.exports = fp(async function (fastify) {
	if (!fastify.db) {
		const knex = Knex({
			client: 'mysql2',
			connection: {
				host: process.env.DB_HOST || 'localhost',
				port: process.env.DB_PORT ? Number(process.env.DB_PORT) : 3306,
				database: process.env.DB_NAME || 'test',
				user: process.env.DB_USER || 'root',
				password: process.env.DB_PASSWORD || '123456',
			},
			debug: process.env.NODE_ENV === 'development',
			pool: {
				min: 2,
				max: 10,
			},
		});

		fastify.decorate('db', knex);

		fastify.addHook('onClose', (fastify, done) => {
			if (fastify.knex === knex) {
				fastify.knex.destroy(done);
			}
		});
	}
});
```

## Create database

```sql
CREATE DATABASE queues;

CREATE TABLE `users` (
  `username` varchar(100) COLLATE utf8mb4_general_ci NOT NULL,
  `password` varchar(250) COLLATE utf8mb4_general_ci NOT NULL,
  `first_name` varchar(100) COLLATE utf8mb4_general_ci NOT NULL,
  `last_name` varchar(250) COLLATE utf8mb4_general_ci NOT NULL,
  `user_id` int NOT NULL AUTO_INCREMENT,
  `role` varchar(100) COLLATE utf8mb4_general_ci DEFAULT NULL,
  PRIMARY KEY (`user_id`),
  UNIQUE KEY `users_username_IDX` (`username`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
```

## Testing

Create route `routes/test.js`

```javascript
'use strict';

module.exports = async function (fastify, opts) {
	fastify.get('/test', async function (request, reply) {
		const db = fastify.db;
		const res = await db('users').select(
			'username',
			'first_name',
			'last_name'
		);
		return reply.send(res);
	});
};
```
