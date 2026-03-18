# TryHackMe: Windows Fundamentals Part 2:


---

* **Room Link:** [TryHackMe](https://tryhackme.com/room/windowsfundamentals2x0x)
* **Category:** Windows Fundamental
* **Difficulty:** easy

---

## Overview

Setelah paham navigasi dasar, fokusnya pindah ke **alat-alat administratif** (System Utilities) yang dipakai buat mengelola konfigurasi, memantau sumber daya, dan mengerti struktur internal Windows. Pengetahuan ini krusial buat seorang security analyst.

---

### System Configuration (msconfig)

`msconfig` itu utility utama buat mengelola proses startup dan opsi boot — seperti **panel kontrol mesin** yang menentukan bagaimana sistem menyala.

| Fitur | Fungsi |
| ----- | ------ |
| **Startup Selection** | Pilih antara Normal, Diagnostic, atau Selective startup buat troubleshooting |
| **Boot Options** | Atur opsi booting seperti _Safe Mode_ — berguna saat sistem error atau terinfeksi malware |
| **Services Management** | Nonaktifkan background process yang tidak diperlukan — bisa buat isolasi proses mencurigakan |

---

### User Account Control (UAC)

**UAC** itu lapisan keamanan yang mencegah perubahan tingkat sistem secara sembarangan — seperti **satpam yang minta konfirmasi** sebelum siapapun boleh masuk ke ruangan penting.

| Aspek | Detail |
| ----- | ------ |
| **Fungsi Utama** | Memberi peringatan / minta credential admin saat aplikasi mencoba memodifikasi file sistem |
| **Konfigurasi** | Tingkat sensitivitas bisa diatur lewat `UserAccountControlSettings.exe` |

---

### Computer Management (compmgmt.msc)

Console administratif terpusat yang menggabungkan berbagai tool penting dalam satu tampilan:

| Tool | Fungsi | Relevansi Keamanan |
| ---- | ------ | ------------------ |
| **Task Scheduler** | Menjadwalkan tugas otomatis | Sering dimanfaatkan malware buat **persistensi** (tetap jalan meski restart) |
| **Event Viewer** | Log sistem yang mencatat setiap kejadian (Error, Warning, Info) | Bagian _Security_ penting buat mendeteksi upaya login tidak sah |
| **Shared Folders** | Memantau folder yang dibagikan di jaringan | Folder tersembunyi ditandai simbol `$` |

---

### System Information & Resource Monitor

Tools buat mendapatkan visibilitas mendalam tentang hardware dan penggunaan resource:

| Tool | Fungsi |
| ---- | ------ |
| `msinfo32.exe` (System Information) | Detail lengkap: spesifikasi hardware, versi kernel, environment variables |
| `resmon.exe` (Resource Monitor) | Lebih detail dari Task Manager — CPU, Memory, Disk, Network per proses |

---

### Windows Registry (regedit)

**Registry** itu database hierarkis yang menyimpan hampir seluruh pengaturan sistem operasi. Ibarat **DNA dari Windows** — mengontrol perilaku seluruh sistem.

| Hive | Cakupan | Fungsi |
| ---- | ------- | ------ |
| **HKEY_LOCAL_MACHINE** (HKLM) | Pengaturan global | Konfigurasi sistem, hardware, software yang berlaku buat semua user |
| **HKEY_CURRENT_USER** (HKCU) | Pengaturan user aktif | Preferensi dan konfigurasi user yang sedang login |

> **Security Insight:** Registry sering jadi target penyerang buat menonaktifkan fitur keamanan (seperti Windows Defender) atau menyembunyikan konfigurasi malware.

---

### Command Line Utilities

Meskipun Windows berbasis GUI, command line tetap penting buat tugas cepat:

| Command | Fungsi |
| ------- | ------ |
| `ipconfig` | Melihat konfigurasi IP, subnet mask, dan gateway |
| `ipconfig /all` | Info lengkap termasuk MAC Address dan DNS server |

---

### Conclusion

Menguasai utility administratif ini yang membedakan pengguna biasa sama seorang profesional IT/Security. Kemampuan navigasi Registry, analisis Event Logs, dan monitoring Resource Monitor itu pondasi utama dalam pertahanan cyber di lingkungan Windows.

> **Mindset Tip:** Selalu verifikasi aktivitas mencurigakan lewat log dan monitor sumber daya buat melihat apa yang benar-benar terjadi di balik layar.
