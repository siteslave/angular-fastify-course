# Page, Routing and Layout

## Page

Create page
```shell
ng g c hello --skip-tests --inline-style
```

## Routing

Edit file `app.routes.ts`

```typescript
import { HelloComponent } from './hello/hello.component';

export const routes: Routes = [
    {
        path: 'hello',
        component: HelloComponent,
        title: 'Hello page',
    },
];

```

## Layout

Edit file `app.component.html`

```html
<div class="bg-base-200 min-h-svh">
    <div class="navbar bg-base-100">
        <div class="flex-1">
            <a class="btn btn-ghost text-xl">ระบบจัดการสมาชิก</a>
        </div>
        <div class="flex-none">
            <ul class="menu menu-horizontal px-1">
                <li>
                    <a
                        href="#"
                        routerLink="/users/main"
                        routerLinkActive="active"
                    >
                        รายชื่อสมาชิก
                    </a>
                </li>
                <li>
                    <a
                        href="#"
                        routerLink="/users/register"
                        routerLinkActive="active"
                    >
                        เพิ่มสมาชิก
                    </a>
                </li>
                <li>
                    <details class="dropdown dropdown-end">
                        <summary>บัญชี</summary>
                        <ul
                            class="dropdown-content z-[1] menu p-2 shadow bg-base-100 rounded-box w-52"
                        >
                            <li><a>ข้อมูลส่วนตัว</a></li>
                            <li><a>ออกจากระบบ</a></li>
                        </ul>
                    </details>
                </li>
            </ul>
        </div>
    </div>

    <div class="max-w-wide p-8">
        <router-outlet></router-outlet>
    </div>
</div>


```

Edit file `app.component.ts`
```typescript
import { RouterModule, RouterOutlet } from '@angular/router';

@Component({
    selector: 'app-root',
    standalone: true,
    imports: [RouterOutlet, RouterModule],
    templateUrl: './app.component.html',
    styleUrl: './app.component.css',
})
```

Open url:  http://localhost:4200/hello

## Default route

file `app.routes.ts`
```typescript
{
    path: '',
    component: HelloComponent
}
```

## Redirect
```typescript
{
    path: '',
    redirectTo: 'hello',
    pathMatch: 'full',
}
```

## 404 Page

file `app.routes.ts`
```typescript
{
    // other routes
},
{
    path: '**',
    component: NotFoundComponent,
    title: 'Not found
}

```

## Nesting routes

```shell
ng g m users --routing
```

create optional routes
```shell
ng g c users/main --skip-tests --inline-style
ng g c users/register --skip-tests --inline-style
```

update file `users-routing.modules.ts`
```typescript
const routes: Routes = [
    {
        path: '',
        redirectTo: 'main',
        pathMatch: 'full',
    },
    {
        path: 'main',
        component: MainComponent,
        title: 'จัดการสมาชิก',
    },
    {
        path: 'register',
        component: RegisterComponent,
        title: 'สมัครสมาชิก',
    },
];
```

update file `app.routes.ts`
```typescript
{
    path: 'users',
    loadChildren: () =>
        import('./users/users.module').then((m) => m.UsersModule),
}
```

open url: http://localhost:4200/users