# TryHackMe: Nmap

---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/nmap)
- **Category:** Networking / Tools
- **Difficulty:** Easy

---

## Introduction

Nmap (Network Mapper) itu *tool open-source* paling primadona buat audit keamanan dan pemetaan jaringan. Ibarat radar, *tool* ini membantu kamu mengirim "gelombang" paket khusus ke target dan menganalisa pantulan responnya. Dari pantulan itu, kamu bisa menebak *host* mana aja yang lagi hidup, pintu (*port*) mana yang kebuka, sampai jenis OS apa yang lagi dipakai si korban.

---

## Nmap Switches

Biar nmap nggak jalan buta, kamu wajib menguasai *flag* dasar ini buat mengendalikan serangannya:

- **`-sT` (TCP Connect Scan):** Sopan banget, dia menyelesaikan proses *three-way handshake* secara utuh.
- **`-sS` (SYN Scan):** Dikenal sbg "Half-open" *scan*. Jalannya cepet dan lebih susah tertangkap radar karena dia kabur sebelum *handshake* beres.
- **`-sU` (UDP Scan):** Khusus dipakai buat mengintip layanan berbasis UDP (kayak DNS atau DHCP).
- **`-O`:** Ahlinya menebak Sistem Operasi (OS).
- **`-sV`:** Detektif buat mencari tau *versi* aplikasi/layanan apa yang bertengger di *port* tersebut.
- **`-p-`:** Sikat semua, Tujuannya memindai seluruh 65.535 *port* tanpa terkecuali.
- **`-A` (Aggressive):** Paket komplit barbar. Gabungan langsung dari tebak OS, versi, *script scanning*, dan jalurnya (*traceroute*).

---

## Scan Types (TCP, SYN, UDP)

Penting banget buat mengerti mekanika di balik layar tiap jenis *scan*-nya:

1. **TCP Connect Scan (`-sT`):** Nmap dengan jujur mencoba menyelesaikan koneksi (membawa SYN -> dibalas SYN/ACK -> mengirim balik ACK). Kalau mulus, berarti *port* itu terbuka lebar.
2. **SYN Scan (`-sS`):** Nmap mengawalinya normal mengirim SYN. Nah, pas dibalas SYN/ACK sama server, Nmap langsung mengirim RST (Reset) buat memutus sambungan sebelum ACK terakhir dikirim. Makanya ini dijadikan *scan default* karena efisien dan licik.
3. **UDP Scan (`-sU`):** UDP itu kan ga mengenal budaya *handshake*. Aturannya: Kalau portnya "ketutup", target biasanya memberikan pesan ICMP "Port Unreachable". Tapi kalau nggak ada respon balik sama sekali, Nmap bakal ragu dan ngasih status `open|filtered`.

---

## NULL, FIN, dan Xmas Scans

Kalau kamu lagi buntu menghadapi *firewall* yang ketat, cobain deh teknik ini:

- **NULL Scan (`-sN`):** Murni mengirim paket kosongan tanpa *flag* aktif sama sekali.
- **FIN Scan (`-sF`):** Cuma modal menyalakan *flag* FIN doang.
- **Xmas Scan (`-sX`):** *Flag*-nya dihidupin rame-rame (PSH, URG, dan FIN).
- **Note:** *Scan* aneh kek gini efeknya mematikan banget di sistem keluarga Unix/Linux lama, tapi biasanya bakal diketawain (gagal total) kalau dipakai menembus Windows.

---

## ICMP Network Scanning

Teknik ini kepake banget buat memetakan *host* mana aja yang lagi aktif di dalem jaringan yang luas (*Ping Sweep*).

- Perintahnya: `nmap -sn 10.10.x.x/24`
- Di sini Nmap nggak bakal peduli mengecek *port*. Dia murni cuma mengetuk pintu pakai ICMP Echo Request buat tanya, *"Bro, lo idup ga?"*

---

## NSE Scripts (The Powerhouse)

Nmap Scripting Engine (NSE) ini yang bikin Nmap naik level dari sekadar *scanner* jadi alat peretas otomatis mini. Kamu bisa mengotomatisasi tugas-tugas *recon* yang kompleks.

- **Default (`-sC`):** Menjalankan skrip bawaan yang *safe* (ga bikin *server* target *crash*).
- **Categories:** NSE ini punya banyak *genre* lho, kayak `auth`, `brute` (buat tebak *password* ringan), `discovery`, `dos`, `exploit`, `malware`, sampai `vuln`.
- **Example:** `nmap --script=vuln <target>` (Mantra andalan buat mencari celah keamanan umum secara instan di *port* yang kebuka).

