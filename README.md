# Kabar Belajar

Website berita pendidikan Indonesia dan dunia yang diperbarui otomatis tiap hari pukul 06.00 WIB.
Gratis di GitHub Pages. Tidak perlu server.

## Isi paket

| Berkas | Fungsi |
| --- | --- |
| `index.html`, `gaya.css` | Halaman utama. Menampilkan 3 berita terbaru, tombol website lain, dan ruang iklan |
| `berita.json` | Daftar berita. Ditulis otomatis oleh robot harian |
| `sumber.json` | Daftar sumber berita (RSS) dan domain yang diterima |
| `tautan.json` | Tombol "Website lain". Edit sendiri |
| `scripts/update_berita.py` | Skrip yang mengambil berita dari sumber |
| `.github/workflows/update-berita.yml` | Jadwal harian di GitHub Actions |
| `tentang.html`, `kebijakan-privasi.html` | Halaman wajib untuk situs beriklan |
| `ads.txt.contoh` | Contoh berkas untuk AdSense |

## Cara memasang (sekitar 20 menit)

1. **Buat akun GitHub** di github.com, lalu buat repositori baru. Beri nama `kabar-belajar`, pilih **Public**.
2. **Unggah semua berkas** lewat tombol **Add file → Upload files**. Folder `.github` bisa gagal terunggah dari browser karena namanya diawali titik. Bila begitu, buat manual lewat **Add file → Create new file**, ketik nama `.github/workflows/update-berita.yml`, lalu tempel isinya.
3. **Aktifkan GitHub Pages:** **Settings → Pages**, pada Source pilih **Deploy from a branch**, branch `main`, folder `/ (root)`, lalu Save. Setelah beberapa menit, alamat situsmu muncul di halaman itu.
4. **Izinkan robot menyimpan:** **Settings → Actions → General → Workflow permissions**, pilih **Read and write permissions**, lalu Save.
5. **Jalankan pertama kali:** buka tab **Actions → Perbarui berita → Run workflow**. Tunggu hingga selesai, lalu klik hasilnya untuk membaca catatan.
   - Baris `[OK]` berarti sumber itu bisa dibaca. Baris `[GAGAL]` berarti alamatnya perlu diperbaiki di `sumber.json`.
   - Berkas `berita.json` akan terisi berita terbaru. Sesudahnya robot jalan sendiri tiap hari.

## Yang perlu kamu periksa sendiri

**Alamat RSS di `sumber.json`.** Hanya alamat ANTARA versi Inggris yang sudah dipastikan benar saat paket ini dibuat. Empat alamat lain memakai Google News RSS dengan pencarian per situs resmi dan belum diuji. Bila ada yang `[GAGAL]` atau selalu 0 berita, cari alamat RSS resmi situs itu (biasanya ada ikon RSS atau halaman `/rss`) dan ganti isi `url`.

**Filter domain.** Hanya berita dari domain di `domain_boleh` yang masuk. Ini yang menjaga sumber tetap tepercaya. Tambahkan domain hanya bila kamu yakin dengan reputasinya.

**Baca sekilas tiap pagi.** Skrip tidak membaca isi artikel penuh. Ia hanya memakai judul dan kutipan dari RSS. Kolom "Dasar informasi" di tiap berita menjelaskan hal ini. Bila ada berita yang ingin dihapus, buka `berita.json` di GitHub, hapus bagiannya, lalu simpan.

## Mengubah tombol website lain

Buka `tautan.json` di GitHub, klik ikon pensil, tambahkan baris seperti ini, lalu **Commit changes**:

```json
{ "nama": "Nama Tombol", "url": "https://contoh.go.id" }
```

Pisahkan tiap baris dengan koma. Tombol akan muncul di situs dalam beberapa menit.

## Mengubah jumlah berita

Di `index.html`, cari `var JUMLAH_BERITA = 3;` dan ganti angkanya.

## Ringkasan AI (opsional, berbayar)

Tanpa AI, situs menampilkan judul dan kutipan resmi dari sumber. Itu gratis dan paling kecil risikonya.

Bila ingin ringkasan dengan bahasa lebih sederhana:

1. Buat API key di console.anthropic.com.
2. Di GitHub: **Settings → Secrets and variables → Actions → New repository secret**. Nama: `ANTHROPIC_API_KEY`, isi: kuncimu.
3. Selesai. Skrip meringkas paling banyak 6 berita baru per hari dengan aturan: hanya memakai informasi dari judul dan kutipan, tidak menambah fakta, bahasa sehari-hari. Berita yang diringkas AI diberi keterangan jelas di kolom "Dasar informasi".

Biaya sangat kecil karena hanya sedikit teks per hari, tetapi cek harga terbaru di situs Anthropic. Ringkasan AI tetap bisa keliru, jadi tetap baca sekilas.

## Memasang iklan

Iklan nyata butuh persetujuan jaringan iklan. Untuk Google AdSense:

1. Pasang situs dulu dan isi dengan berita beberapa minggu. Situs yang masih kosong biasanya ditolak.
2. Siapkan nama domain sendiri bila bisa. AdSense umumnya tidak menerima alamat gratis bersama seperti `namakamu.github.io`, jadi cek syaratnya saat mendaftar.
3. Isi `[ALAMAT-EMAIL-ANDA]` dan `[TANGGAL-PEMBARUAN]` di `tentang.html` dan `kebijakan-privasi.html`.
4. Daftar di google.com/adsense. Ikuti petunjuk untuk menambahkan situsmu.
5. Setelah disetujui, di `index.html` hapus tanda komentar pada baris `<script async src="...adsbygoogle.js...">` di bagian atas dan ganti nomor `ca-pub-` dengan milikmu.
6. Buat unit iklan di AdSense, lalu tempel kodenya di tempat yang ditandai "IKLAN, LANGKAH 2" di `index.html`.
7. Ganti nama `ads.txt.contoh` menjadi `ads.txt` dan isi nomor penerbitmu.

**Menghindari iklan yang berbau SARA:** di AdSense buka **Kontrol pemblokiran (Blocking controls)**, lalu blokir kategori sensitif dan kategori umum yang tidak kamu inginkan, misalnya agama dan politik. Ini mengurangi risiko, tetapi tidak menjamin 100%. Periksa iklan yang tampil sesekali, dan blokir iklan tertentu dari pusat tinjauan iklan bila perlu.

**Aturan penting:** jangan klik iklan di situsmu sendiri dan jangan minta orang lain mengkliknya. Itu bisa membuat akun ditutup.

## Perawatan

- GitHub dapat menonaktifkan jadwal otomatis bila repositori tidak ada aktivitas selama 60 hari. Karena robot menyimpan berita baru hampir tiap hari, ini jarang terjadi. Bila terjadi, buka tab Actions dan aktifkan kembali.
- Bila situs tampak kosong, cek tab Actions untuk melihat sumber mana yang gagal.
