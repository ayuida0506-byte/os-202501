
# Laporan Praktikum Minggu [2]
Topik: [Struktur System Call Dan Fungsi Kernel]

---

## Identitas
- **Nama**  : [Ayu Ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1DDSRA]

**#DESKRIPSI**
1.Jenis-jenis system call yang umum digunakan (file, process, device, communication).
2.Alur eksekusi system call dari mode user menuju mode kernel.
3.Cara melihat daftar system call yang aktif di sistem Linux.5
---# System Call: Jenis, Alur Eksekusi, dan Monitoring

## 1. Jenis-jenis System Call yang Umum Digunakan

### A. File Management System Calls

Untuk operasi file dan direktori:

**Operasi File Dasar:**
- `open()` - Membuka file, mengembalikan file descriptor
- `read()` - Membaca data dari file
- `write()` - Menulis data ke file
- `close()` - Menutup file descriptor
- `lseek()` - Memindahkan posisi pointer file
- `unlink()` - Menghapus file

**Informasi dan Permission:**
- `stat()` / `fstat()` - Mendapatkan informasi file
- `chmod()` - Mengubah permission file
- `chown()` - Mengubah owner file
- `access()` - Mengecek akses permission

**Operasi Direktori:**
- `mkdir()` - Membuat direktori
- `rmdir()` - Menghapus direktori
- `chdir()` - Mengubah working directory
- `getcwd()` - Mendapatkan current directory

**Contoh Kode:**
```c
#include <fcntl.h>
#include <unistd.h>

int main() {
    int fd = open("data.txt", O_RDONLY);
    char buffer[100];
    ssize_t bytes = read(fd, buffer, 100);
    close(fd);
    return 0;
}
```

---

### B. Process Control System Calls

Untuk manajemen proses:

**Pembuatan dan Eksekusi:**
- `fork()` - Membuat child process (duplikasi proses)
- `exec()` family - Menjalankan program baru (execl, execv, execvp, execve)
- `exit()` - Mengakhiri proses
- `wait()` / `waitpid()` - Menunggu child process selesai

**Informasi Proses:**
- `getpid()` - Mendapatkan process ID
- `getppid()` - Mendapatkan parent process ID
- `getuid()` / `getgid()` - User/Group ID

**Kontrol Proses:**
- `kill()` - Mengirim signal ke proses
- `signal()` - Set signal handler
- `pause()` - Suspend proses sampai ada signal
- `nice()` / `setpriority()` - Mengatur prioritas proses

**Contoh Fork dan Exec:**
```c
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();
    
    if (pid == 0) {
        // Child process
        execl("/bin/ls", "ls", "-l", NULL);
    } else if (pid > 0) {
        // Parent process
        wait(NULL);  // Tunggu child selesai
    }
    return 0;
}
```

---

### C. Device Management System Calls

Untuk akses perangkat I/O:

**Operasi Device:**
- `ioctl()` - Device-specific control operations
- `read()` / `write()` - Baca/tulis dari/ke device
- `open()` / `close()` - Buka/tutup device file
- `fcntl()` - File/device control operations

**I/O Multiplexing:**
- `select()` - Monitor multiple file descriptors
- `poll()` - Wait for events on file descriptors
- `epoll()` - Scalable I/O event notification (Linux)

**Contoh ioctl:**
```c
#include <sys/ioctl.h>
#include <fcntl.h>

int main() {
    int fd = open("/dev/ttyS0", O_RDWR);
    int status;
    
    // Get modem status
    ioctl(fd, TIOCMGET, &status);
    
    close(fd);
    return 0;
}
```

---

### D. Communication System Calls

Untuk komunikasi antar proses (IPC):

**1. Pipes:**
- `pipe()` - Membuat anonymous pipe
- `mkfifo()` - Membuat named pipe (FIFO)

```c
int pipefd[2];
pipe(pipefd);

if (fork() == 0) {
    close(pipefd[0]);  // Close read end
    write(pipefd[1], "Hello", 5);
    close(pipefd[1]);
} else {
    close(pipefd[1]);  // Close write end
    char buf[10];
    read(pipefd[0], buf, 5);
    close(pipefd[0]);
}
```

