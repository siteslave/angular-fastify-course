# Basic Routing

## Create route

file `demo.js`

```javascript
'use strict';

module.exports = async function (fastify, opts) {
	fastify.get('/demo', async function (request, reply) {
		return { ok: true, message: 'Hello demo api.' };
	});

	fastify.post('/demo', async function (request, reply) {
		return { ok: true, message: 'POST' };
	});

	fastify.put('/demo', async function (request, reply) {
		return { ok: true, message: 'PUT' };
	});

	fastify.delete('/demo', async function (request, reply) {
		return { ok: true, message: 'DELETE' };
	});
};
```
