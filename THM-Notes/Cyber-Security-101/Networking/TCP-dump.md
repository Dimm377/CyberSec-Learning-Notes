# TryHackMe: tcpdump


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/tcpdump)
- **Category:** Networking / Analysis
- **Difficulty:** Easy

---

---

## Introduction

`tcpdump` itu penganalisis paket berbasis CLI yang kuat banget. Tool ini jadi standar industri buat troubleshooting jaringan dan pengujian keamanan, terutama di sistem yang gak punya tampilan grafis (headless).

---

## Command Line Options

Ngerti switch dasar buat ngontrol gimana `tcpdump` bekerja.

- **`-i` (Interface):** Nentuin interface mana yang bakal dipantau (misal: `-i eth0` atau `-i tun0`).
- **`-n` (Numeric):** Nyegah `tcpdump` nerjemahin alamat IP jadi nama host, jadi prosesnya lebih cepet.
- **`-v`, `-vv`, `-vvv` (Verbose):** Nampilin detail paket yang lebih mendalam.
- **`-c` (Count):** Berhenti setelah nangkep jumlah paket tertentu.
- **`-w` (Write):** Nyimpen hasil tangkapan ke dalam file `.pcap`.
- **`-r` (Read):** Baca file `.pcap` yang udah disimpen sebelumnya.
- **`-A` (ASCII):** Nampilin isi paket dalam format teks yang bisa dibaca (berguna banget buat nyari password plain-text).

---

## Filtering Traffic

Pake filter dasar buat nyaring trafik yang relevan.

- **Host Filter:** `host 10.10.1.1` bakal nangkep trafik dari dan ke IP tersebut.
- **Port Filter:** `port 80` cuma bakal nampilin trafik HTTP.
- **Protocol Filter:** Kita bisa filter berdasarkan protokol kayak `tcp`, `udp`, atau `icmp`.
- **Directional Filter:** Pake `src` (Source) atau `dst` (Destination) buat nentuin arah trafik.
  - Contoh: `src host 10.10.1.1` cuma nangkep paket yang dikirim OLEH host tersebut.

---

## Advanced Filtering

Gabungin beberapa filter pake operator logika (Boolean).

- **AND (`and` atau `&&`):** Nampilin paket yang memenuhi kedua kriteria.
  - Contoh: `port 80 and host 10.10.1.1`.
- **OR (`or` atau `||`):** Nampilin paket yang memenuhi salah satu kriteria.
- **EXCEPT (`not` atau `!`):** Ngecualiin trafik tertentu.
  - Contoh: `not port 22` (Sembunyiin traffic SSH biar gak ganggu analisis).

---

## Practice (The Hunt)

Di bagian praktik ini, kita bakal diminta nyari informasi spesifik di dalam file pcap atau trafik live.

- **Analisis HTTP:** Pake `-A` buat liat request GET/POST dan nyari flag yang tersembunyi.
- **Cek Bendera TCP:** Kita bisa nyari paket pake flag tertentu, kayak paket `SYN` atau `RST` buat mendeteksi adanya scanning port.
  > **Pro tips:** Pake `-X` kalau mau liat data dalam bentuk Hex dan ASCII secara berdampingan buat analisis malware yang lebih dalem.

---

## Conclusion

`tcpdump` itu tool yang wajib dikuasain sama setiap praktisi keamanan. Meskipun Wireshark lebih enak dipandang, `tcpdump` ngasih kecepatan dan fleksibilitas yang gak terkalahkan di lingkungan terminal.

> **Tip:** "The command line is your sword, the packets are your guide."
