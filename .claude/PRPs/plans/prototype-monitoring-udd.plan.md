# Plan: Prototype Monitoring UDD — Halaman Monitoring Obat & BHP per Tanggal Rawat

## Summary

Membuat **prototype frontend** halaman Monitoring Unit Dose Dispensing (UDD) yang menampilkan daftar Tanggal Permintaan Resep pasien di panel kiri, serta data resep obat dan BHP/Alkes di panel kanan berdasarkan tanggal yang dipilih. Prototype ini ditujukan sebagai **lampiran Redmine ticket** untuk developer, lengkap dengan fitur Cetak CPOR sesuai format rekam medis.

## User Story

Sebagai **petugas farmasi/apoteker**,
Saya ingin **melihat riwayat penggunaan obat dan BHP pasien rawat inap berdasarkan Tanggal Permintaan Resep**,
Sehingga **saya dapat memonitor dan menelusuri pemberian obat secara harian selama pasien menjalani perawatan**.

## Problem → Solution

**Current**: Halaman monitoring UDD saat ini (diakses dari `/v2/monitoring-pasien`) belum memiliki tampilan berbasis Tanggal Permintaan Resep — data obat dan BHP belum dikelompokkan per hari rawat.
**Desired**: Halaman monitoring UDD dengan 3 panel (tanggal | obat | BHP) yang interaktif, dilengkapi tombol Cetak CPOR yang menghasilkan laporan matriks format rekam medis.

## Metadata

- **Complexity**: Medium
- **Source PRD**: `sistem_saat_ini/prd_plan_monitoring_udd.md`
- **PRD Phase**: N/A (standalone prototype)
- **Estimated Files**: 1 file HTML + 1 file referensi cetak
- **Target**: Prototype frontend statis dengan mock data — BUKAN implementasi produksi

---

## UX Design

### Before

```
┌──────────────────────────────────────────────────┐
│  [Header RSUD / MORBIS]                          │
│  [Ribbon Menu]                                   │
│                                                  │
│  ┌────────────────────────────────────────────┐  │
│  │         Halaman Monitoring UDD Saat Ini     │  │
│  │  (tabel data pasien / resep — flat,         │  │
│  │   tidak ada pengelompokan per tanggal)      │  │
│  └────────────────────────────────────────────┘  │
│                                                  │
└──────────────────────────────────────────────────┘
```

### After (Prototype yang Dibangun)

```
┌──────────────────────────────────────────────────────┐
│  Monitoring Unit Dose Dispensing (UDD)               │
│  Pasien: RAHMAWATI | No. RM: 00042066                │
│  Ruangan: Rawat Inap Jantung | Kelas: VIP            │
│  Tgl Masuk: 11/03/2026 | Diagnosa: Dyspepsia         │
│                                                      │
│  ┌────────────┬──────────────────────────────────┐   │
│  │ TANGGAL   │  DATA RESEP OBAT                  │   │
│  │ RAWAT     │  ┌──────────────────────────────┐ │   │
│  │           │  │ No | No.Resep | Nama | Qty   │ │   │
│  │ [Detail]  │  │    |          | Obat | Return│ │   │
│  │ 11/03/26  │  │ 1  | R-001    | Ket..│ 3 | 0 │ │   │
│  │ [Detail]  │  │ 2  | R-002    | Ome..│ 1 | 0 │ │   │
│  │ 12/03/26  │  └──────────────────────────────┘ │   │
│  │ [Detail]  │                                    │   │
│  │ 13/03/26  │  DATA BHP / ALKES                  │   │
│  │ [Detail]  │  ┌──────────────────────────────┐ │   │
│  │ 14/03/26  │  │ No | No.Resep | Nama | Qty   │ │   │
│  │           │  │    |          | BHP  | Return│ │   │
│  │           │  │ 1  | R-003    | Spuit│ 2 | 0 │ │   │
│  │           │  └──────────────────────────────┘ │   │
│  └────────────┴──────────────────────────────────┘   │
│                                                      │
│  [Cetak CPOR]                                        │
└──────────────────────────────────────────────────────┘
```

### Interaction Changes

