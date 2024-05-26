# CRUD GUIDE LARAVEL

## Read Feature
1. Buat sebuah controller dengan ctrl+shift+p dan pilih yang resource dan pilih model yang ingin terhubung contoh disini Galeri.
2. Buat Routenya dan arahkan ke Controller.
   ```php
     // Route Buat Galeri
    Route::resource('/dashboard/galeri', GaleriPostController::class);
   ```
3. Buat View nya untuk tampilan route itu.
4. Panggil fungsi untuk diarahkan ke view nya di index otomaris jika pertama untuk view.
   ```php
     return view('dashboard.galeri.index', [
            'dataGaleri' => Galeri::where('user_id', auth()->user()->id)->get()
        ]);
   ```
