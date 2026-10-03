## 1. Analisis Hasil Pengujian 9 Citra Ijazah

Berdasarkan output terminal yang dihasilkan dari proses pipeline, berikut adalah rincian hasil pembacaan untuk kesembilan citra uji:
[03_Blurred (1).jpg]
Nomor Ijazah : Nomor ijazah: 571012022000056   
Tanda Tangan : PRESENT  

[05_LowResolution_Upsampled (1).jpg]
Nomor Ijazah : Nomor yazah: 571012022000056   
Tanda Tangan : PRESENT   

[06_Faded_Underexposed (1).jpg]
Nomor Ijazah : - Nomor yazah: 571012022000056   
Tanda Tangan : PRESENT   

[07_ColorShift_WarmTint (1).jpg]
Nomor Ijazah : - Nomor yazah: 571012022000056   
Tanda Tangan : PRESENT   

[08_JPEGCompression_Artifacts (1).jpg]
Nomor Ijazah : Nomor yazah: 571012022000056   
Tanda Tangan : PRESENT   

[01_HighQuality_Enhanced (1).jpg]
Nomor Ijazah : - Nomor yazah: 571012022000056   
Tanda Tangan : PRESENT   

[02_LowContrast (1) (1).jpg]
Nomor Ijazah : - Nomor yazah: 571012022000056   
Tanda Tangan : PRESENT   

[04_HighNoise (1) (1).jpg]
Nomor Ijazah : -Nomor yazah: 571012022000056   
Tanda Tangan : PRESENT   

[09_CombinedDegradation (1) (1).jpg]
Nomor Ijazah : - Nomor tyazah: 571012022000056   
Tanda Tangan : PRESENT   

Berdasarkan output terminal yang dihasilkan, sistem pipeline yang dibangun terbukti sangat tangguh (robust).   Keberhasilan Kritis: Bagian paling krusial, yaitu angka seri ijazah 571012022000056, berhasil diekstrak dengan akurasi 100% pada kesembilan gambar, terlepas dari degradasi yang diberikan seperti blur, noise, kontras rendah, atau kompresi JPEG.   Deteksi Tanda Tangan: Sistem berhasil memverifikasi kehadiran tanda tangan rektor dengan akurasi 100%, menghasilkan status PRESENT pada seluruh gambar uji.   Kesalahan Minor OCR: Terdapat sedikit kesalahan pembacaan (substitusi/insersi karakter) pada teks pengantar, di mana kata "ijazah" terbaca sebagai "yazah" atau "tyazah" pada beberapa gambar seperti 01_HighQuality, 05_LowResolution, dan 09_CombinedDegradation. Satu-satunya gambar yang menghasilkan teks sempurna tanpa typo adalah 03_Blurred (1).jpg. Kesalahan minor ini wajar terjadi pada font serif/tipis setelah proses binarization.   

## Metode yang digunakan 
1. metode pembacaan nomor ijazah
   Cropping (ROI): Memotong area spesifik kiri atas matriks citra yang menampung teks nomor ijazah.

    Upscaling (Pembesaran): Area di- resize menjadi 2x lipat. Enjin OCR Tesseract membutuhkan resolusi tinggi (idealnya 300 DPI) agar dapat mengenali sudut dan lekukan karakter huruf.
    
    Gaussian Blur: Filter blur spasial diaplikasikan untuk menghaluskan tekstur kertas dan membuang noise (titik-titik kotor) tanpa merusak bentuk huruf.
    
    Otsu's Thresholding: Mengubah citra menjadi biner (teks hitam pekat, latar putih murni) menggunakan pencarian nilai ambang batas (threshold) otomatis berdasarkan variansi histogram citra.
    
    Tesseract OCR: Menggunakan konfigurasi Page Segmentation Mode (--psm 7) yang mengasumsikan gambar adalah satu baris teks, serta menerapkan whitelist agar Tesseract hanya menebak huruf, angka, dan spasi.

2. metode pembacaan tanda tangan
   Cropping (ROI): Memotong area kiri bawah yang merupakan letak tanda tangan rektor.
   Inverse Otsu Thresholding: Citra diubah menjadi biner terbalik (tinta pulpen menjadi putih/nilai 255, sedangkan kertas menjadi hitam/nilai 0).
   Morphological Closing: Operasi dilasi yang diikuti erosi. Operasi ini menambal lubang atau menyambungkan garis tinta tanda tangan yang tipis dan terputus akibat kualitas scan.
   Pixel Calculation: Menghitung persentase piksel putih (tinta) dibagi total piksel keseluruhan area ROI. Jika persentase kepadatan melampaui ambang batas minimum (contoh: 1.5%), sistem memvonis tanda tangan PRESENT. Jika di bawah itu, ABSENT. 

## Metode anhacement yang paling efektif berdasarakan nilai CER
Metode enhancement yang paling efektif dalam pipeline ini adalah kombinasi Gaussian Blur + Otsu Thresholding.

Alasan: Pada citra seperti 04_HighNoise, noise frekuensi tinggi (bintik hitam) sering kali dianggap sebagai huruf oleh OCR, yang akan menaikkan tingkat Insertion (I) sehingga CER melonjak buruk. Gaussian Blur meratakan bintik-bintik noise tersebut agar menyatu dengan latar, sementara Otsu Thresholding secara adaptif mencari batas tengah yang memisahkan teks asli (yang lebih gelap) dari sisa noise yang sudah dikaburkan. Buktinya, sistem mampu membaca deret angka "571012022000056" dengan sempurna (CER = 0 pada bagian angka) pada citra dengan kontras rendah maupun kompresi tinggi.
