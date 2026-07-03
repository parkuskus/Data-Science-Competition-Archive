Dataset Description
Dataset terdiri dari 4096 data polis asuransi kesehatan dan 5781 data klaim asuransi kesehatan yang terjadi pada periode 1 Januari 2024 s/d 31 Juli 2025
Terdapat 5 informasi yang diberikan terkait data polis, yaitu: nomor polis, plancode, gender, tanggal lahir, tanggal efektif polis, dan domisili, dan terdapat 13 informasi yang diberikan terkait data klaim asuransi kesehatan, yaitu: claim ID, nomor polis, reimburse/cashless, inpatient/outpatient, ICD Diagnosis, ICD Description, status klaim, tanggal pembayaran, tanggal pasien masuk RS, tanggal pasien keluar RS, nominal klaim yang disetujui, nominal biaya RS yang terjadi, dan lokasi RS.
Berkas Data

* `Data_Klaim.csv` – Data transaksi klaim asuransi kesehatan hingga periode 2025-07-31.
* `Data_Polis.csv` – Data induk yang berisi informasi seluruh polis aktif.
* `sample_submission.csv` – Berkas contoh format pengumpulan (submisi) hasil prediksi/analisa.
Deskripsi Kolom
1. `Data_Klaim.csv`
Daftar kolom yang tersedia pada data transaksi klaim:

* Claim ID: Identifikasi unik untuk setiap klaim asuransi kesehatan yang diajukan oleh tertanggung.
* Nomor Polis: Nomor identitas unik dari polis asuransi milik tertanggung.
* Reimburse/Cashless: Metode penyelesaian klaim; baik melalui sistem ganti rugi (reimburse) maupun tanpa tunai (cashless).
* Inpatient/Outpatient: Kategori perawatan; Rawat Inap (Inpatient) atau Rawat Jalan (Outpatient).
* ICD Diagnosis: Kode klasifikasi penyakit berdasarkan International Statistical Classification of Diseases and Related Health Problems Revision. Analisa dapat menggunakan kode spesifik (contoh: `H26.9`) atau pengelompokan umum (contoh: `H26`).
* ICD Description: Deskripsi medis atau penjelasan mengenai diagnosis berdasarkan kode ICD terkait.
* Status Klaim: Status pemrosesan klaim (`Paid` = Klaim telah dibayarkan, `Pending` = Klaim dalam proses verifikasi).
* Tanggal Pembayaran Klaim: Tanggal ketika dana klaim dicairkan kepada nasabah atau pihak RS.
* Tanggal Pasien Masuk RS: Tanggal dimulainya perawatan medis di rumah sakit.
* Tanggal Pasien Keluar RS: Tanggal selesainya perawatan medis (kepulangan) dari rumah sakit.
* Nominal Klaim Yang Disetujui: Nilai nominal biaya kesehatan yang disetujui untuk dibayarkan oleh perusahaan asuransi.
* Nominal Biaya RS Yang Terjadi: Total tagihan biaya rumah sakit yang diajukan oleh nasabah.
* Lokasi RS: Letak geografis atau wilayah tempat rumah sakit berada.
2. `Data_Polis.csv`
Daftar kolom yang tersedia pada data profil polis nasabah:

* Nomor Polis: Nomor identitas unik yang menghubungkan data polis dengan data klaim.
* Plan Code: Kode produk yang menentukan cakupan wilayah pertanggungan:
* `M-001`: Wilayah pertanggungan Seluruh Dunia (Worldwide).
* `M-002`: Wilayah pertanggungan regional Asia.
* `M-003`: Wilayah pertanggungan domestik Indonesia.
* Gender: Jenis kelamin pemegang polis/tertanggung.
* Tanggal Lahir: Tanggal lahir tertanggung (digunakan untuk kalkulasi usia).
* Tanggal Efektif Polis: Tanggal awal mulai berlakunya proteksi asuransi kesehatan.
* Domisili: Wilayah tempat tinggal atau alamat resmi tertanggung.

Overview
Produk asuransi kesehatan individu merupakan salah satu alat untuk mitigasi resiko finansial individu dari kejadian tidak terduga seperti penyakit atau kecelakaan, yang berpotensi menimbulkan biaya besar dan berdampak ke perencaan keuangan pribadi.
Salah satu isu yang terjadi beberapa tahun terakhir ini, adalah terkait peningkatan klaim asuransi kesehatan individu, dimana terjadi kenaikan sebesar 25.5% antara periode Januari – Juni 2025 jika dibandingkan periode yang sama di tahun 2024. Adapun peningkatan klaim tersebut akan berdampak ke penyesuaian premi yang berpotensi mengakibatkan harga premi produk asuransi kesehatan individu menjadi lebih tidak terjangkau. Oleh karena itu, diperlukan analisa berbasis data untuk memprediksi faktor-faktor yang paling berpengaruh terhadap nilai klaim, sehingga dapat dilakukan inisiatif baik dari segi seleksi resiko, pencegahan, ataupun deteksi dini untuk meminimalisir dampak peningkatan klaim guna menjaga harga premi yang terjangkau
Pada DSC MCF ITB, untuk komponen penilaian skor Kaggle, peserta ditantang untuk membangun model prediktif yang mampu memprediksi trend frekuensi, trend severitas, dan tren total claim. untuk periode Agustus hingga Desember 2025.
Start
Feb 14, 2026
Close
Mar 15, 2026
Submission
Peserta diwajibkan untuk mengirimkan file prediksi melalui kaggle dalam format .CSV dengan struktur sebagai berikut:

```asciidoc
id, value
2025_08_Claim_Frequency,0
2025_08_Claim_Severity,0
2025_08_Total_Claim,0
2025_09_Claim_Frequency,0
...

```

Evaluation
Metrik yang digunakan untuk mengevaluasi performa model pada kompetisi Kaggle ini adalah Mean Absolute Percentage Error (MAPE):


Perhitungan Skor Akhir
Skor akhir dihitung dengan langkah-langkah berikut:

1. Hitung MAPE untuk prediksi Frekuensi: 

2. Hitung MAPE untuk prediksi Severitas: 

3. Hitung MAPE untuk prediksi Total: 

4. Skor Akhir:

Mape dipilih karena MAPE fokus pada persentase kesalahan relatif terhadap nilai aktual, bukan error absolut. Ini sangat sesuai dengan kebutuhan bisnis asuransi yang peduli pada akurasi proporsi prediksi, bukan hanya selisih nominal. Selain itu, dengan menghitung MAPE secara terpisah untuk frekuensi, severitas, dan total klaim, sistem penilaian ini mendorong peserta untuk membangun model yang akurat di semua aspek, bukan hanya fokus pada satu komponen saja.
Leaderboard
Peringkat dalam kompetisi ini akan didasarkan pada Mean Absolute Percentage Error, di mana skor yang lebih rendah akan mendapatkan peringkat lebih tinggi. Leaderboard akan terdiri dari dua fase:
Public Leaderboard Dihitung berdasarkan sebagian dari data uji (40%) dan akan terlihat sepanjang kompetisi. Ini memberikan gambaran sementara tentang performa model peserta selama kompetisi berlangsung.
Private Leaderboard Dihitung berdasarkan sisa data uji (60%) dan akan digunakan untuk menentukan peringkat akhir di akhir kompetisi.
Pembobotan Peringkat akhir akan ditentukan berdasarkan Private Leaderboard dengan bobot sebagai berikut:

```fortran
Final Submission Score = 100% Private
```



berikan judul singkat dari permasalahan saya