# Langkah membuat Login With Google
1. Persiapkan Kredensial OAuth Google:
- Buat proyek di Console Pengembang Google. https://console.cloud.google.com/
- Dapatkan ID Klien OAuth dan Secret Klien OAuth.
2. Instalasi Google Client Library:
- Instal Google Client Library menggunakan Composer dengan menjalankan perintah composer require google/apiclient.
- composer require google/apiclient:^2.15.0
4. Tambahkan Route:
- Tambahkan rute untuk mengarahkan pengguna ke proses login Google OAuth dan mengelola callback setelah login berhasil.
- seting di env nya untuk clienid,clientsecret,dan clienturi
5. Buat Controller:
- Buat controller untuk menangani proses login dan callback dengan Google OAuth.
- Dalam controller, gunakan Google Client Library untuk berinteraksi dengan API OAuth Google.
- dibawah ini kode yang ditaruh di kontroller.
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Google\Client;
use Google\Service\Oauth2;
use App\Models\User;

class GoogleLoginController extends Controller
{
    public function index()
    {
        return view('login.index',  [
            'title' => 'Login',
        ]);
    }

    public function handleGoogleCallback(Request $request)
    {
        $client = new Client();
        $client->setClientId(env('GOOGLE_CLIENT_ID'));
        $client->setClientSecret(env('GOOGLE_CLIENT_SECRET'));
        $client->setRedirectUri(env('GOOGLE_REDIRECT_URI'));

        $code = $request->get('code');
        $token = $client->fetchAccessTokenWithAuthCode($code);

        if (!isset($token['error'])) {
            $client->setAccessToken($token['access_token']);
            $oauth = new Oauth2($client); // Perubahan pada inisialisasi objek Oauth2
            $user_info = $oauth->userinfo->get();

            // Dapatkan informasi pengguna dari Google
            $google_id = $user_info->id;
            $email = $user_info->email;
            $name = $user_info->name;
            $username = $user_info->name;

            // Periksa apakah pengguna sudah ada di database berdasarkan email
            $existingUser = User::where('email', $email)->first();

            // Jika pengguna tidak ditemukan, tambahkan pengguna baru ke dalam database
            if (!$existingUser) {
                $newUser = new User();
                $newUser->name = $name;
                $newUser->username = $username;
                $newUser->email = $email;
                $newUser->google_id = $google_id;
                $newUser->save();
            }

            return redirect('/');

            // Sekarang Anda dapat melanjutkan dengan logika autentikasi atau tindakan lainnya
        } else {
            // Jika ada kesalahan, tangani di sini
        }
    }
}

```
- jangan lupa tambahkan field baru dengan nama google_id
6. Buat View:
- Buat tampilan yang akan menampilkan tombol "Login with Google" dan mengarahkan pengguna ke proses login Google OAuth.
7. Kelola Callback:
- Setelah pengguna memberikan izin, tangani callback dari Google OAuth untuk mendapatkan informasi pengguna.
- Simpan informasi pengguna ke dalam basis data jika diperlukan.
8. Tambahkan Logika Autentikasi:
- Jika pengguna baru, buat akun pengguna di sistem Anda menggunakan informasi yang diberikan oleh Google OAuth.
- Jika pengguna sudah ada, autentikasi pengguna dan buat sesi atau token autentikasi.
- Tambahkan Fitur Logout (Opsional):
- Tambahkan fitur logout yang memungkinkan pengguna keluar dari sesi atau token autentikasi.
9. Uji dan Debug:
- Uji fitur login dengan Google OAuth untuk memastikan semuanya berfungsi seperti yang diharapkan.
- Debug jika diperlukan untuk menangani masalah atau kesalahan yang mungkin muncul.
