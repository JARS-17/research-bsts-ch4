# Materi Presentasi Lengkap — Bimbingan II
### Proyeksi Emisi CH4 Indonesia dengan Bayesian Structural Time Series

Dokumen ini adalah pelengkap dari file PPTX — berisi naskah/poin bicara untuk tiap slide, tabel literatur lengkap, dan antisipasi pertanyaan dari pembimbing II. Struktur mengikuti nomor slide di PPTX agar mudah disandingkan.

---

## Slide 1 — Pembuka

**Poin bicara:**
- Perkenalkan topik dalam satu kalimat: penelitian ini memproyeksikan total emisi CH4 Indonesia menggunakan Bayesian Structural Time Series (BSTS), dibandingkan dengan Prophet dan SARIMAX, dengan data dari EDGAR.
- Sampaikan tujuan pertemuan hari ini: menyelaraskan metode, menunjukkan hasil kajian literatur, dan meminta arahan pembimbing II sebelum masuk ke penulisan bab metodologi.

---

## Slide 2 — Apa itu BSTS

**Naskah penjelasan:**

BSTS adalah model *state space* Bayesian yang menguraikan deret waktu menjadi dua persamaan: *observation equation* (menghubungkan data yang diamati dengan variabel state laten) dan *state equation* (menjelaskan bagaimana state itu berevolusi antar waktu).

Tiga komponen utama yang dipakai dalam penelitian ini:
- **Trend (local linear trend)** — menangkap arah pergerakan jangka panjang emisi CH4, termasuk perubahan kemiringan tren dari waktu ke waktu.
- **Seasonal** — komponen musiman berulang; tidak dipakai di model utama karena data EDGAR bersifat tahunan, tapi disebutkan sebagai opsi jika ada perluasan ke data bulanan.
- **Regresi dengan spike-and-slab prior** — memasukkan prediktor eksternal (PDB, populasi, energi fosil, luas panen), dengan prior yang secara otomatis menyeleksi variabel mana yang benar-benar relevan (mengurangi risiko overfitting pada data yang pendek).

Estimasi dilakukan lewat dua mesin komputasi: **Kalman filter** untuk mengestimasi state secara rekursif, dan **MCMC** untuk menghasilkan distribusi posterior penuh — sehingga hasil akhirnya bukan angka tunggal, melainkan interval kredibel yang mencerminkan ketidakpastian.

**Kenapa BSTS dipilih (siap dijawab kalau ditanya):** interpretable secara desain (bukan black-box), cocok untuk data pendek karena spike-and-slab mengontrol kompleksitas model, dan secara native menghasilkan kuantifikasi ketidakpastian Bayesian — sesuatu yang tidak dimiliki ARIMA/LSTM secara langsung.

---

## Slide 3 — Alur Penggunaan & Penerapan pada CH4 EDGAR

**Naskah penjelasan per tahap:**

1. **Data & Prediktor** — total emisi CH4 Indonesia diagregasi dari data sektoral EDGAR menjadi satu deret waktu tahunan (~50 titik data), digabung dengan 3–5 prediktor nasional yang dipilih berdasarkan relevansi teoretis (PDB, populasi, konsumsi energi fosil, luas panen padi).
2. **Spesifikasi model** — local linear trend + komponen regresi dengan spike-and-slab prior. Prediktor sengaja dibatasi 3–5 saja agar seleksi variabel tetap bermakna pada data yang pendek.
3. **Estimasi** — Kalman filter untuk state, MCMC (ribuan iterasi, dengan burn-in period yang dibuang) untuk posterior.
4. **Forecast & interpretasi** — proyeksi 10 tahun ke depan dengan interval kredibel 95%, plus dekomposisi komponen (trend vs kontribusi regresi) dan posterior inclusion probability tiap prediktor.

**Rasionalisasi fokus top-down (total, bukan per sektor):**
Dengan data tahunan yang pendek (~50 titik), model per sektor berisiko tidak stabil dan sulit divalidasi. Pendekatan top-down menjaga kestabilan model. Kontribusi sektor tetap dilaporkan, tapi secara deskriptif (porsi terhadap total, kontribusi terhadap perubahan) — bukan sebagai prediktor dalam model, karena itu akan membuat model sirkular (total dijelaskan oleh bagian dari dirinya sendiri).

