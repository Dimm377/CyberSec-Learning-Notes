# TryHackMe: How The Web Works


---

- **Room Link:** [How The Web Works](https://tryhackme.com/room/howwebsiteswork)
- **Category:** How The Web Works
- **Difficulty:** easy

---

# Overview

Belajar proses fundamental dalam pembuatan situs web, termasuk gimana browser berinteraksi sama web server.

### How The Web Works ?

Pada dasarnya, web bekerja pakai model **Client-Server**. Ini interaksi terus-menerus antara browser yang lagi kamu pakai sama komputer di lokasi lain yang menyediakan data.

#### **1. Siklus Request & Response**

Setiap kali kamu akses sebuah URL, ada proses timbal balik seperti gini:

- **Request (Permintaan):** Browser kamu (Client) mengirim permintaan ke web server buat mendapatkan informasi halaman tertentu.
- **Processing:** Server nerima permintaan itu, memprosesnya (misal: mencari file di disk), dan nyiapin datanya.
- **Response (Tanggapan):** Server mengirim balik data yang diminta (seperti HTML, gambar, atau CSS) ke browser kamu.
- **Rendering:** Browser nerima data itu dan nampilinnya jadi halaman web yang bisa kamu melihat dan pakai.

#### **2. Dua Komponen Utama Website**

Website terbagi jadi dua bagian yang punya peran beda tapi saling melengkapi:

- **Front-End (Client-Side):** Semua yang di-render dan ditampilin sama browser. Ini komponen visual yang langsung berinteraksi sama pengguna.
- **Back-End (Server-Side):** Server yang memproses permintaan dan ngelola data di balik layar sebelum dikirim balik sebagai respon.

### HTML (HyperText Markup Language)

Website pada dasarnya dibangun pakai tiga teknologi utama:

- **HTML:** Dipake buat ngebangun struktur dan mendefinisiin kerangka website.
- **CSS:** Dipake buat mempercantik tampilan dengan nambahin gaya atau _styling_.
- **JavaScript:** Dipake buat mengimplementasiin fitur kompleks dan interaktivitas di halaman.

#### **Struktur Dasar HTML**

HTML pakai elemen atau **tags** buat memberi tau browser cara menampilkan konten. Ini komponen utamanya:

| Tag               | Deskripsi                                                  |
| :---------------- | :--------------------------------------------------------- |
| `<!DOCTYPE html>` | Mendefinisiin bahwa halaman ini adalah dokumen HTML5.      |
| `<html>`          | Elemen akar (_root_) dari seluruh halaman HTML.            |
| `<head>`          | Berisi informasi metadata tentang halaman (seperti judul).   |
| `<body>`          | Berisi semua konten yang bakal ditampilin di browser.      |
| `<h1>`            | Dipake buat membuat judul besar (_heading_).                 |
| `<p>`             | Dipake buat membuat paragraf.                                |

#### **Mengenal Attributes**

Setiap tag bisa punya atribut buat memberi informasi tambahan atau gaya:

- **Class (`class`):** Dipake buat memberi gaya yang sama ke banyak elemen sekaligus.
- **Src (`src`):** Dipake di tag gambar (`<img>`) buat menentukan lokasi file gambar.
- **ID (`id`):** Bersifat **unik**. Satu elemen cuma boleh punya satu ID tertentu buat ngebedain dia dari elemen lain. Dipake buat _styling_ khusus dan identifikasi oleh JavaScript.

> **Note:** Hacker sering pakai fitur "View Page Source" (Ctrl+U) buat mencari informasi sensitif yang tidak sengaja ditinggalin developer di komentar HTML.

### JavaScript

### JavaScript (Functionality & Interactivity)

JavaScript dipake buat ngontrol fungsionalitas halaman web. Tanpa JavaScript, halaman web bakal statis dan tidak interaktif.

#### **1. Cara Menambahkan JavaScript**

Kode JavaScript bisa dimasukin ke halaman web dengan dua cara:

- **Internal:** Langsung di dalam tag `<script>`.
- **Remote:** Pakai atribut `src` buat nge-load file eksternal:
  `<script src="/location/of/javascript_file.js"></script>`

#### **2. Memanipulasi Elemen HTML**

JavaScript bisa mencari elemen berdasarkan **ID** dan mengubah isinya secara dinamis. Contohnya:

```javascript
document.getElementById("demo").innerHTML = "Element has been changed";
```

_Kode di atas mencari elemen dengan ID "demo" dan mengubah teksnya jadi "Element has been changed"._

#### **3. Events (Kejadian)**

Elemen HTML bisa trigger JavaScript lewat "Event", seperti:

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

Developer sering lupa ngehapus data penting selama proses development, seperti:

- **Credential Login:** Username atau password yang ditulis langsung di kode HTML/JavaScript.
- **Tautan Tersembunyi:** Link ke halaman privat yang tidak seharusnya diketahui pengguna umum.
- **Komentar HTML:** Catatan internal developer yang berisi informasi sensitif.

#### **Langkah Penilaian Keamanan (Reconnaissance):**

Salah satu hal pertama yang harus dilakuin waktu menilai keamanan aplikasi web adalah ninjau kode sumber halaman (_Page Source Code_) buat mencari kebocoran data.

> **Note:** Penyerang bisa manfaatin informasi yang bocor ini buat eskalasi akses ke komponen _backend_ atau bagian aplikasi lainnya.

### HTML Injection

HTML Injection itu kerentanan yang terjadi waktu input pengguna ditampilin di halaman web tanpa lewat proses filtrasi atau sanitasi yang tepat. Ini membuat penyerang bisa "nyuntikin" kode HTML mereka sendiri ke situs web korban.

#### **Cara Kerja HTML Injection**

1. **Input:** Pengguna memasukkan tag HTML (misal: `<h1>Hacked</h1>`) ke kolom input (seperti kolom komentar atau nama).
2. **Failure to Sanitize:** Server nerima input itu dan nyimpennya atau langsung nampilinnya balik tanpa mengubah karakter khusus jadi teks aman.
3. **Execution:** Browser nerjemahin input itu sebagai kode HTML asli dan nge-render-nya, jadi tampilan halaman berubah sesuai keinginan penyerang.

#### **Dampak dari HTML Injection:**

- **Defacement:** Mengubah konten visual website (misal: ganti gambar atau teks).
- **Phishing:** Nyuntikin formulir login palsu buat nyuri data pengguna lain.
- **Redirection:** Ngarahin pengguna ke situs berbahaya pakai tag `<a>`.

> **Note:** **"All User Input is Evil."** Jangan pernah menampilkan input user secara langsung tanpa proses _encoding_ atau _filtering_ di sisi server.
