# Buat Animasi Teks Seperti Mengetik
1. Tambahkan CDN di bawah sebelum akhir body
   ```html
   {{-- Type Animation --}}
    <script src="https://unpkg.com/typed.js@2.1.0/dist/typed.umd.js"></script>
   ```
2. Buat Teksnya, disini teks tetap nya Hello, Selanjutnya dengan menggunakan Animasi Type.
   ```html
   <h1>Hello, <span id="element" style="color: #ffff76"></span> </h1>
   ```
3. Taruh diatas CDN tadi, dan isi strings sesuai dengan mau ketik apa saja.
   ```html
   <script>
        document.addEventListener('DOMContentLoaded', function() {
            const typed = new Typed('#element', {
                strings: ["Welcome","Nice To Meet You"],
                typeSpeed: 150,
                backSpeed: 150,
                loop: true,
            });
        });
    </script>
   ```
