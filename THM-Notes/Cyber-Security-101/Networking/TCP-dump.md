# TryHackMe: tcpdump


---

**Room Link:** [TryHackMe](https://tryhackme.com/room/tcpdump)
**Category:** Networking / Analysis
**Difficulty:** Easy

---

---

## Task 1: Introduction

`tcpdump` adalah penganalisis paket baris perintah (CLI) yang sangat kuat. Tool ini menjadi standar industri untuk pemecahan masalah jaringan dan pengujian keamanan, terutama pada sistem tanpa antarmuka grafis (headless).

---

## Task 2: Command Line Options

Memahami switch dasar untuk mengontrol bagaimana `tcpdump` bekerja.

- **`-i` (Interface):** Menentukan interface mana yang akan dipantau (misal: `-i eth0` atau `-i tun0`).
- **`-n` (Numeric):** Mencegah `tcpdump` menerjemahkan alamat IP menjadi nama host, sehingga proses lebih cepat.
- **`-v`, `-vv`, `-vvv` (Verbose):** Menampilkan detail paket yang lebih mendalam.
- **`-c` (Count):** Berhenti setelah menangkap jumlah paket tertentu.
- **`-w` (Write):** Menyimpan hasil tangkapan ke dalam file `.pcap`.
- **`-r` (Read):** Membaca file `.pcap` yang sudah disimpan sebelumnya.
- **`-A` (ASCII):** Menampilkan isi paket dalam format teks yang bisa dibaca (sangat berguna untuk mencari password plain-text).

---

## Task 3: Filtering Traffic

Menggunakan filter dasar untuk menyaring trafik yang relevan.

- **Host Filter:** `host 10.10.1.1` akan menangkap trafik dari dan ke IP tersebut.
- **Port Filter:** `port 80` hanya akan menampilkan trafik HTTP.
- **Protocol Filter:** Kita bisa memfilter berdasarkan protokol seperti `tcp`, `udp`, atau `icmp`.
- **Directional Filter:** Menggunakan `src` (Source) atau `dst` (Destination) untuk menentukan arah trafik.
  - Contoh: `src host 10.10.1.1` hanya menangkap paket yang dikirim OLEH host tersebut.

---

## Task 4: Advanced Filtering

Menggabungkan beberapa filter menggunakan operator logika (Boolean).

- **AND (`and` atau `&&`):** Menampilkan paket yang memenuhi kedua kriteria.
  - Contoh: `port 80 and host 10.10.1.1`.
- **OR (`or` atau `||`):** Menampilkan paket yang memenuhi salah satu kriteria.
- **EXCEPT (`not` atau `!`):** Mengecualikan trafik tertentu.
  - Contoh: `not port 22` (Sembunyikan traffic SSH agar tidak mengganggu analisis).

---

## Task 5: Practice (The Hunt)

Dalam bagian praktik, lu bakal diminta buat nyari informasi spesifik di dalam file pcap atau trafik live.

- **Analisis HTTP:** Gunakan `-A` untuk melihat request GET/POST dan mencari flag yang tersembunyi.
- **Cek Bendera TCP:** Kita bisa mencari paket dengan flag tertentu, seperti paket `SYN` atau `RST` untuk mendeteksi adanya scanning port.
  > **Pro tips:** Gunakan `-X` jika lu mau liat data dalam bentuk Hex dan ASCII secara berdampingan untuk analisis malware yang lebih dalam.

---

## Conclusion

`tcpdump` adalah tool yang wajib dikuasai oleh setiap praktisi keamanan. Meskipun Wireshark lebih nyaman dipandang, `tcpdump` memberikan kecepatan dan fleksibilitas yang tidak terkalahkan di lingkungan terminal.

> **Tip:** "The command line is your sword, the packets are your guide"
