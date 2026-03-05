# TryHackMe: Networking Essentials


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/networkingessentials)
- **Category:** Cyber Security 101 Path
- **Difficulty:** Easy

---

## Overview

Setelah paham konsep dasar, room ini membawa kita lebih dalam ke perangkat keras dan protokol inti yang membuat internet bekerja. Mengerti cara kerja Switch, Router, dan protokol seperti ARP/DHCP itu wajib agar bisa melakukan enumerasi jaringan dengan efektif.

---

### Network Hardware

Memahami perbedaan antar perangkat yang menghubungkan jaringan — ibarat memahami perbedaan **alat komunikasi** di sebuah gedung kantor:

| Perangkat | Analogi | Cara Kerja |
| --------- | ------- | ---------- |
| **Hub** | Sound Horeg di ruangan — teriak ke semua orang | Mengirim paket ke **semua** port (Broadcast). Mimpi buruk buat keamanan karena rawan sniffing |
| **Switch** | Resepsionis pintar — tau siapa di meja mana | Menyimpan tabel MAC Address dan cuma mengirim data ke port tujuan yang benar |
| **Router** | Google Maps — Memilih jalur terbaik antar kota | Menghubungkan jaringan berbeda dan menentukan jalur terbaik bagi paket data (Routing) |

---

### MAC Addresses

**Identitas fisik permanen** yang tertanam di kartu jaringan (NIC). Ibarat **akta kelahiran** — setiap perangkat punya satu yang unik sejak diproduksi.

| Aspek | Detail |
| ----- | ------ |
| **Format** | 48-bit (6 oktet) dalam heksadesimal, contoh: `AA:BB:CC:DD:EE:FF` |
| **OUI** (3 oktet pertama) | Menunjukkan produsen perangkat (misal: `00:1A:2B` = produsen X) |

> **Red Team Tip:** MAC Address bisa dimanipulasi (**MAC Spoofing**) buat melewati filter keamanan di jaringan lokal.

---

### Address Resolution Protocol (ARP)

ARP itu protokol yang jadi **jembatan** antara Layer 2 (MAC) dan Layer 3 (IP). Analogi: Bayangkan kamu sebuah **pesta** dan tau nama seseorang (IP) tapi ga tau dia duduk di meja mana (MAC). ARP itu proses teriak ke seluruh ruangan menanyakan orangnya.

| Tahap | Analogi | Yang Terjadi |
| ----- | ------- | ------------ |
| **ARP Request** | Teriak ke seluruh ruangan: Siapa yang punya IP ini? | Broadcast ke semua perangkat di jaringan lokal |
| **ARP Reply** | Orang yang dicari angkat tangan: Itu aku, ini MAC-ku | Jawaban Unicast dari pemilik IP berisi MAC Address-nya |

> **Security Insight:** ARP tidak punya fitur verifikasi — siapa saja bisa mengaku sebagai pemilik IP tertentu. Ini yang membuat serangan **ARP Spoofing/Poisoning** dan teknik **Man-in-the-Middle (MITM)** menjadi mungkin.

---

### Dynamic Host Configuration Protocol (DHCP)

DHCP itu protokol yang **otomatis memberi IP Address** ke perangkat di jaringan, Ibarat proses **sewa kamar apartemen** — kamu datang, minta kamar, dikasih nomor kamar untuk jangka waktu tertentu.

**DORA Process (wajib diingat):**

| Tahap | Analogi Apartemen | Yang Terjadi |
| ----- | ----------------- | ------------ |
| **D**iscover | Halo, ada kamar kosong ga? | Client broadcast mencari DHCP server |
| **O**ffer | Ada! Kamar 192.168.1.50 tersedia | Server menawarkan IP Address |
| **R**equest | Oke, saya mau kamar itu | Client mengonfirmasi mau IP tersebut |
| **A**cknowledge | Deal ya, ini kuncinya, berlaku 24 jam | Server mengonfirmasi dan memberi Lease Time |

**Lease Time:** Jangka waktu perangkat meminjam IP Address sebelum harus memperbaruinya.

---

### Domain Name System (DNS)

Buku telepon internet yang menerjemahkan nama domain (`google.com`) jadi IP Address.

| Konsep | Penjelasan |
| ------ | ---------- |
| **Root Servers** | Tingkat tertinggi dalam hierarki DNS yang mengarahkan query ke TLD yang tepat |
| **A Record** | Memetakan nama domain ke IPv4 |
| **AAAA Record** | Memetakan nama domain ke IPv6 |
| **CNAME** | Alias dari satu domain ke domain lainnya |
| **MX Record** | Menentukan server yang bertanggung jawab mengurus email |

*(Penjelasan lebih lengkap tentang DNS ada di catatan [DNS-In-Details](../../Pre-Security/How-The-Web-Works/DNS-In-Details.md))*

---

### Network Troubleshooting

Tools dasar buat mendiagnosis masalah koneksi — sangat berguna saat melakukan **Reconnaissance**:

| Tool | Analogi | Fungsi |
| ---- | ------- | ------ |
| `ping` | Ketuk pintu: "Kamu masih hidup ga?" | Pakai ICMP buat mengecek konektivitas dasar ke sebuah host |
| `traceroute` | Lacak jalur perjalanan paket | Melihat setiap router (hop) yang dilewati paket — identifikasi bottleneck |
| `nslookup` | Tanya buku telepon DNS secara manual | Query DNS records buat mendapatkan informasi domain target |
