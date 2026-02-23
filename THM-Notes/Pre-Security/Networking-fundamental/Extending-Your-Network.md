# TryHackMe: Extending Your Network


---

- **Room Link:** [ExtendingYourNetwork](https://tryhackme.com/room/extendingyournetwork)
- **Category:** Networking / Fundamental
- **Difficulty:** easy

---

# Overview

Ngerti gimana jaringan lokal bisa terhubung ke jaringan luar lewat port forwarding, Firewall, dan VPN.

### What is Port Forwarding ?

Port forwarding itu teknik jaringan buat ngarahin permintaan komunikasi dari alamat IP publik dan nomor port tertentu ke alamat IP privat dan port di dalam jaringan lokal (LAN) perangkat kita.

### The Firewall 101

Firewall itu perangkat jaringan yang tugasnya nentuin traffic mana yang boleh masuk atau keluar. Anggap aja firewall itu kayak satpam keamanan di suatu jaringan.

Firewall bisa ngeblokir atau ngizinin traffic jaringan tergantung beberapa faktor:

- **Dari mana traffic itu berasal?** (apakah firewall udah dikasih tau buat nerima/nolak traffic dari jaringan tertentu?)

- **Ke mana arah traffic-nya?** (apakah firewall udah dikasih tau buat nerima/nolak traffic yang ditujuin ke jaringan tertentu?)

- **Buat port apa traffic-nya?** (apakah firewall udah dikasih tau buat nerima/nolak traffic yang ditujuin ke port 80 aja?)

- **Protokol apa yang dipake?** (apakah firewall udah dikasih tau buat nerima/nolak traffic UDP, TCP, atau keduanya?)

Firewall ngecek paket buat nentuin jawaban dari pertanyaan-pertanyaan di atas. Firewall bisa dikategoriin jadi 2 sampai 5 kategori:

- **Stateful Firewall:** Jenis firewall ini ngecek seluruh informasi dari koneksi. Stateful firewall bisa kasih perlindungan yang lebih bagus dari serangan dan mastiin cuma sesi yang sah yang diizinin buat berkomunikasi.

- **Stateless Firewall:** Jenis firewall yang cuma ngecek setiap paket data secara individu tanpa peduliin status koneksi. Lebih simpel dan cepet tapi kurang efektif buat nangkal serangan yang lebih kompleks.

### VPN (Virtual Private Network)

VPN itu teknologi yang bikin koneksi jadi aman dan terenkripsi antara perangkat dan jaringan. VPN bikin jalur yang melindungi data pengguna waktu ngirim informasi lewat internet, jadi privasi dan keamanan data tetap terjaga.

Manfaat VPN:

- **Menghubungkan jaringan di lokasi geografis yang berbeda (Region Lock)**

- **Menawarkan privasi:** VPN nawarin enkripsi buat melindungi data, berguna banget kalau mau akses jaringan publik biar nggak disadap.

- **Menawarkan anonimitas:** VPN bakal nyembunyiin IP Address asli pengguna dan nggantinya pake IP Address VPN Server.

Teknologi VPN udah berkembang dari tahun ke tahun, ini beberapa teknologi yang dipake VPN:

- **PPP (Point To Point Protocol):** Teknologi ini dipake sama PPTP buat ngebolehin otentikasi dan nyediain enkripsi data. VPN bekerja pake kunci pribadi dan sertifikat publik (mirip SSH). Kunci pribadi & sertifikat harus cocok biar bisa terhubung. Teknologi ini nggak bisa ninggalin suatu jaringan sendiri (non-routable).

- **PPTP (Point To Point Tunneling Protocol):** Teknologi yang bikin data dari PPP bisa pindah dan ninggalin jaringan. PPTP gampang banget di-setup dan didukung sama sebagian besar perangkat, tapi enkripsinya lemah dibandingin alternatif lain.

- **IPSec (Internet Protocol Security):** Mengenkripsi data pake kerangka Protokol Internet (IP) yang udah ada. IPSec lebih susah di-setup dibandingin alternatif lain, tapi kalau berhasil bakal dapet enkripsi yang kuat dan didukung di banyak perangkat.
