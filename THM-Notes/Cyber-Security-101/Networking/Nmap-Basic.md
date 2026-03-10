# TryHackMe: Nmap

---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/nmap)
- **Category:** Networking / Tools
- **Difficulty:** Easy

---

## Introduction

Nmap (Network Mapper) itu *tool open-source* paling populer untuk audit keamanan dan pemetaan jaringan. Ibarat radar, *tool* ini membantu kamu mengirim paket khusus ke target dan menganalisis respons baliknya. Dari situ, kamu bisa mengetahui *host* mana saja yang aktif, *port* mana yang terbuka, sampai jenis OS apa yang dipakai target.

---

## Nmap Switches

Agar Nmap tidak berjalan tanpa arah, kamu wajib menguasai *flag* dasar ini untuk mengendalikan prosesnya:

- **`-sT` (TCP Connect Scan):** Menyelesaikan proses *three-way handshake* secara utuh. Paling sopan, tapi juga paling mudah terdeteksi.
- **`-sS` (SYN Scan):** Dikenal sebagai "Half-open" *scan*. Lebih cepat dan lebih sulit terdeteksi karena koneksi diputus sebelum *handshake* selesai.
- **`-sU` (UDP Scan):** Khusus untuk mengintip layanan berbasis UDP (seperti DNS atau DHCP).
- **`-O`:** Menebak Sistem Operasi (OS) target.
- **`-sV`:** Mendeteksi *versi* aplikasi/layanan yang berjalan di *port* tersebut.
- **`-p-`:** Memindai seluruh 65.535 *port* tanpa terkecuali.
- **`-A` (Aggressive):** Paket komplit. Gabungan dari deteksi OS, versi, *script scanning*, dan *traceroute*.

---

## Scan Types (TCP, SYN, UDP)

Penting untuk memahami mekanisme di balik setiap jenis *scan*:

1. **TCP Connect Scan (`-sT`):** Nmap mencoba menyelesaikan koneksi secara penuh (SYN → SYN/ACK → ACK). Kalau berhasil, berarti *port* terbuka.
2. **SYN Scan (`-sS`):** Nmap mengirim SYN, lalu saat dibalas SYN/ACK oleh server, Nmap langsung mengirim RST (Reset) untuk memutus koneksi sebelum ACK terakhir dikirim. Karena efisien dan sulit terdeteksi, ini menjadi *scan default*.
3. **UDP Scan (`-sU`):** UDP tidak mengenal *handshake*. Aturannya: kalau port tertutup, target biasanya mengirim ICMP "Port Unreachable". Tapi kalau tidak ada respons sama sekali, Nmap akan memberikan status `open|filtered`.

---

## NULL, FIN, dan Xmas Scans

Kalau kamu menghadapi *firewall* yang ketat, coba teknik-teknik ini:

- **NULL Scan (`-sN`):** Mengirim paket kosong tanpa *flag* aktif sama sekali.
- **FIN Scan (`-sF`):** Hanya menyalakan *flag* FIN.
- **Xmas Scan (`-sX`):** Menyalakan beberapa *flag* sekaligus (PSH, URG, dan FIN).
- **Catatan:** Teknik ini efektif di sistem Unix/Linux lama, tapi biasanya gagal total jika dipakai terhadap Windows.

---

## ICMP Network Scanning

Teknik ini berguna untuk memetakan *host* mana saja yang aktif di dalam jaringan yang luas (*Ping Sweep*).

- Perintahnya: `nmap -sn 10.10.x.x/24`
- Di sini Nmap tidak akan memeriksa *port*. Dia hanya mengirim ICMP Echo Request untuk mencari tahu host mana yang aktif.

---

## NSE Scripts (The Powerhouse)

Nmap Scripting Engine (NSE) menjadikan Nmap lebih dari sekadar *scanner*. Kamu bisa mengotomatisasi tugas-tugas *recon* yang kompleks.

- **Default (`-sC`):** Menjalankan skrip bawaan yang aman (tidak akan membuat target *crash*).
- **Categories:** NSE punya banyak kategori, seperti `auth`, `brute` (menebak *password* ringan), `discovery`, `dos`, `exploit`, `malware`, sampai `vuln`.
- **Contoh:** `nmap --script=vuln <target>` — untuk mencari celah keamanan umum secara cepat di *port* yang terbuka.

---

## Firewall Evasion

Saat target dilindungi *firewall* atau IDS/IPS, kamu perlu teknik *evasion* (menghindar):

- **`-f`:** Melakukan fragmentasi paket (memotong paket menjadi kepingan kecil agar IDS kesulitan merakitnya).
- **`--mtu <value>`:** Mengatur ukuran *Maximum Transmission Unit* (potongan paket) secara manual.
- **`--scan-delay <time>`:** Menambahkan jeda waktu antar paket. IDS biasanya mudah mendeteksi jika paket dikirim terlalu cepat.
- **`--badsum`:** Sengaja mengirim paket dengan *checksum* yang salah, murni untuk menguji respons *firewall*.

---

## Conclusion

Nmap itu pondasi dari setiap operasi *penetration testing*. Dengan menguasai berbagai jenis *scan* dan skrip NSE, kamu bisa mendapatkan peta detail target secara cepat dan terkontrol.

---

## Attack Flow Awareness

Nmap selalu menjadi langkah pertama di hampir semua *engagement* cyber:

| Fase Serangan | Peran Nmap |
| :--- | :--- |
| **Reconnaissance** | Menemukan *host* aktif, *port* terbuka, versi layanan, dan menebak OS target. |
| **Vulnerability Assessment** | Menggunakan NSE scripts (`--script=vuln`) untuk mendeteksi kerentanan umum tanpa meluncurkan *exploit*. |
| **Firewall Evasion** | Memanfaatkan teknik fragmentasi dan pengaturan *scan timing* untuk melewati penjagaan IDS/IPS. |

**Perspektif Blue Team:**
- **IDS/IPS Detection:** Sistem seperti *Snort* atau *Suricata* sudah mengenali *signature* Nmap, terutama SYN *scan* massal atau *Xmas scan*.
- **Honeypots:** Tim Blue sering memasang *honeypot* di beberapa *port* palsu. Siapa pun yang mengetuk pintu itu, IP-nya langsung di-*ban*.
- **Log Monitoring:** Lonjakan koneksi dari satu IP ke puluhan port dalam waktu singkat adalah tanda jelas bahwa ada aktivitas *port scanning*.

---

## Real-World Relevance

- **Pentest Fase Pertama:** Di setiap *pentest* profesional, Nmap adalah langkah pertama. Hasil *scan* ini akan menentukan jalur serangan (*attack vector*) yang akan dipilih.
- **Asset Discovery:** Tim IT internal sering menjadikan Nmap sebagai alat untuk mendeteksi perangkat asing yang terhubung ke jaringan kantor tanpa izin (*rogue devices*).
- **Compliance Scanning:** Standar seperti PCI-DSS mewajibkan organisasi melakukan *network scanning* rutin untuk memastikan tidak ada layanan yang terbuka sembarangan.

---

## Questions

1. Kenapa **SYN Scan (`-sS`)** menjadi *scan default* di Nmap dibandingkan TCP Connect Scan (`-sT`)?
2. Kalau kamu melakukan UDP Scan dan mendapat status **`open|filtered`**, apa artinya dari sisi perilaku target?
3. Di kondisi apa kamu perlu menggunakan opsi `--scan-delay`?
