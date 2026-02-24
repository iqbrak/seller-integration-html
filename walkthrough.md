# Analisis Proyek — Seller Integration HTML

## Ringkasan Proyek

Proyek ini adalah **prototipe front-end statis** untuk platform integrasi seller multi-marketplace. Mengelola order e-commerce dari **Shopee**, **TikTok Shop**, dan **Tokopedia** (ditambah Lazada di tampilan superadmin). Dibuat oleh **Adisatya IT Consultant** (© 2026).

> [!IMPORTANT]
> Ini adalah **prototipe UI saja** — semua data bersifat statis/hardcoded, tidak ada backend API yang terhubung, dan login menggunakan kredensial demo.

---

## Struktur File

| File                                                                                                      | Baris | Ukuran | Fungsi               |
| --------------------------------------------------------------------------------------------------------- | ----: | -----: | -------------------- |
| [login.html](file:///d:/Proyek%20github/seller-integration-html/login.html)                               |   598 |  19 KB | Halaman autentikasi  |
| [admin.html](file:///d:/Proyek%20github/seller-integration-html/admin.html)                               | 1.277 | 109 KB | Dashboard admin      |
| [superadmin/dashboard.html](file:///d:/Proyek%20github/seller-integration-html/superadmin/dashboard.html) | 1.722 | 128 KB | Dashboard superadmin |
| [FEATURE_SUGGESTIONS.md](file:///d:/Proyek%20github/seller-integration-html/FEATURE_SUGGESTIONS.md)       |   116 |   4 KB | Roadmap fitur        |

---

## Teknologi yang Digunakan

| Kategori          | Teknologi                                                               |
| ----------------- | ----------------------------------------------------------------------- |
| **Framework CSS** | TailwindCSS 3.4.17 (CDN)                                                |
| **Tipografi**     | Google Fonts — Plus Jakarta Sans (400–800)                              |
| **Ikon**          | Inline SVG (login & admin), Font Awesome 6.5 (superadmin)               |
| **Grafik**        | SVG buatan manual (tanpa library charting)                              |
| **Modal**         | CSS kustom + vanilla JavaScript                                         |
| **Token Warna**   | Palet `primary` (biru) dan `surface` (abu-abu) via konfigurasi Tailwind |

---

## Detail Setiap Halaman

### 1. Halaman Login (`login.html`)

- **Desain**: Layout kartu di tengah layar dengan background gradien dan animasi saat halaman dimuat
- **Field Input**: Email + Password dengan ikon SVG inline
- **Fitur**:
  - Toggle visibilitas password (tampilkan/sembunyikan)
  - Validasi sisi klien (cek kosong, format email, panjang minimal password)
  - Animasi getar + pesan error per field jika validasi gagal
  - Alert sukses dengan animasi progress bar
  - Checkbox "Remember me" dan link "Forgot password"
- **Kredensial Demo**:
  - `admin@seller-integration.io` / `demo123`
  - `super@seller-integration.io` / `demo123`
- **Fungsi JS**: `togglePassword()`, `handleLogin()`, `showError()`, `showFieldError()`, `clearFieldError()`, `setLoading()`, `dismissAlert()`

---

### 2. Dashboard Admin (`admin.html`)

Badge peran: **ADMIN** (hijau emerald)

**Navigasi Sidebar:**

- Dashboard Overview
- Orders Management (dropdown: Semua, To Ship, Processing, In Transit, Completed, Cancelled)
- Upload Center (CSV/Excel)
- Resi Checker
- Shop Integration (dropdown: Shopee, TikTok Shop, Tokopedia, Tambah Toko Baru)
- Reports & Analytics (dropdown: Harian, Order Analytics, Shipment Metrics, Export)
- Notifikasi (badge: 12)
- Profile & Settings

**Konten Utama:**

| Bagian                     | Deskripsi                                                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Banner Peringatan**      | 47 order menunggu pengiriman, deadline 18:00 WIB                                                                    |
| **6 Quick Actions**        | Upload Orders, Cetak Label, Cek Resi, Sync Toko, Laporan Harian, Order Bermasalah                                   |
| **6 Kartu KPI**            | Orders Hari Ini (247), Siap Dikirim (47), Dalam Pengiriman (156), Selesai (189), Revenue (Rp 75.3M), Dibatalkan (3) |
| **Grafik Tren Order**      | Line chart SVG 14 hari dengan garis Orders + Delivered                                                              |
| **Distribusi Marketplace** | Bar chart — Shopee 50%, TikTok 35%, Tokopedia 15%                                                                   |
| **Status Integrasi Toko**  | Shopee (Online), TikTok (Online), Tokopedia (Syncing) + API rate limit                                              |
| **Breakdown Status Order** | Progress bar: Pending/Diproses/Dikirim/Selesai/Batal                                                                |
| **Performa Kurir**         | JNE 98.1%, SiCepat 97.4%, J&T 96.8%, Anteraja 95.2%                                                                 |
| **Tren Order Bulanan**     | Bar chart Jan–Okt, pertumbuhan YoY +34.2%                                                                           |
| **Aktivitas Terkini**      | Feed event terbaru + notifikasi sistem                                                                              |
| **Modal**                  | Upload orders dan cetak label (dengan animasi)                                                                      |

---

### 3. Dashboard Superadmin (`superadmin/dashboard.html`)

Badge peran: **SUPERADMIN** (biru)

**Menu Tambahan vs Admin:**

- User Management
- Print Shipping Labels
- Tracking Center
- Activity Logs
- Market Integration (Shopee, Tiktok/Tokopedia)
- API Documentation

**Konten Utama:**

| Bagian                       | Deskripsi                                                                                                                                   |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **6 Quick Actions**          | Approve Sellers (12 pending), Retry Failed Sync (7), Refresh API Keys (2 expiring), Generate Report, Add New Seller, System Health          |
| **6 Kartu KPI**              | Total Revenue (Rp 2.84B), Active Sellers (1.247), Total Orders (45.892), API Success Rate (99.7%), Pending Approvals (12), Failed Syncs (7) |
| **Grafik Revenue**           | Line chart bulanan dengan perbandingan target                                                                                               |
| **Traffic Marketplace**      | Bar chart mingguan (Senin–Minggu)                                                                                                           |
| **Status Integrasi**         | 4 platform — Shopee, TikTok, Tokopedia, **Lazada**                                                                                          |
| **Distribusi Volume**        | Persentase order per marketplace                                                                                                            |
| **Monitor Kesehatan Sistem** | CPU (34%), Memory (62%), Disk (45%), Network (28ms)                                                                                         |
| **Tabel Top Seller**         | Daftar seller terbaik + aktivitas terkini + alert sistem                                                                                    |
| **Modal**                    | Approve sellers dan retry sync (dengan animasi)                                                                                             |

---

## Arsitektur Berbasis Peran

```mermaid
graph TD
    L[Halaman Login] --> |admin@...| A[Dashboard Admin]
    L --> |super@...| S[Dashboard Superadmin]

    A --> |Cakupan| A1[Operasi seller tunggal]
    A1 --> A2[Manajemen order]
    A1 --> A3[3 marketplace]
    A1 --> A4[Tracking kurir]

    S --> |Cakupan| S1[Operasi seluruh platform]
    S1 --> S2[Manajemen 1.247 seller]
    S1 --> S3[4 marketplace + Lazada]
    S1 --> S4[Monitoring kesehatan sistem]
    S1 --> S5[Approval user/seller]
```

---

## Status Roadmap Fitur

Dari `FEATURE_SUGGESTIONS.md`, berikut prioritas fitur yang sudah direncanakan:

| Prioritas | Fitur                        | Status                              |
| --------- | ---------------------------- | ----------------------------------- |
| 🔴 Tinggi | Stock Sync & Low Stock Alert | Direncanakan                        |
| 🔴 Tinggi | Finance Reconciliation       | Direncanakan                        |
| 🔴 Tinggi | Mobile API Key Management UI | Direncanakan                        |
| 🟡 Sedang | Analytics & Sales Dashboard  | Sebagian sudah ada (statis)         |
| 🟡 Sedang | Bulk Print Label             | Sebagian sudah ada (modal tersedia) |
| 🟡 Sedang | Notifikasi WhatsApp/Telegram | Direncanakan                        |
| 🟢 Rendah | Unified Inbox (Chat)         | Direncanakan                        |
| 🟢 Rendah | Multi-Store Management       | Direncanakan                        |

Issue GitLab yang diusulkan: **#24 sampai #30**

---

## Kelebihan & Kekurangan

### ✅ Kelebihan

- **Kualitas UI premium** — Desain modern dengan micro-animation, progress bar, ikon gradien
- **Sistem desain konsisten** — Token warna, tipografi Plus Jakarta Sans di semua halaman
- **Pemisahan peran jelas** — Cakupan Admin vs Superadmin berbeda secara signifikan
- **Visualisasi data kaya** — Grafik SVG, bar performa kurir, distribusi marketplace
- **UX detail** — Animasi shake saat login gagal, alert dismissible, loading state pada tombol

### ⚠️ Kekurangan

- **Tidak ada routing** — Semua halaman adalah file HTML mandiri, belum pakai framework SPA
- **Tidak ada backend** — Seluruhnya statis; login disimulasikan dengan `setTimeout`
- **Kode duplikat** — Sidebar, header, dan konfigurasi Tailwind diulang di setiap file
- **Belum responsif mobile** — Sidebar lebar tetap (288px), tidak ada menu hamburger
- **Data hardcoded** — Semua angka, grafik, dan tabel menggunakan nilai statis
- **Bahasa campur** — Teks UI mencampur Bahasa Indonesia dan Inggris secara tidak konsisten
- **Ikon tidak seragam** — Login & admin menggunakan inline SVG, superadmin menggunakan Font Awesome
