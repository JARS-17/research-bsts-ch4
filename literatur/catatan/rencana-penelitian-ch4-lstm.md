# Rencana Penelitian

## Penerapan LSTM untuk Proyeksi Emisi Gas Metana Tahunan Berdasarkan Sektor di Indonesia

**Penyusun:** JARS (Ginda Fajar Riadi Marpaung)
**Program Studi:** Sains Data, Institut Teknologi Sumatera (ITERA)
**Dokumen ini adalah:** rangkuman hasil bimbingan awal, bukan pengganti proposal formal BAB I–III

---

## 1. Latar Belakang

- Metana (CH4) memiliki potensi pemanasan global (*Global Warming Potential*) sekitar 80-an kali CO2 dalam jangka 20 tahun dan ~27–30 kali dalam jangka 100 tahun, tetapi umurnya di atmosfer jauh lebih pendek (~12 tahun) dibanding CO2 yang bertahan ratusan tahun. Mengurangi CH4 memberi efek pendinginan yang jauh lebih cepat — inilah alasan ilmiah CH4 layak diteliti terpisah dari GRK total, dan alasan lahirnya *Global Methane Pledge* (2021) yang juga ditandatangani Indonesia.
- Berdasarkan laporan resmi EDGAR 2025 (Crippa dkk., 2025), Indonesia termasuk 6 penyumbang GRK terbesar dunia pada 2024 (bersama China, AS, India, EU27, Rusia), dan dari 6 negara tersebut Indonesia mencatat **kenaikan emisi relatif terbesar** (+5,0% dibanding 2023).
- Kebijakan mitigasi butuh bertahun-tahun untuk direncanakan dan dieksekusi (transisi energi, infrastruktur pengolahan limbah, dst.), sehingga pembuat kebijakan (KLHK, Bappenas) memerlukan **proyeksi ke depan per sektor**, bukan sekadar laporan historis, agar sumber daya mitigasi bisa dialokasikan tertarget.
- Pola emisi historis menunjukkan dinamika non-linear (mis. sektor *Fuel Exploitation* tumbuh eksponensial, sektor *Processes* sempat turun di era 1970-an, rasio CH4/kapita melompat di suatu titik antara 2010–2024) — pola yang sulit ditangkap model linear sederhana, sehingga LSTM dipilih karena kemampuannya mempelajari dependensi temporal jangka panjang maupun pendek secara non-linear.

## 2. Rumusan Masalah

1. Bagaimana performa berbagai arsitektur LSTM dalam memodelkan dan memproyeksikan emisi CH4 tahunan pada masing-masing sektor emitor di Indonesia?
2. Bagaimana performa tersebut dibandingkan dengan baseline model time series klasik (ARIMA/SARIMA) untuk masing-masing sektor?
3. Faktor/variabel apa yang paling berpengaruh terhadap tren emisi tiap sektor (dijawab melalui pendekatan *explainable AI*)?
4. Bagaimana proyeksi emisi CH4 tiap sektor untuk beberapa tahun ke depan, dan apa implikasinya terhadap kebutuhan kebijakan mitigasi yang tertarget per sektor?

## 3. Tujuan Penelitian

- Membangun dan membandingkan beberapa arsitektur berbasis LSTM (Vanilla, Stacked, BiLSTM, Attention-LSTM) untuk proyeksi CH4 sektoral Indonesia, divalidasi terhadap baseline ARIMA/SARIMA.
- Mengidentifikasi variabel pendorong utama tren emisi tiap sektor melalui SHAP.
- Menghasilkan proyeksi 3–5 tahun ke depan per sektor sebagai bahan pertimbangan kebijakan mitigasi yang tertarget.

## 4. Kebaruan (Novelty) dan Posisi terhadap Riset Sebelumnya

