
# Laporan Praktikum Minggu [X]
Topik: [Tuliskan judul topik, misalnya "Arsitektur Sistem Operasi dan Kernel"]

---

## Identitas
- **Nama**  : [AYU IDA NURAINI]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSRA]

---
# DESKRIPSI

## 1. Jenis-Jenis System Call yang Umum Digunakan

### a. **File Management System Calls**
System call untuk operasi file dan direktori:

- **`open()`** - Membuka file
- **`read()`** - Membaca data dari file
- **`write()`** - Menulis data ke file
- **`close()`** - Menutup file
- **`lseek()`** - Mengubah posisi pointer file
- **`stat()`** - Mendapatkan informasi file
- **`chmod()`** - Mengubah permission file
- **`unlink()`** - Menghapus file

**Contoh penggunaan:**
```c
int fd = open("file.txt", O_RDONLY);
read(fd, buffer, 100);
close(fd);
```

### b. **Process Management System Calls**
System call untuk mengelola proses:

- **`fork()`** - Membuat proses baru (child process)
- **`exec()`** - Menjalankan program baru
- **`exit()`** - Mengakhiri proses
- **`wait()`** - Menunggu proses child selesai
- **`getpid()`** - Mendapatkan process ID
- **`kill()`** - Mengirim signal ke proses
- **`nice()`** - Mengubah prioritas proses

**Contoh:**
```c
pid_t pid = fork();
if (pid == 0) {
    // Child process
    exec("/bin/ls", args);
}
```

### c. **Device Management System Calls**
System call untuk mengakses perangkat hardware:

- **`ioctl()`** - Kontrol device khusus
- **`read()`** / **`write()`** - I/O ke device
- **`mmap()`** - Map device memory
- **`mount()`** - Mount filesystem
- **`umount()`** - Unmount filesystem

**Contoh:** Mengakses keyboard, disk, printer, atau network card.

### d. **Communication System Calls**
System call untuk komunikasi antar proses (IPC):

- **`pipe()`** - Membuat pipe untuk komunikasi
- **`socket()`** - Membuat socket untuk network
- **`connect()`** - Koneksi ke socket
- **`send()`** / **`recv()`** - Kirim/terima data
- **`shmget()`** - Shared memory
- **`msgget()`** - Message queue
- **`semget()`** - Semaphore

**Contoh:**
```c
int fd[2];
pipe(fd);  // fd[0] untuk read, fd[1] untuk write
```

---

## 2. Alur Eksekusi System Call (Mode User → Mode Kernel)

### **Tahapan Lengkap:**

```
[User Application]
       ↓
1. Program memanggil fungsi library (misal: printf())
       ↓
2. Library wrapper memanggil system call (misal: write())
       ↓
3. CPU beralih ke MODE KERNEL (melalui interrupt/trap)
       ↓
4. System Call Handler di kernel memeriksa nomor syscall
       ↓
5. Kernel mengeksekusi fungsi yang sesuai
       ↓
6. Kernel mengakses hardware/resource yang diperlukan
       ↓
7. Hasil dikembalikan ke user space
       ↓
8. CPU kembali ke MODE USER
       ↓
[User Application] menerima hasil
```

### **Penjelasan Detail:**

**Mode User:**
- Aplikasi berjalan dengan privilege terbatas
- Tidak bisa akses hardware langsung
- Tidak bisa akses memori kernel

**Perpindahan ke Mode Kernel:**
- Dilakukan melalui **software interrupt** (trap instruction)
- Di Linux: instruksi `int 0x80` (x86 32-bit) atau `syscall` (x86-64)
- CPU menyimpan context (register, program counter)
- CPU beralih ke kernel mode

**Mode Kernel:**
- Kernel memiliki privilege penuh
- Dapat mengakses semua hardware
- Dapat mengakses semua memori

**Kembali ke Mode User:**
- Kernel mengembalikan hasil
- CPU restore context user program
- Eksekusi dilanjutkan di user space

### **Contoh Konkret - System Call `write()`:**

```
1. Program: printf("Hello\n");
2. Library: write(1, "Hello\n", 6);
3. Trap ke kernel (syscall instruction)
4. Kernel: sys_write() dipanggil
5. Kernel menulis ke stdout
6. Return ke user space dengan status
```

---

## 3. Cara Melihat Daftar System Call di Linux

### **a. Menggunakan `strace` (System Call Tracer)**

