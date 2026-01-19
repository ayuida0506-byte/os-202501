
# Laporan Praktikum Minggu [14]
Topik: [Penyusunan Laporan Praktikum Format IMRAD]

---

## Identitas
- **Nama**  : [Ayu Ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSRA]

---

## Tujuan
Tuliskan tujuan praktikum minggu ini.  
Contoh:  
1.Menyusun laporan praktikum dengan struktur ilmiah (Pendahuluan–Metode–Hasil–Pembahasan–Kesimpulan).
2.Menyajikan hasil uji dalam bentuk tabel dan/atau grafik yang jelas.
3.Menuliskan analisis hasil dengan argumentasi yang logis.
4.Menyusun sitasi dan daftar pustaka dengan format yang konsisten.
5.Mengunggah draft laporan ke repositori dengan rapi dan tepat waktu.

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.
-

## 1. Ringkasan Teori (Introduction)

Praktikum ini didasarkan pada konsep dasar sistem operasi dan virtualisasi. Sistem operasi berfungsi sebagai penghubung antara perangkat keras dan pengguna, serta mengelola sumber daya seperti CPU, memori, dan perangkat I/O. Virtualisasi memungkinkan satu komputer fisik menjalankan beberapa sistem operasi secara bersamaan melalui mesin virtual (Virtual Machine).

Virtual Machine bekerja dengan bantuan hypervisor yang bertugas mengalokasikan sumber daya fisik ke sistem operasi guest. Dengan virtualisasi, pengguna dapat melakukan pengujian, simulasi, dan pembelajaran sistem operasi tanpa mengganggu sistem utama (host).

Penggunaan perintah dasar Linux juga menjadi bagian penting dalam praktikum ini karena perintah tersebut digunakan untuk manajemen file, proses, serta pengamatan kondisi sistem. Pemahaman teori ini menjadi landasan untuk melakukan percobaan secara efektif dan sistematis.

---

## 2. Metode (Methods)

Percobaan dilakukan dengan menginstal perangkat lunak virtualisasi (seperti VirtualBox/VMware), kemudian membuat dan menjalankan sistem operasi guest. Selanjutnya dilakukan pengujian menggunakan beberapa perintah sistem untuk mengamati informasi kernel, user, dan modul sistem.

---

## 3. Hasil Percobaan (Results)

Berdasarkan percobaan yang telah dilakukan, sistem operasi guest berhasil dijalankan di dalam mesin virtual tanpa mengganggu sistem operasi host. Perintah-perintah Linux seperti `uname`, `whoami`, dan `lsmod` berhasil menampilkan informasi sistem sesuai teori yang dipelajari.

Hasil pengamatan menunjukkan bahwa kernel berperan penting dalam mengatur komunikasi antara perangkat keras dan perangkat lunak. Informasi yang ditampilkan oleh sistem sesuai dengan konsep arsitektur sistem operasi, khususnya terkait manajemen proses dan modul kernel.

---

## 4. Pembahasan Singkat (Discussion)

Hasil percobaan sesuai dengan teori yang telah dipelajari, di mana virtualisasi memungkinkan simulasi lingkungan sistem operasi secara aman dan efisien. Percobaan ini membantu memahami cara kerja sistem operasi secara langsung melalui praktik.

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

## TUGAS
1.Susun laporan praktikum format IMRAD di praktikum/week14-laporan-imrad/laporan.md.
2.Sertakan minimal 1 tabel dan 1 gambar (screenshot/grafik).
3.Sertakan sitasi dan daftar pustaka.
4.Pastikan struktur folder rapi sesuai ketentuan.


## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.

