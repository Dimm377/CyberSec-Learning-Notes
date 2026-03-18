# TryHackMe: Windows Fundamentals Part 1


---

* **Room Link:** [TryHackMe](https://tryhackme.com/room/windowsfundamentals1xbx)
* **Category:** Windows Fundamental
* **Difficulty:** easy

---

## Overview

Catatan ini merangkum pondasi dasar sistem operasi Windows. Mengerti komponen-komponen ini penting buat audit keamanan maupun analisis forensik, terutama buat yang terbiasa di lingkungan Linux.

---

### Windows Editions

Windows hadir dalam berbagai edisi yang disesuaikan dengan kebutuhan:

| Edisi | Target Pengguna | Fitur Kunci |
| ----- | --------------- | ----------- |
| **Home** | Pengguna personal | Fitur dasar, tanpa domain join |
| **Pro** | Profesional / bisnis kecil | Enkripsi (BitLocker), bisa gabung ke _Domain_ perusahaan |
| **Enterprise** | Organisasi besar | Manajemen IT skala besar, fitur keamanan tingkat tinggi |
| **Server** | Infrastruktur jaringan | Dirancang buat menjalankan layanan _backend_ dan _database_ |

---

### The Desktop GUI

Antarmuka visual Windows — seperti **dashboard mobil** yang menampilkan semua informasi penting:

| Elemen | Fungsi |
| ------ | ------ |
| **Start Menu** | Pusat navigasi aplikasi, dokumen, dan pencarian sistem |
| **Taskbar** | Menampilkan aplikasi yang sedang berjalan + jalan pintas (_shortcuts_) |
| **System Tray** (pojok kanan bawah) | Status sistem: jam, jaringan, volume, notifikasi |

---

### The File System (NTFS)

Windows pakai **NTFS** (New Technology File System) sebagai standar penyimpanan data modern.

| Aspek | Windows (NTFS) | Linux (ext4) |
| ----- | -------------- | ------------ |
| **Identifikasi Partisi** | Huruf drive (`C:`, `D:`) | Mount points (`/`, `/home`) |
| **Kontrol Akses** | NTFS Permissions (Read, Write, Execute) per file/folder | Unix Permissions (rwx) per user/group/others |
| **Case Sensitivity** | Tidak (File.txt = file.txt) | Ya (File.txt ≠ file.txt) |

---

### The Heart of Windows: System32

Struktur direktori paling penting buat integritas sistem operasi. Ibarat **otak** dari Windows — kalau rusak, seluruh sistem bisa lumpuh.

| Direktori | Fungsi | Catatan Keamanan |
| --------- | ------ | ---------------- |
| `\Windows` | Folder utama sistem operasi | — |
| `\System32` | File sistem inti, driver, `cmd.exe`, PowerShell | Sering jadi target malware buat nyamar atau modifikasi file sistem |

---

### User Accounts & Security Control

Pilar keamanan yang membatasi hak istimewa pengguna di dalam sistem:

| Konsep | Analogi | Fungsi |
| ------ | ------- | ------ |
| **Administrator** | Pemilik gedung — akses ke semua ruangan | Hak penuh atas sistem |
| **Standard User** | Penyewa biasa — akses terbatas | Hak terbatas, tidak bisa install/uninstall bebas |
| **UAC** (User Account Control) | Satpam yang minta konfirmasi sebelum buka pintu | Mencegah eksekusi program berbahaya secara diam-diam |

**Settings vs Control Panel:** Dua antarmuka buat konfigurasi. Control Panel lebih banyak dipakai buat pengaturan teknis yang bersifat _legacy_.

---

### Task Manager

Utility utama buat memantau kesehatan sistem — seperti **monitor kesehatan** yang mendeteksi keanehan:

| Tab | Fungsi | Relevansi Keamanan |
| --- | ------ | ------------------ |
| **Processes** | Melacak penggunaan CPU, memori, dan disk | Mendeteksi proses mencurigakan |
| **Performance** | Grafik _real-time_ beban kerja hardware | Melihat anomali resource usage |
| **Startup** | Mengelola aplikasi yang jalan otomatis saat _booting_ | **Titik kritis buat mencari jejak persistensi malware** |

---

### Conclusion

Menguasai Windows Fundamentals memberikan perspektif baru tentang bagaimana sistem operasi paling populer di dunia ini bekerja. Mengerti struktur file dan manajemen proses adalah langkah awal yang solid untuk menjadi seorang security analyst.
