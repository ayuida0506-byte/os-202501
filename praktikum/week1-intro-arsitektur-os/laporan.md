
# Laporan Praktikum Minggu [X]
Topik:
--"Arsitektur Sistem Operasi dan Kernel"]

3
- **Nama**  : [Ayu Ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1IKRA]

---

## Tujuan
Tuliskan tujuan praktikum minggu ini.  
Contoh:  
> Mahasiswa mampu menjelaskan fungsi utama sistem operasi dan peran kernel serta system call.
---

---

### 🧠 **Fungsi Utama Sistem Operasi (OS)**

Sistem operasi adalah **penghubung antara pengguna (user) dan perangkat keras (hardware)**.
Fungsinya secara umum yaitu:

1. **Manajemen Proses (Process Management)**
   ➜ Mengatur proses yang sedang berjalan di komputer — termasuk menjalankan, menjeda, dan menghentikan program.
   Contoh: Saat kamu buka dua aplikasi, OS yang atur supaya keduanya bisa jalan bergantian tanpa bentrok.

2. **Manajemen Memori (Memory Management)**
   ➜ Mengatur penggunaan RAM agar program bisa berjalan efisien dan tidak saling berebut ruang.

3. **Manajemen Penyimpanan (File System Management)**
   ➜ Mengatur penyimpanan dan pengambilan data di hard disk atau SSD (membaca, menulis, membuat, dan menghapus file).

4. **Manajemen Perangkat (Device Management)**
   ➜ Mengatur komunikasi antara sistem dan perangkat keras seperti printer, keyboard, mouse, dan lainnya.

5. **Manajemen Keamanan dan Akses (Security & Access Control)**
   ➜ Melindungi data dan sumber daya sistem dari akses yang tidak sah dengan sistem login dan izin (permissions).

6. **Antarmuka Pengguna (User Interface)**
   ➜ Menyediakan cara agar user bisa berinteraksi dengan komputer — bisa berupa GUI (seperti Windows) atau CLI (Command Line Interface seperti Linux terminal).

---

### ⚙️ **Peran Kernel**

Kernel adalah **bagian inti dari sistem operasi**.
Ia bekerja di “balik layar” untuk mengatur semua interaksi antara software dan hardware.
Perannya meliputi:

* Mengatur penggunaan **CPU**, **memori**, dan **perangkat keras**.
* Menjalankan **proses multitasking** (menjalankan beberapa program sekaligus).
* Menangani **interupsi** (misal, saat kamu klik mouse, kernel tahu harus ngapain).
* Menyediakan **layanan dasar** agar aplikasi bisa berjalan di atasnya.

🧩 Sederhananya:

> Kernel = otak sistem operasi yang memastikan semuanya bekerja lancar.

---

### 📞 **Peran System Call**

System call adalah **jembatan antara program (user space)** dan **kernel (system space)**.
Program tidak bisa langsung mengakses hardware, jadi harus lewat “pintu” yang disediakan oleh system call.

Contohnya:

* `read()` dan `write()` → untuk membaca dan menulis file.
* `fork()` → membuat proses baru.
* `exec()` → menjalankan program lain.
* `open()` dan `close()` → membuka atau menutup file.

🧩 Analogi gampangnya:

> Kalau kernel itu petugas di dalam gedung, system call adalah **pintu dan interkom** yang kamu pakai untuk minta tolong ke petugas itu.

---

### 🔁 **Hubungan Ketiganya**

```
User → System Call → Kernel → Hardware
```

* **User** menjalankan aplikasi (misalnya membuka file).
* **Aplikasi** menggunakan **system call** untuk meminta layanan ke kernel.
* **Kernel** meneruskan permintaan itu ke **hardware**, lalu hasilnya dikembalikan ke user.

---





## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.---

---

### 🧠 **Ringkasan Teori Dasar**

1. **Konsep Sistem Operasi dan Kernel**
   Sistem operasi adalah perangkat lunak inti yang mengelola sumber daya komputer (CPU, memori, perangkat I/O) serta menyediakan antarmuka antara pengguna dan perangkat keras. Kernel adalah bagian utama sistem operasi yang bekerja langsung dengan perangkat keras dan menangani manajemen proses, memori, serta driver.

