# TryHackMe: What Is Networking?


---

- **Room Link:** [What Is Networking](https://tryhackme.com/room/whatisnetworking)
- **Category:** Networking / Fundamental
- **Difficulty:** easy

---

## Overview

- Membahas fundamental paling dasar tentang apa itu networking
- **Simulasi wifi hotel:** Praktik langsung buat ngelihat alur data di jaringan wifi hotel lewat 2 skenario pengguna:
  - **Pengguna 1 (Sudah bayar layanan wifi):** router bakal ngizinin dan ngirim data dengan lancar
  - **Pengguna 2 (Belum bayar layanan wifi):** setiap kali pengguna 2 ngirim data, permintaannya bakal ditolak sama router
- Identitas perangkat (Ngerti peran penting IP Address dan MAC Address buat mastiin data sampai ke tujuan yang tepat)

## Task Summary & Key Concept

### 1. What is Networking?

Networking itu proses menghubungkan komputer atau perangkat lain supaya bisa saling berkomunikasi dan berbagi sumber daya. Prosesnya melibatkan penggunaan _networking devices_ kayak switch, hub, kabel, dan router buat ngebangun jaringan. Networking juga ngatur dan ngelola koneksi biar data bisa dikirim dan diterima dengan efisien. Tujuannya simpel: bikin perangkat-perangkat ini bisa ngobrol dan kerja bareng di berbagai lingkungan.

### Where can the network be found?

Jaringan bisa ditemuin di mana-mana, mulai dari rumah, kantor, sekolah, fasilitas umum, sampai infrastruktur kayak sistem transportasi, jaringan listrik, dan tentunya internet.

### Identity networks

#### IP Address (Internet Protocol Address)

IP Address itu deretan angka yang dipake buat ngidentifikasi perangkat di jaringan, supaya perangkat-perangkat ini bisa saling berkomunikasi dan tukar data lewat internet. Ada 2 jenis alamat IP:

- **IPv4:** terdiri dari 4 kelompok angka yang dipisahin titik (disebut octet), **contoh: (`192.168.1.1`)**. Pake alamat 32-bit, jadi bisa nampung sekitar 4,3 miliar alamat unik.

- **IPv6:** versi terbaru dari IP Address yang dibuat buat ngatasi keterbatasan IPv4. Pake alamat 128-bit, jadi jumlah alamat uniknya jauh lebih banyak. Alamatnya terdiri dari 8 kelompok 4 digit heksadesimal yang dipisahin titik dua (`:`).
  **contoh:(`2001:0db8:85a3:0000:0000:8a2e:0370:7334`)**

#### MAC Address (Media Access Control Address)

MAC Address itu identitas fisik jaringan yang permanen dan unik, ada di setiap perangkat jaringan. Alamat ini udah ditentuin sama pabrik yang bikin perangkatnya. Pake alamat 48-bit yang terdiri dari 12 karakter heksadesimal, dipisahin tanda titik dua (`:`).

### Ping (ICMP)

Ping itu salah satu alat jaringan paling dasar. Ping pake paket ICMP (Internet Control Message Protocol) buat mendiagnosis masalah konektivitas, ngukur latensi jaringan, dan nentuin apakah perangkat bisa dijangkau di jaringan.

Syntax buat pake ping:

```
$ ping website url / IP address
```

<p align="center">
![Ping](../../Assets/Images/ping.png)
</p>

Di gambar itu aku nge ping ke URL `google.com`. Ping kasih tau bahwa Google bales pake alamat IPv6, aku ngirim 8 paket ICMP dengan waktu sekitar 39.4 ms.
