# TryHackMe: Intro To LAN


---

- **Room Link:** [Intro To LAN](https://tryhackme.com/room/introtolan)
- **Category:** Networking / Fundamental
- **Difficulty:** easy

---

## Overview

- Membahas tentang dasar-dasar jaringan seperti Local area Network (LAN), apa itu Topologi dan jenis" nya (Star, Bus, Ring) perangkat jaringan (Switch, Hub, Router)

- Panduan dasar tentang subnetting (mmebagi jaringan menjadi lebih kecil) dan juga mengetahui tentang protokol jaringan ARP & DHCP

## Task Summary & Key Concept

### 1. What is a LAN ? (**L**ocal **A**rea **N**etwork)

Jenis jaringan lokal yang menghubungkan perangkat dengan jarak yang dekat seperti di rumah, kantor atau sekolah. LAN memungkinkan perangkat komputer untuk saling berkomunikasi dan berbagi sumber daya seperti upload file dan mengirim file dengan kecepatan tinggi, memiliki biaya yang rendah dibandingkan dengan jenis jaringan lainnya

### Jenis-Jenis topologi LAN

- Star Topology (perangkat terhubung secara individu melalui perangkat jaringan pusat seperti switch atau hub) paling umum digunakan tapi biaya mahal

<p align="center">
![Star Topology](../../Assets/Images/star-topology.png)
</p>

- Bus Topology (perangkat terhubung melalui kabel yang sama, karena bergantung ke satu koneksi (kabel backbone) ) biaya murah tapi akan down jika perangkat mengirimkan banyak data secara bersamaan

<p align="center">
![Bus Topology](../../Assets/Images/Bus-topology.png)
</p>

- Ring Topology (setiap perangkat terhubung ke dua perangkat lainnya, membentuk jalur melingkar, mengirim data dari satu perangkat ke perangkat lainnya dalam satu arah di sepanjang jalur sampai mencapai tujuan) mudah untuk mendeteksi kesalahan dalam lalu lintas data, jika satu perangkat mengalami masalah maka seluruh jaringa juga akan terpengaruh

<p align="center">
![Ring Topology](../../Assets/Images/Ring-topology.png)
</p>

### 2. Networking Device

Digunakan untuk menghubungkan dan mengelola jaringan komputer

### Contoh-contoh networking devices

- **Switch:** (Alat yang menghubungkan beberapa perangkat lain seperti komputer, printer,atau perangkat lain yang bisa terhubung ke dalam LAN, switch menggunakan MAC Address untuk hanya mengirimkan paket data ke perangkat tujuan secara spesifik)

- **Router:** (Tugas router adalah menghubungkan jaringan dan meneruskan data diantara jaringan menggunakan proses yang disebut routing, fungsi routing adalah menentukan jalur terbaik bagi paket data untuk melewati jaringan agar sampai tujuan dengan sukses)

### ARP (**A**ddress **R**esolution **P**rotocol)

Adalah protokol untuk menghubungkan IP Address dengan MAC Address di dalam jarinngan lokal, ketika sebuah perangkat ingin mengirim data ke IP Address tertentu maka ARP akan mencari MAC Address yang cocok, jika MAC Address terkait tidak ada di tabel ARP, perangkat akan mengirimkan permintaan (ARP Request) ke semua perangkat di jaringan. Perangkat yang memiliki alamat IP yang diminta ARP akan membalas dengan MAC Address nya, sehingga perangkat dapat mengirim data ke tujuan yang tepat

### DHCP (**D**ynamic **H**ost **C**onfiguration **P**rotocol)

Protokol jaringan yang digunakan untuk secara otomatis memberikan IP Address dan informasi konfigurasi jaringan ke perangkat yang terhubung ke jaringan, DHCP server akan memberikan IP Address yang tersedia dan juga informasi lain seperti subnet mask, gateway dan DNS

Proses DHCP server

<p align="center">
![DHCP](../../Assets/Images/DHCP1.png)
</p>

**DHCP Discover:** Perangkat akan mengirimkan pesan DHCP Discover untuk mencari server DHCP yang ada

**DHCP Offer:** Server DHCP yang menerima pesan dari DHCP Discover akan mengirimkan pesan yang berisi IP Address yang tersedia dan juga informasi lainnya (DHCP Offer)

**DHCP Request:** Perangkat menerima tawaran IP Address dan mengirimkan pesan DHCP Request ke server DHCP untuk meminta IP Address yang diatawarkan

**DHCP ACK:** Server DHCP mengonfirmasi dengan mengirimkan pesan yand disebut ACK (Acknowledgment) yang menyatakan bahwa perangkat sudah diberi IP Address