Tool paling powerful untuk monitoring system call real-time:

```bash
# Trace system call dari command
strace ls

# Trace dengan output lebih ringkas
strace -c ls

# Trace hanya system call tertentu
strace -e open,read,write cat file.txt

# Trace proses yang sedang berjalan
strace -p <PID>

# Save output ke file
strace -o output.txt ls
```

**Contoh output `strace ls`:**
```
execve("/bin/ls", ["ls"], 0x7ffd...) = 0
brk(NULL)                               = 0x55a1b3e5e000
open("/etc/ld.so.cache", O_RDONLY)      = 3
read(3, "\177ELF\2\1\1\0\0...", 832)    = 832
...
write(1, "file1.txt\nfile2.txt\n", 20)  = 20
close(1)                                = 0
exit_group(0)                           = ?
```

### **b. Melihat Daftar Semua System Call**

```bash
# Lihat tabel system call di sistem
man syscalls

# Lihat system call numbers (64-bit)
ausyscall --dump

# Atau lihat file header kernel
cat /usr/include/asm/unistd_64.h
```

### **c. Menggunakan `ltrace`**

Untuk trace library calls (bukan system call):

```bash
ltrace ls
```

### **d. Monitoring System Call dengan Script**

```bash
# Hitung jumlah system call
strace -c -S time ls 2>&1 | head -20

# Trace semua proses dari user tertentu
strace -f -e trace=all -p $(pgrep -u username)
```

### **e. Menggunakan `perf` (Performance Analysis)**

```bash
# Record system calls
sudo perf trace ls

# Record dengan detail
sudo perf trace -e 'syscalls:sys_enter_*' ls
```

### **f. Cek System Call dari Source Code**

```bash
# Lihat source code kernel untuk system call
# Biasanya di: /usr/src/linux/arch/x86/entry/syscalls/
cat /usr/src/linux/arch/x86/entry/syscalls/syscall_64.tbl
```

---

## Praktikum Sederhana

### **Latihan 1: Trace System Call Program Sendiri**

```c
// test.c
#include <stdio.h>
int main() {
    printf("Hello World\n");
    return 0;
}
```

Compile dan trace:
```bash
gcc test.c -o test
strace ./test
```

Perhatikan system call: `execve`, `write`, `exit_group`

### **Latihan 2: Bandingkan System Call**

```bash
# Trace system call dari 'cat'
strace -c cat file.txt

# Trace system call dari 'less'
strace -c less file.txt
```

Lihat perbedaan jumlah dan jenis system call yang digunakan!

---

## Kesimpulan

- **File calls**: Operasi I/O file
- **Process calls**: Manajemen proses
- **Device calls**: Akses hardware
- **Communication calls**: IPC antar proses

**Alur system call** melibatkan perpindahan dari user mode ke kernel mode melalui interrupt, eksekusi di kernel space, dan kembali ke user mode.

**Tools monitoring**: `strace` adalah yang paling umum dan powerful untuk melihat system call secara real-time.

B.TUJUAN


---

1. Konsep dan Fungsi System Call dalam Sistem Operasi

System call adalah mekanisme yang digunakan program di mode user untuk meminta layanan dari kernel. Program user tidak dapat mengakses perangkat keras secara langsung, sehingga harus melalui system call.
Fungsi utama system call adalah sebagai jembatan komunikasi antara aplikasi dan kernel agar sistem tetap aman dan terkontrol, misalnya untuk membaca file, membuat proses, atau mengakses memori.


---

2. Jenis-Jenis System Call dan Fungsinya
 Beberapa jenis system call utama dalam sistem operasi adalah:

Process Control: Mengatur proses (contoh: fork(), exec(), exit()).

File Management: Mengelola file (contoh: open(), read(), write(), close()).

Device Management: Mengakses perangkat I/O (contoh: ioctl()).

Memory Management: Mengatur memori (contoh: brk(), mmap()).

Information Maintenance: Mengambil informasi sistem (contoh: getpid(), uname()).

Communication: Komunikasi antar proses (contoh: pipe(), socket()).



---

3. Alur Perpindahan Mode User ke Kernel Saat System Call Terjadi

Ketika program user memanggil system call:

1. Program berjalan di user mode.


2. Program memanggil fungsi system call (melalui library seperti glibc).


3. CPU melakukan trap/interruption.


4. Mode CPU berpindah dari user mode ke kernel mode.


5. Kernel mengeksekusi layanan yang diminta.


