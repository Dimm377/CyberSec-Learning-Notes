# Web Application Basics

---

- **Room Link:** [Web Application Basics](https://tryhackme.com/room/webapplicationbasics)
- **Category:** Web Hacking
- **Difficulty:** Easy

---

## Task 1: Introduction

Room ini bakal ngajarin kita basic-basic gimana sih cara kerja aplikasi web itu. Penting banget nih buat pondasi hacking.

**Apa aja yang bakal dipelajari:**

1.  Paham apa itu Web Application.
2.  Bedah apa yang ada di URL.
3.  Gimana cara HTTP kerja (Request & Response).
4.  Apa itu HTTP Methods dan kode-kode response-nya.
5.  Kenapa HTTP Headers dan Cookies itu penting.

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
  _(Clue: Tuan rumah)_

- **Question:** Which header identifies the client software (browser) making the request?
- **Answer:** ?
  _(Clue: Agen pengguna)_

- **Question:** What is the Content-Type for standard HTML form submissions?
- **Answer:** ?
  _(Clue: application/x-www-form-ur...)_

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
  - **200 OK:** Request sukses, ini yang kita harapkan.

- **3xx (Redirection):** "Pindah lapak ya."
  - **301 Moved Permanently:** Halaman udah pindah selamanya.
  - **302 Found:** Pindah sementara.

- **4xx (Client Error):** "Salah lu wir." (Error dari sisi kita/browser)
  - **400 Bad Request:** Request kita nggak jelas/ngaco.
  - **401 Unauthorized:** Belum login atau nggak boleh masuk.
  - **403 Forbidden:** Login sih udah, tapi tetep dilarang masuk (hak akses kurang).
  - **404 Not Found:** Halaman yang dicari nggak ada (klasik nih).

- **5xx (Server Error):** "Salah gue wir." (Error dari sisi server)
  - **500 Internal Server Error:** Server-nya error, codingan backend-nya mbledos.
  - **503 Service Unavailable:** Server lagi down atau keberatan beban.

**Answer the questions below:**

- **Question:** What part of an HTTP response provides the HTTP version, status code, and a brief explanation of the response's outcome?
- **Answer:** ?
  _(Clue: Baris paling atas di response)_

- **Question:** Which category of HTTP response codes indicates that the web server encountered an internal error?
- **Answer:** ?
  _(Clue: Kode kepala 5xx)_
