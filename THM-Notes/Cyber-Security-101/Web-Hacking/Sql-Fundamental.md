# TryHackMe - Sql Fundamental

- **Room Link:** [Sql Fundamental](https://tryhackme.com/room/sqlfundamentals)
- **Category:** Web Hacking
- **Difficulty:** Easy

## Task 1: Introduction

Di sisi offensive database dapat membantu kita memahami kerentanan SQL dengan lebih baik, seperti SQL injection, dan membuat Query yang membantu kita memanipulasi atau mengambil data di dalam layanan yang telah disusupi. Di sisi lain, pada sisi defensif, database dapat membantu kita menelusuri basis data dan menemukan aktivitas mencurigakan atau informasi relevan; database juga dapat membantu kita melindungi layanan dengan lebih baik dengan menerapkan batasan saat dibutuhkan

Karena database ada di mana-mana, penting untuk memahaminya, Kita akan mempelajari dasar-dasar database, mencakup istilah-istilah penting, konsep, dan tipe-tipe yang berbeda sebelum memahami SQL

**Learning Objectives:**

- Paham apa itu database, serta istilah dan konsep utama
- Memahami berbagai jenis database
- Paham apa itu SQL
- Memahami dan mampu menggunakan Operasi SQL CRUD **(Create, Read, Update, Delete)**
- Paham cara pakai SQL Clauses **(perintah tambahan untuk menyaring data)**
- Paham cara menjalankan berbagai **Operations**, **Operators**, dan **Functions** di dalam SQL

## Task 2: Databases 101

### Introducing Databases

Seperti yang sudah dijelaskan di task 1, Database itu ada di hampir setiap sistem, jadi besar kemungkinan kita sering berinteraksi dengan layanan yang menggunakannya sehari-hari, database adalah kumpulan informasi atau data terstruktur terorganisir yang mudah diakses dan dapat dimanipulasi atau dianalisis, data tersebut bisa dalam berbagai bentuk misal authentication user (nama pengguna dan password), yang disimpan dan diperiksa saat login ke dalam aplikasi atau situs (seperti TryHackMe, dll), data yang dibuat pengguna di media sosial (Seperti Instagram dan Facebook) tempat data kayak postingan pengguna, komentar, suka, dll dikumpulkan dan disimpan, serta informasi lalui riwayat tontonan yang disimpan oleh layanan streaming seperti Netflix dan digunakan buat menghasilkan rekomendasi

### Different Types of Databases

Ada cukup banyak jenis database yang bisa dibuat, tapi untuk ini, kita hanya fokus pada dua tipe utama: **Relational Databases (atau SQL)** vs **Non-Relational Databases (atau NoSQL)**

<p align="center">
<img src="../../../Assets/Images/database-type.png" width="400">
</p>

**Relational Databases:** Menyimpan data yang terstruktur, yang artinya setiap data yang masuk harus mengikuti pola atau aturan tertentu, Contohnya data user itu isinya wajib ada `nama_depan`, `nama_belakang`, `email`, `username`, sama `password`. Relational databases paling cocok untuk Menyimpan Data Terstruktur, menghubungkan Antar Data yang Kompleks, dan Sistem Autentikasi & Kontrol Akses

```SQL
-- Membuat tabel user dengan struktur yang tetap
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email_address VARCHAR(100),
    occupation VARCHAR(100)
);

-- Memasukkan data (entry) ke dalam tabel mengikuti struktur tersebut
INSERT INTO users (user_id, first_name, last_name, email_address, occupation)
VALUES (1, 'Thomas', 'Anderson', 'neo@matrix.com', 'Cyber Security');

INSERT INTO users (user_id, first_name, last_name, email_address, occupation)
VALUES (2, 'John', 'Doe', 'doe@thm.ac.id', 'Student');

-- Membuat tabel relasi (contoh: riwayat login)
CREATE TABLE login_attempts (
    attempt_id INT PRIMARY KEY,
    user_id INT,
    status VARCHAR(20),
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```

**Non-Relational Databases:** Kalau SQL tadi ibarat tabel Excel yang kaku, NoSQL ini lebih ke arah folder berisi dokumen-dokumen yang isinya bisa beda-beda formatnya, Cocok banget kalau kita punya data yang isinya bervariasi. Contohnya, hasil scan dokumen yang tipe dan jumlah datanya beda-beda setiap lembar, jadi butuh database yang nggak memaksa data harus masuk ke kolom tertentu

```JSON
{
    "_id": ObjectId("65ccba12f3d4567890abcdef"),
    "nama": { "first_name": "John", "last_name": "Doe" },
    "jurusan": "Teknik Informatika",
    "hobi": ["Cyber Security", "Gaming", "Gym"],
    "stats_game": {
        "level": 7,
        "rank": "Newbie",
        "total_playtime_seconds": NumberLong(12500430)
    },
    "is_active": true
}
```

### Tables, Rows and Columns

Semua data yang disimpan dalam relational database akan disimpan di sebuah tabel, misalnya kumpulan buku yang ada di toko buku maka disimpan dalam tabel bernama “Buku”

<p align="center">
<img src="../../../Assets/Images/TRC.png" width="400">
</p>

Saat merancang tabel, kita perlu menetapkan informasi apa saja yang dibutuhkan—seperti `id`, `Nama`, dan `tanggal_terbit` yang nantinya akan menjadi kolom. Setiap kolom ini wajib memiliki tipe data tertentu. Jika ada data yang masuk dengan tipe yang tidak sesuai, sistem secara otomatis akan menolaknya untuk menjaga konsistensi data, secara umum ada empat tipe data inti yang selalu digunakan: **String (teks dan karakter)**, **Integer (angka bulat)**, **Float/Desimal (angka pecahan)**, serta **Date/Time** (waktu dan tanggal).

setelah tabel dan kolomnya selesai dibuat, kita bisa mulai memasukkan data pertama ke database. Misalnya, untuk buku `Android Security Internals` dengan ID `1` dan tanggal terbit `14-10-2014`. Setelah berhasil dimasukkan, seluruh informasi ini akan tersimpan dan muncul sebagai satu baris **(row)** utuh dalam tabel tersebut.

### Primary and Foreign Keys

Ketika data semakin banyak, kita bisa memisahkan informasi ke berbagai tabel, contohnya tabel `Buku` dan tabel `Penulis`, Agar informasi di kedua tabel ini tetap sinkron misalnya kita ingin tau buku tertentu ditulis oleh siapa—kita harus menghubungkan keduanya melalui kunci **(Keys)**. Ada dua jenis kunci yang perlu dipahami:

<p align="center">
<img src="../../../Assets/Images/PK-key.png" width="400">
</p>

- **Primary Key:** digunakan untuk menjamin bahwa data dalam kolom tertentu bersifat unik. Artinya, setiap baris dalam tabel harus memiliki satu nilai khusus yang menjadi pengenal tetap dan tidak boleh sama dengan data lainnya. Dalam setiap tabel, kita wajib memilih satu kolom sebagai Primary Key, biasanya kolom `id` adalah pilihan terbaik karena nilai ini dibuat khusus untuk setiap entry (satu baris di dalam tabel), sementara judul atau tanggal bisa saja memiliki kemiripan.

- **Foreign Key:** adalah kolom (atau kumpulan kolom) yang berfungsi sebagai penghubung antara dua tabel dalam sebuah database. Contohnya, kita bisa menambahkan kolom`id_penulis` di tabel Buku, kolom ini bertindak sebagai **Foreign Key** karena nilainya merujuk langsung ke kolom id di tabel Penulis, Foreign Key inilah yang memungkinkan terciptanya hubungan antar tabel dalam relational database ,dalam satu tabel kita boleh memiliki lebih dari satu Foreign Key.