| Riset sebelumnya | Keterbatasan | Posisi skripsi ini |
|---|---|---|
| Saputra dkk. (2025) — rujukan utama | Memodelkan GRK **total** (CO2-eq), bukan CH4 spesifik; hanya 1 arsitektur LSTM; tanpa baseline pembanding maupun interpretabilitas | Fokus CH4 spesifik; banding beberapa arsitektur; tambah baseline klasik + SHAP |
| GCN-LSTM multi-negara (Emerging Science Journal); Bi-LSTM/GRU top emitters (Frontiers Climate) | CH4 sektoral sudah dikaji, tapi Indonesia hanya salah satu dari puluhan/ratusan negara — tidak ada kedalaman analisis khusus Indonesia | Studi mendalam khusus satu negara (Indonesia), dengan penanganan eksplisit atas keterbatasan sampel |
| Studi Indonesia (BETRIK, J-COSINE, Fraction) | Semua untuk CO2/GRK total, model tunggal | Isolasi CH4, banding arsitektur |

**Pernyataan novelty:** Membangun dan membandingkan beberapa arsitektur LSTM (bukan satu arsitektur tunggal seperti riset rujukan) untuk proyeksi CH4 tahunan per sektor di Indonesia, divalidasi terhadap baseline klasik, dengan penanganan eksplisit atas keterbatasan sampel data (55 observasi tahunan).

## 5. Tinjauan Pustaka (20 Jurnal Terindeks)

*Catatan: entri bertanda ⚠️ perlu verifikasi metadata lengkap (penulis/DOI) sebelum masuk daftar pustaka final skripsi.*

**A. Rujukan utama & fondasi metode**
1. Saputra, R., Nisa, A.K., Ramadhani, N., Almuhayar, M., & Devianto, D. (2025). *Time Series Model with LSTM Effect for GHG Estimation in Indonesia*. BAREKENG, 19(2), 949–960. doi:10.30598/barekengvol19iss2pp0949-0960
2. Hochreiter, S., & Schmidhuber, J. (1997). *Long Short-Term Memory*. Neural Computation, 9(8), 1735–1780.

**B. LSTM/deep learning untuk emisi metana global**
3. Arya, S., Anju, & Onah, J.N. (2026). *A Data-Driven Framework for Methane Emission Prediction Using Machine Learning Methods*. ICCK Transactions on Machine Intelligence, 2(1), 53–64.
4. ⚠️ (2026). *A comparative analysis of advanced modeling techniques for global methane emission forecasting using SARIMA, LSTM, and GRU models*. Clean Technologies and Environmental Policy. doi:10.1007/s10098-026-03538-0
5. Hamdan, A., Al-Salaymeh, A., AlHamad, I.M., Ikemba, S., & Ewim, D.R.E. (2023). *Predicting future global temperature and greenhouse gas emissions via LSTM model*. Sustainable Energy Research, 10, 21. doi:10.1186/s40807-023-00092-x
6. ⚠️ (2025). *Modeling enteric methane emission from dairy cows using deep learning approach*. Science of The Total Environment. PMID: 40414055
7. ⚠️ (2026). *Global temperature anomaly prediction by using additive twin LSTM networks*. Scientific Reports. doi:10.1038/s41598-026-37255-x

**C. Model lanjutan/hybrid untuk emisi bersektor**
8. Tonny, B.D., dkk. (2026). *Multi-Country GHG Emissions Forecasting by Sector Using a GCN-LSTM Model*. Emerging Science Journal, 10(1).
9. Sha, M., Emmanuel, S., Bindhu, A., & Mustaq, M. (2024). *Configuring Gate with Fine-Tuning Shifts with Bi-LSTM and GRU System*. Frontiers in Climate, 6, 1457441. doi:10.3389/fclim.2024.1457441
10. ⚠️ (2026). *A hybrid CNN-BiLSTM-attention model with feature engineering for accurate carbon emission forecasting*. Air Quality, Atmosphere & Health. doi:10.1007/s11869-026-01954-1
11. Begum, M.A., & Mobin, A.M. (2025). *A machine learning approach to carbon emissions prediction of the top eleven emitters by 2030*. Scientific Reports, 15, 19469. doi:10.1038/s41598-025-04236-5

