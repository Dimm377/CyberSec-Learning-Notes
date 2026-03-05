# TryHackMe: REMnux Getting Started

- **Room Link:** [REMnux Getting Started](https://tryhackme.com/room/remnuxgettingstarted)
- **Category:** Defensive Security Tooling
- **Difficulty:** Easy

## Introduction

Menganalisis software yang berpotensi berbahaya (_malware_) bisa jadi tugas yang menegangkan, terutama saat terjadi insiden keamanan (_security incident_). Analis diburu waktu untuk memberikan hasil seakurat mungkin tanpa membahayakan sistem yang ada.

Di sinilah **REMnux** berperan.

### What is REMnux?

**REMnux** adalah distro Linux khusus yang dirancang khusus untuk keperluan **Reverse Engineering Malware**.

Analogi simpelnya: Kalau Kali Linux adalah toolkit untuk anak _Offensive Security_ (Red Team) buat ngebobol sistem, maka REMnux adalah **"meja operasi"** untuk anak _Defensive Security_ (Blue Team / Malware Analyst) buat membedah virus.

Apa keunggulannya?
- Sudah ter-install ratusan _tools_ untuk analisis memori, jaringan, dan bedah file statis. (Contoh: Volatility, YARA, Wireshark, oledump, INetSim).
- Dirancang seperti lingkungan _sandbox_ yang aman untuk mengeksekusi dan mengobservasi software berbahaya tanpa resiko menginfeksi komputer pribadimu.
- Siap pakai (_ready to go_) — gak perlu pusing install tools satu per satu secara manual.

### Learning Objectives

Setelah menyelesaikan room ini, kamu akan menguasai cara:
- Eksplorasi _tools_ bawaan di dalam REMnux VM.
- Menggunakan _tools_ untuk menganalisis dokumen berbahaya (_malicious documents_) secara efektif.
- Mensimulasikan jaringan palsu (_fake network_) untuk mengelabui malware selama analisis.
- Familiar dengan _tools_ yang digunakan untuk menganalisis _memory images_.

---