---

## Firewall Evasion

Pas targetmu dilindungin *firewall* atau IDS/IPS yang cerewet, kamu harus main cantik pakai teknik *evasion* (menghindar):

- **`-f`:** Melakukan fragmentasi paket (memotong-motong paket asli jadi kepingan kecil biar IDS pusing merakitnya).
- **`--mtu <value>`:** Mengatur ukuran *Maximum Transmission Unit* (potongan paket) secara manual.
- **`--scan-delay <time>`:** Menambahkan jeda waktu antar paket. IDS biasanya gampang kepancing kalau kamu menyepam terlalu cepet. Pelan-pelan asal tidak terdeteksi.
- **`--badsum`:** Sengaja mengirim paket pakai *checksum* (itungan validasi) yang salah/cacat murni buat tes respon *firewall* doang.

---

## Conclusion

Nmap itu ibarat pondasi dari setiap operasi *penetration testing*. Dengan menguasai berbagai racikan *scan* dan skrip NSE, kamu bisa dapetin denah peta musuh yang sangat detail dengan cara yang mematikan, cepat, dan tetap terkontrol.

---

## Attack Flow Awareness

Nmap selalu jadi Orang Pertama yang Maju di hampir semua *engagement* cyber sungguhan:

| Fase Serangan | Jatah Kerjanya Nmap |
| :--- | :--- |
| **Reconnaissance** | Murni memecahkan teka-teki: menemukan *host* idup, *port* mana aja yang kebuka, versi layanannya apa, dan menebak OS target. |
| **Vulnerability Assessment** | Mengandalkan NSE scripts (`--script=vuln`) buat mendeteksi kerentanan umum tanpa sampe meluncurkan *exploit* masuk ke sistem. |
| **Firewall Evasion** | Main petak umpet pakai teknik fragmentasi dan mengatur *scan timing* buat melewati penjagaan IDS/IPS yang ketat di depan. |

**Gimana Blue Team (Blue Team) Melihat Ini:**
- **IDS/IPS Detection:** Mesin seperti *Snort* atau *Suricata* sudah hafal banget sama *signature* (pola khusus) Nmap. Terutama kalau kamu nekat menembak SYN *scan* massal atau mencoba *Xmas scan*.
- **Honeypots:** Tim Blue sering memasang pintu jebakan (*honeypots*) di beberapa *port* palsu. Siapa aja yang nekat mengetok pintu itu (lewat *recon* Nmap), IP-nya langsung di-*ban*.
- **Log Monitoring:** Lonjakan koneksi drastis dari satu IP ke puluhan port yang beda-beda di waktu sekejap itu ibarat menyalakan kembang api raksasa kalau lagi ada aktivitas *port scanning*.

---

## Real-World Relevance

- **Pentest Fase Pertama:** Di setiap taruhan *pentest* profesional sungguhan, Nmap adalah peluru pertama yang ditembakkan. Laporan hasil *scan* ini 100% bakal menentukan jalur retas (*attack vector*) apa yang bakal dipilih tim buat menjebol ke dalam.
- **Asset Discovery:** Nyatanya, tim IT internal dan *Security* malah jadikan Nmap sebagai sahabat setia buat mengabsen (*discovery*) perangkat aneh yang asik nempel menumpang ke *Wi-Fi* atau jaringan kantor tanpa izin resmi (*rogue devices*).
- **Compliance Scanning:** Ada standar birokrasi elit macem PCI-DSS (buat lembaga pengelola kartu kredit) yang memaksa organisasi melakukan *network scanning* rutin. Tujuannya cuma satu: memergoki apakah admin sembrono membuka sembarang layanan di internet.

---

## Quick Check (Active Recall)

1. Coba tebak, kenapa si **SYN Scan (`-sS`)** dinobatkan jadi pangeran *scan default* oleh Nmap dibandingin kakaknya si TCP Connect Scan (`-sT`)?
2. Kalau kamu mengirim serbuan UDP Scan, terus dapet laporan balik dengan status aneh **`open|filtered`**, apa maksudnya dari sisi kelakuan si targetnya?
3. Kenapa Nmap memberikan kamu opsi aneh `--scan-delay`? Pas kondisi perang nyata apa kamu bakal terpaksa kepikir pakai ginian?
