# Windows Fundamentals Part 2: System Administration & Internals

**Room Link:** [TryHackMe](https://tryhackme.com/room/windowsfundamentals2x0x)
**Category:** Windows Fundamental
**Difficulty:** easy

---

## Overview

Setelah memahami navigasi dasar, fokus beralih pada alat-alat administratif (System Utilities) yang digunakan untuk mengelola konfigurasi, memantau sumber daya, dan memahami struktur internal Windows. Pengetahuan ini sangat krusial bagi seorang administrator maupun analis keamanan untuk mendeteksi anomali pada sistem.

---

## System Configuration (msconfig)

`msconfig` adalah utilitas utama untuk mengelola proses startup dan opsi boot sistem.

- **Startup Selection:** Memungkinkan pemilihan antara Normal, Diagnostic, atau Selective startup untuk proses troubleshooting.
- **Boot Options:** Mengatur opsi booting seperti _Safe Mode_, yang sangat berguna saat sistem mengalami kegagalan fungsi atau terinfeksi malware.
- **Services Management:** Memungkinkan pengguna untuk menonaktifkan layanan latar belakang yang tidak diperlukan guna meningkatkan performa atau mengisolasi proses mencurigakan.

---

## User Account Control (UAC)

UAC adalah lapisan keamanan yang mencegah perubahan tingkat sistem secara tidak sah.

- **Fungsi Utama:** Memberikan peringatan atau meminta kredensial administratif saat ada aplikasi yang mencoba melakukan modifikasi pada file sistem atau pengaturan sensitif.
- **Konfigurasi:** Tingkat sensitivitas UAC dapat diatur melalui `UserAccountControlSettings.exe` untuk menyeimbangkan antara keamanan dan kenyamanan pengguna.

---

## Computer Management (compmgmt.msc)

Konsol administratif terpusat yang menggabungkan berbagai alat penting dalam satu tampilan.

- **Task Scheduler:** Digunakan untuk menjadwalkan tugas otomatis. Dalam perspektif keamanan, ini sering dimanfaatkan malware untuk teknik persistensi agar tetap berjalan meski sistem telah di-restart.
- **Event Viewer:** Log sistem yang mencatat setiap kejadian penting (Error, Warning, Information). Analisis log pada bagian _Security_ sangat penting untuk mendeteksi upaya login tidak sah.
- **Shared Folders:** Memantau folder mana saja yang dibagikan dalam jaringan, termasuk folder tersembunyi yang ditandai dengan simbol `$`.

---

## System Information & Resource Monitor

Alat untuk mendapatkan visibilitas mendalam tentang perangkat keras dan penggunaan sumber daya secara real-time.

- **System Information (msinfo32.exe):** Memberikan detail lengkap mengenai spesifikasi hardware, versi kernel, hingga variabel lingkungan (environment variables).
- **Resource Monitor (resmon.exe):** Memberikan gambaran lebih detail dibanding Task Manager, mencakup penggunaan CPU, Memory, Disk, dan Network oleh setiap proses yang aktif.

---

## Windows Registry (regedit)

Database hierarkis yang menyimpan hampir seluruh pengaturan sistem operasi dan aplikasi pihak ketiga.

- **Structure:** Data disimpan dalam struktur yang disebut _Hives_, seperti `HKEY_LOCAL_MACHINE` (HKLM) untuk pengaturan global dan `HKEY_CURRENT_USER` (HKCU) untuk pengaturan user aktif.
- **Security Insight:** Registry sering menjadi target modifikasi oleh penyerang untuk menonaktifkan fitur keamanan (seperti Windows Defender) atau menyembunyikan konfigurasi malware.

---

## Command Line Utilities

Meskipun Windows berbasis GUI, penggunaan Command Prompt tetap krusial untuk tugas cepat.

- **ipconfig:** Alat standar untuk melihat konfigurasi IP, subnet mask, dan gateway. Penggunaan parameter `/all` memberikan informasi tambahan seperti alamat fisik (MAC Address) dan server DNS.

---

## Conclusion

Menguasai utilitas administratif ini adalah pembeda antara pengguna biasa dan seorang profesional IT/Security. Kemampuan untuk menavigasi Registry, menganalisis Event Logs, dan memantau Resource Monitor adalah fondasi utama dalam pertahanan siber di lingkungan Windows.

> **Mindset Tip:** "Don't just trust the GUI." Selalu verifikasi aktivitas mencurigakan melalui log dan monitor sumber daya untuk melihat apa yang benar-benar terjadi di balik layar.
