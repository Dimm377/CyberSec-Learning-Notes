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

### MBC Behaviors

Kolom **MBC Behaviors** berisi daftar _behaviours_ dan _micro-behaviors_ beserta method dan identifier-nya.

| Objective | Behavior | ID | Penjelasan |
| --------- | -------- | -- | ---------- |
| **Anti-Behavioral Analysis** | Virtual Machine Detection | `B0009` | Malware memeriksa apakah dia berjalan di lingkungan virtual — mengecek user, artefak, dan informasi sistem |
| **Anti-Static Analysis** | Executable Code Obfuscation | `B0032` | Kode sengaja di-_obfuscate_ untuk menghindari static analysis, termasuk data dan text section-nya |
| **Execution** | Command and Scripting Interpreter | `E1059` | Malware mengeksploitasi interpreter seperti `cmd.exe`, `PowerShell`, `Bash`, `Python`, `Perl`, atau `JavaScript` untuk menjalankan perintah |
| **Discovery** | File and Directory Discovery | `E1083` | Malware mencari file spesifik di lokasi tertentu dengan cara meng-enumerate file dan direktori |
| **Anti-Static Analysis, Defense Evasion** | Obfuscated Files or Information | `E1027` | Malware meng-_obfuscate_ file/informasi dengan encoding, enkripsi, atau metode lain agar sulit dianalisis |

### Micro-Behavior

Istilah "low-level behaviors" dalam analisis malware merujuk pada aksi yang **belum tentu berbahaya** dan bisa ditemukan di berbagai jenis software biasa. Perilaku ini disebut "micro-behaviors" dalam _Malware Behavior Characteristics_ (MBC). Contohnya termasuk pembuatan TCP socket dan evaluasi kondisi tertentu di dalam string.

> Penting: **perilaku yang dikategorikan low-level bukan berarti tidak berbahaya** — ia tetap bisa menjadi bagian dari rantai serangan malware yang lebih besar.

| Micro-Objective | Micro-Behavior | ID | Penjelasan |
| --------------- | -------------- | -- | ---------- |
| **MEMORY** | Allocate Memory | `C0007` | Malware sering mengalokasikan memori sebagai strategi untuk _unpack_ dirinya sendiri dan menjalankan kode berbahaya |
| **PROCESS** | Create Process | `C0017` | Malware membuat proses baru via WMI atau shellcode, bisa juga membuat _suspended process_ |
| **COMMUNICATION** | HTTP Communication | `C0002` | Malware mampu menginisiasi komunikasi HTTP |
| **DATA** | Check String | `C0019` | Malware memeriksa string untuk mengidentifikasi karakteristik spesifik seperti konten ASCII, nomor kartu kredit, dan panjang string |
| **DATA** | Encode Data | `C0026` | Malware mampu meng-encode data menggunakan Base64 dan XOR |
| **FILE SYSTEM** | Create Directory | `C0046` | Malware bisa membuat direktori baru |
| **FILE SYSTEM** | Delete File | `C0047` | Malware bisa menghapus file |
| **FILE SYSTEM** | Read File | `C0051` | Malware bisa membaca file |
| **FILE SYSTEM** | Writes File | `C0052` | Malware bisa menulis ke file |

> Output final CAPA menampilkan Behavior dan Micro-Behavior di bawah satu kolom yang sama yaitu kolom **Behavior**.

---

### Methods

Terakhir, ada **Methods** — detail paling spesifik dari sebuah behavior. Methods terikat ke behavior tertentu; untuk melihat semua method yang tersedia, kita perlu merujuk ke dokumentasi tiap behavior/micro-behavior.

| Behavior | Method (Sub-Technique) | ID | Penjelasan |
| -------- | ---------------------- | -- | ---------- |
| Executable Code Obfuscation | **Argument Obfuscation** | `B0032.020` | Argumen sederhana ke API calls dikalkulasi saat _runtime_, mempersulit analisis |
| Executable Code Obfuscation | **Stack Strings** | `B0032.017` | String dibangun dan didekripsi di stack saat digunakan, lalu dibuang untuk menghindari referensi yang jelas |
| HTTP Communication | **Read Header** | `C0002.014` | Membaca HTTP header |
| Encode Data | **Base64** | `C0026.001` | Malware bisa meng-encode data menggunakan Base64 |
| Encode Data | **XOR** | `C0026.002` | Malware bisa menggunakan XOR untuk meng-encode data |
| Obfuscated Files or Information | **Encoding-Standard Algorithm** | `E1027.m02` | Meng-encode sample malware, file, atau informasi lain menggunakan algoritma standar (misalnya Base64) |

### Recap: Cara Membaca Output MBC

Contoh output dari CAPA:

```
MBC Objective: DATA
MBC Behavior:  Encode Data::Base64 [C0026.001]
```

Cara membacanya:

| Label | Value | Penjelasan |
| ----- | ----- | ---------- |
| **MBC Objective** | `DATA` | Perilaku terkait data: checking strings, compressing, decoding, dan encoding data |
| **MBC Behavior** | `Encode Data` | Malware mampu meng-encode data menggunakan Base64 dan XOR |
| **Method** | `Base64` | Secara spesifik, malware meng-encode data menggunakan Base64 |
| **Identifier** | `C0026.001` | ID unik yang merelasikan informasi tentang behavior ini — berfungsi juga sebagai tag referensi |

**Kesimpulan:** dari informasi ini, kita bisa langsung menyimpulkan bahwa file tersebut **menggunakan skema encoding Base64**.

---
