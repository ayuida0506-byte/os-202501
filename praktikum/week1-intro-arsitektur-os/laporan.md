
# Laporan Praktikum Minggu [X]
Topik: [ Arsitektur Sistem Operasi dan Kernel]

---

## Identitas
- **Nama**  : [Ayu Ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSRA]

---

## Tujuan
Tuliskan tujuan praktikum minggu ini.  
Contoh:  
> Mahasiswa mampu menjelaskan fungsi utama sistem operasi dan peran kernel serta system call.
Sistem operasi berfungsi utama sebagai perantara antara pengguna, aplikasi, dan perangkat keras komputer, mengelola sumber daya seperti proses, memori, file, dan input/output untuk memastikan operasi yang efisien dan aman. 

## Fungsi Utama Sistem Operasi
Sistem operasi menyediakan abstraksi hardware agar aplikasi dapat berjalan tanpa detail teknis perangkat, termasuk manajemen multiprogramming dan proteksi data. Ia juga menangani komunikasi antarproses dan optimalisasi performa melalui penjadwalan tugas.

## Peran Kernel
Kernel beroperasi di mode privileged untuk mencegah konflik sumber daya, memproses permintaan dari aplikasi dengan cara menerima instruksi, mengalokasikan resource, dan mengembalikan hasil. Fungsi kuncinya mencakup pengelolaan CPU time, memori virtual, dan driver perangkat.

## Sistem Call
System call adalah mekanisme yang memungkinkan program user-mode meminta layanan kernel melalui interface standar seperti API, dengan beralih ke mode kernel sementara. Jenis utamanya meliputi pengendalian proses (create/terminate), manajemen file (open/read), komunikasi, dan informasi sistem. Prosesnya melibatkan interrupt, eksekusi di kernel, lalu return ke user mode untuk kesederhanaan dan keamanan.
---

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan:
1️⃣ Peran Sistem Operasi dalam Arsitektur Komputer

Sistem operasi (Operating System/OS) berperan sebagai perantara (interface) antara pengguna/aplikasi dengan perangkat keras komputer. OS mengatur dan mengelola seluruh sumber daya sistem seperti CPU, memori, penyimpanan, dan perangkat input/output agar dapat digunakan secara efisien, aman, dan terkoordinasi. Tanpa sistem operasi, pengguna tidak dapat menjalankan aplikasi maupun berinteraksi langsung dengan hardware.


---

2️⃣ Komponen Utama Sistem Operasi

Komponen utama sistem operasi meliputi:

Kernel: Inti OS yang mengatur manajemen proses, memori, dan komunikasi dengan hardware.

System Call: Mekanisme yang memungkinkan aplikasi (user mode) meminta layanan dari kernel (kernel mode).

Device Driver: Program khusus yang memungkinkan OS berkomunikasi dengan perangkat keras seperti printer, keyboard, dan disk.

File System: Sistem yang mengatur penyimpanan, pengelolaan, dan akses data pada media penyimpanan.



---

3️⃣ Perbandingan Model Arsitektur OS

Monolithic Kernel: Semua layanan OS berjalan di kernel space. Performa tinggi tetapi risiko crash besar (contoh: Linux).

Layered Architecture: OS dibagi menjadi beberapa lapisan dengan fungsi berbeda. Mudah dipahami namun kurang fleksibel.

Microkernel: Kernel dibuat minimal, layanan lain berjalan di user space. Lebih aman dan stabil tetapi performanya lebih rendah (contoh: MINIX).



---


--

---

## Langkah Praktikum
1. Langkah-langkah yang dilakukan.  
2. Perintah yang dijalankan.  
3. File dan kode yang dibuat.  
4. Commit message yang digunakan.

1. Langkah yang digunakan

Pada tahap awal, dilakukan persiapan lingkungan kerja dengan memastikan sistem operasi Linux telah terpasang. Praktikum ini menggunakan Linux Ubuntu (WSL). Selain itu, Git dikonfigurasi agar proses version control dapat berjalan dengan baik menggunakan perintah berikut:

git config --global user.name "Nama Anda"
git config --global user.email "email@contoh.com"

Konfigurasi ini diperlukan agar setiap perubahan yang dikirim ke repository memiliki identitas yang jelas.


---

2. Diskusi Konsep

