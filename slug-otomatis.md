# Auto Slugable
1. Pergi Ke Repositori nya.
   ```html
   https://github.com/cviebrock/eloquent-sluggable
   ```
2. Instal Via Composer Git Bash/CMD.
   ```php
   composer require cviebrock/eloquent-sluggable
   ```
3. Tambahkan kode ini pada model Artikel, Sesuaikan source pada nama database ingin di ambil darimana slug nya, conoth ini dari judul.
   ```php
   use Cviebrock\EloquentSluggable\Sluggable;

    class Artikel extends Model
    {
        use Sluggable;
    
        /**
         * Return the sluggable configuration array for this model.
         *
         * @return array
         */
        public function sluggable(): array
        {
            return [
                'slug' => [
                    'source' => 'judul'
                ]
            ];
        }
    }
   ```
4. Tambahkan pada bagian bawah html,
   ```html
      <script>
       const judul = document.querySelector('#judul');
       const slug = document.querySelector('#slug');
   
       judul.addEventListener("change", function (){
           fetch('/dashboard/artikel/cekSlug?judul=' + judul.value)
           .then(response => response.json())
           .then(data => slug.value = data.slug)
       });
      </script>
   ```

5. Buat method baru di controller dashboard
   ```php
      public function cekSlug(Request $request)
       {
           $slug = SlugService::createSlug(Artikel::class, 'slug', $request->judul);
           return response()->json(['slug' => $slug]);
       }
   ```
6. Jangan lupa buat Route nya letakan sebelum route dashboard:resource.
   ```php
      // Route untuk slug
      Route::get('/dashboard/artikel/cekSlug', [DashboardPostController::class, 'cekSlug']);
   ```

# Cara Mudah AUTO SLUG
   ```html
      Untuk input title :
      <input type="text" name="title" onkeyup="document.getElementById('autoslug').value = this.value.replace(/\s+/g, '-').toLowerCase()">
      
      Untuk input slug :
      <input type="text" name="slug" id="autoslug" readonly>
   ```

# Cara Simpel
1. Tambahkan Kode JS ini pada html dan judul ganti dengan element yang ingin diambil.
   ```html
      <script>
        function generateSlug() {
            let judul = document.getElementById('judul').value;
            let slug = judul.toLowerCase().replace(/[^a-z0-9]+/g, '-').replace(/^-+|-+$/g, '');
            document.getElementById('slug').value = slug;
        }
    </script>
   ```
2. Ini Inputnya.
   ```html
      <label for="judul" class="form-label">Masukan Judul</label>
      <input type="text" class="form-control" name="judul" id="judul" onkeyup="generateSlug()">
   
      <label for="slug" class="form-label">Masukan slug</label>
      <input type="text" class="form-control" name="slug" id="slug" >
   ```
