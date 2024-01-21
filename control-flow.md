# `@for`

file `users/main/main.component.ts`

```typescript
    users: any[] = [
        {
            id: 1,
            username: 'satit',
            firstName: 'Satit',
            lastName: 'Rianpit',
            email: 'rianpit@gmail.com',
            gender: 'male',
        },
        {
            id: 2,
            username: 'naracha',
            firstName: 'Naracha',
            lastName: 'Rianpit',
            email: 'naracha@gmail.com',
            gender: 'female',
        },
    ];
```

file `users/main/main.component.html`

```html
<div class="card bg-base-100 shadow-xl">
	<div class="card-body">
		<h2 class="card-title">รายชื่อสมาชิก</h2>
		<p>รายชื่อสมาชิกทั้งหมดในระบบ</p>

		<table class="table">
			<!-- head -->
			<thead>
				<tr>
					<th>ชื่อผู้ใช้งาน</th>
					<th>ชื่อ - สกุล</th>
					<th>เพศ</th>
					<th>อีเมล์</th>
					<th></th>
				</tr>
			</thead>
			<tbody>
				@for (user of users; track $index) {
				<tr class="hover">
					<td>{{ user.username }}</td>
					<td>{{ user.firstName }} {{ user.lastName }}</td>
					<td>{{user.gender}}</td>
					<td>{{ user.email }}</td>
					<td>
						<div class="space-x-2">
							<button class="btn btn-primary">แก้ไข</button>
							<button class="btn btn-warning">
								เปลี่ยนรหัสผ่าน
							</button>
							<button class="btn btn-error">ลบรายการ</button>
						</div>
					</td>
				</tr>
				} @empty {
				<tr>
					<td colspan="5">
						<div class="text-center">ไม่พบข้อมูล</div>
					</td>
				</tr>
				}
			</tbody>
		</table>
	</div>
</div>
```

# `@if`

file `users/main/main.component.html`

```html
<td>
	@if (user.gender === 'male') { ชาย } @else if (user.gender === 'female') {
	หญิง } @else { ไม่ทราบ }
</td>
```