---

## Slide 4 — Model Pembanding: BSTS vs Prophet vs SARIMAX

**Karakteristik tiap model:**

| Model | Mekanisme | Kekuatan | Keterbatasan untuk kasus ini |
|---|---|---|---|
| **BSTS** | State space Bayesian + spike-and-slab | Interval kredibel, seleksi variabel otomatis, interpretable | Butuh spesifikasi prior yang hati-hati |
| **Prophet** | Tren piecewise-linear + regresor + (opsional) musiman | Robust terhadap outlier, mudah dipakai | Didesain untuk data dengan musiman kuat; kurang optimal untuk data tahunan pendek |
| **SARIMAX** | ARIMA + variabel eksogen | Baseline klasik yang sudah teruji lama | Pada data tahunan efektif menjadi ARIMAX (komponen musiman tidak relevan) |

**Desain evaluasi:**
- **Holdout** — 5 tahun terakhir disisihkan sebagai data uji; prediktor pada periode holdout memakai nilai aktual (*conditional forecast*) supaya perbandingan antar model murni mengukur kemampuan model, bukan tercampur ketidakpastian prediktor.
- **Rolling-origin** — evaluasi berulang dengan titik awal training yang bergeser, untuk memastikan hasil tidak kebetulan cocok di satu potongan data saja.
- **Metrik**: RMSE dan MAE (akurasi titik), MAPE (akurasi relatif), *coverage* interval 95% (apakah nilai aktual benar-benar jatuh dalam interval prediksi sesering yang dijanjikan), dan lebar interval (seberapa informatif intervalnya — interval yang terlalu lebar kurang berguna meski coverage-nya bagus).

---

## Slide 5 — Peta Literatur (17 Sumber Terindeks)

### A. Fondasi BSTS, Spike-and-Slab, Causal Impact

