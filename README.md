# 🌐 **Laporan Instalasi & Konfigurasi Web Server Nginx**

#### *Dengan Dokumentasi Proses, Pengujian, & Kendala*

---
## 👥 **A. Anggota Kelompok**

- *Desi Novta Azizah* - bagian penulisan dan penyusunan laporan 
- *Fajri Abdul Rojak* - bagian penugumpulan data
- *Saskia Purnama Sari* - bagian penulisan dan penyusunan laporan
- *Windi Rosdianti* - bagian pengecekan dan penyusunan laporan 
---


## 🎯 **B. Tujuan Pembuatan Proyek**

memahami dan menerapkan konsep dasar yang telah dipelajari selama pembelajaran, sehingga teori dapat diimplementasikan dalam bentuk karya nyata. Dan melatih kemampuan praktis seperti perancangan, konfigurasi, dan penyusunan proyek secara sistematis sesuai prosedur yang benar

## 🏆 **C. Pembahasan**

1. Apa itu Nginx?
     Nginx (dibaca Egine-X) adalah web server open-source yang ringan dan efisien, Nginx juga berfungsi sebagai reverse proxy, load balancer, dan proxy email. Untuk menjalankan script PHP secara optimal Nginx biasanya dipadukan dengan PHP-FPM (FastCGI Process Manager), yaitu metode eksekusi php yang memisahkan proses dari server web, sehingga meningkatkan performa dan fleksibilitas konfigurasi ideal untuk sistem berskala tinggi dan multi-user.
---
2. Apa itu SSL/TLS?
      SSL/TLS adalah teknologi yang membuat koneksi antara server dan pengguna jadi aman,  karena data yang dikirim akan di enkripsi (disamarkan) agar tidak mudah dibaca orang lain.
   
## 📌 **A. Tahapan Installasi  WebServer hingga Uji Coba**

Berikut tahapan yang sudah dilakukan dalam mengakses server, menginstall web server, hingga tahap uji coba:👇🏻

## 🚀 **B. Proses Instalasi Web Server & Uji Coba**

###### **1. Menyiapkan Debian Server:**

- *siapkan server Debian yang sudah punya IP address dan bisa diakses dari jaringan LAN*
-  *atur respository agar bisa digunakan untuk instalasi software*
-  *coba akses server lewat SSH pakai CMD dan win SCP untuk memastikan koneksinya sudah berfungsi*
###### **2. Perbarui semua paket agar Debian siap digunakan, Menggunakan:**

```bash
apt update && apt upgrade
```

---

###### **3.  pasang web server Nginx, Menggunakan :**

```bash
apt install nginx
```

---

###### **4. jalankan dan aktifkan otomatis saat boot, Menggunakan:**

```bash
systemctl start nginx
systemctl enable nginx
```

---

###### **5. Cek status:**

```bash
systemctl status nginx
```

---

###### **6. Jika status nya active (running), Berarti Nginx sudah berjalan*
###### *7. Buka Web Browser dan akses:**

```bash
https:// ip-server 
```

---

###### **8. Jika muncul halaman "Welcome To Nginx", berarti server aktif.🎉**

###### **agar file bisa berjalan pasang/install .php dan modul pendukung, dengan:**
```bash
apt install php8.4-fpm php8.4-cli
```

###### **jika sudah periksa php-fpm sudah aktif atau belum, menggunakan:**

```bash
systemctl status php8.4-fpm
```

---

## 👾 **C. Mengaktifkan PHP di Konfigurasi Default Nginx ⚙️📄**

###### **1. Supaya mudah kita tulis ulang saja konfigurasinya, namun sebelum kita memulai konfigurasi nya kita backup dulu file aslinya, dengan:**

```bash
mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.asli
```

---

###### **2. jika sudah, buka/buat file konfigurasi bawaan nginx:**

```bash
nano /etc/nginx/sites-available/default 
```

---

###### *3. di sini kelompok kita menyesuaikan dengan yang ada di LMS bapak, yaitu:

```bash
server {
    listen 80 default_server;          # Dengarkan koneksi HTTP di port 80 (standar web)
    listen [::]:80 default_server;     # Dukungan untuk IPv6

    root /var/www/html;                # Folder utama tempat file website disimpan
    index index.php index.html;        # Urutan file index yang akan dicari pertama kali

    server_name _;                     # "_" artinya menerima semua nama domain/host

    # Bagian utama untuk menangani request ke website
    location / {
        # Coba tampilkan file sesuai permintaan
        # Jika tidak ada, coba foldernya
        # Jika tetap tidak ada, arahkan ke index.php (penting untuk WordPress, Moodle, dll.)
        try_files $uri $uri/ /index.php?$query_string;
    }

    # Bagian untuk menjalankan file PHP
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;             # Include konfigurasi standar PHP-FPM
        fastcgi_pass unix:/run/php/php8.4-fpm.sock;    # Jalur socket PHP-FPM versi 8.4

        # Beritahu PHP file mana yang harus dijalankan
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;                        # Include parameter tambahan untuk PHP
    }

    # Bagian untuk file statis (gambar, CSS, JS, font, dll.)
    # Dikasih aturan cache supaya website lebih cepat dibuka
    location ~* \.(?:ico|css|js|gif|jpe?g|png|woff2?|eot|ttf|svg|mp4)$ {
        expires 6M;             # Browser boleh menyimpan file ini 6 bulan
        access_log off;         # Jangan dicatat di log akses (hemat space/log)
        log_not_found off;      # Jangan catat kalau file statis tidak ditemukan
    }

    # Lindungi file .htaccess atau file tersembunyi (.ht*)
    # Biasanya digunakan Apache, tapi tetap diblokir di Nginx agar aman
    location ~ /\.ht {
        deny all;
    }
}
```
*(jika sudah klik ctrl+o, enter, ctrl+x untuk menyimpan)*

###### *Jadi temen temen, fungsi memasukan Konfigurasi ini membuat Nginx siap menjalankan website PHP, melayani file statis dengan cepat, aman dari file sensitif, serta kompatibel dengan aplikasi PHP modern*

---

###### **4.  uji konfigurasi menggunakan::**

```bash
nginx-t
```
---

###### **5. Jika sudah, restart nginx:**

```bash
systemctl restart nginx
```
