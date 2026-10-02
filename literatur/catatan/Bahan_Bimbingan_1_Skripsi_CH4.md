# BIMBINGAN 1 TUGAS AKHIR (SKRIPSI)
### Prediksi Emisi Gas Metana (CH4) Sektoral Indonesia dengan Bayesian Structural Time Series (BSTS)

---

## 1. Identitas

| Item | Keterangan |
|---|---|
| Nama | Ginda Fajar Riadi Marpaung (JARS) |
| NIM | _123450103_ |
| Program Studi | Sains Data, Institut Teknologi Sumatera (ITERA) |
| Pembimbing I (ITERA) | _Rian, M.Si._|
| Pembimbing II (BRIN) | _Rumadi, S.T., M.T._ |
| Judul sementara | Prediksi Emisi Gas Metana (CH4) Sektoral di Indonesia Menggunakan *Bayesian Structural Time Series* (BSTS) |

---

## 2. Latar Belakang

1. Metana (CH4) adalah gas rumah kaca dengan potensi pemanasan jangka pendek yang jauh lebih kuat daripada CO2, sehingga proyeksi emisinya penting untuk perencanaan mitigasi iklim di Indonesia.
2. Berdasarkan data yang sudah dikumpulkan (1970–2024), total emisi CH4 dari 8 sektor di Indonesia naik sekitar **406%** dari 1970 ke 2024, dan emisi CH4 per kapita naik dari **0,739** menjadi **1,52** (naik ±106%).
3. Komposisi sektoral CH4 ternyata berbeda dari pola CO2 pada umumnya: pada 2024, sektor **Fuel Exploitation** (eksplorasi/eksploitasi bahan bakar) mendominasi dengan **±66%** dari total emisi sektoral, jauh di atas **Agriculture (±21%)** dan **Waste (±13%)** — pola ini konsisten dengan karakteristik CH4 yang banyak berasal dari kebocoran fugitif migas, peternakan, dan pengelolaan sampah, bukan dari pembakaran industri/transportasi seperti pada CO2.
4. Pendekatan *Bayesian Structural Time Series* (BSTS) mendekomposisi deret waktu secara eksplisit ke komponen trend/level/slope dan memberikan kuantifikasi ketidakpastian lewat distribusi posterior (interval kredibel) — cocok untuk data tahunan yang relatif pendek (55 observasi) dan berpotensi mengandung perubahan struktural, sebagaimana ditunjukkan pada jurnal acuan utama (poin 5).
5. Riset ini melanjutkan pengalaman riset sebelumnya (perbandingan varian LSTM/BiLSTM untuk prediksi PM2.5 di Bandung), namun kali ini beralih ke pendekatan Bayesian time series untuk mengeksplorasi kuantifikasi ketidakpastian (interval kredibel) yang tidak diberikan oleh model *deep learning* point-forecast sebelumnya.

---

## 3. Rumusan Masalah

1. Bagaimana pola dan tren emisi CH4 sektoral Indonesia periode 1970–2024, dan sektor mana yang paling dominan serta paling cepat pertumbuhannya?
2. Bagaimana performa model BSTS (dengan berbagai pilihan komponen *state*) dalam memprediksi emisi CH4 pada masing-masing sektor tersebut?
3. Bagaimana proyeksi emisi CH4 Indonesia untuk beberapa tahun ke depan berdasarkan model yang dibangun, beserta interval kredibelnya, dan apa implikasinya terhadap kebijakan mitigasi?

---

## 4. Dataset

| Item | Keterangan |
|---|---|
| Nama file | `CH4-emissions-by-sector.xls` (format HTML/Highchart export) |
| Sumber | _(perlu dikonfirmasi — kemungkinan EDGAR/Climate Watch, sama seperti pola data pada jurnal acuan)_ |
| Periode | 1970–2024 (tahunan, **55 observasi**) |
| Dimensi | 55 baris × 9 kolom (1 kolom tahun + 8 sektor) + 1 kolom turunan "Total CH4/cap" |
| Sektor (variabel) | Agriculture, Buildings, Fuel Exploitation, Industrial Combustion, Power Industry, Processes, Transport, Waste |
| Target | Emisi CH4 per sektor (dan/atau agregat), untuk diprediksi ke depan |
| Missing value | Tidak ada (lengkap 1970–2024) |
| Satuan | **Belum tercantum eksplisit di file — perlu ditanyakan ke pembimbing** (dugaan: ton/kt CH4 atau CO2-eq, kolom "Total CH4/cap" tampak dalam ton per kapita) |

