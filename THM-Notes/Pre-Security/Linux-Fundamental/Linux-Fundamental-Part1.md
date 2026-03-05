# TryHackMe: Linux Fundamentals Part 1


---

- **Room Link:** [Linux Fundamentals Part 1](https://tryhackme.com/room/linuxfundamentalspart1)
- **Category:** Linux / Fundamental
- **Difficulty:** Easy

---

## Overview

Room ini memberi pengenalan dasar tentang sistem operasi Linux, mulai dari sejarah singkat, navigasi terminal, sampai penggunaan operator shell. Penting buat siapa pun yang mau masuk ke dunia cybersecurity — soalnya hampir semua tool pentesting berjalan di atas kernel Linux.

---

### A Bit of Background on Linux

Linux dikembangkan oleh **Linus Torvalds** di tahun 1991. Sistem ini bersifat **open-source** dan punya banyak distribusi atau yang umum disebut **distro**, yang masing-masing dirancang buat kebutuhan berbeda:

| Distro | Fokus Utama |
| ------ | ----------- |
| **Ubuntu** | User-friendly, cocok buat pemula |
| **Debian** | Stabilitas tinggi, banyak dipakai di server |
| **Kali Linux** | Penetration testing dan cybersecurity |
| **Arch Linux** | Kontrol penuh, kustomisasi maksimal |

---

### Running Your First Commands

Langkah pertama pakai Linux: berkenalan dengan terminal. Bayangkan terminal itu **remote control** buat komputermu — semua bisa dikontrol dari sini.

| Command | Fungsi | Contoh |
| ------- | ------ | ------ |
| `echo` | Mencetak teks ke terminal | `echo "Hello World"` |
| `whoami` | Melihat username yang sedang login | `whoami` → `dimm` |

---

### Interacting With the Filesystem

Navigasi di Linux dilakukan lewat terminal. Bayangkan filesystem itu sebuah **gedung bertingkat** — kamu butuh perintah buat berpindah antar ruangan dan melihat isinya.

| Command | Analogi | Fungsi |
| ------- | ------- | ------ |
| `ls` | Lihat semua barang di ruangan ini | Menampilkan daftar file dan folder di direktori saat ini |
| `cd` | Pindah ke ruangan lain | Berpindah antar direktori |
| `cat` | Baca isi dokumen tanpa membuka amplop | Menampilkan isi file teks langsung di terminal |
| `pwd` | Cek kamu lagi di lantai/ruangan mana | Mencetak lokasi direktori saat ini secara lengkap |

---

### Searching for Files

Mencari data secara efisien itu skill kunci buat pentester. Dua tool utamanya:

| Command | Analogi | Fungsi |
| ------- | ------- | ------ |
| `find` | **Metal detector** — menyisir area luas cari benda spesifik | Mencari file/direktori berdasarkan nama, tipe, ukuran, dll |
| `grep` | **Highlighter** — mencari kata kunci di tumpukan kertas | Mencari pola teks tertentu di dalam file — sangat berguna buat memfilter log besar |

---

### An Introduction to Shell Operators

Operator bikin kamu bisa mengarahkan output atau menggabungkan beberapa perintah sekaligus — ibarat **sambungan pipa** yang menghubungkan aliran air ke arah yang diinginkan.

| Operator | Fungsi | Contoh |
| -------- | ------ | ------ |
| `&` | Jalankan perintah di latar belakang (_background_) | `nmap 10.10.10.10 &` |
| `&&` | Jalankan perintah kedua **hanya jika** perintah pertama berhasil | `mkdir test && cd test` |
| `>` | Arahkan output ke file — **overwrite** (timpa isi lama) | `echo "data" > file.txt` |
| `>>` | Arahkan output ke file — **append** (tambahkan di akhir) | `echo "more" >> file.txt` |

---

### Conclusions & Summaries

Menguasai dasar-dasar ini itu **syarat wajib** sebelum belajar sysadmin (administrasi sistem) atau keamanan tingkat lanjut. Perintah-perintah simpel inilah yang nantinya bakal dipakai buat nge-exploit sistem atau melakukan _privilege escalation_.