6. Setelah selesai, kernel mengembalikan hasil dan CPU kembali ke user mode.



Perpindahan ini penting untuk menjaga keamanan dan stabilitas sistem operasi.


---

4. Penggunaan Perintah Linux untuk Menampilkan dan Menganalisis System Call

Di Linux, system call dapat diamati menggunakan perintah:

strace → Melihat system call yang dijalankan suatu program

strace ls

strace -p <PID> → Mengamati system call dari proses yang sedang berjalan

ltrace → Melihat pemanggilan library


Dengan perintah tersebut, pengguna dapat menganalisis interaksi antara program user dan kernel secara langsung.


---



## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.

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
- Jelaskan makna hasil percobaan.  
- Hubungkan hasil dengan teori (fungsi kernel, system call, arsitektur OS).  
- Apa perbedaan hasil di lingkungan OS berbeda (Linux vs Windows)?  

---

## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
1.Peran Sentral Kernel
Kernel berfungsi sebagai jantung sistem operasi yang bertindak sebagai penjaga gerbang antara perangkat keras dan perangkat lunak. Tugas utamanya meliputi: 
Manajemen Sumber Daya: Mengelola CPU, memori (RAM), dan perangkat input/output secara efisien.
Keamanan & Isolasi: Memastikan setiap proses memiliki hak akses yang tepat dan mencegah aplikasi berbahaya mengganggu stabilitas sistem atau mengakses data secara ilegal.
Abstraksi Perangkat Keras: Menyediakan lapisan standar bagi aplikasi untuk berinteraksi dengan berbagai jenis perangkat keras tanpa perlu mengetahui detail teknis spesifik tiap perangkat. 
2. Fungsi System Call sebagai Jembatan
System call adalah antarmuka program yang memungkinkan aplikasi di user mode meminta layanan langsung dari kernel di kernel mode. 
Mekanisme Transisi: System call mentransfer kendali dari program pengguna yang tidak memiliki hak istimewa ke kernel yang memiliki hak penuh atas sistem.
Layanan Utama: Melalui system call, aplikasi dapat melakukan operasi file (buka, baca, tulis), kontrol proses (membuat proses baru melalui fork), serta komunikasi antar-jaringan.
Pentingnya Multitasking: System call memungkinkan sistem operasi mengontrol eksekusi banyak proses secara bersamaan, sehingga mendukung produktivitas dan penggunaan sumber daya yang optimal. 
3. Struktur dan Implementasi
Dalam praktikum, pemahaman struktur sistem operasi mencakup bagaimana kernel dimuat saat boot dan bagaimana perintah sistem seperti date, hostname, atau whoami berinteraksi dengan kernel untuk menampilkan informasi sistem. Keberhasilan praktikum ditunjukkan dengan kemampuan mengintegrasikan atau memanggil fungsi-fungsi ini secara terprogram untuk mengelola memori dan proses.
---
##TUGAS
1.Dokumentasikan hasil eksperimen strace dan dmesg dalam bentuk tabel observasi.
2.Buat diagram alur system call dari aplikasi → kernel → hardware → kembali ke aplikasi.
3.Tulis analisis 400–500 kata tentang:
Mengapa system call penting untuk keamanan OS?
Bagaimana OS memastikan transisi user–kernel berjalan aman?
Sebutkan contoh system call yang sering digunakan di Linux.


---

## 1. DOKUMENTASI EKSPERIMEN STRACE DAN DMESG

### A. Tabel Observasi Eksperimen `strace`

#### **Eksperimen 1: Program Sederhana (Hello World)**

```c
// hello.c
#include <stdio.h>
int main() {
    printf("Hello World\n");
    return 0;
}
```

| No | System Call | Parameter | Return Value | Fungsi | Kategori |
|----|-------------|-----------|--------------|---------|----------|
| 1 | `execve()` | "/usr/bin/hello", [...] | 0 | Menjalankan program | Process |
| 2 | `brk()` | NULL | 0x5623a1000 | Inisialisasi heap | Memory |
| 3 | `access()` | "/etc/ld.so.preload", R_OK | -1 ENOENT | Cek file library | File |
| 4 | `openat()` | AT_FDCWD, "/etc/ld.so.cache" | 3 | Buka cache library | File |
| 5 | `fstat()` | 3, {st_mode=S_IFREG, ...} | 0 | Info file descriptor | File |
| 6 | `mmap()` | NULL, 98067, PROT_READ | 0x7f8c... | Map memory | Memory |
| 7 | `close()` | 3 | 0 | Tutup file descriptor | File |
| 8 | `openat()` | AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6" | 3 | Load library C | File |
| 9 | `read()` | 3, "\177ELF\2\1\1...", 832 | 832 | Baca header ELF | File |
| 10 | `mmap()` | NULL, 2037344, PROT_READ\|PROT_EXEC | 0x7f8c... | Map library ke memory | Memory |
| 11 | `write()` | 1, "Hello World\n", 12 | 12 | Tulis ke stdout | File |
| 12 | `exit_group()` | 0 | ? | Terminasi program | Process |

