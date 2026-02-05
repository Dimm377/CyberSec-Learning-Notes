# TryHackMe: Networking Concepts

**Room Link:** [Network concepts](https://tryhackme.com/room/networkingconcepts)
**Category:** Cyber Security 101 path
**Difficulty:** Easy

# Overview

### OSI Model

OSI Model adalah kerangka kerja konseptual yang memebagi proses komunikasi jaringan menjadi 7 layer

- **mnemonic:** "**P**lease **D**o **N**ot **T**hrow **S**pinach **P**izza **A**way"
- **Layer 4 (Transport):** Bertanggung jawab atas komunikasi _end-to-end_ antar aplikasi
- **Layer 3 (Network):** Bertanggung jawab untuk merutekan paket ke jaringan yang tepat menggunakan IP Address
- **Layer 2 (Data Link):** Mengatur perpindahan data antar perangkat di segmen jaringan yang sama (MAC Address).
- **Layer 6 (Presentation):** Menangani encoding, kompresi, dan enkripsi data aplikasi.

### TCP/IP Model

Berbeda dengan OSI Model yang konseptual, TCP/IP adalah model yang benar-benar diimplementasikan di internet saat ini

- **Application Layer:** Gabungan dari layer 5, 6, dan 7 OSI. Protokol seperti HTTP, FTP, dan DNS berada di sini.
- **Internet Layer:** Setara dengan Layer 3 OSI, fokus pada pengalamatan logis (IP).
