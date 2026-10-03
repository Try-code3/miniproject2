# Ijazah Document Verification Pipeline 🎓

Sistem berbasis Computer Vision (OpenCV) dan Optical Character Recognition (Tesseract) untuk mengotomatisasi ekstraksi nomor seri ijazah dan memverifikasi kehadiran tanda tangan Rektor.

## ⚙️ Persyaratan Sistem (Prerequisites)

- Python 3.8+
- [Tesseract OCR Engine](https://github.com/tesseract-ocr/tesseract) terinstal di sistem OS Anda.
- Library Python: `opencv-python`, `pytesseract`, `numpy`, `matplotlib`

## 🚀 Cara Menjalankan Kode (How to Run)

1. **Clone Repository:**
   ```bash
   git clone [https://github.com/Try-code3/miniproject2]

2. Instalasi Dependencies:
   pip install -r requirements.txt

3. Menjalankan Program (Google Colab / Jupyter):
  Buka file miniproject_cv.ipynb.

  Jalankan sel (cell) pertama untuk menginstal environment.
  
  Pada sel eksekusi utama, unggah gambar ijazah (format .jpg atau .png) saat diminta oleh prompt files.upload().
  
  Sistem akan memproses setiap citra secara iteratif.

## cara menjalanakan bisa langsung di download file .ipynb nya lewat repository

-----Pipeline Arsitektur-----------
Grayscale conversion (Orientasi lanskap matriks bawaan).

ROI Cropping berdasarkan persentase spasial area dokumen.

Number OCR: Upscaling 2x -> Gaussian Blur -> Otsu Thresholding -> Pytesseract (--psm 7).

Signature Verification: Inverse Otsu Thresholding -> Morphological Closing -> Ink Density Threshold > 1.5%.
