# TryHackMe: Packet & Frames

**Room Link:** [OSI Model](https://tryhackme.com/room/packetsframes)
**Category:** Networking / Fundamental
**Difficulty:** easy

# Overview

### What is Packet & Frames ?

Paket & Frames adalah bagian kecil dari sebuah data yang mana ketika digabungkan akan membentuk suatu informasi / pesan yang lebih besar, tapi keduanya merupakan dua hal yang berbeda di dalam OSI Model

1. **Packet:** Packet adalah bagian data dari Lapisan ke 3 OSI Model (Network Layer), Packet berisi tentang informasi header IP,

2. **Frames:** Frames adalah unit data yang ada di lapisan ke 3 (Data link layer)

### Encapsulation

Proses membungkus data dengan informasi kontrol berupa (header dan trailer) pada saat data tersebut bergerak turun dari lapisan atas ke lapisan bawah

**alur encapsulation:**

- **Application to Session Layer:** Data asli dibuat oleh aplikasi (misalnya pesan chat atau file gambar)

- **Transport Layer (Segment):** Data dipecah menjadi bagian-bagian kecil dan diberikan Header Transport (seperti nomor port TCP/UDP)

- **Network Layer (Packet):** Segmen tadi dibungkus lagi dengan IP Header yang berisi alamat IP sumber dan tujuan. Di tahap ini, unit datanya disebut Packet

- **Data Link Layer (Frame):** Packet dimasukkan ke dalam bungkusan terakhir yang berisi MAC Address,Unit data ini disebut Frame

- **Physical Layer (Bits):** Frame diubah menjadi sinyal listrik atau cahaya (bit 0 dan 1) untuk beneran dikirim lewat kabel.
