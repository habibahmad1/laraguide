# User Register and Login Guide Laravel
## Register User
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
- Beberapa penjelasan, pastikan register menggunakan method = "POST"
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
           $prosesValidasi = $request->validate([
               'name' => 'required|max:255',
               'username' => ['required', 'unique:users'],
               'email' => 'required|email:dns|unique:users',
               'noHp' => 'required|regex:/^[0-9]{10,}$/',
               'password' => 'required|min:8',
               'password_confirmation' => 'required|min:8|same:password',
           ]);
   
           User::create($prosesValidasi);
   
           // $request = session();
           // $request->flash('success', 'Registration successfull! Please login');
   
           return redirect('/login')->with('success', 'Registration successfull! Please login');
       }
   ```
   - with untuk menampilkan pesan setelah berhasil register.
   - user create untuk langsung insert ke database, pastikan nama validasi sama dengan nama di field database.
   
4. Tambah kan kode ini di bagian form Login untuk tampil pesan sukses register
      ```html
       {{-- Alert Success Register --}}
            @if (session()->has('success'))
                <div class="alert alert-success alert-dismissible fade show" role="alert">
                    <strong>{{ session('success') }}</strong>
                    <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                  </div>
            @endif
      ```
## Login User
1. Buat Form Daftar pada html.
   ```html
   <form class="form-login" action="/login" method="POST">
            @csrf
            {{-- Alert Success Register --}}
            @if (session()->has('success'))
                <div class="alert alert-success alert-dismissible fade show" role="alert">
                    <strong>{{ session('success') }}</strong>
                    <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                  </div>
            @endif

            @if (session()->has('loginError'))
                <div class="alert alert-danger alert-dismissible fade show" role="alert">
                    <strong>{{ session('loginError') }}</strong>
                    <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                  </div>
            @endif

            <center>
                <h3>Masuk Ke Akun Anda</h3>
            </center>
            <div class="email">
                <label for="email">Email <span class="pentingIcon">*</span></label>
                <input type="email" id="email" name="email" placeholder="Masukan Email" required autofocus class="@error('email') is-invalid @enderror" value="{{ old('email') }}">
                @error('email')
                <div class="invalid-feedback">
                    {{ $message }}
                </div>
                @enderror
            </div>
            <div class="password">
                <label for="password">Password <span class="pentingIcon">*</span></label>
                <input type="password" id="password" name="password" placeholder="Masukan Password" required>
                <span class="iconEye" style="cursor: pointer"><i class="fa-solid fa-eye-slash"></i></span>
            </div>
            <div class="cekBox">
                <input type="checkbox" name="remember"> Ingat Saya
                <a href="" class="forgotPw">Lupa Password?</a>
            </div>
            <div class="loginButton">
                <button type="submit">Login</button>
            </div>
            <div class="googleLogin"><a href="https://accounts.google.com/o/oauth2/v2/auth?scope=email%20profile&redirect_uri=http%3A%2F%2F127.0.0.1%3A8000%2Fgooglelogin&response_type=code&client_id=609385636534-lhdf545kp0eafo508hv1adgn2114k3rj.apps.googleusercontent.com&access_type=offline" class="text-decoration-none"><i class="fa-brands fa-google"></i> Login With Google</a>
            </div>
            <div class="daftarSlider">
                <p>Klik Untuk Daftar Akun </p>
                <div class="slider">
                    <div class="bolaSlider"></div>
                </div>
            </div>
        </form>
   ```
   - Atur action ke arah mana saat user tekan button login, dan buat method di form nya jadi POST.
   - Atur @csrf untuk keamanan
   - tambahkan pesan eror di email ketika ada salah di validasi.

3. Buat Route nya di Web.php, jangan lupa pakai POST
   ```php
   // Route untuk Login Form
   Route::post('/login', [LoginController::class, 'authenticate']);
   ```
4. Tambahkan validasi di function pada Controller Login yang sudah ditentukan yaitu, authenticate.
   ```php
   use Illuminate\Support\Facades\Auth;
   
   public function authenticate(Request $request)
    {
        $validasiData = $request->validate([
            'email' => 'required|email:dns',
            'password' => 'required'
        ]);

        $remember = $request->has('remember') ? true : false;

        if (Auth::attempt($validasiData, $remember)) {
            $request->session()->regenerate();
            return redirect()->intended('/dashboard');
        }

        return back()->with('loginError', 'Login Failed!');
    }
   ```
   - jangan lupa tambahkan namespace Auth.
   - menggunakan remember me

## Fitur Middleware
Memfilter jika user sudah login maka tidak bisa lihat halaman login.
1. Tambahkan pada Route yang tidak ingin muncul ketika sudah login
   ```php
   // Route ke Login
   Route::get('/login', [LoginController::class, 'index'])->name('login')->middleware('guest');

   // Route untuk Dashboard
   Route::get('/dashboard', [DashboardController::class, 'index'])->middleware('auth');;
   ```
   - Kode diatas hanya jika user belum login yang bisa mengaksesnya.
   - Jika guest artinya hanya user yang belum login bisa akses, kalau auth yang sudah login yang boleh mengakses itu url.
   - Jika user memaksa ketik manual dashboard maka akan di redirect kembali ke name('login') yang sudah kita buat pada route login

2. Ubah mau diarahkan kemana jika user masih tetap klik login misal sudah login
   ```php
   public const HOME = '/';
   ```
   - Ubah di App/Providers/RouteServiceProvider.php
3. Atur Ketika sudah login Navbar tampilan login hilang dan diganti dengan logout
   ```html
   @auth
          <center>
            <!-- Example single danger button -->
            <li class="nav-item dropdown loginAuth d-block">
              <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                Hi, {{ auth()->user()->name }}
              </a>
              <ul class="dropdown-menu text-center">
                <li><a class="dropdown-item" href="/dashboard"><i class="fa-solid fa-book"></i> My Dashboard</a></li>
                <li><hr class="dropdown-divider"></li>
                <li>
                  <form action="/logout" method="POST">
                    @csrf
                    <button type="submit" class="dropdown-item"><i class="fa-solid fa-arrow-right-from-bracket"></i> Logout</button>
                  </form>
                </li>
              </ul>
            </li>
          </center>
          @else
            <li class="nav-item">
              <a class="nav-link login {{ ($title === "Login") ? 'active' : '' }}" href="/login">Login</a>
            </li>
          @endauth
   ```

## Logout User
1. Buat Route di web.php
   ```php
   // Route untuk Logout Form
   Route::post('/logout', [LoginController::class, 'logout']);
   ```
   - masih menggunakan COntroller Login tapi dengan function logout
2. Tampilan dibuat jika sudah login maka baru tampil logout
   ```html
   @auth
          <center>
            <!-- Example single danger button -->
            <li class="nav-item dropdown loginAuth d-block">
              <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                Hi, {{ auth()->user()->name }}
              </a>
              <ul class="dropdown-menu text-center">
                <li><a class="dropdown-item" href="/dashboard"><i class="fa-solid fa-book"></i> My Dashboard</a></li>
                <li><hr class="dropdown-divider"></li>
                <li>
                  <form action="/logout" method="POST">
                    @csrf
                    <button type="submit" class="dropdown-item"><i class="fa-solid fa-arrow-right-from-bracket"></i> Logout</button>
                  </form>
                </li>
              </ul>
            </li>
          </center>
          @else
            <li class="nav-item">
              <a class="nav-link login {{ ($title === "Login") ? 'active' : '' }}" href="/login">Login</a>
            </li>
          @endauth
   ```
3. Buat function logout di LoginController
   ```php
       public function logout(Request $request)
       {
           Auth::logout();
   
           $request->session()->invalidate();
   
           $request->session()->regenerateToken();
   
           return redirect('/');
       }
   ```
