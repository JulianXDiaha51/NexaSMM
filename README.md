# NexaSMM Bot — Changelog

Platform: Telegram Bot (Node.js + Telegraf)
Integrasi: MedanPedia API + Pakasir Payment Gateway
Total File: 35 file JavaScript

---

## v1.1.0 — Markup Pricing System

Sistem markup harga otomatis. Admin set persentase keuntungan, harga tampil ke user sudah include markup, profit tercatat otomatis.

### Fitur Baru

| Fitur | Keterangan |
|---|---|
| `services/pricing.js` | Engine hitung markup — support global, per kategori, per layanan |
| `/setmarkup global [%]` | Set markup untuk semua layanan |
| `/setmarkup kategori [nama] [%]` | Set markup untuk satu kategori |
| `/setmarkup layanan [id] [%]` | Set markup untuk satu layanan spesifik |
| `/setmarkup info` | Lihat semua markup yang aktif |
| `/setmarkup reset` | Hapus semua markup kembali ke 0% |
| Harga Otomatis | Harga di `/services` dan detail layanan sudah include markup |
| Pencatatan Profit | Setiap order tercatat profit di `data/profit.json` |
| Statistik Profit | Panel admin tampilkan total profit |
| Info Admin | Admin bisa lihat harga asli + markup + profit di detail layanan |
| Pembulatan | Harga markup dibulatkan ke ratusan terdekat ke atas |

### Yang Diperbarui

| File | Perubahan |
|---|---|
| `commands/services.js` | Harga tampil setelah markup |
| `handlers/callbackHandler.js` | Detail layanan tampil info profit khusus admin |
| `handlers/textHandler.js` | Saldo dipotong pakai harga markup, profit dicatat |
| `handlers/adminHandler.js` | Statistik tambah baris total profit |
| `services/database.js` | Tambah fungsi `addProfitRecord` dan `getProfitStats` |
| `package.json` | Versi naik ke 1.1.0 |

### Simulasi Profit (markup 30%)

| Layanan | Harga Asli | Harga User | Profit |
|---|---|---|---|
| IG Followers 1000 | Rp 15.000 | Rp 19.500 | Rp 4.500 |
| TikTok Views 5000 | Rp 8.000 | Rp 10.400 | Rp 2.400 |
| YT Subscribers | Rp 50.000 | Rp 65.000 | Rp 15.000 |

---

## v1.0.0 — Initial Release

Rilis pertama NexaSMM Bot.

### Order & Layanan

| Fitur | Keterangan |
|---|---|
| `/services` | Daftar layanan dengan pagination (8 per halaman), inline keyboard |
| `/order` | Wizard pemesanan: pilih layanan → target → jumlah → konfirmasi |
| Panduan Target | Format target otomatis sesuai platform (IG, TikTok, YT, FB, dll) |
| Validasi Min/Max | Order ditolak jika jumlah di luar batas layanan |
| Cek Saldo | Bot cek saldo sebelum proses order |
| Detail Layanan | Klik layanan untuk lihat detail lengkap |
| Tombol Order Cepat | Order langsung dari halaman detail tanpa ketik ID manual |
| `/status` | Cek status pesanan |
| `/refill` | Request refill pesanan |
| `/refill_status` | Cek status refill |

### Saldo & Pembayaran

| Fitur | Keterangan |
|---|---|
| `/topup` | Top-up saldo (min Rp 1.000) |
| QRIS | Generate gambar QRIS dari Pakasir via library `qrcode` |
| Polling Otomatis | Cek status bayar tiap 10 detik |
| Cek Manual | Tombol cek status di bawah QRIS |
| Batal | Tombol batalkan QRIS aktif |
| Timeout | QRIS expired setelah 10 menit |
| `/saldo` | Cek saldo, total topup, total order |
| `/riwayat` | 5 topup dan 5 order terakhir |
| Database | Data disimpan di `data/users.json` |

### Notifikasi

