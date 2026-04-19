# 🏠 Airbnb Bangkok — Analisis Pasar Properti Sewa Jangka Pendek

> **Capstone Project Module 2 — Data Analysis**  

---

## 📋 Deskripsi Proyek

Proyek ini merupakan analisis mendalam terhadap pasar **Airbnb di Bangkok, Thailand**, yang memiliki lebih dari 15.000 listing yang tersebar di 50 neighborhood. Sebagai seorang *data analyst*, proyek ini bertujuan untuk memetakan faktor-faktor penentu harga, pola distribusi listing, perilaku host, serta dinamika ulasan dan ketersediaan listing guna memberikan **rekomendasi berbasis data** kepada stakeholder Airbnb Bangkok.

---

## 🗂️ Struktur Repositori

```
capstone2-purwadhika/
│
├── README.md                        # Dokumentasi proyek (file ini)
├── Capstone2_Airbnb_Lauzia.ipynb    # Jupyter Notebook analisis lengkap
├── Airbnb Listings Bangkok.csv      # Dataset mentah
└── airbnb_bangkok_clean.csv         # Dataset bersih hasil data cleaning
```

---

## ❓ Business Questions

Analisis ini menjawab 6 pertanyaan bisnis utama:

| # | Topik | Pertanyaan |
|---|-------|------------|
| Q1 | Harga & Faktor Penentu | Apa faktor utama yang memengaruhi variasi harga listing di Bangkok? |
| Q2 | Distribusi Listing | Bagaimana distribusi listing berdasarkan tipe kamar di berbagai neighborhood? |
| Q3 | Ketersediaan & Musiman | Apakah ada pola musiman dalam ketersediaan listing antar tipe kamar? |
| Q4 | Perilaku Host | Sejauh mana pasar didominasi oleh host profesional vs host individu? |
| Q5 | Popularitas & Ulasan | Apakah listing dengan ulasan lebih banyak cenderung lebih mahal? |
| Q6 | Minimum Nights | Bagaimana aturan minimum nights memengaruhi harga dan jumlah ulasan? |

---

## 📊 Dataset

**Sumber:** Airbnb Listings Bangkok (Inside Airbnb)

**Dimensi awal:** 15.854 baris × 16 kolom  
**Dimensi setelah cleaning:** 15.853 baris × 20 kolom (termasuk kolom baru hasil feature engineering)

| Kolom | Deskripsi |
|-------|-----------|
| `id` | ID unik listing |
| `name` | Nama listing |
| `host_id` / `host_name` | Identitas host |
| `neighbourhood` | Lokasi/kawasan listing |
| `latitude` / `longitude` | Koordinat geografis |
| `room_type` | Tipe kamar (Entire home/apt, Private room, Shared room, Hotel) |
| `price` | Harga per malam (THB) |
| `minimum_nights` | Minimum malam menginap |
| `number_of_reviews` | Total jumlah ulasan |
| `last_review` | Tanggal ulasan terakhir |
| `reviews_per_month` | Rata-rata ulasan per bulan |
| `calculated_host_listings_count` | Jumlah listing per host |
| `availability_365` | Ketersediaan dalam 365 hari |
| `number_of_reviews_ltm` | Jumlah ulasan 12 bulan terakhir |
| `price_handled_median` | Harga per malam setelah data dibersihkan |
| `price_category` | Kategori/segmen harga per malam |
| `min_nights_handled` | Minimum malam menginap setelah data dibersihkan |
| `stay_category` | Kategori/segmen inap per malam |

---

## 🧹 Data Cleaning

Proses pembersihan data mencakup:

1. **Missing Values**
   - `reviews_per_month` (5.790 kosong) → diisi `0` (logika: belum pernah direview)
   - `name` & `host_name` (9 kosong) → diisi `"No Name"`
   - `last_review` → dikonversi ke tipe `datetime`

2. **Duplikat** → Tidak ditemukan, data dinyatakan bersih

3. **Outlier `price`**
   - 1 baris dengan harga `0` → dihapus (tidak valid secara bisnis)
   - 1.403 listing (8,85%) dengan harga > 4.722 THB → imputasi median + binning ke kolom `price_category`

4. **Outlier `minimum_nights`**
   - 3.168 listing (19,98%) dengan nilai ekstrem hingga 1.125 malam → imputasi median + binning ke kolom `stay_category`

5. **Kolom `number_of_reviews`, `reviews_per_month`, `calculated_host_listings_count`** → outlier **dibiarkan** karena merepresentasikan segmen bisnis nyata (listing/host populer)

6. **Perbaikan format:** `neighbourhood` diseragamkan ke *Title Case*, kolom kategorikal dikonversi ke tipe `category`

---

## 🔍 Ringkasan Insight Analisis

