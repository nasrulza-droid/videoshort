# Panduan Admin IPDCEC 2026 Short Video

## 1) Link Utama

- Landing page: https://videoshort.ipdcec.online/index.html
- Form peserta (ID): https://videoshort.ipdcec.online/register.html
- Form peserta (EN): https://videoshort.ipdcec.online/register-en.html
- Dashboard admin: https://videoshort.ipdcec.online/panel-panitia-2026.html

## 2) Akun Admin (4 Orang)

Password sementara bersama:

IPDCECAdmin@2026!Team

Daftar admin:

1. asyaidatul070@gmail.com (Syaida)
2. diahsandi563@gmail.com (Diah)
3. zhafirawirda@gmail.com (Zhafira)
4. nasrulza@unimal.ac.id (Nasrul ZA)

Status saat ini:

- nasrulza@unimal.ac.id: aktif dan bisa login.
- asyaidatul070@gmail.com: akun sudah dibuat, menunggu konfirmasi email Supabase.
- diahsandi563@gmail.com: akun sudah dibuat, kemungkinan masih perlu konfirmasi email sebelum sesi login penuh stabil.
- zhafirawirda@gmail.com: belum berhasil dibuat otomatis karena rate limit pengiriman email Supabase.

## 3) Aktivasi Agar 4 Admin Bisa Masuk

Lakukan dari Supabase Dashboard:

1. Buka Authentication > Users.
2. Cari user Syaida dan Diah.
3. Set sebagai email confirmed (atau minta user klik email konfirmasi).
4. Buat user Zhafira manual dengan email zhafirawirda@gmail.com dan password sementara yang sama.
5. Setelah login pertama berhasil, setiap admin wajib ganti password masing-masing.

Catatan keamanan:

- Jangan bagikan password di grup publik.
- Ganti password minimal 1x setiap awal periode lomba.

## 4) Notifikasi Aktivitas Peserta

Halaman admin sudah disiapkan untuk pola notifikasi operasional berikut:

- Menampilkan aktivitas peserta terbaru di panel notifikasi admin.
- Auto-refresh data berkala di dashboard.
- Tombol Tandai Semua Sudah Dibaca untuk tiap akun admin.
- Notifikasi browser lokal (jika admin mengizinkan permission notifikasi di browser).

Makna notifikasi:

- Aktivitas utama peserta adalah submit pendaftaran baru.
- Setiap submit baru akan tampil sebagai item baru di panel notifikasi admin.

## 5) SOP Harian 4 Admin

1. Login ke dashboard admin.
2. Cek panel Notifikasi Aktivitas Peserta.
3. Buka tabel pendaftar, cek berkas wajib:
   - Post Instagram
   - Video
   - Kartu Pelajar
   - Twibbon
   - Poster
   - Bukti Bayar
4. Ubah status:
   - pending: belum dicek
   - verified: lengkap dan valid
   - incomplete: ada berkas kurang/salah
   - rejected: tidak memenuhi syarat
5. Isi Catatan admin singkat dan jelas.
6. Klik Simpan.
7. Jika selesai verifikasi batch, klik Unduh CSV Tampilan untuk backup lokal panitia.

## 6) SOP Komunikasi Peserta

- Gunakan entry key untuk referensi peserta saat komunikasi.
- Saat status incomplete/rejected, kirim alasan spesifik berdasarkan catatan admin.
- Simpan bukti komunikasi di kanal internal panitia.

## 7) Troubleshooting Cepat

Masalah: Login gagal Email not confirmed

Solusi:

1. Buka Supabase Authentication > Users.
2. Set user menjadi confirmed.
3. Coba login ulang.

Masalah: Login gagal dengan pesan Akun belum aktif atau belum tersedia

Solusi:

1. Buka Supabase Authentication > Users.
2. Pastikan user benar-benar ada. Jika belum ada, buat user manual dengan email admin terkait.
3. Set Email Confirmed = true.
4. Set password sementara baru (contoh: IPDCECAdmin@2026!Team), lalu minta admin login ulang.

Masalah: Login gagal Invalid login credentials

Solusi:

1. Klik tombol Kirim Link Reset Password di halaman admin, atau reset manual dari Supabase Dashboard.
2. Pastikan admin memakai password terbaru, bukan password lama.
3. Setelah reset berhasil, login ulang.

Masalah: Supabase mengembalikan error 500 atau unexpected_failure saat login/sign up

Solusi:

1. Catat error_id dari respons Supabase (jika ada).
2. Cek status Supabase project dan Auth service (dashboard service health).
3. Buat ulang user secara manual di Authentication > Users.
4. Jika tetap gagal, laporkan ke Supabase support dengan menyertakan error_id untuk investigasi database Auth.

Masalah: Admin berhasil login tetapi data tidak muncul

Solusi:

1. Klik Refresh Data.
2. Cek koneksi internet.
3. Pastikan URL dan anon key di supabase-config.js benar.

Masalah: Notifikasi browser tidak muncul

Solusi:

1. Izinkan browser notification untuk domain videoshort.ipdcec.online.
2. Reload halaman admin.

## 8) Rekomendasi Operasional

- Bagi tugas per admin (misalnya per negara atau per batch waktu submit).
- Tetapkan jam patroli dashboard, contoh setiap 2 jam.
- Lakukan ekspor CSV di akhir hari sebagai backup.
- Jangan ubah data mentah peserta di luar kolom status dan catatan admin.
