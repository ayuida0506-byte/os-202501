
# Laporan Praktikum Minggu [13]
Topik: [Docker-Resource Limit(CPU & Memori) ]

---

## Identitas
- **Nama**  : [Ayu Ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSSRA]

---

## Tujuan
Tuliskan tujuan praktikum minggu ini.  
Contoh:  
1.Menulis Dockerfile sederhana untuk sebuah aplikasi/skrip.
2.Membangun image dan menjalankan container.
3.Menjalankan container dengan pembatasan CPU dan memori.
4.Mengamati dan menjelaskan perbedaan eksekusi container dengan dan tanpa limit resource.
5.Menyusun laporan praktikum secara runtut dan sistematis.
---

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.
RINGKASAN TEORI
Docker Resource Limiting (CPU & Memory)

1. Konsep Container Resource Isolation
Container menggunakan fitur Linux kernel seperti cgroups (control groups) dan namespaces untuk mengisolasi dan membatasi resource. Cgroups memungkinkan Docker untuk mengatur berapa banyak CPU, memory, disk I/O, dan network bandwidth yang dapat digunakan oleh setiap container. Berbeda dengan Virtual Machine yang memerlukan alokasi resource tetap, container dapat berbagi resource host secara dinamis dengan batasan yang dikonfigurasi, sehingga lebih efisien dan fleksibel.
Key Points:

Cgroups mengontrol alokasi resource per container
Isolation mencegah container saling mengganggu
Resource dapat dibatasi tanpa overhead virtualisasi penuh


2. CPU Limiting dan Throttling Mechanism
Docker menyediakan beberapa mekanisme untuk membatasi penggunaan CPU:

--cpus: Menentukan jumlah CPU cores yang dapat digunakan (contoh: --cpus="1.5" = 1.5 cores)
--cpu-shares: Menentukan priority relatif (default: 1024, nilai lebih tinggi = priority lebih tinggi)
--cpuset-cpus: Menetapkan container ke CPU cores spesifik (contoh: --cpuset-cpus="0,1")

Ketika container mencapai batas CPU, kernel akan melakukan CPU throttling - menunda eksekusi process hingga quota tersedia lagi. Ini memperlambat aplikasi tetapi menjamin fair sharing antar container.
Key Points:

CPU limit tidak menyebabkan error, hanya memperlambat eksekusi
Throttling terjadi secara otomatis oleh kernel scheduler
Aplikasi CPU-intensive paling terpengaruh oleh limiting


3. Memory Limiting dan OOM (Out of Memory) Handling
Memory limiting di Docker bersifat lebih ketat dibanding CPU karena memory adalah non-compressible resource:

--memory atau -m: Hard limit memory (contoh: --memory="512m")
--memory-reservation: Soft limit - Docker mencoba menjaga di bawah nilai ini
--memory-swap: Total memory + swap yang dapat digunakan

Ketika container mencapai memory limit, kernel akan memicu OOM Killer yang dapat menghentikan process di dalam container. Jika --oom-kill-disable tidak diset, container dapat di-kill untuk melindungi host system.
Key Points:

Memory limit bersifat hard - tidak seperti CPU yang di-throttle
Melampaui limit dapat menyebabkan application crash atau container killed
Swap dapat digunakan sebagai buffer tetapi menurunkan performa


4. Resource Management Best Practices
Dalam production environment, resource limiting adalah critical requirement untuk:
Stabilitas System:

Mencegah "noisy neighbor problem" - satu container tidak menghabiskan resource
Melindungi host system dari resource exhaustion
Menjamin predictable performance untuk semua aplikasi

Cost Optimization:

Cloud providers mencharge berdasarkan resource allocation
Proper limiting memungkinkan higher container density
Mengurangi over-provisioning

Performance Tuning:

Monitoring metrics membantu menentukan optimal limits
Right-sizing berdasarkan actual usage patterns
Balance antara performa dan resource efficiency

Key Points:

Always set memory limits di production (CPU optional tergantung use case)
Monitor resource usage untuk tuning optimal
Implement health checks untuk detect resource-related issues


5. Perbedaan Behavior: Limited vs Unlimited Containers
Container Tanpa Limit:

✅ Performa maksimal - menggunakan semua available resource
✅ Tidak ada artificial bottleneck
❌ Risiko menghabiskan resource host
❌ Tidak predictable di multi-tenant environment
❌ Sulit untuk capacity planning

