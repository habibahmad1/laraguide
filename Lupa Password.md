# Fitur Lupa Password
1. Buat Route: Tentukan rute untuk menampilkan formulir lupa password dan untuk mengirim email reset password.
   ```php
    use App\Http\Controllers\ForgotPasswordController;
    Route::get('forgot-password', [ForgotPasswordController::class, 'showForgotPasswordForm'])->name('password.request')->middleware('guest');
    Route::post('forgot-password', [ForgotPasswordController::class, 'sendResetLinkEmail'])->name('password.email')->middleware('guest');
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
    
    class ForgotPasswordController extends Controller
    {
        public function showForgotPasswordForm()
        {
            return view('auth.forgot-password');
        }
    
        public function sendResetLinkEmail(Request $request)
        {
            $request->validate(['email' => 'required|email']);
            
            $status = Password::sendResetLink(
                $request->only('email')
            );
    
            return $status === Password::RESET_LINK_SENT
                        ? back()->with(['status' => __($status)])
                        : back()->withErrors(['email' => __($status)]);
        }
    }
   ```
4. Pastikan Anda telah membuat view auth.forgot-password.blade.php untuk menampilkan formulir lupa password.
5. Konfigurasi Email: Pastikan Anda telah mengatur pengaturan email di file .env, seperti yang dijelaskan sebelumnya.
   ```php
    MAIL_MAILER=smtp
    MAIL_HOST=smtp.gmail.com
    MAIL_PORT=587
    MAIL_USERNAME=your_email@gmail.com
    MAIL_PASSWORD=your_gmail_password
    MAIL_ENCRYPTION=tls
    MAIL_FROM_ADDRESS=your_email@gmail.com
    MAIL_FROM_NAME="${APP_NAME}"
   ```

6. Buat View: Buat view untuk menampilkan formulir lupa password. Biasanya, view ini berisi formulir dengan satu input untuk alamat email.
