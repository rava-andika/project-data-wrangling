<div align="center">
  <h1>project-data-wrangling</h1>
  <p><strong>Pemetaan Ketimpangan Akses Air Minum Layak dan Sanitasi Layak di Indonesia</strong></p>
  
  <p align="center">
    <img src="https://img.shields.io/badge/Course-Data_Wrangling-blue?style=flat-square" alt="Course">
    <img src="https://img.shields.io/badge/Year-2025-orange?style=flat-square" alt="Year">
    <img src="https://img.shields.io/badge/Status-Completed-success?style=flat-square" alt="Status">
    <img src="https://img.shields.io/badge/License-Academic-lightgrey?style=flat-square" alt="License">
  </p>
  
  <p align="center">
    Analisis kewilayahan komprehensif untuk mengidentifikasi disparitas akses air minum layak dan sanitasi layak di Indonesia tingkat provinsi serta kabupaten/kota menggunakan integrasi data statistik BPS, kodifikasi wilayah Kemendagri, dan batas spasial BIG.
  </p>
</div>

## Tech Stack

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=for-the-badge&logo=numpy&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-%23ffffff.svg?style=for-the-badge&logo=Matplotlib&logoColor=black)
![Git](https://img.shields.io/badge/git-%23F05033.svg?style=for-the-badge&logo=git&logoColor=white)

## Profil Proyek

Proyek ini disusun sebagai Proyek Akhir Mata Kuliah Data Wrangling (Semester Gasal 2026/2027).

Disusun oleh Kelompok 2:

| Nama Anggota | NIM |
|---|---|
| Rava Dwi Andika | 25031554039 |
| Welfee Putri Sukardi | 25031554205 |

## Latar Belakang dan Tujuan

Akses terhadap air minum layak dan sanitasi layak merupakan kebutuhan dasar yang berdampak langsung pada derajat kesehatan, produktivitas, dan kesejahteraan masyarakat. Meskipun capaian nasional terus meningkat, disparitas antarwilayah di Indonesia masih menjadi tantangan nyata.

Tujuan utama proyek ini adalah:
1. Memeriksa, membersihkan, dan merekonsiliasi data indikator akses air minum layak dan sanitasi layak tahun 2025 dari Badan Pusat Statistik (BPS).
2. Mengintegrasikan data tabular dengan master kode wilayah Kementerian Dalam Negeri dan data geospasial batas wilayah dari Badan Informasi Geospasial (BIG).
3. Menghitung tingkat ketertinggalan (deprivation) pada masing-masing sektor serta menyusun Combined Deprivation Index (CDI).
4. Menentukan isu dominan (kekurangan air minum vs sanitasi) untuk tiap wilayah administratif.
5. Mengidentifikasi provinsi dengan tingkat deprivasi tertinggi secara nasional, dilanjutkan dengan analisis mendalam pada tingkat kabupaten/kota di provinsi tersebut.
6. Menerapkan clustering K-Means untuk mengelompokkan profil ketertinggalan wilayah dan menyajikan visualisasi spasial interaktif baik skala provinsi maupun nasional.

## Metodologi dan Formulasi

Tingkat ketertinggalan akses dihitung berdasarkan komplementer dari persentase akses yang dilaporkan oleh BPS:

$$
\text{Deprivation Water} = 100 - \text{Access Water}
$$

$$
\text{Deprivation Sanitation} = 100 - \text{Access Sanitation}
$$

Combined Deprivation Index (CDI) merupakan rerata tidak berbobot dari kedua dimensi ketertinggalan tersebut:

$$
\text{CDI} = \frac{\text{Deprivation Water} + \text{Deprivation Sanitation}}{2}
$$

Ketentuan penghitungan:
- CDI hanya dihitung apabila data kedua indikator tersedia secara lengkap (tidak menggantikan nilai hilang dengan angka nol).
- Klasifikasi isu dominan didasarkan pada perbandingan nilai deprivation dengan ambang batas toleransi (delta):
  - Isu Sanitasi Dominan: jika deprivation sanitasi melebihi deprivation air minum sebesar lebih dari delta.
  - Isu Air Minum Dominan: jika deprivation air minum melebihi deprivation sanitasi sebesar lebih dari delta.
  - Seimbang: jika selisih kedua deprivation berada di dalam rentang toleransi delta.

## Sumber Data

Proyek ini mengintegrasikan tiga sumber data resmi tahun 2025:

| Sumber | Deskripsi Data | Format | Lokasi File |
|---|---|---|---|
| Badan Pusat Statistik (BPS) | Persentase rumah tangga dengan akses air minum layak menurut kabupaten/kota (2025) | CSV | `data/raw/Persentase Rumah Tangga yang Memiliki Akses terhadap Sumber Air Minum Layak menurut Kabupaten_Kota, 2025.csv` |
| Badan Pusat Statistik (BPS) | Persentase rumah tangga dengan akses sanitasi layak menurut kabupaten/kota (2025) | CSV | `data/raw/6.2.1_ Persentase rumah tangga yang memiliki akses terhadap sanitasi layak menurut kabupaten_kota, 2025.csv` |
| Badan Pusat Statistik (BPS) | Persentase akses air minum dan sanitasi menurut provinsi dan klasifikasi desa (2025) | XLSX | `data/raw/Persentase Rumah Tangga yang Memiliki Akses terhadap Sumber Air Minum Layak Menurut Provinsi dan Klasifikasi Desa, 2025.xlsx` & sanitasi |
| Badan Informasi Geospasial (BIG) | Batas administrasi kabupaten/kota seluruh Indonesia (2025) | GeoJSON (Git LFS) | `data/raw/batas_kabupaten_kota.geojson` (diperoleh via `notebooks/scrap-geojson.ipynb` dari REST API ArcGIS BIG) |
| Kementerian Dalam Negeri | Master kode dan penamaan wilayah administrasi Indonesia (2025) | XLSX | `data/raw/KODE-WILAYAH-KEPMENDAGRI-2025.xlsx` |

Catatan: File `batas_kabupaten_kota.geojson` berukuran sekitar 612 MB dan dikelola menggunakan Git Large File Storage (Git LFS). Repositori menyediakan notebook `notebooks/scrap-geojson.ipynb` untuk mengunduh ulang layer fitur langsung dari endpoint ArcGIS BIG secara bertahap (batch).

## Tahapan Analisis (Data Wrangling Pipeline)

Alur kerja dalam proyek ini terbagi menjadi 8 tahapan terstruktur:

1. Penyiapan Lingkungan dan Pemeriksaan Sumber Daya: Menginisialisasi pustaka, path proyek, serta memverifikasi ketersediaan seluruh file sumber data.
2. Ekstraksi dan Penyelarasan Data Indikator: Menangani header bertingkat, baris metadata, dan anomali penulisan teks pada file BPS.
3. Standarisasi Kode dan Rekonsiliasi Wilayah: Menyelaraskan variasi penamaan kabupaten/kota terhadap master kodifikasi Kepmendagri 2025 untuk menjamin integritas penggabungan (merge).
4. Penanganan Nilai Hilang dan Validasi Rentang: Memastikan integritas data pada domain 0-100 persen dan mengidentifikasi wilayah dengan data parsial.
5. Komputasi Deprivation dan Indeks CDI: Menghitung nilai deprivation per sektor, skor CDI nasional/provinsi/kabupaten, dan penentuan kategori isu dominan.
6. Analisis Statistik dan Evaluasi Outlier: Memeriksa korelasi Pearson antara sektor air dan sanitasi serta mendeteksi outlier menggunakan metode Interquartile Range (IQR).
7. Segmentasi Wilayah Berbasis K-Means: Melakukan standardisasi fitur (StandardScaler), evaluasi jumlah klaster optimal (Elbow Method dan Silhouette Score), serta pemprofilan klaster wilayah.
8. Integrasi Geospasial dan Visualisasi Interaktif: Menggabungkan atribut analisis dengan batas polygon GeoJSON untuk menghasilkan peta tematik Folium skala provinsi terpilih serta skala nasional, dilengkapi grafik ringkasan.

## Struktur Direktori

```text
project-data-wrangling/
├── .gitattributes
├── .gitignore
├── README.md
├── requirements.txt
├── data/
│   └── raw/
│       ├── 6.2.1_ Persentase rumah tangga yang memiliki akses terhadap sanitasi layak menurut kabupaten_kota, 2025.csv
│       ├── batas_kabupaten_kota.geojson
│       ├── KODE-WILAYAH-KEPMENDAGRI-2025.xlsx
│       ├── Persentase Rumah Tangga yang Memiliki Akses terhadap Sanitasi Layak Menurut Provinsi dan Klasifikasi Desa, 2025.xlsx
│       ├── Persentase Rumah Tangga yang Memiliki Akses terhadap Sumber Air Minum Layak Menurut Provinsi dan Klasifikasi Desa, 2025.xlsx
│       └── Persentase Rumah Tangga yang Memiliki Akses terhadap Sumber Air Minum Layak menurut Kabupaten_Kota, 2025.csv
├── notebooks/
│   ├── notebook.ipynb
│   ├── peta-indonesia.ipynb
│   └── scrap-geojson.ipynb
└── outputs/
    ├── analisis_kabupaten_kota_95.csv
    ├── clean_data_kabupaten_kota.csv
    ├── clustering_kmeans_deprivation.png
    ├── data_peta_95.csv
    ├── deprivation_dan_CDI_kabkota_terpilih.png
    ├── evaluasi_jumlah_klaster.csv
    ├── korelasi_air_sanitasi.png
    ├── peta_CDI_95.html
    ├── peta_CDI_nasional_2025.html
    ├── peta_isu_dominan_nasional_2025.html
    ├── peta_kategori_dominan_95.html
    ├── ranking_CDI_provinsi.csv
    ├── ringkasan_klaster.csv
    ├── scatter_deprivation_nasional.png
    └── top15_provinsi_CDI_2025.png
```

## Panduan Instalasi dan Menjalankan Proyek

### 1. Kloning Repositori dan Menarik Objek LFS

Pastikan Git dan Git LFS telah terinstal pada sistem Anda:

```bash
git clone https://github.com/rava-andika/project-data-wrangling.git
cd project-data-wrangling
git lfs install
git lfs pull
```

### 2. Membuat Virtual Environment dan Menginstal Dependensi

Disarankan menggunakan Python versi 3.10 atau yang lebih baru:

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Linux / macOS
source venv/bin/activate

pip install -r requirements.txt
```

### 3. Menjalankan Notebook

- Analisis Utama: Jalankan `notebooks/notebook.ipynb` untuk mereproduksi pipeline data wrangling dari data mentah hingga visualisasi peta interaktif provinsi terpilih (Papua Pegunungan).
- Pemetaan Nasional: Jalankan `notebooks/peta-indonesia.ipynb` untuk membuat peta interaktif choropleth CDI dan kategori isu dominan skala nasional (seluruh kabupaten/kota di Indonesia).
- Pengunduhan Data Spasial (Opsional): Jalankan `notebooks/scrap-geojson.ipynb` jika ingin memperbarui batas polygon GeoJSON langsung dari server ArcGIS BIG.

Eksekusi notebook dapat dilakukan melalui server Jupyter:

```bash
jupyter notebook
```

Atau buka file notebook langsung melalui Visual Studio Code.

## Temuan Utama Analisis

1. Provinsi dengan Tingkat Ketertinggalan Tertinggi: Papua Pegunungan menempati peringkat pertama nasional dengan skor CDI provinsi sebesar 75,38. Angka ini merefleksikan rata-rata persentase rumah tangga yang belum memperoleh akses air minum layak dan sanitasi layak tertinggi di Indonesia.
2. Kondisi Kabupaten/Kota di Papua Pegunungan: Kabupaten Nduga mencatatkan CDI tertinggi yakni 90,53, sedangkan Kabupaten Pegunungan Bintang memiliki CDI terendah di antara daerah dengan data lengkap di provinsi tersebut yakni 52,66.
3. Pola Ketimpangan Multisektor: Wilayah dengan keterbatasan sanitasi tinggi cenderung memiliki tantangan yang sama di sektor air minum, sebagaimana ditunjukkan oleh analisis korelasi dan sebaran klaster K-Means.

## Daftar Berkas Keluaran (Outputs)

Seluruh artefak hasil eksekusi disimpan secara otomatis di direktori `outputs/` tanpa menimpa data mentah:

| Nama Berkas | Format | Deskripsi |
|---|---|---|
| `clean_data_kabupaten_kota.csv` | CSV | Data tabular bersih seluruh kabupaten/kota beserta nilai deprivation dan CDI |
| `ranking_CDI_provinsi.csv` | CSV | Peringkat 38 provinsi diurutkan berdasarkan skor CDI tertinggi |
| `analisis_kabupaten_kota_95.csv` | CSV | Rincian capaian dan skor CDI kabupaten/kota di Papua Pegunungan (kode 95) |
| `evaluasi_jumlah_klaster.csv` | CSV | Evaluasi metrik inertia dan silhouette score pada model K-Means |
| `ringkasan_klaster.csv` | CSV | Profil statistik rerata deprivation untuk setiap klaster K-Means |
| `peta_CDI_95.html` | HTML | Peta interaktif choropleth persebaran skor CDI kabupaten/kota di Papua Pegunungan |
| `peta_kategori_dominan_95.html` | HTML | Peta interaktif kategori isu dominan (Air Minum vs Sanitasi vs Seimbang) Papua Pegunungan |
| `peta_CDI_nasional_2025.html` | HTML | Peta interaktif choropleth persebaran skor CDI seluruh kabupaten/kota di Indonesia (skala nasional) |
| `peta_isu_dominan_nasional_2025.html` | HTML | Peta interaktif kategori isu dominan seluruh kabupaten/kota di Indonesia (skala nasional) |
| `clustering_kmeans_deprivation.png` | PNG | Visualisasi sebaran klaster wilayah pada ruang dimensi deprivation |
| `top15_provinsi_CDI_2025.png` | PNG | Diagram batang 15 provinsi dengan skor CDI tertinggi di Indonesia |
| `korelasi_air_sanitasi.png` | PNG | Grafik regresi dan korelasi antara indikator air minum dan sanitasi |

## Keterbatasan Analisis

1. Pembobotan Setara: CDI memberikan bobot yang sama (50 : 50) pada dimensi air minum dan sanitasi, serta bukan merupakan indikator resmi kemiskinan multidimensi dari instansi pemerintah.
2. Perlakuan Nilai Hilang: Daerah yang memiliki indikator kosong pada salah satu dimensi tidak dihitung nilai CDI-nya untuk menghindari bias estimasi.
3. Keterbatasan Agregasi: Pemeringkatan provinsi menggunakan data agregat BPS pada tingkat provinsi, bukan rata-rata sederhana dari seluruh kabupaten/kota di dalamnya.
4. Karakteristik Deskriptif: Hubungan korelasi dan visualisasi spasial yang disajikan bersifat deskriptif asosiatif, bukan hubungan kausalitas (sebab-akibat).
