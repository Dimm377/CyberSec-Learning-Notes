# TryHackMe: Shells Overview

- **Room Link:** [Shells Overview](https://tryhackme.com/room/shellsoverview)
- **Category:** Offensive Security Tooling
- **Difficulty:** Easy

## Task 1: Shellls Introduction

Shells dalam cybersecurity sering sekali dipakai oleh attackers buat ngontrol sistem dari jarak jauh (remote), yang bikin mereka jadi bagian penting dari rantai serangan (attack chain)., kita bakal eksplor berbagai jenis shells yang dipake dalam offensive security, bedanya apa aja, dan kapan pakainya. Pengetahuan ini bisa ngebantu ningkatin skill penetration testing dan exploitation, dan juga ngebantu kita paham gimana cara mendeteksi kalau ada remote shell yang lagi dipake attacker di dalam organisasi.

### Learning Objectives

kita bakal bahas tujuan pembelajaran berikut:

- Memahami Shells dalam Offensive Security
- Cara Setup dan Menggunakan Reverse dan Bind Shells
- Deploy Web Shells

## Task 2: Shells Overview

### What is a Shell?

Shell itu ibarat jembatan alias software yang bikin kita bisa ngobrol sama Sistem Operasi (OS). Bisa bentuknya grafis (GUI), tapi biasanya Command-Line Interface (CLI), tergantung OS targetnya.

Di dunia cyber security, shell ini merujuk ke sesi khusus yang dipake attacker pas berhasil jebol sistem. Lewat shell ini, mereka bisa jalanin perintah atau software sesuka hati.

Banyak hal nakal yang bisa dilakuin kalau udah dapet shell:

- **Remote System Control**: Attacker bisa ngontrol sistem target dari jauh.
- **Privilege Escalation**: Kalau akses awal masih terbatas (user biasa), attacker bakal cari cara buat naik pangkat jadi admin/root.
- **Data Exfiltration**: Bisa baca dan copy data-data sensitif keluar dari sistem.
- **Persistence**: Bikin "pintu belakang" (backdoor) atau user baru biar nanti bisa masuk lagi kapan aja tanpa harus nge-hack dari ulang.
- **Post-Exploitation**: Aktivitas lanjutan kayak sebar malware, bikin akun tersembunyi, atau hapus jejak (logs).
- **Pivoting**: Pake sistem yang udah dijebol buat nyerang sistem lain di jaringan yang sama. Jadi batu loncatan gitu.

Intinya, semua jenis shell yang bakal kita bahas nanti tujuannya ya buat memfasilitasi aksi-aksi di atas ini.
