# TryHackMe: Windows Basics

- **Room Link:** [Windows Basics](https://tryhackme.com/room/windowsbasics)
- **Category:** Pre-Security / Computers Fundamentals
- **Difficulty:** Easy

_(Room ini adalah pengantar praktis sebelum masuk ke materi lebih dalam di catatan [Windows Fundamental Part 1](../Windows-Fundamental/Windows-Fundamental-Part1.md) dan [Windows Fundamental Part 2](../Windows-Fundamental/Windows-Fundamental-Part2.md))_

---

## Introduction

Di room sebelumnya (Operating Systems Introduction), kamu sudah belajar apa itu OS secara teori — bagaimana sistem operasi mengelola hardware, menjalankan proses, dan menjadi perantara antara pengguna dan mesin. Room ini mengambil langkah berikutnya: langsung terjun ke **Microsoft Windows**, OS desktop yang paling banyak digunakan di dunia.

Skenarionya sederhana — kamu berperan sebagai karyawan baru di **TryHatMe** yang baru saja mendapat workstation Windows. Tugas pertamamu: kenali lingkungan kerja digital ini. Mulai dari navigasi desktop, membuka tools lewat Start Menu, mengatur file di folder perusahaan, mengecek system settings, sampai memantau performa lewat Task Manager.

Setelah menyelesaikan room ini, kamu akan paham:

- Bagaimana navigasi antarmuka grafis Windows — desktop, taskbar, dan Start Menu.
- Cara menggunakan File Explorer untuk menjelajah folder, memahami file path, dan mengorganisir file.
- Bagaimana mengecek dan mengatur system settings lewat aplikasi Settings.
- Cara menggunakan tools bawaan seperti **Task Manager** dan **Windows Security** untuk memantau performa dan proteksi sistem.

---

## Exploring the Windows Workspace

### Windows Evolution

Sebelum Windows punya tampilan yang rapi seperti sekarang, komputer Microsoft masih mengandalkan **MS-DOS** (_Microsoft Disk Operating System_) — layar hitam yang mengharuskan pengguna mengetik perintah teks secara manual untuk melakukan apapun.

> **for your information:** **MS-DOS** (_Microsoft Disk Operating System_) adalah OS berbasis teks yang mendominasi era komputer personal tahun 1980-an. Tidak ada mouse, tidak ada ikon — semua interaksi dilakukan lewat command line.

Tahun 1985, Microsoft merilis **Windows 1.0** — GUI (_Graphical User Interface_) sederhana yang dibangun di atas DOS. Untuk pertama kalinya, pengguna bisa menggunakan mouse, mengklik ikon, dan bekerja dengan jendela (_windows_). Dari situ, Windows terus berkembang: menambahkan fitur, memperbaiki arsitektur, hingga menjadi OS mandiri yang tidak lagi bergantung pada DOS.

| Versi | Tahun | Catatan Penting |
| :--- | :---: | :--- |
| Windows 1.0 | 1985 | GUI pertama Microsoft, masih berjalan di atas DOS |
| Windows ME | 2000 | Terakhir berbasis DOS, terkenal tidak stabil |
| Windows 11 | 2021 | Versi terbaru dengan desain modern dan persyaratan TPM 2.0 |

### Logging in and Authentication

Sebelum bisa masuk ke Desktop, kamu harus melewati proses **autentikasi** — membuktikan identitasmu ke sistem. Windows menampilkan **login screen** di mana kamu memilih akun pengguna dan memasukkan kredensial (password, PIN, atau metode verifikasi lain).

Setiap akun memiliki level izin (_permission level_) yang menentukan apa saja yang boleh dilakukan di sistem. Windows menggunakan tiga tipe akun utama:

| Tipe Akun | Hak Akses |
| :--- | :--- |
| **Guest** | Akses terbatas dan sementara. Tidak bisa mengubah system settings. Cocok untuk pengguna yang hanya butuh akses sesaat tanpa meninggalkan jejak permanen. |
| **Standard** | Akun harian biasa. Bisa menjalankan aplikasi dan mengubah pengaturan personal, tapi tidak bisa menginstal software atau mengubah konfigurasi level sistem. |
| **Administrator** | Kontrol penuh atas seluruh sistem — instalasi software, perubahan konfigurasi, manajemen user, semuanya. |

> **Common Mistake:** Di lab atau CTF, kamu sering langsung login sebagai Administrator karena butuh akses penuh. Tapi di dunia nyata, menjalankan aktivitas sehari-hari dengan akun Administrator adalah kebiasaan buruk — jika malware berjalan di sesi Administrator, malware itu juga mendapat hak akses penuh ke seluruh sistem.

### The Windows Desktop

Setelah berhasil login, area pertama yang kamu lihat adalah **Desktop** — ruang kerja utama tempat semua aktivitas bermula. Dua komponen besar yang membentuk lingkungan ini:

- **Desktop** — area utama tempat file, folder, dan shortcut ditampilkan.
- **Taskbar** — strip kontrol di bagian bawah layar yang menyediakan akses ke aplikasi, system tools, pengaturan, dan notifikasi.

Komponen-komponen kunci di lingkungan Desktop:

| No | Komponen | Fungsi |
| :---: | :--- | :--- |
| 1 | **Desktop Icons** | Shortcut ke Recycle Bin, folder, dan aplikasi yang sering dipakai. Bisa di-customize sesuai kebutuhan. |
| 2 | **Start Menu** | Titik akses utama ke aplikasi, file, folder, settings, dan opsi power (shutdown/restart/logout). |
| 3 | **Search** | Pencarian cepat untuk menemukan aplikasi, file, folder, dan system settings berdasarkan kata kunci. |
| 4 | **Task View** | Menampilkan semua jendela yang sedang terbuka untuk berpindah cepat antar aplikasi. |
| 5 | **Pinned Apps & Folders** | Aplikasi dan folder yang di-pin ke taskbar agar gampang diakses tanpa membuka Start Menu. |
| 6 | **Network & Audio** | Kontrol cepat untuk koneksi jaringan dan volume audio. |
| 7 | **Date & Time** | Menampilkan tanggal dan waktu, bisa dibuka ke kalender lengkap. |
| 8 | **Notifications** | Pusat notifikasi dari sistem dan aplikasi, plus akses cepat ke beberapa settings. |

### Start Menu

**Start Menu** diakses dengan mengklik ikon Windows di pojok kiri bawah taskbar (atau menekan tombol `Win` di keyboard). Ini adalah pusat komando di Windows — dari sini kamu bisa:

- Membuka aplikasi apa saja yang terinstal di sistem.
- Mengakses folder penting (Documents, Pictures, Downloads).
- Membuka Settings dan Control Panel.
- Shutdown, restart, atau logout dari sistem.

Di sisi kanan Start Menu, biasanya ada area **Quick Access** yang menampilkan aplikasi dan folder yang sering digunakan atau di-pin secara manual.

### Built-in Tools and Apps

Windows sudah dilengkapi dengan tools bawaan yang langsung tersedia tanpa perlu instalasi tambahan. Beberapa yang paling sering dipakai:

| Tool | Fungsi |
| :--- | :--- |
| **Notepad** | Text editor paling sederhana untuk mengedit file teks biasa. |
| **File Explorer** | Alat untuk menjelajahi, memindahkan, menyalin, dan mengorganisir file dan folder. |
| **Calculator** | Kalkulator standar dan scientific. |
| **Paint** | Editor gambar sederhana bawaan Windows. |

Semua tools ini bisa diakses lewat Start Menu atau Search bar.

### Getting System Information

Windows menyediakan aplikasi **Settings** yang menjadi pusat konfigurasi sistem. Untuk mendapatkan gambaran umum tentang spesifikasi mesin yang sedang kamu gunakan, buka **Settings → About your PC**.

Halaman ini menampilkan informasi penting:

- **Device name** — nama komputer di jaringan.
- **Processor** — tipe dan kecepatan CPU.
- **Installed RAM** — jumlah memori yang terpasang.
- **System type** — arsitektur OS (32-bit atau 64-bit).
- **Windows edition & version** — edisi dan versi Windows yang terinstal.

> **for your information:** Informasi ini sangat berguna dalam konteks keamanan. Saat melakukan pentest atau forensik, mengetahui versi OS, arsitektur (32/64-bit), dan build number membantu menentukan exploit mana yang relevan dan patch mana yang sudah atau belum diterapkan.

---

_Catatan ini masih dalam proses drafting. Task selanjutnya akan ditambahkan setelah materi lengkap diterima._
