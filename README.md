# Analisis Kuantitatif dan Visualisasi Human Trafficking

Penulisan Ilmiah (PI) — analisis statistik deskriptif dan visualisasi data
korban perdagangan manusia global menggunakan Python dan Tableau.

| | |
|---|---|
| **Penulis** | Franzen Alexander Samuel Wattimena |
| **NPM** | 10122525 |
| **Program Studi** | Sistem Informasi — Fakultas Ilmu Komputer dan Teknologi Informasi |
| **Universitas** | Universitas Gunadarma |
| **Pembimbing** | Widya Khafa Nofa, S.Kom., MMSI |
| **Tanggal Sidang / Lulus** | 19 September 2025 |
| **Status** | Selesai — sudah disetujui pembimbing & dinyatakan lulus |

**Dashboard interaktif (Tableau Public):**
<https://public.tableau.com/app/profile/franzen.alexander/viz/VisualisasiHumanTrafficking/Dashboard1>
(pendek: <https://bit.ly/49A664m>)

---

## Ringkasan

Perdagangan manusia adalah kejahatan lintas negara yang butuh pendekatan
berbasis data. PI ini melakukan **analisis kuantitatif deskriptif** terhadap
dataset global korban perdagangan manusia, mulai dari pengumpulan data
sekunder, pembersihan data, statistik deskriptif, sampai visualisasi.

Seluruh pengolahan memakai **Python** (Pandas, NumPy, Matplotlib, Seaborn,
SciPy) di Jupyter Notebook, lalu hasil akhirnya disajikan ulang sebagai
**dashboard interaktif di Tableau Public**.

### Temuan utama (sesuai naskah)

- Korban **perempuan** dan kelompok **anak–remaja (9–17 tahun)** adalah yang
  paling rentan.
- **Eksploitasi seksual** adalah bentuk eksploitasi paling dominan,
  di atas kerja paksa.
- Perekrutan banyak dilakukan oleh **orang terdekat** korban (pasangan,
  teman, keluarga), bukan orang asing.
- Metode kontrol yang menonjol bersifat **non-fisik** (tekanan psikologis,
  ancaman) selain pengendalian ekonomi.

> Catatan: angka pada dataset hanya mencakup **korban yang terdeteksi dan
> dilaporkan** oleh otoritas nasional, sehingga dipengaruhi perbedaan
> kapasitas pencatatan tiap negara (lihat [Keterbatasan data](#keterbatasan-data)).

---

## Struktur folder

```
PI/
├── Franzen Alexander_Penulisan Ilmiah.docx   # Naskah PI final (BAB I–IV + lampiran)
├── analisis_perdagangan_manusia_final.ipynb  # Notebook analisis & visualisasi
│
├── human_trafficking.csv                     # Dataset utama (CTDC) — 48.801 baris
├── human-trafficking-victims.csv             # Dataset pendukung (UNODC/OWID, SDG 16.2.2)
├── human_trafficking_merged_cleaned.csv      # Hasil gabungan + pembersihan
│
├── figs/                                     # Chart PNG hasil notebook
│   ├── Chart_Distribusi_Gender.png
│   └── Chart_Distribusi_Usia.png
├── tables_png/                               # Tabel dirender jadi PNG
│   ├── Tabel_Distribusi_Gender.png
│   └── Tabel_Distribusi_Usia.png
├── exports/                                  # Folder output (dibuat otomatis oleh notebook)
│
└── human-trafficking-victims/                # Data package asli Our World in Data
    ├── readme.md                             #   deskripsi & cara sitasi dari OWID
    ├── human-trafficking-victims.metadata.json
    └── Download Bab III - Metodologi Penelitian (Format Resmi)  # draf bab metodologi (.docx)
```

---

## Dataset

### 1. `human_trafficking.csv` — dataset utama

| | |
|---|---|
| **Sumber** | Counter-Trafficking Data Collaborative (CTDC) — IOM, Polaris, Liberty Shared. Diunduh via Kaggle ("Human Trafficking Global Dataset"). |
| **Ukuran** | 48.801 baris, ±63 atribut |
| **Cakupan** | Data korban global yang teridentifikasi (K-anonymized) |
| **Format** | CSV |
| **Catatan** | Nilai `-99` dipakai sebagai penanda data hilang — di notebook diganti jadi `NaN`. |

Kelompok kolom penting:

- **Demografi:** `gender`, `ageBroad`, `citizenship`, `majorityStatus`
- **Metode kontrol:** `meansOfControl*` (biner 0/1, mis. `...PsychologicalAbuse`,
  `...Threats`, `...DebtBondage`, `...TakesEarnings`)
- **Bentuk eksploitasi:** `isSexualExploit`, `isForcedLabour`,
  `isForcedMarriage`, `isOrganRemoval`, dll. + rincian `typeOfLabour*` /
  `typeOfSex*`
- **Perekrut:** `RecruiterRelationship`, `recruiterRelation*`
- **Lokasi:** `CountryOfExploitation`
- **Waktu:** `yearOfRegistration`

### 2. `human-trafficking-victims.csv` — dataset pendukung

| | |
|---|---|
| **Sumber** | UN Office on Drugs and Crime (GLOTIP Database) — diproses oleh Our World in Data |
| **Indikator** | SDG 16.2.2 — *Detected victims of human trafficking, by age and sex* — **under 18 years old, Female** |
| **Kolom** | `Entity`, `Code`, `Year`, jumlah korban |
| **Rentang waktu** | 2007–2022 |
| **Diunduh** | 25 April 2025 |

Dipakai khusus untuk analisis **tren tahunan korban anak perempuan < 18 tahun**
per negara. Detail metadata & cara sitasi ada di
[`human-trafficking-victims/readme.md`](human-trafficking-victims/readme.md).

### 3. `human_trafficking_merged_cleaned.csv`

Hasil penggabungan dataset utama dengan indikator OWID + pembersihan
(`-99` → `NaN`, hapus duplikat, normalisasi kategori). Dipakai sebagai basis
analisis gabungan.

---

## Notebook analisis

`analisis_perdagangan_manusia_final.ipynb` — 38 sel, Python 3.12.

Tahapan (sesuai urutan sel):

1. **Import Library** — pandas, numpy, matplotlib, seaborn
2. **Load Dataset** — `human_trafficking.csv` + `human-trafficking-victims.csv`
3. **Data Cleaning** — `-99` → `NaN`, hapus duplikat
4. **Statistik Deskriptif** — distribusi gender & usia
5. **Bentuk Eksploitasi** — eksploitasi seksual vs kerja paksa
6. **Negara Eksploitasi Terbanyak** — 10 besar `CountryOfExploitation`
7. **Tren Korban Anak Perempuan < 18 Tahun** — dari dataset OWID, 2007–2022
8. **Korelasi Gender × Jenis Eksploitasi** — crosstab / stacked bar
9. **Pola Usia × Jenis Eksploitasi** — stacked bar per `ageBroad`
10. **Distribusi Metode Kontrol (Means of Control)**
11. **Hubungan Korban & Perekrut**
12. **Negara Eksploitasi × Jenis Eksploitasi** — heatmap
13. **Uji Chi-Square** — `scipy.stats.chi2_contingency` untuk uji hubungan antar variabel
14. **Pipeline ekspor** — helper `save_table_png()` & `simple_bar()` yang
    men-generate isi `figs/`, `tables_png/`, dan `exports/`

Menjalankan **Run All** akan menimpa ulang file di `figs/` dan `tables_png/`.

---

## Cara menjalankan

Butuh Python 3.12 (notebook dibuat dengan Anaconda, environment `base`).

```bash
# 1. Siapkan environment
pip install pandas numpy matplotlib seaborn scipy jupyter notebook

# 2. Jalankan dari root folder ini (path dataset di notebook bersifat relatif)
jupyter notebook analisis_perdagangan_manusia_final.ipynb
```

Dengan conda:

```bash
conda create -n pi python=3.12 pandas numpy matplotlib seaborn scipy jupyter -y
conda activate pi
jupyter notebook analisis_perdagangan_manusia_final.ipynb
```

---

## Tools

| Kegunaan | Tools |
|---|---|
| Pembersihan & manipulasi data | Python, Pandas, NumPy |
| Statistik & uji hipotesis | SciPy (`chi2_contingency`) |
| Visualisasi statis | Matplotlib, Seaborn |
| Lingkungan kerja | Jupyter Notebook (Anaconda) |
| Visualisasi interaktif & heatmap geografis | Tableau Public |

---

## Struktur naskah PI

`Franzen Alexander_Penulisan Ilmiah.docx` (xii + 38 halaman + lampiran):

| Bab | Isi |
|---|---|
| **BAB I — Pendahuluan** | Latar belakang, ruang lingkup, batasan dataset, tujuan, metode, sistematika penulisan |
| **BAB II — Tinjauan Pustaka** | Perdagangan manusia, analisis kuantitatif, Python untuk analisis data, teori Tableau, studi terkait |
| **BAB III — Pembahasan** | Alur penelitian, jenis & pendekatan, sumber data, teknik pengumpulan & analisis, statistik deskriptif (13 sub-analisis), visualisasi Tableau |
| **BAB IV — Penutup** | Kesimpulan & saran |
| **Lampiran** | Output & listing kode |

---

## Keterbatasan data

- **Data sekunder.** Kualitas & cakupan bergantung pada pencatatan lembaga
  sumber (UNODC, ILO, IOM), bukan pengumpulan primer.
- **Hanya kasus terdeteksi.** Banyak kasus tidak terlaporkan (*dark figure*),
  jadi data tidak menggambarkan keseluruhan fenomena.
- **Bias pelaporan antar-negara.** Angka tinggi di sebuah negara bisa berarti
  prevalensi tinggi *atau* sistem pencatatan yang lebih baik.
- **Nilai hilang.** Beberapa variabel (usia, jenis eksploitasi, negara tujuan)
  banyak kosong; sudah ditangani di tahap pembersihan tapi tetap berpotensi
  memengaruhi hasil.
- **Tidak untuk generalisasi** di luar rentang waktu & wilayah yang dicakup
  sumber.

---

## Sitasi data

**Dataset utama (CTDC):**
> Counter-Trafficking Data Collaborative (CTDC) — International Organization
> for Migration, Polaris, Liberty Shared. *Global Victim–Perpetrator
> Synthetic Dataset.* Diakses via Kaggle.

**Dataset pendukung (UNODC / Our World in Data):**
> UN Office on Drugs and Crime – processed by Our World in Data. "16.2.2 -
> Detected victims of human trafficking, by age and sex (number) - VC_HTF_DETV
> - under 18 years old - Female" [dataset]. UN Office on Drugs and Crime,
> "GLOTIP Database" [original data]. Diunduh 25 April 2025.

---

## Lisensi & penggunaan

Repositori akademik. Naskah PI dan analisis adalah karya
Franzen Alexander Samuel Wattimena (Universitas Gunadarma, 2025).
Dataset tetap tunduk pada lisensi & ketentuan sumber masing-masing
(CTDC/Kaggle dan UNODC/Our World in Data) — cek lisensi sumber sebelum
memakai ulang datanya.

---
