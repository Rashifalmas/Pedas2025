# Forecasting Penjualan Mobil Bulanan Indonesia

> Hasil di bawah berasal dari eksperimen historis; data berakhir Juli 2025. Prakiraan Agustus 2025 adalah hasil historis.

Peramalan penjualan bulanan lima merek (Daihatsu, Honda, Mitsubishi, Suzuki, Toyota), Jan 2021 – Jul 2025, dengan fokus pada validitas evaluasi: validasi rolling-origin, baseline sederhana, uji kebocoran, dan pengujian terkontrol kontribusi BI-7DRR dan IHK.

## Ringkasan hasil (rolling-origin, horizon 1 bulan, 23 bulan uji × 5 merek = 115 prakiraan)

| Model | sMAPE % | MAE (unit) |
|---|---|---|
| Combo (MA3 + SES + Ridge pooled) | 16,43 | 1.382 |
| Ridge pooled, fitur: dinamika sendiri | 16,58 | 1.384 |
| MA3 (rata-rata 3 bulan) | 16,65 | 1.403 |
| SES | 17,76 | 1.534 |
| Naive (nilai bulan lalu) | 18,36 | 1.481 |
| Ridge + BI-7DRR + IHK (tersedia saat prediksi) | 18,89 | 1.594 |

Tabel lengkap (13 model, periode A/B terpisah, per merek, ablasi) ada di notebook bagian 7.

**Kesimpulan yang didukung data**
- Tidak ada model yang jelas mengalahkan baseline sederhana. `Combo` vs `MA3`: selisih −0,22 ± 0,37 poin sMAPE (menang 15 dari 23 bulan), tidak bermakna. `Combo` ditambahkan setelah putaran eksperimen pertama, jadi posisinya optimistis.
- BI-7DRR dan IHK **tidak meningkatkan** akurasi pada data sepanjang ini (bahkan varian *oracle* yang memakai nilai bulan berjalan tidak membaik). Ini bukan bukti bahwa keduanya tidak berpengaruh.
- Model LightGBM pada notebook lama (`izinserius.ipynb`) ternyata tidak memakai fitur apa pun (1 pohon, 0 split) dan setara dengan rata-rata ekspansif (sMAPE 16,34% pada periode uji B).
- Honda paling sulit diprediksi (sMAPE 22–30%).

## Validasi yang dilakukan
- **Uji perturbasi kebocoran:** data bulan `t` dan sesudahnya dirusak; prakiraan bulan `t` tidak berubah pada semua model sah (model *oracle* berubah, sehingga tes terbukti sensitif).
- **Metrik dihitung ulang secara independen** dan himpunan prakiraan identik di semua model.
- **Reprodusibel:** hasil identik pada dua kombinasi versi pustaka (lihat `requirements.txt`) dan pada pembacaan data lewat `file://` maupun HTTP.
- **Interval 80%** dari galat backtest `Combo` dikalibrasi: cakupan leave-one-out keseluruhan 79%, tetapi Honda hanya 65% (terlalu sempit) dan Daihatsu 87%. Perlakukan sebagai indikatif.

## Keterbatasan
- **Des 2023 hilang** dari data penjualan.
- Hanya 54 bulan; ±13 model dibandingkan pada 23 bulan uji yang sama sehingga peringkat bisa optimistis.

## Struktur repository
```
.
├── README.md
├── requirements.txt
├── pedas2025_carsale.ipynb
└── data/
    ├── README.md
    ├── car_sales_2021_2025.csv
    ├── bi_7day_rr.xlsx
    └── inflasi_ihk_ytd_2022_100.csv
└── data-original-name/
    ├── dataCarSale2021-2025.csv
    ├── BI-7Day-RR.xlsx
    └── Tingkat Inflasi Harga Konsumen Nasional Tahun Kalender (Y-to-D)  (2022=100).csv
└── notebook/
    ├── izinserius.ipynb
    └── pedas2025_carsale.ipynb
```