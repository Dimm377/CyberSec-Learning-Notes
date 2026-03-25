# TryHackMe: Windows CLI Basics

- **Room Link:** [Windows CLI Basics](https://tryhackme.com/room/windowsclibasics)
- **Category:** Pre-Security / Operating System Basics
- **Difficulty:** Easy

_(Prerequisite: [Windows Basics](Windows-Basics.md))_

## Introduction

Setelah menjelajahi _Command-Line Interface_ (CLI) di Linux, perjalanan berlanjut ke sistem operasi yang paling banyak digunakan di lingkungan perkantoran: **Windows**. Karena dominasi Windows di dunia nyata, sebagian besar serangan siber dan investigasi insiden melibatkan mesin Windows. Profesional cyber security harus bisa menavigasi dan melakukan troubleshooting pada sistem ini.

Di Windows, antarmuka teks utamanya adalah **Command Prompt** (sering disebut **CMD**).

### Scenario

Ini adalah hari kedua kamu sebagai IT Support Engineer yang magang di tim **Cyber Operations Support**. Supervisormu mengujimu kembali dengan meninggalkan catatan tertulis: "Temukan dan baca file bernama `task_brief.txt` yang tersembunyi di dalam _user folder_, dengan syarat hanya menggunakan Command Prompt."

Setelah menyelesaikan room ini, kamu akan paham:

- Cara menggunakan Command Prompt Windows dengan percaya diri.
- Navigasi direktori tanpa menggunakan mouse (File Explorer).
- Cara mencari, menemukan, dan membaca file hanya bermodalkan nama file.
- Cara mengumpulkan informasi dasar tentang sistem dan jaringan menggunakan command line.

### A Quick Recap About the Terminal

Sama seperti terminal Linux, Command Prompt adalah cara untuk memberi instruksi langsung ke sistem operasi tanpa melalui Graphical User Interface (GUI). Alasan para praktisi keamanan menggunakannya sama:

- Lebih cepat daripada harus banyak klik.
- Memberikan kontrol yang lebih presisi atas sistem.
- Banyak tools keamanan _offensive_ maupun jaringan yang hanya bisa dijalankan lewat CLI.

Buka _Command Prompt_ lewat shortcut di Desktop, dan mari mulai menyelesaikan misi `task_brief.txt`.

---

## Navigating the Windows Filesystem

Sebelum bisa menemukan file misi, kamu harus tahu cara navigasi di dalam struktur folder Windows menggunakan command line.

### Step 1: "Where Am I?" — `cd`

Langkah pertama saat membuka terminal baru adalah memeriksa di folder mana kamu sedang berada sekarang. Di Windows, kamu bisa mengetik perintah `cd` tanpa tambahan apa-apa:

```cmd
C:\Users\Administrator>cd
C:\Users\Administrator
```

| Komponen | Fungsi |
| :--- | :--- |
| `cd` | Menampilkan _full path_ dari direktori kerja saat ini (jika digunakan tanpa argumen) |

> **for your information:** Di Linux, perintah untuk fungsi ini adalah `pwd`. Di Windows, perintah `cd` punya dua fungsi sekaligus: menampilkan folder saat ini (jika berdiri sendiri), atau berpindah ke folder lain (jika diikuti nama folder tujuan).

### Step 2: "What's Around Me?" — `dir`

Setelah tahu kamu berada di folder mana, langkah selanjutnya adalah melihat daftar file dan folder yang ada di lokasi tersebut. Gunakan perintah `dir`:

```cmd
C:\Users\Administrator>dir
 Volume in drive C has no label.
 Volume Serial Number is A8A4-C362

 Directory of C:\Users\Administrator

01/04/2026  10:14 PM    <DIR>          .
01/04/2026  10:14 PM    <DIR>          ..
...
10/14/2022  10:06 AM    <DIR>          Desktop
10/14/2022  10:06 AM    <DIR>          Documents
...
               0 File(s)              0 bytes
              16 Dir(s)   3,022,721,024 bytes free
```

| Komponen | Fungsi |
| :--- | :--- |
| `dir` | Singkatan dari _directory_, menampilkan isi folder aktif beserta metadata dasarnya. |

Output dari `dir` memberikan beberapa informasi penting:
- Menampilkan tanggal dan waktu terakhir item tersebut dimodifikasi.
- Tag `<DIR>` menandakan bahwa item di kolom kanan adalah sebuah folder (direktori).
- Jika ada file, ukurannya (dalam byte) akan ditampilkan di tempat `<DIR>` berada.
- Di bagian paling bawah, ada ringkasan jumlah file, folder, dan sisa kapasitas penyimpanan di _drive_ C:.

---

### Step 3: Are There Hidden Files?

Sama seperti Linux, Windows memiliki konsep file dan folder tersembunyi. Tujuannya bukan untuk merahasiakan isinya, melainkan agar folder tidak terlihat berantakan karena file konfigurasi.

Gunakan flag `/a` (All) untuk melihat semuanya, termasuk yang tersembunyi:

```cmd
C:\Users\Administrator>dir /a
```

| Komponen | Fungsi |
| :--- | :--- |
| `dir` | Menampilkan isi direktori |
| `/a` | Menampilkan file _hidden_ dan sistem yang biasanya tidak terlihat |

### Step 4: Moving Around the Filesystem

Setelah mengidentifikasi folder mana saja yang ada, kamu bisa masuk ke dalamnya. Jika kamu melihat folder `Documents` di hasil `dir`, kamu bisa berpindah menggunakan `cd`:

```cmd
C:\Users\Administrator>cd Documents
C:\Users\Administrator\Documents>cd ..
C:\Users\Administrator>
```

| Komponen | Fungsi |
| :--- | :--- |
| `cd Documents` | Masuk ke folder bernama `Documents` |
| `cd ..` | Naik satu level (_parent directory_), sama seperti di Linux |

### Step 5: Finding the File on the Disk

Mencari file `task_brief.txt` secara manual dari satu direktori ke direktori lain sangat tidak efisien. Alih-alih menebak, manfaatkan kemampuan pencarian dari `dir`:

```cmd
C:\Users\Administrator>dir /s task_brief.txt
```

| Komponen | Fungsi |
| :--- | :--- |
| `dir` | Perintah dasar melihat isi direktori |
| `/s` | Flag _Subdirectories_ — mencari ke sub-folder tanpa batas |
| `task_brief.txt` | Nama file spesifik yang sedang dicari |

Jika file ditemukan, Command Prompt akan memberikan daftar *Directory of [full_path]* di mana file itu berada. Catat path lengkap tersebut.

### Step 6: Navigate to the File

Gunakan `cd <path_lengkap>` menuju folder tempat `task_brief.txt` ditemukan tadi, dan pastikan keberadaannya menggunakan `dir`.

### Step 7: Read the File — `type`

Setelah sampai di lokasi folder, misi utamanya adalah membaca instruksi di dalam file teks itu. Di Windows, perintah yang melakukan fungsi "baca dan tampilkan di layar" adalah `type`.

```cmd
C:\Users...\Notes...>type task_brief.txt
```

| Komponen | Fungsi |
| :--- | :--- |
| `type` | Mencetak isi file teks langsung ke dalam Command Prompt (mirip `cat` di Linux) |

> **for your information:** `type` sangat efektif untuk membaca file `.txt`, file script, dan konfigurasi ringan dengan cepat tanpa harus repot-repot membuka editor teks eksternal seperti Notepad.

Isi `task_brief.txt` ternyata berisi tantangan selanjutnya dari supervisor: mengenali sistem yang sedang kamu operasikan.

---

## Inquiring About the System

Supervisor ingin tahu _siapa_ kamu, _apa_ nama mesinnya, _versi_ Windows apa yang berjalan, dan _bagaimana_ mesin terhubung ke jaringan. Ini adalah sekumpulan _"pertanyaan awal"_ yang setiap analis IT akan tanyakan pada sistem baru.

### Step 1: Who Am I Logged In As? — `whoami`

Akun pengguna yang berbeda bisa memiliki _permission_ (hak akses) yang sangat jauh berbeda, contohnya seorang Administrator bisa menghapus sistem, sementara Standard User tidak. Mengetahui siapa kamu saat ini adalah langkah pertama dalam enumerasi.

```cmd
C:\Users\Administrator>whoami
<namamesin>\administrator
```

| Komponen | Fungsi |
| :--- | :--- |
| `whoami` | Menampilkan _username_ sesi yang sedang aktif |

### Step 2: What Is the Name of This Computer? — `hostname`

Dalam lingkungan perusahaan dengan ratusan mesin, mengetahui nama komputer ini sangat penting agar tidak salah identifikasi di jaringan.

```cmd
C:\Users\Administrator>hostname
<REDACTED_HOSTNAME>
```

| Komponen | Fungsi |
| :--- | :--- |
| `hostname` | Menampilkan nama lokal komputer di jaringan |

### Step 3: What Version of Windows Is This? — `systeminfo`

Mengetahui informasi OS sangat penting untuk menentukan apakah ada kerentanan (vulnerability) atau _patch_ yang hilang.

```cmd
C:\Users\Administrator>systeminfo
```

Perintah ini akan mencetak banyak data. Untuk penyelidikan awal, fokuskan pada tiga poin ini:
- **OS Name:** Apakah ini Windows 10, Windows 11, atau mungkin Windows Server 2019?
- **OS Version:** Nomor _build_ dan spesifikasi versinya.
- **System Type:** _Architecture_ sistemnya, apakah x64-based (64-bit) atau x86-based (32-bit).

| Komponen | Fungsi |
| :--- | :--- |
| `systeminfo` | Mengumpulkan dan menampilkan informasi _hardware_ dan _software_ secara mendetail dari OS Windows |

### Step 4: How Is This Machine Connected to the Network? — `ipconfig`

Terakhir, periksa konfigurasi jaringannya.

```cmd
C:\Users\Administrator>ipconfig
```

Bagian paling penting yang dicari seorang profesional keamanan atau IT saat melihat output `ipconfig` adalah:
1. **IPv4 Address:** Alamat IP pengenal untuk perangkat ini.
2. **Default Gateway:** Alamat router pengelola keluar-masuk lalu lintas — inilah jalan keluar menuju perangkat lain atau ke _internet_.

| Komponen | Fungsi |
| :--- | :--- |
| `ipconfig` | _Internet Protocol Configuration_ — melihat informasi IP lengkap |

---

## Conclusion

Menguasai antarmuka baris perintah (CLI) di Windows memberikan kontrol dan visibilitas yang jauh lebih baik ke dalam sistem. Di lingkungan kerja nyata, analis keamanan sangat mengandalkan _command line_ karena:
- Tidak semua hal terlihat secara eksplisit di antarmuka grafis (GUI).
- File rahasia atau komponen _malware_ sering disembunyikan jauh di dalam sistem.
- Analis butuh mendapat jawaban dengan cepat tanpa membuang waktu mengklik ratusan menu.
- Praktisi butuh _tools_ yang lebih cepat, presisi, dan mudah diotomasi (via _scripting_).

Skill navigasi, mencari file tersembunyi, hingga membaca file konfigurasi ini adalah keahlian fundamental yang wajib dikuasai di berbagai peran IT dan cyber security.

---

## Review

- **Navigasi Direktori:** Di environment Windows, `cd` digunakan baik untuk menampilkan path saat ini maupun berpindah direktori, sedangkan `dir` bertugas menampilkan isi foldernya. 
- **Eksplorasi Lanjutan:** Jangan menebak, gunakan `dir /a` untuk melihat objek yang disembunyikan dan `dir /s <nama_file>` untuk melacak lokasi persis sebuah file dari titik awal _root_, lalu baca konten teks secara utuh menggunakan `type`.
- **Investigasi Mesin:** Pengintaian kondisi sistem Windows sangat sederhana dengan 3 perintah utama: `whoami` (siapa yang login), `hostname` (nama identitas perangkat lokal), dan `systeminfo` (kondisi arsitektur dan versi OS mesin yang mendalam).
- **Network Basics:** Output perintah `ipconfig` esensial untuk memeriksa konfigurasi awal secara komprehensif seperti IP address lokal, subnet mask, dan jalur rute keluar ke _gateway_.
