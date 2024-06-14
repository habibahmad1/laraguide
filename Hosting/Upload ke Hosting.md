# Jika File baru di ambil dari Github
1. Ketik beberapa kode dibawah ini dahulu sebelum upload ke Hosting.
   ```php
    git clone https://github.com/username/repository.git
    composer install
    php artisan key:generate
    # Edit file .env sesuai kebutuhan
    php artisan migrate
    php artisan db:seed # Opsional
    php artisan serve
   ```
2. Jangan lupa jika .env tidak ada buat baru dengan cara copy .env.sample di folder.
3. Lalu sesuaikan settingan nya seperti nama database, url, dll.

# Upload ke Hosting
1. Keluarkan folder public dari file Laravel
2. lalu jadi ada 2 file public dan laravel kemudian di jadikan zip.
3. Setelah di zip upload file ke hosting.
4. file public upload di folder public_html bawaan hosting.
5. File Laravel upload di folder root/terluaur dari hosting folder.
6. ekstrak file public lalu keluarkan semua isi file public ke public_html.
7. lalu hapus file yang sudah tidak terpakai yaitu folder public dan zip nya.
8. setelah itu ekstrak juga file laravel nya dan hapus zip nya.
9. kemudian pada bagian index.php di folder public_html edit.
10. edit pada file ini:
    ```php
      require __DIR__.'/../laravelAnda/vendor/autoload.php';
      $app = require_once __DIR__.'/../laravelAnda/bootstrap/app.php';
    ```
11. LaravelAnda ganti dengan nama folder yang anda gunakan di root folder hosting tadi.
12. Lalu ubah versi PHP di Hosting.
13. Import file database ke mysql_database di hosting.
14. Pilih new database dan buat nama dan password.
15. Setelah itu buat user dan password, itu yang akan digunakan konek di .env
16. Jangan lupa atur privilage user agar bisa dipakai user nya.
17. Edit file .env yang pada folder root laravel di hosting.
    ```php
      APP_NAME=Blogger
      APP_ENV=production
      APP_KEY=base64:Ctrfbkw0ihJ+zu2aDDb/yhxI+hrYPTPWpcNDoGA8LTY=
      APP_DEBUG=false
      APP_URL=http://habibahmad.my.id
      
      LOG_CHANNEL=stack
      LOG_DEPRECATIONS_CHANNEL=null
      LOG_LEVEL=debug
      
      DB_CONNECTION=mysql
      DB_HOST=127.0.0.1
      DB_PORT=3306
      DB_DATABASE=gghaxkaj_blogger
      DB_USERNAME=gghaxkaj_blogger
      DB_PASSWORD=contoh
    ```
18. Sesuaikan nama nya.
19. app_env ganti ke production dan app_debug ganti ke false.
20. Selesai.

# Jika Upload Image tidak muncul
1. Buat Symlink dengan cara tulis ini di web.php
   ```php
      Route::get('/test', function(){
      $target = storage_path('app/public');
      $link = $_SERVER['DOCUMENT_ROOT'] . '/storage';
      symlink($target, $link);
      });
   ```
2. Lalu kunjungi route itu, yourdomain.com/test
3. Jangan lupa untuk atur .env
   ```php
      FILESYSTEM_DISK=public
   ```
4. Jangan lupa berika izin ceklis pada folder public_html di storage 755.
5. Selesai
