# HttpClient with JWT

## Update `dummy.service.ts`

```typescript
private getHeaders(): HttpHeaders {
    const authToken = sessionStorage.getItem('token');
    return new HttpHeaders({
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${authToken}`,
    });
}

getUsers(limit: number = 10, skip: number = 0): Observable<any> {
    const headers = this.getHeaders();
    const url = `https://dummyjson.com/auth/users?limit=${limit}&skip=${skip}`;
    return this.http.get(url, { headers });
}
```
