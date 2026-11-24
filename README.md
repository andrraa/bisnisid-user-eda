# Analisis Data Eksploratif (EDA) Pengguna Portal User Terpusat Bisnis.id

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)

Proyek ini berisi Analisis Data Eksploratif (EDA) terhadap data pendaftaran dan kelengkapan profil pengguna Portal User Terpusat Bisnis.id (`bisnis_id_prod.User`) sebagai tugas Ujian Tengah Semester (UTS) mata kuliah Data Science (`PSIKS2W002`).

## Gambaran Umum dan Temuan Utama

Tabel `User` di database produksi menyimpan 762.792 akun terdaftar dalam 48 kolom dari April 2012 hingga November 2026. Karena penanda lama `is_retained_user` sudah tidak aktif dipakai di sistem, analisis diarahkan pada kelengkapan profil demografis (`has_demografi` dan `profile_completeness_pct`) serta kanal pendaftaran (`auth_channel`). Seluruh eksplorasi dijalankan menggunakan sampel terstratifikasi yang sudah dianonimkan sebanyak 15.003 baris dan 22 kolom.

Dari hasil eksekusi notebook `EDA_Users_BisnisID_UTS.ipynb`, ditemukan beberapa pola utama:

- Sebanyak 82,1% akun (12.317 dari 15.003 sampel) mendaftar melalui Google SSO, diikuti jalur Direct Email / Legacy sebesar 17,0%.
- Lebih dari 93% kolom demografi (`gender_clean`, `age_years_raw`, `education`, `outcome_clean`) kosong dengan pola *Missing at Random* (MAR), terlihat dari tingkat keterisian jenis kelamin sebesar 1,61% pada pengguna Google SSO dibandingkan 29,58% pada pengguna Direct Email / Legacy.
- Akun yang mengisi minimal satu data demografi (`has_demografi = 1`, sebanyak 6,33% dari baris bersih) memiliki korelasi positif dengan `profile_completeness_pct` (+0,887), `is_migration` (+0,682), dan `account_age_days` (+0,367), serta korelasi negatif dengan `sso_channel_count` (-0,407).
- Proses pembersihan data menghapus kolom `login_recency_days` yang kosong 98,41% dan 101 baris dengan tanggal daftar `'0000-00-00'` (0,67%), sehingga tersisa 14.902 baris bersih sambil tetap mempertahankan umur akun lama sejak 2012.

## Prasyarat dan Cara Menjalankan

1. Clone repositori ini lalu masuk ke direktori proyek:

   ```bash
   git clone https://github.com/haluboyys/bisnisid-user-eda.git
   cd bisnisid-user-eda
   ```

2. Pasang pustaka Python yang diperlukan:

   ```bash
   pip install -r requirements.txt
   ```

3. Buka dan jalankan notebook:

   ```bash
   jupyter notebook EDA_Users_BisnisID_UTS.ipynb
   ```

## Penggunaan dan Privasi Data

Seluruh output kode, tabel statistik, dan delapan grafik visualisasi sudah dijalankan dan tersimpan di dalam `EDA_Users_BisnisID_UTS.ipynb`, sehingga langsung tampil saat dibuka di GitHub. Susunan notebook mengikuti sembilan bab panduan EDA, mulai dari perumusan masalah, pemahaman data, inspeksi awal, penanganan nilai kosong, statistik deskriptif, transformasi data, analisis hubungan antarvariabel, penanganan pencilan, hingga kesimpulan.

Untuk mematuhi prinsip pelindungan data pribadi (UU PDP), repositori ini tidak menyimpan data sensitif seperti alamat email utuh, nama lengkap, nomor telepon, alamat, hash kata sandi, maupun kredensial database. Kolom teks telah dianonimkan menjadi panjang karakter (`fullname_len`, `username_len`) atau dikelompokkan ke dalam kategori umum.

<details><summary>Struktur Proyek</summary>

```text
bisnisid-user-eda/
├── data/
│   └── users_bisnisid_sample.csv              # Sampel terstratifikasi anonim (15.003 baris, 22 kolom)
├── figures/
│   ├── fig1_tren_dan_kanal.png                # Tren pendaftaran tahunan dan distribusi auth_channel
│   ├── fig2_missing_values.png                # Persentase nilai kosong per kolom
│   ├── fig3_distribusi_skewness.png           # Distribusi numerik dengan skewness dan kurtosis
│   ├── fig4_transformasi_log.png              # Perbandingan transformasi log pada account_age_days
│   ├── fig5_univariate.png                    # Analisis univariat (kelengkapan, has_demografi, Active)
│   ├── fig6_bivariate.png                     # Umur akun terhadap kelengkapan profil dan has_demografi
│   ├── fig7_korelasi_heatmap.png              # Heatmap korelasi Spearman
│   └── fig8_outliers.png                      # Boxplot deteksi pencilan (usia dan umur akun)
├── report/
│   ├── EDA_AndraRamadanPratama_26MA41D006.docx
│   └── EDA_AndraRamadanPratama_26MA41D006.pdf
├── .gitignore
├── EDA_Users_BisnisID_UTS.ipynb               # Notebook EDA siap tampil (Bab 1 sampai Bab 9)
├── README.md
└── requirements.txt
```

</details>

## Verifikasi

Untuk memastikan dataset, struktur notebook, dan seluruh hasil eksekusinya masih utuh, jalankan perintah berikut:

```bash
python -c "import nbformat, pandas as pd; df = pd.read_csv('data/users_bisnisid_sample.csv'); nb = nbformat.read('EDA_Users_BisnisID_UTS.ipynb', as_version=4); nbformat.validate(nb); print('CSV:', df.shape, '| Cells:', len(nb.cells))"
```

## Kontribusi

Repositori ini dibuat untuk keperluan tugas kuliah Program Pascasarjana. Masukan atau perbaikan terkait alur analisis tetap terbuka melalui *issue* maupun *pull request*.

## Lisensi

Repositori ini disusun untuk tugas akademik dan belum menyertakan berkas `LICENSE` secara khusus.
