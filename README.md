# OpenClaw - Server Deployment & Setup Guide (`openclaw-unsrit`)

Panduan lengkap untuk melakukan instalasi, konfigurasi, dan manajemen sistem **OpenClaw** pada VPS server (Ubuntu/Debian) berdasarkan repositori [github.com/kereh/openclaw-unsrit](https://github.com/kereh/openclaw-unsrit.git).

---

## 📋 Daftar Isi
1. [Prasyarat Sistem](#prasyarat-sistem)
2. [Persiapan VPS & Dependensi](#persiapan-vps--dependensi)
3. [Clone Repositori](#clone-repositori)
4. [Konfigurasi Environment (.env)](#konfigurasi-environment-env)
5. [Instalasi Dependensi & Build](#instalasi-dependensi--build)
6. [Konfigurasi Process Manager (PM2 / Systemd)](#konfigurasi-process-manager-pm2--systemd)
7. [Konfigurasi Reverse Proxy (Nginx & SSL)](#konfigurasi-reverse-proxy-nginx--ssl)
8. [Troubleshooting & Optimasi Resource](#troubleshooting--optimasi-resource)

---

## 🛠️ Prasyarat Sistem

Sebelum memulai instalasi, pastikan VPS Anda memenuhi spesifikasi minimum berikut:
* **OS**: Ubuntu 22.04 LTS / Debian 11+
* **CPU / RAM**: 1 vCPU, 1 GB RAM (direkomendasikan membuat *Swap Space* minimal 1GB jika RAM terbatas).
* **Akses**: Akses `sudo` atau `root` via SSH.
* **Software**: 
  * Node.js (LTS v18+ atau v20+)
  * Bun (Opsional, untuk performa build yang lebih cepat dan efisien RAM)
  * Git
  * Nginx

---

## ⚙️ 1. Persiapan VPS & Dependensi

Update sistem terlebih dahulu dan instal paket esensial:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl git build-essential nginx ufw -y
```

### Instalasi Node.js & NPM (via NodeSource)
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

### Instalasi Bun (Opsional & Direkomendasikan untuk Server Kecil)
Jika Anda menggunakan Bun untuk manajemen paket dan build agar hemat resource:
```bash
curl -fsSL https://bun.sh/install | bash
source ~/.bashrc
```

---

## 📥 2. Clone Repositori

Masuk ke direktori web server (misalnya `/var/www` atau direktori home Anda), lalu clone repositori:

```bash
cd /var/www
sudo git clone https://github.com/kereh/openclaw-unsrit.git openclaw
cd openclaw
```

Atur kepemilikan folder agar dapat diakses oleh user Anda:
```bash
sudo chown -R $USER:$USER /var/www/openclaw
```

---

## 🔐 3. Konfigurasi Environment (`.env`)

Buat file konfigurasi environment dari template yang tersedia:

```bash
cp .env.example .env
```

Edit file `.env` menggunakan nano atau editor pilihan Anda:
```bash
nano .env
```
Sesuaikan variabel konfigurasi (seperti port aplikasi, koneksi database, URL backend/frontend, dan kunci rahasia) sesuai dengan kebutuhan server Anda.

---

## 📦 4. Instalasi Dependensi & Build

Instal dependensi proyek menggunakan **Bun** atau **npm**:

Menggunakan **Bun**:
```bash
bun install
bun run build
```

Atau menggunakan **npm**:
```bash
npm install
npm run build
```

---

## 🚀 5. Konfigurasi Process Manager (PM2)

Agar aplikasi berjalan di latar belakang secara terus-menerus dan otomatis restart saat VPS reboot, gunakan PM2.

Instal PM2 secara global:
```bash
sudo npm install -g pm2
```

Jalankan aplikasi dengan PM2:
```bash
pm2 start npm --name "openclaw-unsrit" -- run start
# Atau jika menggunakan Bun:
# pm2 start bun --name "openclaw-unsrit" -- run start
```

Simpan konfigurasi PM2 dan aktifkan startup script:
```bash
pm2 save
pm2 startup
```
*(Ikuti perintah terminal yang muncul dari `pm2 startup` jika ada).*

---

## 🌐 6. Konfigurasi Reverse Proxy (Nginx) & SSL

Buat file blok server Nginx baru untuk domain atau IP server Anda:

```bash
sudo nano /etc/nginx/sites-available/openclaw
```

Masukkan konfigurasi berikut (sesuaikan `domain-anda.com` atau IP server):

```nginx
server {
    listen 80;
    server_name domain-anda.com www.domain-anda.com;

    location / {
        proxy_pass http://localhost:3000; # Sesuaikan port aplikasi Anda
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Aktifkan konfigurasi Nginx dan lakukan *test syntax*:
```bash
sudo ln -s /etc/nginx/sites-available/openclaw /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### (Opsional) Amankan dengan SSL Let's Encrypt (Certbot)
```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d domain-anda.com -d www.domain-anda.com
```

---

## 💡 7. Troubleshooting & Optimasi Resource (VPS 1 vCPU / 1GB RAM)

Jika Anda mengalami masalah memori habis (*Out of Memory* / OOM Killer) saat melakukan build aplikasi (terutama pada Next.js atau framework berat):

1. **Buat Swap Space (1GB - 2GB):**
   ```bash
   sudo fallocate -l 1G /swapfile
   sudo chmod 600 /swapfile
   sudo mkswap /swapfile
   sudo swapon /swapfile
   echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
   ```

2. **Batasi Memory Node.js saat Build:**
   ```bash
   NODE_options="--max-old-space-size=512" npm run build
   ```

---

## 📞 Bantuan & Kontribusi
Jika menemui kendala dalam instalasi, silakan buat *Issue* pada repositori resmi [GitHub openclaw-unsrit](https://github.com/kereh/openclaw-unsrit/issues).
