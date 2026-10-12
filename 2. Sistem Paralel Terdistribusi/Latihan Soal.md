# 🧪 Simulasi UTS IF3130/IF3230 Sistem Paralel dan Terdistribusi — Soal Baru
### 2 paket latihan berdasarkan pola soal UTS 2013–2026 (Bundel HMIF) dan slide kuliah 00–06a, lengkap dengan kunci & pembahasan

> **Cara memakai**
> - **Paket A – Esai** (8 soal, 100 poin, ±100 menit): formatnya meniru UTS 2024/2025 dan 2025/2026.
> - **Paket B – Pilihan Ganda** (35 soal, ±50 menit): formatnya meniru UTS 2020/2021 dan 2021/2022.
> - Kerjakan dulu **tanpa melihat kunci**. Kunci dan pembahasan ada di bagian akhir file.
> - Setiap pembahasan diberi tanda **🔁 Pola**: soal lama dan slide yang menjadi dasarnya.
> - Soal tentang **vektorisasi (slide 05b)** dan **hybrid MPI+OpenMP (slide 05c)** belum pernah keluar di bundel, tetapi slidenya baru (2025), sehingga berpeluang muncul.

---

## 📑 Daftar Isi
- [Kisi-kisi](#kisi-kisi)
- [Paket A – Esai](#paket-a--esai)
- [Paket B – Pilihan Ganda](#paket-b--pilihan-ganda)
- [Kunci & Pembahasan Paket A](#kunci--pembahasan-paket-a)
- [Kunci & Pembahasan Paket B](#kunci--pembahasan-paket-b)
- [Rekap Kunci Paket B & Penilaian](#rekap-kunci-paket-b--penilaian)

---

## Kisi-kisi

| Topik (slide) | Paket A | Paket B | Pola soal lama yang ditiru |
|---|---|---|---|
| Dasar: Amdahl, Gustafson, speedup, efisiensi, roofline (01, 02) | A1 | B4–B7 | 13/14 no.3, 16/17 no.1, 18/19 no.1, 24/25 Q1 |
| ILP, pipeline, multithreading (02) | A2 | B1–B3 | 13/14 no.1, 15/16 no.1, 20/21 no.11, 21/22 no.1 |
| Interkoneksi & cache coherence (02) | – | B8–B10 | 21/22 no.2–3, 25/26 no.1 |
| Pthreads (04) | A3 | B23–B24 | 18/19 no.2, 21/22 no.8, 24/25 Q2 |
| OpenMP (05) | A4 | B17–B22 | 15/16 no.4, 18/19 no.5–6, 20/21 no.1, 24/25 Q3, 25/26 no.2 |
| MPI (03) | A5 | B11–B16 | 13/14 no.4, 15/16 no.3, 16/17 no.3, 20/21 no.9–12, 25/26 no.3–4 |
| Hybrid MPI + OpenMP (05c) | A6 | B33–B34 | *baru (slide 2025)* |
| Vektorisasi (05b) | A7 | B31–B32 | *baru (slide 2025)* |
| CUDA / GPU (06a) | A8 | B25–B30, B35 | 16/17 no.4, 19/20 no.6–9, 21/22 no.13–17, 25/26 no.5 |

---

## Paket A – Esai

### A1. Dasar Sistem Paralel (12 poin)
Sebuah komputer memiliki **4 core** dengan **hyperthreading 2 thread/core**, frekuensi **2 GHz**. Setiap core mampu **1 FLOP/cycle** untuk operasi skalar dan memiliki **unit vektor 256-bit** (operasi *double precision*, 1 operasi per elemen per cycle). Kecepatan akses DRAM adalah **2 word/cycle**, dengan **1 word = 8 byte**.

a. Berapa bandwidth DRAM sistem tersebut (GB/s)?
b. Berapa kecepatan komputasi puncak **satu core** untuk operasi skalar (FLOP/s)?
c. Berapa kecepatan komputasi puncak **seluruh sistem** jika memakai unit vektor, semua core, dan hyperthreading (gunakan model perhitungan seperti grafik *Performance* di slide)?
d. Sebuah program memiliki *arithmetic intensity* 0,5 FLOP/byte dan seluruh datanya berada di DRAM. Berapa kecepatan komputasinya? Apakah program ini *memory bound* atau *compute bound*?
e. Berapa *arithmetic intensity* minimal agar program pada sistem ini menjadi *compute bound*?
f. Program lain dapat diparalelkan sempurna sebesar 85%. Berapa jumlah prosesor **minimal** agar speedup-nya **≥ 4**? Berapa speedup maksimum yang mungkin dicapai?

---

### A2. Instruction Level Parallelism (8 poin)
Sebuah prosesor memiliki pipeline **6 tahap**, setiap tahap membutuhkan **0,5 ns**. Prosesor juga mendukung **multiple issue sebanyak 2**. Sebuah program terdiri atas **1200 instruksi** yang saling independen.

a. Berapa waktu eksekusi jika prosesor **tidak** memakai pipeline (setiap instruksi menyelesaikan 6 tahap sebelum instruksi berikutnya dimulai)?
b. Berapa waktu eksekusi dengan pipeline **tanpa** multiple issue?
c. Berapa waktu eksekusi dengan pipeline **dan** multiple issue 2? Berapa CPI-nya?
d. Berapa speedup (c) terhadap (a)?
e. Pada kode `z = x + y; if (z > 0) w = x; else w = y;`, jelaskan apa yang dilakukan prosesor yang memakai *speculation* dan apa yang terjadi jika tebakannya salah.

---

### A3. Pthreads (12 poin)
a. Program Pthreads akan menjalankan perhitungan berikut. Setiap thread boleh mengeksekusi satu instruksi atau lebih. Berapa **jumlah thread minimum** agar paralelisme maksimum tercapai? Jelaskan pembagiannya.
```c
a = b + 1;      // S1
c = a * 2;      // S2
d = b - 4;      // S3
e = d + c;      // S4
f = b * b;      // S5
```
b. Berapa jumlah thread (termasuk main thread) yang ada selama potongan program berikut berjalan? Jelaskan.
```c
int main() {
    pthread_t t[3];
    for (int i = 0; i < 2; i++)
        fork();
    for (int i = 0; i < 3; i++)
        pthread_create(&t[i], NULL, work, NULL);
    for (int i = 0; i < 3; i++)
        pthread_join(t[i], NULL);
    return 0;
}
```
c. Setiap thread `my_rank` (0 … t−1) membangkitkan sebuah matriks `my_mat`. Program ingin menghitung **product = A₀ × A₁ × … × A₍t−1₎** (perkalian matriks tidak komutatif). Kode berikut memakai mutex:
```c
void* Thread_work(void* rank) {
    long my_rank = (long) rank;
    Matrix my_mat = Generate_matrix(my_rank);
    pthread_mutex_lock(&mutex);
    Multiply_matrix(product, my_mat);     // product = product × my_mat
    pthread_mutex_unlock(&mutex);
    return NULL;
}
```
Jelaskan mengapa hasilnya bisa salah walaupun tidak ada race condition, lalu perbaiki menggunakan **semaphore** (tuliskan nilai awal semaphore dan kodenya).

---

### A4. OpenMP (14 poin)
Diberikan program penghitung bilangan prima berikut. Fungsi `is_prime(n)` memeriksa pembagi dari 2 sampai √n, sehingga waktu eksekusinya **semakin lama untuk n yang besar** dan tidak sama untuk setiap n.
```c
int count = 0;
#pragma omp parallel for
for (int n = 2; n <= N; n++) {
    if (is_prime(n))
        count++;
}
printf("Jumlah prima = %d\n", count);
```
a. Identifikasi masalah pada kode di atas dan perbaiki.
b. Dengan penjadwalan default, mengapa speedup program ini kurang baik? Usulkan klausa `schedule` yang lebih tepat dan jelaskan alasannya.
c. Tuliskan ukuran chunk berturut-turut yang dihasilkan `schedule(guided)` untuk sebuah loop dengan **20 iterasi** dan **3 thread** (tanpa chunksize).
d. Tentukan apakah setiap loop berikut dapat langsung diparalelkan dengan `#pragma omp parallel for`. Jika tidak, jelaskan alasannya dan perbaiki bila memungkinkan.
```c
/* L1 */ for (i = 1; i < n; i++) a[i] = a[i-1] + b[i];
/* L2 */ for (i = 0; i < n; i++) { t = a[i] * 2; b[i] = t + 1; }      // t dideklarasikan di luar loop
/* L3 */ for (i = 0; i < n; i++) if (a[i] > maks) maks = a[i];
/* L4 */ for (k = 0; k < n; k++) x[k] = (k % 2 == 0 ? 1.0 : -1.0) / (2*k + 1);
```
e. Tuliskan ulang kode berikut agar **tim thread hanya dibuat sekali**:
```c
#pragma omp parallel for
for (i = 0; i < n; i++) b[i] = a[i] * a[i];
#pragma omp parallel for reduction(+:total)
for (i = 0; i < n; i++) total += b[i];
```

---

### A5. MPI (16 poin)
a. Program berikut dijalankan dengan **4 proses**.
```c
int val  = (rank + 1) * (rank + 1);
int sum  = val, send = val, recv;
int right = (rank + 1) % size, left = (rank - 1 + size) % size;
for (int s = 1; s < size; s++) {
    MPI_Sendrecv(&send, 1, MPI_INT, right, 0,
                 &recv, 1, MPI_INT, left,  0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
    sum  += recv;
    send  = recv;
}
printf("Rank %d: %d\n", rank, sum);
```
   i. Apa output setiap proses? Apa yang sebenarnya dihitung program ini?
   ii. Berapa total pesan yang dikirim? Bandingkan dengan jumlah pesan minimum untuk operasi yang sama.
   iii. Sebutkan satu fungsi kolektif MPI yang dapat menggantikan seluruh loop tersebut.

b. Potongan kode berikut dijalankan dengan beberapa proses (N = 1.000.000).
```c
double buf_out[N], buf_in[N];
int right = (rank + 1) % size, left = (rank - 1 + size) % size;
MPI_Send(buf_out, N, MPI_DOUBLE, right, 0, MPI_COMM_WORLD);
MPI_Recv(buf_in,  N, MPI_DOUBLE, left,  0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
```
   Seorang mahasiswa mengatakan "program ini berjalan lancar ketika N = 10, tetapi macet ketika N = 1.000.000". Jelaskan mengapa. Bagaimana cara menguji program ini, dan berikan **dua** cara memperbaikinya.

c. Definisikan tipe data MPI untuk struct berikut, lalu tuliskan potongan program agar rank 0 membaca `x`, `y`, `id` dari input dan mengirimkannya ke semua proses dengan **satu kali** pemanggilan fungsi kolektif.
```c
typedef struct { double x; double y; int id; } Particle;
```

d. Sebuah program normalisasi data bekerja sebagai berikut. Sebutkan fungsi MPI yang paling tepat untuk setiap langkah.
   1. Rank 0 membaca n dari input; semua proses membutuhkan n.
   2. Rank 0 memiliki array berukuran n; setiap proses harus menerima n/p elemen yang berbeda.
   3. Setiap proses membutuhkan nilai **minimum** dan **maksimum global** untuk menormalkan elemennya.
   4. Rank 0 mengumpulkan seluruh hasil normalisasi untuk dicetak.

---

### A6. Hybrid MPI + OpenMP (10 poin)
a. Untuk setiap potongan program berikut, tentukan **level thread support MPI minimum** yang diperlukan (`MPI_THREAD_SINGLE`, `FUNNELED`, `SERIALIZED`, atau `MULTIPLE`) dan jelaskan.
```c
/* P1 */
#pragma omp parallel for
for (i = 0; i < n; i++) compute(i);
MPI_Reduce(&local, &global, 1, MPI_DOUBLE, MPI_SUM, 0, MPI_COMM_WORLD);

/* P2 */
#pragma omp parallel
{
    compute_part();
    #pragma omp master
    MPI_Send(buf, n, MPI_DOUBLE, 1, 0, MPI_COMM_WORLD);
}

/* P3 */
#pragma omp parallel for
for (i = 0; i < n; i++) {
    compute(i);
    #pragma omp critical
    MPI_Send(&res[i], 1, MPI_DOUBLE, 0, i, MPI_COMM_WORLD);
}

/* P4 */
#pragma omp parallel
{
    int tid = omp_get_thread_num();
    MPI_Send(part[tid], m, MPI_DOUBLE, 0, tid, MPI_COMM_WORLD);
}

/* P5 */
#pragma omp parallel
{
    #pragma omp single
    MPI_Bcast(buf, n, MPI_DOUBLE, 0, MPI_COMM_WORLD);
    compute_part();
}
```
b. Program memanggil `MPI_Init_thread(&argc, &argv, MPI_THREAD_MULTIPLE, &provided)` dan ternyata `provided == MPI_THREAD_SERIALIZED`. Apa artinya, dan apa yang sebaiknya dilakukan program?
c. Pada level `MPI_THREAD_MULTIPLE`, sebutkan dua hal yang harus dijamin oleh programmer.

---

### A7. Vektorisasi (12 poin)
a. Berapa *vector length* instruksi AVX-512 untuk data `float` dan `double`? Berapa speedup teoretis vektorisasi AVX-512 untuk operasi `double` dibanding skalar, dan mengapa speedup nyata sering lebih kecil?
b. Untuk setiap loop berikut (semua array bertipe `float`), tentukan apakah dapat divektorisasi dengan AVX2 (256-bit) dan dengan AVX-512, beserta alasannya.
```c
/* V1 */ for (i = 0; i < n; i++)  c[i] = a[i] + b[i];
/* V2 */ for (i = 1; i < n; i++)  a[i] = a[i-1] * 0.5f + b[i];
/* V3 */ for (i = 8; i < n; i++)  a[i] = a[i-8] + b[i];
/* V4 */ for (i = 0; i < v->n; i++) v->data[i] = v->data[i] * 2.0f;   // v: struct { float* data; int n; }*
/* V5 */ for (i = 0; i < n; i++)  b[i] = my_func(a[i]);                 // my_func didefinisikan di file lain
```
c. Array `float a[1000]` dimulai pada alamat yang **sisa baginya terhadap 64 adalah 20** (alamat mod 64 = 20). Loop `for (i = 0; i < 1000; i++) a[i] = 1.0f;` divektorisasi dengan AVX-512 (alignment 64 byte). Berapa iterasi yang dikerjakan **peel loop**, berapa iterasi vektor penuh di **main loop**, dan berapa iterasi di **remainder loop**?
d. Lengkapi pragma pada fungsi berikut agar perkalian titik (*dot product*) divektorisasi dengan benar.
```c
double dot(const double *a, const double *b, int n) {
    double sum = 0.0;
    ______________________________
    for (int k = 0; k < n; k++)
        sum += a[k] * b[k];
    return sum;
}
```

---

### A8. CUDA (16 poin)
Diberikan program transpose matriks berikut (`in` berukuran rows × cols, `out` berukuran cols × rows, keduanya row-major).
```c
__global__ void transpose(const float *in, float *out, int rows, int cols) {
    int c = ______(A)______;
    int r = ______(B)______;
    if (______(C)______)
        out[______(D)______] = in[r * cols + c];
}

int main() {
    int rows = 1000, cols = 600;
    size_t bytes = rows * cols * sizeof(float);
    float *h_in  = (float*) malloc(bytes);
    float *h_out = (float*) malloc(bytes);
    float *d_in, *d_out;
    /* inisialisasi h_in diabaikan */

    ______(E)______        // alokasi memori device
    ______(F)______        // salin input ke device
    dim3 block(32, 8);
    dim3 grid(______(G)______, ______(H)______);
    transpose<<<grid, block>>>(d_in, d_out, rows, cols);
    ______(I)______        // salin hasil ke host
    ______(J)______        // bebaskan memori
    return 0;
}
```
a. Lengkapi bagian (A)–(J).
b. Hitung jumlah blok, jumlah thread yang diluncurkan, jumlah warp, dan jumlah thread yang menganggur (tidak lolos kondisi (C)).
c. Berapa warp yang mengalami *control divergence*? Jelaskan.
d. Apakah pembacaan `in[...]` dan penulisan `out[...]` bersifat *coalesced*? Jelaskan dan usulkan perbaikan.
e. Di memori mana variabel `r`, `c`, isi array `in`/`out` disimpan? Jika kernel diubah memakai `__shared__ float tile[32][33];`, berapa salinan variabel `tile` selama kernel berjalan?

---
## Paket B – Pilihan Ganda
*Pilih satu jawaban yang paling tepat.*

**B1.** Pipelining mempercepat eksekusi program karena…
a. setiap instruksi menjadi lebih singkat latensinya
b. compiler menghapus instruksi yang tidak perlu
c. jumlah core prosesor bertambah
d. prosesor menebak hasil percabangan
e. tahap-tahap dari instruksi yang berbeda dikerjakan secara tumpang tindih sehingga throughput naik

**B2.** Prosesor yang menjadwalkan beberapa instruksi untuk dieksekusi bersamaan **saat runtime** disebut…
a. superscalar  b. static multiple issue  c. vector processor  d. coarse-grained multithreading  e. SIMD

**B3.** Pernyataan yang tepat tentang *coarse-grained multithreading* adalah…
a. Prosesor berpindah thread setiap instruksi
b. Beberapa thread memakai unit fungsional yang berbeda pada siklus yang sama
c. Prosesor hanya berpindah thread ketika thread yang sedang berjalan stall menunggu operasi yang lama
d. Tidak pernah ada waktu terbuang karena stall
e. Membutuhkan perpindahan thread yang instan

**B4.** Hanya 60% sebuah program yang dapat diparalelkan. Speedup maksimum dengan jumlah prosesor tak terbatas adalah…
a. 1,67  b. 2,5  c. 6  d. 60  e. tak terbatas

**B5.** Sebuah program serial berjalan 48 detik. Dengan 8 core, program paralelnya berjalan 8 detik. Speedup dan efisiensinya adalah…
a. S = 6; E = 0,75  b. S = 8; E = 1  c. S = 6; E = 0,6  d. S = 0,17; E = 0,02  e. S = 48; E = 6

**B6.** Menurut hukum Gustafson–Barsis, jika fraksi serial s = 0,2 dan jumlah prosesor N = 16 (ukuran masalah bertambah sebanding N), speedup-nya adalah…
a. 4,0  b. 3,33  c. 13  d. 16  e. 12,8

**B7.** Sebuah jaringan memiliki latency 5 µs dan bandwidth 2 GB/s. Waktu untuk mengirim pesan 4 MB (1 MB = 10⁶ byte) adalah…
a. 2 µs  b. 2005 µs  c. 2000 µs  d. 8005 µs  e. 2,5 µs

**B8.** Bisection width dari toroidal mesh berukuran 16 × 16 (256 node) adalah…
a. 2  b. 16  c. 32  d. 128  e. 16.384

**B9.** Pada *directory-based cache coherence*…
a. setiap perubahan cache di-broadcast ke semua core melalui bus
b. variabel shared selalu dibaca ulang dari memori utama
c. nilai register disalin ke cache setiap siklus
d. sebuah directory menyimpan status setiap cache line, dan hanya cache yang memegang line tersebut yang di-invalidate
e. tidak diperlukan komunikasi antar core

**B10.** Pada sistem **NUMA**…
a. waktu akses semua lokasi memori sama untuk semua core
b. setiap core hanya dapat mengakses memori miliknya sendiri
c. akses ke memori yang terhubung langsung ke chip sendiri lebih cepat daripada memori milik chip lain
d. komunikasi antar core harus memakai message passing
e. tidak ada cache

**B11.** Tiga proses menjalankan kode berikut, dengan x = 1 dan z = 10 di semua proses. Semua `MPI_Reduce` memakai `MPI_SUM` dan root = 0.
```
Waktu | Proses 0                  | Proses 1                  | Proses 2
  1   | MPI_Reduce(&x, &y, ...)   | MPI_Reduce(&z, &w, ...)   | MPI_Reduce(&x, &y, ...)
  2   | MPI_Reduce(&z, &w, ...)   | MPI_Reduce(&x, &y, ...)   | MPI_Reduce(&z, &w, ...)
```
Nilai y dan w di proses 0 setelah kedua panggilan adalah…
a. y = 3, w = 30  b. y = 12, w = 21  c. y = 21, w = 12  d. y = 3, w = 21  e. y = 12, w = 30

**B12.** Dua proses menjalankan kode berikut:
```c
if (rank == 0) { MPI_Recv(&b, 1, MPI_INT, 1, 0, comm, &st); MPI_Send(&a, 1, MPI_INT, 1, 0, comm); }
if (rank == 1) { MPI_Recv(&b, 1, MPI_INT, 0, 0, comm, &st); MPI_Send(&a, 1, MPI_INT, 0, 0, comm); }
```
Apa yang terjadi?
a. Berjalan normal karena pesannya kecil sehingga di-buffer
b. Deadlock hanya jika pesannya besar
c. Selalu deadlock
d. Error karena rank tidak valid
e. Proses 0 menerima lebih dulu, lalu proses 1

**B13.** Pada `MPI_Scatter(sendbuf, 25, MPI_INT, recvbuf, 25, MPI_INT, 0, comm)` dengan 4 proses…
a. `sendbuf` di root minimal berisi 100 int dan setiap proses menerima 25 int
b. setiap proses mengirim 25 int ke root
c. `sendbuf` di root berisi 25 int yang dibagi menjadi 4 bagian
d. setiap proses menerima seluruh 100 int
e. hanya proses 1–3 yang menerima data

**B14.** Empat proses masing-masing memanggil `MPI_Allgather` dengan `sendcount = 3` (bertipe `MPI_INT`). Berapa elemen yang diterima `recvbuf` di **setiap** proses?
a. 3  b. 4  c. 7  d. 12  e. hanya root yang menerima 12

**B15.** Pada odd-even transposition sort paralel dengan 6 proses, pasangan proses pada **fase ganjil** adalah…
a. (0,1), (2,3), (4,5)
b. (1,2), (3,4); proses 0 dan 5 menganggur
c. (0,2), (1,3), (4,5)
d. (0,5), (1,2), (3,4)
e. semua proses bertukar dengan proses 0

**B16.** Rank 0 membaca nilai `n` dari input dan **semua** proses membutuhkan n. Fungsi yang paling tepat adalah…
a. `MPI_Scatter`  b. `MPI_Reduce`  c. `MPI_Send` dari setiap proses ke rank 0  d. `MPI_Gather`  e. `MPI_Bcast`

**B17.** Dalam OpenMP, variabel yang **dideklarasikan di dalam** blok `#pragma omp parallel` secara default memiliki scope…
a. shared  b. global  c. firstprivate  d. reduction  e. private

**B18.** Berapa baris yang dicetak oleh kode berikut?
```c
#pragma omp parallel num_threads(4)
{
    for (int i = 0; i < 8; i++)
        printf("%d\n", i);
}
```
a. 4  b. 8  c. 12  d. 32  e. tidak tentu

**B19.** Berapa nilai `sum` yang dicetak?
```c
int sum = 10;
#pragma omp parallel for reduction(+:sum) num_threads(4)
for (int i = 1; i <= 4; i++) sum += i;
printf("%d\n", sum);
```
a. 10  b. 40  c. 50  d. 20  e. tidak tentu karena race condition

**B20.** `schedule(static,3)` untuk loop 10 iterasi (indeks 0–9) dengan 3 thread. Iterasi yang dikerjakan **thread 0** adalah…
a. 0, 1, 2, 3  b. 0, 3, 6, 9  c. 0, 1, 2, 3, 4  d. 0, 1, 2  e. 0, 1, 2, 9

**B21.** Pernyataan mana yang **boleh** dilindungi dengan `#pragma omp atomic`?
a. `if (a[i] > max) max = a[i];`
b. `Enqueue(q, msg);`
c. `x = y + 1; z++;`
d. `sum += a[i] * b[i];`
e. `printf("%d", x);`

**B22.** Perbedaan `#pragma omp master` dan `#pragma omp single` yang benar adalah…
a. Keduanya dieksekusi oleh semua thread
b. `single` selalu dieksekusi thread 0, `master` oleh thread mana saja
c. `master` selalu dieksekusi thread 0 dan tidak ada barrier implisit; `single` dieksekusi oleh satu thread mana pun dengan barrier implisit di akhir
d. `master` memiliki barrier implisit; `single` tidak
e. Keduanya identik

**B23.** Thread A sedang memegang **read lock** pada sebuah `pthread_rwlock_t`. Thread C memanggil `pthread_rwlock_wrlock`. Apa yang terjadi pada thread C?
a. Menunggu sampai thread A melepas read lock
b. Langsung mendapat kunci
c. Thread A dipaksa melepas kunci
d. Terjadi error
e. Thread C mendapat read lock sebagai gantinya

**B24.** Barrier dengan semaphore memakai `counter`, `count_sem`, dan `barrier_sem`. Nilai awal yang benar adalah…
a. count_sem = 0, barrier_sem = 0
b. count_sem = 1, barrier_sem = 0
c. count_sem = 1, barrier_sem = 1
d. count_sem = 0, barrier_sem = 1
e. count_sem = thread_count, barrier_sem = 0

**B25.** Kernel diluncurkan dengan `kernel<<<dim3(4, 2), dim3(16, 16)>>>()`. Jumlah warp yang terbentuk adalah…
a. 8  b. 16  c. 32  d. 64  e. 256

**B26.** Pada kernel 2D dengan `Col = blockIdx.x*blockDim.x + threadIdx.x`, `Row = blockIdx.y*blockDim.y + threadIdx.y`, dan `idx = Row*width + Col`, thread dengan blockIdx = (2, 1), blockDim = (16, 8), threadIdx = (5, 3), dan width = 100 mengakses indeks…
a. 137  b. 1305  c. 537  d. 2105  e. 1137

**B27.** Pada reduksi naif `if (t % stride == 0)` dengan blok berukuran 256 thread, berapa warp yang mengalami divergensi saat stride = 4?
a. 0  b. 1  c. 4  d. 8  e. 64

**B28.** Kernel diluncurkan dengan grid berukuran 20 × 10 blok, masing-masing 256 thread. Sebuah variabel `__shared__` memiliki berapa salinan selama eksekusi?
a. 1  b. 200  c. 20  d. 256  e. 51.200

**B29.** Pada kernel `out[threadIdx.x * 32] = in[threadIdx.x];` (satu blok berisi 32 thread)…
a. akses `in` coalesced, akses `out` tidak coalesced
b. akses `in` dan `out` sama-sama coalesced
c. akses `in` tidak coalesced, akses `out` coalesced
d. keduanya tidak coalesced
e. coalescing hanya berlaku untuk shared memory

**B30.** Variabel yang dideklarasikan dengan `__constant__`…
a. dapat ditulis oleh setiap thread
b. hanya dapat dibaca oleh thread, nilainya diisi oleh host, dan hidup selama aplikasi
c. berada di shared memory dan hidup selama blok
d. berada di register
e. hanya dapat diakses oleh host

**B31.** Berapa *vector length* untuk data `float` (32-bit) pada instruksi AVX2 (256-bit)?
a. 2  b. 4  c. 8  d. 16  e. 32

**B32.** `#pragma omp simd` pada sebuah loop akan…
a. membuat tim thread dan membagi iterasi ke thread-thread
b. menghapus dependensi loop secara otomatis
c. memindahkan loop ke GPU
d. memecah loop menjadi potongan yang muat di register SIMD tanpa membuat thread baru
e. menjamin alignment 64 byte

**B33.** Pada program hybrid, fungsi MPI dipanggil di dalam blok `#pragma omp single` yang berada di dalam region `#pragma omp parallel`. Level thread support MPI **minimum** yang diperlukan adalah…
a. MPI_THREAD_SINGLE  b. MPI_THREAD_FUNNELED  c. MPI_THREAD_SERIALIZED  d. MPI_THREAD_MULTIPLE  e. tidak perlu level khusus

**B34.** Jika program memakai `MPI_Init` (bukan `MPI_Init_thread`), level thread support default-nya adalah…
a. MPI_THREAD_SINGLE  b. MPI_THREAD_FUNNELED  c. MPI_THREAD_SERIALIZED  d. MPI_THREAD_MULTIPLE  e. bergantung jumlah core

**B35.** Kasus mana yang **paling tidak cocok** diimplementasikan di GPU?
a. Mengubah 50 juta piksel gambar menjadi grayscale
b. Menjumlahkan dua vektor berukuran 10⁸
c. Perkalian matriks 4096 × 4096
d. Menelusuri sebuah linked list secara rekursif dari kepala sampai ekor
e. Thresholding citra satelit

---
## Kunci & Pembahasan Paket A

### A1 – Dasar Sistem Paralel
**🔁 Pola:** UTS 2024/25 Q1 (roofline), UTS 2013/14 no.3, 18/19 no.1 (Amdahl); slide 02 (grafik *Performance*, Amdahl).

**Jawaban:**
| | Jawaban |
|---|---|
| a | **32 GB/s** |
| b | **2 GFLOP/s** |
| c | **64 GFLOP/s** |
| d | **16 GFLOP/s, memory bound** |
| e | **2 FLOP/byte** |
| f | **9 prosesor** (S(9) ≈ 4,09); S_maks ≈ **6,67** |

**Pembahasan:**
```
a. Bandwidth DRAM = 2 word/cycle × 8 byte/word × 2·10⁹ cycle/s = 32·10⁹ B/s = 32 GB/s
b. Puncak 1 core skalar = 2·10⁹ cycle/s × 1 FLOP/cycle = 2·10⁹ FLOP/s = 2 GFLOP/s
c. Vektor 256-bit double = 256/64 = 4 elemen/instruksi → 4 FLOP/cycle per thread
   Puncak sistem = 2·10⁹ × 4 (vektor) × 4 core × 2 HT = 64·10⁹ FLOP/s = 64 GFLOP/s
d. Batas memori = AI × BW = 0,5 × 32 = 16 GFLOP/s;  batas komputasi = 64 GFLOP/s
   Kinerja = min(64, 16) = 16 GFLOP/s → dibatasi memori → MEMORY BOUND
e. Compute bound jika AI × BW ≥ puncak → AI ≥ 64/32 = 2 FLOP/byte
f. s = 0,15: 1/(0,15 + 0,85/p) ≥ 4 → 0,15 + 0,85/p ≤ 0,25 → 0,85/p ≤ 0,1 → p ≥ 8,5 → p = 9
   S(9) = 1/(0,15 + 0,85/9) ≈ 4,09;  S_maks = 1/0,15 ≈ 6,67
```
Catatan (c): mengikuti model slide (Multicore 4 core × 2 hyperthread = 192 GFLOP/s), hyperthread ikut dikalikan. Jika hyperthread tidak dihitung (karena berbagi unit fungsional), puncaknya 32 GFLOP/s — sebutkan asumsi di jawaban.

---

### A2 – Instruction Level Parallelism
**🔁 Pola:** UTS 2015/16 no.1, 20/21 no.11, 13/14 no.1; slide 02 (pipelining, multiple issue, speculation).

**Jawaban:** a. **3600 ns** · b. **602,5 ns** · c. **302,5 ns, CPI ≈ 0,5** · d. **≈ 11,9×** · e. lihat pembahasan.

**Pembahasan:**
```
a. Tanpa pipeline : 1200 instruksi × 6 tahap × 0,5 ns = 3600 ns
b. Pipeline       : (k + N − 1) siklus = 6 + 1199 = 1205 siklus × 0,5 ns = 602,5 ns
c. Pipeline + issue 2 : 6 + (1200/2 − 1) = 6 + 599 = 605 siklus × 0,5 ns = 302,5 ns
                         CPI = 605/1200 ≈ 0,504 → mendekati 1/2
d. Speedup = 3600 / 302,5 ≈ 11,9   (≈ 6 dari pipeline × 2 dari multiple issue)
```
e. Agar multiple issue terpakai, prosesor **menebak** hasil cabang (mis. "z > 0") dan langsung mengeksekusi `w = x` sebelum z selesai dihitung. Jika tebakan benar, waktu terhemat. Jika **salah**, hasil `w = x` dibuang (instruksi di pipeline dibatalkan/*flush*), lalu prosesor kembali dan menghitung `w = y` → ada penalti waktu.

---

### A3 – Pthreads
**🔁 Pola:** UTS 2024/25 Q2 (menghitung thread), slide 04 (semaphore, contoh perkalian matriks tidak komutatif).

**a. Jawaban: 3 thread.**
Graf dependensi:
```
b ─┬─▶ S1 (a = b+1) ──▶ S2 (c = a*2) ──┐
   ├─▶ S3 (d = b-4) ───────────────────┼─▶ S4 (e = d + c)
   └─▶ S5 (f = b*b)                    
```
- Rantai terpanjang: S1 → S2 → S4 (3 langkah) — harus berurutan (RAW).
- S3 dan S5 hanya membaca b, sehingga bisa berjalan bersamaan dengan S1.
- Pembagian: **Thread 1:** S1, S2, S4 · **Thread 2:** S3 · **Thread 3:** S5. Pada langkah pertama ada 3 instruksi yang berjalan paralel (S1, S3, S5), jadi 3 thread adalah jumlah minimum untuk paralelisme maksimum. S4 harus menunggu S3 selesai (perlu sinkronisasi, mis. join atau semaphore). Menambah thread ke-4 tidak berguna karena tidak ada instruksi lain yang bisa berjalan bersamaan.

**b. Jawaban: 16 thread.**
- Loop `fork()` 2 kali: setelah fork pertama ada 2 proses; keduanya menjalankan fork kedua → **4 proses** (2²).
- Setiap proses: 1 main thread + 3 thread dari `pthread_create` = 4 thread.
- Total = 4 × 4 = **16 thread** (12 jika hanya menghitung thread hasil `pthread_create`).

**c. Jawaban:** mutex hanya menjamin **satu thread pada satu waktu** masuk critical section, tetapi **urutan** thread masuk ditentukan oleh sistem/kebetulan. Karena perkalian matriks tidak komutatif (A₀×A₁ ≠ A₁×A₀), urutan acak memberi hasil salah. Gunakan semaphore untuk memaksa urutan 0, 1, 2, …:
```c
sem_t sems[MAX_THREADS];          /* shared */
/* di main, sebelum membuat thread: */
sem_init(&sems[0], 0, 1);          /* thread 0 boleh langsung masuk */
for (i = 1; i < thread_count; i++)
    sem_init(&sems[i], 0, 0);      /* thread lain menunggu giliran */

void* Thread_work(void* rank) {
    long my_rank = (long) rank;
    Matrix my_mat = Generate_matrix(my_rank);      /* dikerjakan paralel */
    sem_wait(&sems[my_rank]);                      /* tunggu giliran */
    Multiply_matrix(product, my_mat);              /* product = product × A_my_rank */
    if (my_rank < thread_count - 1)
        sem_post(&sems[my_rank + 1]);              /* beri giliran ke thread berikutnya */
    return NULL;
}
```
Pembangkitan matriks tetap berjalan paralel; hanya perkaliannya yang berurutan. (Busy-wait dengan `flag` juga bisa memaksa urutan, tetapi boros CPU.)

---

### A4 – OpenMP
**🔁 Pola:** UTS 2024/25 Q3 (race), 15/16 no.4 (max), 18/19 no.5–6, 20/21 no.1, 25/26 no.2 (schedule), 21/22 no.18 (dependensi); slide 05 (contoh fungsi f(i) dengan beban tidak seragam).

**a.** `count` shared dan diubah banyak thread → **race condition** pada `count++` (read-modify-write tidak atomik) → hasil lebih kecil dari seharusnya dan berubah-ubah. Perbaikan:
```c
#pragma omp parallel for reduction(+:count)
for (int n = 2; n <= N; n++)
    if (is_prime(n)) count++;
```
(Alternatif yang lebih lambat: `#pragma omp atomic` sebelum `count++`.)

**b.** Default = `static` tanpa chunk → iterasi dibagi menjadi **blok berurutan**. Karena biaya `is_prime(n)` naik seiring n, thread yang mendapat blok n terbesar bekerja paling lama, sementara thread lain menganggur → **load imbalance** (mirip contoh slide: 2 thread default speedup 1,33 vs cyclic 1,99). Solusi:
- `schedule(dynamic, chunk)` (mis. chunk 100) — thread yang selesai mengambil potongan berikutnya, cocok untuk beban tidak seragam dan tidak terduga (bilangan prima vs komposit berbeda biayanya).
- `schedule(guided)` — chunk besar di awal (overhead kecil) lalu mengecil di akhir agar semua thread selesai hampir bersamaan.
- `schedule(static, 1)` (cyclic) juga membantu karena setiap thread mendapat campuran n kecil dan besar.

**c.** Guided: ukuran chunk ≈ ⌈sisa iterasi / jumlah thread⌉:
```
sisa 20 → chunk 7   (sisa 13)
sisa 13 → chunk 5   (sisa 8)
sisa 8  → chunk 3   (sisa 5)
sisa 5  → chunk 2   (sisa 3)
sisa 3  → chunk 1   (sisa 2)
sisa 2  → chunk 1   (sisa 1)
sisa 1  → chunk 1   (sisa 0)
Urutan chunk: 7, 5, 3, 2, 1, 1, 1  (total 20)
```
Thread mana yang mendapat chunk mana ditentukan saat runtime (siapa yang lebih dulu selesai), seperti tabel guided di slide.

**d.**
| Loop | Paralel langsung? | Alasan & perbaikan |
|---|---|---|
| L1 `a[i] = a[i-1] + b[i]` | ❌ | Loop-carried dependency (RAW): iterasi i butuh hasil iterasi i−1 (prefix sum). Tidak bisa diperbaiki hanya dengan pragma; perlu algoritma lain (mis. parallel prefix sum/scan) |
| L2 `t = a[i]*2; b[i] = t+1;` | ❌ (apa adanya) | `t` dideklarasikan di luar → **shared** → race (thread saling menimpa t). Perbaikan: `#pragma omp parallel for private(t)` atau deklarasikan `t` di dalam loop |
| L3 `if (a[i] > maks) maks = a[i];` | ❌ (apa adanya) | Race pada `maks`. Perbaikan: `#pragma omp parallel for reduction(max:maks)` |
| L4 `x[k] = (k%2==0 ? 1.0 : -1.0)/(2k+1)` | ✅ | Setiap iterasi hanya bergantung pada k sendiri (tanda dihitung dari k, bukan dari iterasi sebelumnya — sama seperti solusi #2 estimasi π di slide) |

**e.**
```c
#pragma omp parallel                   /* tim dibuat sekali */
{
    #pragma omp for
    for (i = 0; i < n; i++) b[i] = a[i] * a[i];
    /* barrier implisit di akhir omp for → semua b[i] siap sebelum dijumlah */
    #pragma omp for reduction(+:total)
    for (i = 0; i < n; i++) total += b[i];
}
```

---

### A5 – MPI
**🔁 Pola:** UTS 13/14 no.4 & 15/16 no.3 (tracing), 25/26 no.4 (pesan minimum Allreduce), 20/21 no.12 & 19/20 no.3 (safety), 16/17 no.3b (derived type), 20/21 no.9–10 (memilih kolektif); slide 03.

**a.i** Nilai awal: rank 0 = 1, rank 1 = 4, rank 2 = 9, rank 3 = 16. Setiap langkah, setiap proses meneruskan nilai yang baru diterimanya ke kanan dan menerima nilai baru dari kiri:
```
              rank 0       rank 1       rank 2       rank 3
awal sum      1            4            9            16
s=1 terima    16 (dari 3)  1            4            9      → sum: 17, 5, 13, 25
s=2 terima    9            16           1            4      → sum: 26, 21, 14, 29
s=3 terima    4            9            16           1      → sum: 30, 30, 30, 30
```
Output: **semua proses mencetak 30** (urutan baris acak). Program menghitung **jumlah global** (1 + 4 + 9 + 16 = 30) yang diketahui **semua** proses → sebuah **allreduce** berbasis ring.

**a.ii** Setiap langkah, setiap proses mengirim 1 pesan → total p·(p−1) = 4 × 3 = **12 pesan** dalam 3 langkah. Jumlah pesan minimum untuk allreduce adalah 2(p−1) = **6 pesan** (reduce ke satu proses lalu broadcast). Ring ini boros pesan, tetapi bebas deadlock karena memakai `MPI_Sendrecv`.

**a.iii** `MPI_Allreduce(&val, &sum, 1, MPI_INT, MPI_SUM, MPI_COMM_WORLD);`

**b.** Semua proses memanggil `MPI_Send` lebih dulu. Standar MPI mengizinkan `MPI_Send` **mem-buffer** pesan lalu kembali, atau **block** sampai `MPI_Recv` pasangannya dimulai. Kebanyakan implementasi mem-buffer pesan kecil (N = 10 → semua Send kembali, lalu semua Recv berjalan → lancar), tetapi memblokir pesan besar (N = 10⁶ → 8 MB). Jika semua Send block, tidak ada proses yang mencapai Recv → **deadlock**. Program yang bergantung pada buffering seperti ini disebut **unsafe**.
- **Cara menguji:** ganti `MPI_Send` dengan `MPI_Ssend` (synchronous, selalu block sampai receive dimulai). Jika program macet, berarti unsafe.
- **Perbaikan 1:** urutan berlawanan berdasarkan paritas rank (aman untuk jumlah proses genap maupun ganjil):
```c
if (rank % 2 == 0) {
    MPI_Send(buf_out, N, MPI_DOUBLE, right, 0, MPI_COMM_WORLD);
    MPI_Recv(buf_in,  N, MPI_DOUBLE, left,  0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
} else {
    MPI_Recv(buf_in,  N, MPI_DOUBLE, left,  0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
    MPI_Send(buf_out, N, MPI_DOUBLE, right, 0, MPI_COMM_WORLD);
}
```
- **Perbaikan 2:** `MPI_Sendrecv` (MPI yang menjadwalkan agar tidak deadlock):
```c
MPI_Sendrecv(buf_out, N, MPI_DOUBLE, right, 0,
             buf_in,  N, MPI_DOUBLE, left,  0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
```
(Alternatif: `MPI_Isend`/`MPI_Irecv` + `MPI_Waitall`.)

**c.**
```c
typedef struct { double x; double y; int id; } Particle;

void Build_particle_type(Particle* p, MPI_Datatype* new_t) {
    int          blocklen[3] = {1, 1, 1};
    MPI_Datatype types[3]    = {MPI_DOUBLE, MPI_DOUBLE, MPI_INT};
    MPI_Aint     disp[3], base;
    MPI_Get_address(p,      &base);
    MPI_Get_address(&p->x,  &disp[0]);
    MPI_Get_address(&p->y,  &disp[1]);
    MPI_Get_address(&p->id, &disp[2]);
    for (int k = 0; k < 3; k++) disp[k] -= base;    /* posisi relatif terhadap awal struct */
    MPI_Type_create_struct(3, blocklen, disp, types, new_t);
    MPI_Type_commit(new_t);
}

/* di main */
Particle p;
MPI_Datatype MPI_PARTICLE;
Build_particle_type(&p, &MPI_PARTICLE);
if (rank == 0) scanf("%lf %lf %d", &p.x, &p.y, &p.id);   /* hanya rank 0 membaca stdin */
MPI_Bcast(&p, 1, MPI_PARTICLE, 0, MPI_COMM_WORLD);       /* satu kali kirim untuk x, y, id */
MPI_Type_free(&MPI_PARTICLE);
```
Keuntungan: tiga nilai dikirim dalam **satu pesan** (latency dibayar sekali), bukan tiga kali `MPI_Bcast`.

**d.**
| Langkah | Fungsi | Alasan |
|---|---|---|
| 1. n dari rank 0 ke semua | `MPI_Bcast` | Data yang sama ke semua proses |
| 2. bagikan n/p elemen berbeda | `MPI_Scatter` | Data root dipotong per proses (block) |
| 3. min & max global untuk semua | `MPI_Allreduce` (sekali dengan `MPI_MIN`, sekali dengan `MPI_MAX`) | Semua proses butuh hasil reduksi; tidak ada root |
| 4. kumpulkan hasil ke rank 0 | `MPI_Gather` | Potongan semua proses digabung di root |

---

### A6 – Hybrid MPI + OpenMP
**🔁 Pola:** slide 05c (4 level thread safety + contoh kode tiap level).

**a.**
| Potongan | Level minimum | Alasan |
|---|---|---|
| P1 | **FUNNELED** | Program multithread (OpenMP), tetapi MPI dipanggil di luar region paralel → hanya oleh master thread (persis contoh FUNNELED di slide) |
| P2 | **FUNNELED** | `omp master` → MPI hanya dipanggil oleh master thread (thread 0), meski di dalam region paralel |
| P3 | **SERIALIZED** | MPI dipanggil oleh thread mana pun, tetapi `omp critical` menjamin hanya satu pada satu waktu (persis contoh SERIALIZED di slide) |
| P4 | **MULTIPLE** | Setiap thread memanggil `MPI_Send` secara bersamaan tanpa perlindungan |
| P5 | **SERIALIZED** | `omp single` dieksekusi oleh **satu thread mana pun** (belum tentu master) → tidak memenuhi syarat FUNNELED; tetapi hanya satu thread yang memanggil MPI pada satu waktu |

**b.** `provided` adalah level yang **benar-benar** disediakan implementasi MPI. Program meminta MULTIPLE tetapi hanya mendapat SERIALIZED → pemanggilan MPI dari banyak thread sekaligus **tidak aman**. Program sebaiknya memeriksa `if (provided < MPI_THREAD_MULTIPLE)`, lalu berhenti dengan pesan error (`MPI_Abort`) atau beralih ke strategi yang sesuai level yang tersedia (mis. membungkus semua panggilan MPI dengan `omp critical`, atau hanya memanggil MPI dari master thread).

**c.** (1) Pemanggilan **kolektif** pada satu communicator harus terjadi dalam **urutan yang sama** di semua proses (misalnya jangan sampai dua thread di satu proses memanggil `MPI_Bcast` dan `MPI_Reduce` pada communicator yang sama dalam urutan acak). (2) Pemanggilan MPI dari thread-thread berbeda **tidak boleh menimbulkan race condition** (misalnya dua thread menerima ke buffer yang sama). Ditambah fakta: pemanggilan MPI yang blocking hanya mem-block **thread pemanggil**, bukan seluruh proses.

---

### A7 – Vektorisasi
**🔁 Pola:** slide 05b (vector length/width, dependensi, loop-carried, loop not countable, misaligned loop, omp simd).

**a.** AVX-512 = 512 bit → `float` (32 bit): 512/32 = **16 elemen**; `double` (64 bit): 512/64 = **8 elemen**. Speedup teoretis untuk double ≈ **8×**. Speedup nyata sering lebih kecil karena kebanyakan aplikasi **memory bound** (bandwidth memori tidak bertambah walau komputasinya 8× lebih cepat), ditambah peel/remainder loop, data tidak aligned, dan bagian kode yang tidak bisa divektorisasi.

**b.** (float: AVX2 VL = 8, AVX-512 VL = 16)
| Loop | AVX2 | AVX-512 | Alasan |
|---|---|---|---|
| V1 | ✅ | ✅ | Elemen independen, akses berurutan (unit stride) |
| V2 | ❌ | ❌ | RAW **jarak 1**: a[i] butuh a[i−1] yang baru dihitung → tidak bisa diproses 8/16 sekaligus |
| V3 | ✅ | ❌ | RAW **jarak 8**: satu vektor berisi a[i..i+VL−1] butuh a[i−8..i+VL−9]. Jika VL ≤ 8, semua nilai itu sudah dihitung oleh vektor sebelumnya → aman. Dengan VL = 16, vektor membutuhkan a[i..i+7] yang sedang dihitung di vektor yang sama → salah. Dengan AVX-512 bisa dipaksa aman memakai `#pragma omp simd safelen(8)` |
| V4 | ⚠️ kemungkinan gagal | ⚠️ | **Loop not countable**/aliasing: compiler tidak yakin `v->n` tidak berubah ketika `v->data[i]` ditulis (pointer bisa saling tumpang tindih). Perbaikan: `int n = v->n; float * restrict d = v->data; for (i = 0; i < n; i++) d[i] *= 2.0f;` |
| V5 | ❌ | ❌ | Ada **pemanggilan fungsi** di dalam loop yang tidak bisa di-inline (beda file) → compiler tidak bisa membuat versi vektornya. Perbaikan: definisikan `my_func` sebagai inline di file yang sama, atau versi vektornya |

**c.** Alignment AVX-512 = 64 byte; `float` = 4 byte.
```
Alamat awal mod 64 = 20 → perlu (64 − 20)/4 = 11 elemen agar alamat jatuh di kelipatan 64
Peel loop      : 11 iterasi (skalar)
Sisa           : 1000 − 11 = 989 elemen
Main loop      : ⌊989/16⌋ = 61 vektor penuh = 976 iterasi
Remainder loop : 989 − 976 = 13 iterasi
Cek            : 11 + 976 + 13 = 1000 ✓
```
Agar peel loop tidak diperlukan, deklarasikan array aligned: `float a[1000] __attribute__((aligned(64)));`.

**d.**
```c
#pragma omp simd reduction(+:sum)
```
`reduction(+:sum)` wajib: setiap lane vektor mengakumulasi jumlah parsialnya sendiri, lalu digabung di akhir. Tanpa reduction, `sum` membawa dependensi antar iterasi sehingga compiler tidak akan (atau tidak boleh) memvektorisasinya.

---

### A8 – CUDA
**🔁 Pola:** UTS 2016/17 no.4b & 25/26 no.5 (lengkapi kode), 19/20 no.7 (jumlah warp), 21/22 no.17 & 20/21 no.15 (divergence), 19/20 no.6 (coalescing), 19/20 no.8 (salinan shared); slide 06a.

**a.**
```c
(A) blockIdx.x * blockDim.x + threadIdx.x          // kolom pada in
(B) blockIdx.y * blockDim.y + threadIdx.y          // baris pada in
(C) r < rows && c < cols
(D) c * rows + r                                   // out[c][r] (out berukuran cols × rows)
(E) cudaMalloc((void**)&d_in, bytes);  cudaMalloc((void**)&d_out, bytes);
(F) cudaMemcpy(d_in, h_in, bytes, cudaMemcpyHostToDevice);
(G) (cols + block.x - 1) / block.x                 // = (600 + 31)/32 = 19
(H) (rows + block.y - 1) / block.y                 // = (1000 + 7)/8  = 125
(I) cudaMemcpy(h_out, d_out, bytes, cudaMemcpyDeviceToHost);
(J) cudaFree(d_in); cudaFree(d_out); free(h_in); free(h_out);
```
(Sebelum (I) boleh ditambahkan `cudaDeviceSynchronize();`.)

**b.**
```
Blok          : 19 × 125 = 2375 blok
Thread/blok   : 32 × 8 = 256
Thread total  : 2375 × 256 = 608.000
Warp          : 256/32 = 8 per blok → 2375 × 8 = 19.000 warp
Thread kerja  : 1000 × 600 = 600.000
Menganggur    : 608.000 − 600.000 = 8.000 thread
```

**c. 1000 warp divergen.**
- blockDim.x = 32 → satu warp = **satu baris** 32 thread pada blok (8 warp per blok = 8 baris).
- Baris: rows = 1000 = 125 × 8 tepat → tidak ada blok yang melewati batas baris.
- Kolom: blok kolom terakhir (blockIdx.x = 18) mencakup c = 576–607, padahal yang valid hanya 576–599 → di setiap warp 24 thread lolos kondisi, 8 tidak → **divergen**.
- Jumlah = 125 blok di kolom terakhir × 8 warp = **1000 warp**. Warp di 18 kolom blok lainnya seluruhnya valid → tidak divergen.

**d.**
- **Baca `in[r*cols + c]`: coalesced.** Thread berurutan dalam warp punya c berurutan → alamat berurutan.
- **Tulis `out[c*rows + r]`: tidak coalesced.** Thread berurutan (c berurutan) menulis ke alamat yang berjarak `rows` = 1000 float → 32 transaksi terpisah per warp.
- **Perbaikan (tiling dengan shared memory):** setiap blok memuat satu tile 32×32 dari `in` ke `__shared__` dengan baca coalesced, `__syncthreads()`, lalu menulis tile tersebut ke `out` dengan peran indeks ditukar sehingga thread berurutan menulis alamat berurutan (tulis coalesced). Ukuran `[32][33]` (padding 1 kolom) dipakai untuk menghindari *bank conflict* *(tambahan)*.

**e.** `r`, `c` → **register** (variabel lokal per thread). Isi `in`/`out` → **global memory** (dialokasikan dengan `cudaMalloc`). `__shared__ float tile[32][33]` → **satu salinan per blok** → **2375 salinan** (masing-masing 32 × 33 × 4 = 4.224 byte di shared memory SM tempat blok itu berjalan).

---
## Kunci & Pembahasan Paket B

**B1 → e.** Pipelining menumpangtindihkan tahap-tahap instruksi yang berbeda → throughput naik; latensi satu instruksi **tidak** berkurang (contoh slide: satu penjumlahan tetap 7 ns, tetapi 1000 penjumlahan 1006 ns). d = speculation. 🔁 UTS 25/26 no.1d.

**B2 → a.** Dynamic multiple issue = **superscalar**. Static multiple issue dijadwalkan compiler. 🔁 slide 02.

**B3 → c.** Coarse-grained hanya berpindah thread saat stall lama; kelemahannya prosesor bisa menganggur pada stall pendek dan perpindahan thread menimbulkan delay. a = fine-grained, b = SMT. 🔁 UTS 21/22 no.1.

**B4 → b.** s = 0,4 → S_maks = 1/0,4 = **2,5**. (1,67 = 1/0,6 adalah jebakan memakai fraksi paralel.) 🔁 UTS 13/14 no.3.

**B5 → a.** S = 48/8 = **6**; E = 6/8 = **0,75**. 🔁 slide 02.

**B6 → c.** S = N − s(N − 1) = 16 − 0,2 × 15 = **13**. (a = Amdahl untuk N = 16: 1/(0,2 + 0,8/16) = 4,0; e = hanya bagian paralel 16 × 0,8.) 🔁 slide 02.

**B7 → b.** l + n/b = 5 µs + 4·10⁶ B / 2·10⁹ B/s = 5 µs + 2000 µs = **2005 µs**. 🔁 slide 02.

**B8 → c.** Toroidal mesh √p × √p: bisection width = 2√p = 2 × 16 = **32** (memotong 16 baris link, masing-masing 2 karena wrap-around). 🔁 UTS 21/22 no.2.

**B9 → d.** Directory menyimpan status setiap cache line; hanya cache yang tercatat memegang line itu yang di-invalidate. a = snooping. 🔁 UTS 21/22 no.3.

**B10 → c.** NUMA: memori lokal chip sendiri lebih cepat daripada memori chip lain. a = UMA. 🔁 slide 02.

**B11 → b.** Kolektif dicocokkan berdasarkan **urutan pemanggilan**, bukan nama variabel. Panggilan ke-1: x(P0) + z(P1) + x(P2) = 1 + 10 + 1 = **12** → disimpan di y (argumen output panggilan ke-1 di root). Panggilan ke-2: z(P0) + x(P1) + z(P2) = 10 + 1 + 10 = **21** → w. 🔁 slide 03 (contoh b = 4, d = 5).

**B12 → c.** `MPI_Recv` **selalu block** sampai pesan yang cocok tiba. Kedua proses memanggil Recv lebih dulu → tidak ada yang pernah mengirim → **selalu deadlock**, berapa pun ukuran pesannya. (Berbeda dengan pola Send–Send yang hanya *mungkin* deadlock.) 🔁 UTS 20/21 no.12, slide 03.

**B13 → a.** `sendcount` = jumlah elemen yang dikirim **ke setiap** proses → root harus punya 25 × 4 = **100 int**; setiap proses (termasuk root) menerima 25. 🔁 slide 03.

**B14 → d.** Allgather: setiap proses menyumbang 3 int dan **semua** proses menerima gabungannya: 3 × 4 = **12 int**. 🔁 UTS 16/17 no.3a, 18/19 no.4.

**B15 → b.** Fase ganjil: rank ganjil berpasangan dengan rank+1, rank genap dengan rank−1 → (1,2), (3,4); rank 0 (partner −1) dan rank 5 (partner 6 = comm_sz) mendapat `MPI_PROC_NULL` → menganggur. a = fase genap. 🔁 UTS 19/20 no.3, slide 03.

**B16 → e.** Satu nilai yang sama ke semua proses → `MPI_Bcast` (versi `Get_input` di slide). 🔁 UTS 15/16 no.2.

**B17 → e.** Dideklarasikan **di dalam** blok parallel → **private**; dideklarasikan **sebelum** blok → shared. 🔁 slide 05.

**B18 → d.** `#pragma omp parallel` tanpa `for` → **setiap** thread menjalankan seluruh loop (worksharing tidak terjadi). 4 thread × 8 iterasi = **32 baris**. `i` dideklarasikan di dalam loop → private, jadi tidak ada race pada `i`. 🔁 UTS 18/19 no.5.

**B19 → d.** Setiap thread memakai salinan private `sum` bernilai awal 0 (identitas +), lalu semua salinan digabung **ke nilai awal variabel asli** (10): 10 + (1 + 2 + 3 + 4) = **20**. Tidak ada race karena memakai reduction. 🔁 slide 05.

**B20 → e.** Chunk berukuran 3: [0,1,2] → T0, [3,4,5] → T1, [6,7,8] → T2, [9] → T0 → **T0: 0, 1, 2, 9**. 🔁 UTS 18/19 no.6.

**B21 → d.** Atomic hanya untuk satu pernyataan update berbentuk `x op= expr`, `x++`, `++x`, `x--`, `--x`. `sum += a[i]*b[i]` memenuhi syarat. a (cek + tulis), c (dua pernyataan), b (fungsi), e (I/O) harus memakai `critical`. 🔁 UTS 19/20 no.5.

**B22 → c.** `master`: hanya thread 0, **tanpa** barrier implisit. `single`: satu thread **mana pun**, **dengan** barrier implisit di akhir. 🔁 UTS 21/22 no.10.

**B23 → a.** Jika ada thread yang memegang read lock, thread yang meminta write lock harus **menunggu** sampai semua read lock dilepas. 🔁 slide 04, UTS 18/19 no.2.

**B24 → b.** `count_sem` = 1 (berfungsi seperti mutex untuk melindungi counter), `barrier_sem` = 0 (thread menunggu di sini sampai dibangunkan oleh thread terakhir). 🔁 slide 04.

**B25 → d.** Jumlah blok = 4 × 2 = 8; thread per blok = 16 × 16 = 256 = 8 warp → total 8 × 8 = **64 warp**. 🔁 UTS 19/20 no.7.

**B26 → e.** Col = 2 × 16 + 5 = 37; Row = 1 × 8 + 3 = 11; idx = 11 × 100 + 37 = **1137**. 🔁 slide 06a (kernel 2D).

**B27 → d.** 256 thread = 8 warp. Stride 4 → thread aktif hanya t kelipatan 4 → 8 aktif dan 24 tidak aktif di setiap warp → **semua 8 warp** divergen. 🔁 UTS 18/19 no.7–8.

**B28 → b.** `__shared__` = satu salinan per blok → 20 × 10 = **200**. (51.200 = jumlah thread; itu jika variabelnya lokal/register.) 🔁 UTS 19/20 no.8.

**B29 → a.** `in[threadIdx.x]`: thread berurutan → alamat berurutan → **coalesced**. `out[threadIdx.x * 32]`: alamat berjarak 32 elemen → **tidak coalesced** (32 transaksi terpisah). 🔁 UTS 19/20 no.6, 18/19 no.9.

**B30 → b.** `__constant__`: berada di constant memory, **read-only** bagi thread, diisi oleh host, lifetime aplikasi, di-cache. 🔁 UTS 20/21 no.14, slide 06a (memory model).

**B31 → c.** 256 bit / 32 bit = **8** float. (Double: 4.) 🔁 slide 05b.

**B32 → d.** `omp simd` hanya memvektorisasi loop (memecahnya menjadi potongan selebar register SIMD), **tidak** membuat thread. Untuk thread + vektor sekaligus dipakai `#pragma omp parallel for simd`. 🔁 slide 05b.

**B33 → c.** `omp single` boleh dieksekusi oleh thread **mana pun** (belum tentu master) → tidak memenuhi FUNNELED; tetapi hanya satu thread pada satu waktu yang memanggil MPI → **SERIALIZED**. (Jika memakai `omp master`, cukup FUNNELED.) 🔁 slide 05c.

**B34 → a.** Default `MPI_Init` = **MPI_THREAD_SINGLE**. 🔁 slide 05c.

**B35 → d.** Menelusuri linked list secara rekursif bersifat **sekuensial** (setiap langkah bergantung pada pointer sebelumnya, tidak ada paralelisme) dan memakai **rekursi** (stack GPU terbatas). Opsi lain adalah operasi data-parallel masif. 🔁 UTS 24/25 Q5, slide 06a.

---

## Rekap Kunci Paket B & Penilaian

| No | Kunci | No | Kunci | No | Kunci | No | Kunci | No | Kunci |
|---|---|---|---|---|---|---|---|---|---|
| B1 | e | B8 | c | B15 | b | B22 | c | B29 | a |
| B2 | a | B9 | d | B16 | e | B23 | a | B30 | b |
| B3 | c | B10 | c | B17 | e | B24 | b | B31 | c |
| B4 | b | B11 | b | B18 | d | B25 | d | B32 | d |
| B5 | a | B12 | c | B19 | d | B26 | e | B33 | c |
| B6 | c | B13 | a | B20 | e | B27 | d | B34 | a |
| B7 | b | B14 | d | B21 | d | B28 | b | B35 | d |

**Penilaian:**
- Paket A: bobot sesuai poin tiap soal (total 100). Untuk soal hitungan, langkah yang benar tetap layak mendapat sebagian nilai walau angka akhirnya salah.
- Paket B: nilai = (jumlah benar / 35) × 100.

| Nilai Paket B | Arti |
|---|---|
| ≥ 85 | Siap UTS — fokus ulangi soal esai yang masih salah |
| 70–84 | Cukup — ulangi topik yang salah lewat file *Panduan UTS* (Bagian I) |
| < 70 | Pelajari ulang *Catatan Mudah* untuk topik terkait, lalu kerjakan lagi paket ini |

**Topik untuk diulang berdasarkan nomor yang salah:**
| Nomor salah | Ulangi topik |
|---|---|
| B1–B10, A1–A2 | Dasar paralel: ILP, Amdahl/Gustafson, roofline, interkoneksi, cache |
| B11–B16, A5 | MPI: kolektif, safety, odd-even sort, derived datatype |
| B17–B24, A3–A4 | Shared memory: Pthreads & OpenMP |
| B25–B30, B35, A8 | CUDA: indeks, warp, divergence, coalescing, memori |
| B31–B34, A6–A7 | Slide 2025: vektorisasi & hybrid MPI+OpenMP |

---

*Semoga sukses UTS-nya! 🍀 Kalau mau paket simulasi berikutnya dengan tingkat kesulitan berbeda (lebih mudah/lebih sulit) atau fokus satu topik saja, tinggal minta.*
