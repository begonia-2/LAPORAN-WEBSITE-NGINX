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

###### **6. Jika status nya active (running), Berarti Nginx sudah berjalan**
###### **7. Buka Web Browser dan akses:**

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

###### **3. di sini kelompok kita menyesuaikan dengan yang ada di LMS bapak, yaitu:**

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

*Jadi temen temen, fungsi memasukan Konfigurasi ini membuat Nginx siap menjalankan website PHP, melayani file statis dengan cepat, aman dari file sensitif, serta kompatibel dengan aplikasi PHP modern*

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
## 🎊 **D. Menguji PHP**


###### **1. buat file uji coba di direktori bawaan nginx:**

```bash
nano /var/www/html/info.php
```

---


###### **2.Masukan script berikut:**

```bash
<? php 
    phpinfo();
?>
```

---


###### **3. cara mengaksesnya yaitu dengan cara buka web browser dan akses:**

```bash
https://ip-server/info.php
```
###### *jika muncul halaman informasi PHP, artinya nginx dan PHP sudah terhubung dengan baik. 😻*

---

## 🔐 **E. menambahkan sertifikat SSL Selft-Signed**

###### **1. buat folder untuk menyimpan sertifikat:**

```bash
mkdir /etc/ssl/nginx
```

---


###### **2. pastikan openSSL sudah terinstall:**

```bash
apt install openssl
```

---

###### **3. Lalu buat sertifikat dan key:**

``` bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/nginx/selfsigned.key -out /etc/ssl/nginx/selfsigned.crt
```

---

######  **4. Setelah selfsigned.key dah selfsigned.crt berhasil dibuat, kita masukan konfigurasi website kita:**

```bash
nano /etc/nginx/sites-available/default 
```

---

###### **Jika sudah, Ubah isinya dengan konfigurasi yang ada di lms bapa:**

```bash
# ==========================
# Konfigurasi HTTP (port 80)
# ==========================
server {
    listen 80 default_server;          # Dengarkan koneksi HTTP di port 80
    listen [::]:80 default_server;     # Dukungan untuk IPv6

    root /var/www/html;                # Folder utama untuk file website
    index index.php index.html;        # File index yang akan dicari pertama

    server_name _;                     # "_" artinya menerima semua nama domain/host

    # Bagian utama untuk menangani request
    location / {
        # Coba tampilkan file/ folder sesuai permintaan
        # Jika tidak ada, teruskan ke index.php (penting untuk WordPress/Moodle)
        try_files $uri $uri/ /index.php?$query_string;
    }

    # Bagian untuk menjalankan file PHP
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.4-fpm.sock; # Jalur socket PHP-FPM

        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    # Lindungi file tersembunyi (.htaccess, .git, .env, dll.)
    location ~ /\.(?!well-known).* {
        deny all;
    }

    # Atur caching untuk file statis (gambar, css, js, font, video)
    location ~* \.(?:ico|css|js|gif|jpe?g|png|woff2?|eot|ttf|svg|mp4)$ {
        expires 6M;             # Simpan cache selama 6 bulan
        access_log off;         # Tidak perlu dicatat di access log
        log_not_found off;      # Jika file tidak ada, jangan penuhkan log
    }
}

# ==========================
# Konfigurasi HTTPS (port 443, SSL/TLS)
# ==========================
server {
    listen 443 ssl default_server;      # Dengarkan koneksi HTTPS di port 443
    listen [::]:443 ssl default_server; # Dukungan untuk IPv6

    root /var/www/html;                 # Sama seperti HTTP
    index index.php index.html;
    server_name _;

    # Lokasi sertifikat SSL self-signed
    ssl_certificate /etc/ssl/nginx/selfsigned.crt;
    ssl_certificate_key /etc/ssl/nginx/selfsigned.key;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.4-fpm.sock;

        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }

    location ~* \.(?:ico|css|js|gif|jpe?g|png|woff2?|eot|ttf|svg|mp4)$ {
        expires 6M;
        access_log off;
        log_not_found off;
    }
}
```
*jadi temen temen, Konfigurasi tersebut berisi dua bagian utama, yaitu server blok untuk HTTP (port 80) dan server blok untuk HTTPS (port 443). Masing-masing blok mengatur bagaimana Nginx menangani permintaan (request) dari pengguna*

