Overview
Source: BukitVista Seiring dengan berkembangnya industri penyewaan akomodasi jangka pendek secara global, memprediksi harga sewa properti dengan akurat menjadi tantangan penting bagi pemilik properti, platform digital, dan peneliti pasar. Kompetisi Prediksi Harga Sewa Airbnb ini memberikan kesempatan bagi peserta untuk menganalisis berbagai faktor yang memengaruhi penentuan harga listing di berbagai kota dunia. Dataset yang digunakan mencakup karakteristik properti, informasi host, lokasi geografis, serta riwayat ulasan dan ketersediaan. Dengan menerapkan teknik data science, peserta dapat menggali wawasan mendalam tentang dinamika pasar dan strategi penetapan harga yang optimal.

Permasalahan
Penentuan harga sewa yang tepat memiliki dampak ekonomi yang signifikan, baik bagi pemilik properti yang ingin memaksimalkan pendapatan, maupun bagi platform yang ingin menjaga daya saing dan kepuasan pengguna. Dalam kompetisi ini, peserta ditantang untuk mengembangkan model prediktif yang mampu memperkirakan harga sewa per malam berdasarkan data historis dan atribut listing. Dataset yang diberikan mencakup berbagai fitur seperti tipe properti, kapasitas akomodasi, lokasi, atribut host, serta data ulasan dan ketersediaan. Dengan memanfaatkan data tersebut, peserta diharapkan dapat membangun model yang mampu memprediksi harga sewa setiap listing dengan tingkat akurasi yang tinggi.

Tujuan
Tujuan dari kompetisi ini adalah mendorong para data scientist untuk mengasah keterampilan mereka dalam membangun model prediksi harga yang akurat dan aplikatif. Dengan berpartisipasi, peserta turut berkontribusi dalam pemahaman yang lebih mendalam mengenai mekanisme pasar sewa akomodasi, serta dampaknya terhadap strategi bisnis dan pengalaman pengguna. Selain meningkatkan kemampuan teknis, kompetisi ini juga menekankan pentingnya pengambilan keputusan berbasis data dalam mengelola portofolio properti, mengoptimalkan pendapatan, dan menciptakan ekosistem penyewaan yang lebih efisien dan transparan.

Start

Jul 26, 2025
Close

Aug 2, 2025
Description
Kompetisi ini menantang peserta untuk memprediksi harga sewa per malam dari listing Airbnb yang tersebar di berbagai kota dunia. Dataset yang disediakan mencakup beragam fitur yang merepresentasikan karakteristik properti, informasi host, lokasi geografis, serta riwayat ulasan dan ketersediaan. Setiap baris data menggambarkan satu listing Airbnb dengan atribut-atribut yang relevan dalam penentuan harga.

Peserta dapat memanfaatkan fitur-fitur seperti tipe properti, jumlah kamar, kapasitas akomodasi, lokasi, serta data ulasan dan ketersediaan untuk membangun model prediksi harga yang akurat. Selain itu, informasi mengenai host dan lingkungan sekitar juga dapat digunakan untuk menangkap faktor-faktor non-teknis yang memengaruhi harga sewa.

Evaluation
Root Mean Squared Error (RMSE)
Submisi akan dinilai berdasarkan Root Mean Squared Error (Akar dari Rata-rata Kuadrat Kesalahan) antara harga aktual dan harga prediksi untuk setiap listing pada data uji. RMSE adalah metrik standar untuk tugas regresi, memberikan penalti lebih besar pada prediksi yang jauh meleset dan mudah diinterpretasikan karena satuannya sama dengan target (harga).

Secara matematis, RMSE didefinisikan sebagai:

dalam hal ini, adalah nilai aktual (ground truth) dan adalah nilai prediksi untuk setiap .

import numpy as np

def rmse(y_true: np.ndarray, y_pred: np.ndarray) -> float:
    """
    Menghitung Root Mean Squared Error (RMSE).

    Parameters:
    y_true (array-like): Nilai aktual.
    y_pred (array-like): Nilai prediksi.

    Returns:
    float: Nilai RMSE.
    """
    return np.sqrt(np.mean((y_true - y_pred) ** 2))
atau secara simpel menggunakan scikit-learn library

from sklearn.metrics import mean_squared_error
import numpy as np

