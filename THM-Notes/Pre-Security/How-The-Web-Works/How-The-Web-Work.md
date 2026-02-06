# TryHackMe: How The Web Works

**Room Link:** [How The Web Works](https://tryhackme.com/room/howwebsiteswork)
**Category:** How The Web Works
**Difficulty:** easy

# Overview

mempelajari proses fundamental dalam pembuatan situs web, termasuk bagaimana browser berinteraksi dengan server web.

### How The Web Works ?

Pada dasarnya, web bekerja menggunakan model **Client-Server**. Ini adalah interaksi terus-menerus antara browser yang sedang digunakan dengan sebuah komputer di lokasi lain yang menyediakan data.

#### **1. Siklus Request & Response**

Setiap kali kamu mengakses sebuah URL, terjadi proses timbal balik sebagai berikut:

- **Request (Permintaan):** Browser kamu (Client) mengirimkan permintaan ke web server untuk mendapatkan informasi halaman tertentu.
- **Processing:** Server menerima permintaan tersebut, memprosesnya (misal: mencari file di disk), dan menyiapkan datanya.
- **Response (Tanggapan):** Server mengirimkan kembali data yang diminta (seperti HTML, gambar, atau CSS) ke browser kamu.
- **Rendering:** Browser menerima data tersebut dan menampilkannya menjadi halaman web yang bisa kamu lihat dan gunakan.

#### **2. Dua Komponen Utama Website**

Website terbagi menjadi dua bagian yang memiliki peran berbeda namun saling melengkapi:

- **Front-End (Client-Side):** Segala sesuatu yang dirender dan ditampilkan oleh browser. Ini adalah komponen visual yang langsung berinteraksi dengan pengguna.
- **Back-End (Server-Side):** Server yang memproses permintaan dan mengelola data di balik layar sebelum dikirimkan kembali sebagai respon.

### HTML (HyperText Markup Language)

Website pada dasarnya dibangun menggunakan tiga teknologi utama:

- **HTML:** Digunakan untuk membangun struktur dan mendefinisikan kerangka website.
- **CSS:** Digunakan untuk mempercantik tampilan dengan menambahkan gaya atau _styling_.
- **JavaScript:** Digunakan untuk mengimplementasikan fitur kompleks dan interaktivitas pada halaman.

#### **Struktur Dasar HTML**

HTML menggunakan elemen atau **tags** untuk memberi tahu browser cara menampilkan konten, Berikut adalah komponen utama dalam dokumen HTML:

| Tag               | Deskripsi                                                  |
| :---------------- | :--------------------------------------------------------- |
| `<!DOCTYPE html>` | Mendefinisikan bahwa halaman adalah dokumen HTML5.         |
| `<html>`          | Elemen akar (_root_) dari seluruh halaman HTML.            |
| `<head>`          | Berisi informasi metadata tentang halaman (seperti judul). |
| `<body>`          | Berisi semua konten yang akan ditampilkan di browser.      |
| `<h1>`            | Digunakan untuk mendefinisikan judul besar (_heading_).    |
| `<p>`             | Digunakan untuk membuat sebuah paragraf.                   |

#### **Mengenal Attributes**

Setiap tag dapat memiliki atribut untuk memberikan informasi tambahan atau gaya:

- **Class (`class`):** Digunakan untuk memberikan gaya yang sama pada banyak elemen sekaligus.
- **Src (`src`):** Digunakan pada tag gambar (`<img>`) untuk menentukan lokasi file gambar.
- **ID (`id`):** Bersifat **unik**. Satu elemen hanya boleh memiliki satu ID tertentu untuk membedakannya dari elemen lain. Digunakan untuk _styling_ khusus dan identifikasi oleh JavaScript.

> **Note:** Hacker sering menggunakan fitur "View Page Source" (Ctrl+U) untuk mencari informasi sensitif yang tidak sengaja ditinggalkan developer di dalam komentar HTML.

### JavaScript

### JavaScript (Functionality & Interactivity)

JavaScript digunakan untuk mengontrol fungsionalitas halaman web. Tanpa JavaScript, halaman web akan menjadi statis dan tidak interaktif.

#### **1. Cara Menambahkan JavaScript**

Kode JavaScript dapat dimasukkan ke dalam halaman web dengan dua cara:

- **Internal:** Langsung di dalam tag `<script>`.
- **Remote:** Menggunakan atribut `src` untuk memuat file eksternal:
  `<script src="/location/of/javascript_file.js"></script>`

#### **2. Memanipulasi Elemen HTML**

JavaScript dapat mencari elemen berdasarkan **ID** dan mengubah isinya secara dinamis. Contohnya:

```javascript
document.getElementById("demo").innerHTML = "Element has been changed";
```

_Kode di atas mencari elemen dengan ID "demo" dan mengubah teks di dalamnya menjadi "Element has been changed"_.

#### **3. Events (Kejadian)**

Elemen HTML dapat memicu JavaScript melalui sebuah "Event", seperti:

- **onclick:** Terjadi saat elemen di-klik.
- **onhover:** Terjadi saat kursor berada di atas elemen.

**Contoh implementasi pada tombol:**

```html
<button onclick='document.getElementById("demo").innerHTML = "Button Clicked";'>
  Click Me!
</button>
```

_Saat tombol di-klik, teks pada elemen dengan ID "demo" akan berubah menjadi "Button Clicked"_.

### Sensitive Data Exposure

Sensitive Data Exposure terjadi ketika informasi sensitif dalam bentuk teks biasa (_clear-text_) terpapar di kode sumber _frontend_ yang dapat diakses oleh publik.

#### **Mengapa Hal Ini Terjadi?**

Developer sering kali lupa menghapus data penting selama proses Developing, seperti:

- **Credential Login:** Username atau password yang tertulis langsung di kode HTML/JavaScript.
- **Tautan Tersembunyi:** Link menuju halaman privat yang tidak seharusnya diketahui pengguna umum.
- **Komentar HTML:** Catatan internal pengembang yang berisi informasi sensitif.

#### **Langkah Penilaian Keamanan (Reconnaissance):**

Salah satu hal pertama yang harus dilakukan saat menilai keamanan aplikasi web adalah meninjau kode sumber halaman (_Page Source Code_) untuk mencari kebocoran data.

> **Note:** Penyerang dapat memanfaatkan informasi yang bocor ini untuk melakukan eskalasi akses ke komponen _backend_ atau bagian aplikasi lainnya.

### HTML Injection

HTML Injection adalah kerentanan yang terjadi ketika input pengguna ditampilkan pada halaman web tanpa melalui proses filtrasi atau sanitasi yang tepat. Hal ini memungkinkan penyerang untuk "menyuntikkan" kode HTML mereka sendiri ke dalam situs web korban.

#### **Cara Kerja HTML Injection**

1. **Input:** Pengguna memasukkan tag HTML (misal: `<h1>Hacked</h1>`) ke dalam kolom input (seperti kolom komentar atau nama).
2. **Failure to Sanitize:** Server menerima input tersebut dan menyimpannya atau langsung menampilkannya kembali tanpa mengubah karakter khusus menjadi teks aman.
3. **Execution:** Browser menginterpretasikan input tersebut sebagai kode HTML asli dan merendernya, sehingga tampilan halaman berubah sesuai keinginan penyerang.

#### **Dampak dari HTML Injection:**

- **Defacement:** Mengubah konten visual website (misal: mengganti gambar atau teks).
- **Phishing:** Menyuntikkan formulir login palsu untuk mencuri data pengguna lain.
- **Redirection:** Mengarahkan pengguna ke situs berbahaya menggunakan tag `<a>`.

> **Note:** **"All User Input is Evil."** Jangan pernah menampilkan input user secara langsung tanpa proses _encoding_ atau _filtering_ di sisi server.
