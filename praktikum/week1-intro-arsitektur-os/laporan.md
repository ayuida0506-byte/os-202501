
# Laporan Praktikum Minggu 1
Topik:Arsitektur sistem operasi
---

## Identitas
- **Nama**  : Ayu Ida Nuraini
- **NIM**   : 250320588
- **Kelas** : 1DSRA

---

## DESKRIPSI
Tuliskan tujuan praktikum minggu ini.  
1.
## Tabel Perbandingan Kernel Mode vs User Mode

| Aspek              | Kernel Mode                          | User Mode                          |
|--------------------|--------------------------------------|------------------------------------|
| **Hak Akses**     | Akses penuh ke hardware dan memori   | Akses terbatas                    |
| **Instruksi Khusus** | Dapat menjalankan instruksi I/O, manajemen memori, dan interrupt | Tidak dapat menjalankan instruksi khusus |
| **Tujuan**        | Menjalankan kernel OS dan device driver | Menjalankan aplikasi pengguna     |
| **Keamanan**      | Risiko tinggi jika terjadi kesalahan | Lebih aman, kesalahan tidak merusak sistem |
| **Contoh**        | Kernel Linux, device driver          | Browser, editor, aplikasi pengguna |

Tabel ini menggambarkan pemisahan hak istimewa yang krusial dalam OS modern untuk menjaga stabilitas dan keamanan sistem. Kernel Mode beroperasi pada level privilege tertinggi, sementara User Mode membatasi akses untuk mencegah kerusakan sistem akibat bug aplikasi.

2.Mekanisme System Call (Panggilan Sistem)
System call adalah cara program user mode meminta layanan kernel.

Alur System Call:
Program berjalan di user mode
Program memanggil system call API (misalnya read(), fork())
Terjadi trap/interrupt → CPU berpindah ke kernel mode
Kernel menjalankan layanan yang diminta
Hasil dikembalikan → kembali ke user mode

Contoh System Call:
Manajemen proses: fork(), exec()
File: open(), read(), write()
Device: ioctl()
Komunikasi: pipe(), socket()
3.
| Arsitektur        | Ciri Utama                    | Kelebihan               | Kekurangan                 | Contoh      |
| ----------------- | ----------------------------- | ----------------------- | -------------------------- | ----------- |
| Monolithic Kernel | Semua layanan di kernel space | Performa cepat, efisien | Sulit dirawat, kurang aman | Linux, UNIX |
| Layered Approach  | OS dibagi per lapisan fungsi  | Mudah dipahami & debug  | Overhead antar lapisan     | THE OS      |
| Microkernel       | Kernel minimal + services     | Sangat aman, modular    | Overhead komunikasi tinggi | Minix, QNX  |
Ringkasan Singkat:
Monolithic → cepat tapi kompleks
Layered → rapi tapi kurang fleksibel
Microkernel → aman tapi lebih lambat

**##TUJUAN**

1.Peran Sistem Operasi dalam Arsitektur Komputer
Sistem operasi (OS) berperan sebagai penghubung (interface) antara hardware dan pengguna/aplikasi.

Peran utama OS:
1.Manajemen sumber daya → CPU, memori, storage, dan perangkat I/O
2.Pengendali eksekusi program → mengatur proses & penjadwalan
3.Keamanan & proteksi → membatasi akses user ke hardware
4.Abstraksi hardware → menyederhanakan penggunaan perangkat keras
5.Penyedia layanan sistem → melalui system call

📌 Tanpa OS, aplikasi harus mengatur hardware sendiri (tidak efisien & berisiko).
2.. Komponen Utama Sistem Operasi
a. Kernel

Inti dari sistem operasi yang berjalan di kernel mode.
Fungsi:
Manajemen proses
Manajemen memori
Penjadwalan CPU
Manajemen interrupt

b. System Call

Mekanisme komunikasi antara user mode dan kernel mode.
Fungsi:
Meminta layanan OS (file, proses, device)
Menjaga keamanan sistem
Contoh:
read(), write(), fork(), exec()
c. Device Driver

Penghubung antara OS dan perangkat keras.
Fungsi:
Mengontrol hardware spesifik (printer, keyboard, disk)
Menyembunyikan detail hardware dari kernel

d. File System

