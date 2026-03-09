# TryHackMe: OWASP Top 10 (2025)

- **Room Link:** [OWASP Top 10 (2025): IAAA Failures](https://tryhackme.com/room/owasptop102025iaaafailures)
- **Category:** OWASP Top 10 (2025)
- **Difficulty:** Easy

---

## Introduction

Room ini akan membedah 3 kategori dari **OWASP Top 10 (2025)** yang berkaitan dengan kegagalan dalam penerapan **Identity, Authentication, Authorisation, dan Accountability (IAAA)** pada aplikasi.

Kamu akan mempelajari teori dan langsung mempraktikkannya melalui tantangan (*challenges*) yang tersedia. Kategori yang akan dibahas meliputi:

1. **A01: Broken Access Control**
2. **A07: Authentication Failures**
3. **A09: Logging & Alerting Failures**

Room ini dirancang untuk pemula dan tidak memerlukan pengetahuan keamanan sebelumnya.

---

## What is IAAA ?

**IAAA** adalah cara sederhana untuk memahami bagaimana pengguna dan tindakan mereka diverifikasi dalam aplikasi. Setiap kategori berperan sangat penting dan kamu **tidak bisa melompati levelnya**. Jika level sebelumnya tidak dilakukan, maka level berikutnya tidak bisa dijalankan.

Keempat item tersebut adalah:

* **Identity (Identitas):** Akun unik (misal: user ID/email) yang mewakili seseorang atau layanan. -> *Siapa kamu?*
* **Authentication (Autentikasi):** Membuktikan identitas tersebut (misal: password, OTP, passkeys). -> *Buktikan kalau itu benar kamu*
* **Authorisation (Autorisasi):** Apa yang boleh dilakukan oleh identitas tersebut. -> *Kamu boleh ngapain aja di sini?*
* **Accountability (Akuntabilitas):** Mencatat dan memberi peringatan tentang siapa melakukan apa, kapan, dan dari mana. -> *Siapa yang mencatat jejakmu?*

Tiga kategori dari **OWASP Top 10:2025** yang dibahas di room ini berkaitan dengan kegagalan dalam penerapan IAAA. Kelemahan di sini bisa sangat fatal karena memungkinkan penyerang untuk mengakses data pengguna lain atau mendapatkan hak akses lebih (*privilege*) dari yang seharusnya mereka miliki.

---
