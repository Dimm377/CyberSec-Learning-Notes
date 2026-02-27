# TryHackMe: Linux Fundamentals Part 1


---

- **Room Link:** [Linux Fundamentals Part 1](https://tryhackme.com/room/linuxfundamentalspart1)
- **Category:** Linux / Fundamental
- **Difficulty:** Easy

---

## # Overview

Room ini memberi pengenalan dasar tentang sistem operasi Linux, mulai dari sejarah singkat, navigasi terminal, sampai penggunaan operator shell. Penting banget buat siapa pun yang mau berkecimpung di dunia keamanan siber, soalnya hampir semua alat pentesting jalan di atas kernel Linux.

---

### A Bit of Background on Linux

Linux dikembangin sama Linus Torvalds di tahun 1991. Sistem ini bersifat open-source dan punya banyak distribusi (distros) seperti Ubuntu, Debian, Fedora, dan favorit para "sepuh", Arch Linux.

---

### Running Your First Commands

Langkah pertama pakai Linux itu mengerti perintah dasar terminal:

- **`echo`**: Dipake buat nge-print teks ke terminal.
- **`whoami`**: Dipake buat ngeliat username dari akun yang lagi login.

---

### Interacting With the Filesystem

Navigasi di Linux dilakuin lewat terminal dengan memanipulasi direktori dan file:

- **`ls`**: Melihat daftar file dan folder di direktori sekarang.
- **`cd`**: Pindah antar direktori.
- **`cat`**: Baca dan tampilin isi file teks tanpa harus buka di editor.
- **`pwd`**: Cetak jalur lengkap direktori tempat kita lagi berada.

---

### Searching for Files

Mencari data secara efisien itu skill kunci buat pentester:

- **`find`**: Mencari file atau direktori berdasarkan nama, tipe, atau parameter lainnya.
- **`grep`**: Mencari pola teks tertentu di dalam file. Berguna banget buat nyaring log atau data yang gede.

---

### An Introduction to Shell Operators

Operator membuat kita bisa ngarahin output atau nggabungin beberapa perintah sekaligus:

- **`&`**: Jalanin perintah di latar belakang (_background_).
- **`&&`**: Gabungin dua perintah; perintah kedua cuma jalan kalau perintah pertama berhasil.
- **`>`**: Arahin output ke file dan **ganti** isi file itu (overwrite).
- **`>>`**: Arahin output ke file dan **tambahin** teks di bagian akhir tanpa hapus isi lama (append).

---

### Conclusions & Summaries

Menguasai dasar-dasar ini itu syarat mutlak sebelum belajar administrasi sistem atau keamanan tingkat lanjut. Perintah-perintah simpel inilah yang nantinya bakal dipake buat nge-exploit sistem atau lakuin _privilege escalation_.

> **Note:** Terminal bukan cuma cara berinteraksi sama komputer -- terminal itu cara tercepat dan paling efisien buat ngendaliin sistem secara total.
