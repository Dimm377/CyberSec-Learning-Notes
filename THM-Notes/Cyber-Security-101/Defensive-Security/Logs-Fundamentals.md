# TryHackMe: Logs Fundamentals

- **Room Link:** [Logs Fundamentals](https://tryhackme.com/room/logsfundamentals)
- **Kategori:** Defensive Security
- **Difficulty:** easy

---

Room ini bakal ngebahas tuntas soal apa itu _Logs_, kenapa penting banget, dan gimana cara kita ngebaca jejak-jejak digital.

## Task 1: Introduction to Logs

Attacker pasti berusaha sebisa mungkin ngehapus jejak (meninggalkan _trace_ sesedikit mungkin) di sistem korban biar nggak ketahuan. Tapi kok tim Security tetep bisa mengetahui gimana serangannya terjadi, bahkan nemuin siapa dalangnya?

**Analoginya gini:**
Bayangin ada polisi lagi investigasi kasus orang hilang di gubuk tengah hutan salju.
Polisi nemuin petunjuk: pintu kayu hacur, atap jebol, jejak sepatu di salju, sama rekaman CCTV dari tetangga kejauhan. Dengan nyatuin kepingan-kepingan petunjuk (_traces_) ini, polisi akhirnya bisa tau kronologi dan siapa penjahatnya.

Nah, **Logs** di dunia cyber itu ibarat kepingan-kepingan petunjuk yang ditinggalin sama maling tadi. Kumpulan _traces_ dari berbagai sumber bakal digabungin buat jadi petunjuk menuju si kriminal.

Petunjuk (_traces_) ini punya peran super besar banget di dalam investigasi.
Sekarang kalau kejadiannya ada di dalam perangkat digital, di mana kita bisa nemuin _traces_ ini buat ngebantu investigasi lebih lanjut?

Jawabannya: **Logs**. Logs itu ibarat jejak kaki digital (_digital footprints_) yang ditinggalin setiap kali ada aktivitas apa pun (mau itu aktivitas wajar atau serangan jahat sekalipun). Dengan ngebaca logs ini, ngelacak sebuah aktivitas dan siapa dalang di baliknya bakal jadi jauh lebih gampang.

### Use Cases of Logs

Secara garis besar, ini dia beberapa area utama yang sangat bergantung sama logs buat bisa jalan dengan bener:

| Use Case | Description |
| :--- | :--- |
| **Security Events Monitoring** | Kalau dipantau secara langsung (_real-time_), logs ngebantu sistem buat ngedeteksi anomali (tingkah laku yang nggak wajar atau mencurigakan). |
| **Incident Investigation and Forensics** | Karena logs itu rekam jejak setiap aktivitas, tim Security bakal dapet info sedetail-detailnya pas lagi nyari akar masalah penyebab suatu insiden (Root Cause Analysis). |
| **Troubleshooting** | Sistem yang error atau aplikasi _crash_ juga kerasa banget kebantunya. Berdasarkan logs, masalah lebih gampang didiagnosa dan diperbaiki. |
| **Performance Monitoring** | Selain keamanan, logs juga nyediain _insight_ / wawasan berharga soal bagus nggaknya performa aplikasi waktu lagi jalan. |
| **Auditing and Compliance** | Logs penting banget buat urusan kepatuhan peraturan (_compliance_). Punya jejak log mempermudah organisasi buktiin kalau mereka bersih dan legal dalam beraktivitas. |

### Learning Objectives

Setelah nyelesaiin _room_ ini, kita bakal belajar soal:
- Apa aja tipe-tipe logs.
- Gimana cara menganalisa logs.
- Melakukan analisa pada Windows Event logs.
- Melakukan analisa pada Web Access logs.

---

### 🧠 Uji Ingatan (Active Recall)

- Bisa kasih salah satu contoh kenapa _Auditing and Compliance_ sangat butuh campur tangan _Logs_?

## Task 2: Types of Logs

Dari awal kita udah tau betapa pentingnya _logs_. Tapi tunggu dulu, ada tantangan besar di sini. Bayangin kamu lagi menelusuri atau nginvestigasi _error_ (atau bahkan serangan dari _attacker_) di sebuah server. Kamu buka satu file _log_ utama server tersebut, lalu BOOM! Kamu disuguhi puluhan ribu baris teks aneh yang isinya campur aduk dari berbagai macam _service_ sistem operasi tersebut. Pastinya kamu bakal _lost_ (kebingungan) nyari mana yang penting. Ini persis kayak disuruh nyari selembar struk ATM yang jatuh di tengah tumpukan gunungan sampah.

**Solusinya: (Pengelompokan)**

_Logs_ itu wajib banget dipecah dan dikelompokin ke dalam beberapa kategori terpisah berdasarkan "jenis informasi" yang mereka rekam. Dengan begitu, kamu cuma perlu nyari _log_ di "keranjang" log yang tepat sesuai sama insiden yang terjadi.

**Sebagai contoh:**
Kamu mau nyari _log_ rekaman kejadian orang nyoba _successful logins_ kemarin malem di OS Windows. Daripada kamu ngebaca ratusan ribu log yang isinya _random_ dari seluruh aktivitas OS secara keseluruhan, kamu cukup meluncur aja langsung ke bagian **Security Logs**. Informasi login-nya pasti nyangkut di situ. Simpel, presisi, dan hemat waktu investigasi secara efisien!

![Jenis-jenis Logs](../../Assets/Images/logs.png)

Berdasarkan klasifikasi umumnya, ada beberapa tipe _logs_ utama yang biasa kita gunakan sebagai "keranjang sakti" untuk mencari _traces_ saat kita berburu insiden:

| Tipe Log (_Log Type_) | Kegunaan Utama (_Usage_) | Contoh _Events_ (_Traces_) |
| :--- | :--- | :--- |
| **System Logs** | Sangat krusial buat diagnosa masalah di _Operating System_ (OS). OS bakal merekam semua aktivitas intinya di sini. (Bagi _attacker_, log ini sering dihapus/dimatikan biar aktivitas tak wajarnya ga tertangkap). | - _System Startup/Shutdown events_<br>- _Driver Loading events_<br>- _System Error events_<br>- _Hardware events_ |
| **Audit Logs** | Ibarat "CCTV", fokus utamanya merakam pemenuhan aturan _compliance_ (kepatuhan laporan) seperti perubahan pada sistem oleh pengguna. Ini log super *vital* bagi tim _Defensive_ untuk memantau aktivitas tak wajar dan menegakkan aturan (_Policy_). | - _Data Access events_<br>- _System Change events_<br>- _User Activity events_<br>- _Policy Enforcement events_ |
| **Security Logs** | _The holy grail_ buat investigasi insiden keamanan! Merekam semua aktivitas yang berkaitan langsung dengan autentikasi, otorisasi, dan hal-hal yang bersinggungan langsung dengan proteksi sistem keamanan. | - _Authentication events_ (Gagal/Suksesnya suatu _login_)<br>- _Authorization events_<br>- _Security Policy changes_<br>- _User Account changes_<br>- _Abnormal Activity events_ |
| **Network Logs** | Log dewa buat nganalisa _traffic_ masuk/keluar di jaringan. Kalau ada anomali lalu-lintas data (seperti _beaconing malware_ atau *exfiltration* data yang ditranfser ke _Command and Control / C2 Server_ milik _attacker_), semuanya terekam di sini. | - _Incoming/Outgoing Network Traffic_<br>- _Network Connection Logs_<br>- _Network Firewall Logs_ |
| **Access Logs** | Mencatat sumber rinci secara spesifik setiap kali ada sebuah entitas yang me-_request_ akses ke berbagai _resource_ dari layanan (_services_) tertentu, seperti *web server* atau *database*. | - _Webserver Access Logs_<br>- _Database Access Logs_<br>- _Application Access Logs_<br>- _API Access Logs_ |
| **Application Logs** | Merekam dinamika status yang terjadi spesifik di dalam sebuah aplikasi, entah itu interaksi yang dilakukan _user_ secara langsung maupun proses aplikasi yang berjalan di latar belakang tanpa disadari (_non-interactive_). | - _User Interaction events_<br>- _Application Changes events_<br>- _Application Update events_<br>- _Application Error events_ |

---

### Question

- Coba sebutin alasan logis paling utama kenapa _logs_ itu wajib dipisah-pisah golongannya?
- Kalau insting _OffSec/Forensic_ kamu jalan, kalau kita mau ngecek riwayat siapa aja yang berhasil (atau gagal) nyoba otentikasi login ke _remote server_ kita, kategori _log_ apa yang menurutmu bakal kamu bedah duluan?
