# Laravel Guide

### Laravel Steps
1. Model: Pertama-tama, Anda akan membuat atau menggunakan model Eloquent untuk merepresentasikan tabel atau entitas dalam database Anda. Model ini akan berfungsi untuk berinteraksi dengan data dalam tabel tersebut. Jika Anda belum memiliki model yang sesuai, Anda dapat membuatnya dengan perintah artisan make:model.
2. Migrasi: Setelah model dibuat, Anda mungkin perlu membuat atau menyesuaikan migrasi untuk tabel yang terkait. Migrasi Laravel digunakan untuk mendefinisikan struktur database, seperti membuat tabel baru atau menambahkan kolom ke tabel yang sudah ada. Anda dapat menggunakan perintah artisan make:migration untuk membuat migrasi baru.
3. Factory: Jika Anda ingin mengisi tabel dengan data sampel untuk pengembangan atau pengujian, Anda dapat membuat factory untuk model Anda. Factory digunakan untuk menghasilkan data palsu yang dapat Anda gunakan untuk mengisi tabel dengan data sampel. Anda dapat menggunakan perintah artisan make:factory untuk membuat factory baru.
4. Controller: Setelah model Anda dan mungkin juga factory dan migrasi telah dibuat atau disesuaikan, Anda akan membuat controller yang bertanggung jawab untuk menangani logika bisnis dan menyiapkan data yang diperlukan untuk ditampilkan di halaman Anda. Controller ini akan berinteraksi dengan model untuk mengambil data dari database dan mengirimkannya ke tampilan. Anda dapat menggunakan perintah artisan make:controller untuk membuat controller baru.
5. View: Terakhir, Anda akan membuat tampilan (view) yang akan menampilkan data yang diambil dari database. Tampilan ini akan mengambil data yang dikirimkan oleh controller dan menampilkannya sesuai dengan kebutuhan Anda. Anda dapat membuat tampilan dengan menggunakan Blade, template engine bawaan Laravel, yang memudahkan Anda untuk memasukkan data dinamis ke dalam tampilan.
Setelah langkah-langkah di atas selesai, Anda kemungkinan akan mengatur rute (route) di Laravel untuk mengarahkan permintaan HTTP yang masuk ke halaman yang tepat pada controller Anda. Dengan begitu, ketika pengguna mengakses URL tertentu, controller akan menyiapkan data yang diperlukan dan mengarahkan ke tampilan yang sesuai untuk ditampilkan kepada pengguna.

### Relationship Table

Galeri memiliki relasi pada model "User" dan "Kategori" juga. Berikut adalah langkah-langkahnya:

1. Definisikan Struktur Database:
Pastikan tabel "Galeri" memiliki kolom user_id dan kategori_id yang merupakan kunci asing yang merujuk ke tabel "User" dan "Kategori" masing-masing.
2. Buat Model:
Buat model untuk setiap tabel yang terlibat, yaitu "Galeri", "User", dan "Kategori", jika Anda belum memiliki modelnya. Anda dapat menggunakan perintah artisan untuk membuatnya:
```php
php artisan make:model Galeri
php artisan make:model User
php artisan make:model Kategori
```
3. Definisikan Relasi dalam Model:
Di dalam masing-masing model ("Galeri", "User", dan "Kategori"), tambahkan metode untuk mendefinisikan relasi dengan model lain yang sesuai.
```php
// Di dalam model Galeri
class Galeri extends Model {
    public function user() {
        return $this->belongsTo(User::class, 'user_id');
    }

    public function kategori() {
        return $this->belongsTo(Kategori::class, 'kategori_id');
    }
}

// Di dalam model User
class User extends Model {
    public function galeris() {
        return $this->hasMany(Galeri::class);
    }
}

// Di dalam model Kategori
class Kategori extends Model {
    public function galeris() {
        return $this->hasMany(Galeri::class);
    }
}
```
4. Migrate Tabel:
Jalankan migrasi untuk memastikan struktur tabel sudah sesuai dengan definisi model dan relasi yang telah Anda buat.
Copy code
php artisan migrate
5. Gunakan Relasi:
Sekarang, Anda dapat menggunakan relasi yang telah Anda definisikan dalam kode Anda. Misalnya, Anda dapat mengakses informasi tentang pengguna yang memiliki galeri tertentu atau galeri yang dimiliki oleh sebuah kategori.
```php
// Contoh penggunaan relasi
$galeri = Galeri::get();
$user = $galeri->user; // Mendapatkan informasi pengguna yang memiliki galeri ini
$kategori = $galeri->kategori; // Mendapatkan informasi kategori dari galeri ini

$user = User::get();
$galeris = $user->galeris; // Mendapatkan galeri yang dimiliki oleh pengguna ini

$kategori = Kategori::get();
$galeris = $kategori->galeris; // Mendapatkan galeri yang dimiliki oleh kategori ini
```
Dengan langkah-langkah di atas, Anda telah membuat relasi yang jelas antara tabel "Galeri", "User", dan "Kategori" dalam aplikasi Laravel Anda.

