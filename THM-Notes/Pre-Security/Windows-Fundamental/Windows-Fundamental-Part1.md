# TryHackMe: Windows Fundamentals Part 1


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/windowsfundamentals1xbx)
- **Category:** Windows Fundamental
- **Difficulty:** easy

---

---

## Overview

Catatan ini merangkum pondasi dasar sistem operasi Windows. Mengerti komponen-komponen ini penting banget buat audit keamanan maupun analisis forensik, terutama buat kita yang terbiasa di lingkungan Linux. Fokus utamanya di sini itu mengerti struktur sistem, manajemen file, dan kontrol akses.

---

## Windows Editions

Windows hadir dalam berbagai edisi yang disesuaiin sama kebutuhan beban kerja dan fitur keamanan:

- **Home:** Edisi standar buat pengguna personal dengan fitur dasar.
- **Pro:** Nambahin fitur enkripsi (BitLocker) dan kemampuan buat gabung ke _Domain_ perusahaan.
- **Enterprise:** Versi terlengkap buat manajemen IT skala besar dengan fitur keamanan tingkat tinggi.
- **Server:** Didesain khusus buat jalanin infrastruktur jaringan, layanan _backend_, dan _database_.

---

## The Desktop GUI

Antarmuka visual Windows yang jadi tempat kita berinteraksi lewat elemen-elemen berikut:

- **Start Menu:** Pusat navigasi aplikasi, dokumen, dan pencarian sistem.
- **Taskbar:** Menampilkan aplikasi yang lagi jalan dan menyediakan jalan pintas (_shortcuts_).
- **Notification Area (System Tray):** Letaknya di pojok kanan bawah; menampilkan status sistem seperti jam, jaringan, dan ikon **Volume**.

---

## The File System (NTFS)

Windows pakai **NTFS** (New Technology File System) sebagai standar modern buat penyimpanan data.

- **Drive Letters:** Pakai sistem huruf (seperti `C:`) buat identifikasi partisi, beda sama Linux yang pakai _mount points_ (`/`).
- **NTFS Permissions:** Mekanisme kontrol akses (Read, Write, Execute) terhadap file dan folder buat pengguna atau grup tertentu.

---

## The Heart of Windows: System32

Struktur direktori yang paling penting buat integritas sistem operasi Windows:

- **\Windows:** Folder utama tempat sistem operasi terinstal.
- **\System32:** Berisi file sistem inti (_binary_), _driver_ perangkat, dan alat penting seperti Command Prompt (`cmd.exe`) serta PowerShell.
- **Keamanan:** Folder ini sering jadi target malware buat nyamar atau ngelakuin modifikasi file sistem agar susah dideteksi sama pengguna awam.

---

## User Accounts & Security Control

Pilar keamanan yang ngebatasin hak istimewa pengguna di dalam sistem:

- **User Accounts:** Pembagian antara akun **Administrator** (punya hak penuh) dan **Standard User** (punya hak terbatas).
- **User Account Control (UAC):** Fitur keamanan yang minta konfirmasi sebelum aplikasi lakuin perubahan tingkat sistem, buat nyegah eksekusi program berbahaya secara diam-diam.
- **Settings vs Control Panel:** Dua antarmuka utama buat konfigurasi, di mana Control Panel lebih banyak dipake buat pengaturan teknis yang bersifat _legacy_.

---

## Task Manager

Utility utama buat mantau kesehatan performa dan mendeteksi keanehan di sistem:

- **Processes:** Ngelacak penggunaan CPU, memori, dan disk oleh aplikasi serta layanan sistem yang aktif.
- **Performance:** Grafik _real-time_ yang nunjukin beban kerja _hardware_.
- **Startup:** Ngelola aplikasi yang jalan otomatis waktu _booting_. Ini titik kritis buat mencari jejak persistensi malware.

---

## Conclusion

Menguasai Windows Fundamentals memberikan perspektif baru tentang bagaimana sistem operasi paling populer di dunia ini bekerja. Mengerti struktur file dan manajemen proses adalah langkah awal yang solid untuk menjadi seorang security analyst.