---

###### **4. Uji restart Nginx untuk melihat apakah SSL sudah terpasang:**

```bash
nginx-t
systemctl restart nginx 
```
---

###### **5. jika sudah, buka web browser dan akses:**

```bash
https://ip-server
```

---


###### **6. Browser akan memberi peringatan “Not Secure” atau “Untrusted Certificate”. Klik Advanced → Proceed untuk lanjut. jika sudah Klik Lanjutkan, jika error 404 😱, tenang saja, itu artinya halaman yang di tuju tidak ada, dan memang tidak ada karena kita belum buat file index.php. jadi supaya tidak eror kita harus membuat web server nya terlebih dahulu menggunakan:**

```bash
nano /var/www/html/index.php
```
 - *isi dengan script sederhana, bebas mau apa aja. Jika sudah, maka setelah di akses kembali makan tidak akan muncul *error*, melainkan akan muncul halaman website kita..😻*

---


## 💡 **C. Kelebihan & Kekurangan Web Server Nginx**

#### ✔️ **Kelebihan:**

- *Performa sangat cepat untuk trafik tinggi.*
- *Ringan, tidak memakan banyak RAM dan CPU.*
- *Stabil ketika menangani banyak koneksi.*
- *Mendukung reverse proxy dan load balancing.*
- *Konfigurasi mudah dibaca dan dikelola.*


#### ❌ **Kekurangan:**

- *Pengaturan modul tidak bisa semudah Apache (karena harus compile dari awal).*
- *Fungsi .htaccess tidak tersedia.*
- *Untuk pemula, konfigurasi block server mungkin sedikit membingungkan.*

---

## 🧩 **D. Proses Pembuatan Proyek HTML & Upload ke Server**

#### **1. Pembuatan Proyek HTML**
Saya memulai dengan menentukan tema website, lalu membuat file `index.html` sebagai halaman utama. Jika diperlukan, saya menambahkan file CSS dan folder gambar untuk mempercantik tampilan. Setelah selesai, saya mengecek tampilannya secara lokal dengan membuka file HTML di browser.

#### **2. Menyiapkan server Nginx**

Server Nginx dipastikan sudah berjalan, dan direktori utama website berada di `/var/www/html`, tempat file HTML akan disimpan.

#### **3. Mengapload File ke Server (Menggunakan WindSCP)**

Untuk memindahkan file ke server, saya menggunakan aplikasi **WinSCP** karena mudah dan visual.

   * Saya membuka WinSCP → mengisi IP server, username, password, dan memilih protokol **SFTP**.
   * Setelah terhubung, saya tinggal **drag & drop** file proyek HTML dari komputer ke folder " `/var/www/html`" di server.
     Cara ini memudahkan proses upload tanpa perlu mengetik perintah terminal.

#### **4. Mengatur hak akses File**

Setelah upload selesai, saya memastikan file dapat dibaca oleh Nginx dengan mengatur izin folder

#### **5. Pengujian Website**

Terakhir, saya me-restart Nginx dan mengakses IP server melalui browser. Jika halaman website tampil sesuai, berarti proses upload berhasil.

---

## ⚠️ **E. Kendala yang Dialami & Solusinya**

#### **1. Akses server gagal**

**Penyebab:** IP, port, atau username salah.
**Solusi:** Memeriksa ulang data akses sebelum login.

#### **2. File HTML tidak muncul di browser**

**Penyebab:** File berada di folder yang salah.
**Solusi:** Memastikan file berada di `/var/www/html/` sebelum diuji.

---



