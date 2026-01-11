# 📊 Analisis Sentimen Ulasan Aplikasi KAI Access Menggunakan Metode Deep Learning

## 1. Latar Belakang
Aplikasi **KAI Access** merupakan platform digital resmi yang digunakan oleh pelanggan **PT Kereta Api Indonesia** untuk melakukan pemesanan tiket, pengecekan jadwal, dan pengelolaan perjalanan. Seiring meningkatnya jumlah pengguna, aplikasi ini menerima ribuan ulasan dari pengguna melalui **Google Play Store** yang mencerminkan pengalaman, kepuasan, serta permasalahan yang dihadapi pengguna.

Ulasan tersebut mengandung informasi yang sangat bernilai bagi pengembang aplikasi. Namun, volume data yang besar dan sifat teks yang tidak terstruktur membuat analisis manual menjadi tidak efisien. Oleh karena itu, diperlukan pendekatan otomatis berbasis **Deep Learning** untuk mengklasifikasikan sentimen ulasan pengguna ke dalam kategori **negatif**, **netral**, dan **positif** secara akurat.

Penelitian ini bertujuan untuk membangun sistem analisis sentimen berbasis deep learning dengan memanfaatkan ulasan pengguna aplikasi KAI Access yang diperoleh secara mandiri melalui proses **web scraping**.

---

## 2. Dataset dan Sumber Data
- **Sumber data**: Google Play Store  
- **Aplikasi**: KAI Access (`com.kai.kaiticketing`)  
- **Metode pengambilan data**: Web Scraping menggunakan Python  
- **Jumlah data**: 6.000 ulasan pengguna  
- **Bahasa**: Bahasa Indonesia  

Jumlah data yang digunakan telah memenuhi ketentuan minimal **3.000 data** untuk tugas proyek Deep Learning.

---

## 3. Skema Pelabelan Sentimen
Pelabelan sentimen dilakukan secara otomatis berdasarkan rating bintang pengguna dengan skema sebagai berikut:

- ⭐ Rating **1–2** → **Negatif**  
- ⭐ Rating **3** → **Netral**  
- ⭐ Rating **4–5** → **Positif**

Skema ini umum digunakan dalam analisis sentimen ulasan aplikasi dan memungkinkan proses pelabelan yang konsisten serta terukur.

---

## 4. Distribusi Kelas Sentimen
Distribusi data setelah pelabelan adalah sebagai berikut:

- **Positif**: 3.322 ulasan  
- **Negatif**: 2.323 ulasan  
- **Netral**: 355 ulasan  

Distribusi ini menunjukkan bahwa kelas **netral** memiliki jumlah data yang jauh lebih sedikit dibandingkan kelas positif dan negatif, sehingga dataset bersifat **imbalanced**. Kondisi ini menjadi salah satu tantangan dalam pelatihan model.

---

## 5. Pemrosesan Data
Tahapan pemrosesan data dilakukan secara sistematis untuk meningkatkan kualitas input model, meliputi:

### 5.1 Data Cleaning
- Menghapus URL dan karakter non-alfabet  
- Menghilangkan angka dan simbol  
- Normalisasi spasi  
- Mengubah teks menjadi huruf kecil (*case folding*)

### 5.2 Text Preprocessing
- Tokenisasi teks  
- Penghapusan stopwords Bahasa Indonesia  
- Mempertahankan kata penting untuk sentimen, terutama kata negasi dan ekspresi keluhan seperti:  
  `tidak, bukan, gagal, error, lambat, lemot, susah, bermasalah`
- Padding sequence untuk menyamakan panjang input  

Langkah ini bertujuan agar makna sentimen tidak hilang selama proses preprocessing.

---

## 6. Feature Engineering
Representasi teks dilakukan menggunakan pendekatan **Embedding Layer**, yang memungkinkan model mempelajari representasi vektor kata secara langsung selama proses training. Panjang maksimum sequence ditetapkan sebesar **100 token** untuk menjaga konsistensi input model.

---

## 7. Model dan Skema Pelatihan
Untuk memenuhi kriteria evaluasi dan membandingkan performa, dilakukan tiga skema pelatihan berbasis deep learning:

### 7.1 Model 1 — CNN (*Convolutional Neural Network*)
CNN digunakan untuk mengekstraksi pola lokal (*n-gram*) dalam teks ulasan dan efektif dalam menangkap fitur penting dari kombinasi kata.

### 7.2 Model 2 — LSTM (*Long Short-Term Memory*)
LSTM digunakan untuk mempelajari dependensi urutan kata dalam teks, namun memiliki keterbatasan dalam memahami konteks dua arah.

### 7.3 Model 3 — BiLSTM (*Bidirectional LSTM*)
BiLSTM merupakan pengembangan LSTM yang membaca urutan teks dari dua arah (maju dan mundur), sehingga mampu menangkap konteks kalimat secara lebih menyeluruh.

Seluruh model dilatih menggunakan:
- **Optimizer**: Adam  
- **Loss function**: Sparse Categorical Crossentropy  
- **Early Stopping** untuk mencegah overfitting  

---

## 8. Hasil Evaluasi Model
Evaluasi performa dilakukan menggunakan akurasi pada data testing. Ringkasan hasil sebagai berikut:

| Model  | Akurasi Testing |
|--------|------------------|
| CNN    | 84.25%           |
| LSTM   | 55.33%           |
| BiLSTM | 84.92%           |

---

## 9. Analisis Hasil
Berdasarkan hasil eksperimen, dapat disimpulkan bahwa:
1. Model **BiLSTM** menghasilkan performa terbaik dengan akurasi testing sebesar **84.92%**, yang mendekati ambang batas **85%** yang ditetapkan.  
2. **CNN** juga menunjukkan performa yang kompetitif, menandakan efektivitas model dalam menangkap pola lokal pada teks ulasan.  
3. Model **LSTM** memiliki performa yang lebih rendah, kemungkinan disebabkan oleh keterbatasan dalam menangkap konteks kompleks serta pengaruh distribusi kelas yang tidak seimbang.  
4. Perbedaan performa antar model menunjukkan bahwa arsitektur **bidirectional** lebih efektif dalam memahami konteks bahasa alami pada ulasan pengguna.

---

## 10. Inference / Prediksi Sentimen
Model terbaik (**BiLSTM**) digunakan untuk melakukan prediksi sentimen pada data baru. Sistem mampu menerima input teks ulasan dan menghasilkan output kelas sentimen berupa **negatif**, **netral**, atau **positif**, sehingga dapat digunakan sebagai sistem analisis sentimen secara praktis.

---

## 11. Kesimpulan
Penelitian ini berhasil membangun sistem analisis sentimen ulasan aplikasi KAI Access menggunakan metode deep learning dengan pipeline lengkap mulai dari scraping data hingga inference. Dari tiga model yang diuji, **BiLSTM** menunjukkan performa terbaik dengan akurasi pengujian mendekati **85%**.
