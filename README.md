# Projek Ecommerce Funnel

### Mengapa Sebagian Besar Pengunjung Tidak Membeli?
*Analisis funnel lihat produk → tambah ke keranjang → beli dari keranjang pada toko elektronik online*

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-data%20analysis-150458?logo=pandas&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-logistic%20regression-F7931E?logo=scikitlearn&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-dashboard-F2C811?logo=powerbi&logoColor=black)

**Take Home Test Data Analyst | Exam Penyaluran Kerja, Data Analyst Bootcamp Dibimbing**
Oleh **Frederick Rianto Putra**

> **English summary:** I analyzed 885,129 behavior events from an online electronics store to find out why only **4.6%** of product-viewing sessions end in a purchase from the cart. Sessions are lost at two points: **90.8%** never reach the cart, and about **half** of cart sessions do not buy. Conversion is lowest for first-time visitors who view a single product and for long-tail products, and abandoned carts are linked to higher prices. All findings are associations, tested with scenarios and an A/B test plan.

---

## Ringkasan

Toko mencatat setiap `view`, `cart`, dan `purchase` pengunjung. Proyek ini menjawab satu pertanyaan bisnis:

> Mengapa 95,4% sesi yang melihat produk tidak berakhir dengan pembelian dari keranjang, di tahap mana sesi hilang, dan faktor apa yang bisa diubah toko untuk menaikkan konversi?

Konversi lihat → beli adalah hasil kali dua tahap, sehingga setiap segmen dilaporkan pada tiga angka: peluang ke keranjang, peluang beli dari keranjang, dan konversi total.

| Business Question | Pertanyaan |
|---|---|
| **BQ1** | Seberapa besar masalahnya, dan di tahap mana sesi hilang? |
| **BQ2** | Siapa yang tidak membeli (kedalaman browsing, kunjungan pertama vs kembali)? |
| **BQ3** | Apa yang mereka lihat (popularitas, kategori, harga) dan kapan? |
| **BQ4** | Faktor mana yang paling membedakan pembeli di tiap tahap? |

## Temuan Utama

| Temuan | Angka |
|---|---|
| Konversi lihat → beli dari keranjang | **4,6%** (269.858 sesi dengan view, 24.952 ke keranjang, 12.296 membeli) |
| Titik kebocoran | 90,8% sesi tidak pernah ke keranjang; sekitar separuh sesi berkeranjang tidak membeli |
| Pengunjung baru, 1 view | 64,1% dari semua sesi, konversi **3,1%** (vs **10,0%** untuk kunjungan kembali dengan 2+ view) |
| Popularitas produk | Produk paling jarang dilihat **2,2%** vs paling populer **7,7%** |
| Harga | Konversi naik dengan harga, tetapi keranjang termahal hanya dibeli **44,3%** (termurah **58,0%**) |
| Waktu | Jam dan hari berpengaruh kecil (konversi per jam 3,5% sampai 5,0%) |
| Faktor pembeda (lihat → beli) | Popularitas produk, kunjungan kembali, kedalaman browsing (AUC 0,69) |
| Faktor pembeda (keranjang → beli) | Harga dan kunjungan kembali, tetapi daya prediksi rendah (AUC 0,57) |

## Rekomendasi (untuk diuji dengan A/B test)

1. **Kurangi keranjang ditinggalkan:** pengingat cepat di dalam sesi, kejelasan total biaya, opsi pembayaran untuk keranjang bernilai tinggi. Skenario: 10% keranjang ditinggalkan menjadi pembelian menambah sekitar 1.270 pembelian.
2. **Angkat produk ekor panjang:** rekomendasi, pencarian, dan bundling. Skenario: naik ke tingkat Q2 menambah sekitar 750 pembelian.
3. **Perluas penjelajahan di kunjungan pertama:** produk serupa dan perbandingan sejak view pertama. Skenario: 10% berpindah menambah sekitar 470 pembelian.
4. **Dorong kunjungan kembali** dengan pengingat produk yang pernah dilihat (efek sebab-akibat belum terbukti).
5. **Uji dan ukur** setiap perubahan dengan konversi lihat → beli sebagai metrik utama.

