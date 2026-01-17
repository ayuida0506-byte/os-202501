
# Laporan Praktikum Minggu [12]
Topik: [Virtualisasi Mengggunakan Virtual Machine]

---

## Identitas
- **Nama**  : [Ayu ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSRA]

---

## Tujuan
Tuliskan tujuan praktikum minggu ini.  
1.Menginstal perangkat lunak virtualisasi (VirtualBox/VMware).
2.Membuat dan menjalankan sistem operasi guest di dalam VM.
3.Mengatur konfigurasi resource VM (CPU, RAM, storage).
4.Menjelaskan mekanisme proteksi OS melalui virtualisasi.
5.Menyusun laporan praktikum instalasi dan konfigurasi VM secara sistematis.
---

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.

---

## 📚 **RINGKASAN TEORI VIRTUALISASI MENGGUNAKAN VIRTUAL MACHINE**

### **1. Konsep Dasar Virtualisasi**
Virtualisasi adalah teknologi yang memungkinkan **abstraksi hardware fisik** menjadi multiple virtual environments yang terisolasi. Sebuah komputer fisik (host) dapat menjalankan beberapa sistem operasi (guest) secara bersamaan melalui layer software yang disebut **hypervisor** atau Virtual Machine Monitor (VMM). Virtualisasi memisahkan resource fisik (CPU, memori, storage, network) dari logical resource, sehingga satu mesin fisik dapat digunakan seolah-olah menjadi banyak mesin independen. Teknologi ini meningkatkan **efisiensi utilisasi hardware**, mengurangi biaya infrastruktur, dan memberikan fleksibilitas dalam deployment dan management sistem.

### **2. Arsitektur dan Komponen Virtual Machine**
Virtual Machine (VM) terdiri dari beberapa komponen utama: **Virtual CPU (vCPU)** yang mengemulasi prosesor fisik, **Virtual Memory** yang dialokasikan dari RAM host, **Virtual Disk** sebagai storage yang biasanya berupa file image (VDI, VMDK, VHD), dan **Virtual Network Interface** untuk konektivitas jaringan. Arsitektur VM dibagi menjadi dua tipe hypervisor: **Type 1 (Bare-metal)** yang berjalan langsung di atas hardware tanpa OS host (contoh: VMware ESXi, Hyper-V, KVM), dan **Type 2 (Hosted)** yang berjalan di atas sistem operasi host (contoh: VirtualBox, VMware Workstation). Type 1 memberikan performa lebih baik dan digunakan untuk production environment, sedangkan Type 2 lebih mudah digunakan untuk development dan testing.

### **3. Teknik Virtualisasi: Full Virtualization, Paravirtualization, dan Hardware-Assisted**
**Full Virtualization** menggunakan binary translation untuk menjalankan guest OS tanpa modifikasi, hypervisor menerjemahkan instruksi privileged menjadi safe instructions, memberikan isolasi penuh tetapi dengan overhead performa. **Paravirtualization** memodifikasi guest OS untuk aware dengan virtualisasi, guest OS melakukan hypercall langsung ke hypervisor tanpa translation, menghasilkan performa lebih baik tetapi memerlukan modifikasi kernel (contoh: Xen). **Hardware-Assisted Virtualization** menggunakan ekstensi CPU seperti Intel VT-x dan AMD-V yang menyediakan instruksi khusus untuk virtualisasi, menggabungkan kemudahan full virtualization dengan performa mendekati paravirtualization, dan menjadi standar modern untuk virtualisasi.

### **4. Resource Management dan Isolation**
Hypervisor bertanggung jawab untuk **mengalokasikan dan mengelola resource** secara adil dan efisien di antara multiple VMs. **CPU Scheduling** menggunakan algoritma seperti proportional share atau credit-based untuk membagi CPU time. **Memory Management** menerapkan teknik overcommitment (alokasi memori lebih dari fisik tersedia), memory ballooning (reclaim unused memory dari VM), dan page sharing untuk deduplikasi. **Storage Management** menggunakan thin provisioning untuk efisiensi space dan snapshot untuk backup point-in-time. **Network Virtualization** membuat virtual switches dan VLANs untuk isolasi traffic. Isolation memastikan bahwa crash atau malware di satu VM tidak mempengaruhi VM lain, memberikan security boundary yang kuat.

### **5. Keuntungan, Tantangan, dan Use Cases Virtualisasi**
**Keuntungan utama** meliputi: **consolidation** (mengurangi jumlah server fisik hingga 80%), **cost efficiency** (hemat hardware, listrik, cooling), **flexibility** (mudah provisioning, cloning, migration), **disaster recovery** (snapshot dan backup VM mudah), dan **testing environment** yang terisolasi. **Tantangan** yang dihadapi: **performance overhead** (5-15% dibanding bare-metal), **resource contention** saat oversubscription, **I/O bottleneck** terutama pada storage dan network, **licensing complexity**, dan **security concerns** jika hypervisor dikompromikan. **Use Cases populer**: server consolidation di data center, development dan testing environments, cloud computing infrastructure (IaaS), desktop virtualization (VDI), sandboxing untuk analisis malware, dan running legacy applications di modern infrastructure.

---

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

---