Mengatur penyimpanan dan akses data di media penyimpanan.
Fungsi:
Mengelola file & direktori
Hak akses file
Penyimpanan permanen
Contoh:
ext4 (Linux), NTFS (Windows).

3.Perbandingan Model Arsitektur Sistem Operasi
| Model           | Karakteristik           | Kelebihan                   | Kekurangan            | Contoh     |
| --------------- | ----------------------- | --------------------------- | --------------------- | ---------- |
| **Monolithic**  | Semua layanan di kernel | Cepat, efisien              | Kompleks, rawan error | Linux      |
| **Layered**     | OS dibagi lapisan       | Terstruktur, mudah dipahami | Kurang fleksibel      | THE OS     |
| **Microkernel** | Kernel minimal          | Aman, modular               | Overhead IPC tinggi   | Minix, QNX |
Kesimpulan singkat:
Monolithic → performa tinggi
Layered → struktur rapi
Microkernel → keamanan tinggi
4. diagram sederhana arsitektur

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.

1. **Sistem operasi berperan sebagai penghubung antara hardware dan aplikasi****, dengan menyediakan abstraksi perangkat keras serta mengelola sumber daya seperti CPU, memori, dan perangkat I/O.
2. **Arsitektur OS menentukan cara komponen sistem disusun dan berinteraksi**, yang memengaruhi kinerja, keamanan, dan kemudahan pemeliharaan sistem.
3. **Kernel merupakan inti sistem operasi** yang berjalan pada kernel mode dan bertanggung jawab atas manajemen proses, memori, serta penanganan interrupt.
4. **System call menjadi mekanisme komunikasi antara user mode dan kernel mode**, sehingga aplikasi dapat mengakses layanan sistem secara aman.
5. **Model arsitektur OS (monolithic, layered, dan microkernel)** memiliki karakteristik berbeda: monolithic unggul dalam performa, layered dalam keteraturan desain, dan microkernel dalam keamanan serta modularitas.


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

## 1. Makna Hasil Percobaan

Hasil percobaan menunjukkan bahwa sistem operasi bekerja sebagai **pengelola utama sumber daya komputer**. Informasi yang diperoleh dari perintah sistem (seperti identitas kernel, modul, dan pesan kernel) membuktikan bahwa **kernel aktif mengontrol perangkat keras, proses, dan memori**. Percobaan juga memperlihatkan pemisahan yang jelas antara **user mode** dan **kernel mode**, di mana pengguna tidak dapat mengakses hardware secara langsung tanpa perantara sistem operasi.

---

## 2. Keterkaitan Hasil dengan Teori

Hasil percobaan sesuai dengan teori sistem operasi, yaitu:

* **Kernel** berfungsi sebagai inti OS yang mengatur proses, memori, dan perangkat keras, yang terlihat dari informasi kernel dan modul yang sedang berjalan.
* **System call** menjadi mekanisme utama bagi aplikasi user untuk mengakses layanan kernel secara aman, sehingga tidak terjadi akses langsung ke hardware.
* **Arsitektur OS** (monolithic kernel pada Linux) memungkinkan layanan inti dan driver berjalan di kernel space, sehingga sistem lebih responsif tetapi memerlukan kontrol yang ketat terhadap kesalahan.

Dengan demikian, percobaan membuktikan bahwa konsep teoritis kernel, system call, dan arsitektur OS diterapkan secara nyata dalam sistem operasi.

---

## 3. Perbedaan Hasil pada Lingkungan OS Berbeda (Linux vs Windows)

| Aspek                  | Linux                            | Windows                              |
| ---------------------- | -------------------------------- | ------------------------------------ |
| Arsitektur kernel      | Monolithic (modular)             | Hybrid                               |
| Akses perintah sistem  | Terbuka & detail (CLI)           | Lebih terbatas (GUI dominan)         |
| Informasi kernel       | Mudah diakses (`uname`, `dmesg`) | Terbatas (PowerShell / tools khusus) |
| Manajemen modul/driver | Transparan (`lsmod`)             | Tertutup                             |
| Pendekatan sistem      | Open-source, fleksibel           | Proprietary, terkontrol              |

**Kesimpulan:**
Linux memberikan visibilitas yang lebih tinggi terhadap kerja kernel sehingga lebih cocok untuk percobaan dan pembelajaran sistem operasi, sedangkan Windows lebih berfokus pada kemudahan pengguna dan abstraksi sistem.

