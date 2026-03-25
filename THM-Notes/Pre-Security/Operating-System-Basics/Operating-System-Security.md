# TryHackMe: Operating System Security

- **Room Link:** [Operating System Security](https://tryhackme.com/room/operatingsystemsecurity)
- **Category:** Pre-Security / Operating System Basics
- **Difficulty:** Easy

_(Prerequisite: Operating Systems Introduction, Windows Basics, Linux & Windows CLI Basics)_

## Introduction to Operating System Security

Setiap hari, sadar atau tidak, kamu berinteraksi dengan Sistem Operasi (Operating System/OS) entah itu Windows, macOS, Linux, iOS, atau Android di _smartphone_ mu. Tapi, apa sebenarnya peran sebuah sistem operasi? Untuk menjawabnya, kamu perlu mengingat kembali konsep tentang _hardware_.

### Hardware Vs Software (OS)

_Hardware_ adalah semua komponen fisik komputer yang bisa kamu sentuh seperti layar, keyboard, USB flash drive, dan terutama _desktop board_ (motherboard). Papan sirkuit utama ini memuat **CPU** (_Central Processing Unit_), memori **RAM** (_Random Access Memory_), dan terhubung ke penyimpanan permanen seperti HDD atau SSD.

Namun, semua perangkat keras secanggih apa pun hanyalah benda mati, komponen-komponen tersebut tidak berguna sampai ada sistem operasi yang mengontrol dan menggerakkan (_drive_) alat-alat tersebut agar program favoritmu bisa berjalan.

![Software and Hardware Diagram](../../Assets/Images/Software&Hardware.png)

Diagram di atas dengan jelas menunjukkan hierarki sistem komputer. **OS adalah lapisan penerjemah / pembatas** yang berada di antara hardware dan aplikasi.

Program-program seperti *web browser* (Firefox, Chrome) atau aplikasi *chatting* (WhatsApp, Telegram) **tidak bisa dan tidak boleh** mengakses alat hardware secara langsung. Aplikasi harus meminta izin pada OS, dan OS-lah yang akan memberinya jalur akses ke hardware sesuai dengan aturan (_rules_) yang sangat spesifik.

> **for your information:** Ada banyak variasi OS. Sebagian dirancang khusus untuk laptop/PC (Windows 11, macOS), sebagian untuk mobile (Android, iOS), dan ada pula yang khusus spesialis infrastruktur *server* perusahaan berskala besar (Windows Server 2022, IBM AIX, Oracle Solaris). **Linux** merajai dua ranah tersebut sekaligus, andalan di Server perkantoran modern namun nyaman untuk desktop harian.

### The Need for Security

Mengapa OS perlu diamankan dengan ketat? Karena komputer dan smartphone modern menyimpan segudang informasi pribadimu:
1. File rahasia terkait pekerjaan atau perkuliahan.
2. Foto-foto pribadi, atau foto dokumen identitas seperti KTP/Paspor.
3. Klien Email yang berisi korespondensi bisnis.
4. Data Sandi (*Passwords*) yang otomatis tersimpan di dalam _browser_.
5. Aplikasi perbankan elektronik dan finansial.

Tentu saja, kamu tidak akan membiarkan orang tak dikenal membuka pintu rumahmu dan mengacak-acak lacimu. Konsep ini berlaku juga untuk perangkatmu. Mengamankan file dan OS-nya adalah esensi utama dari Cyber Security.

### The CIA Triad

Saat para profesional keamanan siber berbicara tentang melindungi data, fokus utama mereka akan selalu berpusat pada tiga prinsip mendasar, yang dikenal luas sebagai **CIA Triad**:

| Pilar | Deskripsi |
| :--- | :--- |
| **Confidentiality** (Kerahasiaan) | Memastikan file rahasia dan informasi privat **hanya** bisa diakses oleh entitas atau orang yang benar-benar berhak. |
| **Integrity** (Integritas) | Memastikan **tidak ada pihak tak berizin** yang memodifikasi, merusak, atau menyisipkan *malware* (_tamper_) ke dalam file yang tersimpan di sistem maupun data yang sedang berpindah jalur di jaringan. |
| **Availability** (Ketersediaan) | Memastikan laptop, sistem, atau perangkatmu selalu **siap dan tanggap beroperasi kapan saja** kamu memutuskan untuk memakainya. |

Semua *attacks* (serangan siber) yang akan dibahas selanjutnya dirancang untuk menjebol, merusak, atau menumbangkan setidaknya salah satu dari tiga pilar keamanan fundamental ini.

---

## Common Examples of OS Security

Seperti sudah dibahas pada pilar keamanan CIA, segala bentuk pengamanan (*security*) pada intinya bertugas untuk mengatasi berbagai kelemahan sistem. Di task ini kitalah yang mendalami tiga celah utama yang selalu dicari dan dieksploitasi oleh penyerang (*attacker*) dan pengguna jahat.

1.  **Authentication and Weak Passwords**
2.  **Weak File Permissions**
3.  **Malicious Programs**

### 1. Authentication and Weak Passwords

Autentikasi (_authentication_) adalah tindakan untuk membuktikan dan memvalidasi **identitas aslimu** saat mengakses sebuah sistem, baik lokal di depan keyboardmu maupun secara _remote_ dari jaringan jarak jauh. Metode autentikasi umumnya dikategorikan menjadi tiga bentuk:

-   **Something you know:** Sesuatu yang berada di kepalamu. Contoh: *password* akun atau nomor PIN bank.
-   **Something you are:** Karakteristik biologis atau ciri fisikmu yang unik. Contoh: biometrik seperti sensor sidik jari (*fingerprint*) dan deteksi wajah (*Face ID*).
-   **Something you have:** Barang, objek fisik, atau token _hardware_ yang berada di genggamanmu saat _login_. Contoh: SMS kode *OTP* (menggunakan nomor _smartphone_), _authenticator app_, atau kunci fisik USB Yubikey.

Di antara ketiga jenis tersebut, sistem kata sandi (*passwords*) adalah sistem yang pembuatannya paling murah, banyak digunakan di setiap pendaftaran layanan web, dan pada akhirnya **paling sering diretas**.

Berdasarkan laporan *National Cyber Security Centre* (NCSC) tentang 100.000 sandi internet teratas, manusia menggunakan kombinasi yang sangat tidak logis untuk mengamankan data rahasianya. Mulai dari urutan numerik (`123456`, `111111`, `123123`), kata acak dalam kamus bahasa tanpa pelesetan (`password`, `monkey`, `dragon`), hingga menggunakan kombinasi tombol keyboard (`qwerty`, `1q2w3e4r5t`, `qwertyuiop`).

> **Common Mistake:** Sebagian besar pengguna juga sering menggunakan "trik rahasia personal", yaitu menggunakan kata dari profil kehidupan mereka di media sosial seperti tanggal lahir, nomor alamat asli, dan digabung dengan nama anjing/kucing mereka (misal: `mochi**2019**`). Ini adalah pola yang sangat lemah, dan penyerang (*attackers*) amat sangat sadar dan peka terhadap kecenderungan ceroboh tersebut, semuanya akan dimasukkan ke *wordlist* (daftar sandi-sandi sasaran peretas).

Kuncinya selalu gunakan struktur *password* yang membingungkan bagi manusia dan juga komputer penyusup, ditambah bedakan setiap kata sandi untuk semua akun layananmu. Jika satu layanamu disusupi dan basis datanya bocor; setidaknya *attacker* tidak bisa memakai akun email utamamu untuk menyerang rekening bank dan layanan lainnya.

### 2. Weak File Permissions

Ada satu doktrin keamanan tertinggi di lingkungan infrastruktur profesional IT: **The Principle of Least Privilege** (Prinsip Hak Akses Terkecil/Minimal).

Apa maksud dari doktrin ini? Pastikan izin akses kontrol semua *file* yang tersimpan di server kantormu **hanya diberikan** untuk mereka yang tugas dan pekerjaannya memang benar-benar berkepentingan dengan dokumen tersebut. 

Sebagai analogi: Jika kamu sedang mengatur agenda dan urunan uang untuk _road trip_ akhir tahun bersama lima orang teman terdekatmu. Sudah tentu tautan Google Drive berisikan tabel detail pembayaran sewa dan daftar bawaan kelompok **wajib dikunci** dan _hanya_ dibagikan ke lima orang temanmu. Sisa anggota kelas atau publik sama sekali **tidak perlu dan tidak berhak** tahu detail rencana pribadi ini. Itulah representasi dari *principle of least privilege*, bertanya pada diri sendiri *"Siapa yang bisa mengakses apa?"* (_"who can access what?"_)

Lemahnya izin *file permissions* akibat malas mengkonfigurasi OS, akan menghadirkan celah eksploitasi yang mengancam kerahasiaan (*Confidentiality*) sistem, penyerang tiba-tiba bisa seenaknya melihat data keuangan kantor akibat folder rahasia yang tidak dikunci oleh admin, dan integritas (*Integrity*) file terancam dimanipulasi orang iseng yang tidak berkepentingan.

### 3. Access to Malicious Programs

Ancaman fatal terakhir adalah hadirnya program atau perangkat lunak berbahaya (*malicious software/malware*) di dalam perangkat klien. Kategori ancaman ini bergantung pada *strain* jenisnya dan bisa melumpuhkan satu hingga semua tumpuan **CIA**.

*   **Trojan horses:** Seperti sejarah perangnya, *malware* ini datang dengan kamuflase yang sangat bersih. Sebuah aplikasi atau ekstensi legal terinstal seperti layaknya *software* lain, namun aslinya aplikasi fiktif tersebut telah diprogram dengan "penumpang gelap". *Trojan* merangkak ke dalam memori OS dan terhubung ke _server remote_ peretas dari jauh, memberikan akses tanpa sepengetahuanmu, file pentingmu hilang tercuri tanpa sadar dan modifikasi terselubung mulai mengacak infrastruktur OS milikmu.
*   **Ransomware:** Menyerang titik ketersediaan (*Availability*). *Ransomware* akan mengubah OS yang awalnya bekerja menjadi deretan sandi terenkripsi acak tanpa ampun yang mustahil untuk didekripsi (*gibberish texts*); mengunci segala data krusial perusahaan dan menuntut sejumlah bayaran (tebusan/*ransom*). Sampai para pimpinan menebus uang mahal itu, file asli tersebut lenyap untuk selamanya.

---

## Real-World Relevance

Di eksploitasi dunia nyata maupun pengetesan keamanan (pentest), sebagian besar insiden bermula dari celah keamanan mendasar ini:
- **Sandi yang Lemah (Passwords):** Mayoritas pembobolan data berawal dari serangan _brute force_ atau _credential stuffing_ pada kredensial karyawan yang buruk (atau kebiasaan menggunakan sandi yang sama di berbagai akun institusi).
- **Izin yang Asal (Weak File Permissions):** Teknik dasar eskalasi hak istimewa (_Privilege Escalation_) di server Linux maupun OS Windows hampir selalu memanfaatkan celah izin *file* esensial yang dibiarkan terbuka oleh admin.
- **Ransomware Paralysis:** Kelumpuhan total rumah sakit atau infrastruktur transportasi publik sering kali dipicu oleh keteledoran seorang staf yang secara tidak sengaja mengunduh *Trojan* dari *email phishing*.

## Review

- **Konsep Hardware-Software:** _Hardware_ adalah jajaran perangkat fisik murni; ia membutuhkan Sistem Operasi (OS) sebagai lapisan penerjemah agar aplikasi bisa berjalan.
- **The CIA Triad:** Tiga pilar fundamental perlindungan data meliputi Kerahasiaan (_Confidentiality_), Integritas (_Integrity_), dan Ketersediaan (_Availability_).
- **Vektor Ancaman Paling Umum:** _Attacker_ paling sering menyasar _password_ yang mudah ditebak, pengaturan hak akses (_file permissions_) yang terlalu longgar, hingga penyebaran *malware* (seperti _Trojan_ atau _Ransomware_) untuk menumbangkan sistem tanpa disadari pengguna.