**Contoh data (baris pertama & terakhir):**

| Tahun | Agriculture | Buildings | Fuel Exploitation | Industrial Combustion | Power Industry | Processes | Transport | Waste | Total CH4/cap |
|---|---|---|---|---|---|---|---|---|---|
| 1970 | 48.114.500 | 5.174.310 | 26.132.100 | 11.705 | 2.254 | 565 | 60.551 | 5.381.410 | 0,739 |
| 2024 | 88.398.900 | 1.053.620 | 283.200.000 | 749.896 | 457.765 | 350.627 | 915.988 | 54.408.300 | 1,52 |

**Insight awal dari eksplorasi data:**
- Sektor dengan kontribusi terbesar 2024: Fuel Exploitation (±66%) → Agriculture (±21%) → Waste (±13%) → sisanya < 1% masing-masing.
- Pertumbuhan tercepat (dari basis kecil): Power Industry naik >20.000%, Processes >60.000%, Industrial Combustion >6.000% — sektor-sektor ini kecil secara absolut tapi sangat volatil, relevan untuk pemilihan komponen *state* BSTS per sektor (lihat poin 6–7).
- Sektor Buildings justru **menurun** ±80% dari 1970 ke 2024 — pola berlawanan dari sektor lain.

---

## 5. Jurnal Acuan

### 5.1 Acuan Utama
**Osman, Mengistie, Adawe, Takele, & Marine (2026).** *A Comparative Approach to Analyzing and Forecasting Carbon Dioxide Emissions in Ethiopia Using Bayesian Autoregressive Integrated Moving Average (ARIMA) and Bayesian Structural Time Series (BSTS) Models.* Carbon Balance and Management.

| Aspek | Ringkasan |
|---|---|
| Masalah | Membandingkan Bayesian ARIMA vs BSTS untuk memprediksi emisi CO2 per kapita Ethiopia 1941–2022 (82 observasi tahunan, satu seri nasional) |
| Metode BSTS | Dekomposisi ke komponen *local level*, *local linear trend*, *semi-local linear trend*, *Student local linear trend*, dan *autoregressive*; prior *weakly-informative* pada varians (σ_obs, σ_level, σ_slope); estimasi via MCMC (Stan/paket `bsts` di R); evaluasi dengan WAIC, LOOIC, R², residual.sd, prediction.sd |
| Hasil BSTS | R² sangat tinggi (99,39%–99,78%, terbaik pada *Student Local Linear Trend*, 500 iterasi MCMC), namun diagnostik konvergensi kurang baik: Rhat > 1,01 pada semua parameter (s_obs = 1,154; s_level = 1,053; s_slope = 1,032), dengan *effective sample size* s_obs hanya 26 |
| **Catatan penting** | Pada perbandingan akhir, **Bayesian ARIMA(0,1,1) justru dipilih sebagai model terbaik**, bukan BSTS — MAPE Bayesian ARIMA 6,85% vs BSTS 26,46%, RMSE 0,0366 vs 0,2872. Alasannya adalah masalah konvergensi MCMC pada BSTS. Ini penting untuk didiskusikan (lihat poin 7). |
| Yang diadaptasi | Struktur komponen *state* BSTS, skema prior, dan metrik evaluasi (WAIC/LOOIC/R²/MAE/RMSE/MAPE) |
| **Gap terhadap skripsi ini** | Paper ini menerapkan BSTS pada satu seri nasional (CO2 agregat per kapita), sedangkan skripsi ini akan menerapkan BSTS per sektor CH4 (8 seri terpisah, dengan karakteristik sangat berbeda antar sektor) — berpotensi menghasilkan pola konvergensi dan performa yang berbeda per sektor |

### 5.2 Acuan Pembanding (struktur data & metode alternatif)
**Saputra, Nisa, Ramadhani, Almuhayar, & Devianto (2025).** *Time Series Model with Long Short-Term Memory Effect for Greenhouse Gas Estimation in Indonesia.* BAREKENG: Journal of Mathematics and Its Applications, 19(2), 949–960.

