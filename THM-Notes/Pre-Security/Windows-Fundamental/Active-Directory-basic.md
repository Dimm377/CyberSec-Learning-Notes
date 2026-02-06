# TryHackMe: Windows Active Directory Basics:

**Room Link:** [TryHackMe](https://tryhackme.com/room/winadbasics)
**Category:** Windows / Active Directory
**Difficulty:** easy

---

## Overview

Active Directory (AD) adalah tulang punggung jaringan korporat yang memungkinkan manajemen terpusat untuk seluruh sumber daya dalam jaringan. Memahami struktur AD sangat krusial bagi seorang praktisi keamanan untuk memahami bagaimana identitas dan akses dikelola dalam skala besar.

---

## AD Components & Hierarchy

Struktur AD dibangun dari beberapa komponen logis dan fisik yang saling terhubung.

- **Domain Controller (DC):** Server pusat yang menjalankan Active Directory Domain Services (ADDS) dan mengelola autentikasi.
- **Organizational Units (OUs):** Kontainer logis yang digunakan untuk mengelompokkan user, grup, dan komputer guna mempermudah penerapan kebijakan.
- **Trees & Forests:** Kumpulan satu atau lebih domain yang berbagi skema dan katalog global yang sama disebut Forest.

---

## User & Computer Management

Mengelola objek di AD melibatkan alat administratif seperti _Active Directory Users and Computers_.

- **Object Protection:** Untuk menghapus OU yang terproteksi, fitur "Advanced Features" harus diaktifkan agar opsi "Protect object from accidental deletion" dapat dinonaktifkan.
- **Machine Accounts:** Setiap komputer yang bergabung ke domain memiliki akun khusus yang ditandai dengan simbol `$` di akhir namanya (misal: `PC01$`).
- **Delegation:** Fitur yang memungkinkan admin memberikan hak spesifik kepada user biasa (seperti reset password) tanpa memberikan hak administratif penuh.

---

## Groups & Group Policy Objects (GPO)

GPO digunakan untuk menerapkan konfigurasi keamanan secara otomatis ke seluruh perangkat dalam domain.

- **Management Tool:** Kebijakan dikelola melalui _Group Policy Management Console_ (`gpmc.msc`).
- **Distribution:** File kebijakan disimpan dan didistribusikan melalui folder share khusus yang bernama `SYSVOL`.
- **Hierarchy:** GPO dapat diterapkan di tingkat Site, Domain, atau OU.

---

## Authentication Protocols

AD menggunakan dua protokol utama untuk proses login dan akses sumber daya.

- **Kerberos:** Protokol default yang menggunakan sistem tiket (Ticket Granting Ticket/TGT) untuk meminimalisir pengiriman kredensial di jaringan.
- **NTLM:** Protokol challenge-response yang lebih tua namun masih sering digunakan untuk kompatibilitas ke belakang.
- **Security Fact:** Baik Kerberos maupun NTLM dirancang agar password dalam bentuk teks asli (plaintext) tidak pernah dikirimkan melalui jaringan.

---

## Domain Trusts

Trust relationship memungkinkan pengguna dari satu domain untuk mengakses sumber daya di domain lain.

- **Transitive Trust:** Jika Domain A percaya Domain B, dan Domain B percaya Domain C, maka Domain A secara otomatis percaya Domain C.
- **Directional:** Trust dapat bersifat searah (One-way) atau dua arah (Two-way).

---
