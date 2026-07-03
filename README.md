# Face Recognition of Famous Figures

Proyek pengenalan wajah (face recognition) menggunakan **dlib** (deteksi wajah + landmark + 128-D face descriptor) dan **SVM (scikit-learn)** sebagai classifier.

## Struktur Proyek

```
dataset/train/<Nama_Orang>/*.jpg          # dataset wajah untuk training (format LFW)
model/face_classifier.pkl                 # classifier SVM yang sudah dilatih
predictor/shape_predictor_68_face_landmarks.dat       # model landmark wajah dlib (68 titik)
predictor/dlib_face_recognition_resnet_model_v1.dat   # model ekstraksi fitur wajah (ResNet, 128-D)
whl_binary_file/dlib-19.24.99-cp312-cp312-win_amd64.whl  # wheel dlib prebuilt untuk Windows (Python 3.12, 64-bit)
detect.ipynb        # notebook untuk deteksi & klasifikasi wajah pada satu gambar (inference)
experiment.ipynb    # notebook untuk training ulang model dari dataset + evaluasi
requirements.txt    # daftar dependency Python
```

> Model dan file predictor **sudah tersedia** di repo ini, jadi kamu **tidak wajib** menjalankan `experiment.ipynb` (training) hanya untuk mencoba `detect.ipynb` (inference).

## Requirements

- **Python 3.12 (64-bit)** — wajib versi ini karena wheel `dlib` yang disertakan dikompilasi khusus untuk `cp312` (Python 3.12) arsitektur `win_amd64`. Versi Python lain akan gagal saat instalasi dlib.
- Git (untuk clone repo)
- Windows 10/11 64-bit

## Instalasi di Windows

### 1. Cek versi Python

```powershell
python --version
```

Pastikan hasilnya `Python 3.12.x`. Kalau belum ada, download dari [python.org](https://www.python.org/downloads/) dan centang "Add Python to PATH" saat instalasi.

### 2. Clone repository

```powershell
git clone https://github.com/78LUKMAN/face_recognition_of_famous_figures.git
cd face_recognition_of_famous_figures
```

### 3. Buat virtual environment

```powershell
py -3.12 -m venv myenv
```

### 4. Aktifkan virtual environment

PowerShell / CMD:
```powershell
.\myenv\Scripts\activate
```

Git Bash:
```bash
source myenv/Scripts/activate
```

> Jika PowerShell menolak menjalankan script (`execution of scripts is disabled`), jalankan sekali:
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```

### 5. Install dependency Python

```powershell
pip install --upgrade pip
pip install -r requirements.txt
```

### 6. Install dlib dari wheel prebuilt

Wheel dlib sudah disediakan di folder `whl_binary_file/` supaya kamu **tidak perlu** meng-compile dlib dari source (yang butuh CMake + Visual Studio Build Tools dan makan waktu lama). Install langsung:

```powershell
pip install whl_binary_file/dlib-19.24.99-cp312-cp312-win_amd64.whl
```

Jika muncul error `... is not a supported wheel on this platform`, artinya Python aktif di venv-mu bukan Python 3.12 64-bit. Cek ulang dengan `python --version` dan `python -c "import struct; print(struct.calcsize('P')*8)"` (harus keluar `64`).

### 7. Jalankan notebook

```powershell
jupyter notebook
```

Lalu buka salah satu:

- **`detect.ipynb`** — untuk mencoba deteksi & klasifikasi wajah pada sebuah gambar menggunakan model yang sudah ada (`model/face_classifier.pkl`). Notebook ini akan mencoba mengunduh gambar contoh dari internet; jika gagal (misalnya tidak ada koneksi), kamu akan diminta memasukkan path gambar lokal secara manual.
- **`experiment.ipynb`** — untuk melatih ulang classifier dari `dataset/train/`, mengevaluasi akurasi per kelas, dan melihat confusion matrix.

## Troubleshooting

| Masalah | Penyebab & Solusi |
|---|---|
| `pip install -r requirements.txt` error karena `pickle` | `pickle` adalah modul bawaan Python, bukan package pip — sudah dihapus dari `requirements.txt` versi ini. |
| `ERROR: Could not find a version that satisfies the requirement cv2` | Nama package pip untuk OpenCV adalah `opencv-python`, bukan `cv2` (`cv2` cuma nama modul saat `import cv2`) — sudah diperbaiki di `requirements.txt` versi ini. |
| Wheel dlib gagal diinstall | Pastikan pakai Python 3.12 64-bit persis (lihat langkah 6). |
| `ModuleNotFoundError: No module named 'seaborn'` saat menjalankan `experiment.ipynb` | Package ini dipakai untuk visualisasi confusion matrix tapi belum ada di `requirements.txt` asli — sudah ditambahkan di versi ini. |
| Notebook tidak bisa dibuka / `jupyter` command not found | Install dulu: `pip install jupyter notebook ipykernel` (sudah termasuk di `requirements.txt` versi ini). |

## Catatan

- File `.dat` di folder `predictor/` cukup besar (± 22 MB dan ± 99 MB) — pastikan `git clone` selesai sepenuhnya sebelum menjalankan notebook.
- Dataset di `dataset/train/` mengikuti struktur LFW: satu folder per nama orang, berisi beberapa foto wajah orang tersebut.
