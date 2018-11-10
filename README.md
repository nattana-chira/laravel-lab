# เตือนความจำ #
- โฟลเดอร์ public จะเป็นโฟลเดอร์มีไว้เก็บไฟล์รูปภาพ, css, js
- โฟลเดอร์ application มีไว้เก็บ source code ของโปจเจ็คเราทั้งหมด

- application/.env คือไฟล์ที่เอาไว้กำหนดการตั้งค่าของแอปลิเคชัน
- application/routes/web.php คือไฟล์ที่กำหนด route ต่างๆ
- application/app/Http/Controllers คือโฟลเดอร์เก็บ Controller
- application/app/Http/Models คือโฟลเดอร์เก็บ Model
- application/resources/views คือโฟลเดอร์เก็บ View

 
 # สร้าง CRUD กับตารางดาตาเบสหนึ่งตัว #

เริ่มต้นจากการสร้าง route ใหม่ก่อน (人◕ω◕) 

```php
Route::get('/fruit', 'FruitController@index');
```

หลังจากนั้นก็พิมพ์คำสั่งเพื่อสร้าง FruitController ขึ้นมา (ตำแหน่งปัจจุบันของ Terminal เราต้องอยู่ในโฟลเดอร์ application)

```php
php artisan make:controller FruitController
```

แล้วก็สร้าง Model Fruit ในโฟลเดอร์ application/app/Models ด้วยเลย (・ωｰ)～☆ 
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

กลับไปที่บราวเซอร์ localhost/fruit เพื่อดูความเปลี่ยนแปลง ヾ(・ω・ｏ)

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
ที่ application/resources/views สร้างไฟล์ที่ชื่อ fruit.blade.php ԅ( ˘ω˘ԅ)

ใน fruit.blade.php ไห้เราเขียนโค้ด html ลงไปเพื่อสร้างตารางแสดงข้อมูลทั้งหมด

```html
<style>
    td {
        border: 1px solid black;
    }
</style>

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

หลังจากนั้น ย้อนกลับไปที่ FruitController เปลี่ยนฟังชัน index ให้มันส่งตัวแปร $fruits ที่ดึงมาจาก Model Fruits ส่งไปให้ View แทน ((⊂(`ω´∩)

```php
function index()
{
    $fruits = Fruit::get();

    return view("fruit", [
        "fruits" => $fruits
    ]);
}
```

กลับไปที่บราวเซอร์ localhost/fruit เพื่อดูความเปลี่ยนแปลง  (人ゝω・）

จะเห็นได้ว่าหน้าเว็บเราแสดงข้อมูลเป็นตารางของ HTML ได้แล้ว

ต่อมาเราต้องการสร้าง Form HTML เพื่อเพิ่มข้อมูลใหม่ไปยังดาต้าเบส

เราก็สร้าง View ขึ้นมาใหม่หนึ่งตัวชื่อ ชื่อ fruit-create.blade.php ซึ่งในไฟล์นี้เราจะสร้าง Form HTML สำหรับการส่งข้อมูลขึ้นมา (* >ω<)=3

```html
<form action="/fruit/create" method="post"> <!-- method post จะทำให้ Laravel รู้ว่า request นี้เป็น POST request -->
    @csrf <!-- ตัวนี้ทำให้ Laravel รู้ว่าข้อมูลนี้ส่งมาจากโดเมนเดียวกันมีความเชื่อถือได้ -->

    <div>
        <input type="text" name="name" placeholder="ชื่อผลไม้">
    </div>
    <div>
        <select name="color">
            <option>-- เลือกสี --</option>
            <option value="red">แดง</option>
            <option value="yellow">เหลือง</option>
            <option value="green">เขียว</option>
            <option value="purple">ม่วง</option>
            <option value="orange">ส้ม</option>
        </select>
    </div>
    <div>
        <input type="number" name="price" placeholder="ราคา">
    </div>
    
</form>
```

หลังจากนั้นเราต้องสร้าง route อันใหม่ขึ้นมาเพื่อที่จะแสดง Form ที่เราสร้างขึ้น

```php
Route::get('/fruit/create', 'FruitController@renderCreateForm');
```

กลับไปที่ FruitController เพิ่มฟังชั่น renderCreateForm เพื่อให้มันส่ง View กลับมา

```php
function renderCreateForm()
{
    return view("fruit-create");
}
```

เรามี Form HTML สำหรับเพิ่มข้อมูลแล้วทีนี้เราก็ต้องสร้างฟังชันสำหรับรับค่า Form และบันทึกลงดาต้าเบส

สร้าง route สำหรับบันทึกข้อมูล fruit

```php
Route::post('/fruit/create', 'FruitController@saveFruit');
```

สร้างฟังชั่นสำหรับบันทึกข้อมูล และหลังจากบันทึกเสร็จหน้าเว็บจะเด้งกลับไปที่ /fruit โดยอัติโนมัติ

```php
function saveFruit(Request $request)
{
    $fruit = new Fruit();
    $fruit->name    = $request->name;
    $fruit->color   = $request->color;
    $fruit->price   = $request->price;
    $fruit->save();

    return redirect("/fruit");
}
```

---------------------------------------------------------------------------
---------------------------------------------------------------------------

# ความรู้เพิ่มเติม #

- basic knowledge of web development (make it easier to use laravel if you understand these things)
  - CRUD
  - RESTful API
  - Session

- advance knowledge of laravel
  - Database Migration (Factory Seed)
  - Service Provider
  - Middleware
  - Event Listener
  - Repository Pattern


