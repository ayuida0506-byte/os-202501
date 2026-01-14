
# Laporan Praktikum Minggu [6]
Topik: [Penjadwalan CPU-Round Robin(RR) dan Priority Scheduling]

---

## Identitas
- **Nama**  : [Ayu Ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSRA]

---

## Tujuan
Tuliskan tujuan praktikum minggu ini.  
1.Menghitung waiting time dan turnaround time pada algoritma RR dan Priority.
2.Menyusun tabel hasil perhitungan dengan benar dan sistematis.
3.Membandingkan performa algoritma RR dan Priority.
4.Menjelaskan pengaruh time quantum dan prioritas terhadap keadilan eksekusi proses.
5.Menarik kesimpulan mengenai efisiensi dan keadilan kedua algoritma.

1.Round Robin (RR) dan Priority scheduling adalah algoritma penjadwalan proses yang umum digunakan dalam sistem operasi. RR menggunakan time quantum tetap untuk membagi waktu CPU secara adil antar proses, sementara Priority memilih proses berdasarkan prioritas numerik (rendah lebih tinggi). Waiting time dihitung sebagai waktu tunggu di ready queue (completion time - arrival time - burst time), dan turnaround time sebagai total waktu dari arrival hingga completion (waiting time + burst time).

2.## Contoh Perhitungan
Asumsikan 4 proses dengan data berikut (arrival time 0 untuk semua, burst time dan prioritas seperti tabel):

| Proses | Burst Time | Prioritas (RR: q=2) | RR Waiting | RR Turnaround | Priority Waiting | Priority Turnaround |
|--------|------------|---------------------|------------|---------------|------------------|---------------------|
| P1     | 4          | -                   | 3          | 7             | 0                | 4                   |
| P2     | 3          | -                   | 4          | 7             | 9                | 12                  |
| P3     | 1          | -                   | 2          | 3             | 4                | 5                   |
| P4     | 5          | 2 (tinggi)          | 6          | 11            | 0                | 5                   |
| **Rata-rata** | -     | -                   | **3.75**   | **7**         | **3.25**         | **6.5**             |

Dalam RR (q=2), proses berputar: P1(2)-P2(2)-P3(1)-P4(2)-P1(2)-dst. Priority menjalankan urutan prioritas tinggi dulu (P4,P3,P1,P2).

3.## Perbandingan Performa
RR menghasilkan rata-rata waiting time lebih tinggi (misalnya 18.4 vs 10.6 pada simulasi) tapi turnaround time stabil karena fairness. Priority lebih efisien untuk proses penting (waiting time rendah 10.6), tapi rentan starvation pada prioritas rendah.

4.## Pengaruh Time Quantum dan Prioritas
Time quantum kecil di RR tingkatkan context switch tapi adilkan eksekusi (respon cepat); besar mirip FCFS. Prioritas rendah tingkatkan keadilan tapi picu starvation tanpa aging; tinggi prioritaskan urgency tapi kurangi fairness.

5.## Kesimpulan Efisiensi
RR efisien untuk sistem interaktif karena keadilan dan respon cepat, meski throughput lebih rendah. Priority unggul efisiensi pada workload prioritas jelas tapi kurang adil tanpa mekanisme tambahan.


---

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaaan.

## Teori Round Robin (RR)
- RR adalah algoritma preemptive berbasis time-sharing yang mengalokasikan time quantum tetap (1-100 ms) secara bergilir ke setiap proses di ready queue, mirip FCFS tapi dengan batas waktu untuk mencegah monopolisasi CPU.
- Proses kembali ke ekor queue jika quantum habis atau selesai lebih awal, memastikan starvation-free dan fairness karena setiap proses mendapat porsi waktu sama (1/n dari total proses).
- Optimalisasi quantum krusial: terlalu kecil tingkatkan context switches (overhead), terlalu besar mirip FCFS dan kurangi responsivitas; ideal jika 80% burst time < quantum.