| Touchpoint    | Before            | After                               | Notes                                                      |
| ------------- | ----------------- | ----------------------------------- | ---------------------------------------------------------- |
| Panel Tanggal | Tidak ada         | List tanggal rawat dg tombol Detail | Klik → load data obat & BHP                                |
| Panel Obat    | Tabel flat        | Tabel per tanggal terpilih          | Kolom: No, No.Resep, Nama Obat, Qty, Return, Status, Harga |
| Panel BHP     | Tabel flat/campur | Tabel terpisah per tanggal          | Kolom: No, No.Resep, Nama BHP, Qty, Return, Status, Harga  |
| Tombol Cetak  | Tidak ada         | Tombol Cetak CPOR                   | Membuka jendela print format matriks                       |

---

## Mandatory Reading

Files yang WAJIB dibaca sebelum implementasi:

| Priority       | File                                         | Lines   | Why                                                         |
| -------------- | -------------------------------------------- | ------- | ----------------------------------------------------------- |
| P0 (critical)  | `sistem_saat_ini/prd_plan_monitoring_udd.md` | 1-54    | Spesifikasi lengkap fitur                                   |
| P0 (critical)  | `cetak.html`                                 | 1-178   | Format cetak CPOR yang harus direplikasi                    |
| P1 (important) | `sistem_saat_ini/monitoring_udd.html`        | 1-200   | Struktur HTML sistem existing, CSS framework yang digunakan |
| P2 (reference) | `sistem_saat_ini/monitoring_udd.html`        | 328-450 | Header/ribbon/menu system existing                          |

## External Documentation

| Topic           | Source                                  | Key Takeaway                                                     |
| --------------- | --------------------------------------- | ---------------------------------------------------------------- |
| Bootstrap 3 CSS | Sistem existing menggunakan Bootstrap 3 | Prototype harus pakai Bootstrap 3 agar tampilan konsisten        |
| DataTables      | Sistem existing menggunakan DataTables  | Gunakan DataTables untuk tabel yang interaktif (sorting, search) |
| Format CPOR     | `cetak.html` + GitHub Pages referensi   | Print layout landscape, matriks tanggal 1-10, OBAT & BHP dipisah |

---

## Patterns to Mirror

### HTML_STRUCTURE

```html
<!-- SOURCE: sistem_saat_ini/monitoring_udd.html:1-4 -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  </head>
</html>
```

### CSS_FRAMEWORK

```html
<!-- SOURCE: sistem_saat_ini/monitoring_udd.html:15-22 -->
<!-- Sistem existing menggunakan Bootstrap 3 + custom workspace/base CSS -->
<link rel="stylesheet" type="text/css" href="...bootstrap.css" />
<link rel="stylesheet" type="text/css" href="...bootstrap.min.css" />
<link rel="stylesheet" type="text/css" href="...bootstrap-theme.min.css" />
```

### JS_DEPENDENCIES

```html
<!-- SOURCE: sistem_saat_ini/monitoring_udd.html:35-62 -->
<!-- Sistem existing menggunakan jQuery + DataTables + Bootstrap 3 JS -->
<script src="...jquery.min.js"></script>
<script src="...bootstrap.min.js"></script>
<script src="...jquery.dataTables.min.js"></script>
```

### PRINT_STYLING

```css
/* SOURCE: cetak.html:8-9, 31-34 */
@page {
  size: landscape;
  margin: 5mm;
}
body {
  font-size: 10px;
  color: #000;
  font-family: Arial, sans-serif;
}
@media print {
  .btn-print {
    display: none;
  }
  .paper {
    margin: 0;
    padding: 0;
  }
}
```

### TABLE_STYLING

```css
/* SOURCE: cetak.html:14-15 */
.table-cpor {
  width: 100%;
  border-collapse: collapse;
  table-layout: fixed;
}
.table-cpor th,
.table-cpor td {
  border: 1px solid #000 !important;
  padding: 4px 2px;
  vertical-align: middle;
}
```

---

## Files to Change

| File                            | Action    | Justification                                               |
| ------------------------------- | --------- | ----------------------------------------------------------- |
| `prototype-monitoring-udd.html` | CREATE    | Prototype halaman utama monitoring UDD — 3 panel interaktif |
| `sistem_saat_ini/cetak.html`    | REFERENCE | Sudah ada — diformat ulang atau dipanggil via window.open() |

## NOT Building

