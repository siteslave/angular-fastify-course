# JWT Authen

## Install packages

```shell
npm i @fastify/jwt bcrypt -S
```

## Create plugin

create file `plugins/jwt.js`

```javascript
'use strict';

const fp = require('fastify-plugin');

module.exports = fp(async function (fastify, opts) {
	fastify.register(require('@fastify/jwt'), {
		secret: process.env.JWT_SECRET || 'xxxxxx',
		sign: {
			iss: process.env.JWT_ISSUER || 'test.satit.dev',
			expiresIn: '1h',
		},
	});
});
```

## Update password with `bcrypt` package

create file `models/hash.js`

```javascript
const bcrypt = require('bcrypt');

module.exports.hashPassword = async (password) => {
	const salt = await bcrypt.genSalt(10);
	return await bcrypt.hash(password, salt);
};

module.exports.comparePassword = async (password, hash) => {
	return await bcrypt.compare(password, hash);
};
```

update password in `CREATE` route
file `rotues/test.js`

```javascript
const hash = require('../models/hash');

const hashed = await hash.hashPassword(password);
const user = {
	username: username,
	password: hashed,
	// ...
};
```

Add change password model

file `models/test.js`

```javascript
changePassword(db, id, password) {
    return db('users').where('user_id', id).update({ password: password });
},
```

Add change password route

file `routes/test.js`

```javascript
fastify.put(
	'/test/:id/password',
	{
		schema: {
			body: S.object().prop(
				'password',
				S.string().maxLength(20).required()
			),
			params: S.object().prop('id', S.number().required()),
		},
	},
	async function (request, reply) {
		const db = fastify.db;
		const { password } = request.body;
		const { id } = request.params;

		try {
			const hashed = await hash.hashPassword(password);
			await testModel.changePassword(db, id, hashed);
			return reply.status(200).send({ ok: true });
		} catch (error) {
			console.log(error);
			reply
				.status(500)
				.send({ ok: false, message: 'Internal server error' });
		}
	}
);
```

## Login model

create file `models/login.js`

```javascript
module.exports = {
	login(db, username) {
		return db('users')
			.select('username', 'password', 'first_name', 'last_name', 'role')
			.where('username', username)
			.first();
	},
};
```

## Login route

create file `routes/login.js`

```javascript
'use strict';
/** @type {import('fluent-json-schema').default} */
const S = require('fluent-json-schema');

const loginModel = require('../models/login');
const hash = require('../models/hash');

module.exports = async function (fastify, opts) {
	const db = fastify.db;

	fastify.post(
		'/login',
		{
			schema: {
				body: S.object()
					.prop('username', S.string().required())
					.prop('password', S.string().required()),
			},
		},
		async function (request, reply) {
			const { username, password } = request.body;
			const user = await loginModel.login(db, username);

			if (!user) {
				return reply
					.status(401)
					.send({ ok: false, message: 'User not found.' });
			}

			if (!(await hash.comparePassword(password, user.password))) {
				return reply
					.status(401)
					.send({ ok: false, message: 'Wrong password.' });
			}

			const token = fastify.jwt.sign({
				sub: user.user_id,
				role: [user.role],
			});

			return { ok: true, token };
		}
	);
};
```

## Protecting route

update file `plugins/jwt.js`

```javascript
fastify.decorate('authenticate', async function (request, reply) {
	try {
		await request.jwtVerify();
	} catch (err) {
		reply.send(err);
	}
});
```

update file `routes/test.js`

```javascript
fastify.get(
	'/test',
	{
		onRequest: [fastify.authenticate],
	},
	async function (request, reply) {
		// Get jwt decoded
		console.log(request.user);
		// ...
	}
);
```
