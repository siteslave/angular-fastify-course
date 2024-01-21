# Pipe

## Build-in pipes

file `main.component.html`

```html
<td>{{ user.username | uppercase }}</td>
```

file `main.component.ts`

```typescript
@Component({
    selector: 'app-main',
    standalone: true,
    imports: [UpperCasePipe],
    templateUrl: './main.component.html',
})
```

Pipe document url https://angular.dev/guide/pipes

## Create custom pipe

### Create pipe

```shell
ng g pipe pipes/gender --skip-tests
```

update file `pipes/gender.pipe.ts`

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
	name: 'gender',
	standalone: true,
})
export class GenderPipe implements PipeTransform {
	transform(value: unknown, ...args: unknown[]): unknown {
		if (!value) {
			return 'ไม่ระบุ';
		}

		if (value === 'male') {
			return 'ชาย';
		} else if (value === 'female') {
			return 'หญิง';
		} else {
			return 'ไม่ทราบ';
		}
	}
}
```

update file `main.component.ts`

```typescript
@Component({
    selector: 'app-main',
    standalone: true,
    imports: [ GenderPipe ],
    templateUrl: './main.component.html',
})
```

### Pipe usage

```html
<td>{{ user.gender | gender }}</td>
```
