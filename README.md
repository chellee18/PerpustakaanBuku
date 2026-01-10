# Sistem Informasi Peminjaman Buku Perpustakaan Berbasis Web (Laravel)

Project ini merupakan aplikasi web berbasis Laravel yang digunakan untuk mengelola data buku, data member, serta transaksi peminjaman buku pada perpustakaan. Sistem dikembangkan menggunakan arsitektur MVC (Model–View–Controller) dan database MySQL.

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

Struktur Database dan Relasi

Tabel yang digunakan dalam sistem:
- Buku
Atribut: id, judul, penulis, stok

- Member
Atribut: id, nama, alamat, no_hp

- Peminjaman
Atribut: id, member_id, tanggal_pinjam, tanggal_kembali

- DetailPeminjaman
Atribut: id, peminjaman_id, buku_id

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

**Penerapan Konsep OOP**
- Inheritance
Semua controller mewarisi class dasar Controller Laravel.

Contoh:
class BukuController extends Controller

- Polymorphism (Method Overriding)
Beberapa controller memiliki method dengan nama yang sama seperti index(), create(), dan store(), namun dengan implementasi yang berbeda sesuai dengan kebutuhan masing-masing fitur. Hal ini merupakan contoh polymorphism melalui overriding method.

- Overloading
PHP tidak mendukung method overloading berdasarkan perbedaan parameter, sehingga konsep overloading tidak digunakan dalam project ini.
