# Route params

## Query string

Pattern : `http://localhost:3000/demo?fname=Satit&lname=Rianpit`

file `demo.js`

```javascript
fastify.get('/demo', async function (request, reply) {
	const { fname, lname } = request.query;
	return { ok: true, fname, lname };
});
```

## Params

Pattern: `http://localhost:3000/demo/123456`

file `demo.js`

```javascript
fastify.get('/demo/:id', async function (request, reply) {
	const { id } = request.params;
	return { ok: true, id };
});
```

## Body params (JSON)

file `demo.js`

```javascript
fastify.post('/demo', async function (request, reply) {
	const { username, password } = request.body;
	return { ok: true, username, password };
});
```
