# Sistem Case-Based Reasoning (CBR) Analisis Putusan Pajak

Repositori ini memuat implementasi Sistem Case-Based Reasoning (CBR) untuk mendukung analisis putusan pengadilan di bidang pajak, merujuk pada siklus CBR. Proyek ini disusun untuk memenuhi Penugasan Sub CPMK-3 Mata Kuliah Penalaran Komputer.


## Kelas
Kelas Penalaran Komputer A — diampu oleh Ir. Galih Wasis Wicaksono, S.Kom., M.Cs.

## Nama Kelompok
1. Agus Susanto (202310370311373)
2. Boby Yohansyah (202310370311374)

## Struktur Direktori
```text
/data/          # Folder untuk raw data (*.txt) dan processed data (*.csv/*.json)
/notebooks/     # Jupyter notebooks yang dibagi per tahap CBR
/PDFs_Putusan/  # Folder berisi file PDF asli putusan (>=30 dokumen)
/logs/          # Folder untuk menyimpan file log (opsional)
README.md       # Dokumentasi petunjuk instalasi dan eksekusi
requirements.txt# Daftar dependencies yang dibutuhkan
```

## Cara Menjalankan
Untuk menjalankan proyek ini, ikuti langkah-langkah berikut:

### Clone Repository
```bash
git clone [URL_REPOSITORY_ANDA]
cd [NAMA_FOLDER_REPOSITORY]
```

### Siapkan Virtual Environment (Direkomendasikan)
```bash
python -m venv venv
source venv/bin/activate  # Pada Windows gunakan `venv\Scripts\activate`
```

### Install Dependencies
Pastikan Anda memiliki Python 3.x. Install semua library yang dibutuhkan menggunakan file `requirements.txt`.
```bash
pip install -r requirements.txt
```
*Catatan: Library `torch` dan `transformers` mungkin memerlukan waktu untuk diunduh.*

### Struktur Folder Google Drive
Kode ini dirancang untuk bekerja dengan Google Drive. Buatlah struktur folder berikut di dalam Google Drive Anda dan sesuaikan path `BASE_DRIVE_PATH` di dalam setiap notebook jika perlu.

```text
/MyDrive/
└── CBR_Project_Penalaran_Komputer/
    ├── data/
    │   ├── raw/
    │   ├── processed/
    │   ├── eval/
    │   └── results/
    ├── logs/
    ├── PDFs_Putusan/
    ├── Scraper_CSVs/
    └── notebooks/  (tempat Anda menyimpan file .ipynb)
```

## Cara Menjalankan Pipeline End-to-End
Terdapat dua opsi untuk menjalankan pipeline CBR ini: Anda dapat menjalankannya sekaligus atau per tahap.

**Opsi 1: Menjalankan Seluruh Pipeline Sekaligus**
Anda dapat menjalankan satu file Jupyter Notebook utama yang sudah mencakup semua tahap:
- `notebooks/00_Pipeline_Utama_CBR.ipynb`

**Opsi 2: Menjalankan per Tahap**
Sistem CBR ini juga dipecah menjadi 5 tahap. Jalankan masing-masing notebook di dalam folder `notebooks/` secara berurutan:

1. **Tahap 1: Membangun Case Base (`notebooks/01_Membangun_Case_Base.ipynb`)**
   - Berfungsi untuk mengekstrak teks dari PDF putusan yang ada di folder `PDFs_Putusan` dan melakukan pembersihan (cleaning).
   - *Output*: File `.txt` di `data/raw/` dan `cleaning.log` di `logs/`.

2. **Tahap 2: Case Representation (`notebooks/02_Case_Representation.ipynb`)**
   - Berfungsi untuk mengekstrak metadata dan ringkasan dari teks putusan.
   - *Output*: File `cases_processed.csv` di `data/processed/`.

3. **Tahap 3: Case Retrieval (`notebooks/03_Case_Retrieval.ipynb`)**
   - Melakukan embedding (TF-IDF/BERT) dan membagi data (Train/Test). Mencari kasus termirip.
   - *Output*: File `queries.json` di `data/eval/`.

4. **Tahap 4: Case Solution Reuse (`notebooks/04_Case_Solution_Reuse.ipynb`)**
   - Memprediksi amar putusan untuk kasus baru dengan mengambil referensi (solusi) dari kasus sebelumnya yang termirip.
   - *Output*: File `predictions.csv` di `data/results/`.

5. **Tahap 5: Model Evaluation (`notebooks/05_Model_Evaluation.ipynb`)**
   - Mengukur kinerja retrieval dan prediksi dengan metrics: Accuracy, Precision, Recall, F1-Score.
   - *Output*: File `retrieval_metrics.csv` dan `prediction_metrics.csv` di `data/eval/`.