---



## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
1. Sistem operasi berperan sebagai penghubung antara hardware dan aplikasi dengan mengelola sumber daya melalui kernel, sehingga aplikasi dapat berjalan secara aman dan efisien.
2. Pemisahan antara **user mode** dan **kernel mode** serta penggunaan **system call** terbukti penting untuk menjaga keamanan dan stabilitas sistem operasi.
3. Perbedaan arsitektur OS (monolithic pada Linux dan hybrid pada Windows) memengaruhi cara pengelolaan kernel, driver, serta tingkat keterbukaan informasi sistem.


---
**##TUGAS**

1.Buat diagram arsitektur OS (format PNG/SVG).
2.Tuliskan ringkasan (±500 kata) mencakup:
Perbedaan monolithic kernel, microkernel, dan layered architecture.
Contoh OS yang menerapkan tiap model.
Analisis: model mana yang paling relevan untuk sistem modern.
3.Tambahkan hasil ke praktikum/week1-intro-arsitektur-os/laporan.md

# Arsitektur Sistem Operasi: Perbandingan dan Analisis

## Perbedaan Arsitektur Kernel

Arsitektur sistem operasi menentukan bagaimana komponen-komponen kernel diorganisir dan berinteraksi. Tiga model utama yang digunakan adalah monolithic kernel, microkernel, dan layered architecture, masing-masing dengan filosofi desain yang berbeda.

### Monolithic Kernel

Monolithic kernel menempatkan seluruh layanan sistem operasi dalam satu ruang kernel yang besar. Semua komponen seperti manajemen proses, manajemen memori, sistem file, device drivers, dan networking berjalan dalam kernel mode dengan hak akses penuh ke hardware. Komponen-komponen ini dapat saling memanggil secara langsung tanpa mekanisme komunikasi khusus, menghasilkan performa yang sangat tinggi karena minimnya overhead.

Keunggulan utama monolithic kernel adalah kecepatan eksekusi dan efisiensi. Akses langsung antar komponen tanpa context switching membuat operasi sistem sangat cepat. Namun, arsitektur ini memiliki kelemahan signifikan dalam hal maintainability dan reliability. Karena semua kode berjalan dalam privilege level yang sama, bug pada satu komponen dapat menyebabkan crash seluruh sistem. Ukuran kernel yang besar juga membuatnya sulit untuk di-maintain dan di-debug.

### Microkernel

Microkernel mengambil pendekatan minimalis dengan hanya menempatkan fungsi-fungsi paling esensial dalam kernel, seperti IPC (Inter-Process Communication), scheduling dasar, dan manajemen memori tingkat rendah. Layanan lain seperti device drivers, file system, dan network stack dipindahkan ke user space sebagai server terpisah yang berkomunikasi melalui message passing.

Filosofi ini menghasilkan sistem yang lebih modular, aman, dan robust. Ketika driver atau layanan mengalami kegagalan, hanya komponen tersebut yang perlu di-restart tanpa mempengaruhi keseluruhan sistem. Isolasi antar komponen juga meningkatkan keamanan karena bug tidak dapat dengan mudah mengkompromikan kernel. Namun, komunikasi antar komponen melalui IPC menambah overhead yang signifikan, mengakibatkan performa yang umumnya lebih rendah dibanding monolithic kernel.

### Layered Architecture

Layered architecture mengorganisir sistem operasi dalam hierarki berlapis, di mana setiap layer hanya dapat berkomunikasi dengan layer di bawahnya. Layer paling bawah berhubungan langsung dengan hardware, sementara layer atas menyediakan layanan yang semakin abstrak hingga mencapai interface pengguna. Struktur ini memudahkan desain dan debugging karena setiap layer dapat dikembangkan dan diuji secara independen.

Meskipun terstruktur dengan baik, arsitektur berlapis menghadapi tantangan praktis. Sulit menentukan layer yang tepat untuk setiap fungsi karena banyak komponen sistem memiliki ketergantungan kompleks. Overhead dari melewati banyak layer juga dapat mengurangi performa. Karena alasan ini, sistem operasi modern jarang mengimplementasikan layered architecture secara murni.

## Contoh Implementasi

