# TryHackMe - Gobuster: The Basics

- **Room Link:** [Gobuster: The Basics](https://tryhackme.com/room/gobusterthebasics)
- **Category:** Offensive Security Tooling
- **Difficulty:** Easy

## Introduction

Gobuster adalah tools _offensive security_ yang sering dipakai untuk reconnaissance. Di sini kita bakal belajar bagaimana cara tool ini bisa menemukan directory web, subdomain, sampai virtual host. (Vhost)

**Learning Objective**

- Memahami dasar dasar dari enumeration
- Cara menggunakan Gobuster untuk mencari direktori dan file tersembunyi yang ada di web
- Cara pakai Gobuster untuk menemukan **subdomain** yang tidak terekspos
- Cara pakai Gobuster buat ngintip **virtual hosts**
- caranya memilih dan memakai **wordlist** yang cocok untuk scanning

## Environment & Setup

Karena Gobuster ini dibuat pakai bahasa **Go**, dia cukup fleksibel dan bisa jalan di berbagai OS (Linux, macOS, bahkan Windows). Berikut cara install-nya agar kita bisa langsung praktek.

### 1. Kali Linux (Debian-based)

Kalau kamu pakai Kali Linux, biasanya sudah _pre-installed_. Tapi kalau belum ada (atau pakai Ubuntu/Debian lain), tinggal jalanin:

```bash
sudo apt update
sudo apt install gobuster
```

### 2. Arch Linux / BlackArch

Buat pengguna Arch , install lewat repository community:

```bash
sudo pacman -S gobuster
```

### 3. Generic Linux / macOS (via Go)

Kalau distro tidak menyediakan paket bawaan, atau kamu pakai macOS, cara paling universal adalah build langsung dari source pakai Go. Pastikan **Golang** sudah terinstall dulu ya (`go version`).

```bash
go install github.com/OJ/gobuster/v3@latest
```

_Note: Setelah install, pastikan folder `$GOPATH/bin` sudah masuk ke $PATH agar bisa dipanggil dari terminal mana saja._

### 4. Windows

Meskipun tools hacking identik sama Linux, Gobuster tetap bisa jalan di Windows karena dia cuma binary Go.

Caranya:

1.  **Cara Gampang**: Download file `.exe` dari [Halaman Rilis GitHub Resmi](https://github.com/OJ/gobuster/releases).
2.  Ekstrak filenya.
3.  Jalanin lewat CMD atau PowerShell (`.\gobuster.exe ...`).

## Gobuster: Introduction

Gobuster adalah _open-source offensive tool_ yang ditulis menggunakan bahasa **Go**. Alat ini bekerja dengan cara _brute-forcing_ untuk menemukan directory web, subdomain DNS, vhosts, Amazon S3 buckets, hingga Google Cloud Storage menggunakan wordlist tertentu.

Banyak profesional keamanan memanfaatkannya untuk _penetration testing_, _bug bounty hunting_, dan _security assessment_. Dalam fase _ethical hacking_, Gobuster berada di posisi antara **Reconnaissance** (pengintaian) dan **Scanning**.

Sebelum mendalami Gobuster, kita bahas secara singkat konsep **Enumeration** dan **Brute Force.**

- **Enumeration**

  Singkatnya, **Enumeration** (enumerasi) adalah proses list semua resources yang ada di target, entah itu bisa diakses publik atau tersembunyi.

  Contoh gampangnya: Gobuster melakukan enumerasi direktori web untuk mencari tahu ada halaman apa saja di sebuah website (misal: `/admin`, `/login`, `/backup`, dll).

- **Brute Force**

  Ini adalah teknik mencoba setiap kemungkinan yang ada sampai menemukan kecocokan. Ibarat punya 10 kunci tapi tidak tau mana yang pas, jadinya harus dicoba satu-satu sampai gemboknya kebuka.

  Gobuster menggunakan **wordlists** (daftar kata) untuk melakukan hal ini secara otomatis.

### Gobuster: Overview

Gunakan perintah `gobuster --help.` untuk menampilkan help page seperti dibawah ini

```zsh
root@user:~ gobuster --help
Usage:
  gobuster [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  dir         Uses directory/file enumeration mode
  dns         Uses DNS subdomain enumeration mode
  fuzz        Uses fuzzing mode. Replaces the keyword FUZZ in the URL, Headers and the request body
  gcs         Uses gcs bucket enumeration mode
  help        Help about any command
  s3          Uses aws bucket enumeration mode
  tftp        Uses TFTP enumeration mode
  version     shows the current version
  vhost       Uses VHOST enumeration mode (you most probably want to use the IP address as the URL parameter)

Flags:
      --debug                 Enable debug output
      --delay duration        Time each thread waits between requests (e.g. 1500ms)
  -h, --help                  help for gobuster
      --no-color              Disable color output
      --no-error              Don't display errors
  -z, --no-progress           Don't display progress
  -o, --output string         Output file to write results to (defaults to stdout)
  -p, --pattern string        File containing replacement patterns
  -q, --quiet                 Don't print the banner and other noise
  -t, --threads int           Number of concurrent threads (default 10)
  -v, --verbose               Verbose output (errors)
  -w, --wordlist string       Path to the wordlist. Set to - to use STDIN.
      --wordlist-offset int   Resume from a given position in the wordlist (defaults to 0)

Use "gobuster [command] --help" for more information about a command.
```

Isi dari help page nya yaitu:

- **usage:** Menampilkan sintaks tentang cara menggunakan perintah yang ada di gobuster

- **Available Commands:**

  Ada banyak mode yang bisa dipake (seperti enumerasi S3 bucket, Google Cloud, dll), dan kita bakal fokus ke tiga command utama:
  - `dir`: Mode enumeration direktori/file (paling sering dipakai).
  - `dns`: Mode enumeration subdomain.
  - `vhost`: Mode enumeration virtual host.

- **Flags:**

  Ini adalah opsi tambahan yang bisa kita atur buat nyesuain cara kerja command-nya (misal: nambahin jumlah threads, nge-skip error, dll).

| Flag |  Long Flag   | Keterangan                                                                                                                                                                          |
| :--: | :----------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-t` | `--threads`  | Jumlah _concurrent threads_ yang dipake buat scanning. Default-nya 10. Bisa dinaikin agar lebih kencang, tapi hati-hati ini akan memakan resource PC atau membuat server target down. |
| `-w` | `--wordlist` | Path ke file wordlist yang mau dipakai. Ini wajib diisi agar Gobuster tau "kamus" apa yang harus dicoba.                                                                            |
|      |  `--delay`   | Jeda waktu antar request. Berguna agar tidak terlalu agresif dan tidak kedetect firewall/WAF. Agar dikira traffic normal gitu lah.                                                      |
|      |  `--debug`   | Mode _troubleshooting_. Berguna banget kalau kamu nemu error aneh dan pengen tau detail apa yang salah.                                                                               |
| `-o` |  `--output`  | Opsi buat menyimpan hasil scan ke file. Wajib banget dipake pas CTF atau pentest beneran agar tidak ilang datanya.                                                                      |

### Example

contoh bagaimana kita menggunakan perintah dan flag untuk melakukan enumeration web directory:

`gobuster dir -u "http://www.yourbrokenweb.thm/" -w /usr/share/wordlists/dirb/small.txt -t 64`

- `gobuster dir`: Ini memberi tau Gobuster kalau kita mau pakai mode enumerasi direktori dan file.
- `-u "http://www.yourbrokenweb.thm/"`: Ini target URL yang mau kita scan.
- `-w /usr/share/wordlists/dirb/small.txt`:lokasi file wordlist yang bakal dipakai Gobuster buat nebak nama-nama directory. Gobuster bakal mengambil setiap kata di list ini, terus ditempel ke URL target (misal: `http://target.com/admin`, `http://target.com/login`, dst) dan mengirim request ke sana.
- `-t 64`: Ini ngeset jumlah threads jadi 64. Membuat proses scan jadi jauh lebih ngebut dibanding default-nya.

## Use Case: Directory And File Enumeration

Gobuster punya mode `dir` yang berguna untuk enumerate directory dan file di sebuah website. Ini terpakai pas lagi pentest buat mencari tau struktur direktori target.

Biasanya, struktur direktori website itu ikut standar tertentu (seperti CMS), jadi gampang ditebak pakai teknik _Brute Force_ pakai wordlist.

Contohnya, struktur direktori WordPress biasanya seperti ini:

```bash
root@user:~# tree -L 3 -d
.
└── html
    └── wordpress
        ├── wp-admin
        ├── wp-content
        └── wp-includes
```

Gobuster itu _powerful_ karena dia tidak cuma mengecek ada/enggak-nya folder, tapi juga ngembaliin **Status Codes**. Dari kode ini, kita bisa langsung tau apakah directory itu bisa diakses sama kita (sebagai user luar) atau tidak.

### Help

Terkadang kita butuh opsi lebih lanjut untuk hasil scan kita. Berikut beberapa flag penting yang sering dipakai di mode `dir`:

| Flag |         Long Flag          | Keterangan                                                                                                                |
| :--: | :------------------------: | ------------------------------------------------------------------------------------------------------------------------- |
| `-c` |        `--cookies`         | Buat nambahin cookie di setiap request (misal session ID).                                                                |
| `-x` |       `--extensions`       | Spesifikin ekstensi file yang mau dicari (misal: `.php`, `.txt`, `.html`).                                                |
| `-H` |        `--headers`         | Nambahin header HTTP custom di setiap request.                                                                            |
| `-k` |   `--no-tls-validation`    | Skip verifikasi sertifikat SSL/TLS. Wajib dipake kalau targetnya pakai _self-signed certificate_ (sering kejadian di CTF). |
| `-n` |       `--no-status`        | Tidak menampilkan status code di output agar terminal lebih bersih.                                                            |
| `-P` |        `--password`        | Memasukkan password buat _Basic Authentication_ (dipake bareng `-U`).                                                        |
| `-s` |      `--status-codes`      | Menentukan status code mana saja yang mau ditampilin (whitelist). Misal cuma mau melihat `200`.                                  |
| `-b` | `--status-codes-blacklist` | Kebalikannya `-s`, ini buat nyembunyiin status code tertentu (blacklist). Misal tidak mau melihat `404` (Not Found).           |
| `-U` |        `--username`        | Memasukkan username buat _Basic Authentication_.                                                                             |
| `-r` |     `--followredirect`     | Agar Gobuster ngikutin kalau ada redirect (status `301` atau `302`).                                                      |

### How To Use dir Mode

Buat jalanin Gobuster di mode `dir`, format basic-nya gini:

`gobuster dir -u "http://www.yourbrokenweb.thm" -w /path/to/wordlist`

Wajib pakai flag `-u` dan `-w` selain keyword `dir`.

Contoh:

`gobuster dir -u "http://www.yourbrokenweb.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r`

Penjelasannya:

- `gobuster dir`: Mode enumeration folder/file.
- `-u http://www.yourbrokenweb.thm`:
  - Ini adalah _base path_ dimana Gobuster bakal mulai mencari. Kalau misal set URL-nya ke `/resources`, dia bakal mencari di dalem folder `resources` itu.
  - **Penting**: Wajib tulis protokolnya (`http` atau `https`). Kalau salah, scan-nya bakal gagal.
  - Bisa pakai IP atau Hostname. Tapi ingat, satu IP bisa nge-host banyak web (Virtual Hosting), jadi lebih aman pakai Hostname agar tidak salah alamat.
  - Gobuster **tidak** melakukan scan secara rekursif secara default. Jadi kalau nemu folder `admin/`, dia tidak bakal otomatis mencari isi dalem `admin/` kecuali kita suruh.
- `-w ...`: Path ke file wordlist yang isinya daftar kata buat ditebak.
- `-r`: Ngikutin redirect. Kalau dapet status 301 (Pindah), dia bakal mengecek ke alamat barunya.

Contoh kedua, kalau mau mencari file spesifik (misal PHP atau JS), kita bisa pakai flag `-x`:

`gobuster dir -u "http://www.yourbrokenweb.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js`

## Use Case: DNS Subdomain Enumeration

Mode selanjutnya yang bakal kita bahas adalah mode `dns`. Mode ini memungkinkan Gobuster buat nge-bruteforce subdomain.

saat sedang pentest, mengecheck subdomain dari domain utama target itu hukumnya **wajib**. Kenapa? Karena bisa jadi domain utamanya aman (sudah dipatch), tapi subdomainnya broken

Celah keamanan di subdomain bisa jadi pintu masuk buat nge-hack sistem yang ada di web secara keseluruhan.

Contoh simpelnya:
Misal `yourbrokenweb` punya domain `yourbrokenweb.thm` dan `mobile.yourbrokenweb.thm`.
Bisa jadi di `mobile.yourbrokenweb.thm` ada vulnerability yang tidak ada di `yourbrokenweb.thm`.

Makanya, penting banget buat mencari subdomain pakai Gobuster

### Help

Berikut adalah beberapa flag yang sering dipake di mode `dns`:

| Flag |   Long Flag    | Keterangan                                                                                         |
| :--: | :------------: | -------------------------------------------------------------------------------------------------- |
| `-c` | `--show-cname` | Menampilkan CNAME records (alias domain), flag ini tidak bisa dipake bareng flag `-i`.                  |
| `-i` |  `--show-ips`  | Menampilkan alamat IP dari domain/subdomain yang ketemu. Berguna banget buat tau hostingannya dimana. |
| `-r` |  `--resolver`  | Pakai custom DNS server buat resolving. Berguna kalau DNS default lemot atau banyak filtering.      |
| `-d` |   `--domain`   | Domain target yang mau di-scan. Wajib diisi                                                        |

### How to Use dns Mode

Sama seperti mode `dir`,untuk menjalankan mode `dns` formatnya juga simpel:

`gobuster dns -d yourbrokenweb.thm -w /path/to/wordlist`

Perhatikan bedanya, kalau mode `dir` pakai `-u` (URL), di sini kita pakai `-d` (Domain).

Contoh real-nya:

```bash
gobuster dns -d yourbrokenweb.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

Penjelasannya:

- `gobuster dns`: Mode enumeration subdomain.
- `-d yourbrokenweb.thm`: Target domain yang mau kita scan (tanpa http/https).
- `-w ...`: Path ke wordlist. Di contoh ini pakai SecLists top 1 million subdomains (populer banget nih). Gobuster bakal nyobain satu-satu kata di list itu buat jadi subdomain (misal: `www.yourbrokenweb.thm`, `mail.yourbrokenweb.thm`, dll).

Outputnya bakal seperti gini:

```zsh
root@user:~ gobuster dns -d yourbrokenweb.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Domain:     yourbrokenweb.thm
[+] Threads:    10
[+] Timeout:    1s
[+] Wordlist:   /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
===============================================================
Starting gobuster in DNS enumeration mode
===============================================================
Found: www.yourbrokenweb.thm
Found: shop.yourbrokenweb.thm
Found: academy.yourbrokenweb.thm
Found: primary.yourbrokenweb.thm

Progress: 4989 / 4990 (99.98%)
===============================================================
Finished
===============================================================
```

Nah, dari hasil di atas kita nemu subdomain seperti `shop`, `academy`, sama `primary` yang mungkin tidak keliatan dari halaman utama.

## Use Case: Vhost Enumeration

Mode terakhir yang bakal kita bahas adalah mode `vhost`. Mode ini mengizinkan Gobuster buat nge-bruteforce virtual host.

Virtual host itu beda website yang jalan di mesin yang sama. Kadang mereka keliatan seperti subdomain, tapi jangan ketipu Virtual host itu IP-based dan jalan di server yang sama. Subdomain itu di setup di DNS.

> [!NOTE]
> **Apa itu Virtual Hosting?**
>
> **Virtual host** (Vhost) adalah metode untuk nge-host banyak website di server atau alamat IP yang sama.
>
> Setiap situs bisa punya nama domain dan konfigurasinya sendiri, jadi konten yang disajikan bisa beda-beda tergantung domain mana yang diminta. Teknik ini sering banget dipake di web hosting buat memaksimalkan resource server dan mempermudah manajemen.

Bedanya mode `vhost` sama `dns` ada di cara kerjanya pas scanning:

- Mode `vhost` bakal menuju ke URL yang dibuat dari gabungan HOSTNAME (`-u`) sama isi wordlist.
- Mode `dns` bakal ngelakuin DNS lookup ke FQDN yang dibuat dari gabungan domain name (`-d`) sama isi wordlist.

### Help

Berikut adalah beberapa flag yang sering dipake di mode `vhost`:

| Short Flag |     Long Flag      | Description                                                                                                                     |
| :--------: | :----------------: | ------------------------------------------------------------------------------------------------------------------------------- |
|    `-u`    |      `--url`       | Target URL utama yang mau di-scan. Ingat, ini beda sama mode `dns` yang pakai `-d`.                                              |
|            | `--append-domain`  | Nambahin domain utama ke setiap kata di wordlist (misal: `word.yourbrokenweb.thm`).                                             |
|    `-m`    |     `--method`     | Menentukan HTTP method request-nya (misal: `GET`, `POST`). Default-nya `GET`.                                                   |
|            |     `--domain`     | Menambahkan domain ke entry wordlist buat membuat hostname yang valid (berguna kalau tidak disediain secara eksplisit).             |
|            | `--exclude-length` | Filter hasil berdasarkan panjang response body. Berguna banget buat nge-filter halaman error default yang ukurannya sama semua. |

### How To Use vhost Mode

Buat jalanin Gobuster di mode `vhost`, perintah dasarnya seperti gini:

`gobuster vhost -u "http://yourbrokenweb.thm" -w /path/to/wordlist`

Tapi, kekuatan utama `vhost` adalah kemampuannya buat nge-scan IP address tapi seolah-olah kita mengakses domain tertentu (Host Header injection).

Contoh skenario: Kita nemu IP `10.10.10.10` dan kita curiga IP ini nge-host `yourbrokenweb.thm` beserta subdomainnya.

Command-nya jadi:

```bash
gobuster vhost -u "http://10.10.10.10" --domain yourbrokenweb.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320
```

Penjelasannya:

- `-u "http://10.10.10.10"`: Kita nembak langsung ke IP servernya.
- `--domain yourbrokenweb.thm`: Kita kasih tau Gobuster buat nempelin domain ini di Host Header.
- `--append-domain`: Gabungin kata di wordlist sama domain. Jadi kalau wordlist isinya `admin`, bakal jadi `admin.yourbrokenweb.thm`.
- `--exclude-length`: Ini penting! Biasanya kalau vhost tidak valid, server bakal balikin halaman default yang ukurannya sama. Kita filter ukuran itu agar tidak menuhin layar.

Outputnya bakal seperti gini:

```bash
root@tryhackme:~# gobuster vhost -u "http://10.10.10.10" --domain yourbrokenweb.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:            http://10.10.10.10
[+] Method:         GET
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
[+] User Agent:     gobuster/3.6
[+] Timeout:        10s
[+] Append Domain:  true
[+] Exclude Length: 250,254,263...
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Found: blog.yourbrokenweb.thm Status: 200 [Size: 1493]
Found: shop.yourbrokenweb.thm Status: 200 [Size: 2983]
Found: www.yourbrokenweb.thm Status: 200 [Size: 84352]
Found: academy.yourbrokenweb.thm Status: 200 [Size: 434]
Progress: 4989 / 4990 (99.98%)
===============================================================
Finished
===============================================================
```

kita nemu beberapa subdomain seperti `blog`, `shop`, dan `academy` yang sebenernya 'numpang' di IP yang sama!
