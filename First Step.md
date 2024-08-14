# First Step Laravel Guide

1. Install Laravel secara global.
```php
composer global require laravel/installer
```

2. Buat sebuah project baru.
```php
laravel new example-app
```

3. Jalankan Xampp, lalu buka VSCode dan buka project folder yang telah dibuat.
4. Setting file .env dan ubah menggunakan mysql untuk database dan untuk nama database juga masukan.
5. Setelah itu jalankan migrasi untuk bisa menggunakan table dengan cara.
```blade
php artisan migrate
```

6. Jika untuk menghapus migrasi yang sudah dibuat bisa gunakan.
```blade
php artisan migrate:rollback
```

7. Jika untuk memuat ulang/ada perubahan dibuat bisa gunakan.
```blade
php artisan migrate:fresh
```

8. Membuat file dengan sistem partial yang akan dipecah ada bagian Main,Navbar,Footer.
9. Buat folder partial di dalam folder view dan buat file nya dengan nama: main.blade.php , navbar.blade.php , footer.blade.php .
10. Ubah Route agar menuju halaman main/Home, pada folder Route > Web.php.
11. Jika ingin tambah tabel baru kita bisa buat model dan sekaligus dengan migrasi nya jadi akan terbuat juga tabel nya.
```php
php artisan make:model NamaModel
```

12. jangan lupa jalankan laravel nya
```php
php artisan serve
```

13. Setelah itu buat sebuah folder pada views dengan nama partials, didalam buat lagi file dengan nama main.blade.php, navbar.blade.php, footer.blade.php.
14. Agar lebih mudah buat dahulu semuanya di main.blade.php lalu nanti tinggal di cut untuk bagian Navbar dan Footer nya.
15. Jangan lupa tambahkan @include("partials/navbar.blade.php") dan @include("partials/footer.blade.php") pada bagian yang telah di cut.
16. Dan untuk main nya setelah @include("partials/navbar.blade.php") bawah nya buat untuk tempat content artikel untuk tiap page agar dinamis dengan cara.
```html
<div class="container-main">
        @yield('myContent')
</div>
```

17. Sekaang sudah siap tinggal buat file baru untuk tiap page yang akan dibuat dan ditambahkan pada awal agar bisa pakai navbar dan footer dari main.
```html
@extends("partials.main")
@section("myContent")
   <!--Content Anda -->
@endsection
```

18. Setelah buat page jangan lupa buat juga route nya pada web.php.
```php
Route::get('/', function () {
    return view('home');
});

Route::get('best', function () {
    return view('best');
});
```
