# Panduan Lengkap Setup Notion Job Hunting Tracker

Panduan langkah demi langkah (step-by-step) untuk membuat tracker lamaran kerja yang interaktif, visual, dan otomatis di Notion.

---

## 🚀 LANGKAH 1: Buat Halaman Baru di Notion

1. Buka aplikasi Notion atau kunjungi [notion.so](https://notion.so).
2. Di sidebar kiri, klik tombol **"+ Add a page"**.
3. Beri judul halaman: `🎯 Job Hunting Tracker 2026`.
4. Tambahkan icon (misal: 🎯 atau 💼) dan cover image sesuai selera agar rapi.

---

## 📊 LANGKAH 2: Buat Database Tabel

1. Di dalam body halaman yang kosong, ketik `/table` lalu pilih **"Table view"**.
2. Di menu pop-up sumber data, klik **"+ New database"**.
3. Beri nama database: `Daftar Lamaran`.

---

## 🛠 LANGKAH 3: Konfigurasi Kolom (Properties)

Ubah dan tambahkan kolom dengan klik header kolom atau tombol **"+"** di ujung kanan tabel.

| Nama Kolom | Property Type | Pilihan / Opsi Warna |
|---|---|---|
| **Perusahaan** | `Title` (Bawaan) | - |
| **Posisi** | `Text` | - |
| **Status** | `Select` | 🟡 `Applied / Waiting`<br>🟢 `Interviewing`<br>🔴 `Rejected`<br>🎉 `Offer` |
| **Platform** | `Select` | `Dealls`, `LinkedIn`, `Glints`, `Jobstreet`, `Direct / Referral` |
| **Tanggal Apply**| `Date` | - |
| **Follow-up** | `Date` | *(Set tanggal H+7 setelah apply)* |
| **CV Versi** | `Select` | `Mobile (React Native)`, `Machine Learning` |
| **Link Loker** | `URL` | - |
| **Kontak HR/Lead** | `Text` | Nama atau profil LinkedIn recruiter |

---

## 🗂 LANGKAH 4: Buat Kanban Board View (Papan Geser)

Tampilan ini memudahkan kamu memindahkan status lamaran hanya dengan drag-and-drop.

1. Di atas tabel, klik tanda **"+"** di samping tulisan tab `Table`.
2. Pilih tampilan **"Board"**.
3. Di panel kanan (Layout Settings):
   - **Group by:** Pilih `Status`.
4. Sekarang kamu punya papan kolom: `Applied / Waiting` | `Interviewing` | `Rejected` | `Offer`.

---

## 📝 LANGKAH 5: Buat Template Kartu Otomatis (Page Template)

Agar setiap kali menambah lamaran baru kamu punya checklist otomatis:

1. Di pojok kanan atas database, klik tanda panah ke bawah di samping tombol biru **"New"**, lalu pilih **"+ New template"**.
2. Beri judul: `Perusahaan Baru`.
3. Di dalam isi halaman template, ketik checklist berikut:

```markdown
### 📋 Tahapan Lamaran
- [ ] Kirim CV (sesuaikan versi Mobile / ML)
- [ ] Kirim direct message ke Recruiter di LinkedIn
- [ ] Jadwalkan follow-up (H+7)
- [ ] Latihan interview teknis (buka interview-prep/)

### 📌 Catatan Posisi & Gaji
- Gaji yang ditawarkan / ekspektasi:
- Lokasi kerja (WFO / WFH / Hybrid):
- Tech stack utama yang diminta:

### 💬 Review & Catatan Interview
- Pertanyaan teknis yang muncul:
- Hal yang perlu diperbaiki:
```

4. Kembali ke database. Klik panah di samping tombol **"New"** lagi → klik icon titik tiga (`...`) di samping template `Perusahaan Baru` → pilih **"Set as default"** (pilih *For all views*).

---

## 📱 LANGKAH 6: Install di HP

1. Download aplikasi **Notion** di Google Play Store atau App Store.
2. Login dengan akun yang sama.
3. Buka halaman `Job Hunting Tracker 2026` dan tambahkan ke **Favorites** (klik icon bintang) agar mudah diakses saat ada email masuk.
