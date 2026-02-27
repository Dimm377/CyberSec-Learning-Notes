# TryHackMe: IDS Fundamentals

- **Room Link:** [IDS Fundamentals](https://tryhackme.com/room/idsfundamentals)
- **Category:** Security Solutions
- **Difficulty:** easy

## What is an IDS ?

Kita sudah mempelajari bahwa **Firewall** bertugas sebagai satpam penjaga gerbang (mengontrol siapa yang boleh masuk dan keluar). Namun, sebuah sistem tidak pernah luput dari kelemahan. Bagaimana jika ada penyusup yang berhasil menyamar sebagai pengunjung sah dan melewati satpam tersebut? Begitu mereka sudah berada di dalam jaringan internal, Firewall di pintu depan tidak lagi bisa mengawasi mereka.

Untuk memperbaiki celah *blind spot* (titik buta) inilah, para arsitek jaringan menciptakan **Intrusion Detection System (IDS)**.

Jika *Firewall* adalah satpam di pos penjagaan depan, maka **IDS adalah kamera pengawas (CCTV) berteknologi tinggi** yang dipasang menyebar di seluruh sudut ruangan dalam jaringan.

**Sistem Kerja Utama IDS:**
1. **Memantau Tanpa Henti:** IDS berada di dalam infrastruktur (berbeda dengan Firewall yang ada di batas luar) dan terus memantau seluruh lalu lintas data (*traffic*) yang berlalu-lalang di dalam.
2. **Mendeteksi Kejanggalan:** Layaknya CCTV pintar, IDS akan menganalisis setiap paket data untuk mencari pergerakan abnormal (*Anomaly*) atau mencocokkannya dengan rekam jejak kriminal (*Signature-based detection*).
3. **Membunyikan Alarm (Sifat Pasif):** Ini adalah karakteristik paling mutlak dari sebuah IDS konvensional. Ketika IDS melihat ada aktivitas penyusupan yang berhasil lolos, **ia tidak akan melakukan pemblokiran secara mandiri**. Tugas tunggalnya hanyalah membunyikan sistem peringatan dini (*generate alert*) kepada Administrator Keamanan. Keputusan akhir untuk memblokir penjahat tersebut tetap berada di tangan Administrator atau sistem keamanan lain.

### Learning Objectives

Kita akan mengetahui lebih mendalam mengenai Security solution IDS, termasuk eksplorasi alat IDS *open-source* paling populer (Snort). Fokus pembelajaran kita meliputi:
- Konsep berbagai Tipe IDS beserta kemampuan deteksinya.
- Pemahaman cara kerja dasar Snort IDS.
- Mempelajari struktur aturan bawaan (*Default Rules*) dan aturan kustomisasi di dalam Snort.
- Praktik merancang *Custom Rule* sendiri menggunakan bahasa spesifik Snort.

## Type of IDS
IDS dapat dikategorikan berdasarkan fungsinya. Klasifikasi utamanya terbagi atas dua hal: **Berdasarkan Posisi Penempatan (Deployment)** dan **Berdasarkan Cara Deteksi (Detection)**.

### 1. Deployment Modes (Posisi Penempatan)

Berdasarkan letak di mana sistem ini di install, IDS terbagi menjadi dua kelompok:

- **Host Intrusion Detection System (HIDS):**
  Jika NIDS adalah CCTV bangunan, maka HIDS ini ibarat **Pengawal Pribadi (Bodyguard) VIP**.
  Solusi HIDS diinstal secara individu langsung ke dalam satu perangkat spesifik (seperti Server Database atau laptop Direktur). Tugasnya eksklusif hanya mengawasi ancaman yang berpotensi menyerang perangkat *host* tersebut.
  *Keunggulan:* Mampu memberikan pengawasan aktivitas lokal dengan visibilitas yang sangat detail.
  *Kelemahan:* Sangat merepotkan untuk dikelola pada jaringan perusahaan yang masif, karena Administratif harus menginstal dan merawat sistem pengawal ini pada setiap perangkat individu (*resource-intensive*).

- **Network Intrusion Detection System (NIDS):**
  Ini adalah **Sistem CCTV Utama** yang mengawasi seluruh bangunan operasional.
  NIDS tidak peduli dengan urusan individu perangkat. Ia diletakkan di titik persimpangan jaringan (seperti di belakang Router atau *Switch*) untuk memantau lalu lintas data dari semua perangkat yang terhubung dalam ekosistem jaringan tersebut.
  *Fungsi Utama:* Memberikan gambaran (*centralized view*) terpusat mengenai segala hal yang terjadi di dalam jaringan secara keseluruhan.

<P align="center">
<img src="../../Assets/Images/NIDS-HIDS.png" alt="NIDS-HIDS" width="700px"/>
</p>

### 2. Detection Modes (Cara Deteksi)

Setelah menentukan di mana CCTV (IDS) akan dipasang, langkah selanjutnya adalah memahami bagaimana otak dari CCTV tersebut bekerja dalam mendeteksi ancaman. Umumnya ada tiga metode deteksi:

- **Signature-Based IDS (Berdasarkan Rekam Jejak):**
  Metode ini bekerja layaknya polisi yang berpatroli sambil membawa buku **Daftar Pencarian Orang (DPO)**.
  Setiap serangan siber pasti meninggalkan pola atau sidik jari yang unik (disebut *Signature*). IDS jenis ini sudah dibekali *database* berisi ribuan *signature* dari serangan-serangan yang pernah terjadi di dunia.
  *Kelebihan:* Sangat cepat dan akurat dalam mendeteksi ancaman yang sudah populer.
  *Kelemahan Fatal:* Buta terhadap serangan *Zero-Day* (serangan jenis baru yang belum pernah terjadi dan belum ada di buku DPO). Jika penjahat menggunakan modus baru, IDS ini akan diam saja. (*Contoh alat Populer: Snort*).

- **Anomaly-Based IDS (Berdasarkan Perilaku Normal):**
  Berbeda dengan metode sebelumnya, IDS jenis ini tidak membawa buku DPO. Ia bekerja dengan cara **Mempelajari Kebiasaan (*Baseline*)**.
  Awalnya, sistem ini akan diterjunkan ke dalam jaringan untuk belajar: "Oh, biasanya *traffic* harian jam 9 pagi cuma segini", atau "Oh, biasanya server ini cuma diakses oleh IP lokal". Ketika suatu masa ada aktivitas yang menyimpang dari kebiasaan normal tersebut (misal: *traffic* tiba-tiba melonjak tengah malam), ia akan langsung membunyikan alarm.
  *Kelebihan:* Sangat jago menangkap serangan *Zero-Day* yang belum punya *signature*.
  *Kelemahan Fatal:* Sering terjadi *False Positives* (salah sangka). Aktivitas normal yang kebetulan agak tinggi (misal: karyawan sedang mengunduh file besar untuk presentasi besok) bisa ditandai sebagai serangan. Butuh proses *fine-tuning* (penyesuaian manual) yang panjang agar sistem ini tidak terlalu paranoid.

- **Hybrid IDS (Sistem Kombinasi):**
  Sesuai namanya, metode ini merupakan gabungan kekuatan dari *Signature-Based* dan *Anomaly-Based*.
  Jika ada ancaman lama yang masuk, ia akan gunakan buku DPO (*Signature*) untuk memblokir dengan cepat tanpa menguras tenaga. Namun jika ada ancaman misterius yang mencoba menyusup, sistem *Anomaly* miliknya akan mengambil alih deteksi.

## IDS Example: Snort

**Snort** adalah salah satu IDS solution *open-source* paling terkenal dan paling banyak digunakan di dunia (dikembangkan sejak tahun 1998).

Snort pada dasarnya beroperasi sebagai **Hybrid IDS**. Ia menggunakan gabungan dari pendeteksian pola (*Signature-based*) dan pendeteksian perilaku (*Anomaly-based*) untuk mengidentifikasi ancaman keamanan.

Semua kecerdasan Snort bergantung pada *Rule Files* (Kumpulan Aturan). Aturan ini terbagi menjadi dua jenis operasional:

1. **Built-in Rules (Aturan Bawaan):**
   Saat kita menginstal Snort, ia sudah dibekali dengan paket aturan bawaan yang sangat masif (*pre-installed*). Paket ini berisi ribuan *signature* (rekam jejak) dari berbagai pola serangan siber yang sudah dikenali secara global. Dengan bermodalkan *Built-in Rules* ini saja, Snort sudah siap untuk mendeteksi sebagian besar lalu lintas berbahaya (*malicious traffic*).

2. **Custom Rules (Aturan Kustom):**
   Snort memberikan kebebasan penuh kepada Administrator Keamanan. Jika ada jaringan bisnis spesifik yang memiliki karakteristik lalu lintas unik, Administrator dapat menulis **aturan buatan sendiri (*Custom Rules*)**.
   Selain itu, untuk menghindari peringatan palsu (*False Positives*) atau menghemat kinerja sistem, Administrator juga dibebaskan untuk menonaktifkan (*disable*) aturan bawaan manapun yang dirasa tidak relevan dengan lingkungan server mereka.

### Modes of Snort

<p align="center">
<img src="../../Assets/Images/Snort-mode.png" alt="Snort Modes" width="700px"/>
</p>
