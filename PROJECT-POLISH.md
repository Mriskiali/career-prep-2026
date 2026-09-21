# Project Polish Guide

Panduan merapikan repo GitHub agar terlihat profesional dan recruiter-friendly.

---

## Checklist Per Repo

### README.md (Wajib Ada di Setiap Repo)
- [ ] **Judul** yang jelas dan deskriptif
- [ ] **Badge** (tech stack, license, build status)
- [ ] **Screenshot / GIF Demo** (paling penting!)
- [ ] **Deskripsi** 2-3 kalimat yang menjelaskan masalah dan solusi
- [ ] **Fitur** — bullet list 3-5 fitur utama
- [ ] **Tech Stack** — tabel atau badge
- [ ] **Cara Install & Jalankan** — copy-paste command
- [ ] **Struktur Folder** — tree diagram
- [ ] **Link Demo / Live** (kalau ada)
- [ ] **Author & Kontak**

### Code Quality
- [ ] `.gitignore` yang lengkap (node_modules, .env, __pycache__, .DS_Store)
- [ ] Tidak ada secret / API key / password di repo (cek history!)
- [ ] Consistent formatting (Prettier, Black, ESLint)
- [ ] Meaningful commit messages (gunakan conventional commits)
- [ ] Tidak ada file "test", "demo", "backup" yang tidak relevan

### GitHub Features
- [ ] **Topics** (tags) di repo — tambah 3-5: `react-native`, `machine-learning`, `computer-vision`, `flask`, `expo`
- [ ] **About** section di sidebar repo — isi deskripsi singkat
- [ ] **Releases** (kalau ada versi stabil)
- [ ] **Wiki** (opsional, untuk dokumentasi detail)

---

## Template README.md (Mobile Project)

```markdown
# Motion-fits — Fitness Tracker App

[![React Native](https://img.shields.io/badge/React_Native-0.74-61DAFB?logo=react)](https://reactnative.dev)
[![Expo](https://img.shields.io/badge/Expo-SDK_50-000020?logo=expo)](https://expo.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-3178C6?logo=typescript)](https://typescriptlang.org)

> Fitness tracker cross-platform dengan navigasi file-based menggunakan Expo Router.

## 📱 Screenshot

| Home Screen | Workout Detail | Profile |
|-------------|----------------|---------|
| ![home](screenshots/home.png) | ![detail](screenshots/detail.png) | ![profile](screenshots/profile.png) |

## ✅ Fitur

- [x] Navigasi tab & stack dengan Expo Router
- [x] Tracking aktivitas harian (steps, calories, duration)
- [x] Riwayat latihan dengan grafik ringkasan
- [x] UI responsif untuk iOS & Android
- [x] Offline-first dengan local storage

## 🛠 Tech Stack

| Layer | Teknologi |
|-------|-----------|
| Framework | React Native + Expo |
| Navigation | Expo Router (file-based) |
| State | Zustand |
| Styling | NativeWind / StyleSheet |
| Storage | AsyncStorage |

## 🚀 Cara Jalankan

```bash
# Clone repo
git clone https://github.com/Mriskiali/motion-fits.git
cd motion-fits

# Install dependencies
npm install

# Jalankan development server
npx expo start

# Scan QR code dengan Expo Go (iOS/Android)
```

## 📁 Struktur Folder

```
motion-fits/
├── app/                    # Expo Router screens
│   ├── (tabs)/
│   │   ├── _layout.tsx   # Tab navigator
│   │   ├── index.tsx     # Home tab
│   │   └── profile.tsx   # Profile tab
│   ├── workout/
│   │   └── [id].tsx      # Workout detail (dynamic)
│   └── _layout.tsx       # Root stack
├── components/             # Reusable components
├── hooks/                  # Custom hooks
├── stores/                 # Zustand stores
├── utils/                  # Helper functions
├── constants/              # Colors, config, etc.
└── assets/                 # Images, fonts
```

## 👤 Author

**Mu'afa Riski Ali** — [GitHub](https://github.com/Mriskiali) | [LinkedIn](https://linkedin.com/in/muafa-riski-ali-3114b536b)
```

---

## Template README.md (ML Project)