**D. Baseline klasik vs deep learning untuk emisi Indonesia (Sinta)**
12. Elmi, S., Yanti, R., Mardainis, & Asnal. (2025). *Prediksi Emisi CO2 di Indonesia Menggunakan Pendekatan Hybrid ARIMA dan LSTM*. Jurnal Ilmiah BETRIK, 16(2), 95–108.
13. Suwandi, S.I.N., Tyasnurita, R., & Muhayat, H. (2022). *Peramalan Emisi Karbon Menggunakan Metode SARIMA dan LSTM*. J-COSINE, 6(1), 73–80. doi:10.29303/jcosine.v6i1.436
14. Pramita, A., Kholisoh, N., & Lusia, R.A. (2023). *Prediksi Emisi GRK pada Sektor Energi di Indonesia Menggunakan Model ARIMA*. Fraction, 3(2), 63–70. doi:10.33019/fraction.v3i2.47
15. ⚠️ *Greenhouse gas emission forecasting analysis in Jakarta towards net zero emissions in 2050*. ScienceDirect.

**E. Emisi metana sektoral spesifik (Pertanian/Peternakan/Limbah) — konteks Indonesia**
16. ⚠️ (2022). *Machine Learning Methods for Predicting Manure Management Emissions*. JNTETI (UGM, Sinta 2).
17. ⚠️ (2025). *Predicting methane emissions in smallholder dairy systems: a clustering and ensemble learning approach*. Frontiers in Sustainable Food Systems, 9, 1668517.
18. ⚠️ (2024). *Greenhouse Gas Prediction Using LSTM Algorithm Based on Microsensor in Bandung City, Indonesia*. Springer. doi:10.1007/978-3-031-75861-4_12

**F. Explainable AI untuk model emisi**
19. ⚠️ (2025). *Deep learning model based prediction of vehicle CO2 emissions with eXplainable AI integration*. Scientific Reports, 15. doi:10.1038/s41598-025-87233-y
20. ⚠️ (2024). *Paddy rice methane emissions, controlling factors, and mitigation potentials across Monsoon Asia*. Science of The Total Environment.

## 6. Dataset

### 6.1 Sumber Utama
- **EDGAR (Emissions Database for Global Atmospheric Research), JRC** — Country Fact Sheet Indonesia, gas CH4, versi rilis **EDGAR_2025_GHG**.
- Kutipan resmi: Crippa, M., Guizzardi, D., Pagani, F., Banja, M., Muntean, M., dkk. (2025). *GHG Emissions of All World Countries – 2025 Report*. Publications Office of the European Union, Luxembourg. doi:10.2760/9816914
- Cara mengunduh ulang untuk verifikasi: `edgar.jrc.ec.europa.eu/country_profile` (pilih Indonesia + CH4), atau `edgar.jrc.ec.europa.eu/report_2025` → unduh `EDGAR_2025_GHG_booklet_2025.xlsx` → sheet `GHG_by_sector_and_country` (cek satuan di sini).

### 6.2 Struktur Data
- 55 observasi tahunan (1970–2024), 8 sektor (Agriculture, Buildings, Fuel Exploitation, Industrial Combustion, Power Industry, Processes, Transport, Waste) + kolom `Total CH4/cap`.
- Profil sektor CH4 murni **berbeda** dari GRK total di jurnal rujukan: Agriculture dan Fuel Exploitation mendominasi CH4, sedangkan Power Industry dan Industrial Combustion (dominan di GRK total karena CO2) justru sektor terkecil untuk CH4.
- Skala antar sektor timpang hingga 5 orde magnitudo (Agriculture ~10⁷–10⁸ vs Processes ~10²–10⁵) → normalisasi wajib dilakukan per sektor, bukan lintas sektor.

