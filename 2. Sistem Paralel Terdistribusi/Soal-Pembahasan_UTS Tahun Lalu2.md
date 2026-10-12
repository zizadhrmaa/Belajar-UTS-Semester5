# 📝 Soal & Pembahasan UTS IF3130/IF3230 Sistem Paralel dan Terdistribusi
### Seluruh soal UTS di *Bundel Soal HMIF 2024/2025*: UTS 2013/2014 – UTS 2025/2026

> **Format setiap soal**
> - **Soal** ditulis ulang selengkapnya (teks, kode, pilihan). Kode dirapikan formatnya; salah ketik yang jelas di bundel diperbaiki dan diberi catatan.
> - **✅ Jawaban** — jawaban singkat yang bisa langsung ditulis.
> - **📝 Pembahasan** — langkah dan alasan, termasuk kenapa pilihan lain salah.
> - **💡 Konsep** — materi slide yang diuji.
>
> **Tanda khusus:**
> - 🟨 = soal ambigu / ada salah ketik / kunci tidak ada di pilihan → diberi cara menjawab yang aman.
> - *(tambahan)* = butuh pengetahuan di luar slide.
>
> **Catatan:** pada halaman scan (UTS 2021/2022 dan 2024/2025) terdapat centang/coretan mahasiswa. Itu **bukan kunci**; beberapa salah dan sudah dikoreksi di sini. Soal UAS tidak termasuk.

---

