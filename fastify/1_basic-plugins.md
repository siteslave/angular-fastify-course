# Install standard plugins

```shell
npm i @fastify/cors @fastify/formbody -S
```

update file `app.js`

```javascript
module.exports = async function (fastify, opts) {
	fastify.register(require('@fastify/cors'), {
		origin: true,
	});

	fastify.register(require('@fastify/formbody'));

	// ...
};
```
