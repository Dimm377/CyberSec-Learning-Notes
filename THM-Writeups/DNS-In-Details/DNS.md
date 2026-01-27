# TryHackMe: Intro To LAN

**Room Link:** [DNS In Details](https://tryhackme.com/room/dnsindetail)
**Category:** How The Web Works
**Difficulty:** easy

# Overview

### What is DNS ?

DNS (Domain Name System) memberikan cara sederhana bagi kita untuk berkomunikasi dengan perangkat di internet tanpa mengingat angka-angka kompleks.

### Domain Hierarchy

Domain Hierarchy merupakan sistem penamaan terstruktur yang menyerupai pohon terbalik, setiap tingkatan dipisahkan dengan tanda (`.`) dan dibaca dari kanan ke kiri, memastikan bahwa setiap alamat di internet bersifat unik dan dapat dikelola secara terdistribusi

struktur Domain Hierarchy:

<p align="center">
<img src="../../Assets/Images/DNS.png" width="400">
</p>

### 1. Root Domain

Root domain adalah tingkat tertinggi dalam sistem domain hierarchy, diwakili oleh tanda titik (`.`) dan berada di puncak nama domain, contoh `domain.com`, `.` adalah root domain ,Root domain sangat penting untuk resolusi DNS, karena membantu menavigasi seluruh struktur domain

### 2. TLD (Top Level Domain)

TLD adalah bagian paling kanan dari nama domain, contoh `domain.com`, tepat setelah titik terakhir `.com`,

TLD dibagi menjadi dua kategori utama:

1. **gTLID (Generic Top Level Domain):**