Tahap ini dilakukan dengan mempelajari materi pengantar mengenai komponen utama sistem operasi, meliputi:

User/Application

System Call

Kernel

Hardware


Selanjutnya dilakukan identifikasi komponen sistem operasi pada beberapa platform:

Linux: Kernel Linux, GNU utilities, system call POSIX

Windows: NT Kernel, Win32 API

Android: Linux Kernel, Android Runtime (ART)



---

3. Eksperimen Dasar

Eksperimen dilakukan dengan menjalankan beberapa perintah dasar Linux pada terminal untuk memahami sistem dan kernel yang digunakan.

Perintah yang dijalankan:

uname -a
whoami
lsmod | head
dmesg | head

Analisis hasil:

uname -a menampilkan informasi kernel, arsitektur sistem, dan versi OS.

whoami menunjukkan user yang sedang aktif.

lsmod | head menampilkan beberapa modul kernel yang sedang dimuat, seperti driver perangkat keras.

dmesg | head menampilkan pesan log kernel saat proses booting.


Hasil ini menunjukkan bahwa Linux menggunakan kernel monolithic modular, di mana modul dapat dimuat dan dilepas secara dinamis.


---

4. Membuat Diagram Arsitektur

Diagram arsitektur sistem operasi dibuat untuk menunjukkan hubungan antara: User → System Call → Kernel → Hardware

Diagram dibuat menggunakan draw.io / Mermaid, kemudian disimpan dengan format PNG pada direktori berikut:

praktikum/week1-intro-arsitektur-os/screenshots/diagram-os.png

Diagram ini membantu memahami alur interaksi aplikasi dengan perangkat keras melalui kernel.


---

5. Penulisan Laporan

Hasil pengamatan, analisis eksperimen, dan kesimpulan dituliskan ke dalam file:

praktikum/week1-intro-arsitektur-os/laporan.md

Selain itu, screenshot hasil eksekusi terminal disimpan ke dalam folder:

screenshots/


---

6. Commit dan Push

Tahap terakhir adalah menyimpan dan mengirim seluruh perubahan ke repository Git dengan perintah:

git add .
git commit -m "Minggu 1 - Arsitektur Sistem Operasi dan Kernel"
git push origin main
*


---
---

## Kode / Perintah

Tuliskan potongan kode atau perintah utama
uname -a
whoami
lsmod | head
dmesg | head

```

## Perintah-perintah Utama

**1. `uname -a`**
Menampilkan informasi lengkap tentang sistem operasi dan kernel:
```bash
uname -a
# Output: Linux hostname 5.15.0-58-generic #64-Ubuntu SMP x86_64 GNU/Linux
```

**2. `whoami`**
Menampilkan username dari user yang sedang login:
```bash
whoami
# Output: username_anda
```

**3. `lsmod | head`**
Menampilkan 10 modul kernel yang sedang dimuat:
```bash
lsmod | head
# Menampilkan daftar modul kernel dengan kolom: Module, Size, Used by
```

**4. `dmesg | head`**
Menampilkan 10 baris pertama dari log kernel/boot messages:
```bash
dmesg | head
# Menampilkan pesan-pesan awal dari kernel saat boot.

---

