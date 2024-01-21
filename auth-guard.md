# AuthGuard

## Create login page

```shell
ng g c login --skip-tests --inline-style
```

### Update routing

file `app.routes.ts`

```typescript
export const routes: Routes = [
	// ...
	{
		path: 'login',
		component: LoginComponent,
		title: 'เข้าสู่ระบบ',
	},
	// ...
];
```

### Add login form

file `login.component.html`

```html
<div class="flex justify-center">
	<div class="card w-96 bg-base-100 shadow-xl">
		<div class="card-body">
			<form [formGroup]="loginForm" (ngSubmit)="onSubmit()">
				<h2 class="card-title">เข้าสู่ระบบ</h2>
				<p class="pb-4">
					กรุณาระบุชื่อผู้ใช้งานและรหัสผ่านเพื่อเข้าสู่ระบบ
				</p>
				@if (isError) {
				<div role="alert" class="alert alert-error">
					<span>
						เกิดข้อผิดพลาด! ชื่อผู้ใช้งาน/รหัสผ่าน ไม่ถูกต้อง
						กรุณาตรวจสอบ
					</span>
				</div>
				}

				<div class="grid gap-4">
					<div>
						<label class="form-control w-full max-w-xs">
							<div class="label">
								<span class="label-text">ชื่อผู้ใช้งาน</span>
							</div>
							<input
								formControlName="username"
								type="text"
								class="input input-bordered w-full max-w-xs"
							/>
						</label>
					</div>
					<div>
						<label class="form-control w-full max-w-xs">
							<div class="label">
								<span class="label-text">รหัสผ่าน</span>
							</div>
							<input
								formControlName="password"
								type="password"
								class="input input-bordered w-full max-w-xs"
							/>
						</label>
					</div>
				</div>
				<div class="card-actions justify-end pt-4">
					<button
						class="btn btn-primary"
						[disabled]="!loginForm.valid"
					>
						เข้าสู่ระบบ
					</button>
				</div>
			</form>
		</div>
	</div>
</div>
```

update file `login.component.ts`

```typescript
@Component({
	// ...
	imports: [ReactiveFormsModule],
	// ...
})
export class LoginComponent {
	isError = false;

	loginForm = new FormGroup({
		username: new FormControl('', [Validators.required]),
		password: new FormControl('', [Validators.required]),
	});

	constructor(private router: Router, private dummyService: DummyService) {}

	onSubmit() {
		if (this.loginForm.valid) {
			const { username, password } = this.loginForm.value;
			this.dummyService
				.login(username, password)
				.pipe(
					catchError((error) => {
						this.isError = true;
						return of(error);
					})
				)
				.subscribe((data) => {
					if (data.token) {
						const token = data.token;
						sessionStorage.setItem('token', token);
						// redirect to main
						this.router.navigateByUrl('/users');
					} else {
						// Error
						this.isError = true;
						console.error('Login failed');
					}
				});
		}
	}
}
```

## Auth service

Create auth service

```shell
ng g s services/auth --skip-tests
```

Install `@auth0/angular-jwt` package

```shell
npm i @auth0/angular-jwt -S
```

update file `services/auth.service.ts`

```typescript
jwtHelper: JwtHelperService = new JwtHelperService();
constructor(private router: Router) {}
isAuthenticated() {
    const token: any = sessionStorage.getItem('token');
    if (!token) {
        return this.router.navigate(['/login']);
    }

    // is expired?
    const isExpired = this.jwtHelper.isTokenExpired(token);
    if (isExpired) {
        return this.router.navigate(['/login']);
    }
    return true;
}

```

## Create guard

```shell
ng g guard auth --skip-tests
```

update file `auth.guard.ts`

```typescript
export const authGuard: CanActivateFn = (route, state) => {
	const authService = inject(AuthService);
	return authService.isAuthenticated();
};
```

## Add `authGuard` to protect route

update file `app.routes.ts`

```typescript
{
    path: '',
    redirectTo: 'login',
    pathMatch: 'full',
},
{
    path: 'users',
    title: 'จัดการสมาชิก',
    canActivate: [authGuard],
    loadChildren: () =>
        import('./users/users.module').then((m) => m.UsersModule),
}
```

## Logout

update file `app.component.html`

```html
<li>
	<a href="javascript:void(0)" (click)="logout()"> ออกจากระบบ </a>
</li>
```

update file `app.component.ts`

```typescript
constructor(private route: Router) {}

logout() {
    sessionStorage.clear();
    this.route.navigate(['/login']);
}
```
