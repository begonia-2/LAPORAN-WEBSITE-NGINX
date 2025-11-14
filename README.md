# 🌐 **Laporan Instalasi & Konfigurasi Web Server Nginx**

### *Dengan Dokumentasi Proses, Pengujian, & Kendala*

---

## 📌 **A. Tahapan Akses Server hingga Uji Coba**

Berikut tahapan yang sudah dilakukan dalam mengakses server, menginstall web server, hingga tahap uji coba:

---

## 🚀 **B. Proses Instalasi Web Server & Uji Coba**

### **1. Perbarui semua paket agar Debian siap digunakan, menggunakan:**

```bash
apt update && apt upgrade
```

📸 **Screenshot:**
`(tempat screenshot perintah di terminal)`

---

### **2. Install Nginx sebagai web server utama:**

```bash
apt install nginx -y
```

📸 **Screenshot:**
`(tempat screenshot proses instalasi Nginx)`

---

### **3. Cek status layanan Nginx agar memastikan Nginx berjalan:**

```bash
systemctl status nginx
```

📸 **Screenshot:**
`(tempat screenshot status active (running))`

---

### **4. Akses server melalui IP untuk memastikan web server berjalan:**

Buka browser lalu ketik:

```
http://IP-Server-Kamu
```

📸 **Screenshot:**
`(tempat screenshot halaman default Nginx di browser)`

---

### **5. Menempatkan file HTML ke direktori website**

Pastikan file berada di:

```
/var/www/html/
```

📸 **Screenshot:**
`(tempat screenshot file HTML berada di folder)`

---

### **6. Uji coba website**

Buka browser dan kunjungi:

```
http://IP-Server-Kamu
```

📸 **Screenshot:**
`(tempat screenshot tampilan website yang muncul)`

---

---

# 💡 **C. Kelebihan & Kekurangan Web Server Nginx**

## ✔️ **Kelebihan:**

* Ringan dan cepat
* Mendukung banyak koneksi sekaligus
* Konfigurasi fleksibel
* Cocok untuk server statis maupun reverse proxy

## ❌ **Kekurangan:**

* Konfigurasi bisa lebih kompleks
* Tidak memiliki modul dinamis sebanyak Apache

---

# 🧩 **D. Proses Pembuatan Proyek HTML & Upload ke Server**

### **1. Pembuatan Proyek HTML**

Proyek HTML dibuat menggunakan struktur dasar HTML dan diatur sesuai kebutuhan tampilan website.

### **2. Upload ke Server**

File HTML dipindahkan ke server melalui:

* SCP
* SFTP
* Atau langsung dibuat di terminal editor (nano / vim)

📁 File ditempatkan pada:

```
/var/www/html/
```

📸 **Screenshot:**
`(tampilan file yang berhasil diupload)`

---

# ⚠️ **E. Kendala yang Dialami & Solusinya**

### **1. Akses server gagal**

**Penyebab:** IP, port, atau username salah.
**Solusi:** Memeriksa ulang data akses sebelum login.

### **2. File HTML tidak muncul di browser**

**Penyebab:** File berada di folder yang salah.
**Solusi:** Memastikan file berada di `/var/www/html/` sebelum diuji.

---

# 🧾 **F. Deskripsi Tugas Laporan Web Server Nginx**

Tugas ini bertujuan untuk mempelajari proses instalasi, konfigurasi, serta pengujian web server Nginx pada sistem operasi Debian, serta memahami kendala yang mungkin terjadi selama proses tersebut.

---

# 👥 **G. Anggota Kelompok**

* Desi Novta Azizah
* Fajri Abdul Rojak
* Saskia Purnama Sari
* Windi Rosdianti

---

# 🎯 **H. Tujuan Pembuatan Proyek**

* Mengetahui proses instalasi Nginx
* Belajar konfigurasi dasar web server
* Mampu mengupload dan menampilkan website
* Mampu mengatasi kendala server dasar

