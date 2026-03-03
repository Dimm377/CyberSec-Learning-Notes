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



---

## Attack Flow Awareness

Nmap adalah tool pertama yang dijalankan dalam hampir semua engagement:

| Tahap | Peran Nmap |
| ----- | ---------- |
| **Reconnaissance** | Menemukan host aktif, port terbuka, versi layanan, dan OS target |
| **Vulnerability Assessment** | NSE scripts (`--script=vuln`) bisa mendeteksi kerentanan umum tanpa exploit |
| **Firewall Evasion** | Teknik fragmentasi dan scan timing dipakai untuk menghindari deteksi IDS/IPS |

**Perspektif Blue Team:**
- **IDS/IPS Detection:** Snort dan Suricata punya signature khusus untuk mendeteksi pola scan Nmap (terutama SYN scan massal dan Xmas scan).
- **Honeypots:** Blue Team bisa memasang honeypot di port-port tertentu untuk mendeteksi reconnaissance.
- **Log Monitoring:** Lonjakan koneksi ke banyak port dari satu IP dalam waktu singkat adalah indikator kuat sedang terjadi port scanning.

---

## For Real World Relevance

- **Pentest Fase Pertama:** Di setiap engagement pentest profesional, Nmap adalah tool pertama yang dijalankan. Hasilnya menentukan seluruh arah serangan selanjutnya.
- **Asset Discovery:** Tim IT dan Security menggunakan Nmap secara rutin untuk menemukan perangkat yang terhubung ke jaringan tanpa izin (_rogue devices_).
- **Compliance Scanning:** Standar seperti PCI-DSS mengharuskan organisasi melakukan _network scanning_ secara berkala untuk menemukan layanan yang terekspos.

---

## Questions

1. Kenapa SYN Scan (`-sS`) dijadikan scan default oleh Nmap dan bukan TCP Connect Scan (`-sT`)?
2. Apa yang dimaksud dengan status `open|filtered` pada UDP scan dan kenapa Nmap memberikan status ambigu ini?
3. Jika target menggunakan IDS yang mendeteksi SYN scan, teknik evasion apa yang akan kamu coba dan kenapa?
4. Bagaimana cara Blue Team membedakan antara port scanning yang sah (dari tim IT internal) dan yang jahat (dari attacker)?
