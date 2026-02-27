# TryHackMe: Nmap


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/nmap)
- **Category:** Networking / Tools
- **Difficulty:** Easy

---

---

## & 2: Introduction

Nmap (Network Mapper) itu tool open-source paling populer buat audit keamanan dan penemuan jaringan. Tool ini bekerja dengan mengirim paket khusus ke target dan nganalisis responnya buat menentukan host yang aktif, port yang terbuka, sampai sistem operasi yang dipake.

---

## Nmap Switches

Mengerti "mantra" atau flag dasar buat ngontrol Nmap:

- **`-sT` (TCP Connect Scan):** Lakuin three-way handshake lengkap.
- **`-sS` (SYN Scan):** "Half-open" scan, lebih cepat dan lebih susah dideteksi karena tidak menyelesaiin handshake.
- **`-sU` (UDP Scan):** Buat mindai layanan UDP (seperti DNS atau DHCP).
- **`-O`:** Deteksi Sistem Operasi (OS).
- **`-sV`:** Deteksi versi layanan yang jalan di port tersebut.
- **`-p-`:** Mindai seluruh 65.535 port.
- **`-A` (Aggressive):** Gabungan dari OS detection, version detection, script scanning, dan traceroute.

---

## 7: Scan Types (TCP, SYN, UDP)

Mengerti mekanika di balik layar scan:

1. **TCP Connect Scan (`-sT`):** Nmap nyoba menyelesaiin koneksi (SYN -> SYN/ACK -> ACK). Kalau berhasil, port terbuka.
2. **SYN Scan (`-sS`):** Nmap mengirim SYN. Kalau dibales SYN/ACK, Nmap langsung mengirim RST (Reset) tanpa mengirim ACK akhir. Ini scan default karena efisiensinya.
3. **UDP Scan (`-sU`):** UDP tidak punya handshake. Kalau port tertutup, target biasanya mengirim pesan ICMP "Port Unreachable". Kalau tidak ada respon, Nmap nganggep port itu `open|filtered`.

---

## NULL, FIN, and Xmas Scans

Teknik "siluman" buat lewatin firewall lama:

- **NULL Scan (`-sN`):** Mengirim paket tanpa flag sama sekali.
- **FIN Scan (`-sF`):** Mengirim paket cuma pakai flag FIN.
- **Xmas Scan (`-sX`):** Mengirim paket pakai flag PSH, URG, dan FIN (berkelap-kelip seperti pohon natal).
- **Note:** Scan ini efektif banget di sistem Unix/Linux, tapi biasanya gagal di Windows.

---

## ICMP Network Scanning

Dipake buat memetakan host yang aktif di jaringan luas (Ping Sweep).

- Perintah: `nmap -sn 10.10.x.x/24`.
- Nmap tidak bakal mindai port, cuma mengecek apakah host "hidup" pakai ICMP Echo Request.

---

## & 11: NSE Scripts (The Powerhouse)

Nmap Scripting Engine (NSE) membuat kita bisa ngotomatisasi tugas-tugas kompleks.

- **Default (`-sC`):** Jalanin skrip dasar yang aman.
- **Categories:** `auth`, `broadcast`, `brute`, `default`, `discovery`, `dos`, `exploit`, `external`, `fuzzer`, `intrusive`, `malware`, `safe`, `version`, dan `vuln`.
- **Example:** `nmap --script=vuln <target>` buat mencari kerentanan umum.

---

## Firewall Evasion

Teknik buat ngehindarin deteksi IDS/IPS:

- **`-f`:** Lakuin fragmentasi paket (mecah paket jadi bagian kecil).
- **`--mtu <value>`:** Ngatur ukuran Maximum Transmission Unit secara manual.
- **`--scan-delay <time>`:** Nambahin jeda antar paket agar tidak nge-trigger alarm kecepatan scan.
- **`--badsum`:** Mengirim paket pakai checksum yang salah buat melihat respon firewall.

---

## Conclusion

Nmap itu fondasi dari setiap pengujian penetrasi. Dengan menguasai berbagai tipe scan dan skrip NSE, kita bisa mendapatkan informasi mendalam tentang target dengan cara yang efisien dan terkontrol.

> **Tip:** "Scan smart, not just fast."
