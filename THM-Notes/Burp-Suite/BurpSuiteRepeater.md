# TryHackMe: Burp Suite Repeater


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/burpsuiterepeater)
- **Category:** Web Hacking / Tools
- **Difficulty:** Easy

---

## Overview

Burp Repeater adalah bagian dari Burp Suite yang memungkinkan kita untuk menangkap, memodifikasi, dan mengirim ulang permintaan HTTP secara manual berkali-kali. Tool ini sangat krusial untuk pengujian kerentanan web karena kita bisa melihat respons server secara real-time setelah melakukan perubahan kecil pada payload

## Task 2: What is Repeater?

Repeater memungkinkan kita melakukan manipulasi request secara manual.

- **Manual Testing:** Berbeda dengan Intruder yang otomatis, Repeater digunakan untuk pengujian manual yang lebih teliti.
- **Efficiency:** Kita bisa menyimpan riwayat request dan memodifikasi parameter tertentu tanpa harus mengulang proses intercept di browser.

## Task 3: Basic Usage

Proses dasar penggunaan Repeater dalam workflow penetration testing:

1. **Capture:** Tangkap request menggunakan **Proxy** (Intercept On).
2. **Send to Repeater:** Tekan `Ctrl + R` (atau klik kanan -> Send to Repeater).
3. **Modify & Send:** Pindah ke tab Repeater, ubah isi request, lalu klik **Send**.
4. **Analysis:** Amati respon server di panel sebelah kanan untuk mencari kejanggalan.

## Task 4: Views & Layouts

Repeater menyediakan berbagai cara untuk melihat data request dan response.

- **Raw:** Tampilan teks murni (pure HTTP request/response).
- **Hex:** Berguna untuk melihat karakter non-printable atau byte mentah.
- **Render:** Menampilkan respon HTML seolah-olah di dalam browser (visual).
- **Headers:** Daftar header HTTP yang disusun secara sistematis agar mudah dibaca.

## Task 5: Inspector

Panel **Inspector** di sisi kanan memudahkan kita untuk memodifikasi elemen request tanpa harus mengedit teks mentahnya.

- **Query Parameters:** Memungkinkan penambahan, penghapusan, atau pengeditan parameter URL (GET) secara instan.
- **Body Parameters:** Memodifikasi data yang dikirim melalui metode POST.
- **Attributes:** Mengedit detail protokol seperti versi HTTP.

## Task 6: Practical Example

Dalam praktik ini, kita belajar memanipulasi header `User-Agent` atau parameter tertentu untuk melihat bagaimana server memberikan respon yang berbeda.

- **Tip:** Perhatikan perbedaan antara respon `200 OK` dan `404 Not Found` saat sedang mengubah path URL.
- **Flag Capture:** Biasanya melibatkan pengubahan header tertentu yang diminta oleh soal untuk memicu flag muncul di respon

> **Tip:** "Don't just send, observe." Setiap byte dalam respon server bisa menjadi kunci untuk eksploitasi selanjutnya.
