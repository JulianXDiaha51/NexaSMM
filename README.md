# ⚡ NexaSMM Bot — Changelog

> **Platform:** Telegram Bot (Node.js + Telegraf)  
> **Integrasi:** MedanPedia API + Pakasir Payment Gateway  
> **Total File:** 35 file JavaScript

---

## 🆕 v1.1.0 — Markup Pricing System
> Sistem markup harga otomatis — jual lebih mahal dari harga asli, profit masuk ke kamu.

### 💹 Fitur Baru

| Fitur | Keterangan |
|---|---|
| `services/pricing.js` | Engine hitung markup — support global, per kategori, per layanan spesifik |
| `/setmarkup global [%]` | Set markup untuk semua layanan sekaligus |
| `/setmarkup kategori [nama] [%]` | Set markup untuk satu kategori tertentu |
| `/setmarkup layanan [id] [%]` | Set markup untuk satu layanan spesifik |
| `/setmarkup info` | Lihat semua markup yang sedang aktif |
| `/setmarkup reset` | Hapus semua markup kembali ke 0% |
| Harga Otomatis | Harga di `/services` dan detail layanan sudah include markup otomatis |
| Pencatatan Profit | Setiap order tercatat profit-nya di `data/profit.json` |
| Statistik Profit | Panel admin `/admin` → Statistik sekarang tampilkan **Total Profit** |
| Transparansi Admin | Admin bisa lihat harga asli + markup + profit per order di detail layanan |
| Pembulatan Cerdas | Harga markup dibulatkan ke atas ke ratusan terdekat |

### 🔄 Yang Diperbarui

| File | Perubahan |
|---|---|
| `commands/services.js` | Tampilkan harga setelah markup, badge persentase markup |
| `handlers/callbackHandler.js` | Detail layanan tampil harga markup + info profit khusus admin |
| `handlers/textHandler.js` | Cek saldo & potong saldo pakai harga markup, catat profit tiap order |
| `handlers/adminHandler.js` | Statistik tambah baris Total Profit |
| `services/database.js` | Tambah fungsi `addProfitRecord` & `getProfitStats` |
| `package.json` | Versi naik ke 1.1.0 |

### 💡 Simulasi Profit dengan Markup 30%

| Layanan | Harga Asli | Harga User | Profit |
|---|---|---|---|
| IG Followers 1000 | Rp 15.000 | Rp 19.500 | Rp 4.500 |
| TikTok Views 5000 | Rp 8.000 | Rp 10.400 | Rp 2.400 |
| YT Subscribers | Rp 50.000 | Rp 65.000 | Rp 15.000 |

---

## 🚀 v1.0.0 — Initial Release
> Rilis pertama NexaSMM Bot dengan fitur lengkap siap produksi.

### 🛒 Fitur Order & Layanan

| Fitur | Keterangan |
|---|---|
| `/services` | Daftar semua layanan dengan **pagination** (8 per halaman) menggunakan inline keyboard |
| `/order` | Wizard pemesanan bertahap: pilih layanan → target → jumlah → konfirmasi |
| Panduan Target | Bot otomatis tampilkan format target yang benar berdasarkan platform (IG, TikTok, YT, FB, dll) |
| Validasi Min/Max | Order ditolak otomatis jika jumlah di bawah minimum atau di atas maksimum layanan |
| Cek Saldo Sebelum Order | Bot cek saldo user sebelum proses — langsung tolak jika tidak cukup |
| Detail Layanan | Klik nama layanan → tampil detail lengkap (harga, min, max, tipe, estimasi, deskripsi) |
| Tombol Order Cepat | Dari halaman detail langsung bisa order tanpa perlu ketik ID manual |
| `/status` | Cek status pesanan berdasarkan Order ID |
| `/refill` | Request refill pesanan |
| `/refill_status` | Cek status refill berdasarkan Refill ID |

### 💳 Sistem Saldo & Pembayaran

| Fitur | Keterangan |
|---|---|
| `/topup` | Top-up saldo dengan nominal bebas (min Rp 1.000) |
| QRIS Otomatis | Bot generate gambar QRIS dari Pakasir API menggunakan library `qrcode` |
| Polling Otomatis | Cek status pembayaran tiap **10 detik** secara otomatis |
| Cek Manual | Tombol **✅ Cek Status Manual** di bawah QRIS untuk cek instan |
| Batal Pembayaran | Tombol **❌ Batalkan** untuk membatalkan QRIS yang aktif |
| Timeout 10 Menit | QRIS otomatis expired setelah 10 menit |
| `/saldo` | Cek saldo, total topup, dan total order |
| `/riwayat` | Riwayat 5 topup & 5 order terakhir |
| Database JSON | Data user, saldo, riwayat topup & order disimpan di `data/users.json` |

### 🔔 Sistem Notifikasi

| Fitur | Keterangan |
|---|---|
| Notif Order ke Channel | Setiap order berhasil → bot kirim notifikasi ke channel Telegram |
| Notif Topup ke Channel | Setiap topup berhasil → bot kirim notifikasi + **foto QRIS** ke channel |
| Auto Notif Order Selesai | Bot polling status order tiap **30 detik**, notif otomatis ke user saat status berubah jadi Success / Partial / Error |
| Format Notif Lengkap | Notif berisi: nama user, ID, layanan, target, jumlah, biaya, waktu |

