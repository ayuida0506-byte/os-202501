
# Laporan Praktikum Minggu [10]
Topik: [Manajemen Memori - Page Replacement.md"]

---

## Identitas
- **Nama**  : [Ayu Ida Nuraini ]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSRA]

---

## Tujuan
9*### 1. **Implementasi Algoritma**
- **FIFO (First In First Out)**: Mengganti page yang paling lama berada di memory
- **LRU (Least Recently Used)**: Mengganti page yang paling lama tidak digunakan

### 2. **Simulasi Interaktif**
- Input custom page reference string
- Atur jumlah frame memory (1-10)
- Visualisasi real-time hasil simulasi

### 3. **Laporan Performa**
- **Page Faults**: Jumlah kegagalan akses
- **Page Hits**: Jumlah keberhasilan akses
- **Hit Rate**: Persentase efisiensi
- Perbandingan langsung FIFO vs LRU

### 4. **Detail Langkah**
- Tabel step-by-step untuk setiap algoritma
- Status HIT/FAULT pada setiap akses
- Visualisasi kondisi memory di setiap langkah

## **Cara Penggunaan:**

1. **Masukkan page reference string** (contoh: 7,0,1,2,0,3,0,4,2,3,0,3,2)
2. **Tentukan jumlah frame** yang tersedia
3. Klik **"Jalankan Simulasi"**
4. Lihat hasil perbandingan dan analisis
5. Klik detail untuk melihat langkah per langkah



---

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.

### **1. Virtual Memory & Paging**
Virtual memory adalah teknik yang memungkinkan sistem operasi menggunakan secondary storage (disk) sebagai ekstensi dari RAM. Memori dibagi menjadi blok-blok berukuran tetap yang disebut **pages** (di logical memory) dan **frames** (di physical memory). Ketika program membutuhkan page yang tidak ada di RAM, terjadi **page fault** dan sistem harus memuat page tersebut dari disk.

### **2. Page Fault & Page Replacement**
**Page fault** terjadi ketika CPU mencoba mengakses page yang tidak ada di physical memory. Jika semua frame sudah penuh, sistem harus memilih page mana yang akan diganti (replaced) untuk memberi ruang bagi page baru. Proses ini disebut **page replacement**, dan algoritma yang digunakan sangat mempengaruhi performa sistem karena akses disk jauh lebih lambat daripada akses RAM.

### **3. Algoritma FIFO (First In First Out)**
FIFO adalah algoritma page replacement paling sederhana yang mengganti page berdasarkan urutan kedatangan - page yang pertama masuk ke memory akan menjadi kandidat pertama untuk diganti. Algoritma ini mudah diimplementasikan dengan queue, namun memiliki kelemahan yaitu **Belady's Anomaly** (penambahan frame bisa meningkatkan page fault) dan tidak mempertimbangkan pola penggunaan page.

### **4. Algoritma LRU (Least Recently Used)**
LRU mengganti page yang paling lama tidak digunakan, berdasarkan asumsi bahwa page yang baru saja diakses kemungkinan besar akan diakses lagi dalam waktu dekat (**principle of locality**). Algoritma ini umumnya memberikan performa lebih baik dari FIFO karena lebih adaptif, namun memerlukan overhead tambahan untuk tracking waktu akses setiap page. LRU termasuk **stack algorithm** sehingga tidak mengalami Belady's Anomaly.

### **5. Principle of Locality**
Program memiliki kecenderungan untuk mengakses alamat memori yang sama atau berdekatan dalam periode waktu tertentu. **Temporal locality** berarti data yang baru diakses cenderung akan diakses lagi dalam waktu dekat, sedangkan **spatial locality** berarti data di alamat yang berdekatan cenderung diakses bersamaan. Algoritma page replacement yang baik (seperti LRU) memanfaatkan prinsip ini untuk meminimalkan page fault.

---

**Metrik Evaluasi:**
- **Page Fault Rate**: Jumlah page fault / total page reference
- **Hit Rate**: Persentase akses yang berhasil tanpa page fault
- **Efisiensi**: Algoritma dengan page fault lebih rendah = lebih efisien
---