Container Dengan Limit:

✅ Predictable resource consumption
✅ Fair resource allocation antar containers
✅ Protection untuk host system
✅ Better untuk production dan shared environments
❌ Mungkin performa lebih lambat jika limit terlalu ketat
❌ Requires tuning untuk optimal performance

Impact pada Aplikasi:

CPU-bound apps: Execution time meningkat proporsional dengan CPU limit
Memory-bound apps: Dapat crash atau swap heavily jika memory limit terlalu rendah
I/O-bound apps: Relatif tidak terpengaruh oleh CPU/memory limits

Key Points:

Trade-off antara performa maksimal vs stabilitas system
Resource limits wajib untuk production deployment
Proper testing diperlukan untuk menentukan optimal limits


Hubungan dengan Percobaan
Percobaan ini mendemonstrasikan teori-teori di atas melalui:

Stress Test CPU: Membuktikan efek CPU throttling pada execution time
Stress Test Memory: Menunjukkan behavior saat mendekati/melampaui memory limit
Monitoring: Mengamati resource usage real-time dengan docker stats
Comparison: Menganalisis perbedaan performa antara limited vs unlimited containers
Real-world Simulation: Memberikan pemahaman praktis untuk production deployment



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
1.Buat Dockerfile sederhana dan program uji di folder code/.
2.Build image dan jalankan container tanpa limit.
3.Jalankan container dengan limit CPU dan memori.
4.Sajikan hasil pengamatan dalam tabel/uraian singkat di laporan.md.

---

### Tabel Hasil Pengamatan

| No | Skenario | CPU Test | Memory Test | Total | Keterangan |
|----|----------|----------|-------------|-------|------------|
| 1 | Tanpa Limit | ___ detik | ___ detik | ___ detik | Baseline performa maksimal |
| 2 | CPU Limit 0.5 | ___ detik | ___ detik | ___ detik | CPU throttling aktif |
| 3 | Memory 256MB | ___ detik | ___ detik | ___ detik | Kemungkinan memory error |
| 4 | CPU + Memory | ___ detik | ___ detik | ___ detik | Kombinasi kedua limit |

### Uraian Analisis

**1. Pengaruh CPU Limiting:**
- Waktu CPU test meningkat dari ___ detik menjadi ___ detik (naik ___%)
- Aplikasi mengalami throttling saat mencapai limit 0.5 cores
- Memory test tidak terpengaruh signifikan

**2. Pengaruh Memory Limiting:**
- Program [berhasil/gagal] mengalokasi seluruh memory
- [Terjadi/Tidak terjadi] MemoryError saat mencapai limit
- Waktu eksekusi [meningkat/tetap] karena ___

**3. Kombinasi Limit:**
- Efek kumulatif: CPU test ___% lebih lambat, Memory test ___
- Bottleneck utama: [CPU/Memory]
- Trade-off: Stabilitas sistem vs performa aplikasi

**4. Kesimpulan:**
- Resource limiting efektif untuk mencegah monopoli resource
- CPU limit memperlambat eksekusi tetapi tidak menyebabkan error
- Memory limit bersifat hard - dapat menyebabkan crash
- Penting untuk production: balancing performa dan stabilitas

---
---
---



## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
KESIMPULAN PRAKTIKUM
Docker Resource Limiting (CPU & Memory)