- **Integrasi backend/API** — ini prototype statis dengan mock data
- **Koneksi database** — semua data hardcoded sebagai sample
- **Autentikasi/login** — prototype tidak memerlukan session
- **Edit/CRUD data** — hanya tampilan read-only dan cetak
- **Responsive mobile** — fokus pada desktop (1024px+)
- **Integrasi dengan sistem MORBIS** — prototype standalone

---

## Step-by-Step Tasks

### Task 1: Struktur Dasar HTML & Container

- **ACTION**: Buat file `prototype-monitoring-udd.html` dengan struktur HTML5 + Bootstrap 3 CDN
- **IMPLEMENT**:
  ```html
  <!DOCTYPE html>
  <html lang="id">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Monitoring UDD - Prototype</title>
      <!-- Bootstrap 3.4.1 CDN -->
      <link
        rel="stylesheet"
        href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css"
      />
      <!-- DataTables -->
      <link
        rel="stylesheet"
        href="https://cdn.datatables.net/1.10.25/css/dataTables.bootstrap.min.css"
      />
      <!-- Font Awesome -->
      <link
        rel="stylesheet"
        href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css"
      />
      <!-- Custom CSS -->
      <style>
        /* CSS panel layout di sini */
      </style>
    </head>
    <body>
      <div class="container-fluid">
        <!-- Header Info Pasien -->
        <!-- Layout 3 Panel -->
      </div>
      <!-- Scripts -->
    </body>
  </html>
  ```
- **MIRROR**: Bootstrap 3 pattern dari `monitoring_udd.html` (sistem existing)
- **IMPORTS**: Bootstrap 3 CSS, DataTables CSS, Font Awesome, jQuery, Bootstrap JS, DataTables JS
- **GOTCHA**: Jangan gunakan Bootstrap 4/5 — sistem existing menggunakan Bootstrap 3
- **VALIDATE**: File terbuka di browser, tidak ada error 404 pada CDN

### Task 2: Panel Info Header Pasien

- **ACTION**: Tambahkan section header berisi informasi pasien yang dimonitor
- **IMPLEMENT**:
  ```html
  <div class="row">
    <div class="col-md-12">
      <div class="panel panel-info">
        <div class="panel-heading">
          <h3 class="panel-title">Monitoring Unit Dose Dispensing (UDD)</h3>
        </div>
        <div class="panel-body">
          <div class="row">
            <div class="col-md-3"><strong>Nama Pasien:</strong> RAHMAWATI</div>
            <div class="col-md-3"><strong>No. RM:</strong> 00042066</div>
            <div class="col-md-3">
              <strong>Ruangan:</strong> Rawat Inap Jantung
            </div>
            <div class="col-md-3"><strong>Kelas:</strong> VIP</div>
          </div>
          <div class="row" style="margin-top: 8px;">
            <div class="col-md-3"><strong>Tgl Masuk:</strong> 11/03/2026</div>
            <div class="col-md-3"><strong>Tgl Keluar:</strong> -</div>
            <div class="col-md-3">
              <strong>Diagnosa:</strong> Dyspepsia, Hypertension
            </div>
            <div class="col-md-3">
              <strong>Dokter:</strong> dr. Budi Santoso, Sp.PD
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  ```
- **MIRROR**: Data header pasien dari `cetak.html:48-73` (format CPOR header)
- **IMPORTS**: Bootstrap panel component
- **GOTCHA**: Data pasien harus konsisten antara header monitoring dan output cetak
- **VALIDATE**: Header menampilkan seluruh info pasien sesuai PRD

### Task 3: Panel Kiri — Daftar Tanggal Permintaan Resep

