# MatchIQ — Hybrid AI Football Predictor

MatchIQ adalah aplikasi prediksi pertandingan sepak bola berbasis Flask yang dikembangkan untuk dua kompetisi:

- **Piala Dunia** — tim nasional
- **UEFA Champions League (UCL)** — klub

Aplikasi menggabungkan pendekatan statistik dan machine learning untuk menghasilkan probabilitas pertandingan berdasarkan informasi yang tersedia sebelum kick-off.

Antarmuka tersedia dalam bahasa Indonesia dan Inggris.

> **Portfolio Showcase**
>
> Repository ini hanya menampilkan dokumentasi, metodologi, hasil evaluasi, screenshot, dan demo aplikasi.
>
> Full source code, dataset kerja, training pipeline, notebooks, model artifacts, dan detail implementasi disimpan secara privat.

---

## Tampilan dan Video Demo

[![Tampilan awal MatchIQ](docs/images/matchiq-home.png)](https://drive.google.com/file/d/1SYpFYftIpEfsnr3Ezf8IVBcEx36eUHhJ/view?usp=sharing)

Klik gambar di atas atau buka **[Video Demo MatchIQ](https://drive.google.com/file/d/1SYpFYftIpEfsnr3Ezf8IVBcEx36eUHhJ/view?usp=sharing)** untuk melihat alur penggunaan aplikasi.

---

## Fitur Utama

MatchIQ menyediakan:

- probabilitas hasil pertandingan 1X2;
- expected goals;
- prediksi skor paling mungkin;
- score probability heatmap;
- BTTS;
- Over / Under;
- handicap;
- double chance;
- peluang lolos;
- odd / even;
- win to nil;
- exact total goals;
- indikator resiliensi pertandingan.

Pengguna dapat memilih kompetisi, menentukan tim kandang dan tandang, kemudian menjalankan analisis pertandingan melalui antarmuka web.

---

## Cakupan Data

| Kompetisi | Data historis | Cakupan tim |
|---|---:|---:|
| Piala Dunia | 964 pertandingan, 1930–2022 | 48 tim utama |
| UEFA Champions League | 1.997 pertandingan, 2011/12–2025/26 | 161 klub |

Dataset dan model untuk tim nasional dan klub dipisahkan agar karakteristik kedua jenis kompetisi tidak tercampur dalam proses modeling.

---

## Modeling Approach

MatchIQ menggunakan pendekatan hybrid yang menggabungkan:

### Dixon–Coles

Digunakan untuk memodelkan distribusi jumlah gol dan probabilitas scoreline pertandingan.

### Machine Learning

Model machine learning memanfaatkan fitur historis yang tersedia sebelum kick-off untuk mempelajari pola performa dan kekuatan relatif tim.

### Temporal Evaluation

Evaluasi dilakukan secara chronological / walk-forward sehingga model diuji pada pertandingan yang terjadi setelah periode data training.

Pendekatan ini digunakan untuk mengurangi risiko data leakage dan memberikan simulasi evaluasi yang lebih mendekati penggunaan pada pertandingan baru.

### Probability Calibration

Probabilitas hasil model dikalibrasi agar confidence model lebih representatif terhadap hasil aktual.

---

## Model Evaluation — UCL v4

Evaluasi UCL v4 dilakukan menggunakan **nested walk-forward evaluation** pada empat outer seasons dengan total **628 pertandingan**.

| Metric | Result |
|---|---:|
| Argmax Accuracy | **56.21%** |
| Multiclass Log Loss | **0.9399** |
| Dixon–Coles Log Loss | **1.0024** |
| Expected Calibration Error | **2.73%** |
| Draw-aware Accuracy | **52.71%** |
| Draw Recall | **22.12%** |

Multiclass log loss MatchIQ lebih rendah dibandingkan baseline Dixon–Coles pada evaluation split yang sama.

Benchmark lama yang menggunakan informasi setelah kick-off tidak digunakan sebagai benchmark utama. Evaluasi terbaru hanya menggunakan informasi yang tersedia sebelum pertandingan.

---

## System Workflow

```mermaid
flowchart LR
    A[Historical Match Data] --> B[Data Validation]
    B --> C[Pre-match Feature Engineering]
    C --> D[Statistical Model]
    C --> E[Machine Learning Model]
    D --> F[Hybrid Prediction Layer]
    E --> F
    F --> G[Probability Calibration]
    G --> H[Match Prediction]
    H --> I[Flask Web Interface]
