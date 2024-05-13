# Export to Excel
1. Untuk membuat file Excel, saya menggunakan library SheetJS yang dapat diakses melalui CDN:
```html
<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.0/dist/xlsx.full.min.js"></script>
```
Contoh:
```html
<!DOCTYPE html>
<html>
<head>
  <title>Export to Excel</title>
  <script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.0/dist/xlsx.full.min.js"></script>
</head>
<body>
  <button onclick="exportToExcel()">Export to Excel</button>
  <script>
    function exportToExcel() {
      const data = [
        ['Name', 'Age', 'Country'],
        ['John Doe', 30, 'USA'],
        ['Jane Smith', 25, 'Canada']
      ];

      const ws = XLSX.utils.aoa_to_sheet(data);
      const wb = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(wb, ws, 'Sheet1');
      XLSX.writeFile(wb, 'data.xlsx');
    }
  </script>
</body>
</html>
```