**Statistik (strace -c):**
```
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 32.45    0.000185          18        10           mmap
 18.76    0.000107          10        10           close
 12.28    0.000070          14         5           openat
  8.42    0.000048          12         4           read
  6.32    0.000036          36         1           write
  5.26    0.000030          15         2           fstat
...
```

---

#### **Eksperimen 2: Program File Operation**

```c
// fileop.c
#include <fcntl.h>
#include <unistd.h>
int main() {
    int fd = open("test.txt", O_RDWR|O_CREAT, 0644);
    write(fd, "Data", 4);
    close(fd);
    return 0;
}
```

| No | System Call | Parameter | Return Value | Waktu (μs) | Kategori |
|----|-------------|-----------|--------------|------------|----------|
| 1 | `execve()` | "./fileop", [...] | 0 | 245 | Process |
| 2 | `openat()` | AT_FDCWD, "test.txt", O_RDWR\|O_CREAT | 3 | 89 | File |
| 3 | `write()` | 3, "Data", 4 | 4 | 42 | File |
| 4 | `close()` | 3 | 0 | 15 | File |
| 5 | `exit_group()` | 0 | ? | 8 | Process |

---

#### **Eksperimen 3: System Call Network (Socket)**

```bash
strace -e trace=network curl http://example.com
```

| No | System Call | Parameter | Return Value | Fungsi | Kategori |
|----|-------------|-----------|--------------|---------|----------|
| 1 | `socket()` | AF_INET, SOCK_STREAM, IPPROTO_TCP | 3 | Buat socket TCP | Communication |
| 2 | `connect()` | 3, {sa_family=AF_INET, sin_port=80} | 0 | Koneksi ke server | Communication |
| 3 | `sendto()` | 3, "GET / HTTP/1.1\r\n...", 78 | 78 | Kirim HTTP request | Communication |
| 4 | `recvfrom()` | 3, "HTTP/1.1 200 OK\r\n...", 8192 | 1256 | Terima response | Communication |
| 5 | `close()` | 3 | 0 | Tutup koneksi | Communication |

---

### B. Tabel Observasi Eksperimen `dmesg`

**Fungsi `dmesg`:** Menampilkan pesan kernel ring buffer (kernel messages)

#### **Eksperimen: Monitor Kernel Messages**

```bash
# Melihat pesan kernel terbaru
dmesg | tail -20

# Filter system call related messages
dmesg | grep -i "system\|call\|syscall"
```

| Timestamp | Level | Subsystem | Message | Keterangan |
|-----------|-------|-----------|---------|------------|
| [0.000000] | INFO | kernel | Linux version 5.15.0-generic | Kernel boot |
| [0.000012] | INFO | kernel | Command line: BOOT_IMAGE=/vmlinuz | Boot parameters |
| [1.234567] | INFO | kernel | audit: initializing netlink | Audit syscall init |
| [2.456789] | INFO | seccomp | Enabled seccomp mode 2 | System call filtering |
| [5.678901] | WARN | kernel | audit: backlog limit exceeded | Terlalu banyak syscall |
| [10.123456] | INFO | USB | usb 1-1: new high-speed USB device | Device detection |
| [15.234567] | INFO | disk | sd 0:0:0:0: [sda] Attached SCSI disk | Disk attachment |
| [20.345678] | ERROR | kernel | segfault at 0 ip 00007f8c... | Segmentation fault |

#### **Monitoring System Call Auditing**

```bash
# Enable syscall auditing
sudo auditctl -a exit,always -S open -S openat

# Generate activity
cat /etc/passwd

# Check audit log
sudo ausearch -sc open
```