| Aspek | Ringkasan |
|---|---|
| Masalah | Memprediksi emisi gas rumah kaca (GRK) Indonesia 1970–2022 per sektor — 8 sektor yang sama persis dengan dataset CH4 di atas |
| Metode | LSTM sederhana 1 layer (32 unit) per sektor; normalisasi min-max; split 70:30; 1000 epoch |
| Hasil | MAPE training 1,99%–15,49%; MAPE validasi 3,15%–19,80%; proyeksi 3 tahun ke depan naik di hampir semua sektor kecuali Buildings |
| Relevansi bagi skripsi | Struktur 8 sektor dan negara (Indonesia) identik dengan dataset skripsi ini, sehingga berguna untuk membandingkan tren/hasil proyeksi sektoral — juga berpotensi menjadi baseline pembanding performa (LSTM vs BSTS) bila disarankan pembimbing, meskipun metode utamanya berbeda (LSTM, bukan Bayesian time series) |

---

## 6. Rencana Metode (sementara)

```
Dataset CH4 sektoral (1970–2024), 8 seri per sektor
        ↓
Uji stasioneritas (ADF); differencing bila diperlukan
        ↓
Pemilihan komponen state BSTS per sektor
  (local level / local linear trend / semi-local linear trend /
   Student local linear trend / autoregressive)
        ↓
Estimasi Bayesian via MCMC (mis. paket bsts di R atau Stan)
  - Prior weakly-informative untuk varians (s_obs, s_level, s_slope)
        ↓
Diagnostik konvergensi (trace plot, Rhat, effective sample size)
        ↓
Evaluasi & pemilihan model (WAIC, LOOIC, R², RMSE, MAE, MAPE)
        ↓
Forecasting + interval kredibel & analisis kebijakan
```

*Catatan: tahapan ini masih tentatif — bagian yang ingin didiskusikan dengan pembimbing ada di poin 7.*

---

## 7. Hal yang Ingin Didiskusikan

**Topik/metode:**
1. Apakah arah dan rumusan masalah penelitian sudah sesuai?
2. Apakah dataset (termasuk satuan yang belum jelas) sudah tepat digunakan, atau perlu sumber/verifikasi tambahan?
3. Apakah fokus ke CH4 sektoral dengan BSTS (bukan CO2 agregat seperti jurnal acuan utama) sudah menjadi gap yang cukup kuat?
4. **Pada jurnal acuan utama, BSTS justru kalah dibanding Bayesian ARIMA karena masalah konvergensi MCMC (Rhat > 1,01, terutama s_obs) — bagaimana strategi memastikan konvergensi yang baik di skripsi ini** (mis. iterasi/warm-up lebih banyak, prior lebih informatif, reparameterisasi), **atau apakah BSTS tetap perlu dibandingkan dengan Bayesian ARIMA sebagai pembanding seperti pada jurnal acuan?**
5. Komponen *state* BSTS mana yang paling sesuai untuk tiap sektor, mengingat pola tiap sektor sangat berbeda (ada yang monoton naik, menurun, dan sangat volatil — lihat poin 4)?
6. Apakah BSTS diterapkan terpisah per sektor (8 model, seperti pendekatan LSTM pada jurnal acuan kedua) atau digabung secara hierarkis/multivariat?

**Administrasi TA di BRIN:**
1. Bagaimana mekanisme/alur pendaftaran administrasi TA bagi mahasiswa ITERA yang dibimbing peneliti BRIN, dan dokumen apa saja yang perlu disiapkan?
2. Apakah ada surat tugas/penetapan resmi untuk Bapak/Ibu sebagai pembimbing II dari BRIN?
3. Apakah ada izin penggunaan data atau *clearance* tertentu jika data yang dipakai bersumber dari BRIN?
4. Dari sisi ITERA, dokumen apa yang perlu diselesaikan lebih dulu sebelum administrasi di BRIN berjalan?
5. Kepada siapa/unit mana administrasi ini sebaiknya diproses lebih lanjut, dan idealnya diselesaikan sebelum tahap apa dalam penelitian?

---

## 8. Agenda Bimbingan (± 60 menit)

| Waktu | Pembahasan |
|---|---|
| 0–5 menit | Perkenalan + tujuan bimbingan |
| 5–15 menit | Topik dan latar belakang penelitian |
| 15–25 menit | Tunjukkan dataset (buka langsung di laptop) |
| 25–35 menit | Jurnal acuan + gap penelitian |
| 35–45 menit | Diskusi metode dan arah penelitian |
| 45–55 menit | Administrasi TA di BRIN |
| 55–60 menit | Kesimpulan dan langkah selanjutnya |
