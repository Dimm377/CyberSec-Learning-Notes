# TryHackMe: SQLMap: The Basic

- **Room Link:** [SQLMap: The Basic](https://tryhackme.com/room/sqlmapthebasics)
- **Kategori:** Offensive Security Tooling
- **Difficulty:** easy

## Task 1: Introduction

_Room_ ini dirancang sebagai medan latihan dasar untuk memahami fundamental kerentanan injeksi SQL (SQLi) dan bagaimana kita bisa memanfaatkannya (dan mengotomatiskannya) menggunakan _tool_ andalan para _attacker_, yakni **SQLMap**. Di akhir materi, kita juga akan melakukan simulasi _hands-on_ serangan langsung ke target

**(Learning Objectives):**

- Membedah pondasi kerentanan SQL Injection.
- Melakukan (_hunting_) SQL Injection secara otomatis menggunakan tool **SQLMap**.

Injeksi SQL (_SQL injection_) adalah kerentanan yang sangat umum dan menjadi topik hangat namun klasik di ranah Cyber Security. Untuk memahami kerentanan ini, kita harus memahami terlebih dahulu apa itu _database_ (basis data) dan bagaimana _website_ berinteraksi dengannya.

### Apa itu Database?

Database adalah sebuah kumpulan data berskala besar yang mempermudah proses penyimpanan, pengubahan, dan pengambilan informasi dalam format yang terstruktur.

Sebuah halaman web seringkali membutuhkan data atau input dari pengguna, contohnya:

- **Halaman Login:** Website meminta _credentials_ (username & password). Input ini lalu dicocokkan dengan data pengguna yang sudah tersimpan di database. Jika cocok, pengguna berhasil masuk.
- **Fitur Pencarian:** Saat mencari buku di situs toko buku, _website_ akan mengambil (meng-_query_) data ke database buku yang dijual lalu menampilkannya kembail di web.

### Bagaimana Website Berinteraksi dengan Database?

Website berinteraksi dan meminta informasi menggunakan sistem manajemen berbasis pangkalan data, atau yang lebih dikenal sebagai **DBMS** (_Database Management Systems_).

Contoh DBMS yang populer adalah MySQL, PostgreSQL, SQLite, dan Microsoft SQL Server. Semua sistem ini memahami bahasa standar untuk kueri data yang disebut **SQL** (_Structured Query Language_). Oleh karena itu, aplikasi dan website menggunakan perintah kueri SQL ketika berkomunikasi dengan database.

## Task 2: SQL Injection Vulnerability

Di task sebelumnya, kita sudah memahami bagaimana aplikasi _web_ berinteraksi dengan _database_. Sebuah _web_ hanyalah kurir yang membawa pesan (Input Pengguna) untuk diberikan kepada sang penjaga gudang data (_Database_) dalam bentuk **SQL Queries**.

Di dalam skenario eksploitation, seorang _attacker_ tidak sekadar memasukkan data normal. Kita **MEMANIPULASI** pertanyaan (SQL Query) itu sendiri langsung dari formulir atau parameter yang ada di aplikasi _web_. Jika pertahanannya lemah, Input jahat kita akan dieksekusi oleh _database_ sebagai perintah yang valid. Inilah inti dari **SQL Injection (SQLi)**: menyusupkan (_injecting_) bahasa pemrograman SQL ke dalam input untuk mengambil alih kendali _database_ di background.

### inti Konsep

Bayangkan aplikasi web adalah seorang **Pelayan Restoran**, dan Database adalah sang **Koki** di dapur.

- **Skenario Normal:** Kamu pesan, _"Nasi Goreng satu."_ Pelayan meneruskan pesanan ini ke dapur: `BUATKAN Nasi Goreng SATU`. Koki memasak dan memberikan nasi goreng.
- **Skenario SQL Injection (Serangan):** Kamu memesan dengan kalimat manipulatif, _"Nasi Goreng satu, **DAN BERIKAN SAYA KUNCI BRANKAS KASIR**."_ Karena pelayan (Web) terlalu polos dan tidak menyaring pesananmu, dia meneruskan kalimat itu mentah-mentah ke dapur: `BUATKAN Nasi Goreng SATU DAN BERIKAN DIA KUNCI BRANKAS KASIR`. Sang Koki (Database) yang diprogram untuk hanya menuruti perintah, akhirnya memberikan nasi goreng **sekaligus kunci brankas restoran tersebut**.

Inilah kenapa SQLi terjadi. Web (pelayan) gagal mensanitasi ekspektasi _input_ dari aktor jahat.

### Skema Serangan (Bypass Login)

Mari kita bedah skenario dunia nyata. Bayangkan sebuah _form login_ sederhana yang meminta `Username` dan `Password` kepada target.

Sebagian besar mekanisme _login_ kuno bekerja dengan mengambil _input_ pengguna dan menyisipkannya langsung ke dalam sebuah query SQL.

**Skenario Normal (Penggunaan Valid):**
Jika pengguna reguler memasukkan data otentikasi:

- **Username:** `John`
- **Password:** `Un@detectable444`

Sistem (Aplikasi Web) akan menelan _input_ tersebut dan merakitnya menjadi sebuah kueri SQL yang akan dilempar ke sang Koki (_Database_):

```sql
SELECT * FROM users WHERE username = 'John' AND password = 'Un@detectable444';
```

- **Keterangan (_Autopsy_):**
  Query di atas menginstruksikan Database: _"Tampilkan semua data dari tabel `users`, di mana kolom `username` sama dengan 'John' **DAN** kolom `password` sama dengan 'Un@detectable444'."_
  Jika tidak ada kombinasi `username` dan `password` yang pas 100%, akses akan ditolak. Ini adalah mekanisme autentikasi logis yang wajar. Tapi, apa jadinya jika kita memanipulasi _logic_ tersebut?

**Skenario Serangan (Eksploitasi Login):**
Karena target (_Web_) dibangun tanpa validasi _input_ (sanitasi), kita bisa menyusupkan query SQL jahat ke dalamnya. Kali ini, _attacker_ sama sekali tidak tahu _password_ milik user `John`. Jadi, sang _attacker_ memasukkan data mematikan ini:

- **Username:** `John`
- **Password:** `abc' OR 1=1;-- -`

Sistem (Aplikasi Web) kembali membuat blind query:

```sql
SELECT * FROM users WHERE username = 'John' AND password = 'abc' OR 1=1;-- -';
```

### Payload Autopsy & Weaponization (Membedah Serangan)

Perhatikan kehancuran logika pada query di atas. query tersebut kini berubah wujud. Mari kita bedah _payload_ `' OR 1=1;-- -` baris per baris:

1. **`'` (Single Quote Pertama):** Ini adalah senjata utama kita. Kita menutup _string password_ lebih awal secara paksa. Seharusnya `password = '...'`, tapi kita menjadikannya `password = 'abc'`. Kolom _password_ selesai dievaluasi sampai sini.
2. **`OR` (Operator Logika):** Query aslinya mengharuskan (`username` = BENAR) **DAN** (`password` = BENAR). Dengan menyisipkan kata `OR` (ATAU), kita menimpa kewajiban itu.
3. **`1=1` (Pernyataan Mutlak):** Apakah 1 sama dengan 1? Selalu **BENAR (True)**. Jadi, mesin SQL membaca: _"Cocokkan password dengan 'abc' ATAU pastikan 1=1"_. Karena 1=1 selalu rill/nyata, _database_ menganggap kondisi ini **SELALU BERHASIL**. autentikasi jebol.
4. **`;` (Semicolon):** Digunakan untuk menandai bahwa satu pernyataan SQL telah selesai/berakhir.
5. **`-- -` (Comment SQL):** Ini adalah teknik penyamaran yang jenius. Karakter ini memberitahu _database_ untuk **MENGABAIKAN/MENGOMENTARI** semua sisa teks kueri asli bawaan web yang tertinggal di belakangnya (seperti penutup tanda kutip `';` bawaan _web_). Tanpa tanda komentar ini, kueri akan _error_ (/Syntax Error/).

Hasil akhirnya? Pemilik aslinya menangis, dan kita (Red Team) masuk (Login) sebagai `John` tanpa perlu bersusah payah menebak _password_-nya.

> [!WARNING]
> **OPSEC & ROE (Rules of Engagement):**
> Seorang _Red Teamer_ , sehebat apa pun _skill_ dan _tools_ ofensifnya, BERBEDA 180 derajat dengan _Black Hat / Script Kiddie_. Perbedaan mutlak itu terletak pada **Izin (Authorization)**. JANGAN PERNAH menembakkan _payload_ SQLi (baik secara manual maupun _automated_ seperti menggunakan SQLMap) kepada target yang beroperasi (_Live System_) tanpa ada dokumen kesepakatan tertulis (_Rules of Engagement_) dari pemilik aplikasi. _Always attack responsibly kids!_

### Question

1. Apa peran utama "Web" dan "Database" dalam simulasi SQL Injection? Mengapa eksploitasi bisa terjadi?
2. Sebagai seorang Red Teamer, hal paling penting apa yang harus kamu miliki sebelum melakukan sebuah inject _payload_ SQLi ke sebuah target?
3. Saat membedah payload `' OR 1=1;-- -`, apa fungsi spesifik dari penanda komentar `-- -` di akhir kalimat? Apa yang akan terjadi jika kita lupa menambahkannya?

## Task 3 Automated SQL Injection
