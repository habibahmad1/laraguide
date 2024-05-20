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
