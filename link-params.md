# Link parameters

## Static data

Create `info` page

```shell
ng g c users/info --skip-tests --inline-style
```

### Update routing

file `users-routing.module.ts`

```typescript
{
    path: 'info',
    component: InfoComponent,
    data: { id: 1, name: 'Satit Rianpit' },
}
```

### Get data from route

file `info.component.ts`

```typescript
constructor(private activatedRoute: ActivatedRoute) {
    const data = this.activatedRoute.snapshot.data;
    console.log(data);
}
```

### Update link
file `users/main/main.component.html`
```html
<button class="btn btn-primary" routerLink="/users/info">
    Link to info page (Static)
</button>
```

update `users/main/main.component.ts` and import `RouterModule`
```typescript
@Component({
    selector: 'app-main',
    standalone: true,
    imports: [RouterModule],
    templateUrl: './main.component.html',
    styles: ``,
})
```

## Dynamic data

Create route
```shell
ng g c users/edit --skip-tests --inline-style
```
Update routing (`users-routing.module.ts`)
```typescript
{
    path: 'edit/:id',
    component: EditComponent,
    title: 'แก้ไขสมาชิก',
}
```

### Set route data

update file `users/main/main.component.html`
```html
<button class="btn btn-secondary" routerLink="/users/edit/1234">
    Link to edit page (Dynamic)
</button>

<button class="btn btn-accent" [routerLink]="['/users/edit', '1234']">
    Link to edit page (Dynamic)
</button>

<button
    class="btn btn-success"
    [routerLink]="['/users/edit/1234', { name: 'Satit Rianpit' }]"
>
    Link to eidt page (Dynamic)
</button>

<button class="btn btn-info" (click)="goto('111111')">
    Link to eidt page (Dynamic)
</button>
```

update file `users/main/main.component.ts`
```typescript
import { Router, RouterModule } from '@angular/router';

constructor(private router: Router) {}

goto(id: string) {
    const url = `/users/edit/121212?departmentId=${id}`;
    this.router.navigateByUrl(url);
}
```

### Get route data

update file `users/edit.component.ts`
```typescript
import { ActivatedRoute } from '@angular/router';

constructor(private activatedRoute: ActivatedRoute) {
    const id = this.activatedRoute.snapshot.paramMap.get('id');
    console.log(id);

    const name = this.activatedRoute.snapshot.paramMap.get('name');
    console.log(name);

    // Query params
    this.activatedRoute.queryParams.subscribe((params) => {
        console.log(params);
    });
}
```