# Interview Preparation — Machine Learning Engineer Intern

## Struktur Wawancara Umum
1. **Screening HR** (10-15 min) — perkenalan, motivasi, jadwal magang
2. **Technical** (30-60 min) — ML konsep, evaluasi model, coding
3. **Research/Case** — deep-dive ke skripsi, CRISP-DM, eksperimen
4. **Culture** — teamwork, riset mindset

---

## A. Pertanyaan HR / Behavioral

### 1. "Ceritakan tentang diri kamu"
> "Saya Mu'afa Riski Ali, fresh graduate Informatika Universitas Gunadarma dengan IPK 3.76. Fokus saya di machine learning, khususnya computer vision. Skripsi saya membangun model hybrid Autoencoder + CNN untuk klasifikasi tumor otak MRI dengan akurasi 95% — dari preprocessing sampai deployment sebagai web service publik. Saya tertarik bagaimana riset ML diubah jadi solusi yang beneran dipakai."

### 2. "Kenapa mau magang di posisi MLE?"
> "Karena saya ingin belajar bagaimana ML dijalankan di industri — bukan cuma di notebook. Saya sudah pegang end-to-end pipeline dari riset (CRISP-DM) sampai deployment (Flask, Streamlit), tapi saya tahu ada banyak hal yang tidak diajarkan di skripsi: MLOps, experiment tracking, model monitoring, data pipeline. Saya ingin belajar dari tim yang sudah production-grade."

### 3. "Ceritakan skripsi kamu secara singkat"
> **3 kalimat max:**
> "Saya membangun model hybrid Autoencoder + CNN untuk klasifikasi 4 jenis tumor otak pada citra MRI. Autoencoder nge-reconstruct input dan deteksi anomali berbasis error rekonstruksi (MSE 0.00135, SSIM 0.93), sementara CNN klasifikasi 4 kelas dengan akurasi 95%. Saya deploy model sebagai web service di Railway supaya bisa diakses publik."

### 4. "Apa tantangan terbesar di skripsi?"
> "Keterbatasan GPU — saya cuma punya akses T4 Colab dengan waktu terbatas, jadi saya harus optimasi batch size, learning rate schedule, dan early stopping supaya training selesai dalam jatah. Juga, kelas tidak seimbang di dataset MRI (beberapa tipe tumor lebih sedikit), jadi saya harus handle class imbalance pakai class weights."

### 5. "Kenapa Autoencoder + CNN, bukan cuma CNN?"
> "CNN memang kuat buat klasifikasi, tapi dia tidak bisa nge-flag data yang aneh/out-of-distribution — kalau input-nya jauh dari training data, dia tetap kasih prediksi confident. Autoencoder belajar reconstruct normal training data, jadi kalau input aneh, rekonstruksinya jelek (error tinggi). Ini buat deteksi anomali — contohnya, kalau ada citra MRI yang bukan otak atau korup, autoencoder bisa flag itu sebelum CNN klasifikasi. Jadi hybrid = klasifikasi + safety net."

---

## B. Pertanyaan Teknis ML / Deep Learning

### 6. "Jelaskan arsitektur CNN dasar"
> "Convolutional Neural Network: input → convolution (feature extraction) → activation (ReLU) → pooling (downsampling) → ... → flatten → dense layers → output (softmax/sigmoid). Convolution nge-apply filter ke input, menghasilkan feature map. Pooling mengurangi dimensi. Deep layer belajar feature abstrak, shallow layer belajar edge/texture."

### 7. "Apa itu transfer learning?"
> "Menggunakan model yang sudah dilatih di dataset besar (misal ImageNet) sebagai starting point, lalu fine-tune di dataset target. Keuntungan: butuh data lebih sedikit dan training lebih cepat. Cara: freeze layer awal (feature umum), train ulang layer akhir (task-specific). Contoh: ResNet50 pre-trained di ImageNet, fine-tune di dataset MRI."

### 8. "Bagaimana kamu handle class imbalance?"
> - **Class weights** di loss function — kelas minoritas dikasih bobot lebih besar
> - **Oversampling** (duplikasi data minoritas) atau **SMOTE** (synthetic)
> - **Data augmentation** khusus kelas minoritas (rotasi, flip, noise)
> - **Focal Loss** — menurunkan loss untuk easy example, fokus ke hard example
> - Evaluasi dengan **F1-score per kelas**, bukan cuma akurasi total

