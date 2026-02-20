# Next.js Dashboard — 105841115422

> **Mata Kuliah:** Pemrograman Web Lanjut  
> **NIM:** 105841115422  
> **Universitas:** Universitas Muhammadiyah Makassar  
> **Repository:** [nextjs-dashboard-105841115422](https://github.com/Nur-Hidayat-FTI22E/nextjs-dashboard-105841115422)

---

## 📋 Deskripsi Project

Project ini adalah implementasi **Next.js Dashboard Application** berdasarkan panduan dari [Next.js Learn Course](https://nextjs.org/learn). Dashboard ini menampilkan data keuangan fiktif perusahaan "Acme" yang mencakup halaman overview, invoices, dan customers.

---

## 🚀 Cara Menjalankan

### Prasyarat
- Node.js versi 18+
- npm

### Instalasi & Menjalankan

```bash
# Install dependencies
npm install

# Jalankan development server
npm run dev
```

Buka browser dan akses: **http://localhost:3000**

---

## 📖 Ringkasan Pengerjaan

### ✅ Chapter 1 — Memulai Project

Menginisialisasi project Next.js menggunakan starter template dari kursus resmi Next.js. Project menggunakan:
- **Next.js 15** dengan App Router
- **TypeScript** untuk type safety
- **Turbopack** sebagai bundler (via `next dev --turbopack`)

**Struktur folder utama:**
```
app/
├── layout.tsx          # Root Layout
├── page.tsx            # Homepage (/)
├── dashboard/          # Route /dashboard
│   ├── layout.tsx      # Dashboard Layout + SideNav
│   ├── page.tsx        # Halaman utama dashboard
│   ├── invoices/       # Route /dashboard/invoices
│   └── customers/      # Route /dashboard/customers
├── login/              # Route /login
│   └── page.tsx
└── ui/                 # Komponen UI
    ├── fonts.ts
    ├── global.css
    ├── home.module.css
    └── dashboard/
```

---

### ✅ Chapter 2 — CSS Styling

Mengimplementasikan tiga pendekatan styling yang didemokan dalam kursus:

#### 1. Global CSS
File `app/ui/global.css` diimport di root layout (`app/layout.tsx`) sehingga berlaku untuk seluruh aplikasi:
```tsx
// app/layout.tsx
import '@/app/ui/global.css';
```

#### 2. Tailwind CSS
Tailwind utility classes digunakan langsung di JSX:
```tsx
<main className="flex min-h-screen flex-col p-6">
  <div className="flex h-20 shrink-0 items-end rounded-lg bg-blue-500 p-4 md:h-52">
```

#### 3. CSS Modules
File `app/ui/home.module.css` dibuat untuk demonstrasi scoped styling:
```css
/* app/ui/home.module.css */
.shape {
  height: 0;
  width: 0;
  border-bottom: 30px solid black;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}
```
Digunakan di `app/page.tsx`:
```tsx
import styles from '@/app/ui/home.module.css';
// ...
<div className={styles.shape} />
```

#### 4. Library clsx
Library `clsx` digunakan untuk conditional class names (diimplementasikan di Chapter 5).

---

### ✅ Chapter 3 — Optimizing Fonts and Images

#### Fonts
Dua custom Google Font dikonfigurasi di `app/ui/fonts.ts`:

```ts
import { Inter, Lusitana } from 'next/font/google';

export const inter = Inter({ subsets: ['latin'] });

export const lusitana = Lusitana({
  weight: ['400', '700'],
  subsets: ['latin'],
});
```

- **Inter** → diapply ke `<body>` di `app/layout.tsx` dengan class `antialiased`
- **Lusitana** → diapply ke elemen `<p>` di homepage

#### Images
Komponen `<Image>` dari Next.js digunakan untuk optimasi otomatis (lazy loading, WebP conversion, dll):

```tsx
{/* Desktop - hidden di mobile */}
<Image
  src="/hero-desktop.png"
  width={1000}
  height={760}
  className="hidden md:block"
  alt="Screenshots of the dashboard project showing desktop version"
/>

{/* Mobile - hidden di desktop */}
<Image
  src="/hero-mobile.png"
  width={560}
  height={620}
  className="block md:hidden"
  alt="Screenshots of the dashboard project showing mobile version"
/>
```

---

### ✅ Chapter 4 — Creating Layouts and Pages

Memanfaatkan **File-System Routing** Next.js App Router untuk membuat halaman-halaman dashboard:

| Route | File |
|-------|------|
| `/` | `app/page.tsx` |
| `/login` | `app/login/page.tsx` |
| `/dashboard` | `app/dashboard/page.tsx` |
| `/dashboard/invoices` | `app/dashboard/invoices/page.tsx` |
| `/dashboard/customers` | `app/dashboard/customers/page.tsx` |

**Dashboard Layout** dibuat di `app/dashboard/layout.tsx` menggunakan komponen `SideNav` yang di-share ke semua halaman `/dashboard/*`:

```tsx
import SideNav from '@/app/ui/dashboard/sidenav';

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen flex-col md:flex-row md:overflow-hidden">
      <div className="w-full flex-none md:w-64">
        <SideNav />
      </div>
      <div className="flex-grow p-6 md:overflow-y-auto md:p-12">
        {children}
      </div>
    </div>
  );
}
```

---

### ✅ Chapter 5 — Navigating Between Pages

Mengimplementasikan navigasi client-side dengan **active link indicator**:

```tsx
// app/ui/dashboard/nav-links.tsx
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

**Konsep yang diimplementasikan:**
- `'use client'` directive untuk menggunakan React hooks
- `usePathname()` hook untuk membaca URL aktif saat ini
- `clsx` untuk conditional styling (link aktif diberi highlight biru)
- Komponen `<Link>` untuk client-side navigation (tanpa full page reload)

---

## 🛠️ Teknologi yang Digunakan

| Teknologi | Versi | Kegunaan |
|-----------|-------|----------|
| Next.js | 15 (latest) | Framework utama |
| React | latest | UI library |
| TypeScript | 5.7.3 | Type safety |
| Tailwind CSS | 3.4.17 | Utility-first styling |
| next/font | built-in | Optimasi Google Fonts |
| next/image | built-in | Optimasi gambar |
| clsx | ^2.1.1 | Conditional class names |
| @heroicons/react | ^2.2.0 | Icon library |
| next-auth | 5.0.0-beta | Authentication (siap pakai) |

---

## 📁 Struktur File Lengkap

```
nextjs-dashboard/
├── app/
│   ├── layout.tsx              # Root layout (font Inter + global CSS)
│   ├── page.tsx                # Homepage dengan hero image & login button
│   ├── login/
│   │   └── page.tsx            # Halaman login
│   ├── dashboard/
│   │   ├── layout.tsx          # Layout dashboard + SideNav
│   │   ├── page.tsx            # Dashboard overview
│   │   ├── invoices/
│   │   │   └── page.tsx        # Halaman invoices
│   │   └── customers/
│   │       └── page.tsx        # Halaman customers
│   ├── ui/
│   │   ├── fonts.ts            # Konfigurasi Inter & Lusitana
│   │   ├── global.css          # Global styles + Tailwind directives
│   │   ├── home.module.css     # CSS Module (shape demo)
│   │   ├── acme-logo.tsx       # Logo Acme
│   │   ├── login-form.tsx      # Komponen form login
│   │   └── dashboard/
│   │       ├── nav-links.tsx   # Navigasi dengan active state
│   │       ├── sidenav.tsx     # Sidebar navigation
│   │       ├── cards.tsx       # Dashboard cards
│   │       ├── revenue-chart.tsx
│   │       └── latest-invoices.tsx
│   └── lib/
│       ├── data.ts             # Fungsi query database
│       ├── definitions.ts      # TypeScript type definitions
│       ├── placeholder-data.ts # Data placeholder
│       └── utils.ts            # Utility functions
├── public/
│   ├── hero-desktop.png        # Hero image desktop
│   ├── hero-mobile.png         # Hero image mobile
│   └── customers/              # Avatar foto customers
├── package.json
├── tailwind.config.ts
└── tsconfig.json
```

---

## 🔗 Referensi

- [Next.js Learn Course](https://nextjs.org/learn)
- [Next.js Documentation](https://nextjs.org/docs)
- [Repository Panduan](https://github.com/devnolife/nextjs-dashboard)