**2. Shared Memory:**
- `shmget()` - Membuat/akses shared memory segment
- `shmat()` - Attach shared memory ke address space
- `shmdt()` - Detach shared memory
- `shmctl()` - Control shared memory

**3. Message Queues:**
- `msgget()` - Membuat/akses message queue
- `msgsnd()` - Mengirim message
- `msgrcv()` - Menerima message
- `msgctl()` - Control message queue

**4. Semaphores:**
- `semget()` - Membuat/akses semaphore set
- `semop()` - Operasi semaphore (P/V operations)
- `semctl()` - Control semaphore

**5. Sockets (Network Communication):**
- `socket()` - Membuat endpoint komunikasi
- `bind()` - Bind socket ke address
- `listen()` - Listen untuk incoming connections
- `accept()` - Menerima connection request
- `connect()` - Koneksi ke remote socket
- `send()` / `recv()` - Kirim/terima data
- `sendto()` / `recvfrom()` - UDP communication

**Contoh Socket Server:**
```c
int sockfd = socket(AF_INET, SOCK_STREAM, 0);
struct sockaddr_in addr;
addr.sin_family = AF_INET;
addr.sin_port = htons(8080);
addr.sin_addr.s_addr = INADDR_ANY;

bind(sockfd, (struct sockaddr*)&addr, sizeof(addr));
listen(sockfd, 5);

int client = accept(sockfd, NULL, NULL);
send(client, "Hello", 5, 0);
close(client);
```

---

### E. Information Maintenance System Calls

Untuk mendapatkan informasi sistem:

- `time()` / `gettimeofday()` - Waktu sistem
- `uname()` - Informasi sistem operasi
- `sysinfo()` - Informasi sistem (memory, uptime, load)
- `getrusage()` - Resource usage statistics

---

## 2. Alur Eksekusi System Call

### Diagram Lengkap Alur Eksekusi:

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER SPACE                               │
│                      (Ring 3 - Unprivileged)                    │
└─────────────────────────────────────────────────────────────────┘

    [1] Aplikasi User
         │ Memanggil fungsi: read(fd, buffer, size)
         ▼
    
    [2] C Library (libc / glibc)
         │ • Wrapper function untuk syscall
         │ • Setup parameter di CPU registers:
         │   - System call number → %rax (x86-64)
         │   - Arg 1: fd → %rdi
         │   - Arg 2: buffer → %rsi
         │   - Arg 3: size → %rdx
         ▼
    
    [3] Trap/Software Interrupt
         │ Instruksi: syscall (x86-64) atau int 0x80 (x86-32)
         │
         ╔═══════════════════════════════════════════════════╗
         ║         MODE SWITCH: User → Kernel                ║
         ║    • CPU privilege level: Ring 3 → Ring 0        ║
         ║    • Stack switch: user stack → kernel stack     ║
         ║    • Instruksi privileged sekarang dapat diakses ║
         ╚═══════════════════════════════════════════════════╝
         │
         ▼

┌─────────────────────────────────────────────────────────────────┐
│                       KERNEL SPACE                              │
│                      (Ring 0 - Privileged)                      │
└─────────────────────────────────────────────────────────────────┘

    [4] System Call Handler Entry Point
         │ (entry_SYSCALL_64 di Linux)
         │ • Save user context (semua registers)
         │ • Save user stack pointer
         │ • Switch ke kernel stack
         ▼
    
    [5] System Call Dispatcher
         │ • Baca syscall number dari %rax
         │ • Validasi range syscall number
         │ • Lookup di sys_call_table[]
         │ • sys_call_table[syscall_number]
         ▼
    
    [6] Kernel Function Execution
         │ Contoh: sys_read() untuk read syscall
         │ • Validasi parameter (cek buffer valid, fd valid)
         │ • Cek permission (apakah proses boleh akses fd?)
         │ • Akses file system / device driver
         │ • Operasi I/O dengan hardware
         │ • Lock/synchronization jika perlu
         │ • Update kernel data structures
         ▼
    
    [7] Return Preparation
         │ • Simpan return value di %rax
         │ • Set error code jika ada error
         │ • Restore user context
         │
         ╔═══════════════════════════════════════════════════╗
         ║         MODE SWITCH: Kernel → User                ║
         ║    • CPU privilege level: Ring 0 → Ring 3        ║
         ║    • Stack switch: kernel stack → user stack     ║
         ║    • Return via sysret atau iret instruction     ║
         ╚═══════════════════════════════════════════════════╝
         │
         ▼

