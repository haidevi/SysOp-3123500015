<div align="center">
  <h1 style="text-align: center;font-weight: bold">Praktikum 4B<br>Praktek Sistem Operasi</h1>
  <h4 style="text-align: center;">Dosen Pengampu : Dr. Ferry Astika Saputra, S.T., M.Sc.</h4>
</div>
<br />
<div align="center">
  <img src="../assets/week-1/logo_pens.png" alt="Logo PENS">
  <h3 style="text-align: center;">Disusun Oleh :</h3>
  <p style="text-align: center;">
    <strong>Calvin Raditya Sandy Winarto (3123500009)</strong><br>
    <strong>Zada Devi Mariama (3123500015)</strong>
  </p>

<h3 style="text-align: center;line-height: 1.5">Politeknik Elektronika Negeri Surabaya<br>Departemen Teknik Informatika Dan Komputer<br>Program Studi Teknik Informatika<br>2024/2025</h3>
  <hr>
</div>

# Proses dan Manajemen Proses

## POKOK BAHASAN:

* Proses pada Sistem Operasi Linux
* Manajemen Proses pada Sistem Operasi Linux

## TUJUAN PEMBELAJARAN:
Setelah mempelajari materi dalam bab ini, mahasiswa diharapkan mampu:
* Memahami konsep proses pada sistem operasi Linux.
* Menampilkan beberapa cara menampilkan hubungan proses parent dan child.
* Menampilkan status proses dengan beberapa format berbeda.
* Melakukan pengontrolan proses pada shell.
* Memahami penjadwalan prioritas.

  
## DASAR TEORI:

### 1. KONSEP PROSES PADA SISTEM OPERASI LINUX
Proses adalah program yang sedang dieksekusi. Setiap kali menggunakan utilitas sistem atau program aplikasi dari shell, satu atau lebih proses "child" akan dibuat oleh shell sesuai perintah yang diberikan. Setiap kali instruksi diberikan pada Linux shell, maka kernel akan menciptakan sebuah proses-id. Proses ini disebut juga dengan terminology Unix sebagai sebuah Job. Proses Id (PID) dimulai dari 0, yaitu proses INIT, kemudian diikuti oleh proses berikutnya (terdaftar pada /etc/inittab).
Beberapa tipe proses :
##### a. Foreground
   Proses yang diciptakan oleh pemakai langsung pada terminal (interaktif,dialog).
##### b. Batch
   Proses yang dikumpulkan dan dijalankan secara sekuensial (satu persatu). Prose Batch tidak diasosiasikan (berinteraksi) dengan terminal.
#### c. Daemon
Proses yang menunggu permintaan (request) dari proses lainnya dan menjalankan tugas sesuai dengan permintaan tersebut. Bila tidak ada request, maka program ini akan berada dalam kondisi "idle" dan tidak menggunakan waktu hitung CPU.
Umumnya nama proses daemon di UNIX berakhiran d, misalnya inetd, named, popd dil

### 2. SINYAL
Proses dapat mengirim dan menerima sinyal dari dan ke proses lainnya. Proses mengirim sinyal melalui instruksi "kill" dengan format ``kill [-nomor sinyal] PID``

Nomor sinyal: 1 s/d maksimum nomor sinyal yang didefinisikan system
Standar nomor sinyal yang terpenting adalah :
| No Sinyal | Nama | Deskripsi |
| --- | --- | --- |
| 1 | SIGHUP | Hangup, sinyal dikirim bila proses terputus, misalnya melalui putusnya hubungan modem.
| 2 | SIGINT | Sinyal interrupt, melalui ^C
| 3 | SIGQUIT | Sinyal Quit, melalui ^\
| 9 | SIGKILL | Sinyal Kill, menghentikan proses
| 15 | SIGTERM | Sinyal terminasi software 

### 3. MENGIRIM SINYAL
Mengirim sinyal adalah satu alat komunikasi antar proses, yaitu memberitahukan proses yang sedang berjalan bahwa ada sesuatu yang harus dikendalikan. Berdasarkan sinyal yang dikirim ini maka proses dapat bereaksi dan administrator/programmer dapat menentukan reaksi tersebut. Mengirim sinyal menggunakan instruksi
``kill I-nomor sinyall PID``
Sebelum mengirim sinyal PID proses yang akan dikirim harus diketahui terlebih dahulu.

