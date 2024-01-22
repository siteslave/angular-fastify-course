# CRUD

## CREATE

Create `models/test.js` file

```javascript
module.exports = {
	create(db, user) {
		return db('users').insert(user);
	},
};
```

update file `routes/test.js`

```javascript
fastify.post(
	'/test',
	{
		schema: {
			body: S.object()
				.prop('username', S.string().maxLength(15).required())
				.prop('password', S.string().maxLength(20).required())
				.prop('firstName', S.string().maxLength(50).required())
				.prop('lastName', S.string().maxLength(50).required())
				.prop(
					'role',
					S.enum(['admin', 'user']).default('user').required()
				),
		},
	},
	async function (request, reply) {
		const db = fastify.db;
		const { username, password, firstName, lastName, role } = request.body;

		try {
			const user = {
				username: username,
				password: password,
				first_name: firstName,
				last_name: lastName,
				role: role,
			};
			await testModel.create(db, user);
			return reply.status(201).send({ ok: true });
		} catch (error) {
			console.log(error);
			reply
				.status(500)
				.send({ ok: false, message: 'Internal server error' });
		}
	}
);
```

## READ

update file `models/test.js`

```javascript
read(db) {
    return db('users')
        .select('user_id', 'username', 'first_name', 'last_name', 'role')
        .orderBy('username', 'asc');
},
```

update file `routes/test.js`

```javascript
fastify.get('/test', async function (request, reply) {
	const db = fastify.db;
	const users = await testModel.read(db);
	return reply.send({ ok: true, users });
});
```

## UPDATE

update file `models/test.js`

```javascript
fastify.put(
    '/test/:id',
    {
        schema: {
            body: S.object()
                .prop('firstName', S.string().maxLength(50).required())
                .prop('lastName', S.string().maxLength(50).required())
                .prop(
                    'role',
                    S.enum(['admin', 'user']).default('user').required()
                ),
            params: S.object().prop('id', S.number().required()),
        },
    },
    async function (request, reply) {
        const db = fastify.db;
        const { firstName, lastName, role } = request.body;
        const { id } = request.params;

        try {
            const user = {
                first_name: firstName,
                last_name: lastName,
                role: role,
            };
            await testModel.update(db, id, user);
            return reply.status(200).send({ ok: true });
        } catch (error) {
            console.log(error);
            reply
                .status(500)
                .send({ ok: false, message: 'Internal server error' });
        }
    }
```

## DELETE

update file `models/test.js`

```javascript
delete(db, id) {
    return db('users').where('user_id', id).del();
},
```

update file `routes/test.js`

```javascript
fastify.delete(
	'/test/:id',
	{
		schema: {
			params: S.object().prop('id', S.number().required()),
		},
	},
	async function (request, reply) {
		const db = fastify.db;
		const { id } = request.params;

		try {
			await testModel.delete(db, id);
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
