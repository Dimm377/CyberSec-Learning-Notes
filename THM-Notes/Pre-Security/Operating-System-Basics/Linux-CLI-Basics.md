# TryHackMe: Linux CLI Basics

- **Room Link:** [Linux CLI Basics](https://tryhackme.com/room/introtoshells)
- **Category:** Pre-Security / Operating System Basics
- **Difficulty:** Easy

_(Prerequisite: [Operating Systems Introduction](Operating-Systems-Introduction.md) dan [Windows Basics](Windows-Basics.md))_

---

## Introduction

Di dunia cyber security, **Linux** ada di mana-mana — server, security tools, bahkan environment untuk hacking semuanya dominan menggunakan Linux. Sebelum bisa menjalankan tool apapun atau menyelidiki insiden, kamu harus bisa menavigasi sistem Linux lewat **CLI** (_Command-Line Interface_), yaitu antarmuka berbasis teks di mana kamu mengetik perintah langsung ke sistem.

### Scenario

Kamu adalah IT Support Engineer baru di tim **Cyber Operations Support**. Supervisor-mu pergi terburu-buru menangani insiden dan meninggalkan catatan di meja, isinya memintamu membuka terminal Linux dan mulai mengeksplorasi sistem sendiri. Setiap task di room ini memandu langkah demi langkah dari nol.

Setelah menyelesaikan room ini, kamu akan paham:

- Apa itu terminal Linux dan kenapa profesional cyber security menggunakannya.
- Cara berinteraksi dengan Linux lewat command line.
- Navigasi filesystem Linux menggunakan perintah-perintah dasar.

### A Quick Note About the Terminal

**Terminal** adalah antarmuka berbasis teks untuk mengontrol sistem Linux. Alih-alih mengklik ikon dan menu di GUI, kamu mengetik perintah yang langsung memberi instruksi ke komputer.

Kenapa profesional cyber security lebih memilih terminal dibanding GUI?

- **Lebih cepat** — tidak perlu klik-klik menu bertingkat.
- **Kontrol lebih granular** — bisa mengatur sistem sampai ke detail terkecil.
- **Banyak security tools yang _hanya_ berjalan di terminal** — tools seperti Nmap, Metasploit, dan Gobuster tidak punya versi GUI.

Kalau terminal terasa asing sekarang, itu wajar. Tujuan room ini memang membuatmu nyaman dengan lingkungan ini dari awal.

---

## Interacting With the Terminal and find the `mission_brief.txt`

Supervisor-mu menyebutkan bahwa ada dokumen bernama `mission_brief.txt` di suatu tempat dalam sistem. Misi pertamamu: temukan dan baca file itu. Task ini memperkenalkan perintah navigasi dasar yang setiap pengguna Linux pelajari di hari pertama.

### Step 1: "Where Am I?" — `pwd`

Saat pertama kali membuka terminal, kamu mungkin tidak tahu sedang berada di folder mana. Perintah `pwd` menjawab pertanyaan ini.

```bash
ubuntu@tryhackme:~$ pwd
/home/ubuntu
```

> **for your information:** **`pwd`** (_Print Working Directory_) menampilkan _absolute path_ dari folder tempat kamu berada saat ini. Ini seperti mengecek lokasi kamu di peta sebelum mulai bergerak.

### Step 2: "What's Around Me?" — `ls`

Setelah tahu lokasi, langkah berikutnya adalah melihat isi folder saat ini.

```bash
ubuntu@tryhackme:~$ ls
Desktop    Downloads  Pictures  Templates  logs
Documents  Music      Public    Videos     projects
```

Perintah `ls` menampilkan daftar file dan folder di direktori aktif. Tapi output default-nya hanya menampilkan nama, tanpa detail lain.

**Menampilkan Detail Lengkap**

Tambahkan flag `-l` untuk melihat informasi lengkap:

```bash
ubuntu@tryhackme:~$ ls -l
```

| Komponen | Fungsi |
| :--- | :--- |
| `ls` | Menampilkan isi direktori aktif |
| `-l` | Format _long listing_ — menampilkan permission, owner, ukuran, dan tanggal modifikasi |

Output `ls -l` menampilkan informasi penting tentang setiap file: permission, jumlah link, pemilik, grup, ukuran dalam byte, tanggal terakhir dimodifikasi, dan nama file/folder.

**Hidden Files**

Linux punya konsep **hidden files** — file yang namanya diawali dengan tanda titik (`.`). File-file ini tidak muncul di output `ls` biasa. Untuk melihat semuanya termasuk hidden files, gunakan flag `-a`:

```bash
ubuntu@tryhackme:~$ ls -al
```

| Komponen | Fungsi |
| :--- | :--- |
| `ls` | Menampilkan isi direktori |
| `-a` | Menampilkan _semua_ file termasuk hidden files (diawali `.`) |
| `-l` | Format _long listing_ dengan detail lengkap |

> **for your information:** Hidden files di Linux bukan berarti rahasia atau terenkripsi. Mereka hanya diawali titik (`.`) — misalnya `.bashrc`, `.Xauthority`. Linux menyembunyikannya secara default supaya tidak mengganggu pandangan saat melihat isi folder, karena biasanya berisi konfigurasi sistem atau aplikasi.

### Step 3: Let's Move Around — `cd`

Untuk berpindah ke folder lain, gunakan `cd`:

```bash
ubuntu@tryhackme:~$ cd Documents/
ubuntu@tryhackme:~/Documents$ pwd
/home/ubuntu/Documents
```

| Komponen | Fungsi |
| :--- | :--- |
| `cd` | _Change Directory_ — berpindah ke folder yang ditentukan |
| `Documents/` | Nama folder tujuan (relatif terhadap posisi saat ini) |

Untuk kembali ke folder sebelumnya (satu level ke atas), gunakan `cd ..`:

```bash
ubuntu@tryhackme:~/Documents$ cd ..
ubuntu@tryhackme:~$ pwd
/home/ubuntu
```

> **for your information:** `..` merepresentasikan _parent directory_ (folder induk), sedangkan `.` merepresentasikan direktori saat ini. Dua simbol ini akan sering kamu temui di Linux.

### Step 4: Learn the Power of "Find" — `find`

Jika kamu tahu nama file tapi tidak tahu di mana letaknya, perintah `find` adalah jawabannya. Linux akan memeriksa setiap folder dari titik awal yang kamu tentukan.

```bash
ubuntu@tryhackme:~$ find ~ -name mission_brief.txt
/<REDACTED-PATH>/mission_brief.txt
```

| Komponen | Fungsi |
| :--- | :--- |
| `find` | Tool bawaan untuk mencari file dalam sistem |
| `~` | Titik awal pencarian — simbol `~` adalah shortcut untuk _home directory_ (`/home/ubuntu`) |
| `-name` | Filter berdasarkan nama file (case-sensitive) |
| `mission_brief.txt` | Nama file yang dicari |

Jika file ditemukan, `find` menampilkan **full path** ke file tersebut. Dari sini, kamu bisa langsung navigasi ke folder itu menggunakan `cd` dan mengonfirmasi keberadaan file dengan `ls`.

> **Common Mistake:** `find` dengan `-name` bersifat _case-sensitive_. Artinya `find ~ -name Mission_Brief.txt` tidak akan menemukan file bernama `mission_brief.txt`. Kalau tidak yakin dengan huruf besar/kecilnya, gunakan `-iname` sebagai gantinya (case-insensitive).

### Step 5: Read the File — `cat`

Setelah menemukan dan masuk ke folder yang tepat, gunakan `cat` untuk membaca isi file:

```bash
ubuntu@tryhackme:~/<REDACTED-PATH>$ cat mission_brief.txt
Great job finding your way around the terminal.

Your next assignment is to collect a small system report:
- Who you're logged in as
- The kernel version
- Total disk space
- The name of this Linux distribution
```

| Komponen | Fungsi |
| :--- | :--- |
| `cat` | _Concatenate_ — menampilkan seluruh isi file ke terminal |
| `mission_brief.txt` | File yang ingin dibaca |

> **for your information:** Nama `cat` berasal dari **concatenate** (menggabungkan), karena fungsi aslinya adalah menggabungkan beberapa file menjadi satu output. Tapi dalam penggunaan sehari-hari, `cat` paling sering dipakai untuk menampilkan isi satu file secara cepat.

## Investigating the System

Supervisor-mu ingin kamu mengumpulkan informasi dasar tentang sistem yang sedang kamu gunakan. Jenis investigasi ini penting dalam cyber security, memahami environment tempat kamu beroperasi (versi OS, kernel, resource yang tersedia) adalah langkah awal sebelum melakukan apa pun.

Tugas dari `mission_brief.txt` sudah jelas, kumpulkan empat informasi: siapa user yang sedang login, versi kernel, total kapasitas disk, dan nama distribusi Linux.

### Step 1: "Who Are You Logged in As?" — `whoami`

Perintah paling simpel sekaligus paling berguna di Linux:

```bash
ubuntu@tryhackme:~$ whoami
ubuntu
```

| Komponen | Fungsi |
| :--- | :--- |
| `whoami` | Menampilkan username yang sedang aktif di sesi terminal saat ini |

### Step 2: "What System Are You On?" — `uname`

Untuk melihat detail tentang OS, versi kernel, dan arsitektur mesin, gunakan `uname -a`:

```bash
ubuntu@tryhackme:~$ uname -a
Linux tryhackme <REDACTED>-aws #17-Ubuntu SMP Mon Sep 2 13:48:07 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

| Komponen | Fungsi |
| :--- | :--- |
| `uname` | _Unix Name_ — menampilkan informasi tentang sistem |
| `-a` | Menampilkan _semua_ informasi sekaligus (kernel, hostname, arsitektur, OS) |

**Breakdown Output:**

| Field | Nilai | Artinya |
| :--- | :--- | :--- |
| OS Kernel | `Linux` | Sistem berjalan di atas kernel Linux |
| Hostname | `tryhackme` | Nama komputer di jaringan |
| Kernel Version | `<REDACTED>-aws` | Versi kernel yang terinstal |
| Architecture | `x86_64` | Platform hardware 64-bit |
| OS Type | `GNU/Linux` | Kernel Linux + tools GNU |

Kalau kamu hanya butuh nama OS tanpa detail lain, cukup jalankan `uname` tanpa flag:

```bash
ubuntu@tryhackme:~$ uname
Linux
```

### Step 3: Check Disk and Storage Info — `df`

Di lingkungan kerja nyata, kamu sering perlu mengecek kapasitas disk, misalnya sebelum menjalankan tools yang menghasilkan log besar, atau saat menyelidiki kenapa suatu service gagal (bisa jadi disk penuh). Perintah `df -h` menampilkan informasi ini dalam format yang mudah dibaca:

```bash
ubuntu@tryhackme:~$ df -h
Filesystem      Size  Used  Avail Use% Mounted on
/dev/root        ~G   12G    ~G   17%  /
tmpfs           1.9G    0  1.9G    0%  /dev/shm
tmpfs           774M  1.2M  773M   1%  /run
...
```

| Komponen | Fungsi |
| :--- | :--- |
| `df` | _Disk Free_ — menampilkan penggunaan disk setiap filesystem |
| `-h` | _Human readable_ — menampilkan ukuran dalam format yang mudah dibaca (G, M) alih-alih byte mentah |

**Breakdown Output:**

| Filesystem | Keterangan |
| :--- | :--- |
| `/dev/root` | Disk utama sistem — tempat OS dan semua data kamu berada. Kolom `Use%` menunjukkan seberapa penuh disk ini. |
| `tmpfs` | Filesystem sementara yang tersimpan di **RAM**, bukan di disk fisik. Hilang saat sistem di-reboot. |
| `/dev/shm` | Area _shared memory_ — digunakan untuk komunikasi antar proses. |

### Step 4: Read a System File — `/etc`

Linux menyimpan file konfigurasi dan informasi sistem di direktori **`/etc`**. Direktori ini adalah "pusat konfigurasi" sistem — hampir semua pengaturan penting ada di sini.

```bash
ubuntu@tryhackme:~$ cd /etc
ubuntu@tryhackme:/etc$ ls
```

> **for your information:** **`/etc`** adalah singkatan historis dari _"et cetera"_, tapi dalam Linux modern, direktori ini secara spesifik menyimpan **file konfigurasi sistem**. File-file seperti `hostname`, `passwd`, `fstab`, dan `os-release` semuanya ada di sini.

Untuk mengetahui distribusi Linux yang digunakan, baca file `os-release`:

```bash
ubuntu@tryhackme:/etc$ cat os-release
PRETTY_NAME="Ubuntu 24.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.1 LTS (Noble Numbat)"
...
```

File ini memberikan informasi yang lebih jelas dan terstruktur tentang distribusi Linux dibanding output `uname`. Informasi seperti nama distribusi, versi, dan codename semuanya tersedia di sini.

### Mini Challenge

Sebelum hari pertama berakhir, supervisor meninggalkan satu tantangan terakhir: temukan file bernama `day1_report.txt` yang ada di suatu tempat dalam home directory. Tantangan ini tidak ada petunjuk langkah demi langkah — kamu sudah punya semua tools yang dibutuhkan:

1. Gunakan `find` untuk menemukan lokasi file.
2. Navigasi ke folder tempat file berada dengan `cd`.
3. Baca isinya menggunakan `cat`.

---

## Conclusion

Room ini mengajarkan skill navigasi Linux yang menjadi pondasi untuk semua hal yang akan datang setelahnya, file permissions, manajemen user dan grup, proses, package management, dan akhirnya penggunaan security tools sungguhan.

### Key Commands Summary

| Command | Fungsi |
| :--- | :--- |
| `pwd` | Menampilkan direktori aktif saat ini |
| `ls` | Melihat isi direktori (tambah `-l` untuk detail, `-a` untuk hidden files) |
| `cd` | Berpindah direktori (`cd ..` untuk naik satu level) |
| `find` | Mencari file berdasarkan nama di seluruh hierarki folder |
| `cat` | Membaca dan menampilkan isi file |
| `whoami` | Menampilkan username yang sedang login |
| `uname -a` | Menampilkan informasi lengkap tentang sistem (kernel, arsitektur, hostname) |
| `df -h` | Menampilkan penggunaan disk dalam format human-readable |

---

## Review

- **Terminal** adalah antarmuka utama untuk bekerja di Linux — lebih cepat, lebih powerful, dan banyak security tools yang hanya tersedia di sini.
- Lima perintah navigasi dasar (`pwd`, `ls`, `cd`, `find`, `cat`) sudah cukup untuk menjelajahi filesystem, menemukan file, dan membaca isinya.
- Perintah investigasi sistem (`whoami`, `uname -a`, `df -h`, `cat /etc/os-release`) memberikan gambaran lengkap tentang environment: siapa yang login, kernel apa yang jalan, berapa kapasitas disk, dan distribusi Linux apa yang digunakan.
- Direktori `/etc` adalah pusat konfigurasi sistem Linux — memahami isinya penting untuk administrasi dan security.
