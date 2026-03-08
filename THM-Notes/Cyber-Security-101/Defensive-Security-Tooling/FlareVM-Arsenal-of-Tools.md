# TryHackMe: FlareVM Arsenal of Tools

- **Room Link:** [FlareVM Arsenal of Tools](https://tryhackme.com/room/flarevmarsenaloftools)
- **Category:** Defensive Security Tooling
- **Difficulty:** Easy

## Introduction

Kalau di materi sebelumnya kita sudah bahas **REMnux** sebagai meja operasi *malware* berbasis Linux, sekarang saatnya kita kenalan sama saudara kandungnya di dunia Windows: **FlareVM**.

**FlareVM** (singkatan dari *Forensics, Logic Analysis, and Reverse Engineering*) adalah seperangkat kotak perkakas khusus rakitan tim FLARE dari **FireEye**. Isinya sudah dipenuhi ratusan program spesialis yang murni diciptakan buat ngebantu *reverse engineers*, analis *malware*, dan investigator forensik. Ibaratnya, kalau kamu mau menguliti sistem Windows atau ngebongkar wujud asli (*reverse engineer*) dari sebuah virus `.exe`, ini VM andalan.

### Learning Objectives

- Mengenal dan mengeksplorasi isi kotak perkakas di dalam FlareVM.
- Praktik langsung pakai berbagai *tools* bawaannya untuk menganalisis proses/*process* mencurigakan di memori.
- Familiar sama *tools* yang biasa dipakai buat investigasi statis (*static analysis*) dokumen jahat atau *file* biner (*binaries*).

---

## Arsenal of Tools (Persenjataan FlareVM)

Di dalam FlareVM, *tools*-nya dikelompokkan berdasarkan fungsinya biar kamu lebih gampang nyarinya saat lagi menangani kasus.

### 1. Reverse Engineering & Debugging
*Reverse engineering* itu ibarat kamu membongkar sebuah mesin atau barang elektronik (seperti jam mekanik atau radio) yang sudah jadi pabrikan. Tujuannya murni untuk mencari tahu komponen apa saja yang ada di dalamnya dan bagaimana cara alat itu bekerja. Sedangkan *debugging* adalah proses saat alat itu rusak, lalu kamu menganalisis bagian mana yang macet dan mencoba memperbaikinya.

Senjata andalan di kategori ini:
- **Ghidra**: *Software reverse engineering* high class yang dirilis gratis oleh NSA (Badan Intelijen Amerika).
- **x64dbg**: *Debugger* *open-source* favorit untuk membedah *file binaries* format 32-bit maupun 64-bit.
- **OllyDbg**: *Debugger* klasik yang sangat handal membedah instruksi program sampai ke level bahasa *Assembly* (bahasa mesin tingkat rendah).
- **Radare2**: *Platform reverse engineering open-source* berbasis teks yang sangat ringan dan bertenaga.
- **Binary Ninja**: *Tool* visual yang rapi untuk membantu membedah (*disassembling*) dan menyusun ulang (*decompiling*) kode biner.
- **PEiD**: Alat pendeteksi untuk mencari tahu apakah sebuah program itu dibungkus pakai pelindung kode (*packer*, *cryptor*) atau dicetak pakai perangkat lunak pembuat aplikasi (*compiler*) jenis apa.

### 2. Disassemblers & Decompilers
Dua jenis *tools* ini sangat penting dalam analisis *malware*. Tugas utamanya ibarat **Penerjemah Bahasa**, komputer berkomunikasi menggunakan angka dan bahasa mesin yang tidak bisa dibaca manusia biasa. Alat ini bertugas menerjemahkan surat rahasia tersebut ke dalam format logika (seperti kode *C++* atau *Assembly*) yang lebih mudah dipahami oleh analis.

Alat yang sering dipakai:
- **CFF Explorer**: Editor khusus (*PE editor*) untuk menganalisis dan membongkar struktur *file* berformat *Portable Executable* (khas Windows seperti `.exe` atau `.dll`).
- **Hopper Disassembler**: Paket komplit yang bisa merangkap jadi *debugger*, *disassembler*, sekaligus *decompiler*.
- **RetDec**: *Decompiler open-source* tangguh buat menerjemahkan kembali bahasa mesin agar lebih gampang dibaca.

### 3. Static & Dynamic Analysis
Ada dua metode utama dalam menginvestigasi sebuah *malware*. Ibarat pos satpam yang sedang memeriksa paket kotak mencurigakan:
> - **Static Analysis:** Membedah wujud fisik, tulisan pengirim, dan isi kotak tersebut menggunakan alat *X-Ray* **tanpa** membuka kotaknya secara langsung (metode aman).
> - **Dynamic Analysis:** Mengambil paket tersebut, memasukkannya ke dalam ruangan isolasi tebal (*sandbox*), lalu sengaja **membuka dan memicunya** untuk melihat efek apa yang terjadi pada ruangan tersebut (metode tes langsung).

Alat pemantau andalannya:
- **Process Hacker**: Versi Task Manager yang super canggih. Sangat informatif untuk memantau program apa saja yang sedang berjalan (*process watcher*) dan melihat penggunaan memori sistem.
- **PEview**: Alat ringan untuk mengintip isi dari komponen pembangun *file* `.exe` (berjenis PE).
- **Dependency Walker**: Alat yang digunakan untuk melihat daftar *file library* pendukung (DLL bawaan Windows) apa saja yang diam-diam "dipanggil" dan dibutuhkan oleh suatu aplikasi.
- **DIE (Detect It Easy)**: Alat serbaguna untuk mendeteksi apakah suatu *file* dilindungi oleh pengunci (*packer*, *cryptor*) dan alat pembuat (*compiler*) tertentu.

### 4. Forensics & Incident Response
Kategori ini ibarat tim forensik polisi (*CSI*) yang turun ke tempat kejadian perkara (TKP). *Digital Forensics* bertugas mengumpulkan, menganalisis, dan mengamankan barang bukti (dari *harddisk*, jaringan, sampai rekaman RAM memori). Sementara *Incident Response* fokus pada pertolongan pertama: mendeteksi, mengisolasi, membasmi ancaman pencuri (*hacker/malware*), dan memulihkan sistem yang rusak.

Alat spesialis forensik:
- **Volatility**: *Framework* wajib untuk menganalisis dan membongkar barang bukti berupa rekaman memori (*RAM dump*).
- **Rekall**: *Framework* alternatif untuk forensik memori, biasanya sangat berguna saat tim sedang melakukan respon insiden.
- **FTK Imager**: Alat standar industri forensik untuk mengkloning (membuat *image*) isi *harddisk* mentah-mentah secara aman untuk dianalisis.

### 5. Network Analysis
Ini dia alat penyadap jaringannya analis cyber. Ibarat memasang CCTV dan alat penyadap telepon di seluruh rumah, alat-alat ini membantu kamu memeriksa aliran pola data yang keluar-masuk di kabel jaringan.

Alat pemantau jaringan:
- **Wireshark**: Mikroskop jaringan paling populer di dunia. Bisa merekam dan membedah percakapan data (protokol) antar komputer secara *live*.
- **Nmap**: Pemindai jaringan legendaris pemetaan komputer dan mencari tahu pintu (*port*) mana saja yang terbuka dan rentan diserang.
- **Netcat**: Tool Swiss Army Knife dunia jaringan. Bisa dipakai untuk membaca dan menulis data kosong ke dalam koneksi jaringan, sangat berguna untuk tes sederhana.

### 6. File Analysis
Ibarat meneliti sidik jari dan komposisi kimiawi darah di lab, teknik ini murni digunakan untuk meneliti susunan komponen sebuah *file* dari dasar untuk mencari bibit ancaman keamanan di dalamnya.

Alat bedah *file*:
- **FileInsight**: Program spesialis untuk membaca dan mengedit *file* biner (kumpulan angka bahasa komputer).
- **Hex Fiend**: *Hex editor* (editor yang menampilkan angka dasar heksadesimal komputer) yang sangat ringan dan cepat.
- **HxD**: *Hex editor* andalan sejuta umat untuk melihat dan mengedit bahasa mentah biner dari sebuah *file*.

### 7. Scripting & Automation
Analisis manual itu capek dan rentan salah (karena *human error*). Kategori ini ibarat kamu membuat lengan robot pabrik untuk menjalankan tugas-tugas receh dan berulang secara otomatis, sehingga kinerja analis jadi jauh lebih cepat.

Alat otomasi andalan:
- **Python**: Bahasa pemrograman favorit analis keamanan. Fokus utamanya dipakai untuk menjalankan modul dan tools otomatis.
- **PowerShell Empire**: Kerangka kerja (*framework*) berbasis PowerShell yang biasa dipakai *hacker*/*tester* untuk aktivitas setelah penyusupan (mempertahankan akses ke komputer korban).

### 8. Sysinternals Suite
Ini adalah kumpulan perkakas pisau beda khusus sistem operasi Windows. Alat-alat ini dirancang spesifik buat bantu tenaga profesional IT dan developer untuk mengelola, memperbaiki masalah (*troubleshoot*), dan mendiagnosa masalah di dalam Windows.

Tiga serangkai utamanya:
- **Autoruns**: Alat intelijen buat nyari tahu program *executables* apa saja yang diam-diam *dijalankan* secara otomatis ketika komputer baru pertama kali nyala (*boot-up*). Sangat vital buat nyari *malware* yang berada di sistem.
- **Process Explorer**: *Task Manager* yang super detail dan detail menyangkut proses-proses yang lagi hidup di komputer kamu.
- **Process Monitor**: Kamera pengawas *real-time*. Dia mencatat setiap tarikan napas dan pergerakan dari seluruh proses dan *thread* yang aktif di sistem tanpa ada yang lolos.

---

> **Bingung ngeliat daftarnya?**  
> Santai aja Kamu **nggak perlu** menghafal dan menguasai semua alat ini dalam semalam (bisa makan waktu berbulan-bulan). Tujuannya di sini cuma buat mengenalkan bahwa: *"Ada lho satu kotak berisi lengkap perkakas ajaib, jadi kamu tahu alat mana yang pas saat butuh ngerjain tugas spesifik nanti."*

## Commonly Used Tools for Investigation: Overview

Dari sekian banyak *tools* yang dibahas sebelumnya, cuma segelintir saja yang akan sering banget kamu pakai untuk investigasi tahap awal. Anggap ini adalah paket "P3K Utama" atau *starter pack* seorang analis.

Berikut adalah daftar dan fungsinya:

| Nama Tool | Nilai Investigasi (*Investigative Value*) |
| :--- | :--- |
| **Procmon** (*Process Monitor*) | Ahlinya melacak seluruh aktivitas sistem secara detail. Sangat handal untuk ngerjain riset *malware*, *troubleshooting* PC yang rusak, sampai nyari barang bukti digital (*forensic investigations*). |
| **Process Explorer** | Alat untuk melihat silsilah keluarga sebuah program (*Parent-child relationship*). Jadi ketahuan program mana yang melahirkan proses jahat tersebut, lengkap dengan lokasi *file library* (DLL) yang dimuat. |
| **HxD** | *Hex editor* andalan untuk memeriksa sampai mengedit struktur dasar sebuah *file* dari balik layar (level bahasa biner/heksadesimal). |
| **Wireshark** | Bertugas mirip pos pantau CCTV lalu lintas. Kerjanya mengobservasi pergerakan data di kabel jaringan buat nyari aktivitas komunikasi yang ganjil (*unusual activity*). |
| **CFF Explorer** | Sering dipakai untuk mengekstrak sidik jari digital (*file hashes*) demi mengecek integritas file, serta memastikan apakah sebuah file bawaan sistem itu asli atau udah disusupi. |
| **PEStudio** | *Tool* spesialis untuk urusan *Static Analysis*. Kamu bisa menguliti properti dan sifat-sifat mencurigakan dari sebuah aplikasi **tanpa harus** menjalankannya sama sekali (main aman). |
| **FLOSS** | Ahli pemeras teks. Dia bertugas mengekstrak dan memperjelas teks/kata-kata rahasia (*strings*) yang sengaja disamarkan (*obfuscated*) di dalam program jahat menggunakan teknik *static analysis* tingkat lanjut. |

> Tips: Kamu bisa ikutan ngebuka *tools*-nya satu-satu di dalam **FlareVM** biar terbiasa sama <i>interface</i> aslinya pas kita bahas cara kerjanya lebih dalam nanti.

### 1. Process Monitor (Procmon)

**Procmon** adalah alat super canggih bawaan Windows, Bayangkan **Procmon** sebagai **Dashcam Kamera Pelacak** beresolusi tinggi yang terpasang di jantung sistem operasi komputer kamu.

Tugas utamanya adalah membiarkan kamu **melihat, merekam, dan melacak** semua aktivitas *file* Windows secara *real-time*. Dia terus-menerus memantau dan mencatat tiga area vital:
1. Pergerakan *File System* (buka/tutup/edit *file*).
2. Perubahan *Registry* (buku catatan inti Windows).
3. Aktivitas *Thread/Process* (naik-turunnya program yang hidup).

Karena kemampuannya yang sangat detail, *tool* ini menjadi andalan untuk riset *malware*, *troubleshooting* aplikasi, hingga investigasi forensik digital.

**Contoh Investigasi Menggunakan Procmon:**

Mari kita lihat gambar layar dari hasil rekaman Procmon di bawah ini:

![Procmon Activity Log](../../Assets/Images/Procmon.png)

Berdasarkan baris rekaman tersebut, terlihat bahwa ada proses bernama `lsass.exe` yang sukses membaca *file* penting bernama `samsrv.dll` secara berulang kali.

**Apa itu LSASS?**
**LSASS** (*Local Security Authority Subsystem Service*) adalah pos satpam utama di Windows yang mengurus urusan autentikasi (pemeriksaan identitas dan *password* pengguna saat *login*). Karena dia memegang kunci, aplikasi wajar ini akan sering berkomunikasi dengan *file* sistem krusial lainnya (seperti `lsasrv.dll`).

**Lalu, di mana letak bahayanya?**
Meski `lsass.exe` adalah aplikasi resmi bawaan Windows, dia sering **dijadikan target vulnerability**. *Hacker* sangat suka menggunakan *tool* jahat seperti **Mimikatz** untuk menyerang dan menyedot memori LSASS demi mencuri *password* yang ada di sana (teknik ini disebut *Credential Dumping*).

**Mental Model Analis:**
Jadi, saat kamu melihat layar Procmon, insting utamamu adalah mencari **anomali (keganjilan)**. Jika kamu melihat aktivitas yang mencurigakan di sekitar proses LSASS misalnya ada program asing yang tiba-tiba membaca atau mencoba menulis data ke dalam memori `lsass.exe` itu adalah sinyal bahaya (bendera merah) yang harus segera kamu investigasi lebih dalam

*(Tenang saja, contoh gambar di atas hanyalah aktivitas sistem normal, tidak ada tanda-tanda infeksi malware).*

### 2. Process Explorer (Procexp)

**Process Explorer** adalah jenis alat yang memberikan analisis mendalam tentang semua program yang sedang aktif di komputermu. Kalau Procmon tadi adalah CCTV yang merekam aktivitas gerakan, maka **Process Explorer** adalah **Buku Induk Kependudukan** dan **Pohon Silsilah Keluarga** dari proses komputer.

Alat ini memungkinkanmu untuk:
- Melihat daftar lengkap program yang aktif beserta akun pengguna (*user accounts*) yang menjalankannya.
- Mencari tahu program mana yang saat ini sedang mengakses, membuka, atau mengunci sebuah *file* atau folder tertentu.
- Memeriksa asal-usul atau silsilah keluarga dari sebuah program (hubungan *parent-child*).

**Contoh Investigasi Menggunakan Process Explorer:**

Perhatikan gambar di bawah ini saat kita membuka aplikasi CFF Explorer:

![Process Explorer showing parent-child relationship](../../Assets/Images/Procexp.png)

Dari gambar di bawah ini, kamu bisa melihat **Silsilah Keluarga** (*Parent-child relationship*) dari prosesnya:
- `explorer.exe` (Wujud antarmuka layar *desktop* Windows) bertindak sebagai aplikasi **Induk** (*Parent*).
- Induk ini berada di tingkat atas, dan di bawahnya muncul sebuah cabang baru berisikan `CFF Explorer.exe` yang bertindak sebagai aplikasi **Anak** (*Child*).

Ini membuktikan bahwa aplikasi CFF Explorer tadi secara sah dibuka melalui layar *desktop* biasa oleh sang pengguna.

**Lalu, kenapa Silsilah Keluarga (*Parent-Process*) ini penting?**
Teknik ini sangat berguna untuk memantau apakah ada proses aneh yang mendadak dimunculkan **(spawned)** dari aplikasi yang tidak seharusnya. Pelaku kejahatan cyber (*threat actors*) sangat sering menyalahgunakan dokumen kantoran (seperti Word Document, *file* LNK, atau ISO) untuk menyebarkan ancaman.

**Mental Model Analis:**
Ketika kamu curiga, lihat *parent process*-nya Sebagai contoh, sangatlah wajar jika aplikasi *Microsoft Word* (*Parent*) membuka dokumen teks (*Child*). Tapi akan menjadi **sangat tidak wajar dan berbahaya** jika *Microsoft Word* (*Parent*) tiba-tiba secara diam-diam membuka aplikasi *Command Prompt* atau *PowerShell* (*Child*) di belakang layar. Itulah fungsi utama Process Explorer: melacak asal-usul lahirnya sebuah program jahat

### 3. HxD (Hex Editor)

Kalau *Process Explorer* tadi mengurus program yang lagi hidup, **HxD** adalah spesialis operasi bedah untuk *file* yang mati. Alat ini disebut *Hex Editor*, yang fungsinya ibarat **Mikroskop DNA**. Ia sanggup membedah struktur paling dasar penyusun sebuah *file* hingga ke level bongkahan angka primitif (*hexadecimal*). 

Dengan HxD, kamu bisa membaca, mencari, memulihkan, hingga memodifikasi data mentah biner secara sangat presisi.

**Contoh Investigasi Menggunakan HxD:**

Anggap saja analis kita menemukan *file* teks mencurigakan bernama `possible_medusa.txt`. Pas dibuka pakai *Notepad* biasa, bentuknya pasti cuma huruf acak-acakan kayak bahasa alien. Nah, lewat HxD, wujud aslinya akan ketahuan.

![HxD Hex Editor Inspection](../../Assets/Images/HxD.png)

Cara membaca *interface* HxD di atas:
- Ruangan sebelah **Kiri** menampilkan susunan kode DNA mentah dari *file* (kumpulan angka/huruf *Hexadecimal*).
- Ruangan sebelah **Tengah** mencoba menerjemahkan angka-angka DNA tadi menjadi bentuk teks *ASCII* (tulisan manusia) sebisanya.
- Ruangan sebelah **Kanan** adalah panel **Data Inspector**. Panel ini sangat krusial karena bertugas sebagai kalkulator pintar penyadur bahasa alien ke berbagai wujud tipe data manusia (apakah itu angka biasa, tanggal, atau nilai spesifik spesifik lainnya).

**Mental Model Analis - Number "4D 5A":**
Satu insting terpenting analis saat membuka sebuah *file* asing pakai Hex Editor adalah melihat dua pasang angka pertama di pojok kiri atas (*Header*). Pada gambar di atas, tertulis angka **`4D 5A`** (atau huruf "MZ" di sisi ASCII).

Ini adalah **Bendera Merah**, Angka `4D 5A` adalah kode sandi universal bawaan Microsoft Windows untuk menandakan bahwa *file* ini adalah **Aplikasi yang Bisa Dijalankan (*Executable* / `.exe`)**, terlepas dari apapun nama dan ekstensinya  

Jadi, walaupun *hacker*-nya pintar menyamarkan nama *file* menjadi `possible_medusa.txt` biar dikira tulisan *memo* biasa, HxD berhasil membongkar kedoknya bahwa *file* itu sejatinya siap dieksekusi sebagai virus. Jangan pernah ketipu sama tampilan luar nya ya
