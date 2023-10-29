# Employee Attrition EDA

## Overview
Proyek ini adalah analisis eksploratif data (EDA) untuk memahami faktor-faktor yang mempengaruhi keinginan karyawan untuk mengganti pekerjaan. Data ini mencakup informasi tentang demografi, pendidikan, pengalaman, dan ukuran serta tipe perusahaan. Analisis ini bertujuan untuk memberikan wawasan tentang karakteristik karyawan dan potensi pengaruhnya terhadap keinginan untuk mengganti pekerjaan.

## Dataset
Dataset yang digunakan adalah [nama_dataset]. Dataset ini memiliki berbagai atribut seperti enrollee_id, city, city_development_index, gender, education_level, major_discipline, experience, company_size, dan lain-lain.

## Insight
Berikut adalah beberapa insight yang diperoleh dari EDA:
- Mayoritas karyawan adalah laki-laki.
- Kebanyakan karyawan memiliki pengalaman bekerja lebih dari 10 tahun.
- Sekitar 25% dari karyawan tertarik untuk mengganti pekerjaan.
- Distribusi ukuran perusahaan bervariasi, dengan mayoritas karyawan bekerja di perusahaan kecil hingga menengah.
- Mayoritas pencari pekerjaan baru bertempat pada kota dengan CDI yang rendah (<0.75)
- Durasi training hours tidak mempengaruhi keputusan untuk mencari pekerjaan baru

## Analisis Tambahan
Selain itu, kami juga melakukan analisis korelasi dan distribusi untuk memahami hubungan antar fitur dan distribusi masing-masing fitur dalam dataset.

## Rekomendasi
Berdasarkan analisis ini, kami merekomendasikan:
- Perusahaan untuk fokus mempertahankan karyawan dengan pengalaman relevan dengan memberikan insentif dan kesempatan pengembangan.
- Menyesuaikan strategi perekrutan untuk mencocokkan preferensi dan kebutuhan karyawan.
- Penempatan perusahaan baru di kota-kota dengan CDI yang lebih tinggi.
- Tindak lanjut untuk perekrutan **Graduate** karena persentase berpindah pekerjaannya tinggi.

# Data Pre-Processing

## Overview
Data Pre-Processing adalah langkah kunci. Ini melibatkan pembersihan data dengan mengidentifikasi dan mengatasi nilai hilang, duplikat, serta outlier. Transformasi data diperlukan untuk memastikan kesesuaian format, dan variabel kategoris di-encode. Fitur ekstraksi dan penghapusan variabel tidak relevan dilakukan untuk memusatkan analisis. Penanganan data tidak seimbang dan pengujian data adalah tahapan tambahan yang penting.

## Data Cleansing
## Handle Missing Values
Untuk menangani missing values, pendekatan yang diambil adalah menggantinya dengan modus (nilai yang paling sering muncul) dari setiap kolom. Setelah proses pengisian missing values dengan modus selesai, dataset tidak lagi memiliki missing values dalam setiap kolom. Hal ini akan memungkinkan analisis yang lebih akurat dan kohesif pada dataset. Kesimpulannya, proses penanganan missing values dengan menggantinya menggunakan modus pada setiap kolom telah berhasil membersihkan dataset dari missing values, sehingga dataset siap digunakan untuk analisis selanjutnya.

## Handle Data Duplicate
Tidak terdapat data duplicate pada dataset ini.

## Handle Outliers
Hasil analisis menunjukkan bahwa proses filtering outliers menggunakan Z-score telah dilakukan pada dua kolom, yaitu 'city_development_index' dan 'training_hours'. Setelah proses filtering, jumlah baris pada dataset mengalami pengurangan dari 19,158 baris awal menjadi 18,691 baris setelah menghilangkan data yang dianggap sebagai outlier.
Distribusi data setelah penghilangan outlier pada kedua kolom tersebut ditampilkan dalam grafik. Dapat dilihat bahwa distribusi data 'city_development_index' dan 'training_hours' menjadi lebih terkonsentrasi tanpa adanya outlier, seperti yang terlihat pada grafik histogram dan box plot. Ini akan memberikan dampak positif pada pemodelan karena data menjadi lebih bersih dan lebih sesuai dengan asumsi pemodelan statistik.
Selain itu, distribusi data 'target' juga ditampilkan, namun tidak mengalami perubahan karena proses filtering outlier tidak diterapkan pada kolom ini. Kesimpulannya, proses filtering outlier menggunakan Z-score telah berhasil meningkatkan kualitas data pada kolom 'city_development_index' dan 'training_hours', yang akan meningkatkan performa pemodelan selanjutnya.

