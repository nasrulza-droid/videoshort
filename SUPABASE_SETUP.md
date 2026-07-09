# Supabase Setup for IPDCEC 2026 Admin

Halaman [admin.html](admin.html) memakai Supabase Auth + PostgreSQL.

## 1. Create Project

1. Buat project baru di Supabase.
2. Buka Authentication > Providers.
3. Aktifkan Email provider.
4. Buat minimal satu user admin di Authentication > Users.
5. Catat `Project URL` dan `anon public key` dari Project Settings > API.

Catatan: project Supabase tidak bisa dibuat otomatis dari repo GitHub Pages ini tanpa login akun Supabase milikmu. Yang bisa saya siapkan dari sisi kode adalah seluruh struktur frontend, SQL schema, dan file konfigurasi agar setelah project dibuat kamu tinggal mengisi kredensialnya.

## 2. Fill Frontend Config

Edit [supabase-config.js](supabase-config.js) dan isi:

```js
export const SUPABASE_CONFIG = {
  url: "https://YOUR_PROJECT.supabase.co",
  anonKey: "YOUR_PUBLIC_ANON_KEY"
};
```

## 3. Run SQL Schema

Jalankan SQL berikut di Supabase SQL Editor:

```sql
create extension if not exists pgcrypto;

create table if not exists public.admin_users (
  email text primary key,
  created_at timestamptz not null default now()
);

create table if not exists public.registrations (
  id uuid primary key default gen_random_uuid(),
  entry_key text not null unique,
  submitted_at timestamptz,
  team_name text,
  participant_name text,
  school_name text,
  country text,
  email text,
  phone text,
  instagram text,
  work_title text,
  verification_status text not null default 'pending',
  admin_notes text not null default '',
  raw_data jsonb not null default '{}'::jsonb,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),
  constraint registrations_status_check check (
    verification_status in ('pending', 'verified', 'incomplete', 'rejected')
  )
);

create or replace function public.is_admin_user()
returns boolean
language sql
stable
as $$
  select exists (
    select 1
    from public.admin_users
    where email = auth.email()
  );
$$;

alter table public.admin_users enable row level security;
alter table public.registrations enable row level security;

create policy "admin_users_select_self"
on public.admin_users
for select
to authenticated
using (email = auth.email());

create policy "registrations_select_admin"
on public.registrations
for select
to authenticated
using (public.is_admin_user());

create policy "registrations_insert_admin"
on public.registrations
for insert
to authenticated
with check (public.is_admin_user());

create policy "registrations_insert_public"
on public.registrations
for insert
to anon
with check (true);

create policy "registrations_update_admin"
on public.registrations
for update
to authenticated
using (public.is_admin_user())
with check (public.is_admin_user());
```

## 4. Register Admin Emails

Tambahkan email admin yang boleh membuka dashboard:

```sql
insert into public.admin_users (email)
values
  ('admin@ipdcec.online'),
  ('your-real-admin@example.com')
on conflict (email) do nothing;
```

Pastikan email di tabel `admin_users` sama dengan email user di Supabase Authentication.

## 5. Public Registration Form

Setelah `supabase-config.js` diisi, halaman berikut akan langsung submit ke database:

- [register.html](register.html)
- [register-en.html](register-en.html)

Google Form lama bisa tetap dipakai sebagai fallback sementara, tetapi jalur utama pendaftaran sekarang bisa diarahkan ke form internal ini.

## 6. Daily Workflow

1. Peserta daftar lewat [register.html](register.html) atau [register-en.html](register-en.html).
2. Data langsung masuk ke tabel `registrations`.
3. Login ke [admin.html](admin.html) untuk review, verifikasi, dan catatan admin.
4. Gunakan impor CSV hanya untuk migrasi data lama dari Google Form bila diperlukan.
5. Review status pendaftar dan simpan catatan admin.

## 7. Important Notes

- Karena situs ini di GitHub Pages, backend dijalankan oleh Supabase, bukan server di repo.
- `anonKey` aman dipakai di frontend, tapi policy RLS wajib aktif.
- Jika ingin pendaftaran tanpa Google Form, tahap berikutnya adalah membuat form submit langsung ke Supabase.
