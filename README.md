# Analisis Tutupan Lahan Wilayah Sarbagita (Tahun 2020, 2023, 2026)

Repositori ini berisi alur kerja (*workflow*) spasial untuk melakukan ekstraksi, klasifikasi, dan proyeksi perubahan tutupan lahan di wilayah Sarbagita (Kota Denpasar, Badung, Gianyar, dan Tabanan), Provinsi Bali. Model mengintegrasikan Google Earth Engine (GEE) API untuk ekstraksi data skala besar dan algoritma Machine Learning (Random Forest) untuk pemodelan prediksi spasial.

Model prediktif dibangun menggunakan pendekatan *Feature Trend* dan *Spatial Multi-Criteria Evaluation* dengan mempertimbangkan:
*   Driving Factors: Kepadatan/Jarak ke Pemukiman, Kemiringan Lereng (*Slope*), dan Jarak ke Jalan Raya.
*   Restricting Factors: Badan Air eksisting, Kawasan Lindung (WDPA Darat), dan Sempadan Sungai.

## Hasil dan Temuan Utama
### 1. Performa Model Klasifikasi (Random Forest)
Model Random Forest dilatih menggunakan fitur spektral asli dan indeks turunan (NDVI, MNDWI, NDBI, BSI). Evaluasi akurasi menggunakan *training sample* menunjukkan hasil yang reliabel:
*   Tahun 2020: *Overall Accuracy* (OA) mencapai 81,4% dengan nilai *Cohen's Kappa* 0,76.
*   Tahun 2023: *Overall Accuracy* (OA) mencapai 76,2% dengan nilai *Cohen's Kappa* 0,69.
*   Berdasarkan pedoman interpretasi nilai Kappa, skor 0,76 dan 0,69 tersebut berada pada rentang 0,61 - 0,80. Hal ini menunjukkan bahwa tingkat keeratan kesepakatan (reliability) klasifikasi model untuk kedua tahun tersebut masuk dalam kategori Kuat. Kategori Kuat ini menunjukkan bahwa algoritma Random Forest yang digunakan telah berhasil mengenali dan memisahkan kelas-kelas tutupan lahan di wilayah Sarbagita dengan tingkat kepastian yang tinggi, sehingga sangat layak digunakan sebagai acuan proyeksi tahun 2026..
*   Model Transisi: Untuk prediksi perubahan lahan, model menghasilkan *Producer's Accuracy* (PA) sebesar 60,7% dan *User's Accuracy* (UA) 46,1% pada kelas lahan yang berubah.

### 2. Tren Perubahan Tutupan Lahan (2020 - 2023)
Hasil klasifikasi mengonfirmasi adanya tren urbanisasi yang terdeteksi oleh model:
*   Ekspansi Perkotaan: Terdapat peningkatan luasan pada kelas Permukiman (Kelas 1) dari ~2,6 juta piksel menjadi ~2,8 juta piksel. Lahan Terbuka (Kelas 5) juga meningkat dari ~123 ribu menjadi ~191 ribu piksel.
*   Alih Fungsi Lahan: Peningkatan urbanisasi ini selaras dengan menyusutnya luasan Sawah/Vegetasi Campur (Kelas 4), yang turun dari ~9,2 juta piksel menjadi ~8,5 juta piksel. 
*   Vegetasi Lebat: Tercatat adanya peningkatan luasan Vegetasi (Kelas 3) dari ~5,4 juta menjadi ~6,0 juta piksel. Badan air (Kelas 2) mengalami sedikit penurunan luasan.

## Struktur Repositori
*   `notebook/` : Berisi *script* Python / Jupyter Notebook utama (`sarbagita_landcover_projection_rifky_maulana_akbar.py`).
*   `data/` : (Lokal) Tempat menyimpan *Shapefile* batas administrasi dan sungai pendukung. File raster mentah Sentinel-2 dan data lainnya termasuk di dalamnya.
*   `output/` : Peta klasifikasi tutupan lahan 2020 dan 2023 serta peta proyeksi tutupan lahan 2026 berformat *.tif*. Tabel akurasi hasil (*Overall Accuracy, Kappa, Producer/User Accuracy*) dalam format CSV.

## Cara Penggunaan (Panduan Replikasi)
Script pada repositori ini dirancang untuk dieksekusi menggunakan Google Colab. Ikuti langkah-langkah berikut sebelum menjalankan semua blok kode (*Run All*):

1. Persiapan Direktori Google Drive:
   Script ini menggunakan fungsi `drive.mount('/content/drive')`. Anda harus menyesuaikan variabel *path* direktori yang mengarah ke folder Google Drive lokal Anda (seperti variabel `PATH_IMG2020`, `PATH_SAMPLE`, dan `FOLDER_RESULT`) agar proses *input/output* file dapat berjalan lancar.
2. Autentikasi Google Earth Engine (GEE):
   * Script ini menggunakan akses langsung ke GEE API. 
   * **PENTING:** Ganti `project='...'` pada fungsi `ee.Initialize()` dengan ID Google Cloud project Earth Engine Anda sendiri.
3. Persiapan Data Eksternal:
   Jika ingin melakukan klasifikasi ulang dari nol, pastikan Anda mengunduh citra Sentinel-2 Sarbagita (.tif) dan data pendukung lainnya dan menyimpannya di folder Drive Anda terlebih dahulu.
 
*Catatan: Seluruh alur analisis spasial, konseptualisasi, dan keputusan metodologi dikerjakan dan divalidasi sendiri oleh saya, dengan dukungan asistensi kecerdasan buatan (AI) dalam perumusan struktur script Python/GEE dan dokumentasi teknis.*

-----
