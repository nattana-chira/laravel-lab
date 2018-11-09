สร้าง CRUD กับตารางดาตาเบสหนึ่งตัว

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




