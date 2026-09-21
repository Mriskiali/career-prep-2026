# Panduan Konversi CV ke PDF (ATS-Friendly)

CV dalam format Markdown harus dikonversi ke PDF agar bisa di-upload ke portal magang (MagangHub, LinkedIn, dll).

## Opsi 1 — VS Code (Paling mudah)

1. Install ekstensi **"Markdown PDF"** (yzane.markdown-pdf) di VS Code.
2. Buka file `CV_Mobile_Muafa_Riski_Ali.md`.
3. `Ctrl+Shift+P` → ketik `Markdown PDF: Export (pdf)`.
4. PDF muncul di folder yang sama.

> Ganti dulu `---` di bagian bawah (footer) dengan spasi agar tidak jadi page break aneh. Atau hapus baris `---` terakhir.

## Opsi 2 — Pandoc (Kualitas terbaik)

```bash
# Install
sudo apt install pandoc texlive-latex-base texlive-fonts-recommended

# Konversi ke PDF
pandoc cv/CV_Mobile_Muafa_Riski_Ali.md -o cv/CV_Mobile_Muafa_Riski_Ali.pdf \
  --pdf-engine=xelatex -V geometry:margin=2cm

pandoc cv/CV_ML_Muafa_Riski_Ali.md -o cv/CV_ML_Muafa_Riski_Ali.pdf \
  --pdf-engine=xelatex -V geometry:margin=2cm
```

## Opsi 3 — Google Docs (Paling aman buat ATS)

1. Copy isi Markdown ke Google Docs.
2. Rapikan heading (bold untuk section), pastikan **tidak ada tabel kompleks** di versi ATS — ubah tabel ke bullet list kalau perlu.
3. **File → Download → PDF Document (.pdf)**.

## Checklist ATS Sebelum Kirim

- [ ] Nama file: `CV_Muafa_Riski_Ali_Mobile.pdf` / `CV_Muafa_Riski_Ali_ML.pdf` (jangan "cv_final_revisi2.pdf")
- [ ] Tidak ada foto, grafik, atau ikon (ATS lama tidak bisa baca)
- [ ] Gunakan heading standar: EXPERIENCE, EDUCATION, SKILLS
- [ ] Keyword sesuai job desc (lihat catatan per posisi di bawah)
- [ ] File < 2 MB
- [ ] Tiap halaman ada nama + kontak (di header/footer)

## Keyword Mapping per Posisi

### Magang: Mobile Platform Development
Pastikan keyword ini muncul:
`React Native`, `Expo`, `Expo Router`, `TypeScript`, `state management`, `REST API`, `responsive UI`, `file-based routing`, `cross-platform`, `mobile deployment`, `Play Store / App Store`

### Magang: Machine Learning Engineer Intern
Pastikan keyword ini muncul:
`Python`, `TensorFlow`, `Keras`, `CNN`, `Autoencoder`, `computer vision`, `model evaluation`, `precision/recall/F1`, `CRISP-DM`, `data preprocessing`, `model deployment`, `Flask`, `confusion matrix`

## Tips Cover Letter (2 paragraf, < 200 kata)

**Template Mobile:**
> Saya Mu'afa Riski Ali, fresh graduate Informatika Universitas Gunadarma (IPK 3.76) dengan fokus pengembangan aplikasi mobile menggunakan React Native dan Expo. Saya membangun Motion-fits, fitness tracker dengan arsitektur navigasi file-based (Expo Router), serta memiliki pengalaman fullstack dari proyek Bloxboxd yang mengintegrasikan REST API dan database cloud. Saya tertarik pada posisi Magang Mobile Platform Development karena ingin berkontribusi membangun aplikasi mobile yang scalable dan berdampak nyata bagi pengguna.

**Template ML:**
> Saya Mu'afa Riski Ali, fresh graduate Informatika Universitas Gunadarma (IPK 3.76) yang berfokus pada machine learning dan computer vision. Skripsi saya membangun model hybrid Autoencoder + CNN untuk klasifikasi tumor otak MRI dengan akurasi 95%, lengkap dari preprocessing CLAHE hingga deployment web service publik. Saya siap berkontribusi pada posisi Machine Learning Engineer Intern untuk mengubah riset menjadi solusi produksi.
