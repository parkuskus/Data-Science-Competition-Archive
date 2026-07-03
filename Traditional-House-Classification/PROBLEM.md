Overview
Seiring dengan semakin berkurangnya pengetahuan generasi muda tentang rumah adat, upaya pelestarian budaya menjadi semakin penting. Kompetisi Klasifikasi Rumah Adat Nusantara ini memberi kesempatan bagi peserta untuk membangun model yang mampu mengenali berbagai rumah adat dari seluruh Indonesia. Dataset yang digunakan berisi ribuan citra rumah adat dari berbagai daerah dengan variasi pencahayaan, sudut pandang, dan kualitas gambar. Dengan menerapkan teknik data science, peserta dapat menggali wawasan baru tentang pola visual arsitektur Nusantara sekaligus berkontribusi dalam upaya digitalisasi dan pelestarian warisan budaya bangsa.

Objektif Kompetisi
Kurangnya pengetahuan tentang rumah adat Nusantara dapat mengakibatkan hilangnya identitas budaya dan warisan arsitektur yang berharga. Dalam kompetisi ini, peserta diharapkan dapat membangun model yang mampu mengklasifikasikan setiap gambar ke dalam kategori rumah adat yang tepat dengan tingkat akurasi dan generalisasi yang tinggi.

Tujuan
Tujuan dari kompetisi ini adalah mendorong para peserta untuk mengasah keterampilan mereka dalam bidang data science khususnya computer vision. Dengan berpartisipasi, peserta turut berkontribusi pada pelestarian budaya Nusantara dengan membantu mengenali dan mendokumentasikan keragaman arsitektur tradisional Indonesia. Selain meningkatkan kemampuan teknis, kompetisi ini juga menekankan pentingnya pemanfaatan teknologi berbasis data untuk mendukung upaya pelestarian warisan budaya di era digital.

Start

Sep 21, 2025
Close

Oct 11, 2025
Evaluation
Submisi dinilai berdasarkan Macro F1-Score yang menyatakan rata-rata F1-Score per kelas. Secara matematis, Macro F1-Score dihitung dengan:


Metrik ini dipilih karena dataset memiliki distribusi kelas yang cukup tidak seimbang, sehingga metrik biasa seperti accuracy dapat menyesatkan. Macro F1-Score mengatasi masalah ini dengan memberikan bobot yang sama pada setiap kelas, sehingga model didorong untuk berperforma baik pada semua kategori rumah adat.

File Submisi
Untuk setiap gambar pada test set, Anda harus memprediksi kategori rumah adat yang sesuai. File submisi harus memiliki format sebagai berikut:

csv
id,style
Test_001,balinese
Test_002,minangkabau
Test_003,javanese
...
Pastikan format file sesuai dengan contoh sample_submission.csv yang telah disediakan dalam dataset.

Dataset Description
Setiap gambar dalam dataset ini dilengkapi dengan label yang menunjukkan kategori rumah adat tempat gambar tersebut berasal. Label digunakan sebagai target untuk membangun model klasifikasi citra.

Kategori yang tersedia mencakup:
Javanese – mencakup rumah tradisional Jawa seperti Joglo dan Kraton
Balinese – rumah tradisional Bali, termasuk area pura
Minangkabau – Rumah Gadang dari Sumatra Barat
Batak – Rumah Bolon dari Sumatra Utara
Dayak – rumah panjang khas masyarakat Dayak di Kalimantan
Dengan label yang jelas dan terstruktur ini, pengguna dapat melatih dan menguji model image classification dengan lebih mudah, sekaligus mengenal keragaman arsitektur tradisional Indonesia.-