## 📑 Daftar Isi
- [UTS 2013/2014](#uts-20132014) (4 soal)
- [UTS 2015/2016](#uts-20152016) (5 soal)
- [UTS 2016/2017](#uts-20162017) (4 soal)
- [UTS 2018/2019](#uts-20182019) (10 soal)
- [UTS 2019/2020](#uts-20192020) (9 soal)
- [UTS 2020/2021](#uts-20202021) (15 soal)
- [UTS 2021/2022](#uts-20212022) (18 soal)
- [UTS 2024/2025](#uts-20242025) (5 soal)
- [UTS 2025/2026](#uts-20252026) (5 soal)
- [Rekap Kunci Pilihan Ganda](#rekap-kunci-pilihan-ganda)

---

## UTS 2013/2014

### Soal 1 · ILP (10 poin)
**Soal:** Jelaskan apa yang dimaksud dengan ILP, dan berikan contohnya.

**✅ Jawaban:** ILP (*Instruction Level Parallelism*) adalah teknik meningkatkan kinerja prosesor dengan membuat beberapa komponen/unit fungsional prosesor **mengeksekusi beberapa instruksi secara bersamaan**. Contoh: pipelining, multiple issue (superscalar), dan speculation.

**📝 Pembahasan:**
ILP bekerja **di dalam satu core** dan dilakukan otomatis oleh hardware/compiler; programmer tidak perlu menulis kode paralel. Tiga bentuknya:
1. **Pipelining** – unit fungsional disusun bertahap. Contoh slide: penjumlahan floating point dipecah menjadi 7 tahap (fetch operands, compare exponents, shift, add, normalize, round, store). Untuk loop `z[i] = x[i] + y[i]` sebanyak 1000 kali, tanpa pipeline butuh 7 × 1000 = 7000 ns, dengan pipeline hanya 7 + 999 = **1006 ns**, karena setelah pipeline penuh satu hasil keluar setiap ns.
2. **Multiple issue** – unit fungsional digandakan sehingga beberapa instruksi dimulai bersamaan. Contoh: adder #1 menghitung z[1], z[3], … sementara adder #2 menghitung z[2], z[4], …. *Static* (dijadwalkan compiler) atau *dynamic* (dijadwalkan saat runtime = **superscalar**).
3. **Speculation** – compiler/prosesor menebak hasil suatu instruksi lalu mengeksekusi instruksi berikutnya berdasarkan tebakan itu. Contoh: `z = x + y; if (z > 0) w = x; else w = y;` → prosesor menebak z > 0 dan langsung menjalankan `w = x`; jika ternyata salah, hasilnya dibuang dan `w = y` dihitung ulang.

**💡 Konsep:** slide 02 – Instruction Level Parallelism.

---

### Soal 2 · Thread vs core (10 poin)
**Soal:** Pada sebuah sistem multicore, jelaskanlah apakah ada manfaatnya membuat program paralel dengan menggunakan jumlah thread yang lebih besar dibandingkan jumlah core fisik yang ada.

**✅ Jawaban:** **Tergantung karakter programnya.** Ada manfaat jika thread sering menunggu (memori/I/O/blocking) atau jika CPU mendukung SMT/Hyperthreading; tidak ada manfaat (bahkan lebih lambat) untuk program compute-bound yang selalu sibuk.

**📝 Pembahasan:**
**Kapan bermanfaat:**
- **Latency hiding:** jika satu thread stall (menunggu data dari memori, I/O, atau pesan), OS/hardware menjalankan thread lain di core yang sama, sehingga core tidak menganggur.
- **Hardware multithreading (SMT/Hyperthreading):** satu core fisik memiliki 2 konteks thread hardware yang dapat memakai unit fungsional berbeda secara bersamaan → jumlah thread efektif > core fisik.
- **Load balancing:** jika beban tidak seragam, memecah kerja menjadi lebih banyak thread/chunk membantu meratakan beban.

**Kapan tidak bermanfaat / merugikan:**
- Program **compute-bound** yang tidak pernah menunggu: core sudah penuh; thread tambahan hanya menambah **overhead context switch**, rebutan cache, dan biaya pembuatan thread. Speedup tetap dibatasi jumlah core.
- Sinkronisasi **busy-wait** menjadi sangat buruk. Data slide (π, n = 10⁸, 8 core): busy-wait 8 thread = 0,38 s, tetapi 64 thread = **3,56 s**; mutex tetap ±0,38 s. Penyebabnya: thread yang mendapat giliran bisa sedang di-suspend OS, sementara core dipakai thread lain untuk busy-wait sia-sia.

**💡 Konsep:** hardware multithreading (fine/coarse-grained, SMT), busy-wait vs mutex (slide 02 & 04).

---

### Soal 3 · Amdahl (10 poin)
**Soal:** Jika sebuah algoritma sekuensial diubah menjadi paralel, hanya 80% bagian algoritma tersebut yang dapat diparalelkan. Jelaskan berapa maksimum speedup yang dapat dicapai dengan menggunakan jumlah prosesor tak terbatas.

**✅ Jawaban:** **Speedup maksimum = 5×.**

**📝 Pembahasan:**
Fraksi serial s = 1 − 0,8 = 0,2. Hukum Amdahl:
```
S(p) = 1 / (s + (1 − s)/p) = 1 / (0,2 + 0,8/p)
p → ∞  ⇒  0,8/p → 0  ⇒  S_maks = 1 / 0,2 = 5
```
Bagian serial 20% tidak bisa dipercepat oleh prosesor tambahan. Jika T_serial = 100 detik, bagian serial tetap 20 detik sehingga T_parallel tidak pernah < 20 detik → speedup ≤ 100/20 = 5.

**💡 Konsep:** Amdahl's law (slide 02).

---

### Soal 4 · Tracing MPI (20 poin)
**Soal:** Diberikan kode MPI berikut. Jelaskan dengan singkat apa yang dihitung pada kode tersebut.
```c
int main(int argc, char *argv[]) {
    int rank, received, myvalue, p, neighbor;
    MPI_Init(&argc, &argv);
    MPI_Status status;
    MPI_Comm_size(MPI_COMM_WORLD, &p);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    myvalue = rank;
    for (int step = 1; step < p; step = step*2) {
        if (rank % step == 0) {
            if (isEven(rank/step)) {   // isEven(x) mengembalikan 1 jika x genap, 0 jika ganjil
                MPI_Recv(&received, 1, MPI_INT, rank+step, 0, MPI_COMM_WORLD, &status);
                myvalue += received;
            } else {
                MPI_Send(&myvalue, 1, MPI_INT, rank+step, 0, MPI_COMM_WORLD);
            }
        } else {
            break;
        }
    }
    if (rank == 0) {
        printf("%d\n", myvalue);
    }
    return 0;
}
```

**✅ Jawaban:** Program menghitung **jumlah seluruh rank 0 + 1 + … + (p−1) = p(p−1)/2** dengan **penjumlahan global berbentuk pohon (tree-structured reduction)** dalam log₂p langkah, lalu rank 0 mencetak hasilnya (p = 8 → 28).

**📝 Pembahasan:**
- Setiap proses memulai dengan `myvalue = rank`.
- Pada langkah dengan jarak `step` (1, 2, 4, …), hanya proses dengan `rank % step == 0` yang masih aktif. Di antara mereka, yang `rank/step` **genap** menjadi **penerima** (menjumlahkan nilai pasangannya), yang **ganjil** menjadi **pengirim**. Pengirim akan keluar (`break`) pada langkah berikutnya karena `rank % (2·step) ≠ 0`.

Simulasi p = 8 (sesuai maksud algoritma):
```
Awal         : r0=0  r1=1  r2=2  r3=3  r4=4  r5=5  r6=6  r7=7
step=1       : r0 ← r1 : 0+1=1     r2 ← r3 : 2+3=5     r4 ← r5 : 4+5=9     r6 ← r7 : 6+7=13
step=2       : r0 ← r2 : 1+5=6     r4 ← r6 : 9+13=22
step=4       : r0 ← r4 : 6+22=28   → rank 0 mencetak 28 = 8·7/2
```
Ini sama dengan pola "Better parallel algorithm" di slide 01: master hanya melakukan log₂p penerimaan, bukan p−1.

🟨 **Catatan teliti (nilai tambah):**
- Pada kode tertulis, pengirim mengirim ke `rank+step`, padahal penerima menunggu dari `rank+step` (penerima ada di **rank−step** milik pengirim). Contoh: rank 0 menunggu dari rank 1, tetapi rank 1 mengirim ke rank 2; rank 3 mengirim ke rank 4 yang tidak ada jika p = 4. Akibatnya program akan **hang/error**. Perbaikannya: `MPI_Send(&myvalue, 1, MPI_INT, rank-step, ...)`.
- Algoritma mengasumsikan **p pangkat 2**; jika tidak, `rank+step` milik penerima bisa ≥ p.
- `MPI_Finalize()` tidak dipanggil sebelum `return`.

**💡 Konsep:** tree-structured global sum, `MPI_Send`/`MPI_Recv` (slide 01 & 03).

---

## UTS 2015/2016

### Soal 1 · Multiple issue & pipelining
**Soal:** Bagaimana hubungan antara multiple issue processors dengan pipelining? Berikan ilustrasi, misalkan jumlah issues = 4 dengan 5 tahapan pipeline IF, ID, EX, MEM, WB. Hitunglah CPI (*cycle per instruction*)-nya.

**✅ Jawaban:** Multiple issue mereplikasi pipeline sehingga setiap siklus **4 instruksi** masuk dan (setelah pipeline penuh) **4 instruksi selesai** → **CPI = 1/4 = 0,25**.

**📝 Pembahasan:**
- **Pipelining** membuat tahap-tahap instruksi yang berbeda berjalan tumpang tindih; idealnya 1 instruksi selesai per siklus (CPI = 1).
- **Multiple issue** menggandakan unit fungsional/jalur pipeline sehingga **w instruksi dikeluarkan bersamaan** per siklus. Keduanya saling melengkapi: pipelining memberi paralelisme "antar tahap", multiple issue memberi paralelisme "selebar w".

Ilustrasi (setiap baris = 4 instruksi yang dikeluarkan bersamaan):
```
Siklus :   1     2     3     4     5     6     7     8
I1–I4  :   IF    ID    EX    MEM   WB
I5–I8  :         IF    ID    EX    MEM   WB
I9–I12 :               IF    ID    EX    MEM   WB
I13–I16:                     IF    ID    EX    MEM   WB
```
- Siklus 5: I1–I4 selesai; siklus 6: I5–I8 selesai; dst. → 4 instruksi selesai per siklus.
- Jumlah siklus untuk N instruksi = 5 + N/4 − 1. N = 100 → 29 siklus (CPI 0,29); N = 1000 → 254 siklus (CPI 0,254). Untuk N besar, **CPI → 1/4 = 0,25**.

**💡 Konsep:** pipelining & multiple issue (slide 02).

---

### Soal 2 · MPI_Bcast vs MPI_Scatter
**Soal:** Jelaskan perbedaan antara perintah `MPI_Bcast` dan `MPI_Scatter`, berikan ilustrasi aliran data dari sender ke dest untuk kedua perintah tersebut.

**✅ Jawaban:** `MPI_Bcast` mengirim **data yang sama secara utuh** dari root ke semua proses; `MPI_Scatter` **memotong** data root menjadi p bagian dan mengirim **bagian ke-i ke proses i**.

**📝 Pembahasan:**
```
MPI_Bcast (root = P0)                      MPI_Scatter (root = P0)
P0: [A B C D] ──┬──▶ P0: [A B C D]         P0: [A B C D] ──┬──▶ P0: [A]
                ├──▶ P1: [A B C D]                         ├──▶ P1: [B]
                ├──▶ P2: [A B C D]                         ├──▶ P2: [C]
                └──▶ P3: [A B C D]                         └──▶ P3: [D]
```
| | `MPI_Bcast` | `MPI_Scatter` |
|---|---|---|
| Data yang diterima tiap proses | Salinan **identik** seluruh data | **Potongan berbeda** (pembagian block) |
| Ukuran data per proses | count elemen (sama dengan root) | sendcount = n/p elemen |
| Signature | `MPI_Bcast(buf, count, type, root, comm)` | `MPI_Scatter(sendbuf, sendcount, sendtype, recvbuf, recvcount, recvtype, root, comm)` |
| Contoh pemakaian | Menyebarkan input a, b, n ke semua proses | Membagi vektor besar agar tiap proses mengolah bagiannya |

Keduanya fungsi **kolektif**: semua proses di communicator wajib memanggilnya (biasanya diimplementasikan berbentuk pohon).

**💡 Konsep:** collective communication (slide 03).

---

### Soal 3 · Tracing MPI ring
**Soal:** Diberikan bagian kode program MPI berikut:
```c
next = (rank + 1) % size;
prev = (rank + size - 1) % size;
if (0 == rank) {
    message = 10;
    MPI_Send(&message, 1, MPI_INT, next, tag, MPI_COMM_WORLD);
}
while (1) {
    MPI_Recv(&message, 1, MPI_INT, prev, tag, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
    if (0 == rank) {
        --message;
    }
    MPI_Send(&message, 1, MPI_INT, next, tag, MPI_COMM_WORLD);
    if (0 == message) {
        break;
    }
}
if (0 == rank) {
    MPI_Recv(&message, 1, MPI_INT, prev, tag, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
}
```
Jelaskan apa yang dilakukan oleh program tersebut secara lengkap sampai program selesai. Apa yang dilakukan oleh setiap proses, misalkan ada 5 proses yang di-spawn.

**✅ Jawaban:** Program **mengedarkan sebuah token (bilangan) berkeliling cincin** 0 → 1 → 2 → 3 → 4 → 0. Token awal bernilai 10 dan **dikurangi 1 setiap melewati rank 0**. Setelah rank 0 membuat token menjadi 0, token 0 berkeliling satu putaran terakhir; setiap proses keluar setelah meneruskan token 0, dan rank 0 menerima token 0 terakhir lalu selesai.

**📝 Pembahasan:**
Peran tiap proses (5 proses):
- `next`/`prev`: rank 0 → next 1, prev 4; rank 1 → next 2, prev 0; …; rank 4 → next 0, prev 3. Ini topologi **ring**.
- **Rank 0** memulai dengan mengirim 10 ke rank 1, lalu masuk loop. Di loop ia menerima dari rank 4, **mengurangi 1**, meneruskan ke rank 1.
- **Rank 1–4** di loop hanya menerima dari prev dan meneruskan **tanpa mengubah** nilai.

Jalannya:
```
Awal       : rank 0 mengirim 10 ke rank 1
Putaran 1  : nilai 10 lewat 1 → 2 → 3 → 4 → kembali ke 0; rank 0 mengubahnya jadi 9 dan mengirim 9
Putaran 2  : nilai 9 lewat 1 → 2 → 3 → 4 → 0; rank 0 mengubahnya jadi 8 dan mengirim 8
...
Putaran 10 : nilai 1 lewat 1 → 2 → 3 → 4 → 0; rank 0 mengubahnya jadi 0, mengirim 0 ke rank 1, lalu BREAK
Putaran 11 : nilai 0 → rank 1 terima, kirim ke 2, BREAK → rank 2, rank 3 sama → rank 4 terima, kirim ke 0, BREAK
Akhir      : rank 0 (sudah di luar loop) memanggil MPI_Recv terakhir, menerima 0 dari rank 4, selesai
```
Statistik: setiap proses melakukan **11 kali Recv dan 11 kali Send**; token berkeliling 10 putaran bernilai 10..1 ditambah 1 putaran bernilai 0.

**Kenapa ada `MPI_Recv` tambahan di rank 0?** Supaya pesan 0 terakhir dari rank 4 tetap diterima. Tanpa itu, ada pesan yang menggantung dan Send terakhir rank 4 bisa tidak pernah selesai.

**Urutan keluar:** tidak bersamaan, melainkan berurutan mengikuti cincin: rank 1 → 2 → 3 → 4, dan **rank 0 paling akhir**. Program tidak deadlock karena pada setiap saat hanya ada satu pesan yang beredar dan selalu ada penerimanya.

**💡 Konsep:** point-to-point `MPI_Send`/`MPI_Recv`, pola ring (slide 03).

---

### Soal 4 · OpenMP – mencari maksimum
**Soal:** Jelaskan apa yang kurang tepat pada kode berikut ini dan perbaikilah:
```c
#pragma omp parallel for
for (i = 0; i < N; ++i) {
    #pragma omp critical
    {
        if (arr[i] > max) max = arr[i];
    }
}
```

**✅ Jawaban:** Hasilnya benar, tetapi **setiap iterasi masuk critical section**, sehingga semua thread bergiliran satu per satu → **eksekusi menjadi serial** (bahkan lebih lambat dari serial karena overhead kunci). Perbaikan: gunakan `reduction(max:max)` atau maksimum lokal per thread lalu `critical` sekali per thread.

**📝 Pembahasan:**
Perbaikan A (OpenMP ≥ 3.1):
```c
#pragma omp parallel for reduction(max:max)
for (i = 0; i < N; ++i)
    if (arr[i] > max) max = arr[i];
```
Perbaikan B (tanpa reduction max):
```c
#pragma omp parallel
{
    int local_max = max;                 // private: setiap thread punya salinan sendiri
    #pragma omp for
    for (i = 0; i < N; ++i)
        if (arr[i] > local_max) local_max = arr[i];
    #pragma omp critical                 // hanya sekali per thread
    { if (local_max > max) max = local_max; }
}
```
Prinsipnya sama dengan contoh global sum di slide: **hitung secara lokal, gabung sekali**. Critical section N kali → hanya t kali (t = jumlah thread).
Catatan: tidak boleh memakai `atomic` di sini, karena `if (...) max = ...` bukan satu pernyataan update sederhana.

**💡 Konsep:** critical, reduction, private variable (slide 05).

---

### Soal 5 · CUDA bucket sort
**Soal:**
a. Jelaskan struktur data yang Anda gunakan untuk implementasi bucket sort pada CUDA.
b. Jelaskan strategi (berupa algoritma dan penjelasannya) pembagian data ke dalam masing-masing bucket dengan menggunakan threads pada kernel CUDA.

**✅ Jawaban:**
a. Array input di global memory, array **`count[B]`** (jumlah elemen per bucket), array **`offset[B]`** (posisi awal setiap bucket = prefix sum dari count), array output; opsional histogram lokal `__shared__` per block.
b. ① Satu thread per elemen menghitung indeks bucket lalu `atomicAdd(&count[b], 1)`; ② prefix sum → offset; ③ satu thread per elemen menempatkan elemen di `out[atomicAdd(&offset[b], 1)]`; ④ setiap bucket diurutkan (mis. satu block per bucket).

**📝 Pembahasan** *(tambahan — operasi atomik tidak ada di slide, tetapi diperlukan untuk menghindari race condition)*:
```c
// Kernel 1: histogram (1 thread = 1 elemen)
__global__ void count_k(const int* in, int* count, int n, int minv, int range, int B) {
    int i = blockIdx.x * blockDim.x + threadIdx.x;
    if (i < n) {
        int b = (in[i] - minv) * B / range;   // indeks bucket, range = max - min + 1
        atomicAdd(&count[b], 1);               // banyak thread bisa menambah bucket yang sama
    }
}
// Langkah 2: exclusive prefix sum: offset[0] = 0, offset[b] = offset[b-1] + count[b-1]
// Kernel 3: sebar elemen ke bucket
__global__ void scatter_k(const int* in, int* out, int* offset, int n, int minv, int range, int B) {
    int i = blockIdx.x * blockDim.x + threadIdx.x;
    if (i < n) {
        int b   = (in[i] - minv) * B / range;
        int pos = atomicAdd(&offset[b], 1);   // ambil slot berikutnya dalam bucket b
        out[pos] = in[i];
    }
}
// Kernel 4: urutkan isi tiap bucket (1 block per bucket; data bucket dimuat ke shared memory)
```
Alasan desain:
- Bucket ditentukan **independen** untuk setiap elemen → cocok untuk ribuan thread (data parallel).
- Banyak thread menulis ke counter yang sama → wajib **atomik** (jika tidak, race condition seperti `x += my_val`).
- Optimasi: histogram lokal per block di **shared memory** (atomik di memori cepat), lalu digabung ke global → mengurangi rebutan.
- Setelah tersebar, setiap bucket kecil dan independen → bisa diurutkan paralel; hasil akhir otomatis terurut karena bucket sudah berurutan.

**💡 Konsep:** thread-per-element, race condition, shared memory (slide 06a) + atomik *(tambahan)*.

---

## UTS 2016/2017

### Soal 1 · [General] Amdahl
**Soal:** Seorang programmer akan memparalelkan sebuah kode serial dengan hanya mengubah sebuah fungsi yang membutuhkan waktu 90% dari keseluruhan waktu jika dijalankan secara serial.
a. Hitunglah berapa speedup yang mungkin dicapai.
b. Hitunglah speedup maksimum jika menggunakan 9 processor.

**✅ Jawaban:** a. **10×** (prosesor tak terbatas). b. **5×**.

**📝 Pembahasan:** fraksi serial s = 0,1.
```
a. S_maks = 1/s = 1/0,1 = 10
b. S(9)   = 1 / (0,1 + 0,9/9) = 1 / (0,1 + 0,1) = 1/0,2 = 5      → efisiensi = 5/9 ≈ 55,6%
```
Interpretasi: dengan 9 prosesor, bagian paralel (90%) menjadi 10% dari waktu semula, sama dengan bagian serial (10%) → total 20% → 5× lebih cepat.

**💡 Konsep:** Amdahl's law.

---

### Soal 2 · [OpenMP] Dependensi loop bersarang
**Soal:** Diberikan potongan kode berikut:
```c
int i, j, k;
float x[n][n][m];
float scale = 2.0;
...
for (k = 1; k < n; k++)
    for (j = 1; j < n; j++)
        for (i = 0; i < m; i++)
            x[i][j][k] = x[i][j][k-1] + x[i][j-1][k] * scale;     // versi 1
            // x[k][j][i] = x[k-1][j][i] + x[k][j-1][i] * scale;  // versi 2
```
🟨 *Kalimat pertanyaan tidak tercantum di bundel. Pertanyaan yang lazim untuk kode ini (dan yang muncul lagi di UTS 2021/2022 no. 18): loop mana yang bisa diparalelkan dengan OpenMP, dan bagaimana cara paling efisien?*

**✅ Jawaban:** Loop **k** dan **j** membawa dependensi (tidak boleh diparalelkan); loop **i** bebas dependensi → paralelkan loop i. Cara efisien: `#pragma omp parallel` di luar loop k (dengan j, k private) + `#pragma omp for` di loop i; atau tukar urutan loop sehingga i paling luar lalu satu `#pragma omp parallel for`.

**📝 Pembahasan:**
Analisis dependensi versi 1 (`x[i][j][k] = x[i][j][k-1] + x[i][j-1][k]*scale`):

| Loop | Ada akses ke iterasi sebelumnya? | Boleh diparalelkan? |
|---|---|---|
| k | Ya: `x[...][...][k-1]` (hasil iterasi k−1) | ❌ |
| j | Ya: `x[...][j-1][...]` (hasil iterasi j−1) | ❌ |
| i | Tidak: semua akses memakai i yang sama | ✅ |

Opsi implementasi:
```c
// (1) Benar tapi boros: tim thread dibuat & dibubarkan (n-1)^2 kali
for (k = 1; k < n; k++)
    for (j = 1; j < n; j++) {
        #pragma omp parallel for
        for (i = 0; i < m; i++) x[i][j][k] = x[i][j][k-1] + x[i][j-1][k] * scale;
    }

// (2) Lebih efisien: tim dibuat sekali; omp for punya barrier implisit yang menjaga urutan (k, j)
#pragma omp parallel private(j, k)
for (k = 1; k < n; k++)
    for (j = 1; j < n; j++) {
        #pragma omp for
        for (i = 0; i < m; i++) x[i][j][k] = x[i][j][k-1] + x[i][j-1][k] * scale;
    }

// (3) Paling efisien: loop interchange — i di luar, tanpa barrier berulang
#pragma omp parallel for private(j, k)
for (i = 0; i < m; i++)
    for (k = 1; k < n; k++)
        for (j = 1; j < n; j++)
            x[i][j][k] = x[i][j][k-1] + x[i][j-1][k] * scale;
```
⚠️ j dan k dideklarasikan di luar region paralel → default **shared**; wajib dibuat `private`.

**Versi 2** (`x[k][j][i] = x[k-1][j][i] + x[k][j-1][i]*scale`): pola dependensinya sama (k dan j membawa dependensi, i bebas). Bedanya, i adalah indeks **terakhir** sehingga loop i mengakses memori **berurutan** (row-major) → lebih ramah cache dan bisa divektorisasi (`#pragma omp simd`). Pada versi 1, loop i terdalam melompat jauh di memori (stride n·n), sehingga loop interchange (opsi 3) sangat dianjurkan.

**💡 Konsep:** data dependency / loop-carried dependence, scope variabel, parallel vs for (slide 05) + layout row-major.

---

### Soal 3 · [MPI] Allreduce vs Allgather & derived datatype
**Soal:**
a. Jelaskan perbedaan kegunaan fungsi `MPI_Allreduce` dan `MPI_Allgather`.
b. Buatlah suatu program MPI yang mendefinisikan tipe data `Point` (terdiri atas integer X dan integer Y). Program menerima input berupa nilai X dan Y, kemudian mengirimkan input tersebut ke seluruh proses. Data dikirimkan dalam bentuk tipe data Point, bukan dalam bentuk integer.

**✅ Jawaban:**
a. `MPI_Allreduce` **menggabungkan** nilai semua proses dengan operator (SUM/MAX/…) dan memberikan **hasil reduksi** ke semua proses. `MPI_Allgather` **mengumpulkan/menjajarkan** data semua proses tanpa operasi dan memberikan **seluruh kumpulan** ke semua proses.
b. Program di bawah.

**📝 Pembahasan:**
a.
```
Nilai lokal: P0=3  P1=1  P2=4  P3=2
MPI_Allreduce(SUM)  → P0=10    P1=10    P2=10    P3=10         (1 nilai: hasil operasi)
MPI_Allgather       → P0=[3,1,4,2]  P1=[3,1,4,2]  P2=[3,1,4,2]  P3=[3,1,4,2]  (p nilai: dijajarkan)
```
Allreduce ≈ Reduce + Bcast; dipakai jika semua proses butuh, misalnya, total/maksimum global. Allgather ≈ Gather + Bcast; dipakai jika semua proses butuh seluruh data (contoh slide: setiap proses butuh vektor x utuh pada perkalian matriks–vektor).

b.
```c
#include <stdio.h>
#include <mpi.h>

typedef struct { int x; int y; } Point;

int main(int argc, char* argv[]) {
    int rank;
    Point p;
    MPI_Datatype MPI_POINT;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    /* 1. Definisikan tipe turunan Point */
    int          blocklengths[2] = {1, 1};
    MPI_Datatype types[2]        = {MPI_INT, MPI_INT};
    MPI_Aint     displacements[2], base;
    MPI_Get_address(&p,   &base);
    MPI_Get_address(&p.x, &displacements[0]);
    MPI_Get_address(&p.y, &displacements[1]);
    displacements[0] -= base;          /* posisi relatif terhadap awal struct */
    displacements[1] -= base;
    MPI_Type_create_struct(2, blocklengths, displacements, types, &MPI_POINT);
    MPI_Type_commit(&MPI_POINT);

    /* 2. Hanya rank 0 membaca input (aturan I/O MPI) */
    if (rank == 0) {
        printf("Masukkan X dan Y: ");
        scanf("%d %d", &p.x, &p.y);
    }

    /* 3. Kirim ke seluruh proses sebagai SATU Point */
    MPI_Bcast(&p, 1, MPI_POINT, 0, MPI_COMM_WORLD);

    printf("Proses %d menerima Point(%d, %d)\n", rank, p.x, p.y);

    MPI_Type_free(&MPI_POINT);
    MPI_Finalize();
    return 0;
}
```
Poin penilaian: tipe dibuat dengan `MPI_Type_create_struct` (+ `MPI_Get_address`), di-`commit`, input hanya di rank 0, dikirim dengan `MPI_Bcast` memakai count 1 bertipe `MPI_POINT`, lalu `MPI_Type_free`. (Alternatif valid karena dua int berurutan: `MPI_Type_contiguous(2, MPI_INT, &MPI_POINT)`.)

**💡 Konsep:** collective communication, derived datatypes (slide 03).

---

### Soal 4 · [CUDA] Arsitektur & melengkapi kode
**Soal:**
a. Jelaskan perbedaan desain arsitektur antara CPU dan GPU. Gambarkan skema umum arsitektur CPU dan GPU untuk mempertegas penjelasan perbedaan.
b. Lengkapilah kode CUDA berikut ini (ada 7 bagian, ditandai (1) sampai (7)).
```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

__global__ void Mat_add(float A[], float B[], float C[], int m, int n) {
    int my_ij;
    my_ij = (1)________________________;
    C[my_ij] = A[my_ij] + B[my_ij];
} /* Mat_add */

void Read_matrix(float A[], int m, int n) {
    int i, j;
    for (i = 0; i < m; i++)
        for (j = 0; j < n; j++)
            scanf("%f", &A[i*n + j]);
} /* Read_matrix */

void Print_matrix(char title[], float A[], int m, int n) {
    int i, j;
    printf("%s\n", title);
    for (i = 0; i < m; i++) {
        for (j = 0; j < n; j++)
            printf("%.1f ", A[i*n + j]);
        printf("\n");
    }
} /* Print_matrix */

int main(int argc, char* argv[]) {
    int m, n;
    float *h_A, *h_B, *h_C;   /* variables in host */
    float *d_A, *d_B, *d_C;   /* variables in device */
    size_t size;

    /* Size of matrices */
    m = 512; n = 256;
    size = m * n * sizeof(float);

    /* Allocate matrices in host and device memory */
    (2)________________________

    Read_matrix(h_A, m, n);
    Read_matrix(h_B, m, n);
    Print_matrix("A =", h_A, m, n);
    Print_matrix("B =", h_B, m, n);

    /* Copy matrices from host memory to device memory */
    (3)________________________

    /* Invoke kernel using m thread blocks, each contains n threads */
    (4)________________________

    /* Wait for the kernel to complete */
    (5)________________________

    /* Copy result from device memory to host memory */
    (6)________________________
    Print_matrix("The sum is: ", h_C, m, n);

    /* Free host & device memory */
    (7)________________________
    return 0;
} /* main */
```

**✅ Jawaban (b):**
```c
(1) blockIdx.x * blockDim.x + threadIdx.x

(2) h_A = (float*) malloc(size);
    h_B = (float*) malloc(size);
    h_C = (float*) malloc(size);
    cudaMalloc((void**)&d_A, size);
    cudaMalloc((void**)&d_B, size);
    cudaMalloc((void**)&d_C, size);

(3) cudaMemcpy(d_A, h_A, size, cudaMemcpyHostToDevice);
    cudaMemcpy(d_B, h_B, size, cudaMemcpyHostToDevice);

(4) Mat_add<<<m, n>>>(d_A, d_B, d_C, m, n);

(5) cudaDeviceSynchronize();          /* versi CUDA lama: cudaThreadSynchronize(); */

(6) cudaMemcpy(h_C, d_C, size, cudaMemcpyDeviceToHost);

(7) cudaFree(d_A); cudaFree(d_B); cudaFree(d_C);
    free(h_A); free(h_B); free(h_C);
```

**📝 Pembahasan:**
**(a) CPU vs GPU**
```
          CPU (latency-oriented)                   GPU (throughput-oriented)
 ┌────────────────┬───────┬───────┐       ┌──┬──────────────────────────────┐
 │                │  ALU  │  ALU  │       │C │ ▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢ │
 │    Control     ├───────┼───────┤       ├──┤ ▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢ │
 │    (besar)     │  ALU  │  ALU  │       │C │ ▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢ │   ▢ = ALU / CUDA core
 ├────────────────┴───────┴───────┤       ├──┤ ▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢ │   C = control + cache kecil
 │          Cache (besar)         │       │C │ ▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢ │       (per SM)
 ├────────────────────────────────┤       ├──┴──────────────────────────────┤
 │              DRAM              │       │              DRAM               │
 └────────────────────────────────┘       └─────────────────────────────────┘
```
| Aspek | CPU | GPU |
|---|---|---|
| Tujuan desain | **Latency** rendah: satu thread secepat mungkin | **Throughput** tinggi: sebanyak mungkin operasi per detik |
| Alokasi transistor | Banyak untuk **cache** & **control** (branch prediction, out-of-order, superscalar, speculation) | Sebagian besar untuk **ALU** |
| Core | Sedikit, kompleks | Ribuan, sederhana, dikelompokkan dalam SM |
| Menyembunyikan latency memori | Cache besar + spekulasi | Berganti **warp** (hardware multithreading, context switch hampir gratis) |
| Thread | Sedikit thread berat | Ribuan thread ringan (SIMT, warp 32) |
| Bandwidth memori | Lebih kecil | Jauh lebih besar (±10×) |
| Cocok untuk | Kode bercabang, sekuensial, beragam | Data parallel masif |

**(b) Penjelasan isian:**
- (1) Kernel dijalankan dengan **m blok × n thread**, sehingga blockIdx.x = nomor baris i dan threadIdx.x = nomor kolom j, dengan blockDim.x = n. Indeks row-major: i·n + j = `blockIdx.x * blockDim.x + threadIdx.x`. Jumlah thread pas m·n, jadi tidak perlu cek batas.
- (2) Alokasi host dengan `malloc`, device dengan `cudaMalloc(&ptr, size)`.
- (3) `cudaMemcpy(tujuan, sumber, byte, arah)` — **tujuan dulu**: d_A ← h_A (HostToDevice).
- (4) Konfigurasi eksekusi `<<<jumlah_blok, thread_per_blok>>>` = `<<<m, n>>>` = 512 blok × 256 thread (256 ≤ batas thread per blok).
- (5) Peluncuran kernel bersifat **asinkron** terhadap host → `cudaDeviceSynchronize()` menunggu kernel selesai. (`cudaMemcpy` berikutnya juga otomatis menunggu, tetapi soal meminta baris ini.)
- (6) Salin hasil d_C → h_C (DeviceToHost).
- (7) Bebaskan memori device (`cudaFree`) dan host (`free`).

**💡 Konsep:** CPU vs GPU, model pemrograman CUDA, alokasi & transfer memori, konfigurasi eksekusi (slide 06a).

---
## UTS 2018/2019

### Soal 1 · Amdahl & efisiensi
**Soal:** Diketahui sebuah program serial dapat dijalankan dengan waktu 600 second. Kode pada program serial tersebut dapat diubah ke paralel sebesar 80%. Hitunglah:
a. Berapa speedup yang dapat dicapai jika jumlah processor tidak terbatas.
b. Berapa efisiensi yang didapatkan jika memakai 8 processor.

**✅ Jawaban:** a. **S = 5**. b. **E ≈ 0,417 (41,7%)** (T_par = 180 s, S = 3,33).

**📝 Pembahasan:** bagian serial = 20% × 600 = 120 s; bagian paralel = 480 s.
```
a. p → ∞ : T_par → 120 s  ⇒  S = 600/120 = 5            (= 1/0,2)
b. p = 8 : T_par = 120 + 480/8 = 120 + 60 = 180 s
           S = 600/180 = 3,33
           E = S/p = 3,33/8 = 0,417 ≈ 41,7%
```
Interpretasi: dengan 8 prosesor, rata-rata setiap prosesor hanya 41,7% produktif karena 120 detik bagian serial tetap dikerjakan satu prosesor.

**💡 Konsep:** Amdahl, efisiensi.

---

### Soal 2 · Pthreads – data dominan dibaca
**Soal:** Diketahui Anda memiliki program serial yang ingin diubah menjadi paralel dengan PThread. Program ini didominasi membaca sebuah data dan jarang melakukan pengubahan data. Bagaimana cara Anda menghindari race condition pada saat melakukan pengubahan data, namun di saat yang sama proses pembacaan data mendapatkan keuntungan pemrograman paralel? Berikan pula contoh potongan program pada saat pembacaan dan pengubahan data.

**✅ Jawaban:** Gunakan **read-write lock** (`pthread_rwlock_t`): pembacaan memakai **read lock** (banyak thread boleh membaca bersamaan), pengubahan memakai **write lock** (eksklusif).

**📝 Pembahasan:**
Aturan read-write lock:
- Banyak thread boleh memegang **read lock** bersamaan → pembacaan tetap paralel.
- Hanya satu thread boleh memegang **write lock**; ia menunggu sampai semua pembaca selesai, dan selama ia menulis, pembaca/penulis lain menunggu → tidak ada race condition.

Mutex biasa juga mencegah race, tetapi membuat **semua** pembacaan serial (kehilangan keuntungan paralel).
```c
#include <pthread.h>
pthread_rwlock_t rwlock;                 /* shared, global */
int data[N];                             /* data bersama */

/* di main */
pthread_rwlock_init(&rwlock, NULL);

/* ---- PEMBACAAN (sering) ---- */
int Baca(int idx) {
    int v;
    pthread_rwlock_rdlock(&rwlock);      /* read lock: boleh bersamaan dengan pembaca lain */
    v = data[idx];
    pthread_rwlock_unlock(&rwlock);
    return v;
}

/* ---- PENGUBAHAN (jarang) ---- */
void Ubah(int idx, int nilai) {
    pthread_rwlock_wrlock(&rwlock);      /* write lock: eksklusif */
    data[idx] = nilai;
    pthread_rwlock_unlock(&rwlock);
}

/* di akhir main */
pthread_rwlock_destroy(&rwlock);
```
Bukti dari slide (linked list, 99,9% Member, 4 thread): read-write lock **0,098 s** vs satu mutex **0,385 s** vs mutex per node 3,450 s.

**💡 Konsep:** read-write lock (slide 04).

---

### Soal 3 · Tracing MPI ring
**Soal:** Jelaskan apa yang dilakukan oleh program paralel MPI di bawah (BUKAN menerjemahkan instruksi), dan jelaskan bagaimana masing-masing task selesai (EXIT): apakah bersama-sama, saling menunggu, atau dengan cara lain.
```c
int main(int argc, char *argv[]) {
    int rank, size, a, b, message, tag = 201;
    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);
    a = (rank + 1) % size;            /* tetangga kanan (next) */
    b = (rank + size - 1) % size;     /* tetangga kiri (prev)  */
    if (0 == rank) {
        message = 10;
        MPI_Send(&message, 1, MPI_INT, a, tag, MPI_COMM_WORLD);
    }
    while (1) {
        MPI_Recv(&message, 1, MPI_INT, b, tag, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        if (0 == rank)
            --message;
        MPI_Send(&message, 1, MPI_INT, a, tag, MPI_COMM_WORLD);
        if (0 == message)
            break;
    }
    if (0 == rank)
        MPI_Recv(&message, 1, MPI_INT, b, tag, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
    MPI_Finalize();
    return 0;
}
```

**✅ Jawaban:** Program mengedarkan **token** berkeliling **cincin** proses (0 → 1 → … → size−1 → 0). Token awal 10 dikurangi 1 setiap melewati rank 0, sampai menjadi 0. Task **tidak selesai bersama-sama**: mereka keluar **berurutan mengikuti cincin** setelah meneruskan token 0 (rank 1, 2, …, size−1), dan **rank 0 terakhir** setelah menerima token 0 kembali.

**📝 Pembahasan:**
Program ini identik dengan **UTS 2015/2016 no. 3** (lihat simulasi lengkap di sana). Ringkasan untuk size proses:
1. Rank 0 menyuntikkan token 10 ke rank 1.
2. Setiap proses menunggu (`MPI_Recv` dari tetangga kiri), lalu meneruskan token ke kanan. Hanya rank 0 yang mengurangi nilainya.
3. Token berkeliling 10 putaran (nilai 10, 9, …, 1) ditambah 1 putaran terakhir bernilai 0. Setiap proses melakukan 11 Recv dan 11 Send.
4. **Cara keluar:** saat rank 0 mengubah token menjadi 0, ia mengirimkannya lalu keluar dari loop. Rank 1 menerima 0, meneruskan, keluar; demikian seterusnya hingga rank size−1 yang mengirim 0 ke rank 0 lalu keluar. Rank 0 menerima 0 itu dengan `MPI_Recv` tambahan (agar tidak ada pesan tertinggal), lalu semua memanggil `MPI_Finalize`.
5. Jadi task-task **saling menunggu secara berantai**: setiap proses hanya bisa keluar setelah menerima token 0 dari tetangga kirinya.

**💡 Konsep:** point-to-point & pola ring (slide 03).

---

### Soal 4 · Gather/Allgather & Reduce/Allreduce
**Soal:** Jelaskan perbedaan antara:
a. `MPI_Gather` vs `MPI_Allgather`, dan berikan ilustrasi (gambar).
b. `MPI_Reduce` vs `MPI_Allreduce`, dan berikan ilustrasi (gambar).

**✅ Jawaban:**
a. Gather mengumpulkan potongan data semua proses **hanya ke root**; Allgather mengumpulkannya ke **semua** proses.
b. Reduce menggabungkan data dengan operator dan hasilnya **hanya di root**; Allreduce memberikan hasil reduksi ke **semua** proses.

**📝 Pembahasan:**
```
(a) MPI_Gather (root P0)                   MPI_Allgather
    P0 [A] ─┐                               P0 [A] ─┐      ┌▶ P0 [A B C D]
    P1 [B] ─┼──▶ P0 [A B C D]               P1 [B] ─┼──────┼▶ P1 [A B C D]
    P2 [C] ─┤    (P1–P3 tidak dapat apa-apa) P2 [C] ─┤      ├▶ P2 [A B C D]
    P3 [D] ─┘                               P3 [D] ─┘      └▶ P3 [A B C D]

(b) MPI_Reduce (MPI_SUM, root P0)          MPI_Allreduce (MPI_SUM)
    P0  3 ─┐                                P0  3 ─┐      ┌▶ P0 10
    P1  1 ─┼──▶ P0: 3+1+4+2 = 10            P1  1 ─┼──────┼▶ P1 10
    P2  4 ─┤                                P2  4 ─┤      ├▶ P2 10
    P3  2 ─┘                                P3  2 ─┘      └▶ P3 10
```
| | Root? | Ada operasi? | Hasil di |
|---|---|---|---|
| `MPI_Gather` | Ya | Tidak (konkatenasi) | Root saja |
| `MPI_Allgather` | Tidak | Tidak | Semua proses (= Gather + Bcast) |
| `MPI_Reduce` | Ya | Ya (SUM, MAX, …) | Root saja |
| `MPI_Allreduce` | Tidak | Ya | Semua proses (= Reduce + Bcast, atau pola butterfly) |

**💡 Konsep:** collective communication (slide 03).

---

### Soal 5 · OpenMP `parallel` + `for`
**Soal:** Pada OpenMP, pragma PARALLEL dan FOR kadang-kadang bisa dituliskan jadi satu, kadang-kadang dituliskan terpisah (di bawahnya). Jelaskan kapan dapat dituliskan dalam satu baris (jadi satu) dan kapan TIDAK boleh dituliskan dalam satu baris, berikan contoh.

**✅ Jawaban:** **Satu baris** (`#pragma omp parallel for`) jika region paralel **hanya berisi satu loop for**. **Harus terpisah** (`#pragma omp parallel` { … `#pragma omp for` … }) jika di dalam region ada **lebih dari satu loop**, ada **kode lain selain loop** (variabel private, single, critical setelah loop), atau tim thread ingin **dipakai ulang** di dalam loop luar yang serial.

**📝 Pembahasan:**
- `#pragma omp parallel` = **membuat tim thread** (fork) untuk blok berikutnya.
- `#pragma omp for` = **membagi iterasi** loop ke thread dalam tim yang **sudah ada** (worksharing). Tanpa region parallel di sekelilingnya, `omp for` hanya dijalankan 1 thread.
- `#pragma omp parallel for` = singkatan keduanya, hanya untuk blok yang berisi **tepat satu** loop for.

```c
/* Boleh satu baris: region hanya berisi 1 loop */
#pragma omp parallel for reduction(+:sum)
for (i = 0; i < n; i++) sum += a[i];

/* Harus terpisah: ada variabel per-thread, dua loop, dan critical di akhir */
#pragma omp parallel
{
    double my_max = -1e30;                   /* kode non-loop, private per thread */
    #pragma omp for
    for (i = 0; i < n; i++) b[i] = 2 * a[i];
    #pragma omp for
    for (i = 0; i < n; i++) if (b[i] > my_max) my_max = b[i];
    #pragma omp critical
    if (my_max > global_max) global_max = my_max;
}

/* Harus terpisah demi efisiensi: odd-even sort, tim dibuat sekali di luar loop fase */
#pragma omp parallel num_threads(t) default(none) shared(a, n) private(i, tmp, phase)
for (phase = 0; phase < n; phase++) {
    if (phase % 2 == 0) {
        #pragma omp for
        for (i = 1; i < n; i += 2) { /* compare-swap a[i-1], a[i] */ }
    } else {
        #pragma omp for
        for (i = 1; i < n-1; i += 2) { /* compare-swap a[i], a[i+1] */ }
    }
}
```
Data slide (odd-even sort, 4 thread): dua `parallel for` 0,305 s vs `parallel` + dua `for` **0,239 s**, karena fork/join hanya terjadi sekali.

**💡 Konsep:** parallel, for, parallel for (slide 05).

---

### Soal 6 · Schedule
**Soal:** Alokasi task ke masing-masing thread dilakukan dengan tambahan perintah `SCHEDULE(type, chunksize)`:
a. Sebutkan macam-macam type schedule dan jelaskan.
b. Jika ada array 12 elemen berisi bilangan bulat 1, 2, 3, …, 12 akan dialokasikan pada 3 thread, berikan ilustrasi (gambar) dari: i. `schedule(STATIC,1)`, ii. `schedule(STATIC,2)`, iii. `schedule(STATIC,4)`.

**✅ Jawaban:**
a. **static**, **dynamic**, **guided**, **auto**, **runtime** (penjelasan di tabel).
b.
```
                     Thread 0          Thread 1          Thread 2
i.   STATIC,1   :    1  4  7  10       2  5  8  11       3  6  9  12
ii.  STATIC,2   :    1  2  7  8        3  4  9  10       5  6  11 12
iii. STATIC,4   :    1  2  3  4        5  6  7  8        9  10 11 12
```

**📝 Pembahasan:**
a.
| Type | Penjelasan |
|---|---|
| `static` | Iterasi dibagi **sebelum loop berjalan** dalam potongan berukuran chunksize, dibagikan bergiliran (round-robin). Tanpa chunksize: dibagi rata menjadi blok berurutan (±n/t per thread). Overhead runtime paling kecil |
| `dynamic` | Iterasi dipecah menjadi potongan chunksize; thread yang selesai **meminta potongan berikutnya** saat runtime. Default chunksize = 1. Seimbang untuk beban tidak seragam, tetapi ada overhead tiap permintaan |
| `guided` | Seperti dynamic, tetapi ukuran potongan **mengecil** seiring berjalannya loop (±sisa/jumlah thread), sampai 1 atau sampai chunksize |
| `auto` | Compiler dan/atau runtime memilih jadwal |
| `runtime` | Jadwal ditentukan saat runtime dari variabel lingkungan `OMP_SCHEDULE` (mis. `export OMP_SCHEDULE="dynamic,4"`) |

b. Cara membaginya: potong array menjadi chunk berukuran chunksize, lalu bagikan chunk ke thread 0, 1, 2, 0, 1, 2, … secara bergiliran.
- STATIC,1: chunk {1},{2},{3},{4},… → T0 mendapat 1, 4, 7, 10.
- STATIC,2: chunk {1,2},{3,4},{5,6},{7,8},{9,10},{11,12} → T0 = {1,2},{7,8}.
- STATIC,4: chunk {1–4},{5–8},{9–12} → masing-masing satu chunk (sama dengan static default untuk 12/3).

**💡 Konsep:** scheduling loops (slide 05).

---

### Soal 7 · Divergence reduksi naif (stride = 1)
**Soal:** Untuk potongan kode kernel untuk operasi reduksi berikut ini, jika ukuran block size adalah 1024 dan ukuran warp 32, ada berapa warp dalam sebuah blok yang akan memiliki divergence saat iterasi dengan stride bernilai 1?
```c
unsigned int t = threadIdx.x;
unsigned int start = 2 * blockIdx.x * blockDim.x;
partialSum[t] = input[start + t];
partialSum[blockDim.x + t] = input[start + blockDim.x + t];
for (unsigned int stride = 1; stride <= blockDim.x; stride *= 2) {
    __syncthreads();
    if (t % stride == 0) { partialSum[2*t] += partialSum[2*t + stride]; }
}
```
a. 0  b. 1  c. 16  d. 32

**✅ Jawaban:** **a. 0**

**📝 Pembahasan:** blok 1024 thread = 1024/32 = **32 warp**. Pada stride = 1, kondisi `t % 1 == 0` **benar untuk semua t** → semua 32 thread di setiap warp mengambil cabang yang sama → **tidak ada divergensi**. (Divergensi = sebagian thread dalam satu warp masuk `if`, sebagian tidak.)

**💡 Konsep:** warp, SIMT, control divergence.

---

### Soal 8 · Divergence reduksi naif (stride = 16)
**Soal:** Pada pertanyaan di atas, ada berapa warp dalam sebuah blok yang memiliki divergence saat iterasi dengan stride bernilai 16?
a. 0  b. 1  c. 16  d. 32

**✅ Jawaban:** **d. 32**

**📝 Pembahasan:** Pada stride = 16, hanya thread dengan `t % 16 == 0` yang aktif. Di setiap warp (thread 32w … 32w+31) ada tepat **2 thread aktif** (32w dan 32w+16) dan 30 tidak aktif → **setiap warp divergen** → 32 warp. Inilah kelemahan reduksi naif: begitu stride ≥ 2, semua warp divergen. Versi yang diperbaiki (`if (t < stride)` dengan stride menurun) membuat thread aktif selalu berkelompok di awal sehingga warp penuh aktif atau penuh diam.

| stride | Thread aktif per warp | Warp divergen |
|---|---|---|
| 1 | 32 | 0 |
| 2, 4, 8, 16, 32 | 16, 8, 4, 2, 1 | 32 |
| 64 | 1 (warp genap) / 0 (warp ganjil) | 16 |

**💡 Konsep:** control divergence pada reduksi.

---

### Soal 9 · Inisialisasi indeks 2 elemen per thread
**Soal:** Misal kita ingin menggunakan setiap thread untuk menghitung 2 (adjacent) elemen output dari penjumlahan vektor. Variabel i diinisialisasi dengan indeks elemen pertama yang harus diproses oleh sebuah thread. Manakah kode berikut ini yang harus digunakan untuk inisialisasi variabel i agar akses memori benar dan coalesced untuk perintah `if (i < n) C[i] = A[i] + B[i];`?
a. `int i = (blockIdx.x*blockDim.x)*2 + threadIdx;`
b. `int i = (blockIdx.x*blockDim.x + threadIdx.x)*2;`
c. `int i = (threadIdx.x*blockDim.x)*2 + blockIdx.x;`
d. `int i = (threadIdx.x*blockDim.x + blockIdx.x)*2;`

**✅ Jawaban:** **a** (dengan `threadIdx.x`). 🟨 Jika "adjacent" dimaksud literal → **b**.

**📝 Pembahasan:**
Soal ini mencampur dua latihan buku Kirk & Hwu:
- **Versi coalesced (cocok dengan kata "coalesced" di soal):** setiap block mengolah 2·blockDim.x elemen berurutan yang dibagi dua "seksi". Thread t mengolah elemen ke-t di seksi pertama, lalu elemen ke-t di seksi kedua. Indeks pertama: `i = blockIdx.x*blockDim.x*2 + threadIdx.x` = **opsi a** (opsi a salah ketik `threadIdx` tanpa `.x`). Thread berurutan → alamat berurutan → **coalesced penuh** pada kedua akses.
- **Versi adjacent literal:** thread t mengolah elemen 2t dan 2t+1: `i = (blockIdx.x*blockDim.x + threadIdx.x)*2` = **opsi b**. Benar secara hasil, tetapi akses antar-thread berjarak 2 → hanya setengah efisien (kurang coalesced).
- c dan d salah: indeks bergantung terbalik pada threadIdx.x dan blockIdx.x sehingga thread berurutan melompat sejauh blockDim.x·2 → tidak coalesced, dan sebagian elemen tidak terjangkau.

Karena soal menekankan "coalesced", pilih **a** dan tuliskan alasan singkat ini.

**💡 Konsep:** indeks thread & coalescing.

---

### Soal 10 · Elemen kedua
**Soal:** Melanjutkan pertanyaan sebelumnya, apakah statement yang benar untuk setiap thread untuk memproses elemen kedua?
a. `if (i < n) C[i+1] = A[i+1] + B[i+1];`
b. `if (i+1 < n) C[i+1] = A[i+1] + B[i+1];`
c. `if (i+threadIdx.x < n) C[i+threadIdx.x] = A[i+threadIdx.x] + B[i+threadIdx.x];`
d. `if (i+blockDim.x < n) C[i+blockDim.x] = A[i+blockDim.x] + B[i+blockDim.x];`

**✅ Jawaban:** **d** (pasangan dari jawaban no. 9 = a). 🟨 Jika no. 9 dijawab b → **b**.

**📝 Pembahasan:**
- Versi coalesced: elemen kedua berada di seksi kedua, tepat **blockDim.x** setelah elemen pertama → `i + blockDim.x`, dan pengecekan batas harus memakai indeks yang **sama** dengan yang diakses → **d**.
- Versi adjacent: elemen kedua = `i + 1`, cek `i + 1 < n` → **b**.
- a salah: memeriksa `i < n` tetapi mengakses `i+1` → bisa keluar batas ketika i = n−1.
- c salah: offset threadIdx.x membuat thread berbeda mengakses elemen yang bertumpuk/terlewat.

**💡 Konsep:** boundary check & coalescing.

---

## UTS 2019/2020

### Soal 1 · [Umum] Speedup & scalability
**Soal:** Suatu program paralel mempunyai bagian yang tidak bisa diparalelkan sebanyak 20%, sedangkan 80% bisa dikodekan secara paralel. Misalkan program tersebut dijalankan di komputer single core dan waktu eksekusinya T_ser = n (n adalah problem size; diasumsikan program berjalan seperti algoritma serial). Jika program tersebut dijalankan pada multicore sebanyak p, maka:
a. Hitunglah T_par dan speedup (S) dari program tersebut (asumsi overhead = 0).
b. Andaikan program bisa diparalelkan 100%, tetapi ada tambahan overhead 1. Jika jumlah core ditambah sebanyak k kali (menjadi kp processor), apakah program tersebut scalable? Jika ya, apakah strongly scalable atau weakly scalable? (Tunjukkan dengan perhitungan.)

**✅ Jawaban:**
a. **T_par = 0,2n + 0,8n/p**, **S = 5p/(p + 4)**.
b. E = n/(n + p). **Scalable, tetapi hanya weakly scalable** (efisiensi tetap jika n dinaikkan k kali bersama p; turun jika n tetap).

**📝 Pembahasan:**
a.
```
T_par = 0,2·n + 0,8·n/p
S     = T_ser / T_par = n / (0,2n + 0,8n/p) = 1 / (0,2 + 0,8/p) = p / (0,2p + 0,8) = 5p / (p + 4)
E     = S/p = 5 / (p + 4)
Cek: p = 1 → S = 1;  p = 4 → S = 2,5;  p → ∞ → S → 5
```
b.
```
T_par = n/p + 1
S     = n / (n/p + 1) = np / (n + p)
E     = S/p = n / (n + p)

(1) Core × k, n tetap:      E' = n / (n + kp)          < n/(n + p)   → efisiensi turun → TIDAK strongly scalable
(2) Core × k, n × k:        E' = kn / (kn + kp) = n / (n + p)        → efisiensi tetap → WEAKLY scalable
```
Kesimpulan: program **scalable**, khususnya **weakly scalable**: efisiensi dapat dipertahankan jika ukuran masalah dinaikkan dengan laju yang sama dengan jumlah core.

**💡 Konsep:** speedup, efisiensi, overhead, strong/weak scalability (slide 02).

---

### Soal 2 · [Umum] False sharing
**Soal:** Jelaskan bagaimana terjadinya false sharing, berikan contoh/ilustrasi. False sharing dapat menurunkan performansi; jelaskan alasannya.

**✅ Jawaban:** False sharing terjadi ketika **thread-thread di core berbeda menulis variabel yang berbeda tetapi terletak dalam satu cache line yang sama**. Karena koherensi cache bekerja per cache line, setiap tulisan meng-**invalidate** salinan cache line itu di core lain → core lain mengalami cache miss dan harus mengambil ulang line tersebut → lalu lintas memori/bus naik dan kinerja turun, padahal tidak ada data yang benar-benar dibagi.

**📝 Pembahasan:**
Ilustrasi (cache line 64 byte = 8 double):
```
             cache line yang sama
      ┌──────┬──────┬──────┬──────┬──────┬──────┬──────┬──────┐
      │ y[0] │ y[1] │ y[2] │ y[3] │ y[4] │ y[5] │ y[6] │ y[7] │
      └──▲───┴──────┴──▲───┴──────┴──▲───┴──────┴──▲───┴──────┘
     Thread0       Thread1       Thread2       Thread3   (masing-masing menulis elemennya sendiri)

1. Thread 0 (core 0) menulis y[0] → cache line di core 1, 2, 3 di-invalidate
2. Thread 1 (core 1) menulis y[2] → cache miss → ambil ulang line → line di core 0, 2, 3 di-invalidate
3. … cache line "ping-pong" antar core terus-menerus
```
Contoh slide: perkalian matriks–vektor Pthreads 8 × 8.000.000. Vektor y hanya 8 elemen (satu cache line) dan setiap thread berulang kali menulis `y[i]` miliknya → efisiensi 4 thread hanya **0,29**, sedangkan untuk 8000 × 8000 efisiensinya 0,75.

**Mengapa menurunkan kinerja:** setiap tulisan memicu invalidasi + pengambilan ulang (*coherence miss*) yang jauh lebih lambat daripada akses cache biasa; bus/interconnect menjadi sibuk oleh lalu lintas koherensi; akhirnya thread-thread efektif berjalan bergiliran.

**Cara mengatasi:** akumulasi ke variabel lokal lalu tulis sekali, *padding* agar data tiap thread berada di cache line berbeda, atau partisi data sehingga tiap thread menulis blok besar yang berbeda.

**💡 Konsep:** cache coherence, false sharing (slide 02 & 04).

---

### Soal 3 · [MPI] Odd-even transposition sort
**Soal:** Diberikan potongan program transposition sort dengan MPI seperti di bawah:
```c
void Odd_even_iter(int local_A[], int temp_B[], int temp_C[], int local_n, int phase,
                   int even_partner, int odd_partner, int my_rank, int p, MPI_Comm comm) {
    MPI_Status status;
    if (phase % 2 == 0) {
        if (even_partner >= 0) {
            MPI_Sendrecv(local_A, local_n, MPI_INT, even_partner, 0,
                         temp_B,  local_n, MPI_INT, even_partner, 0, comm, &status);
            if (my_rank % 2 != 0)
                Merge_high(local_A, temp_B, temp_C, local_n);
            else
                Merge_low(local_A, temp_B, temp_C, local_n);
        }
    } else {
        /* ... dst ... */
```
a. Jika `MPI_Sendrecv()` diganti dengan `MPI_Send()` dan `MPI_Recv()`, lengkapi (i) s.d. (iv):
```c
if (phase % 2 == 0) {
    if (even_partner >= 0) {
        if (my_rank % 2 != 0) {
            (i) ...........
            (ii) ..........
            Merge_high(local_A, temp_B, temp_C, local_n);
        } else {
            (iii) .........
            (iv) ..........
            Merge_low(local_A, temp_B, temp_C, local_n);
        }
    }
} else { /* ... dst ... */ }
```
b. Perintah `MPI_Sendrecv()` adalah thread-safe; jelaskan yang dimaksud dengan thread-safe.

**✅ Jawaban:**
a.
```c
(i)   MPI_Recv(temp_B,  local_n, MPI_INT, even_partner, 0, comm, &status);
(ii)  MPI_Send(local_A, local_n, MPI_INT, even_partner, 0, comm);
(iii) MPI_Send(local_A, local_n, MPI_INT, even_partner, 0, comm);
(iv)  MPI_Recv(temp_B,  local_n, MPI_INT, even_partner, 0, comm, &status);
```
b. **Thread-safe** = kode/fungsi dapat dijalankan **bersamaan oleh beberapa thread tanpa menimbulkan masalah** (hasil tetap benar, tidak ada race condition atau state bersama yang saling menimpa).

**📝 Pembahasan:**
a. Pada fase genap, pasangannya (rank genap, rank ganjil) = (0,1), (2,3), …. Jika **keduanya** memanggil `MPI_Send` lebih dulu dan Send-nya block (pesan besar/tidak di-buffer), keduanya menunggu Recv yang tidak pernah dipanggil → **deadlock**. Maka urutan harus **berlawanan**: rank ganjil (i, ii) **Recv dulu lalu Send**, rank genap (iii, iv) **Send dulu lalu Recv**. (Kebalikannya — ganjil Send dulu, genap Recv dulu — juga benar.)
Merge_high/Merge_low tetap setelah data diterima: rank lebih kecil (genap) menyimpan setengah kunci terkecil, rank lebih besar (ganjil) menyimpan setengah terbesar.

b. Definisi slide: *"A block of code is thread-safe if it can be simultaneously executed by multiple threads without causing problems."* Contoh tidak thread-safe: `strtok` (menyimpan posisi di variabel `static` bersama), `random`, `localtime`; versi thread-safe: `strtok_r`.
Dalam konteks soal, keunggulan utama `MPI_Sendrecv` sebenarnya adalah **"safe"**: MPI sendiri menjadwalkan pengiriman dan penerimaan sehingga program **tidak deadlock**, apa pun perilaku buffering `MPI_Send`. (Pemanggilan MPI dari banyak thread sekaligus memerlukan dukungan level `MPI_THREAD_MULTIPLE`.)

**💡 Konsep:** odd-even transposition sort, safety MPI, `MPI_Sendrecv`, thread safety (slide 03, 04, 05c).

---

### Soal 4 · [Shared Memory] Linked list paralel
**Soal:** Diberikan suatu linked list dengan serangkaian akses paralel terhadap struktur data tersebut untuk proses IsMember (mengecek elemen pada list), Insert (menambah elemen baru), dan Delete (menghapus suatu elemen). Manakah dari kasus di bawah ini yang bila dilakukan secara paralel berpotensi mengakibatkan masalah terkait critical section (I)? Sertakan penjelasan (II) serta jelaskan bagaimana pemrograman menggunakan PThread menangani persoalan tersebut (III).
a. Dua pemanggilan IsMember. b. Proses Insert dan Delete. c. Proses Delete dan IsMember. d. Proses IsMember dan Insert.
Bonus (IV): Apakah istilah umum dari problem sinkronisasi tersebut?

**✅ Jawaban:** (I) **b, c, d**. (III) mutex untuk seluruh list, mutex per node, atau **read-write lock** (terbaik bila dominan IsMember). (IV) ***Readers–writers problem***.

**📝 Pembahasan (II):**
| Kasus | Bermasalah? | Penjelasan |
|---|---|---|
| a. IsMember + IsMember | ❌ Tidak | Keduanya hanya **membaca** pointer dan data; tidak ada yang mengubah list |
| b. Insert + Delete | ✅ Ya | Keduanya **mengubah** pointer `next` di sekitar posisi yang sama (mis. Insert menambahkan node setelah node 5 sementara Delete menghapus node 5) → node baru hilang atau list terputus |
| c. Delete + IsMember | ✅ Ya | IsMember sedang berada di node yang di-`free` oleh Delete → akses memori yang sudah dibebaskan (crash/hasil salah) |
| d. IsMember + Insert | ✅ Ya | IsMember bisa membaca pointer `next` yang sedang diubah → melewatkan node atau membaca node yang belum lengkap |

Aturannya: **masalah muncul jika minimal satu operasi menulis** (Insert/Delete) pada struktur yang sedang dipakai operasi lain.

**(III) Penanganan Pthreads (slide 04):**
1. **Satu mutex untuk seluruh list** — mudah, tetapi semua operasi menjadi serial (termasuk IsMember yang sebenarnya aman bersamaan).
2. **Satu mutex per node** (*fine-grained*) — lebih paralel, tetapi kode rumit, lebih lambat (lock/unlock setiap node), dan memori bertambah.
3. **Read-write lock** — `pthread_rwlock_rdlock` untuk IsMember (banyak boleh bersamaan), `pthread_rwlock_wrlock` untuk Insert/Delete (eksklusif). Paling cepat jika mayoritas operasi IsMember (99,9% Member, 4 thread: 0,098 s vs 0,385 s satu mutex).
```c
pthread_rwlock_rdlock(&rwlock);  IsMember(v);  pthread_rwlock_unlock(&rwlock);
pthread_rwlock_wrlock(&rwlock);  Insert(v);    pthread_rwlock_unlock(&rwlock);
pthread_rwlock_wrlock(&rwlock);  Delete(v);    pthread_rwlock_unlock(&rwlock);
```
**(IV)** Masalah sinkronisasi "banyak pembaca boleh bersamaan, penulis harus eksklusif" dikenal sebagai **readers–writers problem**.

**💡 Konsep:** critical section, read-write lock (slide 04).

---

### Soal 5 · [Shared Memory] critical vs atomic
**Soal:** Diberikan dua bagian kode program menggunakan OpenMP:
```c
/* I */                          /* II */
int count = 0;                   int count = 0;
#pragma omp critical             #pragma omp atomic
count++;                         count++;
```
a. Apakah terdapat perbedaan pada kedua hasil count? Berikan penjelasan!
b. Apa keuntungan penggunaan direktif atomic ketimbang critical?
c. Pada keadaan apa direktif critical lebih baik digunakan ketimbang atomic?

**✅ Jawaban:**
a. **Tidak ada perbedaan**: keduanya mencegah race condition sehingga count bernilai benar (= jumlah thread yang mengeksekusinya).
b. **Atomic lebih cepat/efisien**, karena memakai instruksi hardware *load-modify-store* tanpa mekanisme kunci umum.
c. Critical lebih baik (dan wajib) jika critical section **lebih dari satu update sederhana**: beberapa pernyataan, ada kondisi `if`, update beberapa variabel, pemanggilan fungsi, atau I/O.

**📝 Pembahasan:**
- `#pragma omp critical` melindungi **blok apa pun** dengan semacam mutex global (semua critical tanpa nama berbagi satu kunci).
- `#pragma omp atomic` hanya untuk **satu** pernyataan berbentuk `x op= expr`, `x++`, `++x`, `x--`, `--x` (op: + * − / & ^ | << >>). Banyak prosesor punya instruksi khusus untuk ini sehingga jauh lebih murah.
- Contoh yang **harus** critical: `if (a[i] > max) max = a[i];` (cek + tulis), `Enqueue(q, msg);`, `sum += x; count++;` (dua variabel).
- Jangan mencampur atomic dan critical untuk variabel yang sama — keduanya tidak saling mengecualikan.

**💡 Konsep:** critical, atomic (slide 05).

---

### Soal 6 · [GPU] Coalescing perkalian matriks
**Soal:** Diberikan kode perkalian matriks berikut:
```c
__global__ void MatrixMulKernel(float* M, float* N, float* P, int Width) {
    int Row = blockIdx.y*blockDim.y + threadIdx.y;
    int Col = blockIdx.x*blockDim.x + threadIdx.x;
    if ((Row < Width) && (Col < Width)) {
        float Pvalue = 0;
        for (int k = 0; k < Width; ++k)
            Pvalue += M[Row*Width+k] * N[k*Width+Col];
        P[Row*Width+Col] = Pvalue;
    }
}
```
Pilihlah mana yang benar:
a. `M[Row*Width+k]` dan `N[k*Width+Col]` coalesced, tetapi `P[Row*Width+Col]` tidak.
b. `M[Row*Width+k]`, `N[k*Width+Col]`, dan `P[Row*Width+Col]` semua coalesced.
c. `M[Row*Width+k]` tidak coalesced, tetapi `N[k*Width+Col]` dan `P[Row*Width+Col]` coalesced.
d. `M[Row*Width+k]` coalesced, tetapi `N[k*Width+Col]` dan `P[Row*Width+Col]` tidak.

**✅ Jawaban:** **c**

**📝 Pembahasan:**
Coalesced = thread-thread berurutan dalam satu warp (threadIdx.x berurutan → Col berurutan) mengakses **alamat memori berurutan**, sehingga digabung menjadi sedikit transaksi.
- `N[k*Width + Col]`: pada iterasi k yang sama, thread berurutan mengakses kolom berurutan pada baris k → alamat berurutan → **coalesced**.
- `P[Row*Width + Col]`: thread berurutan menulis kolom berurutan → **coalesced**.
- `M[Row*Width + k]`: indeksnya **tidak bergantung pada threadIdx.x**. Thread berurutan tidak mengakses alamat berurutan; setiap thread berjalan menyusuri baris M sepanjang iterasi k (pola "per thread", bukan "antar thread") → dalam analisis Kirk & Hwu dikategorikan **tidak coalesced**. (Secara teknis, thread pada baris yang sama membaca alamat yang sama sehingga hardware modern melakukan broadcast; tetapi jawaban yang diharapkan tetap c.)
- Inilah alasan tiling memuat tile M ke shared memory secara kolaboratif dengan pola yang coalesced.

**💡 Konsep:** memory coalescing (slide 06a / Kirk & Hwu).

---

### Soal 7 · [GPU] Jumlah warp
**Soal:** Diberikan kode berikut untuk mengolah gambar 600×800 (800 piksel horizontal dan 600 piksel vertikal), di mana 600 adalah parameter m dan 800 parameter n.
```c
__global__ void PictureKernel(float* d_Pin, float* d_Pout, int n, int m) {
    int Row = blockIdx.y*blockDim.y + threadIdx.y;
    int Col = blockIdx.x*blockDim.x + threadIdx.x;
    if ((Row < m) && (Col < n)) {
        d_Pout[Row*n+Col] = 2*d_Pin[Row*n+Col];
    }
}
```
Jika digunakan blok berukuran 16×16, tentukan jumlah warp yang digunakan saat eksekusi kernel di atas.
a. 37*16  b. 38*50  c. 38*8*50  d. 38*50*2

**✅ Jawaban:** **c. 38 × 8 × 50 = 15.200 warp**

**📝 Pembahasan:**
```
Blok arah x (kolom, n = 800):  ⌈800/16⌉ = 50
Blok arah y (baris, m = 600):  ⌈600/16⌉ = ⌈37,5⌉ = 38      ← dibulatkan ke atas agar semua baris tercakup
Jumlah blok                  :  50 × 38 = 1900
Warp per blok                :  16 × 16 = 256 thread / 32 = 8
Total warp                   :  38 × 50 × 8 = 15.200
```
- a (37·16) salah: 37 blok tidak cukup menutupi 600 baris, dan 16 bukan jumlah warp per blok.
- b (38·50) hanya jumlah **blok**, bukan warp.
- d (38·50·2) memakai 2 warp/blok — salah, 256 thread = 8 warp.
Thread di baris 600–607 (blok baris terakhir) tetap diluncurkan, tetapi tidak bekerja karena `if (Row < m)`.

**💡 Konsep:** konfigurasi grid/blok, warp.

---

### Soal 8 · [GPU] Versi variabel shared
**Soal:** Sebuah kernel dijalankan dengan menggunakan 1000 thread block yang masing-masing terdiri atas 512 thread. Jika sebuah variabel dideklarasikan sebagai shared memory variable, ada berapa versi variabel tersebut selama eksekusi kernel?
a. 1  b. 1000  c. 512  d. 512000

**✅ Jawaban:** **b. 1000**

**📝 Pembahasan:** variabel `__shared__` berlingkup **per block**: setiap block mendapat **satu salinan** yang dipakai bersama oleh semua (512) thread di block itu. 1000 block → 1000 versi.
- 1 = jika `__device__`/global (satu untuk seluruh aplikasi).
- 512000 = jika variabel lokal biasa (register, satu per thread).

**💡 Konsep:** CUDA memory model.

---

### Soal 9 · [GPU] Reduksi bandwidth tiling
**Soal:** Pada kernel perkalian matriks di GPU dengan menggunakan tiling, jika digunakan tile berukuran 32×32, berapakah reduksi/pengurangan bandwidth memory yang digunakan untuk input matriks A dan B?
a. 1/8 dari penggunaan awal  b. 1/16  c. 1/32  d. 1/1024

**✅ Jawaban:** **c. 1/32**

**📝 Pembahasan:** tanpa tiling, setiap elemen A dan B dibaca dari global memory oleh **setiap** thread yang membutuhkannya. Dengan tile T×T, satu block memuat setiap elemen tile **sekali** ke shared memory, lalu elemen itu dipakai oleh **T** thread (sebaris/sekolom) → akses global memory berkurang menjadi **1/T** = 1/32. (Bukan 1/1024: walaupun tile berisi 1024 elemen, setiap elemen hanya dipakai ulang 32 kali.)

**💡 Konsep:** tiled matrix multiply, shared memory.

---
## UTS 2020/2021
*(Modul 1, pilihan ganda. Kode ditulis ulang; salah ketik di bundel seperti `argy`, `omp_get_thread_num(1)`, `sum - sum + i`, `_device_` sudah diperbaiki.)*

### Soal 1 · OpenMP schedule & waktu thread
**Soal:** Diberikan kode program di bawah. Jika `OMP_SCHEDULE=static` dan nilai max yang dimasukkan adalah 14, manakah jawaban yang benar? (hint: `sleep(1)` membuat thread tidur selama 1 detik)
```c
#include <stdio.h>
#include <unistd.h>
#include <omp.h>
int main(int argc, char *argv[]) {
    int max; sscanf(argv[1], "%d", &max);
    long int sum = 0;
    #pragma omp parallel for reduction(+: sum) schedule(runtime)
    for (int i = 1; i <= max; i++) {
        printf("%2d @ %d\n", i, omp_get_thread_num());
        sleep(i < 4 ? i + 1 : 1);
        sum = sum + i;
    }
    printf("%ld\n", sum);
    return 0;
}
```
a. Thread dengan rank 0 akan berjalan selama 10 detik
b. Thread dengan rank 1 akan berjalan selama 5 detik
c. Thread dengan rank 2 akan berjalan selama 4 detik
d. Thread dengan rank 3 akan berjalan selama 2 detik
e. Tidak ada jawaban yang benar

**✅ Jawaban:** **a**

**📝 Pembahasan:**
1. `schedule(runtime)` + `OMP_SCHEDULE=static` → static **tanpa chunk** (blok berurutan).
2. Jumlah thread tidak disebut; opsi menyebut sampai rank 3 → asumsikan **4 thread**.
3. Lama tidur: i = 1 → 2 s, i = 2 → 3 s, i = 3 → 4 s, i ≥ 4 → 1 s.
4. 14 iterasi / 4 thread: q = 3, sisa 2 → **dua thread pertama mendapat 4 iterasi**, sisanya 3 (cara GCC/libgomp).

| Thread | Iterasi | Lama |
|---|---|---|
| 0 | 1, 2, 3, 4 | 2 + 3 + 4 + 1 = **10 s** ✅ |
| 1 | 5, 6, 7, 8 | 4 s (bukan 5) |
| 2 | 9, 10, 11 | 3 s (bukan 4) |
| 3 | 12, 13, 14 | 3 s (bukan 2) |

Output akhir `sum` = 1 + 2 + … + 14 = **105** (benar karena reduction). Program total ±10 detik karena menunggu thread 0 (contoh **load imbalance** pada static).
🟨 Jika implementasi membagi chunk ⌈14/4⌉ = 4 (T2: 9–12 = 4 s, T3: 13–14 = 2 s), maka c dan d juga benar; untuk pilihan tunggal, jawaban yang konsisten adalah **a**.

**💡 Konsep:** schedule static/runtime, reduction, load imbalance.

---

### Soal 2 · CUDA grid-stride loop
**Soal:** Diberikan kode CUDA berikut (array_size adalah ukuran array a dan b, num_threads adalah jumlah total thread = `blockDim.x * gridDim.x`). Manakah jawaban yang benar?
```c
__device__ void prob(int array_size) {
    int tid = threadIdx.x + blockIdx.x * blockDim.x;
    for (int i = tid; i < array_size; i += num_threads)
        result[i] = a[i] + b[i];
}
```
a. Akses memori pada kode di atas tidak coalesced
b. Kode di atas sudah menangani boundary condition dengan sesuai
c. Jika ukuran blok diperbesar maka eksekusinya akan lebih cepat
d. Code divergence pada eksekusi kode di atas mengakibatkan eksekusinya tidak optimal
e. Pemanfaatan shared memory akan membuat eksekusi kode tersebut lebih cepat

**✅ Jawaban:** **b**

**📝 Pembahasan:** ini pola **grid-stride loop**: setiap thread mulai dari indeks globalnya lalu melompat sejauh jumlah total thread.
- **b benar:** kondisi `i < array_size` memastikan tidak ada akses di luar batas, berapa pun ukuran array dan jumlah thread.
- a salah: pada setiap putaran, thread berurutan mengakses i berurutan → **coalesced**.
- c salah: total kerja tetap; blok lebih besar tidak otomatis lebih cepat (bergantung occupancy).
- d salah: divergensi hanya terjadi pada putaran terakhir di sebagian warp (ketika sisa elemen tidak habis dibagi) → dampaknya kecil.
- e salah: setiap elemen a dan b hanya dibaca **sekali** (tidak ada penggunaan ulang data) → shared memory tidak membantu.

**💡 Konsep:** indeks thread, coalescing, boundary check.

---

### Soal 3 · MPI dengan 1 proses
**Soal:** Diberikan pseudocode MPI berikut. Manakah jawaban yang benar jika program dijalankan dengan 1 proses?
```
MPI_INIT()
MPI_COMM_RANK(MPI_COMM_WORLD, myrank, ierr)
if (myrank == 1)
    MPI_SEND(some data to processor 2 in MPI_COMM_WORLD)
else {
    MPI_RECV(data from processor 1 in MPI_COMM_WORLD)
    print "Message received!"
}
MPI_FINALIZE()
```
a. Hasil eksekusi "Message received!" dan program selesai
b. Program akan selesai normal tanpa output
c. Program akan hang tanpa output
d. Muncul error
e. Tidak ada jawaban di atas yang benar

**✅ Jawaban:** **d**

**📝 Pembahasan:** dengan 1 proses, satu-satunya rank adalah **0** → masuk cabang else → `MPI_RECV` dari **processor 1**, padahal `MPI_COMM_WORLD` hanya berisi rank 0. MPI memeriksa validitas argumen rank → error **`MPI_ERR_RANK`** (invalid rank) dan error handler default (`MPI_ERRORS_ARE_FATAL`) menghentikan program. "Message received!" tidak pernah tercetak.
(Jika dijalankan dengan ≥ 3 proses: rank 1 mengirim ke rank 2, sedangkan rank 0 dan 2 menunggu dari rank 1 → rank 2 menerima, tetapi rank 0 menunggu selamanya → hang.)

**💡 Konsep:** rank & communicator (slide 03).

---

### Soal 4 · Semantik MPI_Send
**Soal:** `MPI_SEND` digunakan untuk mengirim data array yang berukuran besar. Saat pemanggilan `MPI_SEND` selesai, programmer dapat mengasumsikan bahwa:
a. Proses destinasi telah menerima data tersebut
b. Array telah dikopi ke internal buffer
c. Array masih belum tentu telah dikopi ke internal buffer
d. Salah satu dari A atau B
e. Tidak ada jawaban di atas yang benar

**✅ Jawaban:** **d**

**📝 Pembahasan:** standar MPI mengizinkan `MPI_Send` (slide 03 "Safety in MPI programs"):
1. **menyalin pesan ke buffer internal MPI lalu kembali** (→ B), atau
2. **block sampai `MPI_Recv` pasangannya dimulai** dan data dikirim (→ A).

Ketika `MPI_Send` kembali, yang **pasti** hanyalah **buffer kirim boleh dipakai ulang**, karena salah satu dari dua hal itu sudah terjadi; programmer tidak tahu yang mana. Untuk array besar, implementasi biasanya memakai cara 2 (block), tetapi itu tidak dijamin standar. Jadi asumsi yang aman adalah "salah satu dari A atau B".

**💡 Konsep:** blocking send, buffering, safety (slide 03).

---

### Soal 5 · Reduksi pohon dengan OpenMP
**Soal:** Diberikan kode berikut. Manakah jawaban yang benar?
```c
for (int d = 1; d < ts; d = d * 2)
    #pragma omp parallel for
    for (int t = 0; t < ts; t = t + 2 * d)
        if (t + d < ts) sums[t] = sums[t] + sums[t + d];
```
a. Kode menghitung reduksi penjumlahan dan hasilnya disimpan pada sums[0]
b. Kode menghitung reduksi penjumlahan dan hasilnya disimpan pada sums[ts]
c. Kode menghitung 2 pangkat ts dan hasilnya disimpan pada sums[0]
d. Kode menghitung 2 pangkat ts dan hasilnya disimpan pada sums[d]
e. Kode menghitung 2 pangkat ts dan hasilnya disimpan pada sums[ts]

**✅ Jawaban:** **a**

**📝 Pembahasan:** simulasi ts = 8, sums = [s0 … s7]:
```
d = 1 : t = 0,2,4,6   → sums[0]=s0+s1, sums[2]=s2+s3, sums[4]=s4+s5, sums[6]=s6+s7
d = 2 : t = 0,4       → sums[0]+=sums[2] (s0..s3),  sums[4]+=sums[6] (s4..s7)
d = 4 : t = 0         → sums[0]+=sums[4]  = s0+s1+…+s7
```
Ini **penjumlahan pohon** (seperti contoh slide 01): log₂ts langkah, setiap langkah paralel. Iterasi di dalam satu langkah d menulis elemen berbeda dan membaca elemen yang tidak ditulis di langkah itu → aman diparalelkan. Hasil akhir ada di **sums[0]**. sums[ts] di luar batas array, dan tidak ada perhitungan pangkat.

**💡 Konsep:** tree-structured reduction, parallel for.

---

### Soal 6 · SIMD & vector processor
**Soal:**
Pernyataan I: Salah satu paralelisasi menggunakan komputer kelas SIMD adalah dengan memanfaatkan vector processor, yaitu perangkat yang memungkinkan eksekusi instruksi yang sama pada satu waktu terhadap beberapa data pada lokasi yang berbeda dalam memori.
**SEHINGGA**
Pernyataan II: Pemrograman paralel pada arsitektur SIMD dapat dilakukan dengan memanfaatkan komunikasi antar-proses melalui message passing menggunakan struktur producer–consumer pada proses-proses yang mengakses message buffer sebagai critical section.
a. I & II benar dan berhubungan  b. I & II benar dan tidak berhubungan  c. I benar, II salah  d. I salah, II benar  e. I & II salah

**✅ Jawaban:** **c**

**📝 Pembahasan:**
- **I benar:** vector processor = SIMD (satu instruksi vektor diterapkan ke banyak elemen data, dengan vector register, interleaved memory, strided access).
- **II salah:** message passing antar **proses**, producer–consumer, dan buffer sebagai critical section adalah ciri pemrograman **MIMD** (distributed memory / shared memory dengan thread independen). Pada SIMD hanya ada **satu aliran instruksi** yang dijalankan sinkron oleh semua ALU; tidak ada proses-proses independen yang saling berkirim pesan.

**💡 Konsep:** Flynn's taxonomy, SIMD, vector processor (slide 02).

---

### Soal 7 · Linked list paralel
**Soal:** Diberikan suatu linked list dengan serangkaian akses paralel untuk proses IsMember, Insert, dan Delete. Manakah dari kasus di bawah ini yang bila dilakukan secara paralel berpotensi mengakibatkan masalah terkait critical section? (boleh lebih dari satu)
- Dua pemanggilan isMember
- Proses Insert dan Delete
- Proses Delete dan IsMember
- Proses IsMember dan Insert
- Dua proses Delete

**✅ Jawaban:** **Insert & Delete; Delete & IsMember; IsMember & Insert; dua Delete.** (Hanya "dua isMember" yang aman.)

**📝 Pembahasan:** masalah terjadi jika **minimal satu operasi menulis** (mengubah pointer atau membebaskan node). Dua Delete bisa menghapus node yang sama/bersebelahan → double free atau pointer rusak. Lihat penjelasan lengkap di **UTS 2019/2020 no. 4**.

**💡 Konsep:** critical section, readers–writers.

---

### Soal 8 · critical vs atomic
**Soal:** Periksalah kedua kode program di bawah ini dan pilihlah pernyataan yang benar!
```c
/* I */                          /* II */
int count = 0;                   int count = 0;
#pragma omp critical             #pragma omp atomic
count++;                         count++;
```
1. Bila lingkungan implementasi dan cakupan program sama, kedua kode program di atas memberikan keadaan hasil yang sama untuk variabel count.
2. Program yang dibuat menggunakan kode I mengakibatkan eksekusi yang lebih cepat daripada kode II bila dijalankan pada lingkungan yang sama.
3. Kode program II lebih baik bila digunakan untuk operasi yang lebih kompleks.
4. Program dapat dioptimalkan dengan menggunakan kedua atribut pragma sekaligus.

**✅ Jawaban:** **Hanya pernyataan 1.**

**📝 Pembahasan:**
1. ✅ Keduanya mencegah race → count sama.
2. ❌ Terbalik: **atomic (II) lebih cepat** (instruksi hardware); critical memakai kunci umum.
3. ❌ Terbalik: atomic hanya untuk **satu update sederhana**; untuk operasi kompleks harus **critical (I)**.
4. ❌ Tidak bisa/tidak berguna; slide melarang mencampur jenis mutual exclusion untuk critical section yang sama.

**💡 Konsep:** critical, atomic (slide 05).

---

### Soal 9 & 10 · Memilih kolektif MPI
**Soal:** Periksa kode program MPI berikut:
```c
random_nums = create_random_numbers(number_of_elements_per_process);

// Penjumlahan lokal
float local_sum = 0;
int i;
for (i = 0; i < number_of_elements_per_process; i++)
    local_sum += random_nums[i];

// Hitung rata-rata dan simpan di global sum
float global_sum;
(I) _______(&local_sum, &global_sum, 1, MPI_FLOAT, MPI_SUM, MPI_COMM_WORLD);
float mean = global_sum / (num_elements_per_proc * process_size);

// Hitung local sum untuk selisih kuadrat dari rata-rata
float local_square_diff = 0;
for (i = 0; i < number_of_elements_per_process; i++)
    local_square_diff += (random_nums[i] - mean) * (random_nums[i] - mean);

// Hitung global sum sebelum dihitung nilai akarnya
float global_square_diff;
(II) _______(&local_square_diff, &global_square_diff, 1, MPI_FLOAT, MPI_SUM, 0, MPI_COMM_WORLD);

// Hitung deviasi standar
if (process_rank == 0) {
    float stddev = sqrt(global_square_diff / (num_elements_per_proc * process_size));
    printf("Rata-rata = %f, Deviasi Standar = %f\n", mean, stddev);
}
```
**No. 9:** Untuk bagian (I), komunikasi antar proses dapat dilakukan dengan pemanggilan… **No. 10:** Untuk bagian (II)…
a. MPI_GATHER  b. MPI_SCATTER  c. MPI_REDUCE  d. MPI_ALLREDUCE  e. MPI_ALLGATHER

**✅ Jawaban:** **No. 9 → d (MPI_ALLREDUCE)**, **No. 10 → c (MPI_REDUCE)**

**📝 Pembahasan:**
- **(I)** Argumen: `(&local_sum, &global_sum, 1, MPI_FLOAT, MPI_SUM, MPI_COMM_WORLD)` → ada **operator** (MPI_SUM) tetapi **tidak ada root** → `MPI_Allreduce`. Secara logika juga: `mean` dipakai **semua proses** untuk menghitung selisih kuadrat lokal, jadi semua proses harus menerima `global_sum`.
- **(II)** Argumen memiliki **root = 0** dan operator MPI_SUM → `MPI_Reduce`. Logikanya: deviasi standar hanya dihitung dan dicetak oleh rank 0.
- Gather/Allgather tidak memakai operator; Scatter membagi data, bukan menggabungkan.

**💡 Konsep:** Reduce vs Allreduce (slide 03).

---

### Soal 11 · Waktu pipeline multiple issue
**Soal:** Pipelining dengan multiple issue bisa meningkatkan kinerja dengan berkurangnya waktu eksekusi. Kinerja pipelining multiple issue biasanya ditentukan juga oleh nilai CPI. Jika diketahui CPI = 1/3, panjang cycle (jumlah tahap pipeline) = 7, dengan waktu 10⁻⁹ detik (1 ns) per tahap, maka waktu yang diperlukan untuk memproses array berisi 6000 elemen adalah:
a. 14.000 × 10⁻⁹  b. 6.006 × 10⁻⁹  c. 2.006 × 10⁻⁹  d. 42.000 × 10⁻⁹  e. Tidak ada jawaban yang benar

**✅ Jawaban:** **c. 2006 × 10⁻⁹ detik**

**📝 Pembahasan:**
```
CPI = 1/3  ⇒  issue width w = 3 (3 instruksi/elemen masuk per siklus)
Siklus = k + (N/w − 1) = 7 + (6000/3 − 1) = 7 + 1999 = 2006
Waktu  = 2006 × 1 ns = 2006 × 10⁻⁹ s
```
Perbandingan: tanpa multiple issue (pipeline biasa): 7 + 5999 = 6006 ns (opsi b); tanpa pipeline sama sekali: 6000 × 7 = 42.000 ns (opsi d).

**💡 Konsep:** pipelining & multiple issue (slide 02).

---

### Soal 12 · Send–Send / Recv–Recv
**Soal:** Diberikan potongan program MPI berikut. Dari kode program berikut, kemungkinan yang akan terjadi adalah:
```c
if (rank == 0) {
    MPI_Send(msg1 to process 1);
    MPI_Recv(msg2 from process 1);
}
if (rank == 1) {
    MPI_Send(msg2 to process 0);
    MPI_Recv(msg1 from process 0);
}
```
a. Proses 1 menerima msg1, setelah itu proses 0 menerima msg2.
b. Proses 0 menerima msg2, setelah itu proses 1 menerima msg1.
c. Proses 0 dan 1 menerima msg1 dan msg2 secara bersamaan.
d. Proses 0 dan 1 tidak menerima apa pun karena deadlock.
e. Tidak ada jawaban yang benar.

**✅ Jawaban:** **d**

**📝 Pembahasan:** kedua proses memanggil `MPI_Send` **lebih dulu**. Jika `MPI_Send` block (pesan besar atau implementasi tidak mem-buffer), proses 0 menunggu proses 1 memanggil Recv, sementara proses 1 juga menunggu proses 0 memanggil Recv → keduanya menunggu selamanya → **deadlock**. Program seperti ini disebut **unsafe**: bisa jalan jika pesannya kecil dan di-buffer, tetapi bisa macet untuk pesan besar. Perbaikan: balik urutan di salah satu proses, atau pakai `MPI_Sendrecv`.

**💡 Konsep:** safety in MPI programs (slide 03).

---

### Soal 13 · OpenMP single + task
**Soal:** Pada OpenMP, pragma single biasanya diikuti oleh pragma task, yang berarti satu task blok tersebut hanya dieksekusi oleh satu thread. Jika diberikan kode berikut, maka output yang mungkin adalah (asumsi program dieksekusi dengan 2 thread):
```c
#pragma omp parallel
{
    #pragma omp single
    {
        printf("Mulai ");
        #pragma omp task
        { printf("task1 "); }
        #pragma omp task
        { printf("task2 "); }
        printf("selesai mencetak ");
    }
}
```
a. "Mulai task1 task2 selesai mencetak"
b. "Mulai task1 task2 selesai mencetak" (dicetak 2 kali)
c. "Mulai task2 task1 selesai mencetak"
d. "Mulai selesai mencetak task2 task1"
e. "Mulai selesai mencetak task1 task2" (dicetak 2 kali)

**✅ Jawaban:** **a, c, dan d semuanya mungkin**; b dan e **tidak mungkin**. Jika hanya boleh satu, pilih **d**.

**📝 Pembahasan:**
- `single` → blok dijalankan **sekali** oleh salah satu thread → "Mulai" dan "selesai mencetak" hanya muncul **sekali** → b dan e mustahil.
- `task` hanya **membuat tugas**; tugas bisa dikerjakan oleh thread **mana pun** (misalnya thread kedua yang sedang menunggu di barrier implisit akhir `single`) dan **kapan pun** sebelum barrier tersebut. Maka task1/task2 bisa tercetak sebelum atau sesudah "selesai mencetak", dalam urutan apa pun.
- Kemungkinan lain juga sah, misalnya "Mulai task1 selesai mencetak task2".
- Jika soal menuntut satu jawaban: **d** adalah contoh khas yang menunjukkan sifat task (dibuat sekarang, dieksekusi belakangan), sama dengan contoh klasik OpenMP tasks ("A is fun to watch race car").

**💡 Konsep:** single, task, barrier implisit (OpenMP).

---

### Soal 14 · Variabel `__device__`
**Soal:** Pada CUDA, jika deklarasi variabel dengan `__device__` tanpa diikuti `__shared__` atau `__constant__`, maka variabel tersebut:
a. Berada di shared memory dan memiliki lifetime sama dengan lifetime thread
b. Berada di constant memory dan memiliki lifetime sama dengan lifetime block
c. Berada di global memory dan memiliki lifetime sama dengan lifetime block
d. Berada di global memory dan memiliki lifetime sama dengan lifetime aplikasi
e. Tidak ada jawaban benar

**✅ Jawaban:** **d**

**📝 Pembahasan:**
| Deklarasi | Memori | Lingkup | Lifetime |
|---|---|---|---|
| variabel lokal di kernel | register | thread | thread |
| `__device__ __shared__ int x;` | shared | block | block |
| `__device__ int x;` | **global** | grid (semua thread) | **aplikasi** |
| `__device__ __constant__ int x;` | constant | grid | aplikasi |

**💡 Konsep:** CUDA memory model (slide 06a).

---

### Soal 15 · Divergence tiling 104×104
**Soal:** Diketahui matriks berukuran 104×104, dibuat tiling berukuran 16×16 dalam satu thread block. Asumsi 1 warp berukuran 32 thread. Jika dilakukan operasi matriks dengan pendekatan tiling, tentu akan terjadi control divergence. Setelah dilakukan analisis, ternyata terjadi penurunan performansi sebesar (diharapkan tidak lebih dari):
a. 13,26%  b. 15,40%  c. 18,21%  d. 20,66%  e. Tidak ada jawaban yang benar

**✅ Jawaban:** **a. 13,26%**

**📝 Pembahasan:**
**Setup**
- Tile per dimensi: ⌈104/16⌉ = **7** (6 × 16 = 96; tile terakhir mencakup indeks 96–111, yang valid hanya 96–103 = **8 elemen**).
- Grid 7 × 7 = 49 blok; blok 16×16 = 256 thread = **8 warp**; satu warp = 2 baris × 16 kolom.
- Ada **7 fase**; di setiap fase, satu warp memuat 1 baris tile M dan 1 baris tile N → **2 operasi load** per warp per fase.
- Total warp-load = 49 × 8 × 7 × 2 = **5488**.

**Kapan load divergen?** Jika di dalam satu warp sebagian thread mengakses indeks valid dan sebagian tidak (kondisi batas `if (Row < W && kolom < W)` memecah warp).

1. **Load tile M** (`M[Row][ph·16 + tx]`): kolom hanya tidak penuh di **fase terakhir** (kolom 96–111, valid 8 dari 16) → setiap warp yang barisnya valid menjadi divergen.
   - 42 blok bukan baris terakhir × 8 warp = **336**
   - 7 blok baris terakhir: hanya 4 warp yang barisnya valid (96–103) → **28**
   - Subtotal **364**
2. **Load tile N** (`N[ph·16 + ty][Col]`): kolom tidak penuh hanya di **7 blok kolom terakhir** (Col 96–111, valid 8 dari 16).
   - Fase 1–6: 8 warp × 6 = 48; fase 7: baris N 96–111 hanya 96–103 valid → 4 warp yang barisnya valid tetap divergen karena kolomnya campuran → 4
   - 52 × 7 blok = **364**
   - (Di blok lain, fase 7: warp 0–3 baris valid semua & kolom valid semua; warp 4–7 baris tidak valid semua → tidak divergen.)
3. Total warp-load divergen = 364 + 364 = **728**.
```
Penurunan performa ≈ 728 / 5488 = 0,13265 ≈ 13,26%
```
(Jika penulisan P ikut dihitung: (728 + 52) / (5488 + 392) = 780/5880 = 13,265% → tetap 13,26%.)

**💡 Konsep:** control divergence pada tiled matrix multiplication (batas tile).

---

## UTS 2021/2022
*(Soal dari halaman scan Google Form. Pilihan yang tercentang di scan **bukan kunci**; jawaban yang berbeda dengan centang diberi catatan.)*

### Soal 1 · SMT
**Soal:** Hardware multithreading dapat meningkatkan kinerja parallel processing karena bisa membuat processor selalu busy. Pada kasus Simultaneous Multithreading (SMT), di antara pernyataan berikut, yang paling tepat adalah:
a. Setiap clock cycle terjadi context switching beberapa thread dari instruksi yang sama saja.
b. Setiap clock cycle terjadi context switching beberapa thread dari instruksi yang sama atau berbeda.
c. SMT tidak mendukung multiple issue instruksi per clock cycle.
d. SMT selalu meningkatkan kinerja meskipun diterapkan pada hardware yang tidak mendukung multithreading.
e. Hanya ada 2 pernyataan yang benar di antara 4 pernyataan di atas.

**✅ Jawaban:** **b**

**📝 Pembahasan:** SMT adalah variasi fine-grained multithreading yang **memungkinkan beberapa thread memakai unit-unit fungsional (multiple issue) pada siklus yang sama**; instruksi dari thread-thread itu bisa sejenis maupun berbeda.
- a terlalu sempit ("instruksi yang sama saja").
- c salah: SMT justru **memanfaatkan** multiple issue.
- d salah: SMT adalah fitur hardware; tanpa dukungan hardware tidak ada SMT, dan kinerjanya juga tidak selalu naik.
- e salah karena hanya b yang benar.

**💡 Konsep:** hardware multithreading (slide 02).

---

### Soal 2 · Bisection width fully connected
**Soal:** Bisection width sering digunakan untuk mengukur kualitas jaringan. Keterhubungan jaringan yang ideal adalah fully connected, di mana setiap node terhubung dengan node lain secara langsung. Jika jumlah node adalah 18 dan terhubung secara fully connected, maka nilai bisection width-nya adalah:
a. 9  b. 36  c. 81  d. 171  e. Tidak ada jawaban yang benar

**✅ Jawaban:** **c. 81**

**📝 Pembahasan:** bagi 18 node menjadi dua kelompok @ 9 node. Pada fully connected, **setiap** node di kiri punya link langsung ke **setiap** node di kanan → jumlah link yang melintasi pemisah = 9 × 9 = **81** = (p/2)².
- 9 = p/2 (rumus hypercube), 36 = salah hitung, **171** = jumlah **seluruh** link fully connected = 18·17/2 (bukan hanya yang melintasi pemisah).

**💡 Konsep:** bisection width (slide 02).

---

### Soal 3 · Snooping cache coherence
**Soal:** Masalah cache coherence dapat menghasilkan output yang salah. Ada dua pendekatan untuk menjamin cache coherence: snooping dan directory-based. Pada pendekatan snooping, pernyataan yang tepat adalah:
a. Nilai register dari setiap core dikopikan ke private cache core tersebut.
b. Nilai dari variabel yang di-share dibaca ulang dari memory.
c. Nilai cache yang diubah oleh core di-broadcast lewat bus.
d. Nilai cache yang diubah dituliskan pada suatu directory.
e. Tidak ada jawaban yang benar.

**✅ Jawaban:** **c**

**📝 Pembahasan:** pada snooping, core-core berbagi **bus**; ketika core 0 mengubah salinan x di cache-nya, ia **mem-broadcast** informasi itu di bus, dan core lain yang "menguping" menandai salinan x miliknya **invalid**. Opsi d adalah ciri **directory-based**; a dan b tidak menggambarkan mekanisme koherensi.

**💡 Konsep:** cache coherence (slide 02).

---

### Soal 4 · Strongly scalable
**Soal:** Scalability memungkinkan untuk mempertahankan nilai efisiensi. Scalability dibagi menjadi dua, strongly dan weakly scalable. Strong scalability bisa dicapai dengan cara:
a. Menambah jumlah processor sebanding dengan meningkatkan problem size.
b. Menambah jumlah processor diikuti dengan mengurangi problem size.
c. Mengurangi problem size dan mempertahankan jumlah processor tetap.
d. Menambah jumlah processor dan mempertahankan problem size.
e. Tidak ada jawaban benar.

**✅ Jawaban:** **d**

**📝 Pembahasan:** definisi slide: *strongly scalable* = efisiensi tetap ketika jumlah proses/thread dinaikkan **tanpa menaikkan problem size**. Opsi a adalah definisi **weakly scalable**.

**💡 Konsep:** scalability (slide 02).

---

### Soal 5 · Pemilihan library
**Soal:** Jika sumber daya komputasinya berupa PC dengan spesifikasi yang cukup dan beberapa workstation yang terhubung dengan jaringan LAN, library yang paling sesuai untuk implementasi komputasi kinerja tinggi adalah:
a. MPI  b. OpenMP atau PThread  c. OpenCL  d. GPU dengan CUDA  e. Tidak ada jawaban yang tepat

**✅ Jawaban:** **a. MPI**

**📝 Pembahasan:** beberapa komputer terpisah yang terhubung LAN = **distributed memory** (cluster); setiap mesin punya memori sendiri dan harus berkomunikasi lewat jaringan → **message passing (MPI)**. OpenMP/Pthreads hanya bekerja di dalam **satu** mesin shared memory; CUDA/OpenCL untuk akselerator GPU (tidak disebut ada).

**💡 Konsep:** shared vs distributed memory (slide 01–03).

---

### Soal 6 · Linked list (TIDAK bermasalah)
**Soal:** Diberikan linked list dengan akses paralel untuk IsMember, Insert, dan Delete. Manakah kasus yang bila dilakukan secara paralel **TIDAK** berpotensi mengakibatkan masalah terkait critical section?
- Dua pemanggilan IsMember
- Proses Insert dan Delete
- Proses Delete dan IsMember
- Proses IsMember dan Insert
- Dua proses Delete

**✅ Jawaban:** **Dua pemanggilan IsMember**

**📝 Pembahasan:** IsMember hanya **membaca**; banyak pembaca bersamaan tidak saling mengganggu. Semua pasangan lain melibatkan penulisan (Insert/Delete) → bisa merusak list (lihat UTS 2019/2020 no. 4).

**💡 Konsep:** readers–writers.

---

### Soal 7 · critical vs atomic
**Soal:** Periksalah kedua kode berikut dan pilih pernyataan yang benar:
```c
/* I */                          /* II */
int count = 0;                   int count = 0;
#pragma omp critical             #pragma omp atomic
count++;                         count++;
```
- Bila lingkungan implementasi dan cakupan program sama, kedua kode di atas memberikan keadaan hasil yang sama untuk variabel count.
- Program yang dibuat menggunakan kode I mengakibatkan eksekusi yang lebih cepat daripada kode II bila dijalankan pada lingkungan yang sama.
- Kode program II lebih baik bila digunakan untuk operasi yang lebih kompleks.
- Program dapat dioptimalkan dengan menggunakan kedua atribut pragma sekaligus.

**✅ Jawaban:** **Pernyataan pertama** (hasil count sama).

**📝 Pembahasan:** sama dengan **UTS 2020/2021 no. 8**: atomic lebih cepat (pernyataan 2 terbalik), critical untuk operasi kompleks (pernyataan 3 terbalik), dan keduanya tidak boleh dicampur (pernyataan 4 salah).

**💡 Konsep:** critical vs atomic.

---

### Soal 8 · Fungsi kode condition variable
**Soal:** Penggunaan potongan kode program berikut yang paling utama adalah untuk:
```c
pthread_mutex_lock(&mutex);
counter++;
if (counter == thread_count) {
    counter = 0;
    pthread_cond_broadcast(&cond_var);
} else {
    while (pthread_cond_wait(&cond_var, &mutex) != 0);
}
pthread_mutex_unlock(&mutex);
```
a. Menjumlahkan nilai variabel counter sampai batas tertentu
b. Mem-broadcast variabel cond_var ke semua thread
c. Melakukan busy waiting sampai batas waktu tertentu
d. Menunggu thread lain yang belum selesai memproses tasknya
e. Tidak ada jawaban benar

**✅ Jawaban:** **d**

**📝 Pembahasan:** ini **barrier dengan condition variable** (slide 04): setiap thread yang tiba menaikkan counter; jika belum semua tiba, ia **tidur** di `pthread_cond_wait` (melepas mutex); thread terakhir mereset counter dan **membangunkan semua** dengan `pthread_cond_broadcast`. Jadi tujuannya: menunggu sampai semua thread mencapai titik yang sama.
- a: counter hanya alat bantu menghitung kedatangan, bukan tujuan.
- b: broadcast adalah mekanisme membangunkan, bukan tujuan utama.
- c: bukan busy-wait — thread tidur, tidak memakai CPU.

**💡 Konsep:** barrier, condition variable (slide 04).

---

### Soal 9 · Syarat worksharing
**Soal:** Pada OpenMP terdapat istilah worksharing (pembagian kerja yang dialokasikan ke beberapa thread). Worksharing bisa dilakukan dengan menambahkan pragma PARALLEL FOR. Syarat agar dapat dilakukan worksharing adalah:
a. Ukuran loop harus kelipatan jumlah thread
b. Indeks loop harus mulai dengan satu
c. Data yang diproses harus berupa data integer
d. Data yang sedang diproses tidak bergantung pada data yang lain
e. Tidak ada jawaban benar

**✅ Jawaban:** **d**

**📝 Pembahasan:** OpenMP **tidak memeriksa** dependensi antar iterasi; loop yang hasil iterasinya bergantung pada iterasi lain (contoh Fibonacci di slide: hasil 1 1 2 3 5 8 0 0 0 0) tidak bisa diparalelkan dengan benar. Opsi lain bukan syarat: jumlah iterasi tidak harus kelipatan jumlah thread (sisa dibagi otomatis), indeks boleh mulai dari mana saja, dan data boleh bertipe apa pun (yang harus integer/pointer hanya **variabel indeks loop**).

**💡 Konsep:** parallel for, data dependency (slide 05).

---

### Soal 10 · `omp single`
**Soal:** Program OpenMP dijalankan dengan 4 thread. Jika di dalam blok `#pragma omp parallel` disisipkan `#pragma omp single`, maka output yang terjadi adalah (misal print "Hello"):
a. String "Hello" dicetak sebanyak 4 kali
b. String "Hello" akan dicetak oleh thread 0 saja
c. String "Hello" akan dicetak oleh thread yang paling akhir sebanyak 4 kali
d. String "Hello" akan dicetak sekali saja oleh salah satu thread
e. Tidak ada jawaban benar

**✅ Jawaban:** **d**

**📝 Pembahasan:** `single` → blok dijalankan **sekali** oleh **salah satu** thread (thread mana pun yang pertama tiba), thread lain menunggu di barrier implisit. Opsi b adalah perilaku `#pragma omp master` (selalu thread 0). Tanpa `single`, "Hello" tercetak 4 kali (opsi a).

**💡 Konsep:** OpenMP single vs master.

---

### Soal 11 · Optimasi konvolusi 2D
**Soal:** Diberikan kode kernel CUDA untuk konvolusi matriks berikut. Untuk membuat eksekusi program lebih efisien, Anda dapat melakukan:
```c
__global__ void convolution_2D_basic_kernel(unsigned char *in, unsigned char *mask,
                                            unsigned char *out, int maskwidth, int w, int h) {
    int Col = blockIdx.x * blockDim.x + threadIdx.x;
    int Row = blockIdx.y * blockDim.y + threadIdx.y;
    if (Col < w && Row < h) {
        int pixVal = 0;
        int N_start_col = Col - (maskwidth/2);
        int N_start_row = Row - (maskwidth/2);
        // Get the of the surrounding box
        for (int j = 0; j < maskwidth; ++j) {
            for (int k = 0; k < maskwidth; ++k) {
                int curRow = N_start_row + j;
                int curCol = N_start_col + k;
                // Verify we have a valid image pixel
                if (curRow > -1 && curRow < h && curCol > -1 && curCol < w) {
                    pixVal += in[curRow * w + curCol] * mask[j*maskwidth + k];
                }
            }
        }
        // Write our new pixel value out
        out[Row * w + Col] = (unsigned char)(pixVal);
    }
}
```
1. Membuat shared memory untuk menyimpan matriks input (in) yang digunakan dalam 1 blok thread
2. Membuat shared memory untuk menyimpan matriks output (out) yang dituliskan dalam 1 blok thread
3. Membuat shared memory untuk menyimpan matriks mask yang digunakan dalam 1 blok thread
4. Membuat shared memory untuk menyimpan matriks input (in) dan mask yang digunakan dalam 1 blok thread
5. Membuat shared memory untuk menyimpan matriks output (out) dan mask yang digunakan dalam 1 blok thread

**✅ Jawaban:** **4 (input dan mask)**

**📝 Pembahasan:** shared memory menguntungkan untuk data yang **dibaca berulang kali** oleh thread-thread dalam satu block:
- Setiap piksel **input** dibaca oleh hingga maskwidth² thread tetangga (misalnya 25 kali untuk mask 5×5) → reuse tinggi.
- **Mask** dibaca oleh **semua** thread di block (setiap thread membaca seluruh mask) → reuse sangat tinggi.
- **Output** setiap piksel hanya **ditulis sekali** → tidak ada gunanya disimpan di shared memory (opsi 2 dan 5 salah).
Jadi yang paling mengurangi akses global memory adalah memuat **tile input dan mask** ke shared memory.
*Catatan (tambahan):* dalam buku Kirk & Hwu, mask biasanya diletakkan di `__constant__` memory (di-cache dan di-broadcast ke semua thread), sedangkan input di-tiling ke shared memory. Karena opsi constant memory tidak tersedia, jawaban terbaik adalah 4.

**💡 Konsep:** shared memory, data reuse.

---

### Soal 12 · Karakteristik algoritma untuk memori lokal
**Soal:** Sebuah algoritma yang dapat dioptimalkan dengan memanfaatkan memory local (cache memory pada CPU dan shared memory pada GPU) adalah yang memiliki karakteristik:
- memiliki ukuran data besar
- memiliki data paralelism yang besar
- memerlukan bandwidth memory yang besar
- memiliki arithmetic density yang besar
- memiliki loop/iterasi yang besar

**✅ Jawaban:** **memiliki arithmetic density yang besar** (centang "data paralelism" di scan kurang tepat)

**📝 Pembahasan:** memori lokal (cache/shared memory) hanya menguntungkan jika data yang sudah dimuat **dipakai ulang** berkali-kali. Algoritma dengan **arithmetic density/intensity tinggi** (banyak operasi per byte data) seperti perkalian matriks (O(n³) operasi atas O(n²) data) dan konvolusi memiliki reuse tinggi → tiling ke memori lokal sangat efektif.
- Ukuran data besar, loop besar, atau data parallelism besar tidak menjamin ada reuse (contoh: penjumlahan vektor sangat data-parallel, tetapi setiap elemen hanya dipakai sekali → shared memory tidak membantu).
- Butuh bandwidth besar tanpa reuse = memory bound; memori lokal tidak mengurangi kebutuhan itu.

**💡 Konsep:** roofline/arithmetic intensity, tiling.

---

### Soal 13 · Konfigurasi kernel untuk iterasi 0–9
**Soal:** Program CUDA berikut ditujukan untuk mengeluarkan output "This is iteration number 0" hingga "This is iteration number 9". Kode yang menggantikan baris dalam komentar adalah:
```c
__global__ void loop() {
    printf("This is iteration number %d\n", threadIdx.x);
}
int main() {
    /* pemanggilan loop(), ganti bagian ini dengan kode yang sesuai */
    cudaDeviceSynchronize();
}
```
1. `for (int i = 0; i < 10; i++) { loop<<<1, 10>>>(); }`
2. `loop<<<1, 10>>>();`
3. `loop<<<10, 1>>>();`
4. `loop<<<5, 2>>>();`
5. `for (int i = 0; i < 10; i++) { loop<<<2, 5>>>(); }`

**✅ Jawaban:** **2. `loop<<<1, 10>>>();`**

**📝 Pembahasan:** output memakai **threadIdx.x**, jadi dibutuhkan threadIdx.x bernilai 0..9 → **1 blok berisi 10 thread**.
- Opsi 1: mencetak 0–9 sebanyak **10 kali** (10 peluncuran).
- Opsi 3: 10 blok × 1 thread → threadIdx.x selalu 0 → "0" sepuluh kali.
- Opsi 4: 5 blok × 2 thread → "0" dan "1" masing-masing lima kali.
- Opsi 5: 0–4 sebanyak 20 kali.

**💡 Konsep:** execution configuration, threadIdx vs blockIdx.

---

### Soal 14 · Divergensi reduksi yang diperbaiki
**Soal:** Jika ukuran blok adalah 256 thread, potongan kode operasi REDUKSI berikut akan mengalami divergensi pada langkah ke: (asumsi algoritma reduksi yang sudah diperbaiki)
```c
for (unsigned int stride = blockDim.x; stride > 0; stride /= 2) {
    __syncthreads();
    if (t < stride)
        partialSum[t] += partialSum[t + stride];
}
```
a. pertama  b. kedua  c. empat  d. delapan  e. Tidak ada jawaban benar

**✅ Jawaban:** Divergensi mulai saat **stride = 16**. 🟨 Itu iterasi ke-**5** jika dihitung dari 1 → **e**; jika iterasi dihitung mulai dari 0 → ke-**4** → **c**. (Centang di scan: c.)

**📝 Pembahasan:** 256 thread = 8 warp. Pada versi yang diperbaiki, thread aktif adalah t = 0 … stride−1 (berurutan dari 0):

| Iterasi (dari 1) | stride | Thread aktif | Warp divergen |
|---|---|---|---|
| 1 | 256 | 0–255 (8 warp penuh) | 0 |
| 2 | 128 | 0–127 (warp 0–3 penuh, 4–7 diam) | 0 |
| 3 | 64 | 0–63 (warp 0–1) | 0 |
| 4 | 32 | 0–31 (warp 0 penuh) | 0 |
| **5** | **16** | 0–15 (setengah warp 0) | **1** |
| 6–9 | 8, 4, 2, 1 | < 32 thread | 1 |

Selama stride ≥ 32, setiap warp penuh aktif atau penuh diam → tidak divergen. Divergensi baru muncul ketika jumlah thread aktif < ukuran warp. Di lembar jawaban, tulis eksplisit "mulai stride = 16 (iterasi ke-5 / langkah ke-4 jika dihitung dari 0), hanya 1 warp yang divergen".

**💡 Konsep:** control divergence pada reduksi.

---

### Soal 15 · Tujuan custom datatype MPI
**Soal:** MPI menyediakan fasilitas untuk membuat custom data types seperti `MPI_Type_create_struct` atau `MPI_Type_vector`. Manakah yang **bukan** tujuan penggunaan custom data types pada MPI?
- Membuat pengiriman data menjadi lebih efisien
- Membuat penyimpanan data menjadi lebih kecil
- Menghindari pengkopian data yang tidak perlu
- Membuat kode program menjadi lebih bersih dan rapi
- Menghindari pengiriman data kecil terpisah

**✅ Jawaban:** **Membuat penyimpanan data menjadi lebih kecil**

**📝 Pembahasan:** derived datatype hanya **mendeskripsikan** tipe dan posisi (displacement) item-item data di memori, sehingga MPI dapat mengumpulkan item yang tersebar dan mengirimnya dalam **satu pesan** (latency dibayar sekali), tanpa programmer harus menyalin manual ke buffer, dan kodenya lebih rapi. Ukuran data di memori **tidak berubah**.

**💡 Konsep:** MPI derived datatypes (slide 03).

---

### Soal 16 · Konfigurasi agar "Success!"
**Soal:** Agar program CUDA di bawah ini memberikan output "Success!", pasangan jumlah block dan thread yang harus mengganti bagian `/*ganti*/` adalah:
```c
__global__ void printSuccessForCorrectExecutionConfiguration() {
    if (threadIdx.x == 1023 && blockIdx.x == 255) {
        printf("Success!\n");
    }
}
int main() {
    printSuccessForCorrectExecutionConfiguration<<</*ganti*/>>>();
    cudaDeviceSynchronize();
}
```
1. 1023, 255  2. 1024, 256  3. 1, 1  4. 10, 10  5. 256, 1024

**✅ Jawaban:** **5. `<<<256, 1024>>>`**

**📝 Pembahasan:** argumen pertama = jumlah blok, kedua = thread per blok.
- `blockIdx.x == 255` butuh minimal **256 blok** (indeks 0–255).
- `threadIdx.x == 1023` butuh minimal **1024 thread per blok** (indeks 0–1023; 1024 = batas maksimum thread per blok).
- Opsi 1 (`1023, 255`): blockIdx maks 1022 oke, tetapi threadIdx maks 254 → tidak ada thread 1023.
- Opsi 2 (`1024, 256`): threadIdx maks 255 → gagal (urutan terbalik).

**💡 Konsep:** execution configuration.

---

### Soal 17 · Warp divergen pada perkalian matriks 100×100
**Soal:** Diberikan kode perkalian matriks CUDA berikut. Jika ukuran matriks M, N, dan P adalah 100×100 dengan ukuran thread block 16×16, ada berapa warp yang mengalami control divergence?
```c
__global__ void MatrixMulKernel(float* M, float* N, float* P, int Width) {
    // Calculate the row index of the P element and M
    int Row = blockIdx.y*blockDim.y + threadIdx.y;
    // Calculate the column index of P and N
    int Col = blockIdx.x*blockDim.x + threadIdx.x;
    if ((Row < Width) && (Col < Width)) {
        float Pvalue = 0;
        // each thread computes one element of the block sub-matrix
        for (int k = 0; k < Width; ++k)
            Pvalue += M[Row*Width+k] * N[k*Width+Col];
        P[Row*Width+Col] = Pvalue;
    }
}
```
1. 7  2. 13  3. 54  4. 56  5. 104

**✅ Jawaban:** Hitungan ketat = **50** (tidak ada di pilihan). 🟨 Pilih **56** sebagai opsi terdekat yang kemungkinan dimaksud penyusun soal, dan tuliskan analisisnya.

**📝 Pembahasan:**
- Grid: ⌈100/16⌉ = 7 → 7 × 7 = 49 blok × 8 warp = **392 warp**.
- Thread dilinearkan `threadIdx.y·16 + threadIdx.x` → satu warp = **2 baris × 16 kolom**.
- Blok terakhir di setiap dimensi mencakup indeks 96–111, yang valid hanya **96–99** (4 elemen).
- Warp divergen = warp yang **sebagian** thread-nya memenuhi `Row < 100 && Col < 100` dan sebagian tidak.

| Jenis blok | Jumlah blok | Analisis per warp | Warp divergen |
|---|---|---|---|
| Interior (bukan tepi) | 36 | Semua valid | 0 |
| Kolom terakhir, bukan sudut | 6 | Setiap baris: Col 96–99 valid, 100–111 tidak → semua 8 warp campuran | 6 × 8 = **48** |
| Baris terakhir, bukan sudut | 6 | Warp 0–1 (baris 96–99) semua valid; warp 2–7 (baris 100–111) semua tidak valid | 0 |
| Sudut kanan-bawah | 1 | Warp 0–1: baris valid, kolom campuran → divergen; warp 2–7: semua tidak valid | **2** |
| **Total** | | | **50** |

Penyusun soal kemungkinan menghitung **semua warp di kolom blok paling kanan**: 7 blok × 8 warp = **56** (menganggap juga 6 warp sudut yang seluruhnya di luar batas sebagai divergen). Karena 50 tidak tersedia, jawab **56** sambil menuliskan perhitungan di atas.

**💡 Konsep:** control divergence pada kondisi batas.

---

### Soal 18 · Solusi OpenMP paling efisien
**Soal:** Diberikan potongan kode berikut. Manakah solusi OpenMP yang paling efisien?
```c
int i, j, k;
float x[m][n][n];
float scale = 2.0;
...
/* (a) */
for (k = 1; k < n; k++)
/* (b) */
  for (j = 1; j < n; j++)
/* (c) */
    for (i = 0; i < m; i++)
      x[i][j][k] = x[i][j][k-1] + x[i][j-1][k] * scale;
```
1. Menambahkan pragma omp parallel for pada (a)
2. Menambahkan pragma omp parallel for pada (b)
3. Menambahkan pragma omp parallel for pada (c)
4. Menambahkan pragma omp parallel pada (a) dan omp for pada (c)
5. Menambahkan pragma omp parallel pada (a) dan omp for pada (b)

**✅ Jawaban:** **4. `omp parallel` pada (a) dan `omp for` pada (c)** (centang di scan pada opsi 5 **salah**)

**📝 Pembahasan:**
| Loop | Dependensi | Boleh di-worksharing? |
|---|---|---|
| k (a) | `x[i][j][k-1]` → membaca hasil iterasi k−1 | ❌ |
| j (b) | `x[i][j-1][k]` → membaca hasil iterasi j−1 | ❌ |
| i (c) | semua akses memakai i yang sama | ✅ |

- Opsi 1 & 2: membagi loop k atau j ke thread → melanggar dependensi → **hasil salah**.
- Opsi 5 (tercentang di scan): `omp for` di loop j → juga melanggar dependensi `j-1` → **salah**.
- Opsi 3: benar, tetapi tim thread dibuat dan dibubarkan **(n−1)² kali** (setiap pasangan k, j) → overhead besar.
- **Opsi 4:** tim thread dibuat **sekali** di (a); semua thread menjalankan loop k dan j bersama-sama, dan hanya loop i yang dibagi dengan `omp for`. Barrier implisit di akhir `omp for` memastikan semua i untuk (k, j) selesai sebelum melangkah ke (k, j) berikutnya → urutan dependensi terjaga → **benar dan paling efisien** di antara opsi.
```c
#pragma omp parallel private(j, k)      // j, k wajib private (dideklarasikan di luar → default shared)
for (k = 1; k < n; k++)
    for (j = 1; j < n; j++) {
        #pragma omp for
        for (i = 0; i < m; i++)
            x[i][j][k] = x[i][j][k-1] + x[i][j-1][k] * scale;
    }
```

**💡 Konsep:** data dependency, parallel vs for, fork-join overhead (slide 05).

---
## UTS 2024/2025
*(Ujian Tengah Semester IF3130 – 2025, 100 menit. Soal dari halaman scan; coretan tangan di scan bukan kunci.)*

### Q1 · Dasar Sistem Paralel – FLOP/s & bandwidth
**Soal:** Diberikan sebuah sistem komputer 4 core, hyperthreaded (2 thread/core) dengan kecepatan 3 GHz, kecepatan komputasi 1 FLOP/cycle, dan kecepatan akses memori/DRAM sebesar 1 word/cycle, dengan ukuran 1 word 8 byte. Komputer tersebut juga memiliki L3 cache dengan kecepatan/bandwidth akses 600 GB/s. Jawablah:
(a) Berapakah kecepatan/bandwidth akses DRAM pada sistem tersebut (dalam GB/s)? (hint: 1 Hz = 1 cycle/s, 1 GHz = 10⁹ Hz)
(b) Berapakah kecepatan komputasi maksimum sistem tersebut (dalam FLOP/s)?
(c) Sebuah program memiliki arithmetic intensity sebesar 1 FLOP/byte. Berapakah kecepatan komputasi program ini pada sistem komputer ini (dalam FLOP/s) jika semua akses data selalu berada pada DRAM?
(d) Berapakah kecepatan komputasi program pada poin (c) pada sistem ini (dalam FLOP/s) jika semua akses data selalu berada pada L3 cache?

**✅ Jawaban:** (a) **24 GB/s**; (b) **24 × 10⁹ FLOP/s** (model slide, hyperthread ikut dihitung); (c) **24 × 10⁹ FLOP/s**; (d) **24 × 10⁹ FLOP/s** (dibatasi kemampuan komputasi, bukan 600 × 10⁹).

**📝 Pembahasan:**
Rumus kunci (model *roofline* dari grafik "Performance" di slide 02):
```
Kinerja tercapai = min( kemampuan komputasi puncak , arithmetic intensity × bandwidth )
```
(a) Bandwidth DRAM:
```
1 word/cycle × 8 byte/word × 3·10⁹ cycle/s = 24·10⁹ byte/s = 24 GB/s
```
(Sama persis dengan label "DRAM: 1 word/cycle: 24.0 GB/s" di grafik slide.)

(b) Komputasi puncak. Model slide menghitung hyperthread sebagai pengali ("Multicore (4 core, 2 hyperthreads): 192 GFLOP/s" = 24 GFLOP/s vektor × 4 × 2):
```
4 core × 2 thread/core × 3·10⁹ cycle/s × 1 FLOP/cycle = 24·10⁹ FLOP/s = 24 GFLOP/s
```
*Catatan:* secara fisik, hyperthread berbagi unit fungsional yang sama sehingga ada yang menjawab 4 × 3·10⁹ × 1 = 12 GFLOP/s. Sebutkan asumsi yang kamu pakai; jawaban yang mengikuti slide adalah 24 GFLOP/s.

(c) Data di DRAM:
```
Batas memori   = 1 FLOP/byte × 24 GB/s = 24 GFLOP/s
Batas komputasi = 24 GFLOP/s
Kinerja = min(24, 24) = 24·10⁹ FLOP/s        (tepat di titik pertemuan memory-bound & compute-bound)
```
(Dengan asumsi 12 GFLOP/s di (b), hasilnya min(12, 24) = 12 GFLOP/s, compute bound.)

(d) Data di L3:
```
Batas memori   = 1 FLOP/byte × 600 GB/s = 600 GFLOP/s
Batas komputasi = 24 GFLOP/s
Kinerja = min(24, 600) = 24·10⁹ FLOP/s       → compute bound
```
Coretan "600·10⁹" di scan salah: memori yang lebih cepat tidak membuat prosesor melampaui kemampuan hitungnya. Bandwidth L3 hanya memindahkan bottleneck dari memori ke CPU.

> 🏭 **Analogi:** pabrik (CPU) hanya mampu mengolah 24 ton/jam. Truk DRAM mengantar 24 ton/jam → produksi 24. Truk L3 mengantar 600 ton/jam → produksi tetap 24, karena pabriknya yang membatasi.

**💡 Konsep:** performance limitation (speed vs feed), roofline (slide 02).

---

### Q2 · Pthread – menghitung thread
**Soal:**
(a) Anda ditugaskan membuat program paralel dengan Pthread. Jika setiap thread diizinkan mengeksekusi satu instruksi atau lebih, berapa thread yang digunakan untuk perhitungan berikut? Jelaskan.
```c
x++;
p = x + 5;
q = p + 3;
c++;
```
(b) Sebutkan jumlah thread dalam potongan program berikut. Jelaskan.
```c
int main(int argc, char **argv) {
    pthread_t thread[2];
    fork();
    pthread_create(&thread[0], NULL, run, (void *) &argv[0]);
    pthread_create(&thread[1], NULL, run, (void *) &argv[1]);
    pthread_join(thread[0], NULL);
    pthread_join(thread[1], NULL);
    return 0;
}
```

**✅ Jawaban:** (a) **2 thread**. (b) **6 thread** (2 proses × 3 thread).

**📝 Pembahasan:**
(a) Analisis dependensi:
```
x++;          ─┐  p membaca x yang baru (RAW)
p = x + 5;     ├─ q membaca p yang baru (RAW)       → rantai dependensi → harus berurutan → THREAD 1
q = p + 3;    ─┘
c++;          ── tidak membaca/menulis x, p, q      → independen → THREAD 2
```
Memecah rantai x → p → q ke thread berbeda tidak memberi keuntungan, karena setiap langkah harus menunggu langkah sebelumnya (dan butuh sinkronisasi tambahan). Hanya `c++` yang bisa berjalan bersamaan. Maksimum paralelisme yang berguna = **2 thread**.

(b) Hitung langkah demi langkah:
1. Awal: 1 proses dengan **1 main thread**.
2. `fork()` menduplikasi proses → **2 proses** (induk & anak); keduanya melanjutkan kode setelah `fork()`.
3. Di **setiap** proses, dua `pthread_create` membuat 2 thread baru → per proses: 1 main + 2 = **3 thread**.
4. Total = 2 × 3 = **6 thread**.
(Jika yang dihitung hanya thread yang dibuat `pthread_create`: 2 × 2 = 4 — sebutkan juga di jawaban.)

**💡 Konsep:** dependensi data (RAW), proses vs thread, `pthread_create` (slide 04 & 05b).

---

### Q3 · OpenMP – race condition
**Soal:** Pada potongan program berikut terdapat potensi masalah ketika dieksekusi. Identifikasi bagian program yang mempunyai potensi masalah dan lakukan perbaikan sehingga masalah bisa dihindari.
```c
#include <stdio.h>
#include <omp.h>
int main() {
    int sum = 0;
    #pragma omp parallel for
    for (int i = 0; i < 1000; i++) {
        sum += i;
    }
    printf("Sum: %d\n", sum);
    return 0;
}
```

**✅ Jawaban:** Masalah: **race condition** pada `sum += i;` karena `sum` adalah variabel **shared** yang diubah banyak thread tanpa perlindungan. Perbaikan: `#pragma omp parallel for reduction(+:sum)` → hasil pasti **499500**.

**📝 Pembahasan:**
- `sum` dideklarasikan sebelum region paralel → default **shared**.
- `sum += i` sebenarnya 3 langkah: *load* sum → tambah i → *store* sum. Dua thread bisa membaca nilai yang sama lalu saling menimpa (seperti contoh x += my_val di slide), sehingga sebagian penjumlahan hilang → hasil lebih kecil dari 499500 dan berbeda tiap eksekusi.
```c
#pragma omp parallel for reduction(+:sum)    // ✅ setiap thread punya salinan private sum (awal 0),
for (int i = 0; i < 1000; i++)               //    lalu di akhir semua salinan dijumlahkan ke sum
    sum += i;
```
Alternatif yang benar tetapi lebih lambat:
```c
#pragma omp parallel for
for (int i = 0; i < 1000; i++) {
    #pragma omp atomic          // atau #pragma omp critical (lebih lambat lagi)
    sum += i;
}
```
Hasil yang benar: 0 + 1 + … + 999 = 999 × 1000 / 2 = **499500**.

**💡 Konsep:** race condition, reduction clause (slide 05).

---

### Q4 · MPI – penjumlahan array 12 elemen
**Soal:** Diketahui program berbasis MPI di bawah ini yang ingin menghitung total penjumlahan seluruh elemen array berukuran 12 elemen:
```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    // MPI INIT
    // ..

    int data[12], local_data[3];
    int local_sum = 0, total_sum = 0;

    if (rank == 0) {
        for (int i = 0; i < 12; i++) {
            data[i] = i + 1;            // data = {1, 2, ..., 12}
        }
    }

    MPI_Scatter(data, 3, MPI_INT, local_data, 3, MPI_INT, 0, MPI_COMM_WORLD);

    for (int i = 0; i < 3; i++) {
        local_sum += local_data[i];
    }

    if (rank == 0) {
        total_sum = local_sum;
        for (int i = 1; i < size; i++) {
            int temp;
            MPI_Recv(&temp, 1, MPI_INT, i, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
            total_sum += temp;
        }
        printf("Total sum is %d\n", total_sum);
    } else {
        MPI_Send(&local_sum, 1, MPI_INT, 0, 0, MPI_COMM_WORLD);
    }

    MPI_Finalize();
    return 0;
}
```
(a) Berdasarkan program di atas, output apakah yang diharapkan keluar pada console/terminal?
(b) Apakah ada masalah pada program di atas? Jelaskan masalahnya jika ada dan saran perbaikannya.

**✅ Jawaban:**
(a) Dengan **4 proses**: **`Total sum is 78`**.
(b) Ada beberapa masalah: inisialisasi MPI (rank, size) tidak ada; program **hanya benar jika dijalankan tepat 4 proses**; pengumpulan hasil manual tidak efisien → gunakan `MPI_Reduce` (dan `MPI_Scatterv`/pembagian dinamis).

**📝 Pembahasan:**
(a) `MPI_Scatter` mengirim **3 elemen ke setiap proses** secara berurutan (block):
```
P0: {1, 2, 3}    → local_sum = 6
P1: {4, 5, 6}    → local_sum = 15
P2: {7, 8, 9}    → local_sum = 24
P3: {10, 11, 12} → local_sum = 33
Rank 0: 6 + 15 + 24 + 33 = 78   → "Total sum is 78"
```

(b) Masalah dan perbaikan:
1. **Inisialisasi MPI tidak ada / tidak lengkap.** `MPI_Init`, `MPI_Comm_rank`, dan `MPI_Comm_size` hanya berupa komentar, serta variabel `rank` dan `size` tidak dideklarasikan → program tidak bisa dikompilasi/dijalankan.
   ```c
   int rank, size;
   MPI_Init(&argc, &argv);
   MPI_Comm_rank(MPI_COMM_WORLD, &rank);
   MPI_Comm_size(MPI_COMM_WORLD, &size);
   ```
2. **Ukuran pembagian di-hardcode (3 elemen per proses).** Hanya benar jika `size × 3 = 12`, yaitu tepat 4 proses.
   - size = 2 → hanya 6 elemen pertama yang dijumlah → output 21 (salah).
   - size > 4 → Scatter membaca melewati batas `data[12]` → hasil acak/crash.
   Perbaikan: `local_n = 12 / size` (alokasi `local_data` dinamis) dan gunakan `MPI_Scatterv` untuk sisa pembagian, atau validasi `if (12 % size != 0) MPI_Abort(...)`.
3. **Pengumpulan hasil tidak efisien.** Rank 0 menerima p−1 pesan satu per satu (O(p), rank 0 jadi bottleneck). Gunakan fungsi kolektif yang memakai pola pohon (O(log p)) dan kodenya lebih ringkas:
   ```c
   MPI_Reduce(&local_sum, &total_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);
   if (rank == 0) printf("Total sum is %d\n", total_sum);
   ```

**💡 Konsep:** Scatter, Reduce, inisialisasi MPI, pembagian data (slide 03).

---

### Q5 · GPU – citra medis
**Soal:** Bayangkan Anda diminta mengoptimalkan sebuah program komputasi ilmiah yang melakukan pemrosesan terhadap citra medis berukuran besar (misalnya 8000 × 8000 piksel) dengan melakukan operasi yang sama pada setiap piksel, seperti filter sharpening, peningkatan kontras, dan thresholding.
(a) Jelaskan 2 alasan mengapa kasus ini cocok untuk diimplementasikan menggunakan GPU.
(b) Berikan 1 contoh kasus yang tidak cocok untuk diimplementasikan pada GPU.

**✅ Jawaban:**
(a) ① **Paralelisme data masif**: 64 juta piksel dapat dipetakan **1 piksel = 1 thread**, dengan **operasi yang sama** untuk semua piksel (cocok dengan model SIMT/warp). ② Setiap piksel diproses **independen** (tidak ada dependensi antar piksel, percabangan minim → hampir tanpa divergence) dan aksesnya teratur (coalesced), sehingga memanfaatkan **throughput** dan bandwidth memori GPU yang besar.
(b) Contoh: algoritma **sekuensial/rekursif dengan dependensi antar langkah** (mis. menghitung deret Fibonacci, menelusuri linked list, atau parsing teks), atau data kecil yang waktu transfer CPU↔GPU-nya lebih besar dari waktu komputasinya.

**📝 Pembahasan:**
Hubungkan dengan slide 06a:
- GPU cocok untuk **data-parallel algorithms**: large data arrays, streaming throughput, fine-grain SIMD parallelism, floating-point computation. Gambar 8000 × 8000 dengan blok 16×16 → 500 × 500 = 250.000 blok → GPU terpakai penuh (butuh ribuan thread).
- Filter sharpening (konvolusi) juga punya **data reuse** antar piksel tetangga → bisa dioptimasi dengan shared memory (tiling).

Kasus yang **tidak** cocok (slide: "Kapan GPU tidak memiliki kinerja bagus"):
| Alasan | Contoh |
|---|---|
| Kurang paralelisme | Fibonacci iteratif, simulasi langkah demi langkah yang setiap langkahnya bergantung pada sebelumnya |
| Thread divergence | Program dengan banyak `if-else` berbeda per data (mis. interpreter, logika bisnis) |
| Kebutuhan memori dinamis | Struktur data yang tumbuh dinamis (alokasi dilakukan di CPU) |
| Rekursi | Quicksort rekursif dalam, traversal pohon rekursif (stack GPU terbatas) |
| Data kecil | Menjumlahkan 100 angka: overhead transfer PCIe > waktu hitung |

**💡 Konsep:** GPGPU, kapan GPU bagus/tidak (slide 06a).

---

## UTS 2025/2026

### Soal 1 · Benar atau salah
**Soal:** Tentukan apakah pernyataan berikut benar atau salah:
a. Pada shared memory, cache coherence menyebabkan overhead komunikasi.
b. Model SPMD tidak cocok untuk sistem shared memory karena tidak memungkinkan komunikasi antar proses.
c. Amdahl's law menunjukkan bahwa peningkatan prosesor secara eksponensial dapat meningkatkan speedup hampir serial.
d. Pipelining pada ILP mempercepat eksekusi instruksi dengan melakukan beberapa tahap instruksi yang berbeda secara paralel.
e. Speculative execution pada CPU tidak relevan pada GPU karena fokus yang berbeda pada latency dan throughput.
f. Pada sistem shared address space, akses memori dapat dilakukan tanpa mekanisme sinkronisasi.

**✅ Jawaban:** a **Benar**, b **Salah**, c **Salah**, d **Benar**, e **Benar**, f **Salah**.

**📝 Pembahasan:**
- **a. Benar.** Menjaga koherensi butuh pesan invalidate/broadcast (snooping) atau pesan ke directory, serta pengambilan ulang cache line → lalu lintas tambahan di bus/interconnect. Overhead ini makin parah pada **false sharing**.
- **b. Salah.** SPMD (satu program, perilaku dibedakan oleh rank/id) dipakai juga di shared memory: thread-thread Pthreads/OpenMP menjalankan fungsi yang sama dan bercabang berdasarkan `my_rank`. Komunikasi di shared memory dilakukan **implisit lewat variabel bersama**, jadi tetap mungkin.
- **c. Salah.** (Kata "hampir serial" kemungkinan salah ketik dari "hampir linear".) Amdahl justru menunjukkan speedup **dibatasi 1/s** berapa pun prosesornya; menambah prosesor secara eksponensial memberi tambahan speedup yang makin kecil (*diminishing returns*).
- **d. Benar.** Pipelining menumpangtindihkan tahap-tahap (fetch, decode, execute, …) dari instruksi yang **berbeda** sehingga throughput naik (latensi satu instruksi tetap).
- **e. Benar.** Spekulasi/branch prediction adalah teknik CPU yang **latency-oriented** untuk mempercepat satu thread. GPU yang **throughput-oriented** memakai core sederhana tanpa spekulasi dan menyembunyikan latency dengan berganti ke warp lain (hardware multithreading).
- **f. Salah.** Semua thread memang bisa **mengakses** semua memori, tetapi akses bersamaan dengan minimal satu penulisan menimbulkan **race condition**; perlu sinkronisasi (mutex, atomic, barrier, semaphore, dsb.) agar hasilnya benar.

**💡 Konsep:** cache coherence, SPMD, Amdahl, ILP, CPU vs GPU, race condition.

---

### Soal 2 · OpenMP schedule static vs dynamic
**Soal:** Perhatikan cuplikan kode berikut:
```c
#pragma omp for schedule(static)
for (int i = 0; i < row; i++) {
    for (int j = 0; j < col; j++) {
        result[i][j] = A[i][j] + B[i][j];
    }
}
```
a. Ukuran matriks adalah 100×100 dan waktu eksekusi program adalah 5 detik. Saat program dijalankan dengan schedule dynamic, waktu eksekusi bertambah menjadi 8 detik. Berikan alasan mengapa hal ini mungkin terjadi.
b. Penggunaan penjadwalan dynamic memperlambat eksekusi program. Berikan saran yang dapat diaplikasikan untuk mempercepat waktu eksekusi program tersebut.
c. Saat ukuran matriks bertambah menjadi 10000×10000, waktu eksekusi dengan penjadwalan statik adalah 50 detik, sementara dengan penjadwalan dinamik 35 detik. Jelaskan mengapa hal ini dapat terjadi. Apakah ada hubungan antara ukuran matriks dan penjadwalan tugas?

**✅ Jawaban:**
a. Beban setiap baris **seragam dan kecil**, sehingga dynamic tidak memberi manfaat load balancing tetapi menambah **overhead penjadwalan** (chunk default = 1 → 100 kali meminta pekerjaan ke runtime, dengan sinkronisasi) dan memperburuk lokalitas cache.
b. Gunakan **static** (beban seragam), atau dynamic/guided dengan **chunk size besar**; untuk matriks kecil kurangi jumlah thread atau jalankan serial.
c. Untuk matriks besar, overhead dynamic **terbayar oleh besarnya kerja per chunk**, sedangkan kecepatan thread di dunia nyata **tidak sama** (NUMA, hyperthread berbagi core, rebutan bandwidth memori, gangguan OS). Static menunggu thread paling lambat; dynamic membiarkan thread cepat mengambil lebih banyak baris → lebih seimbang. **Ya**, ada hubungannya: rasio overhead/kerja dan dampak ketidakseimbangan bergantung pada ukuran masalah.

**📝 Pembahasan:**
(a) Mekanisme masing-masing jadwal:
- **static:** sebelum loop, 100 baris langsung dibagi menjadi blok berurutan (mis. 4 thread × 25 baris). Tidak ada komunikasi dengan runtime saat loop berjalan; setiap thread membaca baris berurutan → prefetch/cache efektif.
- **dynamic (chunk default 1):** setiap thread mengambil **1 baris**, mengerjakannya (hanya 100 penjumlahan — sangat cepat), lalu kembali **meminta baris berikutnya** ke antrean bersama milik runtime (perlu operasi atomik/kunci) → 100 kali overhead untuk pekerjaan yang sangat kecil. Baris yang dikerjakan satu thread juga **tidak berurutan** → lokalitas cache berkurang.
- Karena beban sudah seimbang, dynamic hanya menambah biaya → 5 s menjadi 8 s.

(b) Saran:
1. Pakai **`schedule(static)`** (default) untuk beban seragam.
2. Jika tetap dynamic, **perbesar chunk**: `schedule(dynamic, 25)` atau `schedule(guided)` → jumlah permintaan ke runtime jauh berkurang.
3. Untuk matriks kecil, overhead fork/join pun signifikan → kurangi jumlah thread, gunakan clause `if(row*col > THRESHOLD)`, atau jalankan serial.
4. Pastikan pragma berada di dalam region paralel (`#pragma omp parallel for ...`); `#pragma omp for` saja di luar region `parallel` hanya dijalankan 1 thread.

(c) Pada 10000 × 10000 (10⁸ elemen, ratusan MB per matriks):
- Setiap baris berisi 10.000 operasi → biaya meminta chunk menjadi **sangat kecil** dibanding kerjanya.
- Program menjadi **memory bound**; kecepatan efektif thread bisa berbeda (sebagian data ada di node NUMA yang lebih jauh, dua hyperthread berbagi satu core, rebutan bandwidth, interupsi OS). Dengan **static**, setiap thread mendapat jatah tetap sehingga waktu total = thread **paling lambat**. Dengan **dynamic**, thread yang lebih cepat otomatis mengambil lebih banyak baris → beban seimbang → 35 s < 50 s.
- **Hubungan ukuran & penjadwalan:** ada. Masalah kecil dan seragam → static (overhead nol). Masalah besar, atau beban/kecepatan iterasi tidak seragam → dynamic/guided dengan chunk yang wajar, karena overhead per chunk menjadi tidak signifikan dibanding manfaat load balancing.

**💡 Konsep:** scheduling loops, load balancing, overhead (slide 05).

---

### Soal 3 · MPI – synchronous atau asynchronous?
**Soal:** Perhatikan cuplikan kode berikut.
```c
x = f1(y);
MPI_XXX_Send(/* kirim x ke proses lain */);
x = f2(z);
```
Apakah implementasi `MPI_XXX_Send` seharusnya bersifat synchronous atau asynchronous? Jelaskan.

**✅ Jawaban:** **Synchronous/blocking.** Variabel `x` langsung ditimpa oleh `f2(z)` setelah perintah kirim, sehingga pengiriman harus memastikan data `x` (= f1(y)) **sudah aman terkirim/tersalin** sebelum fungsi kembali.

**📝 Pembahasan:**
- Pada pengiriman **asynchronous/non-blocking** (mis. `MPI_Isend`), fungsi kembali **segera** tanpa menjamin data sudah disalin atau dikirim. Jika `x = f2(z)` dieksekusi sebelum transfer selesai, yang terkirim bisa nilai **f2(z)** atau data campuran → **salah**.
- Pada pengiriman **blocking** (`MPI_Send`), fungsi baru kembali setelah buffer `x` **aman dipakai ulang** (sudah disalin ke buffer internal atau sudah dikirim). `MPI_Ssend` (synchronous) bahkan menunggu sampai penerima mulai menerima. Keduanya menjamin nilai f1(y) yang terkirim.
- Jika ingin tetap asynchronous (agar komputasi dan komunikasi tumpang tindih), harus ada `MPI_Wait(&request, ...)` sebelum `x = f2(z)`, atau kirim **salinan** x (variabel lain).
```c
/* Benar – blocking */
x = f1(y);
MPI_Send(&x, 1, MPI_DOUBLE, dest, tag, comm);   /* atau MPI_Ssend */
x = f2(z);

/* Benar – non-blocking + wait */
x = f1(y);
MPI_Isend(&x, 1, MPI_DOUBLE, dest, tag, comm, &req);
/* ... kerja lain yang tidak menyentuh x ... */
MPI_Wait(&req, MPI_STATUS_IGNORE);
x = f2(z);
```

**💡 Konsep:** semantik `MPI_Send`, `MPI_Ssend`, keamanan buffer (slide 03).

---

### Soal 4 · Jumlah Send/Recv minimum untuk Allreduce
**Soal:** Berapa banyak `MPI_Send` dan `MPI_Recv` minimum untuk mengimplementasikan prosedur `MPI_Allreduce`? Jelaskan.

**✅ Jawaban:** Dengan p proses: minimal **2(p − 1) `MPI_Send` dan 2(p − 1) `MPI_Recv`** (reduce ke satu proses p − 1 pesan, lalu broadcast p − 1 pesan). Contoh p = 8 → 14 Send dan 14 Recv.

**📝 Pembahasan:**
Konstruksi yang mencapai 2(p − 1):
```
Fase 1 – Reduce (pohon atau linear) : p − 1 pesan → satu proses (mis. rank 0) memegang total
Fase 2 – Broadcast hasil            : p − 1 pesan → semua proses memegang total
Total = 2(p − 1) Send + 2(p − 1) Recv
```
Contoh pohon p = 8: reduce 1→0, 3→2, 5→4, 7→6; 2→0, 6→4; 4→0 (7 pesan, 3 langkah); broadcast 0→4; 0→2, 4→6; 0→1, 2→3, 4→5, 6→7 (7 pesan, 3 langkah). Total 14 pesan, 6 langkah.

Mengapa tidak bisa kurang?
1. Agar **ada satu** proses yang mengetahui gabungan semua nilai, informasi dari p proses harus disatukan. Satu pesan paling banyak menyatukan dua "kelompok informasi" → minimal **p − 1 pesan**.
2. Pada saat itu, p − 1 proses lain belum tahu hasil lengkap → masing-masing harus menerima minimal **1 pesan lagi** → **p − 1 pesan**.
3. Total ≥ 2(p − 1). *(Dikenal sebagai hasil gossip problem untuk komunikasi satu arah.)*

Pembanding: pola **butterfly** hanya butuh **log₂p langkah** (lebih cepat dari segi waktu), tetapi setiap proses mengirim & menerima di setiap langkah → total **p·log₂p** pesan (p = 8: 24 pesan, 3 langkah). Jadi minimum jumlah pesan = 2(p − 1); minimum jumlah langkah = log₂p.

**💡 Konsep:** collective communication, tree-structured & butterfly allreduce (slide 03).

---

### Soal 5 · CUDA local maximum filter
**Soal:** Perhatikan cuplikan kode berikut.
```c
#include <stdio.h>
#include <cuda_runtime.h>

___[A]___ float localMaximum(float left, float center, float right) {
    return fmaxf(fmaxf(left, center), right);
}

___[B]___ void localMaximumFilter(const float *input, float *output, int N) {
    int i = _____[C]_____;
    if (i >= 1 && i < N-1) {
        float left   = input[i-1];
        float center = input[i];
        float right  = input[i+1];
        output[i] = localMaximum(left, center, right);
    }
}

int main() {
    const int N = 1 << 20;
    float *input, *d_input;
    float *output, *d_output;
    // Inisialisasi array input diabaikan
    cudaMalloc(&d_input,  N * sizeof(float));
    cudaMalloc(&d_output, N * sizeof(float));
    cudaMemcpy(d_input, input, N * sizeof(float), ___[D]___);

    dim3 blockSize = dim3(256);
    dim3 gridSize  = dim3(___[E]___);

    localMaximumFilter<<<gridSize, blockSize>>>(___[F]___);

    cudaMemcpy(output, d_output, N * sizeof(float), ___[G]___);

    ___[H]___;
    ___[I]___;
    return 0;
}
```
a. Lengkapi bagian yang kosong pada kode di atas.
b. Apakah program tersebut berpotensi memiliki race condition? Jelaskan.
c. Di manakah variabel `left`, `right`, `center`, `input`, dan `output` disimpan dalam memori? Jelaskan.

**✅ Jawaban:**
a.
```c
[A] __device__
[B] __global__
[C] blockIdx.x * blockDim.x + threadIdx.x
[D] cudaMemcpyHostToDevice
[E] (N + blockSize.x - 1) / blockSize.x        // = (2^20 + 255)/256 = 4096
[F] d_input, d_output, N
[G] cudaMemcpyDeviceToHost
[H] cudaFree(d_input)
[I] cudaFree(d_output)
```
b. **Tidak ada race condition.**
c. `left`, `center`, `right` → **register** (per thread). `input`, `output` → **global memory** GPU.

**📝 Pembahasan:**
(a)
- **[A] `__device__`**: `localMaximum` dipanggil **dari kernel** (berjalan di GPU, dipanggil oleh kode GPU).
- **[B] `__global__`**: `localMaximumFilter` adalah **kernel** yang diluncurkan dari host dengan `<<<...>>>` dan return `void`.
- **[C]** indeks global 1D: `blockIdx.x * blockDim.x + threadIdx.x`.
- **[D]** `cudaMemcpy(tujuan = d_input, sumber = input, …)` → host ke device.
- **[E]** jumlah blok agar 2²⁰ elemen tercakup dengan 256 thread/blok: pembulatan ke atas (N + 255)/256 = **4096** blok. (Karena N habis dibagi 256, `N/256` juga benar, tetapi rumus pembulatan ke atas lebih aman.)
- **[F]** argumen kernel harus pointer **device**: `d_input, d_output, N`.
- **[G]** hasil dari device ke host: `cudaMemcpyDeviceToHost`.
- **[H], [I]** bebaskan memori device: `cudaFree(d_input)`, `cudaFree(d_output)`. (Opsional: `cudaDeviceSynchronize()` setelah kernel; `free(input)`/`free(output)` jika dialokasikan dengan malloc. Kode soal juga punya salah ketik: baris `float *input, *d_input,` seharusnya diakhiri `;`.)

(b) Race condition terjadi jika beberapa thread mengakses lokasi yang sama dan minimal satu **menulis**. Di sini:
- Setiap thread hanya **menulis** `output[i]` dengan **i unik** miliknya → tidak ada dua thread menulis lokasi yang sama.
- `input` hanya **dibaca** (bahkan `const`); thread i, i−1, dan i+1 sama-sama membaca `input[i]`, tetapi pembacaan bersamaan aman.
→ **tidak ada race condition**. (Catatan: `output[0]` dan `output[N-1]` tidak pernah ditulis karena kondisi `i >= 1 && i < N-1`; nilainya tidak terdefinisi, tetapi itu bukan race condition.)

(c)
| Variabel | Lokasi | Alasan |
|---|---|---|
| `left`, `center`, `right` (juga `i`) | **Register** | Variabel lokal skalar di dalam kernel → privat per thread, lifetime = thread (bisa *spill* ke local memory jika register kurang) |
| `input`, `output` (isi array) | **Global memory** (DRAM GPU) | Dialokasikan dengan `cudaMalloc` → dapat diakses semua thread & host via `cudaMemcpy`, lifetime sampai `cudaFree` |
| pointer `input`/`output` (parameter kernel) | Parameter kernel (constant/parameter space), lalu disalin ke register | Nilai pointer diteruskan saat kernel diluncurkan |

**💡 Konsep:** function qualifiers, indeks thread, cudaMalloc/cudaMemcpy/cudaFree, race condition, CUDA memory model (slide 06a).

---

## Rekap Kunci Pilihan Ganda

| Ujian | Soal → Jawaban |
|---|---|
| **UTS 2018/2019** | 7 → **a (0)** · 8 → **d (32)** · 9 → **a** 🟨 (b jika "adjacent") · 10 → **d** 🟨 (b jika no. 9 = b) |
| **UTS 2019/2020** | 6 → **c** · 7 → **c (38·8·50)** · 8 → **b (1000)** · 9 → **c (1/32)** |
| **UTS 2020/2021** | 1 → **a** · 2 → **b** · 3 → **d** · 4 → **d** · 5 → **a** · 6 → **c** · 7 → **semua kecuali "dua isMember"** · 8 → **pernyataan 1** · 9 → **d** · 10 → **c** · 11 → **c** · 12 → **d** · 13 → **a, c, d mungkin (pilih d)** · 14 → **d** · 15 → **a (13,26%)** |
| **UTS 2021/2022** | 1 → **b** · 2 → **c (81)** · 3 → **c** · 4 → **d** · 5 → **a (MPI)** · 6 → **dua IsMember** · 7 → **pernyataan 1** · 8 → **d** · 9 → **d** · 10 → **d** · 11 → **input & mask** · 12 → **arithmetic density** · 13 → **`loop<<<1,10>>>`** · 14 → **stride 16 (ke-5; c jika dihitung dari 0)** 🟨 · 15 → **penyimpanan lebih kecil** · 16 → **256, 1024** · 17 → **50 ketat; pilih 56** 🟨 · 18 → **parallel (a) + for (c)** |
| **UTS 2025/2026 no. 1** | a **B** · b **S** · c **S** · d **B** · e **B** · f **S** |

---

*Semoga sukses UTS-nya! 🍀 Kalau ada soal yang ingin dibahas lebih dalam atau dibuatkan variasi soalnya, tinggal tanyakan.*
