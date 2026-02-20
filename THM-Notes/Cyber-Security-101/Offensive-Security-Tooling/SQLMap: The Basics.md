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

Eksploitasi SQL Injection secara manual (terutama jenis _blind_) memang sangat penting untuk memahami fondasinya. Tapi di lapangan, ngekstrak _database_ segede gaban secara manual bakal ngabisin banyak tenaga dan waktu banget, Bro. Di situlah kita main pakai **SQLMap**.

### Apa itu SQLMap?

Ibaratnya, SQLMap itu _sniper rifle_ otomatis buat urusan SQL Injection. Ini adalah _tool command-line open-source_ sakti yang dirancang khusus untuk mengotomatisasi proses pendeteksian dan eksploitasi kerentanan SQLi. Tool ini bahkan udah bawaan (_built-in_) di beberapa OS Linux _pentesting_.

### Menggunakan SQLMap

Karena ini murni _command-line tool_, kamu harus buka terminal OS Linux lo. Lo bisa pakai _flag_ `--help` buat ngeliat deretan fungsi dan parameter buas yang bisa kamu setel. Tapi, buat pemula atau kalau kamu pengen panduan anti-ribet, ada _flag_ andalan:

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

> **(Red Team Perspective - OPSEC):**
> Mengeksekusi SQLMap pakai insting barbar (_default parameters_) ibarat maling teriak-teriak masuk rumah orang. SQLMap itu **SANGAT BERISIK**. Kalau nge-_scan_ ke target production yang dipasang WAF (Web Application Firewall) atau dimonitor sama _Blue Team_ (SOC), IP kamu bakal kena blokir atau di-_flag_ seketika. Kenapa? Karena _log_ server mereka bakal meledak nerima ratusan sampai ribuan _request payload_ SQL aneh secara massal.
>
> Hacker (Red Team) bakal main _stealth_. Mereka memodifikasi _flag_ SQLMap kayak ngatur jeda batas waktu (_rate limiting_ pakai `--delay`), mengacak User-Agent (`--random-agent`), nyamperin dari banyak koneksi beda pakai proxy list, biar serangannya terlihat seperti jejak _traffic_ user normal dan bikin pusing para analis forensik si Blue Team.

---

### Data Extraction

Kalau kerentanan udah ketemu, saatnya panen data. SQLMap punya _flag_ spesifik dari ngambil nama database sampai membongkar isi tabelnya:

- `--dbs`: Mengekstrak/menampilkan semua nama _database_ yang ada di dalam server.
- `-D <nama_database> --tables`: Setelah kamu tau nama database target, gunakan _flag_ ini untuk mengintip semua tabel di dalam database tersebut.
- `-D <nama_database> -T <nama_tabel> --dump`: _The Holy Grail_. Digunakan untuk memuntahkan (dumping) seluruh isi _records/entries_ di dalam tabel spesifik tersebut.

### Titik Injeksi (Injection Points)

Secara umum, _hunting_ kerentanan ini dimulai dengan mencari celah URL yang nampung _input_ data (parameter):

1. **HTTP GET-Based Testing:**
   URL yang pakai parameter GET biasanya gampang terlihat di _address bar_. Contoh: `http://sqlmaptesting.thm/search?cat=1`. Di sini SQLMap bakal ngehajar parameter `cat` dengan berbagai payload SQLi buat nguji seberapa kuat sanitasi _backend_-nya. Cukup lempar URL ini pakai _flag_ `-u`.

   ```bash
   sqlmap -u "http://sqlmaptesting.thm/search?cat=1"
   ```

2. **Authenticated Scanning (Cookie-Based Testing):**
   Di dunia nyata, jalur fatal yang rentan SQLi seringkali _di belakang pintu masuk_ (contoh: dashboard admin, profile user), yang artinya kamu harus **_login_** dulu. Kalau kamu langsung pakai sqlmap biasa, _request_ aneh bakal nyangkut di halaman login (unauthenticated).

   **Solusi:** Lo harus tangkep (capture) dan injek _Session Cookie_ (seperti `PHPSESSID`, `JSESSIONID`, dll) yang valid ke dalam SQLMap pakai _flag_ `--cookie`. Jadi, SQLMap bakal "menyamar" jadi diri user yang udah _login_.

   ```bash
   sqlmap -u "http://target.com/admin/search?id=1" --cookie="SESSIONID=abcdef123456"
   ```