### 4. MENGONTROL PROSES PADA shell
Shell menyediakan fasilitas job control yang memungkinkan mengontrol beberapa job atau proses yang sedang berjalan pada waktu yang sama. Misanya bila melakukan pengeditan file teks dan ingin melakukan interrupt pengeditan untuk mengerjakan hal lainnya. Bila selesai, dapat kembali (switch) ke editor dan melakukan pengeditan file teks kembali.
Job bekerja pada foreground atau background. Pada foreground hanya diper untukkan untuk satu job pada satu waktu. Job pada foreground akan mengontrol shell - menerima input dari keyboard dan mengirim output ke layar. Job pada background tidak menerima input dari terminal, biasanya berjalan tanpa memerlukan interaksi.
Job pada foreground kemungkinan dihentikan sementara (suspend), dengan menekan [Ctr-Z]. Job yang dihentikan sementara dapat dijalankan kembali pada foreground atau background sesuai keperluan dengan menekan "fg" atau "bg". Sebagai catatan, menghentikan job sementara sangat berbeda dengan melakuakan interrupt job (biasanya menggunakan [CtrIC]), dimana job yang diinterrup akan dimatikan secara permanen dan tidak dapat dijalankan lagi.

### 5. MENGONTROL PROSES LAIN 
Perintah ps dapat digunakan untuk menunjukkan semua proses yang sedang berjalan pada mesin (bukan hanya proses pada shell saat ini) dengan format :
``ps -fae`` atau ``ps -aux``
Beberapa versi UNIX mempunyai utilitas sistem yang disebut ``top`` yang menyediakan cara interaktif untuk memonitor aktifitas sistem. Statistik secara detail dengan proses yang berjalan ditampilkan dan secara terus-menerus di refresh. Proses ditampilkan secara terurut dari utilitas CPU. Kunci yang berguna pada ``top`` adalah

``s``- set update frequency
``u`` - display proses dari satu user
``k`` - kill proses (dengan PID)
``a``- quit

Utilitas untuk melakukan pengontrolan proses dapat ditemukan pada sistem
UNIX adalah perintah ``killall``. Perintah ini akan menghentikan proses sesuai PID atau job number proses.

## Percobaan 5: Menghentikan dan memulai kembali job
1. Cara lain meletakkan job pada background dengan memulai job secara normal (pada foreground), stop job dan memulai lagi pada background
   ```
   & yes > /dev/null
   ```
   ![img](../assets/week-6/1.png)

   Hentikan sementara job (suspend), bukan menghentikannya (terminate), tetapi menghentikan sementara job sampai di restart. Untuk menghentikan sementara job gunakan Ctri-Z.
2. Untuk restart job pada foreground, gunakan perintah £ g.
   ```
   $ fg
   ```
   ![img](../assets/week-6/2.png)
3. Shell akan menampilkan nama perintah yang diletakkan di foreground. Stop job lagi dengan Ctri-Z. Kemudian gunakan perintah bg untuk meletakkan job pada background.
   ```
   $ bg
   ```
   ![img](../assets/week-6/3.1.png)

   Job tidak bisa dihentikan dengan Ctrl-Z karena job berada pada background.
   Untuk menghentikannya, letakkan job pada foreground dengan fg dan kemudian hentikan sementara dengan CtrI-Z.
   ```
   $ fg
   ```

   ![img](../assets/week-6/3.2.png)
4. Job pada background dapat digunakan untuk menampilkan teks pada terminal, dimana dapat diabaikan jika mencoba mengerjakan job lain.
   ```
   $ yes &
   ```
   ![img](../assets/week-6/4.png)

   Untuk menghentikannya tidak dapat menggunakan CtrI-C. Job harus dipindah ke foreground, baru dihentikan dengan cara tekan fg dan tekan Enter, kemudian dilanjutkan dengan Ctrl-Z untuk menghentikan sementara.
5. Apabila ingin menjalankan banyak job dalam satu waktu, letakkan job pada foreground atau background dengan memberikan job ID
   ```
   $ fg %2      atau $ %2
   $ bg %2
   ```
   ![img](../assets/week-6/5.png)
6. Tekan fg dan tekan Enter, kemudian dilanjutkan dengan Ctrl-Z untuk menghentikan sementara.
   ![img](../assets/week-6/6.png)
