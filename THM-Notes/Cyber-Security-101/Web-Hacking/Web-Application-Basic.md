# TryHackMe: Web Application Basics

- **Room Link:** [Web Application Basics](https://tryhackme.com/room/webapplicationbasics)
- **Category:** Web Hacking
- **Difficulty:** Easy

---

## Introduction

_Room_ ini memberikan dasar-dasar mengenai mekanisme kerja aplikasi web, yang merupakan fondasi krusial bagi aktivitas pengujian keamanan web.

### Learning Objective:
- Memahami pengertian Aplikasi Web.
- Membedah komponen-komponen yang terdapat pada URL.
- Memahami cara kerja protokol HTTP (Permintaan & Tanggapan).
- Mengenal Metode HTTP dan kode status tanggapan.
- Memahami peran penting Header HTTP dan _Cookies_.

---

## Web Application Overview

Aplikasi web beroperasi menggunakan arsitektur **Client-Server**. Klien (seperti peramban web) mengirimkan permintaan, sementara peladen (_server_) memproses dan memberikan tanggapan.

### Komponen Utama:

1.  **Front-End (Sisi Klien):**
    - Bagian yang dapat dilihat dan berinteraksi langsung dengan pengguna.
    - Beroperasi di dalam peramban web pengguna.
    - Teknologi utama: **HTML** (Struktur), **CSS** (Presentasi), dan **JavaScript** (Logika Interaktif).

2.  **Back-End (Sisi Peladen):**
    - Bagian yang mengelola logika aplikasi dan pengolahan data di balik layar.
    - Terdiri dari:
      - **Web Server:** Mengirimkan konten web kepada klien (Contoh: Apache, Nginx).
      - **Database:** Penyimpanan data terstruktur (Contoh: MySQL, PostgreSQL).
      - **Web Application Firewall (WAF):** Lapisan keamanan untuk menyaring lalu lintas berbahaya ke peladen.

---

## Uniform Resource Locator (URL)

URL itu sebenernya alamat lengkap buat mencari resource di internet/web server. Ibaratnya seperti alamat rumah lengkap (Jalan, Nomor, Kota, Kode Pos).

**Bedah Anatomi URL:**
Misal ada URL: `https://www.tryhackme.com/room/network?join=true#task1`

1.  **Scheme / Protocol (`https://`):**
    - Aturan main komunikasinya.
    - **HTTP:** Biasa saja, tidak dienkripsi (bahaya buat kirim password).
    - **HTTPS:** Aman, dienkripsi .

2.  **User (`user:password@`):**
    - Jarang dipake sekarang. Dulu buat login langsung lewat URL (misal FTP).

3.  **Host / Domain (`www.tryhackme.com`):**
    - Nama server yang kita tuju. Ini di-resolve jadi IP Address sama DNS.

4.  **Port (`:443`):**
    - Pintu masuk ke server. Defaultnya HTTP=80, HTTPS=443 (biasanya disembunyiin/otomatis).

5.  **Path (`/room/network`):**
    - Lokasi file atau halaman yang kita minta di server itu.

6.  **Query String (`?join=true`):**
    - Info tambahan yang kita kirim ke server. Mulai pakai tanda tanya `?`.

7.  **Fragment (`#task1`):**
    - Penunjuk lokasi spesifik di dalam halaman itu sendiri (scroll otomatis ke bagian tertentu).

**Hati-hati sama Typosquatting!**
Ini teknik hacker yang beli domain mirip-mirip domain asli (misal `goggle.com` bukan `google.com`) buat nipu orang. Jadi mata harus jeli untuk melihat url nya

**Answer the questions below:**

- **Question:** Which protocol provides encrypted communication to ensure secure data transmission between a web browser and a web server?
- **Answer:** ?

- **Question:** What term describes the practice of registering domain names that are misspelled variations of popular websites to exploit user errors and potentially engage in fraudulent activities?
- **Answer:** ?

- **Question:** What part of a URL is used to pass additional information, such as search terms or form inputs, to the web server?
- **Answer:** ?

---

## HTTP Messages

HTTP itu bahasa yang dipake Client dan Server buat ngobrol. komunikasi mereka bentuknya pesan teks sederhana.

Ada dua jenis pesan utama:

1.  **HTTP Request (Permintaan):**
    - Pesan yang dikirim **Client (Browser)** ke Server.
    - Isinya: "Eh server, minta halaman ini dong!" atau "Nih gw kirim data login".

2.  **HTTP Response (Jawaban):**
    - Pesan balasan dari **Server** ke Client.
    - Isinya: "Oke, ini halamannya wir" atau "Waduh, file-nya tidak ketemu le (404)".

**Struktur Pesan HTTP:**

Sebuah pesan HTTP biasanya punya format seperti ini:

1.  **Start Line:** Baris pertama. Isinya jenis request (GET/POST) atau status response (200 OK).
2.  **Headers:** Info tambahan. Seperti metadata (Jenis browser, bahasa, cookies, dll).
3.  **Empty Line (Baris Kosong):** Ini PENTING! Pemisah antara Header dan Body.
4.  **Body:** Isinya data benerannya (misal kode HTML halaman web, atau data form yang kita kirim).

**Answer the questions below:**

- **Question:** Which HTTP message is returned by the web server after processing a client's request?
- **Answer:** ?

- **Question:** What follows the headers in an HTTP message?
- **Answer:** ?

---

## HTTP Request (Method & Request Line)

membahas komponen paling atas dari HTTP Request, yaitu **Request Line**. Isinya ada 3 bagian penting:

`GET /room/webapplicationbasics HTTP/1.1`

1.  **HTTP Method (`GET`):** Kata kerja yang nunjukin kita mau ngapain.
    - **GET:** Minta data (buka website, download file).
    - **POST:** Mengirim data baru (isi form, upload file).
    - **PUT:** Update data yang sudah ada.
    - **DELETE:** Hapus data.
    - **HEAD:** Cuma minta header-nya saja (buat mengecek file ada/tidak tanpa download isinya).
    - **OPTIONS:** Nanya ke server "eh, method apa saja sih yang boleh dipake di sini?".

2.  **Path (`/room/webapplicationbasics`):**
    - Halaman atau file spesifik yang kita minta di server itu.

3.  **HTTP Version (`HTTP/1.1`):**
    - Versi protokol-nya. **HTTP/1.1** itu yang paling standar dan umum dipake sekarang.

---

## HTTP Request (Headers & Body)

Setelah baris pertama (Request Line), ada **Headers** dan **Body**.

### 1. HTTP Headers

Ini seperti label paketan. Memberi tau info tambahan soal paketnya.
Contoh header yang sering muncul:

- **Host:** Domain tujuan permintaan (Wajib disertakan).
- **User-Agent:** Menyediakan informasi mengenai jenis peramban atau perangkat klien.
- **Referer:** Alamat asal tautan sebelum menuju ke halaman saat ini.
- **Cookie:** Token identitas yang digunakan peladen untuk mengenali sesi pengguna.

---

## HTTP Response (Status Line & Codes)

Pas server ngebales request kita, dia kirim **HTTP Response**. Baris pertamanya namanya **Status Line**.

Contoh Status Line:
`HTTP/1.1 200 OK`

Isinya ada 3 macem:

1.  **HTTP Version:** Protokol yang dipake (biasanya HTTP/1.1).
2.  **Status Code:** Angka ajaib yang nunjukin hasil requestnya (sukses/gagal).
3.  **Status Message:** Penjelasan singkat dari kodenya (misal "OK", "Not Found").

**Kamus Status Code (Wajib Hapal dikit-dikit):**

- **1xx (Informational):** "Bentar ya, lagi diproses."
  - Jarang banget kita melihat langsung.

- **2xx (Success):** "Mantap, berhasil!"
  - **200 OK:** Request sukses

- **3xx (Redirection):** "Pindah page ya."
  - **301 Moved Permanently:** Halaman sudah pindah selamanya.
  - **302 Found:** Pindah sementara.

- **4xx (Client Error):** "Salah kamu wir." (Error dari sisi kita/browser)
  - **400 Bad Request:** Request tidak jelas/ngaco.
  - **401 Unauthorized:** Belum login atau tidak boleh masuk,
  - **403 Forbidden:** Login sudah, tapi tetep dilarang masuk (kamu siapa mpruy).
  - **404 Not Found:** Halaman yang dicari tidak ada (klasik nih).

- **5xx (Server Error):** "Salah saya wir." (Error dari sisi server)
  - **500 Internal Server Error:** Server-nya error, codingan backend-nya ngawur.
  - **503 Service Unavailable:** Server lagi down atau keberatan beban (gara ga ada dana).

---

## HTTP Response (Headers & Body)

Sama seperti Request, Response juga punya **Headers** dan **Body**.

### 1. HTTP Response Headers

Buat memberi informasi tambahan dari server ke browser.
Contoh penting:

- **Server:** Memberi tau server pakai software apa (misal Apache/Nginx). _Bahaya kalo ketauan version-nya, bisa dicari celahnya!_
- **Set-Cookie:** Server memerintah browser buat menyimpan tiket/cookie.
  - **Secure:** Cookie cuma boleh dikirim lewat HTTPS (agar aman).
  - **HttpOnly:** Cookie tidak boleh diakses sama JavaScript (agar tidak kena hack XSS).
- **Content-Length:** Ukuran file/halaman yang dikirim.
- **Content-Type:** Jenis file yang dikirim (misal `text/html` atau `application/json`).

### 2. HTTP Response Body

Isi surat balesan dari server. Biasanya berupa:

- Kode HTML halaman webnya.
- File gambar/PDF.
- Data JSON (kalo API).

---

## Security Headers

Selain header biasa, ada juga **Security Headers** yang tugasnya jadi tameng tambahan buat website kita. Ini penting banget buat ngehalau serangan seperti XSS, Clickjacking, dll.

**Beberapa Security Header Penting:**

1.  **Strict-Transport-Security (HSTS):**
    - Header ini maksa browser buat SELALU pakai HTTPS kalo mau mengakses website kita.
    - Jadi kalo ada user iseng ngetik `http://`, browser bakal otomatis mengubah jadi `https://`.
    - Agar aman total (termasuk subdomain), biasanya ditambahin opsi `includeSubDomains`.

2.  **Content-Security-Policy (CSP):**
    - Ini rajanya security header buat client-side.
    - Dia ngontrol resource apa saja yang boleh diload sama browser.
    - Misal: "Cuma boleh load gambar dari domain saya tsendiri, script dari google.com boleh, sisanya BLOKIR!".
    - Berguna banget buat nyegah **XSS (Cross-Site Scripting)**.
    - Directive penting:
      - `script-src`: Ngatur sumber script (JavaScript) yang boleh jalan.
      - `img-src`: Ngatur sumber gambar.

3.  **X-Content-Type-Options:**
    - Kadang browser itu "sotoy". Kita kirim file text, dia malah nebak itu HTML trus dijalanin. Bahaya kan?
    - Nah, header ini bilang ke browser: "Eh, jangan sotoy! Baca tipe file sesuai yang saya kasih tau saja (`Content-Type`). Jangan ditebak-tebak (sniffing)".
    - Valuenya cuma satu: `nosniff`.

4.  **X-Frame-Options:**
    - Buat nyegah website kita di-embed di dalam `<iframe>` website orang lain.
    - Ini buat nyegah serangan **Clickjacking** (orang membuat layer transparan di atas website kita buat nipu user suruh klik tombol).

---

## Conclusion

**Ringkasan yang sudah kita pelajari:**

1.  **Web App:** Interaksi Client-Server (Planet Analogy).
2.  **HTTP:** Bahasa komunikasinya (Request & Response).
3.  **URL:** Alamat lengkap resource.
4.  **Methods:** GET, POST, PUT, DELETE (Cara kita ngomong).
5.  **Headers:** Info tambahan (Cookie, User-Agent, Security Headers).
6.  **Status Codes:** 200 (OK), 404 (Not Found), 500 (Error).

Pondasi ini bakal kepake banget buat materi selanjutnya seperti **OWASP Top 10**, **Burp Suite**, atau **Web Hacking** lainnya. Jangan lupa santuy dan terus belajar!

---
