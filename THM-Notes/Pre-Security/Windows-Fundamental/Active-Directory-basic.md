# TryHackMe: Windows Active Directory Basics:


---

* **Room Link:** [TryHackMe](https://tryhackme.com/room/winadbasics)
* **Category:** Windows / Active Directory
* **Difficulty:** easy

---

## Overview

**Active Directory (AD)** itu tulang punggung jaringan korporat yang memungkinkan manajemen terpusat buat seluruh sumber daya di jaringan. Analogi: AD itu ibarat **sistem administrasi gedung perkantoran besar** — mengatur siapa boleh masuk ruangan mana, siapa punya kunci apa, dan aturan apa yang berlaku di setiap lantai.

---

### AD Components & Hierarchy

Struktur AD dibangun dari komponen logis dan fisik yang saling terhubung:

| Komponen | Analogi | Fungsi |
| -------- | ------- | ------ |
| **Domain Controller (DC)** | Meja resepsionis utama | Server pusat yang menjalankan AD Domain Services (ADDS) dan mengelola autentikasi |
| **Organizational Units (OUs)** | Folder departemen (HR, IT, Finance) | Kontainer logis buat mengelompokkan user, grup, dan komputer agar mudah menerapkan kebijakan |
| **Trees** | Cabang perusahaan di satu kota | Kumpulan domain yang saling terkait |
| **Forests** | Seluruh grup perusahaan | Kumpulan satu atau lebih Trees yang berbagi skema dan katalog global yang sama |

---

### User & Computer Management

Mengelola objek di AD menggunakan tool _Active Directory Users and Computers_:

| Konsep | Detail |
| ------ | ------ |
| **Object Protection** | Buat hapus OU yang terproteksi, aktifkan "Advanced Features" lalu nonaktifkan "Protect object from accidental deletion" |
| **Machine Accounts** | Setiap komputer yang gabung ke domain punya akun khusus — ditandai simbol `$` di akhir nama (contoh: `PC01$`) |
| **Delegation** | Admin bisa memberi hak spesifik ke user biasa (misal: reset password) tanpa memberikan hak administratif penuh |

---

### Groups & Group Policy Objects (GPO)

GPO dipakai buat menerapkan konfigurasi keamanan secara otomatis ke seluruh perangkat dalam domain:

| Aspek | Detail |
| ----- | ------ |
| **Management Tool** | Kebijakan dikelola lewat _Group Policy Management Console_ (`gpmc.msc`) |
| **Distribution** | File kebijakan disimpan dan didistribusikan lewat folder share `SYSVOL` |
| **Hierarchy** | GPO bisa diterapkan di tingkat **Site → Domain → OU** (urutan prioritas) |

---

### Authentication Protocols

AD pakai dua protokol utama buat proses login dan akses sumber daya:

| Protokol | Analogi | Cara Kerja | Status |
| -------- | ------- | ---------- | ------ |
| **Kerberos** | Tiket VIP — sekali dapat, bisa masuk ke banyak acara | Pakai sistem tiket (TGT) buat meminimalisir pengiriman kredensial di jaringan | Default, lebih aman |
| **NTLM** | Pertanyaan keamanan — server kasih tantangan, client harus jawab | Challenge-response tanpa tiket | Legacy, masih dipakai buat kompatibilitas |

> **Security Fact:** Baik Kerberos maupun NTLM dirancang agar password plaintext **tidak pernah** dikirim lewat jaringan.

---

### Domain Trusts

Trust relationship memungkinkan pengguna dari satu domain mengakses sumber daya di domain lain:

| Tipe Trust | Cara Kerja |
| ---------- | ---------- |
| **Transitive** | A percaya B, B percaya C → A otomatis percaya C |
| **One-way** | Hanya satu arah (A percaya B, tapi B ga percaya A) |
| **Two-way** | Dua arah (A dan B saling percaya) |
