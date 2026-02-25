# TryHackMe: Linux Fundamentals Part 2


---

- **Room Link:** [Linux Fundamentals Part 2](https://tryhackme.com/room/linuxfundamentalspart2)
- **Category:** Linux / Fundamental
- **Difficulty:** Easy

---

---

## # Overview

Di bagian kedua ini, fokusnya pindah dari perintah navigasi dasar ke manajemen sistem yang lebih lanjut. Materinya mencakup akses jarak jauh (SSH), penggunaan parameter perintah (Flags/Switches), manipulasi file dan direktori, serta pemahaman mendalam tentang sistem perizinan (Permissions) di Linux.

---

## ### Task 2: Accessing Your Linux Machine Using SSH

SSH (Secure Shell) itu protokol standar buat ngakses dan ngelola mesin Linux dari jarak jauh lewat jalur yang terenkripsi.

- **Koneksi:** Pake perintah `ssh [username]@[IP_Address]`.
- **Keamanan:** Beda sama Telnet, SSH mengenkripsi seluruh trafik termasuk kredensial login, jadi aman dari penyadapan di jaringan.

---

## ### Task 3: Introduction to Flags and Switches

Perintah Linux jarang dipake sendirian. Sebagian besar perintah punya "Flags" atau "Switches" buat nge-modif perilakunya.

- **Manual Pages:** Perintah `man` itu sumber info utama buat ngeliat semua opsi yang tersedia di suatu perintah (contoh: `man ls`).
- **Common Flags:**
  - `--help`: Nampilin ringkasan penggunaan perintah.
  - `-a` (All): Dipake di `ls` buat ngeliat file tersembunyi (dotfiles).
  - `-l` (Long): Nampilin detail file kayak izin, pemilik, dan ukuran.

---

## ### Task 4: Filesystem Management

Manajemen file itu kemampuan dasar buat ngorganisir data di dalam sistem.

- **mkdir:** Bikin direktori baru.
- **touch:** Bikin file kosong atau update timestamp.
- **cp (Copy):** Salin file atau direktori. Pake `-r` buat salin direktori secara rekursif.
- **mv (Move):** Pindahin file atau ganti nama file/direktori.
- **rm (Remove):** Hapus file. Pake `-r` buat hapus direktori beserta isinya. _Hati-hati: Di Linux, file yang dihapus lewat terminal nggak masuk ke Trash._

---

## ### Task 5: Permissions 101

Linux itu sistem multi-user, jadi hak akses itu penting banget buat keamanan.

- **User Types:**
  - **Owner:** User yang bikin file.
  - **Group:** Sekelompok user yang punya hak akses sama.
  - **Others:** Semua user lain di sistem.
- **Permission Types:**
  - **Read (r):** Izin buat liat isi file atau daftar file di direktori.
  - **Write (w):** Izin buat modifikasi file atau nambahin/hapus file di direktori.
  - **Execute (x):** Izin buat jalanin file sebagai program atau masuk ke direktori.
- **chmod:** Perintah buat ngubah izin file, bisa pake mode simbolik (u+x, g-w) atau numerik (755, 644).

---

## ### Task 6: Summary

Bagian ini merangkum pentingnya nguasain terminal buat efisiensi kerja di Linux. Ngerti SSH dan Permissions itu pondasi utama sebelum lanjut ke materi yang lebih kompleks kayak _Privilege Escalation_ atau _Server Configuration_.

> **Note:** Di Linux, "Everything is a file". Kalau kamu udah nguasain cara ngelola file dan izinnya, kamu nguasain sistemnya.