2. **Perintah `uname -a` (Informasi Sistem)**
   Perintah ini digunakan untuk menampilkan informasi detail tentang sistem operasi seperti nama kernel, versi, arsitektur mesin, dan nama host. Ini membantu mengenali **lingkungan sistem** tempat user bekerja, misalnya versi Linux atau kernel yang digunakan.

3. **Perintah `whoami` (Identitas Pengguna Aktif)**
   `whoami` menampilkan nama pengguna yang sedang aktif atau login ke sistem. Ini menunjukkan konsep **manajemen user dan hak akses** pada sistem operasi, di mana setiap pengguna memiliki identitas dan izin tertentu.

4. **Perintah `lsmod | head` (Daftar Modul Kernel)**
   `lsmod` menampilkan daftar modul kernel yang sedang dimuat (loaded modules), seperti driver perangkat keras. Pipa `| head` digunakan untuk menampilkan hanya beberapa baris awal daftar tersebut. Ini menggambarkan **konsep modularitas kernel**, di mana fungsi tambahan dapat dimasukkan tanpa perlu memodifikasi kernel utama.

5. **Perintah `dmesg | head` (Pesan Kernel dan Log Sistem)**
   `dmesg` menampilkan pesan atau log dari kernel, terutama terkait proses booting, deteksi perangkat keras, dan aktivitas sistem. Dengan `| head`, hanya beberapa pesan awal yang terlihat. Ini menunjukkan **mekanisme komunikasi antara kernel dan pengguna** melalui log sistem.

---

## Langkah Praktikum
1. Langkah-langkah yang dilakukan.  
2. Perintah yang dijalankan.  
3. File dan kode yang dibuat.  
4. Commit message yang digunakan.

-pastikan linux [buntu/wsl] sudah terinstal 
pastikan git sudah terkonfigurasi--

## Kode / Perintah
Baik 👍
Gambar itu menunjukkan bagian **eksperimen dasar sistem operasi Linux**, di mana kamu diminta menjalankan beberapa perintah terminal berikut:

```
uname -a
whoami
lsmod | head
dmesg | head
```

Berikut penjelasan dan analisis singkatnya 👇

---

### 🧩 1. `uname -a`

**Fungsi:**
Menampilkan informasi lengkap tentang sistem operasi dan kernel Linux yang digunakan.

**Output contoh:**

```
Linux ubuntu 5.15.0-92-generic #102-Ubuntu SMP x86_64 GNU/Linux
```

**Analisis:**
Dari output ini bisa diketahui jenis kernel (misal `5.15.0-92-generic`), arsitektur sistem (`x86_64`), dan nama OS (`Ubuntu`).

---

### 👤 2. `whoami`

**Fungsi:**
Menampilkan nama pengguna (user) yang sedang aktif di terminal.

**Output contoh:**

```
student
```

**Analisis:**
Menunjukkan bahwa perintah dijalankan oleh user bernama “student”. Penting untuk memastikan hak akses user terhadap sistem.

---

### ⚙️ 3. `lsmod | head`

**Fungsi:**
Menampilkan daftar **modul kernel** yang sedang dimuat di sistem.
(`| head` hanya menampilkan beberapa baris pertama agar tidak terlalu panjang.)

**Output contoh:**

```
Module                  Size  Used by
snd_hda_codec_hdmi      57344  1
snd_hda_intel           45056  3
i915                   1617920  4
```

**Analisis:**
Baris-baris ini menunjukkan modul kernel aktif seperti:

* `snd_hda_codec_hdmi` → untuk audio HDMI
* `i915` → untuk driver grafis Intel
  Modul-modul ini penting karena menambah kemampuan kernel tanpa harus reboot atau recompile.

---

### 🧠 4. `dmesg | head`

**Fungsi:**
Menampilkan log pesan kernel (biasanya dari proses booting atau deteksi hardware).

**Output contoh:**

```
[    0.000000] Linux version 5.15.0-92-generic ...
[    0.123456] ACPI: Initializing tables
```

**Analisis:**
Log ini memperlihatkan proses awal kernel saat mendeteksi perangkat keras, seperti CPU, RAM, atau driver yang dimuat.

---

### 🔍 Kesimpulan Analisis Modul Kernel