### 9. "Apa perbedaan precision, recall, dan F1?"
> - **Precision** = TP / (TP + FP) — dari semua yang diprediksi positif, berapa yang bener? (False positive = prediksi tumor padahal sehat)
> - **Recall** = TP / (TP + FN) — dari semua yang beneran positif, berapa yang tertangkap? (False negative = tumor tapi diprediksi sehat → **sangat bahaya di medis**)
> - **F1** = 2 × (P × R) / (P + R) — harmonic mean, seimbangkan keduanya
>
> **Di citra medis, recall penting** — tidak boleh ada tumor yang terlewat (FN mahal).

### 10. "Apa itu confusion matrix?"
> Tabel 2D yang nunjukin distribusi prediksi vs label asli. Baris = aktual, kolom = prediksi. Diagonal = benar, off-diagonal = salah. Dari sini bisa liat tipe error: model sering keliru klasifikasi kelas mana?

```
                 Pred
           glioma  menin  pituit  no_t
Akt glioma  [  142     5      2     1  ]
     menin  [    3   138      4     2  ]
     pituit [    1     3    140     0  ]
     no_t   [    0     1      0   140  ]
```

### 11. "Jelaskan CRISP-DM"
> Cross-Industry Standard Process for Data Mining — 6 fase:
> 1. **Business Understanding** — masalah apa? Klasifikasi tumor → bantu dokter deteksi dini
> 2. **Data Understanding** — kumpul & eksplorasi data (7.022 citra, 4 kelas)
> 3. **Data Preparation** — preprocessing (grayscale, resize, normalisasi, CLAHE)
> 4. **Modeling** — pilih algoritma, training, tuning (Autoencoder + CNN)
> 5. **Evaluation** — ukur performa (akurasi, confusion matrix, F1)
> 6. **Deployment** — integrasi ke aplikasi (Flask API, Streamlit demo)
>
> Iteratif — kalau evaluasi kurang, balik ke data prep atau modeling.

### 12. "Apa itu CLAHE dan kenapa dipakai?"
> "Contrast Limited Adaptive Histogram Equalization — teknik preprocessing citra buat tingkatkan kontras lokal tanpa over-amplifikasi noise. Bedanya sama histogram equalization biasa: CLAHE kerja di region kecil (adaptive) dan membatasi kontras (contrast limiting) supaya noise tidak meledak. Di citra MRI, kontras antara jaringan otak dan tumor seringkali rendah, jadi CLAHE bikin batas tumor lebih terlihat tanpa merusak detail."

### 13. "Apa itu vanishing gradient?"
> "Di deep network, gradient dikali weight < 1 tiap layer saat backprop, jadi gradient makin kecil sampai mendekati nol di layer awal — layer awal hampir tidak belajar. Solusi: (1) ReLU activation (gradient 0 atau 1), (2) BatchNorm, (3) Residual connection (ResNet — shortcut bypass), (4) weight initialization He/Xavier."

### 14. "Bagaimana cara mencegah overfitting?"
> - **Data augmentation** (rotasi, flip, zoom, noise)
> - **Dropout** — matikan neuron random saat training
> - **Early stopping** — stop kalau val loss naik
> - **L2 regularization** (weight decay)
> - **Batch normalization**
> - **Lebih banyak data** (selalu yang paling efektif)
> - **Transfer learning** — sudah belajar feature bagus, fine-tune sedikit

### 15. "Apa perbedaan batch, epoch, dan iteration?"
> - **Epoch** — satu kali seluruh dataset diproses
> - **Batch** — jumlah sampel diproses sebelum update weight
> - **Iteration** — satu batch diproses = 1 iteration
> - Contoh: 7.022 data, batch 32 → 1 epoch = 220 iteration

---

## C. Pertanyaan Deployment / MLOps (Junior Level)

### 16. "Bagaimana kamu deploy model ML?"
> "Save model pakai `model.save()` (HDF5) atau `SavedModel` format. Buat Flask API yang load model saat startup, terima upload citra via POST, preprocess input, panggil `model.predict()`, return hasil + confidence sebagai JSON. Di Streamlit, pakai `st.file_uploader` dan `st.image` buat UI interaktif dengan heatmap."