Kesimpulan 1: Efektivitas Resource Limiting Menggunakan Linux Cgroups
Praktikum ini membuktikan bahwa Docker menggunakan mekanisme Linux cgroups untuk mengimplementasikan resource limiting yang efektif pada container. Hasil pengujian menunjukkan bahwa CPU limiting dengan parameter --cpus="0.5" memperlambat waktu eksekusi operasi CPU-intensive hingga hampir 2 kali lipat melalui mekanisme throttling, tanpa menyebabkan error atau kegagalan aplikasi. Sebaliknya, memory limiting dengan --memory="256m" bersifat hard limit yang dapat menyebabkan MemoryError atau container termination oleh OOM Killer ketika aplikasi mencoba mengalokasi memory melebihi batas yang ditentukan. Perbedaan karakteristik ini menunjukkan bahwa CPU adalah compressible resource (dapat di-throttle), sedangkan memory adalah non-compressible resource (tidak dapat dikompres), sehingga memerlukan pendekatan yang berbeda dalam strategi resource management.
Kesimpulan 2: Dampak Signifikan Resource Limiting terhadap Performa Aplikasi
Berdasarkan hasil pengamatan dan analisis data, pembatasan resource secara signifikan mempengaruhi performa aplikasi dengan pola yang dapat diprediksi. Container dengan CPU limit 0.5 cores mengalami peningkatan waktu eksekusi pada CPU test, sementara memory test relatif tidak terpengaruh karena operasi alokasi memory bukan CPU-bound task. Container dengan memory limit 256 MB menunjukkan potensi kegagalan alokasi memory atau penurunan performa drastis akibat excessive swapping. Kombinasi kedua limit (CPU 0.5 cores + Memory 256 MB) menciptakan environment paling restriktif dengan total waktu eksekusi paling lama, menunjukkan efek kumulatif dari multiple resource constraints. Temuan ini mengkonfirmasi bahwa resource limiting menciptakan trade-off antara performa aplikasi dan stabilitas sistem, dimana pembatasan yang terlalu ketat dapat menghambat performa, namun tanpa batasan dapat mengancam stabilitas keseluruhan sistem.
Kesimpulan 3: Pentingnya Resource Limiting sebagai Best Practice Production Environment
Praktikum ini mendemonstrasikan bahwa resource limiting merupakan best practice yang wajib diterapkan dalam production environment untuk menjaga stabilitas, fairness, dan efisiensi sistem containerized. Tanpa resource limiting, ditemukan bahwa satu container dapat mengkonsumsi seluruh resource host (noisy neighbor problem), mengganggu atau bahkan menghentikan container lain yang berjalan di host yang sama. Implementasi resource limit yang tepat memastikan fair resource allocation, meningkatkan predictability performa aplikasi, memudahkan capacity planning, dan mengoptimalkan cost terutama di cloud environment. Namun, penentuan nilai limit yang optimal memerlukan profiling aplikasi yang cermat, monitoring berkelanjutan, dan iterative tuning untuk mencapai keseimbangan antara performa maksimal dan stabilitas sistem. Hasil praktikum menunjukkan bahwa memahami karakteristik aplikasi (CPU-bound vs memory-bound) menjadi kunci dalam menentukan strategi resource limiting yang tepat.

---

## Quiz
1. [Mengapa format IMRAD membantu membuat laporan laporan praktikum lebih ilmiah dan mudah dievaluasi?]  
   **Jawaban:**  Karena format IMRAD menyusun laporan secara sistematis dan logis, memisahkan latar belakang, metode, hasil, dan analisis. Struktur ini memudahkan dosen atau evaluator menilai kelengkapan, kejelasan metode, serta kesesuaian hasil dengan tujuan praktikum.
2. [Apa perbedaan antara bagian hasil dan pembahasan?]  
   **Jawaban:**  Bagian hasil menyajikan data atau temuan percobaan secara objektif dalam bentuk tabel, grafik, atau deskripsi singkat, sedangkan bagian pembahasan berisi analisis, interpretasi hasil, serta perbandingan dengan teori atau ekspektasi.
3. [Mengapa sitasi dan daftar pustaka penting,bahkan untuk laporan praktikum?
   **Jawaban:**  Sitasi dan daftar pustaka penting untuk menunjukkan dasar teori yang digunakan, menghargai sumber ilmiah, serta meningkatkan kredibilitas dan keabsahan laporan praktikum.

---
****##REFERENSI
Daftar Pustaka
Silberschatz, A., Galvin, P. B., & Gagne, G. (2018). Operating System Concepts (10th ed.). John Wiley & Sons.
Tanenbaum, A. S., & Bos, H. (2015). Modern Operating Systems (4th ed.). Pearson Education.
Arpaci-Dusseau, R. H., & Arpaci-Dusseau, A. C. (2018). Operating Systems: Three Easy Pieces (OSTEP). https://pages.cs.wisc.edu/~remzi/OSTEP
## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