## Feature Transformation
Setelah melakukan transformasi fitur dengan menghitung logaritma alami (natural log) dari kolom 'training_hours' dan kemudian mengevaluasi distribusi data setelah transformasi, beberapa hal dapat disimpulkan:
Transformasi Logaritma: Transformasi logaritma pada kolom 'training_hours' telah diterapkan, yang umumnya digunakan untuk mengatasi distribusi data yang sangat condong (skewed) ke satu sisi. Hasil transformasi ini adalah kolom baru yang disebut 'th_log'.
Distribusi Data Setelah Transformasi: Setelah transformasi, distribusi data pada kolom 'training_hours' menjadi lebih simetris dan mendekati distribusi normal. Hal ini terlihat dari plot KDE (Kernel Density Estimation) di mana kurva menjadi lebih merata. Transformasi ini dapat memudahkan penggunaan data ini dalam analisis dan pemodelan.
Visualisasi KDE Plot: Grafik KDE Plot menunjukkan distribusi setelah transformasi untuk berbagai kolom numerik dalam dataset. Terlihat bahwa sebagian besar kolom numerik memiliki distribusi yang lebih terdistribusi merata setelah transformasi logaritma. Ini bisa bermanfaat dalam analisis lebih lanjut dan pemodelan data.

## Feature Encoding
- Penggantian Tanda Pemisah: Tanda pemisah "/" pada kolom 'company_size' telah diganti dengan "-" untuk mengubah format menjadi lebih sesuai. Misalnya, "10/49" menjadi "10-49".
- One-Hot Encoding: Kolom-kolom kategoris seperti 'enrolled_university', 'education_level', 'major_discipline', dan 'company_type' telah di-encode menggunakan one-hot encoding. Ini menghasilkan kolom-kolom baru yang mewakili nilai-nilai kategori sebagai variabel biner.
- Label Encoding: Beberapa kolom seperti 'gender', 'relevent_experience', 'enrolled_university', 'education_level', 'company_size', 'last_new_job', 'major_discipline', 'experience', dan 'company_type' telah di-encode dengan label encoding. Nilai-nilai kategori dalam kolom-kolom ini digantikan dengan angka sesuai dengan pemetaan yang ditentukan.

## Feature Extraction
- Pengelompokan Pengalaman (Experience Grouping): Membuat fungsi 'group_experience' yang membagi pengalaman menjadi tiga kelompok, yaitu "Junior," "Mid," dan "Senior," berdasarkan ambang batas pengalaman. Ini membantu dalam mengkategorikan karyawan berdasarkan tingkat pengalaman mereka.
- Pengelompokan Ukuran Perusahaan (Company Size Grouping): Membuat fungsi 'group_company_size' yang membagi ukuran perusahaan menjadi tiga kelompok, yaitu "Small Company," "Medium Company," dan "Big Company," berdasarkan ambang batas ukuran perusahaan. Ini membantu dalam mengkategorikan perusahaan berdasarkan ukurannya.
- Label Encoding: Mengelompokkan pengalaman dan ukuran perusahaan, Anda melakukan label encoding pada kolom 'group_experience' dan 'group_company_size' dengan menggantikan kategori-kategori ini dengan nilai numerik yang sesuai.

## Feature Selection
- Drop Kolom: Menghapus beberapa kolom dari dataset yang tidak akan digunakan dalam pemodelan. Ini termasuk ID enrollee, kolom gender, pendidikan, disiplin ilmu, ukuran perusahaan, pengalaman terakhir, jam pelatihan, dan lainnya.
- Feature Scaling: Melakukan penskalaan fitur (feature scaling) pada kolom-kolom numerik dalam dataset menggunakan StandardScaler. Ini penting untuk memastikan bahwa semua variabel numerik memiliki skala yang serupa, yang diperlukan oleh beberapa algoritma pemodelan.
- Pembagian Data: Data telah dibagi menjadi data latih (X_train dan y_train) dan data uji (X_test dan y_test) menggunakan train_test_split. Data uji digunakan untuk menguji performa model yang akan dibangun.

## Handle Class Imbalance
- SMOTE Oversampling: Menggunakan SMOTE untuk melakukan oversampling pada data latih. Oversampling adalah teknik yang digunakan untuk mengatasi ketidakseimbangan kelas, di mana kelas minoritas (dalam hal ini, mungkin kelas target yang kurang umum) "diperbanyak" dengan menciptakan sampel sintetis berdasarkan data yang ada.
- Penyelarasan Data Latih: Setelah menerapkan SMOTE, jumlah data latih telah diperluas, dan sekarang terdapat 21,014 baris data latih dan 21,014 label target yang sesuai.

## Feature Tambahan
- Age Of Candidate: Feature tersebut dapat menjelaskan sebaran umur kandidat yang ada dalam dataset.
- Salary Information: Feature tersebut dapat menjelaskan terkait gaji saat ini, gaji yang diharapkan atau tingkat pertumbuhan gaji. Fitur ini penting dibuat untuk memahami motivasi kanddat dalam pergantian pekerjaan atau tidak.
- Industry Experience: Feature tersebut dapat menjelaskan total pengalaman kandidat yang relevan dengan role Data Scientists. Dalam data set hanya ada feature experience yang menjelaskan total pengalaman kandidat secara umum dan tidak spesifik total berapa tahun kandidat tersebut memiliki pengalaman di role Data Scientists.
- Company Information : Feature tersebut dapat menjelaskan informasi lebih rinci tentang perusahaan tempat kandidat bekerja seperti nama perusahaan atau perusahaan tersebut bergerak dibidang apa.

## Referensi
- [Link ke Dataset](link_dataset)
- [Sumber Data](sumber_data)
