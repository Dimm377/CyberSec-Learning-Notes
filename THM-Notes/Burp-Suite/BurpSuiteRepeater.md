# TryHackMe: Burp Suite Repeater


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/burpsuiterepeater)
- **Category:** Web Hacking / Tools
- **Difficulty:** Easy

---

## Overview

Burp Repeater itu bagian dari Burp Suite yang membuat kita bisa nangkep, modifikasi, dan mengirim ulang permintaan HTTP secara manual berkali-kali. Tool ini penting banget buat pengujian kerentanan web karena kita bisa melihat respons server secara real-time setelah lakuin perubahan kecil di payload.

## What is Repeater?

Repeater membuat kita bisa lakuin manipulasi request secara manual.

- **Manual Testing:** Beda sama Intruder yang otomatis, Repeater dipake buat pengujian manual yang lebih teliti.
- **Efficiency:** Kita bisa menyimpan riwayat request dan modifikasi parameter tertentu tanpa harus ngulang proses intercept di browser.

## Basic Usage

Proses dasar penggunaan Repeater dalam workflow penetration testing:

1. **Capture:** Tangkep request pakai **Proxy** (Intercept On).
2. **Send to Repeater:** Tekan `Ctrl + R` (atau klik kanan -> Send to Repeater).
3. **Modify & Send:** Pindah ke tab Repeater, ubah isi request, terus klik **Send**.
4. **Analysis:** Amati respon server di panel sebelah kanan buat mencari kejanggalan.

## Views & Layouts

Repeater menyediakan berbagai cara buat melihat data request dan response.

- **Raw:** Tampilan teks murni (pure HTTP request/response).
- **Hex:** Berguna buat melihat karakter non-printable atau byte mentah.
- **Render:** Menampilkan respon HTML seolah-olah di dalam browser (visual).
- **Headers:** Daftar header HTTP yang disusun secara sistematis agar gampang dibaca.

## Inspector

Panel **Inspector** di sisi kanan membuat kita gampang modifikasi elemen request tanpa harus edit teks mentahnya.

- **Query Parameters:** Membuat kita bisa nambah, hapus, atau edit parameter URL (GET) secara instan.
- **Body Parameters:** Modifikasi data yang dikirim lewat metode POST.
- **Attributes:** Edit detail protokol seperti versi HTTP.

## Practical Example

Dalam praktik ini, kita belajar manipulasi header `User-Agent` atau parameter tertentu buat melihat gimana server memberi respon yang beda.

- **Tip:** Perhatiin perbedaan antara respon `200 OK` dan `404 Not Found` waktu lagi ubah path URL.
- **Flag Capture:** Biasanya melibatkan pengubahan header tertentu yang diminta soal buat trigger flag muncul di respon.

> **Tip:** "Don't just send, observe." Setiap byte dalam respon server bisa jadi kunci buat eksploitasi selanjutnya.