Angka dampak adalah skenario dengan asumsi eksplisit, bukan prediksi.

## Metodologi

1. **Data cleaning:** hapus duplikat (655 baris) dan harga tidak valid; **baris dengan `category_code` atau `brand` kosong dihapus** (tidak diisi); sesi yang event keranjang atau pembeliannya ikut terhapus dikeluarkan agar funnel tidak bias; outlier harga dipertahankan karena komponen premium sah.
2. **Uji kualitas sesi:** `user_session` diuji terhadap sesi dengan jeda 30 menit; selisih konversi kecil (4,59% vs 4,56%).
3. **Feature engineering:** fitur sesi hanya dari view sebelum atau saat keranjang atau pembelian pertama, agar hasil tidak bocor ke fitur.
4. **Analisis diagnostik:** konversi per kedalaman browsing, jenis kunjungan, popularitas produk, kategori, harga, dan waktu.
5. **Model penjelas:** regresi logistik dua tahap (lihat → beli dan keranjang → beli), data uji 25%, kontribusi diukur dari penurunan ROC-AUC saat fitur atau kelompok fitur dibuang.
6. **Rekomendasi:** simulasi skenario dan rencana A/B test dengan metrik pengaman.

### Catatan penting tentang data

Menghapus nilai kosong membuang **41,6% event** (tersisa 498.199 event dari 271.049 sesi) dan sekitar **41% pembelian dari keranjang**. Sesi yang dibuang berkonversi lebih rendah (3,65% vs 4,56%), sehingga hasil berlaku untuk **produk dengan kategori dan brand tercatat**, dan konversi pada data bersih sedikit lebih tinggi daripada seluruh toko (4,56% vs 4,25%).

## Struktur Repository

```
Projek-Ecommerce-Funnel/
├── Codingan_Projek_FIXFIX.ipynb              # Notebook analisis lengkap (sudah dijalankan, beserta output)
├── Take_Home_Test_FIX_banget_moga_aman.pptx  # Slide presentasi
└── README.md
```

Dataset tidak disertakan karena ukurannya besar. Hasil eksekusi notebook (folder `powerbi_export/`) berisi `events_cleaned.csv` dan semua grafik PNG.

## Dataset

[eCommerce Events History in Electronics Store](https://www.kaggle.com/datasets/mkechinov/ecommerce-events-history-in-electronics-store/data) (Kaggle, mkechinov; data dari REES46 Marketing Platform).

- 885.129 event (`view`, `cart`, `purchase`), 24 Sep 2020 sampai 28 Feb 2021 (waktu UTC)
- 8 kolom dipakai: `event_time`, `event_type`, `product_id`, `category_code`, `brand`, `price`, `user_id`, `user_session`

## Cara Menjalankan

1. Unduh `events.csv` dari Kaggle (link di atas). Pastikan ukurannya utuh dan sel profiling menampilkan **885.129 baris**.
2. Buka `Codingan_Projek_FIXFIX.ipynb` di **Google Colab** atau Jupyter lokal.
3. Letakkan `events.csv` di folder kerja, `/content` (Colab), atau Google Drive. Jika di folder lain, isi `DATA_PATH` di sel "Lokasi Dataset".
4. Jalankan semua sel dari atas ke bawah (sekitar satu menit). Jalankan sel **Output Akhir** paling akhir.

Library: `pandas`, `numpy`, `matplotlib`, `scipy`, `scikit-learn`.

```bash
pip install pandas numpy matplotlib scipy scikit-learn
```

## Dashboard

Dashboard Power BI terdiri dari tiga halaman dengan filter interaktif (kategori dan jenis kunjungan):
"<img width="1529" height="853" alt="image" src="https://github.com/user-attachments/assets/1642a758-2d41-4dc4-8877-2724d40229b4" />
"
1. **Funnel Overview:** KPI konversi, funnel tiga tahap, hasil sesi
2. **Siapa yang Tidak Membeli:** kedalaman browsing, kunjungan pertama vs kembali, segmen
3. **Apa yang Dilihat dan Kapan:** popularitas, kategori, harga, jam, tren mingguan