7. Lihat job dengan perintah ps -fae dan tekan Enter. Kemudian hentikan proses dengan perintah kill.
   ```
   $ ps -fae
   $ kill -9 <Nomor PID>
   ```
   ![img](../assets/week-6/7.1.png)

   ![img](../assets/week-6/7.2.png)

   ![img](../assets/week-6/7.3.png)
8. Logout dan tekan Alt+F7 untuk kembali ke mode grafis
   

## Percobaan 6: Percobann dengan Penjadwalan Prioritas 
1. Login sebagai root.

2. Buka 3 terminal, tampilkan pada screen yang sama.
    ![img](../assets/week-6/8.png)
3. Pada setiap terminal, ketik PS1 = "\w:" diikuti Enter. \w menampilkan path pada direktori home.
    ![img](../assets/week-6/9.png)
4. Karena login sebagai root, maka akan ditampilkan ~: pada setiap terminal.
   Untuk setiap terminal ketik pwd dan tekan Enter untuk melihat bahwa Anda sedang berada pada direktori / root.
     ![img](../assets/week-6/10.png)
5. Buka terminal lagi (keempat), atur posisi sehingga keempat terminal terlihat pada screen.
     ![img](../assets/week-6/11.png)
6. Pada terminal keempat, ketik top dan tekan Enter. Maka program top akan muncul. Ketik i. Top akan menampilkan proses yang aktif. Ketik Imt.
   Top tidak lagi menampilkan informasi pada bagian atas dari screen. Pada percobaan ini, terminal ke empat sebagai jendela Top.
     ![img](../assets/week-6/12.1.png)
     
     Ketik ``lmt``

     ![img](../assets/week-6/12.2.png)
7. Pada terminal 1, bukalah program executable C++ dengan mengetik program yes dan tekan Enter.
8. Ulangi langkah 7 untuk terminal 2.
     ![img](../assets/week-6/13.png)
9. Jendela Top akan menampilkan dua program yes sebagai proses yang berjalan. Nilai %CPU sama pada keduanya. Hal ini berarti kedua proses mengonsumsi waktu proses yang sama dan berjalan sama cepat. PID dari kedua proses akan berbeda, misalnya 3020 dan 3022. Kemudian gunakan terminal 3 (yang tidak menjalankan primes maupun Jendela Top) dan ketik renice 19 < PID terimnal 1> (contoh: renice 19 3020) dan diikuti Enter.
    Hal ini berarti mengganti penjadwalan prioritas dari proses ke 19.
     ![img](../assets/week-6/14.png)
10. Tunggu beberapa saat sampai program top berubah dan terlihat pada jendela Top. Pada kolom STAT memperlihatkan N untuk proses 3148. Hal ini berarti bahwa penjadwalan prioritas untuk proses 3148 lebih besar (lebih lambat) dari 0. Proses 3149 berjalan lebih cepat.
     ![img](../assets/week-6/15.png)
11. Program top juga mempunyai fungsi yang sama dengan program renice. Pilih Jendela Top dan tekan r. Program top terdapat prompt PID to renice: tekan 3020 (ingat bahwa Anda harus mengganti 3020 dengan PID Anda sendiri) dan tekan Enter. Program top memberikan prompt Renice PID 3020 to value: tekan -19 dan tekan Enter.
     
     ![img](../assets/week-6/16.1.png)

     ![img](../assets/week-6/16.2.png)
12. Tunggu beberapa saat sampai top berubah dan lihat nilai %CPU pada kedua proses. Sekarang proses 3020 lebih cepat dari proses 3022. Kolom status menunjukkan < pada proses 3020 yang menunjukkan penjadwalan prioritas lebih rendah (lebih cepat) dari nilai 0.
     ![img](../assets/week-6/17.png)
13. Pilih terminal 3 (yang sedang tidak menjalankan yes atau program top) dan ketik nice -n -10 yes dan tekan Enter. Tunggu beberapa saat agar program top berubah dan akan terlihat proses primes ketiga. Misalnya PID nya 4107. Opsi -10 berada pada kolom NI (penjadwalan prioritas).
     ![img](../assets/week-6/18.1.png)

     ![img](../assets/week-6/18.2.png)
