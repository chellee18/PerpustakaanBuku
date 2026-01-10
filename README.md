# Web Peminjaman Buku Perpustakaan (Laravel)

Project ini merupakan web berbasis Laravel yang digunakan untuk mengelola data buku, data member, serta transaksi peminjaman buku pada perpustakaan. Sistem dikembangkan menggunakan arsitektur MVC (Model–View–Controller) dan database MySQL.

**Tech Stack**
- Bahasa pemrograman: PHP
- Framework: Laravel
- Database: MySQL (phpMyAdmin / XAMPP)
- Frontend: Blade Template, HTML, CSS
- Arsitektur: MVC (Model View Controller)

**Struktur Project (Garis Besar)**

Struktur folder utama disusun berdasarkan tanggung jawab tiap komponen aplikasi.

app/Models

Berisi representasi tabel database dan relasi antar tabel.

Model yang digunakan:
- Buku
- Member
- Peminjaman
- DetailPeminjaman
- User (default Laravel)

Relasi utama:
- Member memiliki banyak Peminjaman
- Peminjaman memiliki banyak DetailPeminjaman
- DetailPeminjaman berelasi dengan Buku

app/Http/Controllers

Berisi logic aplikasi dan penghubung antara model dan view.

Controller yang digunakan:
- BukuController untuk mengelola data buku
- MemberController untuk mengelola data member
- PeminjamanController untuk mengelola transaksi peminjaman
- LoginController untuk autentikasi user

resources/views

Berisi tampilan antarmuka menggunakan Blade Template.

File utama:
- buku.blade.php untuk halaman buku
- member.blade.php untuk halaman member
- peminjaman.blade.php untuk halaman transaksi peminjaman
- welcome_role.blade.php untuk halaman utama (pemilihan role)
- katalog.blade.php untuk anggota melihat katalog buku

Tampilan menggunakan conditional rendering berdasarkan variabel mode, misalnya mode list dan mode create.

routes/web.php

Berisi routing aplikasi yang menghubungkan URL ke controller.

Contoh route:
/buku
/buku/create
/member
/member/create
/peminjaman
/peminjaman/create
/katalog

Struktur Database dan Relasi

Tabel yang digunakan dalam sistem:
- Buku
(Atribut: id, judul, penulis, stok)

- Member
(Atribut: id, nama, alamat, no_hp)

- Peminjaman
(Atribut: id, member_id, tanggal_pinjam, tanggal_kembali)

- DetailPeminjaman
(Atribut: id, peminjaman_id, buku_id)

- Users
(Atribut: id, name, username, password, role)

Relasi:
- Satu member dapat memiliki banyak transaksi peminjaman
- Satu transaksi peminjaman dapat memiliki banyak detail buku
- Setiap detail peminjaman mereferensikan satu buku

Relasi many-to-many antara peminjaman dan buku diimplementasikan melalui tabel detail_peminjaman sebagai tabel penghubung.

**Cara Menjalankan Aplikasi**

Persyaratan:
- PHP 8.x
- Composer
- MySQL (XAMPP)

Langkah-langkah:
- Clone repository dari GitHub
- Masuk ke folder project
- Jalankan perintah:
composer install
- Salin file .env:
cp .env.example .env
- Generate application key:
php artisan key:generate
- Atur konfigurasi database di file .env
- Jalankan migration:
php artisan migrate
- Jalankan server:
php artisan serve
- Akses aplikasi melalui browser pada alamat:
http://127.0.0.1:8000

**Tentang Folder Vendor**

Folder vendor tidak disertakan dalam repository GitHub karena ukuran file yang besar dan melebihi batas upload GitHub. Dependency Laravel dapat diinstal ulang menggunakan perintah composer install setelah project di-clone.

**Fitur Aplikasi**
- Pengelolaan data buku
- Pengelolaan data member
- Transaksi peminjaman buku
- Peminjaman lebih dari satu buku dalam satu transaksi
- Perhitungan otomatis tanggal kembali
- Penampilan status peminjaman berdasarkan tanggal

**Alur**
1. Alur Penggunaan Aplikasi
- Pengguna membuka aplikasi dan masuk ke halaman utama.
- Petugas dapat memilih menu Buku untuk mengelola data buku.
- Petugas dapat memilih menu Member untuk mengelola data anggota perpustakaan.
- Petugas dapat memilih menu Peminjaman untuk mencatat transaksi peminjaman buku.

  - Setiap menu memiliki:
    1. Halaman list untuk menampilkan data.
    2. Halaman create untuk menambahkan data baru.
  - Navigasi antar halaman dilakukan melalui:
    1. Menu navbar.
    2. Tombol Tambah pada masing-masing halaman.

2. Alur Proses Peminjaman Buku
- Petugas menekan tombol Tambah pada halaman Data Peminjaman.
- Sistem menampilkan form peminjaman.
- Petugas memilih member yang melakukan peminjaman.
- Petugas memilih satu atau lebih buku yang akan dipinjam.
- Sistem mengecek stok buku: Jika stok masih tersedia, buku dapat dipilih & Jika stok sudah habis, buku tidak dapat dipilih.
- Petugas menekan tombol Simpan.

Setelah data disimpan, sistem akan:

- Menyimpan data ke tabel peminjaman.
- Menyimpan detail buku ke tabel detail_peminjaman.
- Mengurangi stok pada tabel buku sesuai jumlah buku yang dipinjam.
- Menentukan tanggal kembali otomatis (7 hari setelah tanggal pinjam).

Pada halaman Data Peminjaman:
- Status peminjaman akan ditampilkan sebagai:
  Sedang Dipinjam & Overdue (jika lewat dari tanggal kembali)

**Penerapan Konsep OOP**
- Inheritance
Semua controller mewarisi class dasar Controller Laravel.

Contoh:
class BukuController extends Controller

- Polymorphism (Method Overriding)
Konsep polymorphism diterapkan pada pengelolaan stok buku dengan menggunakan pendekatan Strategy Pattern melalui beberapa class service, yaitu:
  - StokStrategy.php (sebagai parent / interface)
  - StokAda.php (implementasi ketika stok masih tersedia)
  - StokHabis.php (implementasi ketika stok sudah habis)
Class StokStrategy mendefinisikan method umum yang harus dimiliki oleh semua strategi stok, misalnya method untuk menentukan status atau tindakan terhadap stok buku.
Kemudian method tersebut dioverride (diimplementasikan ulang) pada class StokAda dan StokHabis dengan perilaku yang berbeda sesuai kondisi stok.

- Overloading
PHP tidak mendukung method overloading berdasarkan perbedaan parameter, sehingga konsep overloading tidak digunakan dalam project ini.

**Skenario Pengujian dan Hasil**
- Menambah buku,
  Hasil: Data buku tersimpan dan tampil di katalog.
- Menambah member,
  Hasil: Data member tersimpan dan tampil di daftar member.
- Menambah peminjaman buku,
  Hasil: Data peminjaman tersimpan dan stok buku berkurang.
- Meminjam lebih dari satu buku,
  Hasil: Semua buku tercatat pada detail peminjaman.
- Stok buku habis (0),
  Hasil: Buku tidak bisa dipilih untuk dipinjam.
- Cek status keterlambatan,
  Hasil: Status berubah menjadi overdue jika lewat tanggal kembali.
- Validasi input kosong,
  Hasil: Data tidak bisa disimpan jika form belum lengkap.
