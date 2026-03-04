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
## Dissecting CAPA Results Part 1: General Information, MITRE and MAEC

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

---

**Contoh output MITRE ATT&CK dari CAPA:**

| ATT&CK Tactic | ATT&CK Technique |
| -------------- | ----------------- |
| **DEFENSE EVASION** | Obfuscated Files or Information `[T1027]` |
| | Obfuscated Files or Information::Indicator Removal from Tools `[T1027.005]` |
| | Virtualization/Sandbox Evasion::System Checks `[T1497.001]` |
| **DISCOVERY** | File and Directory Discovery `[T1083]` |
| **EXECUTION** | Command and Scripting Interpreter::PowerShell `[T1059.001]` |
| | Shared Modules `[T1129]` |
| **IMPACT** | Resource Hijacking `[T1496]` |
| **PERSISTENCE** | Scheduled Task/Job::At `[T1053.002]` |
| | Scheduled Task/Job::Scheduled Task `[T1053.005]` |

Dengan memetakan perilaku file ke MITRE ATT&CK, analis atau defender bisa **menyempitkan cakupan investigasi** selama insiden — karena sudah tahu apa saja yang _mungkin_ dilakukan file tersebut berdasarkan _playbook_ penyerang.

### MAEC

**MAEC** (_Malware Attribute Enumeration and Characterization_) adalah bahasa standar yang dirancang untuk meng-_encode_ dan mengkomunikasikan detail kompleks tentang malware. MAEC mencakup berbagai atribut: _behaviours_, _artefacts_, dan _interconnections_ antar berbagai sample malware. Fungsinya sebagai **sistem standar untuk melacak dan menganalisis** kerumitan suatu malware.

**Contoh output MAEC dari CAPA:**

| MAEC Category | MAEC Value |
| ------------- | ---------- |
| `malware-category` | `launcher` |
| `downloader` | `Emotet` |

Dua nilai (values) MAEC yang paling sering ditemukan oleh CAPA adalah **Launcher** dan **Downloader**.

| MAEC Value | Keterangan |
| ---------- | ---------- |
| **Launcher** | Menunjukkan perilaku yang memicu jalannya program lain, mirip dengan cara kerja malware. |
| **Downloader** | Menunjukkan perilaku mengambil (download) dan menjalankan file dari luar. Biasanya ditemukan pada malware yang lebih kompleks. |

#### Launcher
Jika CAPA menandai sebuah file sebagai `launcher`, artinya file tersebut menunjukkan behavior seperti (tapi tidak terbatas pada):
- **Dropping additional payloads:** Menjatuhkan/menyimpan file berbahaya lain ke dalam sistem.
- **Activating persistence mechanisms:** Memasang _backdoor_ agar tetap bisa diakses meskipun sistem di-_restart_.
- **Connecting to C2 servers:** Menghubungi _Command-and-Control server_ untuk menerima perintah dari penyerang.
- **Executing specific functions:** Menjalankan fungsi atau perintah tertentu secara langsung.

#### Downloader
Jika CAPA memberikan tag `downloader`, artinya file tersebut menunjukkan behavior seperti:
- **Fetching additional payloads:** Mengunduh file malware sekunder atau _resource_ tambahan langsung dari internet.
- **Pulling in updates:** Mengambil kode atau instruksi terbaru dari server luar.
- **Executing secondary stages:** Menjalankan tahapan infection berikutnya (stage 2).
- **Retrieving configuration files:** Mengambil file konfigurasi yang akan mendikte bagaimana malware tersebut harus beroperasi.
---

## Dissecting CAPA Results Part 2: Malware Behavior and Catalogue

### Malware Behavior Catalogue (MBC)

**MBC** dirancang untuk mendukung berbagai aspek analisis malware — seperti _labelling_, _similarity analysis_, dan _standardized reporting_. Intinya, MBC adalah **katalog objektif dan perilaku malware**.

MBC bisa dihubungkan ke metode ATT&CK dan mencatat semua _behaviours_ serta _code features_ yang ditemukan saat analisis malware. Perlu dicatat: **nama-nama perilaku di MBC bisa berbeda dari ATT&CK** — informasi di MBC **melengkapi** (bukan menduplikasi) konten yang ada di ATT&CK.

**Contoh output MBC dari CAPA:**

