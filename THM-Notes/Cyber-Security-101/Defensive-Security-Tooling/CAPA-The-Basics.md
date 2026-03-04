# TryHackMe: CAPA The Basics

- **Room Link:** [CAPA The Basics](https://tryhackme.com/room/capathebasics)
- **Category:** Defensive Security Tooling
- **Difficulty:** Easy

### Learning Objectives

- Memahami **apa itu CAPA** dan perannya dalam static analysis.
- Mempelajari cara **menggunakan CAPA secara efektif** untuk membedah file.
- Memahami **field dan hasil (results)** yang ditampilkan oleh tool ini.
- Memanfaatkan CAPA untuk **mengidentifikasi potensi aktivitas** berbahaya dari sebuah program.


## Introduction

### Analyzing Suspicious Files

Salah satu tantangan terbesar di dunia malware analysis adalah: **bagaimana cara menganalisis file mencurigakan tanpa membahayakan sistem kita?**

Kalau kita langsung jalankan file tersebut, ada resiko mesin kita ikut terinfeksi — kecuali kita punya _sandbox_ atau lingkungan yang benar-benar terisolasi seperti menggunakan _virtual machine_.

Secara umum, ada dua pendekatan analisis:

| Metode | Cara Kerja | Risiko |
| ------ | ---------- | ------ |
| **Dynamic Analysis** | Menjalankan file lalu mengamati perilakunya secara real-time | Butuh _sandbox_, risiko infeksi jika tidak terisolasi |
| **Static Analysis** | Membedah file **tanpa menjalankannya** — cukup melihat isi dan strukturnya | Lebih aman, tapi butuh skill reverse engineering |

disini kita akan fokus pada **Static Analysis** menggunakan tool bernama **CAPA**.

### What is CAPA?

**CAPA** (_Common Analysis Platform for Artifacts_) adalah tool yang dikembangkan oleh tim **FireEye Mandiant**. Fungsinya sederhana tapi powerful: **mengidentifikasi kemampuan (capabilities) yang dimiliki sebuah file executable**.

Analoginya begini: bayangkan kamu menemukan kotak alat asing di jalan. Daripada membuka dan mencoba setiap alat satu per satu (yang bisa berbahaya), CAPA seperti _X-ray scanner_ yang bisa melihat isi kotak itu dari luar dan memberi tahu kamu: "Di dalam ada obeng, pisau, dan kunci inggris."

CAPA bekerja dengan cara:
- Menganalisis file (tanpa menjalankannya)
- Mencocokkan pola di dalam file dengan **kumpulan rules** yang mendeskripsikan perilaku umum
- Memberikan laporan tentang apa saja yang **mampu dilakukan** file tersebut

**Jenis file yang bisa dianalisis:**
- **PE** (Portable Executables) — file `.exe` dan `.dll` di Windows
- **ELF** binaries — executable di Linux
- **.NET modules**
- **Shellcode**
- Bahkan **laporan sandbox**

**Contoh kemampuan yang bisa dideteksi:**
- _Network communication_ (file ini bisa berkomunikasi ke internet)
- _File manipulation_ (file ini bisa membuat/menghapus file lain)
- _Process injection_ (file ini bisa menyuntikkan kode ke proses lain)

### Kenapa CAPA Penting?

Keindahan CAPA adalah dia **mengemas pengalaman bertahun-tahun reverse engineering** ke dalam satu tool otomatis. Artinya, kamu tidak perlu menjadi ahli reverse engineering untuk bisa memahami kemampuan sebuah malware. Ini sangat berguna untuk:
- **Malware Analyst** yang butuh analisis cepat
- **Threat Hunter** yang ingin memahami kapabilitas binary tanpa harus membongkar assembly-nya
- **Incident Responder** yang butuh jawaban cepat: "File ini bisa ngapain aja?"

---

### MITRE ATT&CK

**MITRE ATT&CK** (_Adversarial Tactics, Techniques, and Common Knowledge_) adalah framework global yang mendokumentasikan **taktik dan teknik** yang digunakan oleh _threat actors_ di setiap tahap serangan cyber. Anggap saja ini sebagai **buku panduan strategis** yang menjelaskan metode penyerang — mulai dari _gaining initial access_, _escalating privileges_, _evading defenses_, _moving laterally_, hingga _data exfiltration_.

CAPA memetakan hasilnya ke format ATT&CK. Berikut cara membaca formatnya:

**Format dasar:**

```
ATT&CK Tactic::ATT&CK Technique::Technique Identifier
```

| Format | Contoh | Penjelasan |
| ------ | ------ | ---------- |
| `Tactic::Technique::ID` | `Defense Evasion::Obfuscated Files or Information::T1027` | **Defense Evasion** = Taktik, **Obfuscated Files or Information** = Teknik, **T1027** = ID Teknik |
| `Tactic::Technique::Sub-Technique::ID.Sub-ID` | `Defense Evasion::Obfuscated Files or Information::Indicator Removal from Tools T1027.005` | Sama seperti di atas + **Indicator Removal from Tools** = Sub-Teknik, **005** = Sub-ID |

> Tidak semua output CAPA akan memiliki Sub-Technique. Beberapa hanya menampilkan sampai level Technique saja.

---
