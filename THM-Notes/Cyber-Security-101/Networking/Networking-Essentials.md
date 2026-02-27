# TryHackMe: Networking Essentials


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/networkingessentials)
- **Category:** Cyber Security 101 Path
- **Difficulty:** Easy

---

## Overview

Setelah paham konsep dasar, room ini bawa kita lebih dalem ke perangkat keras dan protokol inti yang membuat internet bekerja. Mengerti cara kerja Switch, Router, dan protokol seperti DNS itu wajib agar bisa lakuin enumerasi jaringan dengan efektif.

## Network Hardware

Mengerti perbedaan antar perangkat yang menghubungkan jaringan.

- **Hubs:** Perangkat "bodoh" yang mengirim paket ke semua port yang tersedia (Broadcast). Ini mimpi buruk buat keamanan jaringan karena rawan untuk sniffing.
- **Switches:** Perangkat yang lebih pinter karena menyimpan tabel MAC Address dan cuma mengirim data ke port tujuan yang bener.
- **Routers:** Menghubungkan berbagai jaringan yang beda dan bertanggung jawab buat menentukan jalur terbaik bagi paket data (Routing).

## MAC Addresses

Identitas fisik permanen yang tertanam di kartu jaringan (NIC).

- **Format:** Terdiri dari 48-bit (6 oktet) yang direpresentasiin dalam heksadesimal.
- **OUI (Organizationally Unique Identifier):** 3 oktet pertama nunjukin siapa produsen perangkat tersebut.
  > Red Team Tip: MAC Address bisa dimanipulasi (MAC Spoofing) buat lewatin filter keamanan di jaringan lokal.

## Address Resolution Protocol (ARP)

Protokol krusial yang jadi jembatan komunikasi antara Layer 2 dan Layer 3.

- **Fungsi:** Nerjemahin IP Address jadi MAC Address agar data bisa sampai ke perangkat fisik yang tepat.
- **ARP Request:** Proses pencarian MAC Address yang dilakuin secara Broadcast ke seluruh jaringan lokal.
- **ARP Reply:** Jawaban Unicast dari pemilik IP yang berisi informasi MAC Address-nya.

> Security Insight: ARP tidak punya fitur verifikasi, jadi rentan terhadap serangan ARP Spoofing/Poisoning yang membuat teknik Man-in-the-Middle (MITM) jadi mungkin.

## Dynamic Host Configuration Protocol (DHCP)

Protokol yang otomatis memberi IP Address ke perangkat di jaringan.

- **DORA Process:** DHCP bekerja lewat empat tahap: **D**iscover, **O**ffer, **R**equest, dan **A**cknowledge.
- **Lease Time:** Jangka waktu tertentu di mana sebuah perangkat "minjem" IP Address itu sebelum harus memperbaruinya.

## Domain Name System (DNS)

"Buku telepon" internet yang nerjemahin nama domain (google.com) jadi IP Address.

- **Root Servers:** Tingkat tertinggi dalam hierarki DNS yang ngarahin query ke TLD (Top Level Domain) yang tepat.
- **Record Types:**

* **A Record:** Memetakan nama domain ke IPv4.
* **AAAA Record:** Memetakan nama domain ke IPv6.
* **CNAME:** Alias buat satu domain ke domain lainnya.
* **MX Record:** Menentukan server yang bertanggung jawab buat urusan email.

## Network Troubleshooting

Tools dasar buat mendiagnosis masalah koneksi yang berguna banget waktu lakuin Reconnaissance.

- **Ping:** Pakai protokol ICMP buat mengecek konektivitas dasar ke sebuah host.
- **Traceroute:** Ngelacak setiap router (hop) yang dilewatin paket buat identifikasi jalur jaringan.
- **Nslookup:** Lakuin query manual terhadap DNS records buat mendapatkan informasi domain target.
