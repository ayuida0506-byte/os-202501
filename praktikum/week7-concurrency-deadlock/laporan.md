
# Laporan Praktikum Minggu [7]
Topik: [Sinkronisasi proses dan masalah deadlock]

---

## Identitas
- **Nama**  : [Ayu Ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSRA]

---

## Tujuan
Tuliskan tujuan praktikum minggu ini.  
Contoh:  
1.Mengidentifikasi empat kondisi penyebab deadlock (mutual exclusion, hold and wait, no preemption, circular wait).
2.Menjelaskan mekanisme sinkronisasi menggunakan semaphore atau monitor.
3.Menganalisis dan memberikan solusi untuk kasus deadlock.
4.Berkolaborasi dalam tim untuk menyusun laporan analisis.
5.Menyajikan hasil studi kasus secara sistematis.

1.## Empat Kondisi Deadlock
- **Mutual Exclusion**: Sumber daya hanya bisa digunakan satu proses sekaligus (misalnya printer eksklusif).
- **Hold and Wait**: Proses memegang sumber daya sambil menunggu sumber daya lain yang dipegang proses berbeda.
- **No Preemption**: Sumber daya tidak bisa dipaksa dilepaskan dari proses; hanya proses itu sendiri yang bisa melepaskannya.
- **Circular Wait**: Rantai siklus saling tunggu terbentuk (P1 tunggu P2, P2 tunggu P1).

2.## Mekanisme Sinkronisasi
**Semaphore** menggunakan variabel integer (init >0) dengan operasi wait() (P: decrement) dan signal() (V: increment) untuk mutual exclusion dan sinkronisasi antar-proses, mencegah race condition. **Monitor** menyediakan abstraksi higher-level dengan prosedur mutual exclusion otomatis dan condition variables (wait/signal) untuk koordinasi yang lebih aman daripada semaphore manual.

3.## Analisis Kasus dan Solusi
**Kasus**: Dua proses P1 pegang R1 tunggu R2; P2 pegang R2 tunggu R1 (circular wait).
**Solusi**:
- **Pencegahan**: Hilangkan satu kondisi, misal urutkan sumber daya (request R1 sebelum R2 selalu).
- **Penghindaran**: Gunakan Banker's Algorithm cek safe state sebelum alokasi.
- **Deteksi**: Periodik buat Resource Allocation Graph, kill satu proses jika siklus terdeteksi.




## Dasar Teori
Sinkronisasi proses dalam sistem operasi mengatur eksekusi konkuren agar menghindari inkonsistensi data, sementara deadlock adalah kebuntuan ketika proses saling tunggu sumber daya secara siklik.

## Dasar Teori Sinkronisasi Proses
- **Race Condition**: Terjadi saat proses akses data bersama bersamaan, hasil akhir tergantung urutan eksekusi; dicegah dengan critical section yang hanya boleh diakses satu proses (mutual exclusion).
- **Solusi Critical Section**: Harus memenuhi mutual exclusion, progress (pemilihan proses tidak ditunda), dan bounded waiting (tidak ada starvation); Peterson's solution dan hardware (test-and-set) digunakan untuk implementasi.
- **Semaphore**: Variabel integer dengan operasi wait(P: decrement/block jika 0) dan signal(V: increment/wake), binary semaphore untuk mutex, counting untuk resource multi-unit.
- **Monitor**: Abstraksi tingkat tinggi dengan mutual exclusion otomatis, condition variables (wait/signal), lebih aman daripada semaphore manual untuk kompleksitas tinggi.

## Dasar Teori Masalah Deadlock
- **Empat Kondisi**: Mutual exclusion (sumber daya eksklusif), hold-and-wait (pegang sambil tunggu), no preemption (tidak bisa rampas), circular wait (siklus saling tunggu).
- **Penanganan**: Pencegahan (hilangkan satu kondisi, misal urut resource request), avoidance (Banker's Algorithm cek safe state), detection (Resource Allocation Graph cari siklus), recovery (kill proses/rollback).
- **Hubungan dengan Sinkronisasi**: Deadlock sering muncul dari semaphore salah urutan; solusi seperti hierarchical resource allocation atau timeout mencegahnya.


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
1.Analisis versi Dining Philosophers yang menyebabkan deadlock dan versi fixed yang bebas deadlock.
2.Dokumentasikan hasil diskusi kelompok ke dalam laporan.md.
3.Sertakan diagram atau screenshot hasil simulasi/pseudocode.
4.Laporkan temuan penyebab deadlock dan solusi pencegahannya.

1.---## Versi Deadlock Dining Philosophers
Versi deadlock terjadi ketika setiap filsuf mengambil sumpit kiri terlebih dahulu, kemudian menunggu sumpit kanan secara bersamaan, memenuhi keempat kondisi deadlock (mutual exclusion, hold-and-wait, no preemption, circular wait).

## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
Sinkronisasi proses dan deadlock merupakan konsep kunci dalam sistem operasi untuk mengelola eksekusi konkuren yang aman dan efisien.

## Kesimpulan Sinkronisasi Proses
- Sinkronisasi mencegah race condition melalui critical section dengan mutual exclusion, progress, dan bounded waiting, diimplementasikan via semaphore (P/V operations) atau monitor.
- Semaphore binary berfungsi sebagai mutex untuk satu proses, counting semaphore mengatur multiple resource instances, tapi rentan deadlock jika urutan P/V salah.
- Monitor menyediakan abstraksi lebih aman dengan mutual exclusion otomatis dan condition variables untuk koordinasi kompleks antar thread.

## Kesimpulan Masalah Deadlock
- Deadlock memerlukan empat kondisi simultan: mutual exclusion, hold-and-wait, no preemption, circular wait; pencegahan dengan menghilangkan salah satu kondisi (resource ordering efektif).
- Dining Philosophers ilustrasikan deadlock klasik; solusi resource hierarchy atau room semaphore (N-1 limit) memutus circular wait secara elegan.
- Strategi penanganan: prevention (hilangkan kondisi), avoidance (Banker's Algorithm), detection+recovery (Resource Allocation Graph), dengan tradeoff overhead vs robustness.


## Quiz
1. [sebutkan empat kondisi utama penyebab deadlock?]  
   **Jawaban:**  Empat Kondisi Deadlock
Mutual Exclusion: Sumber daya hanya dapat digunakan satu proses sekaligus.
Hold and Wait: Proses memegang sumber daya sambil menunggu sumber daya lain.
​No Preemption: Sumber daya tidak dapat dirampas dari proses.
​Circular Wait: Terbentuk siklus saling tunggu antar proses.

2. [Mengapa sinkronisasi diperlukan dalam sistem operasi?]  
   **Jawaban:**  Mengapa Sinkronisasi Diperlukan
   dalam sistem operasi karna untuk mencegah race condition di mana hasil eksekusi bergantung urutan proses konkuren, menjamin critical section hanya diakses satu proses (mutual exclusion) sambil memenuhi progress dan bounded waiting untuk fairness.

3. [Jelaskan perbedaan antara semaphore dan monitor?]  
   **Jawaban:** Perbedaan Semaphore dan Monitor
Semaphore: Variabel integer low-level dengan P(wait)/V(signal) manual; fleksibel tapi rawan deadlock jika urutan salah; cocok resource counting.
Monitor: Abstraksi high-level dengan mutual exclusion otomatis (entry/exit), condition variables (wait/signal), dan tipe aman; lebih mudah tapi kurang fleksibel untuk kasus kompleks.
 

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
