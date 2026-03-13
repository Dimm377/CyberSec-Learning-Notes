# TryHackMe: Web Application Basics

* **Room Link:** [Web Application Basics](https://tryhackme.com/room/webapplicationbasics)
* **Category:** Web Hacking
* **Difficulty:** Easy

---

## Introduction

_Room_ ini memberikan dasar-dasar tentang cara kerja aplikasi web. Ini adalah pondasi penting sebelum kamu masuk ke materi pengujian keamanan web.

(Penjelasan mendalam tentang protokol HTTP ada di catatan [HTTP In Details](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Pre-Security/How-The-Web-Works/HTTP-In-details.md). Untuk memahami sisi database, cek [Sql Fundamental](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Cyber-Security-101/Web-Hacking/Sql-Fundamental.md))

### Learning Objective:
* Memahami apa itu Aplikasi Web.
* Membedah komponen-komponen URL.
* Memahami cara kerja protokol HTTP (Request & Response).
* Mengenal HTTP Methods dan Status Codes.
* Memahami peran HTTP Headers dan _Cookies_.

---

## Web Application Overview

Aplikasi web bekerja dengan arsitektur **Client-Server**. Client (browser) mengirimkan permintaan, lalu Server memproses dan mengirimkan balasan.

### Komponen Utama:

1.  **Front-End (Sisi Client):**
* Bagian yang terlihat dan bisa diinteraksikan langsung oleh pengguna.
* Berjalan di dalam browser.
* Teknologi utama: **HTML** (Struktur), **CSS** (Tampilan), dan **JavaScript** (Logika Interaktif).

2.  **Back-End (Sisi Server):**
* Bagian yang mengelola logika aplikasi dan pemrosesan data di balik layar.
* Terdiri dari:
* **Web Server:** Mengirimkan konten web ke client (Contoh: Apache, Nginx).
* **Database:** Penyimpanan data terstruktur (Contoh: MySQL, PostgreSQL).
* **Web Application Firewall (WAF):** Lapisan keamanan untuk menyaring traffic berbahaya.

---

## Uniform Resource Locator (URL)

URL itu pada dasarnya alamat lengkap untuk mencari resource di internet/web server. Ibaratnya seperti alamat rumah lengkap (Jalan, Nomor, Kota, Kode Pos).

**Bedah Anatomi URL:**
Misal ada URL: `https://www.tryhackme.com/room/network?join=true#task1`

1.  **Scheme / Protocol (`https://`):**
* Aturan main komunikasinya.
* **HTTP:** Biasa saja, tidak dienkripsi (berbahaya untuk mengirim password).
* **HTTPS:** Aman, dienkripsi.

2.  **User (`user:password@`):**
* Jarang dipakai sekarang. Dulu untuk login langsung lewat URL (misal FTP).

3.  **Host / Domain (`www.tryhackme.com`):**
* Nama server yang kita tuju. Ini di-resolve jadi IP Address sama DNS.

4.  **Port (`:443`):**
* Pintu masuk ke server. Default-nya HTTP=80, HTTPS=443 (biasanya otomatis, tidak terlihat di URL).

5.  **Path (`/room/network`):**
* Lokasi file atau halaman yang kita minta di server itu.

6.  **Query String (`?join=true`):**
* Info tambahan yang kita kirim ke server. Mulai pakai tanda tanya `?`.

7.  **Fragment (`#task1`):**
* Penunjuk lokasi spesifik di dalam halaman itu sendiri (scroll otomatis ke bagian tertentu).

**Hati-hati dengan Typosquatting!**
Ini teknik di mana penyerang membeli domain yang mirip dengan domain asli (misal `goggle.com` alih-alih `google.com`) untuk menipu pengguna. Jadi kamu harus jeli memperhatikan URL-nya.

**Answer the questions below:**

* **Question:** Which protocol provides encrypted communication to ensure secure data transmission between a web browser and a web server?
* **Answer:** ?

* **Question:** What term describes the practice of registering domain names that are misspelled variations of popular websites to exploit user errors and potentially engage in fraudulent activities?
* **Answer:** ?

* **Question:** What part of a URL is used to pass additional information, such as search terms or form inputs, to the web server?
* **Answer:** ?

---

## HTTP Messages

HTTP itu bahasa yang dipakai Client dan Server untuk berkomunikasi. Bentuknya berupa pesan teks sederhana.

Ada dua jenis pesan utama:

1.  **HTTP Request (Permintaan):**
* Pesan yang dikirim **Client (Browser)** ke Server.
* Contoh: "Server, minta halaman ini" atau "Ini data login yang saya kirim".

2.  **HTTP Response (Jawaban):**
* Pesan balasan dari **Server** ke Client.
* Contoh: "Oke, ini halamannya" atau "File yang dicari tidak ada (404)".

**Struktur Pesan HTTP:**

Sebuah pesan HTTP biasanya punya format seperti ini:

1.  **Start Line:** Baris pertama. Isinya jenis request (GET/POST) atau status response (200 OK).
2.  **Headers:** Info tambahan. Seperti metadata (Jenis browser, bahasa, cookies, dll).
3.  **Empty Line (Baris Kosong):** Ini PENTING! Pemisah antara Header dan Body.
4.  **Body:** Isinya data benerannya (misal kode HTML halaman web, atau data form yang kita kirim).

---

## HTTP Request (Method & Request Line)

Komponen paling atas dari HTTP Request adalah **Request Line**. Isinya ada 3 bagian penting:

`GET /room/webapplicationbasics HTTP/1.1`

1.  **HTTP Method (`GET`):** Kata kerja yang menunjukkan tindakan yang diminta.
* **GET:** Meminta data (membuka website, download file).
* **POST:** Mengirim data baru (mengisi form, upload file).
* **PUT:** Memperbarui data yang sudah ada.
* **DELETE:** Menghapus data.
* **HEAD:** Hanya meminta header-nya saja (untuk mengecek file ada/tidak tanpa men-download isinya).
* **OPTIONS:** Menanyakan ke server method apa saja yang diizinkan.

2.  **Path (`/room/webapplicationbasics`):**
* Halaman atau file spesifik yang diminta di server.

3.  **HTTP Version (`HTTP/1.1`):**
* Versi protokol-nya. **HTTP/1.1** adalah yang paling standar dan umum dipakai saat ini.

---

## HTTP Request (Headers & Body)

Setelah baris pertama (Request Line), ada **Headers** dan **Body**.

### 1. HTTP Headers

Ini seperti label pada paket kiriman. Memberikan info tambahan tentang request-nya.
Contoh header yang sering muncul:

* **Host:** Domain tujuan permintaan (wajib disertakan).
* **User-Agent:** Informasi tentang jenis browser atau perangkat client.
* **Referer:** Alamat halaman sebelumnya yang mengarahkan kamu ke halaman saat ini.
* **Cookie:** Token identitas yang digunakan server untuk mengenali sesi pengguna.

---

## HTTP Response (Status Line & Codes)

Saat server membalas request, dia mengirimkan **HTTP Response**. Baris pertamanya disebut **Status Line**.

Contoh Status Line:
`HTTP/1.1 200 OK`

Isinya ada 3 bagian:

1.  **HTTP Version:** Protokol yang dipakai (biasanya HTTP/1.1).
2.  **Status Code:** Angka yang menunjukkan hasil request (sukses/gagal).
3.  **Status Message:** Penjelasan singkat dari kode tersebut (misal "OK", "Not Found").

**Kamus Status Code (Wajib Kamu Hafal):**

* **1xx (Informational):** "Bentar ya Sedang diproses."
* Jarang sekali terlihat langsung.

* **2xx (Success):** "Berhasil!"
* **200 OK:** Request sukses.

* **3xx (Redirection):** "Dipindahkan."
* **301 Moved Permanently:** Halaman sudah pindah secara permanen.
* **302 Found:** Pindah sementara.

* **4xx (Client Error):** Error dari sisi client/browser.
* **400 Bad Request:** Request tidak valid.
* **401 Unauthorized:** Belum login atau tidak punya izin.
* **403 Forbidden:** Sudah login, tapi tetap tidak diizinkan mengakses.
* **404 Not Found:** Halaman yang dicari tidak ada.

* **5xx (Server Error):** Error dari sisi server.
* **500 Internal Server Error:** Server mengalami kesalahan internal.
* **503 Service Unavailable:** Server sedang down atau kelebihan beban.

---

## HTTP Response (Headers & Body)

Sama seperti Request, Response juga punya **Headers** dan **Body**.

### 1. HTTP Response Headers

Memberikan informasi tambahan dari server ke browser.
Contoh penting:

* **Server:** Menunjukkan software apa yang dipakai server (misal Apache/Nginx). _Berbahaya jika versinya terekspos, karena penyerang bisa mencari celahnya._
* **Set-Cookie:** Server memerintahkan browser untuk menyimpan cookie.
* **Secure:** Cookie hanya boleh dikirim lewat HTTPS (agar aman).
* **HttpOnly:** Cookie tidak bisa diakses oleh JavaScript (mencegah serangan XSS).
* **Content-Length:** Ukuran file/halaman yang dikirim.
* **Content-Type:** Jenis file yang dikirim (misal `text/html` atau `application/json`).

### 2. HTTP Response Body

Isi surat balesan dari server. Biasanya berupa:

* Kode HTML halaman webnya.
* File gambar/PDF.
* Data JSON (kalo API).

---

## Security Headers

Selain header pasif biasa, ada juga sosok pelindung bernama **Security Headers**. Mereka bertugas menangkis gempuran serangan _web_ seperti XSS atau Clickjacking, pastikan kamu tidak meremehkan keberadaan mereka saat *pentesting*.

**Beberapa Security Header Penting:**

1.  **Strict-Transport-Security (HSTS):**
* Header ini memaksa browser untuk SELALU menggunakan HTTPS saat mengakses website.
* Jika pengguna mengetik `http://`, browser akan otomatis mengubahnya jadi `https://`.
* Agar mencakup subdomain juga, biasanya ditambahkan opsi `includeSubDomains`.

2.  **Content-Security-Policy (CSP):**
* Ini adalah security header paling kuat untuk sisi client.
* Dia mengontrol resource apa saja yang boleh di-load oleh browser.
* Misal: "Hanya boleh memuat gambar dari domain sendiri, script dari google.com diizinkan, sisanya diblokir".
* Sangat berguna untuk mencegah **XSS (Cross-Site Scripting)**.
* Directive penting:
* `script-src`: Mengatur sumber script (JavaScript) yang boleh dijalankan.
* `img-src`: Mengatur sumber gambar.

3.  **X-Content-Type-Options:**
* Terkadang browser mencoba menebak tipe file yang dikirim server, padahal bisa berbahaya (misal file teks ditebak sebagai HTML lalu dieksekusi).
* Header ini memerintahkan browser untuk membaca tipe file sesuai `Content-Type` yang diberikan server, tanpa menebak-nebak (*sniffing*).
* Nilainya hanya satu: `nosniff`.

4.  **X-Frame-Options:**
* Mencegah website kamu di-embed di dalam `<iframe>` website lain.
* Ini untuk mencegah serangan **Clickjacking** (penyerang membuat layer transparan di atas website untuk menipu pengguna agar mengklik tombol tertentu).

---

## Conclusion

**Ringkasan yang sudah kita pelajari:**

1.  **Web App:** Interaksi Client-Server (Planet Analogy).
2.  **HTTP:** Bahasa komunikasinya (Request & Response).
3.  **URL:** Alamat lengkap resource.
4.  **Methods:** GET, POST, PUT, DELETE (Cara kita ngomong).
5.  **Headers:** Info tambahan (Cookie, User-Agent, Security Headers).
6.  **Status Codes:** 200 (OK), 404 (Not Found), 500 (Error).

Pondasi ini akan sangat terpakai untuk materi selanjutnya seperti **OWASP Top 10**, **Burp Suite**, atau **Web Hacking** lainnya.

---
