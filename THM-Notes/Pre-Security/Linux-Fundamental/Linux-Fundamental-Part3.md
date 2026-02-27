# TryHackMe: Linux Fundamentals Part 3


---

- **Room Link:** [Linux Fundamentals Part 3](https://tryhackme.com/room/linuxfundamentalspart3)
- **Category:** Linux / Fundamental
- **Difficulty:** Easy

---

---

# Overview

Room ini fokus ke alat-alat utility yang bakal sering dipake sehari-hari, cara ngelola proses sistem, sampai mengotomatisasi tugas agar sistem tetap jalan optimal.

---

### Terminal Text Editors

Edit file langsung lewat terminal itu skill wajib. Room ini ngenalin dua editor utama:

- **Nano:** Editor yang ramah banget buat pemula, ada panduan tombol di bagian bawah.
- **Vim/Vi:** Editor yang jauh lebih sakti dengan mode perintah yang beda-beda. _Note: Karena aku pengguna LazyVim di Arch, bagian ini terasa sangat familiar._

---

### General/Useful Utilities

Linux menyediakan banyak alat kecil yang berguna banget buat transfer data dan komunikasi:

- **`wget`**: Download file langsung dari web server lewat terminal.
- **`scp` (Secure Copy)**: Salin file antar mesin secara aman lewat protokol SSH.
- **Serving Files**: Kita bisa pakai modul Python (`python3 -m http.server`) buat membuat web server instan dan berbagi file di jaringan lokal.

---

### Processes 101

Belajar cara melihat dan ngendaliin aplikasi yang lagi jalan di latar belakang:

- **`ps aux`**: Melihat daftar semua proses yang lagi jalan secara detail.
- **`top`**: Monitor penggunaan sumber daya sistem (CPU & RAM) secara real-time.
- **`kill`**: Kirim sinyal ke proses berdasarkan PID (Process ID).
  - **SIGTERM (15)**: Minta proses berhenti secara sopan (cleanup).
  - **SIGKILL (9)**: Hentiin proses secara paksa.
  - **SIGSTOP (19)**: Tangguhkan proses sementara.

---

### Maintaining Your System: Automation

Agar tidak perlu ngerjain tugas yang sama berulang-ulang, Linux pakai **cron**.

- **`crontab -e`**: Edit file konfigurasi cron buat ngatur jadwal eksekusi skrip atau perintah secara otomatis di waktu tertentu.

---

### Maintaining Your System: Package Management

Ngatur perangkat lunak di distribusi berbasis Debian/Ubuntu:

- **`apt`**: Alat utama buat install, update, dan hapus aplikasi.
- **Repositories**: Pahami bahwa Linux narik aplikasi dari gudang data (repo) terpusat, beda sama Windows yang biasanya pakai file installer `.exe`.

---

### Maintaining Your System: Logs

Sistem Linux nyatet semua aktivitas di dalam file log yang biasanya ada di direktori `/var/log`. Mengecek log itu langkah penting waktu troubleshooting atau investigasi keamanan.

---

### Conclusions & Summaries

Dengan selesainya room ini, kita sudah punya pondasi yang kuat buat ngoperasiin Linux secara profesional. Langkah selanjutnya adalah mendalami teknik-teknik keamanan dan administrasi yang lebih kompleks.

> **Note:** Automate everything. Jangan habisin waktu buat tugas manual kalau kamu bisa tulis satu baris perintah di crontab.