### 6.3 Sumber Alternatif/Pembanding
| Sumber | Karakteristik |
|---|---|
| SIGN-SMART (KLHK) | Inventarisasi domestik, 5 kategori IPCC, bottom-up, tahunan |
| PRIMAP-hist | Komposit multi-negara, prioritas data resmi UNFCCC, tahunan |
| FAOSTAT | Breakdown CH4 pertanian lebih rinci (enterik, manajemen kotoran, padi) |
| UNFCCC BUR/NC Indonesia | Sumber akar PRIMAP-hist & SIGN-SMART |
| Climate TRACE | Satelit + AI, **benar-benar bulanan**, tapi histori pendek (~2015 tahunan, ~2021+ bulanan) — trade-off besar bila dipakai |

### 6.4 Keterbatasan Data
- Tidak ada data resmi CH4 sektoral Indonesia dengan resolusi bulanan/harian untuk rentang panjang — data aktivitas dasarnya (sensus pertanian, neraca energi) memang dikumpulkan tahunan.
- 55 titik data adalah keterbatasan yang juga dialami hampir semua riset rujukan (bukan cacat unik penelitian ini), tetapi tetap perlu ditangani eksplisit dalam desain metodologi (lihat Bagian 7.3–7.4).

## 7. Metodologi (Pipeline Penelitian)

### 7.1 Problem Understanding
Identifikasi gap riset, keterkaitan SDGs, dan perumusan masalah (lihat Bagian 1–4).

### 7.2 Literature and Data Mining
Kajian 20 jurnal (Bagian 5) + akuisisi data dari EDGAR dan sumber pembanding (Bagian 6).

### 7.3 Data Preparation
- Pembersihan data, normalisasi min-max **per sektor**.
- *Data sequencing* dengan window `k` (uji beberapa nilai `k` untuk memaksimalkan jumlah sequence dari 55 titik).
- Strategi mengatasi keterbatasan sampel — pilih salah satu atau kombinasi:
  1. **Pooling lintas-sektor**: satu model belajar dari 8 sektor sekaligus (sektor sebagai fitur/embedding) → efektif ~440 titik sebelum windowing.
  2. **Pooling lintas-negara**: pretrain dari data CH4 multi-negara EDGAR, fine-tune untuk Indonesia (pendekatan *global forecasting model*).
  3. **Fitur eksogen tahunan**: tambahkan populasi, PDB, konsumsi energi, populasi ternak, luas panen padi (FAOSTAT/BPS/IEA) sebagai variabel multivariat — ini juga prasyarat untuk explainable AI di tahap 7.6.
  4. Augmentasi deret waktu (bootstrap/jittering) — jika dipakai, wajib diungkap eksplisit sebagai data sintetis untuk regularisasi.
- Split data mempertimbangkan validasi *walk-forward*, bukan sekadar 70:30 acak, mengingat ukuran sampel kecil.

### 7.4 Modelling
- **Baseline**: ARIMA/SARIMA per sektor (wajib, untuk pembanding ilmiah).
- **LSTM variants**: Vanilla, Stacked, BiLSTM, Attention-LSTM (mengikuti pola perbandingan yang sudah digunakan pada studi PM2.5/JIKI sebelumnya).
- Regularisasi ketat (unit kecil, dropout, early stopping) mengingat data terbatas.

### 7.5 Evaluation
- Metrik: MSE, MAPE, RMSE per sektor per arsitektur.
- Uji signifikansi antar model (mis. Diebold-Mariano dengan koreksi HLN, seperti pada studi PM2.5 sebelumnya).
- ↻ Jika performa belum memadai, iterasi kembali ke tahap 7.3–7.4 (bukan proses satu arah).

### 7.6 Explainable AI
SHAP (mis. GradientExplainer, sesuai pengalaman studi PM2.5) pada model terbaik untuk mengidentifikasi variabel eksogen paling berpengaruh per sektor — menjawab pertanyaan "kenapa sektor A meningkat".

