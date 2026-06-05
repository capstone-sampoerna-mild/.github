# SkillsGap AI — Capstone Project CC26-PSU404

> **Coding Camp 2026 powered by DBS Foundation**  
> Tema: *Future-Ready Work & Economy*

Platform berbasis AI untuk membantu pencari kerja di sektor IT menemukan jalur karier yang tepat, menganalisis kesenjangan skill (*skill gap*), dan mendapatkan rekomendasi skill tambahan yang relevan berdasarkan data lowongan kerja riil.

---

## Daftar Isi

- [Tentang Proyek](#tentang-proyek)
- [Fitur Utama](#fitur-utama)
- [Arsitektur Sistem](#arsitektur-sistem)
- [Tech Stack](#tech-stack)
- [Anggota Tim](#anggota-tim)
- [Repositori](#repositori)
- [Panduan Instalasi & Menjalankan](#panduan-instalasi--menjalankan)
  - [1. Training Model AI](#1-training-model-ai)
  - [2. Menambahkan Artefak Model ke AI Service](#2-menambahkan-artefak-model-ke-ai-service)
  - [3. Instalasi & Menjalankan AI Service](#3-instalasi--menjalankan-ai-service)
  - [4. Instalasi & Menjalankan Backend](#4-instalasi--menjalankan-backend)
  - [5. Instalasi & Menjalankan Frontend](#5-instalasi--menjalankan-frontend)
- [Endpoint API](#endpoint-api)
- [Alur Sistem](#alur-sistem)

---

## Tentang Proyek

Banyak pencari kerja dan lulusan baru mengalami kebingungan dalam memetakan skill yang mereka miliki ke posisi pekerjaan yang relevan di industri IT. Terjadi kesenjangan (*skill gap*) yang lebar antara kompetensi pelamar dan kualifikasi yang dibutuhkan perusahaan.

**SkillsGap AI** hadir sebagai solusi nyata (*painkiller*), bukan sekadar pelengkap. Sistem ini:

- Mengumpulkan data lowongan kerja IT riil melalui *web scraping*
- Melatih model klasifikasi *job role* menggunakan arsitektur **BiLSTM + Multi-Head Attention + TF-IDF** berbasis TensorFlow/Keras
- Menyediakan rekomendasi posisi pekerjaan yang paling sesuai berdasarkan input skill, CV, atau sertifikat pengguna
- Menganalisis *skill gap* dan merekomendasikan skill tambahan yang perlu dipelajari
- Menyediakan AI assistant chat untuk mendampingi eksplorasi karier pengguna

**Fokus industri:** Sektor *Information Technology* (Software Engineer, Data Scientist, Data Engineer, Frontend/Backend/Fullstack Developer, DevOps Engineer, ML Engineer, Data Analyst, UI/UX Designer, dll.)

---

## Fitur Utama

- Login dengan Google Authentication (Firebase)
- Upload CV / sertifikat berbentuk PDF untuk ekstraksi skill otomatis
- Input skill secara manual
- Rekomendasi *job role* berbasis model ML lokal (BiLSTM + Attention + TF-IDF)
- Rekomendasi *job role* via Gemini AI (dengan penjelasan lebih kaya)
- Analisis gabungan CV + skill manual untuk hasil yang lebih akurat
- AI Assistant Chat (streaming SSE)
- Riwayat pencarian dan histori pengguna
- Antarmuka responsif (mobile & desktop)

---

## Arsitektur Sistem

```
┌─────────────────────────────────────────────────────────────┐
│                        FRONTEND                             │
│              React 19 + Vite + Tailwind CSS                 │
│           (Google Firebase Authentication)                  │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTP / SSE
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                    BACKEND (API Gateway)                    │
│                  Express.js — Port 5000                     │
│   - Verifikasi Firebase ID Token                            │
│   - Proxy request ke AI Service                             │
│   - Error handling terpusat                                 │
│   - Swagger API Documentation                               │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTP / SSE / Multipart
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                   AI SERVICE (FastAPI)                      │
│                  Python 3.11 — Port 8001                    │
│   ┌──────────────────────────────────────────────────────┐  │
│   │  Model: BiLSTM + Multi-Head Attention + TF-IDF       │  │
│   │  (TensorFlow/Keras — job_role_model.keras)           │  │
│   │  + LabelEncoder + TF-IDF Vectorizer (scikit-learn)   │  │
│   └──────────────────────────────────────────────────────┘  │
│   ┌──────────────────────────────────────────────────────┐  │
│   │  Gemini AI API (google-genai)                        │  │
│   │  Chat Stream + Job Role Recommend (Gemini)           │  │
│   └──────────────────────────────────────────────────────┘  │
│   ┌──────────────────────────────────────────────────────┐  │
│   │  Supabase Vector DB                                  │  │
│   │  (penyimpanan embeddings & dokumen PDF)              │  │
│   └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## Tech Stack

| Layer | Teknologi |
|---|---|
| **Frontend** | React 19, Vite, React Router DOM, Axios, Tailwind CSS, Lucide React, Firebase Auth |
| **Backend** | Node.js, Express.js, Swagger (OpenAPI), Firebase Admin SDK |
| **AI Service** | Python 3.11, FastAPI, Uvicorn, TensorFlow/Keras, scikit-learn, joblib, google-genai (Gemini), PyMuPDF, LangChain Text Splitters |
| **Database** | Supabase (Relational — user data & history), Supabase Vector DB (embeddings) |
| **Model ML** | BiLSTM + Multi-Head Attention + TF-IDF Vectorizer (arsitektur dual-branch) |
| **DevOps** | Docker, Docker Compose, Caddy (reverse proxy) |
| **Data** | Web scraping (BeautifulSoup/Selenium), CSV dari portal lowongan IT |

---

## Anggota Tim

| ID | Nama | Peran |
|---|---|---|
| CFCC726D6Y0039 | Eval Putra Parasdika | Full Stack Developer |
| CFCC726D6Y0175 | Arbasya Anandito | Full Stack Developer |
| CACC726D6Y0208 | Bikra Abna Filqiyast Dzaki | AI Engineer |
| CDCC006D6Y1858 | Fredo Dwi Rangga | Data Scientist |
| CDCC006D6Y2351 | Firman Adi Kurniawan | Data Scientist |

---

## Repositori

| Komponen | Repository |
|---|---|
| AI Service (FastAPI) | https://github.com/capstone-sampoerna-mild/capstone-ai-service |
| Backend (Express.js) | https://github.com/capstone-sampoerna-mild/capstone-backend |
| Frontend (React + Vite) | https://github.com/capstone-sampoerna-mild/capstone-frontend |

---

## Panduan Instalasi & Menjalankan

> **Urutan yang disarankan:**
> Training AI → Tambahkan artefak model → Jalankan AI Service → Jalankan Backend → Jalankan Frontend

---

### 1. Training Model AI

Model dilatih menggunakan Jupyter Notebook (`main.ipynb`) di Google Colab atau environment lokal.

#### Prasyarat

- Python 3.11 (direkomendasikan)
- Dataset CSV hasil scraping lowongan kerja IT (simpan di folder `datasets/`)
- GPU (opsional, namun sangat mempercepat training)

#### Dependensi Training

```bash
pip install tensorflow scikit-learn pandas numpy joblib nltk
```

#### Langkah Training

**1. Siapkan dataset**

Letakkan satu atau lebih file CSV hasil scraping ke dalam folder `datasets/`. Setiap file harus memiliki minimal kolom:

- `title` — judul posisi pekerjaan
- `job_description` — deskripsi pekerjaan
- `extracted_skills` — daftar skill (format list Python sebagai string, misal `"['Python', 'TensorFlow']"`)
- `search_role` — label role target (misal `"Data Scientist"`, `"Backend Developer"`)

**2. Jalankan notebook**

Buka `main.ipynb` di Google Colab atau Jupyter, lalu jalankan semua cell secara berurutan:

- **Cell 1–3:** Mount drive (Colab) atau set path lokal, import library
- **Cell 4–6:** Load dan gabungkan semua CSV, drop kolom tidak relevan, filter judul non-IT
- **Cell 7–9:** Feature engineering — parse skill, bersihkan teks, buat kolom `text_input` (format: `skills [SEP] title [SEP] job_description`)
- **Cell 10:** Buat `skills_freq_per_role.json` (frekuensi skill per role)
- **Cell 11–12:** Normalisasi label role, filter role dengan sampel < 200
- **Cell 13:** Fit `LabelEncoder`, encode label `y`
- **Cell 14:** Split train/val (85:15), hitung `class_weight`, fit `TfidfVectorizer` (max_features=10000, ngram_range=(1,2)), buat `TextVectorization` layer
- **Cell 15:** Definisikan `CustomEarlyStopping` callback
- **Cell 16:** Bangun dan compile model `BiLSTM_Attention_TFIDF`
- **Cell 17:** Training dengan `model.fit()` (max 150 epoch, batch 128, early stopping patience=15)
- **Cell 18:** Evaluasi dengan `classification_report`
- **Cell 19:** Simpan artefak

```python
# Cell simpan artefak — dijalankan otomatis di akhir notebook
model.save(f'{dataset_path}/job_role_model.keras')
joblib.dump(le,    f'{dataset_path}/label_encoder.pkl')
joblib.dump(tfidf, f'{dataset_path}/tfidf_vectorizer.pkl')
```

Setelah selesai, tiga file ini akan tersedia di folder `datasets/`:

- `job_role_model.keras` — model TensorFlow tersimpan
- `label_encoder.pkl` — LabelEncoder scikit-learn
- `tfidf_vectorizer.pkl` — TF-IDF Vectorizer scikit-learn

---

### 2. Menambahkan Artefak Model ke AI Service

Setelah training selesai, salin ketiga file artefak ke dalam folder `models/` di repositori `capstone-ai-service`:

```
capstone-ai-service/
└── models/
    ├── job_role_model.keras        ← dari hasil training
    ├── label_encoder.pkl           ← dari hasil training
    └── tfidf_vectorizer.pkl        ← dari hasil training
```

Cara menyalin (contoh dari Google Drive ke lokal, lalu ke repo):

```bash
# Jika training di Colab, download dari Google Drive ke lokal
# Lalu salin ke folder models/ di repo AI service

cp /path/to/datasets/job_role_model.keras    capstone-ai-service/models/
cp /path/to/datasets/label_encoder.pkl       capstone-ai-service/models/
cp /path/to/datasets/tfidf_vectorizer.pkl    capstone-ai-service/models/
```

> **Catatan:** Folder `models/` sudah ada di repositori (terdapat file placeholder). Pastikan ketiga file di atas menggantikan atau ditambahkan ke dalamnya. File `.keras` dan `.pkl` tidak dicommit ke Git (ada di `.gitignore`), jadi harus disalin secara manual.

---

### 3. Instalasi & Menjalankan AI Service

#### Prasyarat

- Python 3.11 (wajib — TensorFlow paling stabil di Python 3.11)
- `pyenv` (direkomendasikan untuk manajemen versi Python)
- Akun Supabase (URL + Service Key)
- Gemini API Key

#### Clone Repositori

```bash
git clone https://github.com/capstone-sampoerna-mild/capstone-ai-service.git
cd capstone-ai-service
```

#### Setup Python Environment

```bash
# Install Python 3.11.9 via pyenv (jika belum ada)
pyenv install 3.11.9

# Set versi Python untuk repo ini (file .python-version sudah tersedia)
pyenv local 3.11.9

# Buat virtual environment
python -m venv venv

# Aktifkan venv
source venv/bin/activate        # Linux / macOS
# atau
venv\Scripts\activate           # Windows
```

#### Install Dependensi

```bash
pip install -r requirements.txt
```

Dependensi utama yang akan terinstall:

```
fastapi
uvicorn
tensorflow
scikit-learn==1.6.1
joblib
google-genai
supabase
PyMuPDF
langchain-text-splitters
pydantic
pydantic-settings
python-dotenv
python-multipart
```

#### Konfigurasi Environment

```bash
cp .env.example .env
```

Edit `.env` dan isi nilai berikut:

```env
GEMINI_API_KEY="your_gemini_api_key"
SUPABASE_URL="https://your-project.supabase.co"
SUPABASE_SERVICE_KEY="your_supabase_service_role_key"
```

#### Pastikan Artefak Model Tersedia

Sebelum menjalankan service, pastikan folder `models/` sudah berisi:

```
models/
├── job_role_model.keras
├── label_encoder.pkl
└── tfidf_vectorizer.pkl
```

#### Menjalankan AI Service

```bash
# Dari root direktori repo (metode utama)
uvicorn main:app --reload --host 0.0.0.0 --port 8001

# Alternatif (path modul langsung)
uvicorn app.main:app --reload --host 0.0.0.0 --port 8001
```

Service akan berjalan di: `http://localhost:8001`

Dokumentasi API otomatis tersedia di: `http://localhost:8001/docs`

#### Menjalankan via Docker (Opsional)

```bash
# Build dan jalankan dengan Docker Compose
docker-compose up --build

# Jalankan di background
docker-compose up -d --build
```

---

### 4. Instalasi & Menjalankan Backend

Backend adalah API Gateway berbasis Express.js yang menjadi perantara antara frontend dan AI service.

#### Prasyarat

- Node.js v18 atau lebih baru
- npm
- AI Service sudah berjalan di port 8001
- Firebase project (untuk Google Authentication)
- Supabase project

#### Clone Repositori

```bash
git clone https://github.com/capstone-sampoerna-mild/capstone-backend.git
cd capstone-backend
```

#### Install Dependensi

```bash
npm install
```

#### Konfigurasi Environment

```bash
cp .env.example .env
```

Edit `.env` dan isi semua nilai:

```env
# Server Configuration
PORT=5000
HOST=0.0.0.0
NODE_ENV=development
API_VERSION=v1

# FastAPI AI Service
FASTAPI_BASE_URL=http://127.0.0.1:8001
FASTAPI_CHAT_STREAM_PATH=/chat-ai/chat-ai/stream
FASTAPI_JOB_ROLE_RECOMMEND_PATH=/job-role/job-role/recommend
FASTAPI_JOB_ROLE_RECOMMEND_GEMINI_PATH=/job-role/job-role/recommend/gemini
FASTAPI_JOB_ROLE_RECOMMEND_STREAM_PATH=/job-role/job-role/recommend/stream
FASTAPI_JOB_ROLE_RECOMMEND_GEMINI_STREAM_PATH=/job-role/job-role/recommend/gemini/stream
FASTAPI_DOCUMENT_UPLOAD_PATH=/document/predict-pdf
FASTAPI_TIMEOUT_MS=60000

# CORS Configuration
CORS_ORIGIN=http://localhost:3000

# Logging
LOG_LEVEL=debug

# Firebase (Google Login)
FIREBASE_PROJECT_ID=your-firebase-project-id

# JWT
JWT_SECRET=your_jwt_secret_here
JWT_REFRESH_SECRET=your_jwt_refresh_secret_here
JWT_ACCESS_TTL_MINUTES=15
JWT_REFRESH_TTL_DAYS=7

# Supabase
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key
```

> **Catatan path FastAPI:** Beberapa route di AI service menggunakan prefix ganda (misal `/job-role/job-role/recommend`). Jika path di AI service berubah, cukup sesuaikan nilai `FASTAPI_*_PATH` di `.env` tanpa mengubah kode backend.

#### Menjalankan Backend

```bash
# Development (dengan auto-reload via nodemon)
npm run dev

# Production
npm start
```

Backend akan berjalan di: `http://localhost:5000`

Dokumentasi Swagger tersedia di: `http://localhost:5000/api-docs/`

Health check: `http://localhost:5000/api/v1/health`

---

### 5. Instalasi & Menjalankan Frontend

#### Prasyarat

- Node.js v18 atau lebih baru
- npm
- Backend sudah berjalan di port 5000
- Firebase project (konfigurasi sama dengan backend)

#### Clone Repositori

```bash
git clone https://github.com/capstone-sampoerna-mild/capstone-frontend.git
cd capstone-frontend
```

#### Install Dependensi

```bash
npm install
```

#### Konfigurasi Environment

Buat file `.env` di root direktori frontend:

```env
VITE_API_URL=http://localhost:5000/api/v1

VITE_FIREBASE_API_KEY=your_firebase_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_firebase_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_firebase_app_id
```

> Nilai konfigurasi Firebase dapat ditemukan di **Firebase Console → Project Settings → Your Apps → Web App Config**.

#### Menjalankan Frontend

```bash
# Development server
npm run dev
```

Frontend akan berjalan di: `http://localhost:3000` (atau port lain yang ditampilkan di terminal)

```bash
# Build untuk production
npm run build

# Preview hasil build production
npm run preview
```

---

## Endpoint API

Semua request ke AI service dilakukan melalui backend (Express API Gateway) dengan base URL `http://localhost:5000/api/v1`.

| Method | Path | Keterangan |
|---|---|---|
| `GET` | `/health` | Status server dan info runtime |
| `POST` | `/auth/google` | Verifikasi login Google via Firebase ID Token |
| `POST` | `/chat/ai/stream` | AI assistant chat (streaming SSE) |
| `POST` | `/job-role/recommend` | Rekomendasi job role via model ML lokal (BiLSTM) |
| `POST` | `/job-role/recommend/gemini` | Rekomendasi job role via Gemini AI |
| `POST` | `/job-role/recommend/stream` | Streaming rekomendasi job role (SSE) |
| `POST` | `/job-role/recommend/gemini/stream` | Streaming rekomendasi job role via Gemini (SSE) |
| `POST` | `/document/upload` | Upload CV atau sertifikat PDF untuk analisis skill |

### Contoh Request — Rekomendasi Job Role (Model Lokal)

```json
POST /api/v1/job-role/recommend
{
  "nama": "Budi",
  "skillset": ["React", "NextJS", "TypeScript", "PostgreSQL"]
}
```

```json
// Response
{
  "reply": "hai Budi, pekerjaan yang cocok untukmu adalah Frontend Developer"
}
```

### Contoh Upload Dokumen

```bash
# Upload CV
curl -X POST http://localhost:5000/api/v1/document/upload \
  -F "cv=@/path/to/cv.pdf" \
  -F "documentType=cv"

# Upload sertifikat
curl -X POST http://localhost:5000/api/v1/document/upload \
  -F "certificate=@/path/to/certificate.pdf" \
  -F "documentType=certificate"
```

---

## Alur Sistem

### Analisis via Input Skill Manual

```
User input skill → Frontend → Backend → AI Service (BiLSTM Model) → Rekomendasi job role
```

### Analisis via Upload CV / Sertifikat

```
User upload PDF → Frontend → Backend → AI Service (PyMuPDF extract text) 
  → Gemini AI (ekstrak skill dari teks) → Model BiLSTM → Rekomendasi job role
```

### Analisis Gabungan (CV + Skill Manual)

```
User upload PDF + input skill → Frontend → Backend → AI Service
  → Gabungkan skill dari PDF dan input manual → Model BiLSTM → Rekomendasi lebih akurat
```

### AI Chat Stream

```
User kirim pesan → Frontend (SSE) → Backend (SSE proxy) → AI Service (Gemini streaming) → Respons real-time
```

---

## Struktur Folder

### AI Service (`capstone-ai-service`)

```
capstone-ai-service/
├── app/                  # Modul FastAPI (router, controller, service)
├── models/               # Artefak model ML (*.keras, *.pkl)
├── static/               # File statis
├── main.py               # Entry point FastAPI
├── requirements.txt      # Dependensi Python
├── Dockerfile
├── docker-compose.yml
├── Caddyfile             # Konfigurasi reverse proxy Caddy
└── .env.example
```

### Backend (`capstone-backend`)

```
capstone-backend/
├── src/
│   ├── config/           # Konfigurasi environment dan swagger
│   ├── constants/        # Konstanta aplikasi
│   ├── controllers/      # Logic handler endpoint
│   ├── middlewares/      # CORS, logging, error handling
│   ├── routes/           # Definisi route API (versioned)
│   ├── schemas/          # Template schema respons
│   └── utils/            # Proxy FastAPI, formatter respons, auth Firebase
├── migrations/           # Migrasi database
├── package.json
├── schema.sql
└── .env.example
```

### Frontend (`capstone-frontend`)

```
capstone-frontend/
├── src/
│   ├── components/       # Komponen UI reusable
│   ├── layouts/          # Layout halaman
│   ├── pages/            # Halaman utama (Login, Dashboard, Chat, dll)
│   ├── services/         # Axios API calls
│   ├── routes/           # Konfigurasi routing React Router
│   ├── assets/           # Gambar dan aset statis
│   └── main.jsx          # Entry point React
├── public/
├── index.html
├── vite.config.js
├── tailwind.config.js
└── vercel.json
```

---

*SkillsGap AI — Capstone Project CC26-PSU404 | Coding Camp 2026 powered by DBS Foundation*
