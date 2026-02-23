# TryHackMe: How The Web Works


---

- **Room Link:** [How The Web Works](https://tryhackme.com/room/howwebsiteswork)
- **Category:** How The Web Works
- **Difficulty:** easy

---

# Overview

Belajar proses fundamental dalam pembuatan situs web, termasuk gimana browser berinteraksi sama web server.

### How The Web Works ?

Pada dasarnya, web bekerja pake model **Client-Server**. Ini interaksi terus-menerus antara browser yang lagi kamu pake sama komputer di lokasi lain yang nyediain data.

#### **1. Siklus Request & Response**

Setiap kali kamu akses sebuah URL, ada proses timbal balik kayak gini:

- **Request (Permintaan):** Browser kamu (Client) ngirim permintaan ke web server buat dapetin informasi halaman tertentu.
- **Processing:** Server nerima permintaan itu, memprosesnya (misal: nyari file di disk), dan nyiapin datanya.
- **Response (Tanggapan):** Server ngirim balik data yang diminta (kayak HTML, gambar, atau CSS) ke browser kamu.
- **Rendering:** Browser nerima data itu dan nampilinnya jadi halaman web yang bisa kamu liat dan pake.

#### **2. Dua Komponen Utama Website**

Website terbagi jadi dua bagian yang punya peran beda tapi saling melengkapi:

- **Front-End (Client-Side):** Semua yang di-render dan ditampilin sama browser. Ini komponen visual yang langsung berinteraksi sama pengguna.
- **Back-End (Server-Side):** Server yang memproses permintaan dan ngelola data di balik layar sebelum dikirim balik sebagai respon.

### HTML (HyperText Markup Language)

Website pada dasarnya dibangun pake tiga teknologi utama:

- **HTML:** Dipake buat ngebangun struktur dan mendefinisiin kerangka website.
- **CSS:** Dipake buat mempercantik tampilan dengan nambahin gaya atau _styling_.
- **JavaScript:** Dipake buat mengimplementasiin fitur kompleks dan interaktivitas di halaman.

#### **Struktur Dasar HTML**

HTML pake elemen atau **tags** buat ngasih tau browser cara nampilin konten. Ini komponen utamanya:

| Tag               | Deskripsi                                                  |
| :---------------- | :--------------------------------------------------------- |
| `<!DOCTYPE html>` | Mendefinisiin bahwa halaman ini adalah dokumen HTML5.      |
| `<html>`          | Elemen akar (_root_) dari seluruh halaman HTML.            |
| `<head>`          | Berisi informasi metadata tentang halaman (kayak judul).   |
| `<body>`          | Berisi semua konten yang bakal ditampilin di browser.      |
| `<h1>`            | Dipake buat bikin judul besar (_heading_).                 |
| `<p>`             | Dipake buat bikin paragraf.                                |

#### **Mengenal Attributes**

Setiap tag bisa punya atribut buat ngasih informasi tambahan atau gaya:

- **Class (`class`):** Dipake buat ngasih gaya yang sama ke banyak elemen sekaligus.
- **Src (`src`):** Dipake di tag gambar (`<img>`) buat nentuin lokasi file gambar.
- **ID (`id`):** Bersifat **unik**. Satu elemen cuma boleh punya satu ID tertentu buat ngebedain dia dari elemen lain. Dipake buat _styling_ khusus dan identifikasi oleh JavaScript.

> **Note:** Hacker sering pake fitur "View Page Source" (Ctrl+U) buat nyari informasi sensitif yang nggak sengaja ditinggalin developer di komentar HTML.

### JavaScript

### JavaScript (Functionality & Interactivity)

JavaScript dipake buat ngontrol fungsionalitas halaman web. Tanpa JavaScript, halaman web bakal statis dan nggak interaktif.

#### **1. Cara Menambahkan JavaScript**

Kode JavaScript bisa dimasukin ke halaman web dengan dua cara:

- **Internal:** Langsung di dalam tag `<script>`.
- **Remote:** Pake atribut `src` buat nge-load file eksternal:
  `<script src="/location/of/javascript_file.js"></script>`

#### **2. Memanipulasi Elemen HTML**

JavaScript bisa nyari elemen berdasarkan **ID** dan ngubah isinya secara dinamis. Contohnya:

```javascript
document.getElementById("demo").innerHTML = "Element has been changed";
```

_Kode di atas nyari elemen dengan ID "demo" dan ngubah teksnya jadi "Element has been changed"._

#### **3. Events (Kejadian)**

Elemen HTML bisa trigger JavaScript lewat "Event", kayak:

- **onclick:** Terjadi waktu elemen di-klik.
- **onhover:** Terjadi waktu kursor ada di atas elemen.

**Contoh implementasi pada tombol:**

```html
<button onclick='document.getElementById("demo").innerHTML = "Button Clicked";'>
  Click Me!
</button>
```

_Waktu tombol di-klik, teks di elemen dengan ID "demo" bakal berubah jadi "Button Clicked"._

### Sensitive Data Exposure

Sensitive Data Exposure terjadi waktu informasi sensitif dalam bentuk teks biasa (_clear-text_) keekspos di kode sumber _frontend_ yang bisa diakses publik.

#### **Mengapa Hal Ini Terjadi?**

Developer sering lupa ngehapus data penting selama proses development, kayak:

- **Credential Login:** Username atau password yang ditulis langsung di kode HTML/JavaScript.
- **Tautan Tersembunyi:** Link ke halaman privat yang nggak seharusnya diketahui pengguna umum.
- **Komentar HTML:** Catatan internal developer yang berisi informasi sensitif.

#### **Langkah Penilaian Keamanan (Reconnaissance):**

Salah satu hal pertama yang harus dilakuin waktu menilai keamanan aplikasi web adalah ninjau kode sumber halaman (_Page Source Code_) buat nyari kebocoran data.

> **Note:** Penyerang bisa manfaatin informasi yang bocor ini buat eskalasi akses ke komponen _backend_ atau bagian aplikasi lainnya.

### HTML Injection

HTML Injection itu kerentanan yang terjadi waktu input pengguna ditampilin di halaman web tanpa lewat proses filtrasi atau sanitasi yang tepat. Ini bikin penyerang bisa "nyuntikin" kode HTML mereka sendiri ke situs web korban.

#### **Cara Kerja HTML Injection**

1. **Input:** Pengguna masukin tag HTML (misal: `<h1>Hacked</h1>`) ke kolom input (kayak kolom komentar atau nama).
2. **Failure to Sanitize:** Server nerima input itu dan nyimpennya atau langsung nampilinnya balik tanpa ngubah karakter khusus jadi teks aman.
3. **Execution:** Browser nerjemahin input itu sebagai kode HTML asli dan nge-render-nya, jadi tampilan halaman berubah sesuai keinginan penyerang.

#### **Dampak dari HTML Injection:**

- **Defacement:** Ngubah konten visual website (misal: ganti gambar atau teks).
- **Phishing:** Nyuntikin formulir login palsu buat nyuri data pengguna lain.
- **Redirection:** Ngarahin pengguna ke situs berbahaya pake tag `<a>`.

> **Note:** **"All User Input is Evil."** Jangan pernah nampilin input user secara langsung tanpa proses _encoding_ atau _filtering_ di sisi server.
