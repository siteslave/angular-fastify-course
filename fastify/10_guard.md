# Guard

## Intall pacakge

```shell
npm i fastify-guard -S
```

## Create plugin

file `plugins/guard.js`

```javascript
'use strict';

const fp = require('fastify-plugin');

module.exports = fp(async function (fastify, opts) {
	fastify.register(require('fastify-guard'), {
		errorHandler: (result, request, reply) => {
			return reply.status(405).send({
				statusCode: 405,
				error: 'You are not allowed to call this route',
			});
		},
	});
});
```

## Add guard to route

filet `routes/test.js`

```javascript
fastify.post(
	'/',
	{
		schema: {
			/** schemas */
		},
		preHandler: [fastify.guard.role('admin')],
	},
	async function (request, reply) {
		// ...
	}
);
```
