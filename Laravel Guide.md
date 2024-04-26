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

### Menampilkan Data Galeri
1. Buat File view dengan menggunakan metode Partial Nvbar dan Main Bar dipisah, jadi tiap view sekarang hanya menghubungkan dan isi nya saja.
   ```html
    @extends('./layouts/main')
    @section('content')
    <h1 class="text-center">{{ $judulPage }}</h1>
    <center>
        <div class="category"><a href="/kategoriGaleri">All Category</a></div>
    </center>
        <div class="container-galeri">
            
            @foreach ($galeri as $item)
            <div class="boxImg">
                <img src="https://source.unsplash.com/500x400?{{ $item->kategoriGaleri->nama }}" alt="" class="img-fluid" >
                
                <div class="galeriKategori"><a href="/kategoriGaleri/{{ $item->kategoriGaleri->slug }}" class="text-decoration-none text-white">{{ $item->kategoriGaleri->nama }}</a></div>
    
                <div class="judulGaleri">
                    <a href="/detail/{{ $item->id }}" class="text-decoration-none text-white"><h5 class="mx-3 mt-2">{{ $item->judul }}</h5></a>
    
                    <a href="/uploaded/{{ $item->user->username }}" class="uploadBy">Upload By : {{ $item->user->name }}</a>
    
                    <p>{{ $item->created_at->diffForHumans() }}</p>
                </div>
            </div>
    
            @endforeach
        </div>
    @endsection
   ```

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


- Parameter pada Fungsi Controller:
```php
public function index2(User $uploaded)
```
Parameter User $uploaded di sini adalah fitur yang disebut "Implicit Binding" dalam Laravel. Ini berarti bahwa Laravel akan mencoba untuk secara otomatis mencari model User yang sesuai berdasarkan nilai dari parameter yang diberikan di URL. Misalnya, jika URL adalah /uploaded/username, maka Laravel akan mencari model User yang memiliki username yang sesuai dengan nilai username dalam URL. Jika ditemukan, objek User tersebut akan disertakan sebagai parameter $uploaded dalam fungsi.

- Membuat Query dengan Eloquent:
```php
$galeri = Galeri::with(['kategoriGaleri', 'user'])
    ->where('user_id', $uploaded->id)
    ->get();
```
Di sini, Anda menggunakan Eloquent untuk membuat query ke tabel Galeri. Metode with() digunakan untuk memuat relasi yang terhubung dengan model Galeri, dalam hal ini kategoriGaleri dan user. Kemudian, Anda menambahkan kondisi where untuk memfilter entri galeri berdasarkan user_id yang diterima dari parameter $uploaded. Dengan kata lain, ini akan mengambil semua entri galeri yang diunggah oleh pengguna yang disediakan sebagai parameter.

- Mengembalikan Data ke View:
php
```php
return view('galeri', [
    'title' => "Galeri",
    'galeri' => $galeri,
]);
```
Di sini, Anda mengembalikan data ke view dengan menggunakan view() helper Laravel. Anda menyediakan judul halaman (title) dan data galeri yang telah diambil dari database. Data ini kemudian dapat digunakan dalam tampilan (view) untuk ditampilkan kepada pengguna.

   ### Menampilkan seluruh Kategori pada Galeri
   1. Buat link yang akan di klik.
      ```html
       <div class="category"><a href="/kategoriGaleri">All Category</a></div>
      ```
   2. Tentukan URL nya di Route pada web.php dan jalur menggunakan Controller nya dengan function allKategori.
      ```php
      Route::get('/kategoriGaleri', [KategoriGaleriController::class, 'allKategori']);Route::get('/kategoriGaleri', [KategoriGaleriController::class, '                  allKategori']);
      ```
   3. Masuk Ke KategoriGaleriController dan arahkan akan ditampilkan ke view mana, berikan title, dan ambil data dengan menggunakan model KategoriGaleri.
      ```php
      public function allKategori()
        {
        return view('kategoriGaleri',  [
            'title' => 'Galeri',
            'judulPage' => 'Kategori Galeri',
            'kategoriGaleri' => KategoriGaleri::all()

        ]);
        }
       ```

### Membuat Fitur Search
1. Buat sebuah search pada halaman yang akan dicari.
   ```html
   <div class="search">
            <form action="/artikel">
                <div class="input-group mb-3">
                    <input type="text" class="form-control" placeholder="Cari Artikel ..." name="search">
                    <button class="btn btn-primary" type="submit" >Search</button>
                  </div>
            </form>
   </div>
   ```
2. Karena Form itu mengarah ke Artikel dan dengan name search pada input, kita cek dulu apakah ada isinya di ArtikelController
   ```php
   dd(request('search'));
   ```
3. Kalau ada kita buat fungsi di Artikel Model, nama fungsi awal harus scope
   ```php
      public function scopeFiltercoy($query)
    {
        dd(request('search'));
        if (request('search')) {
            return $query->where('judul', 'like', '%' . request('search') . '%')
                ->orWhere('artikelPost', 'like', '%' . request('search') . '%');
        }
    }
   ```
4. Jangan lupa tambahkan value, agar saat di serach masih ada teks di baris pencarian nya
   ```html
   value="{{ request('search') }}"
   '''

5. Jangan lupa tambahkan function yang sudah dibuat tadi pada Artikel Controller
   ```php
   "article" => Artikel::latest()->filtercoy()->get()
   ```

   ### Membuat Fitur Paginate
   1. Ganti get dengan Paginate dan jumlah artikel yg di inginkan
      ```php
      "article" => Artikel::latest()->filtercoy()->paginate(7)->withQueryString()
      ```
   2. Tambahkan sebuah function links untuk bisa ke page selanjutnya
      ```php
      {{ $article->links() }}
      ```
   3. Untuk Bootstrap bisa tambahkan kode dibawah ini ke folder App\Providers\AppServiceProvider
      ```php
      use Illuminate\Pagination\Paginator;
      Paginator::useBootstrapFive();
      ```