- **ACTION**: Buat panel kiri yang menampilkan list Tanggal Permintaan Resep pasien dari tgl masuk s/d tgl terakhir perawatan
- **IMPLEMENT**:
  ```html
  <div class="col-md-3">
    <div class="panel panel-default">
      <div class="panel-heading"><strong>Tanggal Permintaan Resep</strong></div>
      <div class="panel-body" style="padding: 0;">
        <table class="table table-hover" id="tabel-tanggal">
          <thead>
            <tr>
              <th style="width:60px;">Aksi</th>
              <th>Tanggal</th>
            </tr>
          </thead>
          <tbody>
            <!-- Row sample: -->
            <tr data-tanggal="2026-03-11">
              <td>
                <button class="btn btn-xs btn-info btn-detail">Detail</button>
              </td>
              <td>11/03/2026</td>
            </tr>
            <!-- ...s/d tanggal terakhir -->
          </tbody>
        </table>
      </div>
    </div>
  </div>
  ```

  - Data tanggal: `11/03/2026`, `12/03/2026`, `13/03/2026`, `14/03/2026` (4 hari rawat, pasien masih dirawat)
  - Setiap baris punya tombol "Detail" (btn-info btn-xs)
  - Row yang sedang dipilih diberi class `active` (highlight biru Bootstrap 3)
  - Default: tanggal pertama terpilih saat halaman load
- **MIRROR**: Pattern tabel-hover dari existing Bootstrap tables di `monitoring_udd.html`
- **IMPORTS**: Bootstrap table classes
- **GOTCHA**: Pastikan format tanggal konsisten `dd/mm/yyyy` (format Indonesia)
- **VALIDATE**: Panel kiri menampilkan 4 tanggal + tombol Detail, klik berpindah active state

### Task 4: Panel Kanan Atas — Tabel Resep Obat

- **ACTION**: Buat panel kanan atas yang menampilkan DataTable obat berdasarkan tanggal yang dipilih
- **IMPLEMENT**:
  ```html
  <div class="col-md-9">
    <div class="panel panel-success">
      <div class="panel-heading">
        <strong>Data Resep Obat</strong>
        <span class="label label-info pull-right" id="label-tanggal-obat"
          >11/03/2026</span
        >
      </div>
      <div class="panel-body">
        <table class="table table-bordered table-striped" id="tabel-obat">
          <thead>
            <tr>
              <th>No</th>
              <th>No. Resep</th>
              <th>Nama Obat</th>
              <th>Qty</th>
              <th>Return</th>
              <th>Status</th>
              <th>Harga</th>
            </tr>
          </thead>
          <tbody>
            <!-- Diisi via JS berdasarkan tanggal -->
          </tbody>
        </table>
      </div>
    </div>
    <!-- Panel Kanan Bawah menyusul -->
  </div>
  ```

  - Data mock per tanggal (contoh `11/03/2026`):
    | No | No. Resep | Nama Obat | Qty | Return | Status | Harga |
    |----|-----------|-----------|-----|--------|--------|-------|
    | 1 | R-2026-001 | Ketorolac Inj 30 | 3 | 0 | Diberikan | 15.000 |
    | 2 | R-2026-002 | Omeprazole Inj 40 mg | 1 | 0 | Diberikan | 18.500 |
    | 3 | R-2026-003 | Ceftriaxone Inj 1 gr | 2 | 0 | Diberikan | 22.000 |
- **MIRROR**: Kolom sesuai PRD: No, No. Resep, Nama Obat, Qty, Return, Status, Harga
- **IMPORTS**: DataTables library untuk sorting/pagination
- **GOTCHA**: Jika tidak ada data obat untuk tanggal tertentu, tampilkan "Tidak ada resep obat pada tanggal ini"
- **VALIDATE**: Klik tanggal berbeda → data obat berubah sesuai, label tanggal update

### Task 5: Panel Kanan Bawah — Tabel BHP / Alkes

- **ACTION**: Buat panel kanan bawah untuk data BHP/Alkes pada tanggal terpilih
- **IMPLEMENT**:
  ```html
  <div class="panel panel-warning">
    <div class="panel-heading">
      <strong>Data BHP / Alkes</strong>
      <span class="label label-info pull-right" id="label-tanggal-bhp"
        >11/03/2026</span
      >
    </div>
    <div class="panel-body">
      <table class="table table-bordered table-striped" id="tabel-bhp">
        <thead>
          <tr>
            <th>No</th>
            <th>No. Resep</th>
            <th>Nama BHP / Alkes</th>
            <th>Qty</th>
            <th>Return</th>
            <th>Status</th>
            <th>Harga</th>
          </tr>
        </thead>
        <tbody>
          <!-- Diisi via JS berdasarkan tanggal -->
        </tbody>
      </table>
    </div>
  </div>
  ```

  - Data mock per tanggal (contoh `11/03/2026`):
    | No | No. Resep | Nama BHP / Alkes | Qty | Return | Status | Harga |
    |----|-----------|------------------|-----|--------|--------|-------|
    | 1 | B-2026-001 | Spuit 10 mL | 1 | 0 | Digunakan | 2.500 |
    | 2 | B-2026-002 | Infuset Dewasa | 1 | 0 | Digunakan | 12.500 |
    | 3 | B-2026-003 | IV Catheter 20G | 1 | 0 | Digunakan | 14.000 |