---

### Membedah Output SQLMap (Terminal Autopsy)

Ketika berhasil nge-_run_ SQLMap dan celahnya terkonfirmasi (Vulnerable), terminal bakal ngeluarin banyak harta karun informasi. Gimana cara bacanya?

![SQLMap Testing URL](/home/dimm/PlayGround/CyberSec-Learning-Notes/THM-Notes/Cyber-Security-101/Offensive-Security-Tooling/assets/sqlmap_testing_url.png) _(Catatan: Gambar referensi output)_

Perhatikan baris-baris krusial ini:

1. **WAF/IPS/IDS Check (`[INFO] checking if the target is protected...`)**
   SQLMap secara pinter bakal nyari tau dulu apakah ada "satpam" (Firewall/IPS) yang jagain website itu sebelum mulai ngegas payload yang berisik.

2. **Injection Point Confirmation (`GET parameter 'cat' is vulnerable...`)**
   Kalau dapet pesan ini, _JACKPOT!_ Artinya parameter `cat` terbukti bisa disuntik. SQLMap bakal nanya: _Do you want to keep testing the others?_ (Kalau _target_ lu parameter tunggal, jawab aja **N** / No buat irit waktu).

3. **Payload Autopsy (Tipe Serangan yang Berhasil)**
   SQLMap dengan baik hatinya bakal ngasih tau kamu _exact payload_ apa yang sukses merobohkan _backend_-nya. Misal di _output_:
   - **Type: boolean-based blind** (Ngetes dengan kondisi Benar/Salah).
   - **Type: error-based** (Memaksa database ngeluarin _error message_ yang isinya data dicolong).
   - **Type: UNION query** (Teknik menggabungkan _output_ tabel).
     _(Ini berguna banget kalau kamu mau masukin celah ini ke laporan Pentest, kamu tinggal_ copas payload _aslinya)._

4. **Fingerprinting (Mengintai Spesifikasi Target)**
   Di bagian bawah, SQLMap bakal ngoceh soal identitas asli si _Koki_ (Server & Database):
   - **back-end DBMS:** (Misal: MySQL >= 5.1). Ini ngasih tau kamu jenis dan versi _database_.
   - **web server operating system:** (Misal: Linux Ubuntu). Lo tau OS servernya.
   - **web application technology:** (Misal: Nginx, PHP 5.6.40). Lo tau web server dan bahasa pemrogramannya.

   _(Pengetahuan OS & Tech Stack ini krusial buat Red Team kalau skenarionya udah mau eskalasi ke pengambilalihan server/RCE)._

---

**Question**

1. Dalam skenario nyata, kenapa kita memakai SQLMap daripada mengeksploitasi kerentanan SQLi secara manual murni?
2. Jika kamu adalah anggota _Red Team_, kenapa menggunakan SQLMap _default_ ke target itu ide yang buruk, dan taktik apa yang bakal kamu ubah?
3. Urutkan _flag_ SQLMap yang benar (dari struktur tertinggi ke terendah) jika kita ingin mendapatkan isi dari sebuah spesifik _table_!
4. Target kamu ternyata memiliki celah pencarian (search) yang rentan di halaman _Settings_. Masalahnya, halaman itu hanya bisa diakses kalau kamu udah _login_. _Flag_ apa yang harus kamu bawa di SQLMap agar serangan ini berhasil nembus layar autentikasi?
5. Saat SQLMap menemukan celah, ada bagian informasi _Fingerprinting_ (seperti OS, _Tech Stack_, DBMS version) yang ikut bocor. Buat seorang _Attacker/Red Team_, kenapa mendapatkan info OS _server target_ itu sangat berharga?
