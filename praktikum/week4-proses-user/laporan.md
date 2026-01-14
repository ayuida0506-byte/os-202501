
# Laporan Praktikum Minggu [4]
Topik: [Manajemen proses dan User di Linux]

---

## Identitas
- **Nama**  : [Ayu Ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSRA]

---
**##DESKRIPSI SINGKAT**
1.Membuat dan mengatur proses (process management).
2.Mengelola user, group, serta hak akses pengguna.
3.Menampilkan, menghentikan, dan mengontrol proses yang sedang berjalan.
4.Menghubungkan konsep user management dengan keamanan sistem operasi.
Manajemen proses dan user di Linux memungkinkan pengendalian sumber daya sistem secara efisien melalui perintah seperti `ps`, `top`, `kill`, serta `useradd` dan `chmod`. Topik ini krusial untuk keamanan, karena permission user terkait langsung dengan proses yang berjalan. Berikut panduan lengkap berdasarkan praktikum sebelumnya.

1.## Membuat dan Mengatur Proses
Gunakan `ps aux` untuk menampilkan semua proses (PID, CPU%, MEM%, command), `top` atau `htop` untuk monitoring real-time dengan sort CPU (Shift+P). Buat proses baru via `&` (background, e.g., `sleep 100 &`) atau `nohup command &`. Hentikan dengan `kill PID` (sinyal TERM) atau `kill -9 PID` (force KILL).

2.## Mengelola User dan Group
Tambah user: `sudo useradd -m username` (buat home dir), set password `sudo passwd username`. Kelola group: `sudo groupadd groupname`, `sudo usermod -aG groupname username`. Hak akses via `chmod` (file) dan `chown` (owner), verifikasi `ls -l` untuk uid/gid.

3.## Menampilkan dan Mengontrol Proses
- `ps -ef`: Tampilkan tree proses (PPID relation).
- `kill -STOP PID`: Tunda; `kill -CONT PID`: Lanjutkan.
- `renice -n 10 PID`: Ubah prioritas (nice value -20 tinggi hingga 19 rendah).
Gunakan `pkill pattern` untuk hentikan berdasarkan nama.

4.## Hubungan dengan Keamanan
User management batasi proses via UID/GID; non-root user tak bisa `kill` proses root (permission denied). Permission file cegah proses akses unauthorized data, sementara `sudo` grant temporary privilege. Ini bentuk DAC + capability model Linux cegah escalation, esensial multi-user/server.

## Tujuan
Tuliskan tujuan praktikum minggu ini.  
1.Menjelaskan konsep proses dan user dalam sistem operasi Linux.
2.Menampilkan daftar proses yang sedang berjalan dan statusnya.
3.Menggunakan perintah untuk membuat dan mengelola user.
4.Menghentikan atau mengontrol proses tertentu menggunakan PID.
5.Menjelaskan kaitan antara manajemen user dan keamanan sistem.

1.Konsep Proses dan User dalam Linux
Proses: Sebuah proses adalah program yang sedang dieksekusi. Setiap proses memiliki ID Proses (PID) unik, status (misalnya, berjalan, berhenti, atau tidur), dan sumber daya sistem yang dialokasikan (CPU, memori, dll.). Semua proses merupakan turunan dari proses sistem pertama, systemd (atau init pada sistem lama).
User: Linux adalah sistem operasi multi-user, yang berarti beberapa pengguna dapat masuk dan berinteraksi dengan sistem secara bersamaan. Setiap file, proses, dan sumber daya sistem lainnya memiliki pemilik (owner) dan izin (permissions) yang terkait dengan pengguna atau grup tertentu. Hal ini mendasari model keamanan Linux.
2.Menampilkan Daftar Proses 
Perintah utama untuk melihat proses yang sedang berjalan adalah:
ps: Menampilkan snapshot proses saat ini.
ps aux: Menampilkan semua proses di sistem, dengan informasi detail seperti pemilik proses, penggunaan CPU/memori, dan status.
top: Menampilkan daftar proses yang berjalan secara real-time dan kondisi sistem secara keseluruhan, termasuk penggunaan sumber daya.
htop: Alternatif top yang lebih interaktif dan visual (mungkin perlu diinstal).
3.Membuat dan Mengelola User 
Perintah untuk manajemen user umumnya memerlukan hak akses administrator (sudo): 
useradd [nama_user]: Perintah untuk membuat user baru.
passwd [nama_user]: Mengatur atau mengubah kata sandi untuk user yang ditentukan.
usermod: Memodifikasi properti user (misalnya, menambahkan ke grup tambahan).
userdel [nama_user]: Menghapus akun user.
whoami: Menampilkan nama user yang sedang login saat ini. 
4.Mengontrol Proses Tertentu Menggunakan PID
Perintah kill digunakan untuk mengirimkan sinyal ke proses guna mengontrol atau menghentikannya. 
kill [PID]: Mengirim sinyal SIGTERM (sinyal penghentian yang lembut, memungkinkan proses untuk membersihkan dirinya sendiri sebelum keluar) ke proses dengan PID tertentu.
kill -9 [PID]: Mengirim sinyal SIGKILL (penghentian paksa) ke proses. Proses tidak dapat mengabaikan sinyal ini, memastikan penghentian segera. 
5.Kaitan antara Manajemen User dan Keamanan Sistem
Manajemen user sangat penting untuk keamanan sistem Linux: 
Prinsip Hak Akses Terkecil (Principle of Least Privilege): Setiap user hanya memiliki izin yang diperlukan untuk menjalankan tugasnya. Ini mengurangi potensi kerusakan jika akun user disusupi.
Isolasi Proses: Proses yang dijalankan oleh user yang berbeda diisolasi satu sama lain secara default. Satu proses berbahaya yang dimiliki oleh user dengan hak akses rendah tidak dapat dengan mudah memengaruhi proses atau file yang dimiliki oleh user lain (terutama user root).
Akuntabilitas: Setiap tindakan dalam sistem dapat dilacak kembali ke user tertentu, memungkinkan audit dan penanganan insiden keamanan.
sudo: Penggunaan perintah sudo secara terkontrol memungkinkan user non-root untuk menjalankan perintah administratif, menghindari kebutuhan untuk selalu masuk sebagai root (yang sangat berisiko). 