14. Jangan menggunakan mouse dan keyboard selama 10 detik. Program top menampilkan proses yang aktif selain program yes. Maka akan terlihat proses top terdaftar tetapi %CPU kecil (dibawah 1.0) dan konsisten. Juga terlihat proses berhubungan dengan dekstop grafis seperti X, panel dll.
     ![img](../assets/week-6/19.png)
15. Pindahkan mouse sehingga kursor berubah pada screen dan lihat apa yang terjadi dengan tampilan top. Proses tambahan akan muncul dan nilai %CPU berubah sebagai bagian grafis yang bekerja. Satu alasan adalah bahwa proses 4107 berjalan pada penjadwalan prioritas tinggi. Pilih jendela Top, ketik r. PID to renice: muncul prompt. Ketik 4107 (ubahlah 4107 dengan PID Anda) dan tekan Enter. Renice PID 4107 to value: muncul prompt. Ketik 0 dan tekan Enter: Sekarang pindahkan mouse ke sekeliling screen. Lihat perubahannya.
     ![img](../assets/week-6/20.1.png)

     ![img](../assets/week-6/20.2.png)

     ![img](../assets/week-6/20.3.png)

     ![img](../assets/week-6/20.4.png)
16. Tutup semua terminal window.
17. Logout dan login kembali sebagai user.

### LATIHAN: 
1. Masuk ke tty2 dengan **CtrI+Alt+F2**. Ketik **ps -au** dan tekan **Enter**. Kemudian perhatikan keluaran sebagai berikut :
      ![img](../assets/week-6/21.png)
   a. Sebutkan nama-nama proses yang bukan root
      
      * Dari keseluruhan proses hanya proses ``/bin/login -p--``

   b. Tulis PID dan COMMAND dari proses yang paling banyak menggunakan CPU time
      * PID = 5858
      * COMMAND = bash

   c. Sebutkan buyut proses dan PID dari proses tersebut
      * ``/usr/libexec/gdm-wayland-session``
      * PID = 4508

   d. Sebutkan beberapa proses daemon
      * Tidak terdapat proses daemon

   e. Pada prompt login lakukan hat hal sebagai berikut :
      ```
      $ csh
      $ who 
      $ bash
      $ 1s 
      $ sh 
      $ ps
      ```

      ![img](../assets/week-6/22.png)

      * Perintah  ``csh`` menampilkan Shell interaktif yang memiliki sintaks yang lebih banyak dibandingkan bourne shell.
        Perintah ``who`` digunakan untuk menampilkan informasi tentang pengguna yang saat ini login ke sistem.
        Perintah ``ls`` digunakan untuk menampilkan daftar file dan direktori dalam direktori saat ini.
        Perintah ``bash`` Shell interpreter bahasa sh untuk mengeksekusi perintah yang dibaca dari standart input atau dari sebuah file.
        Perintah ``ps`` menampilkan proses yang sedang berjalan di komputer

   f. Sebutkan PID yang paling besar dan kemudian buat urut-urutan proses sampai ke PPID = 1. 
      
      ![img](../assets/week-6/23.png) 

      * PID = 6323 (ps)
      * PID = 6322 (sh)
      * PID = 6318 (bash)
      * PID = 6316 (csh)
      * PID = 6092 (bash)
2. Cobalah format tampilan ps dengan opsi berikut dan perhatikan hasil tampilannya :
   * -f  daftar penuh
      
      ![img](../assets/week-6/24.1.png)

      Opsi -f : menampilkan output dalam format yang lengkap.

   * -j  format job
      
      ![img](../assets/week-6/24.2.png)

      Opsi -j : menampilkan informasi job control untuk setiap proses.

   *  j  format job control
      
      ![img](../assets/week-6/24.3.png)

   *  l  daftar memanjang 
         
      ![img](../assets/week-6/24.4.png)

      Opsi l : menampilkan output yang lebih luas.

   *  s  format sinyal 
        
      ![img](../assets/week-6/24.6.png)

      Opsi s : menampilkan informasi tentang sinyal yang dikirimkan kepada proses.

   *  v  format virtual memory
         
      ![img](../assets/week-6/24.7.png)

      Opsi v : menampilkan informasi tentang penggunaan memori virtual oleh setiap proses.

   *  X  format register i386
         
      ![img](../assets/week-6/24.8.png)

      Opsi X : menampilkan semua proses termasuk proses yang tidak terkait dengan terminal.

