# 🦞 Panduan Lengkap Setup OpenClaw di VPS SumoPod (Khusus Pemula Tanpa Latar Belakang IT)

Panduan ini dibuat seringkas dan semudah mungkin untuk siapa saja yang **belum pernah menggunakan VPS, Terminal, ataupun koding**. Ikuti langkah demi langkah dari awal sampai OpenClaw bot AI Anda aktif 24 jam nonstop!

---

## 📌 Daftar Isi
1. [Kenalan Dulu: Apa itu VPS & OpenClaw?](#1-kenalan-dulu-apa-itu-vps--openclaw)
2. [Langkah 1: Sewa VPS di SumoPod](#langkah-1-sewa-vps-di-sumopod)
3. [Langkah 2: Cara Masuk (Koneksi) ke VPS Anda](#langkah-2-cara-masuk-koneksi-ke-vps-anda)
4. [Langkah 3: Menyiapkan VPS (Instalasi Dasar)](#langkah-3-menyiapkan-vps-instalasi-dasar)
5. [Langkah 4: Install OpenClaw](#langkah-4-install-openclaw)
6. [Langkah 5: Menjalankan Wizard Onboarding & Daemon 24 Jam](#langkah-5-menjalankan-wizard-onboarding--daemon-24-jam)
7. [Langkah 6: Menghubungkan AI Model (DeepSeek / OpenAI)](#langkah-6-menghubungkan-ai-model-deepseek--openai)
8. [Langkah 7: Menghubungkan WhatsApp (Scan QR Code)](#langkah-7-menghubungkan-whatsapp-scan-qr-code)
9. [Langkah 8: Perintah Sehari-hari (Restart, Cek Status, Update)](#langkah-8-perintah-sehari-hari)
10. [Troubleshooting & Masalah yang Sering Dialami Pemula](#troubleshooting--masalah-yang-sering-dialami-pemula)
11. [Kamus Istilah untuk Orang Awam](#kamus-istilah-untuk-orang-awam)

---

## 1. Kenalan Dulu: Apa itu VPS & OpenClaw?

- **VPS (Virtual Private Server)**: Bayangkan seperti menyewa komputer virtual di gedung data center (misal di Jakarta) yang menyala 24 jam sehari, tidak pernah mati lampu, dan punya internet kencang terus-menerus.
- **SumoPod**: Penyedia cloud & VPS lokal Indonesia. Keunggulannya: server berada di Jakarta (cepat), pembayaran mudah menggunakan **QRIS / Rupiah** tanpa perlu kartu kredit.
- **OpenClaw**: Aplikasi asisten AI otonom yang bisa Anda pasang di VPS. OpenClaw bisa dihubungkan ke WhatsApp, Telegram, dll., serta menggunakan otak AI canggih (seperti DeepSeek atau OpenAI) untuk membalas pesan, mengeksekusi tugas otomatis, dan bekerja tanpa henti.

---

## Langkah 1: Sewa VPS di SumoPod

Kunjungi situs resmi SumoPod untuk membuat akun dan menyewa server:

👉 **Link SumoPod**: [https://sumopod.com](https://sumopod.com) atau [https://app.sumopod.com](https://app.sumopod.com)

### Panduan Memilih Spesifikasi VPS:
1. **Daftar / Login**: Masuk menggunakan akun Google atau email Anda.
2. **Top Up Saldo**: Isi saldo sesuai kebutuhan (bisa langsung scan QRIS pakai BCA, Mandiri, GoPay, OVO, ShopeePay, dll.).
3. **Pilih Menu "Create Server" / "Deploy VPS"**:
   - **Lokasi / Region**: Pilih **Jakarta, Indonesia** (agar koneksi paling cepat dan lancar).
   - **Sistem Operasi (OS)**: Pilih **Ubuntu 24.04 LTS** (atau **Ubuntu 22.04 LTS**). *Jangan pilih Windows atau OS lain.*
   - **Spesifikasi Server**:
     - *Minimal*: 1 vCPU, 2 GB RAM, 25 GB SSD (cukup untuk testing dasar).
     - *Rekomendasi Terbaik*: **2 vCPU, 4 GB RAM** (sangat stabil agar bot tidak kehabisan memori saat memproses file/gambar).
   - **Metode Autentikasi / Password**: Masukkan password root yang kuat (ingat dan catat baik-baik password ini!).
4. **Klik "Deploy" / "Buat VPS"**: Tunggu sekitar 1–2 menit sampai status server berubah menjadi **Running / Active**.

### Catat 3 Informasi Penting Ini dari Dashboard:
Setelah server aktif, catat 3 data berikut di Notepad komputer/HP Anda:
- **IP Address VPS** (Contoh: `103.187.xxx.xxx`)
- **Username**: Biasanya default-nya adalah `root`
- **Password**: Password yang Anda buat saat memesan VPS tadi

---

## Langkah 2: Cara Masuk (Koneksi) ke VPS Anda

Bagi pemula yang tidak pernah memakai Terminal koding, Anda bisa menggunakan aplikasi gratis yang mudah dipahami.

### Pilihan A: Menggunakan Termius (Paling Direkomendasikan untuk Pemula)
Aplikasi Termius sangat rapi dan ramah pemula, tersedia untuk Windows, Mac, Android, dan iPhone.

1. Download dan pasang **[Termius](https://termius.com/)** di laptop/PC Anda.
2. Buka Termius, klik tombol **"New Host"** (atau ikon `+`).
3. Isi formulir sederhana:
   - **Label**: `VPS OpenClaw` (bebas)
   - **Address (IP)**: Masukkan alamat IP VPS SumoPod Anda.
   - **Port**: Biarkan `22` (default).
   - **Username**: Ketik `root`.
   - **Password**: Ketik password VPS Anda.
4. Klik **Save**, lalu klik 2x pada server tersebut untuk terhubung.
5. Jika muncul notifikasi *"Host Key Fingerprint"*, klik **Accept / Continue**.
6. Anda akan melihat layar hitam dengan tulisan seperti `root@sumopod:~#`. Selamat, Anda sudah berada di dalam komputer VPS!

---

> ⚠️ **TIPS PENTING SAAT MENGGUNAKAN TERMINAL:**
> - Di Terminal, saat Anda mengetik password, **huruf atau tanda bintang (***) memang TIDAK AKAN MUNCUL**. Ini fitur keamanan standar Linux, bukan rusak/macet. Ketik saja password Anda dengan benar lalu tekan **Enter**.
> - Untuk paste teks di terminal: Klik kanan mouse atau tekan kombinasi `Ctrl + Shift + V` (Windows) / `Cmd + V` (Mac).

---

## Langkah 3: Menyiapkan VPS (Instalasi Dasar)

Sekarang kita akan menginstal piranti yang dibutuhkan OpenClaw: Node.js, npm, curl, dan git.

Cukup **copy dan paste** perintah-perintah berikut satu per satu ke terminal VPS Anda, lalu tekan **Enter**:

### 1. Perbarui Sistem VPS
```bash
apt update && apt upgrade -y
```
*Tunggu proses berjalan hingga selesai (sekitar 1-2 menit).*

### 2. Pasang Alat Pembantu (curl, git, build tools)
```bash
apt install -y curl git build-essential
```

### 3. Pasang Node.js Versi 22 (LTS Terbaru yang Direkomendasikan)
OpenClaw membutuhkan Node.js versi 22+. Jalankan perintah resmi ini:
```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -
apt install -y nodejs
```

### 4. Pastikan Node.js dan npm Sudah Terpasang
Ketik perintah ini:
```bash
node -v
npm -v
```
Jika muncul angka versi (misalnya `v22.x.x` dan `10.x.x`), artinya VPS Anda sudah 100% siap!

---

## Langkah 4: Install OpenClaw

Pasang OpenClaw secara global dengan satu perintah berikut:

```bash
npm install -g openclaw@latest
```

*Tunggu proses download dan instalasi selesai.*

Jika instalasi berhasil, cek dengan mengetik:
```bash
openclaw --version
```
Maka akan keluar versi OpenClaw terbaru yang terpasang.

---

## Langkah 5: Menjalankan Wizard Onboarding & Daemon 24 Jam

OpenClaw memiliki asisten setup otomatis (*wizard*) yang akan memandu Anda membuat konfigurasi awal dan mengatur agar OpenClaw tetap hidup 24 jam meskipun aplikasi terminal Anda ditutup.

Jalankan perintah ini:
```bash
openclaw onboard --install-daemon
```

### Apa yang Dilakukan Perintah Ini?
1. Menyiapkan folder data OpenClaw di `~/.openclaw`.
2. Menyiapkan service latar belakang (*systemd daemon*) sehingga bot otomatis hidup kembali jika VPS sempat restart.
3. Menjalankan pemeriksaan konfigurasi (*doctor*).

---

## Langkah 6: Menghubungkan AI Model (DeepSeek / OpenAI)

OpenClaw butuh "otak AI" untuk berpikir. Sangat disarankan memakai **DeepSeek** karena harganya sangat murah, cerdas, dan bisa dibeli langsung atau melalui API.

### Cara Mendapatkan API Key DeepSeek:
1. Buka [https://platform.deepseek.com](https://platform.deepseek.com).
2. Daftar/Login dan masuk ke menu **API Keys**.
3. Klik **Create new API Key**, beri nama (misal `OpenClaw-VPS`), lalu salin kunci yang diawali dengan `sk-...`.

### Memasukkan API Key ke OpenClaw:
Jalankan wizard konfigurasi model dengan mengetik:
```bash
openclaw config
```
Atau Anda bisa langsung mengeset environment variable:
```bash
openclaw config set auth.profiles.deepseek:default.mode api_key
```

Atau cukup jalankan perintah onboarding interaktif:
```bash
openclaw onboard
```
Lalu pilih provider model: **DeepSeek**, dan paste API Key Anda saat diminta.

---

## Langkah 7: Menghubungkan WhatsApp (Scan QR Code)

Salah satu fitur utama OpenClaw adalah menjadi asisten pribadi di WhatsApp.

### 1. Aktifkan Channel WhatsApp
Jalankan perintah untuk menghubungkan WhatsApp:
```bash
openclaw channels login whatsapp
```

### 2. Scan QR Code
1. Terminal akan menampilkan gambar **QR Code**.
2. Buka aplikasi **WhatsApp** di HP Anda.
3. Buka menu **Pengaturan (Settings)** > **Perangkat Tertaut (Linked Devices)**.
4. Pilih **Tautkan Perangkat (Link a Device)**.
5. Arahkan kamera HP ke QR Code yang ada di layar terminal Termius Anda.
6. Tunggu beberapa detik hingga WhatsApp menyatakan perangkat terhubung!

### 3. Membatasi Siapa yang Boleh Menyuruh Bot (Keamanan)
Secara default, Anda tentu tidak ingin sembarang orang di grup menyuruh-nyuruh bot Anda.
Buka konfigurasi untuk mengizinkan nomor WhatsApp Anda saja:
```bash
openclaw config set commands.ownerAllowFrom '["+6281234567890"]'
```
*(Ganti `+6281234567890` dengan nomor WhatsApp pribadi Anda lengkap dengan kode negara `+62`).*

---

## Langkah 8: Perintah Sehari-hari

Setelah bot Anda berjalan, berikut daftar perintah yang sering Anda perlukan:

| Kebutuhan | Perintah Terminal | Penjelasan |
| :--- | :--- | :--- |
| **Cek Kesehatan Bot** | `openclaw doctor` | Memeriksa apakah ada error atau modul yang terputus |
| **Cek Status Service** | `openclaw status` | Melihat apakah bot sedang menyala aktif di background |
| **Mulai Bot** | `openclaw gateway start` | Menjalankan gateway OpenClaw |
| **Hentikan Bot** | `openclaw gateway stop` | Menghentikan gateway |
| **Restart Bot** | `openclaw gateway restart` | Me-refresh bot setelah merubah konfigurasi |
| **Lihat Log / Aktivitas** | `openclaw logs --follow` | Melihat pesan masuk & respon bot secara live |
| **Update OpenClaw** | `npm install -g openclaw@latest` | Memperbarui OpenClaw ke versi terbaru |

---

## Troubleshooting & Masalah yang Sering Dialami Pemula

### 1. "Command not found: openclaw" setelah install npm
- **Penyebab**: Jalur instalasi global npm belum terdeteksi sistem.
- **Solusi**: Tutup aplikasi Termius, lalu buka dan login kembali ke VPS. Atau jalankan:
  ```bash
  hash -r
  ```

### 2. QR Code WhatsApp Rusak / Pecah di Layar
- **Penyebab**: Ukuran font terminal terlalu besar atau jendela terminal terlalu kecil.
- **Solusi**: Perbesar jendela Termius ke mode fullscreen (layar penuh), atau kecilkan ukuran font di pengaturan Termius (`Ctrl` + scroll mouse ke bawah). Lalu jalankan kembali `openclaw channels login whatsapp`.

### 3. Pesan WhatsApp Masuk tapi Bot Tidak Menjawab
- **Penyebab**: 
  1. API Key AI habis kuota / saldo (cek saldo di platform DeepSeek/OpenAI Anda).
  2. Nomor pengirim belum masuk daftar izin (`ownerAllowFrom`).
- **Solusi**: Cek log error dengan perintah:
  ```bash
  openclaw logs --lines 50
  ```
  Baca pesan error berwarna merah untuk mengetahui penyebabnya.

### 4. VPS SumoPod Tiba-tiba Tidak Bisa Dihubungi
- Buka dashboard [SumoPod](https://sumopod.com), cek status server Anda:
  - Pastikan status **Active**.
  - Jika macet, klik menu **Restart / Reboot Server**.

---

## Kamus Istilah untuk Orang Awam

- **IP Address (Alamat IP)**: Nomor identitas server Anda di internet, fungsinya seperti alamat rumah.
- **Root**: Akun administrator tertinggi di sistem Linux yang memiliki izin penuh mengubah apa saja.
- **SSH (Secure Shell)**: Terowongan aman untuk mengontrol komputer server dari jauh lewat teks.
- **Port**: Saluran/pintu masuk khusus untuk jenis lalu lintas data tertentu (misal port 22 untuk SSH, port 18789 untuk Gateway OpenClaw).
- **Daemon / Background Service**: Program yang berjalan di belakang layar secara diam-diam tanpa perlu jendela aplikasinya dibuka terus-menerus.
- **API Key**: Kode rahasia (seperti kata sandi) yang digunakan OpenClaw untuk mengakses kecerdasan buatan dari penyedia seperti DeepSeek atau OpenAI.

---

## 🔒 Tips Keamanan Penting untuk Pemula
1. **Jangan Sebarkan API Key**: Jangan pernah membagikan API key DeepSeek/OpenAI Anda kepada siapa pun atau mengunggahnya ke media sosial publik.
2. **Gunakan Password VPS yang Kuat**: Kombinasikan huruf besar, huruf kecil, angka, dan simbol (minimal 12 karakter).
3. **Selalu Cadangkan File Konfigurasi**:
   File konfigurasi OpenClaw ada di `~/.openclaw/openclaw.json`. Anda bisa mencadangkannya dengan perintah:
   ```bash
   cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.backup
   ```

Selamat! OpenClaw Anda sekarang sudah resmi online di VPS SumoPod dan siap membantu aktivitas harian Anda 24/7. 🎉