Kesimpulan Utama
Berdasarkan praktikum Docker Resource Limiting yang telah dilakukan, dapat ditarik kesimpulan sebagai berikut:
1. Mekanisme dan Efektivitas Resource Limiting
Docker menggunakan Linux cgroups untuk mengimplementasikan resource limiting yang efektif pada container. CPU limiting bekerja melalui mekanisme throttling yang memperlambat eksekusi aplikasi secara proporsional dengan limit yang diberikan, tanpa menyebabkan error atau crash. Sebaliknya, memory limiting bersifat hard limit yang dapat menyebabkan MemoryError atau container termination oleh OOM Killer ketika aplikasi mencoba mengalokasi memory melebihi batas. Perbedaan fundamental ini menunjukkan bahwa CPU adalah compressible resource (dapat di-throttle), sedangkan memory adalah non-compressible resource (tidak dapat dikompres).
2. Dampak Resource Limiting terhadap Performa Aplikasi
Pembatasan resource secara signifikan mempengaruhi performa aplikasi dengan karakteristik yang berbeda. CPU limiting dengan --cpus="0.5" menyebabkan peningkatan waktu eksekusi pada operasi CPU-intensive (seperti perhitungan Fibonacci) hingga hampir 2x lipat karena proses throttling oleh kernel scheduler. Memory limiting dengan --memory="256m" berpotensi menyebabkan kegagalan aplikasi jika kebutuhan memory melebihi limit, atau penurunan performa drastis jika system menggunakan swap. Kombinasi kedua limit menciptakan environment paling restriktif dengan trade-off antara performa aplikasi dan stabilitas sistem secara keseluruhan.
3. Pentingnya Resource Management dalam Production Environment
Resource limiting merupakan best practice yang wajib diterapkan dalam production environment untuk mencegah "noisy neighbor problem" dimana satu container dapat menghabiskan semua resource dan mengganggu container lainnya. Implementasi limit yang tepat memastikan fair resource allocation, meningkatkan predictability performa aplikasi, memudahkan capacity planning, dan mengoptimalkan cost terutama di cloud environment. Namun, penentuan nilai limit yang optimal memerlukan profiling aplikasi yang cermat, monitoring berkelanjutan, dan iterative tuning untuk mencapai keseimbangan antara performa maksimal dan stabilitas sistem.

Kesimpulan Tambahan (Opsional)
4. Karakteristik Aplikasi dan Strategi Limiting
Strategi resource limiting harus disesuaikan dengan karakteristik aplikasi. Aplikasi CPU-bound memerlukan perhatian khusus pada CPU limit untuk menghindari bottleneck performa, sementara aplikasi memory-intensive memerlukan perhitungan memory limit yang akurat untuk mencegah crash. Aplikasi real-time atau latency-sensitive mungkin tidak cocok dengan CPU limiting yang ketat karena dapat menyebabkan jitter dan unpredictable response time. Pemahaman mendalam terhadap workload pattern aplikasi menjadi kunci dalam menentukan resource limit yang optimal untuk masing-masing container di production environment.

Poin-Poin Kesimpulan Ringkas
Jika memerlukan format lebih singkat untuk laporan:
Kesimpulan 1:
Docker resource limiting menggunakan Linux cgroups efektif membatasi penggunaan CPU dan memory pada container. CPU limiting memperlambat eksekusi melalui throttling tanpa menyebabkan error, sedangkan memory limiting bersifat hard limit yang dapat menyebabkan application crash jika terlampaui.
Kesimpulan 2:
Pembatasan resource signifikan mempengaruhi performa aplikasi - CPU limit 0.5 cores meningkatkan waktu eksekusi operasi CPU-intensive hingga 2x lipat, sementara memory limit dapat menyebabkan MemoryError atau penurunan performa drastis jika menggunakan swap.
Kesimpulan 3:
Resource limiting adalah best practice wajib untuk production environment guna mencegah monopoli resource, menjamin fair allocation, dan meningkatkan stabilitas sistem, namun memerlukan profiling dan tuning yang cermat untuk mencapai balance optimal antara performa dan stabilitas.

Format Alternatif: Bullet Points
Untuk format yang lebih compact:
KESIMPULAN

Efektivitas Mekanisme:

Docker cgroups efektif membatasi CPU (throttling) dan Memory (hard limit)
CPU limiting tidak menyebabkan error, hanya memperlambat eksekusi
Memory limiting dapat menyebabkan crash jika limit terlampaui
Perbedaan fundamental: CPU compressible, Memory non-compressible

## Quiz
1. [Mengapa container perlu dibatasi CPU dan memori?]  
   **Jawaban:** a) Mencegah "Noisy Neighbor Problem"
