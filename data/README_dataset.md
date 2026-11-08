# Dokumentasi Sumber Dataset — AI4I 2020 Predictive Maintenance

- **Nama:** AI4I 2020 Predictive Maintenance Dataset
- **Pembuat:** Stephan Matzka, Hochschule für Technik und Wirtschaft Berlin
- **Sumber resmi:** UCI Machine Learning Repository, id 601 — https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset
- **DOI dataset:** 10.24432/C5HS5C
- **Mirror:** Kaggle — https://www.kaggle.com/datasets/stephanmatzka/predictive-maintenance-dataset-ai4i-2020
- **Lisensi:** Creative Commons Attribution 4.0 (CC BY 4.0) — boleh didistribusikan ulang dengan atribusi
- **Publikasi rujukan:** S. Matzka, "Explainable artificial intelligence for predictive maintenance applications," in *Proc. 3rd Int. Conf. Artif. Intell. Ind. (AI4I)*, 2020, pp. 69–74, doi: 10.1109/AI4I49448.2020.00023.

## Cara memperoleh
File `ai4i2020.csv` sudah disertakan di folder ini. Alternatif: unduh dari tautan UCI di atas, atau biarkan notebook
mengunduh otomatis melalui paket `ucimlrepo` (`fetch_ucirepo(id=601)`) jika file tidak ditemukan.

## Karakteristik
- 10.000 baris (1 baris = 1 siklus proses pemesinan), 14 kolom, **data sintetis**.
- Tidak ada nilai hilang maupun baris duplikat.
- 339 siklus gagal (3,39%).

| Kolom | Arti | Satuan / tipe | Peran di proyek |
|---|---|---|---|
| UDI | nomor baris | int | dibuang (pengenal) |
| Product ID | kode produk (L/M/H + nomor seri) | teks | dibuang (pengenal) |
| Type | varian kualitas produk: L 50%, M 30%, H 10% | kategorikal | fitur (ordinal L=0, M=1, H=2) |
| Air temperature [K] | suhu udara | K | fitur |
| Process temperature [K] | suhu proses | K | fitur |
| Rotational speed [rpm] | kecepatan putar | rpm | fitur |
| Torque [Nm] | torsi | Nm | fitur |
| Tool wear [min] | keausan pahat | menit | fitur |
| Machine failure | 1 jika siklus gagal | biner | label evaluasi |
| TWF, HDF, PWF, OSF, RNF | mode kegagalan: tool wear, heat dissipation, power, overstrain, random | biner | hanya analisis galat (dikeluarkan dari fitur) |

## Catatan kualitas
27 baris berlabel tidak konsisten: 9 baris `Machine failure = 1` tanpa mode aktif, dan 18 baris RNF dengan
`Machine failure = 0`. Pada Milestone 1 label dipakai apa adanya (lihat laporan Subbab 2.4).