### 👑 Panel Admin

| Fitur | Keterangan |
|---|---|
| `/admin` | Panel admin dengan inline keyboard |
| Statistik | Total user, total topup, total order, total saldo, total profit, jumlah order aktif |
| Daftar User | Lihat 10 user terbaru + detail per user (saldo, riwayat topup) |
| Broadcast | Kirim pesan ke **semua user** sekaligus (support HTML formatting) |
| `/addsaldo [id] [nominal]` | Tambah saldo user secara manual |
| `/delsaldo [id] [nominal]` | Kurangi saldo user secara manual |
| Notif ke User | Setiap kelola saldo → user otomatis dapat notifikasi |
| Proteksi Admin | Semua command admin hanya bisa diakses oleh `ADMIN_ID` di `.env` |

### 🎨 UI & UX

| Fitur | Keterangan |
|---|---|
| Menu Bergambar | `/menu` tampilkan foto header + caption profesional |
| Custom Keyboard | Keyboard shortcut di bawah chat (Order, Cek Status, Layanan, dll) |
| Auto Delete Pesan | Semua pesan bot terhapus otomatis (3 detik - 10 menit tergantung jenis) |
| Pesan Error Deskriptif | Setiap error dari API ditampilkan pesannya ke user |
| Konfirmasi Order | Tampil ringkasan lengkap sebelum konfirmasi |

### 🛠 Teknis & Infrastruktur

| Fitur | Keterangan |
|---|---|
| Modular Architecture | 35 file terpisah — commands, handlers, services, utils |
| Session Per User | Setiap user punya session sendiri untuk wizard multi-step |
| Colored Logger | Console log berwarna dengan level INFO, OK, WARN, ERR, ORDER, BOT |
| Request Logging | Setiap update dari user tercatat di console |
| Error Handling | Try-catch di semua layer — API, handler, service |
| Environment Variables | Semua kredensial di `.env` |
| Axios Interceptor | Error 4xx dari API ditangkap dan dikembalikan sebagai response |

### 📦 Stack & Dependencies

| Package | Versi | Fungsi |
|---|---|---|
| `telegraf` | ^4.15.3 | Telegram Bot Framework |
| `axios` | ^1.6.0 | HTTP Client untuk API |
| `qrcode` | ^1.5.3 | Generate gambar QR dari string |
| `dotenv` | ^16.3.1 | Environment variables |

### ⚙️ Environment Variables

| Variable | Keterangan |
|---|---|
| `BOT_TOKEN` | Token dari @BotFather |
| `API_ID` | API ID MedanPedia |
| `API_KEY` | API Key MedanPedia |
| `PAKASIR_PROJECT` | Project slug Pakasir |
| `PAKASIR_API_KEY` | API Key Pakasir |
| `NOTIFY_CHANNEL_ID` | ID channel notifikasi (opsional) |
| `ADMIN_ID` | Telegram ID admin |

### 📁 Struktur File

```
NexaSMM/
├── index.js                    ← Entry point
├── .env                        ← Kredensial
│
├── commands/                   ← 17 commands
│   ├── start.js    menu.js     order.js      status.js
│   ├── services.js topup.js    saldo.js      riwayat.js
│   ├── refill.js   refill_status.js          help.js
│   ├── cancel.js   admin.js    addsaldo.js   delsaldo.js
│   ├── setmarkup.js            balance.js
│
├── handlers/
│   ├── textHandler.js          ← Wizard multi-step
│   ├── callbackHandler.js      ← Inline keyboard
│   ├── topupHandler.js         ← Proses topup QRIS
│   └── adminHandler.js         ← Panel admin
│
├── services/
│   ├── medanpedia.js           ← MedanPedia API
│   ├── pakasir.js              ← Pakasir Payment
│   ├── database.js             ← JSON database
│   ├── notifier.js             ← Notif channel
│   ├── topupChecker.js         ← Polling topup
│   ├── orderChecker.js         ← Polling order
│   └── pricing.js              ← Engine markup harga
│
├── data/                       ← Auto dibuat saat runtime
│   ├── users.json              ← Data user & saldo
│   ├── markup.json             ← Setting markup
│   └── profit.json             ← Rekap profit
│
└── utils/
    ├── autoDelete.js
    ├── formatter.js
    ├── logger.js
    ├── session.js
    ├── targetGuide.js
    └── validator.js
```

---

## 🗺️ Roadmap

| Versi | Fitur | Status |
|---|---|---|
| v1.0.0 | Initial Release — fitur dasar lengkap | ✅ Done |
| v1.1.0 | Markup Pricing System + Profit Tracking | ✅ Done |
| v1.2.0 | Upgrade Database ke MySQL | 🔜 Planned |
| v1.3.0 | Web Panel Admin | 🔜 Planned |
| v1.4.0 | Multi Payment Gateway | 🔜 Planned |
| v1.5.0 | Sistem Referral / Affiliate | 🔜 Planned |
| v2.0.0 | White Label Ready + Installer Otomatis | 🔜 Planned |

---

*NexaSMM v1.1.0 — Built with ❤️*
