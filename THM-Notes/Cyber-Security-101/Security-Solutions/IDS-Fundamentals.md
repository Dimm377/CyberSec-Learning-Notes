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