### 17. "Apa itu model serving?"
> "Menyediakan model ML sebagai service yang bisa diakses lewat API (REST/gRPC). Input datang, model inferensi, output dikembaliin. Tools: TensorFlow Serving, TorchServe, Triton, atau Flask custom. Penting: latency rendah, bisa handle concurrent request, versioning model."

### 18. "Bagaimana cara monitor model di produksi?"
> - **Data drift** — distribusi input berubah dari training (pakai Evidently, WhyLabs)
> - **Prediction drift** — distribusi output berubah
> - **Performance metrics** — akurasi, latency, error rate
> - **Logging** — simpan setiap input + prediksi untuk audit
> - **Alert** — kalau metrik turun di bawah threshold, trigger retraining

---

## D. Pertanyaan Python / Coding

### 19. "Tulis fungsi untuk menghitung F1-score"
```python
def f1_score(precision, recall):
    if precision + recall == 0:
        return 0.0
    return 2 * (precision * recall) / (precision + recall)

# Atau dari confusion matrix
from sklearn.metrics import f1_score
y_true = [0, 1, 1, 0, 1, 2]
y_pred = [0, 1, 0, 0, 1, 2]
print(f1_score(y_true, y_pred, average='macro'))  # rata-rata semua kelas
print(f1_score(y_true, y_pred, average='weighted'))  # weighted by support
```

### 20. "Apa perbedaan list comprehension dan generator?"
```python
# List comprehension — bikin list lengkap di memory
squares = [x**2 for x in range(1000000)]  # 1M elemen di RAM

# Generator — lazy, satu item tiap saat
squares_gen = (x**2 for x in range(1000000))  # hampir tidak pakai RAM

# Pakai generator kalau data besar atau hanya perlu iterasi sekali
```

### 21. "Jelaskan NumPy broadcasting"
> "Operasi antar array dengan shape berbeda — NumPy otomatis 'stretch' array kecil supaya cocok tanpa copy data."
```python
import numpy as np
a = np.array([[1, 2, 3],    # shape (2, 3)
              [4, 5, 6]])
b = np.array([10, 20, 30])  # shape (3,)
a + b  # b di-broadcast jadi [[10,20,30],[10,20,30]] → shape (2,3)
```

---

## E. Pertanyaan yang Harus Kamu Tanyakan Balik

- "Model apa yang tim ML di perusahaan ini pakai, dan di-deploy bagaimana?"
- "Apakah ada MLOps pipeline (experiment tracking, CI/CD untuk model)?"
- "Apa ekspektasi output dari intern — riset paper, model deployment, atau keduanya?"
- "Bagaimana tim menghandle data drift dan retraining?"
- "Apakah intern bisa dapat akses ke dataset produksi (anonymized)?"

---

## F. STAR Stories (Siapin 3)

### Story 1: Skripsi — dari Notebook ke Deployment
> **S** — Model ML cuma jalan di Colab, tidak bisa dipakai orang lain.
> **T** — Saya harus bikin model bisa diakses publik tanpa setup lokal.
> **A** — Saya bikin Flask API (upload gambar → inferensi → JSON) dan deploy ke Railway. Juga bikin Streamlit demo dengan heatmap.
> **R** — Model bisa diakses siapa saja lewat browser, ada demo interaktif, dan API siap diintegrasikan.

### Story 2: Optimasi Training dengan GPU Terbatas
> **S** — Training CNN di Colab T4 timeout karena dataset 7.022 citra dan epoch banyak.
> **T** — Harus selesai training dalam 12 jam GPU Colab.
> **A** — Saya tuning batch size (32→64), pakai learning rate scheduler (reduce on plateau), dan early stopping (patience 5). Juga pakai mixed precision training.
> **R** — Training selesai 8 jam, akurasi tetap 95%, resource efisien.

### Story 3: Handle Kelas Tidak Seimbang di MRI
> **S** — Dataset tumor otak tidak seimbang — kelas no_tumor jauh lebih banyak.
> **T** — Model cenderung bias ke kelas mayoritas, recall kelas minoritas rendah.
> **A** — Saya hitung class weights dari distribusi, terapkan di loss function. Juga augmentasi khusus kelas minoritas (rotasi, flip).
> **R** — Recall semua kelas di atas 0.93, F1-score rata-rata 0.95, tidak ada kelas yang terabaikan.
