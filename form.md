# Form

Create register form component

```shell
ng g c components/register-user-form --skip-tests --inline-style
```

### Create form layout

file `register-user-form.component.ts`

```typescript
@Component({
	// ...
	imports: [ReactiveFormsModule],
	// ...
})
export class RegisterUserFormComponent {
	registerForm = new FormGroup({
		firstName: new FormControl('', [Validators.required]),
		lastName: new FormControl('', [Validators.required]),
		email: new FormControl('', [
			Validators.pattern('^[a-z0-9._%+-]+@[a-z0-9.-]+\\.[a-z]{2,4}$'),
			Validators.required,
		]),
		gender: new FormControl('', [Validators.required]),
	});
}
```

file `register-user-form.component.html`

```html
<form [formGroup]="registerForm">
	<div class="grid grid-cols-2 gap-4">
		<!-- First Name -->
		<div>
			<label class="form-control w-full max-w-xs">
				<div class="label">
					<span class="label-text">ชื่อ</span>
				</div>
				<input
					formControlName="firstName"
					type="text"
					class="input input-bordered w-full max-w-xs"
				/>
				<div class="label">
					<span class="label-text-alt"></span>
				</div>
			</label>
		</div>
		<!-- Last Name  -->
		<div>
			<label class="form-control w-full max-w-xs">
				<div class="label">
					<span class="label-text">นามสกุล</span>
				</div>
				<input
					formControlName="lastName"
					type="text"
					class="input input-bordered w-full max-w-xs"
				/>
				<div class="label">
					<span class="label-text-alt"></span>
				</div>
			</label>
		</div>
		<!-- Email -->
		<div>
			<label class="form-control w-full max-w-xs">
				<div class="label">
					<span class="label-text">อีเมล์</span>
				</div>
				<input
					formControlName="email"
					type="email"
					class="input input-bordered w-full max-w-xs"
				/>
				<div class="label">
					<span class="label-text-alt"></span>
				</div>
			</label>
		</div>
		<!-- Gender -->
		<div>
			<label class="form-control w-full max-w-xs">
				<div class="label">
					<span class="label-text">เพศ</span>
				</div>
				<select
					formControlName="gender"
					class="select select-bordered w-full max-w-xs"
				>
					<option value="male">ชาย</option>
					<option value="female">หญิง</option>
				</select>
				<div class="label">
					<span class="label-text-alt"></span>
				</div>
			</label>
		</div>
	</div>

	<div class="pt-4">
		<button class="btn btn-primary">บันทึกข้อมูล</button>
	</div>
</form>
```

### Check form valid

```html
<button class="btn btn-primary" [disabled]="!registerForm.valid">
	บันทึกข้อมูล
</button>
```

or in `*.component.ts` file

```typescript
if (!registerForm.valid) {
	// alert form is invalid!
}
```

## Get form data

file `register-user-form.component.html`

```html
<form [formGroup]="registerForm" (ngSubmit)="onSubmit()"></form>
```

file `register-user-form.component.ts`

```typescript
onSubmit() {
    console.log(this.registerForm.value);
}
```

## Initial form data

```typescript
constructor() {
    // Initial form data
    this.registerForm.setValue({
        firstName: 'Satit',
        lastName: 'Rianpit',
        email: 'rianpit@gmail',
        gender: 'male',
    });
}
```

## Advanced form validation

file `register-user-form.component.ts`

```typescript
get form(): { [key: string]: AbstractControl } {
    return this.registerForm.controls;
}
```

file `register-user-form.component.html`

```html
@if (form['email'].errors) {
<div class="label">
	<span class="label-text-alt text-error"> กรุณาระบุอีเมล์ให้ถูกต้อง </span>
</div>
}
```
