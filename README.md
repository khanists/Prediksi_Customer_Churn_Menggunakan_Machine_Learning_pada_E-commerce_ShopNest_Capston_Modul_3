[README (2).md](https://github.com/user-attachments/files/27228998/README.2.md)
# 🛒 Prediksi Customer Churn Menggunakan Machine Learning pada E-commerce ShopNest

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)
![LightGBM](https://img.shields.io/badge/Model-LightGBM-green)
![Streamlit](https://img.shields.io/badge/App-Streamlit-red?logo=streamlit)
![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)

---

## 📌 Overview Project

**ShopNest** adalah platform e-commerce yang berdiri sejak 2015 dengan lebih dari **3,9 juta pelanggan aktif** yang tersebar di 50+ kota di Indonesia. Meskipun bisnis tumbuh pesat — GMV naik 42% dari 2022 ke 2023 — perusahaan menghadapi tantangan serius: **churn rate meningkat hingga 17% dalam 1 tahun terakhir**.

Proyek ini membangun model machine learning berbasis **supervised learning (binary classification)** untuk memprediksi apakah seorang pelanggan akan churn atau tidak, sehingga tim CRM dapat mengambil tindakan preventif **sebelum pelanggan benar-benar pergi**.

Pendekatan yang digunakan mencakup eksplorasi data (EDA), feature engineering, benchmarking berbagai algoritma, penanganan data imbalanced dengan **Random OverSampling (ROS)**, hyperparameter tuning, threshold optimization, hingga explainability model menggunakan **SHAP values**.

---

## 👥 Stakeholder

### 💼 Investor
| Pihak | Kepentingan |
|-------|-------------|
| Founder / C-Level | Memantau dampak churn terhadap GMV dan pertumbuhan bisnis |
| External Investor | Mengevaluasi efisiensi operasional dan ROI dari inisiatif retensi |

> Model ini diproyeksikan dapat **mengurangi churn rate dari 17% → 10–12%** dalam 6 bulan pertama, dengan ROI intervensi sebesar **24.9% per siklus kampanye**.

### 👤 User (Internal)
| Tim | Penggunaan |
|-----|------------|
| **Tim CRM / Marketing** | Menggunakan output model untuk menentukan target kampanye retensi (voucher, cashback, outreach) |
| **Tim Data / Analyst** | Memantau performa model dan menentukan kapan retraining diperlukan |
| **Product Manager** | Membaca insight fitur untuk perbaikan produk dan pengalaman pelanggan |

---

## 🎯 Tujuan

1. **Membangun model prediksi churn** yang mampu mengklasifikasikan pelanggan sebagai churn (1) atau tidak churn (0)
2. **Mencapai Recall ≥ 80%** pada kelas churn — meminimalkan False Negative (pelanggan churn yang tidak terdeteksi)
3. **Mencapai PR-AUC ≥ 0.70** sebagai validasi performa pada data imbalanced
4. **Membantu tim CRM** beralih dari pendekatan reaktif ke **proaktif** dalam menangani risiko churn
5. **Menghemat biaya re-akuisisi** — biaya mencari pelanggan baru 5× lebih mahal dibanding mempertahankan pelanggan lama

### Metric yang Digunakan
| Metric | Peran | Alasan |
|--------|-------|--------|
| **Recall** | Primary | Meminimalkan FN — pelanggan churn yang tidak terdeteksi |
| **F1-Score** | Secondary | Keseimbangan antara precision dan recall |
| **PR-AUC** | Validation | Robust terhadap class imbalance |
| **Precision** | Monitoring | Mengontrol false alarm (FP) |

---

## 📊 Result / Output Project

### Model Terbaik: 🔥 ROS | LightGBM Hypertuning

| Metric | Sebelum Tuning | Setelah Tuning + Threshold Optimization |
|--------|---------------|----------------------------------------|
| Recall | 0.73 | **0.88** ✅ |
| F1-Score | 0.69 | **0.75** |
| PR-AUC | 0.77 | **0.79** |
| Precision | 0.66 | 0.65 |

### Perbandingan Semua Model (Test Set)

| Model | Recall | F1 | PR-AUC | Precision |
|-------|--------|----|--------|-----------|
| None \| LightGBM (Baseline) | 0.71 | 0.69 | 0.77 | 0.68 |
| None \| XGBoost | 0.69 | 0.67 | 0.75 | 0.65 |
| ROS \| XGBoost | 0.75 | 0.72 | 0.78 | 0.69 |
| ROS \| LightGBM | 0.79 | 0.73 | 0.79 | 0.68 |
| Penalized \| LightGBM | 0.76 | 0.71 | 0.78 | 0.67 |
| **🔥 ROS \| LightGBM Hypertuning** | **0.88** | **0.75** | **0.79** | **0.65** |

### Hyperparameter Terpilih
```python
{
    'learning_rate'    : 0.09807054515547668,
    'max_bin'          : 252,
    'min_data_in_leaf' : 35,
    'n_estimators'     : 117,
    'num_leaves'       : 30,
    'random_state'     : 42
}
```

### Top 5 Faktor Penyebab Churn (SHAP)
1. 💰 **CashbackAmount rendah** — pelanggan yang mendapat sedikit cashback lebih mudah churn
2. ⏳ **Tenure singkat** — pelanggan baru belum memiliki loyalitas yang kuat
3. 😤 **Pernah komplain** — komplain yang tidak tertangani mendorong pelanggan pergi
4. 🕐 **DaySinceLastOrder tinggi** — inaktif lama adalah sinyal kuat akan meninggalkan platform
5. 📍 **NumberOfAddress / Device tinggi** — pengguna yang terdaftar di banyak perangkat/alamat cenderung kurang loyal

### Business Impact (Cost-Benefit Analysis)
| Skenario | Keterangan |
|----------|------------|
| **Tanpa model** | Seluruh pelanggan churn tidak tertangani → revenue loss penuh |
| **Dengan model** | 88% churn terdeteksi → ~60% berhasil diretain |
| **Revenue diselamatkan** | Estimasi Rp 2–5 juta/bulan (asumsi ~50 pelanggan at-risk/bulan, CAC Rp 150rb) |
| **ROI model** | **24.9%** per siklus kampanye retensi |
| **Efisiensi** | Model mengurangi kerugian revenue churn hingga **±70%** vs pendekatan reaktif |

---

## 💡 Rekomendasi

### 📁 Rekomendasi Data
Performa model dapat ditingkatkan dengan penambahan fitur berikut:
- **RFM Metrics** (Recency, Frequency, Monetary) — sangat relevan untuk e-commerce
- **CustomerSupportInteractions** — frekuensi menghubungi customer service
- **AppEngagementScore** — seberapa aktif pelanggan membuka aplikasi
- **PromotionResponseRate** — respon terhadap promo/voucher yang dikirim
- **ReturnRate** — frekuensi pengembalian produk
- **PaymentMethod** — metode pembayaran (kartu kredit cenderung lebih loyal)
- **Geolocation / Region** — pola churn berbeda antar wilayah

> Penambahan fitur-fitur ini diprediksi dapat mendorong Recall ke **> 90%**

### 🤖 Rekomendasi Model
- Tambah **feature interaksi**: `Tenure × Complain`, `CashbackAmount ÷ DaySinceLastOrder`
- Coba **XGBoost tuned** sebagai model alternatif untuk perbandingan
- Eksplorasi **Survival Analysis** untuk memodelkan *time-to-churn*
- Pertimbangkan **ensemble stacking** (LightGBM + XGBoost) untuk performa lebih tinggi

### 🎯 Rekomendasi Bisnis (Tim CRM / Marketing)
- **Prioritas intervensi**: Pelanggan dengan Tenure < 6 bulan **DAN** pernah komplain
- Kirim **voucher cashback targeted** dalam 24 jam setelah pelanggan terdeteksi berisiko tinggi
- Buat program **onboarding yang lebih baik** untuk pelanggan baru agar tenure bertambah
- Luncurkan **Winback Campaign** bulanan untuk pelanggan yang sudah lama tidak belanja
- Tingkatkan **kualitas customer service** untuk mengurangi komplain yang tidak tertangani

### 🔧 Rekomendasi Operasional (Deployment & Maintenance)
| Parameter | Rekomendasi |
|-----------|-------------|
| **Frekuensi prediksi** | Batch prediction 1× per bulan untuk seluruh database pelanggan aktif |
| **Jadwal retraining** | Setiap 3 bulan (quarterly) |
| **Trigger retraining** | Jika PR-AUC turun > 5% dari baseline |
| **Monitor concept drift** | Distribusi fitur Tenure dan CashbackAmount |
| **Minimum data baru** | 1.000 transaksi baru sebelum retraining |

---

## 🗂️ Struktur Repository

```
📦 shopnest-churn-prediction
├── 📓 Prediksi_Customer_Churn_...ShopNest.ipynb   # Notebook utama
├── 🐍 app.py                                       # Streamlit web app
├── 🤖 lgbm_best_model.pkl                          # Saved model
├── 📄 requirements.txt                             # Dependencies
└── 📘 README.md                                    # Dokumentasi ini
```

---

## ⚙️ Cara Menjalankan Aplikasi

```bash
# 1. Clone repository
git clone https://github.com/username/shopnest-churn-prediction.git
cd shopnest-churn-prediction

# 2. Install dependencies
pip install -r requirements.txt

# 3. Jalankan Streamlit app
streamlit run app.py
```

---

## 🛠️ Tech Stack

| Library | Versi | Kegunaan |
|---------|-------|----------|
| `scikit-learn` | 1.7.2 | Preprocessing & pipeline |
| `lightgbm` | 4.6.0 | Model utama |
| `imbalanced-learn` | 0.14.0 | Random OverSampling |
| `pandas` | 2.3.3 | Data manipulation |
| `numpy` | 2.3.5 | Komputasi numerik |
| `joblib` | 1.5.2 | Save/load model |
| `shap` | latest | Model explainability |
| `streamlit` | latest | Web app deployment |

---

> *"Dengan model ini, ShopNest beralih dari reactive CRM ke proaktif — mengidentifikasi siapa yang akan pergi, sebelum mereka benar-benar pergi."*

Link Streamlit : 
https://prediksicustomerchurnmenggunakanmachinelearningpadae-commerces1.streamlit.app/
