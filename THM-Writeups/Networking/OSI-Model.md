# TryHackMe: OSI Model

**(My favourite Part)**

**Room Link:** [OSI Model](https://tryhackme.com/room/osimodelzi)
**Category:** Networking / Fundamental
**Difficulty:** easy

## Overview

-

### What is OSI Model ?

OSI Model (Open System Interconnection Model) adalah kerangka konsep yang membagi jaringan menjadi 7 lapisan: Physical, Data Link, Network, Transport, Session, Presentation, Application

#### Ini gambaran 7 layer OSI Model:

<p align="center">
<img src="../../Assets/Images/OSI.png" width="400">
</p>

### Penjelasan setiap Layer OSI model:

1. **Physical:** Lapisan Pertama yang bertanggung jawab untuk mentransmisikan data secara fisik melalui media transmisi seperti kabel ethernet, Sinyal listrik, Kabel optic, Modem, dan Antena

2. **Data Link:** Lapisan kedua yang berfokus pada pengalamatan fisik transmisi data, lapisan ini menerima data dari lapisan jaringan dan menambahkan MAC Address agar data perangkat saling mengenali, simple nya lapisan ini memebantu perangkat yang terhubung ke jaringan agar bisa saling berkomunikasi

3. **Network:** Lapisan ketiga yang bertugas untuk mengarahkan data dari satu perangkat ke perangkat lainnya di jaringan yang berbeda, menggunakan IP Address untuk menentukan rute terbaik untuk mengirimkan data di jaringan yang kompleks

ada 5 protokol dari yang ku pelajari yaitu:

1. **IP (Internet Protocol):** Protokol untuk pengalamatan dan pengiriman data di dalam jaringan

2. **ICMP (Internet Control Message Protocol):** Protokol untuk mengirim pesan kontrol dan error antara perangkat jaringan

3. **ARP (Address Resolution Protocol):** Protokol yang menghubungkan / menerjemahkan IP Address ke MAC Address

4. **OSPF (Open Shortest Path First):** Protokol _link-state_ yang menentukan jalur tercepat berdasarkan _bandwidth_ (cost). OSPF sangat efisien karena memiliki peta lengkap topologi jaringan.

5. **RIP (Routing Information Protocol):** Protokol _distance-vector_ yang menggunakan jumlah lompatan router (**hop count**) untuk menentukan jalur terbaik. Protokol ini memiliki batas maksimal 15 lompatan (hop).

6. **Transport:** Lapisan keempat yang bertugas untuk mengelola pengiriman data antar perangkat dan memastikan data diterima dengan benar, unit data di lapisan ini disebut segmen bukan lagi packet

terdapat 2 protokol di lapisan Transport yaitu:

1. **TCP (Transmission Control Protocol):**

2. **UDP (User Datagram Protocol):**