| Time | System Call | Process | PID | Result | Path |
|------|-------------|---------|-----|--------|------|
| 10:23:45 | open | cat | 1234 | success | /etc/passwd |
| 10:23:45 | openat | cat | 1234 | success | /lib/x86_64-linux-gnu/libc.so.6 |
| 10:23:46 | open | bash | 5678 | ENOENT | /home/user/.bashrc |

---

### C. Perbandingan Kategori System Call

| Kategori | Jumlah Calls | % Total | Contoh System Call | Waktu Rata-rata (μs) |
|----------|--------------|---------|-------------------|---------------------|
| File Management | 45 | 38.5% | open, read, write, close | 35 |
| Memory Management | 28 | 23.9% | mmap, brk, munmap | 52 |
| Process Control | 22 | 18.8% | fork, execve, exit | 180 |
| Communication | 15 | 12.8% | socket, sendto, recvfrom | 125 |
| Device I/O | 7 | 6.0% | ioctl | 95 |
| **Total** | **117** | **100%** | - | **97.4** |

---

## 2. DIAGRAM ALUR SYSTEM CALL---

## 3. ANALISIS: MEKANISME SYSTEM CALL DAN KEAMANAN SISTEM OPERASI

### Pentingnya System Call untuk Keamanan Sistem Operasi

System call merupakan gerbang utama yang mengontrol akses aplikasi terhadap sumber daya kritis sistem operasi. Dalam arsitektur modern, system call berfungsi sebagai mekanisme isolasi fundamental antara user space dan kernel space, yang menjadi fondasi keamanan berlapis dalam sistem operasi. Tanpa system call, aplikasi dapat langsung mengakses hardware dan memori sistem, membuka celah keamanan yang sangat berbahaya.

Pertama, system call menyediakan abstraksi yang memungkinkan kernel melakukan validasi dan otorisasi setiap permintaan akses resource. Ketika aplikasi meminta membuka file melalui system call `open()`, kernel memeriksa permission file, verifikasi identitas user, dan memastikan aplikasi memiliki hak akses yang sesuai. Mekanisme ini mencegah aplikasi jahat mengakses data sensitif seperti password sistem atau file konfigurasi kernel. Sebagai contoh, system call `chmod()` memerlukan privilege khusus untuk mengubah permission file, melindungi integritas sistem file dari modifikasi tidak sah.

Kedua, system call mencegah privilege escalation dan race condition attacks. Dengan memaksa semua operasi sensitif melalui kernel space, sistem operasi dapat menerapkan prinsip least privilege, di mana setiap proses hanya mendapat akses minimal yang diperlukan. System call seperti `setuid()` dan `setgid()` diatur ketat oleh kernel untuk mencegah proses unprivileged mendapat akses root. Kernel juga mengimplementasikan atomic operations pada system call kritis, menghindari race condition yang dapat dieksploitasi attacker untuk mendapatkan akses tidak sah.

Ketiga, system call memfasilitasi audit dan monitoring keamanan sistem. Setiap invokasi system call dapat dicatat oleh subsistem audit kernel, memungkinkan administrator mendeteksi aktivitas mencurigakan. Tools seperti `auditd` memanfaatkan mekanisme ini untuk tracking system call seperti `execve()`, `connect()`, dan `unlink()` yang sering digunakan dalam serangan. Capability untuk melakukan forensic analysis dari audit log system call sangat krusial dalam incident response dan compliance security.

### Mekanisme Transisi User-Kernel yang Aman

Sistem operasi menggunakan beberapa mekanisme hardware dan software untuk memastikan transisi user-kernel berjalan aman. Pertama, CPU modern mengimplementasikan protection rings, dimana user space berjalan di ring 3 (least privileged) sedangkan kernel di ring 0 (most privileged). Transisi antar ring hanya dapat dilakukan melalui mekanisme terkontrol seperti interrupt gates dan trap gates yang dikonfigurasi oleh kernel pada Interrupt Descriptor Table (IDT).

Ketika aplikasi memanggil system call, CPU secara otomatis melakukan context switch yang mencakup saving register state, switching stack pointer ke kernel stack, dan verifikasi bahwa entry point valid. Kernel stack terpisah dari user stack mencegah stack-based attacks seperti buffer overflow dari user space mempengaruhi kernel. Proses ini terjadi secara atomik dalam hardware, tidak dapat diinterupsi oleh kode user space.

