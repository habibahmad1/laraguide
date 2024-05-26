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
          <a href="/dashboard/artikel/create" class="btn btn-primary">Buat Galeri</a>
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
                  <a href="/dashboard/galeri/{{ $galeri->slug }}" class="badge bg-info"><i class="fa-solid fa-eye text-white"></i></a>
                  <a href="/dashboard/galeri/{{ $galeri->slug }}/edit" class="badge bg-success"><i class="fa-solid fa-pencil"></i></a>
                  <form action="/dashboard/galeri/{{ $galeri->slug }}" method="POST" class="d-inline">
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
