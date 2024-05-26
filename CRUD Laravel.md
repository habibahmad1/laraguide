# CRUD GUIDE LARAVEL

## Read Feature
1. Buat sebuah controller dengan ctrl+shift+p dan pilih yang resource dan pilih model yang ingin terhubung contoh disini Galeri.
2. Buat Routenya dan arahkan ke Controller.
   ```php
     // Route Buat Galeri
    Route::resource('/dashboard/galeri', GaleriPostController::class);
   ```
3. Buat View nya untuk tampilan route itu.
   ```html
   @extends('./dashboard.layouts.main')
      @section('content')
      <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
          <h1 class="h2">My Galeri</h1>
        </div>
       
        <div class="table-responsive small col-lg-8">
          @if(session()->has('success')) 
          <div class="alert alert-success" role="alert">
            {{ session('success') }}
          </div>  
        @endif
          <a href="/dashboard/galeri/create" class="btn btn-primary">Buat Galeri</a>
          <table class="table table-warning table-striped table-sm text-center">
            <thead>
              <tr class="table-primary">
                <th scope="col">No.</th>
                <th scope="col">Judul</th>
                <th scope="col">Kategori</th>
                <th scope="col">Aksi</th>
              </tr>
            </thead>
            <tbody>
              @foreach ($dataGaleri as $galeri)
                  
              <tr>
                <td>{{ $loop->iteration }}</td>
                <td>{{ $galeri->judul }}</td>
                <td>{{ $galeri->kategoriGaleri->nama }}</td>
                <td>
                  <a href="/dashboard/galeri/{{ $galeri->id }}" class="badge bg-info"><i class="fa-solid fa-eye text-white"></i></a>
                  <a href="/dashboard/galeri/{{ $galeri->id }}/edit" class="badge bg-success"><i class="fa-solid fa-pencil"></i></a>
                  <form action="/dashboard/galeri/{{ $galeri->id }}" method="POST" class="d-inline">
                    @method('delete')
                    @csrf
                    <button class="badge bg-danger border-0" onclick="return confirm('Yakin Mau Hapus?')"><i class="fa-solid fa-trash-can"></i></button>
                  </form>
                </td>
              </tr>
              @endforeach
            </tbody>
          </table>
        </div>
      @endsection
   ```
4. Panggil fungsi untuk diarahkan ke view nya di index otomaris jika pertama untuk view.
   ```php
     return view('dashboard.galeri.index', [
            'dataGaleri' => Galeri::where('user_id', auth()->user()->id)->get()
        ]);
   ```
5. Perthatikan untuk ke Create kita menggunakan button Buat Galeri pada view diatas.

## Create Feature
1. Karena sudah menggunakan Resource pada Route maka tidak usah buat lagi, langsung isi saja pada function create di Controller yang sudah dibuat tadi.
   ```php
      public function create()
       {
           return view('dashboard.galeri.create', [
               'data' => KategoriGaleri::all()
           ]);
       }
   ```
2. Buat view nya pada folder dashboard/galeri/create.blade.php.
   ```php
   @extends('dashboard.layouts.main')
      @section('content')
      
      <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
          <h1 class="h2">Buat Galeri Baru</h1>
      </div>
      
      <div class="col-lg-7">
          <form action="/dashboard/galeri" method="POST" enctype="multipart/form-data">
              @csrf
              <div class="mb-3">
                <label for="judul" class="form-label">Judul Galeri</label>
                <input type="text" class="form-control @error('judul')
                  is-invalid
                @enderror" id="judul" name="judul" required autofocus value="{{ old('judul') }}">
                @error('judul')
                  <div class="invalid-feedback">
                    {{ $message }}
                  </div>
                @enderror
              </div>
      
              <div class="mb-3">
                <label for="kategoriGaleri_id" class="form-label">Kategori Artikel</label>
                <select class="form-select" name="kategoriGaleri_id">
                  @foreach ($data as $item)
                  <option value="{{ $item->id }}" {{ old('kategoriGaleri_id') == $item->id ? 'selected' : '' }}>{{ $item->nama }}</option>
                  @endforeach
                </select>
              </div>
      
              <div class="mb-3">
                <label for="img" class="form-label">Gambar Galeri</label>
                <img class="img-preview img-fluid mb-3 col-sm-5">
                <input class="form-control @error('img')
                is-invalid
              @enderror" type="file" id="img" name="img" onchange="previewImage()">
              @error('img')
                  <div class="invalid-feedback">
                    {{ $message }}
                  </div>
                @enderror
              </div>
      
              <button type="submit" class="btn btn-primary mb-5">Buat Galeri</button>
          </form>
      </div>
      
      
      <script>
          const judul = document.querySelector('#judul');
          const slug = document.querySelector('#slug');
      
          judul.addEventListener("change", function (){
              fetch('/dashboard/artikel/cekSlug?judul=' + judul.value)
              .then(response => response.json())
              .then(data => slug.value = data.slug)
          });
      
          document.addEventListener('trix-file-accept',(e)=>{
              e.prevendDefault();
          })
      
          function previewImage(){
            const image = document.querySelector('#img');
            const imagePreview = document.querySelector('.img-preview');
            
            imagePreview.style.display ="block";
      
            const ofReader = new FileReader();
      
            ofReader.readAsDataURL(image.files[0]);
      
            ofReader.onload = function (ofRevent){
              imagePreview.src = ofRevent.target.result;
            }
      
          }
      </script>
      @endsection
   ```
