# TryHackMe - Hydra

- **Room Link:** [Hydra](https://tryhackme.com/room/hydra)
- **Category:** Offensive Security Tooling
- **Difficulty:** Easy

## Introduction

### What is Hydra?

**Hydra** adalah tools password cracking yang menggunakan metode _brute force_. Singkatnya, ini adalah tools buat "maksa masuk" ke sistem dengan nebak password secara cepat.

Hydra bisa jalanin _brute force_ attack ke berbagai layanan autentikasi. Bayangin kamu lagi nyoba nebak password seseorang secara manual di layanan seperti **SSH**, **Web Application Form**, **FTP**, atau **SNMP**. Nah, Hydra ini ngelakuin hal yang sama tapi jauh lebih cepat karena dia bakal nyoba login pakai daftar password (wordlist) yang sudah kita siapkan.

Tujuannya simpel: Nemuin password yang bener.

Menurut repositori resminya, Hydra support alias bisa nge-brute force BANYAK banget protokol, contohnya:
"Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, TeamSpeak (TS2), Telnet, VMware-Auth, VNC, dan XMPP."

Buat info lebih lengkap soal opsi tiap protokol, bisa cek di [Kali Hydra tool page](https://www.kali.org/tools/hydra/).

### Kenapa Password Kuat Itu Penting?

Tool seperti Hydra ini menunjukkan kenapa kita WAJIB banget pakai password yang kuat.

Kalau passwordnya misal:

- password umum (seperti `password123`, `qwerty`, `admin123`),
- Tidak ada karakter spesialnya,
- Atau kurang dari 8 karakter,

Maka password itu bakal gampang banget ditebak. Sebuah wordlist (daftar kata) bisa berisi seratus juta password umum. Jadi kalau kita pakai password pasaran, Hydra bakal nemuin itu dalam sekejap.

Seringkali, CCTV atau Web Frameworks pakai default credentials seperti `admin:password`. Ini jelas tidak aman sama sekali dan bakal jadi sasaran empuk buat tools seperti gini.

Jadi, ganti default password dan membuat kombinasi yang rumit agar susah di hack

## Using Hydra

Sebenernya Hydra ini sudah masuk ke official repository banyak distro Linux, jadi install nya mudah.

- **Ubuntu / Kali / Debian**:

  ```bash
  sudo apt install hydra
  ```

- **Fedora**:

  ```bash
  sudo dnf install hydra
  ```

- **Arch Linux / BlackArch**:
  ```bash
  sudo pacman -S hydra
  ```

Kalau mau compile sendiri atau cari versi paling baru, bisa langsung cek di repository resminya: [THC-Hydra Repository](https://github.com/vanhauser-thc/thc-hydra).

### Hydra Commands

Opsi yang kita pakai di Hydra itu tergantung banget sama servis (protokol) apa yang mau kita serang. Contohnya, kalau kita mau nge-brute force **FTP** dengan username `user` dan password list `passlist.txt`, command-nya bakal seperti gini:

`hydra -l user -P passlist.txt ftp://10.48.163.55`

Nah, buat mesin yang lagi kita deploy ini, berikut adalah command buat pakai Hydra di **SSH** dan **Web Form** (POST method).

#### SSH

Format dasarnya:

`hydra -l <username> -P <full path to pass> 10.48.163.55 -t 4 ssh`

| Option | Description                                                  |
| :----- | :----------------------------------------------------------- |
| `-l`   | Spesifik username (SSH) buat login.                          |
| `-P`   | Memberi tau Hydra kalau kita pakai list password (wordlist).   |
| `-t`   | jumlah threads yang jalan bersamaan (paralel / multithread). |

Contoh real-nya: `hydra -l root -P passwords.txt 10.48.163.55 -t 4 ssh`

Artinya:

- Hydra bakal pakai `root` sebagai username buat login **ssh**.
- Dia bakal nyobain semua password yang ada di file `passwords.txt`.
- Bakal ada 4 threads yang jalan bersamaan (paralel), ditandain sama flag `-t 4`. Agar lebih ngebut dikit tapi tidak membuat server down.

### Post Web Form

Kita juga bisa pakai Hydra buat brute force form login di sebuah website. Tapi sebelumnya, kita harus tau dulu request apa yang dipake (GET atau POST). Cara paling gampang ya buka **Developer Tools** (Network Tab) di browser pas lagi coba login, terus melihat request nya / pakai tools yang namanya **Burp Suite**

Kalau ternyata pakai **POST**, command-nya kira-kira bakal seperti ini:

`hydra -l <username> -P <passlist> 10.48.163.55 http-post-form "<path>:<login_credentials>:<invalid_response>"`

Nah, bagian yang agak ribet itu string panjang di belakang `http-post-form`. Ini formatnya:

| Placeholder           | Description                                                                                                |
| :-------------------- | :--------------------------------------------------------------------------------------------------------- |
| `<path>`              | URL path tempat form login berada, misal `/login.php` atau `/`.                                            |
| `<login_credentials>` | Body request yang dikirim pas login. Di sini kita ganti username jadi `^USER^` dan password jadi `^PASS^`. |
| `<invalid_response>`  | String unik yang muncul di response kalau login **GAGAL**. Misal `F=incorrect` atau `Login Failed`.        |

**Contoh Command Lengkap:**

`hydra -l root -P passwords.txt 10.48.163.55 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V`

Penjelasannya:

- `http-post-form`: Modul yang dipake buat POST request.
- `"/`: Path login page-nya ada di root (`/`).
- `:username=^USER^&password=^PASS^`: Ini body request-nya. `^USER^` bakal diganti `root`, dan `^PASS^` bakal diganti sama isi `passwords.txt` satu-satu.
- `:F=incorrect"`: Kalau di response ada kata `incorrect`, berarti password SALAH. Hydra bakal lanjut ke password berikutnya.
- `-V`: Verbose mode, agar keliatan prosesnya.

_Note: Kalau web servernya jalan di port yang tidak standar (bukan 80/443), tambahin flag `-s <port>`._

---

## Attack Flow Awareness

Hydra beroperasi di tahap **Initial Access** dalam _attack chain_:

| Tahap | Peran Hydra |
| ----- | ----------- |
| **Reconnaissance** | Setelah Nmap menemukan port SSH/FTP/HTTP terbuka, Hydra menjadi langkah logis berikutnya |
| **Initial Access** | Brute force credentials untuk mendapatkan akses pertama ke sistem target |
| **Lateral Movement** | Bisa digunakan lagi untuk brute force layanan internal setelah mendapatkan akses di satu mesin |

**Perspektif Blue Team:**
- **Deteksi:** Monitoring _failed login attempts_ yang massal dalam waktu singkat (log analysis via SIEM).
- **Mitigasi:** Implementasi _rate limiting_, _account lockout policy_, dan _fail2ban_ untuk memblokir IP setelah sejumlah percobaan gagal.
- **MFA:** Multi-Factor Authentication membuat brute force menjadi tidak berguna meskipun password ditemukan.

---

## For Real World Relevance

- **Pentest Engagement:** Hydra sering dipakai di fase awal pentest untuk menguji kekuatan password policy organisasi. Default credentials di router, CCTV, dan IoT devices adalah target paling umum.
- **Credential Stuffing:** Di dunia nyata, attacker tidak hanya brute force random — mereka menggunakan database password yang bocor dari breach sebelumnya dan mencobanya di layanan lain (_credential stuffing_). Hydra bisa disimulasikan untuk skenario ini.
- **Compliance Audit:** Banyak standar keamanan (seperti PCI-DSS) mengharuskan organisasi menguji ketahanan passwordnya. Hydra menjadi salah satu tool yang dipakai auditor.

---

## Questions

1. Kenapa flag `-t 4` direkomendasikan untuk SSH dan bukan angka yang lebih tinggi?
2. Apa perbedaan mendasar antara _brute force_ dan _credential stuffing_? Teknik mana yang lebih berbahaya di dunia nyata dan kenapa?
3. Jika target menerapkan _account lockout_ setelah 5 percobaan gagal, strategi apa yang bisa dipakai attacker untuk tetap melakukan brute force tanpa mengunci akun?
4. Bagaimana cara mendeteksi serangan Hydra dari sisi Blue Team menggunakan SIEM?
