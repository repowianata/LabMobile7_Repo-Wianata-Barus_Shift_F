# CARA KERJA LOGIN
Repo Wianata Barus
H1D022029
Shift Lama : C
Shift Baru : F

## 1. Set Up API

## Struktur Database

### Pembuatan Tabel `user`

```sql
CREATE TABLE user (
    username varchar(100) NOT NULL,
    password varchar(255) NOT NULL
);
```

### Menambahkan Data Awal ke dalam Tabel `user`

```sql
INSERT INTO user (username, password) VALUES ('tes', MD5('tes123'));
```

- **Tabel `user`**: Menyimpan informasi username dan password pengguna.
  - Kolom `username` digunakan untuk menyimpan nama pengguna.
  - Kolom `password` digunakan untuk menyimpan password yang sudah di-hash menggunakan fungsi `MD5`.
- **Contoh Data Awal**: `username` = "tes" dan `password` = hash dari "tes123".

## Konfigurasi API PHP

### Membuka Koneksi Database (`koneksi.php`)

```php
$con = mysqli_connect('localhost', 'root', '', 'coba-ionic') or die("koneksi gagal");
```

- **Deskripsi**: Membuat koneksi ke database MySQL menggunakan fungsi `mysqli_connect`.
  - **Host**: `'localhost'`
  - **Username**: `'root'`
  - **Password**: `''` (kosong)
  - **Database**: `'coba-ionic'`
- Jika koneksi gagal, akan menampilkan pesan "koneksi gagal".

### Endpoint Login (`login.php`)

Kode ini berfungsi sebagai endpoint API untuk memproses login.

#### 1. Mengatur Header untuk Cross-Origin Resource Sharing (CORS)

```php
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Credentials: true');
header('Access-Control-Allow-Methods: PUT, GET, HEAD, POST, DELETE, OPTIONS');
header('Content-Type: application/json; charset=utf-8');
header('Access-Control-Allow-Headers: X-Requested-With, Content-Type, Origin, Authorization, Accept, Client-Security-Token, Accept-Encoding');
```

- Header ini digunakan untuk mengizinkan akses lintas domain, pengaturan kredensial, dan metode HTTP yang diperbolehkan (misalnya, POST, GET).
- Format respons diset dalam JSON (`application/json`), yang memungkinkan API digunakan oleh berbagai aplikasi web.

#### 2. Mengambil Data JSON dari Input

```php
$input = file_get_contents('php://input');
$data = json_decode($input, true);
```

- **`file_get_contents('php://input')`**: Mengambil input mentah dari body request, biasanya dikirim oleh client dalam format JSON.
- **`json_decode($input, true)`**: Mengubah data JSON menjadi array asosiatif di PHP, sehingga dapat diakses melalui indeks (contoh: `$data['username']`).

#### 3. Menyimpan Data Login

```php
$username = trim($data['username']);
$password = md5(trim($data['password']));
```

- **`trim($data['username'])`**: Menghilangkan spasi di awal dan akhir dari input `username`.
- **`md5(trim($data['password']))`**: Meng-hash password yang dimasukkan untuk dibandingkan dengan data di database.

#### 4. Mengecek Kredensial Pengguna

```php
$query = mysqli_query($con, "select * from user where username='$username' and password='$password'");
$jumlah = mysqli_num_rows($query);
```

- **`mysqli_query`**: Menjalankan query SQL untuk mencari pengguna yang sesuai dengan `username` dan `password` di tabel `user`.
- **`mysqli_num_rows($query)`**: Menghitung jumlah baris hasil query. Jika hasilnya bukan 0, berarti kredensial cocok.

Berikut adalah README yang menjelaskan langkah-langkah dan cara kerja login di aplikasi Ionic menggunakan Angular tanpa menyertakan kode secara mendetail:

---

# Panduan Login di Aplikasi Ionic dengan Angular

## Deskripsi
Dokumentasi ini menjelaskan langkah-langkah dan cara kerja fitur login pada aplikasi Ionic berbasis Angular yang mengirim permintaan ke server API PHP untuk memverifikasi kredensial pengguna. Setelah login berhasil, aplikasi akan menyimpan token untuk sesi autentikasi.

## Langkah-langkah Implementasi

### 1. **Membuat Proyek Ionic**
- Buat proyek baru menggunakan perintah Ionic CLI.
- Konfigurasi proyek sesuai kebutuhan aplikasi Anda, termasuk pengaturan halaman login.

### 2. **Menyiapkan API PHP**
- Pastikan server sudah siap dengan API PHP untuk autentikasi pengguna.
- Endpoint API ini harus menerima username dan password, serta mengirimkan respons JSON yang menunjukkan status login (`berhasil` atau `gagal`) dan token autentikasi jika login berhasil.

### 3. **Membuat Halaman Login di Ionic**
- Buat halaman login di aplikasi Ionic yang berisi form input untuk `username` dan `password`.
- Tambahkan tombol "Login" untuk mengirim data input ke API saat ditekan.

### 4. **Mengatur Layanan HTTP di Angular**
- Buat layanan (service) HTTP di Angular untuk menghubungkan aplikasi dengan API PHP.
- Layanan ini akan bertanggung jawab mengirim permintaan POST ke endpoint API login di server.
- Jika login berhasil, respons dari server akan diterima dalam bentuk JSON.

### 5. **Mengirim Permintaan Login**
- Ketika pengguna memasukkan username dan password, aplikasi akan mengambil data input, lalu memanggil layanan HTTP yang telah dibuat.
- Data login dikirim dalam metode POST ke API.
- Setelah itu, aplikasi menerima respons dari server untuk mengecek apakah login berhasil atau gagal.

### 6. **Memproses Respons dari Server**
- Jika status respons adalah `berhasil`, simpan token autentikasi yang diberikan oleh server ke `localStorage` atau `sessionStorage` agar dapat digunakan untuk sesi berikutnya.
- Jika login gagal, tampilkan pesan error kepada pengguna.

### 7. **Mengelola Sesi Autentikasi**
- Gunakan token yang disimpan untuk menjaga sesi login pengguna.
- Saat pengguna melakukan logout, hapus token dari `localStorage` atau `sessionStorage` untuk mengakhiri sesi.

## Cara Kerja Sistem Login

1. **Pengguna Mengisi Form Login**: Pengguna memasukkan username dan password pada halaman login di aplikasi Ionic.

2. **Pengiriman Permintaan ke Server**: Aplikasi Ionic mengirimkan permintaan HTTP POST berisi data login ke endpoint API PHP.

3. **Verifikasi Kredensial di Server**: API PHP memverifikasi username dan password. Jika cocok, API mengirimkan respons sukses beserta token autentikasi.

4. **Penyimpanan Token di Aplikasi**: Jika login berhasil, aplikasi menyimpan token autentikasi di penyimpanan lokal (`localStorage` atau `sessionStorage`).

5. **Penggunaan Token**: Token digunakan untuk mengakses endpoint yang memerlukan autentikasi selama sesi aktif.

6. **Logout**: Ketika pengguna logout, aplikasi menghapus token untuk mengakhiri sesi.


#   L a b M o b i l e 7 _ R e p o - W i a n a t a - B a r u s _ S h i f t _ F 
 
 # ScreenShot
<img src="1.png" width="300px">
<img src="2.png" width="300px">
<img src="3.png" width="300px">