* Modul kernel adalah komponen tambahan yang memperluas fungsi kernel (misalnya driver perangkat).
* Dengan `lsmod`, kita bisa melihat modul mana yang aktif.
* `dmesg` membantu memastikan modul tersebut berhasil dimuat tanpa error.
* Eksperimen ini penting untuk memahami bagaimana sistem operasi berinteraksi dengan perangkat keras melalui kernel.

--


## Hasil Eksekusi
Sertakan screenshot hasil percobaan atau diagram:vv- Jelaskan makna hasil percobaan.  
![Screenshot hasil](screenshots/example.png)

---

## Analisis
- Jelaskan makna hasil percobaan.  
- Hubungkan hasil dengan teori (fungsi kernel, system call, arsitektur OS).  
- Apa perbedaan hasil di lingkungan OS berbeda (Linux vs Windows)?  
  vBaik 👍
Gambar itu menunjukkan bagian **eksperimen dasar sistem operasi Linux**, di mana kamu diminta menjalankan beberapa perintah terminal berikut:

```
uname -a
whoami
lsmod | head
dmesg | head
```

Berikut penjelasan dan analisis singkatnya 👇

---

### 🧩 1. `uname -a`

**Fungsi:**
Menampilkan informasi lengkap tentang sistem operasi dan kernel Linux yang digunakan.

**Output contoh:**

```
Linux ubuntu 5.15.0-92-generic #102-Ubuntu SMP x86_64 GNU/Linux
```

**Analisis:**
Dari output ini bisa diketahui jenis kernel (misal `5.15.0-92-generic`), arsitektur sistem (`x86_64`), dan nama OS (`Ubuntu`).

---

### 👤 2. `whoami`

**Fungsi:**
Menampilkan nama pengguna (user) yang sedang aktif di terminal.

**Output contoh:**

```
student
```

**Analisis:**
Menunjukkan bahwa perintah dijalankan oleh user bernama “student”. Penting untuk memastikan hak akses user terhadap sistem.

---

### ⚙️ 3. `lsmod | head`

**Fungsi:**
Menampilkan daftar **modul kernel** yang sedang dimuat di sistem.
(`| head` hanya menampilkan beberapa baris pertama agar tidak terlalu panjang.)

**Output contoh:**

```
Module                  Size  Used by
snd_hda_codec_hdmi      57344  1
snd_hda_intel           45056  3
i915                   1617920  4
```

**Analisis:**
Baris-baris ini menunjukkan modul kernel aktif seperti:

* `snd_hda_codec_hdmi` → untuk audio HDMI
* `i915` → untuk driver grafis Intel
  Modul-modul ini penting karena menambah kemampuan kernel tanpa harus reboot atau recompile.

---

### 🧠 4. `dmesg | head`

**Fungsi:**
Menampilkan log pesan kernel (biasanya dari proses booting atau deteksi hardware).

**Output contoh:**

```
[    0.000000] Linux version 5.15.0-92-generic ...
[    0.123456] ACPI: Initializing tables
```

**Analisis:**
Log ini memperlihatkan proses awal kernel saat mendeteksi perangkat keras, seperti CPU, RAM, atau driver yang dimuat.

---

Asumsi saya, percobaan yang dimaksud adalah percobaan praktikum atau simulasi yang saya lakukan, yang melibatkan interaksi dengan komponen-komponen tersebut (misalnya, membuat program yang menggunakan system call, mengamati penggunaan memori/CPU, atau membandingkan kecepatan eksekusi).
Untuk memberikan analisis yang akurat, saya memerlukan data data yang lengkap
Pertanyaan Klarifikasi yang Perlu Dijawab=
 
