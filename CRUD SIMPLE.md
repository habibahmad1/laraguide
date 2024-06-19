# CRUD Simple dengan Laravel
1. Buat Route bisa dengan resource/manual.
   ```php
     //Route Manual

    //Route form new contact
    Route::get('/', function () {
    return view('homepage.index', [
        "contact" => Contact::all()
    ]);
    });
   
    //Route simpan contact
    Route::post('/contact', [ContactController::class, 'store'])->name('contact.submit');
    
    //Route delete
    Route::delete('/contact/{contact}', [ContactController::class, 'destroy'])->name('contact.destroy');
    
    // Route form edit contact
    Route::get('/contact/{name}/edit', [ContactController::class, 'edit'])->name('contact.edit');
    
    // Route update contact
    Route::put('/contact/{contact}', [ContactController::class, 'update'])->name('contact.update');
    
    // Route detail contact
    Route::get('/contact/{name}', [ContactController::class, 'show'])->name('contact.show');
   ```
2. jika ingin berdasarkan slug/nama {contact} bisa diganti dengan {slug}, samakan dengan nama parameter di Controller nya.
3. Jika ingin menggunakan slug semua bisa dengan menambahkan getRouteKeyName di Model nya atau bisa tidak usah ditambahkan.
4. Ini beberapa contoh Controller untuk menampilkan form Create dan Edit dan Delete pada Controllernya.
  ```php
    public function show(Contact $contact, $name)
      {
          $contact = Contact::where('name', $name)->firstOrFail();
          return view('homepage.detail', compact("contact"));
      }  
  ```
# Create pada Controllernya.
