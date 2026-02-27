# TryHackMe: Firewall Fundamentals

- **Room Link:** [Firewall Fundamentals](https://tryhackme.com/room/firewallfundamentals)
- **Category:** Security Solutions
- **Difficulty:** easy

## What is Purpose of Firewall

Pernahkah kamu memperhatikan satpam (*security guard*) yang berjaga di pintu masuk *mall*, bank, atau rumah? Tugas utama mereka adalah mengawasi setiap orang yang keluar-masuk, dan memastikan tidak ada penyusup yang bisa masuk tanpa izin. Satpam ini bertindak sebagai **tembok penghalang** antara pengunjung luar dan area gedung.

Nah, sama seperti fungsi satpam tersebut, **Firewall** adalah sistem keamanan yang bertugas mengawasi ribuan lalu lintas data (*traffic*) yang masuk dan keluar dari sebuah jaringan digital kita.

Firewall bertindak sebagai **tembok pertahanan utama** yang memisahkan jaringan internal kita yang aman dari jaringan eksternal (Internet). Berbekal aturan khusus (*rules*), Firewall akan memutuskan apakah suatu data diizinkan masuk atau akan diblokir mentah-mentah.

<P>
<img src="../../Assets/Images/Firewall.png" alt="Firewall" width="700px"/>
</p>

### Learning Objectives

- Tipe-tipe firewall
- Firewall rules dan komponennya
- Windows Defender Firewall
- Linux firewall (iptables)

## Type Of Firewall

<P>
<img src="../../Assets/Images/Type-of-Firewall.png" alt="Type of Firewall" width="700px"/>
</p>
nyatanya ada banyak **tipe Firewall berbeda** di luar sana—masing-masing dirancang dengan tujuannya sendiri.

Yang membuat keren Firewall adalah beda jenis Firewall, beda juga tempat kerjanya. Mereka beroperasi di **lapisan (*layer*) OSI Model yang berbeda-beda**. Ibaratnya, ada satpam yang hanya memeriksa KTP di pagar depan, ada juga yang sampai memeriksa isi tas sampai ke dalam-dalamnya di ruang VIP

Berikut adalah beberapa kategori Firewall yang wajib kita tahu:


