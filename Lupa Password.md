# Fitur Lupa Password
1. Buat Route: Tentukan rute untuk menampilkan formulir lupa password dan untuk mengirim email reset password.
   ```php
    use App\Http\Controllers\ForgotPasswordController;

   // Route untuk tampil form lupa pw
   Route::get('forgot-password', [ForgotPasswordController::class, 'showForgotPasswordForm'])->name('password.request')->middleware('guest');
   
   
   // Route untuk kirim email
   Route::post('forgot-password', [ForgotPasswordController::class, 'sendResetLinkEmail'])->name('password.email')->middleware('guest');
   
   // Untuk tampil form Lupa PW
   Route::get('reset-password/{token}/{email}', [ForgotPasswordController::class, 'showResetForm'])->name('password.reset');
   
   // Untuk proses form lupa pw
   Route::post('reset-password', [ForgotPasswordController::class, 'resetPassword'])->name('password.update');

   ```

2. Buat Controller: Buat controller Anda sendiri untuk mengelola logika formulir lupa password dan pengiriman email reset.
   ```php
   php artisan make:controller ForgotPasswordController
   ```

3. Kemudian, dalam controller ForgotPasswordController, Anda akan memiliki dua metode:
   ```php
      <?php
      
      namespace App\Http\Controllers;
      
      use Illuminate\Http\Request;
      use Illuminate\Support\Facades\Password;
      use Illuminate\Support\Facades\Hash;
      use App\Models\User;
      
      class ForgotPasswordController extends Controller
      {
          public function showForgotPasswordForm()
          {
              return view('login.index');
          }
      
          public function sendResetLinkEmail(Request $request)
          {
              $validasiData = $request->validate(['email' => 'required|email']);

              // Cari pengguna dengan email yang diberikan
              $user = User::where('email', $validasiData['email'])->first();
      
              // Periksa apakah pengguna tidak memiliki kata sandi di database
              if ($user && empty($user->password)) {
                  // Jika pengguna tidak memiliki kata sandi, tampilkan pesan untuk login menggunakan Google
                  return back()->with('loginError', 'Please login using Google.');
              }
      
              $status = Password::sendResetLink(
                  $request->only('email')
              );
      
              return $status === Password::RESET_LINK_SENT
                  ? back()->with(['status' => __($status)])
                  : back()->withErrors(['email' => __($status)]);
          }
      
          public function showResetForm(Request $request, $token)
          {
              return view('login.changepw', [
                  'token' => $token,
                  'email' => $request->email,
                  'title' => 'Login'
              ]);
          }
      
          public function resetPassword(Request $request)
          {
              $request->validate([
                  'email' => 'required|email',
                  'password' => 'required|min:8|confirmed',
                  'token' => 'required'
              ]);
      
              $status = Password::reset(
                  $request->only('email', 'password', 'password_confirmation', 'token'),
                  function ($user, $password) {
                      $user->forceFill([
                          'password' => Hash::make($password)
                      ])->save();
                  }
              );
      
              switch ($status) {
                  case Password::PASSWORD_RESET:
                      return redirect()->route('login')->with('status', __('Password berhasil direset. Silakan login dengan password baru Anda.'));
                  case Password::INVALID_TOKEN:
                      return back()->withErrors(['token' => __('Token reset password tidak valid.')]);
                  case Password::INVALID_USER:
                      return back()->withErrors(['email' => __('Email tidak ditemukan.')]);
                  case Password::RESET_THROTTLED:
                      return back()->withErrors(['email' => __('Silakan tunggu sebelum mencoba lagi.')]);
                  default:
                      return back()->withErrors(['email' => __('Gagal mereset password. Silakan coba lagi.')]);
              }
          }
      }
   ```
4. Pastikan Anda telah membuat view auth.forgot-password.blade.php untuk menampilkan formulir lupa password.
5. Konfigurasi Email: Pastikan Anda telah mengatur pengaturan email di file .env, seperti yang dijelaskan sebelumnya.
   ```php
   MAIL_MAILER=smtp
   MAIL_HOST=smtp.gmail.com
   MAIL_PORT=465
   MAIL_USERNAME=habibahmad4580@gmail.com
   MAIL_PASSWORD=diambil dari buat pw di akun google nya
   MAIL_ENCRYPTION=ssl
   MAIL_FROM_ADDRESS="habibahmad4580@gmail.com"
   MAIL_FROM_NAME="Reset Password"
   ```

6. Buat View: Buat view untuk menampilkan formulir lupa password. Biasanya, view ini berisi formulir dengan satu input untuk alamat email.
   - Form Lupa Password
   ```html
   <form class="formForget" action="{{ route('password.email') }}" method="POST">
            @csrf
            <center>
                <h3 class="mt-4">Lupa Password</h3>
            </center>
            @if (session('status'))
                <div class="status-message">{{ session('status') }}</div>
            @endif
            @if ($errors->any())
                <div class="error-message">
                    @foreach ($errors->all() as $error)
                        {{ $error }}<br>
                    @endforeach
                </div>
            @endif
            <div class="forget">
                <label for="forget">Email <span class="pentingIcon">*</span></label>
                <input type="email" id="forget" name="email" placeholder="Masukan Email Anda" class="form-control"required value="{{ old('email') }}">
            </div>
            <a href="/login" class="text-decoration-none px-2 d-flex justify-content-end backLogin">Kembali Login</a>
            <div class="forgetButton">
                <button type="submit">Kirim</button>
            </div>
        </form>
   ```

   - Form Input Password Baru
   ```html
   <form class="formChangePW" action="{{ route('password.update') }}" method="POST">
            @csrf
            <center>
                <h3 class="mt-4">Ganti Password</h3>
            </center>
            @if ($errors->any())
            <div>
                <ul>
                    @foreach ($errors->all() as $error)
                        <li>{{ $error }}</li>
                    @endforeach
                </ul>
            </div>
            @endif
        
            @if (session('status'))
                <div>{{ session('status') }}</div>
            @endif
            <div class="passwordDaftar">
                <input type="hidden" name="token" value="{{ $token }}">
                <input type="hidden" name="email" value="{{ $email }}">
                <label for="passwordDaftar">Password Baru <span class="pentingIcon">*</span></label>
                <input type="password" id="passwordForget" name="password" placeholder="Masukan Password" class="form-control" required>
            </div>
            <div class="password_confirmation">
                <label for="password_confirmation">Ulangi Password <span class="pentingIcon">*</span></label>
                <input type="password" id="password_confirmationForget" name="password_confirmation" placeholder="Ketik Ulang Password" class="form-control" required>
            </div>
            <div class="forgetButton">
                <button type="submit">Ganti Password</button>
            </div>
        </form>
   ```
