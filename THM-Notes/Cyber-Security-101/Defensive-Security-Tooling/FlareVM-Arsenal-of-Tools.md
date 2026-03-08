# TryHackMe: FlareVM Arsenal of Tools

- **Room Link:** [FlareVM Arsenal of Tools](https://tryhackme.com/room/flarevmarsenaloftools)
- **Category:** Defensive Security Tooling
- **Difficulty:** Easy

## Introduction

Kalau di materi sebelumnya kita sudah bahas **REMnux** sebagai meja operasi *malware* berbasis Linux, sekarang saatnya kita kenalan sama saudara kandungnya di dunia Windows: **FlareVM**.

**FlareVM** (singkatan dari *Forensics, Logic Analysis, and Reverse Engineering*) adalah seperangkat kotak perkakas khusus rakitan tim FLARE dari **FireEye**. Isinya sudah dipenuhi ratusan program spesialis yang murni diciptakan buat ngebantu *reverse engineers*, analis *malware*, dan investigator forensik. Ibaratnya, kalau kamu mau menguliti sistem Windows atau ngebongkar wujud asli (*reverse engineer*) dari sebuah virus `.exe`, ini VM andalan.

### Learning Objectives

- Mengenal dan mengeksplorasi isi kotak perkakas di dalam FlareVM.
- Praktik langsung pakai berbagai *tools* bawaannya untuk menganalisis proses/*process* mencurigakan di memori.
- Familiar sama *tools* yang biasa dipakai buat investigasi statis (*static analysis*) dokumen jahat atau *file* biner (*binaries*).

---