```markdown
# Brain Tumor MRI Classification

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python)](https://python.org)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.15-FF6F00?logo=tensorflow)](https://tensorflow.org)
[![Keras](https://img.shields.io/badge/Keras-2.15-D00000?logo=keras)](https://keras.io)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> Klasifikasi tumor otak pada citra MRI menggunakan arsitektur hybrid Autoencoder + CNN.

## 📊 Hasil

| Metrik | Nilai |
|--------|-------|
| Akurasi Uji | **95.0%** |
| Precision (macro) | 0.95 |
| Recall (macro) | 0.95 |
| F1-Score (macro) | 0.95 |
| MSE (Autoencoder) | 0.00135 |
| SSIM (Autoencoder) | 0.93 |

## 📸 Confusion Matrix

![confusion matrix](assets/confusion_matrix.png)

## ✅ Fitur

- [x] Preprocessing otomatis (grayscale, CLAHE, resize, normalisasi)
- [x] Arsitektur hybrid: Autoencoder (anomaly detection) + CNN (klasifikasi)
- [x] Training dengan early stopping & learning rate scheduling
- [x] Evaluasi lengkap: confusion matrix, classification report, ROC-AUC
- [x] Deploy sebagai web service (Flask API + Streamlit demo)
- [x] Attention heatmap untuk interpretabilitas model

## 🛠 Tech Stack

| Layer | Teknologi |
|-------|-----------|
| ML Framework | TensorFlow, Keras |
| Preprocessing | OpenCV, NumPy, PIL |
| Visualization | Matplotlib, Seaborn |
| API | Flask, Gunicorn |
| Demo | Streamlit |
| Deployment | Railway, Streamlit Cloud |

## 🚀 Cara Pakai

### Training
```bash
# Install dependencies
pip install -r requirements.txt

# Jalankan training
python train.py --epochs 30 --batch_size 32 --data_dir data/
```

### Inference (API)
```bash
# Jalankan Flask API
python app.py

# Kirim request
curl -X POST -F "file=@mri_scan.jpg" http://localhost:5000/predict
```

### Streamlit Demo
```bash
streamlit run demo.py
```

## 📁 Struktur Folder

```
brain-tumor-mri/
├── data/                     # Dataset (tidak di-commit)
│   ├── training/
│   ├── testing/
│   └── validation/
├── notebooks/                # Eksperimen & EDA
│   └── 01_eda.ipynb
├── src/
│   ├── model.py              # Arsitektur model
│   ├── preprocess.py         # Pipeline preprocessing
│   ├── train.py              # Script training
│   └── evaluate.py           # Evaluasi & visualisasi
├── app.py                    # Flask API
├── demo.py                   # Streamlit demo
├── requirements.txt
├── README.md
└── LICENSE
```

## 📖 Dataset

- **Sumber:** [Brain Tumor MRI Dataset (Kaggle)](https://www.kaggle.com/datasets/sartajbhuvaji/brain-tumor-classification-mri)
- **Jumlah:** 7.022 citra MRI
- **Kelas:** Glioma, Meningioma, Pituitary, No Tumor
- **Preprocessing:** Grayscale → CLAHE → Resize 128×128 → Normalisasi

## 🌐 Demo & Aset Publik

- **Streamlit Demo:** [brain-tumor-mri-classification-web.streamlit.app](https://brain-tumor-mri-classification-web.streamlit.app)
- **Kaggle Notebook:** [kaggle.com/code/riskiali/skripsi-brain-tumor-mri-classification](https://kaggle.com/code/riskiali/skripsi-brain-tumor-mri-classification)
- **Flask API:** [api-url-di-railway.com](https://api-url-di-railway.com)

## 👤 Author

**Mu'afa Riski Ali** — [GitHub](https://github.com/Mriskiali) | [LinkedIn](https://linkedin.com/in/muafa-riski-ali-3114b536b) | [Kaggle](https://kaggle.com/code/riskiali)
```

---

## Quick Wins (30 Menit Per Repo)

1. **Tambah screenshot/GIF** — paling penting, recruiter males baca teks panjang
2. **Tambah badge** — shields.io, pilih yang relevan
3. **Rapikan struktur folder** — hapus file sampah, pisahkan src/notebooks/data
4. **Tambah `.gitignore`** — jangan sampai node_modules atau model .h5 masuk repo
5. **Tambah `requirements.txt` / `package.json`** — pastikan lengkap dan bisa di-install langsung
6. **Tambah `LICENSE`** — MIT paling aman untuk portfolio
7. **Pin repo terbaik** — di profil GitHub, pin 3-4 repo unggulan

## Tools Bantu

- **Shields.io** — bikin badge: https://shields.io
- **GIF Recorder** — record demo app: ScreenToGif (Windows), Kap (Mac), Peek (Linux)
- **Tree Generator** — `tree -L 3 -I 'node_modules|__pycache__|*.pyc'`
- **README.so** — editor visual README: https://readme.so
