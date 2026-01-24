# TryHackMe: Intro to LAN 🌐

**Room Link:** [Intro to LAN](https://tryhackme.com/room/introtolan)
**Category:** Networking / Fundamental
**Difficulty:** Easy

---

## 📝 Overview

Room ini ngebahas dasar-dasar Local Area Network (LAN), mulai dari topologi, perangkat jaringan (Hub, Switch, Router), sampe cara kerja OSI Model dalam ngirim data. Sebagai calon _security professional_, paham LAN itu wajib biar tau gimana cara trafik data "bergerak" di dalem network.

---

## 🚀 Task Summaries & Key Concepts

### 1. What is a LAN?

LAN itu jaringan kecil (rumah, kantor, sekolah). Point pentingnya:

- Komunikasi antar perangkat biasanya lewat **Ethernet** atau **Wi-Fi**.
- Gak butuh internet buat komunikasi antar komputer di dalem satu LAN.

### 2. Networking Devices (The Hardware)

Di sini gw belajar bedanya "kasta" perangkat:

- **Hub:** Cupu, asal broadcast data ke semua port (berisik & gak aman).
- **Switch:** Lebih pinter, pake MAC Address buat kirim data ke tujuan yang bener.
- **Router:** "Gerbang" antar network yang beda (pake IP Address).

### 3. The OSI Model (7 Layers)

Ini bagian paling penting! Gw ngerangkum biar gampang inget:

1. **Physical:** Kabel, sinyal listrik.
2. **Data Link:** MAC Address, Frames (Switch main di sini).
3. **Network:** IP Address, Packets (Router main di sini).
4. **Transport:** TCP/UDP (Ngatur pengiriman data).
5. **Session:** Jaga koneksi tetep kebuka.
6. **Presentation:** Format data (enkripsi, kompresi).
7. **Application:** Apa yang user liat (HTTP, FTP).

---

## 🚩 Task Answers (Summary)

_Note: Gue nggak naruh semua flag biar lo tetep nyari, tapi ini rangkuman logic-nya._

- **Task 4 (Subnetting):** Paham kalau `/24` itu artinya ada 256 IP, tapi yang bisa dipake cuma 254.
- **Task 5 (ARP):** Address Resolution Protocol itu cara komputer nanya: _"Eh, IP ini MAC Address-nya siapa?"_

---

## 💡 Personal Takeaway

Jangan remehin teori LAN. Kalau lo gak tau bedanya **Switch** sama **Router**, lo bakal bingung pas nanti belajar _Man-in-the-Middle (MitM) Attack_ atau _Network Pivoting_.
