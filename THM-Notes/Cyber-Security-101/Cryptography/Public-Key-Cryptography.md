# TryHackMe: Public Key Cryptography Basics

**Room Link:** [TryHackMe](https://tryhackme.com/room/publickeycrypto)  
**Category:** Cryptography  
**Difficulty:** Easy  
**Duration:** ~60 minutes

---

## Overview

Room ini membahas fundamental dari **Public Key Cryptography (Asymmetric Cryptography)**, termasuk bagaimana RSA bekerja dan aplikasinya dalam teknologi seperti SSH. Berbeda dengan symmetric encryption yang menggunakan satu key untuk encrypt dan decrypt, asymmetric encryption menggunakan **sepasang key** yang berbeda.

---

## Task 1: Introduction to Public Key Cryptography

### Symmetric vs Asymmetric Encryption

| Aspect | Symmetric | Asymmetric |
|--------|-----------|------------|
| Keys | 1 shared key | 2 keys (public + private) |
| Speed | Faster | Slower |
| Use Case | Bulk data encryption | Key exchange, digital signatures |
| Example | AES, DES | RSA, ECC, Diffie-Hellman |

### Key Concepts

- **Public Key**: Dapat dibagikan ke siapapun, digunakan untuk **encrypt** data
- **Private Key**: Harus **dirahasiakan**, digunakan untuk **decrypt** data
- Data yang di-encrypt dengan public key **hanya bisa** di-decrypt dengan private key yang sesuai

---

## Task 2: RSA (Rivest-Shamir-Adleman)

### Cara Kerja RSA

RSA menggunakan **dua bilangan prima besar** untuk generate key pair:

1. **Key Generation:**
   - Pilih dua bilangan prima besar: `p` dan `q`
   - Hitung `n = p × q` (modulus)
   - Hitung `φ(n) = (p-1)(q-1)` (Euler's totient)
   - Pilih `e` (public exponent), biasanya 65537
   - Hitung `d` (private exponent) dimana `d × e ≡ 1 (mod φ(n))`

2. **Public Key:** `(n, e)`
3. **Private Key:** `(n, d)`

### Encryption & Decryption

```
Encryption:  ciphertext = message^e mod n
Decryption:  message = ciphertext^d mod n
```

### Keamanan RSA

- Keamanan RSA bergantung pada **kesulitan factoring bilangan besar**
- Jika `n` bisa di-factor menjadi `p` dan `q`, private key bisa dihitung
- Key size minimum yang aman: **2048-bit** (4096-bit untuk high security)

### Tools untuk RSA

```bash
# Generate RSA key pair dengan OpenSSL
openssl genrsa -out private.pem 2048
openssl rsa -in private.pem -pubout -out public.pem

# Python - RSA dengan pycryptodome
from Crypto.PublicKey import RSA
key = RSA.generate(2048)
private_key = key.export_key()
public_key = key.publickey().export_key()
```

---

## Task 3: SSH Keys & Authentication

### Apa itu SSH Key Authentication?

SSH (Secure Shell) menggunakan **RSA keys by default** untuk authentication. Lebih aman daripada password karena:
- Tidak bisa di-brute force seperti password
- Private key tidak pernah dikirim ke server

### Generate SSH Key Pair

```bash
# Generate SSH key pair
ssh-keygen -t rsa -b 4096

# Output:
# - ~/.ssh/id_rsa (private key)
# - ~/.ssh/id_rsa.pub (public key)
```

### File & Directory Penting

| Path | Description |
|------|-------------|
| `~/.ssh/` | Default directory untuk SSH keys |
| `~/.ssh/id_rsa` | Private key (JANGAN dibagikan!) |
| `~/.ssh/id_rsa.pub` | Public key |
| `~/.ssh/authorized_keys` | List public keys yang diizinkan connect |
| `~/.ssh/known_hosts` | Fingerprint dari server yang pernah diconnect |

### Setup SSH Key Authentication

```bash
# 1. Generate key di LOCAL machine
ssh-keygen -t rsa -b 4096

# 2. Copy public key ke server
ssh-copy-id user@server
# ATAU manual:
cat ~/.ssh/id_rsa.pub | ssh user@server "cat >> ~/.ssh/authorized_keys"

# 3. Connect menggunakan private key
ssh -i ~/.ssh/id_rsa user@server
```

---

## Task 4: SSH Key Permissions

### Pentingnya Permissions

SSH **sangat strict** soal file permissions. Jika permissions salah, SSH akan **menolak** menggunakan key tersebut!

### Permissions yang Benar

```bash
# Set correct permissions
chmod 700 ~/.ssh              # Directory: hanya owner
chmod 600 ~/.ssh/id_rsa       # Private key: read/write owner only
chmod 644 ~/.ssh/id_rsa.pub   # Public key: readable by others
chmod 600 ~/.ssh/authorized_keys
chmod 644 ~/.ssh/known_hosts
```

### Common Error

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/home/user/.ssh/id_rsa' are too open.
```

**Fix:** `chmod 600 ~/.ssh/id_rsa`

---

## Task 5: SSH Key Passphrase

### Apa itu Passphrase?

Passphrase adalah **password tambahan** yang mengenkripsi private key. Jadi meskipun attacker dapat private key file, mereka masih butuh passphrase untuk menggunakannya.

### Membuat Key dengan Passphrase

```bash
ssh-keygen -t rsa -b 4096
# Enter passphrase: [masukkan passphrase]
```

### Cracking SSH Key Passphrase

Jika menemukan encrypted private key, dapat di-crack dengan **John the Ripper**:

```bash
# 1. Convert key ke format John
ssh2john id_rsa > id_rsa.hash

# 2. Crack dengan wordlist
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.hash

# 3. Lihat hasil
john --show id_rsa.hash
```

---

## Task 6: Practical - SSH Authentication

### Langkah-langkah Praktis

1. **Deploy VM** dari TryHackMe

2. **Generate SSH key pair:**
   ```bash
   ssh-keygen -t rsa -b 4096 -f thm_key
   ```

3. **Add public key ke server:**
   ```bash
   # Jika sudah ada akses
   ssh-copy-id -i thm_key.pub user@TARGET_IP
   ```

4. **Set permissions:**
   ```bash
   chmod 600 thm_key
   ```

5. **Connect ke server:**
   ```bash
   ssh -i thm_key user@TARGET_IP
   ```

---

## Task 7: Diffie-Hellman Key Exchange

### Konsep

Diffie-Hellman memungkinkan dua pihak untuk **membuat shared secret** melalui channel yang tidak aman, tanpa pernah mengirimkan secret itu sendiri.

### Simplified Process

```
1. Alice dan Bob setuju pada nilai public: p (prime) dan g (generator)
2. Alice pilih secret a, kirim A = g^a mod p ke Bob
3. Bob pilih secret b, kirim B = g^b mod p ke Alice
4. Alice hitung: s = B^a mod p
5. Bob hitung: s = A^b mod p
6. Keduanya sekarang punya shared secret s yang sama!
```

### Penggunaan

- **TLS/HTTPS** - Key exchange untuk encrypted web traffic
- **SSH** - Establishing session keys
- **VPN** - Secure tunnel setup

---

## Key Takeaways

1. **Asymmetric encryption** menggunakan sepasang key (public & private)
2. **RSA** keamanannya bergantung pada kesulitan factoring bilangan besar
3. **SSH keys** lebih aman dari password untuk authentication
4. **Private key** harus selalu dijaga dan set permission `600`
5. **Passphrase** menambah layer keamanan pada private key
6. **Diffie-Hellman** memungkinkan key exchange yang aman

---

## Useful Commands Cheatsheet

```bash
# RSA Key Generation
openssl genrsa -out private.pem 2048
openssl rsa -in private.pem -pubout -out public.pem

# SSH Key Generation
ssh-keygen -t rsa -b 4096 -f mykey

# SSH Connect with Key
ssh -i private_key user@host

# Fix Permissions
chmod 600 private_key

# Crack SSH Key Passphrase
ssh2john id_rsa > hash.txt
john --wordlist=rockyou.txt hash.txt
```

---

> **Tip:** "Public key cryptography is the foundation of secure communication on the internet. Understanding RSA and SSH is crucial for any security professional."
