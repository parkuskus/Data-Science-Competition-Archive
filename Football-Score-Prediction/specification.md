Overview
🌻Perjalanan Yooji dan Mina Menonton Piala Dunia✈️
Namaku Yooji, dan di sampingku adalah Mina, gadis yang senyumnya selalu menjadi alasan bagiku untuk terus melangkah. Tahun 2026 ini seharusnya menjadi puncak kebahagiaan kami, setelah bertahun-tahun menabung, aku akhirnya bisa membawa Mina terbang ke Amerika untuk mewujudkan mimpinya menonton Piala Dunia. Di dalam kabin pesawat yang tenang, kami duduk bersandar sambil berbagi sepasang earphone, menonton film tentang pasukan lebah yang menyabotase penerbangan. Aku masih ingat hangat tangannya yang menggenggam jemariku, sebelum akhirnya cahaya aneh dari layar monitor menyambar dan merenggut segalanya dalam sekejap mata.

Aku hanya bisa mematung saat melihat tubuh Mina terurai menjadi partikel cahaya dan tersedot masuk ke dalam layar monitor tepat di depan mataku. Detik berikutnya, wajah manis Mina lenyap, digantikan oleh sosok dingin nan angkuh bernama Beeta, Sang Raja Lebah penguasa dimensi data. Beeta menyeringai dari balik kaca, suaranya berdengung di kepalaku, menyatakan bahwa Mina kini adalah tawanannya di dalam penjara digital yang rumit. Ia hanya akan melepaskan Mina jika aku mampu membuktikan bahwa masa depan sepak bola bukanlah sebuah misteri yang acak, melainkan pola yang bisa aku taklukkan dengan pasti.

Kini, nyawa Mina berada sepenuhnya di ujung jariku. Beeta memberiku sebuah "kitab data" raksasa berisi sejarah pertandingan dan statistik pemain, lalu menantangku untuk menebak skor seluruh turnamen dengan ketepatan yang sempurna. Jika prediksiku meleset, Mina akan selamanya terhapus dari dunia nyata dan jiwanya akan dilebur menjadi deretan angka mati di dalam sarang digital Beeta. Di dalam kabin pesawat yang kini sunyi dan membeku dalam waktu, aku harus berpacu dengan detik demi menyusun ramalan paling akurat. Aku harus membawa Mina kembali, dan aku butuh bantuan kalian untuk memenangkan permainan hidup-mati ini sebelum peluit akhir dibunyikan.

Start

2 days ago
Close

19 days to go
Evaluation
🐝Beeta’s Judgment: Bagaimana Nasib Mina Ditentukan?
Beeta tidak menoleransi kesalahan. Untuk menyelamatkan Mina, model prediksi Anda harus melewati standarnya yang kejam. Dalam tantangan ini, ketepatan ramalan Anda akan diukur menggunakan:

Augmented Weighted Mean Absolute Error (AW-MAE)
Kompetisi ini menggunakan Augmented Weighted Mean Absolute Error (AW-MAE) sebagai metrik evaluasi.

Berbeda dari MAE biasa, metrik ini tidak hanya melihat seberapa dekat prediksi skor dengan hasil asli, tetapi juga memperhatikan:

ketepatan skor akhir,
ketepatan hasil pertandingan (menang / seri / kalah),
ketepatan selisih gol,
dan tingkat kepentingan turnamen.
Semakin kecil nilai AW-MAE, semakin baik performa model.


Di mana:

Loss_i adalah nilai error akhir pada pertandingan ke-i
Weight_i adalah bobot pertandingan ke-i
1. Base Error (MAE)
Untuk setiap pertandingan, pertama dihitung error dasar menggunakan rata-rata selisih absolut antara skor prediksi dan skor asli:

$MAE = \frac{|\text{teamgoals}_{true} - \text{teamgoals}_{pred}| + |\text{oppgoals}_{true} - \text{oppgoals}_{pred}|}{2}$

Semakin dekat prediksi dengan skor asli, semakin kecil nilai MAE.

2. Penalty Components
Setelah MAE dihitung, sistem akan menambahkan penalti jika prediksi meleset pada aspek-aspek penting berikut:

Exact Score Penalty (0.30)
Dikenakan jika skor prediksi tidak sama persis dengan skor asli.

Outcome Penalty (0.25)
Dikenakan jika hasil pertandingan yang diprediksi (Menang / Seri / Kalah) tidak sesuai dengan hasil asli.

Goal Difference Penalty (0.15)
Dikenakan jika selisih gol prediksi tidak sama dengan selisih gol hasil asli.

Secara ringkas:

$\text{Penalty} = 0.3(1-\text{Exact}) + 0.25(1-\text{Outcome}) + 0.15(1-\text{GD})$

dengan:

