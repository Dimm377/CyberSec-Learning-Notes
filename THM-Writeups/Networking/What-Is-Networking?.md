# TryHackMe: What Is Networking?

**Room Link:** [What Is Networking](https://tryhackme.com/room/whatisnetworking)
**Category:** Networking / Fundamental
**Difficulty:** easy

## Overview

- Membahas tentang fundamental paling dasar tentang apa itu networking
- **Simulasi wifi hotel:** Praktek langsung untuk memahami alur data pada jaringan wifi hotel melalui 2 skenario pengguna:
- **Pengguna 1 (Sudah bayar layanan wifi):** router akan mengizinkan dan mengirimkan data dengan baik
- **Pengguna 2 (Belum membayar Layanan wifi):** setiap kali pengguna 2 mengirim data maka permintaan nya akan ditolak oleh router
- Identitas perangkat (Mengenal peran penting IP address dan MAC adress untuk memastikan data agar sampai tujuan secara tepat)

## Task Summary & Key Concept

### 1. What is Networking?

Proses menghubungkan komputer atau perangkat lain sehingga dapat berkomunikasi dan berbagi sumber daya, melibatkan penggunaan networking devices seperi switch ,hub ,kabel , dan router untuk membangun jaringan, networking juga mengatur dan mengelola koneksi agar data dapat dikirim dan diterima dengan efisien, tujuan adalah memungkinkan komunikasi dan kolaborasi di lingkungan yang berbeda

### Where can the network be found?

Jaringan bisa ditemukan di berbagai tempat, termasuk rumah, kantor, sekolah, fasilitas umum, dan infrastruktur seperti sistem transportasi, jaringan listrik, dan internet.

### Identity networks

#### IP Address

IP Address (Internet protocol address) adalah serangkaian angka yang digunakan untuk menghidentifikasi perangkat di jaringan, agar perangkat di alamat ini bisa saling berkomunikasi dan bertukar data melalui internet, ada 2 jenis alamat IP

- **IPv4:** terdiri dari 4 kelompok angka yang dipisahkan oleh titik atau disebut octet **contoh: (`192.168.1.1`)**, menggunakan alamat 32-bit sehingga bisa mendukung 4,3 miliar angka alamat unik

- **IPv6:** versi terbaru dari IP address yang dibuat untuk mengatasi keterbatasan IPv4, Menggunakan alamat 128-bit, mendukung jumlah alamat unik yang jauh lebih besar dibanding IPv4, Alamat ini terdiri dari 8 kelompok 4 digit heksadesimal yang dipisahkan oleh simbol titik dua (`:`)
  **contoh:(`2001:0db8:85a3:0000:0000:8a2e:0370:7334`)`**
