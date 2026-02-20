# Next.js Dashboard — Laporan Penyelesaian Chapter 1 s/d 6

Proyek ini merupakan implementasi aplikasi **financial dashboard** berbasis Next.js, dikembangkan sebagai bagian dari kursus Next.js Foundations yang diadaptasi oleh Laboratorium Informatika FT-UNISMUH (Universitas Muhammadiyah Makassar).

Dokumen ini menjelaskan secara teknis penyelesaian setiap chapter, mulai dari inisialisasi proyek hingga konfigurasi database.

---

## Daftar Isi

1. [Tech Stack](#tech-stack)
2. [Struktur Proyek](#struktur-proyek)
3. [Chapter 1 — Inisialisasi Proyek](#chapter-1--inisialisasi-proyek)
4. [Chapter 2 — CSS Styling](#chapter-2--css-styling)
5. [Chapter 3 — Optimasi Font dan Image](#chapter-3--optimasi-font-dan-image)
6. [Chapter 4 — Layouts dan Pages](#chapter-4--layouts-dan-pages)
7. [Chapter 5 — Navigasi Antar Halaman](#chapter-5--navigasi-antar-halaman)
8. [Chapter 6 — Setup Database](#chapter-6--setup-database)
9. [Cara Menjalankan](#cara-menjalankan)

---

## Tech Stack

| Komponen         | Teknologi                          |
|------------------|------------------------------------|
| Framework        | Next.js 16.0.10 (App Router)       |
| Bahasa           | TypeScript                         |
| Bundler          | Turbopack                          |
| Package Manager  | pnpm                               |
| CSS Framework    | Tailwind CSS 3.4.17                |
| Database         | PostgreSQL 18 (lokal)              |
| DB Client        | postgres (npm package)             |
| Validasi         | Zod                                |
| Autentikasi      | NextAuth.js v5 (beta.25)           |
| Ikon             | Heroicons (@heroicons/react)       |

---

## Struktur Proyek

```
nextjs-dashboard/
├── app/
│   ├── layout.tsx                        # Root layout (font global, CSS)
│   ├── page.tsx                          # Halaman beranda publik
│   ├── dashboard/
│   │   ├── layout.tsx                    # Dashboard layout (SideNav)
│   │   ├── (overview)/
│   │   │   ├── page.tsx                  # Halaman utama dashboard
│   │   │   └── loading.tsx               # Loading skeleton
│   │   ├── invoices/
│   │   │   ├── page.tsx                  # Daftar invoice (search + pagination)
│   │   │   ├── create/page.tsx           # Form buat invoice baru
│   │   │   └── [id]/edit/page.tsx        # Form edit invoice
│   │   └── customers/
│   │       └── page.tsx                  # Halaman customers
│   ├── lib/
│   │   ├── data.ts                       # Fungsi query database
│   │   ├── actions.ts                    # Server Actions (CRUD invoice)
│   │   ├── definitions.ts               # Type definitions (TypeScript)
│   │   ├── placeholder-data.ts           # Data placeholder untuk seeding
│   │   └── utils.ts                      # Utility functions
│   ├── ui/
│   │   ├── global.css                    # Global stylesheet (Tailwind)
│   │   ├── fonts.ts                      # Konfigurasi font (Inter, Lusitana)
│   │   ├── home.module.css               # CSS Module untuk halaman beranda
│   │   ├── search.tsx                    # Komponen pencarian
│   │   ├── skeletons.tsx                 # Komponen loading skeleton
│   │   ├── dashboard/
│   │   │   ├── sidenav.tsx               # Side navigation
│   │   │   ├── nav-links.tsx             # Link navigasi dengan active state
│   │   │   ├── cards.tsx                 # Kartu statistik
│   │   │   ├── revenue-chart.tsx         # Grafik pendapatan
│   │   │   └── latest-invoices.tsx       # Daftar invoice terbaru
│   │   └── invoices/
│   │       ├── table.tsx                 # Tabel invoice
│   │       ├── pagination.tsx            # Komponen pagination
│   │       ├── buttons.tsx               # Tombol CRUD
│   │       ├── breadcrumbs.tsx           # Breadcrumb navigasi
│   │       ├── create-form.tsx           # Form pembuatan invoice
│   │       ├── edit-form.tsx             # Form pengeditan invoice
│   │       └── status.tsx                # Badge status invoice
│   ├── seed/
│   │   └── route.ts                      # API route untuk seeding database
│   └── query/
│       └── route.ts                      # API route untuk test koneksi DB
├── public/
│   ├── hero-desktop.png                  # Hero image desktop
│   ├── hero-mobile.png                   # Hero image mobile
│   └── customers/                        # Avatar pelanggan
├── .env                                  # Environment variables (tidak di-commit)
├── .gitignore
├── tailwind.config.ts
├── postcss.config.js
├── tsconfig.json
├── next.config.ts
└── package.json
```

---

## Chapter 1 — Inisialisasi Proyek

### Tujuan
Membuat proyek Next.js menggunakan starter template resmi dari Vercel.

### Langkah yang Dilakukan

1. Instalasi pnpm secara global sebagai package manager:
   ```bash
   npm install -g pnpm
   ```

2. Pembuatan proyek menggunakan starter template:
   ```bash
   npx create-next-app@latest nextjs-dashboard \
     --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example" \
     --use-pnpm
   ```

3. Instalasi seluruh dependensi:
   ```bash
   pnpm i
   ```

4. Menjalankan development server:
   ```bash
   pnpm dev
   ```

### Hasil
Development server berjalan pada `http://localhost:3000` dengan halaman beranda awal yang belum memiliki styling.

---

## Chapter 2 — CSS Styling

### Tujuan
Menerapkan styling pada aplikasi menggunakan tiga pendekatan: Global CSS, Tailwind CSS, dan CSS Modules.

### Implementasi

**1. Global CSS**

File `app/ui/global.css` berisi Tailwind directives yang menjadi dasar seluruh styling aplikasi:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

File ini diimpor pada root layout (`app/layout.tsx`) sehingga berlaku di seluruh halaman.

**2. Tailwind CSS**

Seluruh komponen menggunakan Tailwind utility classes secara langsung pada JSX. Contoh pada halaman beranda:
```tsx
<main className="flex min-h-screen flex-col p-6">
  <div className="flex h-20 shrink-0 items-end rounded-lg bg-blue-500 p-4 md:h-52">
```

**3. CSS Modules**

File `app/ui/home.module.css` mendefinisikan class `.shape` yang membentuk segitiga menggunakan CSS border trick:
```css
.shape {
  height: 0;
  width: 0;
  border-bottom: 30px solid black;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}
```

Class ini diimpor dan digunakan di `app/page.tsx` melalui:
```tsx
import styles from '@/app/ui/home.module.css';
// ...
<div className={styles.shape} />
```

**4. Conditional Styling dengan clsx**

Library `clsx` digunakan pada komponen seperti `app/ui/invoices/status.tsx` untuk menerapkan class secara kondisional berdasarkan status invoice (pending/paid).

---

## Chapter 3 — Optimasi Font dan Image

### Tujuan
Mengoptimasi performa aplikasi melalui penggunaan `next/font` untuk font dan `next/image` untuk gambar.

### Implementasi

**1. Konfigurasi Font (`app/ui/fonts.ts`)**

Dua Google Font dikonfigurasi:
```typescript
import { Inter, Lusitana } from 'next/font/google';

export const inter = Inter({ subsets: ['latin'] });
export const lusitana = Lusitana({
  weight: ['400', '700'],
  subsets: ['latin'],
});
```

- **Inter** — diterapkan sebagai font utama pada `<body>` melalui root layout dengan class `antialiased` untuk rendering yang halus.
- **Lusitana** — diterapkan pada elemen heading di halaman beranda dan komponen `AcmeLogo`.

**2. Penerapan Font pada Root Layout (`app/layout.tsx`)**

```tsx
<body className={`${inter.className} antialiased`}>{children}</body>
```

**3. Optimasi Gambar (`app/page.tsx`)**

Komponen `next/image` digunakan untuk hero image dengan konfigurasi responsif:

```tsx
<Image
  src="/hero-desktop.png"
  width={1000}
  height={760}
  className="hidden md:block"
  alt="Screenshots of the dashboard project showing desktop version"
/>
<Image
  src="/hero-mobile.png"
  width={560}
  height={620}
  className="block md:hidden"
  alt="Screenshots of the dashboard project showing mobile version"
/>
```

Keuntungan yang diperoleh:
- Pencegahan Cumulative Layout Shift (CLS) melalui deklarasi dimensi eksplisit
- Lazy loading secara default (gambar hanya dimuat saat memasuki viewport)
- Penyajian format modern (WebP/AVIF) jika browser mendukung
- Tampilan responsif menggunakan class `hidden`/`block` pada breakpoint `md`

---

## Chapter 4 — Layouts dan Pages

### Tujuan
Membuat sistem routing berbasis file-system dengan nested layouts untuk halaman dashboard.

### Implementasi

**1. Pembuatan Halaman Dashboard**

Tiga halaman dibuat sesuai konvensi file-system routing Next.js:

| Route                     | File                                  |
|---------------------------|---------------------------------------|
| `/dashboard`              | `app/dashboard/(overview)/page.tsx`   |
| `/dashboard/invoices`     | `app/dashboard/invoices/page.tsx`     |
| `/dashboard/customers`    | `app/dashboard/customers/page.tsx`    |

Route group `(overview)` digunakan untuk mengelompokkan halaman utama dashboard tanpa memengaruhi struktur URL.

**2. Dashboard Layout (`app/dashboard/layout.tsx`)**

Layout bersama yang berisi komponen `SideNav` diterapkan pada seluruh halaman dashboard:

```tsx
import SideNav from '@/app/ui/dashboard/sidenav';

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen flex-col md:flex-row md:overflow-hidden">
      <div className="w-full flex-none md:w-64">
        <SideNav />
      </div>
      <div className="flex-grow p-6 md:overflow-y-auto md:p-12">{children}</div>
    </div>
  );
}
```

Desain responsif diterapkan: layout kolom pada mobile, layout baris pada desktop dengan sidebar selebar 64 unit (`md:w-64`).

**3. Root Layout (`app/layout.tsx`)**

Root layout berfungsi sebagai wrapper global yang menerapkan font Inter dan global CSS ke seluruh aplikasi. Semua nested layout dan page secara otomatis ter-render di dalam root layout ini.

**4. Partial Rendering**

Dengan arsitektur layout Next.js, navigasi antar halaman hanya me-render ulang konten page, sedangkan komponen layout (termasuk SideNav) tetap persisten tanpa re-render.

---

## Chapter 5 — Navigasi Antar Halaman

### Tujuan
Menggantikan tag `<a>` dengan komponen `<Link>` untuk client-side navigation, serta menampilkan indikator halaman aktif.

### Implementasi

**1. Komponen Link (`app/ui/dashboard/nav-links.tsx`)**

Tag `<a>` pada navigasi diganti dengan `<Link>` dari `next/link` untuk mendapatkan:
- Client-side navigation tanpa full page refresh
- Automatic code-splitting per route segment
- Prefetching otomatis saat link muncul di viewport

**2. Active Link dengan usePathname**

File `nav-links.tsx` ditandai sebagai Client Component (`'use client'`) untuk menggunakan hook `usePathname()`:

```tsx
'use client';

import Link from 'next/link';
import { usePathname } from 'next/navigation';
import clsx from 'clsx';

export default function NavLinks() {
  const pathname = usePathname();

  return (
    <>
      {links.map((link) => {
        const LinkIcon = link.icon;
        return (
          <Link
            key={link.name}
            href={link.href}
            className={clsx(
              'flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3',
              {
                'bg-sky-100 text-blue-600': pathname === link.href,
              },
            )}
          >
            <LinkIcon className="w-6" />
            <p className="hidden md:block">{link.name}</p>
          </Link>
        );
      })}
    </>
  );
}
```

Library `clsx` digunakan untuk menerapkan class `bg-sky-100 text-blue-600` secara kondisional ketika `pathname` cocok dengan `link.href`, memberikan indikasi visual halaman yang sedang aktif.

---

## Chapter 6 — Setup Database

### Tujuan
Menyiapkan database PostgreSQL, menghubungkannya dengan aplikasi, melakukan seeding data awal, dan memverifikasi koneksi.

### Implementasi

**1. Konfigurasi Database**

PostgreSQL 18 diinstal dan dikonfigurasi secara lokal. Database dan user dibuat sebagai berikut:

```sql
CREATE USER h3llo WITH PASSWORD 'password123';
CREATE DATABASE nextjs_dashboard OWNER h3llo;
\c nextjs_dashboard
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
```

**2. Environment Variables (`.env`)**

File `.env` dibuat dengan connection string ke database lokal:

```
POSTGRES_URL=postgresql://h3llo:password123@localhost:5432/nextjs_dashboard
AUTH_SECRET=supersecretauthkey1234567890abcdef
AUTH_URL=http://localhost:3000/api/auth
```

File `.env` telah ditambahkan ke `.gitignore` untuk mencegah kebocoran kredensial.

**3. Koneksi Database (`app/lib/data.ts`)**

Koneksi menggunakan library `postgres` tanpa opsi SSL (tidak diperlukan untuk koneksi lokal):

```typescript
import postgres from 'postgres';
const sql = postgres(process.env.POSTGRES_URL!);
```

**4. Database Seeding (`app/seed/route.ts`)**

API route `/seed` menjalankan proses seeding yang mencakup:

| Tabel       | Deskripsi                                               |
|-------------|---------------------------------------------------------|
| `users`     | Data pengguna dengan password ter-hash (bcrypt)         |
| `customers` | Data pelanggan (nama, email, avatar)                    |
| `invoices`  | Data invoice (amount, status, date, customer reference) |
| `revenue`   | Data pendapatan bulanan untuk grafik                    |

Setiap tabel dibuat dengan `CREATE TABLE IF NOT EXISTS` dan data diisi menggunakan `ON CONFLICT DO NOTHING` untuk idempotency.

Seeding dieksekusi melalui browser pada `http://localhost:3000/seed` dan menghasilkan respons:
```json
{ "message": "Database seeded successfully" }
```

**5. Verifikasi Koneksi (`app/query/route.ts`)**

API route `/query` diaktifkan (di-uncomment) untuk memverifikasi koneksi database:

```typescript
import postgres from 'postgres';

const sql = postgres(process.env.POSTGRES_URL!);

async function listInvoices() {
  const data = await sql`
    SELECT invoices.amount, customers.name
    FROM invoices
    JOIN customers ON invoices.customer_id = customers.id
    WHERE invoices.amount = 666;
  `;
  return data;
}

export async function GET() {
  try {
    return Response.json(await listInvoices());
  } catch (error) {
    return Response.json({ error }, { status: 500 });
  }
}
```

Hasil verifikasi pada `http://localhost:3000/query`:
```json
[{"amount": 666, "name": "Evil Rabbit"}]
```

Koneksi database berfungsi dengan benar dan data berhasil di-query.

---

## Cara Menjalankan

### Prasyarat

- Node.js versi 18.18.0 atau lebih baru
- pnpm (diinstal secara global)
- PostgreSQL (terinstal dan berjalan)

### Langkah-langkah

1. Clone repository dan masuk ke direktori proyek:
   ```bash
   cd nextjs-dashboard
   ```

2. Instal dependensi:
   ```bash
   pnpm install
   ```

3. Salin file environment dan sesuaikan kredensial database:
   ```bash
   cp .env.example .env
   ```

4. Pastikan PostgreSQL berjalan, lalu buat database:
   ```sql
   CREATE DATABASE nextjs_dashboard;
   ```

5. Jalankan development server:
   ```bash
   pnpm dev
   ```

6. Lakukan seeding database dengan mengakses:
   ```
   http://localhost:3000/seed
   ```

7. Verifikasi koneksi database:
   ```
   http://localhost:3000/query
   ```

8. Akses aplikasi:
   - Halaman beranda: `http://localhost:3000`
   - Dashboard: `http://localhost:3000/dashboard`
   - Invoice: `http://localhost:3000/dashboard/invoices`
   - Customers: `http://localhost:3000/dashboard/customers`

---

## Referensi

- [Next.js Official Learn Course](https://nextjs.org/learn/dashboard-app)
- Materi asli oleh Next.js Team (Vercel)
- Diadaptasi oleh Laboratorium Informatika FT-UNISMUH
