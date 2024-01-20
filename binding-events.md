# Data binding and Events

## Data binding

file `app.component.ts`
```typescript
firstName = '';
lastName = '';
```

file `app.component.html`
```html
<!-- Binding -->
<input
    [(ngModel)]="firstName"
    type="text"
    class="input input-bordered w-full max-w-xs"
/>

<!-- Template render -->
First name {{ firstName }}
```

## Event
file `app.component.ts`
```typescript
import { FormsModule } from '@angular/forms';

@Component({
    selector: 'app-root',
    standalone: true,
    imports: [CommonModule, FormsModule],
    templateUrl: './app.component.html',
    styleUrl: './app.component.css',
})
// ...
register() {
    console.log(this.firstName, this.lastName);
}

onChange($event: Event) {
    const target = $event.target as HTMLSelectElement;
    console.log(target.value);
}

```

file `app.component.html`
```html
<button class="btn btn-primary" (click)="register()">
    Register
</button>

<select
    [(ngModel)]="gender"
    (change)="onChange($event)"
    class="select select-bordered w-full max-w-xs"
>
    <option value="" disabled selected>
        Select gender
    </option>
    <option value="M">Man</option>
    <option value="W">Woman</option>
</select>

```


HTML template: 
```html
<div class="container mx-auto p-8 space-y-8">
    <div class="flex w-full">
        <div
            class="grid p-4 flex-grow card bg-base-300 rounded-box place-items-center"
        >
            <h1 class="text-lg font-semibold">Registration</h1>
            <div class="space-y-4">
                <div class="grid grid-rows-2 gap-4">
                    <div>
                        <label class="form-control w-full max-w-xs">
                            <div class="label">
                                <span class="label-text">First name</span>
                            </div>
                            <input
                                type="text"
                                class="input input-bordered w-full max-w-xs"
                            />
                        </label>
                    </div>
                    <div>
                        <label class="form-control w-full max-w-xs">
                            <div class="label">
                                <span class="label-text">Last name</span>
                            </div>
                            <input
                                type="text"
                                class="input input-bordered w-full max-w-xs"
                            />
                        </label>
                    </div>
                    <div>
                        <label class="form-control w-full max-w-xs">
                            <div class="label">
                                <span class="label-text">Gender</span>
                            </div>
                            <select
                                [(ngModel)]="gender"
                                (change)="onChange($event)"
                                class="select select-bordered w-full max-w-xs"
                            >
                                <option value="" disabled selected>
                                    Select gender
                                </option>
                                <option value="M">Man</option>
                                <option value="W">Woman</option>
                            </select>
                        </label>
                    </div>
                </div>
                <div>
                    <button class="btn btn-primary">
                        Register
                    </button>
                </div>
            </div>
        </div>
        <div class="divider divider-horizontal"></div>
        <div
            class="grid p-4 flex-grow card bg-base-300 rounded-box place-items-center"
        >
            Register info
            <p>First name: </p>
            <p>Last name: </p>
        </div>
    </div>
</div>

```