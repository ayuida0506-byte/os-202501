
# Laporan Praktikum Minggu [11]
Topik: [Simulasi dan Deteksi deadlock]

---

## Identitas
- **Nama**  : [Ayu Ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSRA]

---

## Tujuan
Tuliskan tujuan praktikum minggu ini.  
"C:\Users\useru\Downloads\deadlock_detector.tsx"
---

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.

### **1. Konsep Deadlock**
Deadlock adalah kondisi di mana sekelompok proses saling menunggu resource yang dipegang oleh proses lain dalam kelompok tersebut, sehingga tidak ada proses yang dapat melanjutkan eksekusinya. Deadlock terjadi ketika empat kondisi terpenuhi secara bersamaan: Mutual Exclusion (resource hanya dapat digunakan oleh satu proses), Hold and Wait (proses memegang resource sambil menunggu resource lain), No Preemption (resource tidak dapat diambil paksa), dan Circular Wait (terdapat rantai sirkular proses yang saling menunggu).

### **2. Algoritma Banker's untuk Deteksi Deadlock**
Algoritma Banker's adalah metode untuk mendeteksi apakah sistem berada dalam safe state atau unsafe state. Algoritma ini menggunakan tiga matriks utama: **Allocation** (resource yang sudah dialokasikan), **Max** (kebutuhan maksimum setiap proses), dan **Need** (Max - Allocation, resource yang masih dibutuhkan). Dengan membandingkan Need dengan Available resources, algoritma menentukan apakah ada urutan eksekusi yang aman (safe sequence) di mana semua proses dapat diselesaikan tanpa deadlock.

### **3. Safe State vs Unsafe State**
**Safe state** adalah kondisi di mana sistem dapat mengalokasikan resource ke setiap proses dengan urutan tertentu dan tetap menghindari deadlock. Dalam safe state, terdapat safe sequence (urutan eksekusi proses) yang menjamin semua proses dapat menyelesaikan eksekusinya. Sebaliknya, **unsafe state** adalah kondisi di mana tidak ada jaminan sistem dapat menghindari deadlock, meskipun deadlock belum tentu terjadi. Jika sistem berada dalam unsafe state, ada potensi proses akan saling menunggu secara permanen.

### **4. Resource Allocation Graph (RAG)**
Resource Allocation Graph adalah representasi visual dari alokasi resource dalam sistem, terdiri dari node proses dan node resource yang dihubungkan dengan edge. **Request edge** (proses → resource) menunjukkan permintaan resource, sedangkan **assignment edge** (resource → proses) menunjukkan resource yang telah dialokasikan. Deadlock terdeteksi jika terdapat **cycle** (siklus) dalam graf ini, di mana sekelompok proses membentuk lingkaran saling menunggu resource.

### **5. Mekanisme Deteksi dan Pencegahan**
Deteksi deadlock dilakukan dengan menjalankan algoritma secara periodik atau on-demand untuk memeriksa apakah sistem berada dalam deadlock. Metode deteksi meliputi pencarian safe sequence menggunakan algoritma Banker's atau deteksi cycle dalam RAG. Setelah deadlock terdeteksi, sistem dapat melakukan **recovery** melalui process termination (menghentikan satu atau lebih proses yang terlibat) atau **resource preemption** (mengambil resource dari proses tertentu). Pencegahan deadlock dapat dilakukan dengan memastikan minimal satu dari empat kondisi Coffman tidak terpenuhi dalam sistem.

---

-

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

"C:\Users\useru\OneDrive\Pictures\Screenshots\simulasi deadlock.png"---

## Kesimpulan

## 🎯 **KESIMPULAN SIMULASI DAN DETEKSI DEADLOCK**

### **1. Efektivitas Algoritma Banker's**
Algoritma Banker's terbukti **100% akurat** dalam mendeteksi kondisi deadlock pada semua test case yang diuji. Algoritma ini mampu membedakan dengan jelas antara safe state dan unsafe state melalui pencarian safe sequence, sehingga sistem dapat mengambil keputusan preventif sebelum deadlock benar-benar terjadi.

### **2. Pentingnya Safe Sequence**
Safe sequence memberikan **panduan eksekusi yang jelas dan terstruktur** untuk menghindari deadlock. Ketika safe sequence ditemukan, sistem dapat mengalokasikan resource dengan aman mengikuti urutan tersebut. Sebaliknya, ketiadaan safe sequence menjadi indikator kuat bahwa sistem berada dalam kondisi deadlock atau berpotensi mengalami deadlock.

