# User Register Guide Laravel
1. Buat Form untuk daftar user.
   ```html
     {{-- Form Daftar --}}
        <div class="gambarKanan">
            <img src="./img/imgDaftar.jpg" alt="imgLogin">
        </div>
        <form class="form-daftar" action="/register" method="POST">
            @csrf
            <center>
                <h3 class="mt-4">Daftar Buat Akun</h3>
            </center>
            <div class="namaDaftar">
                <label for="namaDaftar">Nama Lengkap <span class="pentingIcon">*</span></label>
                <input type="text" id="namaDaftar" name="name" placeholder="Masukan Nama Lengkap" class="form-control @error('name')is-invalid @enderror" value="{{ old('name') }}">
                @error('name')
                    <div class="invalid-feedback">
                        {{ $message }}
                    </div>
                @enderror
            </div>
            <div class="username">
                <label for="username">Username <span class="pentingIcon">*</span></label>
                <input type="text" id="username" name="username" placeholder="Masukan Username" class="form-control @error('username')is-invalid @enderror"  value="{{ old('username') }}">
                @error('username')
                    <div class="invalid-feedback">
                        {{ $message }}
                    </div>
                @enderror
            </div>
            <div class="emailDaftar">
                <label for="email">Email <span class="pentingIcon">*</span></label>
                <input type="email" id="emailDaftar" name="email" placeholder="Masukan Email" class="form-control @error('email')is-invalid @enderror"  value="{{ old('email') }}">
                @error('email')
                    <div class="invalid-feedback">
                        {{ $message }}
                    </div>
                @enderror
            </div>
            <div class="noHp">
                <label for="noHp">No.Hp <span class="pentingIcon">*</span></label>
                <input type="number" id="noHp" name="noHp" placeholder="Nomor Telepon" class="form-control @error('noHp')is-invalid @enderror" value="{{ old('noHp') }}">
                @error('noHp')
                    <div class="invalid-feedback">
                        {{ $message }}
                    </div>
                @enderror
            </div>
            <div class="passwordDaftar">
                <label for="passwordDaftar">Password <span class="pentingIcon">*</span></label>
                <input type="password" id="passwordDaftar" name="password" placeholder="Masukan Password" class="form-control @error('password')is-invalid @enderror">
                @error('password')
                    <div class="invalid-feedback">
                        {{ $message }}
                    </div>
                @enderror
            </div>
            <div class="password_confirmation">
                <label for="password_confirmation">Ulangi Password <span class="pentingIcon">*</span></label>
                <input type="password" id="password_confirmation" name="password_confirmation" placeholder="Ketik Ulang Password" class="form-control @error('password_confirmation')is-invalid @enderror">
                @error('password_confirmation')
                    <div class="invalid-feedback">
                        {{ $message }}
                    </div>
                @enderror
            </div>
            <div class="daftarButton">
                <button type="submit">Daftar</button>
            </div>
            <div class="loginSlider">
                <p>Klik Jika Sudah Punya Akun</p>
                <div class="sliderDaftar">
                    <div class="bolaDaftarSlider"></div>
                </div>
            </div>
        </form>
   ```
- Beberapa penjelasan, oastikan register menggunakan method = "POST"
- @CSRF untuk keamanan jalur di saat user klik daftar.
- @error('name')is-invalid @enderror" menampilkan tanda merah eror, sesuaikan dengan nama name nya di input.
- ```html
  @error('password_confirmation')
   <div class="invalid-feedback">
   {{ $message }}
   </div>
  @enderror
  ```
- Kode diatas untuk menampilkan pesan error.
2. Buat Route nya di web.php
  ```php
  // Route untuk Daftar dengan Menggunakan Krontroller bukan Clousure
  Route::post('/register', [RegisterController::class, 'store']);
  ```
3. Buat Controller nya untuk jalur route dan validasi form
   ```php
   //Sesuaikan nama validasi nya dengan name pada input di form
    public function store(Request $request)
    {
        $request->validate([
            'name' => 'required|max:255',
            'username' => ['required', 'unique:users'],
            'email' => 'required|email:dns|unique:users',
            'noHp' => 'required|regex:/^[0-9]{10,}$/',
            'password' => 'required|min:8',
            'password_confirmation' => 'required|min:8|same:password',
        ]);

        dd("Berhasil");
    }
   ```
