# Mini Recruitment Dashboard

Mini Recruitment Dashboard adalah aplikasi sederhana untuk mengelola data recruitment seperti jobs, candidates, dan statistik dashboard. Project ini dibuat menggunakan Vue 3 dan Vite tanpa backend, sehingga data disimpan di browser menggunakan LocalStorage.

## Teknologi

- Vue 3 Composition API untuk membuat komponen dan mengelola state sederhana.
- Vite sebagai development server dan build tool.
- Vue Router untuk navigasi halaman.
- Bootstrap 5 untuk komponen UI dan layout responsive.
- Bootstrap Icons untuk ikon pada sidebar, tombol, card, dan tabel.
- Google Font Poppins untuk tampilan typography yang modern.
- LocalStorage untuk menyimpan data login, jobs, dan candidates.
- JavaScript, bukan TypeScript.

## Struktur Project

```text
src/
├── assets/
│   └── main.css
├── components/
│   ├── Navbar.vue
│   └── Sidebar.vue
├── layouts/
│   └── MainLayout.vue
├── router/
│   └── index.js
├── utils/
│   └── .gitkeep
├── views/
│   ├── CandidatesView.vue
│   ├── DashboardView.vue
│   ├── JobsView.vue
│   └── LoginView.vue
├── App.vue
└── main.js
```

## Penjelasan Folder

- `assets/` berisi file CSS global untuk styling aplikasi.
- `components/` berisi komponen kecil yang bisa digunakan ulang, seperti sidebar dan navbar.
- `layouts/` berisi layout utama dashboard.
- `router/` berisi konfigurasi route dan route guard.
- `utils/` disiapkan untuk helper function jika project dikembangkan lebih lanjut.
- `views/` berisi halaman utama aplikasi.
- `App.vue` menjadi root component.
- `main.js` menjadi entry point aplikasi.

## Fitur Utama

- Login sederhana dengan dummy authentication.
- Route guard untuk membatasi akses dashboard jika belum login.
- Dashboard statistik dari data LocalStorage.
- Halaman Jobs dengan tabel, search, filter status, dan add new job.
- Halaman Candidates dengan tampilan Kanban Board.
- Responsive layout untuk desktop dan mobile.

## Cara Menjalankan Project

Install dependency:

```bash
npm install
```

Jalankan development server:

```bash
npm run dev
```

Build untuk production:

```bash
npm run build
```

Preview hasil build:

```bash
npm run preview
```

## Akun Login Dummy

```text
Email: admin@recruit.com
Password: 123456
```

## Catatan Deployment

Project ini sudah memiliki `vercel.json` untuk mendukung Vue Router mode history saat deploy ke Vercel. Dengan konfigurasi ini, halaman seperti `/dashboard`, `/jobs`, dan `/candidates` tetap bisa dibuka langsung atau di-refresh tanpa error 404.
