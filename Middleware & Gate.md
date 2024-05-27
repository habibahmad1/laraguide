## Middleware 
1. Middleware yang sudah disediakan oleh Laravel ada beberapa yang sering digunakan yaitu, auth dan guest.
2. Auth untuk unser yang sudah login sedangkan Guest untuk yang belum Login.
3. Jika ingin buat Middleware baru bisa ctrl+shift+p dan pilih make Middleware dan beri nama.
4. Di function Handle bisa ditulis seperti ini.
   ```php
    public function handle(Request $request, Closure $next, ...$roles): Response
    {
        if (Auth::guest() || !in_array(Auth::user()->role, $roles)) {
            abort(403, 'Anda tidak memiliki izin untuk mengakses halaman ini.');
        }
        return $next($request);
    }
   ```
5. Lalu daftarkan Middleware yang telah dibuat di Kernel.php pada $middlewareAliases.
   ```php
     'role' => \App\Http\Middleware\isAdmin::class
   ```

## Gate
6. Lalu untuk buat Gate buat menghilangkan menu jika user bukan Admin pada folder Providers/AppServiceProvider.php.
   ```php
     Gate::define('admin-superadmin', function ($user) {
            return in_array($user->role, ['Admin', 'Super Admin']);
        });
   ```
7. lalu bisa digubakan menggunakan @can dan @endcan, jadi yang akan diatur siapa saja yang diatur boleh melihat menu itu.
   ```php
    @can('admin-superadmin')
          <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-white">
            <span>Administrator</span>
          </h6>

          <ul class="nav flex-column mb-auto">
            <li class="nav-item">
              <a class="nav-link d-flex align-items-center gap-2 iconColor {{ Request::is('dashboard/categories') ? 'activeSidebar' : '' }}" href="/dashboard/categories">
                <i class="fa-solid fa-border-none iconColor"></i>
                Create Category
              </a>
            </li>
            <li class="nav-item">
              <a class="nav-link d-flex align-items-center gap-2 iconColor {{ Request::is('dashboard/data') ? 'activeSidebar' : '' }}" href="/dashboard/data">
                <i class="fa-solid fa-folder iconColor"></i> 
                Data User
              </a>
            </li>
            <li class="nav-item">
              <a class="nav-link d-flex align-items-center gap-2 iconColor {{ Request::is('dashboard/lapor') ? 'activeSidebar' : '' }}" href="/dashboard/lapor">
                <i class="fa-solid fa-flag iconColor"></i> 
                Laporan 
              </a>
            </li>
          </ul>
        @endcan
   ```
8. Penggunaan pada Route di web.php
    ```php
     // Route Buat Galeri
    Route::resource('/dashboard/galeri', GaleriPostController::class)->middleware(['auth', 'member', 'role:Admin,Super Admin']);
   ```
