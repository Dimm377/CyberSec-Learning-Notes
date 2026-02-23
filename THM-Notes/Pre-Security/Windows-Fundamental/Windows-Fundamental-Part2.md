# TryHackMe: Windows Fundamentals Part 2:


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/windowsfundamentals2x0x)
- **Category:** Windows Fundamental
- **Difficulty:** easy

---

---

## Overview

Setelah paham navigasi dasar, fokusnya pindah ke alat-alat administratif (System Utilities) yang dipake buat ngelola konfigurasi, mantau sumber daya, dan ngerti struktur internal Windows. Pengetahuan ini penting banget buat seorang administrator maupun security analyst buat mendeteksi keanehan di sistem.

---

## System Configuration (msconfig)

`msconfig` itu utility utama buat ngelola proses startup dan opsi boot sistem.

- **Startup Selection:** Bikin kita bisa milih antara Normal, Diagnostic, atau Selective startup buat proses troubleshooting.
- **Boot Options:** Ngatur opsi booting kayak _Safe Mode_, yang berguna banget waktu sistem error atau terinfeksi malware.
- **Services Management:** Bikin kita bisa nonaktifin background process yang gak diperluin buat ningkatin performa atau ngisolasi proses mencurigakan.

---

## User Account Control (UAC)

UAC itu lapisan keamanan yang nyegah perubahan tingkat sistem secara sembarangan.

- **Fungsi Utama:** Ngasih peringatan atau minta credential admin waktu ada aplikasi yang nyoba ngemodif file sistem atau pengaturan sensitif.
- **Konfigurasi:** Tingkat sensitivitas UAC bisa diatur lewat `UserAccountControlSettings.exe` buat nyeimbangin antara keamanan dan kenyamanan pengguna.

---

## Computer Management (compmgmt.msc)

Console administratif terpusat yang nggabungin berbagai alat penting dalam satu tampilan.

- **Task Scheduler:** Dipake buat menjadwalkan tugas otomatis. Dalam perspektif keamanan, ini sering dimanfaatin malware buat teknik persistensi biar tetap jalan meski sistem udah di-restart.
- **Event Viewer:** Log sistem yang nyatet setiap kejadian penting (Error, Warning, Information). Analisis log di bagian _Security_ penting banget buat mendeteksi upaya login yang gak sah.
- **Shared Folders:** Mantau folder mana aja yang dibagiin di jaringan, termasuk folder tersembunyi yang ditandai sama simbol `$`.

---

## System Information & Resource Monitor

Alat buat dapetin visibilitas mendalam tentang perangkat keras dan penggunaan sumber daya secara real-time.

- **System Information (msinfo32.exe):** Ngasih detail lengkap soal spesifikasi hardware, versi kernel, sampai variabel lingkungan (environment variables).
- **Resource Monitor (resmon.exe):** Ngasih gambaran lebih detail dibanding Task Manager, mencakup penggunaan CPU, Memory, Disk, dan Network oleh setiap proses yang aktif.

---

## Windows Registry (regedit)

Database hierarkis yang nyimpen hampir seluruh pengaturan sistem operasi dan aplikasi pihak ketiga.

- **Structure:** Data disimpen dalam struktur yang disebut _Hives_, kayak `HKEY_LOCAL_MACHINE` (HKLM) buat pengaturan global dan `HKEY_CURRENT_USER` (HKCU) buat pengaturan user aktif.
- **Security Insight:** Registry sering jadi target modifikasi sama penyerang buat nonaktifin fitur keamanan (kayak Windows Defender) atau nyembunyiin konfigurasi malware.

---

## Command Line Utilities

Meskipun Windows berbasis GUI, penggunaan Command Prompt tetap penting buat tugas cepat.

- **ipconfig:** Alat standar buat liat konfigurasi IP, subnet mask, dan gateway. Pake parameter `/all` buat info tambahan kayak alamat fisik (MAC Address) dan server DNS.

---

## Conclusion

Nguasain utility administratif ini yang ngebedain pengguna biasa sama seorang profesional IT/Security. Kemampuan navigasi Registry, analisis Event Logs, dan monitoring Resource Monitor itu pondasi utama dalam pertahanan cyber di lingkungan Windows.

> **Mindset Tip:** "Don't just trust the GUI." Selalu verifikasi aktivitas mencurigakan lewat log dan monitor sumber daya buat liat apa yang beneran terjadi di balik layar.
