# TryHackMe - Gobuster: The Basics

- **Room Link:** [Gobuster: The Basics](https://tryhackme.com/room/gobusterthebasics)
- **Category:** Offensive Security Tooling
- **Difficulty:** Easy

## Task 1: Introduction

Gobuster adalah tools _offensive security_ yang sering dipakai untuk reconnaissance. Di sini kita bakal belajar bagaimana cara tool ini bisa menemukan directory web, subdomain, sampai virtual host.

**Learning Objective**

- Memahami dasar dasar dari enumeration
- Cara menggunakan Gobuster untuk mencari direktori dan file tersembunyi yang ada di web
- Cara pakai Gobuster untuk menemukan **subdomain** yang gak terekspos
- Cara pakai Gobuster buat ngintip **virtual hosts**
- caranya memilih dan memakai **wordlist** yang pas untuk scanning

## Task 2: Environment & Setup

Karena Gobuster ini dibuat pakai bahasa **Go**, dia cukup fleksibel dan bisa jalan di berbagai OS (Linux, macOS, bahkan Windows). Berikut cara install-nya biar kita bisa langsung praktek.

### 1. Kali Linux (Debian-based)

Kalau lu pake Kali Linux, biasanya udah _pre-installed_. Tapi kalau belum ada (atau pake Ubuntu/Debian lain), tinggal jalanin:

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

Kalau distro gak menyediakan paket bawaan, atau lu pake macOS, cara paling universal adalah build langsung dari source pake Go. Pastikan **Golang** udah terinstall dulu ya (`go version`).

```bash
go install github.com/OJ/gobuster/v3@latest
```

_Note: Setelah install, pastikan folder `$GOPATH/bin` udah masuk ke $PATH biar bisa dipanggil dari terminal mana aja._

### 4. Windows

Meskipun tools hacking identik sama Linux, Gobuster tetap bisa jalan di Windows karena dia cuma binary Go.

Caranya:

1.  **Cara Gampang**: Download file `.exe` dari [Halaman Rilis GitHub Resmi](https://github.com/OJ/gobuster/releases).
2.  Ekstrak filenya.
3.  Jalanin lewat CMD atau PowerShell (`.\gobuster.exe ...`).

## Task 3: Gobuster: Introduction

Gobuster adalah _open-source offensive tool_ yang ditulis menggunakan bahasa **Go**. Alat ini bekerja dengan cara _brute-forcing_ untuk menemukan directory web, subdomain DNS, vhosts, Amazon S3 buckets, hingga Google Cloud Storage menggunakan wordlist tertentu.

Banyak profesional keamanan memanfaatkannya untuk _penetration testing_, _bug bounty hunting_, dan _security assessment_. Dalam fase _ethical hacking_, Gobuster berada di posisi antara **Reconnaissance** (pengintaian) dan **Scanning**.

Sebelum mendalami Gobuster, kita bahas secara singkat konsep **Enumeration** dan **Brute Force.**

- **Enumeration**

  Singkatnya, **Enumeration** (enumerasi) adalah proses list semua resources yang ada di target, entah itu bisa diakses publik atau tersembunyi.

  Contoh gampangnya: Gobuster melakukan enumerasi direktori web untuk mencari tahu ada halaman apa saja di sebuah website (misal: `/admin`, `/login`, `/backup`, dll).

- **Brute Force**

  Ini adalah teknik mencoba setiap kemungkinan yang ada sampai menemukan kecocokan. Ibarat punya 10 kunci tapi gak tau mana yang pas, jadinya harus dicoba satu-satu sampe gemboknya kebuka.

  Gobuster menggunakan **wordlists** (daftar kata) untuk melakukan hal ini secara otomatis.

### Gobuster: Overview

```zsh
root@user:~# gobuster --help
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