┌─────────────────────────────────────────────────────────────────┐
│                        USER SPACE                               │
└─────────────────────────────────────────────────────────────────┘

    [8] C Library Return Handler
         │ • Terima return value dari %rax
         │ • Cek error (jika negatif, set errno)
         │ • Format return value untuk aplikasi
         ▼
    
    [9] Aplikasi User
         │ • Terima hasil syscall
         │ • Lanjutkan eksekusi program
         ▼
```

### Penjelasan Detail Setiap Stage:

**Stage 1-2: User Space Preparation**
- Aplikasi memanggil fungsi library seperti `read()`
- Library function menyiapkan parameter di register CPU sesuai calling convention

**Stage 3: Mode Transition**
- Instruksi `syscall` (atau `int 0x80`) memicu hardware exception
- CPU otomatis:
  - Switch ke kernel mode (Ring 0)
  - Jump ke system call handler address
  - Switch stack pointer ke kernel stack
  - Save instruction pointer untuk return nanti

**Stage 4-5: Kernel Entry**
- Kernel save semua register user ke kernel stack
- Dispatcher lookup fungsi kernel yang sesuai berdasarkan syscall number
- Di Linux: `sys_call_table` adalah array pointer ke fungsi kernel

**Stage 6: Actual Work**
- Kernel function dengan full privilege dieksekusi
- Dapat mengakses hardware, memori, dan resource lain
- Melakukan operasi yang diminta aplikasi

**Stage 7-9: Return to User**
- Result disimpan di register
- Context user di-restore
- CPU switch kembali ke user mode
- Eksekusi dilanjutkan di user space

### Performance Cost:

**Overhead System Call:**
- Context switch: ~100-300 CPU cycles
- TLB flush: ~50-200 cycles
- Cache effects: variabel
- **Total**: ~1-2 microseconds per syscall

**Strategi Optimasi:**
- Batch I/O operations (buffering)
- Gunakan `mmap()` untuk file access
- Reduce syscall frequency
- Asynchronous I/O (`io_uring` di Linux modern)

---

## 3. Cara Melihat System Call di Linux

### A. Menggunakan `strace` (Recommended)

**Instalasi:**
```bash
# Debian/Ubuntu
sudo apt install strace

# RHEL/CentOS/Fedora
sudo dnf install strace

# Arch Linux
sudo pacman -S strace
```

**Penggunaan Dasar:**

```bash
# Trace semua syscall dari command
strace ls -l

# Output ke file
strace -o trace.log ls -l

# Trace proses yang sedang running
strace -p 1234

# Trace dengan timestamp
strace -t ls

# Dengan relative time
strace -r ls

# Dengan waktu eksekusi tiap syscall
strace -T ls
```

**Filter Syscall Spesifik:**

```bash
# Trace hanya open syscalls
strace -e open ls

# Trace kategori file operations
strace -e trace=file ls

# Trace multiple syscalls
strace -e open,read,write cat file.txt

# Trace semua network syscalls
strace -e trace=network wget example.com

# Trace process-related syscalls
strace -e trace=process ./program
```

**Statistical Summary:**

```bash
# Count syscalls
strace -c ls -lR /usr

# Output example:
# % time     seconds  usecs/call     calls    errors syscall
# ------ ----------- ----------- --------- --------- ----------------
#  48.23    0.002145          11       195           openat
#  22.15    0.000986           8       123           read
#  15.67    0.000697          10        70           close
# ...
```

**Advanced Options:**

```bash
# Trace child processes
strace -f ./parent_program