## Hasil Eksekusi
Sertakan screenshot hasil percobaan atau diagram:
![Screenshot hasil](screenshots/example.png)8*-
![WhatsApp Image 2026-01-01 at 11 34 31](https://github.com/user-attachments/assets/b1ac0d06-fdc0-47d2-9879-63efcedfc515)

---

## Analisis
- Jelaskan makna hasil percobaan.  
- Hubungkan hasil dengan teori (fungsi kernel, system call, arsitektur OS).  
- Apa perbedaan hasil di lingkungan OS berbeda (Linux vs Windows)?  

---


--

1. Makna Hasil Percobaan

Hasil percobaan menunjukkan bahwa sistem operasi Linux menyediakan informasi sistem dan kernel secara langsung melalui perintah terminal. Perintah uname -a menampilkan detail kernel yang sedang berjalan, menandakan bahwa kernel berperan sebagai inti pengelola sistem. Perintah whoami menunjukkan identitas user aktif, yang menegaskan adanya pemisahan hak akses antara pengguna dan sistem. Perintah lsmod | head menampilkan modul kernel yang sedang dimuat, yang membuktikan bahwa Linux menggunakan kernel monolithic modular. Sementara itu, dmesg | head menampilkan pesan log kernel saat proses booting, yang menunjukkan bagaimana kernel berinteraksi dengan hardware sejak sistem mulai berjalan.


---

2. Keterkaitan Hasil dengan Teori

Hasil percobaan sesuai dengan teori fungsi kernel, yaitu mengelola sumber daya sistem seperti proses, memori, dan perangkat keras. Informasi yang ditampilkan oleh perintah terminal diperoleh melalui system call, yang memungkinkan aplikasi di user mode berkomunikasi dengan kernel di kernel mode secara aman. Arsitektur OS Linux yang bersifat monolithic modular terlihat dari adanya modul kernel (device driver) yang dapat dimuat secara dinamis. Hal ini menunjukkan implementasi nyata konsep arsitektur OS yang telah dipelajari secara teori.


---

3. Perbedaan Hasil pada Lingkungan OS Berbeda (Linux vs Windows)

Pada lingkungan Linux, pengguna dapat mengakses informasi kernel, modul, dan log sistem secara terbuka melalui terminal. Hal ini memudahkan analisis dan pembelajaran sistem operasi. Sebaliknya, pada Windows, informasi kernel tidak dapat diakses secara langsung melalui perintah yang setara seperti lsmod atau dmesg. Windows menggunakan pendekatan kernel hybrid dan akses ke informasi sistem lebih banyak dilakukan melalui GUI atau tool khusus seperti Task Manager dan Event Viewer. Perbedaan ini menunjukkan bahwa Linux lebih terbuka dan fleksibel untuk eksperimen sistem operasi, sedangkan Windows lebih fokus pada kemudahan penggunaan dan keamanan pengguna umum.


---69

## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
Berikut kesimpulan praktikum Arsitektur Sistem Operasi dan Kernel yang ringkas, formal, dan siap ditempel ke laporan


Berdasarkan praktikum yang telah dilakukan, dapat disimpulkan bahwa sistem operasi memiliki peran penting sebagai penghubung antara pengguna, aplikasi, dan perangkat keras komputer. Sistem operasi mengelola sumber daya sistem seperti CPU, memori, perangkat input/output, serta penyimpanan agar dapat digunakan secara efisien dan aman.

Melalui pengamatan langsung menggunakan perintah terminal Linux, dapat dipahami bahwa kernel merupakan inti dari sistem operasi yang bertanggung jawab terhadap manajemen proses, memori, serta komunikasi dengan perangkat keras melalui device driver. Hasil eksperimen menunjukkan bahwa Linux menggunakan arsitektur kernel monolithic modular, di mana modul kernel dapat dimuat dan dilepas sesuai kebutuhan sistem.

Selain itu, perbandingan model arsitektur sistem operasi seperti monolithic, microkernel, dan layered architecture memberikan pemahaman bahwa setiap model memiliki kelebihan dan kekurangan masing-masing. Untuk sistem modern, pendekatan monolithic modular atau hybrid dinilai paling relevan karena mampu menyeimbangkan antara performa, stabilitas, dan fleksibilitas.

Dengan adanya diagram arsitektur sistem operasi, alur interaksi antara user, system call, kernel, dan hardware dapat dipahami secara lebih jelas. Praktikum ini berhasil meningkatkan pemahaman mengenai konsep dasar arsitektur sistem operasi dan peran kernel dalam mendukung kinerja sistem komputer secara keseluruhan.

---

## Quiz
1. [sebutkan tiga fungsi utama sistem operasi]  
   **Jawaban: 1.Manajemen proses(process management)
              2.Manajemen memori(memory manajement)
              3.Manajemen perangkat dan sistem berkas

2. [jelaskan perbedaan antara kernel mode dan user mode]  
   **Jawaban: Kernel mode :memiliki akses penuh ke hardware dan sumber daya sisem
sedangkan,User mode :akses terbatas,tidak biisa langsung mengakses hardware,harus melalui sistem call. 
3. [sebutkan contoh OS dengan arsitektur monolithic dan microkernel]  
   **Jawaban: Monolithic:LINUK
              Microkernel:MINIX

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?
Kemalasan 
- Bagaimana cara Anda mengatasinya?
tidak boleh malas karna ingin mendapat wishlist impian.


---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
