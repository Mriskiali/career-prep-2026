# Docker Showcase — Host Proyek di VPS

Setup ini bikin lu bisa nunjukin proyek live ke recruiter/HRD langsung dari VPS lu sendiri.

## Struktur

```
docker-showcase/
├── docker-compose.yml      # Orkestrasi semua service
├── nginx/
│   └── nginx.conf          # Reverse proxy + routing subdomain
├── brain-tumor-api/        # Model ML inferensi (Flask)
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
├── bloxboxd/              # (link ke repo asli, di-deploy via Vercel)
└── portfolio-site/        # Landing page sederhana (static)
    ├── index.html
    └── Dockerfile
```

---

## docker-compose.yml

```yaml
version: "3.9"

services:
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - brain-tumor-api
    restart: unless-stopped

  brain-tumor-api:
    build: ./brain-tumor-api
    expose:
      - "5000"
    environment:
      - MODEL_PATH=/app/model
    restart: unless-stopped

  portfolio-site:
    build: ./portfolio-site
    expose:
      - "3000"
    restart: unless-stopped
```

---

## nginx/nginx.conf

```nginx
events {
    worker_connections 1024;
}

http {
    upstream brain_tumor {
        server brain-tumor-api:5000;
    }

    upstream portfolio {
        server portfolio-site:3000;
    }

    server {
        listen 80;
        server_name muafariskiali.com www.muafariskiali.com;

        location / {
            proxy_pass http://portfolio;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }

    server {
        listen 80;
        server_name ml.muafariskiali.com;

        location / {
            proxy_pass http://brain_tumor;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            client_max_body_size 10M;  # upload citra MRI
        }
    }
}
```

---

## brain-tumor-api/Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Asumsi model sudah ada di folder model/
EXPOSE 5000

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "2", "--timeout", "120", "app:app"]
```

## brain-tumor-api/requirements.txt

```
flask==3.0.0
gunicorn==21.2.0
tensorflow==2.15.0
numpy==1.24.3
Pillow==10.2.0
opencv-python-headless==4.9.0.80
```

## brain-tumor-api/app.py (Skeleton)

```python
import os
import numpy as np
from flask import Flask, request, jsonify
from PIL import Image
import tensorflow as tf

app = Flask(__name__)

MODEL_PATH = os.environ.get("MODEL_PATH", "/app/model")
model = tf.keras.models.load_model(MODEL_PATH)

CLASS_NAMES = ["glioma", "meningioma", "pituitary", "no_tumor"]
IMG_SIZE = 128

def preprocess(image_file):
    img = Image.open(image_file).convert("L")  # grayscale
    img = img.resize((IMG_SIZE, IMG_SIZE))
    arr = np.array(img) / 255.0
    arr = np.expand_dims(arr, axis=(0, -1))  # batch + channel
    return arr

@app.route("/predict", methods=["POST"])
def predict():
    if "file" not in request.files:
        return jsonify({"error": "No file uploaded"}), 400

    file = request.files["file"]
    try:
        processed = preprocess(file)
        preds = model.predict(processed)
        class_idx = int(np.argmax(preds[0]))
        confidence = float(preds[0][class_idx])

        return jsonify({
            "class": CLASS_NAMES[class_idx],
            "confidence": round(confidence * 100, 2),
            "all_probabilities": {
                name: round(float(p) * 100, 2)
                for name, p in zip(CLASS_NAMES, preds[0])
            }
        })
    except Exception as e:
        return jsonify({"error": str(e)}), 500

@app.route("/health", methods=["GET"])
def health():
    return jsonify({"status": "ok", "model_loaded": model is not None})

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## portfolio-site/index.html (Landing Page)

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mu'afa Riski Ali — ML & Mobile Developer</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
            background: #0d1117; color: #c9d1d9; line-height: 1.6;
        }
        .container { max-width: 800px; margin: 0 auto; padding: 60px 20px; }
        h1 { font-size: 2.5em; margin-bottom: 10px; }
        .subtitle { color: #58a6ff; font-size: 1.2em; margin-bottom: 40px; }
        .project {
            background: #161b22; border: 1px solid #30363d;
            border-radius: 8px; padding: 24px; margin-bottom: 20px;
            transition: border-color 0.3s;
        }
        .project:hover { border-color: #58a6ff; }
        .project h3 { color: #f0f6fc; margin-bottom: 8px; }
        .project p { margin-bottom: 12px; }
        .badge {
            display: inline-block; background: #238636; color: #fff;
            padding: 2px 12px; border-radius: 16px; font-size: 0.85em;
            margin-right: 8px;
        }
        .links { margin-top: 40px; display: flex; gap: 20px; flex-wrap: wrap; }
        .links a {
            color: #58a6ff; text-decoration: none;
            border-bottom: 1px dashed; padding-bottom: 2px;
        }
        .links a:hover { color: #f0f6fc; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Mu'afa Riski Ali</h1>
        <p class="subtitle">Machine Learning & Mobile Developer · IPK 3.76 · Fresh Grad Informatika</p>

        <div class="project">
            <h3>🧠 Brain Tumor MRI Classification</h3>
            <p>Hybrid Autoencoder + CNN untuk klasifikasi 4 jenis tumor otak dari citra MRI. Akurasi 95% pada 7.022 citra.</p>
            <span class="badge">TensorFlow</span>
            <span class="badge">CNN</span>
            <span class="badge">Flask API</span>
            <a href="https://ml.muafariskiali.com/predict">→ Coba Live Demo</a>
        </div>

        <div class="project">
            <h3>🎮 Bloxboxd — Social Logging Platform</h3>
            <p>Platform social logging dan review untuk pengalaman Roblox. Terintegrasi Roblox API & Turso DB.</p>
            <span class="badge">TypeScript</span>
            <span class="badge">Turso</span>
            <span class="badge">Vercel</span>
        </div>

        <div class="project">
            <h3>📱 Motion-fits — Fitness Tracker</h3>
            <p>Aplikasi mobile fitness tracker dengan Expo Router dan navigasi file-based.</p>
            <span class="badge">React Native</span>
            <span class="badge">Expo</span>
        </div>

        <div class="links">
            <a href="https://github.com/Mriskiali">GitHub</a>
            <a href="https://linkedin.com/in/muafa-riski-ali-3114b536b">LinkedIn</a>
            <a href="mailto:muafariskiali1805@gmail.com">Email</a>
            <a href="https://kaggle.com/code/riskiali">Kaggle</a>
        </div>
    </div>
</body>
</html>
```

## portfolio-site/Dockerfile

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 3000
```

---

## Cara Menjalankan

```bash
# 1. Clone repo ini
git clone https://github.com/Mriskiali/career-prep-2026.git
cd career-prep-2026/docker-showcase

# 2. Siapkan model
# Copy model.h5 / saved_model ke brain-tumor-api/model/

# 3. Build & run semua
docker compose up -d --build

# 4. Cek status
docker compose ps
curl http://localhost/health
```

## Opsional: Domain + HTTPS

```bash
# Install certbot
sudo apt install certbot python3-certbot-nginx

# Dapat sertifikat SSL
sudo certbot --nginx -d muafariskiali.com -d www.muafariskiali.com -d ml.muafariskiali.com

# Auto-renewal sudah otomatis via systemd timer
```

---

## Yang Harus Lu Siapin Sebelum Demo

- [ ] Model `.h5` atau `SavedModel` dari skripsi
- [ ] VPS punya minimal 2GB RAM (TF butuh memory)
- [ ] Domain (opsional, bisa pakai IP dulu)
- [ ] Docker + Docker Compose terinstall
