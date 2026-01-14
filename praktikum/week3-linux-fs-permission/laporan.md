
# Laporan Praktikum Minggu [3]
Topik:[Manajemen file dan permission di linux]

---

## Identitas
- **Nama**  : [Ayu Ida Nuraini]  
- **NIM**   : [250320588]  
- **Kelas** : [1DSRA]

---

**#DESKRIPSI**
Tuliskan tujuan praktikum minggu ini.  
Contoh:  
1 Navigasi sistem file dengan ls, pwd, cd, dan cat.
2.Pengaturan hak akses file menggunakan chmod.
3.Pengubahan kepemilikan file menggunakan chown.
4.Dokumentasi hasil eksekusi dan pengelolaan repositori praktikum.
Navigasi sistem file menggunakan perintah dasar Linux seperti `ls`, `pwd`, `cd`, dan `cat` memungkinkan eksplorasi direktori dan isi file dengan efisien.  Perintah ini esensial untuk pemula dalam mengelola terminal Linux.9

1.## Navigasi Dasar
Gunakan `pwd` untuk menampilkan direktori saat ini, misalnya menghasilkan `/root` atau `/home/username`.  Perintah `ls` mencantumkan file dan folder, dengan opsi `-l` untuk detail seperti izin dan ukuran, serta `-a` untuk file tersembunyi.  `cd` mengubah direktori, seperti `cd /` ke root, `cd ..` ke parent, atau `cd ~` ke home; gunakan `cd -` untuk kembali ke direktori sebelumnya.

## Melihat Isi File
Perintah `cat` menampilkan isi file secara langsung, berguna untuk file teks kecil seperti `cat example.txt`.  Kombinasikan dengan navigasi sebelumnya untuk membaca file di direktori target setelah `cd`.

2.## Pengaturan Hak Akses
Gunakan `chmod` untuk mengubah izin file, dengan sintaks `chmod [mode] [file]`, di mana mode bisa simbolik seperti `u+wx` (tambah write/execute untuk owner) atau numerik seperti 755.  Contoh: `chmod 644 file.txt` memberikan read/write untuk owner dan read untuk group/others.

3.## Pengubahan Kepemilikan
Perintah `chown` mengubah owner dan group file, dengan sintaks `sudo chown [user][:group] file`, seperti `sudo chown root example.txt`.  Opsi `-R` untuk rekursif pada direktori, dan `-h` untuk symbolic link.

4.## Dokumentasi dan Git
Dokumentasikan hasil dengan `script` untuk merekam sesi terminal atau screenshot, lalu simpan di repositori Git.  Inisialisasi repo dengan `git init`, commit perubahan via `git add . && git commit -m "Praktikum navigasi dan izin"`, dan push ke GitHub; gunakan Git LFS untuk file besar.

## Tujuan
1.Menggunakan perintah ls, pwd, cd, cat untuk navigasi file dan direktori.
2.Menggunakan chmod dan chown untuk manajemen hak akses file.
3.Menjelaskan hasil output dari perintah Linux dasar.
4.Menyusun laporan praktikum dengan struktur yang benar.
5.Mengunggah dokumentasi hasil ke Git Repository tepat waktu.
Perintah Linux dasar seperti `ls`, `pwd`, `cd`, dan `cat` memungkinkan navigasi dan inspeksi file sistem secara efisien. `chmod` dan `chown` digunakan untuk mengelola izin akses, sementara dokumentasi hasil praktikum memerlukan struktur laporan yang jelas dan unggah ke Git tepat waktu. Penjelasan output perintah membantu memahami hasil eksekusi.

## Navigasi File
`pwd` menampilkan path direktori saat ini, seperti `/home/user`. `ls -l` mencantumkan file dengan detail izin, owner, ukuran, dan tanggal modifikasi; `ls -a` termasuk file tersembunyi. Gunakan `cd /path` untuk pindah direktori, `cd ~` ke home, dan `cat file.txt` untuk isi file.

