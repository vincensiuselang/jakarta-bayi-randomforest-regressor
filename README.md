# Random Forest Regressor — Jakarta Open Data (bayi usia 0–6 bulan)

Repository ini berisi notebook `RandomForest_Regressor.ipynb` yang membangun model regresi menggunakan RandomForest untuk memprediksi jumlah bayi usia kurang dari 6 bulan berdasarkan data wilayah (kecamatan) dan fitur lain yang tersedia.

Ringkasan singkat
- Notebook: `RandomForest_Regressor.ipynb`
- Dataset (Excel): `data-bayi-usia-0-6-bulan-mendapatkan-air-susu-ibu-(asi)-eksklusif-(1759118864404).xlsx`
- Tujuan: membangun, mengevaluasi, dan melakukan tuning hyperparameter pada model `RandomForestRegressor`.

Apa yang ada di notebook
- Import library (pandas, numpy, seaborn, matplotlib, scikit-learn)
- Muat data dari file Excel (`pandas.read_excel` + `openpyxl` sebagai engine)
- Eksplorasi data (dtypes, info, missing, boxplot untuk outlier)
- One-hot encoding pada kolom `kecamatan` (menggunakan `OneHotEncoder`)
- Drop kolom `wilayah`
- Split data (`train_test_split`) dan (opsional) scaling (`StandardScaler`)
- Train: `RandomForestRegressor` (fit pada data train)
- Evaluasi: MSE, MAE, R², RMSE (notebook menggunakan `sklearn.metrics.root_mean_squared_error`)
- Learning curve dan GridSearchCV untuk hyperparameter tuning

Tahap Pra-pemrosesan (detail sesuai notebook)
Notebook melakukan pra-pemrosesan sederhana yang mengikuti langkah-langkah ini (potongan kode diambil/dimiliki agar mudah direplikasi):

1. Inspect & cek missing
- Periksa tipe kolom, informasi ringkas, dan jumlah nilai kosong.
- Notebook hanya mencetak hasil pemeriksaan (`df.dtypes`, `df.info()`, `df.isnull().sum()`); jika ada missing value, Anda bisa memilih imputasi atau drop sesuai kebutuhan.

2. Cek outlier
- Notebook menggambarkan boxplot semua kolom numerik untuk melihat outlier tetapi memutuskan untuk tidak menghapusnya karena data merepresentasikan populasi wilayah nyata.

3. One-hot encode kolom `kecamatan`
- Notebook menggunakan `OneHotEncoder(sparse_output=False)` lalu menggabungkan kolom encoded ke DataFrame dan menghapus kolom `kecamatan` asli.

Contoh (sesuai notebook):

```text
from sklearn.preprocessing import OneHotEncoder
encoder = OneHotEncoder(sparse_output=False)
encoded = encoder.fit_transform(df[["kecamatan"]])
encoded_df = pd.DataFrame(encoded, columns=encoder.get_feature_names_out(["kecamatan"]))
df_ohe = pd.concat([df.drop(columns=["kecamatan"]), encoded_df], axis=1)
```

4. Drop kolom `wilayah`
- Notebook memanggil `df_ohe.drop(columns=["wilayah"], inplace=True)`.

5. Pilih fitur dan target
- Target yang dipakai di notebook: `jumlah_bayi_usia__kurang_6_bulan`.

```text
X = df_ohe.drop(columns=["jumlah_bayi_usia__kurang_6_bulan"])  # fitur
y = df_ohe["jumlah_bayi_usia__kurang_6_bulan"]  # target
```

6. Split data
- Notebook memakai `train_test_split` dengan `test_size=0.2` dan `random_state=42`.

```text
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

7. Scaling (opsional)
- Notebook membuat `StandardScaler` dan menghasilkan `X_train_scaled` dan `X_test_scaled`, tetapi contoh training di notebook memakai `X_train` (tanpa scaling). Jika Anda ingin menggunakan scaling, latih model dengan data yang telah discale.

```text
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

Catatan penting tentang missing values dan outlier
- Notebook hanya memeriksa missing values; tidak melakukan imputasi otomatis. Jika dataset Anda memiliki missing, disarankan untuk memilih strategi imputasi (mean/median/most_frequent) atau menghapus baris/kolom terkait sebelum encoding.
- Untuk outlier, notebook memilih untuk tidak menghapus karena nilai tersebut dianggap representasi real dari wilayah.

Dependensi
Semua dependensi dicantumkan di `requirements.txt`. Paket penting yang dipakai di notebook:
- pandas (membaca/manipulasi data)
- numpy (numerik)
- scikit-learn (modeling, preprocessing, metrics — membutuhkan `>=1.2` untuk `root_mean_squared_error`)
- matplotlib (plot)
- seaborn (visualisasi)
- openpyxl (engine untuk membaca `.xlsx`)
- jupyter (opsional, menjalankan notebook)

Instruksi cepat (Windows, cmd.exe)
1) Buat virtual environment dan aktifkan:

```bat
python -m venv venv
venv\Scripts\activate
```

2) Install dependensi:

```bat
pip install --upgrade pip
pip install -r "c:\\Users\\formylife\\formylife\\BELAJAR MACHINE LEARNING\\latihan\\jakartaopendata_bayi\\requirements.txt"
```

3) Jalankan notebook:
- Buka `jupyter lab` atau `jupyter notebook` di folder proyek, atau buka `RandomForest_Regressor.ipynb` di editor yang mendukung notebook (mis. PyCharm, VS Code).

Contoh menjalankan Jupyter di folder proyek (cmd):

```bat
cd "c:\\Users\\formylife\\formylife\\BELAJAR MACHINE LEARNING\\latihan\\jakartaopendata_bayi"
jupyter lab
```

Catatan & tips
- Notebook membuat `X_train_scaled` dan `X_test_scaled` menggunakan `StandardScaler`, tetapi model pada contoh dilatih menggunakan `X_train` (tanpa scaling). Bila Anda ingin memakai scaling untuk model, latih model dengan `X_train_scaled` dan gunakan `X_test_scaled` untuk prediksi.
- GridSearchCV dapat memakan waktu tergantung ukuran data dan kombinasi hyperparameter; `n_jobs=-1` digunakan untuk memanfaatkan semua core CPU.
- Pastikan `scikit-learn` >= 1.2 supaya fungsi `root_mean_squared_error` tersedia. Jika Anda menggunakan versi lebih lama, Anda bisa hitung RMSE manual. Contoh self-contained (dapat langsung dijalankan):

Reproduksibilitas
- Tetapkan `random_state` di `train_test_split` dan `RandomForestRegressor` (notebook sudah memakai `random_state=42`).
- Untuk reproduksi jangka panjang, Anda bisa pin versi paket dengan `pip freeze > requirements.txt` setelah menguji environment.

Author
Vincensiuselang

Lisensi
Atur sesuai kebutuhan (mis. MIT). Jika Anda ingin, saya bisa menambahkan file `LICENSE`.

---
Jika mau, saya bisa:
- Mengunci versi dependensi (pinned `requirements.txt`) berdasarkan environment yang Anda gunakan
- Membuat `environment.yml` untuk conda
- Menambahkan instruksi singkat untuk menjalankan sel tertentu di notebook (contoh: training, evaluation)