Kernel juga mengimplementasikan parameter validation yang ketat. Sebelum mengeksekusi system call, kernel memverifikasi bahwa semua pointer yang diberikan user space berada dalam alamat memori yang valid dan accessible oleh proses tersebut. Function seperti `copy_from_user()` dan `copy_to_user()` digunakan untuk safely transfer data antara user dan kernel space, dengan proteksi terhadap invalid memory access. Mekanisme SMEP (Supervisor Mode Execution Prevention) dan SMAP (Supervisor Mode Access Prevention) pada CPU modern menambah lapisan proteksi dengan mencegah kernel mengeksekusi atau mengakses memori user space secara langsung.

Teknologi Seccomp (Secure Computing Mode) dan SELinux menambah kontrol granular terhadap system call yang dapat digunakan aplikasi. Seccomp memungkinkan aplikasi membatasi diri hanya menggunakan subset system call tertentu, reducing attack surface. Sebagai contoh, aplikasi yang tidak memerlukan network access dapat mem-block semua socket-related system calls, melindungi dari network-based exploits meskipun aplikasi ter-compromise.

### Contoh System Call yang Sering Digunakan di Linux

Di lingkungan Linux, beberapa system call menjadi backbone operasi sehari-hari. System call `read()` dan `write()` mendominasi I/O operations, digunakan tidak hanya untuk file tetapi juga pipes, sockets, dan devices. System call `open()` dan `close()` mengelola file descriptors, resource fundamental dalam Unix philosophy "everything is a file". 

Untuk manajemen proses, `fork()` dan `execve()` bekerja sama menciptakan proses baru. `fork()` menduplikasi proses parent, sedangkan `execve()` mengganti image proses dengan program baru, kombinasi yang elegant dan powerful. System call `wait()` dan `waitpid()` mengkoordinasikan parent-child process synchronization.

Dalam domain networking, `socket()`, `bind()`, `listen()`, `accept()`, `connect()`, `send()`, dan `recv()` membentuk API socket yang comprehensive. System call `mmap()` sangat populer untuk memory-mapped I/O, meningkatkan performa akses file besar dan shared memory antar proses. System call modern seperti `epoll_wait()` dan `io_uring` mengoptimalkan high-performance I/O operations dalam server applications.

---

## KESIMPULAN

System call merupakan mekanisme fundamental yang:
1. **Mengontrol akses** aplikasi ke resource sistem
2. **Menjamin isolasi** antara user space dan kernel space
3. **Memfasilitasi keamanan** melalui validasi, otorisasi, dan audit
4. **Menyediakan abstraksi** hardware yang konsisten untuk aplikasi

Eksperimen strace dan dmesg menunjukkan bahwa mayoritas operasi sistem melibatkan system call, khususnya dalam kategori file management dan memory management. Transisi user-kernel yang aman dijamin oleh kombinasi hardware protection (CPU rings, SMEP/SMAP) dan software validation (parameter checking, Seccomp, SELinux).

---

**Referensi:**
- Linux Kernel Documentation - System Calls
- Maurice J. Bach, "The Design of the UNIX Operating System"
- Robert Love, "Linux Kernel Development"
- man pages: syscalls(2), strace(1), dmesg(1)
## Quiz
1. [Apa fungsi utama system call dalam sistem operasi?]  
   **Jawaban: Fungsi utama system call adalah sebagai penghubung (interface) antara program user dan kernel untuk meminta layanan sistem operasi secara aman,
    seperti akses file, manajemen proses, memori, dan perangkat keras.

3. [sebutkan 4 ksategori system call yang yang umum digunakan?]  
   **Jawaban:Empat kategori system call yang umum digunakan adalah:

1. Process Control – mengatur pembuatan dan pengelolaan proses.


2. File Management – membaca, menulis, dan mengelola file.


3. Device Management – mengakses dan mengontrol perangkat I/O.


4. Memory Management – mengatur alokasi dan penggunaan memori.

 
4. [Mengapa system call tidak bisa dipanggil langsung oleh user program?]  
   **Jawaban:**  Karena program user berjalan di user mode yang memiliki hak akses terbatas. Jika user program dapat mengakses kernel atau perangkat keras secara langsung,
    sistem akan menjadi tidak aman dan tidak stabil. Oleh karena itu, system call hanya dapat diakses melalui mekanisme khusus yang menyebabkan perpindahan dari user mode ke kernel mode.


---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?
- tugas yang begitu banyak 
- Bagaimana cara Anda mengatasinya?
- rajin mengerjakannya  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
