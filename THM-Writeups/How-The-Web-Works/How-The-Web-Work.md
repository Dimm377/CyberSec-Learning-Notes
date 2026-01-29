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

> **Mindset Tip:** Sebagai orang yang belajar Cyber Security, ingatlah bahwa kode **Front-end** berjalan di sisi klien sehingga bisa dilihat dan dimanipulasi oleh siapa pun, sedangkan **Back-end** adalah area tertutup yang menyimpan logika utama aplikasi.
