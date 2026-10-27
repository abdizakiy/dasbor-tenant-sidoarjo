# Dasbor Tenant

Dasbor satu halaman untuk menganalisis tenant (usaha) dalam radius 1 km dari satu titik survei. Datanya dibaca langsung dari sheet **Tenant** di Google Sheets milik pengguna, lalu dipakai untuk peta sebaran, kepadatan per cincin jarak dan arah, persaingan per niche, dan daftar tenant yang bisa difilter dan diunduh sebagai CSV.

**Repo ini hanya berisi kode. Tidak ada data tenant di dalamnya.**

## Cara kerja

Halaman ini ditulis sebagai [Artifact Claude](https://claude.ai) dan memakai dua kemampuan runtime-nya:

- `mcp`: membaca sheet lewat konektor Google Sheets milik pembuka halaman (tool `get_values`). Data tidak disimpan di halaman dan diperbarui tiap 2 menit.
- `downloads`: menyimpan hasil filter sebagai CSV.

Karena itu halaman ini **tidak berjalan sebagai situs statis biasa** (misalnya GitHub Pages). Di luar claude.ai ia hanya menampilkan pesan bahwa data tidak bisa dimuat.

## Pemasangan

1. Siapkan Google Sheets dengan sheet bernama `Tenant` dan kolom pada baris pertama:

   `No, Nama Tenant, Kategori, Alamat, Kelurahan/Kecamatan, Telepon, Website, Rating, Jumlah Ulasan, Status, Jarak (m), Latitude, Longitude, Link Google Maps, Sub-kelompok, Kategori Besar`

   Kolom dicocokkan berdasarkan nama header, jadi urutannya bebas. `Status` berisi `Buka`, `Tutup sementara`, atau `Tutup permanen`.
2. Buka `index.html` dan isi blok `CONFIG` di bagian atas skrip:
   - `LAT` dan `LNG`: koordinat titik survei (desimal).
   - `SHEET_ID`: ID spreadsheet, yaitu bagian antara `/d/` dan `/edit` pada URL.
3. Publikasikan sebagai Artifact di claude.ai dengan kemampuan berikut, dan biarkan privat bila datanya sensitif:

   ```json
   {"mcp": {"servers": [{"server": "Google Sheets", "tools": ["get_values"]}]}, "downloads": true}
   ```
4. Saat pertama dibuka, setujui akses Google Sheets untuk halaman itu.

## Catatan

- Penilaian Jenuh, Sedang, dan Longgar pada tabel niche bersifat indikatif. Aturannya tertulis di bawah tabel pada halaman.
- Niche dicocokkan dari kata kunci pada nama dan kategori tenant, jadi satu tenant bisa masuk lebih dari satu niche dan hasilnya perlu dibaca sebagai perkiraan.
- Jumlah ulasan dipakai sebagai petunjuk keramaian, bukan angka penjualan.
- Jika datamu berasal dari Google Maps, pastikan pengambilannya sesuai ketentuan layanan dan hukum yang berlaku.
