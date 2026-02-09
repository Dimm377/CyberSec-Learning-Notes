# TryHackMe: Extending Your Network


---

**Room Link:** [ExtendingYourNetwork](https://tryhackme.com/room/extendingyournetwork)
**Category:** Networking / Fundamental
**Difficulty:** easy

---

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

Firewall melakukan pemeriksaan paket untuk menentukan jawaban atas pertanyaan-pertanyaan ini, Firewall dapat dikategorikan 2 sampai 5 kategori

- **Stateful Firewall:** Firewall jenis ini memeriksa seluruh informasi dari koneksi, stateful firewall dapat memberikan perlindungan yang lebih baik terhadap serangan dan memastikan bahwa hanya sesi yang sah yang diizinkan untuk berkomunikasi

- **Stateless Firewall:** jenis Firewall yang cuma memeriksa setiap paket data sevara individu tanpa peduli status koneksi, lebih sederhana dan cepat tetapi kurang efektif mencegah serangan yang lebih kompleks

### VPN (Virtual Private Network)

VPN adalah teknologi yeng memungkinkan koneksi yang aman dan terenkripsi antara perangkat dan jaringan, VPN menciptakan jalur yang melindungi data pengguna saat mengirim informasi melalui internet sehingga menjaga privasi dan juga keamanan data

Manfaat VPN yaitu:

- **Memungkinkan jaringan di lokasi geografis yang berbeda untuk terhubung (Region Lock)**

- **Menawarkan privasi:** Teknologi VPN menawarkan enkripsi untuk melindungi data, berguna jika ingin mengakses jaringan publik agar tidak disadap

- **Menawarkan Anonimitas:** VPN akan menyembunyikan IP Address asli pengguna dan mengganti nya dengan IP Address VPN Server

Teknologi VPN sudah berkembang dari Tahun ke tahun, ini beberapa Teknologi yang digunakan oleh VPN:

- **PPP (Point To Point Protocol):** Teknologi ini digunakan oleh PPTP untuk memungkinkan otentikasi dan menyediakan enkripsi data, VPN bekerja dengan menggunakan kunci pribadi dan sertifikat publik (mirip dengan SSH), Kunci pribadi & sertifikat harus cocok agar Anda dapat terhubung, Teknologi ini tidak bisa meninggalkan suatu jaringan dengan sendirinya (non-routable)

- **PPTP (Point To Point Tunneling Protocol):** teknologi yang memungkinkan data dari PPP berpindah dan meninggalkan jaringan, PPTP sangat mudah diatur dan didukung oleh sebagian besar perangkat. tapi enkripsinya lemah dibandingkan dengan alternatif lain

- **IPSec (Internet Protocol Security):** mengenkripsi data menggunakan kerangka Protokol Internet (IP) yang ada,
  IPSec sulit diatur dibandingkan dengan alternatif lain, jika berhasil akan memiliki enkripsi yang kuat dan juga didukung di banyak perangkat.
