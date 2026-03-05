# TryHackMe: Extending Your Network


---

- **Room Link:** [ExtendingYourNetwork](https://tryhackme.com/room/extendingyournetwork)
- **Category:** Networking / Fundamental
- **Difficulty:** easy

---

## Overview

Room ini membahas cara jaringan lokal bisa terhubung ke jaringan luar lewat Port Forwarding, Firewall, dan VPN.

---

### What is Port Forwarding?

**Port Forwarding** itu teknik mengarahkan permintaan dari alamat IP publik ke perangkat tertentu di jaringan lokal (LAN). Analogi: Bayangkan gedung apartemen punya **satu nomor telepon** (IP publik), tapi port forwarding memastikan telepon masuk diarahkan ke **kamar yang tepat** (IP privat + port spesifik).

---

### Firewall 101

Firewall itu perangkat yang menentukan traffic mana yang boleh masuk atau keluar. Ibarat **satpam di gerbang** — mengecek setiap orang yang mau masuk berdasarkan daftar aturan.

**Firewall memutuskan berdasarkan:**
- Dari mana traffic berasal? (sumber)
- Ke mana arah traffic? (tujuan)
- Untuk port apa? (layanan)
- Protokol apa yang dipakai? (TCP/UDP)

| Tipe Firewall | Analogi | Cara Kerja | Kelebihan | Kekurangan |
| ------------- | ------- | ---------- | --------- | ---------- |
| **Stateful** | Satpam yang ingat siapa yang sudah masuk | Mengecek seluruh informasi koneksi (sesi lengkap) | Perlindungan lebih menyeluruh | Lebih lambat, butuh resources lebih |
| **Stateless** | Satpam yang cuma cek KTP di pintu | Mengecek setiap paket secara individu, tanpa peduli status koneksi | Lebih cepat dan simpel | Kurang efektif menangkal serangan kompleks |

---

### VPN (Virtual Private Network)

VPN membuat koneksi jadi **aman dan terenkripsi** antara perangkat dan jaringan. Analogi: Ibarat **terowongan bawah tanah pribadi** — data kamu lewat jalur tersembunyi yang ga bisa dilihat orang lain di jalan raya (internet publik).

**Manfaat VPN:**

| Manfaat | Penjelasan |
| ------- | ---------- |
| **Bypass Region Lock** | Menghubungkan jaringan di lokasi geografis berbeda |
| **Privasi** | Enkripsi data — aman dari penyadapan di jaringan publik |
| **Anonimitas** | Menyembunyikan IP Address asli, diganti dengan IP VPN Server |

**Teknologi VPN:**

| Teknologi | Cara Kerja | Kelebihan | Kekurangan |
| --------- | ---------- | --------- | ---------- |
| **PPP** (Point-to-Point Protocol) | Otentikasi + enkripsi pakai private key & public certificate (mirip SSH) | Dasar dari PPTP | Non-routable — ga bisa keluar jaringan sendiri |
| **PPTP** (Point-to-Point Tunneling Protocol) | Membuat data PPP bisa berpindah antar jaringan | Mudah di-setup, didukung banyak perangkat | Enkripsi lemah |
| **IPSec** (Internet Protocol Security) | Enkripsi data pakai framework IP yang sudah ada | Enkripsi kuat, didukung banyak perangkat | Lebih susah di-setup |
