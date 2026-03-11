# TryHackMe: How The Web Works


---

* **Room Link:** [How The Web Works](https://tryhackme.com/room/howwebsiteswork)
* **Category:** How The Web Works
* **Difficulty:** easy

---

## Overview

Belajar proses fundamental dalam pembuatan situs web, termasuk bagaimana browser berinteraksi sama web server.

(Untuk detail lengkap tentang protokol HTTP, cek catatan [HTTP In Details](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Pre-Security/How-The-Web-Works/HTTP-In-details.md). Proses pencarian alamat server dijelaskan di [DNS In Details](file:///home/dimm/CyberSec-Learning-Notes/THM-Notes/Pre-Security/How-The-Web-Works/DNS-In-Details.md))

### How The Web Works?

Pada dasarnya, web bekerja pakai model **Client-Server**. Analogi paling gampang: bayangkan kamu lagi di **restoran** (lagi lagi restoran, karena emang mirip).

* **Client** = Kamu (yang memesan makanan lewat browser)
* **Server** = Dapur (yang menyiapkan dan mengirimkan makanan/data)

#### Siklus Request & Response

Setiap kali kamu akses sebuah URL, terjadi proses bolak balik seperti ini:

| Tahap | Analogi Restoran | Yang Terjadi |
| ----- | ---------------- | ------------ |
| **Request** | Kamu kasih orderan ke pelayan | Browser mengirim permintaan ke web server |
| **Processing** | Dapur masak pesananmu | Server memproses permintaan (cari file, query database, dll) |
| **Response** | Pelayan bawa makanan ke mejamu | Server mengirim balik data (HTML, gambar, CSS) ke browser |
| **Rendering** | Kamu makan dan menikmatinya | Browser menampilkan data jadi halaman web yang bisa dilihat |

#### Dua Komponen Utama Website

| Komponen | Analogi | Penjelasan |
| -------- | ------- | ---------- |
| **Front-End** (Client-Side) | Ruang makan restoran (yang dilihat pengunjung) | Semua yang di-render dan ditampilkan oleh browser — komponen visual yang langsung berinteraksi sama pengguna |
| **Back-End** (Server-Side) | Dapur restoran (tersembunyi di belakang) | Server yang memproses permintaan dan mengelola data di balik layar |

---

### HTML (HyperText Markup Language)

Website dibangun pakai tiga teknologi utama. Analogi paling gampang — bayangkan membangun **rumah**:

| Teknologi | Analogi Rumah | Fungsi |
| --------- | ------------- | ------ |
| **HTML** | Kerangka dan struktur bangunan (dinding, atap, pintu) | Membangun struktur dan mendefinisikan kerangka website |
| **CSS** | Cat, wallpaper, dan dekorasi interior | Mempercantik tampilan dengan styling |
| **JavaScript** | Listrik, AC, dan sistem otomatisasi | Mengimplementasikan fitur kompleks dan interaktivitas |

#### Struktur Dasar HTML

HTML pakai elemen atau **tags** buat memberi tau browser cara menampilkan konten:

| Tag | Fungsi |
| :-- | :----- |
| `<!DOCTYPE html>` | Mendefinisikan bahwa halaman ini adalah dokumen HTML5 |
| `<html>` | Elemen akar (_root_) dari seluruh halaman |
| `<head>` | Berisi metadata tentang halaman (judul, link CSS, dll) |
| `<body>` | Berisi semua konten yang ditampilkan di browser |
| `<h1>` | Membuat judul besar (_heading_) |
| `<p>` | Membuat paragraf |

#### Mengenal Attributes

Setiap tag bisa punya **atribut** — ibarat label nama atau stiker identitas yang ditempel di elemen:

| Atribut | Analogi | Fungsi |
| ------- | ------- | ------ |
| **class** | Seragam tim (banyak orang bisa pakai seragam yang sama) | Memberi gaya yang sama ke banyak elemen sekaligus |
| **src** | Alamat foto di album | Menentukan lokasi file (gambar, script, dll) |
| **id** | Nomor KTP (unik per orang) | Identitas unik per elemen — untuk styling khusus dan akses JavaScript |

> **Note:** Hacker sering pakai fitur "View Page Source" (Ctrl+U) buat mencari informasi sensitif yang tidak sengaja ditinggalkan developer di komentar HTML.

---

### JavaScript (Functionality & Interactivity)

JavaScript membuat halaman web menjadi hidup. Tanpa JavaScript, halaman web itu ibarat **poster statis** — cuma bisa dilihat, ga bisa diajak interaksi.

#### Cara Menambahkan JavaScript

| Metode | Cara |
| ------ | ---- |
| **Internal** | Langsung di dalam tag `<script>` di file HTML |
| **Remote/Eksternal** | Pakai atribut `src`: `<script src="/path/to/file.js"></script>` |

#### Memanipulasi Elemen HTML

JavaScript bisa mencari elemen berdasarkan **ID** dan mengubah isinya secara dinamis:

```javascript
document.getElementById("demo").innerHTML = "Element has been changed";
```

_Kode di atas mencari elemen dengan ID "demo" dan mengubah teksnya._

#### Events (Kejadian)

Elemen HTML bisa men-trigger JavaScript lewat yang namanya "Event":

| Event | Kapan Terjadi |
| ----- | ------------- |
| **onclick** | Saat elemen di-klik |
| **onhover** | Saat kursor ada di atas elemen |

Contoh implementasi pada tombol:

```html
<button onclick='document.getElementById("demo").innerHTML = "Button Clicked";'>
  Click Me!
</button>
```

---

### Sensitive Data Exposure

Sensitive Data Exposure terjadi saat informasi sensitif dalam bentuk teks biasa (_clear-text_) keekspos di kode sumber _frontend_ yang bisa diakses publik.

Bayangkan jika seorang developer **lupa mengunci buku catatan rahasia** mereka dan meninggalkannya terbuka di meja resepsionis. Siapa saja yang lewat bisa membacanya.

**Contoh data yang sering bocor:**

| Tipe Kebocoran | Contoh |
| -------------- | ------ |
| **Credential Login** | Username/password yang ditulis langsung di kode HTML/JavaScript |
| **Tautan Tersembunyi** | Link ke halaman admin atau panel privat |
| **Komentar HTML** | Catatan internal developer yang berisi informasi sensitif |

> **Note:** Langkah pertama seorang pentester saat menilai keamanan web app: periksa _Page Source Code_ (Ctrl+U) untuk mencari kebocoran data.

---

### HTML Injection

HTML Injection itu kerentanan yang terjadi saat input pengguna ditampilkan di halaman web **tanpa proses sanitasi yang tepat**. Analogi: Bayangkan papan pengumuman publik yang siapa saja bisa mencoret dan menambahkan tulisan — termasuk tulisan berbahaya.

**Cara Kerja HTML Injection:**

| Tahap | Yang Terjadi |
| ----- | ------------ |
| **Input** | Penyerang memasukkan tag HTML (misal: `<h1>Hacked</h1>`) ke kolom input |
| **Failure to Sanitize** | Server menerima input dan menampilkannya balik tanpa memfilter karakter khusus |
| **Execution** | Browser menerjemahkan input itu sebagai kode HTML asli dan me-render-nya |

**Dampak dari HTML Injection:**
* **Defacement:** Mengubah konten visual website
* **Phishing:** Menyuntikkan formulir login palsu buat mencuri data pengguna
* **Redirection:** Mengarahkan pengguna ke situs berbahaya pakai tag `<a>`

> **Note:** **"All User Input is Evil."** Jangan pernah menampilkan input user secara langsung tanpa proses _encoding_ atau _filtering_ di sisi server.
