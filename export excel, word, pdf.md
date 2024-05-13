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
  <table id="myTable">
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th>Country</th>
    </tr>
    <tr>
      <td>John Doe</td>
      <td>30</td>
      <td>USA</td>
    </tr>
    <tr>
      <td>Jane Smith</td>
      <td>25</td>
      <td>Canada</td>
    </tr>
  </table>
  <button onclick="exportToExcel()">Export to Excel</button>
  <script>
    function exportToExcel() {
      const table = document.getElementById('myTable');
      const rows = Array.from(table.querySelectorAll('tr'));
      const data = rows.map(row => Array.from(row.querySelectorAll('th, td')).map(cell => cell.innerText));
      
      const ws = XLSX.utils.aoa_to_sheet(data);
      const wb = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(wb, ws, 'Sheet1');
      XLSX.writeFile(wb, 'data.xlsx');
    }
  </script>
</body>
</html>

```

# Export to PDF
2. Untuk membuat file PDF, saya menggunakan library jsPDF yang juga dapat diakses melalui CDN:
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.4.0/jspdf.umd.min.js"></script>
```
Contoh: 
```html
<!DOCTYPE html>
<html>
<head>
  <title>Export to PDF</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.4.0/jspdf.umd.min.js"></script>
</head>
<body>
  <table id="myTable">
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th>Country</th>
    </tr>
    <tr>
      <td>John Doe</td>
      <td>30</td>
      <td>USA</td>
    </tr>
    <tr>
      <td>Jane Smith</td>
      <td>25</td>
      <td>Canada</td>
    </tr>
  </table>
  <button onclick="exportToPDF()">Export to PDF</button>
  <script>
    function exportToPDF() {
      const doc = new jsPDF();
      doc.autoTable({ html: '#myTable' });
      doc.save('table.pdf');
    }
  </script>
</body>
</html>

```

# Export to Word
3. Dan untuk membuat file Word, saya menggunakan library html-docx-js yang juga dapat diakses melalui CDN:
```html
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html-docx-js/0.2.2/html-docx.js"></script>
```
Contoh:
```html
<!DOCTYPE html>
<html>
<head>
  <title>Export to Word</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html-docx-js/0.2.2/html-docx.js"></script>
</head>
<body>
  <table id="myTable">
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th>Country</th>
    </tr>
    <tr>
      <td>John Doe</td>
      <td>30</td>
      <td>USA</td>
    </tr>
    <tr>
      <td>Jane Smith</td>
      <td>25</td>
      <td>Canada</td>
    </tr>
  </table>
  <button onclick="exportToWord()">Export to Word</button>
  <script>
    function exportToWord() {
      const content = document.getElementById('myTable').outerHTML;
      const converted = htmlDocx.asBlob(content);
      saveAs(converted, 'table.docx');
    }
  </script>
</body>
</html>

```