- **MIRROR**: Kolom sama dengan tabel obat (sesuai PRD: No, No. Resep, Nama BHP, Qty, Return, Status, Harga)
- **IMPORTS**: DataTables library
- **GOTCHA**: Panel BHP harus di bawah panel obat dalam satu col-md-9 yang sama (gunakan row bersarang)
- **VALIDATE**: Panel BHP muncul di bawah panel obat, data berubah sesuai tanggal terpilih

### Task 6: JavaScript Interaktivitas — Klik Tanggal → Load Data

- **ACTION**: Tulis JavaScript untuk menangani klik pada panel tanggal dan memuat data mock
- **IMPLEMENT**:

  ```javascript
  // Mock data: objek dengan key tanggal berisi data obat dan BHP
  var mockData = {
    "2026-03-11": {
      obat: [
        {
          no: 1,
          resep: "R-2026-001",
          nama: "Ketorolac Inj 30",
          qty: 3,
          ret: 0,
          status: "Diberikan",
          harga: 15000,
        },
        {
          no: 2,
          resep: "R-2026-002",
          nama: "Omeprazole Inj 40 mg",
          qty: 1,
          ret: 0,
          status: "Diberikan",
          harga: 18500,
        },
        {
          no: 3,
          resep: "R-2026-003",
          nama: "Ceftriaxone Inj 1 gr",
          qty: 2,
          ret: 0,
          status: "Diberikan",
          harga: 22000,
        },
      ],
      bhp: [
        {
          no: 1,
          resep: "B-2026-001",
          nama: "Spuit 10 mL",
          qty: 1,
          ret: 0,
          status: "Digunakan",
          harga: 2500,
        },
        {
          no: 2,
          resep: "B-2026-002",
          nama: "Infuset Dewasa",
          qty: 1,
          ret: 0,
          status: "Digunakan",
          harga: 12500,
        },
        {
          no: 3,
          resep: "B-2026-003",
          nama: "IV Catheter 20G",
          qty: 1,
          ret: 0,
          status: "Digunakan",
          harga: 14000,
        },
      ],
    },
    "2026-03-12": {
      /* data untuk 12 Maret */
    },
    "2026-03-13": {
      /* data untuk 13 Maret */
    },
    "2026-03-14": {
      /* data untuk 14 Maret */
    },
  };

  // Inisialisasi DataTables
  var tabelObat = $("#tabel-obat").DataTable({
    /* config */
  });
  var tabelBhp = $("#tabel-bhp").DataTable({
    /* config */
  });

  // Handler klik tombol Detail
  $("#tabel-tanggal").on("click", ".btn-detail", function () {
    var row = $(this).closest("tr");
    var tanggal = row.data("tanggal");

    // Update active row
    $("#tabel-tanggal tbody tr").removeClass("active");
    row.addClass("active");

    // Update label tanggal
    var tglFormatted = formatTanggal(tanggal);
    $("#label-tanggal-obat, #label-tanggal-bhp").text(tglFormatted);

    // Clear & reload DataTables dengan data baru
    reloadTabelObat(tanggal);
    reloadTabelBhp(tanggal);
  });
  ```

- **MIRROR**: Pattern `$(document).ready(...)` + jQuery `.on('click', ...)` dari `monitoring_udd.html:244-301`
- **IMPORTS**: jQuery, DataTables API (`clear()`, `rows.add()`, `draw()`)
- **GOTCHA**: DataTables harus di-clear dulu sebelum reload (`tabelObat.clear().rows.add(data).draw()`)
- **VALIDATE**: Klik tombol Detail pada setiap tanggal → tabel obat & BHP reload, active row berubah, label tanggal update

### Task 7: Tombol Cetak CPOR + Jendela Print

