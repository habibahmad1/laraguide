# Redeem Code Feature
1. Buat view di html
   ```html
    @extends('./dashboard/layouts/main')

    @section('content')
    <center>
       <h2>Redeem Code</h2> 
    </center>
        <div class="row">
            <div class="col-md-6 mx-auto justifiy-content-center">
                    {{-- Alert Success Register --}}
                @if (session()->has('success'))
                <div class="alert alert-success alert-dismissible fade show" role="alert">
                    <strong>{{ session('success') }}</strong>
                    <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                </div>
                @endif

            {{-- Alert Success Register --}}
            @if (session()->has('error'))
            <div class="alert alert-danger alert-dismissible fade show" role="alert">
                <strong>{{ session('error') }}</strong>
                <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
            </div>
            @endif 
        </div>
    </div>
    


    <form class="container-reedem" action="{{ route('reedem') }}" method="POST">
        @csrf
        <label for="code">Masukan Kode Redeem</label>
        <input type="text" id="code" name="code" required>
        <button class="btn btn-primary my-3 px-5" type="submit" >Redeem</button>
    </form>

    @endsection
   ```
2. Buat model, migrasi, controller, dan factory jika ingin gunakan seeder.
  - Model untuk buat relasi
   ```php
      <?php

      namespace App\Models;
      
      use Illuminate\Database\Eloquent\Factories\HasFactory;
      use Illuminate\Database\Eloquent\Model;
      
      class Reedem extends Model
      {
          use HasFactory;
      
          protected $guarded = ['id'];
      
          public function user()
          {
              return $this->belongsTo(User::class, 'user_id');
          }
      }

   ```

  - Migrasi untuk buat table.
  ```php
     Schema::create('reedems', function (Blueprint $table) {
            $table->id();
            $table->foreignId('user_id');
            $table->string('code')->unique();
            $table->integer('duration');
            $table->integer('kuota')->default(1);
            $table->boolean('status_redeemed')->default(false);
            $table->timestamps();
        });
  ```

  - Controller untuk logika
  ```php
    public function index()
    {
        return view('dashboard.reedem');
    }

    public function reedem(Request $request)
    {
        $request->validate([
            'code' => 'required|string',
        ]);

        $code = $request->input('code');

        $redeemCode = Reedem::where('code', $code)->first();

        if (!$redeemCode) {
            return redirect()->back()->with('error', 'Kode redeem tidak valid');
        }

        $user = auth()->user();

        if ($redeemCode->user_id === $user->id) {
            return redirect()->back()->with('error', 'Anda sudah melakukan redeem untuk kode ini');
        }

        if ($redeemCode->kuota <= 0) {
            return redirect()->back()->with('error', 'Kuota redeem untuk kode ini sudah habis');
        }

        // Mengurangi kuota sebelum menyimpan perubahan
        $redeemCode->kuota--;
        $redeemCode->user_id = $user->id;
        $redeemCode->save();

        return redirect()->back()->with('success', 'Kode redeem berhasil digunakan');
    }
 ```

3. buat 2 route di web.php, 1 untuk view dan 1 untuk form
   ```php
    // Route untuk Redem Code
    Route::get('/dashboard/reedem', [ReedemController::class, 'index'])->middleware('auth');
    
    // Route untuk Form Redeem Code
    Route::post('/dashboard/reedem', [ReedemController::class, 'reedem'])->name('reedem')->middleware('auth');
   ```
  
