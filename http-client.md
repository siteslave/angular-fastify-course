# HTTP Client

## Add `HTTPClient`

file `app.config.ts`

```typescript
export const appConfig: ApplicationConfig = {
	providers: [provideRouter(routes), provideHttpClient(withFetch())],
};
```

## Create service

```shell
ng g s services/dummy --skip-tests
```

update `services/dummy.service.ts`

```typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';

@Injectable({
	providedIn: 'root',
})
export class DummyService {
	constructor(private http: HttpClient) {}

	getUsers(): Observable<any> {
		const url = `https://dummyjson.com/users?limit=10`;
		return this.http.get(url);
	}
}
```

## Using service

file `main.component.ts`

```typescript
constructor( private dummyService: DummyService) {}

ngOnInit() {
    this.getUsers();
}

async getUsers() {
    this.dummyService.getUsers()
    .pipe(
        catchError((error) => {
            console.error(error);
            return of(error);
        })
    )
    .subscribe((data) => {
        this.users = data.users;
    });
}
```

## Update user info

file `dummy.service.ts`

```typescript
updateUser(id: any, data: any): Observable<any> {
    const url = `https://dummyjson.com/users/${id}`;
    return this.http.put(url, { ...data });
}
```

file `main.component.ts`

```typescript
doUpdate(user: any) {
    const data: any = {
        firstName: 'Satit',
        lastName: 'Rianpit',
        gender: 'male',
    };

    const id = user.id;

    // do update
    this.dummyService.updateUser(id, data)
        .pipe(
            catchError((error) => {
                console.error(error);
                return of(error);
            })
        )
        .subscribe((data) => {  console.log(data);  });
}
```

file `main.component.html`

```html
<button class="btn btn-sm btn-primary" (click)="doUpdate(user)">แก้ไข</button>
```

## Delete user

file `dummy.service.ts`

```typescript
deleteUser(id: any): Observable<any> {
    const url = `https://dummyjson.com/users/${id}`;
    return this.http.delete(url);
}
```

file `main.component.ts`

```typescript
doDelete(user: any) {
    const id = user.id;

        // do update
        this.dummyService.deleteUser(id)
            .pipe(
                catchError((error) => {
                    console.error(error);
                    return of(error);
                })
            )
            .subscribe((data) => { console.log(data); });
}
```

file `main.component.html`

```html
<button class="btn btn-sm btn-error" (click)="doDelete(user)">ลบรายการ</button>
```

## Pagination

update `dummy.service.ts`

```typescript
getUsers(limit: number = 10, skip: number = 0): Observable<any> {
    const url = `https://dummyjson.com/users?limit=${limit}&skip=${skip}`;
    return this.http.get(url);
}
```

update `main.component.ts`

```typescript
total = 0;
currentPage = 1;
allPages: any[] = [];
limit = 10;
skip = 0;

async getUsers() {
    this.dummyService
        .getUsers(this.limit, this.skip)
        .pipe(
            catchError((error) => {
                console.error(error);
                return of(error);
            })
        )
        .subscribe((data) => {
            this.users = data.users;
            this.total = data.total ?? 0;
            const pages = Math.ceil(this.total / this.limit);
            // Create array from pages
            this.allPages = Array(pages)
                .fill(0)
                .map((x, i) => i + 1);
        });
}

goto(page: number) {
    if (page === this.currentPage) {
        return;
    }

    this.currentPage = page;
    this.skip = (this.currentPage - 1) * this.limit;
    this.getUsers();
}

```

update `main.component.html`

```html
<div class="flex justify-center">
	<div class="join">
		@for (page of allPages ; track $index) {
		<button
			class="join-item btn"
			[class.btn-active]="page === currentPage"
			(click)="goto(page)"
		>
			{{ page }}
		</button>
		}
	</div>
</div>
```
