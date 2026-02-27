# TryHackMe: Windows Active Directory Basics:


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/winadbasics)
- **Category:** Windows / Active Directory
- **Difficulty:** easy

---

---

## Overview

Active Directory (AD) itu tulang punggung jaringan korporat yang membuat manajemen terpusat buat seluruh sumber daya di jaringan jadi mungkin. Mengerti struktur AD itu penting banget buat praktisi keamanan agar paham gimana identitas dan akses dikelola dalam skala besar.

---

## AD Components & Hierarchy

Struktur AD dibangun dari beberapa komponen logis dan fisik yang saling terhubung.

- **Domain Controller (DC):** Server pusat yang jalanin Active Directory Domain Services (ADDS) dan ngelola autentikasi.
- **Organizational Units (OUs):** Kontainer logis yang dipake buat ngelompokin user, grup, dan komputer agar gampang nerapin kebijakan.
- **Trees & Forests:** Kumpulan satu atau lebih domain yang berbagi skema dan katalog global yang sama disebut Forest.

---

## User & Computer Management

Ngelola objek di AD melibatkan alat administratif seperti _Active Directory Users and Computers_.

- **Object Protection:** Buat hapus OU yang terproteksi, fitur "Advanced Features" harus diaktifin agar opsi "Protect object from accidental deletion" bisa dinonaktifin.
- **Machine Accounts:** Setiap komputer yang gabung ke domain punya akun khusus yang ditandai sama simbol `$` di akhir namanya (misal: `PC01$`).
- **Delegation:** Fitur yang membuat admin bisa memberi hak spesifik ke user biasa (seperti reset password) tanpa harus memberi hak administratif penuh.

---

## Groups & Group Policy Objects (GPO)

GPO dipake buat nerapin konfigurasi keamanan secara otomatis ke seluruh perangkat dalam domain.

- **Management Tool:** Kebijakan dikelola lewat _Group Policy Management Console_ (`gpmc.msc`).
- **Distribution:** File kebijakan disimpen dan didistribusiin lewat folder share khusus yang namanya `SYSVOL`.
- **Hierarchy:** GPO bisa diterapin di tingkat Site, Domain, atau OU.

---

## Authentication Protocols

AD pakai dua protokol utama buat proses login dan akses sumber daya.

- **Kerberos:** Protokol default yang pakai sistem tiket (Ticket Granting Ticket/TGT) buat meminimalisir pengiriman credensial di jaringan.
- **NTLM:** Protokol challenge-response yang lebih tua tapi masih sering dipake buat kompatibilitas ke belakang.
- **Security Fact:** Baik Kerberos maupun NTLM dirancang agar password dalam bentuk teks asli (plaintext) tidak pernah dikirim lewat jaringan.

---

## Domain Trusts

Trust relationship membuat pengguna dari satu domain bisa mengakses sumber daya di domain lain.

- **Transitive Trust:** Kalau Domain A percaya Domain B, dan Domain B percaya Domain C, maka Domain A secara otomatis percaya Domain C.
- **Directional:** Trust bisa bersifat searah (One-way) atau dua arah (Two-way).

---
