# Fitur Foto per Resi — Superadmin

## Ringkasan

Fitur ini memungkinkan superadmin untuk **upload, melihat, dan mengelola foto bukti pengiriman** yang terkait dengan setiap nomor resi. Foto dapat berupa bukti serah terima ke kurir, foto paket yang sudah di-pack, atau foto resi fisik.

Fitur ini ditambahkan langsung ke 3 halaman yang sudah ada di role **Superadmin**, tanpa membuat halaman baru:

| Halaman         | Path                                       | Fitur Foto               |
| --------------- | ------------------------------------------ | ------------------------ |
| Resi Checker    | `superadmin/shipping/resi-checker.html`    | Upload + View + Lightbox |
| Tracking Center | `superadmin/shipping/tracking-center.html` | View + Lightbox          |
| Print Label     | `superadmin/shipping/print-label.html`     | View + Lightbox          |

---

## Detail Implementasi per Halaman

### 1. Resi Checker (`resi-checker.html`)

**Halaman utama** untuk fitur foto. Di sini superadmin bisa:

- **Upload foto** per resi melalui:
  - Drag & drop zone
  - File picker (klik untuk memilih)
  - Camera capture (untuk mobile — langsung buka kamera)
- **Melihat thumbnail** foto di kolom "Foto" pada tabel Scanned Resi (Session Detail view)
- **Lightbox viewer** — klik thumbnail untuk melihat foto full-size, navigasi antar foto dengan tombol prev/next atau keyboard arrow
- **Stats foto** di dashboard view:
  - Total Foto: jumlah semua foto yang sudah di-upload
  - Resi Dengan Foto: berapa resi yang sudah ada fotonya
  - Resi Tanpa Foto: berapa resi yang belum ada fotonya

**Komponen baru:**

- **Photo Upload Modal** (`#photoUploadModal`) — modal untuk upload foto dengan drag & drop, camera capture, dan preview grid
- **Photo Lightbox** (`#photoLightbox`) — modal fullscreen untuk view foto dengan navigasi
- **Photo Stats Cards** — 3 kartu statistik foto di dashboard view

**Batasan:**

- Max 3 foto per resi
- Max 5MB per file
- Format: JPG, PNG, WEBP

**Data mock:** `RESI_PHOTOS` object berisi contoh foto untuk 3 resi dari mock session data.

### 2. Tracking Center (`tracking-center.html`)

- **Kolom "Foto"** ditambahkan ke tabel All Shipments, di antara kolom "Last Update" dan "ETA"
- Menampilkan **thumbnail** jika ada foto, atau ikon kosong jika belum ada
- Klik thumbnail → **lightbox viewer** fullscreen
- Badge "+N" jika resi punya lebih dari 1 foto

**Komponen baru:**

- **Photo Lightbox** (`#trackingLightbox`) — lightbox khusus halaman tracking

**Data mock:** `TRACKING_PHOTOS` array berisi contoh foto untuk shipment JNE, J&T, dan Ninja.

### 3. Print Label (`print-label.html`)

- **Kolom "Foto"** ditambahkan ke tabel order, di antara kolom "Tracking No." dan "Label Status"
- Superadmin bisa **melihat foto** sebelum print label — berguna untuk verifikasi paket
- Klik thumbnail → **lightbox viewer** fullscreen

**Komponen baru:**

- **Photo Lightbox** (`#labelPhotoLightbox`) — lightbox khusus halaman print label

**Data mock:** Property `photos` ditambahkan ke beberapa entry di `MOCK_ORDERS` (order TT-001, TT-003, SP-008).

---

## Komponen UI

### Photo Thumbnail (`.photo-thumb`)

```css
width: 32-36px, border-radius: 8px, border: 2px solid #e9ecef
hover: border-color primary, scale 1.1, shadow
```

### Photo Upload Modal

- Drop zone dengan drag & drop support
- Camera capture button (menggunakan `capture="environment"` untuk kamera belakang)
- Preview grid 3 kolom sebelum submit
- Tombol hapus per preview
- Validasi: tipe file, ukuran, jumlah max

### Photo Lightbox

- Overlay bg-black/80 dengan backdrop blur
- Navigasi: prev/next button + keyboard arrow keys + Escape to close
- Counter "1 / 3" di bawah foto
- Click outside to close

---

## Mobile Support

Fitur ini sudah mobile-friendly:

- **Camera capture** menggunakan `<input type="file" accept="image/*" capture="environment">` — langsung membuka kamera belakang di perangkat mobile
- **Drag & drop zone** juga berfungsi sebagai tap-to-select di mobile
- **Lightbox** responsif, menggunakan `max-h-[80vh]` dan `object-contain` agar foto tidak terpotong

---

## Alur Kerja (User Flow)

```
1. Admin mobile scan resi → data masuk ke Resi Checker
2. Superadmin buka Resi Checker → lihat session detail
3. Klik ikon kamera di kolom Actions → modal upload muncul
4. Drag & drop foto ATAU klik "Ambil Foto dari Kamera"
5. Preview foto → klik "Simpan Foto"
6. Thumbnail muncul di kolom Foto
7. Klik thumbnail → lightbox fullscreen
8. Foto juga terlihat di Tracking Center dan Print Label
```

---

## Catatan Teknis

- **Data sementara**: Saat ini foto disimpan di memori browser (mock data). Pada implementasi real, perlu backend API untuk:
  - `POST /api/resi/{resi_number}/photos` — upload foto
  - `GET /api/resi/{resi_number}/photos` — list foto per resi
  - `DELETE /api/resi/{resi_number}/photos/{photo_id}` — hapus foto
- **Object URL**: Foto yang di-upload menggunakan `URL.createObjectURL()` untuk preview. Pada implementasi real, URL akan berasal dari server.
- **Placeholder images**: Menggunakan `placehold.co` untuk mock data, harus diganti dengan URL real di production.