1. Makna Hasil Percobaan
Makna hasil percobaan adalah interpretasi dari data numerik atau observasi yang Anda peroleh, yang menunjukkan bagaimana komponen OS bekerja dalam situasi nyata.
 * Hasil terkait System Call: Jika Anda mengukur waktu eksekusi system call (misalnya read(), write(), fork()), maknanya adalah menunjukkan overhead yang dibutuhkan oleh kernel untuk melakukan context switch dari user mode ke kernel mode dan menjalankan operasi I/O atau manajemen proses.
 * Hasil terkait Fungsi Kernel (Contoh: Penjadwalan): Jika hasilnya menunjukkan satu proses mendapatkan lebih banyak waktu CPU daripada yang lain, maknanya adalah demonstrasi dari algoritma penjadwalan kernel yang sedang aktif, mungkin berdasarkan prioritas atau kebijakan waktu yang sama (round robin).
 * Hasil terkait Arsitektur OS (Contoh: Komunikasi Antar Proses - IPC): Jika Anda mengukur kecepatan IPC, maknanya adalah demonstrasi dari efisiensi implementasi mekanisme IPC (shared memory akan lebih cepat daripada message passing), yang merupakan bagian dari desain arsitektur OS.
2. Hubungan dengan Teori (Fungsi Kernel, System Call, Arsitektur OS)
Anda harus menghubungkan temuan eksperimental Anda dengan konsep teoretis:
| Hasil Percobaan | Konsep Teori yang Terkait | Hubungan Teori-Praktik |
|---|---|---|
| Waktu eksekusi system call yang relatif lama | System Call (sebagai interface antara aplikasi dan OS), Mode Ganda (User mode dan Kernel mode), Fungsi Kernel (I/O, manajemen proses) | Overhead waktu tersebut adalah biaya context switch dan eksekusi kode kernel untuk menjalankan fungsi yang diminta. Ini membuktikan system call adalah mekanisme yang relatif mahal dibandingkan operasi user mode. |
| Perbedaan performa antar proses/aplikasi | Fungsi Kernel (Penjadwalan CPU, Manajemen Memori) | Hasil ini menunjukkan kebijakan penjadwalan yang digunakan kernel (misalnya, time slice, prioritas) dan bagaimana manajemen sumber daya (misalnya, paging atau swapping) memengaruhi waktu eksekusi. |
| Ketergantungan library atau driver pada performa I/O | Arsitektur OS (Modularitas, Lapisan, Monolitik vs Microkernel) | Ini membuktikan bagaimana modularitas atau lapisan arsitektur (misalnya, driver berada di ruang kernel atau ruang user) memengaruhi efisiensi path I/O. |
3. Perbedaan Hasil di Lingkungan OS Berbeda (Linux vs Windows)
Perbedaan hasil antara Linux (umumnya berarsitektur Monolitik atau Modular Monolitik) dan Windows (umumnya berarsitektur Hibrida atau Microkernel yang dimodifikasi) dapat dijelaskan sebagai berikut:
| Aspek | Linux (Kernel Monolitik/Modular) | Windows (Kernel Hibrida) | Perbedaan yang Mungkin Terlihat dalam Percobaan |
|---|---|---|---|
| Overhead System Call | Cenderung lebih rendah (kode kernel berjalan di ruang alamat tunggal, context switch lebih langsung). | Cenderung lebih tinggi (komponen utama kernel berjalan di ruang kernel, namun banyak subsistem seperti Win32/POSIX berada di ruang user - arsitektur hibrida). | Waktu eksekusi system call yang mendasar mungkin lebih cepat di Linux, tergantung pada kompleksitas fungsinya. |
| Penjadwalan (Multithreading) | Penjadwal seperti CFS (Completely Fair Scheduler) berorientasi pada keadilan dan performa server, cepat dalam context switch. | Penjadwal berorientasi pada responsivitas desktop dan throughput. | Linux mungkin menunjukkan keadilan yang lebih baik antar proses yang bersaing; Windows mungkin menunjukkan latensi rendah yang lebih baik untuk aplikasi foreground. |
| Implementasi IPC | Umumnya menggunakan Pipes, Shared Memory, Sockets, dll. Cepat dan straightforward. | Menggunakan mekanisme seperti LPC (Local Procedure Call), yang lebih terstruktur seperti message passing kecil (seperti microkernel). | Shared memory akan sangat cepat di kedua OS, tetapi message passing Windows mungkin memiliki overhead yang berbeda karena sifat arsitektur hibridanya. |

.

---

## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
Berdasarkan kerangka analisis teoretis yang telah disampaikan, berikut adalah Kesimpulan Umum yang dapat ditarik dari hasil percobaan yang melibatkan fungsi kernel, system call, dan arsitektur OS:
Kesimpulan
Hasil percobaan Anda secara keseluruhan berfungsi sebagai bukti empiris yang memvalidasi konsep-konsep inti dari Sistem Operasi:
 * System Call adalah Mekanisme Krusial yang Memiliki Overhead:
   * Setiap kali aplikasi membutuhkan layanan OS (misalnya I/O atau manajemen memori), harus dilakukan system call. Hasil percobaan menunjukkan bahwa operasi ini membutuhkan waktu yang signifikan (dibandingkan operasi user-mode), yang membuktikan adanya overhead context switch (perpindahan dari user mode ke kernel mode) dan eksekusi kode kernel.
 * Kernel Adalah Pengendali Sumber Daya Utama yang Aktif Bekerja:
   * Hasil yang berkaitan dengan performa (waktu eksekusi, penggunaan CPU/Memori) menunjukkan bahwa fungsi kernel (terutama penjadwalan CPU dan manajemen memori) secara aktif mengalokasikan dan mengatur sumber daya. Perbedaan performa antar proses adalah demonstrasi langsung dari kebijakan dan algoritma penjadwalan yang digunakan oleh kernel (misalnya, fairness vs. responsiveness).
 * Arsitektur OS Mendikte Efisiensi dan Performa:
   * Perbandingan hasil di lingkungan OS yang berbeda (Linux vs. Windows) membuktikan bahwa arsitektur OS (Monolitik, Hibrida, Microkernel) secara fundamental memengaruhi performa. Perbedaan kecepatan system call atau mekanisme IPC menunjukkan bahwa pilihan desain kernel (misalnya, seberapa dekat driver atau layanan berada dengan inti kernel) memiliki dampak nyata pada efisiensi path eksekusi dan overhead secara keseluruhan.
Singkatnya: Percobaan tersebut menunjukkan bahwa kernel adalah jantung operasional OS, system call adalah pintu masuknya yang memiliki biaya, dan pilihan arsitektur OS (seperti Linux vs. Windows) menghasilkan implementasi dan performa yang berbeda untuk fungsi-fungsi dasar tersebut.
---

## Quiz
1.[Sebutkan 3 fungsi utama sistem operasi] 
   **Jawaban:** 
3 Fungsi Utama Sistem Operasi
Tiga fungsi utama Sistem Operasi (OS) adalah:
 * Manajemen Sumber Daya (Resource Management): Mengelola dan mengalokasikan semua sumber daya perangkat keras (hardware) seperti CPU, Memori Utama (RAM), dan Perangkat I/O (disk, printer, keyboard). Tujuannya adalah memastikan setiap program mendapatkan alokasi yang adil dan efisien.
 * Menyediakan Antarmuka (User Interface): Menyediakan cara bagi pengguna (melalui GUI, CLI, atau sentuhan) dan program aplikasi untuk berinteraksi dengan komputer. Ini memungkinkan pengguna menjalankan program tanpa harus memahami detail teknis perangkat keras.
 * Manajemen Proses dan File: Mengatur jalannya program (proses) secara bersamaan (multitasking) dengan melakukan penjadwalan CPU, serta mengelola penyimpanan dan organisasi data (file) di disk.
    
2. [Jelaskan perbedaan antara kernel mode dan user mode]  
   **Jawaban:**  kernel mode dan user mode adalah dua tingkat proteksi/hak istimewa yang digunakan oleh CPU ,untuk memisahkan fungsi inti sistem operasi dari bahasa biasa.
perbedaannya dalam mode kernel ,CPU memiliki akses tak terbatas ke sumber daya sistem dan dapat menjalankan instruksi istimewa.Sementara itu,dalam user mode akses dibatasi dan operasi tertentu dilarang untuk memastikan stabilitas dan keamanan sistem.
4. [Sebutkan contoh os dengan arsitektur monolithic dan microkernel]  
   **Jawaban:** Arsitektur monolithic= Linux,windows,dan keluarga UNIX [termasuk BSD]
                 Arsitektur microkernel=QNX,L4.Symbian,MINIX
---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  
mengenal hal baru yang belum pernah dipelajari,cara mengatasinya adalah mempelajarinya dengan cara mengenalnya lebih dalam
---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