3. Banyak fitur di view tersebut, untuk slug bisa baca Slug pada artikel lain disini.
4. PreviewImage menggunakan Javascript dan fitur Trix untuk kolom isi Artikel/Deskripsi yang bisa baca pada artikel lain disini.
5. Pastikan nama NAME nya sesuai dengan nama di Database agar bisa langsung masuk ke database.
6. kemudian untuk menyimpannya ketika klik button Buat Galeri maka akan menuju form pada url yang akan dikirim ke controller Store untuk lakukan Validasi agar bisa di Insert ke Database.

## Insert Feature
1. Tulis di controller Store yang sudah disediakan untuk insert ke Database, jadi tidak perlu buat Route lagi.
2. Tulis ini dulu di Git Bash/CMD agar folder Storage bisa diakses di public, dengan Symlink .
   ```php
      php artisan storage:link
   ```
   ```php
      public function store(Request $request)
       {
           $validasiData = $request->validate([
               'judul' => 'required|max:255',
               'img' => 'image|file|max:2048',
               'kategoriGaleri_id' => 'required',
   
           ]);
   
           if ($request->file('img')) {
               $validasiData['img'] = $request->file('img')->store('galeri-img');
           }
           $validasiData['user_id'] = auth()->user()->id;
   
           Galeri::create($validasiData);
   
           return redirect('/dashboard/galeri')->with('success', 'Berhasil Menambahkan Galeri');
       }
    ```
3. Pastikan nama di $validasiData sesuai dengan nama Name di view nya dan Database.
4. Pada kode ini akan membuat folder baru yang sebelumnya sudah kita buat Symlink.
   ```php
      if ($request->file('img')) {
          $validasiData['img'] = $request->file('img')->store('galeri-img');
       }
   ```

## Read Setiap Galeri yang dibuat
1. Tidak usah buat Route masih gunakan yang disediakan karena pakai Resource, Tambahkan pada function Show.
   ```php
      public function show(Galeri $galeri)
       {
           return view('dashboard.galeri.detail', [
               'galeri' => $galeri
           ]);
       }
   ```
2. Buat View untuk menampilkan tiap Galeri yang dipilih.
   ```html
      @extends('./dashboard.layouts.main')

      @section('content')
      
      <div class="kanvasAll">
          <article class="setiapArtikel">
              
              <h2>{{ $galeri->judul }}</h2>
      
              <a href="/dashboard/galeri/{{ $galeri->slug }}/edit" class="badge bg-success py-2 text-decoration-none"><i class="fa-solid fa-pencil"></i> Edit</a>
              <form action="/dashboard/galeri/{{ $galeri->slug }}" method="POST" class="d-inline">
                  @method('delete')
                  @csrf
                  <button class="badge bg-danger border-0 py-2" onclick="return confirm('Yakin Mau Hapus?')"><i class="fa-solid fa-trash-can"></i> Delete</button>
                </form>
      
              @if ($galeri->image)
      
                  <div class="text-center gambarTiapPost mb-4 mt-1">
                      <img src="{{ asset('storage/' . $galeri->img) }}"  alt="imgPost" class="rounded mb-3">
                  </div> 
              
              @else
                  <div class="text-center gambarTiapPost mb-4 mt-1">
                      <img src="https://source.unsplash.com/1200x400?{{ $galeri->kategoriGaleri->nama }}"  alt="imgPost" class="rounded mb-3">
                  </div> 
              @endif
      
              <h6 class="fw-bold mb-3">Upload :<a href="/authors/{{ $galeri->user->username }}" style="color: #41a77e" class="text-decoration-none"> {{ $galeri->user->name }} </a>                         
                                  
                  <div class="badge text-bg-danger"><a href="/categories/{{ $galeri->kategoriGaleri->id }}" class="text-white text-decoration-none">{{ $galeri->kategoriGaleri->nama }}</a></div>
                  <a class="text-info text-decoration-none">{{ $galeri->created_at->diffForHumans() }}</a>
                  </h6> 
      
              <p>{!! $galeri->galeriPost !!}</p>
              <a href="/dashboard/galeri" class="kembaliButton my-5"><i class="fa-solid fa-arrow-left-long"></i> Back</a>
          </article>
      </div>
      
      @endsection
   ```