## Manajemen Izin
`chmod 755 file.txt` beri read/execute semua, write hanya owner (rwxr-xr-x). Output menunjukkan perubahan seperti `-rwxr-xr-x 1 user group`. `sudo chown user:group file.txt` ubah owner; `-R` untuk rekursif.

## Penjelasan Output
Output `ls -l` dimulai dengan tipe/izin (e.g., drwxrwxrwx untuk direktori), diikuti link count, owner, group, ukuran byte, tanggal. `cat` tampilkan teks mentah; jika binary, output acak. `chmod` konfirmasi diam, verifikasi ulang dengan `ls -l`.

## Struktur Laporan
Sertakan judul praktikum, tujuan, alat (Linux terminal), langkah prosedur dengan screenshot output, analisis hasil, dan kesimpulan. Gunakan format: Pendahuluan, Landasan Teori, Rancangan Percobaan, Hasil dan Pembahasan, Kesimpulan.

## Unggah ke Git
Inisialisasi `git init`, `git add .`, `git commit -m "Laporan Praktikum 1"`, `git remote add origin URL`, `git push origin main`. Dokumentasikan dengan README.md dan screenshot; push sebelum deadline untuk histori tepat waktu.

## Dasar Teori
Tuliskan ringkasan teori (3–5 poin) yang mendasari percobaan.
Linux menggunakan hierarki direktori berbasis tree structure dengan root `/` sebagai titik awal, memungkinkan navigasi efisien melalui perintah seperti `pwd`, `cd`, dan `ls`. Sistem file seperti ext4 mengandalkan inode untuk menyimpan metadata file (ukuran, izin, owner) terpisah dari data aktual, mendukung operasi cepat tanpa duplikasi. Permission diatur dalam model owner-group-others dengan tiga hak (read=4, write=2, execute=1) menggunakan oktal atau simbolik via `chmod`, sementara `chown` mengubah kepemilikan untuk keamanan multi-user.

## Ringkasan Teori
- **Struktur Hierarki**: Semua file dan direktori terorganisir dalam pohon dari root `/`, dengan pathname absolut (mulai `/home/user`) atau relatif untuk akses cepat.
- **Inode System**: Setiap file memiliki inode unik yang menyimpan metadata seperti izin akses, timestamp, dan pointer blok data, memisahkan nama file dari konten fisik.
- **Permission Model**: Terdiri rwx untuk user/group/others (rwxr-xr-x), dicegah akses tidak sah; `chmod` ubah bit izin, `chown` atur owner/group dengan sudo.
- **Operasi File Dasar**: Create/read/update/delete via `cat`, `cp`, `mv`, `rm`; Virtual File System (VFS) Linux dukung multiple filesystem seperti ext4 untuk portabilitas.
- **Optimasi Akses**: Blok alokasi (1-4KB) dan caching kurangi I/O disk, dengan superblock dan bitmap kelola ruang bebas secara efisien.

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


---

## Analisis
1.Dokumentasikan hasil seluruh perintah pada tabel observasi di laporan.md.
2.Jelaskan fungsi tiap perintah dan arti kolom permission (rwxr-xr--).
3.Analisis peran chmod dan chown dalam keamanan sistem Linux.
4.Upload hasil dan laporan ke repositori Git sebelum deadline.
Linux menyediakan perintah dasar untuk navigasi file serta manajemen permission yang krusial untuk keamanan sistem. Dokumentasi hasil dalam tabel observasi memudahkan analisis output, sementara `chmod` dan `chown` mencegah akses tidak sah. Unggah ke Git memastikan dokumentasi ter-versioning tepat waktu.

1.## Tabel Observasi Perintah
Berikut tabel hasil eksekusi perintah utama (contoh output simulasi; sesuaikan dengan terminal Anda):