### 7.7 Result and Interpretation
Proyeksi 3–5 tahun ke depan per sektor, dikaitkan dengan implikasi kebijakan dan SDGs (Bagian 8).

### 7.8 Reporting and Presentation
Laporan skripsi, draf artikel jurnal, dan (opsional) dashboard/diseminasi (Bagian 9–10).

## 8. Keterkaitan dengan SDGs

- **SDG 13** (Penanganan Perubahan Iklim) — target 13.2 dan 13.3, paling langsung relevan.
- **SDG 7** (Energi Bersih) — sektor Power Industry, Fuel Exploitation.
- **SDG 11 & 12** (Kota Berkelanjutan; Konsumsi-Produksi Bertanggung Jawab) — sektor Waste.
- **SDG 2** (Tanpa Kelaparan, target 2.4) — sektor Agriculture.
- **SDG 9** (Industri, Inovasi, Infrastruktur) — sektor Transport/Industrial Combustion, dan dari sisi metodologi (penerapan deep learning untuk masalah lingkungan).
- Konteks kebijakan pendukung: kenaikan target NDC Indonesia (29%→31,89% usaha sendiri; 41%→43,20% dengan dukungan internasional).

## 9. Luaran Penelitian

**Wajib (inti nilai ilmiah):**
- Laporan skripsi format ITERA (BAB I–V).
- Draf artikel jurnal (target Sinta 2–3 atau Scopus tier bawah, mengikuti pola submission JIKI sebelumnya).
- Tabel & grafik perbandingan arsitektur + baseline per sektor.
- Proyeksi 3–5 tahun ke depan per sektor.

**Sangat disarankan (nilai tambah besar, biaya kecil):**
- Analisis SHAP atas fitur eksogen per sektor.

**Nice-to-have (jika waktu memungkinkan):**
- Experiment tracking (MLflow) untuk mencatat eksperimen perbandingan arsitektur.
- Dashboard (Streamlit) yang membaca model yang sudah dilatih — bukan retraining real-time, karena data sumber (EDGAR) hanya rilis tahunan.

## 10. Strategi Pemberdayaan/Dampak Penelitian

**Level 1 — Sepenuhnya di kendali sendiri:**
- Rilis kode & model di GitHub (open source), daftarkan ke Zenodo untuk DOI dataset olahan.
- Diseminasi internal ke HMSD Adyatama (studi kasus/workshop forecasting untuk isu lingkungan).

**Level 2 — Butuh sedikit usaha ekstra:**
- Tulisan populer/policy brief (2–4 halaman, non-teknis) untuk media seperti *The Conversation Indonesia*/*Katadata*.
- Deploy dashboard secara publik (Streamlit Community Cloud) jika dibuat.

**Level 3 — Butuh jembatan pihak lain, hasil tidak pasti:**
- Manfaatkan koneksi institusi magang (BRIN) untuk menjajaki apakah hasil riset relevan sebagai bahan internal atau bahan diskusi forum terkait NDC/Global Methane Pledge.
- Catatan realistis: adopsi kebijakan langsung dari skripsi S1 jarang terjadi secara langsung; dampak paling realistis tetaplah di Level 1–2 (sitasi, portofolio kredibilitas, inspirasi riset lanjutan).

## 11. Manajemen Ruang Lingkup (Prioritas Pengerjaan)

1. Perbandingan arsitektur LSTM + baseline klasik, evaluasi ketat (**wajib**)
2. Fitur eksogen + SHAP (**sangat disarankan**, biaya kecil dampak besar)
3. MLflow tracking + dashboard (**nice-to-have**, kerjakan jika waktu tersisa)

---

*Dokumen ini adalah rangkuman kerja bimbingan berjalan dan akan berkembang seiring diskusi lanjutan (mis. detail teknis BAB III, hasil eksperimen di BAB IV).*
