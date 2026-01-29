# TryHackMe: How The Web Works

**Room Link:** [How The Web Works](https://tryhackme.com/room/howwebsiteswork)
**Category:** How The Web Works
**Difficulty:** easy

# Overview

mempelajari proses fundamental dalam pembuatan situs web, termasuk bagaimana browser berinteraksi dengan server web.

### How The Web Works ?

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
