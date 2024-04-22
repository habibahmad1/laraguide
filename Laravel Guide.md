# Laravel Guide

## Laravel Steps
1. Model: Pertama-tama, Anda akan membuat atau menggunakan model Eloquent untuk merepresentasikan tabel atau entitas dalam database Anda. Model ini akan berfungsi untuk berinteraksi dengan data dalam tabel tersebut. Jika Anda belum memiliki model yang sesuai, Anda dapat membuatnya dengan perintah artisan make:model.
2. Migrasi: Setelah model dibuat, Anda mungkin perlu membuat atau menyesuaikan migrasi untuk tabel yang terkait. Migrasi Laravel digunakan untuk mendefinisikan struktur database, seperti membuat tabel baru atau menambahkan kolom ke tabel yang sudah ada. Anda dapat menggunakan perintah artisan make:migration untuk membuat migrasi baru.
3. Factory: Jika Anda ingin mengisi tabel dengan data sampel untuk pengembangan atau pengujian, Anda dapat membuat factory untuk model Anda. Factory digunakan untuk menghasilkan data palsu yang dapat Anda gunakan untuk mengisi tabel dengan data sampel. Anda dapat menggunakan perintah artisan make:factory untuk membuat factory baru.
4. Controller: Setelah model Anda dan mungkin juga factory dan migrasi telah dibuat atau disesuaikan, Anda akan membuat controller yang bertanggung jawab untuk menangani logika bisnis dan menyiapkan data yang diperlukan untuk ditampilkan di halaman Anda. Controller ini akan berinteraksi dengan model untuk mengambil data dari database dan mengirimkannya ke tampilan. Anda dapat menggunakan perintah artisan make:controller untuk membuat controller baru.
5. View: Terakhir, Anda akan membuat tampilan (view) yang akan menampilkan data yang diambil dari database. Tampilan ini akan mengambil data yang dikirimkan oleh controller dan menampilkannya sesuai dengan kebutuhan Anda. Anda dapat membuat tampilan dengan menggunakan Blade, template engine bawaan Laravel, yang memudahkan Anda untuk memasukkan data dinamis ke dalam tampilan.
Setelah langkah-langkah di atas selesai, Anda kemungkinan akan mengatur rute (route) di Laravel untuk mengarahkan permintaan HTTP yang masuk ke halaman yang tepat pada controller Anda. Dengan begitu, ketika pengguna mengakses URL tertentu, controller akan menyiapkan data yang diperlukan dan mengarahkan ke tampilan yang sesuai untuk ditampilkan kepada pengguna.

## Relationship Table

Jika Anda ingin menambahkan relasi antara dua tabel di Laravel, Anda perlu melakukan beberapa langkah:

Tambahkan Foreign Key pada Migrasi: Pertama, Anda perlu menambahkan foreign key pada migrasi tabel yang ingin Anda relasikan. Misalnya, jika Anda ingin menambahkan relasi antara tabel artikel dan tabel galeri, Anda harus menambahkan kolom galeri_id pada tabel artikel.Contoh migrasi untuk menambahkan kolom galeri_id pada tabel artikel:
php
Copy code
Schema::table('artikel', function (Blueprint $table) {
    $table->unsignedBigInteger('galeri_id')->nullable();
    $table->foreign('galeri_id')->references('id')->on('galeri');
});
Dalam contoh ini, galeri_id adalah foreign key yang akan merujuk ke kolom id pada tabel galeri.
Definisikan Relasi di Model: Setelah menambahkan foreign key dalam migrasi, Anda perlu mendefinisikan relasi antara model-model terkait. Dalam kasus ini, Anda akan mendefinisikan relasi antara model Artikel dan model Galeri.Contoh definisi relasi di model Artikel:

class Artikel extends Model
{
    public function galeri()
    {
        return $this->belongsTo(Galeri::class);
    }
}
Contoh definisi relasi di model Galeri:

class Galeri extends Model
{
    public function artikel()
    {
        return $this->hasMany(Artikel::class);
    }
}
Dalam contoh ini, kita mendefinisikan relasi belongsTo pada model Artikel yang menunjukkan bahwa setiap artikel dimiliki oleh satu galeri. Sedangkan pada model Galeri, kita mendefinisikan relasi hasMany yang menunjukkan bahwa satu galeri dapat memiliki banyak artikel.
Dengan menambahkan relasi ini, Anda dapat dengan mudah mengakses data terkait dari kedua model. Misalnya, Anda dapat mengambil galeri yang terkait dengan sebuah artikel dengan menggunakan $artikel->galeri, atau mengambil semua artikel yang terkait dengan sebuah galeri dengan menggunakan $galeri->artikel.


