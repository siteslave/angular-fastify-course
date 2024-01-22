# Transaction

Document url https://knexjs.org/guide/transactions.html

```javascript
try {
    db.transaction(trx => {
        await trx('users').insert(user);
        await trx('users').where('id', 1).del();
    });
} catch(error) {
    console.log(error);
}
```
