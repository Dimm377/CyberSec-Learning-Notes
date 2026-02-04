# TryHackMe: Linux Fundamentals Part 3

**Room Link:** [Linux Fundamentals Part 3](https://tryhackme.com/room/linuxfundamentalspart3)
**Category:** Linux / Fundamental
**Difficulty:** Easy

---

# Overview

Room ini fokus pada alat-alat utility yang akan sering digunakan sehari-hari, cara mengelola proses sistem, hingga mengotomatisasi tugas agar sistem tetap berjalan optimal.

---

### Terminal Text Editors

Mengedit file langsung lewat terminal adalah skill wajib. Room ini memperkenalkan dua editor utama:

- **Nano:** Editor yang sangat ramah pemula dengan panduan tombol di bagian bawah.
- **Vim/Vi:** Editor yang jauh lebih sakti dengan mode perintah yang berbeda. _Note: Karena saya pengguna LazyVim di Arch, bagian ini terasa sangat familiar._

---

### General/Useful Utilities

Linux menyediakan banyak alat kecil yang sangat berguna untuk transfer data dan komunikasi:

- **`wget`**: Mengunduh file langsung dari web server melalui terminal.
- **`scp` (Secure Copy)**: Menyalin file antar mesin secara aman melalui protokol SSH.
- **Serving Files**: Kita bisa menggunakan modul Python (`python3 -m http.server`) untuk membuat web server instan guna berbagi file dalam jaringan lokal.

---

### Processes 101

Belajar cara melihat dan mengendalikan aplikasi yang sedang berjalan di latar belakang:

- **`ps aux`**: Melihat daftar semua proses yang berjalan secara detail.
- **`top`**: Memonitor penggunaan sumber daya sistem (CPU & RAM) secara real-time.
- **`kill`**: Mengirimkan sinyal ke proses berdasarkan PID (Process ID).
  - **SIGTERM (15)**: Meminta proses berhenti secara sopan (cleanup).
  - **SIGKILL (9)**: Menghentikan proses secara paksa.
  - **SIGSTOP (19)**: Memberhentikan/menangguhkan proses sementara.

---

### Maintaining Your System: Automation

Agar tidak perlu mengerjakan tugas yang sama berulang kali, Linux menggunakan **cron**.

- **`crontab -e`**: Mengedit file konfigurasi cron untuk mengatur jadwal eksekusi skrip atau perintah secara otomatis pada waktu tertentu.

---

### Maintaining Your System: Package Management

Mengatur perangkat lunak pada distribusi berbasis Debian/Ubuntu:

- **`apt`**: Alat utama untuk menginstal, memperbarui, dan menghapus aplikasi.
- **Repositories**: Memahami bahwa Linux menarik aplikasi dari gudang data (repo) terpusat, berbeda dengan Windows yang biasanya menggunakan file installer `.exe`.

---

### Maintaining Your System: Logs

Sistem Linux mencatat segala aktivitas di dalam file log yang biasanya berada di direktori `/var/log`. Memeriksa log adalah langkah krusial saat melakukan troubleshooting atau investigasi keamanan.

---

### Conclusions & Summaries

Dengan selesainya room ini, kita sudah memiliki pondasi yang kuat untuk mengoperasikan Linux secara profesional. Perjalanan selanjutnya adalah mendalami teknik-teknik keamanan dan administrasi yang lebih kompleks.

> **Note:** Automate everything. Jangan habiskan waktu untuk tugas manual jika kamu bisa menuliskan satu baris perintah di crontab.