- **ACTION**: Tambahkan tombol Cetak CPOR dan implementasikan fungsi cetak matriks
- **IMPLEMENT**:
  ```html
  <!-- Tombol di bawah layout (full width) -->
  <div class="row" style="margin-top: 15px;">
    <div class="col-md-12 text-right">
      <button class="btn btn-danger btn-lg" onclick="cetakCPOR()">
        <i class="fa fa-print"></i> Cetak CPOR (Catatan Pemberian Obat Rawat)
      </button>
    </div>
  </div>
  ```
  ```javascript
  function cetakCPOR() {
    // 1. Kumpulkan data dari mockData (semua tanggal)
    // 2. Buat jendela baru (window.open)
    // 3. Render tabel matriks landscape:
    //    - Header: Ruangan, Kelas, Nama Pasien, No.RM, JK, Tgl Masuk/Keluar, Diagnosa, Dokter
    //    - Tabel OBAT: kolom tanggal 1 s/d N, kolom RET, JML, Harga Satuan, Total Harga
    //    - Tabel BHP/ALKES: format sama, section terpisah
    //    - Footer: Tanda Terima + TOTAL BIAYA
    // 4. Auto-trigger window.print()
    // 5. Tutup jendela setelah print
    var printWindow = window.open("", "_blank", "width=1200,height=800");
    printWindow.document.write(generateCPORContent());
    printWindow.document.close();
    printWindow.focus();
    printWindow.onload = function () {
      printWindow.print();
    };
  }
  ```

  - Format cetak MENGIKUTI `cetak.html:37-177`:
    - Landscape orientation (`@page { size: landscape; margin: 5mm; }`)
    - Font 10px Arial
    - Tabel dengan border 1px solid black
    - Header: instruksi terapi + info pasien (4 kolom)
    - Section OBAT dengan data seluruh tanggal (kolom 1-10)
    - Section ALKES/BHP dengan data seluruh tanggal
    - Footer: tanda terima + total biaya
- **MIRROR**: EXACT layout dari `cetak.html` — setiap class `.table-cpor`, `.bg-grey`, `.col-tgl`, `.col-price`, `.header-val`
- **IMPORTS**: Bootstrap 4 CDN (untuk jendela print saja, karena cetak.html menggunakannya)
- **GOTCHA**: Jangan gunakan Bootstrap 3 di jendela print — `cetak.html` menggunakan Bootstrap 4.5.2 untuk styling print yang sudah proven
- **VALIDATE**: Klik tombol Cetak CPOR → jendela baru terbuka dengan format matriks landscape → print dialog muncul → layout sesuai referensi cetak.html

### Task 8: Styling & Polish

- **ACTION**: Finalisasi CSS untuk memastikan tampilan prototype rapi dan profesional
- **IMPLEMENT**:
  - Panel layout menggunakan Bootstrap grid (`.col-md-3` + `.col-md-9`)
  - Row aktif di panel tanggal: `background-color: #d9edf7` (Bootstrap 3 info color)
  - Hover effect pada row tanggal menggunakan Bootstrap `.table-hover`
  - Panel obat (hijau/success), panel BHP (kuning/warning) — bedakan secara visual
  - Badge/label tanggal di panel header
  - Tombol Cetak CPOR: merah, besar, fixed di kanan bawah
  - Semua tabel menggunakan DataTables dengan pagination 5 rows
- **MIRROR**: Warna dan spacing dari `.panel`, `.table`, `.btn` Bootstrap 3
- **IMPORTS**: Bootstrap 3 theme CSS
- **GOTCHA**: Jangan gunakan CSS custom berlebihan — manfaatkan class Bootstrap 3 bawaan agar developer mudah memahami struktur
- **VALIDATE**: Prototype terlihat profesional, tidak seperti template mentah

---

## Testing Strategy

### Visual Testing

| Test                               | Expected Result                                   |
| ---------------------------------- | ------------------------------------------------- |
| Buka prototype di Chrome/Edge      | Layout 3 panel tampil sempurna                    |
| Klik tombol Detail tanggal berbeda | Tabel obat & BHP reload data                      |
| Klik tombol Cetak CPOR             | Jendela print terbuka dengan format matriks       |
| Print preview                      | Landscape, 10 kolom tanggal, OBAT dan BHP dipisah |
| Resize window ke 1024px            | Layout masih terbaca dengan baik                  |