| MBC Objective | MBC Behavior |
| ------------- | ------------ |
| **ANTI-BEHAVIORAL ANALYSIS** | Virtual Machine Detection `[B0009]` |
| **ANTI-STATIC ANALYSIS** | Executable Code Obfuscation::Argument Obfuscation `[B0032.020]` |
| | Executable Code Obfuscation::Stack Strings `[B0032.017]` |
| **COMMUNICATION** | HTTP Communication `[C0002]` |
| | HTTP Communication::Read Header `[C0002.014]` |
| **DATA** | Check String `[C0019]` |
| | Encode Data::Base64 `[C0026.001]` |
| | Encode Data::XOR `[C0026.002]` |
| **DEFENSE EVASION** | Obfuscated Files or Information::Encoding-Standard Algorithm `[E1027.m02]` |
| **DISCOVERY** | File and Directory Discovery `[E1083]` |
| **EXECUTION** | Command and Scripting Interpreter `[E1059]` |
| **FILE SYSTEM** | Create Directory `[C0046]`, Delete File `[C0047]`, Read File `[C0051]`, Writes File `[C0052]` |
| **MEMORY** | Allocate Memory `[C0007]` |
| **PROCESS** | Create Process `[C0017]` |

**Format output MBC di CAPA:**

MBC memiliki dua format representasi:

| Format | Contoh | Penjelasan |
| ------ | ------ | ---------- |
| `OBJECTIVE::Behavior::Method[Identifier]` | `ANTI-STATIC ANALYSIS::Executable Code Obfuscation::Argument Obfuscation [B0032.020]` | **Anti-static Analysis** = Objective, **Executable Code Obfuscation** = Behavior, **Argument Obfuscation** = Method, **B0032.020** = Identifier |
| `OBJECTIVE::Behavior::[Identifier]` | `COMMUNICATION::HTTP Communication:: [C0002]` | **Communication** = Objective, **HTTP Communication** = Behavior, **C0002** = Identifier |

Perbedaan antara kedua format: format pertama memiliki detail tambahan berupa **Method**, yang bisa dianggap sebagai _sub-technique_.

**Istilah kunci yang harus dipahami:**

| Istilah | Arti |
| ------- | ---- |
| **Objective** | Tujuan utama dari perilaku malware (mirip "Tactic" di ATT&CK) |
| **Behavior** | Tindakan spesifik yang dilakukan malware untuk mencapai objective (mirip "Technique") |
| **Method** | Detail lebih lanjut dari sebuah behavior — cara spesifik yang dipakai (mirip "Sub-Technique") |

---

### Objective

**Objective** di MBC didasarkan pada **taktik ATT&CK dalam konteks perilaku malware** — namun tidak semuanya disertakan. MBC juga punya objective tambahan seperti _Anti-Behavioral_ dan _Anti-Static Analysis_ yang dirancang khusus untuk **karakterisasi malware**.

| Objective | Penjelasan |
| --------- | ---------- |
| **Anti-Behavioral Analysis** | Malware mencoba menghindari deteksi dengan menghalangi analisis perilaku menggunakan tools seperti _sandbox_ atau _debugger_ |
| **Anti-Static Analysis** | Malware menambah kompleksitas pada _static analysis_, sehingga mempersulit analis untuk memahami perilaku dan niat malware tersebut |
| **Collection** | Malware fokus mengidentifikasi dan mengumpulkan informasi dari mesin atau jaringan target |
| **Command and Control** | Malware membangun komunikasi dengan _C2 server_, _peer-to-peer network_, atau metode lain untuk menerima perintah, mengeksfiltrasi data, atau menjalankan aksi berbahaya |
| **Credential Access** | Malware bertujuan mencuri credential akun seperti _username_ dan _password_ |
| **Defense Evasion** | Malware berusaha melewati dan mengelabui berbagai mekanisme deteksi dan keamanan dalam sistem agar tidak terdeteksi |

### Micro-Objective

**Micro-objective** berkaitan dengan **micro-behaviors** — yaitu aksi yang dilakukan oleh software yang **belum tentu berbahaya** secara inheren dan bisa ditemukan di berbagai jenis aplikasi (contoh: aplikasi messaging). Namun, perilaku-perilaku ini **sering disalahgunakan** oleh malware — itulah kenapa CAPA tetap menandainya.

| Micro-Objective | Penjelasan |
| --------------- | ---------- |
| **PROCESS** | Perilaku terkait proses: Creating Process, Setting Thread Context, Terminating Process, Checking Mutex |
| **MEMORY** | Perilaku terkait memori: Allocating Memory, Changing Memory Protection, Freeing Memory |
| **COMMUNICATION** | Perilaku terkait jaringan: DNS, FTP, HTTP, ICMP, SMTP network traffic |
| **DATA** | Perilaku terkait data: Checking strings, compressing, decoding, dan encoding data |

> Output final CAPA menampilkan Objective dan Micro-Objective di bawah satu kolom yang sama yaitu kolom **Objective**.

---
