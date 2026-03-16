# TryHackMe: Security Principles

* **Room Link:** [Security Principles](https://tryhackme.com/room/securityprinciples)
* **Category:** Cyber Security 101 : Build Your Cyber Security Career
* **Difficulty:** Easy

---

### Learning Objectives
Di *room* ini, kita akan membedah pondasi utama cybersecurity, yaitu:
* Memahami fungsi keamanan: **CIA Triad** (*Confidentiality, Integrity, Availability*).
* Mengenal kebalikan dari CIA, yaitu **DAD Triad** (*Disclosure, Alteration, Destruction/Denial*).
* Mengenal model keamanan dasar seperti **Bell-LaPadula**.
* Mempelajari prinsip keamanan penting: **Defense-in-Depth**, **Zero Trust**, dan **Trust but Verify**.
* Mengenal standar **ISO/IEC 19249**.
* Memahami perbedaan antara **Vulnerability**, **Threat**, dan **Risk**.

---

## 1. Introduction

Saat ini, kata **Security** atau keamanan sudah menjadi semacam **Istilah populer** di perusahaan teknologi. Hampir semua perusahaan berani mengklaim bahwa layanan atau produk mereka itu aman. Tapi pertanyaannya: **Aman buat siapa?**

Sebelum kita masuk ke teori-teori prinsip keamanan yang kompleks, hal pertama yang wajib kamu pahami adalah **siapa lawan yang sedang kamu hadapi**. 

### Balita vs Mata-mata Industri

Bayangkan kamu sedang menjaga sebuah laptop, strategi keamanan yang kamu berikan akan sangat bergantung pada siapa lawan yang sedang kamu hadapi:

1. **Kasus A (Lawan Balita):** Kamu cuma mau mencegah seorang balita (anak kecil) supaya tidak asal pencet *keyboard* dan menghapus tugas sekolahmu. 
* *Solusi:* Cukup ditaruh di tempat tinggi atau pakai *password* sederhana saja sudah sangat aman.
2. **Kasus B (Lawan Mata-mata):** Laptop kamu berisi desain rahasia teknologi senilai miliaran rupiah, dan targetnya adalah **Mata-mata Industri** yang profesional.
* *Solusi:* Memakai cara yang sama seperti kasus A tentu sudah masuk kategori konyol/tidak cukup, di sini kamu butuh **Appropriate Security Controls** (kontrol keamanan yang sesuai porsinya), seperti:
* Enkripsi data tingkat tinggi (biar datanya nggak bisa dibaca kalau dicuri).
* Authentikasi berlapis (*Multi-Factor Authentication*) menggunakan biometric atau kunci fisik.
* Tim **Security Operations Center (SOC):** Seperti memiliki tim detektif yang memantau laptopmu 24 jam sehari dari kejauhan, mereka akan langsung bereaksi jika ada pergerakan atau akses yang tidak wajar.
     
     

### 100% Security Is a Myth

Satu prinsip yang paling keras di dunia *cyber security* adalah: **Tidak ada sistem yang 100% aman**

Sistem sekuat apa pun pasti punya celah, entah itu di kodenya, di orang yang mengelolanya, atau di perangkat kerasnya. Karena itulah, tujuan kita bukan membangun **"sistem yang tidak bisa dibobol"**, melainkan memperkuat **Security Posture** (kondisi keamanan kita).

Tujuannya? Untuk membuat biaya dan waktu yang harus dikeluarkan oleh lawan (*Adversary*) menjadi sangat mahal dan menyulitkan, sehingga mereka berpikir ulang untuk menyerang.

**Gembok Rumah vs Maling Pro**
Bayangkan rumahmu, kamu bisa pasang pagar tinggi, CCTV, kabel berduri, hingga satpam 24 jam
* **Apakah rumahmu 100% aman?** Tidak, kalau ada agen rahasia sekelas *Mission Impossible* pakai helikopter dan alat tercanggih di dunia, mereka tetap bisa masuk.
* **Lalu untuk pasang itu semua?** Supaya maling biasa menyerah saat melihat pertahananmu yang rumit, lalu dia memilih pindah ke rumah tetangga yang pagarnya terbuka lebar.

**Inti Pembelajaran:**
Sangatlah lucu kalau kamu menerapkan cara pengamanan **balita** untuk menghadapi **mata-mata profesional**. Karena itulah, **mengenal lawan (Adversary)** adalah langkah wajib supaya kita bisa mempelajari pola serangan mereka dan menerapkan **Kontrol Keamanan** yang memang sesuai porsinya.

## The CIA Triad

Sebelum kita mengeklaim suatu sistem itu "aman", kita perlu membedah apa saja unsur yang membentuk keamanan tersebut. Di dunia *cybersecurity*, ada tiga tiang utama yang menjadi standar penilaian, yaitu **CIA Triad**:

### 1. Confidentiality (Kerahasiaan)
Menjamin bahwa data hanya bisa diakses oleh orang yang memang berhak.

* **Contoh 1: Resep Rahasia Perusahaan.** Perusahaan seperti KCF atau Coca-Cola punya resep rahasia yang cuma diketahui segelintir orang, Kalau resepnya bocor ke saingan, mereka rugi besar.
* **Contoh 2: Chat/DM Pribadi.** Saat kamu kirim pesan curhat ke teman lewat aplikasi chat, pesan tersebut di-enkripsi agar cuma kamu dan temanmu yang bisa baca. Pihak lain (termasuk hacker atau penyedia layanan) tidak boleh tahu isinya.

### 2. Integrity (Integritas)
Menjamin bahwa data tidak bisa diubah secara tidak sah, dan kita bisa mendeteksi jika ada perubahan yang terjadi.

* **Contoh 1: Saldo Rekening Bank.** Bayangkan kamu punya saldo 100 miliar, tiba-tiba besok berubah jadi 100 juta tanpa ada transaksi apa pun, Itu artinya integritas datanya dirusak, Saldo harus tetap benar dan tidak boleh berubah secara tidak sah.
* **Contoh 2: Isi Surat Keputusan (SK).** Jika isi sebuah surat keputusan resmi pemerintah diubah satu kata saja (misal dari "Ditolak" jadi "Disetujui") oleh pihak luar, maka seluruh sistem birokrasi bisa kacau.

### 3. Availability (Ketersediaan)
Menjamin bahwa sistem atau layanan selalu siap digunakan kapan pun dibutuhkan oleh pengguna yang sah.

* **Contoh 1: Nomor Darurat (112).** Saat ada kebakaran atau keadaan darurat, nomor telepon darurat harus bisa dihubungi setiap detik, Kalau sistem teleponnya mati, nyawa orang bisa jadi taruhannya.
* **Contoh 2: Layanan Video Streaming.** Bayangkan malam minggu kamu sedang mau menonton film favorit, tapi aplikasinya terus-menerus *loading* atau servernya *down*, Pengguna pasti merasa dirugikan karena layanan yang mereka bayar tidak tersedia saat dibutuhkan.

---

### Penting: Skala Prioritas CIA
Tiga unsur di atas tidak selalu memiliki beban prioritas yang sama. Tergantung pada apa yang sedang kita amankan, terkadang salah satu unsur jauh lebih penting dibanding yang lain.

**Contoh: Pengumuman Kelulusan Kampus**
* **Confidentiality:** Tidak terlalu penting, karena pengumumannya memang bersifat publik.
* **Integrity:** **Sangat Kritis.** Jangan sampai nama orang yang tidak lulus diubah menjadi lulus di daftar pengumuman.
* **Availability:** Cukup penting agar semua mahasiswa bisa melihat hasilnya tanpa kendala teknis.

---

## Authenticity & Non-repudiation

Selain tiga pilar CIA, ada dua konsep tambahan yang sangat krusial agar sebuah sistem bisa dipercaya:

### 1. Authenticity (Keaslian)
Memastikan bahwa data atau transaksi benar-benar berasal dari sumber yang asli, bukan palsu atau hasil manipulasi.

* **Contoh:** Jika sebuah perusahaan menerima pesanan 1000 unit mobil, mereka harus yakin kalau pesanan itu benar-benar dari pembeli asli, bukan *prank* atau pesanan fiktif dari kompetitor.

### 2. Non-repudiation (Anti penyangkalan)
Memastikan bahwa pengirim data atau pelaku transaksi tidak bisa menyangkal perbuatannya di kemudian hari.

* **Contoh:** Masih di kasus pesanan 1000 mobil tadi. Setelah mobil dikirim, jangan sampai si pembeli bisa bilang, "Eh, saya nggak pernah pesan mobil itu kok". Sistem harus punya bukti kuat (seperti tanda tangan digital) yang membuat mereka tidak bisa mengelak.

---

## Parkerian Hexad

Pada tahun 1998, Donn Parker mengusulkan **Parkerian Hexad**, yaitu kumpulan enam elemen keamanan. Empat di antaranya sudah kita bahas (Availability, Integrity, Authenticity, Confidentiality). Mari kita bahas dua elemen sisanya:

### 1. Utility (Kegunaan)
Fokus pada seberapa bermanfaat informasi tersebut. Data mungkin ada, tapi kalau tidak bisa digunakan, nilainya nol (*no utility*).

* **Contoh:** Kamu punya laptop dengan penyimpanan terenkripsi. Kamu masih punya laptopnya, disk-nya utuh (masih ada *Availability*), tapi kalau kamu **kehilangan kunci dekripsinya**, datanya jadi tidak berguna. Kamu punya datanya, tapi tidak bisa dibaca.

### 2. Possession (Kepemilikan/Kendali)
Fokus pada perlindungan informasi dari pengambilan, penyalinan, atau pengendalian oleh pihak yang tidak sah.

* **Contoh 1:** Seorang penyerang mencuri *backup drive* (hardisk cadangan) milikmu. Meskipun kamu mungkin masih punya data aslinya di laptop, fakta bahwa orang lain memegang kendali atas salinan datamu berarti kamu telah kehilangan *Possession*.
* **Contoh 2:** *Ransomware*, Penyerang mengenkripsi datamu dan meminta tebusan. Meskipun datanya masih ada di komputermu, kamu kehilangan kendali untuk mengakses atau mengelolanya.

## The DAD Triad

Kalau CIA Triad adalah apa yang ingin kita **capai**, maka **DAD Triad** adalah kebalikannya—yaitu apa yang terjadi kalau keamanan kita **gagal**. DAD adalah singkatan dari *Disclosure, Alteration,* dan *Destruction*.

### 1. Disclosure (Pengungkapan)
Ini adalah lawan dari **Confidentiality**. Terjadi saat data rahasia terekspos ke pihak yang tidak berwenang.

Bayangkan kamu punya buku harian rahasia, lalu seseorang mencurinya dan memposting isinya di media sosial. Rahasiamu sudah terungkap (*disclosed*).

### 2. Alteration (Perubahan)
Ini adalah lawan dari **Integrity**. Terjadi saat data diubah secara tidak sah, sehingga kebenarannya tidak bisa lagi dipercaya.

Bayangkan kamu menulis cek senilai 1 juta rupiah, tapi seseorang menambahkan angka nol di belakangnya sehingga menjadi 10 juta. Nilai integritas cek tersebut sudah rusak karena adanya perubahan (*alteration*).

### 3. Destruction/Denial (Penghancuran/Penolakan)
Ini adalah lawan dari **Availability**. Terjadi saat data atau sistem dihancurkan atau dibuat tidak bisa diakses sama sekali.

Bayangkan kamu ingin masuk ke rumahmu, tapi seseorang mengelas pintu rumahmu atau membakar rumah tersebut. Layanan rumahmu sudah tidak tersedia lagi karena penghancuran (*destruction*).

---

### Studi Kasus: Rekam Medis Pasien

Mari kita satukan semuanya dengan contoh sistem rumah sakit:

* **Disclosure:** Hacker mencuri database riwayat penyakit pasien dan menjualnya di *dark web*. Pasien malu dan rumah sakit kehilangan kepercayaan karena kegagalan menjaga kerahasiaan.
* **Alteration:** Bayangkan betapa bahayanya jika hacker mengubah golongan darah pasien di database, Dokter bisa memberikan transfusi yang salah, dan ini bisa berakibat **fatal (kematian)** karena rusaknya integritas data.
* **Destruction/Denial:** Rumah sakit modern sekarang sudah *paperless* (semua digital). Jika hacker melakukan serangan **Ransomware** yang mengunci semua database, dokter tidak bisa melihat riwayat alergi obat pasien. Operasi bisa tertunda dan seluruh fasilitas lumpuh.

**Poin Penting:**
Menjaga keseimbangan antara CIA itu sangat sulit. Kalau kamu terlalu ekstrem menjaga kerahasiaan (*Confidentiality*), bisa-bisa akses datanya jadi sangat lambat dan susah (*Availability* terganggu). Implementasi keamanan yang baik adalah tentang mencari **titik tengah (balance)** yang pas.

## Fundamental Concepts of Security Models

Setelah kita paham pilar CIA, pertanyaannya: **Gimana cara kita membangun sistem yang menjamin pilar-pilar tersebut tetap kokoh?** Jawabannya adalah dengan menggunakan **Security Models**.

Di sini kita akan bahas tiga model dasar:

### 1. Bell-LaPadula Model

Model ini tujuannya cuma satu: Menjaga **Confidentiality** (Kerahasiaan). Model ini sangat kaku dan biasanya dipakai di lingkungan militer. 

Prinsip utamanya ada tiga:

1. **Simple Security Property:** Dikenal dengan istilah **"No Read Up"**. Artinya, subjek dengan level keamanan rendah dilarang membaca data yang level keamanannya lebih tinggi. (Anak magang nggak boleh baca dokumen rahasia Direktur).
2. **Star Security Property ($\star$):** Dikenal dengan istilah **"No Write Down"**. Artinya, subjek dengan level keamanan tinggi dilarang menulis/membocorkan informasi ke level yang lebih rendah. (Direktur dilarang menulis rahasia perusahaan di grup chat karyawan umum).
3. **Discretionary-Security Property:** Menggunakan *Access Matrix* untuk menentukan siapa boleh baca/tulis apa.

| Subjects | Object A | Object B |
| :--- | :--- | :--- |
| Subject 1 | Write | No access |
| Subject 2 | Read/Write | Read |

* **Ringkasan:** **"Write Up, Read Down"**. Kamu boleh berbagi info ke level atas, tapi cuma boleh baca info dari level bawah/setara.
* **Kelemahan:** Model ini tidak dirancang untuk urusan berbagi file (*file sharing*) yang fleksibel.

---

### 2. Biba Model

Kalau Bell-LaPadula fokus ke kerahasiaan, **Biba Model** fokus ke **Integrity** (Integritas). Tujuannya biar data nggak dikotori atau dimanipulasi oleh pihak yang nggak punya otoritas.

Prinsip utamanya adalah kebalikan dari Bell-LaPadula:

1. **Simple Integrity Property:** Dikenal dengan istilah **"No Read Down"**. Subjek dengan integritas tinggi dilarang membaca data dari level bawah agar pikirannya tidak termakan oleh info yang hoax.
2. **Star Integrity Property ($\star$):** Dikenal dengan istilah **"No Write Up"**. Subjek dengan integritas rendah dilarang menulis/mengubah data di level yang lebih tinggi agar data di atas tetep bersih.

* **Ringkasan:** **"Read Up, Write Down"**. Ini kontras banget sama Bell-LaPadula. Logikanya: Info dari atas pasti valid, jadi boleh dibaca. Tapi orang bawah nggak boleh nulis ke atas biar integritas data di atas terjaga.
* **Kelemahan:** Model ini lemah terhadap ancaman dari dalam (*insider threat*).

---

### 3. Clark-Wilson Model

Sama seperti Biba, model ini juga mengejar **Integrity** (Integritas), tapi dengan cara yang lebih modern dan kompleks. Model ini punya beberapa konsep kunci:

* **Constrained Data Item (CDI):** Ini adalah tipe data yang integritasnya sangat penting dan ingin kita jaga (misal: saldo bank).
* **Unconstrained Data Item (UDI):** Ini adalah data yang belum divalidasi, seperti input dari user atau sistem lain yang bisa saja "kotor".
* **Transformation Procedures (TPs):** Ini adalah prosedur atau operasi terprogram (seperti baca/tulis) yang berfungsi menjaga integritas CDI. Jadi, akses ke data nggak langsung, tapi harus lewat prosedur ini.
* **Integrity Verification Procedures (IVPs):** Prosedur yang bertugas mengecek dan memastikan bahwa data (CDI) tetap valid dan benar.

Selain tiga model di atas (Bell-LaPadula, Biba, Clark-Wilson), masih banyak model lain yang bisa dipelajari, contohnya:
*   Brewer and Nash model
*   Goguen-Meseguer model
*   Sutherland model
*   Graham-Denning model
*   Harrison-Ruzzo-Ullman model

## Defence in Depth

**Defence-in-Depth** (atau sering disebut *Multi-Level Security*) adalah strategi keamanan yang menggunakan banyak lapisan pertahanan untuk melindungi data atau sistem. 

Konsep utamanya sangat sederhana: **Kalau satu lapisan jebol, masih ada lapisan lain yang menghadang penyerang.**

<p align="center">
<img src="../../../Assets/Images/Defence-ind-depth.png" alt="Defence in Depth">
</p>

### Mengamankan Barang Berharga

Bayangkan kamu punya dokumen sangat penting dan perhiasan mahal di rumah:

1.  **Level 1:** Kamu menaruhnya di dalam **laci meja** yang dikunci.
2.  **Level 2:** Apakah laci saja cukup? Tentu tidak. Kamu memastikan **pintu kamar** selalu dikunci.
3.  **Level 3:** Kamu juga mengunci **pintu utama rumah**.
4.  **Level 4:** Kamu mengunci **pagar depan rumah**.
5.  **Level 5:** Kamu memasang **kamera CCTV** di berbagai sudut untuk memantau pergerakan.

**Tujuannya?**
Meskipun lapisan-lapisan ini mungkin tidak bisa menghentikan pencuri yang sangat profesional (ingat Rule: *100% Security is a Myth*), tapi lapisan yang banyak ini akan **memperlambat** mereka dan membuat sebagian besar pencuri biasa menyerah karena pertahanannya terlalu rumit. 

Dalam dunia digital, lapisan ini bisa berupa kombinasi dari *firewall*, antivirus, enkripsi, *multi-factor authentication* (MFA), hingga pelatihan kesadaran keamanan bagi karyawan.

## ISO / IEC 19249

International Organization for Standardization (ISO) dan International Electrotechnical Commission (IEC) membuat standar **ISO/IEC 19249** sebagai katalog prinsip-prinsip arsitektur dan desain untuk produk dan sistem yang aman.

Ada lima prinsip arsitektur utama yang disebutkan dalam standar ini:

### 1. Domain Separation (Pemisahan Domain)
Menyusun komponen-komponen yang berkaitan ke dalam satu entitas yang terpisah. Setiap entitas punya domain dan atribut keamanannya masing-masing.

* **Analogi:** Bayangkan sebuah **Hotel**, Tamu di kamar 101 tidak bisa (dan tidak boleh) masuk ke kamar 102. Mereka dipisahkan oleh dinding dan kunci yang berbeda. 
* **Contoh Teknis:** Di dalam prosesor x86, ada yang namanya **Privilege Levels**. Sistem operasi (Kernel) berjalan di **Ring 0** (paling berkuasa), sedangkan aplikasi biasa seperti browser berjalan di **Ring 3** (paling terbatas). Ini menjaga agar aplikasi tidak bisa mengubah sistem utama.

### 2. Layering (Pelapisan)
Membagi sistem menjadi berbagai level atau lapisan abstrak. Ini memudahkan kita untuk menerapkan kebijakan keamanan yang berbeda di setiap levelnya.

* **Analogi:** Bayangkan **Baju Musim Dingin**. Kamu pakai kaos (lapisan dasar), lalu sweter (lapisan hangat), baru jaket (lapisan pelindung luar). Setiap lapisan punya fungsi sendiri untuk melindungimu.
* **Contoh Teknis:** Model **OSI (Open Systems Interconnection)** yang punya 7 lapisan. Setiap lapisan (seperti Transport atau Network) memberikan layanan spesifik dan menjaga integritas data sebelum dikirim ke lapisan di atasnya.

### 3. Encapsulation (Enkapsulasi)
Menyembunyikan detail implementasi di level rendah dan mencegah manipulasi data secara langsung. Kita hanya memberikan akses melalui metode tertentu yang sudah ditentukan

* **Analogi:** Bayangkan sebuah **Remote TV**. Kamu cuma perlu tahu cara pencet tombol Volume Up, kamu tidak perlu tahu bagaimana sirkuit di dalamnya bekerja mengubah tegangan listrik untuk mengeraskan suara
* **Contoh Teknis:** Dalam pemrograman (OOP), kita pakai metode `increment()` untuk menambah angka, daripada membiarkan user mengubah variabel `seconds` secara langsung. Ini mencegah user memasukkan data yang nggak valid (misal: detik diisi angka negatif)

### 4. Redundancy (Redundansi)
Menyediakan komponen cadangan untuk menjamin ketersediaan (*Availability*) dan integritas data jika terjadi kegagalan.

* **Analogi:** Bayangkan sebuah **Ban cadangan** di mobil, Kalau ban utama bocor di jalan, perjalananmu tidak terhenti sepenuhnya karena kamu punya ban cadangan.
* **Contoh Teknis:** Penggunaan dua *Power Supply* pada server (kalau satu mati, server tetap hidup) atau konfigurasi **RAID 5** pada penyimpanan data (kalau satu hardisk rusak, datanya masih bisa dipulihkan dari hardisk yang tersisa)

### 5. Virtualization (Virtualisasi)
Berbagi satu perangkat keras fisik untuk digunakan oleh banyak sistem operasi secara bersamaan. Ini memberikan kemampuan *sandboxing* yang kuat.

* **Analogi:** Bayangkan kamu menyewa sebuah **Unit Apartemen**, Meskipun semua unit berada di satu gedung fisik yang sama, setiap unit mandiri dan terisolasi. Jika ada kebakaran (serangan malware) di satu unit, unit lain tidak langsung ikut terbakar.
* **Contoh Teknis:** Menggunakan *Virtual Machines* (VM) atau *Container* untuk menjalankan aplikasi. Ini menciptakan batasan keamanan yang jelas dan memungkinkan kita memantau program berbahaya tanpa merusak sistem utama.

---

Selain prinsip arsitektur, ISO/IEC 19249 juga mengajarkan lima **prinsip desain** keamanan:

### 1. Least Privilege (Hak Akses Minimum)
Memberikan hak akses sekecil mungkin yang dibutuhkan seseorang untuk menyelesaikan tugasnya, tidak lebih dan tidak kurang.

* **Analogi:** Jika kamu mempekerjakan tukang kebun, kamu hanya memberinya kunci **pagar**, bukan kunci **kamar tidurmu**. Dia cuma butuh akses ke halaman untuk sekedar memotong rumput.
* **Contoh Teknis:** Jika seorang user hanya perlu melihat sebuah dokumen, berikan dia akses **Read-only**, jangan berikan akses **Write** (edit) apalagi **Admin**.

### 2. Attack Surface Minimisation (Minimalisir Permukaan Serangan)
Setiap sistem pasti punya celah. Strategi terbaik adalah mengurangi permukaan yang bisa diserang oleh hacker.

* **Analogi:** Bayangkan sebuah rumah dengan 10 pintu dan 20 jendela. Itu sangat sulit dijaga. Lebih aman jika rumah tersebut cuma punya **1 pintu utama** dan jendela yang tertutup rapat.
* **Contoh Teknis:** Mematikan (*disable*) layanan atau port yang tidak digunakan di sistem Linux. Semakin sedikit layanan yang jalan, semakin sedikit pintu yang bisa dicoba oleh hacker.

### 3. Centralized Parameter Validation (Validasi Parameter Terpusat)
Banyak serangan (seperti DoS atau RCE) terjadi karena sistem menerima input berbahaya dari user. Semua input harus divalidasi di satu tempat terpusat agar konsisten.

* **Analogi:** Seperti pemeriksaan di **Bandara**. Semua penumpang harus lewat satu pintu *security check* yang sama sebelum masuk ke pesawat. Tidak boleh ada yang masuk lewat pintu belakang tanpa diperiksa.
* **Contoh Teknis:** Menggunakan satu *library* atau sistem khusus untuk mengecek semua data yang masuk ke aplikasi, pastikan tidak ada karakter aneh yang bisa merusak database.

### 4. Centralized General Security Services (Layanan Keamanan Terpusat)
Memusatkan layanan keamanan (seperti autentikasi) di satu tempat agar lebih mudah dikelola dan diawasi.

* **Analogi:** Menggunakan **Satu Master Key** atau sistem kartu akses yang dikelola oleh admin pusat di sebuah gedung perkantoran.
* **Contoh Teknis:** Membangun server pusat untuk proses *login* (Authentication). Jadi, semua aplikasi di perusahaan tersebut menggunakan satu database user yang sama untuk memverifikasi identitas.

### 5. Preparing for Error and Exception Handling (Penanganan Error & Eksepsi)
Sistem harus dirancang untuk tetap aman bahkan saat terjadi error (*fail-safe*). Pastikan pesan error tidak membocorkan informasi rahasia.

* **Analogi:** **Pintu Darurat Otomatis**. Jika terjadi kebakaran (error), pintu tersebut harus otomatis terbuka (*fail-safe open*) agar orang bisa keluar, bukan malah terkunci didalam.
* **Contoh Teknis:** Jika sebuah *firewall* mengalami *crash*, pengaturannya harus dibuat untuk **memblokir semua traffic** (*block all*), bukan malah membiarkan semua trafik lewat. Selain itu, jangan tampilkan pesan error yang isinya detail database atau memori server ke user umum.

## Zero Trust VS Trust but Verify

Kepercayaan (*Trust*) adalah topik yang sangat kompleks. Di dunia nyata, kita nggak bisa hidup tanpa rasa percaya misalnya, kita harus percaya bahwa vendor laptop tidak menanam *spyware* di perangkat kita, Tapi di dunia keamanan, kita butuh prinsip yang lebih ketat.

Ada dua prinsip utama mengenai kepercayaan ini:

### 1. Trust but Verify (Percaya tapi Verifikasi)
Prinsip ini mengajarkan bahwa meskipun kita mempercayai sebuah entitas (user atau sistem) dan perilakunya, kita harus **selalu melakukan verifikasi**.

* **Analogi:** Bayangkan kamu punya **Asisten Rumah Tangga** yang sudah bekerja bertahun-tahun dan sangat kamu percayai. Kamu memberinya kunci rumah, tapi kamu tetap memasang **CCTV** dan sesekali mengecek laporan pengeluaran belanja untuk memastikan semuanya tetap jujur dan normal.
* **Konsep Teknis:** Verifikasi biasanya dilakukan melalui mekanisme **Logging** (pencatatan aktivitas). Karena memeriksa semua log secara manual itu mustahil, kita menggunakan sistem otomatis seperti *Proxy*, IDS (*Intrusion Detection System*), dan IPS (*Intrusion Prevention System*).

### 2. Zero Trust (Nol Kepercayaan)
Prinsip ini menganggap rasa percaya sebagai sebuah **kerahasiaan/kerentanan** (*vulnerability*). Slogannya adalah: **"Never trust, always verify."** (Jangan pernah percaya, selalu verifikasi).

* **Analogi:** Bayangkan sebuah **Gedung Rahasia Pemerintah**. Tidak peduli kamu itu karyawan lama atau bos besar, setiap mau naik lift kamu harus *tap* kartu, mau masuk ruangan harus *tap* lagi, bahkan mau buka lemari pun harus pakai sidik jari. Tidak ada rasa percaya hanya berdasarkan jabatan atau lokasi.
* **Konsep Teknis:** 
* Setiap entitas dianggap berbahaya sampai terbukti sebaliknya. 
* Lokasi jaringan (seperti berada di dalam kantor) tidak otomatis membuat perangkat dipercayai. 
* **Microsegmentation:** Membagi jaringan menjadi segmen-segmen yang sangat kecil (bisa sekecil satu perangkat saja), di mana setiap komunikasi antar segmen butuh autentikasi dan pengecekan akses (ACL).

**Poin Utama:**
*Zero Trust* membantu membatasi kerusakan jika terjadi kebocoran (*data breach*), karena penyerang tidak bisa bebas bergerak ke bagian lain tanpa verifikasi ulang. Meskipun implementasi *Zero Trust* yang ekstrem bisa menghambat bisnis, perusahaan tetap harus menerapkannya sesuai kemampuan dan kebutuhan.

## Vulnerability, Threat, dan Risk

Dalam dunia keamanan, ada tiga istilah yang sering bikin bingung tapi punya arti yang sangat spesifik. Memahami perbedaan ketiganya adalah kunci untuk bisa menganalisis keamanan secara profesional.

### 1. Vulnerability (Kerentanan)
**Vulnerability** adalah sebuah kelemahan. Ini bisa berupa celah di kode software, konfigurasi yang salah, atau bahkan kecerobohan manusia.

* **Analogi:** Bayangkan sebuah **Showroom Mobil** yang dinding dan pintunya terbuat dari **kaca biasa**. Sifat kaca yang mudah pecah adalah sebuah **Vulnerability** (kelemahan).

### 2. Threat (Ancaman)
**Threat** adalah potensi bahaya yang bisa memanfaatkan kelemahan tersebut. Ini adalah aksi atau kejadian yang bisa merusak sistem.

* **Analogi:** Karena dinding showroom tadi dari kaca, maka ada **Threat** (ancaman) bahwa seseorang bisa memecah kaca tersebut untuk masuk dan mencuri mobil. Ancaman ini nyata karena ada kelemahan yang bisa dimanfaatkan.

### 3. Risk (Risiko)
**Risk** adalah perhitungan antara seberapa besar kemungkinan (*likelihood*) sebuah ancaman terjadi dan apa dampak (*impact*)-nya terhadap bisnis atau sistem.

* **Analogi:** Pemilik showroom harus menghitung **Risk**-nya. Seberapa besar kemungkinan kaca itu pecah? Dan kalau pecah, berapa kerugian mobil yang hilang? Jika showroom itu ada di lingkungan rawan kejahatan, maka *Risk*-nya sangat tinggi.

---

### Studi Kasus: Database Rumah Sakit

Bayangkan kamu bekerja di tim IT sebuah rumah sakit:

1.  **Vulnerability:** Kamu baru tahu kalau sistem database rekam medis yang kamu pakai punya celah keamanan (*bug*).
2.  **Threat:** Di forum hacker, beredar kabar bahwa sudah ada kode eksploitasi (*Proof of Concept*) yang bisa menembus database tersebut. Sekarang, ancamannya menjadi sangat nyata.
3.  **Risk:** Kamu harus menghitung risikonya. Jika database itu dibobol, data pribadi ribuan pasien akan hilang dan rumah sakit bisa dituntut secara hukum.

**Kesimpulan:**
Tugas utama seorang ahli keamanan bukan cuma mencari **Vulnerability**, tapi juga menilai **Threat** yang ada, lalu menghitung **Risk** untuk menentukan langkah apa yang harus diambil (apakah harus segera dipatch, atau cukup dipantau saja).

## Conclusion

Room ini telah membahas berbagai prinsip dan konsep dasar keamanan yang sangat penting bagi seorang praktisi cybersecurity. Kita sudah membahas konsep **CIA** dan **DAD**, memahami istilah seperti *authenticity*, *non-repudiation*, hingga perbedaan antara *vulnerability*, *threat*, dan *risk*. Kita juga sudah mengintip model keamanan klasik dan standar internasional **ISO/IEC 19249**, serta strategi pertahanan seperti **Defence in Depth** dan **Zero Trust**.

Satu hal lagi yang sangat relevan di era *Cloud Computing* saat ini adalah:

### Shared Responsibility Model (Model Tanggung Jawab Bersama)
Di dunia cloud, keamanan itu bukan cuma tugas penyedia layanan (seperti AWS atau Azure), tapi tugas bersama antara **penyedia (Provider)** dan **pengguna (User)**.

* **Analogi:** Bayangkan kamu **Menyewa Apartemen**.
* **Tanggung Jawab Pengelola:** Memastikan gerbang utama aman, lift berfungsi, dan bangunan tidak roboh.
* **Tanggung Jawab Kamu:** Memastikan pintu unitmu dikunci, tidak sembarang kasih kunci ke orang asing, dan mematikan kompor saat keluar. Kalau kamu lupa kunci pintu lalu kecurian, itu bukan salah pengelola gedung.

Dalam dunia IT:
* **IaaS (Infrastructure as a Service):** User punya kontrol penuh (dan tanggung jawab penuh) atas Sistem Operasi dan aplikasi di dalamnya. Provider cuma urus hardware dan kabel.
* **SaaS (Software as a Service):** User hampir tidak punya akses ke OS. Tanggung jawab user lebih ke arah pengelolaan data dan hak akses user, sementara provider urus hampir semuanya (OS, hardware, jaringan).

---

### Real-World Relevance

Prinsip-prinsip ini bukan cuma teori buku teks. Di dunia nyata:
* **Zero Trust** sekarang jadi standar wajib di perusahaan besar untuk mencegah serangan *Ransomware* menyebar ke seluruh jaringan.
* Kegagalan memahami **Shared Responsibility Model** sering jadi penyebab kebocoran data di Cloud (misalnya: salah konfigurasi *bucket* penyimpanan yang terbuka untuk publik).
* Tanpa **Defence in Depth**, satu kesalahan kecil dari karyawan (misal: klik link phishing) bisa langsung membuat seluruh sistem perusahaan lumpuh.
