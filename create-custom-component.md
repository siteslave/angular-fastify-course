# Create custom component (Change password dialog)

## Create component

```shell
ng g c components/modals/modal-change-password --skip-tests --inline-style
```

Add `daisyUI` modal (`modal-change-password.html`)
```html
<dialog id="modal-change-password" class="modal">
    <div class="modal-box">
        <h3 class="font-bold text-lg">เปลี่ยนรหัสผ่าน</h3>
        <p class="py-4">กรุณาระบุรหัสผ่านใหม่ที่ต้องการเปลี่ยน</p>
        <section>
            <label class="form-control w-full max-w-xs">
                <div class="label">
                    <span class="label-text">ระบุรหัสผ่านใหม่</span>
                </div>
                <input
                    [(ngModel)]="password"
                    type="password"
                    placeholder="New password"
                    class="input input-bordered w-full max-w-xs"
                />
                <div class="label">
                    <span class="label-text-alt"> </span>
                    <span class="label-text-alt">ความยาว 4 ตัวอักษรขึ้นไป</span>
                </div>
            </label>
        </section>
        <div class="modal-action">
            <button class="btn btn-primary">เปลี่ยนรหัสผ่าน</button>
            <button class="btn" (click)="closeModal()">ปิดหน้าต่าง</button>
        </div>
    </div>
</dialog>
```

update `modal-change-password.ts`
```typescript
import { Component, ElementRef } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
    selector: 'app-modal-change-password',
    standalone: true,
    imports: [FormsModule],
    templateUrl: './modal-change-password.component.html',
    styles: ``,
})
export class ModalChangePasswordComponent {
    password: string = '';

    constructor(private elementRef: ElementRef<HTMLElement>) {}
    closeModal() {
        const modal: any = this.elementRef.nativeElement.querySelector(
            '#modal-change-password'
        );
        if (modal) {
            modal.close();
        }
    }
}

```

### Using component

update `main.component.html`

```html
<button class="btn btn-error" (click)="changePassword()">
    Change Password
</button>

<app-modal-change-password />
```

update `main.component.ts`
```typescript
@Component({
    [ModalChangePasswordComponent],
})
// ...
constructor(
    private router: Router,
    private elementRef: ElementRef<HTMLElement>
) {}    

changePassword() {
    const modal: any = this.elementRef.nativeElement.querySelector(
        '#modal-change-password'
    );
    if (modal) {
        modal.showModal();
    }
}
```

### Component property

update `modal-change-password.ts`
```typescript
@Input({ required: true }) info: any;
```

update `modal-change-password.html`
```html
<h3 class="font-bold text-lg">
    เปลี่ยนรหัสผ่าน ({{ info.name }})
</h3>
```

update `main.component.html`
```html
<app-modal-change-password 
[info]="{ id: 111, name: 'Satit Rianpit' }" 
/>
```

### Component event

update `modal-change-password.ts`
```typescript
 @Output() onSubmit: EventEmitter<any> = new EventEmitter<any>();

changePassword() {
    this.onSubmit.emit(this.password);
    // close modal
}
```

update `modal-change-password.html`
```html
<div class="modal-action">
    <button
        class="btn btn-primary"
        (click)="changePassword()"
        [disabled]="!password"
    >
        เปลี่ยนรหัสผ่าน
    </button>
</div>
```

#### Get event result
update `main.component.html`
```html
<app-modal-change-password
    [info]="{ id: 111, name: 'Satit Rianpit' }"
    (onSubmit)="onChangePassword($event)"
/>
```

update `main.component.ts`
```typescript
onChangePassword($event: any) {
    console.log($event);
}
```