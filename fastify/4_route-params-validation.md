# Route params validation

## Install package

```shell
npm i fluent-json-schema -S
```

## Import `flent-json-schema`

file `demo.js`

```javascript
/** @type {import('fluent-json-schema').default} */
const S = require('fluent-json-schema');
```

## Usage

### Query string

file `demo.js`

```javascript
fastify.get(
	'/demo',
	{
		schema: {
			querystring: S.object()
				.prop('name', S.string().minLength(5).required())
				.prop('limit', S.number().min(10).max(30))
				.prop('offset', S.number().min(0)),
		},
	},
	async function (request, reply) {
		// ...
	}
);
```

### Params

file `demo.js`

```javascript
fastify.get(
	'/demo/:id',
	{
		schema: {
			params: S.object().prop('id', S.number().required()),
		},
	},
	async function (request, reply) {
		// ...
	}
);
```

### Body params (JSON)

file `demo.js`

```javascript
fastify.post(
	'/demo',
	{
		schema: {
			body: S.object()
				.prop('username', S.string().minLength(5))
				.prop('password', S.string().minLength(8))
				.required(['username', 'password']),
		},
	},
	async function (request, reply) {
		// ...
	}
);
```
