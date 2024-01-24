# Environment

Generate environment files

```shell
ng generate environments
```

## Updaet environments

`environtments/environment.ts`

```typescript
export const environment = {
	production: true,
	apiUrl: '/api',
};
```

`environtments/environment.development.ts`

```typescript
export const environment = {
	production: false,
	apiUrl: 'http://localhost:3000',
};
```

## Usage

```typescript
apiUrl = '';
constructor(private http: HttpClient) {
    this.apiUrl = environment.apiUrl;
}
```
