
# Laporan Praktikum Minggu [5]
Topik: [Penjadwalan CPU-FCFS dan SJF]

---

## Identitas
- **Nama**  : [Ayu Ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSRA]

---

## Tujuan
Tuliskan tujuan praktikum minggu ini.  

1.Menghitung waiting time dan turnaround time untuk algoritma FCFS dan SJF.
2.Menyajikan hasil perhitungan dalam tabel yang rapi dan mudah dibaca.
3.Membandingkan performa FCFS dan SJF berdasarkan hasil analisis.
4.Menjelaskan kelebihan dan kekurangan masing-masing algoritma.
5.Menyimpulkan kapan algoritma FCFS atau SJF lebih sesuai digunakan


1.Waiting time dihitung sebagai turnaround time dikurangi burst time proses, sedangkan turnaround time adalah completion time dikurangi arrival time. Perhitungan menggunakan contoh proses standar: P1 (arrival 0, burst 5), P2 (1,3), P3 (2,8), P4 (3,6). SJF lebih unggul dalam mengurangi rata-rata waiting time dan turnaround time dibandingkan FCFS.[1][2]

2.## Tabel FCFS
| Proses | CT | TAT | WT |
|--------|----|-----|----|
| P1     | 5  | 5   | 0  |
| P2     | 8  | 7   | 4  |
| P3     | 16 | 14  | 6  |
| P4     | 22 | 19  | 13 |
**Rata-rata TAT: 11.25, WT: 5.75**[1]

## Tabel SJF
| Proses | CT | TAT | WT |
|--------|----|-----|----|
| P1     | 5  | 5   | 0  |
| P2     | 8  | 7   | 4  |
| P3     | 14 | 11  | 5  |
| P4     | 22 | 20  | 12 |
**Rata-rata TAT: 10.75, WT: 5.25**[1]

3.## Perbandingan Performa
SJF menghasilkan rata-rata waiting time lebih rendah (5.25 vs 5.75) dan turnaround time lebih rendah (10.75 vs 11.25) dibandingkan FCFS, sehingga meningkatkan efisiensi CPU. FCFS mengakibatkan convoy effect di mana proses panjang menunda yang lain, sementara SJF memprioritaskan burst pendek untuk throughput lebih baik.

4.## Kelebihan dan Kekurangan
**FCFS:**
- Kelebihan: Mudah diimplementasikan, adil berdasarkan urutan kedatangan, tanpa starvation.
- Kekurangan: Waiting time panjang jika ada proses burst besar di awal, rendah utilisasi CPU.

**SJF:**
- Kelebihan: Minimum rata-rata waiting time, throughput tinggi.
- Kekurangan: Risiko starvation proses panjang, sulit prediksi burst time akurat.

5.## Kesimpulan Penggunaan
Gunakan FCFS untuk sistem sederhana dengan proses serupa atau saat fairness berdasarkan arrival penting. Pilih SJF saat burst time diketahui akurat dan prioritas pada mengurangi average waiting time, terutama lingkungan batch dengan job pendek dominan.



## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.
Teori dasar penjadwalan CPU FCFS dan SJF mendasari percobaan untuk mengoptimalkan utilisasi CPU melalui pengurutan eksekusi proses berdasarkan kriteria tertentu. Berikut ringkasan 4 poin utama.[1][3]

## Konsep Proses dan Scheduler
Setiap proses memiliki arrival time (waktu kedatangan) dan burst time (waktu CPU dibutuhkan); scheduler memilih proses dari ready queue untuk CPU. Tujuan utama: memaksimalkan utilisasi CPU, minimalkan waiting time (WT = TAT - burst time) dan turnaround time (TAT = completion time - arrival time).[8][1]

## FCFS (First-Come First-Served)
Algoritma non-preemptive ini menjalankan proses sesuai urutan kedatangan (FIFO), sederhana tanpa perlu prediksi burst time. Menghasilkan fairness tapi rentan convoy effect jika proses panjang datang lebih dulu.[2][5]

## SJF (Shortest Job First)
Non-preemptive SJF memprioritaskan proses dengan burst time terpendek saat CPU bebas, optimal untuk rata-rata WT minimum secara teori. Lebih efisien daripada FCFS tapi butuh estimasi burst akurat.[7][8]

## Kriteria Evaluasi
Performa diukur dari average WT, TAT, throughput, dan fairness; SJF unggul di WT tapi rawan starvation proses panjang, sementara FCFS adil tapi throughput rendah.[3][9]


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
1.Hitung waiting time dan turnaround time dari minimal 2 skenario FCFS dan SJF.
2.Sajikan hasil perhitungan dalam tabel perbandingan (FCFS vs SJF).
3.Analisis kelebihan dan kelemahan tiap algoritma.

