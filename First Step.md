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
