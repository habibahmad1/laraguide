## Jika tidak bisa edit atau hapus
Bisa tambahkan ini pada file Appserviceprovider.php pada function boot

```php
use App\Models\KategoriArtikel;
use Illuminate\Support\Facades\Route;

Route::model('kategoriartikel', KategoriArtikel::class);
```

Itu diambil dari dalam kurung Route

```php
Route::get('/dashboard/kategoriartikel/{kategoriartikel}', [KategoriArtikelController::class, 'show']);
```

lalu dengan Model nya
