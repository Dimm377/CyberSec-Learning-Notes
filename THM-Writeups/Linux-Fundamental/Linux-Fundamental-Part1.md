# TryHackMe: Linux Fundamentals Part 1

**Room Link:** [Linux Fundamentals Part 1](https://tryhackme.com/room/linuxfundamentalspart1)
**Category:** Linux / Fundamental
**Difficulty:** Easy

---

## # Overview

Room ini memberikan pengenalan dasar tentang sistem operasi Linux, mulai dari sejarah singkat, navigasi terminal, hingga penggunaan operator shell. Sangat penting bagi siapa pun yang ingin berkecimpung di dunia keamanan siber karena hampir semua alat penetrasi berjalan di atas kernel Linux.

---

## A Bit of Background on Linux

Linux dikembangkan oleh Linus Torvalds pada tahun 1991. Sistem ini bersifat open-source dan memiliki banyak distribusi (distros) seperti Ubuntu, Debian, Fedora, dan favorit para "sepuh", Arch Linux.

---

## ### Task 4: Running Your First Commands

Langkah pertama dalam menggunakan Linux adalah memahami perintah dasar terminal:

- **`echo`**: Digunakan untuk mencetak teks ke terminal.
- **`whoami`**: Digunakan untuk mengetahui username dari akun yang sedang login saat ini.

---

## ### Task 5: Interacting With the Filesystem

Navigasi di Linux dilakukan melalui terminal dengan memanipulasi direktori dan file:

- **`ls`**: Melihat daftar file dan folder di direktori saat ini.
- **`cd`**: Berpindah antar direktori.
- **`cat`**: Membaca dan menampilkan isi dari sebuah file teks tanpa membukanya di editor.
- **`pwd`**: Mencetak jalur lengkap direktori tempat kita berada sekarang.

---

## ### Task 6: Searching for Files

Mencari data secara efisien adalah skill kunci bagi seorang pentester:

- **`find`**: Mencari file atau direktori berdasarkan nama, tipe, atau parameter lainnya.
- **`grep`**: Mencari pola teks tertentu di dalam sebuah file. Sangat berguna untuk menyaring log atau data yang besar.

---

## ### Task 7: An Introduction to Shell Operators

Operator memungkinkan kita untuk mengarahkan output atau menggabungkan beberapa perintah sekaligus:

- **`&`**: Menjalankan perintah di latar belakang (_background_).
- **`&&`**: Menggabungkan dua perintah; perintah kedua hanya berjalan jika perintah pertama berhasil.
- **`>`**: Mengarahkan output ke file dan **mengganti** isi file tersebut (overwrite).
- **`>>`**: Mengarahkan output ke file dan **menambahkan** teks di bagian akhir tanpa menghapus isi lama (append).

---

## ### Task 8: Conclusions & Summaries

Menguasai dasar-dasar ini adalah syarat mutlak sebelum mempelajari administrasi sistem atau keamanan tingkat lanjut. Perintah-perintah sederhana inilah yang nantinya akan digunakan untuk mengeksploitasi sistem atau melakukan _privilege escalation_.

> **Note:** Terminal bukan sekadar cara berinteraksi dengan komputer, terminal adalah cara tercepat dan paling efisien untuk mengendalikan sistem secara total.
