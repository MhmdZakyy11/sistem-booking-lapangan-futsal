# Sistem Booking Lapangan Futsal (Laravel 12)

Aplikasi web untuk memesan lapangan futsal secara online: kelola lapangan, slot jadwal, dan transaksi booking berbasis Laravel 12 + Tailwind + Vite.

## Fitur Utama
- ✅ Autentikasi pengguna (login/registrasi, reset password) — gunakan Laravel Breeze/Fortify.
- ✅ Manajemen lapangan: tambah/edit/hapus lapangan, foto, harga per jam.
- ✅ Jadwal & ketersediaan: definisi slot harian/mingguan, penandaan “booked/available”.
- ✅ Booking real-time: validasi bentrok (lapangan + tanggal + jam unik).
- ✅ Riwayat booking pengguna & admin dashboard.
- ✅ Status pembayaran sederhana (pending/paid) dengan opsi mock/manual; siap integrasi payment gateway.
- ✅ Notifikasi email (opsional) saat booking dibuat/dikonfirmasi.
- ✅ Mode gelap/terang mengikuti Tailwind + preferensi OS.

## Arsitektur Singkat
- **Framework**: Laravel 12 (PHP >= 8.2)
- **View**: Blade + Tailwind CSS (via Vite)
- **DB**: MySQL/MariaDB (bisa SQLite untuk dev)
- **ORM**: Eloquent + Migrations + Seeders
- **Tooling**: PHPUnit, Laravel Pint, Laravel Pail, Vite

## Persiapan & Instalasi
```bash
git clone https://github.com/<owner>/sistem-booking-lapangan-futsal.git
cd sistem-booking-lapangan-futsal

# 1) Dependensi PHP
composer install

# 2) File environment & kunci aplikasi
cp .env.example .env
php artisan key:generate

# 3) Konfigurasi database di .env
# DB_DATABASE=...
# DB_USERNAME=...
# DB_PASSWORD=...

# 4) Migrasi & seeder (opsional seeder demo)
php artisan migrate --seed

# 5) Dependensi front-end
npm install
npm run dev     # atau npm run build untuk produksi
```

## Menjalankan Aplikasi
```bash
php artisan serve        # http://localhost:8000
# jika pakai Sail (Docker): ./vendor/bin/sail up
```

## Struktur Direktori Penting
- `app/Models` — Model domain (Field, Schedule, Booking, Payment)
- `app/Http/Controllers` — Controller web & API
- `app/Policies` — Akses admin/user
- `database/migrations` — Skema DB (lapangan, jadwal, booking, pembayaran)
- `database/seeders` — Data awal lapangan & slot contoh
- `resources/views` — Blade templates (landing, dashboard, booking form)
- `routes/web.php` — Route web utama
- `routes/api.php` — Endpoint API (opsional)
- `public/` — Assets build

## Rancangan Skema Data (ringkas)
- `fields` : id, name, description, price_per_hour, photo_url, status
- `schedules` : id, field_id, date, start_time, end_time, is_available
- `bookings` : id, user_id, field_id, schedule_id, total_price, status (pending/confirmed/cancelled), payment_status
- `payments` (opsional): booking_id, method, reference, paid_at

## Alur Booking
1) User login/registrasi.  
2) Pilih lapangan → pilih tanggal & slot tersedia.  
3) Sistem cek bentrok (unique constraint `field_id+date+start_time`).  
4) Simpan booking (status *pending*).  
5) (Opsional) Proses pembayaran → set `payment_status=paid`, `status=confirmed`.  
6) Tampilkan riwayat booking & detail.

## Validasi & Keamanan
- CSRF & session security bawaan Laravel.
- Form Request Validation untuk input booking/lapangan.
- Policy/ Gate: hanya admin boleh CRUD lapangan & slot; user biasa hanya mengelola booking-nya.
- Unique index pada kombinasi `field_id, schedule_id` mencegah double-booking.

## Testing
```bash
php artisan test        # PHPUnit
```

## Format Code
```bash
composer lint    # alias ke laravel/pint
```

## Deployment Cepat
- Pastikan `.env` produksi (APP_KEY, DB, MAIL, QUEUE_CONNECTION, dll.).
- `php artisan migrate --force`
- `npm run build`
- Gunakan queue worker untuk email/notification jika diaktifkan.
- Konfigurasi storage symlink: `php artisan storage:link`.

## Roadmap (opsional)
- Integrasi payment gateway (Midtrans/Xendit).
- Fitur kupon/discount.
- Mode multi-cabang (beberapa lokasi futsal).
- Peta & penjadwalan multi-lapangan paralel.
- Pemberitahuan WhatsApp via webhook.

## Lisensi
MIT License.