**Monolithic kernel** digunakan oleh sistem operasi paling populer saat ini, yaitu Linux dan Unix tradisional. Linux, meskipun monolithic, menggunakan loadable kernel modules untuk menambah fleksibilitas. Windows versi awal dan MS-DOS juga menggunakan pendekatan monolithic.

**Microkernel** diadopsi oleh sistem seperti Minix (yang menginspirasi penciptaan Linux), QNX (digunakan dalam sistem embedded dan automotive seperti BlackBerry), L4 family, dan seL4 (microkernel yang telah diverifikasi secara formal untuk keamanan tinggi). GNU Hurd, bagian dari proyek GNU, juga dirancang sebagai microkernel meskipun pengembangannya lambat.

**Layered architecture** secara murni diimplementasikan pada THE operating system (salah satu OS eksperimental pertama) dan Venus OS. Namun, beberapa sistem modern seperti Windows NT menggunakan pendekatan hybrid yang menggabungkan elemen layered dengan karakteristik lain.

## Analisis untuk Sistem Modern

Untuk sistem modern, tidak ada satu model yang sempurna untuk semua kebutuhan. Konteks penggunaan menentukan arsitektur yang paling relevan.

**Monolithic kernel** tetap dominan untuk sistem general-purpose seperti desktop, laptop, dan server karena performanya yang superior. Linux membuktikan bahwa dengan desain yang baik dan penggunaan modules, monolithic kernel dapat mencapai keseimbangan antara performa dan fleksibilitas. Untuk aplikasi yang membutuhkan throughput tinggi dan latensi rendah, monolithic kernel masih menjadi pilihan terbaik.

**Microkernel** semakin relevan untuk sistem yang memprioritaskan keamanan, reliability, dan real-time performance. Industri otomotif, avionics, sistem medis, dan perangkat IoT kritis mengadopsi microkernel karena isolasi fault dan verifiability-nya. Dengan kemajuan hardware modern, overhead IPC menjadi kurang signifikan, membuat microkernel lebih viable untuk berbagai aplikasi.

**Hybrid approach** menjadi tren dominan, menggabungkan keunggulan berbagai arsitektur. macOS dengan XNU kernel menggabungkan Mach microkernel dengan komponen BSD monolithic. Windows menggunakan arsitektur hybrid dengan executive services yang berlapis di atas kernel mode. Pendekatan ini memberikan fleksibilitas untuk mengoptimalkan performa sambil mempertahankan modularitas.

Kesimpulannya, sistem modern cenderung pragmatis, memilih atau menggabungkan arsitektur berdasarkan kebutuhan spesifik. Monolithic kernel tetap relevan untuk performa, microkernel untuk keamanan dan kritikalitas tinggi, sementara hybrid architecture menawarkan kompromi terbaik untuk berbagai use case./



## Quiz

1.Sebutkan tiga fungsi utama sistem operasi.

   **Jawaban:**
   
1.Manajemen sumber daya (CPU, memori, dan perangkat I/O).
2.Manajemen proses (menjalankan, menjadwalkan, dan menghentikan proses).
3.Penyedia layanan bagi aplikasi melalui system call dan antarmuka pengguna. 

2. Jelaskan perbedaan antara kernel mode dan user mode.
 
   **Jawaban:**
   
   Kernel Mode: Mode eksekusi dengan hak akses penuh ke hardware dan memori, digunakan oleh kernel dan device driver untuk menjalankan operasi kritis.
User Mode: Mode eksekusi dengan hak akses terbatas, digunakan untuk menjalankan aplikasi pengguna dan harus menggunakan system call untuk mengakses layanan sistem.

3.Sebutkan contoh OS dengan arsitektur monolithic dan microkernel.

   **Jawaban:** 
   
Arsitektur Monolithic: Linux, UNIX.
Arsitektur Microkernel: Minix, QNX.
---

**
## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---
**F. Referensi**
1.Abraham Silberschatz, Peter Baer Galvin, Greg Gagne. Operating System Concepts, 10th Edition, Wiley, 2018.
2.Andrew S. Tanenbaum, Herbert Bos. Modern Operating Systems, 4th Edition, Pearson, 2015.
3.OSTEP – Operating Systems: Three Easy Pieces, 2018.
4.Linux Kernel Documentation – https://www.kernel.org/doc/

**

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
