# Portfolio & GitHub Profile Setup

## 1. GitHub Profile README (Repo: Mriskiali/Mriskiali)

Copy ini ke `README.md` di repo `Mriskiali` (repo khusus yang jadi halaman depan profil GitHub lu).

```markdown
<h1 align="center">Hi, I'm Mu'afa Riski Ali 👋</h1>

<p align="center">
  <strong>Machine Learning & Mobile Development</strong> | Fresh Grad Informatika (IPK 3.76)
</p>

<p align="center">
  <a href="https://linkedin.com/in/muafa-riski-ali-3114b536b"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
  <a href="mailto:muafariskiali1805@gmail.com"><img src="https://img.shields.io/badge/Email-EA4335?style=flat&logo=gmail&logoColor=white" alt="Email"></a>
</p>

---

### 🧠 What I Do

- **Computer Vision** — Brain tumor MRI classification (Hybrid Autoencoder + CNN, **95% accuracy**)
- **Mobile Dev** — React Native / Expo, file-based navigation, cross-platform apps
- **Fullstack** — TypeScript, Flask, Turso/LibSQL, Vercel deployment

### 🛠 Tech Stack

| ML / Data | Web / Backend | Mobile |
|-----------|---------------|--------|
| TensorFlow | TypeScript | React Native |
| Keras | Node.js / Flask | Expo Router |
| OpenCV | HTML/CSS | Zustand |
| NumPy/Pandas | REST API | PWA |

### 📌 Featured Projects

| Project | Stack | Description |
|---------|-------|-------------|
| [🧠 Brain Tumor MRI](https://github.com/Mriskiali/brain-tumor-mri-classification) | TF/Keras, Flask | Hybrid Autoencoder+CNN, 7K images, **95% acc**, deployed web service |
| [🎮 Bloxboxd](https://github.com/Mriskiali/bloxboxd) | TypeScript, Turso, Vercel | Social logging platform for Roblox experiences |
| [📱 Motion-fits](https://github.com/Mriskiali/motion-fits) | RN, Expo, Expo Router | Fitness tracker with file-based navigation |

### 📊 Stats

<p align="center">
  <img height="160" src="https://github-readme-stats.vercel.app/api?username=Mriskiali&show_icons=true&theme=default&hide_border=true" />
  <img height="160" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Mriskiali&layout=compact&theme=default&hide_border=true" />
</p>
```

**Cara pasang:**
1. Buat repo baru bernama `Mriskiali` (harus sama dengan username).
2. Di repo itu, buat file `README.md` dengan isi di atas.
3. File ini otomatis muncul di halaman depan profil GitHub lu.

---

## 2. Template README untuk Setiap Repo

### Template untuk Proyek ML

```markdown
# [Nama Proyek]

[![Python](https://img.shields.io/badge/Python-3.x-blue)](https://python.org)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-FF6F00)](https://tensorflow.org)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

## Deskripsi
[Jelaskan 2-3 kalimat apa proyek ini dan kenapa penting]

## Arsitektur
[Gambar arsitektur atau penjelasan singkat]

## Dataset
- Sumber: [link]
- Jumlah data: X
- Kelas: [daftar]

## Hasil
| Metrik | Nilai |
|--------|-------|
| Akurasi | 95% |
| Precision (macro) | 0.95 |
| Recall (macro) | 0.95 |
| F1-Score (macro) | 0.95 |

## Cara Pakai
\`\`\`bash
pip install -r requirements.txt
python train.py
\`\`\`

## Demo
- Streamlit: [link]
- Kaggle Notebook: [link]

## Struktur
\`\`\`
project/
├── notebooks/          # Eksperimen
├── src/
│   ├── model.py        # Arsitektur model
│   ├── preprocess.py   # Preprocessing
│   └── utils.py        # Helper functions
├── app.py              # Flask API
├── requirements.txt
└── README.md
\`\`\`
```

### Template untuk Proyek Web/Mobile

```markdown
# [Nama Proyek]

[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue)](https://typescriptlang.org)
[![React Native](https://img.shields.io/badge/React_Native-0.74-61DAFB)](https://reactnative.dev)

## Deskripsi
[2-3 kalimat]

## Fitur
- [ ] Fitur 1
- [ ] Fitur 2
- [ ] Fitur 3

## Tech Stack
- Frontend: [framework]
- Backend: [API]
- Database: [DB]
- Deploy: [platform]

## Cara Jalankan
\`\`\`bash
npm install
# atau
npx expo start
\`\`\`

## Screenshots
[Tambah screenshot dari device/simulator]

## Roadmap
- [ ] Fitur A
- [ ] Fitur B
```

---

## 3. Tips Profil GitHub Profesional

### Wajib Ada:
- ✅ Profile README (di atas)
- ✅ Foto profil (bukan default avatar)
- ✅ Bio singkat: "ML & Mobile Dev | Gunadarma '26"
- ✅ Pin 3-4 repo terbaik (klik ⭐ di repo → Pin to profile)
- ✅ Repo punya README dengan screenshot/demo link

### Jangan:
- ❌ Repo kosong tanpa README
- ❌ Commit message "update", "fix", "wip"
- ❌ File sensitif (`.env`, credentials) pernah masuk git history
- ❌ Repo "test", "demo", "learning" yang tidak relevan — archive saja

### Commit Message Convention:
```
feat: tambah fitur login OAuth
fix: bug navigasi tab di Android
docs: update README deployment
refactor: pindah state management ke Zustand
test: tambah unit test untuk API client
chore: upgrade dependencies
```
