# TryHackMe: Linux Fundamentals Part 2


---

* **Room Link:** [Linux Fundamentals Part 2](https://tryhackme.com/room/linuxfundamentalspart2)
* **Category:** Linux / Fundamental
* **Difficulty:** Easy

---

## Overview

Di bagian kedua ini, fokusnya pindah dari perintah navigasi dasar ke **manajemen sistem** yang lebih lanjut. Materinya mencakup akses jarak jauh (SSH), penggunaan parameter perintah (Flags/Switches), manipulasi file dan direktori, serta pemahaman mendalam tentang sistem perizinan (Permissions) di Linux.

---

### Accessing Your Linux Machine Using SSH

**SSH (Secure Shell)** itu protokol standar buat mengakses dan mengelola mesin Linux dari jarak jauh lewat jalur yang terenkripsi.

Analogi: Kalau Telnet itu seperti berkomunikasi lewat **jalan terbuka** (siapa saja bisa menyadap), SSH itu berkomunikasi lewat **terowongan bawah tanah yang terkunci** — hanya kamu dan server yang bisa dengar.

```bash
ssh username@10.10.10.10
```

| Aspek | Telnet | SSH |
| ----- | ------ | --- |
| **Enkripsi** | Tidak ada (plain text) | Seluruh traffic terenkripsi |
| **Keamanan** | Rentan penyadapan | Aman dari Man in the Middle Attack (MITM) |
| **Penggunaan** | Cek port terbuka | Remote access server |

---

### Introduction to Flags and Switches

Perintah Linux jarang dipakai sendirian. Sebagian besar perintah punya **Flags** atau **Switches** buat memodifikasi perilakunya — seperti **tombol pengaturan** di remote AC (bisa di dinginkan, bisa di hangatkan, bisa diatur timer).

| Tool/Flag | Fungsi |
| --------- | ------ |
| `man <command>` | Membuka halaman manual — sumber info lengkap semua opsi yang tersedia |
| `--help` | Menampilkan ringkasan penggunaan perintah |
| `-a` (All) | Di `ls` → menampilkan file tersembunyi (dotfiles yang diawali `.`) |
| `-l` (Long) | Di `ls` → menampilkan detail: izin, pemilik, ukuran, dan tanggal |

---

### Filesystem Management

Manajemen file itu kemampuan dasar buat mengorganisir data di dalam sistem. Ibarat **penataan barang di rumah** — kamu harus bisa membuat lemari baru, memindahkan barang, menyalin dokumen, dan membuang yang tidak perlu.

| Command | Fungsi | Catatan Penting |
| ------- | ------ | --------------- |
| `mkdir` | Membuat direktori baru | `mkdir folder_baru` |
| `touch` | Membuat file kosong / update timestamp | `touch catatan.txt` |
| `cp` | Menyalin file atau direktori | Pakai `-r` buat salin direktori secara rekursif |
| `mv` | Memindahkan file / mengganti nama | `mv lama.txt baru.txt` |
| `rm` | Menghapus file | Pakai `-r` buat hapus direktori + isinya |

> **Hati-hati:** Di Linux, file yang dihapus lewat terminal **tidak masuk ke Trash** — langsung hilang permanen

---

### Permissions 101

Linux itu sistem **multi-user**, jadi hak akses itu krusial buat keamanan, Setiap file punya **3 jenis kunci pintu**, dan setiap kunci bisa dipegang oleh 3 kelompok orang berbeda.

**Tiga kelompok pemegang kunci:**

| Kelompok | Siapa |
| -------- | ----- |
| **Owner** | User yang membuat file |
| **Group** | Sekelompok user yang punya hak akses sama |
| **Others** | Semua user lain di sistem |

**Tiga jenis izin:**

| Izin | Simbol | Fungsi | Nilai Numerik |
| ---- | :----: | ------ | :-----------: |
| **Read** | `r` | Melihat isi file / daftar file di direktori | `4` |
| **Write** | `w` | Memodifikasi file / menambah-hapus file di direktori | `2` |
| **Execute** | `x` | Menjalankan file sebagai program / masuk ke direktori | `1` |

**Mengubah izin dengan `chmod`:**

| Format | Contoh | Artinya |
| ------ | ------ | ------- |
| **Simbolik** | `chmod u+x script.sh` | Tambahkan izin execute untuk owner |
| **Numerik** | `chmod 755 script.sh` | Owner: rwx (7), Group: r-x (5), Others: r-x (5) |
| **Numerik** | `chmod 644 file.txt` | Owner: rw- (6), Group: r-- (4), Others: r-- (4) |

---

### Summary

Mengerti SSH dan Permissions itu **pondasi utama** sebelum lanjut ke materi yang lebih kompleks seperti _Privilege Escalation_ atau _Server Configuration_.

> **Note:** Di Linux, **"Everything is a file"**. Kalau kamu sudah menguasai cara mengelola file dan izinnya, kamu menguasai sistemnya.