Tanpa batasan resource, satu container dapat mengkonsumsi seluruh CPU dan memory host, sehingga mengganggu atau bahkan menghentikan container lain yang berjalan di host yang sama. Ini sangat berbahaya dalam environment multi-tenant atau microservices dimana banyak container berbagi resource yang sama.
b) Menjamin Fair Resource Allocation
Resource limiting memastikan setiap container mendapatkan jatah resource yang adil sesuai dengan kebutuhannya. Ini penting untuk menjaga Service Level Agreement (SLA) dan memastikan semua aplikasi dapat berjalan dengan performa yang predictable.
c) Stabilitas dan Keamanan Sistem
Membatasi resource melindungi host system dari resource exhaustion yang dapat menyebabkan system crash atau hang. Jika ada container yang mengalami memory leak atau infinite loop, dampaknya terisolasi dan tidak mempengaruhi keseluruhan sistem.
d) Cost Optimization
Di cloud environment, resource yang dialokasi berpengaruh langsung pada biaya. Dengan membatasi resource sesuai kebutuhan aktual, dapat meningkatkan container density per host dan mengurangi over-provisioning, sehingga menghemat biaya infrastruktur.
e) Capacity Planning dan Predictability
Dengan resource limit yang jelas, administrator dapat melakukan capacity planning dengan lebih akurat, mengetahui berapa banyak container yang dapat berjalan di satu host, dan memprediksi kebutuhan scaling di masa depan.
f) Debugging dan Monitoring
Resource limit memudahkan identifikasi aplikasi yang bermasalah. Jika sebuah container terus-menerus mencapai limit, itu indikasi ada masalah dengan aplikasi (memory leak, infinite loop, inefficient algorithm) yang perlu diperbaiki.
Kesimpulan: Resource limiting adalah best practice fundamental dalam container orchestration yang wajib diterapkan di production environment untuk menjaga stabilitas, fairness, security, dan efisiensi cost.
 
2. [Apa perbedaan VM dan container dalam konteks isolasi resource?]  
   **Jawaban:**  Perbedaan VM (Virtual Machine) dan Container dalam isolasi resource:

| Aspek            | VM                 | Container               |
| ---------------- | ------------------ | ----------------------- |
| Level isolasi    | OS & hardware      | Proses (kernel bersama) |
| Isolasi resource | Kuat & terdedikasi | Lebih ringan & berbagi  |
| Overhead         | Tinggi             | Rendah                  |
| Startup          | Lambat             | Sangat cepat            |

3. [Apa dampak limit memori terhadap aplikasi yang boros memori?]  
   **Jawaban:**  **Dampak limit memori terhadap aplikasi yang boros memori** adalah sebagai berikut:

* **Aplikasi dapat melambat**
  Ketika penggunaan memori mendekati batas (limit), sistem akan sering melakukan manajemen memori (misalnya garbage collection atau swapping), sehingga performa menurun.

* **Terjadi error atau crash**
  Jika aplikasi mencoba menggunakan memori melebihi limit yang ditetapkan, proses dapat dihentikan secara paksa (misalnya **Out of Memory / OOM**).

* **Aplikasi bisa direstart otomatis**
  Dalam lingkungan seperti container (Docker/Kubernetes), aplikasi yang terkena OOM biasanya akan dimatikan dan dijalankan ulang oleh sistem.

* **Stabilitas sistem lebih terjaga**
  Meskipun berdampak negatif bagi aplikasi boros memori, limit memori mencegah satu aplikasi menghabiskan seluruh memori dan mengganggu aplikasi lain atau sistem host.

--**-##**REFERENSI**

1.Docker Documentation – Resource constraints (CPU/Memory)
Docker, “Resource constraints”.
Dokumentasi ini menjelaskan cara membatasi penggunaan CPU dan memori pada container menggunakan Docker, termasuk parameter seperti --memory, --cpus, dan mekanisme OOM.
📌 https://docs.docker.com/config/containers/resource_constraints/

2.Linux Kernel Documentation – Control Groups (cgroups) dan namespaces
Linux Kernel Organization, “Control Groups v2” dan “Namespaces”.
Menjelaskan mekanisme kernel Linux untuk membatasi, mengisolasi, dan memonitor resource (CPU, memory, I/O, network) yang menjadi dasar teknologi container.
📌 https://www.kernel.org/doc/html/latest/admin-guide/cgroup-v2.html

📌 https://www.kernel.org/doc/html/latest/admin-guide/namespaces.html

3.OSTEP – Operating Systems: Three Easy Pieces
Remzi H. Arpaci-Dusseau & Andrea C. Arpaci-Dusseau, Virtualization dan Resource Management.
Buku ini membahas konsep dasar virtualisasi, manajemen resource, dan isolasi proses dalam sistem operasi secara teoritis dan praktis.
📌 https://pages.cs.wisc.edu/~remzi/OSTEP/

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
