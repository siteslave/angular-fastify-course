# Third Party Pacakge (`luxon`)

## Install `luxon` package

```shell
npm i luxon -S
npm i @types/luxon -D
```

## Create pipe

```shell
ng g pipe pipes/thai-date --skip-tests
```

edit file `pipes/thai-date.pipe.ts`

```typescript
import { Pipe, PipeTransform } from '@angular/core';
import { DateTime } from 'luxon';
@Pipe({
	name: 'thaiDate',
	standalone: true,
})
export class ThaiDatePipe implements PipeTransform {
	transform(value: unknown, ...args: unknown[]): unknown {
		if (!value) {
			return '-';
		}

		return DateTime.fromFormat(value as string, 'yyyy-MM-dd')
			.setLocale('th')
			.toLocaleString(DateTime.DATE_FULL);
	}
}
```

edit file `main.component.ts`

```typescript
@Component({
    selector: 'app-main',
    standalone: true,
    imports: [
        // ...
        ThaiDatePipe,
    ],
    templateUrl: './main.component.html',
    styles: ``,
})
```

### Usage `thaiDate` pipe

```html
<td>{{ user.birthDate | thaiDate }}</td>
```