1.Perhitungan waiting time (WT = TAT - burst time) dan turnaround time (TAT = completion time - arrival time) dilakukan untuk dua skenario proses berbeda guna membandingkan FCFS dan SJF (non-preemptive). Skenario 1 menggunakan proses P1(0,7), P2(2,4), P3(4,1), P4(5,4); Skenario 2 menggunakan P1(0,24), P2(0,3), P3(0,3).

2.## Tabel Perbandingan FCFS vs SJF

### Skenario 1
| Proses | FCFS CT | FCFS TAT | FCFS WT | SJF CT | SJF TAT | SJF WT |
|--------|---------|----------|---------|--------|---------|--------|
| P1     | 7       | 7        | 0       | 7      | 7       | 0      |
| P2     | 11      | 9        | 5       | 11     | 9       | 5      |
| P3     | 12      | 8        | 7       | 12     | 8       | 7      |
| P4     | 16      | 11       | 7       | 16     | 11      | 7      |
**Avg FCFS: TAT 8.75, WT 4.75 | Avg SJF: TAT 8.75, WT 4.75**

### Skenario 2
| Proses | FCFS CT | FCFS TAT | FCFS WT | SJF CT | SJF TAT | SJF WT |
|--------|---------|----------|---------|--------|---------|--------|
| P1     | 24      | 24       | 0       | 24     | 24      | 0      |
| P2     | 27      | 27       | 24      | 3      | 3       | 0      |
| P3     | 30      | 30       | 27      | 6      | 6       | 3      |
**Avg FCFS: TAT 27, WT 17 | Avg SJF: TAT 11, WT 1**

3.## Analisis Kelebihan dan Kelemahan
FCFS menawarkan kesederhanaan implementasi dan fairness berdasarkan arrival time, tanpa starvation, tetapi rentan convoy effect—proses panjang awal menunda semua proses lain, meningkatkan WT rata-rata signifikan (Skenario 2: 17 vs 1). SJF optimal untuk WT minimum dengan memprioritaskan burst pendek, meningkatkan throughput (Skenario 2: TAT turun 59%), namun berisiko starvation proses panjang dan memerlukan estimasi burst akurat yang sulit di real-time.



## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
Praktikum penjadwalan CPU FCFS dan SJF menunjukkan perbedaan signifikan dalam performa berdasarkan waiting time dan turnaround time pada berbagai skenario proses.

## Poin Kesimpulan Utama
- SJF secara konsisten menghasilkan rata-rata waiting time dan turnaround time lebih rendah dibanding FCFS, terutama saat ada campuran burst time pendek dan panjang (Skenario 2: WT SJF 1 vs FCFS 17).
- FCFS lebih sederhana dan adil berdasarkan urutan kedatangan tanpa starvation, cocok untuk sistem dengan proses homogen, sementara SJF optimal untuk throughput tinggi tapi berisiko starvation proses panjang.
- Pemilihan algoritma tergantung konteks: gunakan FCFS untuk fairness dan kemudahan, SJF saat burst time dapat diprediksi akurat demi efisiensi CPU maksimum.


---

## Quiz
1. [Apa perbedaan utama antara FCFS dan SJF?]  
   **Jawaban:** FCFS (First-Come, First-Served) dan SJF (Shortest Job First) adalah algoritma penjadwalan proses non-preemptive dalam sistem operasi. Perbedaan utama terletak pada cara pemilihan proses: FCFS menjalankan proses sesuai urutan kedatangan, sementara SJF memprioritaskan proses dengan burst time (waktu eksekusi) terpendek.
​Perbedaan Utama
FCFS sederhana dan adil berdasarkan urutan antrian, tapi rentan terhadap convoy effect di mana proses panjang menghambat proses pendek.
​SJF mengoptimalkan throughput dengan mengeksekusi job pendek lebih dulu, menghasilkan rata-rata waktu tunggu lebih rendah dibanding FCFS (misalnya, 4,71 menit vs 6,24 menit dalam simulasi).
 
2. [MengapaSJF dapat menghasilkan rata-rata waktu tunggu minimum?]  
   **Jawaban:**  Alasan SJF Minimalkan Waktu Tunggu
SJF meminimalkan rata-rata waktu tunggu karena secara matematis optimal untuk non-preemptive scheduling dengan burst time diketahui; job pendek tidak tertunda oleh job panjang, sehingga total waktu tunggu keseluruhan berkurang. Ini terbukti dalam berbagai simulasi di mana SJF unggul pada beban kerja ringan atau variasi durasi layanan
3. [Apa kelemahan SJF jika diterapkan pada sistem interaktif]  
   **Jawaban:**  Kelemahan SJF di Sistem Interaktif
SJF tidak cocok untuk sistem interaktif karena risiko starvation (kelaparan) pada proses panjang yang terus tertunda oleh proses pendek baru, menyebabkan waktu respons buruk dan ketidakadilan. Selain itu, memerlukan prediksi burst time akurat yang sulit di sistem real-time dinamis, serta tidak memberikan tanggapan cepat untuk interaksi pengguna.


---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
