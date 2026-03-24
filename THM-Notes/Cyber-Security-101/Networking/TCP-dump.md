# TryHackMe: TCP-dump


---

* **Room Link:** [TryHackMe](https://tryhackme.com/room/tcpdump)
* **Category:** Networking / Analysis
* **Difficulty:** Easy

---

---

## Introduction

`tcpdump` itu penganalisis paket berbasis CLI yang jadi standar industri buat troubleshooting jaringan dan pengujian keamanan. Seperti **penyadap telepon** — dia diam-diam mendengar dan mencatat semua percakapan yang terjadi di jaringan. Terutama berguna di sistem tanpa GUI (headless).

*(Kalau butuh versi GUI-nya, lihat catatan [Wireshark](./Wireshark-basic.md))*

---

### Command Line Options

| Flag | Nama | Fungsi |
| :--: | ---- | ------ |
| `-i` | Interface | Menentukan interface yang dipantau (contoh: `-i eth0`, `-i tun0`) |
| `-n` | Numeric | Mencegah resolusi hostname → proses lebih cepat |
| `-v` / `-vv` / `-vvv` | Verbose | Detail paket makin mendalam setiap level |
| `-c` | Count | Berhenti setelah menangkap sejumlah paket tertentu |
| `-w` | Write | Menyimpan hasil tangkapan ke file `.pcap` |
| `-r` | Read | Membaca file `.pcap` yang sudah disimpan |
| `-A` | ASCII | Menampilkan isi paket dalam format teks — berguna buat mencari password plain-text |
| `-X` | Hex + ASCII | Menampilkan data dalam Hex dan ASCII berdampingan — berguna buat analisis malware |

---

### Filtering Traffic

Filter dasar buat menyaring traffic yang relevan:

| Tipe Filter | Contoh | Fungsi |
| ----------- | ------ | ------ |
| **Host** | `host 10.10.1.1` | Menangkap traffic dari dan ke IP tersebut |
| **Port** | `port 80` | Hanya menampilkan traffic HTTP |
| **Protocol** | `tcp`, `udp`, `icmp` | Filter berdasarkan protokol |
| **Source** | `src host 10.10.1.1` | Hanya paket yang **dikirim oleh** host tersebut |
| **Destination** | `dst port 443` | Hanya paket yang **menuju** port tersebut |

---

### Advanced Filtering (Boolean Operators)

Menggabungkan beberapa filter pakai operator logika:

| Operator | Alias | Fungsi | Contoh |
| -------- | ----- | ------ | ------ |
| **AND** | `&&` | Paket harus memenuhi **kedua** kriteria | `port 80 and host 10.10.1.1` |
| **OR** | `\|\|` | Paket memenuhi **salah satu** kriteria | `port 80 or port 443` |
| **NOT** | `!` | **Mengecualikan** traffic tertentu | `not port 22` (sembunyikan SSH) |

---

### Practice (The Hunt)

Di bagian praktik, kita diminta mencari informasi spesifik di dalam file pcap atau traffic live:

* **Analisis HTTP:** Pakai `-A` buat melihat request GET/POST dan mencari flag tersembunyi
* **Cek Flag TCP:** Mencari paket dengan flag tertentu (seperti `SYN` atau `RST`) buat mendeteksi port scanning

---

### Conclusion

`tcpdump` itu tool yang wajib dikuasai setiap praktisi keamanan. Meskipun Wireshark lebih enak dipandang, `tcpdump` memberi kecepatan dan fleksibilitas yang tidak terkalahkan di lingkungan terminal.
