<div align="center">
  <h1 style="text-align: center;font-weight: bold">Tugas UTS<br>Praktek Sistem Operasi</h1>
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

# Process - Fork - Multithread
Setiap program atau bagian dari program yang sedang dieksekusi oleh CPU disebut dengan proses. Proses dapat berjalan secara _foreground_ atau _background_. 

Untuk melihat seluruh proses yang sedang berjalan gunakan perintah `$ ps -e` .
Bisa juga menggunakan perintah `$pstree | more` untuk melihat secara detil proses yang sefan berjalan dengan format **tree**.

Setiap proses akan memilik **PID**  (Process ID). Apabila dibutuhkan Sebuah proses bisa memiliki proses anakan. Dalam hubungan tersebut proses dapat diibaratkan seperti orang tua (_parent_) dengan anak (_child_) yang turun temurun.
- Setiap proses memiliki parent dan child.
- Setiap proses memiliki ID (_pid_) dan parent ID (_ppid_), kecuali proses `init` atau `systemd`.
- _ppid_ dari sebuah proses adalah ID dari parent proses tersebut. 

```mermaid
classDiagram
      Parent_Process --|> Child_Process
      Parent_Process : PID =4900
      Parent_Process : PPID = 4
      Parent_Process: bash
      class Child_Process{
          PID=4901
          PPID = 4900
          fork01
      }
   ```

   Perhatikan, ppid dari proses `fork01` adalah pid dari proses `bash`.

**fork** digunakan untuk menduplikasi proses. Proses yang baru disebut dengan child proses, sedangkan proses pemanggil disebut dengan parent proses. Spesifikasi fork bisa dilihat dengan `$ man 2 fork`. 
```
int main() { 
                            pid: 2308, ppid: 10 
                             [Main process]
                                 |
  fork();              > Child process created <
                                 +
                               /   \
                             /       \
               pid: 2308, ppid: 10    pid: 30, ppid: 2308
                [Parent Process]    [Child Process]

  return 0;
}
```
perhatikan bahwa :
- `pid` Parent Process == `ppid` Child
- `child_id` Parent Process == `pid` Child Process

**Exec** adalah function yang digunakan untuk menjalankan program baru dan mengganti program yang sedang berlangsung. `exec` adalah program family yang memiliki berbagai fungsi variasi, yaitu `execvp`, `execlp`, `execv`, dan lain lain.

**wait** adalah function yang digunakan untuk mendapatkan informasi ketika child proses berganti _state_-nya. Pergantian state dapat berupa _termination_, _resume_, atau _stop_.

Manual: `$ man 3 exec`

## 1. Fork : Parent - Child Process
* Buat tulisan tentang konsep fork dan implementasinya dengan menggunakan bahasa pemrograman C! (minimal 2 paragraf disertai dengan gambar)
  
  - Fork, pemanggilan sistem untuk membuat proses baru. Fork membuat proses baru dengan menduplikasi proses yang dipanggil. Proses baru ini disebut juga sebagai proses anak (child process). Proses pemanggil yang membuat proses baru disebut sebagai proses induk (parent process). Proses baru (child process) adalah salinan persis dari proses pemanggil (parent process).
    
    Child process dan parent process berjalan di ruang memori yang terpisah. Pada saat fork, kedua ruang memori tersebut memiliki isi yang sama. Penulisan memori, pemetaan file, dan penghapusan pemetaan yang dilakukan oleh salah satu proses tidak mempengaruhi proses lainnya.

     ![img](../assets/week-7/fork.jpeg)

     ![img](../assets/week-7/tgs1.png)

* Akses dan clonning repo : https://github.com/ferryastika/operatingsystem.git

* Deskripsikan dan visualisasikan pohon proses hasil eksekusi dari kode program ``fork01.c``, ``fork02.c``, ``fork03.c``, ``fork04.c``, ``fork05.c``dan ``fork06.c``.

  1. ``fork01.c``

![img](../assets/week-7/1.png)

 ```
       int main() {
  for(int i = 0;i < 3; i++){
    PID : 36288, PPID : 35057, uid : 1000
              [Main Process]
                    |
                sleep(3)
                    |
    PID : 36288, PPID : 35057, uid : 1000
              [Main Process]
                    |
                sleep(3)
                    |
    PID : 36288, PPID : 35057, uid : 1000
              [Main Process]
                    |
                sleep(3)
       }
    }
 ```
Output dari program menunjukkan bahwa proses tersebut (disebut proses "I am process 4010") memiliki ID 4010 dan parent process memiliki ID 3888. Selain itu, proses ini dimiliki oleh pengguna dengan ID 1000. Dari output tersebut, dapat disimpulkan bahwa program ini menggunakan sistem panggilan seperti `getpid()`, `getppid()`, dan `getuid()` untuk mendapatkan informasi tentang proses yang sedang berjalan, parent process, dan pemiliknya. Program kemudian menunggu selama 3 detik sebelum mengulangi proses tersebut.

  2. ``fork02.c``

 ![img](../assets/week-7/2.png)

