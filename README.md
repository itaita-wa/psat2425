# 📦 Deploy Aplikasi psat2425 Secara Otomatis dengan .env dan Shell Script  
**SMKN 1 Banyumas**  
*Mata Pelajaran: DevOps – XI TJKT 1*  
*Tahun Ajaran 2024/2025*

---

## 1. Persiapan Sebelum Deploy
### A. Buat Repositori GitHub Baru
Kunjungi: https://github.com
Buat repositori baru (bukan fork) dengan nama: ```psat2425```
Contoh URL: ```https://github.com/<username-kamu>/psat2425```

### B. Download & Siapkan Proyek
Clone proyek dari repositori asli:
```git clone https://github.com/paknux/psat2425```
Masukkan ke dalam repositori GitHub kamu.
Tambahkan file ```.env``` (biarkan kosong).
Buat atau periksa file ```.gitignore.``` Tambahkan:
```
.env
uploads/*
!uploads/

```
### 3. Upload ke GitHub
```
git init
git remote add origin https://github.com/<username-kamu>/psat2425
git add .
git commit -m "initial commit"
git branch -M main
git push -u origin main

```
## 2. 🌐 Membuat Database di AWS RDS
Login ke AWS Academy.

Buka RDS > Databases > Create database.

Pilih:

 - Engine: MySQL

 - Template: Free tier

 - DB Identifier: psat-db

 - Username & Password: buat sesuai keinginan

Pada bagian networking:

 - Pastikan Public Access: No

 - Security Group: izinkan port 3306

- Klik Create database dan tunggu sampai endpoint muncul.

### 3. ☁️ Deploy Aplikasi ke AWS EC2 (Metode Otomatis)
A. Buat EC2 Instance

- OS: Ubuntu

- Instance Type: t2.micro

- Key Pair: pilih/buat key pair

- Security Group:

  HTTP (80)

  HTTPS (443)

  SSH (22)

B. Tambahkan Shell Script Otomatis
Saat membuat instance, scroll ke Advanced Details > User data lalu masukkan script berikut:
```

#!/bin/bash
echo '#!/bin/bash
sudo apt update -y
sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
sudo rm -rf /var/www/html/{,.}
sudo git clone https://github.com/<username-kamu>/psat2425 /var/www/html
sudo chmod -R 777 /var/www/html
echo DB_USER=admin > /var/www/html/.env
echo DB_PASS=P4ssw0rd123 >> /var/www/html/.env
echo DB_NAME=psat2425 >> /var/www/html/.env
echo DB_HOST=rdsku.czt6n8ylfvyb.us-east-1.rds.amazonaws.com >> /var/www/html/.env
sudo apt install -y openssl
sudo a2enmod ssl
sudo a2ensite default-ssl.conf
sudo systemctl reload apache2' > /home/ubuntu/otomatis.sh

chmod +x /home/ubuntu/otomatis.sh


```

C.  Buka terminal instance
-  klik "Instance ID" ke instance yang sudah kita buat tadi
-  Klik "Connect"
-  Scroll sedikit ke bawah dan klik "Connect"
  
D.  Jalankan shell script yang sudah tadi dibuat
Gunakan perintah :
```
~ $ ./otomatis.sh
``` 

### 4. 🧪 Uji Aplikasi
Buka IP publik EC2 instance di browser.
Login menggunakan:
```
Username: admin

Password: 123
```
Jika berhasil, aplikasi berjalan dan terhubung ke database!
Kemudian inputkanlah data sesuai dengan datamu!!

## 📌 Penjelasan Perintah Penting

| Perintah                        | Fungsi                                                         |
|---------------------------------|----------------------------------------------------------------|
| `apt update`                   | Memperbarui daftar paket                                       |
| `apt install ...`              | Menginstal Apache2, PHP, MySQL client                          |
| `rm -rf /var/www/html/{,.}`    | Menghapus isi folder html (termasuk file tersembunyi)         |
| `git clone`                    | Mengunduh proyek dari GitHub                                   |
| `chmod -R 777`                 | Memberikan akses penuh pada folder aplikasi                    |
| `echo ... >> .env`             | Membuat file `.env` dengan isi kredensial DB                   |
| `a2enmod ssl` & `a2ensite`     | Mengaktifkan SSL di Apache                                     |
| `systemctl reload apache2`     | Memuat ulang Apache agar konfigurasi SSL aktif                 |

# SEMOGA BERHASIL DAN TETAP SEMANGAT!