# String length untuk output
strace -s 200 cat file.txt

# Trace specific file operations
strace -e trace=%file -e trace=%desc ls

# Trace dengan detail penuh
strace -v -s 1024 ./program

# Filter by return value
strace -e open -e status=successful ls
strace -e open -e status=failed ls
```

**Contoh Output:**
```
openat(AT_FDCWD, ".", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
getdents64(3, /* 15 entries */, 32768) = 480
getdents64(3, /* 0 entries */, 32768)  = 0
close(3)                                = 0
write(1, "file1.txt\n", 10)            = 10
```

---

### B. Menggunakan `ltrace`

Trace library calls (termasuk syscalls):

```bash
# Install
sudo apt install ltrace

# Trace library dan system calls
ltrace ls

# Syscalls only
ltrace -S ls

# Count calls
ltrace -c ./program
```

---

### C. Melihat System Call Table

**1. Dari Header Files:**

```bash
# System call numbers (x86-64)
cat /usr/include/x86_64-linux-gnu/asm/unistd_64.h | grep __NR

# Output example:
# #define __NR_read 0
# #define __NR_write 1
# #define __NR_open 2
# #define __NR_close 3
```

**2. Menggunakan `ausyscall`:**

```bash
# Install audit package
sudo apt install auditd

# List all syscalls
ausyscall --dump

# Search specific syscall
ausyscall open
ausyscall 2  # By number

# Output:
# open          2
# openat      257
```

**3. Dari Kernel Source:**

```bash
# Clone kernel (atau lihat online)
git clone --depth 1 https://github.com/torvalds/linux.git

# System call table
cat linux/arch/x86/entry/syscalls/syscall_64.tbl

# Excerpt:
# 0    common  read           sys_read
# 1    common  write          sys_write
# 2    common  open           sys_open
# 3    common  close          sys_close
```

---

### D. Real-time Monitoring dengan `perf`

**Instalasi:**
```bash
sudo apt install linux-tools-common linux-tools-generic
```

**Trace Syscalls:**

```bash
# Trace single command
sudo perf trace ls -l

# Trace with statistics
sudo perf trace -s ls -l

# Trace specific syscalls
sudo perf trace -e open,close ls

# Trace all processes
sudo perf trace

# Record and report
sudo perf record -e 'syscalls:sys_enter_*' ls
sudo perf report
```

**Performance Statistics:**

```bash
# Count syscalls
sudo perf stat -e 'syscalls:sys_enter_*' ls -lR /usr

# Output:
# Performance counter stats for 'ls -lR /usr':
#            1,234      syscalls:sys_enter_openat
#              567      syscalls:sys_enter_read
#              234      syscalls:sys_enter_close
```

---

### E. Menggunakan `/proc` Filesystem

**Informasi Runtime:**

```bash
# Syscall yang sedang running
cat /proc/self/syscall
cat /proc/1234/syscall

# System call statistics (jika CONFIG_PROC_FS enabled)
cat /proc/1234/status | grep voluntary

# Open files (file descriptors)
ls -l /proc/self/fd
ls -l /proc/1234/fd
```

---

### F. Audit System dengan `auditd`

**Setup:**

```bash
# Install
sudo apt install auditd audispd-plugins

# Start service
sudo systemctl start auditd

# Monitor specific syscall
sudo auditctl -a always,exit -F arch=b64 -S open -S openat

# Watch file access
sudo auditctl -w /etc/passwd -p war -k password_changes

# View logs
sudo ausearch -sc open
sudo ausearch -k password_changes

# View real-time
sudo aureport --summary
```

---

### G. Custom Program untuk System Call

**Program C untuk Trace Syscall:**

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/syscall.h>

int main() {
    // Direct syscall tanpa library wrapper
    long result;
    
    // getpid syscall (number: 39 di x86-64)
    result = syscall(SYS_getpid);
    printf("PID via syscall: %ld\n", result);
    
    // write syscall (number: 1)
    const char *msg = "Hello via direct syscall!\n";
    syscall(SYS_write, 1, msg, 26);
    
    // open syscall
    result = syscall(SYS_open, "/tmp/test.txt", 0);
    printf("FD: %ld\n", result);
    
    return 0;
}
```

**Compile dan trace:**
```bash
gcc -o syscall_test syscall_test.c
strace ./syscall_test
```

---

### H. BPF/eBPF untuk Advanced Tracing

**Menggunakan `bpftrace`:**

```bash
# Install
sudo apt install bpftrace

# List available syscalls
sudo bpftrace -l 'tracepoint:syscalls:*'

# Trace all syscalls with count
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_* { @[probe] = count(); }'

# Trace specific syscall dengan argument
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_open { printf("%s\n", str(args->filename)); }'

# Histogram of syscall durations
sudo bpftrace -e '
  tracepoint:syscalls:sys_enter_read { @start[tid] = nsecs; }
  tracepoint:syscalls:sys_exit_read /@start[tid]/ {
    @duration = hist(nsecs - @start[tid]);
    delete(@start[tid]);
  }
'
```

---

### I. Praktik Terbaik

**Debugging Application:**
```bash
# Full trace dengan timestamps
strace -ttT -o debug.log ./myapp

# Analyze most frequent syscalls
strace -c ./myapp

# Find slow syscalls
strace -T ./myapp 2>&1 | grep "^.*<.*>" | sort -k2 -n
```

**Security Auditing:**
```bash
# Monitor file access
sudo auditctl -w /etc/shadow -p rwxa

# Monitor privileged syscalls
sudo auditctl -a always,exit -F arch=b64 -S setuid -S setgid
```

**Performance Analysis:**
```bash
# System-wide syscall frequency
sudo perf top -e raw_syscalls:sys_enter

# Per-process analysis
sudo perf trace -p $(pidof myapp)
```

---

## Rangkuman

**System Call Categories:**
- **File**: open, read, write, close
- **Process**: fork, exec, wait, exit
- **Device**: ioctl, device I/O
- **Communication**: pipe, socket, IPC mechanisme

## Tujuan
Tuliskan tujuan praktikum minggu ini.  
1.Menjelaskan konsep dan fungsi system call dalam sistem operasi.
2.Mengidentifikasi jenis-jenis system call dan fungsinya.
3.Mengamati alur perpindahan mode user ke kernel saat system call terjadi.
4.Menggunakan perintah Linux untuk menampilkan dan menganalisis system call.
System call merupakan mekanisme yang memungkinkan program di mode user berinteraksi dengan kernel sistem operasi untuk mengakses layanan seperti pengelolaan proses, file, dan perangkat keras. Fungsinya menyediakan antarmuka aman antara aplikasi pengguna dan sumber daya sistem, mencegah akses langsung yang berpotensi merusak.

## Jenis System Call
System call dikategorikan menjadi lima jenis utama berdasarkan fungsinya.

- **Process control**: Membuat, menghentikan proses, menunggu event, atau mengalokasikan memori (contoh: fork, exit).
- **File management**: Membuat, membuka, membaca, menulis, atau menghapus file (contoh: open, read, write).
- **Device management**: Meminta, membaca/menulis perangkat, atau mengatur atribut device.
- **Information maintenance**: Mengambil atau mengatur data sistem seperti waktu, tanggal, atau atribut proses/file.
- **Communication**: Mengirim pesan antar-proses, berbagi memori, atau koneksi (contoh: send, receive).

## Alur Perpindahan Mode
Saat system call dipanggil, proses di user mode menyiapkan parameter dan nomor system call di register. Kemudian, instruksi seperti software interrupt (syscall atau int) memicu trap, memindahkan CPU ke kernel mode melalui Interrupt Descriptor Table (IDT). Kernel mengeksekusi handler, memproses permintaan, lalu mengembalikan hasil dan beralih kembali ke user mode setelah restore state.

## Perintah Linux untuk Analisis
Gunakan perintah **strace** untuk melacak system call secara real-time. Contoh penggunaan:

- `strace ls`: Menampilkan semua system call saat menjalankan `ls`, termasuk open, read, dan exit.
- `strace -e read ls`: Filter hanya system call read.
- `strace -p <PID>`: Attach ke proses berjalan untuk memantau system call-nya.
- `strace -c ls`: Ringkasan jumlah dan waktu system

Output strace menunjukkan nama call, argumen, dan return value, berguna untuk debugging error atau performa.


## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.
**Ringkasan teori mendasari percobaan system call** :

1. System call berfungsi sebagai antarmuka terprogram antara aplikasi user mode dan kernel OS, memungkinkan akses aman ke sumber daya hardware dan layanan sistem.
2. Proses berpindah dari user mode ke kernel mode melalui software interrupt (syscall instruction), yang memicu eksekusi handler di kernel setelah validasi parameter.
3. Kernel menyediakan tabel system call dengan nomor unik yang memetakan ke fungsi spesifik, memastikan eksekusi layanan seperti file I/O atau process control.
4. Setelah pemrosesan, kernel mengembalikan hasil via return value dan memulihkan user mode, menjaga isolasi proteksi memori antar-proses.

## Struktur System Call
Struktur system call terdiri dari nomor system call (di register seperti EAX pada x86), parameter (stack/register), dan instruksi trap seperti `syscall` atau `int 0x80`. Kernel menggunakan System Call Table (syscall_table) untuk memetakan nomor ke fungsi C di kernel space, dengan validasi akses melalui pointer dan argumen. Implementasi modern Linux menggunakan fast syscall (VDSO atau sysenter) untuk overhead minimal.

## Fungsi Kernel Terkait
Kernel mengeksekusi fungsi system call dalam mode supervisor untuk mengelola:
- **Process Management**: fork(), exec(), exit() untuk lifecycle proses.
- **File Operations**: open(), read(), write(), close() via VFS layer.
- **Memory Allocation**: brk(), mmap() untuk heap dan virtual memory.
- **I/O Control**: ioctl(), poll() untuk device dan network.
Fungsi-fungsi ini diimplementasikan sebagai sys_xxx() di kernel, dengan multiplexing untuk concurrency dan proteksi.



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
![)

---

## Analisis
Tulis analisis 400–500 kata tentang:
1.Mengapa system call penting untuk keamanan OS?
2.Bagaimana OS memastikan transisi user–kernel berjalan aman?
3.Sebutkan contoh system call yang sering digunakan di Linux.

System call memainkan peran krusial dalam menjaga keamanan sistem operasi dengan membatasi akses langsung aplikasi user ke hardware dan sumber daya kernel. Tanpa system call, program bisa memanipulasi memori kernel atau perangkat secara sembarangan, menyebabkan crash atau eksploitasi. Mekanisme ini memaksa transisi ke kernel mode untuk validasi dan eksekusi, mencegah pelanggaran isolasi.

1.Pentingnya untuk Keamanan OS
System call penting karena menerapkan prinsip least privilege: user process hanya berinteraksi via antarmuka terdefinisi, bukan instruksi hardware langsung. Kernel memvalidasi parameter, hak akses (seperti permission file), dan kuota sumber daya sebelum bertindak, menghalau serangan buffer overflow atau privilege escalation. Selain itu, setiap call diaudit untuk logging, memudahkan deteksi intrusi. Di era modern, system call melindungi dari malware yang mencoba akses unauthorized, seperti ransomware yang gagal tanpa enkripsi kernel-level.


2.OS memastikan transisi aman melalui dual-mode operation: user mode (ring 3) dibatasi, kernel mode (ring 0) punya akses penuh. Prosesnya:
- User mode menyiapkan argumen dan nomor syscall di register.
- Instruksi `syscall` memicu exception handler via Model-Specific Register (MSR), menyimpan context di kernel stack.
- Kernel memeriksa validitas (pointer bounds, syscall number), menjalankan fungsi, lalu `sysret` mengembalikan control dengan restore flags dan PC.
Hardware seperti CR0 dan segment descriptor mencegah return prematur ke user mode rusak. Linux gunakan seccomp untuk filter syscall per-process, blokir call berbahaya.

3.Contoh System Call yang sering digunakan di Linux
Beberapa system call umum di Linux:
- **read/write (1/0)**: Akses file atau socket, dengan check permission.
- **open/close (2/3)**: Buka file descriptor, validasi path dan mode.
- **fork/execve (57/59)**: Buat proses child atau ganti image.
- **mmap/munmap (9/11)**: Alokasi virtual memory.
- **ioctl (16)**: Kontrol device spesifik.
Contoh trace: `strace cat file.txt` tunjukkan openat(AT_FDCWD, "file.txt", O_RDONLY) = 3; read(3, ...) 


## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.

1. System call terbukti sebagai jembatan esensial antara user space dan kernel space, memungkinkan akses aman ke layanan OS seperti process control, file I/O, dan memory management melalui struktur terdefinisi (nomor syscall, parameter register, syscall_table).

2. Transisi mode user-kernel berjalan efisien via instruksi syscall/int 0x80 yang memicu handler kernel untuk validasi dan eksekusi fungsi sys_xxx(), diikuti sysret untuk pulihkan context, menjaga isolasi proteksi sambil minimalkan overhead.

3. Tools Linux seperti strace efektif untuk observasi real-time system call, mengungkap alur eksekusi (contoh: open/read/exit pada ls) dan analisis performa/debugging, mengonfirmasi peran kernel dalam multiplexing layanan sistem secara aman.

## Struktur System Call Lanjutan
Setiap system call dimulai dengan nomor unik di register EAX/R10, parameter di stack/register berikutnya, dan trap instruction yang lookup syscall_table untuk dispatch ke fungsi kernel C (sys_read, sys_write, dll.). Kernel validasi bounds checking dan capability sebelum akses resource.

## Fungsi Kernel Utama
Kernel menyediakan fungsi sys_* yang diimplementasikan di /kernel/sys.c dan fs/*, seperti:
- **sys_fork**: Duplikasi process descriptor.
- **sys_open**: Alokasi file descriptor via VFS.
- **sys_brk**: Adjust heap pointer untuk malloc.
Fungsi ini beroperasi di ring 0 dengan proteksi page table dan reference counting.



## Quiz
1. [Apa Fungsi Utama System Call Dalam Sistem Operasi?]  
   **Jawaban:**  Fungsi utama system call dalam sistem operasi adalah menyediakan antarmuka terstruktur dan aman bagi program user-mode untuk meminta layanan kernel, seperti akses hardware, pengelolaan proses, dan operasi file, tanpa mengizinkan akses langsung yang berbahaya.
   
2. [Sebutkan 4 kategori system call yang umum  digunakan?]  
   **Jawaban:**  | Kategori          | Fungsi Utama               | Contoh                                       |
| ----------------- | -------------------------- | -------------------------------------------- |
| Process Control   | Mengelola lifecycle proses | fork(), exit(), wait() scribd​               |
| File Management   | Operasi sistem file        | open(), read(), write(), close() guru99​     |
| Device Management | Kontrol perangkat keras    | ioctl(), read_device() ubung-style.blogspot​ |
| Communication     | Interaksi antar-proses     | send(), receive(), pipe() scribd​            |

3. [Mengapa system call tidak bisa dipanggil langsung oleh user program?]  
   **Jawaban:**User program tidak bisa memanggil system call langsung karena CPU dijalankan dalam user mode (ring 3) yang membatasi akses instruksi privileged seperti akses memori kernel atau I/O port. Panggilan langsung akan:
​Melewati validasi kernel (parameter bounds, permission check), berisiko crash atau eksploitasi.
Melanggar proteksi memori via page table dan segment descriptor.
Mengabaikan concurrency control (locking, reference counting).
Proses aman: user mode → syscall instruksi → software interrupt → kernel mode → validasi → eksekusi → sysret → user mode.
​  

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
