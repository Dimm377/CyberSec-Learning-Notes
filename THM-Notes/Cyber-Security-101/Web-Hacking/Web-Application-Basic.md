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
2.  Bedah anatomi URL.
3.  Gimana cara HTTP kerja (Request & Response).
4.  Apa itu HTTP Methods dan kode-kode response-nya.
5.  Kenapa HTTP Headers dan Cookies itu penting.

---

## Task 2: Web Application Overview

Intinya, aplikasi web itu kerjanya pakai konsep **Client-Server**. Ada yang minta (kita), ada yang ngelayanin (server).

**Analogi (Planet):**
Biar gampang, bayangin web app itu kayak sebuah planet 🌍.

- **Permukaan Planet (Front-End):** Bagian luar yang kelihatan, bisa kita injak, dan kita nikmati pemandangannya. Ini ibarat **Front-End** yang kita lihat di browser (tampilan website, tombol, warna-warni HTML/CSS).
- **Inti Planet (Back-End):** Bagian dalem planet yang nggak kelihatan tapi super penting. Di sana ada magma dan struktur geologi yang bikin planetnya tetep "hidup" dan utuh. Ini ibarat **Back-End** (server & database) yang ngerjain semua proses di balik layar.

**Komponen Utama:**

1.  **Front-End (Client-Side):**
    - Ini bagian yang kita liat dan klik-klik.
    - Jalan di browser laptop/hp kita.
    - Teknologinya: **HTML** (Kerangkanya), **CSS** (Bajunya/Tampilannya), **JavaScript** (Otaknya/Interaksinya).

2.  **Back-End (Server-Side):**
    - Bagian "dapur" yang ngeracik data & logika bisnis.
    - Isinya ada:
      - **Web Server:** Tukang kirim konten web ke kita (Contoh: Apache, Nginx).
      - **Database:** Gudang penyimpanan data (Contoh: MySQL, PostgreSQL).
      - **WAF (Web Application Firewall):** Satpam opsional yang jagain server dari serangan hacker.

**Answer the questions below:**

- **Question:** Which component on a computer is responsible for hosting and delivering content for web applications?
- **Answer:** ?
  *(Clue: Web Server)*

- **Question:** Which tool is used to access and interact with web applications?
- **Answer:** ?
  *(Clue: Web Browser)*

- **Question:** Which component acts as a protective layer, filtering incoming traffic to block malicious attacks, and ensuring the security of the web application?
- **Answer:** ?
  *(Clue: Web Application Firewall)*

---

## Task 3: Uniform Resource Locator (URL)

URL itu sebenernya alamat lengkap buat nyari resource di internet/web server. Ibaratnya kayak alamat rumah lengkap (Jalan, Nomor, Kota, Kode Pos).

**Bedah Anatomi URL:**
Misal ada URL: `https://www.tryhackme.com/room/network?join=true#task1`

1.  **Scheme / Protocol (`https://`):**
    - Aturan main komunikasinya.
    - **HTTP:** Biasa aja, nggak dienkripsi (bahaya buat kirim password).
    - **HTTPS:** Aman, dienkripsi (gembok ijo).

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
Ini teknik hacker yang beli domain mirip-mirip domain asli (misal `goggle.com` bukan `google.com`) buat nipu orang. Jadi mata harus jeli liat URL!

**Answer the questions below:**

- **Question:** Which protocol provides encrypted communication to ensure secure data transmission between a web browser and a web server?
- **Answer:** ?
  *(Clue: Protokol yang ada 'S'-nya, Secure)*

- **Question:** What term describes the practice of registering domain names that are misspelled variations of popular websites to exploit user errors and potentially engage in fraudulent activities?
- **Answer:** ?
  *(Clue: Typo...)*

- **Question:** What part of a URL is used to pass additional information, such as search terms or form inputs, to the web server?
- **Answer:** ?
  *(Clue: Yang diawali tanda tanya `?`)*

---

## Task 4: HTTP Messages

HTTP itu kayak bahasa yang dipake Client dan Server buat ngobrol. Obrolan mereka bentuknya pesan teks sederhana.

Ada dua jenis pesan utama:

1.  **HTTP Request (Permintaan):**
    - Pesan yang dikirim **Client (Browser)** ke Server.
    - Isinya: "Eh server, gue minta halaman ini dong!" atau "Nih gue kirim data login".

2.  **HTTP Response (Jawaban):**
    - Pesan balasan dari **Server** ke Client.
    - Isinya: "Oke, ini halamannya" atau "Waduh, file-nya nggak ketemu (404)".

**Struktur Pesan HTTP:**

Sebuah pesan HTTP biasanya punya format kayak gini:

1.  **Start Line:** Baris pertama. Isinya jenis request (GET/POST) atau status response (200 OK).
2.  **Headers:** Info tambahan. Kayak metadata (Jenis browser, bahasa, cookies, dll).
3.  **Empty Line (Baris Kosong):** Ini PENTING! Pemisah antara Header dan Body.
4.  **Body:** Isinya data benerannya (misal kode HTML halaman web, atau data form yang kita kirim).

**Answer the questions below:**

- **Question:** Which HTTP message is returned by the web server after processing a client's request?
- **Answer:** ?
  *(Clue: Kebalikannya Request)*

- **Question:** What follows the headers in an HTTP message?
- **Answer:** ?
  *(Clue: Pemisah sebelum body)*
