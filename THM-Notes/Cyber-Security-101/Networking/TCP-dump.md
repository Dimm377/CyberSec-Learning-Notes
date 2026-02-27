# TryHackMe: tcpdump


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/tcpdump)
- **Category:** Networking / Analysis
- **Difficulty:** Easy

---

---

## Introduction

`tcpdump` itu penganalisis paket berbasis CLI yang kuat banget. Tool ini jadi standar industri buat troubleshooting jaringan dan pengujian keamanan, terutama di sistem yang tidak punya tampilan grafis (headless).

---

## Command Line Options

Mengerti switch dasar buat ngontrol gimana `tcpdump` bekerja.

- **`-i` (Interface):** Menentukan interface mana yang bakal dipantau (misal: `-i eth0` atau `-i tun0`).
- **`-n` (Numeric):** Nyegah `tcpdump` nerjemahin alamat IP jadi nama host, jadi prosesnya lebih cepat.
- **`-v`, `-vv`, `-vvv` (Verbose):** Menampilkan detail paket yang lebih mendalam.
- **`-c` (Count):** Berhenti setelah nangkep jumlah paket tertentu.
- **`-w` (Write):** Menyimpan hasil tangkapan ke dalam file `.pcap`.
- **`-r` (Read):** Baca file `.pcap` yang sudah disimpen sebelumnya.
- **`-A` (ASCII):** Menampilkan isi paket dalam format teks yang bisa dibaca (berguna banget buat mencari password plain-text).

---

## Filtering Traffic

Pakai filter dasar buat nyaring trafik yang relevan.

- **Host Filter:** `host 10.10.1.1` bakal nangkep trafik dari dan ke IP tersebut.
- **Port Filter:** `port 80` cuma bakal menampilkan trafik HTTP.
- **Protocol Filter:** Kita bisa filter berdasarkan protokol seperti `tcp`, `udp`, atau `icmp`.
- **Directional Filter:** Pakai `src` (Source) atau `dst` (Destination) buat menentukan arah trafik.
  - Contoh: `src host 10.10.1.1` cuma nangkep paket yang dikirim OLEH host tersebut.

---

## Advanced Filtering

Gabungin beberapa filter pakai operator logika (Boolean).

- **AND (`and` atau `&&`):** Menampilkan paket yang memenuhi kedua kriteria.
  - Contoh: `port 80 and host 10.10.1.1`.
- **OR (`or` atau `||`):** Menampilkan paket yang memenuhi salah satu kriteria.
- **EXCEPT (`not` atau `!`):** Ngecualiin trafik tertentu.
  - Contoh: `not port 22` (Sembunyiin traffic SSH agar tidak ganggu analisis).

---

## Practice (The Hunt)

Di bagian praktik ini, kita bakal diminta mencari informasi spesifik di dalam file pcap atau trafik live.

- **Analisis HTTP:** Pakai `-A` buat melihat request GET/POST dan mencari flag yang tersembunyi.
- **Cek Bendera TCP:** Kita bisa mencari paket pakai flag tertentu, seperti paket `SYN` atau `RST` buat mendeteksi adanya scanning port.
  > **Pro tips:** Pakai `-X` kalau mau melihat data dalam bentuk Hex dan ASCII secara berdampingan buat analisis malware yang lebih dalem.

---

## Conclusion

`tcpdump` itu tool yang wajib dikuasain sama setiap praktisi keamanan. Meskipun Wireshark lebih enak dipandang, `tcpdump` memberi kecepatan dan fleksibilitas yang tidak terkalahkan di lingkungan terminal.

> **Tip:** "The command line is your sword, the packets are your guide."