```
 int main() {
  fork();         > Child process created <
                              +
                            /   \
                          /       \
                        /           \
  while(1){           /               \
          PID: 36476, PPID: -       PID: 36477, PPID: 36476
          [Parent Process]          [Child Process]
                      \               /
                        \           /
                          \       /
                            \   /
                              |
                          sleep(2)
                              |
                             x++
  }
}
```
Dari output program, terlihat bahwa child process memiliki PID yang berbeda dari parent process, dan keduanya berjalan secara bersamaan. Kedua proses terus mencetak nilai `x` yang meningkat. Karena tidak ada kondisi berhenti dalam loop, program akan berjalan tanpa henti sampai dihentikan secara paksa.

  3. ``fork03.c``

![img](../assets/week-7/3.png)

```
int main() {
      fork();         > Child Process created <
                              +
                            /   \
                          /       \
  x=5;                  /           \
  while(x<=5){        /               \
          PID: 36325, PPID: -      PID: 36326, PPID: 36325
          [Parent Process]          [Child Process]
                      \               /
                        \           /
                          \       /
                            \   /
                              |
                          sleep(2)
                              |
                            x++
  }
}
```
Output program menunjukkan bahwa terdapat dua proses yang berjalan: parent proces dan child process. Child process memiliki PID yang berbeda dari parent process, namun keduanya melakukan hal yang sama, yaitu mencetak PID mereka sendiri secara bergantian setiap 2 detik.
  
  4. ``fork04.c``

![img](../assets/week-7/4.png)

```
int main() {
  fork();               > Child process created <
                                    +
                                  /   \
                                 /     \
                                /       \
                               /         \
                    PID: 36382, PPID: -   \
                    [Parent Process]       \
                            |               \
                            |                \
                            |        PID: 36383, PPID: 36382
                          wait         [Child Process]
                            \                /
                              \            /
                                \        /
                                  \    /
                                    \/
                                   exit
}
```
Output dari program menunjukkan bahwa parent process pertama kali berjalan, mencetak PID-nya sendiri dan PID anaknya. Kemudian, child process berjalan, mencetak PID-nya sendiri dan PID pparent process. Program ini mengilustrasikan cara menggunakan `fork()` untuk menciptakan proses baru, serta cara parent process dan child process berinteraksi dan menunggu satu sama lain untuk menyelesaikan tugasnya.

  5. ``fork05.c``

![img](../assets/week-7/5.png)

```
int main() {
  fork();                > Child process created <
                                    +
                                  /   \
                                /       \
                PID : 36394 PPID : -      \
                 [Parent Process]           \
                        |                     \
                        |                       \
                        |               PID : 36395 PPID : 36394
                      wait                    execl(/bin/ls)
                        \                     [Child Process]
                          \                       /
                            \                   /
                              \               /
                                \           /
                                  \       /
                                    \   /
                                      |
                                    exit
}
```
Output program menunjukkan bahwa parent process mencetak PID-nya sendiri dan PID child process. Kemudian, child process menjalankan perintah ls -l /home, yang mencetak daftar isi dari direktori /home dengan detail. Setelah itu, parent process mencetak pesan bahwa dia akan menunggu child process. Setelah child process selesai, parent process keluar dan mencetak pesan bahwa dia juga akan berhenti.

  6. ``fork06.c``

![img](../assets/week-7/6.png)

```
int main() {
  fork();           > Child process created <
                                +
                              /   \
                            /      \
              PID: 36440, PPID: -   \
              [Parent Process]       \
                      |               \
                      |          PID: 36441, PPID: 36440
                    wait          [Child Process]
                      \               /
                        \           /
                          \       /
                            \   /
                              |
                            exit
}
```
Output program menunjukkan bahwa parent process mencetak PID-nya sendiri dan PID child process. Kemudian, child process mencetak PID-nya sendiri. Namun, setelah itu, pesan "Could not execl file fork3" dicetak, yang menunjukkan bahwa execl() gagal. Kemudian, parent process mencetak pesan bahwa dia akan menunggu child proces. Setelah child process selesai, parent process keluar dan mencetak pesan bahwa dia juga akan berhenti.

## 2. Tugas

Buatlah program perkalian 2 matriks [4 x 4] dalam bahasa C yang memanfaatkan ``fork()``.

![img](../assets/week-7/mtrx.png)

![img](../assets/week-7/mtrx2.png)

Program dengan bahasa C diatas melakukan perkalian matriks dengan skalar menggunakan child process dan parent process dengan menggunakan fungsi `fork()`. Program menginisialisasi sebuah matriks dengan ukuran `ROWS`x`COLS` yang diisi dengan nilai-nilai yang merupakan hasil dari perkalian baris dan kolom matriks. Program melakukan proses fork untuk menciptakan child process. Jika child process berhasil dibuat (`pid == 0`), maka child process akan mengalikan setiap elemen matriks dengan skalar yang telah ditentukan, dan kemudian mencetak matriks hasil perkalian. Jika parent process yang menjalankan program (`pid > 0`), maka parent process akan menunggu child process selesai menggunakan fungsi `wait(NULL)`, dan kemudian mencetak pesan bahwa parent process telah selesai. Jika proses fork gagal (`pid < 0`), program akan mencetak pesan kesalahan.