### belongsTo & hasMany
1. belongsTo:
belongsTo digunakan untuk mendefinisikan relasi "mengarah ke" antara dua model, di mana model yang memanggil metode belongsTo merupakan model yang memiliki kunci asing yang merujuk ke model lain.
Dalam contoh ini, pada model "Galeri", kita mendefinisikan dua relasi belongsTo:
user(): Menunjukkan bahwa setiap entitas "Galeri" dimiliki oleh satu entitas "User". Artinya, setiap galeri memiliki satu pengguna yang terkait dengannya.
kategori(): Menunjukkan bahwa setiap entitas "Galeri" dimiliki oleh satu entitas "Kategori". Artinya, setiap galeri memiliki satu kategori yang terkait dengannya.
2. hasMany:
hasMany digunakan untuk mendefinisikan relasi "miliki banyak" antara dua model, di mana model yang memanggil metode hasMany memiliki banyak entitas yang terkait dengan model lain.
Dalam contoh ini, pada model "User" dan "Kategori", kita mendefinisikan relasi hasMany:
Pada model "User", kita memiliki relasi galeris(), yang menunjukkan bahwa setiap entitas "User" dapat memiliki banyak entitas "Galeri". Artinya, seorang pengguna dapat memiliki banyak galeri.
Pada model "Kategori", kita juga memiliki relasi galeris(), yang menunjukkan bahwa setiap entitas "Kategori" dapat memiliki banyak entitas "Galeri". Artinya, sebuah kategori dapat memiliki banyak galeri.

### Membuka setiap postingan yang sesuai isi nya

Rangkuman alur pembuatan dan penanganan permintaan untuk menampilkan detail setiap galeri yang dipilih:

1. Membuat View Detail Galeri:
Buat sebuah file blade untuk menampilkan detail galeri, misalnya *detailGaleri.blade.php*.
Desain tampilan sesuai kebutuhan, termasuk bagaimana judul, deskripsi, dan gambar galeri ditampilkan.
2. Menambahkan Rute untuk Detail Galeri:
Tambahkan rute baru ke dalam file web.php untuk menangani permintaan detail galeri.

```php
Contoh: Route::get('/detail/{galeri}', [GaleriController::class, 'detail']);
```

Pastikan placeholder dalam rute ({galeri}) sesuai dengan parameter dalam metode controller.
4. Menambahkan Metode Controller untuk Detail Galeri:
Tambahkan metode baru ke dalam controller yang akan menangani permintaan detail galeri.
Dalam metode ini, gunakan model binding untuk mendapatkan instance galeri berdasarkan ID yang diberikan.
Kirimkan instance galeri ke tampilan detail galeri.
Contoh:
```php
public function detail(Galeri $galeri)
{
    return view('detailGaleri', [
        'title' => 'Detail Galeri',
        'galeri' => $galeri
    ]);
}
```

4. Menampilkan Detail Galeri dalam View:
Dalam view detailGaleri.blade.php, gunakan data galeri yang diterima dari controller untuk menampilkan judul, deskripsi, dan gambar galeri.
```html
<h1>{{ $galeri->judul }}</h1>
<p>{{ $galeri->deskripsi }}</p>
<img src="{{ $galeri->gambar }}" alt="Gambar Galeri">
```
Dengan langkah-langkah ini, Anda akan memiliki halaman detail galeri yang berfungsi dengan baik di aplikasi Laravel Anda. Pastikan untuk memeriksa setiap langkah dengan hati-hati dan menyesuaikannya dengan kebutuhan aplikasi Anda.

### Melihat Setiap User upload galeri apa saja
1. Buat Sebuah link untuk user ketika di klik, bisa melihat postingan apa saja yang diupload oleh user itu.
   ```html
   <a href="/uploaded/{{ $item->user->username }}" class="uploadBy">Upload By : {{ $item->user->name }}</a>
   ```
2. Buat URL di route web.php untuk bisa di akses url nya.
   ```php
   Route::get('/uploaded/{uploaded:username}', [UserController::class, "index2"]);
   ```
3. Buka UserController dan buat function baru untuk route nya.
   ```php
    public function index2(User $uploaded)
    {
        $galeri = Galeri::with(['kategoriGaleri', 'user'])
            ->where('user_id', $uploaded->id)
            ->get();

        return view('galeri', [
            'title' => "Galeri",
            'galeri' => $galeri,
        ]);
    }
   ```
   Pastikan menggunakan model Galeri:: dan dengan relasi apa saja yang sudah terhubung di model galeri, disini menggunakan kategoriGaleri dan user karena untuk menampilkan data nya butuh itu agar bisa diakses.

