 # สร้าง CRUD กับตารางดาตาเบสหนึ่งตัว #

เริ่มต้นจากการสร้าง route ใหม่ก่อน

```php
Route::get('/fruit', 'FruitController@index');
```

หลังจากนั้นก็พิมพ์คำสั่งเพื่อสร้าง FruitController ขึ้นมา (ตำแหน่งปัจจุบันของ Terminal เราต้องอยู่ในโฟลเดอร์ application)

```php
php artisan make:controller FruitController
```

แล้วก็สร้าง Model Fruit ในโฟลเดอร์ application/app/Models ด้วยเลย
```php
php artisan make:model Models/FruitController
```

จากนั้นเราจะได้ FruitController.php ใน application/app/Http/Controllers ซึ่งมีหน้าตาดังนี้
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class FruitController extends Controller
{
    
}
```

อย่าลืมดึง Model Fruit เข้ามาใช้งานด้วย เพื่อที่เราจะดึงข้อมูลจากตาราง fruits ได้

```php
<?php

namespace App\Http\Controllers;
use App\Models\Fruit; // ดึง Model Fruit เข้ามา

use Illuminate\Http\Request;

class FruitController extends Controller
{
    
}
```

สร้างฟังชั่น index ขึ้นมาและให้ ฟังชันนี้ส่งค่าแถวทั้งหมดในตาราง fruits กลับไป

```php
function index()
{
    $fruits = Fruit::get();

    return $fruits;
}
```

กลับไปที่บราวเซอร์ localhost/fruit เพื่อดูความเปลี่ยนแปลง

```json
[
    {
        "id": 1,
        "name": "apple",
        "color": "red",
        "price": "50.00"
    },
    {
        "id": 2,
        "name": "banana",
        "color": "yellow",
        "price": "30.00"
    }
]
```

ทีนี้เราอยากจะแสดงข้อมูลที่เราดึงมาในรูปแบบของหน้าเว็บแทน ไห้ไปสร้าง View ขึ้นมาใหม่
ที่ application/resources/views สร้างไฟล์ที่ชื่อ fruit.blade.php

ใน fruit.blade.php ไห้เราเขียนโค้ด html ลงไปเพื่อสร้างตารางแสดงข้อมูลทั้งหมด

```html
<table>
    <thead>
        <th>id</th>
        <th>name</th>
        <th>color</th>
        <th>price</th>
    </thead>
    <tbody>      
    @foreach($fruits as $fruit)
        <tr> 
            <td> {{ $fruit->id }} </td>
            <td> {{ $fruit->name }} </td>
            <td> {{ $fruit->price }} </td>
            <td> {{ $fruit->color }} </td>
        </tr>
    @endforeach
    </tbody>
</table>
```

หลังจากนั้น ย้อนกลับไปที่ FruitController เปลี่ยนฟังชัน index ให้มันส่งตัวแปร $fruits ที่ดึงมาจาก Model Fruits ส่งไปให้ View แทน

```php
function index()
{
    $fruits = Fruit::get();

    return view("fruit", [
        "fruits" => $fruits
    ]);
}
```

กลับไปที่บราวเซอร์ localhost/fruit เพื่อดูความเปลี่ยนแปลง



