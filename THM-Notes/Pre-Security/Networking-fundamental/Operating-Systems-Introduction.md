# TryHackMe: Operating Systems Introduction

- **Room Link:** [Operating Systems Introduction](https://tryhackme.com/room/introtoos)
- **Category:** Pre-Security
- **Difficulty:** Easy

---

## Introduction

Setiap hari kamu menyalakan laptop atau HP, dan semuanya langsung *just works*,  aplikasi terbuka, file bisa diakses, musik bisa diputar. Tapi pernahkah kamu berpikir, siapa yang sebenarnya mengatur semua itu di balik layar?

Di room Computer Fundamentals sebelumnya, kamu sudah belajar soal komponen fisik komputer dan jenis-jenis perangkat komputasi. Sekarang saatnya membongkar lapisan tak terlihat yang menyatukan semua komponen itu menjadi satu kesatuan yang bisa kamu pakai: **Operating System** (OS).

### Scenario

Temanmu baru saja meng-upgrade PC nya dan memberikan laptop lamanya ke kamu secara gratis, laptopnya sudah lama tidak dipakai, dan dia cuma ingat laptopnya masih bisa nyala. Sebelum kamu memutuskan mau di-upgrade, di wipe, dijual, atau dijadikan proyek baru, kamu perlu tahu dulu apa yang sedang kamu hadapi, dan itu dimulai dari memahami OS-nya.

### Learning Objectives

Setelah menyelesaikan room ini, kamu akan paham:
- Apa itu **Operating System** dan peran utamanya dalam sebuah komputer.
- Apa saja tugas inti yang dijalankan oleh sebuah OS.
- Jenis-jenis OS yang umum dipakai dan kapan masing-masing cocok digunakan.
- Bagaimana cara berinteraksi dengan OS untuk menggali informasi sistem.

---

## The Invisible Manager

### What is an Operating System?

**Operating System** (OS) adalah software inti yang mengkoordinasi seluruh aktivitas di dalam komputer. OS duduk di antara **user**, **aplikasi**, dan **hardware fisik** — berperan sebagai manajer tak terlihat yang menjaga agar semuanya berjalan sebagai satu kesatuan.

Susunan lapisannya seperti ini:

```
┌─────────────────────┐
│       User          │
├─────────────────────┤
│    Applications     │
├─────────────────────┤
│  Operating System   │
├─────────────────────┤
│     Hardware        │
└─────────────────────┘
```

Kenapa kita butuh OS? Karena tanpanya, setiap aplikasi harus mengakses CPU, memory, file, perangkat, dan keamanan secara langsung — dan pasti bakal bentrok satu sama lain. OS hadir sebagai pengatur pusat yang mencegah kekacauan itu.

Cara paling gampang memahaminya: bayangkan komputermu sebagai **bandara yang sibuk**.

- **Hardware** (CPU, RAM, storage) = landasan pacu, pesawat, radar, dan infrastruktur fisik bandara.
- **Aplikasi** (browser, game, music player) = maskapai penerbangan dan penumpangnya, yang semuanya ingin lepas landas, mendarat, dan meminta layanan.
- **Operating System** = **seluruh sistem kontrol lalu lintas udara (ATC)**. OS yang menjadwalkan, mengarahkan, menyelesaikan konflik antar pesawat, dan memastikan keselamatan seluruh operasi bandara.

Tanpa ATC, pesawat-pesawat itu akan saling tabrak di landasan. Tanpa OS, aplikasi-aplikasimu akan saling rebutan resource dan membuat sistem *crash*.

---

### System Privilege Layers

Di dalam komputer, tidak semua bagian sistem punya level akses yang sama. Ada pemisahan yang dibuat secara sengaja untuk mencegah konflik dan menjaga keamanan:

- **Kernel Space**: Area yang sangat terbatas di mana **kernel** (inti dari OS) berjalan. Kernel punya akses penuh dan langsung ke CPU, memory, storage, dan semua hardware. Hanya komponen terpercaya yang boleh beroperasi di sini.

- **User Space**: Area tempat semua aplikasi standar berjalan. Aplikasi di user space **sengaja dilarang** mengakses hardware secara langsung. Kalau mereka butuh membuka file, menyimpan data, atau mengakses jaringan, mereka harus mengirim permintaan melalui **system call** ke kernel, dan kernel yang akan mengeksekusi permintaan tersebut atas nama aplikasi.

Kembali ke analogi bandara: **kernel space** itu seperti menara kontrol (ATC tower) — area yang sangat terbatas dan hanya petugas ATC terpercaya yang boleh masuk. Sementara itu, **user space** itu seperti maskapai dan penumpang di area terminal. Mereka tidak boleh masuk ke menara kontrol atau menyentuh peralatan langsung. Kalau mereka butuh sesuatu (izin lepas landas, perubahan rute), mereka harus menghubungi menara lewat radio — dan itulah **system call**.

Pemisahan ini krusial karena menjaga OS tetap stabil: satu aplikasi yang bermasalah tidak akan bisa merusak keseluruhan sistem, sama seperti satu maskapai yang bermasalah tidak bisa meruntuhkan operasi seluruh bandara tanpa kendali dari menara ATC.

---

### Operating System Duties

Sekarang kamu sudah paham apa itu OS dan bagaimana privilege-nya dipisahkan. Saatnya melihat tugas-tugas inti apa saja yang dijalankan OS di balik layar agar komputermu bisa berjalan dengan aman, efisien, dan konsisten.

| Tanggung Jawab | Yang Dilakukan OS | Contoh |
| :--- | :--- | :--- |
| **Process Management** | Membuat, menjadwalkan, memprioritaskan, dan menghentikan program yang sedang berjalan. OS yang menentukan berapa banyak waktu CPU yang didapat setiap proses agar *multitasking* terasa mulus. | Membuka browser, music player, dan media sosial bersamaan tanpa komputer *freeze*. |
| **Memory Management** | Mengalokasikan **RAM** (*Random Access Memory* — memori utama yang dipakai untuk menyimpan data sementara selagi program berjalan) ke setiap proses, melindunginya dari proses lain, dan merebut kembali memori saat aplikasi ditutup. Kalau RAM mulai penuh, OS memakai **virtual memory** untuk menjaga stabilitas. | Membuka banyak aplikasi sekaligus — OS memastikan masing-masing mendapat jatah RAM sendiri dan tidak saling mengganggu. |
| **File System Management** | Mengorganisir file ke dalam folder, menangani penamaan, path, permissions, dan metadata (nama, ukuran, tipe, timestamp). | Membuat folder baru, menyimpan foto, atau mengatur file menjadi "read only". |
| **User Management** | Mengelola banyak akun pengguna, proses autentikasi, dan permission untuk menentukan siapa yang boleh mengakses apa. | Login dengan password milikmu dan file-filemu tidak bisa diakses oleh akun user lain. |
| **Device Management** | Memuat driver dan menyediakan antarmuka universal (**Hardware Abstraction Layer**) sehingga aplikasi cukup memerintahkan "cetak ini" atau "putar suara ini" tanpa perlu tahu detail hardware-nya. | Mencolokkan mouse baru, printer, atau hard drive eksternal — dan langsung bisa dipakai. |

> **for your information:**
> **Virtual memory** — mekanisme di mana OS "meminjam" sebagian ruang storage (hard disk/SSD) untuk berpura-pura menjadi RAM tambahan saat RAM fisik sudah penuh.
> **Hardware Abstraction Layer (HAL)** — lapisan software yang menyembunyikan perbedaan hardware dari aplikasi, sehingga satu perintah yang sama bisa berjalan di berbagai perangkat tanpa modifikasi.

---

### Operating System Security

Sebelum antivirus, *firewall*, atau tool keamanan apapun dipasang, OS sudah menjalankan mekanisme proteksi dasarnya sendiri. Ini adalah fondasi keamanan yang selalu aktif:

- **Authentication**: Memverifikasi identitas pengguna melalui password, PIN, atau biometrik.
- **Permissions**: Mengontrol secara persis apa yang boleh dilakukan oleh setiap user dan aplikasi — membaca, menulis, atau mengeksekusi file.
- **Isolation**: Memisahkan setiap proses ke dalam "kotak" terisolasinya sendiri (ingat pemisahan kernel space dan user space). Satu proses yang rusak tidak akan bisa menjatuhkan proses lain.
- **System Protection**: Menjaga file-file sistem yang kritikal dan pengaturan penting dari perubahan yang tidak sah.

---
