# TryHackMe - Hydra

- **Room Link:** [Hydra](https://tryhackme.com/room/hydra)
- **Category:** Offensive Security Tooling
- **Difficulty:** Easy

## Task 1: Introduction

### What is Hydra?

**Hydra** adalah tools password cracking yang menggunakan metode _brute force_. Singkatnya, ini adalah tools buat "maksa masuk" ke sistem dengan nebak password secara cepat.

Hydra bisa jalanin _brute force_ attack ke berbagai layanan autentikasi. Bayangin lu lagi nyoba nebak password seseorang secara manual di layanan kayak **SSH**, **Web Application Form**, **FTP**, atau **SNMP**. Nah, Hydra ini ngelakuin hal yang sama tapi jauh lebih cepet karena dia bakal nyoba login pake daftar password (wordlist) yang udah kita siapkan.

Tujuannya simpel: Nemuin password yang bener.

Menurut repositori resminya, Hydra support alias bisa nge-brute force BANYAK banget protokol, contohnya:
"Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, TeamSpeak (TS2), Telnet, VMware-Auth, VNC, dan XMPP."

Buat info lebih lengkap soal opsi tiap protokol, bisa cek di [Kali Hydra tool page](https://www.kali.org/tools/hydra/).

### Kenapa Password Kuat Itu Penting?

Tool kayak Hydra ini menunjukkan kenapa kita WAJIB banget pake password yang kuat.

Kalau passwordnya misal:

- password umum (kayak `password123`, `qwerty`, `admin123`),
- Gak ada karakter spesialnya,
- Atau kurang dari 8 karakter,

Maka password itu bakal gampang banget ditebak. Sebuah wordlist (daftar kata) bisa berisi seratus juta password umum. Jadi kalau kita pake password pasaran, Hydra bakal nemuin itu dalam sekejap.

Seringkali, CCTV atau Web Frameworks pake default credentials kayak `admin:password`. Ini jelas gak aman sama sekali dan bakal jadi sasaran empuk buat tools kayak gini.

Jadi, ganti default password dan bikin kombinasi yang rumit biar susah di hack
