# TryHackMe: Networking Secure Protocol

**Room Link:** [TryHackMe](https://tryhackme.com/room/networkingsecureprotocols)
**Category:** Cyber Security 101
**Difficulty:** Easy

## Overview

Setelah mempelajari protokol inti (core), room ini membahas versi secure nya dari protokol-protokol tersebut, Fokus utama kita adalah memahami bagaimana enkripsi (SSL/TLS) melindungi data sensitif agar tidak bisa dibaca oleh pihak ketiga saat transit di jaringan

## Task 2: SSH (Secure Shell)

SSH adalah pengganti Telnet yang jauh lebih aman untuk akses via remote terminal

- **Enkripsi:** Berbeda dengan Telnet yang mengirimkan password dalam bentuk _plain-text_, SSH mengenkripsi seluruh sesi communication
- **Port:** Secara default berjalan pada **Port 22**.
- **Autentikasi:** Mendukung penggunaan _Public Key Authentication_ yang jauh lebih aman daripada sekedar password biasa

## Task 3: HTTPS (Hypertext Transfer Protocol Secure)

Versi aman dari HTTP yang digunakan hampir oleh seluruh website modern saat ini

- **SSL/TLS:** HTTPS menggunakan sertifikat SSL/TLS untuk mengenkripsi traffic antara browser dan server
- **Port:** Secara default berjalan pada **Port 443**
- **Integritas:** Menjamin bahwa data yang dikirim tidak dimanipulasi di tengah jalan (Integrity)