| No | Penulis & Tahun | Judul | Sumber | DOI |
|---|---|---|---|---|
| 1 | Scott & Varian (2013/2014) | Predicting the Present with Bayesian Structural Time Series | IJMMNO, 5(1/2), 4–23 | [10.2139/ssrn.2304426](https://doi.org/10.2139/ssrn.2304426) |
| 2 | Brodersen et al. (2015) | Inferring Causal Impact using Bayesian Structural Time-Series Models | Annals of Applied Statistics, 9(1), 247–274 | [10.1214/14-AOAS788](https://doi.org/10.1214/14-AOAS788) |
| 3 | Ishwaran & Rao (2005) | Spike and Slab Variable Selection: Frequentist and Bayesian Strategies | Annals of Statistics, 33(2), 730–773 | [10.1214/009053604000001147](https://doi.org/10.1214/009053604000001147) |
| 4 | Andrews & Kimpton (2023) | Econometric Forecasting of Tourist Arrivals Using BSTS | Economic Papers, 42(2), 200–211 | [10.1111/1759-3441.12383](https://doi.org/10.1111/1759-3441.12383) |

### B. Basis Data EDGAR & Inventarisasi Emisi

| No | Penulis & Tahun | Judul | Sumber | DOI |
|---|---|---|---|---|
| 5 | Crippa et al. (2018) | Gridded Emissions of Air Pollutants within EDGAR v4.3.2 | ESSD, 10, 1987–2013 | [10.5194/essd-10-1987-2018](https://doi.org/10.5194/essd-10-1987-2018) |
| 6 | Janssens-Maenhout et al. (2019) | EDGAR v4.3.2 Global Atlas of GHG Emissions 1970–2012 | ESSD, 11, 959–1002 | [10.5194/essd-11-959-2019](https://doi.org/10.5194/essd-11-959-2019) |
| 7 | Solazzo et al. (2021) | Uncertainties in the EDGAR Emission Inventory of GHG | ACP, 21, 5655–5675 | [10.5194/acp-21-5655-2021](https://doi.org/10.5194/acp-21-5655-2021) |
| 8 | Crippa et al. (2024) | Spatial Distribution of Global, National, Subnational GHG in EDGAR v8.0 | ESSD, 16, 2811–2830 | [10.5194/essd-16-2811-2024](https://doi.org/10.5194/essd-16-2811-2024) |

### C. Forecasting Emisi GRK — Pembanding Metode & Konteks Indonesia

| No | Penulis & Tahun | Judul | Sumber | DOI |
|---|---|---|---|---|
| 9 | Saputra et al. (2025) | Time Series Model with LSTM for GHG Estimation in Indonesia | BAREKENG, 19(2), 949–960 | [10.30598/barekengvol19iss2pp949-960](https://doi.org/10.30598/barekengvol19iss2pp949-960) |
| 10 | Dhika et al. (2025) | Hybrid ARIMA-XGBoost untuk Prediksi CO2 di ASEAN | IJAIDM, 8(3) | [10.24014/ijaidm.v8i3.36685](https://doi.org/10.24014/ijaidm.v8i3.36685) |
| 11 | Herawati et al. (2024) | Hybrid Predictive Model — Emission Reduction Power Plants | IEEE Access, 12, 47119–47133 | [10.1109/ACCESS.2024.3380809](https://doi.org/10.1109/ACCESS.2024.3380809) |
| 12 | Hermawan (2009) | Climate Projection over Indonesia — Box-Jenkins ARIMA | Agromet (IPB), 23(1), 1–10 | [10.29244/j.agromet.23.1.1-10](https://doi.org/10.29244/j.agromet.23.1.1-10) |
| 13 | Kour (2023) | Modelling and Forecasting CO2 Emissions in South Africa — ARIMA | Int. J. Environ. Sci. Technol., 20(10), 11267–11274 | [10.1007/s13762-022-04609-7](https://doi.org/10.1007/s13762-022-04609-7) |
| 14 | Emami Javanmard & Ghaderi (2022) | Hybrid ML + Optimization Model to Forecast GHG Emissions | Sustainable Cities and Society, 82, 103886 | [10.1016/j.scs.2022.103886](https://doi.org/10.1016/j.scs.2022.103886) |

### D. Explainable AI (XAI) pada Prediksi Emisi

| No | Penulis & Tahun | Judul | Sumber | DOI |
|---|---|---|---|---|
| 15 | Aras (2023) | Interpretable Forecasting Framework for Energy & CO2 (SHAP) | Applied Energy, 328, 120163 | [10.1016/j.apenergy.2022.120163](https://doi.org/10.1016/j.apenergy.2022.120163) |
| 16 | Sholehurrohman et al. (2026) | Prediction of Carbon Emissions from Vehicles — Interpretable NN | Bulletin of EEI, 15(4), 3587–3597 | [10.11591/eei.v15i4.12064](https://doi.org/10.11591/eei.v15i4.12064) |
| 17 | Ma, Li, Kovshov (2025) | Deep Learning and XAI for CO2 Emissions — MLP + SHAP | RIMNI | [10.23967/j.rimni.2025.10.76077](https://doi.org/10.23967/j.rimni.2025.10.76077) |

*(Status indeksasi beberapa entri masih perlu verifikasi mandiri — lihat catatan di dokumen literatur sebelumnya.)*

---

## Slide 6 — Gap Analysis

**Yang sudah banyak dikerjakan (state of the art):**
- Forecasting emisi Indonesia dengan ARIMA/LSTM/hybrid ML (#9–13) — fokus akurasi titik.
- XAI/SHAP untuk emisi CO2 (#15–17) — tapi selalu pada model black-box.
- BSTS sudah mapan secara metodologis (#1–4) — terbukti unggul di domain lain (pariwisata, ekonomi), belum pernah dipakai untuk emisi CH4.

**Empat gap yang ditemukan:**

1. **Belum ada BSTS untuk CH4 Indonesia berbasis EDGAR.** Semua studi forecasting emisi Indonesia yang ditemukan memakai ARIMA, LSTM, atau hybrid ML.
2. **Ketidakpastian prediksi jarang dikuantifikasi secara Bayesian.** Studi pembanding melaporkan RMSE/MAPE tanpa interval kredibel yang principled — padahal untuk keperluan kebijakan, rentang ketidakpastian sama pentingnya dengan titik prediksi.
3. **XAI belum disandingkan dengan model interpretable-by-design.** SHAP selalu diterapkan pada neural network/XGBoost; posterior inclusion probability dari BSTS (yang sudah interpretable secara bawaan) belum pernah dibandingkan atau dikombinasikan dengan SHAP dalam konteks emisi.
4. **Belum ada perbandingan sistematis BSTS vs Prophet vs SARIMAX** khusus untuk data emisi GRK Indonesia.

---

## Slide 7 — Posisi & Kontribusi Penelitian

**Pernyataan posisi (bisa dibacakan langsung):**

> "Penelitian ini menerapkan Bayesian Structural Time Series (dengan spike-and-slab prior) untuk memproyeksikan total emisi CH4 Indonesia berbasis data EDGAR, dibandingkan dengan Prophet dan SARIMAX, serta menambahkan lapisan interpretasi (posterior inclusion probability dan/atau SHAP) yang belum pernah disandingkan dalam konteks ini — sekaligus menyediakan interval kredibel yang lebih transparan dibanding pendekatan ARIMA/LSTM yang selama ini dominan pada literatur emisi Indonesia."

**Kontribusi teoretis:** memperluas penerapan BSTS ke domain emisi gas rumah kaca yang sebelumnya belum tereksplorasi, sekaligus menguji apakah interpretability bawaan BSTS bisa disandingkan secara sistematis dengan XAI post-hoc seperti SHAP.

**Kontribusi praktis:** menyediakan baseline proyeksi CH4 Indonesia dengan ketidakpastian yang transparan — berguna sebagai pembanding independen bagi penyusun inventori nasional, peneliti, dan pemangku kebijakan iklim.

---

## Slide 8 — Untuk Didiskusikan dengan Pembimbing II

**Poin yang perlu konfirmasi:**
1. Cakupan model (total vs breakdown sektor) — sudah dijelaskan rasionalisasinya, minta persetujuan.
2. Validasi status Scopus/Sinta final untuk 3 entri literatur yang masih ditandai "perlu cek".
3. Timeline penulisan bab metodologi dan bab penerapan data.

---

## Lampiran: Antisipasi Pertanyaan (Q&A)

**Q: Kenapa pilih BSTS, bukan LSTM atau Prophet yang lebih populer?**
A: LSTM butuh data lebih banyak dan sulit diinterpretasi (black-box); Prophet didesain untuk data dengan musiman kuat dan kurang optimal untuk data tahunan pendek. BSTS unggul untuk data pendek (spike-and-slab mengontrol kompleksitas), dan secara native menghasilkan interval kredibel serta interpretasi variabel — dua hal yang jadi fokus kontribusi penelitian ini.

**Q: Seberapa valid data EDGAR sebagai ground truth?**
A: EDGAR adalah inventori hasil estimasi (bottom-up dari data aktivitas + faktor emisi), bukan pengukuran atmosfer langsung — jadi penelitian ini memodelkan estimasi, bukan realitas fisik secara langsung. Ini akan disebutkan eksplisit sebagai limitasi, dan idealnya ditriangulasi dengan inventori nasional atau data satelit jika waktu memungkinkan.

**Q: Kenapa fokus ke total emisi, bukan per sektor?**
A: Data tahunan EDGAR untuk Indonesia hanya sekitar 50 titik. Model per sektor (8–16 sektor) akan memecah data itu jadi lebih sedikit lagi dan berisiko tidak stabil, terutama untuk sektor kecil. Pendekatan top-down menjaga kestabilan model; kontribusi sektor tetap dilaporkan secara deskriptif.

**Q: Apa yang membedakan penelitian ini dari studi Saputra et al. (2025) yang juga memakai data EDGAR untuk Indonesia?**
A: Studi tersebut memakai LSTM dan fokus pada akurasi prediksi per sektor tanpa kuantifikasi ketidakpastian Bayesian atau interpretasi variabel yang principled. Penelitian ini menawarkan pendekatan Bayesian dengan interval kredibel eksplisit dan interpretasi driver lewat posterior inclusion probability/SHAP — sudut yang belum dieksplorasi studi tersebut.

**Q: Bagaimana rencana validasi model?**
A: Evaluasi holdout (5 tahun terakhir) dan rolling-origin, dibandingkan dengan Prophet dan SARIMAX sebagai baseline, menggunakan RMSE, MAE, MAPE, coverage interval, dan lebar interval. Jika memungkinkan, hasil proyeksi juga akan dibandingkan dengan proyeksi resmi nasional (dokumen NDC/inventori) sebagai sanity check tambahan.
