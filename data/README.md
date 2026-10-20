# Data

Tiga file input untuk `pedas2025_carsale.ipynb`. Nama file di sini sengaja disederhanakan (tanpa spasi/tanda kurung) agar URL raw GitHub stabil; isinya **tidak diubah** dari file asli yang diberikan.

| File | Isi | Rentang | Baris |
|---|---|---|---|
| `car_sales_2021_2025.csv` | Penjualan mobil bulanan (unit) per merek: DAIHATSU, HONDA, MITSUBISHI, SUZUKI, TOYOTA. Kolom `waktu` = tanggal awal bulan (`YYYY-MM-01`) | Jan 2021 – Jul 2025 | 54 (satu bulan hilang, lihat di bawah) |
| `bi_7day_rr.xlsx` | Suku bunga BI 7-Day Reverse Repo Rate per rapat Dewan Gubernur (tanggal dalam bahasa Indonesia, nilai berformat teks `5.25 %`). Tabel dimulai pada baris judul "NO / Tanggal / BI-7Day-RR" | 21 Jan 2021 – 16 Jul 2025 | 55 (satu rapat per bulan) |
| `inflasi_ihk_ytd_2022_100.csv` | Inflasi harga konsumen nasional **tahun kalender (Y-to-D)**, tahun dasar 2022=100, format lebar (baris = bulan, kolom = tahun 2006–2025) | s.d. Jul 2025 (Agu–Des 2025 kosong) | 12 bulan × 20 tahun |

## Catatan penting (sudah diperiksa di notebook)
- **Des 2023 tidak ada** di `car_sales_2021_2025.csv`. Notebook membuatnya NaN eksplisit dan tidak mengimputasi.
- **Patahan Jan 2024.** Rata-rata Jan–Jun 2024 dibanding Jun–Nov 2023: Honda 0,37×, Suzuki 0,47×, Toyota 0,46×, Mitsubishi 0,71×, Daihatsu 0,83×. Seluruh nilai 2021–2023 bilangan bulat, sedangkan 2024–2025 banyak yang pecahan.
- **IHK adalah YTD kumulatif** (perubahan terhadap Desember tahun sebelumnya, direset tiap Januari). Notebook mengubahnya menjadi inflasi bulanan: `((1+YTD_t)/(1+YTD_{t-1}) - 1)·100`, dengan Januari = nilai YTD-nya. Catatan di file asli menyebut perubahan tahun dasar/pola konsumsi (2008, 2014, 2020, 2024), yang tidak disesuaikan.
- **BI**: nilai bulanan diambil dari rapat bulan itu (level setelah rapat). Notebook memakainya dengan lag 1 bulan; IHK dengan lag 2 bulan.