| Perintah          | Fungsi                          | Output Contoh                          | Penjelasan Output
|-------------------|---------------------------------|----------------------------------------|-----------------------------------|
| `pwd`            | Tampilkan direktori kerja saat ini | `/home/user/docs`                     | Path absolut dari lokasi terminal. |
| `ls -l`          | Daftar file dengan detail       | `-rwxr-xr-- 1 user group 1024 Jan 14 file.txt` | Detail: tipe/izin, link, owner, group, ukuran, tanggal, nama. |
| `cd /tmp`        | Pindah ke direktori `/tmp`      | (Tidak ada output, hanya ubah lokasi) | Gunakan `pwd` untuk verifikasi. |
| `cat file.txt`   | Tampilkan isi file              | `Isi teks file...`                    | Teks mentah; error jika binary. |
| `chmod 755 file.txt` | Ubah izin ke rwxr-xr-x       | (Diam); verifikasi: `ls -l`           | Owner: rwx (7), group/others: rx (5). |
| `chown user:group file.txt` | Ubah owner/group         | (Diam); verifikasi: `ls -l`           | Ganti nama user/group dengan sudo. |

2.## Arti Permission (rwxr-xr--)
Notasi `rwxr-xr--` terdiri 10 karakter: posisi 1 (tipe: `-` file, `d` direktori), 2-4 (owner: rwx=read/write/execute), 5-7 (group: r-x=read/execute), 8-10 (others: r--=read only). Setara oktal 754; nol berarti tidak ada akses. Ini cegah eksekusi oleh group/others untuk keamanan.

3.## Analisis chmod dan chown
`chmod` mengatur bit akses (r=4,w=2,x=1) untuk mencegah modifikasi tidak sah, vital di multi-user seperti server. `chown` memastikan hanya owner/group sah mengakses, cegah privilege escalation; gunakan `-R` hati-hati untuk direktori. Keduanya enforcement DAC (Discretionary Access Control) Linux, tingkatkan isolasi proses.

4.## Upload ke Git

## Kesimpulan
Tuliskan 2–3 poin kesimpulan dari praktikum ini.
Praktikum manajemen file dan permission di Linux menunjukkan perintah dasar seperti `ls`, `pwd`, `cd`, dan `cat` efektif untuk navigasi sistem file hierarkis. Pemahaman `chmod` dan `chown` krusial untuk mengamankan akses berbasis owner-group-others.

## Kesimpulan Praktikum
- Perintah navigasi memungkinkan eksplorasi efisien tree structure Linux dari root `/`, sementara permission model (rwx oktal) dan inode metadata menjamin integritas data di lingkungan multi-user.
- `chmod` dan `chown` berperan sentral dalam keamanan DAC Linux, mencegah akses tidak sah melalui kontrol granular yang diverifikasi via `ls -l`, esensial untuk server produksi.
- Dokumentasi tabel observasi dan versioning Git memastikan hasil praktikum terlacak, mendukung reproduktibilitas eksperimen sistem operasi.


## Quiz
1. [Apa fungsi dari perintah chmod?]  
   **Jawaban:**  Perintah chmod berfungsi untuk mengubah mode izin akses (permissions) pada file atau direktori di Linux, seperti read (r), write (w), dan execute (x) untuk owner, group, dan others.

2. [Apa arti dari kode permission rwxr-xr--?]  
   **Jawaban:** Kode rwxr-xr-- terdiri dari 10 karakter: karakter pertama menunjukkan tipe (misalnya - untuk file biasa), posisi 2-4 (rwx) untuk owner (read/write/execute), 5-7 (r-x) untuk group (read dan execute, tanpa write), serta 8-10 (r--) untuk others (hanya read). Setara oktal 754, artinya owner punya akses penuh, group bisa baca/jalankan, others hanya baca. 
3. [Jelaskan perbedaaan antara chown dan chmod?]  
   **Jawaban:**  chmod mengubah izin akses (rwx) tanpa memengaruhi kepemilikan, sementara chown mengubah owner dan/atau group file (misalnya chown user:group file.txt), biasanya butuh sudo. chmod fokus keamanan akses, chown ke kepemilikan identitas.
​



---

## Refleksi Diri
Tuliskan secara singkat:
- Apa bagian yang paling menantang minggu ini?  
- Bagaimana cara Anda mengatasinya?  

---

**Credit:**  
_Template laporan praktikum Sistem Operasi (SO-202501) – Universitas Putra Bangsa_
