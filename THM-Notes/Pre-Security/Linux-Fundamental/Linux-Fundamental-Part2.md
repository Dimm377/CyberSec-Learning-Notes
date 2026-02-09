# TryHackMe: Linux Fundamentals Part 2


---

**Room Link:** [Linux Fundamentals Part 2](https://tryhackme.com/room/linuxfundamentalspart2)
**Category:** Linux / Fundamental
**Difficulty:** Easy

---

---

## # Overview

Pada bagian kedua ini, fokus beralih dari perintah navigasi dasar ke manajemen sistem yang lebih lanjut. Materi mencakup akses jarak jauh (SSH), penggunaan parameter perintah (Flags/Switches), manipulasi file dan direktori, serta pemahaman mendalam tentang sistem perizinan (Permissions) di Linux.

---

## ### Task 2: Accessing Your Linux Machine Using SSH

SSH (Secure Shell) adalah protokol standar untuk mengakses dan mengelola mesin Linux secara jarak jauh melalui saluran yang terenkripsi.

- **Koneksi:** Menggunakan perintah `ssh [username]@[IP_Address]`.
- **Keamanan:** Berbeda dengan Telnet, SSH mengenkripsi seluruh trafik, termasuk kredensial login, sehingga aman dari penyadapan di jaringan.
- **Hands-on:** Praktik dilakukan dengan menghubungkan terminal lokal ke mesin target menggunakan kredensial `tryhackme`.

---

## ### Task 3: Introduction to Flags and Switches

Perintah Linux jarang digunakan sendirian. Sebagian besar perintah memiliki "Flags" atau "Switches" untuk memodifikasi perilakunya.

- **Manual Pages:** Perintah `man` adalah sumber informasi utama untuk melihat semua opsi yang tersedia pada suatu perintah (contoh: `man ls`).
- **Common Flags:**
  - `--help`: Menampilkan ringkasan penggunaan perintah.
  - `-a` (All): Digunakan pada `ls` untuk melihat file tersembunyi (dotfiles).
  - `-l` (Long): Menampilkan detail file seperti izin, pemilik, dan ukuran.

---

## ### Task 4: Filesystem Management

Manajemen file adalah kemampuan dasar untuk mengorganisir data di dalam sistem.

- **mkdir:** Membuat direktori baru.
- **touch:** Membuat file kosong atau memperbarui timestamp.
- **cp (Copy):** Menyalin file atau direktori. Gunakan `-r` untuk menyalin direktori secara rekursif.
- **mv (Move):** Memindahkan file atau mengganti nama file/direktori.
- **rm (Remove):** Menghapus file. Gunakan `-r` untuk menghapus direktori beserta isinya. _Hati-hati: Di Linux, file yang dihapus lewat terminal tidak masuk ke Trash._

---

## ### Task 5: Permissions 101

Linux adalah sistem multi-user, sehingga hak akses sangat krusial untuk keamanan.

- **User Types:** - **Owner:** User yang membuat file.
  - **Group:** Sekelompok user yang memiliki hak akses sama.
  - **Others:** Semua user lain di sistem.
- **Permission Types:**
  - **Read (r):** Izin melihat isi file atau daftar file dalam direktori.
  - **Write (w):** Izin memodifikasi file atau menambah/menghapus file di direktori.
  - **Execute (x):** Izin menjalankan file sebagai program atau masuk ke dalam direktori.
- **chmod:** Perintah untuk mengubah izin file, baik menggunakan mode simbolik (u+x, g-w) atau numerik (755, 644).

---

## ### Task 6: Summary

Bagian ini merangkum pentingnya penguasaan terminal untuk efisiensi kerja di Linux. Pemahaman tentang SSH dan Permissions adalah pondasi utama sebelum melangkah ke materi yang lebih kompleks seperti _Privilege Escalation_ atau _Server Configuration_.

> **Note:** Di Linux, "Everything is a file". Jika kamu menguasai cara mengelola file dan izinnya, kamu menguasai sistem tersebut.
