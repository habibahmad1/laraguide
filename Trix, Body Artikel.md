# Trix | Untuk buat BodyPost pada Form

1. Tambahkan CDN sebelum head
  ```html
    <link rel="stylesheet" type="text/css" href="https://unpkg.com/trix@2.0.8/dist/trix.css">
    <script type="text/javascript" src="https://unpkg.com/trix@2.0.8/dist/trix.umd.min.js"></script>
  ```

2. Masukan ke Form input untuk Body Post, id harus sama dengan input.
  ```html
         <div class="mb-3">
            <label for="bodyPost" class="form-label">Isi Artikel</label>
            <input id="bodyPost" type="hidden" name="bodyPost">
            <trix-editor input="bodyPost"></trix-editor>
        </div>
  ```

3. Jika ingin tidak bisa menambahkan file pada bodypost setting pada Trix nya, tambahkan CSS .
  ```html
      <style>
        trix-toolbar [data-trix-button-group="file-tools"]{
          display: none;
        }
      </style>
  ```

4.  Dan tambahkan Javascript agar tidak bisa dijalankan file.
```javascript
    document.addEventListener('trix-file-accept',(e)=>{
        e.prevendDefault();
    })
```
    
