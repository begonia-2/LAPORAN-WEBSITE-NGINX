# 🌐 **Laporan Instalasi & Konfigurasi Web Server Nginx**

### *Dengan Dokumentasi Proses, Pengujian, & Kendala*

---
# 👥 **A. Anggota Kelompok**

- *Desi Novta Azizah* - bagian penulisan dan penyusunan laporan 
- *Fajri Abdul Rojak* - bagian penugumpulan data
- *Saskia Purnama Sari* - bagian penulisan dan penyusunan laporan
- *Windi Rosdianti* - bagian pengecekan dan penyusunan laporan 

# 🎯 **B. Tujuan Pembuatan Proyek**

memahami dan menerapkan konsep dasar yang telah dipelajari selama pembelajaran, sehingga teori dapat diimplementasikan dalam bentuk karya nyata. Dan melatih kemampuan praktis seperti perancangan, konfigurasi, dan penyusunan proyek secara sistematis sesuai prosedur yang benar

# 🏆** C. Pembahasan

1. Apa itu Nginx?
     Nginx (dibaca Egine-X) adalah web server open-source yang ringan dan efisien, Nginx juga berfungsi sebagai reverse proxy, load balancer, dan proxy email. Untuk menjalankan script PHP secara optimal Nginx biasanya dipadukan dengan PHP-FPM (FastCGI Process Manager), yaitu metode eksekusi php yang memisahkan proses dari server web, sehingga meningkatkan performa dan fleksibilitas konfigurasi ideal untuk sistem berskala tinggi dan multi-user.
---
2. Apa itu SSL/TLS?
      SSL/TLS adalah teknologi yang membuat koneksi antara server dan pengguna jadi aman,  karena data yang dikirim akan di enkripsi (disamarkan) agar tidak mudah dibaca orang lain.
   
## 📌 **A. Tahapan Installasi  WebServer hingga Uji Coba**

Berikut tahapan yang sudah dilakukan dalam mengakses server, menginstall web server, hingga tahap uji coba:👇🏻

## 🚀 **B. Proses Instalasi Web Server & Uji Coba**

###### *1. Menyiapkan Debian Server:*

- *siapkan server Debian yang sudah punya IP address dan bisa diakses dari jaringan LAN*
-  *atur respository agar bisa digunakan untuk instalasi software*
-  *coba akses server lewat SSH pakai CMD dan win SCP untuk memastikan koneksinya sudah berfungsi*
###### *2. Perbarui semua paket agar Debian siap digunakan, Menggunakan:*

```bash
apt update && apt upgrade
```

---

###### *3.  pasang web server Nginx, Menggunakan :*

```bash
apt install nginx
```

---

###### *4. jalankan dan aktifkan otomatis saat boot, Menggunakan:*

```bash
systemctl start nginx
systemctl enable nginx
```

---

###### *5. Cek status:*

```bash
systemctl status nginx
```

---

###### *6. Jika status nya active (running), Berarti Nginx sudah berjalan*