## Langkah Praktikum
1. Langkah-langkah yang dilakukan.  
2. Perintah yang dijalankan.  
3. File dan kode yang dibuat.  
4. Commit message yang digunakan.

---

## Kode / Perintah
Tuliskan potongan kode atau perintah utama:
```bash
uname -a
lsmod | head
dmesg | head
```

---

## Hasil Eksekusi
Sertakan screenshot hasil percobaan atau diagram:
![Screenshot hasil](screenshots/example.png)

---

## Analisis
1.Mengapa jumlah page fault bisa berbeda - Karena strategi penghapusan page berbeda.
FIFO hanya melihat waktu masuk, sementara LRU mempertimbangkan pola penggunaan sebenarnya.

2.Algoritma mana yang lebih efisien - LRU jauh lebih efisien karena:
  Mengikuti prinsip lokalitas temporal
  Mengurangi page fault secara signifikan
  Adaptif terhadap berbagai pola akses
  Tidak rentan terhadap Belady's anomaly seperti FIFO.



## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
Berikut kesimpulan dari praktikum manajemen memori - page replacement:

### **1. Perbedaan Performa FIFO dan LRU**
Algoritma **LRU (Least Recently Used)** secara umum menghasilkan jumlah page fault yang lebih sedikit dibandingkan **FIFO (First In First Out)** karena LRU mempertimbangkan pola penggunaan page dengan memanfaatkan **principle of locality**. LRU mempertahankan page yang baru saja diakses sehingga kemungkinan besar akan digunakan lagi, sedangkan FIFO hanya mempertimbangkan urutan kedatangan tanpa melihat frekuensi atau waktu akses terakhir, yang dapat menyebabkan page yang masih sering digunakan justru diganti lebih dulu.

### **2. Trade-off Antara Kompleksitas dan Efisiensi**
Meskipun LRU memberikan performa yang lebih baik dalam meminimalkan page fault, algoritma ini memiliki **overhead implementasi yang lebih tinggi** karena perlu melacak waktu akses setiap page secara terus-menerus. Sebaliknya, FIFO sangat sederhana dan cepat diimplementasikan dengan struktur data queue, sehingga cocok untuk sistem dengan keterbatasan sumber daya komputasi. Pemilihan algoritma harus mempertimbangkan keseimbangan antara efisiensi memori dan kompleksitas komputasi.

### **3. Pengaruh Jumlah Frame terhadap Page Fault**
Jumlah frame yang tersedia di physical memory berpengaruh signifikan terhadap jumlah page fault - semakin banyak frame, semakin rendah page fault untuk kedua algoritma. Namun, FIFO dapat mengalami **Belady's Anomaly** dimana penambahan frame justru meningkatkan page fault pada pola akses tertentu, sedangkan LRU sebagai stack algorithm tidak mengalami anomali ini. Hal ini menunjukkan bahwa LRU lebih stabil dan dapat diprediksi performanya ketika konfigurasi memori berubah.

---
**##TUGAS**
╔════════════════════════════════════════════════════════════╗
║         GRAFIK PERBANDINGAN PAGE FAULTS                    ║
╠════════════════════════════════════════════════════════════╣
║                                                            ║
║  FIFO │██████████████████████████████████████████████ 9
║       │                                                    ║
║  LRU  │███████████████████████████████████████ 7
║       │                                                    ║
╠════════════════════════════════════════════════════════════╣
║  0    10    20    30    40    50                          ║
╚════════════════════════════════════════════════════════════╝

## Quiz
1. [Apa perbedaan utama FIFO dan LRU?]  
   **Jawaban:** Perbedaan utama antara algoritma FIFO (First-In, First-Out) dan LRU (Least Recently Used) terletak pada
    kriteria pemilihan data yang akan dihapus dari memori (cache):
    Logika Dasar:
    FIFO: Menghapus data yang paling lama masuk ke memori, tanpa memperhitungkan seberapa sering data tersebut diakses .
    LRU: Menghapus data yang paling lama tidak digunakan atau tidak diakses oleh sistem.
    Efisiensi:
    FIFO: Kurang efisien karena data yang sering digunakan bisa saja terhapus hanya karena ia masuk pertama kali . Ini
    dapat menyebabkan fenomena yang dikenal sebagai Belady’s  Anomalys.
    LRU: Umumnya lebih efisien dan memiliki performa lebih tinggi karena mempertahankan data yang masih aktif digunakan oleh sistem   .
    Implementasi:
    FIFO: Sangat mudah diimplementasikan menggunakan struktur data Queue (antrean) sederhana .
    LRU: Lebih kompleks dan memerlukan sumber daya tambahan (seperti timestamp atau daftar terhubung) untuk melacak waktu penggunaan terakhir setiap data. 
    Singkatnya, FIFO berfokus pada urutan kedatangan, sedangkan LRU berfokus pada pola penggunaan data . 

