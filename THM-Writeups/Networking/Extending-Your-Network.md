# TryHackMe: Extending Your Network

**Room Link:** [ExtendingYourNetwork](https://tryhackme.com/room/extendingyournetwork)
**Category:** Networking / Fundamental
**Difficulty:** easy

# Overview

Memahami bagaimana jaringan lokal dapat terhubung ke jaringan luar melalui port forwarding, Firewall, dan VPN

### What is Port Forwarding ?

Port forwarding adalah teknik jaringan untuk mengarahkan permintaan komunikasi dari alamat IP publik dan nomor port tertentu ke alamat IP privat dan port di dalam jaringan lokal (LAN) perangkat kita

### The Firewall 101

Firewall adalah perangkat jaringan yang bertanggung jawab untuk menentukan lalu lintas apa diperbolehkan masuk atau keluar, anggap saja firewall itu seperti satpam keamanan pada suatu jaringan

firewall dapat memblokir atau mengizinkan lalu lintas jaringan tergantung beberapa faktor yaitu:

- **Dari mana lalu lintas itu berasal? (apakah firewall telah diberitahu untuk menerima/menolak lalu lintas dari jaringan tertentu?)**

- **Kemana arah lalu lintasnya? (apakah firewall telah diberitahu untuk menerima/menolak lalu lintas yang ditujukan untuk jaringan tertentu?)**

- **Untuk port apa lalu lintasnya? (apakah firewall telah diberitahu untuk menerima/menolak lalu lintas yang ditujukan untuk port 80 saja?)**

- **Protokol apa yang digunakan lalu lintas? (apakah firewall telah diberitahu untuk menerima/menolak lalu lintas UDP, TCP atau keduanya?)**

Firewall melakukan pemeriksaan paket untuk menentukan jawaban atas pertanyaan-pertanyaan ini, Firewall dapat dikategorikan