| Fitur | Keterangan |
|---|---|
| Notif Order | Setiap order masuk → notifikasi ke channel |
| Notif Topup | Topup berhasil → notifikasi + foto QRIS ke channel |
| Notif Order Selesai | Polling status tiap 30 detik, notif ke user saat Success/Partial/Error |

### Panel Admin

| Fitur | Keterangan |
|---|---|
| `/admin` | Panel admin dengan inline keyboard |
| Statistik | Total user, topup, order, saldo, profit, order aktif |
| Daftar User | 10 user terbaru + detail per user |
| Broadcast | Kirim pesan ke semua user, support HTML formatting |
| `/addsaldo [id] [nominal]` | Tambah saldo user |
| `/delsaldo [id] [nominal]` | Kurangi saldo user |
| Proteksi | Hanya `ADMIN_ID` di `.env` yang bisa akses |

### UI & UX

| Fitur | Keterangan |
|---|---|
| Menu | Foto header + caption + custom keyboard |
| Auto Delete | Pesan bot terhapus otomatis (3 detik - 10 menit) |
| Error Message | Pesan error dari API ditampilkan langsung ke user |
| Konfirmasi Order | Ringkasan lengkap sebelum order diproses |

### Teknis

| Fitur | Keterangan |
|---|---|
| Struktur | Modular, 35 file terpisah |
| Session | Tiap user punya session sendiri |
| Logger | Console log berwarna (INFO, OK, WARN, ERR, ORDER) |
| Error Handling | Try-catch di semua layer |
| Axios Interceptor | Error 4xx dari API tidak throw, dikembalikan sebagai response |

### Dependencies

| Package | Versi | Fungsi |
|---|---|---|
| `telegraf` | ^4.15.3 | Telegram Bot Framework |
| `axios` | ^1.6.0 | HTTP Client |
| `qrcode` | ^1.5.3 | Generate gambar QR |
| `dotenv` | ^16.3.1 | Environment variables |

### Environment Variables

| Variable | Keterangan |
|---|---|
| `BOT_TOKEN` | Token dari @BotFather |
| `API_ID` | API ID MedanPedia |
| `API_KEY` | API Key MedanPedia |
| `PAKASIR_PROJECT` | Project slug Pakasir |
| `PAKASIR_API_KEY` | API Key Pakasir |
| `NOTIFY_CHANNEL_ID` | ID channel notifikasi (opsional) |
| `ADMIN_ID` | Telegram ID admin |

### Struktur File

```
NexaSMM/
├── index.js
├── .env
├── commands/           (17 files)
│   ├── start.js        menu.js         order.js
│   ├── status.js       services.js     topup.js
│   ├── saldo.js        riwayat.js      refill.js
│   ├── refill_status.js help.js        cancel.js
│   ├── admin.js        addsaldo.js     delsaldo.js
│   ├── setmarkup.js    balance.js
├── handlers/
│   ├── textHandler.js
│   ├── callbackHandler.js
│   ├── topupHandler.js
│   └── adminHandler.js
├── services/
│   ├── medanpedia.js
│   ├── pakasir.js
│   ├── database.js
│   ├── notifier.js
│   ├── topupChecker.js
│   ├── orderChecker.js
│   └── pricing.js
├── data/               (auto dibuat)
│   ├── users.json
│   ├── markup.json
│   └── profit.json
└── utils/
    ├── autoDelete.js
    ├── formatter.js
    ├── logger.js
    ├── session.js
    ├── targetGuide.js
    └── validator.js
```

---

## Roadmap

| Versi | Fitur | Status |
|---|---|---|
| v1.0.0 | Initial Release | Done |
| v1.1.0 | Markup Pricing + Profit Tracking | Done |
| v1.2.0 | Database MySQL | Planned |
| v1.3.0 | Web Panel Admin | Planned |
| v1.4.0 | Multi Payment Gateway | Planned |
| v1.5.0 | Sistem Referral | Planned |
| v2.0.0 | White Label + Installer Otomatis | Planned |

---

NexaSMM v1.1.0
