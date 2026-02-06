# TryHackMe: Nmap

**Room Link:** [TryHackMe](https://tryhackme.com/room/nmap)
**Category:** Networking / Tools
**Difficulty:** Easy

---

## Task 1 & 2: Introduction

Nmap (Network Mapper) adalah tool open-source paling populer untuk audit keamanan dan penemuan jaringan. Tool ini bekerja dengan mengirimkan paket khusus ke target dan menganalisis responnya untuk menentukan host yang aktif, port yang terbuka, hingga sistem operasi yang digunakan.

---

## Task 3: Nmap Switches

Memahami "mantra" atau flag dasar untuk mengontrol Nmap:

- **`-sT` (TCP Connect Scan):** Melakukan three-way handshake lengkap.
- **`-sS` (SYN Scan):** "Half-open" scan, lebih cepat dan lebih sulit dideteksi karena tidak menyelesaikan handshake.
- **`-sU` (UDP Scan):** Untuk memindai layanan UDP (seperti DNS atau DHCP).
- **`-O`:** Deteksi Sistem Operasi (OS).
- **`-sV`:** Deteksi versi layanan yang berjalan pada port tersebut.
- **`-p-`:** Memindai seluruh 65.535 port.
- **`-A` (Aggressive):** Gabungan dari OS detection, version detection, script scanning, dan traceroute.

---

## Task 4 - 7: Scan Types (TCP, SYN, UDP)

Memahami mekanika di balik layar scan:

1. **TCP Connect Scan (`-sT`):** Nmap mencoba menyelesaikan koneksi (SYN -> SYN/ACK -> ACK). Jika berhasil, port terbuka.
2. **SYN Scan (`-sS`):** Nmap mengirim SYN. Jika dibalas SYN/ACK, Nmap langsung mengirim RST (Reset) tanpa mengirim ACK akhir. Ini adalah scan default karena efisiensinya.
3. **UDP Scan (`-sU`):** UDP tidak memiliki handshake. Jika port tertutup, target biasanya mengirim pesan ICMP "Port Unreachable". Jika tidak ada respon, Nmap menganggap port tersebut `open|filtered`.

---

## Task 8: NULL, FIN, and Xmas Scans

Teknik "siluman" untuk melewati firewall lama:

- **NULL Scan (`-sN`):** Mengirim paket tanpa flag sama sekali.
- **FIN Scan (`-sF`):** Mengirim paket hanya dengan flag FIN.
- **Xmas Scan (`-sX`):** Mengirim paket dengan flag PSH, URG, dan FIN (berkelap-kelip seperti pohon natal).
- **Note:** Scan ini sangat efektif pada sistem berbasis Unix/Linux, namun biasanya gagal pada sistem Windows.

---

## Task 9: ICMP Network Scanning

Digunakan untuk memetakan host yang aktif di jaringan luas (Ping Sweep).

- Perintah: `nmap -sn 10.10.x.x/24`.
- Nmap tidak akan memindai port, hanya mengecek apakah host "hidup" menggunakan ICMP Echo Request.

---

## Task 10 & 11: NSE Scripts (The Powerhouse)

Nmap Scripting Engine (NSE) memungkinkan kita mengotomatisasi tugas-tugas kompleks.

- **Default (`-sC`):** Menjalankan skrip dasar yang aman.
- **Categories:** `auth`, `broadcast`, `brute`, `default`, `discovery`, `dos`, `exploit`, `external`, `fuzzer`, `intrusive`, `malware`, `safe`, `version`, dan `vuln`.
- **Example:** `nmap --script=vuln <target>` untuk mencari kerentanan umum.

---

## Task 12: Firewall Evasion

Teknik untuk menghindari deteksi IDS/IPS:

- **`-f`:** Melakukan fragmentasi paket (memecah paket menjadi bagian kecil).
- **`--mtu <value>`:** Mengatur ukuran Maximum Transmission Unit secara manual.
- **`--scan-delay <time>`:** Menambahkan jeda antar paket agar tidak memicu alarm kecepatan scan.
- **`--badsum`:** Mengirim paket dengan checksum yang salah untuk melihat respon firewall.

---

## Conclusion

Nmap adalah fondasi dari setiap pengujian penetrasi. Dengan menguasai berbagai tipe scan dan skrip NSE, kita bisa mendapatkan informasi mendalam tentang target dengan cara yang sangat efisien dan terkontrol.

> **Tip:** "Scan smart, not just fast."
