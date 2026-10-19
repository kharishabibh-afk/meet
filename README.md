# Ruang Cerita

Chat dengan karakter buatan sendiri. Stack: Cloudflare Pages + Functions, D1, Workers AI (Llama 3 8B), login Google.

## Alur aplikasi
Login Google, lalu buat karakter (nama, jenis kelamin, karakter yang disukai), lalu profil pengguna (nama pengguna, jenis kelamin), lalu chat.
Di dalam chat ada tab Chat, Chat baru, dan Buku ingatan. Di bawah tiap balasan karakter ada tombol Tambah ingatan dan Reload respons (reload hanya untuk balasan terakhir).

## Persiapan
1. Pasang Node.js (LTS), VS Code, dan Git. Buka folder ini di VS Code.
2. Di terminal: `npm install`
3. `npx wrangler login` (masuk ke akun Cloudflare)
4. `npx wrangler d1 create chat-karakter-db`, lalu salin `database_id` yang muncul ke `wrangler.toml`.
5. Buat tabel: `npm run db:init` (online) dan `npm run db:init:local` (untuk tes lokal).

## Google Client ID
1. Buka Google Cloud Console, APIs & Services, Credentials, Create credentials, OAuth client ID, tipe Web application.
2. Authorized JavaScript origins: `http://localhost:8788` dan `https://NAMA-PROYEK.pages.dev` (isi setelah deploy; domain sendiri juga boleh).
3. Salin Client ID ke `GOOGLE_CLIENT_ID` di `wrangler.toml`.

## Jalankan lokal
`npm run dev`, lalu buka http://localhost:8788
Catatan: Workers AI tetap memakai kuota akunmu walau dites lokal.

## Deploy
1. Push ke GitHub.
2. Cloudflare Dashboard, Workers & Pages, Create, Pages, Connect to Git, pilih repo.
   Build command: kosongkan. Build output directory: `public`.
3. Binding D1, AI, dan variabel dibaca dari `wrangler.toml`. Setelah deploy, tambahkan domain `.pages.dev` ke Authorized origins Google.
Alternatif tanpa Git: `npm run deploy`.

## Kuota
Free tier Workers AI: 10.000 Neurons per hari, dipakai bersama semua pengguna. Llama 3 8B kira-kira 60 Neurons per giliran chat (perkiraan kasar). Reload respons juga memakai kuota.