2. Lakukan urutan pekerjaan berikut :

   a. Gunakan perintah ``find`` ke seluruh direktory pada sistem, belokkan output sehingga daftar direktori dialihkan ke file directories. txt dan daftar pesan error dialihkan ke file errors.txt
      ![img](../assets/week-6/25.png)
   
   b. Gunakan perintah ``sleep 5``. Apa yang terjadi dengan perintah ini ?
      ![img](../assets/week-6/26.png)
   
   c. Jalankan perintah pada background menggunakan &
      ![img](../assets/week-6/27.png)
   
   d. Jalankan ``sleep 15`` pada foreground, hentikan sementara dengan Ctrl-Z dan kemudian letakkan pada background dengan ``bg``. Ketikkan ``jobs``. Ketikkan ``ps``. Kembalikan job ke foreground dengan perintah ``fg``.
      ![img](../assets/week-6/28.png)
  
   e. Jalankan ``sleep 15`` pada background menggunakan & dan kemudian gunakan perintah ``kill`` untuk menghentikan proses diikuti job number.
      ![img](../assets/week-6/29.png)
   
   f. Jalankan ``sleep 15`` pada background menggunakan & dan kemudian gunakan ``kill`` untuk menghentikan sementara proses. Gunakan bg untuk melanjutkan menjalankan proses.
      ![img](../assets/week-6/30.png)
   
   g. Jalankan ``sleep 60`` pada background 5 kali dan terminasi semua pada dengan menggunakan perintah ``killall``.
      ![img](../assets/week-6/31.png)
   
   h. Gunakan perintah ``ps``, ``w`` dan ``top`` untuk menunjukkan semua proses yang sedang dieksekusi.
      ![img](../assets/week-6/32.1.png)

      ![img](../assets/week-6/32.2.png)
   
   i. Gunakan perintah ``ps -aeH`` untuk menampilkan hierarki proses. Carilah init proses. Apakah Anda bisa identifikasi sistem daemon yang penting ?
      Dapatkan Anda identifikasi shell dan subproses ?
      ![img](../assets/week-6/33.1.png)

      ![img](../assets/week-6/33.2.png)
      
      ![img](../assets/week-6/33.3.png)
   
   j. Kombinasikan ``ps -fae`` dan ``grep``, apa yang Anda lihat ?
      ![img](../assets/week-6/34.png)

      Perintah ps -fae : untuk menampilkan semua proses yang sedangberjalan pada mesin.
      Penggabungan dengan grep, meanampilkan proses yang dicari dengan menggunakan grep.
   
   k. Jalankan proses ``sleep 300`` pada background. Log off komputer dan log in kembali. Lihat daftar semua proses yang berjalan. Apa yang terjadi pada proses sleep ?
      ![img](../assets/week-6/35.1.png)

      ![img](../assets/week-6/35.2.png)

      Proses sleep tidak dalam kondisi running.

### Apa bedanya interrupt dengan system call dalam diagram state.

1. Interrupt : Interrupt terjadi pada waktu yang tidak terduga, seperti saat perangkat keras membutuhkan perhatian kernel. Ketika interrupt terjadi, CPU akan menghentikan eksekusi instruksi yang sedang berlangsung dan beralih ke penanganan interrupt oleh kernel. Setelah interrupt terjadi, CPU beralih ke mode kernel dan menjalankan penanganan interrupt yang sesuai. Hal ini bisa berupa penanganan interrupt dari perangkat keras (seperti interrupt dari keyboard atau mouse) atau interrupt yang dihasilkan oleh kondisi tertentu yang memerlukan intervensi kernel.

2. System Call : Fungsi yang memungkinkan suatu proses untuk berkomunikasi dengan kernel Linux. System call terjadi  dipicu oleh instruksi dalam program pengguna (user program). Saat sebuah program pengguna membutuhkan akses ke layanan yang dikelola oleh kernel (seperti membaca atau menulis file), ia memanggil fungsi sistem yang sesuai. System call dipanggil, CPU beralih ke mode kernel untuk menjalankan kode kernel yang terkait dengan system call tersebut. Dalam diagram state, system call sering ditampilkan sebagai transisi dari mode pengguna (user mode) ke mode kernel.
   

