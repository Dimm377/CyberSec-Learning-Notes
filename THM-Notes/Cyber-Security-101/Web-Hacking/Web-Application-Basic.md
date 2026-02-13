# Web Application Basics

---

- **Room Link:** [Web Application Basics](https://tryhackme.com/room/webapplicationbasics)
- **Category:** Web Hacking
- **Difficulty:** Easy

---

## Task 1: Introduction

Room ini bakal ngajarin kita basic-basic gimana sih cara kerja aplikasi web itu. Penting banget nih buat pondasi hacking.

**Learning Objectives:**

- Paham apa itu Web Application.
- Bedah apa yang ada di URL.
- Gimana cara HTTP bekerja (Request & Response).
- Apa itu HTTP Methods dan kode-kode response-nya.
- Kenapa HTTP Headers dan Cookies itu penting.

---

## Task 2: Web Application Overview

Intinya, aplikasi web itu kerjanya pakai konsep **Client-Server**. Ada yang minta (kita), ada yang ngelayanin (server).

**Analogi (Planet):**
Biar gampang, bayangin web app itu seperti sebuah planet.

- **Permukaan Planet (Front-End):** Bagian luar yang kelihatan, bisa kita injak, dan kita nikmati pemandangannya. Ini ibarat **Front-End** yang kita lihat di browser (tampilan website, tombol, warna-warni HTML/CSS).
- **Inti Planet (Back-End):** Bagian dalem planet yang nggak kelihatan tapi super penting. Di sana ada magma dan struktur geologi yang bikin planetnya tetep hidup dan utuh. Ini ibarat **Back-End** (server & database) yang mengerjakan semua proses di balik layar.

**Komponen Utama:**

1.  **Front-End (Client-Side):**
    - Ini bagian yang kita liat dan bisa kita interaksi.
    - Jalan di browser laptop/hp kita.
    - Teknologinya: **HTML** (Kerangkanya), **CSS** (Bajunya/Tampilannya), **JavaScript** (Otaknya/Interaksinya).

2.  **Back-End (Server-Side):**
    - Bagian dapur yang ngeracik data & logika sebuah aplikasi.
    - Isinya ada:
      - **Web Server:** Tukang kirim konten web ke kita (Contoh: Apache, Nginx).
      - **Database:** Gudang penyimpanan data (Contoh: MySQL, PostgreSQL).
      - **WAF (Web Application Firewall):** Satpam opsional yang jagain server dari serangan hacker, sebenarnya bukan opsional kalo buat zaman sekarang wkwk

**Answer the questions below:**

- **Question:** Which component on a computer is responsible for hosting and delivering content for web applications?
- **Answer:** ?
  _(Clue: Web Server)_

- **Question:** Which tool is used to access and interact with web applications?
- **Answer:** ?
  _(Clue: Web Browser)_

- **Question:** Which component acts as a protective layer, filtering incoming traffic to block malicious attacks, and ensuring the security of the web application?
- **Answer:** ?
  _(Clue: Web Application Firewall)_

---

## Task 3: Uniform Resource Locator (URL)

URL itu sebenernya alamat lengkap buat nyari resource di internet/web server. Ibaratnya kayak alamat rumah lengkap (Jalan, Nomor, Kota, Kode Pos).

**Bedah Anatomi URL:**
Misal ada URL: `https://www.tryhackme.com/room/network?join=true#task1`

1.  **Scheme / Protocol (`https://`):**
    - Aturan main komunikasinya.
    - **HTTP:** Biasa aja, nggak dienkripsi (bahaya buat kirim password).
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
    - Info tambahan yang kita kirim ke server. Mulai pake tanda tanya `?`.

7.  **Fragment (`#task1`):**
    - Penunjuk lokasi spesifik di dalam halaman itu sendiri (scroll otomatis ke bagian tertentu).

**Hati-hati sama Typosquatting!**
Ini teknik hacker yang beli domain mirip-mirip domain asli (misal `goggle.com` bukan `google.com`) buat nipu orang. Jadi mata harus jeli untuk melihat url nya

**Answer the questions below:**

- **Question:** Which protocol provides encrypted communication to ensure secure data transmission between a web browser and a web server?
- **Answer:** ?
  _(Clue: Protokol yang ada 'S'-nya)_

- **Question:** What term describes the practice of registering domain names that are misspelled variations of popular websites to exploit user errors and potentially engage in fraudulent activities?
- **Answer:** ?
  _(Clue: Typo...)_

- **Question:** What part of a URL is used to pass additional information, such as search terms or form inputs, to the web server?
- **Answer:** ?
  _(Clue: Yang diawali tanda tanya `?`)_

---

## Task 4: HTTP Messages

HTTP itu bahasa yang dipake Client dan Server buat ngobrol. komunikasi mereka bentuknya pesan teks sederhana.

Ada dua jenis pesan utama:

1.  **HTTP Request (Permintaan):**
    - Pesan yang dikirim **Client (Browser)** ke Server.
    - Isinya: "Eh server, minta halaman ini dong!" atau "Nih gw kirim data login".

2.  **HTTP Response (Jawaban):**
    - Pesan balasan dari **Server** ke Client.
    - Isinya: "Oke, ini halamannya wir" atau "Waduh, file-nya nggak ketemu le (404)".

**Struktur Pesan HTTP:**

Sebuah pesan HTTP biasanya punya format seperti ini:

1.  **Start Line:** Baris pertama. Isinya jenis request (GET/POST) atau status response (200 OK).
2.  **Headers:** Info tambahan. Kayak metadata (Jenis browser, bahasa, cookies, dll).
3.  **Empty Line (Baris Kosong):** Ini PENTING! Pemisah antara Header dan Body.
4.  **Body:** Isinya data benerannya (misal kode HTML halaman web, atau data form yang kita kirim).

**Answer the questions below:**

- **Question:** Which HTTP message is returned by the web server after processing a client's request?
- **Answer:** ?
  _(Clue: Kebalikan Request)_

- **Question:** What follows the headers in an HTTP message?
- **Answer:** ?
  _(Clue: Pemisah sebelum body)_

---

## Task 5: HTTP Request (Method & Request Line)

membahas komponen paling atas dari HTTP Request, yaitu **Request Line**. Isinya ada 3 bagian penting:

`GET /room/webapplicationbasics HTTP/1.1`

1.  **HTTP Method (`GET`):** Kata kerja yang nunjukin kita mau ngapain.
    - **GET:** Minta data (buka website, download file).
    - **POST:** Ngirim data baru (isi form, upload file).
    - **PUT:** Update data yang udah ada.
    - **DELETE:** Hapus data.
    - **HEAD:** Cuma minta header-nya doang (buat ngecek file ada/nggak tanpa download isinya).
    - **OPTIONS:** Nanya ke server "eh, method apa aja sih yang boleh dipake di sini?".

2.  **Path (`/room/webapplicationbasics`):**
    - Halaman atau file spesifik yang kita minta di server itu.

3.  **HTTP Version (`HTTP/1.1`):**
    - Versi protokol-nya. **HTTP/1.1** itu yang paling standar dan umum dipake sekarang.

**Answer the questions below:**

- **Question:** What is the most common HTTP version used?
- **Answer:** ?
  _(Clue: HTTP yang umum dipakai)_

- **Question:** What HTTP verb is used to request the options available for a given service?
- **Answer:** ?
  _(Clue: Method untuk nanya opsi?)_

- **Question:** Which part of the HTTP request indicates the resource the client wants to access?
- **Answer:** ?
  _(Clue: Bagian tengah di request line)_

---

## Task 6: HTTP Request (Headers & Body)

Setelah baris pertama (Request Line), ada **Headers** dan **Body**.

### 1. HTTP Headers

Ini kayak label paketan. Ngasih tau info tambahan soal paketnya.
Contoh header yang sering muncul:

- **Host:** Wajib ada! Ngasih tau kita mau ke domain mana (misal `tryhackme.com`).
- **User-Agent:** Identitas browser yang kita pake (misal Chrome, Firefox, atau script Python/curl).
- **Referer:** Ngasih tau kita dateng dari link mana (asal muasal).
- **Accept-Encoding:** Client bilang "gue bisa baca file yang dikompres pake format gzip lho".
- **Cookie:** Tiket/token identitas kita (biar server tau kita udah login).

### 2. HTTP Body

Ini isi paketannya. Biasanya dipake kalo kita kirim data (pake method POST).
Format isinya wajib dikasih tau lewat header **Content-Type**:

**a. application/x-www-form-urlencoded**
Format paling standar, mirip query string.

```http
POST /profile HTTP/1.1
Host: tryhackme.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 33

name=Aleksandra&age=27&country=US
```

**b. multipart/form-data**
Buat upload file. Ada batas pemisah (boundary) antar datanya.

```http
POST /upload HTTP/1.1
Host: tryhackme.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

aleksandra
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"
Content-Type: image/jpeg

[Binary Data Here representing the image]
----WebKitFormBoundary7MA4YWxkTrZu0gW--
```

**c. application/json**
Format data modern, pake kurung kurawal `{}`. Sering dipake API.

```http
POST /api/user HTTP/1.1
Host: tryhackme.com
Content-Type: application/json

{
    "name": "Aleksandra",
    "age": 27,
    "country": "US"
}
```

**d. application/xml**
Format agak jadul, pake tag HTML-style `<>`.

```http
POST /api/user HTTP/1.1
Host: tryhackme.com
Content-Type: application/xml

<user>
    <name>Aleksandra</name>
    <age>27</age>
    <country>US</country>
</user>
```

**Answer the questions below:**

- **Question:** What header is used to specify the domain name of the web server?
- **Answer:** ?

- **Question:** Which header identifies the client software (browser) making the request?
- **Answer:** ?

- **Question:** What is the Content-Type for standard HTML form submissions?
- **Answer:** ?

---

## Task 7: HTTP Response (Status Line & Codes)

Pas server ngebales request kita, dia kirim **HTTP Response**. Baris pertamanya namanya **Status Line**.

Contoh Status Line:
`HTTP/1.1 200 OK`

Isinya ada 3 macem:

1.  **HTTP Version:** Protokol yang dipake (biasanya HTTP/1.1).
2.  **Status Code:** Angka ajaib yang nunjukin hasil requestnya (sukses/gagal).
3.  **Status Message:** Penjelasan singkat dari kodenya (misal "OK", "Not Found").

**Kamus Status Code (Wajib Hapal dikit-dikit):**

- **1xx (Informational):** "Bentar ya, lagi diproses."
  - Jarang banget kita liat langsung.

- **2xx (Success):** "Mantap, berhasil!"
  - **200 OK:** Request sukses

- **3xx (Redirection):** "Pindah page ya."
  - **301 Moved Permanently:** Halaman udah pindah selamanya.
  - **302 Found:** Pindah sementara.

- **4xx (Client Error):** "Salah lu wir." (Error dari sisi kita/browser)
  - **400 Bad Request:** Request nggak jelas/ngaco.
  - **401 Unauthorized:** Belum login atau nggak boleh masuk,
  - **403 Forbidden:** Login udah, tapi tetep dilarang masuk (lu siapa mpruy).
  - **404 Not Found:** Halaman yang dicari nggak ada (klasik nih).

- **5xx (Server Error):** "Salah gue wir." (Error dari sisi server)
  - **500 Internal Server Error:** Server-nya error, codingan backend-nya ngawur.
  - **503 Service Unavailable:** Server lagi down atau keberatan beban (gara ga ada dana).

**Answer the questions below:**

- **Question:** What part of an HTTP response provides the HTTP version, status code, and a brief explanation of the response's outcome?
- **Answer:** ?
  _(Clue: Baris paling atas di response)_

- **Question:** Which category of HTTP response codes indicates that the web server encountered an internal error?
- **Answer:** ?
  _(Clue: Kode 5xx)_

---

## Task 8: HTTP Response (Headers & Body)

Sama kayak Request, Response juga punya **Headers** dan **Body**.

### 1. HTTP Response Headers

Buat ngasih informasi tambahan dari server ke browser.
Contoh penting:

- **Server:** Ngasih tau server pake software apa (misal Apache/Nginx). _Bahaya kalo ketauan version-nya, bisa dicari celahnya!_
- **Set-Cookie:** Server nyuruh browser buat nyimpen tiket/cookie.
  - **Secure:** Cookie cuma boleh dikirim lewat HTTPS (biar aman).
  - **HttpOnly:** Cookie nggak boleh diakses sama JavaScript (biar gak kena hack XSS).
- **Content-Length:** Ukuran file/halaman yang dikirim.
- **Content-Type:** Jenis file yang dikirim (misal `text/html` atau `application/json`).

### 2. HTTP Response Body

Isi surat balesan dari server. Biasanya berupa:

- Kode HTML halaman webnya.
- File gambar/PDF.
- Data JSON (kalo API).

**Answer the questions below:**

- **Question:** Which HTTP response header can reveal information about the web server's software and version, potentially exposing it to security risks if not removed?
- **Answer:** ?
  _(Clue: Header nama software server)_

- **Question:** Which flag should be added to cookies in the Set-Cookie HTTP response header to ensure they are only transmitted over HTTPS, protecting them from being exposed during unencrypted transmissions?
- **Answer:** ?
  _(Clue: Flag biar aman/secure)_

- **Question:** Which flag should be added to cookies in the Set-Cookie HTTP response header to prevent client-side scripts (like JavaScript) from accessing them, thereby mitigating XSS attacks?
- **Answer:** ?
  _(Clue: Cuma HTTP aja)_

---

## Task 9: Security Headers

Selain header biasa, ada juga **Security Headers** yang tugasnya jadi tameng tambahan buat website kita. Ini penting banget buat ngehalau serangan kayak XSS, Clickjacking, dll.

**Beberapa Security Header Penting:**

1.  **Strict-Transport-Security (HSTS):**
    - Header ini maksa browser buat SELALU pake HTTPS kalo mau ngakses website kita.
    - Jadi kalo ada user iseng ngetik `http://`, browser bakal otomatis ngubah jadi `https://`.
    - Biar aman total (termasuk subdomain), biasanya ditambahin opsi `includeSubDomains`.

2.  **Content-Security-Policy (CSP):**
    - Ini rajanya security header buat client-side.
    - Dia ngontrol resource apa aja yang boleh diload sama browser.
    - Misal: "Cuma boleh load gambar dari domain gue tsendiri, script dari google.com boleh, sisanya BLOKIR!".
    - Berguna banget buat nyegah **XSS (Cross-Site Scripting)**.
    - Directive penting:
      - `script-src`: Ngatur sumber script (JavaScript) yang boleh jalan.
      - `img-src`: Ngatur sumber gambar.

3.  **X-Content-Type-Options:**
    - Kadang browser itu "sotoy". Kita kirim file text, dia malah nebak itu HTML trus dijalanin. Bahaya kan?
    - Nah, header ini bilang ke browser: "Eh, jangan sotoy! Baca tipe file sesuai yang gue kasih tau aja (`Content-Type`). Jangan ditebak-tebak (sniffing)".
    - Valuenya cuma satu: `nosniff`.

4.  **X-Frame-Options:**
    - Buat nyegah website kita di-embed di dalam `<iframe>` website orang lain.
    - Ini buat nyegah serangan **Clickjacking** (orang bikin layer transparan di atas website kita buat nipu user suruh klik tombol).

**Answer the questions below:**

- **Question:** In a Content Security Policy (CSP) configuration, which property can be set to define where scripts can be loaded from?
- **Answer:** ?
  _(Clue: src untuk script)_

- **Question:** When configuring the Strict-Transport-Security (HSTS) header to ensure that all subdomains of a site also use HTTPS, which directive should be included to apply the security policy to both the main domain and its subdomains?
- **Answer:** ?
  _(Clue: include+Sub+Domains)_

- **Question:** Which HTTP header directive is used to prevent browsers from interpreting files as a different MIME type than what is specified by the server, thereby mitigating content type sniffing attacks?
- **Answer:** ?
  _(Clue: no...)_

---

## Task 10: Practical Task: Making HTTP Requests

Nah, sekarang saatnya praktek! Di task ini kita bakal mainan sama **Mini Emulator** buat simulasi kirim request ke API.

**Skenario:**
Kita punya akses ke sistem manajemen user sederhana.

### 1. GET Request (Ambil Data)

Misi: Ambil daftar semua user.

- **Method:** GET
- **URL/Endpoint:** `/api/users`
- **Caranya:** Pilih method GET, ketik endpoint-nya, klick Send.
- **Hasil:** Dapet list user + Flag 1.

### 2. POST Request (Update Data)

Misi: Ubah data user "Bob" (ID 2) biar negaranya jadi "US".

- **Method:** POST
- **URL/Endpoint:** `/api/user/2`
  - _Angka `2` di URL itu ID-nya si Bob. Jadi kita gak perlu tulis "Bob" di datanya._
- **Body/Data:** `country=US`
- **Caranya:**
  - Pilih method POST.
  - Ketik endpoint-nya `/api/user/2`.
  - Di bagian **Parameters** (atau Body):
    - **Key:** `country`
    - **Value:** `US`
  - Klik Send.
- **Hasil:** Data keupdate + Flag 2.

### 3. DELETE Request (Hapus Data)

Misi: Hapus user dengan ID 1.

- **Method:** DELETE
- **URL/Endpoint:** `/api/user/1`
- **Caranya:** Pilih method DELETE, ketik endpoint-nya, klik Send.
- **Hasil:** User kehapus + Flag 3.

**Answer the questions below:**

- **Question:** Make a GET request to /api/users. What is the flag?
- **Answer:** ?

- **Question:** Make a POST request to /api/user/2 and update the country of Bob from UK to US. What is the flag?
- **Answer:** ?

- **Question:** Make a DELETE request to /api/user/1 to delete the user. What is the flag?
- **Answer:** ?

---

## Task 11: Conclusion


**Ringkasan yang udah kita pelajari:**

1.  **Web App:** Interaksi Client-Server (Planet Analogy).
2.  **HTTP:** Bahasa komunikasinya (Request & Response).
3.  **URL:** Alamat lengkap resource.
4.  **Methods:** GET, POST, PUT, DELETE (Cara kita ngomong).
5.  **Headers:** Info tambahan (Cookie, User-Agent, Security Headers).
6.  **Status Codes:** 200 (OK), 404 (Not Found), 500 (Error).

Pondasi ini bakal kepake banget buat materi selanjutnya kayak **OWASP Top 10**, **Burp Suite**, atau **Web Hacking** lainnya. Jangan lupa santuy dan terus belajar! ☕

---
