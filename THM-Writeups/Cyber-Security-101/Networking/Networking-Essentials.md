# TryHackMe: Networking Essentials

**Room Link:** [TryHackMe](https://tryhackme.com/room/networkingessentials)
**Category:** Cyber Security 101 Path
**Difficulty:** Easy

## Overview

Setelah memahami konsep dasar, room ini membawa kita lebih dalam ke perangkat keras dan protokol inti yang membuat internet bekerja, memahami cara kerja Switch, Router, dan protokol seperti DNS adalah hal wajib agar bisa melakukan enumerasi jaringan dengan efektif.

### Task 2: Network Hardware

Memahami perbedaan antara perangkat yang menghubungkan jaringan

- **Hubs:** Perangkat "bodoh" yang mengirimkan paket ke semua port yang tersedia (Broadcast). Ini adalah mimpi buruk untuk keamanan jaringan karena memudahkan sniffing
- **Switches:** Perangkat yang lebih pintar karena menyimpan tabel MAC Address dan hanya mengirimkan data ke port tujuan yang benar
- **Routers:** Menghubungkan berbagai jaringan yang berbeda dan bertanggung jawab untuk menentukan jalur terbaik bagi paket data (Routing)

### Task 3: MAC Addresses

Identitas fisik permanen yang tertanam pada kartu jaringan (NIC)

- **Format:** Terdiri dari 48-bit (6 oktet) yang direpresentasikan dalam heksadesimal.
- **OUI (Organizationally Unique Identifier):** 3 oktet pertama menunjukkan siapa produsen perangkat tersebut.
- > Red Team Tip: MAC Address bisa dimanipulasi (MAC Spoofing) untuk melewati filter keamanan di jaringan lokal.

### Task 4: Address Resolution Protocol (ARP)

Protokol krusial yang menjembatani komunikasi antara Layer 2 dan Layer 3.

- **Fungsi:** Menerjemahkan IP Address menjadi MAC Address agar data bisa sampai ke perangkat fisik yang tepat.
- **ARP Request:** Proses pencarian MAC Address yang dilakukan secara Broadcast ke seluruh jaringan lokal.
- **ARP Reply:** Jawaban Unicast dari pemilik IP yang berisi informasi MAC Address-nya.

> Security Insight: ARP tidak memiliki fitur verifikasi, sehingga rentan terhadap serangan ARP Spoofing/Poisoning yang memungkinkan teknik Man-in-the-Middle (MITM).