### Q1 — Faktor Penentu Harga
- **Tipe kamar** adalah faktor paling dominan (uji Kruskal-Wallis, p < 0.05)
- **Lokasi (neighbourhood)** berpengaruh signifikan; kawasan CBD seperti Parthum Wan memiliki median harga tertinggi (1.693 THB)
- **Ketersediaan** berkorelasi negatif lemah dengan harga
- Pasar didominasi segmen **Budget** (4.102 listing) dan **High** (3.954 listing)

### Q2 — Distribusi Listing
- **Entire Home/Apt** mendominasi ~60% listing di seluruh Bangkok
- **Vadhana** adalah neighborhood dengan listing terbanyak (area bisnis & hiburan Sukhumvit)
- Ada asosiasi signifikan antara tipe kamar dan neighborhood (Chi-Square, p < 0.05; Cramer's V moderat)

### Q3 — Ketersediaan & Musiman
- Distribusi availability bersifat **bimodal**: banyak listing sangat jarang tersedia (sering dipesan) atau sangat banyak tersedia (kurang diminati)
- **Hotel Room** paling konsisten tersedia sepanjang tahun
- Listing kategori **Luxury** justru memiliki ketersediaan rata-rata lebih tinggi dibanding **Budget**

### Q4 — Perilaku Host
- Mayoritas listing dimiliki **host individu** (Single Listing)
- Host profesional (>20 listing) menetapkan harga ~100 THB lebih tinggi dari host individu (signifikan, p < 0.05)
- Fenomena **"power host"**: sedikit host yang mengontrol banyak properti

### Q5 — Popularitas & Ulasan
- Listing **lebih murah** cenderung mendapat **lebih banyak ulasan** (korelasi negatif lemah)
- **Entire Home/Apt** mendapat rata-rata ulasan tertinggi
- **Vadhana** dan **Khlong Toei** memimpin dalam total ulasan keseluruhan

### Q6 — Minimum Nights & Strategi Host
- **Short Stay (1–3 malam)** mendominasi >70% listing, sesuai karakter kota wisata
- Listing **Long-Term** menawarkan harga per malam lebih rendah (strategi diskon volume)
- Short Stay mendapatkan lebih banyak ulasan karena pergantian tamu yang lebih sering

---

## 💡 Actionable Recommendations

| # | Kategori | Rekomendasi | Target KPI |
|---|----------|-------------|------------|
| 1 | **Pricing** | Implementasi *Smart/Dynamic Pricing* untuk mengatasi harga statis saat low season | Occupancy Rate |
| 2 | **Supply** | Ekspansi unit Private Room di area penyangga CBD untuk menjangkau segmen budget | New Listings Growth |
| 3 | **Experience** | Program *Superhost Fast-Track* untuk listing budget dengan ulasan tinggi | Guest Satisfaction |
| 4 | **Product** | Fokus konversi ruang kosong menjadi Private Room (bukan Shared Room) | Solo Traveler Segment |
| 5 | **Policy** | Fleksibilitas minimum nights di area wisata untuk meningkatkan perputaran unit | Total Bookings |

**Prioritas Utama: Smart Pricing (Opsi 2)** — Quick win dengan dampak bisnis tertinggi, meningkatkan GBV (Gross Booking Value), occupancy rate host, dan memberikan *fair price* bagi tamu.

---

## 🛠️ Tools & Libraries

| Kategori | Tools |
|----------|-------|
| Bahasa | Python 3 |
| Analisis Data | `pandas`, `numpy` |
| Visualisasi | `matplotlib`, `seaborn` |
| Statistik | `scipy.stats` (Kruskal-Wallis, Mann-Whitney U, Chi-Square, Spearman Correlation) |
| Dashboard | Tableau Public |
| Notebook | Jupyter Notebook |

---

## 📈 Metode Statistik yang Digunakan

- **Statistik Deskriptif**: mean, median, standar deviasi, distribusi frekuensi, crosstab
- **Kruskal-Wallis Test**: membandingkan distribusi harga/ulasan antar >2 kelompok
- **Mann-Whitney U Test**: membandingkan 2 kelompok independen
- **Spearman Correlation**: mengukur korelasi antar variabel numerik (non-parametrik)
- **Chi-Square & Cramer's V**: mengukur asosiasi antar variabel kategorikal

---

## 🔗 Link Deliverables

| Deliverables | Link |
|---|---|
| 📓 Jupyter Notebook | *https://github.com/lauziafadhilanareswari/capstone2-purwadhika.git* |
| 📊 Dashboard Tableau | *https://public.tableau.com/views/Capstone2_LauziaFN/DashboardAirbnbBangkok?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link* |
| 🎞️ Slide Presentasi | *https://drive.google.com/file/d/1QoVz4g7q3BLGIk-csl-cpsdyKKssaGus/view?usp=sharing* |
| 🎥 Video Penjelasan | *YouTube: https://youtu.be/vw33FPv63F0 / Google Drive: https://drive.google.com/file/d/1bcR0yUnKBvMI48UzKdCjB6yGchRPME6F/view?usp=drive_link* |


---

## 👤 Author

**Nama:** *Lauzia Fadhila Nareswari*  
