# Signal

## Update `auth.service.ts`

```typescript
isLogged = signal(false);

isAuthenticated() {
    const token: any = sessionStorage.getItem('token');
    if (!token) {
        this.isLogged.set(false);
        // ...
    }
    // ...
    if (isExpired) {
        this.isLogged.set(false);
        // ...
    }

    this.isLogged.set(true);
    return true;
}

loggout() {
    sessionStorage.clear();
    this.isLogged.set(false);
}
```

update `app.component.ts`

```typescript
isLogged = false;

constructor(private authService: AuthService) {
    effect(() => {
        // console.log(`isLogged: ${this.authService.isLogged()}`);
        this.isLogged = this.authService.isLogged();
    });
}

logout() {
    this.authService.loggout();
    this.route.navigate(['/login']);
}

```

### Hide menu

update file `app.component.html`

```html
@if (isLogged) {
<ul class="menu menu-horizontal px-1">
	<!-- Other menu -->
</ul>
}
```