2. [Mengapa FIFO dapat menghasilkan Belady's Anomaly?]  
   **Jawaban:**  FIFO menghasilkan Belady's Anomaly (peningkatan jumlah page fault saat jumlah frame memori
   ditambah) karena ia bukan termasuk algoritma berbasis stack (non-stack algorithm). Berikut adalah alasan teknis mengapa hal
    ini terjadi: Pengabaian Pola Penggunaan: FIFO hanya mempedulikan urutan kedatangan data. Jika sebuah data sudah beradalama di memori tetapi sangat sering digunakan,
    FIFO akan tetap menghapusnya hanya karena data tersebut "paling tua" di antrean.Pelanggaran Sifat Subset: Pada algoritma berbasis
    stack (seperti LRU), kumpulan data dalam memori dengan \(n\) frame selalu merupakan subset dari data dalam memori dengan \(n+1\) frame. FIFO tidak menjamin hal ini.
   Penambahan frame dapat mengubah urutan penghapusan sedemikian rupa sehingga data yang seharusnya masih ada di memori justru
 terbuang lebih awal.Konflik dengan Pola Referensi: Untuk pola akses data tertentu, penambahan memori justru membuat data yang sering dibutuhkan "terpental" keluar dari antrean lebih cepat atau pada waktu yang salah, yang mengakibatkan sistem harus terus-menerus mengambil ulang data tersebut dari disk (page fault). Sebaliknya, algoritma seperti LRU tidak mengalami anomali ini karena selalu mempertahankan data yang paling baru digunakan, sehingga penambahan frame memori dipastikan akan menurunkan atau setidaknya mempertahankan jumlah page fault. 
4. [Menapa LRU umumnya menghasilkan performa lebih baik dibanding FIFO?]  
   **Jawaban:**LRU umumnya menghasilkan performa yang lebih baik dibandingkan FIFO dalam manajemen memori dan cache karena alasan-alasan berikut: 
Prinsip Lokalitas Temporal (Temporal Locality): LRU bekerja berdasarkan asumsi bahwa data yang baru saja diakses memiliki kemungkinan besar untuk diakses kembali dalam waktu dekat. Dengan mempertahankan data yang "baru saja digunakan", LRU menjaga data yang relevan tetap berada di memori.
Adaptasi terhadap Pola Penggunaan: Berbeda dengan FIFO yang hanya melihat urutan waktu masuk, LRU secara dinamis memperbarui status data setiap kali diakses. Ini mencegah penghapusan data yang sangat sering digunakan tetapi sudah lama berada di memori, sebuah kesalahan umum pada algoritma FIFO.
Minimalisasi Page Fault: Karena lebih cerdas dalam memilih data yang akan dihapus (yaitu data yang paling tidak mungkin digunakan lagi menurut riwayat akses), LRU cenderung menghasilkan jumlah page fault atau cache miss yang lebih rendah.
Bebas dari Anomali Belady: Sebagai algoritma berbasis stack, LRU menjamin bahwa penambahan jumlah frame memori akan selalu memberikan performa yang sama atau lebih baik. Hal ini tidak dijamin oleh FIFO, di mana penambahan memori terkadang justru menurunkan performa. 
Meskipun LRU memerlukan sumber daya lebih (overhead) untuk melacak urutan akses, efisiensi yang dihasilkannya dalam mengurangi akses ke penyimpanan sekunder (disk) yang lambat jauh lebih menguntungkan bagi performa sistem secara keseluruhan.  

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
