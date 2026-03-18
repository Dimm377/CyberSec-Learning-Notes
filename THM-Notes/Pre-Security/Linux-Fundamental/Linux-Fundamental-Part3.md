# TryHackMe: Linux Fundamentals Part 3


---

* **Room Link:** [Linux Fundamentals Part 3](https://tryhackme.com/room/linuxfundamentalspart3)
* **Category:** Linux / Fundamental
* **Difficulty:** Easy

---

## Overview

Room ini fokus ke alat-alat utility yang akan sering dipakai sehari-hari, cara mengelola proses sistem, sampai mengotomatisasi tugas agar sistem tetap berjalan optimal.

---

### Terminal Text Editors

Edit file langsung lewat terminal itu skill wajib. Dua editor utama:

| Editor | Analogi | Karakteristik |
| ------ | ------- | ------------- |
| **Nano** | Notepad sederhana — tinggal tulis | Ramah pemula, ada panduan shortcut di bagian bawah layar |
| **Vim/Neovim** | Cockpit pesawat — powerful tapi butuh pelatihan | Editor dengan mode berbeda (Normal, Insert, Visual). Jauh lebih cepat begitu sudah terbiasa |

---

### General/Useful Utilities

Linux menyediakan banyak tools kecil yang sangat berguna buat transfer data dan komunikasi:

| Tool | Analogi | Fungsi |
| ---- | ------- | ------ |
| `wget` | **Kurir pengantar** — kasih URL, dia langsung jemput | Download file langsung dari web server lewat terminal |
| `scp` | **Truk lapis baja** — kirim file lewat jalur aman | Salin file antar mesin secara aman lewat protokol SSH |
| `python3 -m http.server` | **Buka lapak dadakan** | Membuat web server instan buat berbagi file di jaringan lokal |

---

### Processes 101

Belajar cara melihat dan mengendalikan aplikasi yang sedang berjalan — seperti **task manager** di Windows, tapi lebih powerful.

| Command | Fungsi |
| ------- | ------ |
| `ps aux` | Melihat daftar semua proses yang sedang berjalan secara detail |
| `htop / btop` | Monitor penggunaan CPU & RAM secara real-time |
| `kill <PID>` | Mengirim sinyal ke proses berdasarkan Process ID |

**Jenis sinyal `kill`:**

| Signal | Nomor | Analogi | Fungsi |
| ------ | :---: | ------- | ------ |
| **SIGTERM** | 15 | Minta sopan: Tolong berhenti ya | Proses diminta berhenti dan diberi waktu cleanup |
| **SIGKILL** | 9 | Menghentikan secara paksa | Hentikan proses secara paksa — tanpa cleanup |
| **SIGSTOP** | 19 | Menangguhkan proses | Tangguhkan proses sementara (bisa dilanjut nanti) |

---

### Maintaining Your System: Automation

Agar tidak perlu mengerjakan tugas yang sama berulang-ulang, Linux punya **cron** — seperti **alarm/pengingat otomatis** yang menjalankan perintah di waktu yang sudah ditentukan.

| Command | Fungsi |
| ------- | ------ |
| `crontab -e` | Edit jadwal cron — atur kapan script/perintah dieksekusi otomatis |

---

### Maintaining Your System: Package Management

Mengelola perangkat lunak di distribusi berbasis Debian/Ubuntu:

| Konsep | Analogi | Penjelasan |
| ------ | ------- | ---------- |
| **`apt`** | Play Store di HP | Tool utama buat install, update, dan hapus aplikasi |
| **Repositories** | Gudang aplikasi terpusat | Linux mengambil aplikasi dari repo, beda sama Windows yang pakai file installer `.exe` |

---

### Maintaining Your System: Logs

Sistem Linux mencatat semua aktivitas di dalam file log yang tersimpan di `/var/log`. Ibarat **rekaman CCTV** — kalau ada masalah atau insiden keamanan, log adalah tempat pertama yang harus dicek.

---

### Conclusions & Summaries

Dengan selesainya room ini, kita sudah punya pondasi yang kuat buat mengoperasikan Linux secara profesional. Langkah selanjutnya adalah mendalami teknik-teknik keamanan dan administrasi yang lebih kompleks.

> **Note:** Automate everything. Jangan habiskan waktu buat tugas manual kalau kamu bisa tulis satu baris perintah di crontab.