---

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.

---
Berikut adalah ringkasan teori yang mendasari percobaan manajemen proses dan user dalam Linux:
Abstraksi Proses dan PID: Setiap program yang dieksekusi oleh sistem operasi disebut sebagai proses. Linux mengelola setiap proses secara unik menggunakan nomor identitas yang disebut PID (Process ID), yang berfungsi untuk memonitor, mengalokasikan sumber daya, dan mengontrol siklus hidup proses tersebut [1].
Model Multi-User dan Kepemilikan: Linux dirancang sebagai sistem multi-user, di mana setiap proses yang berjalan selalu diasosiasikan dengan user tertentu yang memulainya. Hal ini memastikan bahwa hak akses proses tersebut terbatas pada izin (permissions) yang dimiliki oleh user tersebut [1, 2].
Hirarki dan Kontrol Sinyal: Manajemen proses memungkinkan interaksi antar-proses melalui pengiriman sinyal (seperti SIGTERM atau SIGKILL). Sistem operasi menyediakan mekanisme bagi administrator untuk menghentikan proses yang bermasalah atau tidak responsif berdasarkan PID-nya [1].
Keamanan melalui Isolasi dan Privilege: Manajemen user berfungsi sebagai lapisan keamanan utama dengan menerapkan kontrol akses. Melalui pemisahan antara user biasa dan superuser (root), sistem mencegah akses tidak sah ke file sensitif atau konfigurasi sistem yang kritis [2].
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
1.Dokumentasikan hasil semua perintah dan jelaskan fungsi tiap perintah.
2.Gambarkan hierarki proses dalam bentuk diagram pohon (pstree) di laporan.
3.Jelaskan hubungan antara user management dan keamanan sistem Linux.
4.Upload laporan ke repositori Git tepat waktu
1. Dokumentasi Hasil Perintah dan Penjelasan Fungsi
Perintah	Contoh Output (Hasil)	Fungsi & Penjelasan
ps aux	root 1 0.0 0.1 1684 ... /sbin/init	Menampilkan snapshot seluruh proses yang aktif di sistem secara detail (User, PID, %CPU, %MEM).
top	PID USER PR NI VIRT RES ... %CPU %MEM	Menampilkan proses secara real-time dan interaktif, berguna untuk memantau konsumsi sumber daya tertinggi.
sudo useradd mhs1	(Tidak ada output jika berhasil)	Membuat user baru bernama mhs1 ke dalam sistem.
sudo passwd mhs1	Enter new UNIX password:	Mengatur atau mengubah kata sandi untuk user agar bisa melakukan login.
kill -9 1234	(Tidak ada output jika berhasil)	Mengirim sinyal penghentian paksa ke proses dengan PID 1234. Digunakan jika proses "hang".
whoami	namauser_anda	Menampilkan nama user yang saat ini sedang aktif di sesi terminal.
2. Hierarki Proses (pstree)
Gunakan perintah pstree untuk melihat hubungan orang tua dan anak (parent-child) antar proses. Berikut adalah representasi visualnya:
text
systemd (PID 1)
 ├── cron
 ├── networkd
 ├── sshd
 │    └── sshd (user login)
 │         └── bash
 │              └── pstree
 └── systemd-journal
