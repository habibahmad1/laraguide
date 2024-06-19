# CRUD Simple dengan Laravel

# Read pada Controller
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
2. jika ingin berdasarkan slug/nama, {contact} bisa diganti dengan {slug}, samakan dengan nama parameter di Controller nya.
3. Jika ingin menggunakan slug semua bisa dengan menambahkan getRouteKeyName di Model nya atau bisa tidak usah ditambahkan.
4. Perlu di ingan jika sudah pakai getRouteKeyName maka semua route harus berdasarkan slug tersebut.
5. ```php
      public function getRouteKeyName()
      {
         return 'slug';
      }
      
   ```
6. Ini beberapa contoh Controller untuk menampilkan detail setiap contact.
   ```php
    public function show(Contact $contact, $name)
      {
          $contact = Contact::where('name', $name)->firstOrFail();
          return view('homepage.detail', compact("contact"));
      }  
   ```
7. Fungsi compact sama dengan seperti "contact" = $contact.
8. Ini untuk menampilkan form edit.
   ```php
   public function edit(Contact $contact, $name)
    {
        $contact = Contact::where('name', $name)->firstOrFail();
        return view('homepage.edit', compact("contact"));
    }
   ```

# Create pada Controllernya.
   ```php
      public function store(Request $request)
    {
        $dataContact = $request->validate([
            "name" => "required|string|max:255",
            "email" => "required|email",
            "message" => "required|max:255"
        ]);

        Contact::create($dataContact);

        return redirect()->back()->with('success', 'Your message has been sent successfully!');
    }
   ```
1. Diatas menggunakan validasi manual di Controllernya.

# Update pada Controller
   ```php
   public function update(UpdateContactRequest $request, Contact $contact)
    {
        $contact->update($request->validated());

        return redirect('/')->with('success', 'updated successfully');
    }
   ```
1. Update ini memisahkan validasi nya di Http/Request/StoreContactRequest.php
2. Jadi tinggal update saja karena sudah divalidasi.

# Delete pada Controller
   ```php
   public function destroy(Contact $contact)
    {
        $contact->delete();

        return redirect()->back()->with('success', 'Deleted Successfull');
    }
   ```

# View pada Semua form
   ```html
   <a href="{{ route('contact.show', $item->name) }}">detail</a>
   ```
1. Untuk melihat setiap detail contact.
2. Untuk edit setiap contact.
   ```html
   <a href="{{ route('contact.edit', $item->name) }}">edit</a>
   ```
3. Untuk Delete.
   ```html
         <form action="{{ route('contact.destroy', $item->id) }}" method="POST">
         @method('delete')
         @csrf
         <button type="submit" onclick="return confirm('delete?')">Delete</button>
         </form>
         ```
      4. Untuk update
         ```html
         {{ route('contact.update', $contact->id) }}
         ```
      5. Kode diatas menggunakan route untuk url dan data dari mana yang ingin di lihat/edit/update dengan menambahkan berdasarkan apa.  Misal di Route nya berdasarkan nama maka $item->name.
      6. Jika berdasarkan {contact} maka itu berdasarkan id secara default jadi $item->id.
      7. Contoh View Form Edit ,
         ```html
         @extends('partials.main')
      @section('content')
      <div class="contact" id="contact">
      <h1 class="titleIntro">Edit Contact</h1>
      
          @if (session('success'))
              <div>{{ session('success') }}</div>
          @endif
      
          <form action="{{ route('contact.update', $contact->id) }}" method="POST">
              @csrf
              @method('PUT')
              <div class="fullname">
                  <label for="name">Name:</label>
                  <input type="text" id="name" name="name" value="{{ old('name', $contact->name) }}">
                  @error('name')
                      <div>{{ $message }}</div>
                  @enderror
              </div>
      
              <div class="email">
                  <label for="email">Email:</label>
                  <input type="text" id="email" name="email" value="{{ old('email', $contact->email) }}">
                  @error('email')
                      <div>{{ $message }}</div>
                  @enderror
              </div>
      
              <div class="message">
                  <label for="message">Message:</label>
                  <textarea id="message" name="message">{{ old('message', $contact->message) }}</textarea>
                  @error('message')
                      <div>{{ $message }}</div>
                  @enderror
              </div>
      
              <button type="submit">Update</button>
          </form>
      </div>
      @endsection
   ```

# Message Success & Error

1. Untuk menampilkan pesan Sukses saat mengedit data.
   ```php
      @if (session('success'))
              <div>{{ session('success') }}</div>
      @endif
   ```
2. Untuk menampilkan Eror dari validasi form edit.
   ```php
      @error('message')
         <div>{{ $message }}</div>
      @enderror
   ```
    
