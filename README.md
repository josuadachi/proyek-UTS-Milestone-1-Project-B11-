# Deteksi Anomali Kondisi & Parameter Proses Produksi (AI4I 2020) — UTS Milestone 1

| | |
|---|---|
| **Project ID** | B11 |
| **Nama kelompok** | Kelompok 12 |
| **Anggota** | Josua Christian Dachi "231112624" · Andrew Samuel "231112435"| 

## Deskripsi masalah
Kegagalan mesin pada proses pemesinan (pahat aus/patah, panas tidak terbuang, daya di luar batas, beban berlebih)
menyebabkan *downtime* dan produk cacat, tetapi kejadiannya **langka** (3,39% siklus) dan **mahal untuk diberi label**.
Proyek ini merumuskan masalahnya sebagai **deteksi anomali**: model mempelajari kondisi proses normal dari parameter
proses (suhu udara, suhu proses, kecepatan putar, torsi, keausan pahat, tipe produk) lalu memberi **skor anomali per siklus**.
Label `machine_failure` hanya dipakai untuk evaluasi, untuk memilih data normal pada skenario semi-supervised, dan untuk
menetapkan proporsi alarm.

Pertanyaan yang dijawab pada Milestone 1:
- **RQ1:** apakah fitur turunan berbasis domain (selisih suhu, daya, keausan × torsi) meningkatkan deteksi?
- **RQ2:** apakah melatih hanya pada data normal lebih baik daripada melatih pada semua data tanpa label?
- **RQ3:** jenis kegagalan mana yang sulit dideteksi?

Metode: *classical machine learning* (scikit-learn) — 2 baseline (acak, Z-score maks) dan 6 kandidat
(Mahalanobis, PCA reconstruction, Isolation Forest, LOF, One-Class SVM, autoencoder MLP), evaluasi hold-out
stratified 60/20/20 pada 5 seed dengan metrik utama PR-AUC.

## Struktur repositori
```
├── UTS_Milestone1_Project_B11.ipynb   # notebook eksperimen (sudah dijalankan, output tersimpan)
├── Laporan_UTS_Milestone1_B11.pdf                       # laporan Milestone 1
├── requirements.txt
├── data/
│   ├── ai4i2020.csv                                     # dataset (CC BY 4.0)
│   └── README_dataset.md                                # dokumentasi sumber & kolom dataset
└── outputs/
    ├── data_preprocessed.csv               # data + fitur turunan
    ├── tabel_hasil_eksperimen_awal.csv     # tabel utama laporan (baseline + kandidat, rata-rata ± std 5 seed)
    ├── hasil_baseline_ringkas.csv          # baseline per fitur × skenario
    ├── hasil_perbandingan_model.csv        # semua model × fitur × skenario × seed (data mentah)
    ├── pr_auc_uji_per_konfigurasi.csv      # pivot PR-AUC uji
    ├── pemilihan_konfigurasi_validasi.csv  # PR-AUC validasi per konfigurasi
    ├── ablasi_rq1_rq2.csv                  # E1: ΔPR-AUC ENG−RAW dan semi−unsup
    ├── hasil_tuning.csv, ringkasan_tuning.csv  # E2: penyetelan awal
    ├── hasil_kandidat_akhir.csv            # E3: model tersetel + ensemble per seed
    ├── recall_per_mode.csv, recall_per_tipe.csv  # E4: analisis galat
    ├── kepentingan_fitur.csv               # E5: permutation importance
    ├── ringkasan_model_akhir.json          # kandidat terbaik sementara (bukan model final)
    └── figures/*.png                       # semua grafik di laporan
```

## Dataset
AI4I 2020 Predictive Maintenance Dataset (Matzka, 2020), 10.000 baris × 14 kolom, data sintetis.
- UCI: https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset (doi:10.24432/C5HS5C)
- Kaggle: https://www.kaggle.com/datasets/stephanmatzka/predictive-maintenance-dataset-ai4i-2020
- Lisensi: CC BY 4.0 (boleh didistribusikan ulang dengan atribusi).

Letakkan file sebagai `ai4i2020.csv` atau `data/ai4i2020.csv`. Jika tidak ada, notebook mencoba
mengunduh lewat `ucimlrepo` (id 601).

## Cara menjalankan
```bash
pip install -r requirements.txt
jupyter notebook UTS_Milestone1_Project_B11.ipynb   # lalu Restart & Run All
# atau tanpa antarmuka:
jupyter nbconvert --to notebook --execute --inplace UTS_Milestone1_Project_B11.ipynb
```
Waktu jalan ±2–3 menit pada CPU 2 inti. Semua seed tetap (42–46) sehingga hasil deterministik;
menjalankan ulang menghasilkan angka yang sama persis dengan yang dilaporkan.

## Ringkasan hasil Milestone 1 (data uji, rata-rata ± std 5 seed, fitur ENG, latih hanya data normal)
| Model | PR-AUC | ROC-AUC | F1 |
|---|---|---|---|
| Acak (baseline) | 0,042 ± 0,011 | 0,493 | 0,050 |
| Z-score maks (baseline) | 0,310 ± 0,079 | 0,860 | 0,376 |
| LOF (bawaan, k=35) | 0,464 ± 0,056 | 0,901 | 0,470 |
| One-Class SVM (setelan awal) | 0,466 ± 0,071 | 0,893 | 0,455 |
| **Ensemble top-2 (OCSVM + LOF)** — kandidat terbaik sementara | **0,486 ± 0,066** | 0,909 | 0,462 |

Rincian, keterbatasan, rencana menuju UAS, dan 37 referensi (format IEEE) ada di laporan dan bagian 6 notebook.