### Edge Cases Checklist

- [x] Tanggal tanpa resep obat → tampilkan "Tidak ada resep obat"
- [x] Tanggal tanpa BHP → tampilkan "Tidak ada BHP/Alkes"
- [x] Pasien dengan masa rawat > 10 hari → kolom print perlu handling kolom tanggal tambahan
- [x] Klik cepat tombol Detail → tidak ada race condition (pakai DataTables draw())
- [x] Jendela print diblokir browser → beri alert manual untuk allow popup

---

## Validation Commands

### Browser Validation

```bash
# Buka file langsung di browser
start d:\laragon\www\monitoring_uud\prototype-monitoring-udd.html
```

EXPECT: Halaman terbuka tanpa error, semua interaksi berfungsi

### Console Validation (DevTools)

```javascript
// Cek DataTables terinisialisasi
$("#tabel-obat").DataTable();
$("#tabel-bhp").DataTable();
```

EXPECT: Tidak ada error di console

### Print Validation

- Klik "Cetak CPOR" → jendela baru terbuka → Print Preview → Pastikan format landscape
  EXPECT: Format sesuai cetak.html (header, tabel matriks, footer)

---

## Acceptance Criteria

- [x] Panel kiri menampilkan daftar Tanggal Permintaan Resep dengan tombol Detail
- [x] Panel kanan atas menampilkan tabel obat sesuai tanggal terpilih (7 kolom: No, No.Resep, Nama Obat, Qty, Return, Status, Harga)
- [x] Panel kanan bawah menampilkan tabel BHP/Alkes sesuai tanggal terpilih (7 kolom: No, No.Resep, Nama BHP, Qty, Return, Status, Harga)
- [x] Klik tombol Detail → kedua tabel reload, active row berubah
- [x] Tombol Cetak CPOR berfungsi dan menghasilkan format sesuai referensi
- [x] Format cetak: landscape, header info pasien, tabel matriks tanggal, OBAT & BHP dipisah, footer tanda terima + total biaya
- [x] Prototype menggunakan Bootstrap 3 konsisten dengan sistem existing
- [x] Semua data mock realistis (nama obat/BHP generik farmasi Indonesia)
- [x] File tunggal (1 HTML), tidak ada dependency ke server/API

## Completion Checklist

- [x] Code menggunakan Bootstrap 3 (konsisten dengan sistem existing)
- [x] jQuery pattern mengikuti existing codebase (`$(document).ready`, `.on('click')`)
- [x] DataTables digunakan untuk tabel interaktif
- [x] Format cetak EXACT mengikuti cetak.html yang sudah proven
- [x] Tidak ada hardcoded credential atau URL production
- [x] Mock data menggunakan data farmasi Indonesia yang realistis
- [x] Self-contained — developer bisa langsung melihat dan memahami requirement

## Risks

| Risk                                            | Likelihood | Impact | Mitigation                                                      |
| ----------------------------------------------- | ---------- | ------ | --------------------------------------------------------------- |
| Format cetak tidak cocok dengan ekspektasi user | MEDIUM     | HIGH   | Sudah ada referensi cetak.html yang proven + GitHub Pages demo  |
| Jumlah hari rawat > 10 kolom                    | LOW        | MEDIUM | Beri keterangan di prototype bahwa kolom print bisa disesuaikan |
| Developer salah paham ini kode produksi         | MEDIUM     | LOW    | Beri komentar jelas di HTML: "PROTOTYPE — NOT FOR PRODUCTION"   |

## Notes

- Prototype ini ditujukan sebagai **lampiran visual** Redmine ticket — bukan kode produksi
- Data mock dibuat serealistis mungkin dengan nama obat generik yang lazim di farmasi RS Indonesia
- Warna panel sengaja dibedakan (info/biru untuk header pasien, success/hijau untuk obat, warning/kuning untuk BHP) agar mudah dibedakan oleh user
- Format cetak CPOR sudah proven melalui `cetak.html` dan GitHub Pages — tidak perlu eksperimen baru
- Jika di kemudian hari prototype akan diintegrasikan ke sistem MORBIS, maka perlu ditambahkan: API endpoint, session handling, dan integrasi dengan database resep existing
