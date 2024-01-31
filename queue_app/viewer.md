# Queue Viewer UI

file `viewer.component.html`

```html
<div class="hero min-h-screen bg-base-200">
	<div class="hero-content text-center">
		<div class="max-w-full">
			<div class="card w-72 bg-base-100 shadow-xl">
				<div class="card-body">
					<h2 class="font-semibold text-2xl">สถิตย์</h2>
					<p>
						<span class="font-semibold"> คลินิกโรคเรื้อรัง </span>
					</p>

					<div>
						<div class="badge badge-accent">
							<span class="font-semibold"> คิวที่ 56 </span>
						</div>
					</div>

					<div class="divider">คิวรับริการปัจจุบัน</div>

					<div class="space-y-4">
						<p>
							<span class="font-semibold text-3xl"> 37 </span>
						</p>
						<div class="text-center">
							<span class="loading loading-spinner text-primary"></span>
						</div>
						<p>
							<span class="text-sm"> อัปเดท 13:30:45 น. </span>
						</p>
					</div>

					<div class="card-actions">
						<div class="flex w-full">
							<div class="flex-grow">
								<button class="btn btn-primary" routerLink="/verify">
									คิวอื่น
								</button>
							</div>
							<div class="flex-grow">
								<button class="btn btn-secondary">รีเฟรช</button>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</div>
```

file `viewer.component.ts`

```typescript
import { Component } from '@angular/core';
import { RouterModule } from '@angular/router';

@Component({
	selector: 'app-viewer',
	standalone: true,
	imports: [RouterModule],
	templateUrl: './viewer.component.html',
	styles: ``,
})
export class ViewerComponent {
	timmer: any;
	ngOnInit() {
		// Timmer
		this.timmer = setInterval(async () => {
			await this.callApi();
		}, 5000);
	}

	async callApi() {
		// Call Api
		const url = `https://dummyjson.com/users`;
		const response = await fetch(url);
		const data = await response.json();
		console.log(data);
	}

	ngOnDestroy() {
		clearInterval(this.timmer);
	}
}
```
