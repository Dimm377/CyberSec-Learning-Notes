# TryHackMe: Introduction to SIEM

- **Room Link:** [Introduction to SIEM](https://tryhackme.com/room/introductiontosiem)
- **Kategori:** Security Solutions
- **Difficulty:** easy

## Introduction to SIEM

Security Information and Event Management system (SIEM) adalah sistem yang mengumpulkan, menganalisis, dan mengelola data keamanan dari berbagai sumber dalam jaringan. SIEM membantu dalam mendeteksi, menganalisis, dan merespons insiden keamanan secara real-time

### Learning Objectives

- Memahami berbagai jenis data keamanan yang dikumpulkan oleh SIEM
- Identifikasi batasan dalam bekerja dengan log yang terisolasi
- Mengenali tentang pentingnya SIEM sebagai security solution
- Explore tentang fitur-fitur yang ada di SIEM
- Mempelajari berbagai jenis log yang dikumpulkan oleh SIEM

## Logs Everywhere, Answers Anywhere

### Log Everywhere

Beberapa perangkat dalam jaringan berkomunikasi satu sama lain dan, sebagian besar waktu, dengan Internet melalui router. Gambar di bawah menunjukkan contoh jaringan sederhana yang terdiri dari beberapa Endpoint berbasis Linux/Windows, satu server data, dan satu situs web.

<p align="center">
<img src="../../Assets/Images/Logs-Event.png" alt="Network Diagram" width="400px"/>
</p>

Perangkat-perangkat ini terus menghasilkan log eventyang terjadi di dalamnya,  Log yang mereka hasilkan berfungsi sebagai jejak semua aktivitas dan sangat membantu untuk mengidentifikasi aktivitas berbahaya atau melakukan troubleshooting.

 Sumber log dibagi menjadi dua kategori:

### Host-Centric Log Sources

Sumber log ini mencatat peristiwa yang terjadi di dalam atau terkait dengan host
Perangkat yang menghasilkan log yang berpusat pada host mencakup Windows, Linux, server, dll.

Beberapa contoh log yang berpusat pada host adalah:

- User sedang mengakses sebuah file
- User sedang login ke dalam sistem
- Suatu proses menambah/mengedit/menghapus kunci atau nilai registry
- Menjalankan PowerShell
- Menjalankan sebuah program executable

### Network-Centric Log Sources

jenis log yang terkait jaringan, dihasilkan ketika host berkomunikasi satu sama lain atau mengakses internet untuk mengunjungi situs web, Perangkat yang menghasilkan log yang berpusat pada jaringan adalah firewall, IDS/IPS, router, dll. Beberapa contoh log yang berpusat pada jaringan adalah:

- Koneksi ke SSH
- Akses File melalui FTP
- Web Traffic
- User sedang mengakses internet menggunakan VPN
- Aktivitas berbagi file

### Answers Nowhere

Dari Penjelasan soal log tersebut, sepertinya terlihat sangat mudah untuk mencari log yang terisolasi.