## Isolasi VM antara Host dan Guest

Virtual Machine (VM) menyediakan isolasi dengan cara **memisahkan lingkungan eksekusi guest OS dari host OS** melalui **hypervisor**. Guest OS tidak memiliki akses langsung ke hardware fisik, melainkan melalui lapisan virtualisasi. Semua instruksi hardware (CPU, memori, I/O) dimediasi oleh hypervisor sehingga aktivitas guest tidak dapat memengaruhi host secara langsung.

Isolasi ini memastikan bahwa:

* Crash atau error pada guest OS **tidak merusak host OS**
* Proses di dalam VM **tidak dapat mengakses memori host**
* VM satu dengan lainnya **tidak saling mengganggu**

---

## Keterkaitan dengan Konsep Sandboxing

**Sandboxing** adalah teknik menjalankan aplikasi atau sistem dalam lingkungan terbatas dan terkontrol. VM berfungsi sebagai **sandbox tingkat sistem operasi**, karena:

* Aplikasi berisiko dijalankan di guest OS, bukan di host
* Dampak serangan atau malware hanya terjadi di dalam VM
* Lingkungan dapat di-reset menggunakan snapshot

Dengan demikian, VM menyediakan sandbox yang **lebih kuat** dibanding sandbox berbasis aplikasi.

---

## Keterkaitan dengan Hardening OS

**Hardening OS** bertujuan memperkecil permukaan serangan sistem. Virtualisasi mendukung hardening dengan:

* Membatasi akses guest OS ke hardware fisik
* Mengontrol resource (CPU, RAM, disk) melalui hypervisor
* Memisahkan layanan penting ke VM yang berbeda
* Memudahkan rollback saat terjadi kompromi sistem

Isolasi VM membantu menerapkan prinsip **least privilege** pada level sistem operasi.

---

## Kesimpulan

Virtual Machine menyediakan isolasi kuat antara host dan guest melalui hypervisor. Isolasi ini menjadikan VM sebagai sandbox yang aman serta mendukung hardening OS dengan pembatasan akses, kontrol resource, dan pemisahan layanan, sehingga keamanan sistem secara keseluruhan meningkat.

## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
1.Efisiensi dan Isolasi Sumber Daya: Virtualisasi memungkinkan satu perangkat keras fisik untuk menjalankan beberapa sistem operasi (Guest OS) secara bersamaan melalui bantuan Hypervisor. Hal ini meningkatkan efisiensi penggunaan CPU, RAM, dan penyimpanan, serta memberikan isolasi di mana kegagalan pada satu VM tidak akan mempengaruhi sistem utama (Host) atau VM lainnya.
2.Fleksibilitas dalam Pengujian Sistem: Praktikum ini menunjukkan bahwa VM sangat efektif untuk melakukan simulasi instalasi jaringan, pengujian perangkat lunak, atau penggunaan OS yang berbeda tanpa harus melakukan partisi ulang atau merusak sistem operasi asli. Fitur seperti Snapshot mempermudah proses pemulihan sistem ke kondisi sebelumnya jika terjadi kesalahan konfigurasi.
3.Abstraksi Perangkat Keras: Virtualisasi membuktikan bahwa sistem operasi dapat berjalan di atas perangkat keras virtual yang diabstraksikan. Hal ini memudahkan portabilitas, di mana sebuah Virtual Machine yang sudah dikonfigurasi dapat dipindahkan atau disalin ke komputer lain dengan spesifikasi berbeda selama memiliki perangkat lunak Hypervisor yang kompatibel.


---

## Quiz
1. [Apa perbedaan antar host dan guest OS?]  
   **Jawaban:**  🔹 Host OS

Sistem operasi utama yang terpasang langsung pada hardware
Menyediakan resource fisik (CPU, RAM, storage)
Menjalankan software virtualisasi (VirtualBox/VMware)
Contoh:
Windows 10 sebagai host OS yang menjalankan VirtualBox

🔹 Guest OS

Sistem operasi yang berjalan di dalam virtual machine
Menggunakan resource virtual dari host OS
Terisolasi dari sistem utama
Contoh:
Ubuntu Linux yang berjalan di VirtualBox

2. [Apa peran hypervisor dalam virtualisasi?]  
   **Jawaban:**  🔹 Hypervisor

Adalah perangkat lunak yang berfungsi sebagai pengelola virtual machine.
🔧 Peran utama hypervisor:
Membagi resource hardware
CPU, RAM, storage ke setiap VM
Mengatur isolasi
Mencegah VM saling mengganggu
Menjadi perantara
Antara hardware fisik dan guest OS
Mengontrol eksekusi VM
Start, stop, snapshot, monitoring

3. [Mengapa virtualisasi meningkatkan keamanan sistem?]  
   **Jawaban:**  
🔒 Isolasi Sistem

Guest OS terpisah dari host
Serangan di VM tidak langsung menyebar

🔒 Sandboxing

Aplikasi berbahaya dijalankan di VM
Aman untuk testing

🔒 Kontrol Resource

Pembatasan CPU, RAM, storage
Mencegah eksploitasi resource

🔒 Snapshot & Recovery

Bisa kembali ke kondisi aman
Mengurangi risiko kerusakan permanen

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
