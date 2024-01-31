# View UI

file `view.component.html`

```html
<div class="hero min-h-screen bg-base-200">
	<div class="text-center">
		<div class="max-w-full">
			<form [formGroup]="form" (ngSubmit)="onSubmit()">
				<div class="card w-72 bg-base-100 shadow-xl">
					<div class="card-body">
						<h2 class="card-title">ลงทะเบียน</h2>

						<label class="form-control w-full max-w-xs">
							<div class="label">
								<span class="label-text">กรุณาระบุ HN 7 หลัก</span>
							</div>
							<input
								type="text"
								maxlength="7"
								placeholder="000344"
								formControlName="hn"
								class="input input-bordered w-full max-w-xs"
							/>
							<div class="label">
								<span class="label-text-alt"> </span>
							</div>
						</label>

						<div class="card-actions justify-center">
							<button
								type="submit"
								class="btn btn-primary"
								[disabled]="!form.valid"
							>
								ตรวจสอบ
							</button>
						</div>
					</div>
				</div>
			</form>
		</div>
	</div>
</div>
```

file `view.comonent.ts`

```typescript
import { Component } from '@angular/core';
import {
	FormControl,
	FormGroup,
	ReactiveFormsModule,
	Validators,
} from '@angular/forms';
import { Router } from '@angular/router';

@Component({
	selector: 'app-verify',
	standalone: true,
	imports: [ReactiveFormsModule],
	templateUrl: './verify.component.html',
	styles: ``,
})
export class VerifyComponent {
	form = new FormGroup({
		hn: new FormControl('', [
			Validators.required,
			Validators.minLength(7),
			Validators.maxLength(7),
			Validators.pattern('^[0-9]*$'),
		]),
	});

	constructor(private router: Router) {}

	onSubmit() {
		this.router.navigateByUrl('/viewer');
	}
}
```
