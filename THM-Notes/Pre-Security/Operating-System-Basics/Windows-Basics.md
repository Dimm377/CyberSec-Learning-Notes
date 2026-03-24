# TryHackMe: Windows Basics

- **Room Link:** [Windows Basics](https://tryhackme.com/room/windowsbasics)
- **Category:** Pre-Security / Computers Fundamentals
- **Difficulty:** Easy

_(Room ini adalah pengantar praktis sebelum masuk ke materi lebih dalam di catatan [Windows Fundamental Part 1](../Windows-Fundamental/Windows-Fundamental-Part1.md) dan [Windows Fundamental Part 2](../Windows-Fundamental/Windows-Fundamental-Part2.md))_

---

## Introduction

Di room sebelumnya (Operating Systems Introduction), kamu sudah belajar apa itu OS secara teori — bagaimana sistem operasi mengelola hardware, menjalankan proses, dan menjadi perantara antara pengguna dan mesin. Room ini mengambil langkah berikutnya: langsung terjun ke **Microsoft Windows**, OS desktop yang paling banyak digunakan di dunia.

Skenarionya sederhana — kamu berperan sebagai karyawan baru di **TryHatMe** yang baru saja mendapat workstation Windows. Tugas pertamamu: kenali lingkungan kerja digital ini. Mulai dari navigasi desktop, membuka tools lewat Start Menu, mengatur file di folder perusahaan, mengecek system settings, sampai memantau performa lewat Task Manager.

Setelah menyelesaikan room ini, kamu akan paham:

- Bagaimana navigasi antarmuka grafis Windows — desktop, taskbar, dan Start Menu.
- Cara menggunakan File Explorer untuk menjelajah folder, memahami file path, dan mengorganisir file.
- Bagaimana mengecek dan mengatur system settings lewat aplikasi Settings.
- Cara menggunakan tools bawaan seperti **Task Manager** dan **Windows Security** untuk memantau performa dan proteksi sistem.

---

## Exploring the Windows Workspace

### Windows Evolution

Sebelum Windows punya tampilan yang rapi seperti sekarang, komputer Microsoft masih mengandalkan **MS-DOS** (_Microsoft Disk Operating System_) — layar hitam yang mengharuskan pengguna mengetik perintah teks secara manual untuk melakukan apapun.

> **for your information:** **MS-DOS** (_Microsoft Disk Operating System_) adalah OS berbasis teks yang mendominasi era komputer personal tahun 1980-an. Tidak ada mouse, tidak ada ikon — semua interaksi dilakukan lewat command line.

Tahun 1985, Microsoft merilis **Windows 1.0** — GUI (_Graphical User Interface_) sederhana yang dibangun di atas DOS. Untuk pertama kalinya, pengguna bisa menggunakan mouse, mengklik ikon, dan bekerja dengan jendela (_windows_). Dari situ, Windows terus berkembang: menambahkan fitur, memperbaiki arsitektur, hingga menjadi OS mandiri yang tidak lagi bergantung pada DOS.

| Versi | Tahun | Catatan Penting |
| :--- | :---: | :--- |
| Windows 1.0 | 1985 | GUI pertama Microsoft, masih berjalan di atas DOS |
| Windows ME | 2000 | Terakhir berbasis DOS, terkenal tidak stabil |
| Windows 11 | 2021 | Versi terbaru dengan desain modern dan persyaratan TPM 2.0 |

### Logging in and Authentication

Sebelum bisa masuk ke Desktop, kamu harus melewati proses **autentikasi** — membuktikan identitasmu ke sistem. Windows menampilkan **login screen** di mana kamu memilih akun pengguna dan memasukkan kredensial (password, PIN, atau metode verifikasi lain).

Setiap akun memiliki level izin (_permission level_) yang menentukan apa saja yang boleh dilakukan di sistem. Windows menggunakan tiga tipe akun utama:

| Tipe Akun | Hak Akses |
| :--- | :--- |
| **Guest** | Akses terbatas dan sementara. Tidak bisa mengubah system settings. Cocok untuk pengguna yang hanya butuh akses sesaat tanpa meninggalkan jejak permanen. |
| **Standard** | Akun harian biasa. Bisa menjalankan aplikasi dan mengubah pengaturan personal, tapi tidak bisa menginstal software atau mengubah konfigurasi level sistem. |
| **Administrator** | Kontrol penuh atas seluruh sistem — instalasi software, perubahan konfigurasi, manajemen user, semuanya. |

> **Common Mistake:** Di lab atau CTF, kamu sering langsung login sebagai Administrator karena butuh akses penuh. Tapi di dunia nyata, menjalankan aktivitas sehari-hari dengan akun Administrator adalah kebiasaan buruk — jika malware berjalan di sesi Administrator, malware itu juga mendapat hak akses penuh ke seluruh sistem.

### The Windows Desktop

Setelah berhasil login, area pertama yang kamu lihat adalah **Desktop** — ruang kerja utama tempat semua aktivitas bermula. Dua komponen besar yang membentuk lingkungan ini:

- **Desktop** — area utama tempat file, folder, dan shortcut ditampilkan.
- **Taskbar** — strip kontrol di bagian bawah layar yang menyediakan akses ke aplikasi, system tools, pengaturan, dan notifikasi.

Komponen-komponen kunci di lingkungan Desktop:

| No | Komponen | Fungsi |
| :---: | :--- | :--- |
| 1 | **Desktop Icons** | Shortcut ke Recycle Bin, folder, dan aplikasi yang sering dipakai. Bisa di-customize sesuai kebutuhan. |
| 2 | **Start Menu** | Titik akses utama ke aplikasi, file, folder, settings, dan opsi power (shutdown/restart/logout). |
| 3 | **Search** | Pencarian cepat untuk menemukan aplikasi, file, folder, dan system settings berdasarkan kata kunci. |
| 4 | **Task View** | Menampilkan semua jendela yang sedang terbuka untuk berpindah cepat antar aplikasi. |
| 5 | **Pinned Apps & Folders** | Aplikasi dan folder yang di-pin ke taskbar agar gampang diakses tanpa membuka Start Menu. |
| 6 | **Network & Audio** | Kontrol cepat untuk koneksi jaringan dan volume audio. |
| 7 | **Date & Time** | Menampilkan tanggal dan waktu, bisa dibuka ke kalender lengkap. |
| 8 | **Notifications** | Pusat notifikasi dari sistem dan aplikasi, plus akses cepat ke beberapa settings. |

### Start Menu

**Start Menu** diakses dengan mengklik ikon Windows di pojok kiri bawah taskbar (atau menekan tombol `Win` di keyboard). Ini adalah pusat komando di Windows — dari sini kamu bisa:

- Membuka aplikasi apa saja yang terinstal di sistem.
- Mengakses folder penting (Documents, Pictures, Downloads).
- Membuka Settings dan Control Panel.
- Shutdown, restart, atau logout dari sistem.

Di sisi kanan Start Menu, biasanya ada area **Quick Access** yang menampilkan aplikasi dan folder yang sering digunakan atau di-pin secara manual.

### Built-in Tools and Apps

Windows sudah dilengkapi dengan tools bawaan yang langsung tersedia tanpa perlu instalasi tambahan. Beberapa yang paling sering dipakai:

| Tool | Fungsi |
| :--- | :--- |
| **Notepad** | Text editor paling sederhana untuk mengedit file teks biasa. |
| **File Explorer** | Alat untuk menjelajahi, memindahkan, menyalin, dan mengorganisir file dan folder. |
| **Calculator** | Kalkulator standar dan scientific. |
| **Paint** | Editor gambar sederhana bawaan Windows. |

Semua tools ini bisa diakses lewat Start Menu atau Search bar.

### Getting System Information

Windows menyediakan aplikasi **Settings** yang menjadi pusat konfigurasi sistem. Untuk mendapatkan gambaran umum tentang spesifikasi mesin yang sedang kamu gunakan, buka **Settings → About your PC**.

Halaman ini menampilkan informasi penting:

- **Device name** — nama komputer di jaringan.
- **Processor** — tipe dan kecepatan CPU.
- **Installed RAM** — jumlah memori yang terpasang.
- **System type** — arsitektur OS (32-bit atau 64-bit).
- **Windows edition & version** — edisi dan versi Windows yang terinstal.

> **for your information:** Informasi ini sangat berguna dalam konteks keamanan. Saat melakukan pentest atau forensik, mengetahui versi OS, arsitektur (32/64-bit), dan build number membantu menentukan exploit mana yang relevan dan patch mana yang sudah atau belum diterapkan.

## Configuring and Securing Windows

### File Exploration and Management

Windows mengorganisir data menggunakan **struktur folder hierarkis** — folder bisa berisi folder lain di dalamnya, dan di dalam folder-folder itu ada file. Struktur ini menjaga data tetap rapi dan mudah ditemukan seiring bertambahnya isi sistem.

**File Explorer** adalah tools utama untuk navigasi struktur ini. Beberapa lokasi yang paling sering diakses:

| Lokasi | Fungsi |
| :--- | :--- |
| **Desktop** | Tempat shortcut dan file yang sering dipakai langsung dari layar utama |
| **Documents** | Folder default untuk menyimpan dokumen kerja |
| **Downloads** | Tempat semua file yang diunduh dari internet masuk secara otomatis |

Setiap lokasi bisa memiliki **subfolder** untuk mengelompokkan file berdasarkan kategori. Memahami layout ini penting — saat melakukan forensik atau investigasi, kamu perlu tahu di mana Windows menyimpan file secara default.

### Updating Your Applications

Menjaga OS dan aplikasi tetap up-to-date adalah salah satu langkah keamanan paling mendasar. Update biasanya berisi **security patches** (menambal kerentanan), perbaikan bug, dan peningkatan performa.

**Windows Update**

Windows punya tools bawaan bernama **Windows Update** yang menangani pembaruan OS dan beberapa fitur keamanan bawaan. Aksesnya lewat **Settings → Update & Security → Windows Update**.

Dari sini, kamu bisa melihat update yang tersedia, mengecek secara manual, dan mengatur jadwal instalasi. Beberapa organisasi mengontrol kebijakan update secara terpusat — dalam kasus itu, akan muncul pesan _"Some settings are managed by your organization"_.

**Update Aplikasi**

Untuk aplikasi selain komponen OS, mekanisme update-nya berbeda-beda tergantung bagaimana software tersebut diinstal:

- Aplikasi bawaan Windows sering update otomatis di background.
- Aplikasi pihak ketiga biasanya punya mekanisme update sendiri.
- Beberapa aplikasi menampilkan prompt update saat diluncurkan.
- Ada juga yang mengharuskan kamu mengecek dan mengunduh installer baru secara manual.

> **Common Mistake:** Menunda update adalah kebiasaan yang sangat berisiko. Banyak serangan besar (seperti WannaCry) memanfaatkan kerentanan yang patch-nya _sudah tersedia_ berminggu-minggu sebelum serangan terjadi. Organisasi yang telat menerapkan patch menjadi korban.

### Installing Applications

Ada dua cara utama menginstal aplikasi di Windows:

1. **Microsoft Store** — menyediakan aplikasi yang sudah dikurasi dan relatif aman. Tapi perlu dicatat: Microsoft Store **tidak tersedia secara default di Windows Server**.
2. **Dari Internet** — mengunduh installer langsung dari website vendor tepercaya. File installer biasanya berformat `.exe` atau `.msi`.

> **for your information:** File **`.exe`** (_Executable_) adalah format standar untuk program yang bisa dijalankan di Windows. File **`.msi`** (_Microsoft Installer_) adalah format installer khusus Windows yang menyediakan fitur instalasi terstruktur seperti rollback dan repair.

### Uninstalling Applications

Untuk menghapus aplikasi yang sudah tidak diperlukan, Windows menyediakan beberapa cara:

- Lewat **Microsoft Store** (untuk aplikasi yang diinstal dari sana).
- Lewat **Settings → Apps & Features** (Add or Remove Programs).
- Lewat **Control Panel → Programs → Uninstall a program**.
- Menggunakan **uninstaller bawaan** aplikasi itu sendiri.

### Diving Into Settings

Windows punya dua antarmuka konfigurasi yang hidup berdampingan:

| Antarmuka | Karakteristik |
| :--- | :--- |
| **Windows Settings** | Antarmuka modern dan terpusat. Mengelola konfigurasi perangkat, sistem, personalisasi, dan keamanan. |
| **Control Panel** | Antarmuka legacy yang masih menyediakan akses ke beberapa tools konfigurasi lama yang belum sepenuhnya dimigrasikan ke Settings. |

Dari kedua antarmuka ini, kamu bisa mengatur:
- System preferences (display, audio).
- User accounts dan permission.
- Pengaturan jaringan dan konektivitas.
- Opsi aksesibilitas.
- Fitur keamanan dan firewall.

> **for your information:** Secara bertahap, Microsoft memindahkan semua fungsi dari **Control Panel** ke **Windows Settings**. Tapi di versi Windows saat ini, beberapa konfigurasi lanjutan masih hanya bisa diakses lewat Control Panel — terutama di lingkungan Windows Server.

### The Task Manager

**Task Manager** adalah tools monitoring bawaan Windows yang menampilkan kondisi sistem secara _real-time_. Dari sini kamu bisa melihat aplikasi yang sedang berjalan, proses di background, serta penggunaan CPU dan memori.

Task Manager memiliki **lima tab utama**:

| Tab | Fungsi |
| :--- | :--- |
| **Processes** | Daftar semua aplikasi dan proses background yang aktif beserta konsumsi resource-nya (CPU, RAM, Disk, Network). |
| **Performance** | Grafik dan statistik _real-time_ untuk CPU, memori, disk, dan jaringan. Berguna untuk mendeteksi anomali performa. |
| **Users** | Menampilkan user yang sedang login beserta resource yang dikonsumsi masing-masing. |
| **Details** | Tampilan teknis yang lebih detail — termasuk **PID** (_Process ID_), nama executable, dan status setiap proses. |
| **Services** | Daftar semua Windows services beserta statusnya (Running atau Stopped). |

> **for your information:** **PID** (_Process ID_) adalah nomor unik yang diberikan sistem operasi ke setiap proses yang sedang berjalan. PID penting dalam konteks forensik dan incident response — kamu menggunakannya untuk mengidentifikasi, melacak, dan menghentikan proses tertentu secara presisi.

### Native Windows Security

Windows sudah dilengkapi dengan tools keamanan bawaan yang aktif secara default. Tools ini dirancang untuk melindungi sistem dari malware, aplikasi berbahaya, dan akses jaringan yang tidak sah.

**Windows Security**

Aplikasi **Windows Security** adalah dashboard utama untuk mengelola seluruh proteksi bawaan Windows. Ada empat area utama:

| Area | Fungsi |
| :--- | :--- |
| **Virus & threat protection** | Mendeteksi dan menghapus malware menggunakan proteksi real-time dan scan yang bisa dikustomisasi. |
| **Firewall & network protection** | Mengontrol traffic jaringan masuk dan keluar untuk mencegah akses tidak sah. |
| **App & browser control** | Melindungi dari aplikasi, file, dan website yang berpotensi berbahaya. |
| **Device security** | Proteksi berbasis hardware yang membantu mengamankan sistem di level firmware. |

**Menjalankan Custom Scan**

Kamu bisa menjalankan scan khusus untuk memeriksa folder tertentu:

1. Buka **Windows Security** (lewat shortcut di Desktop atau Start Menu).
2. Pilih bagian **Virus & Threat protection**.
3. Klik **Scan options**.
4. Pilih **Custom scan** → **Scan now**.
5. Tentukan folder target yang ingin diperiksa.

Jika scan menemukan ancaman, Windows menampilkan detail temuan beserta opsi tindakan:

- **Allow on device** — mengizinkan file tetap ada (hanya jika kamu yakin itu _false positive_).
- **Quarantine** — mengisolasi file berbahaya agar tidak bisa dieksekusi, tapi belum dihapus permanen.
- **Remove** — menghapus file sepenuhnya dari sistem.

> **for your information:** **EICAR test file** adalah file standar yang digunakan untuk menguji apakah antivirus berfungsi dengan benar. File ini tidak berbahaya sama sekali — isinya hanya string teks khusus yang sengaja dikenali semua antivirus sebagai "ancaman" untuk keperluan testing.

**Windows Defender Firewall**

**Windows Defender Firewall** adalah firewall bawaan yang memonitor koneksi jaringan dan menerapkan aturan untuk menentukan traffic mana yang diizinkan dan mana yang diblokir. Firewall ini beroperasi berdasarkan **network profile** — tiga profil yang masing-masing punya tingkat keamanan berbeda:

| Profil | Digunakan Saat | Tingkat Keamanan |
| :--- | :--- | :--- |
| **Domain** | Sistem terhubung ke jaringan domain organisasi (Active Directory) | Dikelola oleh administrator jaringan |
| **Private** | Jaringan yang dipercaya, seperti jaringan rumah atau lab | Lebih permisif, mengizinkan _network discovery_ |
| **Public** | Jaringan yang tidak dipercaya, seperti Wi-Fi publik | Paling ketat, membatasi sebagian besar koneksi masuk |

> **Common Mistake:** Saat terhubung ke Wi-Fi publik (kafe, bandara), pastikan profil jaringan di-set ke **Public**. Jika secara tidak sengaja di-set ke Private, firewall akan lebih longgar dan perangkat lain di jaringan yang sama bisa menemukan dan mencoba mengakses komputermu.

## Conclusion

Room ini menggunakan **Windows Server 2019** sebagai lingkungan lab. Kamu sudah mengeksplorasi interface Windows, memeriksa spesifikasi sistem, mengelola file dan aplikasi, serta menjalankan scan keamanan — semua skill dasar yang menjadi pondasi sebelum masuk ke eksplorasi lebih dalam.

### Key Terminology

| Istilah | Definisi |
| :--- | :--- |
| **Desktop** | Ruang kerja utama tempat file, folder, dan shortcut ditampilkan |
| **Taskbar** | Strip kontrol yang menyediakan akses ke aplikasi, tools, settings, dan notifikasi |
| **Start Menu** | Titik akses utama ke aplikasi, settings, dan opsi power (ditandai logo Windows) |
| **Search** | Fitur pencarian cepat untuk menemukan aplikasi, settings, dan file berdasarkan kata kunci |
| **File Explorer** | Tools bawaan untuk menjelajahi, mengelola, dan mengorganisir file dan folder |
| **Windows Update** | Tools bawaan untuk menjaga OS, aplikasi native, dan fitur keamanan tetap up-to-date |
| **Microsoft Store** | Aplikasi Windows untuk menginstal aplikasi yang sudah dikurasi |
| **Windows Settings** | Antarmuka modern dan terpusat untuk konfigurasi sistem, perangkat, personalisasi, dan keamanan |
| **Control Panel** | Antarmuka legacy untuk konfigurasi sistem yang belum sepenuhnya dimigrasikan ke Settings |
| **Task Manager** | Tools monitoring real-time untuk memantau proses, performa, dan resource sistem |
| **Windows Security** | Dashboard utama untuk mengelola seluruh proteksi keamanan bawaan Windows |
| **Windows Defender Firewall** | Firewall bawaan untuk melindungi sistem dari traffic jaringan yang tidak sah |

### Further Learning

Room selanjutnya dalam modul ini akan mengajak kamu berinteraksi dengan OS lewat **CLI** (_Command-Line Interface_) — antarmuka berbasis teks yang jauh lebih powerful dibanding GUI untuk keperluan administrasi dan keamanan.

- [Linux CLI Basics](https://tryhackme.com/room/introtoshells)
- [Windows CLI Basics](https://tryhackme.com/room/introtoshells)

---

## Review

- Windows menggunakan **tiga tipe akun** (Guest/Standard/Administrator) dengan level izin berbeda — menjalankan aktivitas sehari-hari sebagai Administrator adalah kebiasaan berisiko karena malware juga mendapat hak akses penuh.
- **Windows Security** adalah dashboard terpusat dengan 4 area proteksi, dan **Windows Defender Firewall** beroperasi berdasarkan 3 network profile (Domain/Private/Public) yang menentukan keketatan aturan filtering.
- **Task Manager** menyediakan monitoring real-time lewat 5 tab (Processes, Performance, Users, Details, Services) — kemampuan membaca informasi di sini adalah skill dasar untuk troubleshooting dan incident response.
- Menjaga sistem **up-to-date** lewat Windows Update adalah langkah keamanan paling mendasar — banyak serangan besar memanfaatkan kerentanan yang patch-nya sudah tersedia tapi belum diterapkan.