Exact = 1 jika skor prediksi tepat, selain itu 0
Outcome = 1 jika hasil menang/seri/kalah benar, selain itu 0
GD = 1 jika selisih gol benar, selain itu 0
Artinya, jika suatu aspek diprediksi benar, maka penalti untuk aspek tersebut adalah 0.

3. Outcome Multiplier
Dalam kompetisi ini, ketepatan menebak hasil pertandingan menjadi faktor penting.

Jika prediksi salah pada outcome, total error akan diperbesar dengan pengali:

1.0 jika outcome benar
1.5 jika outcome salah
Dengan demikian, prediksi yang salah pada hasil inti pertandingan akan mendapat konsekuensi lebih besar dibanding kesalahan biasa.

4. Non-Linear Scaling
Setelah MAE dan penalti digabung, nilai tersebut masih akan dipangkatkan agar prediksi yang sangat meleset menerima hukuman lebih besar daripada prediksi yang hampir tepat.

Langkahnya:

$\text{RawLoss} = \text{MAE} + \text{Penalty}$
$\text{Loss} = (\text{RawLoss} \cdot \text{Multiplier})^{1.5}$

Pendekatan ini membuat gap antar peserta menjadi lebih jelas, terutama antara prediksi yang benar-benar kuat dan prediksi yang hanya mendekati.

5. Tournament Weighting
Setiap pertandingan memiliki bobot berbeda sesuai tingkat kepentingan turnamennya.

Turnamen yang lebih prestisius akan memberikan pengaruh lebih besar terhadap skor akhir dibanding pertandingan dengan bobot lebih rendah.

Sebagai contoh:

FIFA World Cup memiliki bobot tinggi, hingga 2.00
AFC Championship memiliki bobot 1.80
Friendly Match memiliki bobot lebih rendah, yaitu 0.96
Jika turnamen tidak termasuk dalam daftar khusus, maka digunakan default weight = 1.20
Dengan kata lain, kesalahan pada pertandingan yang lebih penting akan berdampak lebih besar terhadap nilai akhir.

Metrik ini dirancang agar model yang:

dekat dengan skor asli,
benar menebak hasil pertandingan,
dan tepat pada selisih gol
akan memperoleh nilai yang lebih baik dibanding model yang hanya mendekati skor secara umum.

Submission File
Untuk setiap Iddalam test set, Anda harus memprediksi nilai integer untuk team_goalsdan opp_goals. File submisi harus berisi header dan memiliki format sebagai berikut:

Id	team_goals	opp_goals
M000001_Seychelles	0	0
M000001_Mauritius	0	0
M000002_Argentine	4	2
M000002_Brazil	2	4
M000003_Indonesia	1	2
M000003_Malaysia	2	1


1. Identitas & Info Dasar
Id: Identitas unik untuk setiap baris (Format: match_id_nama_tim).
match_id: ID unik untuk satu pertandingan (satu pertandingan memiliki dua baris Id untuk masing-masing tim).
date: Tanggal pertandingan dilaksanakan.
gender: Jenis kelamin pemain (M untuk Pria, W untuk Wanita).
team: Nama tim utama.
opponent: Nama tim lawan.

2. Kondisi Pertandingan
is_home: Binary (1/0), apakah tim bermain di kandang sendiri.
neutral: Binary (1/0), apakah pertandingan dimainkan di tempat netral.
tournament: Nama kompetisi atau jenis turnamen (misal: Friendly, FIFA World Cup, dll).
venue_country: Negara tempat pertandingan berlangsung.
confederation_team/opp: Konfederasi sepak bola tim/lawan (misal: UEFA, CAF, CONMEBOL).

3. Metrik Performa (Hanya tersedia lengkap di train.csv)
elo_team / elo_opponent: Peringkat Elo masing-masing tim saat pertandingan.
rank_team / rank_opponent: Peringkat FIFA masing-masing tim.
team_points_last5 / last10: Poin yang dikumpulkan dalam 5 atau 10 pertandingan terakhir.
team_gd_last5: Selisih gol (Goal Difference) dalam 5 pertandingan terakhir.
team_win_rate_last10: Persentase kemenangan dalam 10 pertandingan terakhir.
h2h_points_last5: Poin dari rekam jejak pertemuan kedua tim (Head to Head).
days_since_last_match: Jumlah hari sejak pertandingan terakhir tim tersebut.

4. Data Geografis & Sosio-Ekonomi
population_team / opp: Populasi negara tim/lawan.
gdp_per_capita_team / opp: Produk Domestik Bruto per kapita negara tim/lawan.
altitude_venue: Ketinggian lokasi pertandingan di atas permukaan laut.
distance_travel_team / opp: Estimasi jarak perjalanan yang ditempuh tim menuju lokasi.
temperature_venue: Estimasi suhu udara di lokasi pertandingan.

5. Target Variables
team_goals: Jumlah gol yang dicetak oleh team.
opp_goals: Jumlah gol yang dicetak oleh opponent.