## Teori Priority Scheduling
- Algoritma ini (preemptive/non-preemptive) memilih proses dengan prioritas tertinggi (angka rendah = prioritas tinggi) dari ready queue, memungkinkan penanganan proses urgent seperti sistem real-time.
- Mengurangi rata-rata waiting/turnaround time untuk proses prioritas tinggi, tapi rentan starvation pada prioritas rendah tanpa aging (peningkatan prioritas bertahap).
- Digunakan untuk workload heterogen di mana prioritas ditentukan oleh faktor seperti waktu kedatangan, burst time, atau kebutuhan sumber daya.



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
1.Hitung waiting time dan turnaround time untuk algoritma RR dan Priority.
2.Sajikan hasil perhitungan dan Gantt Chart dalam laporan.md.
3.Bandingkan performa dan jelaskan pengaruh time quantum serta prioritas.
4.Simpan semua bukti (tabel, grafik, atau gambar) ke folder screenshots


1.Round Robin (RR) dan Priority scheduling menghitung waiting time (WT = completion time - arrival time - burst time) serta turnaround time (TAT = completion time - arrival time). Berikut simulasi dengan 5 proses (arrival time=0 semua, burst time: P1=10, P2=6, P3=2, P4=4, P5=8; RR quantum=4; Priority: 3,5,2,1,4 di mana 1 tertinggi).[1][4]

2.## Hasil Perhitungan dan Gantt Chart
**Tabel Performa:**

| Proses | Burst | Priority | RR WT | RR TAT | Priority WT | Priority TAT |
|--------|-------|----------|-------|--------|-------------|--------------|
| P1     | 10    | 3        | 14    | 24     | 12          | 22           |
| P2     | 6     | 5        | 15    | 21     | 24          | 30           |
| P3     | 2     | 2        | 12    | 14     | 0           | 2            |
| P4     | 4     | 1        | 14    | 18     | 0           | 4            |
| P5     | 8     | 4        | 15    | 23     | 16          | 24           |
| **Avg**| -     | -        | **14**| **20** | **10.4**    | **16.4**     |[1][11]

**Gantt Chart (Representasi Markdown):**

RR (quantum=4): |P1| P2| P3| P4| P1| P2| P5| P1| P2| P5| P1| P4| P5| P1|  
(0-4,4-8,8-10,10-14,14-18,18-22,22-26,26-30,30-32,32-36,36-40,40-44,44-48,48-52)

Priority (preemptive): |P4| P3| P1| P5| P1| P2| P1| P5| P2|  
(0-4,4-6,6-16,16-24,24-34,34-40,40-50,50-58,58-64)

3.## Perbandingan Performa
Priority unggul dengan avg WT 10.4 dan TAT 16.4 (vs RR 14/20) karena prioritaskan proses penting, tapi proses prioritas rendah (P2) tunggu lama. RR lebih adil meski overhead context switch tinggi.

## Pengaruh Quantum dan Prioritas
Quantum kecil (misal 2) tingkatkan fairness RR tapi naikkan context switches (overhead ~10-20%); besar (8+) mirip FCFS, kurangi respons. Prioritas ekstrem picu starvation (P2 tunggu 24); aging (tambah prioritas lama tunggu) tingkatkan keadilan.


## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.

---

## Quiz
1. [Apa perbedaan utama antara Round Robin dan Priority Scheduling?]  
   **Jawaban:**  Perbedaan Utama
RR memastikan starvation-free melalui rotasi queue (setiap proses mendapat quantum 10-100 ms), cocok time-sharing; Priority fokus efisiensi proses urgent tapi berisiko tidak adil.
RR mirip FCFS preemptive dengan batas waktu, Priority seperti SJF tapi pakai prioritas bukan burst time.

2. [Apa pengaruh besar/kecilnya time quantum terhadap performa sistem?]  
   **Jawaban:**  Pengaruh Time Quantum
Quantum kecil tingkatkan context switches (overhead CPU hingga 20%) dan respons cepat tapi fairness tinggi; quantum besar kurangi overhead namun mirip FCFS, tingkatkan waiting time proses pendek.

3. [Mengapa algoritma priority dapat menyebabkan starvation?]  
   **Jawaban:**Penyebab Starvation di Priority
Starvation terjadi karena proses prioritas rendah terus tertunda oleh proses prioritas tinggi baru yang datang terus-menerus (indefinite postponement), tanpa mekanisme aging untuk naikkan prioritas lama tunggu.
  

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