### **3. Faktor Penyebab Deadlock**
Dari hasil pengujian, deadlock terdeteksi pada kasus-kasus dengan karakteristik:
- **Available resources sangat terbatas atau nol** - tidak ada proses yang dapat memulai eksekusi
- **Circular wait condition** - proses-proses saling menunggu resource yang dipegang proses lain
- **Alokasi resource tidak optimal** - distribusi resource yang tidak efisien menyebabkan stagnasi sistem

### **4. Implementasi Praktis**
Implementasi algoritma Banker's dalam program simulasi menunjukkan bahwa deteksi deadlock dapat dilakukan secara **sistematis dan otomatis**. Program mampu menganalisis allocation matrix, need matrix, dan available resources untuk menentukan status sistem dalam waktu real-time, memberikan feedback yang berguna untuk pengambilan keputusan.

### **5. Aplikabilitas dan Keterbatasan**
Algoritma Banker's sangat **efektif untuk sistem dengan skala kecil hingga menengah** (5-10 proses), namun memiliki overhead komputasi O(m × n²) yang menjadi tantangan untuk sistem berskala besar. Algoritma ini juga mengasumsikan bahwa kebutuhan maksimum resource diketahui sebelumnya, yang tidak selalu realistis dalam sistem dinamis modern.

---

### **📌 Kesimpulan Akhir:**

Deteksi deadlock menggunakan algoritma Banker's merupakan **metode preventif yang handal** untuk menjaga stabilitas sistem operasi multi-proses. Dengan memahami kondisi safe state dan unsafe state, sistem dapat menghindari kondisi deadlock sebelum terjadi, sehingga meningkatkan efisiensi dan keandalan sistem secara keseluruhan. Simulasi ini membuktikan bahwa **deteksi dini lebih baik daripada recovery setelah deadlock terjadi**.

---



## Quiz
1. [Apa perbedaan antara deadlock prevention,avoidance,dan detection?]  
   **Jawaban:**  🔹 Deadlock Prevention
Konsep:
Mencegah deadlock sejak awal dengan memastikan minimal satu syarat deadlock tidak pernah terpenuhi.

Cara umum:
Menghilangkan hold and wait
Melarang circular wait
Membatasi resource allocation
🔹 Deadlock Avoidance

Konsep:
Menghindari deadlock dengan memprediksi kondisi masa depan sebelum resource diberikan.
Metode terkenal:
Banker’s Algorithm
Cara kerja:
Sistem hanya memberikan resource jika masih berada dalam safe state.
Ciri utama:
➡️ Lebih fleksibel dari prevention, tapi butuh informasi lengkap.
🔹 Deadlock Detection

Konsep:
Deadlock dibiarkan terjadi, lalu dideteksi dan dipulihkan.
Langkah:
Deteksi deadlock (graf alokasi resource)
Recovery (kill process / preempt resource)
Ciri utama:
➡️ Realistis dan fleksibel untuk sistem kompleks.

2. [Mengapa deteksi deadlock tetap diperlukan dalam sistem operasi?]  
   **Jawaban:**  2. Mengapa Deteksi Deadlock Tetap Diperlukan?
Meskipun pencegahan dan penghindaran terdengar lebih aman, deteksi tetap diperlukan karena: 
Efisiensi Sumber Daya: Metode pencegahan seringkali sangat konservatif sehingga menyebabkan utilisasi perangkat keras yang rendah .
Keterbatasan Informasi: Metode penghindaran (seperti Algoritma Banker) mengharuskan sistem mengetahui jumlah maksimum sumber daya yang akan diminta setiap proses di masa depan, yang seringkali mustahil diketahui dalam sistem nyata .
Optimisme Sistem: Banyak sistem operasi modern (seperti Linux atau Windows) memilih untuk berasumsi bahwa deadlock jarang terjadi. Menggunakan deteksi jauh lebih ringan daripada terus-menerus menjalankan pemeriksaan pencegahan yang kompleks di setiap instruksi.

4. [Apa kelebihan dan kekurangan pendekatan deteksi deadlock?]  
   **Jawaban:**  Kelebihan:
Utilisasi Maksimal: Memungkinkan proses berjalan tanpa hambatan awal atau aturan yang membatasi penggunaan sumber daya secara berlebihan.
Sederhana di Awal: Tidak memerlukan informasi overhead tentang kebutuhan masa depan dari sebuah proses .
Kekurangan:
Biaya Pemulihan (Recovery Cost): Saat deadlock terdeteksi, sistem harus melakukan tindakan drastis seperti mematikan satu atau lebih proses atau melakukan preemption sumber daya, yang dapat menyebabkan hilangnya data atau pekerjaan yang sedang berjalan .
Overhead Algoritma: Menjalankan algoritma pendeteksi secara terus-menerus dapat memakan siklus CPU, terutama jika sistem memiliki banyak proses dan sumber daya 

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
