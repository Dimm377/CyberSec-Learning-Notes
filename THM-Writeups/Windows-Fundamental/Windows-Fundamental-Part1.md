# Windows Fundamentals Part 1: Exploration Notes

**Room Link:** [TryHackMe](https://tryhackme.com/room/windowsfundamentals1xbx)

---

## Overview

Catatan ini merangkum fondasi dasar sistem operasi Windows. Memahami komponen ini sangat krusial untuk melakukan audit keamanan maupun analisis forensik, terutama bagi kita yang terbiasa di lingkungan Linux. Fokus utama di sini adalah memahami struktur sistem, manajemen file, dan kontrol akses.

---

## Windows Editions

Windows hadir dalam berbagai edisi yang disesuaikan dengan kebutuhan beban kerja dan fitur keamanan:

- **Home:** Edisi standar untuk pengguna personal dengan fitur dasar.
- **Pro:** Menambah fitur enkripsi (BitLocker) dan kemampuan untuk bergabung ke dalam _Domain_ perusahaan.
- **Enterprise:** Versi terlengkap untuk manajemen IT skala besar dengan fitur keamanan tingkat tinggi.
- **Server:** Didesain khusus untuk menjalankan infrastruktur jaringan, layanan _backend_, dan _database_.

---

## The Desktop GUI

Antarmuka visual Windows yang memfasilitasi interaksi pengguna melalui elemen-elemen berikut:

- **Start Menu:** Pusat navigasi aplikasi, dokumen, dan pencarian sistem.
- **Taskbar:** Menampilkan aplikasi yang sedang berjalan dan menyediakan jalan pintas (_shortcuts_).
- **Notification Area (System Tray):** Terletak di pojok kanan bawah; menampilkan status sistem seperti jam, jaringan, dan ikon **Volume**.

---

## The File System (NTFS)

Windows menggunakan **NTFS** (New Technology File System) sebagai standar modern untuk penyimpanan data.

- **Drive Letters:** Menggunakan sistem huruf (seperti `C:`) untuk identifikasi partisi, berbeda dengan Linux yang menggunakan _mount points_ (`/`).
- **NTFS Permissions:** Mekanisme kontrol akses (Read, Write, Execute) terhadap file dan folder untuk pengguna atau grup tertentu.

---

## The Heart of Windows: System32

Struktur direktori yang paling krusial bagi integritas sistem operasi Windows:

- **\Windows:** Folder utama tempat sistem operasi terinstal.
- **\System32:** Berisi file sistem inti (_binary_), _driver_ perangkat, dan alat penting seperti Command Prompt (`cmd.exe`) serta PowerShell.
- **Keamanan:** Folder ini sering menjadi target malware untuk menyamar atau melakukan modifikasi file sistem agar sulit dideteksi oleh pengguna awam.

---

## User Accounts & Security Control

Pilar keamanan yang membatasi hak istimewa pengguna di dalam sistem:

- **User Accounts:** Pembagian antara akun **Administrator** (memiliki hak penuh) dan **Standard User** (memiliki hak terbatas).
- **User Account Control (UAC):** Fitur keamanan yang meminta konfirmasi sebelum aplikasi melakukan perubahan tingkat sistem, guna mencegah eksekusi program berbahaya secara diam-diam.
- **Settings vs Control Panel:** Dua antarmuka utama untuk konfigurasi, di mana Control Panel lebih banyak digunakan untuk pengaturan teknis yang bersifat _legacy_.

---

## Task Manager

Utilitas utama untuk memantau kesehatan performa dan mendeteksi anomali pada sistem:

- **Processes:** Melacak penggunaan CPU, memori, dan disk oleh aplikasi serta layanan sistem yang aktif.
- **Performance:** Grafik _real-time_ yang menunjukkan beban kerja _hardware_.
- **Startup:** Mengelola aplikasi yang berjalan otomatis saat _booting_. Ini adalah titik kritis untuk mencari jejak persistensi malware.

---

## Conclusion

Menguasai Windows Fundamentals memberikan perspektif baru tentang bagaimana sistem operasi paling populer di dunia ini bekerja. Pemahaman tentang struktur file dan manajemen proses adalah langkah awal yang solid untuk menjadi seorang analis keamanan.

> **Note:** Jangan hanya melihat Windows sebagai user; lihatlah sebagai sistem yang bisa diaudit. Setiap klik di GUI sebenarnya memicu eksekusi proses yang berada di System32.