rmse = np.sqrt(mean_squared_error(y_true, y_pred))
Mengapa RMSE?
RMSE dipilih karena memberikan penalti lebih besar pada kesalahan prediksi yang besar, sehingga mendorong peserta untuk menghasilkan model yang tidak hanya akurat secara rata-rata, tetapi juga stabil pada berbagai rentang harga. Metrik ini sangat relevan dalam konteks prediksi harga sewa, di mana kesalahan besar pada listing tertentu dapat berdampak signifikan secara bisnis.

Citation
Muhammad Fathur Rizky. SPARTA 2024 Data Science Competition. https://kaggle.com/competitions/sparta-2024-data-science-competition, 2025. Kaggle.


Dataset Description
This dataset contains Airbnb listing information from multiple cities around the world. The data is provided in CSV format and is split into training and test sets for the purpose of building and evaluating machine learning models to predict listing prices.

Files
train.csv - the training set. Contains 300,000 Airbnb listings with all features and the target variable (price).
test.csv - the test set. Contains 100,000 Airbnb listings with all features except the target variable (price). Your task is to predict the price for these listings.
sample_submission.csv - a sample submission file in the correct format. Use this as a template for your predictions.
Columns
Below are the columns included in the dataset, along with their descriptions:

id - Airbnb's unique identifier for the listing.
name - Name of the listing.
description - Detailed description of the listing.
neighborhood_overview - Host's description of the neighbourhood.
host_id - Airbnb's unique identifier for the host.
host_url - The Airbnb page for the host.
host_name - Name of the host.
host_since - The date the host/user was created.
host_location - The host's self-reported location.
host_about - Description about the host.
host_response_time - How quickly the host responds to booking requests.
host_response_rate - The rate at which a host responds to booking requests.
host_acceptance_rate - The rate at which a host accepts booking requests.
host_is_superhost - Whether the host is a superhost (t=true, f=false).
host_neighbourhood - The host's neighbourhood.
host_listings_count - The number of listings the host has.
host_total_listings_count - The total number of listings the host has.
host_verifications - Methods used by Airbnb to verify the host.
host_has_profile_pic - Whether the host has a profile picture (t=true, f=false).
host_identity_verified - Whether the host's identity is verified (t=true, f=false).
neighbourhood - Name of the neighbourhood.
neighbourhood_cleansed - Geocoded neighbourhood name.
latitude - Latitude of the listing (WGS84).
longitude - Longitude of the listing (WGS84).
property_type - Type of property (e.g., Apartment, House, etc.).
room_type - Room type (Entire home/apt, Private room, Shared room, Hotel).
accommodates - Maximum capacity of the listing.
bathrooms - Number of bathrooms.
bathrooms_text - Textual description of bathrooms.
bedrooms - Number of bedrooms.
beds - Number of beds.
amenities - List of amenities provided (JSON-formatted string).
price - Daily price in local currency (target variable for train.csv, to be predicted for test.csv).
has_availability - Whether the listing has availability (t=true, f=false).
availability_30 - Availability in the next 30 days.
availability_60 - Availability in the next 60 days.
availability_90 - Availability in the next 90 days.
availability_365 - Availability in the next 365 days.
number_of_reviews - Total number of reviews.
number_of_reviews_ltm - Number of reviews in the last 12 months.
number_of_reviews_l30d - Number of reviews in the last 30 days.
availability_eoy - Availability at the end of the year.
number_of_reviews_ly - Number of reviews in the last year.
estimated_occupancy_l365d - Estimated occupancy in the last 365 days.
estimated_revenue_l365d - Estimated revenue in the last 365 days.
first_review - Date of the first review.
last_review - Date of the last review.
review_scores_rating - Overall review rating.
review_scores_accuracy - Review score for accuracy.
review_scores_cleanliness - Review score for cleanliness.
review_scores_checkin - Review score for check-in.
review_scores_communication - Review score for communication.
review_scores_location - Review score for location.
review_scores_value - Review score for value.
reviews_per_month - Average number of reviews per month.
city - City where the listing is located.
What acronyms will I encounter?

t/f: Boolean values, where t = true and f = false.
WGS84: World Geodetic System 1984, a standard for latitude/longitude.
ltm: Last twelve months.
l30d: Last 30 days.
ly: Last year.
Data Format
All files are in CSV format, encoded in UTF-8.
Dates are in ISO format (YYYY-MM-DD).
Prices are in local currency, as floats (decimal numbers).
The amenities column is a JSON-formatted string listing all amenities for the property.
Some columns may contain missing values (NaN).