Gunakan kode dengan hati-hati.

Penjelasan: Di Linux, semua proses bermuara pada systemd (atau init). Jika Anda menjalankan terminal, maka bash adalah anak dari proses emulator terminal tersebut.
3. Hubungan User Management dan Keamanan Sistem
Manajemen user adalah pilar utama keamanan Linux melalui mekanisme berikut:
Isolasi Hak Akses: Setiap user memiliki ruang lingkup (home directory) masing-masing. User biasa tidak dapat menghapus file sistem atau file user lain, sehingga membatasi dampak jika terjadi kesalahan atau serangan.
Hak Istimewa Sudo: Dengan sudo, administrator tidak perlu login sebagai root secara terus-menerus. Ini mencegah perubahan sistem yang tidak disengaja dan memberikan jejak audit (logging) siapa yang menjalankan perintah berbahaya.
Kepemilikan Proses: Setiap proses yang berjalan mewarisi hak akses dari user yang menjalankannya. Jika sebuah aplikasi (misal: browser) diserang, penyerang hanya mendapatkan hak akses terbatas sesuai user tersebut, bukan akses penuh ke sistem.

---

## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
Berikut adalah 2–3 poin kesimpulan dari praktikum manajemen proses dan user di Linux:
Kendali Penuh terhadap Sumber Daya: Praktikum ini membuktikan bahwa Linux memberikan kendali penuh kepada administrator untuk memantau dan mengelola setiap aktivitas sistem melalui PID. Dengan perintah seperti ps, top, dan kill, pengguna dapat memastikan stabilitas sistem dengan menghentikan proses yang tidak responsif atau memakan sumber daya berlebih secara presisi.
Efektivitas Sistem Multi-User: Melalui manajemen user, Linux berhasil mengimplementasikan sistem keamanan yang kokoh. Pemisahan hak akses antara user biasa dan root memastikan bahwa setiap individu hanya dapat mengakses sumber daya yang diizinkan, sehingga melindungi integritas file sistem dari perubahan yang tidak disengaja maupun serangan keamanan.
Struktur Hirarki yang Terorganisir: Penggunaan pstree menunjukkan bahwa sistem Linux bekerja secara terstruktur, di mana setiap proses memiliki induk (parent). Hal ini mempermudah pelacakan asal-usul proses dan memahami bagaimana layanan sistem saling terkait satu sama lain sejak proses booting (systemd).

---

## Quiz
1. [Apa fungsi dari proses int atau systemd dalam sistem Linux?]  
   **Jawaban:** 
. Fungsinya meliputi: 
Induk dari Semua Proses: Ia menjadi leluhur (ancestor) bagi semua proses lain yang berjalan di sistem dan secara otomatis "mengadopsi" proses yang kehilangan induknya.
Manajemen Layanan: Menginisialisasi perangkat keras serta memulai dan mengelola layanan (services) sistem dari awal hingga sistem dimatikan.
Kontrol Booting: Menentukan urutan pemuatan komponen sistem agar berjalan efisien dan stabil.  
2. [Apa perbedaan antara kill dan killall?]  
   **Jawaban:**  Perbedaan antara kill dan killall
Meskipun keduanya digunakan untuk mengirim sinyal (seperti penghentian proses), cakupan dan cara kerjanya berbeda:
kill: Lebih presisi karena menargetkan satu proses spesifik menggunakan PID (Process ID). Contoh: kill 1234.
killall: Menghentikan semua proses sekaligus berdasarkan nama proses. Contoh: killall firefox akan menghentikan semua jendela browser Firefox yang terbuka. Ini memudahkan pembatalan banyak proses yang sama tanpa harus mencari PID satu per satu. 
3. [Mengapa user root memiliki hak istimewa disitem Linux?]  
   **Jawaban:**  Hak Istimewa User Root
User root (superuser) memiliki hak istimewa tak terbatas karena perannya sebagai Administrator Sistem. Alasan root memiliki hak khusus meliputi: 
Kendali Penuh: Root dapat memodifikasi file apa pun, menambah/menghapus pengguna, serta menginstal atau menghapus perangkat lunak tanpa batasan izin.
Manajemen Sistem Kritis: Root diperlukan untuk melakukan tugas-tugas administratif yang memengaruhi keamanan dan integritas sistem, seperti mengonfigurasi perangkat keras atau mengubah izin akses file sistem.
Keamanan melalui Isolasi: Dengan membatasi hak istimewa ini hanya pada root, Linux memastikan bahwa user biasa tidak dapat merusak sistem secara tidak sengaja atau sengaja. 

---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
