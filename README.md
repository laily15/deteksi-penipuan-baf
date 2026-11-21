# Deteksi Penipuan Pembukaan Rekening Bank

## Metode dan Alasannya

Saya pakai algoritma LightGBM digabung dengan One-Hot Encoding (buat ngubah data teks/kategori jadi angka) dan pengaturan bobot kelas (`class_weight="balanced"`) karena data penipu jumlahnya jauh lebih sedikit daripada nasabah jujur.

Alasannya pakai LightGBM: prosesnya cepat untuk data yang banyak, cocok buat data tabel, dan ngasih nilai peluang (0 sampai 1) sehingga batas keputusannya bisa diatur agar nasabah jujur yang salah tertandai tidak lebih dari 5%.

## Pembagian Data dan Alasannya

Data `Base.csv` diambil yang `month` 0 sampai 3, lalu dibagi berdasarkan urutan waktu:

- **Data latih (`month` 0–2):** 397.039 baris (penipu sekitar 0,98%)
- **Data validasi (`month` 3):** 150.936 baris (penipu sekitar 0,92%)

Alasannya: pengajuan rekening itu berjalan dari bulan ke bulan. Menguji model di bulan berikutnya (`month` 3) lebih masuk akal untuk melihat apakah model tetap jalan saat pola data berubah, dibanding diacak campur aduk.

## Hasil di Data Validasi (`month` 3)

- **ROC-AUC:** `0.9097`
- **Batas skor / threshold (FPR ≤ 5%):** `0.807555`
- **Nasabah jujur yang salah tertandai (FPR):** `4.92%`
- **Penipu yang berhasil tertangkap (TPR pada FPR ≤ 5%):** `55.82%`
