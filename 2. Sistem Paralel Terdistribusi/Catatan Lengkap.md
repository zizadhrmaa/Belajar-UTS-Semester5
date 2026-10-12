# 📘 Catatan Mudah IF3230 – Sistem Paralel dan Terdistribusi

> Catatan ini menjelaskan ulang **seluruh** materi kuliah (slide 00 s.d. 06a) dengan bahasa sederhana, analogi sehari-hari, contoh angka, dan potongan kode yang diberi komentar.
> Setiap bab ditutup dengan **"Inti yang harus diingat"** dan **"Uji diri"** (jawaban bisa dibuka).
> Catatan bertanda *(tambahan)* adalah penjelasan pelengkap yang tidak tertulis di slide, tetapi membantu pemahaman.

---

## 🗺️ Daftar Isi

0. [Gambaran Besar: Kenapa Kuliah Ini Ada?](#bab-0--gambaran-besar-kenapa-kuliah-ini-ada)
1. [Konsep Dasar Paralel](#bab-1--konsep-dasar-paralel-slide-01)
2. [Hardware Paralel](#bab-2--hardware-paralel-slide-02-bagian-1)
3. [Software Paralel & Mengukur Kinerja](#bab-3--software-paralel--mengukur-kinerja-slide-02-bagian-2)
4. [MPI – Pemrograman Distributed Memory](#bab-4--mpi-pemrograman-distributed-memory-slide-03)
5. [Pthreads – Pemrograman Shared Memory](#bab-5--pthreads-pemrograman-shared-memory-slide-04)
6. [OpenMP – Shared Memory yang Lebih Praktis](#bab-6--openmp-shared-memory-yang-lebih-praktis-slide-05)
7. [Vectorization (SIMD di dalam CPU)](#bab-7--vectorization-simd-di-dalam-cpu-slide-05b)
8. [Hybrid MPI + OpenMP](#bab-8--hybrid-mpi--openmp-slide-05c)
9. [GPU & CUDA](#bab-9--gpu--cuda-slide-06a)
10. [Rangkuman Akhir: Perbandingan, Rumus, dan Jebakan](#bab-10--rangkuman-akhir)
11. [Glosarium](#bab-11--glosarium)

---

## Bab 0 – Gambaran Besar: Kenapa Kuliah Ini Ada?

### Analogi utama yang dipakai di seluruh catatan: 🍳 Dapur Restoran

Bayangkan sebuah restoran:

| Dunia komputer | Dunia dapur |
|---|---|
| Program | Daftar pesanan yang harus dimasak |
| Core / prosesor | Koki |
| Thread | Koki yang bekerja di **dapur yang sama** |
| Proses (MPI) | Koki di **cabang restoran yang berbeda** |
| Shared memory | Satu meja bahan bersama yang bisa dijangkau semua koki |
| Distributed memory | Tiap cabang punya kulkas sendiri; kalau butuh bahan cabang lain harus **dikirim kurir** |
| Komunikasi | Kurir / teriak "oper bawangnya!" |
| Sinkronisasi | "Tunggu semua lauk siap, baru disajikan bersamaan" |
| Load balancing | Membagi pesanan agar tidak ada koki yang kewalahan sementara yang lain menganggur |

Dulu restoran hanya punya **satu koki super** yang setiap tahun makin cepat. Sekarang koki itu sudah mentok (tidak bisa lebih cepat lagi tanpa "terbakar"), jadi restoran **menambah jumlah koki**. Masalahnya: menambah koki tidak otomatis membuat masakan lebih cepat — **harus ada yang mengatur pembagian kerjanya**. Itulah isi kuliah ini.

### Isi kuliah (dari slide 00)
- **Tujuan:** memahami konsep paralel & terdistribusi, memahami model pemrograman paralel (**Pthreads, MPI, MapReduce, OpenMP, CUDA/OpenCL**), mampu membuat aplikasi paralel & terdistribusi yang cukup besar, memahami arsitektur paralel, sinkronisasi, konsistensi & koherensi, model kegagalan, dan fault tolerance.
- **Silabus:** konsep dasar → model shared memory → OpenMP → message passing (MPI) → CUDA → arsitektur mesin paralel → sinkronisasi → transaksi terdistribusi → model konsistensi → model kegagalan → fault tolerance → peer-to-peer → studi kasus Google & Facebook.
- **Buku acuan:** Pacheco (*An Introduction to Parallel Programming*), Robey & Zamora (*Parallel and High Performance Computing*), Hennessy & Patterson (*Computer Architecture*), Trobec dkk., van Steen (*Distributed Systems*), Coulouris dkk.

> Materi yang dibahas di catatan ini: konsep dasar, hardware & software paralel, MPI, Pthreads, OpenMP, vektorisasi, hybrid MPI+OpenMP, dan GPU/CUDA.

---

## Bab 1 – Konsep Dasar Paralel (slide 01)

### 1.1 Kenapa harus paralel?

**Sejarah singkat kecepatan prosesor:**
- 1986–2002: kinerja prosesor naik ±**50% per tahun** (zaman keemasan).
- Setelah 2002: turun jadi ±**20% per tahun**, lalu makin melambat (grafik Hennessy-Patterson: 23%/th → 12%/th → 3.5%/th).

**Kenapa melambat?**
1. **Clock speed jenuh** di sekitar 3 GHz.
2. Trik mempercepat eksekusi instruksi (ILP) sudah mentok → disebut **ILP wall**.
3. **"Pelajaran fisika kecil"**:
   - Transistor lebih kecil → prosesor lebih cepat
   - Prosesor lebih cepat → konsumsi daya naik
   - Daya naik → panas naik
   - Panas naik → prosesor tidak andal (bisa rusak/error)

**Solusi:** berhenti membuat satu core super cepat, ganti dengan **banyak core dalam satu chip** (multicore, Intel mulai 2005).

> 🔥 **Analogi:** Seorang pelari tidak bisa terus dipaksa lari lebih cepat — ia akan kepanasan dan pingsan. Lebih masuk akal membuat **tim estafet** atau membagi pekerjaan ke banyak orang.

**Timeline komputasi berkinerja tinggi (HPC):**
Mainframe (1965–77) → Vector computer (1977–90) → Massively parallel / shared memory multiprocessor (1990–2000) → Berbasis mikroprosesor (2000–05) → Mikroprosesor + akselerator/GPU (2005–kini) & Cloud (2005–kini).

Fakta menarik dari slide: HP seharga 8 juta saat ini setara kinerja komputer tercepat yang dibeli tahun 1993 seharga 80 miliar. Kinerja naik ±50.000× (1978–2018).

### 1.2 Jenis-jenis paralelisme

| Jenis | Artinya | Analogi |
|---|---|---|
| **Data Level Parallelism (DLP)** | Banyak *data* diproses bersamaan dengan operasi yang sama | 10 orang mengupas 100 kentang, masing-masing 10 kentang |
| **Task Level Parallelism (TLP)** | Banyak *tugas berbeda* dikerjakan bersamaan dan independen | Satu orang mengupas, satu menggoreng, satu membuat saus |

Bentuk-bentuknya di arsitektur komputer:
- **Instruction Level Parallelism (ILP):** compiler/prosesor memanfaatkan pipelining & speculative execution. (Otomatis, programmer tidak perlu apa-apa.)
- **Vector / GPU / instruksi multimedia:** 1 instruksi bekerja pada banyak data (DLP).
- **Thread Level Parallelism:** banyak thread pada hardware yang terhubung erat (multicore).
- **Request Level Parallelism:** banyak permintaan independen (mis. ribuan user mengakses web server).

### 1.3 Komputer di sekitar kita sudah paralel

Contoh dari slide: CPU 16 core + Hyperthreading (2 thread/core) + unit vektor AVX2 256-bit.
Satu angka `double` = 64 bit → satu instruksi vektor memuat 256/64 = 4 angka.

```
Paralelisme maksimum = 16 core × 2 HT × 4 angka = 128 jalur paralel
Program serial hanya memakai 1/128 ≈ 0,8% kapasitas!
```

> 🏭 **Analogi:** Kamu punya pabrik dengan 128 mesin, tapi hanya menyalakan 1. Sisanya menganggur.

**Peran programmer:** multicore **tidak** otomatis mempercepat program. Program serial hanya berjalan di satu core. Programmer harus **mengidentifikasi** paralelisme dalam algoritma, **menuliskannya** dalam kode, dan **memahami biaya, manfaat, dan batasannya**. Fokus: kecepatan, ukuran masalah yang bisa ditangani, efisiensi energi.

### 1.4 Kenapa tidak otomatis saja?

- Menjalankan banyak salinan program serial tidak berguna (bayangkan membuka 8 jendela game yang sama — game-nya tidak jadi lebih cepat).
- Penerjemah otomatis serial→paralel **sangat sulit** dan hasilnya terbatas/tidak efisien.
- Sering kali solusi paralel terbaik adalah **merancang algoritma baru**, bukan sekadar memecah algoritma lama.

### 1.5 Contoh klasik: menjumlahkan n angka

**Versi serial:**
```c
sum = 0;
for (i = 0; i < n; i++) {
    x = Compute_next_value(...);
    sum += x;
}
```

**Versi paralel (p core, p jauh lebih kecil dari n):** tiap core menjumlahkan ±n/p angka miliknya sendiri di variabel **private** `my_sum`:
```c
my_sum = 0;
my_first_i = ...; my_last_i = ...;   // jatah indeks milik core ini
for (my_i = my_first_i; my_i < my_last_i; my_i++) {
    my_x = Compute_next_value(...);
    my_sum += my_x;
}
```

Contoh: 8 core, 24 angka `1,4,3 | 9,2,8 | 5,1,1 | 5,2,7 | 2,5,0 | 4,1,8 | 6,5,1 | 2,3,9`

| Core | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|---|
| my_sum | 8 | 19 | 7 | 15 | 7 | 13 | 12 | 14 |

**Cara 1 – Master mengerjakan semuanya:** semua core mengirim `my_sum` ke core 0, core 0 menjumlahkan → 95.
Core 0 melakukan **7 kali terima + 7 kali tambah**.

**Cara 2 – Pohon (tree) ✅ lebih baik:**
```
Core:   0    1    2    3    4    5    6    7
       (8) (19) (7) (15) (7) (13) (12) (14)
        \  /     \  /     \  /     \  /
        27        22       20       26        ← langkah 1: pasangan (0+1, 2+3, 4+5, 6+7)
          \      /           \     /
            49                 46             ← langkah 2: (0+2, 4+6)
               \              /
                     95                       ← langkah 3: (0+4)
```
Core 0 hanya **3 kali terima + 3 kali tambah**.

| Jumlah core | Cara 1 (master) | Cara 2 (pohon) |
|---|---|---|
| 8 | 7 langkah | 3 langkah |
| 1000 | 999 langkah | 10 langkah (≈ log₂ 1000) |

> 🏆 **Analogi:** Turnamen sepak bola sistem gugur. 1000 tim tidak perlu bertanding satu per satu melawan juara bertahan (999 pertandingan berturut-turut untuk satu tim), cukup 10 babak gugur.

### 1.6 Dua cara membagi pekerjaan

**Analogi Professor P (dari slide):** ada 300 lembar ujian, masing-masing 15 soal, dan 3 asisten (TA).

| Pembagian | Cara | Jenis |
|---|---|---|
| Tiap TA menilai 100 lembar (semua soal) | Bagi **data** | **Data parallelism** |
| TA#1 soal 1–5, TA#2 soal 6–10, TA#3 soal 11–15 (di semua lembar) | Bagi **tugas** | **Task parallelism** |

Pada contoh penjumlahan: menghitung `my_sum` = data parallelism; sedangkan core master yang bertugas *menerima* dan *menjumlah* = task parallelism (tugasnya berbeda dari core lain).

### 1.7 Tiga hal yang harus dikoordinasi

1. **Komunikasi** – core saling mengirim hasil (mis. partial sum).
2. **Load balancing** – bagi kerja secara merata agar tidak ada yang kewalahan.
3. **Sinkronisasi** – karena tiap core punya kecepatan sendiri, pastikan tidak ada yang "berlari terlalu jauh" mendahului yang lain.

> 🍳 **Analogi dapur:** Koki harus saling oper bahan (komunikasi), jumlah pesanan per koki harus adil (load balancing), dan semua hidangan satu meja harus keluar bersamaan (sinkronisasi).

### 1.8 Dua jenis sistem paralel

```
   SHARED MEMORY                         DISTRIBUTED MEMORY
 ┌───────┐                          ┌─────────────────┐
 │Core 0 ├──┐                       │ Core 0 ─ Mem 0  ├──┐
 ├───────┤  │   ┌────────┐          ├─────────────────┤  │  ┌─────────┐
 │Core 1 ├──┼───┤ Memori │          │ Core 1 ─ Mem 1  ├──┼──┤ Network │
 ├───────┤  │   │ bersama│          ├─────────────────┤  │  └─────────┘
 │Core p ├──┘   └────────┘          │ Core p ─ Mem p  ├──┘
 └───────┘                          └─────────────────┘
```

| | Shared memory | Distributed memory |
|---|---|---|
| Memori | Satu, dipakai bersama | Tiap core punya memori **pribadi** |
| Cara koordinasi | Membaca/menulis lokasi memori bersama | **Mengirim pesan** lewat jaringan |
| Analogi | Satu papan tulis di satu ruangan | Tiap orang di rumah masing-masing, berkomunikasi via WhatsApp |
| Alat pemrograman | Pthreads, OpenMP | MPI |

### 1.9 Tiga istilah yang sering tertukar

- **Concurrent computing:** beberapa tugas *sedang berlangsung* pada saat yang sama (bisa saja bergantian di satu core). Contoh: kamu memasak sambil menjawab chat — bergantian, tapi keduanya "sedang berjalan".
- **Parallel computing:** beberapa tugas **bekerja sama erat** untuk menyelesaikan satu masalah (benar-benar bersamaan).
- **Distributed computing:** sebuah program perlu **bekerja sama dengan program lain** (biasanya di mesin lain) untuk menyelesaikan masalah. Contoh: aplikasi ojek online yang bicara dengan server peta dan server pembayaran.

### 1.10 Bahasa yang dipakai di kuliah
Bahasa **C** dengan ekstensi: **MPI**, **Pthreads**, **OpenMP** (juga disebut TBB, Cilk), dan **CUDA**.

### ✅ Inti yang harus diingat (Bab 1)
- Prosesor tidak bisa terus dipercepat karena masalah panas/daya → solusinya multicore.
- Program serial **tidak** otomatis memanfaatkan multicore; programmer harus menulis program paralel.
- Data parallelism = bagi data; task parallelism = bagi tugas.
- Koordinasi = komunikasi + load balancing + sinkronisasi.
- Shared memory (papan tulis bersama) vs distributed memory (kirim pesan).
- Penjumlahan pohon: p core cukup ±log₂ p langkah.

<details><summary>🧠 Uji diri Bab 1 (klik untuk jawaban)</summary>

1. **Mengapa clock speed tidak dinaikkan terus?** → Daya & panas naik sehingga prosesor tidak andal.
2. **Ada 1024 core; berapa langkah penjumlahan pohon?** → log₂ 1024 = 10 langkah.
3. **Pembagian "TA#1 menilai lembar 1–100" itu data atau task parallelism?** → Data parallelism.
4. **Bedanya concurrent vs parallel?** → Concurrent: beberapa tugas sedang berlangsung (bisa bergantian). Parallel: tugas-tugas bekerja sama erat dan berjalan bersamaan.
</details>

---

## Bab 2 – Hardware Paralel (slide 02, bagian 1)

### 2.1 ILP – Paralelisme di dalam satu core (otomatis)

#### a) Pipelining 🚗
**Ide:** pecah pekerjaan menjadi tahap-tahap; tiap tahap dikerjakan unit berbeda, sehingga beberapa pekerjaan bisa "mengantre di tahap yang berbeda" secara bersamaan.

> 🚗 **Analogi (dari slide): jalur perakitan mobil / laundry.** Tanpa pipeline: cuci → keringkan → setrika baju A, baru mulai baju B. Dengan pipeline: saat baju A dikeringkan, baju B sudah mulai dicuci.

Contoh slide – penjumlahan floating point 9.87×10⁴ + 6.54×10³ butuh 7 tahap:

| Tahap | Operasi | Hasil |
|---|---|---|
| 1 | Fetch operands | 9.87×10⁴, 6.54×10³ |
| 2 | Compare exponents | |
| 3 | Shift one operand | 6.54×10³ → 0.654×10⁴ |
| 4 | Add | 10.524×10⁴ |
| 5 | Normalize | 1.0524×10⁵ |
| 6 | Round | 1.05×10⁵ |
| 7 | Store | 1.05×10⁵ |

Loop `for (i=0;i<1000;i++) z[i] = x[i] + y[i];`, tiap tahap 1 ns:
- **Tanpa pipeline:** 1000 × 7 = **7000 ns**.
- **Dengan pipeline:** penjumlahan pertama selesai di ns ke-7, setelah itu **satu hasil keluar tiap ns** → 7 + 999 = **1006 ns**.

> Satu penjumlahan tetap 7 ns (latensi sama), tetapi **throughput** (jumlah hasil per waktu) naik hampir 7×.

#### b) Multiple issue 🛒
**Ide:** gandakan unit fungsional (mis. 2 adder) dan jalankan beberapa instruksi sekaligus. Adder #1 mengerjakan z[1], z[3], …; adder #2 mengerjakan z[2], z[4], ….

> 🛒 **Analogi:** supermarket membuka 2 kasir, bukan 1.

- **Static multiple issue:** jadwal ditentukan **compiler** (saat kompilasi).
- **Dynamic multiple issue:** jadwal ditentukan **prosesor saat runtime** → disebut **superscalar**.

#### c) Speculation 🔮
Agar multiple issue berguna, prosesor harus mencari instruksi yang bisa dijalankan bersamaan — kadang dengan **menebak**.
```c
z = x + y;
if (z > 0) w = x;   // prosesor menebak "z pasti positif", langsung kerjakan w = x
else       w = y;   // kalau tebakan salah → batalkan, hitung ulang w = y
```
> 🔮 **Analogi:** Kamu menanak nasi sebelum tamu memastikan datang. Kalau tamu datang, waktu hemat. Kalau tidak, ya sudah, buang dan rugi sedikit.

### 2.2 Hardware multithreading 🧑‍🍳
Saat thread yang sedang jalan **stall** (misalnya menunggu data dari memori yang lambat), core bisa mengerjakan thread lain.

| Jenis | Cara kerja | Kelebihan | Kekurangan |
|---|---|---|---|
| **Fine-grained** | Ganti thread **setiap instruksi**, lewati yang stall | Hampir tidak ada waktu terbuang karena stall | Thread yang siap jalan panjang harus menunggu giliran tiap instruksi |
| **Coarse-grained** | Ganti thread **hanya saat stall lama** | Pergantian tidak perlu instan | Core bisa menganggur saat stall pendek; pergantian juga butuh waktu |
| **SMT (Simultaneous Multithreading)** | Variasi fine-grained: **beberapa thread memakai unit-unit fungsional berbeda secara bersamaan** | Pemakaian unit fungsional maksimal | — |

> 🧑‍🍳 **Analogi:** Koki sedang menunggu air mendidih (stall). Daripada bengong, ia memotong sayur untuk pesanan lain.
> - Fine-grained: koki berpindah pesanan setiap satu gerakan.
> - Coarse-grained: koki baru pindah kalau harus menunggu lama (air mendidih 10 menit).
> - SMT: kompor dipakai pesanan A, pisau dipakai pesanan B, pada saat yang sama.
> (Intel Hyperthreading adalah contoh SMT.)

### 2.3 Taksonomi Flynn 🗂️
Mengelompokkan komputer berdasarkan jumlah **aliran instruksi** dan **aliran data**:

| | Single Data | Multiple Data |
|---|---|---|
| **Single Instruction** | **SISD** – komputer klasik von Neumann | **SIMD** – satu perintah untuk banyak data |
| **Multiple Instruction** | **MISD** – (tidak dibahas) | **MIMD** – banyak core independen |

### 2.4 SIMD 🧘
- Satu **control unit** mengirim instruksi yang **sama** ke banyak **ALU**, masing-masing memproses data berbeda. Disebut **data parallelism**.

> 🧘 **Analogi:** Instruktur senam memberi aba-aba "angkat tangan kanan!" dan 30 peserta melakukannya bersamaan.

- Jika ALU lebih sedikit dari data → kerjakan bergelombang. Contoh 4 ALU, 15 data:

| Ronde | ALU1 | ALU2 | ALU3 | ALU4 |
|---|---|---|---|---|
| 1 | x[0] | x[1] | x[2] | x[3] |
| 2 | x[4] | x[5] | x[6] | x[7] |
| 3 | x[8] | x[9] | x[10] | x[11] |
| 4 | x[12] | x[13] | x[14] | (menganggur) |

- **Kelemahan SIMD:**
  - Semua ALU harus menjalankan instruksi yang sama atau **diam**. Contoh `if (x[i]%2==0) x[i]+=y[i]; else x[i]-=y[i];` → saat cabang `if` dijalankan, ALU yang datanya ganjil menganggur, lalu gantian.
  - Desain klasik harus sinkron; ALU tidak menyimpan instruksi sendiri.
  - Bagus untuk masalah data-paralel besar, buruk untuk masalah paralel yang kompleks.

> 🧘 Kalau instruktur bilang "yang memakai baju merah angkat tangan, yang lain diam", sebagian peserta menganggur.

### 2.5 Vector processor 🥄➡️🪣
CPU biasa memproses **satu angka (skalar)** per instruksi; vector processor memproses **satu deret angka (vektor)** per instruksi.

> 🥄 **Analogi:** Memindahkan pasir dengan sendok (skalar) vs dengan sekop (vektor).

Komponen kunci:
- **Vector register:** register yang menampung banyak operand sekaligus.
- **Unit fungsional ter-vektorisasi & ter-pipeline:** operasi sama diterapkan ke tiap elemen.
- **Instruksi vektor:** bekerja pada vektor, bukan skalar.
- **Interleaved memory:** memori dibagi ke beberapa **bank** yang bisa diakses hampir independen; elemen vektor disebar ke bank berbeda supaya bisa dimuat berurutan tanpa menunggu.
  > 🏦 Seperti bank dengan banyak teller: nasabah berurutan dilayani teller berbeda, jadi tidak menunggu satu teller.
- **Strided access & scatter/gather:** mengakses elemen pada jarak tetap (mis. setiap elemen ke-4).

| ✅ Kelebihan | ❌ Kekurangan |
|---|---|
| Cepat, mudah dipakai | Kurang baik untuk struktur data tidak teratur (irregular) |
| Compiler pintar mendeteksi kode yang bisa divektorisasi & memberi info yang tidak bisa | Kemampuan menangani masalah yang makin besar terbatas (**skalabilitas**) |
| Bandwidth memori tinggi, memakai setiap item di cache line | |

### 2.6 GPU 🎮
- Grafis 3D direpresentasikan sebagai titik, garis, segitiga. **Graphics pipeline** mengubahnya menjadi piksel di layar.
- Beberapa tahap pipeline bisa diprogram (**shader function**, biasanya beberapa baris kode C).
- Shader secara alami paralel (diterapkan ke jutaan titik/piksel) → GPU memakai **SIMD** (walau bukan SIMD murni).
- **GPU integrated** (di dalam chip CPU) vs **GPU discrete** (kartu terpisah, punya banyak *streaming multiprocessor* dan DRAM sendiri, data harus lewat bus **PCI**).

### 2.7 MIMD
Banyak core yang **sepenuhnya independen**, masing-masing punya control unit dan ALU sendiri. Inilah kebanyakan komputer modern. Dua jenis: shared memory & distributed memory.

### 2.8 Shared memory system: UMA vs NUMA 🧊
Prosesor-prosesor terhubung ke memori lewat interconnect; tiap prosesor bisa mengakses semua lokasi memori; berkomunikasi **implisit** lewat data bersama.

| | **UMA** (Uniform Memory Access) | **NUMA** (Non-Uniform Memory Access) |
|---|---|---|
| Struktur | Semua chip terhubung ke **satu** memori lewat satu interconnect | Tiap chip punya memori **lokal**; chip saling terhubung |
| Waktu akses | **Sama** untuk semua lokasi memori | Memori lokal **lebih cepat** daripada memori milik chip lain |
| Analogi | Satu kulkas di tengah rumah, semua orang berjarak sama | Tiap kamar punya kulkas mini; ambil dari kulkas kamar sendiri cepat, dari kamar lain lebih lama |

### 2.9 Distributed memory system: cluster 🏢
- **Cluster** (paling populer): kumpulan komputer biasa (commodity) yang dihubungkan jaringan biasa.
- Tiap komputer disebut **node**. Karena node sendiri biasanya multicore (shared memory), cluster sering disebut **sistem hybrid**.
- Komputer modern umumnya heterogen: beberapa node, tiap node berisi CPU multicore + GPU integrated + GPU discrete + DRAM.

### 2.10 Jaringan interkoneksi 🛣️
Sangat memengaruhi kinerja kedua jenis sistem.

**Untuk shared memory:**
- **Bus:** sekumpulan kabel yang dipakai bersama. Makin banyak perangkat → makin rebutan → makin lambat.
  > 🛣️ Jalan satu lajur yang dipakai semua kendaraan.
- **Switched interconnect / Crossbar:** switch mengatur rute; banyak perangkat bisa berkomunikasi **bersamaan**; lebih cepat dari bus tetapi **mahal**.
  > 🛣️ Persimpangan dengan banyak jalan layang — tidak ada yang saling menunggu, tapi biaya bangunnya besar.

**Untuk distributed memory:**
- **Direct interconnect:** tiap switch terhubung langsung ke satu pasangan prosesor-memori, switch saling terhubung (contoh: ring, mesh, hypercube).
- **Indirect interconnect:** switch tidak harus terhubung langsung ke prosesor (contoh: crossbar, omega network).

**Ukuran kualitas jaringan:**
- **Bisection width:** potong jaringan menjadi dua bagian sama besar; berapa komunikasi yang bisa berjalan **bersamaan melintasi potongan**? (Diambil potongan terburuk.)
  > ✂️ Seperti menghitung berapa jembatan yang menghubungkan dua sisi kota. Makin banyak jembatan, makin lancar.
  *(tambahan, dari buku Pacheco)* ring p node → bisection width 2; toroidal mesh → 2√p; hypercube → p/2; crossbar penuh → p.
- **Bandwidth:** laju transmisi data sebuah link (Mbit/s atau MB/s).
- **Bisection bandwidth:** jumlah **bandwidth** semua link yang melintasi potongan (bukan jumlah link-nya).
- **Latency (l):** waktu dari sumber mulai mengirim sampai tujuan mulai menerima **byte pertama**.
- **Bandwidth (b):** laju tujuan menerima data **setelah** byte pertama tiba.

### 🧮 Rumus waktu kirim pesan
```
Waktu kirim pesan = l + n / b
      l = latency (detik), n = ukuran pesan (byte), b = bandwidth (byte/detik)
```
> 🚿 **Analogi selang air:** latency = waktu dari keran dibuka sampai air pertama keluar dari ujung selang; bandwidth = besar diameter selang. Mengisi ember (n liter) = waktu tunggu awal + n / laju aliran.

Contoh: l = 2 µs, b = 1 GB/s, pesan 1 MB → 2 µs + 10⁶/10⁹ s = 2 µs + 1000 µs = 1002 µs. Untuk pesan **kecil** (8 byte): 2 µs + 0,008 µs ≈ 2 µs → **latency yang dominan**. Maka lebih baik mengirim sedikit pesan besar daripada banyak pesan kecil.

### 2.11 Cache coherence 📝
Setiap core punya **cache** (salinan kecil & cepat dari memori). Programmer **tidak bisa mengontrol** kapan cache diperbarui. Masalah muncul kalau beberapa core menyimpan salinan variabel yang sama.

Contoh slide (x = 2 adalah variabel bersama):

| Waktu | Core 0 | Core 1 |
|---|---|---|
| 0 | `y0 = x;` | `y1 = 3*x;` |
| 1 | `x = 7;` | (tidak menyentuh x) |
| 2 | (tidak menyentuh x) | `z1 = 4*x;` |

- y0 = 2 ✅, y1 = 6 ✅, **z1 = ???** → seharusnya 4×7 = **28**, tetapi kalau cache Core 1 masih menyimpan x lama, hasilnya **8** ❌.

> 📝 **Analogi:** Dua orang menyalin jadwal dari papan pengumuman ke buku catatan masing-masing. Lalu jadwal di papan diganti. Orang kedua masih memakai catatan lama → salah jadwal.

**Dua solusi:**
- **Snooping:** semua core terhubung ke **bus**. Saat Core 0 mengubah x, ia **mengumumkan** di bus; Core 1 yang "menguping" (snooping) menandai salinan x miliknya **invalid**.
  > 📢 Seperti pengumuman lewat toa: "Jadwal berubah! Coret catatan lama kalian!"
  > Kelemahan *(tambahan)*: broadcast tidak skala untuk sistem besar.
- **Directory-based:** ada struktur data **directory** yang mencatat status setiap cache line (siapa yang menyimpan salinan). Saat variabel diubah, directory dicek, dan **hanya** core yang memegang salinan yang diberi tahu (di-invalidate).
  > 📒 Seperti buku daftar peminjam: hanya orang yang tercatat memegang fotokopi yang ditelepon.

*(Masalah turunan dari cache, yaitu **false sharing**, dibahas di Bab 5.)*

### ✅ Inti yang harus diingat (Bab 2)
- Pipelining: latensi per operasi tetap, throughput naik (1000 op × 7 tahap = 1006 ns, bukan 7000 ns).
- Superscalar = dynamic multiple issue. Speculation = menebak, kalau salah diulang.
- Fine-grained / coarse-grained / SMT.
- Flynn: SISD, SIMD, MISD, MIMD.
- SIMD lemah pada percabangan; vector processor lemah pada data tidak teratur & skalabilitas.
- UMA: waktu akses sama; NUMA: lokal lebih cepat.
- Waktu pesan = l + n/b. Bisection width/bandwidth = ukuran kualitas jaringan.
- Cache coherence: snooping (broadcast di bus) vs directory (daftar pemegang salinan).

<details><summary>🧠 Uji diri Bab 2</summary>

1. **Pipeline 5 tahap, 1 ns per tahap, 100 operasi. Berapa waktunya?** → 5 + 99 = 104 ns (tanpa pipeline 500 ns).
2. **Superscalar termasuk static atau dynamic multiple issue?** → Dynamic.
3. **Kenapa SIMD tidak efisien untuk kode yang banyak `if`?** → Semua ALU harus menjalankan instruksi yang sama; ALU yang tidak memenuhi cabang menganggur.
4. **Pesan 8 byte vs 8 MB, mana yang didominasi latency?** → Pesan 8 byte.
5. **Pada contoh cache coherence, berapa z1 jika koheren?** → 28.
</details>

---

## Bab 3 – Software Paralel & Mengukur Kinerja (slide 02, bagian 2)

### 3.1 Beban ada di software
Hardware & compiler sudah tidak bisa "menyulap" kecepatan secara otomatis. Mulai sekarang:
- **Shared memory:** jalankan **satu proses**, lalu proses itu membuat (*fork*) banyak **thread**; thread mengerjakan tugas.
- **Distributed memory:** jalankan **banyak proses**; proses mengerjakan tugas.

### 3.2 Empat teknik umum paralelisasi (Robey & Zamora)
| Teknik | Cara | Contoh alat |
|---|---|---|
| **Process-based** | Library message passing membuat banyak proses (rank 0, 1, 2, …), OS menempatkannya di core-core node; tiap proses punya memori sendiri | MPI |
| **Thread-based** | Satu proses membuat banyak thread dalam satu node; sebagian memori dipakai bersama | Pthreads, OpenMP |
| **Vectorization** | Banyak operasi dengan satu instruksi; hasil bergantung pada kecerdasan compiler (compiler A bisa 2× lebih cepat, compiler B 3×) | AVX, `omp simd` |
| **Stream processing** | Data & kernel dikirim ke GPU lewat bus PCI, diproses di streaming multiprocessor, hasilnya dikembalikan ke CPU | CUDA, OpenCL |

### 3.3 SPMD – Single Program, Multiple Data
Satu program (executable) yang dijalankan banyak kali, tetapi tiap salinan bertingkah berbeda berdasarkan **identitasnya** (rank/id).
```c
if (I'm thread/process i)
    do this;
else
    do that;
```
> 🎭 **Analogi:** Satu naskah drama dibagikan ke semua pemain. Setiap pemain hanya membaca dialog untuk perannya sendiri.

### 3.4 Langkah menulis program paralel
1. **Bagi pekerjaan** ke proses/thread: (a) tiap bagian kira-kira **sama besar** (load balancing), (b) komunikasi **seminimal** mungkin.
2. Atur **sinkronisasi**.
3. Atur **komunikasi**.

### 3.5 Thread dinamis vs statis
| | Dynamic threads | Static threads |
|---|---|---|
| Cara | Master menunggu kerja, membuat thread baru saat ada kerja; thread mati setelah selesai | Kolam (*pool*) thread dibuat di awal, diberi kerja, tidak mati sampai program selesai |
| + | Hemat resource | Kinerja lebih baik |
| − | Membuat/menghapus thread memakan waktu | Bisa memboroskan resource |
| Analogi | Memanggil pekerja harian setiap ada proyek | Punya karyawan tetap |

### 3.6 Nondeterminisme, race condition, dan cara mengatasinya ⚠️
**Nondeterminisme:** program yang sama + input yang sama → output bisa **berbeda** karena urutan eksekusi thread berubah-ubah.

Contoh 1 – urutan cetak:
```
Thread 0 > my_val = 7          Thread 1 > my_val = 19
Thread 1 > my_val = 19   atau  Thread 0 > my_val = 7
```

Contoh 2 – **race condition** (lebih berbahaya):
```c
my_val = Compute_val(my_rank);
x += my_val;          // x awalnya 0
```
| Waktu | Core 0 | Core 1 |
|---|---|---|
| 1 | Load x = 0 ke register | Selesai menghitung my_val |
| 2 | Load my_val = 7 | Load x = 0 ke register |
| 3 | Tambah 7 ke x | Load my_val = 19 |
| 4 | Store x = 7 | Tambah 19 ke x |
| 5 | … | Store x = **19** ❌ (seharusnya 26) |

> 🏦 **Analogi:** Saldo rekening Rp0. Dua orang menyetor bersamaan dari ATM berbeda (Rp7 dan Rp19). Keduanya membaca saldo Rp0, menambahkan setorannya, lalu menyimpan. Hasil akhirnya Rp19 — setoran Rp7 hilang!

**Istilah penting:**
- **Race condition:** beberapa thread mengakses resource bersama, minimal satu **menulis**, dan hasilnya bergantung pada urutan → bisa salah.
- **Critical section:** bagian kode yang mengubah resource bersama dan hanya boleh dijalankan **satu thread pada satu waktu**.
- **Mutually exclusive:** saling meniadakan (tidak boleh bersamaan).
- **Mutex / lock:** "kunci" untuk critical section.

```c
my_val = Compute_val(my_rank);
Lock(&add_my_val_lock);     // 🔒 masuk, kunci pintu
x += my_val;                // critical section
Unlock(&add_my_val_lock);   // 🔓 keluar, buka kunci
```
> 🚻 **Analogi:** Kamar mandi dengan satu kunci. Siapa pun yang masuk mengunci pintu; yang lain menunggu di luar.

**Busy-waiting** (alternatif tanpa lock):
```c
if (my_rank == 1)
    while (!ok_for_1);   // terus-menerus mengecek, tidak mengerjakan apa pun
x += my_val;
if (my_rank == 0)
    ok_for_1 = true;     // izinkan thread 1
```
> 🚪 Seperti berdiri di depan pintu kamar mandi sambil terus memutar gagang pintu setiap detik. Membuang tenaga (CPU).

**Message passing** (distributed memory):
```c
if (my_rank == 1) {
    sprintf(message, "Greetings from process 1");
    Send(message, MSG_CHAR, 100, 0);          // kirim ke proses 0
} else if (my_rank == 0) {
    Receive(message, MSG_CHAR, 100, 1);       // terima dari proses 1
    printf("Process 0 > Received: %s\n", message);
}
```

**PGAS (Partitioned Global Address Space):** bahasa yang membuat memori terdistribusi *terlihat* seperti satu memori bersama, tetapi tiap bagian "dimiliki" proses tertentu (variabel bisa `shared` atau `private`):
```c
shared int n = ...;
shared double x[n], y[n];
private int i, my_first_element, my_last_element;
for (i = my_first_element; i <= my_last_element; i++)
    x[i] += y[i];
```
> 🏘️ Seperti perumahan dengan satu peta alamat bersama — kamu bisa menulis surat ke alamat mana pun, tapi mengakses rumahmu sendiri jauh lebih cepat.

### 3.7 Aturan input/output (I/O)
- Hanya **proses 0** (distributed) / **thread 0 atau master** (shared) yang membaca **stdin**.
- Semua boleh menulis ke **stdout/stderr**, tetapi karena urutannya acak, biasanya **hanya satu** proses/thread yang mencetak hasil (kecuali untuk debug).
- Output debug **wajib menyertakan rank/id** pembuatnya.
- Satu file (selain stdin/stdout/stderr) hanya diakses satu proses/thread; tiap proses boleh punya file pribadi.

### 3.8 Apa yang membatasi kinerja?
Batasan hardware: FLOPS/ops CPU, **memory bandwidth**, **memory latency**, instruction cache, jaringan, disk. Dikelompokkan menjadi:
- **Speed:** seberapa cepat operasi dikerjakan.
- **Feed:** seberapa cepat data bisa **disuplai** (bandwidth memori, cache, jaringan, disk).

> 🏭 **Analogi:** Mesin pabrik super cepat tidak berguna kalau truk bahan bakunya lambat. Kebanyakan program modern justru dibatasi "truk"-nya (memory bound), bukan "mesin"-nya.

- Kalau data tidak bisa dialirkan secara *streaming* (berurutan), **latency** menjadi penentu, dan latency bisa berorde-orde lebih lambat dari bandwidth. Mengatur data agar dibaca berurutan bisa memberi percepatan dramatis.
- Grafik di slide (bergaya *roofline*): CPU skalar 3 GFLOP/s; dengan FMA 6; vektor 256-bit 24; multicore 4 core × 2 HT 192 GFLOP/s. Bandwidth: L1 1800 GB/s, L3 600 GB/s, DRAM 24 GB/s, SSD 0,6 GB/s, jaringan 1 Gbps 0,1 GB/s. Makin jauh datanya, makin lambat suplainya.

**Alat ukur (benchmarking):**
- `lstopo` (paket **hwloc**): menggambar topologi mesin (socket, NUMA node, cache L1/L2/L3, core, hyperthread/PU).
- **STREAM benchmark:** mengukur *sustained memory bandwidth* dengan 4 kernel:

| Kernel | Operasi | Byte/iterasi | Operasi aritmetika |
|---|---|---|---|
| Copy | a(i) = b(i) | 16 | 0 |
| Scale | a(i) = q·b(i) | 16 | 1 |
| Sum | a(i) = b(i) + c(i) | 24 | 1 |
| Triad | a(i) = b(i) + q·c(i) | 24 | 2 |

### 3.9 Speedup dan efisiensi 📈
```
p          = jumlah core
T_serial   = waktu program serial
T_parallel = waktu program paralel

Speedup     S = T_serial / T_parallel
Efisiensi   E = S / p = T_serial / (p × T_parallel)

Ideal (linear speedup): T_parallel = T_serial / p  →  S = p, E = 1
```
> 👷 **Analogi:** Membangun pagar sendiri butuh 8 jam. Dengan 4 orang selesai 2,5 jam → S = 8/2,5 = 3,2; E = 3,2/4 = 0,8 → tiap orang rata-rata 80% produktif (sisanya habis untuk koordinasi, menunggu, dll.).

Tabel dari slide (satu program):

| p | 1 | 2 | 4 | 8 | 16 |
|---|---|---|---|---|---|
| S | 1,0 | 1,9 | 3,6 | 6,5 | 10,8 |
| E | 1,0 | 0,95 | 0,90 | 0,81 | 0,68 |

Pengaruh ukuran masalah:

| Ukuran | p=16: S | p=16: E |
|---|---|---|
| Setengah | 6,2 | 0,39 |
| Asli | 10,8 | 0,68 |
| Dua kali | 14,2 | 0,89 |

👉 **Pola penting:** makin banyak core → efisiensi turun; makin besar masalah → efisiensi naik.

**Kenapa begitu?** Karena ada **overhead** (komunikasi, sinkronisasi, membuat thread):
```
T_parallel = T_serial / p + T_overhead
```
Masalah besar → porsi kerja "sungguhan" jauh lebih besar dari overhead → efisien.

### 3.10 Hukum Amdahl ⚖️
> "Kecuali **hampir seluruh** program diparalelkan, speedup akan sangat terbatas — **berapa pun** jumlah core-nya."

```
S = fraksi serial, P = fraksi paralel, S + P = 1, N = jumlah prosesor

SpeedUp(N) = 1 / (S + P/N)          Batas maksimum (N → ∞) = 1 / S
```

**Contoh slide:** 90% program bisa diparalelkan sempurna, T_serial = 20 detik.
- Bagian paralel: 0,9 × 20 / p = 18/p
- Bagian serial: 0,1 × 20 = 2 detik (tidak bisa dikurangi!)
- T_parallel = 18/p + 2 → S = 20 / (18/p + 2)

| p | 1 | 2 | 4 | 8 | 16 | ∞ |
|---|---|---|---|---|---|---|
| S | 1 | 1,82 | 3,08 | 4,71 | 6,40 | **10** |

Grafik slide (32 prosesor): 90% paralel → ±7,8×; 75% → ±3,7×; 50% → ±1,9×.

> 🚗 **Analogi:** Perjalanan Bandung–Jakarta: 2 jam di tol (bisa dipercepat dengan mobil lebih cepat) + 20 menit macet di gerbang tol (tidak bisa dipercepat). Secepat apa pun mobilmu — bahkan pesawat — kamu tetap terjebak 20 menit itu. Bagian serial = gerbang tol.

### 3.11 Hukum Gustafson–Barsis 📏
Amdahl berasumsi ukuran masalah **tetap**. Gustafson berkata: dalam praktik, kalau prosesor bertambah, kita akan mengerjakan **masalah yang lebih besar** (simulasi lebih detail, data lebih banyak). Jika ukuran masalah naik sebanding dengan jumlah prosesor:
```
SpeedUp(N) = N − S × (N − 1)      (= S + N × P)
```
Contoh: S = 0,1, N = 16 → 16 − 0,1 × 15 = **14,5** (jauh lebih optimis daripada Amdahl yang 6,4).

| | Amdahl (fixed-size) | Gustafson (scaled-size) |
|---|---|---|
| Asumsi | Ukuran masalah tetap | Ukuran masalah membesar sebanding N |
| Waktu paralel | s + p/N (waktu serial dinormalisasi = 1) | 1 (waktu serial hipotetis = s + N·p) |
| Speedup | 1/(s + p/N) | s + N·p |
| Kesan | Pesimis, ada plafon | Optimis, hampir linear |

> 🍲 **Analogi:** Amdahl: "Dengan 10 koki, seberapa cepat kita memasak **1 panci sup**?" (tidak banyak membantu, ada langkah yang hanya bisa dikerjakan 1 orang). Gustafson: "Dengan 10 koki, kita bisa memasak **10 panci sup** dalam waktu yang sama."

### 3.12 Strong scaling vs weak scaling 🍰
| | **Strong scaling** | **Weak scaling** |
|---|---|---|
| Yang tetap | **Ukuran total** masalah | **Ukuran per prosesor** |
| Contoh slide | Mesh 1000×1000 dibagi: 1 proc (1000²), 4 proc (500² masing-masing), 16 proc (250² masing-masing) | Tiap prosesor tetap 1000²; total mesh membesar sesuai jumlah prosesor |
| Terkait hukum | Amdahl | Gustafson |
| Analogi | **Satu kue** dipotong untuk makin banyak orang | Tiap orang yang datang membawa **satu kue** lagi |

**Definisi scalability (slide):**
- Program **scalable** jika bisa menangani ukuran masalah yang terus naik (efisiensi bisa dipertahankan dengan menaikkan ukuran masalah).
- **Strongly scalable:** efisiensi tetap meski jumlah proses/thread bertambah **tanpa** menaikkan ukuran masalah.
- **Weakly scalable:** efisiensi tetap jika ukuran masalah dinaikkan **dengan laju yang sama** dengan jumlah proses/thread.

**Catatan memori (Figure 1.6):** pada weak scaling, array yang **direplikasi** (disalin penuh ke tiap prosesor) membengkak: 100 MB → 200 MB → 400 MB per prosesor, sedangkan array yang **didistribusikan** tetap 100 MB per prosesor. Jadi replikasi data membatasi skalabilitas.

### 3.13 Mengukur waktu ⏱️
Waktu yang dimaksud biasanya **wall clock time** (waktu jam dinding dari awal sampai akhir bagian yang diukur), bukan CPU time.
```c
double start, finish;                // harus PRIVATE per thread/proses
start = Get_current_time();          // di MPI: MPI_Wtime(); di OpenMP: omp_get_wtime()
/* kode yang diukur */
finish = Get_current_time();
printf("The elapsed time = %e seconds\n", finish - start);
```
Untuk program paralel, ukur waktu **thread/proses paling lambat**:
```c
shared  double global_elapsed;
private double my_start, my_finish, my_elapsed;
Barrier();                              // samakan garis start
my_start = Get_current_time();
/* kode yang diukur */
my_finish  = Get_current_time();
my_elapsed = my_finish - my_start;
global_elapsed = Global_max(my_elapsed); // ambil yang terlama
if (my_rank == 0) printf("The elapsed time = %e seconds\n", global_elapsed);
```
> 🏃 **Analogi:** Lomba estafet tim baru dianggap selesai ketika **pelari terakhir** sampai. Barrier = semua pelari berdiri di garis start sebelum peluit.

### ✅ Inti yang harus diingat (Bab 3)
- SPMD: satu program, perilaku dibedakan oleh rank/id.
- Race condition → lindungi critical section dengan mutex/lock (atau busy-wait, yang boros CPU).
- Hanya rank/thread 0 membaca stdin; output debug sertakan rank.
- S = T_serial/T_parallel; E = S/p; T_parallel = T_serial/p + T_overhead.
- Amdahl: 1/(S + P/N), plafon 1/S. Gustafson: N − S(N−1).
- Strong scaling = total tetap; weak scaling = per prosesor tetap.
- Ukur dengan wall clock, barrier dulu, ambil waktu maksimum.

<details><summary>🧠 Uji diri Bab 3</summary>

1. **T_serial = 100 s, T_parallel dengan 8 core = 20 s. Hitung S dan E.** → S = 5; E = 5/8 = 0,625.
2. **Fraksi serial 5%. Speedup maksimum menurut Amdahl?** → 1/0,05 = 20×.
3. **Fraksi serial 5%, N = 64, menurut Gustafson?** → 64 − 0,05 × 63 = 60,85.
4. **Mesh total dibuat tetap, prosesor ditambah — strong atau weak scaling?** → Strong.
5. **Mengapa `start` dan `finish` harus private?** → Agar tiap thread mencatat waktunya sendiri tanpa saling menimpa.
</details>

---
## Bab 4 – MPI: Pemrograman Distributed Memory (slide 03)

### 4.0 Analogi besar: 🏢 Kantor-kantor cabang yang berkirim surat

| MPI | Analogi |
|---|---|
| Proses | Satu kantor cabang (punya lemari arsip sendiri = memori sendiri) |
| Rank | Nomor kantor: 0, 1, 2, …, p−1 |
| Communicator | Grup WhatsApp berisi kantor-kantor yang boleh saling kirim pesan |
| `MPI_COMM_WORLD` | Grup "semua kantor" yang otomatis dibuat di awal |
| `MPI_Send` / `MPI_Recv` | Mengirim / menerima surat lewat kurir |
| Tag | Subjek/label surat (agar surat "gaji" tidak tertukar dengan surat "laporan") |
| Collective communication | Rapat yang **wajib dihadiri semua** anggota grup |

Satu-satunya cara bertukar data antar kantor adalah **mengirim pesan secara eksplisit** — tidak ada lemari arsip bersama.

### 4.1 Program MPI pertama

```c
#include <stdio.h>
#include <string.h>
#include <mpi.h>                       // wajib: header MPI

const int MAX_STRING = 100;

int main(void) {
    char greeting[MAX_STRING];
    int  comm_sz;                      // jumlah proses
    int  my_rank;                      // rank saya

    MPI_Init(NULL, NULL);              // 1. mulai MPI
    MPI_Comm_size(MPI_COMM_WORLD, &comm_sz);   // 2. ada berapa proses?
    MPI_Comm_rank(MPI_COMM_WORLD, &my_rank);   // 3. saya nomor berapa?

    if (my_rank != 0) {                // semua selain proses 0: kirim salam ke 0
        sprintf(greeting, "Greetings from process %d of %d!", my_rank, comm_sz);
        MPI_Send(greeting, strlen(greeting)+1, MPI_CHAR, 0, 0, MPI_COMM_WORLD);
    } else {                           // proses 0: cetak salam sendiri, lalu terima dari yang lain
        printf("Greetings from process %d of %d!\n", my_rank, comm_sz);
        for (int q = 1; q < comm_sz; q++) {
            MPI_Recv(greeting, MAX_STRING, MPI_CHAR, q, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
            printf("%s\n", greeting);
        }
    }
    MPI_Finalize();                    // 4. selesai, bersihkan
    return 0;
}
```

**Kompilasi & menjalankan:**
```bash
mpicc -g -Wall -o mpi_hello mpi_hello.c   # mpicc = "pembungkus" gcc; -g debug; -Wall semua warning
mpiexec -n 4 ./mpi_hello                  # jalankan 4 proses (bisa juga mpirun -n 4)
```
Output (urutan rapi karena proses 0 menerima berurutan q = 1, 2, 3):
```
Greetings from process 0 of 4!
Greetings from process 1 of 4!
Greetings from process 2 of 4!
Greetings from process 3 of 4!
```

**Aturan penamaan:** semua identifier MPI diawali `MPI_`, huruf setelah garis bawah kapital (`MPI_Send`, `MPI_Comm`).
**Kerangka wajib:** `MPI_Init` → … → `MPI_Finalize`. Jangan memanggil fungsi MPI sebelum Init atau sesudah Finalize.
**SPMD:** satu program dikompilasi sekali; `if (my_rank != 0) … else …` membuat proses 0 berperan beda.

### 4.2 Kirim & terima (point-to-point)

```c
int MPI_Send(void* buf, int count, MPI_Datatype type, int dest,   int tag, MPI_Comm comm);
int MPI_Recv(void* buf, int count, MPI_Datatype type, int source, int tag, MPI_Comm comm, MPI_Status* status);
```
| Parameter | Arti |
|---|---|
| `buf` | alamat data yang dikirim / tempat menampung |
| `count` | jumlah elemen (di Recv = kapasitas buffer) |
| `type` | `MPI_CHAR`, `MPI_INT`, `MPI_FLOAT`, `MPI_DOUBLE`, `MPI_LONG`, `MPI_UNSIGNED`, `MPI_BYTE`, `MPI_PACKED`, dst. |
| `dest`/`source` | rank tujuan / asal |
| `tag` | label pesan |
| `comm` | communicator |

**Kapan pesan "cocok" (message matching)?** Proses q memanggil `MPI_Send(dest = r, tag, comm)` dan proses r memanggil `MPI_Recv(source = q, tag, comm)` dengan **comm sama** dan **tag sama**.

**Menerima tanpa tahu detail:** pakai `MPI_ANY_SOURCE` (dari siapa saja), `MPI_ANY_TAG` (tag apa saja). Setelah menerima, cek amplopnya lewat `MPI_Status`:
```c
MPI_Status status;
MPI_Recv(buf, MAX, MPI_DOUBLE, MPI_ANY_SOURCE, MPI_ANY_TAG, comm, &status);
status.MPI_SOURCE;   // siapa pengirimnya
status.MPI_TAG;      // tag-nya apa
int count; MPI_Get_count(&status, MPI_DOUBLE, &count);   // berapa banyak datanya
```
> 📬 Seperti kotak surat kantor: kamu bisa menerima surat dari siapa saja, lalu melihat amplop untuk tahu pengirim dan perihalnya.

**Hal penting:**
- `MPI_Recv` **selalu menunggu (block)** sampai pesan yang cocok datang.
- `MPI_Send` bisa **menyalin ke buffer lalu langsung lanjut**, atau **menunggu** sampai penerima siap — tergantung implementasi & ukuran pesan. **Jangan berasumsi!**
- Diagram slide: kalau dua proses sama-sama **Receive dulu** → keduanya menunggu selamanya (**deadlock**). Solusi: urutan bergantian (satu Send dulu, yang lain Recv dulu), atau versi non-blocking (`MPI_Isend`, `MPI_Irecv`, lalu `MPI_Waitall`).

### 4.3 Studi kasus: Aturan Trapesium (menghitung integral) 📐
Luas di bawah kurva f(x) dari a ke b didekati dengan n trapesium selebar h = (b−a)/n:
```
Luas satu trapesium = h/2 × [f(x_i) + f(x_{i+1})]
Total ≈ h × [ f(x_0)/2 + f(x_1) + f(x_2) + … + f(x_{n−1}) + f(x_n)/2 ]
```
```c
// serial
h = (b - a) / n;
approx = (f(a) + f(b)) / 2.0;
for (i = 1; i <= n-1; i++) { x_i = a + i*h; approx += f(x_i); }
approx = h * approx;
```

**Merancang versi paralel (metodologi Foster):**
1. **Partisi** masalah menjadi tugas-tugas kecil (hitung luas tiap trapesium).
2. Identifikasi **komunikasi** antar tugas (semua luas harus dijumlahkan).
3. **Agregasi** tugas kecil menjadi tugas besar (satu proses mengerjakan sekumpulan trapesium berurutan).
4. **Petakan** tugas besar ke core.

> 🧱 **Analogi:** Mengecat tembok 1000 m². Pecah jadi 1000 petak 1 m² → sadari semua petak harus dilaporkan ke mandor → gabungkan jadi 4 bagian besar → beri satu bagian ke tiap tukang.

```c
int main(void) {
    int my_rank, comm_sz, n = 1024, local_n;
    double a = 0.0, b = 3.0, h, local_a, local_b, local_int, total_int;

    MPI_Init(NULL, NULL);
    MPI_Comm_rank(MPI_COMM_WORLD, &my_rank);
    MPI_Comm_size(MPI_COMM_WORLD, &comm_sz);

    h = (b - a) / n;                       // h sama untuk semua proses
    local_n = n / comm_sz;                 // jumlah trapesium per proses
    local_a = a + my_rank * local_n * h;   // titik awal jatah saya
    local_b = local_a + local_n * h;       // titik akhir jatah saya
    local_int = Trap(local_a, local_b, local_n, h);

    if (my_rank != 0) {
        MPI_Send(&local_int, 1, MPI_DOUBLE, 0, 0, MPI_COMM_WORLD);
    } else {
        total_int = local_int;
        for (int source = 1; source < comm_sz; source++) {
            MPI_Recv(&local_int, 1, MPI_DOUBLE, source, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
            total_int += local_int;
        }
        printf("With n = %d trapezoids, our estimate of the integral from %f to %f = %.15e\n",
               n, a, b, total_int);
    }
    MPI_Finalize();
    return 0;
}

double Trap(double left, double right, int count, double base_len) {
    double estimate = (f(left) + f(right)) / 2.0;
    for (int i = 1; i <= count-1; i++) estimate += f(left + i*base_len);
    return estimate * base_len;
}
```
Contoh: n = 1024, 4 proses → tiap proses 256 trapesium; proses 2 mulai dari a + 2·256·h.

### 4.4 Input & output di MPI
- **Output:** semua proses boleh `printf`, tetapi urutannya **acak** (contoh 6 proses "Does anyone have a toothpick?" keluar tidak berurutan).
- **Input:** kebanyakan implementasi hanya mengizinkan **proses 0** membaca stdin. Proses 0 harus `scanf`, lalu **mengirimkannya** ke proses lain.
```c
void Get_input(int my_rank, int comm_sz, double* a_p, double* b_p, int* n_p) {
    if (my_rank == 0) {
        printf("Enter a, b, and n\n");
        scanf("%lf %lf %d", a_p, b_p, n_p);
        for (int dest = 1; dest < comm_sz; dest++) {
            MPI_Send(a_p, 1, MPI_DOUBLE, dest, 0, MPI_COMM_WORLD);
            MPI_Send(b_p, 1, MPI_DOUBLE, dest, 0, MPI_COMM_WORLD);
            MPI_Send(n_p, 1, MPI_INT,    dest, 0, MPI_COMM_WORLD);
        }
    } else {
        MPI_Recv(a_p, 1, MPI_DOUBLE, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        MPI_Recv(b_p, 1, MPI_DOUBLE, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        MPI_Recv(n_p, 1, MPI_INT,    0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
    }
}
```
(Nanti versi yang lebih baik memakai `MPI_Bcast`.)

### 4.5 Collective communication 👥
**Masalah:** di program trapesium, proses 0 menerima satu per satu → proses 0 jadi "leher botol". Solusi: **penjumlahan pohon** (lihat Bab 1.5), yang di MPI sudah disediakan sebagai fungsi kolektif.

#### Ringkasan semua fungsi kolektif (dengan analogi kelas)

| Fungsi | Apa yang terjadi | Analogi |
|---|---|---|
| `MPI_Reduce` | Gabungkan data semua proses (jumlah, maks, dll.) → hasil **di satu proses** | Semua siswa menyetor uang kas ke **bendahara** |
| `MPI_Allreduce` | Seperti Reduce, tetapi hasil **diterima semua** proses | Hasil voting diumumkan ke **semua** siswa |
| `MPI_Bcast` | Data dari satu proses **disalin ke semua** | Ketua kelas membacakan pengumuman |
| `MPI_Scatter` | Satu array dipotong-potong, **tiap proses dapat satu potong** | Guru membagikan soal: tiap siswa dapat halaman berbeda |
| `MPI_Gather` | Potongan dari semua proses **dikumpulkan ke satu proses** | Guru mengumpulkan lembar jawaban |
| `MPI_Allgather` | Potongan dari semua proses dikumpulkan dan **semua mendapat salinan lengkap** | Semua siswa saling fotokopi catatan sehingga semua punya catatan lengkap |
| `MPI_Barrier` | Tunggu sampai **semua** proses tiba | Rombongan tour menunggu semua naik bus |

```
Reduce (SUM ke P0)       Bcast (dari P0)        Scatter (dari P0)         Gather (ke P0)
P0: 2 ─┐                 P0: 7 ──┬─> 7          P0: [a b c d] ┬─> a        P0: a ─┐
P1: 5 ─┼─> P0: 14        P1:     ├─> 7          P1:           ├─> b        P1: b ─┼─> P0: [a b c d]
P2: 3 ─┤                 P2:     ├─> 7          P2:           ├─> c        P2: c ─┤
P3: 4 ─┘                 P3:     └─> 7          P3:           └─> d        P3: d ─┘
```

#### MPI_Reduce
```c
int MPI_Reduce(void* input, void* output, int count, MPI_Datatype type,
               MPI_Op op, int dest_process, MPI_Comm comm);

MPI_Reduce(&local_int, &total_int, 1, MPI_DOUBLE, MPI_SUM, 0, MPI_COMM_WORLD);
```
Operator: `MPI_MAX`, `MPI_MIN`, `MPI_SUM`, `MPI_PROD`, `MPI_LAND` (AND logika), `MPI_BAND` (AND bit), `MPI_LOR`, `MPI_BOR`, `MPI_LXOR`, `MPI_BXOR`, `MPI_MAXLOC` (maks + lokasinya), `MPI_MINLOC`.
Bisa untuk array: `MPI_Reduce(local_x, sum, N, MPI_DOUBLE, MPI_SUM, 0, comm)` → menjumlah elemen per elemen.

#### ⚠️ Aturan kolektif vs point-to-point
1. **Semua** proses di communicator harus memanggil fungsi kolektif **yang sama**. (Memasangkan `MPI_Reduce` di satu proses dengan `MPI_Recv` di proses lain → hang/crash.)
2. Argumennya harus **kompatibel** (mis. semua harus menyebut `dest_process` yang sama).
3. Argumen `output` hanya dipakai di proses tujuan, tapi proses lain **tetap** harus mengisinya (boleh `NULL`).
4. Point-to-point dicocokkan berdasarkan **tag & communicator**. Kolektif **tidak memakai tag** — dicocokkan hanya berdasarkan **communicator dan URUTAN pemanggilan**.

**Contoh jebakan (slide):** a = 1, c = 2 di semua proses, operator SUM ke proses 0.

| Waktu | Proses 0 | Proses 1 | Proses 2 |
|---|---|---|---|
| 1 | `MPI_Reduce(&a, &b, …)` | `MPI_Reduce(&c, &d, …)` | `MPI_Reduce(&a, &b, …)` |
| 2 | `MPI_Reduce(&c, &d, …)` | `MPI_Reduce(&a, &b, …)` | `MPI_Reduce(&c, &d, …)` |

Kelihatannya b = 1+1+1 = 3 dan d = 2+2+2 = 6. **Salah!** Yang dipasangkan adalah **panggilan ke-1 dengan panggilan ke-1** (urutan), bukan nama variabel:
- Panggilan ke-1 → b = a(P0) + c(P1) + a(P2) = 1 + 2 + 1 = **4**
- Panggilan ke-2 → d = c(P0) + a(P1) + c(P2) = 2 + 1 + 2 = **5**

> 🎤 Seperti paduan suara: yang menentukan siapa bernyanyi bersama siapa adalah **ketukan ke berapa**, bukan judul lagu di kertas masing-masing.

#### MPI_Allreduce
```c
MPI_Allreduce(&local_x, &global_sum, 1, MPI_DOUBLE, MPI_SUM, comm);   // tidak ada dest_process
```
Bisa diimplementasikan sebagai pohon penjumlahan + pohon distribusi terbalik, atau pola **butterfly** (kupu-kupu) di mana semua proses saling bertukar berpasangan dan di akhir semua punya total.

#### MPI_Bcast
```c
int MPI_Bcast(void* data, int count, MPI_Datatype type, int source_proc, MPI_Comm comm);
```
Penyebaran berbentuk pohon: 0→4; lalu 0→2 dan 4→6; lalu 0→1, 2→3, 4→5, 6→7 (3 langkah untuk 8 proses).
`Get_input` versi lebih baik:
```c
if (my_rank == 0) { printf("Enter a, b, and n\n"); scanf("%lf %lf %d", a_p, b_p, n_p); }
MPI_Bcast(a_p, 1, MPI_DOUBLE, 0, MPI_COMM_WORLD);   // SEMUA proses memanggil ini
MPI_Bcast(b_p, 1, MPI_DOUBLE, 0, MPI_COMM_WORLD);
MPI_Bcast(n_p, 1, MPI_INT,    0, MPI_COMM_WORLD);
```

### 4.6 Membagi data: block, cyclic, block-cyclic 🃏
Vektor 12 elemen dibagi ke 3 proses:

| Proses | **Block** | **Cyclic** | **Block-cyclic** (blok = 2) |
|---|---|---|---|
| 0 | 0 1 2 3 | 0 3 6 9 | 0 1 6 7 |
| 1 | 4 5 6 7 | 1 4 7 10 | 2 3 8 9 |
| 2 | 8 9 10 11 | 2 5 8 11 | 4 5 10 11 |

> 🃏 **Analogi membagi kartu:**
> - Block: pemain 1 ambil 4 kartu teratas, pemain 2 ambil 4 berikutnya, dst.
> - Cyclic: dibagi satu-satu memutar (seperti bandar kartu biasa).
> - Block-cyclic: dibagi dua-dua memutar.
> Cyclic berguna kalau beban tiap elemen tidak sama (mis. elemen belakang lebih berat), supaya semua kebagian yang berat.

Penjumlahan vektor paralel (z = x + y) cukup loop lokal:
```c
void Parallel_vector_sum(double local_x[], double local_y[], double local_z[], int local_n) {
    for (int local_i = 0; local_i < local_n; local_i++)
        local_z[local_i] = local_x[local_i] + local_y[local_i];
}
```

#### Scatter, Gather, Allgather
```c
int MPI_Scatter(void* send_buf, int send_count, MPI_Datatype send_type,
                void* recv_buf, int recv_count, MPI_Datatype recv_type, int src_proc, MPI_Comm comm);
int MPI_Gather (void* send_buf, int send_count, MPI_Datatype send_type,
                void* recv_buf, int recv_count, MPI_Datatype recv_type, int dest_proc, MPI_Comm comm);
int MPI_Allgather(void* send_buf, int send_count, MPI_Datatype send_type,
                  void* recv_buf, int recv_count, MPI_Datatype recv_type, MPI_Comm comm);
```
- `send_count` di Scatter = jumlah elemen yang dikirim **ke tiap** proses (bukan total). Scatter mengasumsikan pembagian **block**.
- `recv_count` di Gather/Allgather = jumlah elemen yang diterima **dari tiap** proses.

Pola membaca vektor: proses 0 `malloc` seluruh vektor, `scanf`, lalu `MPI_Scatter` → `free`. Pola mencetak: `MPI_Gather` ke proses 0 → cetak → `free`.

### 4.7 Perkalian matriks–vektor y = A·x 🧮
- y[i] = baris ke-i dari A **dot** x.
- Array 2D di C disimpan **baris demi baris** (row-major) sebagai array 1D: `A[i][j]` → `A[i*n + j]`.
- Tiap proses memegang beberapa **baris** A (`local_A`) dan sebagian x (`local_x`). Tapi untuk menghitung satu baris, proses butuh **seluruh** x → pakai `MPI_Allgather`:
```c
void Mat_vect_mult(double local_A[], double local_x[], double local_y[],
                   int local_m, int n, int local_n, MPI_Comm comm) {
    double* x = malloc(n * sizeof(double));
    MPI_Allgather(local_x, local_n, MPI_DOUBLE, x, local_n, MPI_DOUBLE, comm); // semua dapat x utuh
    for (int local_i = 0; local_i < local_m; local_i++) {
        local_y[local_i] = 0.0;
        for (int j = 0; j < n; j++)
            local_y[local_i] += local_A[local_i*n + j] * x[j];
    }
    free(x);
}
```

### 4.8 Derived datatype 📦
**Masalah:** mengirim a, b, n dengan 3 kali Send/Bcast itu boros (ingat: tiap pesan kena latency!).
**Solusi:** buat **tipe data turunan** yang mendeskripsikan beberapa item beserta **posisinya relatif di memori**, sehingga semuanya bisa dikirim dalam **satu** pesan.

> 📦 **Analogi:** Daripada mengirim 3 paket terpisah lewat kurir (3× ongkir + 3× waktu tunggu), masukkan semuanya dalam **satu kardus** dengan daftar isi "barang A di posisi 0, barang B di posisi 16, barang C di posisi 24".

Secara formal: deret pasangan (tipe dasar, displacement). Contoh: a di alamat 24, b di 40, n di 48 → `{(MPI_DOUBLE, 0), (MPI_DOUBLE, 16), (MPI_INT, 24)}`.

```c
void Build_mpi_type(double* a_p, double* b_p, int* n_p, MPI_Datatype* input_mpi_t_p) {
    int          blocklengths[3] = {1, 1, 1};
    MPI_Datatype types[3]        = {MPI_DOUBLE, MPI_DOUBLE, MPI_INT};
    MPI_Aint     a_addr, b_addr, n_addr;          // MPI_Aint: integer cukup besar untuk alamat
    MPI_Aint     displacements[3] = {0};

    MPI_Get_address(a_p, &a_addr);
    MPI_Get_address(b_p, &b_addr);
    MPI_Get_address(n_p, &n_addr);
    displacements[1] = b_addr - a_addr;           // posisi b relatif terhadap a
    displacements[2] = n_addr - a_addr;           // posisi n relatif terhadap a

    MPI_Type_create_struct(3, blocklengths, displacements, types, input_mpi_t_p);
    MPI_Type_commit(input_mpi_t_p);               // izinkan MPI mengoptimasi representasinya
}

// pemakaian
Build_mpi_type(a_p, b_p, n_p, &input_mpi_t);
if (my_rank == 0) scanf(...);
MPI_Bcast(a_p, 1, input_mpi_t, 0, MPI_COMM_WORLD);   // satu kali kirim untuk a, b, n
MPI_Type_free(&input_mpi_t);                          // bebaskan setelah selesai
```

### 4.9 Mengukur kinerja program MPI ⏱️
```c
double local_start, local_finish, local_elapsed, elapsed;
MPI_Barrier(comm);                    // semua mulai bersamaan
local_start = MPI_Wtime();            // detik sejak suatu titik di masa lalu
/* kode yang diukur */
local_finish  = MPI_Wtime();
local_elapsed = local_finish - local_start;
MPI_Reduce(&local_elapsed, &elapsed, 1, MPI_DOUBLE, MPI_MAX, 0, comm);   // ambil yang terlama
if (my_rank == 0) printf("Elapsed time = %e seconds\n", elapsed);
```
- `MPI_Barrier(comm)`: tidak ada proses yang keluar dari Barrier sampai **semua** proses di communicator memanggilnya.
- Untuk program serial: `GET_TIME(now)` dari `timer.h` (mikrodetik, tidak perlu MPI).

**Hasil perkalian matriks–vektor (detik):**

| p \ ukuran | 1024 | 2048 | 4096 | 8192 | 16384 |
|---|---|---|---|---|---|
| 1 | 4,1 | 16,0 | 64,0 | 270 | 1100 |
| 2 | 2,3 | 8,5 | 33,0 | 140 | 560 |
| 4 | 2,0 | 5,1 | 18,0 | 70 | 280 |
| 8 | 1,7 | 3,3 | 9,8 | 36 | 140 |
| 16 | 1,7 | 2,6 | 5,9 | 19 | 71 |

Speedup p=16: 2,4 (ukuran 1024) sampai 15,5 (ukuran 16384). Efisiensi p=16: 0,15 sampai 0,97.
👉 Matriks kecil + banyak proses = buruk (komunikasi mendominasi). Matriks besar = hampir ideal.

### 4.10 Sorting paralel: Odd-Even Transposition Sort 🔢
**Target:** n kunci, p proses, n/p kunci per proses. Di akhir: kunci tiap proses terurut, dan semua kunci di proses q ≤ semua kunci di proses r jika q < r.

**Bubble sort sulit diparalelkan.** Gantinya **odd-even transposition sort**:
- **Fase genap:** bandingkan-tukar pasangan (a[0],a[1]), (a[2],a[3]), …
- **Fase ganjil:** bandingkan-tukar pasangan (a[1],a[2]), (a[3],a[4]), …
- Teorema: **n fase** cukup untuk mengurutkan n elemen.

Contoh: `5, 9, 4, 3`
```
Genap : (5,9) (4,3)  → 5 9 3 4
Ganjil: (9,3)        → 5 3 9 4
Genap : (5,3) (9,4)  → 3 5 4 9
Ganjil: (5,4)        → 3 4 5 9 ✅
```
Kenapa bagus untuk paralel? Dalam satu fase, semua pasangan **independen** → bisa dikerjakan bersamaan.

> 👫 **Analogi:** Barisan siswa ingin urut tinggi badan. Bergantian, siswa berpasangan dengan tetangga kanan (pasangan genap) lalu tetangga kiri (pasangan ganjil), dan bertukar posisi jika urutannya salah.

**Versi paralel (tiap proses memegang banyak kunci):**
1. Tiap proses **mengurutkan kuncinya sendiri** (sort lokal).
2. Untuk fase 0 sampai p−1: tentukan **partner**, **tukar seluruh kunci** dengan partner, lalu:
   - Proses dengan rank lebih kecil **menyimpan setengah terkecil** (`Merge_low`).
   - Proses dengan rank lebih besar **menyimpan setengah terbesar** (`Merge_high`).

Contoh slide (4 proses, 16 kunci):
```
Awal         : P0 {15,11,9,16}  P1 {3,14,8,7}   P2 {4,6,12,10}  P3 {5,2,13,1}
Sort lokal   : P0 {9,11,15,16}  P1 {3,7,8,14}   P2 {4,6,10,12}  P3 {1,2,5,13}
Fase 0 (0↔1, 2↔3): {3,7,8,9}   {11,14,15,16}   {1,2,4,5}       {6,10,12,13}
Fase 1 (1↔2)     : {3,7,8,9}   {1,2,4,5}       {11,14,15,16}   {6,10,12,13}
Fase 2 (0↔1, 2↔3): {1,2,3,4}   {5,7,8,9}       {6,10,11,12}    {13,14,15,16}
Fase 3 (1↔2)     : {1,2,3,4}   {5,6,7,8}       {9,10,11,12}    {13,14,15,16} ✅
```

**Menentukan partner:**
```c
if (phase % 2 == 0)                     // fase genap
    partner = (my_rank % 2 != 0) ? my_rank - 1 : my_rank + 1;
else                                    // fase ganjil
    partner = (my_rank % 2 != 0) ? my_rank + 1 : my_rank - 1;
if (partner == -1 || partner == comm_sz)
    partner = MPI_PROC_NULL;            // tidak punya partner → menganggur di fase ini
```

Waktu (ms), kunci 200 ribu → 3,2 juta: p=1: 88 → 1800; p=16: 7,5 → 130.

### 4.11 Keamanan program MPI (safety) ☠️
Standar MPI mengizinkan `MPI_Send` berperilaku dua cara:
1. Menyalin pesan ke **buffer internal** MPI lalu langsung kembali, **atau**
2. **Menunggu (block)** sampai `MPI_Recv` pasangannya mulai.

Banyak implementasi: pesan **kecil** → di-buffer; pesan **besar** → block.

**Bahaya:** jika semua proses melakukan `Send` dulu lalu `Recv`, dan Send-nya block (karena pesannya besar), maka **tidak ada yang sempat Recv** → **deadlock**.
```c
// Pola ring yang UNSAFE
MPI_Send(msg,     size, MPI_INT, (my_rank+1) % comm_sz,          0, comm);
MPI_Recv(new_msg, size, MPI_INT, (my_rank+comm_sz-1) % comm_sz, 0, comm, MPI_STATUS_IGNORE);
```
> 📞 **Analogi:** Dua orang menelepon satu sama lain bersamaan — keduanya mendengar nada sibuk dan terus menunggu. Atau dua orang di pintu sempit saling berkata "silakan duluan" dan tidak ada yang bergerak.

- Program yang **bergantung pada buffering** MPI disebut **unsafe**: bisa jalan untuk input kecil, tapi hang untuk input besar.
- **`MPI_Ssend`** (synchronous send): parameter sama dengan `MPI_Send`, tetapi **dijamin block** sampai Recv pasangannya mulai. Gunakan untuk **menguji** apakah program aman (ganti semua Send dengan Ssend; kalau hang, berarti unsafe).

**Cara memperbaiki:**
1. **Atur ulang urutan:** rank genap Send dulu lalu Recv; rank ganjil Recv dulu lalu Send.
```c
if (my_rank % 2 == 0) { MPI_Send(...); MPI_Recv(...); }
else                  { MPI_Recv(...); MPI_Send(...); }
```
2. **`MPI_Sendrecv`:** kirim & terima dalam satu panggilan; MPI yang mengatur jadwalnya sehingga tidak deadlock. Tujuan dan sumber boleh sama atau berbeda.
```c
int MPI_Sendrecv(void* send_buf, int send_size, MPI_Datatype send_type, int dest,   int send_tag,
                 void* recv_buf, int recv_size, MPI_Datatype recv_type, int source, int recv_tag,
                 MPI_Comm comm, MPI_Status* status);
```
> 🤝 Seperti tukar kado: "kamu kasih, aku kasih" dalam satu gerakan, bukan saling menunggu siapa yang memberi duluan.

### ✅ Inti yang harus diingat (Bab 4)
- Kerangka: `MPI_Init` → `MPI_Comm_size` / `MPI_Comm_rank` → kerja → `MPI_Finalize`. Compile `mpicc`, run `mpiexec -n p`.
- Pesan cocok jika comm, tag, sumber/tujuan cocok. `MPI_Recv` selalu block; `MPI_Send` bisa buffer atau block.
- Kolektif: semua proses wajib memanggil, tanpa tag, dicocokkan berdasarkan **urutan**.
- Reduce/Allreduce/Bcast/Scatter/Gather/Allgather/Barrier — hafalkan analoginya.
- Partisi data: block, cyclic, block-cyclic.
- Derived datatype = satu kardus untuk banyak barang.
- Ukur waktu: Barrier → MPI_Wtime → Reduce MAX.
- Odd-even sort: p fase, tukar dengan partner, simpan setengah kecil/besar.
- Unsafe = bergantung pada buffering. Uji dengan `MPI_Ssend`, perbaiki dengan urutan genap/ganjil atau `MPI_Sendrecv`.

<details><summary>🧠 Uji diri Bab 4</summary>

1. **Fungsi apa agar semua proses mendapat hasil penjumlahan global?** → `MPI_Allreduce`.
2. **Di `MPI_Scatter`, `send_count` berarti apa?** → Jumlah elemen yang dikirim ke **tiap** proses.
3. **Vektor 9 elemen, 3 proses, pembagian cyclic: proses 1 dapat apa?** → Elemen 1, 4, 7.
4. **Mengapa program yang bergantung pada buffering MPI_Send disebut unsafe?** → Untuk pesan besar Send bisa block; jika semua Send dulu, tidak ada yang Recv → deadlock.
5. **Fase genap odd-even sort dengan 4 proses: siapa berpasangan dengan siapa?** → 0↔1, 2↔3.
6. **Fase ganjil dengan 4 proses?** → 1↔2; proses 0 dan 3 menganggur (`MPI_PROC_NULL`).
</details>

---

## Bab 5 – Pthreads: Pemrograman Shared Memory (slide 04)

### 5.0 Analogi besar: 🍳 Para koki di satu dapur
- **Proses** = satu restoran (punya dapur & bahan sendiri).
- **Thread** = koki-koki di dalam restoran yang sama; mereka berbagi **meja bahan** (memori bersama). Thread disebut **"proses ringan"** (*light-weight process*) karena lebih murah dibuat daripada proses.
- Masalah utamanya bukan mengirim pesan, melainkan **rebutan** meja bahan yang sama.

**Pthreads (POSIX Threads):** standar API thread untuk sistem Unix-like (Linux, macOS, Solaris, HP-UX); berupa **library** yang di-link ke program C.

### 5.1 Hello world Pthreads
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>          // deklarasi fungsi, konstanta, tipe Pthreads

int thread_count;             // variabel GLOBAL → dibagi semua thread

void* Hello(void* rank);      // fungsi yang akan dijalankan tiap thread

int main(int argc, char* argv[]) {
    long thread;              // long (bukan int) supaya aman di-cast ke void* di sistem 64-bit
    pthread_t* thread_handles;

    thread_count = strtol(argv[1], NULL, 10);              // jumlah thread dari command line
    thread_handles = malloc(thread_count * sizeof(pthread_t));

    for (thread = 0; thread < thread_count; thread++)      // FORK: buat thread
        pthread_create(&thread_handles[thread], NULL, Hello, (void*) thread);

    printf("Hello from the main thread\n");

    for (thread = 0; thread < thread_count; thread++)      // JOIN: tunggu thread selesai
        pthread_join(thread_handles[thread], NULL);

    free(thread_handles);
    return 0;
}

void* Hello(void* rank) {
    long my_rank = (long) rank;                            // variabel lokal → PRIVATE
    printf("Hello from thread %ld of %d\n", my_rank, thread_count);
    return NULL;
}
```
```bash
gcc -g -Wall -o pth_hello pth_hello.c -lpthread    # -lpthread = link library Pthreads
./pth_hello 4
```

**Fungsi kunci:**
```c
int pthread_create(pthread_t* thread_p,               // OUT: "kartu identitas" thread (alokasikan dulu!)
                   const pthread_attr_t* attr_p,      // atribut → kita isi NULL
                   void* (*start_routine)(void*),     // fungsi yang dijalankan thread
                   void* arg_p);                      // argumen untuk fungsi tsb
int pthread_join(pthread_t thread, void** ret_val_p); // tunggu satu thread selesai
```
- `pthread_t` bersifat **opaque** (isinya tergantung sistem, tidak bisa diakses langsung), tetapi dijamin cukup untuk mengidentifikasi thread secara unik.
- Prototipe fungsi thread selalu `void* f(void* args)`. `void*` bisa di-cast ke pointer apa pun → bisa membawa banyak argumen lewat struct.
- **Variabel global = shared**, **variabel lokal = private**. Hati-hati: global variable bisa menimbulkan bug halus → pakai hanya bila memang perlu dibagi.
- Di MPI, proses dijalankan oleh **script** (`mpiexec`); di Pthreads, thread dibuat oleh **program itu sendiri**.

> 👨‍🍳 **Fork & join:** Kepala koki (main thread) memanggil koki-koki pembantu (create), lalu menunggu semua selesai sebelum menutup dapur (join).

### 5.2 Contoh: perkalian matriks–vektor dengan Pthreads
A (m×n), x, y semuanya **global/shared**. Tiap thread mengerjakan sekumpulan **baris** y. Contoh m = 6, 3 thread: thread 0 → y[0], y[1]; thread 1 → y[2], y[3]; thread 2 → y[4], y[5].
```c
void* Pth_mat_vect(void* rank) {
    long my_rank = (long) rank;
    int local_m      = m / thread_count;
    int my_first_row = my_rank * local_m;
    int my_last_row  = (my_rank + 1) * local_m - 1;

    for (int i = my_first_row; i <= my_last_row; i++) {
        y[i] = 0.0;
        for (int j = 0; j < n; j++)
            y[i] += A[i][j] * x[j];
    }
    return NULL;
}
```
Aman karena tiap thread menulis ke elemen y yang **berbeda** (tidak ada rebutan).

### 5.3 Critical section: studi kasus menghitung π 🥧
```
π = 4 × (1 − 1/3 + 1/5 − 1/7 + … )
```
```c
void* Thread_sum(void* rank) {
    long my_rank = (long) rank;
    double factor;
    long long my_n = n / thread_count;
    long long my_first_i = my_n * my_rank;
    long long my_last_i  = my_first_i + my_n;

    factor = (my_first_i % 2 == 0) ? 1.0 : -1.0;   // tanda suku pertama milik thread ini
    for (long long i = my_first_i; i < my_last_i; i++, factor = -factor)
        sum += factor / (2*i + 1);                  // ⚠️ sum GLOBAL → RACE CONDITION
    return NULL;
}
```
Hasil di dual core: 1 thread makin akurat (3,14159264 untuk n = 10⁸), 2 thread **salah** (3,14164686) → race condition pada `sum`.

### 5.4 Solusi 1: Busy-waiting 🚪
```c
y = Compute(my_rank);
while (flag != my_rank);      // tunggu giliran (flag diinisialisasi 0 oleh main)
x = x + y;                    // critical section
flag++;                       // giliran thread berikutnya
```
- Thread terus-menerus mengecek kondisi tanpa melakukan kerja berguna.
- ⚠️ **Compiler yang mengoptimasi** bisa menukar urutan atau "menghapus" loop ini karena dianggap tidak berguna → hasil bisa salah.
- Kalau busy-wait diletakkan **di dalam** loop for (tiap suku), program jadi sangat lambat (efektif serial + overhead).
- **Lebih baik:** hitung `my_sum` lokal dulu, lalu critical section **sekali** di akhir:
```c
for (i = my_first_i; i < my_last_i; i++, factor = -factor)
    my_sum += factor / (2*i + 1);          // private, tidak rebutan
while (flag != my_rank);
sum += my_sum;                             // hanya sekali per thread
flag = (flag + 1) % thread_count;
```
> 💡 **Prinsip emas:** perkecil frekuensi masuk critical section. Hitung sendiri dulu, setor sekali.
> 🗳️ Seperti pemilu: tiap TPS menghitung suaranya sendiri, lalu hanya mengirim **total TPS** ke KPU — bukan mengirim tiap surat suara satu per satu.

### 5.5 Solusi 2: Mutex 🔒
Busy-wait membuang CPU. **Mutex** = variabel khusus yang membatasi critical section hanya untuk **satu thread pada satu waktu**.
```c
pthread_mutex_t mutex;
pthread_mutex_init(&mutex, NULL);      // inisialisasi (di main)

// di dalam thread
for (...) my_sum += factor / (2*i + 1);
pthread_mutex_lock(&mutex);            // 🔒 tunggu sampai kunci tersedia
sum += my_sum;                         // critical section
pthread_mutex_unlock(&mutex);          // 🔓 lepas kunci

pthread_mutex_destroy(&mutex);         // setelah selesai dipakai (di main)
```
> 🚻 Thread yang menunggu kunci mutex "tidur" (tidak memakai CPU), berbeda dengan busy-wait yang terus mengetuk pintu.

**Waktu (detik), n = 10⁸, mesin 8 core:**

| Thread | Busy-wait | Mutex |
|---|---|---|
| 1 | 2,90 | 2,90 |
| 2 | 1,45 | 1,45 |
| 4 | 0,73 | 0,73 |
| 8 | 0,38 | 0,38 |
| 16 | 0,50 | 0,38 |
| 32 | 0,80 | 0,40 |
| 64 | **3,56** | 0,38 |

👉 Jika **thread > core**, busy-wait memburuk drastis. Kenapa? Thread yang sedang mendapat giliran (flag) mungkin sedang **di-suspend** OS (tidak kebagian core), sementara thread lain memakai core hanya untuk busy-wait sia-sia.

**Perbedaan penting:** busy-wait **menentukan urutan** (thread 0, lalu 1, lalu 2…). Mutex **tidak** — urutan diserahkan ke sistem/kebetulan.

### 5.6 Semaphore & producer–consumer 🎟️
Kadang **urutan** penting. Contoh: perkalian matriks A = A1 × A2 × A3 × … (tidak komutatif!). Dengan mutex, urutan perkalian acak → salah.

Contoh lain: thread saling kirim pesan lewat array `messages[]` bersama:
```c
dest = (my_rank + 1) % thread_count;
sprintf(my_msg, "Hello to %ld from %ld", dest, my_rank);
messages[dest] = my_msg;
if (messages[my_rank] != NULL) printf("Thread %ld > %s\n", my_rank, messages[my_rank]);
else                           printf("Thread %ld > No message from %ld\n", my_rank, source);
```
Masalah: penerima mungkin mengecek **sebelum** pengirim menaruh pesan.

**Semaphore** = bilangan bulat tak-negatif dengan dua operasi:
- `sem_wait(&s)`: jika s = 0 → tunggu (block); jika s > 0 → kurangi 1 dan lanjut.
- `sem_post(&s)`: tambah 1 (membangunkan yang menunggu).
```c
#include <semaphore.h>        // ⚠️ semaphore BUKAN bagian Pthreads, include terpisah
int sem_init(sem_t* s, int shared, unsigned initial_val);
int sem_destroy(sem_t* s);
int sem_post(sem_t* s);
int sem_wait(sem_t* s);
```
Perbaikan program pesan:
```c
messages[dest] = my_msg;
sem_post(&semaphores[dest]);       // "pesan untukmu sudah ada!"
sem_wait(&semaphores[my_rank]);    // tunggu sampai pesan untukku ada
printf("Thread %ld > %s\n", my_rank, messages[my_rank]);
```
> 🎟️ **Analogi:** Semaphore = **jumlah tiket** di loket. `sem_wait` = mengambil tiket (kalau habis, antre). `sem_post` = mengembalikan/menambah tiket. Mutex itu seperti semaphore dengan **satu** tiket saja. Semaphore lebih fleksibel karena boleh diawali angka berapa pun (≥ 0).

### 5.7 Barrier 🚌
**Barrier** = titik di program di mana **tidak ada thread yang boleh lewat sebelum semua thread tiba**.
> 🚌 Rombongan study tour: bus baru berangkat setelah **semua** peserta naik.

Kegunaan: mengukur waktu thread paling lambat, debugging (`if (my_rank == 0) printf("All threads reached this point\n");`).

**Tiga cara membuat barrier:**

**(a) Busy-wait + mutex**
```c
int counter = 0;                       // shared
pthread_mutex_lock(&barrier_mutex);
counter++;
pthread_mutex_unlock(&barrier_mutex);
while (counter < thread_count);        // tunggu semua
```
Kelemahan: boros CPU, dan butuh **counter berbeda untuk tiap barrier** (counter tidak bisa direset dengan aman).

**(b) Semaphore**
```c
int counter = 0;            // shared
sem_t count_sem;            // diinisialisasi 1 (melindungi counter)
sem_t barrier_sem;          // diinisialisasi 0 (tempat menunggu)

sem_wait(&count_sem);
if (counter == thread_count - 1) {           // saya thread terakhir yang tiba
    counter = 0;
    sem_post(&count_sem);
    for (j = 0; j < thread_count - 1; j++)
        sem_post(&barrier_sem);              // bangunkan semua yang menunggu
} else {
    counter++;
    sem_post(&count_sem);
    sem_wait(&barrier_sem);                  // tunggu dibangunkan
}
```

**(c) Condition variable** (cara paling "resmi")
**Condition variable** = objek yang membuat thread **tidur sampai suatu kondisi terjadi**; thread lain bisa **membangunkannya**. Selalu dipasangkan dengan **mutex**.
```c
pthread_cond_wait(&cond_var, &mutex);   // lepas mutex + tidur; saat bangun, mutex dikunci lagi
pthread_cond_signal(&cond_var);         // bangunkan SATU thread
pthread_cond_broadcast(&cond_var);      // bangunkan SEMUA thread
```
Barrier dengan condition variable:
```c
pthread_mutex_lock(&mutex);
counter++;
if (counter == thread_count) {          // thread terakhir
    counter = 0;
    pthread_cond_broadcast(&cond_var);  // "semua sudah lengkap, ayo jalan!"
} else {
    while (pthread_cond_wait(&cond_var, &mutex) != 0);   // tidur sampai dibangunkan
}
pthread_mutex_unlock(&mutex);
```
> ⏰ **Analogi:** Kamu tidur di bus dan berpesan ke pemandu, "Bangunkan saya kalau semua sudah naik." Tidak perlu terus melihat ke pintu.

### 5.8 Read-write lock 📚
**Kasus:** linked list terurut berisi int, dengan operasi `Member` (cek ada/tidak — hanya **membaca**), `Insert` dan `Delete` (**menulis**). `head_p` global sehingga semua thread bisa mengaksesnya.

Bahaya: Thread 0 sedang membaca node 5 (`Member`), Thread 1 menghapus node 5 (`Delete`) → Thread 0 mengakses memori yang sudah dibebaskan.

| Solusi | Cara | Masalah |
|---|---|---|
| **#1 Satu mutex untuk seluruh list** | Kunci list setiap kali ada operasi | Semua operasi jadi **serial**. Buruk jika mayoritas operasi `Member` (yang sebenarnya aman dijalankan bersamaan). Tetapi bagus jika mayoritas Insert/Delete, dan mudah. |
| **#2 Satu mutex per node** (*fine-grained*) | Kunci node yang sedang diakses saja | Kode jauh lebih rumit, **lebih lambat** (lock/unlock tiap node), boros memori (tiap node bertambah satu mutex) |
| **#3 Read-write lock** ✅ | Dua jenis kunci: kunci baca & kunci tulis | — |

**Aturan read-write lock:**
- **Banyak** thread boleh memegang **kunci baca** bersamaan.
- Hanya **satu** thread boleh memegang **kunci tulis**.
- Jika ada yang memegang kunci baca → yang mau menulis **menunggu**.
- Jika ada yang memegang kunci tulis → yang mau membaca/menulis **menunggu**.

```c
pthread_rwlock_rdlock(&rwlock);   Member(value);   pthread_rwlock_unlock(&rwlock);
pthread_rwlock_wrlock(&rwlock);   Insert(value);   pthread_rwlock_unlock(&rwlock);
pthread_rwlock_wrlock(&rwlock);   Delete(value);   pthread_rwlock_unlock(&rwlock);
```
> 📚 **Analogi perpustakaan:** Banyak orang boleh **membaca** buku referensi yang sama bersamaan. Tapi kalau pustakawan mau **merevisi** buku itu, ia harus menunggu semua pembaca selesai, dan selama revisi tidak ada yang boleh membaca.

**Hasil (detik, 100.000 operasi per thread):**

| Kasus | Metode | 1 thr | 2 thr | 4 thr | 8 thr |
|---|---|---|---|---|---|
| 99,9% Member | RW lock | 0,213 | 0,123 | 0,098 | 0,115 |
| | Satu mutex | 0,211 | 0,450 | 0,385 | 0,457 |
| | Mutex per node | 1,680 | 5,700 | 3,450 | 2,700 |
| 80% Member | RW lock | 2,48 | 4,97 | 4,69 | 4,71 |
| | Satu mutex | 2,50 | 5,13 | 5,04 | 5,11 |
| | Mutex per node | 12,00 | 29,60 | 17,00 | 12,00 |

👉 RW lock unggul saat **mayoritas operasi membaca**. Mutex per node paling lambat. Jika banyak penulisan, semua metode sama-sama tidak bisa banyak paralel.

### 5.9 Cache, write-miss, dan false sharing 🧾
- **Write-miss:** core ingin menulis variabel yang tidak ada di cache → harus ambil dari memori utama (lambat).
- **False sharing:** dua thread menulis ke variabel **berbeda** yang kebetulan berada dalam **satu cache line** yang sama. Walau tidak berbagi data secara logis, hardware menganggap cache line itu "diubah" → saling meng-invalidate cache satu sama lain terus-menerus.

> 🧾 **Analogi:** Dua orang menulis di **baris berbeda** pada **halaman yang sama** dari satu buku. Walau tidak saling mengganggu isinya, buku itu harus terus dioper bolak-balik karena hanya satu orang yang bisa memegangnya.

Hasil perkalian matriks–vektor Pthreads (detik, efisiensi):

| Ukuran | 1 thread | 2 thread | 4 thread |
|---|---|---|---|
| 8.000.000 × 8 | 0,393 (1,000) | 0,217 (0,906) | 0,139 (0,707) |
| 8000 × 8000 | 0,345 (1,000) | 0,188 (0,918) | 0,115 (0,750) |
| 8 × 8.000.000 | 0,441 (1,000) | 0,300 (0,735) | 0,388 (**0,290**) |

- 8.000.000 × 8: vektor y sangat besar → banyak **write-miss**.
- 8 × 8.000.000: y hanya 8 elemen (muat dalam **satu cache line**) dan tiap thread menulis elemen y-nya berulang kali → **false sharing** parah → efisiensi 4 thread jatuh ke 0,29.

### 5.10 Thread-safety 🔖
Kode **thread-safe** = bisa dijalankan bersamaan oleh banyak thread tanpa masalah.

**Kasus tokenizer:** file teks dibagi per baris secara round-robin ke thread (baris 1 → thread 0, baris 2 → thread 1, …), lalu tiap baris dipecah per kata dengan `strtok`.

`strtok` bekerja begini: panggilan pertama diberi string, panggilan berikutnya diberi `NULL`; ia **mengingat** posisi string di variabel **static** (internal, bersama).

Input:
```
Pease porridge hot.
Pease porridge cold.
Pease porridge in the pot
Nine days old.
```
- 1 thread: benar.
- 2 thread: **"Oops!"** — thread 1 hanya mendapat kata "Pease" dari barisnya, karena thread 0 memanggil `strtok` dengan baris ke-3 dan **menimpa** posisi yang diingat untuk baris ke-2 milik thread 1.

> 🔖 **Analogi:** Dua orang membaca buku yang berbeda, tetapi berbagi **satu pembatas buku**. Saat orang kedua memindahkan pembatas ke bukunya, orang pertama kehilangan posisinya.

- Fungsi C lain yang tidak thread-safe: `random` (stdlib.h), `localtime` (time.h).
- Solusi: versi **re-entrant** (thread-safe), misalnya:
```c
char* strtok_r(char* string, const char* separators, char** saveptr_p);   // posisi disimpan di saveptr milik pemanggil
```

### ✅ Inti yang harus diingat (Bab 5)
- `pthread_create` (fork) & `pthread_join` (join); compile dengan `-lpthread`.
- Global = shared, lokal = private.
- Race condition → critical section → lindungi.
- Busy-wait: boros CPU, rusak oleh optimasi compiler, buruk jika thread > core, tetapi menjamin urutan.
- Mutex: tidak boros CPU, urutan acak.
- Semaphore: angka ≥ 0, `sem_wait`/`sem_post`, bisa mengatur urutan (`#include <semaphore.h>`).
- Barrier: busy-wait+mutex, semaphore, condition variable (`wait`/`signal`/`broadcast`).
- Read-write lock: banyak pembaca, satu penulis.
- False sharing: variabel berbeda di cache line yang sama.
- Fungsi dengan variabel static (strtok, random, localtime) tidak thread-safe → pakai versi `_r`.

<details><summary>🧠 Uji diri Bab 5</summary>

1. **Mengapa `long` (bukan `int`) dipakai untuk rank yang di-cast ke `void*`?** → Ukuran `long` sama dengan pointer di sistem 64-bit, sehingga cast aman.
2. **Mengapa busy-wait sangat lambat saat 64 thread di 8 core?** → Thread yang mendapat giliran bisa sedang di-suspend; core dipakai thread lain hanya untuk busy-wait.
3. **Apa beda `pthread_cond_signal` dan `pthread_cond_broadcast`?** → Signal membangunkan satu thread; broadcast membangunkan semua.
4. **Kapan read-write lock paling menguntungkan?** → Saat mayoritas operasi hanya membaca.
5. **Apa itu false sharing?** → Thread menulis variabel berbeda dalam satu cache line yang sama sehingga cache saling di-invalidate.
6. **Kenapa strtok tidak thread-safe?** → Menyimpan posisi di variabel static yang dibagi semua thread.
</details>

---
## Bab 6 – OpenMP: Shared Memory yang Lebih Praktis (slide 05)

### 6.0 Analogi besar: 🏷️ Memberi label "kerjakan beramai-ramai"
Dengan Pthreads, kamu sendiri yang membuat thread, membagi indeks, dan menggabungkan hasil. Dengan **OpenMP**, kamu cukup menempelkan **label (pragma)** di atas bagian kode, dan compiler + runtime yang mengurus pembagian kerjanya.

> 👷 Pthreads = kamu jadi mandor yang membagi tugas satu per satu ke tukang. OpenMP = kamu cukup menempel kertas "**dikerjakan beramai-ramai**" di pekerjaan itu, dan kepala tukang yang membagi.

- OpenMP = API untuk **shared-memory**; MP = *multiprocessing*.
- Sistem dipandang sebagai kumpulan core yang semuanya bisa mengakses memori utama.
- Memakai **pragma** (`#pragma omp ...`): instruksi khusus preprocessor. Compiler yang tidak mendukung **mengabaikannya** → program tetap jalan secara serial.

### 6.1 Hello world OpenMP
```c
#include <stdio.h>
#include <stdlib.h>
#include <omp.h>

void Hello(void);

int main(int argc, char* argv[]) {
    int thread_count = strtol(argv[1], NULL, 10);

#   pragma omp parallel num_threads(thread_count)   // blok berikut dijalankan oleh thread_count thread
    Hello();

    return 0;
}

void Hello(void) {
    int my_rank      = omp_get_thread_num();    // id thread ini (0, 1, ...)
    int thread_count = omp_get_num_threads();   // jumlah thread di tim
    printf("Hello from thread %d of %d\n", my_rank, thread_count);
}
```
```bash
gcc -g -Wall -fopenmp -o omp_hello omp_hello.c
./omp_hello 4        # urutan output acak
```

**Istilah:**
- `# pragma omp parallel` → directive paling dasar: jalankan blok berikutnya dengan banyak thread (jumlahnya ditentukan sistem jika tidak disebut).
- **Clause** = teks yang memodifikasi directive, mis. `num_threads(thread_count)`.
- OpenMP **tidak menjamin** benar-benar membuat sebanyak `thread_count` (ada batas sistem), tetapi biasanya dapat.
- **Team** = kumpulan thread yang menjalankan parallel block. Thread asal = **master**, thread tambahan = **slave**. Polanya **fork–join**: master membelah diri, di akhir blok semua bergabung kembali (ada **barrier implisit**).

**Supaya tetap bisa dikompilasi tanpa OpenMP:**
```c
#ifdef _OPENMP
#  include <omp.h>
#endif
...
#ifdef _OPENMP
    int my_rank = omp_get_thread_num();
    int thread_count = omp_get_num_threads();
#else
    int my_rank = 0;
    int thread_count = 1;
#endif
```

### 6.2 Trapesium versi OpenMP + `critical`
Desain sama seperti MPI: (a) hitung luas trapesium, (b) jumlahkan; tiap thread mendapat blok trapesium berurutan.
```c
int main(int argc, char* argv[]) {
    double global_result = 0.0;                 // shared
    ...
#   pragma omp parallel num_threads(thread_count)
    Trap(a, b, n, &global_result);
    printf("... = %.14e\n", global_result);
}

void Trap(double a, double b, int n, double* global_result_p) {
    int my_rank      = omp_get_thread_num();
    int thread_count = omp_get_num_threads();
    double h         = (b - a) / n;
    int local_n      = n / thread_count;
    double local_a   = a + my_rank * local_n * h;
    double local_b   = local_a + local_n * h;
    double my_result = (f(local_a) + f(local_b)) / 2.0;
    for (int i = 1; i <= local_n - 1; i++) my_result += f(local_a + i*h);
    my_result = my_result * h;

#   pragma omp critical                         // hanya satu thread sekaligus
    *global_result_p += my_result;
}
```
`# pragma omp critical` = mutex versi OpenMP.

### 6.3 Scope variabel: shared vs private 🔑
- Di OpenMP, **scope** = himpunan thread yang bisa mengakses variabel di dalam parallel block.
- **Shared:** bisa diakses semua thread di tim.
- **Private:** hanya bisa diakses satu thread (tiap thread punya salinan sendiri).
- **Default:** variabel yang dideklarasikan **sebelum** parallel block = **shared**; yang dideklarasikan **di dalam** block = **private**.

> 🔑 **Analogi kos-kosan:** Dapur dan ruang tamu = shared (semua penghuni pakai). Kamar tidur = private (tiap penghuni punya sendiri).

### 6.4 Reduction clause ➕
Kita ingin menulis:
```c
global_result = 0.0;
# pragma omp parallel num_threads(thread_count)
global_result += Local_trap(a, b, n);   // ❌ race condition!
```
Kalau diberi `critical` di dalam:
```c
# pragma omp parallel num_threads(thread_count)
{
#   pragma omp critical
    global_result += Local_trap(a, b, n);   // ❌ benar, tapi Local_trap ikut dikunci → semua jadi serial!
}
```
Perbaikan manual: hitung ke variabel private dulu, critical hanya untuk penjumlahan:
```c
# pragma omp parallel num_threads(thread_count)
{
    double my_result = 0.0;                 // private
    my_result += Local_trap(a, b, n);
#   pragma omp critical
    global_result += my_result;
}
```
Cara **paling rapi**: `reduction`.
```c
global_result = 0.0;
# pragma omp parallel num_threads(thread_count) reduction(+: global_result)
global_result += Local_trap(a, b, n);
```
- **Reduction operator:** operasi biner (+, *, -, &, |, ^, &&, ||).
- **Reduction:** menerapkan operator yang sama berulang kali pada deret nilai untuk menghasilkan satu hasil. Hasil antara disimpan di **reduction variable**.
- Di balik layar: tiap thread mendapat **salinan private** (diinisialisasi nilai identitas: 0 untuk +, 1 untuk *), lalu di akhir semua salinan digabung ke variabel shared.

> 🗳️ **Analogi rekap suara:** tiap TPS menghitung suaranya sendiri (salinan private), lalu KPU menjumlahkan semua hasil TPS (penggabungan akhir). Tidak ada antrean panjang di satu kotak suara.

### 6.5 `parallel for` 🔁
Directive yang **membagi iterasi loop** ke thread-thread. Blok yang mengikutinya **harus** berupa loop `for`.
```c
h = (b - a) / n;
approx = (f(a) + f(b)) / 2.0;
# pragma omp parallel for num_threads(thread_count) reduction(+: approx)
for (i = 1; i <= n-1; i++)
    approx += f(a + i*h);
approx = h * approx;
```
Variabel indeks loop (`i`) otomatis **private**.

**Bentuk loop yang boleh diparalelkan:**
```
for (index = start; index <, <=, >=, > end; index++ / ++index / index-- / --index /
                                           index += incr / index -= incr /
                                           index = index + incr / index = incr + index /
                                           index = index - incr)
```
**Syarat (caveats):**
- `index` bertipe integer atau pointer (bukan float).
- `start`, `end`, `incr` bertipe kompatibel dan **tidak berubah** selama loop.
- `index` hanya boleh diubah oleh bagian *increment* di header `for`.
- Intinya: jumlah iterasi harus bisa dihitung **sebelum** loop dimulai. (Loop `while`, atau `for` yang punya `break`, tidak bisa.)

### 6.6 Data dependency ⛓️
```c
fibo[0] = fibo[1] = 1;
# pragma omp parallel for num_threads(2)
for (i = 2; i < n; i++)
    fibo[i] = fibo[i-1] + fibo[i-2];
```
Benar: `1 1 2 3 5 8 13 21 34 55`. Kadang hasil: `1 1 2 3 5 8 0 0 0 0` ❌ — thread 2 mulai menghitung fibo[6..9] **sebelum** thread 1 selesai menghitung fibo[5].

1. Compiler OpenMP **tidak memeriksa** dependensi antar iterasi.
2. Loop yang hasil iterasinya **bergantung pada iterasi lain** (*loop-carried dependence*) umumnya tidak bisa diparalelkan dengan benar oleh OpenMP.

> 🧱 **Analogi:** Menyusun menara balok: balok ke-10 tidak bisa dipasang sebelum balok ke-9 ada. Menambah tukang tidak membantu.

**Contoh π (versi OpenMP):**
```c
// ❌ Solusi #1: factor = -factor membuat iterasi bergantung pada iterasi sebelumnya
# pragma omp parallel for num_threads(thread_count) reduction(+: sum)
for (k = 0; k < n; k++) {
    sum += factor / (2*k + 1);
    factor = -factor;
}

// ✅ Solusi #2: hitung factor langsung dari k, dan jadikan factor private
# pragma omp parallel for num_threads(thread_count) reduction(+: sum) private(factor)
for (k = 0; k < n; k++) {
    factor = (k % 2 == 0) ? 1.0 : -1.0;
    sum += factor / (2*k + 1);
}
```
> 💡 **Trik penting:** hilangkan dependensi dengan menghitung nilai **langsung dari indeks**, bukan dari iterasi sebelumnya.

### 6.7 Clause `default(none)` 🛡️
Memaksa programmer **menyebut scope setiap variabel** yang dipakai di blok dan dideklarasikan di luar blok. Bagus untuk mencegah bug salah-scope.
```c
# pragma omp parallel for num_threads(thread_count) \
      default(none) reduction(+: sum) private(k, factor) shared(n)
```

### 6.8 Sorting dengan OpenMP: odd-even transposition sort
```c
// Versi 1: parallel for di tiap fase → thread dibuat & dibubarkan tiap fase (overhead fork/join)
for (phase = 0; phase < n; phase++) {
    if (phase % 2 == 0)
#       pragma omp parallel for num_threads(thread_count) default(none) shared(a, n) private(i, tmp)
        for (i = 1; i < n; i += 2) if (a[i-1] > a[i]) { tmp = a[i-1]; a[i-1] = a[i]; a[i] = tmp; }
    else
#       pragma omp parallel for num_threads(thread_count) default(none) shared(a, n) private(i, tmp)
        for (i = 1; i < n-1; i += 2) if (a[i] > a[i+1]) { tmp = a[i+1]; a[i+1] = a[i]; a[i] = tmp; }
}

// Versi 2: buat tim SEKALI di luar, lalu pakai "omp for" (memakai tim yang sudah ada)
# pragma omp parallel num_threads(thread_count) default(none) shared(a, n) private(i, tmp, phase)
for (phase = 0; phase < n; phase++) {
    if (phase % 2 == 0)
#       pragma omp for
        for (i = 1; i < n; i += 2) { ... }
    else
#       pragma omp for
        for (i = 1; i < n-1; i += 2) { ... }
}   // "omp for" punya barrier implisit di akhir → fase berikutnya menunggu fase ini selesai
```
| Thread | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| 2× `parallel for` (detik) | 0,770 | 0,453 | 0,358 | 0,305 |
| 2× `for` (detik) | 0,732 | 0,376 | 0,294 | 0,239 |

> 👷 Versi 2 = mempekerjakan tim tukang **sekali** untuk seluruh proyek, bukan merekrut dan memecat tim setiap hari.

### 6.9 Scheduling loop 📅
**Masalah:** jika beban tiap iterasi tidak sama, pembagian default (blok berurutan) bisa timpang.
Contoh: `sum += f(i)` di mana `f(i)` memanggil `sin` sebanyak i kali → iterasi belakang jauh lebih berat.
```c
double f(int i) {
    int start = i*(i+1)/2, finish = start + i;
    double return_val = 0.0;
    for (int j = start; j <= finish; j++) return_val += sin(j);
    return return_val;
}
```
n = 10.000:
- 1 thread: 3,67 s
- 2 thread, pembagian default (block): 2,76 s → speedup **1,33** (thread 2 dapat bagian yang berat)
- 2 thread, pembagian cyclic (`schedule(static,1)`): 1,84 s → speedup **1,99** ✅

**Bentuk umum:** `schedule(type, chunksize)`

| Type | Cara kerja | Analogi |
|---|---|---|
| `static` | Iterasi dibagi **sebelum** loop jalan, dalam potongan sebesar chunksize, berputar (round-robin) | Guru membagi soal di awal: "kamu nomor 1–4, kamu 5–8" |
| `dynamic` | Iterasi dibagi jadi potongan chunksize; thread yang selesai **meminta** potongan berikutnya. Default chunksize = 1 | Antrean ambil nomor: siapa yang selesai duluan, ambil pekerjaan berikutnya |
| `guided` | Seperti dynamic, tetapi ukuran potongan **mengecil** seiring berjalannya loop (±sisa iterasi / jumlah thread). Mengecil hingga 1, atau hingga chunksize jika disebut | Makan prasmanan: awalnya ambil porsi besar, lama-lama ambil sedikit-sedikit agar semua selesai bersamaan |
| `auto` | Compiler/runtime yang memilih | Serahkan ke panitia |
| `runtime` | Ditentukan saat program jalan lewat variabel lingkungan `OMP_SCHEDULE` (mis. `export OMP_SCHEDULE="static,1"`) | Diputuskan pada hari-H |

**Static, 12 iterasi, 3 thread:**

| | Thread 0 | Thread 1 | Thread 2 |
|---|---|---|---|
| `schedule(static,1)` | 0, 3, 6, 9 | 1, 4, 7, 10 | 2, 5, 8, 11 |
| `schedule(static,2)` | 0, 1, 6, 7 | 2, 3, 8, 9 | 4, 5, 10, 11 |
| `schedule(static,4)` = default | 0, 1, 2, 3 | 4, 5, 6, 7 | 8, 9, 10, 11 |

**Guided, iterasi 1–9999, 2 thread (dari slide):**

| Thread | Potongan | Ukuran | Sisa |
|---|---|---|---|
| 0 | 1–5000 | 5000 | 4999 |
| 1 | 5001–7500 | 2500 | 2499 |
| 1 | 7501–8750 | 1250 | 1249 |
| 1 | 8751–9375 | 625 | 624 |
| 0 | 9376–9687 | 312 | 312 |
| 1 | 9688–9843 | 156 | 156 |
| 0 | 9844–9921 | 78 | 78 |
| 1 | 9922–9960 | 39 | 39 |
| 1 | 9961–9980 | 20 | 19 |
| 1 | 9981–9990 | 10 | 9 |
| 1 | 9991–9995 | 5 | 4 |
| 0 | 9996–9997 | 2 | 2 |
| 1 | 9998–9998 | 1 | 1 |
| 0 | 9999–9999 | 1 | 0 |

**Kapan pakai yang mana?** *(ringkasan)* Beban seragam → `static` (overhead paling kecil). Beban tidak seragam/tidak terduga → `dynamic` atau `guided` (lebih seimbang, overhead lebih besar).

### 6.10 Producer–consumer & message passing antar thread 📨
**Queue (antrean)** = struktur data alami untuk aplikasi multithread (seperti antrean kasir). **Producer** menghasilkan permintaan, **consumer** memprosesnya.

Program contoh: tiap thread punya **antrean pesan** sendiri (array antrean di-share). Mengirim = memasukkan pesan ke antrean thread tujuan; menerima = mengambil dari antrean sendiri.
```c
for (sent_msgs = 0; sent_msgs < send_max; sent_msgs++) {
    Send_msg();       // kirim pesan acak ke thread acak
    Try_receive();    // cek antrean sendiri
}
while (!Done())
    Try_receive();
```
```c
// Send_msg
mesg = random();
dest = random() % thread_count;
#   pragma omp critical
Enqueue(queue, dest, my_rank, mesg);

// Try_receive
queue_size = enqueued - dequeued;
if (queue_size == 0) return;
else if (queue_size == 1)
#   pragma omp critical            // tinggal 1 pesan → bisa rebutan dengan pengirim
    Dequeue(queue, &src, &mesg);
else
    Dequeue(queue, &src, &mesg);   // ≥2 pesan → aman tanpa kunci
Print_message(src, mesg);

// Done (deteksi selesai)
queue_size = enqueued - dequeued;
if (queue_size == 0 && done_sending == thread_count) return TRUE;   // done_sending++ tiap thread selesai mengirim
else return FALSE;
```
**Startup:** master membuat array antrean (satu per thread). Karena ada thread yang selesai menyiapkan antreannya lebih cepat, perlu **barrier eksplisit**:
```c
# pragma omp barrier      // semua thread menunggu di sini sampai lengkap
```

### 6.11 Empat alat mutual exclusion di OpenMP 🔐
| Alat | Sintaks | Ciri |
|---|---|---|
| **critical** | `# pragma omp critical` | Semua critical tanpa nama dianggap **satu** critical section global |
| **named critical** | `# pragma omp critical(nama)` | Critical dengan nama berbeda bisa jalan **bersamaan**. Tapi nama ditentukan saat kompilasi → tidak bisa membuat "satu kunci per antrean" yang jumlahnya dinamis |
| **atomic** | `# pragma omp atomic` | Hanya untuk **satu** pernyataan sederhana: `x <op>= expr;`, `x++;`, `++x;`, `x--;`, `--x;` dengan `<op>` ∈ {+, *, −, /, &, ^, \|, <<, >>}. Memakai instruksi *load-modify-store* hardware → **paling cepat** |
| **lock** | `omp_lock_t` + fungsi | Kunci sebagai **data**, sehingga bisa dibuat satu per objek (mis. satu per antrean) |

```c
omp_lock_t lock;
omp_init_lock(&lock);        // satu thread menginisialisasi
omp_set_lock(&lock);         // kunci (tunggu jika dipakai)
/* critical section */
omp_unset_lock(&lock);       // buka
omp_destroy_lock(&lock);     // satu thread menghancurkan

// di program pesan: tiap antrean punya lock sendiri
omp_set_lock(&q_p->lock);
Enqueue(q_p, my_rank, mesg);
omp_unset_lock(&q_p->lock);
```
> 🔐 **Analogi:** `critical` = satu kunci untuk seluruh gedung. `critical(nama)` = beberapa kunci yang dicetak saat gedung dibangun (jumlahnya tetap). `lock` = kunci yang bisa kamu buat sebanyak jumlah loker. `atomic` = mesin pencatat otomatis yang tidak bisa diganggu saat mencatat satu angka.

**Peringatan:**
1. Jangan **mencampur** jenis mutual exclusion untuk satu critical section yang sama (mis. `atomic` di satu tempat dan `critical` di tempat lain untuk variabel yang sama).
2. **Tidak ada jaminan keadilan (fairness)**: bisa saja satu thread terus-menerus kalah berebut kunci.
3. **Menyarangkan (nesting)** konstruksi mutual exclusion berbahaya → bisa **deadlock** (mis. di dalam `critical` memanggil fungsi yang juga memakai `critical` tanpa nama).

### 6.12 Perkalian matriks–vektor & thread-safety di OpenMP
```c
# pragma omp parallel for num_threads(thread_count) \
      default(none) private(i, j) shared(A, x, y, m, n)
for (i = 0; i < m; i++) {
    y[i] = 0.0;
    for (j = 0; j < n; j++)
        y[i] += A[i][j] * x[j];
}
```
| Ukuran | 1 thread | 2 thread | 4 thread |
|---|---|---|---|
| 8.000.000 × 8 | 0,322 (1,000) | 0,219 (0,735) | 0,141 (0,571) |
| 8000 × 8000 | 0,264 (1,000) | 0,189 (0,698) | 0,119 (0,555) |
| 8 × 8.000.000 | 0,333 (1,000) | 0,300 (0,555) | 0,303 (0,275) |

Sama seperti Pthreads: bentuk 8 × 8.000.000 buruk karena **false sharing** pada y.
Tokenizer dengan OpenMP (`# pragma omp for schedule(static,1)` + `strtok`) juga **tidak thread-safe** → pakai `strtok_r`.

### ✅ Inti yang harus diingat (Bab 6)
- Compile `-fopenmp`. `#pragma omp parallel num_threads(t)`; `omp_get_thread_num()`, `omp_get_num_threads()`.
- Team, master, fork-join, barrier implisit di akhir parallel block & `omp for`.
- Default: dideklarasikan sebelum blok = shared; di dalam blok = private.
- `reduction(op: var)` = cara rapi menggabungkan hasil.
- `parallel for`: loop harus bisa dihitung jumlah iterasinya; OpenMP **tidak** mengecek dependensi.
- Hapus loop-carried dependency dengan menghitung dari indeks + `private`.
- `default(none)` memaksa deklarasi scope.
- `schedule`: static / dynamic / guided / auto / runtime.
- Mutual exclusion: critical, critical(nama), atomic, lock. Jangan campur, tidak fair, jangan nesting.

<details><summary>🧠 Uji diri Bab 6</summary>

1. **Variabel `x` dideklarasikan di `main` sebelum `#pragma omp parallel`. Scope default-nya?** → Shared.
2. **Mengapa `factor = -factor` di dalam loop π salah saat diparalelkan?** → Loop-carried dependency: nilai factor bergantung iterasi sebelumnya.
3. **`schedule(static,2)`, 10 iterasi, 2 thread: thread 1 dapat iterasi apa?** → 2, 3, 6, 7.
4. **Kapan sebaiknya memakai `atomic` daripada `critical`?** → Saat critical section hanya satu pernyataan update sederhana seperti `x += expr`.
5. **Kenapa butuh `omp_lock_t` di program antrean pesan?** → Butuh satu kunci per antrean (jumlahnya dinamis); named critical tidak bisa karena namanya ditetapkan saat kompilasi.
6. **Mengapa versi "omp for" pada odd-even sort lebih cepat?** → Tim thread dibuat sekali, tidak fork-join di tiap fase.
</details>

---
## Bab 7 – Vectorization: SIMD di dalam CPU (slide 05b)

### 7.0 Analogi besar: 🖨️ Mesin cetak 8 halaman sekaligus
Instruksi biasa (skalar) memproses **satu angka** per instruksi. Instruksi vektor memproses **beberapa angka sekaligus** dengan satu instruksi.

> 🖨️ Printer biasa mencetak 1 halaman per tarikan; mesin cetak besar mencetak 8 halaman per tarikan. Sama-sama satu "perintah cetak", tapi hasilnya 8×.

- Prosesor punya **unit vektor**: MMX, SSE, AVX, AVX2, AVX512.
- Paling bermanfaat untuk program **computation bound** (dibatasi kecepatan hitung). Kebanyakan program **memory bound** (dibatasi bandwidth memori), tetapi vektorisasi tetap memberi tambahan kinerja dengan usaha kecil.

### 7.1 Istilah
- **Vector length:** jumlah elemen data yang diproses dalam **satu** instruksi.
- **Vector width:** ukuran unit vektor dalam **bit**.
- Contoh: AVX512 → width 512 bit → 512/64 = **8 angka double** sekaligus (vector length 8 untuk double, 16 untuk float).

```
Skalar:  a[0]+b[0] → c[0]    (1 siklus)       8 siklus untuk 8 angka
         a[1]+b[1] → c[1]    (1 siklus)
         ...
Vektor:  [a0 a1 a2 a3 a4 a5 a6 a7]
       + [b0 b1 b2 b3 b4 b5 b6 b7]
       = [c0 c1 c2 c3 c4 c5 c6 c7]  (1 instruksi, 1 siklus) → 1 cache line 64 byte sekaligus
```

### 7.2 Hardware dan software harus cocok
- Instruksi vektor dibuat oleh **compiler** (otomatis) atau **manual** (intrinsics/assembly).
- Instruksi harus **didukung hardware**: program AVX512 akan **gagal** di prosesor lama yang tidak punya AVX512.
- Compiler harus versi yang cukup baru untuk mengenal hardware terbaru.

**Sejarah singkat:**

| Generasi | Lebar | Catatan |
|---|---|---|
| MMX (~1997) | 64 bit, 8 register `mm` | Awalnya untuk grafis; GPU kemudian mengambil alih. AMD: 3DNow! |
| SSE, SSE2–SSE4 (~1999–2008) | 128 bit, 16 register `xmm` | SSE: floating point single precision pertama Intel; SSE2: double precision |
| AVX, AVX2 (~2011–2013) | 256 bit, 16 register `ymm` | AVX: panjang vektor 2×. AVX2: FMA (fused multiply-add) |
| AVX512 (~2017) | 512 bit, 32 register `zmm` | Pertama di Knights Landing, masuk prosesor mainstream 2017 |

### 7.3 Lima teknik vektorisasi (dari paling mudah)
1. **Pakai library yang sudah teroptimasi:** BLAS, LAPACK, ScaLAPACK, FFT, sparse solvers.
   > 🛒 Beli kue jadi dari toko terbaik daripada membuat sendiri.
2. **Auto-vectorization oleh compiler** (paling umum, usaha paling kecil). Tugas kita: menulis kode yang **mudah dikenali** compiler.
   - Pakai kata kunci `restrict` (C) / `__restrict` / `__restrict__` (C++): janji bahwa data yang ditunjuk pointer ini **tidak** diakses lewat pointer lain (tidak ada *aliasing*).
   - Flag GCC: `-ftree-vectorize`, `-fstrict-aliasing`, `-fopt-info-vec-optimized` (laporan loop mana yang berhasil divektorisasi), `-march=native -mtune=native`.
3. **Memberi petunjuk (hints) ke compiler**, mis. `#pragma omp simd`.
4. **Vector intrinsics:** fungsi C tingkat rendah (`__m128`, `__m256`, `__m512`, load/store/aritmetika). Rujukan: Intel Intrinsics Guide.
5. **Assembly:** paling sulit, paling jarang.

**Flag compiler lain (tabel slide):**

| Compiler | Strict aliasing | Vectorization | Floating-point |
|---|---|---|---|
| GCC/G++/GFortran v9 | `-fstrict-aliasing` | `-ftree-vectorize -march=native -mtune=native` (v8+: `-mprefer-vector-width=512`) | `-fno-trapping-math -fno-math-errno` |
| Clang v9 | `-fstrict-aliasing` | `-fvectorize -march=native -mtune=native` | `-fno-math-errno` |
| Intel icc v19 | `-ansi-alias` | `-restrict -xHost -vecabi=cmdtarget` (v18+: `-qopt-zmm-usage=high`) | |
| MSVC | tidak ada | aktif secara default | |
| IBM XLC v16 | `-qalias=ansi -qalias=restrict` | `-qsimd=auto -qhot -qarch=pwr9 -qtune=pwr9` | |

### 7.4 Kenapa auto-vectorization bisa gagal? ❌
- **Data dependency** (penyebab utama)
- **Alignment** (data tidak sejajar)
- Ada **pemanggilan fungsi** di dalam loop
- **Control flow kompleks** / banyak `if`
- **Loop tidak bisa dihitung** (*not countable*)
- **Tipe data campur**
- Akses **tidak berurutan** (non-unit stride, mis. `a[i*3]`)
- Badan loop terlalu kompleks
- Compiler menilai vektorisasi tidak menguntungkan

#### Tiga jenis dependensi data 🍳
| Jenis | Nama lain | Arti | Contoh |
|---|---|---|---|
| **Flow dependency** | **RAW** (read-after-write) | Variabel **dibaca setelah ditulis** | `a = 40; …; c = a + 2;` (c butuh a yang baru) |
| **Anti-flow dependency** | **WAR** (write-after-read) | Variabel **ditulis setelah dibaca** | `a = b + 1; b = 21;` (b tidak boleh diubah sebelum dibaca) |
| **Output dependency** | (WAW) | Variabel **ditulis lebih dari sekali** | `x = 1; … x = 2;` |

> 🍳 **Analogi resep:** RAW = tidak bisa menggoreng bawang sebelum bawangnya dipotong. WAR = jangan mencuci talenan sebelum bahan di atasnya dipindahkan. WAW = dua orang mengisi garam ke panci yang sama; urutan menentukan rasa akhir.

#### Loop-carried dependency
Dependensi **antar iterasi** loop:
```c
void lcd_ex(float* a, float* b, size_t n, float c1, float c2) {
    for (size_t i = 0; i < n; i++)
        a[i] = c1 * a[i + 17] + c2 * b[i];   // iterasi i membaca a[i+17] yang nanti ditulis iterasi i+17
}
```
- **Tidak bisa diparalelkan** dengan thread (urutan bisa kacau).
- **Bisa divektorisasi** asalkan **vector length < 17** (jarak dependensi). Dalam satu "paket" vektor (mis. 8 elemen), tidak ada elemen yang saling bergantung.

> 📏 Seperti barisan orang yang masing-masing harus menunggu orang ke-17 di depannya. Kelompok berisi 8 orang yang bersebelahan bisa bergerak bersamaan tanpa melanggar aturan.

#### Loop not countable
```c
typedef struct { float* data; size_t size; } vec_t;
void vec_eltwise_product(vec_t* a, vec_t* b, vec_t* c) {
    for (size_t i = 0; i < a->size; i++)          // compiler takut a->size bisa berubah di dalam loop
        c->data[i] = a->data[i] * b->data[i];     // (mis. jika c->data menunjuk ke a->size)
}
```
Solusi praktis *(tambahan)*: simpan batas di variabel lokal (`size_t n = a->size;`) dan pakai `restrict`.

### 7.5 `#pragma omp simd` 🧩
- Memecah loop menjadi potongan yang muat di register SIMD.
- **TIDAK** membuat thread (tidak memparalelkan antar core) — hanya vektorisasi di satu core.
```c
float sprod(float *a, float *b, int n) {
    float sum = 0.0f;
#   pragma omp simd reduction(+:sum)
    for (int k = 0; k < n; k++)
        sum += a[k] * b[k];          // (di slide tertulis a[k](b[k] – maksudnya a[k] * b[k])
    return sum;
}
```
Clause lain: `private(myvar)`, `reduction(min:var)`, `safelen(n)` (maks iterasi yang boleh berjalan bersamaan tanpa melanggar dependensi — cocok untuk kasus jarak 17 di atas), `simdlen(n)` (panjang vektor yang diinginkan).

### 7.6 Alignment (keselarasan memori) 🅿️
- Operasi vektor paling efisien bila data **selaras (aligned)** dengan ukuran vektor (AVX512: kelipatan **64 byte**).
- `float a[N];` biasanya hanya aligned 4 byte → compiler memecah loop menjadi:
  - **Peel loop:** mengerjakan beberapa elemen awal secara skalar sampai alamat jatuh tepat di kelipatan 64.
  - **Main loop:** bagian tengah, aligned, divektorisasi penuh.
  - **Remainder loop:** sisa elemen di akhir yang terlalu sedikit untuk satu vektor penuh.
- Deklarasi agar aligned: `double a[N] __attribute__((aligned(64)));`

> 🅿️ **Analogi parkir:** Truk pengangkut 8 mobil hanya bisa memuat jika mobil berbaris tepat di garis petak. Mobil-mobil di luar garis di awal (peel) dan sisa di akhir (remainder) diantar satu per satu.

### ✅ Inti yang harus diingat (Bab 7)
- Vector width (bit) vs vector length (jumlah elemen). AVX512 = 8 double.
- MMX 64 → SSE 128 → AVX/AVX2 256 → AVX512 512.
- Teknik: library → auto-vectorization (restrict, flag) → hints (`omp simd`) → intrinsics → assembly.
- Gagal karena dependensi, alignment, fungsi di loop, if kompleks, loop tidak countable, stride, dll.
- RAW, WAR, output dependency; loop-carried dependency jarak 17 bisa divektorisasi jika vector length < 17.
- `omp simd` = vektorisasi, bukan multithreading.
- Peel loop & remainder loop; `aligned(64)`.

<details><summary>🧠 Uji diri Bab 7</summary>

1. **AVX2 (256 bit) bisa memproses berapa float sekaligus?** → 256/32 = 8 float.
2. **`b = a + 1; a = 5;` jenis dependensi pada a?** → WAR (anti-flow).
3. **Apakah `#pragma omp simd` membuat banyak thread?** → Tidak, hanya vektorisasi.
4. **Apa itu remainder loop?** → Loop untuk sisa elemen yang terlalu sedikit untuk satu vektor penuh.
5. **Apa fungsi kata kunci `restrict`?** → Menjanjikan ke compiler bahwa data pointer itu tidak diakses lewat pointer lain, sehingga compiler berani memvektorisasi.
</details>

---

## Bab 8 – Hybrid MPI + OpenMP (slide 05c)

### 8.0 Analogi besar: 🌏 Perusahaan multinasional
- **MPI** = komunikasi **antar kantor** di kota berbeda (lewat email/kurir) → paralel **antar node**.
- **OpenMP** = karyawan-karyawan **di dalam satu kantor** yang berbagi meja & lemari → paralel **di dalam node** (multithreading).

```
MPI-only:        [Rank 0: 1 thread]     [Rank 1: 1 thread]
MPI + threads:   [Rank 0: ≋≋≋≋ banyak thread]   [Rank 1: ≋≋≋≋ banyak thread]
```
Pada hybrid, beberapa thread dalam satu proses bisa berjalan bersama dan **berbagi objek MPI** (communicator, request). Pertanyaannya: **thread mana yang boleh memanggil fungsi MPI?**

### 8.1 Empat level thread safety MPI ☎️
> ☎️ **Analogi:** Kantor cabang punya akses telepon ke kantor pusat. Siapa yang boleh menelepon?

| Level | Arti | Analogi telepon |
|---|---|---|
| `MPI_THREAD_SINGLE` | Hanya ada **1 thread** | Kantor kecil, hanya 1 pegawai |
| `MPI_THREAD_FUNNELED` | Banyak thread, tetapi **hanya master thread** yang memanggil MPI | Banyak pegawai, hanya **manajer** yang boleh menelepon |
| `MPI_THREAD_SERIALIZED` | Banyak thread, siapa pun boleh memanggil MPI, tetapi **hanya satu pada satu waktu** | Siapa pun boleh menelepon, tapi teleponnya cuma **satu** — bergantian |
| `MPI_THREAD_MULTIPLE` | Banyak thread, **semua boleh memanggil MPI kapan saja**, bersamaan | Setiap pegawai punya **telepon sendiri** |

- Urutan dari paling ketat ke paling bebas: SINGLE < FUNNELED < SERIALIZED < MULTIPLE. Aplikasi yang aman di suatu level juga aman di level yang lebih tinggi.
- Inisialisasi:
```c
int provided;
MPI_Init_thread(&argc, &argv, MPI_THREAD_FUNNELED, &provided);  // minta level, MPI menjawab level yang tersedia
```
- Jika memakai `MPI_Init` biasa → default **`MPI_THREAD_SINGLE`**.
- Tidak semua implementasi MPI mendukung di atas SINGLE; implementasi yang thread-safe mendukung MULTIPLE. *(Sebaiknya cek `provided` ≥ yang diminta.)*

### 8.2 Contoh kode tiap level
```c
// SINGLE: tidak ada thread
MPI_Init(&argc, &argv);
for (i = 0; i < 100; i++) compute(buf[i]);
/* Do MPI stuff */
MPI_Finalize();

// FUNNELED: loop diparalelkan OpenMP, MPI dipanggil di luar region paralel (oleh master)
MPI_Init_thread(&argc, &argv, MPI_THREAD_FUNNELED, &provided);
#pragma omp parallel for
for (i = 0; i < 100; i++) compute(buf[i]);
/* Do MPI stuff */            // hanya master yang sampai sini
MPI_Finalize();

// SERIALIZED: MPI dipanggil di dalam region paralel, tetapi dibungkus critical
MPI_Init_thread(&argc, &argv, MPI_THREAD_SERIALIZED, &provided);
#pragma omp parallel for
for (i = 0; i < 100; i++) {
    compute(buf[i]);
#   pragma omp critical
    { /* Do MPI stuff */ }    // satu thread pada satu waktu
}
MPI_Finalize();

// MULTIPLE: setiap thread memanggil MPI kapan saja
MPI_Init_thread(&argc, &argv, MPI_THREAD_MULTIPLE, &provided);
#pragma omp parallel for
for (i = 0; i < 100; i++) {
    compute(buf[i]);
    /* Do MPI stuff */        // bebas, bersamaan
}
MPI_Finalize();
```

### 8.3 Aturan MPI_THREAD_MULTIPLE
- **Ordering:** jika beberapa thread memanggil MPI bersamaan, hasilnya seolah-olah dipanggil **satu per satu dalam suatu urutan**.
  - Urutan dijamin **di dalam satu thread** yang sama.
  - User harus memastikan **pemanggilan kolektif** pada satu communicator terjadi dalam **urutan yang sama** di semua proses (ingat jebakan urutan Reduce di Bab 4!).
  - User harus memastikan pemanggilan MPI antar thread **tidak menimbulkan race condition** (mis. dua thread menerima ke buffer yang sama).
- **Blocking:** pemanggilan MPI yang blocking hanya mem-block **thread pemanggil**, bukan seluruh proses. Thread lain tetap jalan.

### ✅ Inti yang harus diingat (Bab 8)
- Hybrid: MPI antar node, OpenMP di dalam node.
- SINGLE (1 thread), FUNNELED (hanya master yang memanggil MPI), SERIALIZED (bergantian), MULTIPLE (bebas).
- `MPI_Init_thread(&argc, &argv, required, &provided)`; `MPI_Init` = SINGLE.
- MULTIPLE: hasil seperti urutan serial; kolektif harus berurutan sama; blocking hanya mem-block thread pemanggil.

<details><summary>🧠 Uji diri Bab 8</summary>

1. **Program memanggil MPI hanya di luar region `#pragma omp parallel`. Level minimal?** → FUNNELED.
2. **MPI dipanggil di dalam region paralel tetapi dibungkus `omp critical`. Level minimal?** → SERIALIZED.
3. **Level default jika memakai `MPI_Init`?** → MPI_THREAD_SINGLE.
4. **Di level MULTIPLE, jika satu thread memanggil `MPI_Recv` yang block, apakah thread lain ikut berhenti?** → Tidak, hanya thread pemanggil.
</details>

---

## Bab 9 – GPU & CUDA (slide 06a)

### 9.0 Analogi besar: 🏫 Sekolah raksasa
| CUDA | Analogi |
|---|---|
| **Host** (CPU) | Kantor kepala sekolah — memberi perintah, mengatur |
| **Device** (GPU) | Gedung sekolah berisi ribuan siswa |
| **Kernel** | Lembar tugas yang dikerjakan **semua siswa** (isinya sama) |
| **Thread** | Satu siswa |
| **Block** | Satu kelas (siswa sekelas bisa bekerja sama & memakai papan tulis kelas) |
| **Grid** | Seluruh sekolah (semua kelas) |
| **Warp** | Satu barisan 32 siswa yang bergerak serempak mengikuti aba-aba yang sama |
| **SM (Streaming Multiprocessor)** | Satu gedung/lantai dengan guru pengawas (scheduler) |
| **Registers** | Ingatan di kepala siswa (paling cepat, pribadi) |
| **Local memory** | Buku catatan pribadi siswa |
| **Shared memory** | Papan tulis kelas (cepat, hanya untuk satu kelas) |
| **Global memory** | Perpustakaan sekolah (besar, semua bisa akses, tapi jauh/lambat) |
| **Constant/texture memory** | Papan pengumuman sekolah (hanya bisa dibaca siswa, diisi kepala sekolah) |

### 9.1 Kenapa GPU? 💪
- GPU awalnya untuk grafis; kini dipakai untuk komputasi umum = **GPGPU** (*General Purpose computation on GPU*).
- Punya **sangat banyak core** (RTX 3080: 8704 core), bisa menjalankan **puluhan ribu thread**. Pasar game yang besar membuat teknologinya cepat berkembang.
- **Massively parallel computing:** komputasi paralel dengan unit/thread dalam jumlah sangat besar.
- Tren: GPU mencapai TFLOPs dan bandwidth memori ±10× CPU.

**CPU vs GPU:**

| | CPU | GPU |
|---|---|---|
| Desain | **Latency-oriented** (satu tugas secepat mungkin) | **Throughput-oriented** (sebanyak mungkin tugas per detik) |
| Transistor | Banyak untuk **cache** & **control** (branch prediction, out-of-order, superscalar) | Sebagian besar untuk **ALU** |
| Core | Sedikit, kompleks, sangat pintar | Sangat banyak, sederhana |
| Thread | Butuh sedikit thread | Butuh **ribuan** thread agar penuh |
| Analogi | **Beberapa profesor jenius**: bisa menyelesaikan soal apa pun dengan cepat | **Ribuan siswa SD**: masing-masing hanya bisa soal sederhana, tetapi kalau ada 10.000 soal penjumlahan, mereka jauh lebih cepat |

**GPU cocok untuk:** algoritma **data-parallel** (array data besar, streaming throughput), paralelisme SIMD halus, komputasi floating-point.
GPU dipakai untuk mempercepat **critical path** (bagian program yang paling berat).

**GPU TIDAK cocok jika:**
1. **Kurang paralelisme** — "With great power comes great need for parallelism." 🕷️
2. **Thread divergence** — thread GPU berjalan SIMD; banyak percabangan → tidak efisien.
   > 🪖 Barisan baris-berbaris: kalau setengah barisan harus belok kiri dan setengah belok kanan, mereka harus **bergantian** — separuh menunggu.
3. **Kebutuhan memori dinamis** — alokasi memori dilakukan di CPU.
4. **Algoritma rekursif** — memori stack GPU terbatas.

**Sejarah:** dulu GPGPU memakai API grafis (data diubah jadi *texture*). Kini: **CUDA** (milik NVIDIA) dan **OpenCL** (standar multi-vendor).

### 9.2 Arsitektur CUDA (lapisan software)
Aplikasi bisa memakai DirectX (HLSL compute shaders), OpenCL (OpenCL C kernels), CUDA Driver API, atau C/C++/Fortran/Java/Python (C for CUDA) → semuanya turun ke **CUDA Driver (PTX ISA)** → **dukungan CUDA di kernel OS** → **compute engine di dalam GPU NVIDIA**.

### 9.3 Model pemrograman CUDA 🔄
```
Host (CPU)         Device (GPU)
  │ kode serial
  │ ──── panggil Kernel0<<<...>>> ───▶  Grid 0: banyak block, banyak thread
  │ ◀──────────── hasil ───────────────
  │ kode serial
  │ ──── panggil Kernel1<<<...>>> ───▶  Grid 1
  ▼
```
- GPU = **coprocessor** untuk CPU, punya **DRAM sendiri** (device memory).
- Program dimulai di host (serial). Saat butuh, host **meluncurkan kernel** yang dijalankan paralel di device. Hasil dikirim kembali ke host.
- Thread GPU **sangat ringan** (biaya pembuatan kecil); GPU butuh **ribuan** thread agar terpakai penuh.
- Host dan device terhubung lewat bus **PCI Express** (di slide: ±8 GB/s) — jauh lebih lambat dari memori GPU. Jadi **transfer data host↔device itu mahal**; minimalkan.

### 9.4 Streaming Multiprocessor (SM) 🏭
- GPU terdiri dari beberapa **SM** (GTX 780: 12 SM), tiap SM berisi banyak core (GTX 780: 192 core/SM).
- Tiap SM punya: **ribuan register** (mis. 64K), **shared memory/L1 cache 64 KB** (bisa dikonfigurasi), constant cache, texture cache, **warp scheduler**, load/store unit, special function unit.
- Shared memory: RAM on-chip **cepat**, dibagi antar core dalam SM, memungkinkan **komunikasi antar thread** dalam satu block.

**Ide arsitektur kunci:**
- **SIMT (Single Instruction, Multiple Thread):** thread dijalankan dalam kelompok **32** yang disebut **warp**. Satu warp menjalankan instruksi yang sama bersamaan. Hardware otomatis menangani jika thread dalam warp mengambil cabang berbeda (tetapi jadi lebih lambat).
- **Hardware multithreading:** hardware mengatur alokasi resource & penjadwalan thread; GPU **menyembunyikan latency** memori dengan berganti ke warp lain yang siap.
- Thread punya semua resource yang dibutuhkan → warp yang tidak sedang menunggu bisa langsung jalan; **context switch praktis gratis**.

> 🍽️ **Analogi pelayan restoran:** Saat satu meja (warp) menunggu masakan (data dari memori), pelayan langsung melayani meja lain. Pelayan tidak pernah berdiri diam.

**Istilah lintas vendor:**

| Host | OpenCL | AMD | NVIDIA/CUDA | Intel Gen11 |
|---|---|---|---|---|
| CPU | Compute device | GPU | GPU | GPU |
| Multiprocessor | Compute unit (CU) | Compute unit | **Streaming multiprocessor (SM)** | Subslice |
| Processing core | Processing element (PE) | PE | **CUDA core** | Execution unit (EU) |
| Thread | Work item | Work item | **Thread** | – |
| Vector/SIMD | Vector | Vector | **Diemulasikan dengan SIMT warp** | SIMD |

### 9.5 Kompilasi 🔧
```
kode .cu ──▶ NVCC ──┬──▶ kode CPU (dikompilasi compiler biasa)
                    └──▶ kode PTX (virtual) ──▶ compiler PTX→target ──▶ kode mesin GPU (G80, ...)
```
```bash
nvcc file.cu -o program        # mode release
nvcc -g file.cu                # mode debug
nvcc -deviceemu file.cu        # emulasi device (jalan di CPU)
nvcc -deviceemu -g file.cu     # emulasi + debug
```

### 9.6 Ekstensi C untuk CUDA ✍️
Filosofi: tambahan **seminimal** mungkin pada C.

**Qualifier fungsi:**

| Qualifier | Dijalankan di | Dipanggil dari | Catatan |
|---|---|---|---|
| `__global__` | GPU | **CPU** (host) | Ini **kernel**; harus return `void`; tidak bisa dipanggil dari GPU |
| `__device__` | GPU | GPU | Fungsi bantu di GPU; tidak bisa dipanggil CPU |
| `__host__` | CPU | CPU | Fungsi C biasa |
| `__host__ __device__` | keduanya | keduanya | Mis. untuk overloading operator |

**Qualifier variabel:** `__shared__` (shared memory per block), `__constant__` (constant memory), `__device__` (global memory).

**Konfigurasi eksekusi & variabel bawaan:**
```c
dim3 grid_dim(100, 50);      // 100 × 50 = 5000 block
dim3 block_dim(4, 8, 8);     // 4 × 8 × 8 = 256 thread per block
my_kernel<<<grid_dim, block_dim>>>(...);                  // luncurkan kernel
my_kernel<<<grid_dim, block_dim, SharedMemBytes>>>(...);  // + ukuran shared memory dinamis

// di dalam kernel tersedia:
gridDim     // ukuran grid (jumlah block)
blockDim    // ukuran block (jumlah thread per block)
blockIdx    // nomor block saya
threadIdx   // nomor thread saya di dalam block
__syncthreads();   // barrier untuk semua thread dalam SATU block
```

**Batasan kode di GPU:** hanya bisa mengakses memori GPU; tidak ada argumen jumlah variabel (varargs); tidak ada variabel static; **tidak ada rekursi**; tidak ada polimorfisme dinamis.

### 9.7 Contoh 1: Penjumlahan vektor ➕
```c
// DEVICE CODE: tiap thread menjumlahkan SATU pasang elemen
__global__ void vector_add(float* A, float* B, float* C) {
    int i = threadIdx.x + blockDim.x * blockIdx.x;   // indeks global thread
    C[i] = A[i] + B[i];
}

// HOST CODE
int main() {
    float *h_A = ..., *h_B = ...;                    // h_ = memori host (CPU)
    float *d_A, *d_B, *d_C;                          // d_ = memori device (GPU)

    cudaMalloc((void**) &d_A, N * sizeof(float));   // 1. alokasi di GPU
    cudaMalloc((void**) &d_B, N * sizeof(float));
    cudaMalloc((void**) &d_C, N * sizeof(float));

    cudaMemcpy(d_A, h_A, N * sizeof(float), cudaMemcpyHostToDevice);   // 2. kirim data ke GPU
    cudaMemcpy(d_B, h_B, N * sizeof(float), cudaMemcpyHostToDevice);

    vector_add<<<N/256, 256>>>(d_A, d_B, d_C);       // 3. jalankan: N/256 block × 256 thread

    // 4. (seharusnya) ambil hasil: cudaMemcpy(h_C, d_C, ..., cudaMemcpyDeviceToHost);
    // 5. bebaskan: cudaFree(d_A); ...
}
```
**🧮 Menghitung indeks global:** `i = threadIdx.x + blockDim.x * blockIdx.x`
Contoh: blockDim.x = 256, thread nomor 5 di block nomor 2 → i = 5 + 256 × 2 = **517**.
> 🏫 Seperti nomor absen se-sekolah: (nomor kelas × jumlah siswa per kelas) + nomor absen di kelas.

**Pola 5 langkah program CUDA:** ① alokasi memori GPU → ② salin data host→device → ③ luncurkan kernel → ④ salin hasil device→host → ⑤ bebaskan memori.

### 9.8 Hierarki thread 🧱
- Kernel dijalankan sebagai **grid** berisi **block**, tiap block berisi **thread**.
- **Block ID:** 1D atau 2D; **Thread ID:** 1D, 2D, atau 3D → memudahkan pengolahan data multidimensi (gambar, volume).
- Satu block: hingga **512 thread** (GPU generasi lama; GPU modern 1024), berkomunikasi lewat shared memory, dijamin berada di SM yang sama (*resident*), sinkronisasi dengan `__syncthreads()`.
- Semua thread dalam block menjalankan program yang sama; memakai **thread id** untuk memilih bagian data yang dikerjakan.

### 9.9 Model memori 🗄️
| Memori | Lingkup | Akses thread | Akses host | Kecepatan |
|---|---|---|---|---|
| **Registers** | per thread | baca/tulis | – | ⚡ paling cepat |
| **Local memory** | per thread (variabel otomatis, *register spill*) | baca/tulis | – | lambat (ada di DRAM) |
| **Shared memory** | per **block** | baca/tulis | – | ⚡ cepat (on-chip) |
| **Global memory** | per grid / aplikasi (semua thread, antar kernel) | baca/tulis | baca/tulis | 🐢 lambat |
| **Constant memory** | per grid | **hanya baca** | baca/tulis | cepat jika di-cache |
| **Texture memory** | per grid | **hanya baca** | baca/tulis | di-cache |

- **Global memory** = jalur utama pertukaran data host ↔ device; isinya terlihat semua thread.
- Local memory → komunikasi tidak ada (pribadi). Shared memory → komunikasi **antar thread dalam block**. Global memory → komunikasi **antar grid/kernel**.

**Fungsi memori:**
```c
cudaMalloc((void**)&ptr, size);   // alokasi di global memory: (alamat pointer, ukuran byte)
cudaFree(ptr);                    // bebaskan
cudaMemcpy(dst, src, size, kind); // 4 parameter: tujuan, sumber, jumlah byte, arah
                                  // kind: cudaMemcpyHostToHost / HostToDevice / DeviceToHost / DeviceToDevice
cudaMemset(ptr, value, size);     // isi memori device dengan nilai
```
Contoh slide: matriks 64×64 float:
```c
#define BLOCK_SIZE 64
Matrix Md;                                              // "d" = data di device
int size = BLOCK_SIZE * BLOCK_SIZE * sizeof(float);
cudaMalloc((void**)&Md.elements, size);
cudaMemcpy(Md.elements, M.elements, size, cudaMemcpyHostToDevice);  // M di host → Md di device
cudaMemcpy(M.elements, Md.elements, size, cudaMemcpyDeviceToHost);  // kembali ke host
cudaFree(Md.elements);
```

### 9.10 Contoh 2: Code walkthrough & kernel 2D
```c
// walkthrough1.cu — alokasi, isi nol di GPU, salin ke host, cetak
int main() {
    int dimx = 16;
    int num_bytes = dimx * sizeof(int);
    int *d_a = 0, *h_a = 0;                       // pointer device & host
    h_a = (int*) malloc(num_bytes);
    cudaMalloc((void**)&d_a, num_bytes);
    if (0 == h_a || 0 == d_a) { printf("couldn't allocate memory\n"); return 1; }
    cudaMemset(d_a, 0, num_bytes);                // isi 0 di GPU
    cudaMemcpy(h_a, d_a, num_bytes, cudaMemcpyDeviceToHost);
    for (int i = 0; i < dimx; i++) printf("%d ", h_a[i]);   // 0 0 0 ... 0
    printf("\n");
    free(h_a); cudaFree(d_a);
    return 0;
}
```
```c
// Kernel dengan indeks 2D: tiap thread menambah 1 pada satu elemen matriks
__global__ void mykernel(int *a, int dimx, int dimy) {
    int ix  = blockIdx.x * blockDim.x + threadIdx.x;   // kolom global
    int iy  = blockIdx.y * blockDim.y + threadIdx.y;   // baris global
    int idx = iy * dimx + ix;                          // ubah (baris, kolom) → indeks 1D (row-major)
    a[idx] = a[idx] + 1;
}
// host: dimx = dimy = 16
dim3 grid, block;
block.x = 4;  block.y = 4;                 // 16 thread per block (4×4)
grid.x = dimx / block.x;  grid.y = dimy / block.y;   // 4×4 = 16 block
mykernel<<<grid, block>>>(d_a, dimx, dimy); // total 256 thread untuk 256 elemen → semua elemen jadi 1
```
Contoh lain (*shuffle*): `new_array[i] = prev_array[indices[i]];` — tiap thread memindahkan satu elemen.

### 9.11 Aturan emas: block harus independen 🧩
- **Semua urutan eksekusi block harus valid:** block diasumsikan jalan sampai selesai tanpa disela, bisa dijalankan **dalam urutan apa pun**, **bersamaan atau berurutan**.
- Block boleh **berkoordinasi** tetapi **tidak boleh saling menunggu (sinkronisasi)**:
  - Penunjuk antrean bersama (*shared queue pointer*): ✅ OK
  - Kunci bersama (*shared lock*) antar block: ❌ mudah **deadlock** (block yang memegang kunci mungkin belum dijadwalkan).
- Independensi inilah yang membuat program CUDA **scalable**: GPU kecil menjalankan block bergantian, GPU besar menjalankan banyak sekaligus — kode yang sama.

> 🏫 **Analogi:** Setiap kelas mengerjakan tugasnya sendiri tanpa menunggu kelas lain. Sekolah kecil (sedikit ruangan) menjalankan kelas bergiliran; sekolah besar menjalankan semuanya bersamaan. Hasilnya sama.
> `__syncthreads()` hanya berlaku **di dalam satu kelas (block)**, bukan antar kelas.

### 9.12 Contoh 3: Perkalian matriks P = M × N 🧮
**Versi sederhana (tanpa tiling):** satu thread menghitung satu elemen P.
```c
typedef struct { int width; int height; float* elements; } Matrix;   // row-major
#define BLOCK_SIZE 16

__global__ void MatMulKernel(Matrix A, Matrix B, Matrix C) {
    float Cvalue = 0;
    int row = blockIdx.y * blockDim.y + threadIdx.y;
    int col = blockIdx.x * blockDim.x + threadIdx.x;
    for (int e = 0; e < A.width; ++e)
        Cvalue += A.elements[row * A.width + e] * B.elements[e * B.width + col];
    C.elements[row * C.width + col] = Cvalue;
}

void MatMul(const Matrix A, const Matrix B, Matrix C) {     // host
    // alokasi d_A, d_B, d_C + cudaMemcpy A, B ke device ...
    dim3 dimBlock(BLOCK_SIZE, BLOCK_SIZE);
    dim3 dimGrid(B.width / dimBlock.x, A.height / dimBlock.y);
    MatMulKernel<<<dimGrid, dimBlock>>>(d_A, d_B, d_C);
    // cudaMemcpy C kembali ke host, cudaFree semua ...
}
```
**Masalah: terlalu sering ke global memory.**
- Tiap perkalian-tambah (2 FLOP) butuh 2 akses memori (2 × 4 byte = 8 byte) → **4 byte per FLOP**.
- Bandwidth 86,4 GB/s ÷ 4 byte/FLOP = **21,6 GFLOPS** maksimum (realistis ±15 GFLOPS), padahal puncak GPU **346,5 GFLOPS**.
- M dan N dibaca dari global memory **WIDTH kali** (setiap baris/kolom dibaca berulang oleh banyak thread).

> 📚 **Analogi:** Untuk setiap angka yang dibutuhkan, siswa berjalan ke perpustakaan, mengambil satu angka, lalu kembali. Sangat boros waktu jalan.

**Versi tiled (memakai shared memory) ✅:**
- Satu **block** menghitung satu sub-matriks persegi **Csub** (BLOCK_SIZE × BLOCK_SIZE); satu **thread** menghitung satu elemen Csub.
- Iterasi per "ubin" (tile): seluruh block **bersama-sama** menyalin satu tile A dan satu tile B dari global ke **shared memory** (tiap thread menyalin satu elemen), lalu semua thread menghitung dari shared memory yang cepat.

```c
typedef struct { int width; int height; int stride; float* elements; } Matrix;

__device__ float GetElement(const Matrix A, int row, int col) { return A.elements[row * A.stride + col]; }
__device__ void  SetElement(Matrix A, int row, int col, float value) { A.elements[row * A.stride + col] = value; }
__device__ Matrix GetSubMatrix(Matrix A, int row, int col) {      // ambil tile ke-(row,col)
    Matrix Asub;
    Asub.width = BLOCK_SIZE; Asub.height = BLOCK_SIZE; Asub.stride = A.stride;
    Asub.elements = &A.elements[A.stride * BLOCK_SIZE * row + BLOCK_SIZE * col];
    return Asub;
}

__global__ void MatMulKernel(Matrix A, Matrix B, Matrix C) {
    int blockRow = blockIdx.y, blockCol = blockIdx.x;
    Matrix Csub = GetSubMatrix(C, blockRow, blockCol);   // tile C milik block ini
    float Cvalue = 0;
    int row = threadIdx.y, col = threadIdx.x;            // posisi thread di dalam tile

    for (int m = 0; m < (A.width / BLOCK_SIZE); ++m) {   // geser sepanjang tile-tile A dan B
        Matrix Asub = GetSubMatrix(A, blockRow, m);
        Matrix Bsub = GetSubMatrix(B, m, blockCol);

        __shared__ float As[BLOCK_SIZE][BLOCK_SIZE];     // "papan tulis kelas"
        __shared__ float Bs[BLOCK_SIZE][BLOCK_SIZE];

        As[row][col] = GetElement(Asub, row, col);       // tiap thread menyalin 1 elemen
        Bs[row][col] = GetElement(Bsub, row, col);
        __syncthreads();                                 // ⏸️ tunggu semua salinan selesai

        for (int e = 0; e < BLOCK_SIZE; ++e)
            Cvalue += As[row][e] * Bs[e][col];           // hitung dari shared memory (cepat)
        __syncthreads();                                 // ⏸️ tunggu semua selesai sebelum tile ditimpa
    }
    SetElement(Csub, row, col, Cvalue);
}
```
**Kenapa dua `__syncthreads()`?**
1. Yang pertama: jangan mulai menghitung sebelum **semua** elemen tile selesai disalin.
2. Yang kedua: jangan menimpa tile dengan tile berikutnya sebelum **semua** thread selesai memakainya.

**Hasilnya:** tiap elemen yang diambil dari global memory dipakai **BLOCK_SIZE kali** dari shared memory → akses global memory berkurang ±BLOCK_SIZE kali (mis. 16×).

> 📋 **Analogi:** Satu kelas memfotokopi **satu bab** buku perpustakaan dan menempelkannya di papan tulis kelas. Lalu semua siswa mengerjakan soal dari papan tulis. Setelah semua selesai, ganti bab berikutnya. Jauh lebih hemat daripada tiap siswa bolak-balik ke perpustakaan untuk tiap angka.

### ✅ Inti yang harus diingat (Bab 9)
- CPU = latency-oriented (sedikit core pintar + cache besar); GPU = throughput-oriented (banyak ALU sederhana).
- GPU buruk jika: kurang paralelisme, thread divergence, memori dinamis, rekursi.
- Host/device, kernel, grid → block → thread; warp = 32 thread (SIMT).
- `__global__` (dipanggil CPU, return void), `__device__` (dipanggil GPU), `__host__`.
- Indeks global: `blockIdx.x * blockDim.x + threadIdx.x`; 2D: `idx = iy * dimx + ix`.
- Pola: cudaMalloc → cudaMemcpy H2D → kernel<<<grid, block>>> → cudaMemcpy D2H → cudaFree.
- Memori: register & shared (cepat) vs local & global (lambat); constant/texture hanya-baca bagi thread.
- Block harus independen; `__syncthreads()` hanya dalam satu block.
- Tiling dengan shared memory memangkas akses global memory ±BLOCK_SIZE kali.

<details><summary>🧠 Uji diri Bab 9</summary>

1. **`kernel<<<64, 128>>>` meluncurkan berapa thread?** → 64 × 128 = 8192.
2. **Thread ke-10 di block ke-3, blockDim.x = 128. Indeks globalnya?** → 3 × 128 + 10 = 394.
3. **Satu warp berisi berapa thread?** → 32.
4. **Fungsi `__global__` bisa dipanggil dari mana dan harus mengembalikan apa?** → Dari host (CPU); harus `void`.
5. **Memori mana yang dipakai untuk komunikasi antar thread dalam satu block?** → Shared memory.
6. **Kenapa shared lock antar block berbahaya?** → Block tidak dijamin berjalan bersamaan; block pemegang kunci mungkin belum dijadwalkan → deadlock.
7. **Mengapa perkalian matriks sederhana hanya ±21,6 GFLOPS di GPU 86,4 GB/s?** → Butuh 4 byte per FLOP; 86,4 / 4 = 21,6.
</details>

---
## Bab 10 – Rangkuman Akhir

### 10.1 Perbandingan model pemrograman 🆚

| Aspek | **MPI** | **Pthreads** | **OpenMP** | **CUDA** |
|---|---|---|---|---|
| Jenis sistem | Distributed memory (juga jalan di shared) | Shared memory | Shared memory | GPU (akselerator) |
| Unit kerja | Proses (rank) | Thread | Thread (team) | Thread → block → grid |
| Memori | Private per proses | Global = shared, lokal = private | Shared/private per clause | Register, shared, global, constant, texture |
| Cara bertukar data | Kirim pesan eksplisit | Baca/tulis variabel bersama | Baca/tulis variabel bersama | cudaMemcpy host↔device; shared memory dalam block |
| Cara membuat paralel | `mpiexec -n p` | `pthread_create` / `join` | `#pragma omp parallel` | `kernel<<<grid, block>>>` |
| Sinkronisasi | `MPI_Barrier`, kolektif, blocking recv | mutex, semaphore, barrier, cond. var., rwlock | critical, atomic, lock, barrier, reduction | `__syncthreads()` (dalam block) |
| Compile | `mpicc` | `gcc ... -lpthread` | `gcc -fopenmp` | `nvcc` |
| Kesulitan | Sedang–tinggi | Tinggi (semua manual) | Rendah (pragma) | Sedang–tinggi |
| Analogi | Kantor cabang berkirim surat | Koki satu dapur, mengatur sendiri | Menempel label "kerjakan bareng" | Sekolah dengan ribuan siswa |

### 10.2 Padanan konsep antar model 🔁

| Konsep | MPI | Pthreads | OpenMP | CUDA |
|---|---|---|---|---|
| Identitas | `MPI_Comm_rank` | argumen `rank` | `omp_get_thread_num()` | `threadIdx`, `blockIdx` |
| Jumlah pekerja | `MPI_Comm_size` | `thread_count` | `omp_get_num_threads()` | `blockDim`, `gridDim` |
| Barrier | `MPI_Barrier` | buat sendiri / `pthread_barrier` | `#pragma omp barrier` | `__syncthreads()` |
| Penjumlahan global | `MPI_Reduce` / `MPI_Allreduce` | `my_sum` + mutex | `reduction(+: sum)` | (reduksi di shared memory) |
| Mutual exclusion | (tidak perlu, memori terpisah) | `pthread_mutex_*` | `critical`, `atomic`, `omp_lock_t` | (atomic functions) |
| Timer | `MPI_Wtime()` | `GET_TIME` | `omp_get_wtime()` | – |

### 10.3 Lembar rumus 🧮
| Rumus | Keterangan |
|---|---|
| **Waktu pesan = l + n/b** | l latency, n byte, b byte/detik |
| **S = T_serial / T_parallel** | Speedup |
| **E = S / p = T_serial / (p · T_parallel)** | Efisiensi |
| **T_parallel = T_serial / p + T_overhead** | Efek overhead |
| **Amdahl: S(N) = 1 / (S + P/N)**, maks = 1/S | Ukuran masalah tetap |
| **Gustafson: S(N) = N − S·(N − 1)** | Ukuran masalah bertambah sebanding N |
| **Pipeline k tahap, n operasi: k + (n − 1)** siklus | vs n·k tanpa pipeline |
| **Penjumlahan pohon: ⌈log₂ p⌉ langkah** | vs p − 1 langkah dengan master |
| **Paralelisme mesin = core × HT × (vector width / ukuran data)** | contoh 16 × 2 × 256/64 = 128 |
| **Indeks CUDA 1D = blockIdx.x · blockDim.x + threadIdx.x** | |
| **Indeks CUDA 2D: idx = iy · dimx + ix** | row-major |
| **Array 2D C: A[i][j] = A[i·n + j]** | row-major |

### 10.4 Sepuluh jebakan yang sering keluar ⚠️
1. **Menambah core ≠ otomatis lebih cepat.** Ada bagian serial (Amdahl) dan overhead.
2. **Race condition** muncul jika ≥2 thread mengakses data bersama dan minimal satu menulis.
3. **Critical section di dalam loop** → program jadi serial. Hitung lokal dulu, gabungkan sekali.
4. **Busy-waiting** boros CPU, bisa rusak oleh optimasi compiler, buruk bila thread > core.
5. **Kolektif MPI dicocokkan berdasarkan urutan**, bukan nama variabel (contoh b = 4, d = 5).
6. **Program MPI unsafe** jika bergantung pada buffering `MPI_Send` → uji dengan `MPI_Ssend`, perbaiki dengan urutan genap/ganjil atau `MPI_Sendrecv`.
7. **OpenMP tidak memeriksa dependensi** antar iterasi (contoh Fibonacci).
8. **Variabel yang dideklarasikan sebelum parallel block itu shared** secara default — sering jadi sumber bug; pakai `default(none)`.
9. **False sharing** membuat program lambat walau logikanya benar (contoh matriks 8 × 8.000.000).
10. **Fungsi dengan variabel `static` (strtok, random, localtime) tidak thread-safe**.

Bonus GPU: thread divergence, transfer host↔device yang mahal, dan **jangan sinkronisasi antar block**.

### 10.5 Peta konsep satu halaman 🗺️
```
                         KENAPA PARALEL?
          batas daya/panas → multicore → programmer harus menulis kode paralel
                                   │
           ┌───────────────────────┼─────────────────────────┐
       HARDWARE                SOFTWARE                  KINERJA
  ILP: pipeline,          SPMD, race condition,     Speedup, efisiensi,
  multiple issue,         critical section,         overhead, Amdahl,
  speculation, SMT        mutex, busy-wait,         Gustafson, strong/weak
  Flynn: SIMD/MIMD        message passing, PGAS     scaling, timing
  vektor, GPU             I/O rules
  UMA/NUMA, cluster
  interconnect, cache coherence
                                   │
          ┌────────────────┬───────┴────────┬────────────────┐
     DISTRIBUTED       SHARED MEMORY      DALAM CORE        AKSELERATOR
        MPI          Pthreads / OpenMP    Vectorization     GPU / CUDA
   send/recv,        mutex, semaphore,    SSE/AVX,          grid/block/thread,
   kolektif,         barrier, cond var,   omp simd,         warp/SIMT,
   derived type,     rwlock, false        dependensi,       memori hierarkis,
   safety            sharing, schedule,   alignment         tiling
                     reduction
          └──────────── HYBRID: MPI antar node + OpenMP di dalam node ───────────┘
```

---

## Bab 11 – Glosarium

| Istilah | Penjelasan singkat |
|---|---|
| **Amdahl's law** | Speedup dibatasi oleh fraksi serial: 1/(S + P/N) |
| **Atomic** | Operasi yang tidak bisa disela; di OpenMP `#pragma omp atomic` untuk satu update sederhana |
| **Bandwidth** | Laju pengiriman data (byte/detik) |
| **Barrier** | Titik tunggu sampai semua thread/proses tiba |
| **Bisection width/bandwidth** | Jumlah link / total bandwidth yang melintasi potongan jaringan menjadi dua |
| **Block (CUDA)** | Kelompok thread yang bisa bekerja sama lewat shared memory |
| **Block / cyclic / block-cyclic** | Cara membagi data: berurutan / berselang-seling / berselang-seling per kelompok |
| **Broadcast** | Mengirim data dari satu ke semua |
| **Bus** | Jalur komunikasi bersama; makin banyak perangkat makin rebutan |
| **Busy-waiting** | Menunggu dengan terus mengecek kondisi (boros CPU) |
| **Cache coherence** | Menjaga agar salinan data di cache berbagai core konsisten |
| **Cluster** | Kumpulan komputer biasa yang dihubungkan jaringan |
| **Communicator** | Kelompok proses MPI yang boleh saling kirim pesan (`MPI_COMM_WORLD`) |
| **Concurrent / parallel / distributed computing** | Berlangsung bersamaan / bekerja sama erat / bekerja sama dengan program lain |
| **Condition variable** | Objek untuk tidur sampai kondisi terpenuhi; selalu bersama mutex |
| **Critical section** | Kode yang mengubah data bersama; hanya satu thread pada satu waktu |
| **Crossbar** | Interkoneksi switch yang memungkinkan komunikasi simultan; cepat tapi mahal |
| **Data / task parallelism** | Membagi data / membagi jenis tugas |
| **Deadlock** | Semua pihak saling menunggu selamanya |
| **Derived datatype** | Tipe MPI buatan yang menggabungkan beberapa data + posisinya agar dikirim sekali |
| **Device / host** | GPU / CPU |
| **Directory-based coherence** | Koherensi cache dengan tabel status tiap cache line |
| **Efficiency** | Speedup dibagi jumlah core |
| **False sharing** | Thread menulis variabel berbeda di cache line yang sama → saling invalidate |
| **Fine/coarse-grained multithreading** | Ganti thread tiap instruksi / hanya saat stall lama |
| **Flynn's taxonomy** | SISD, SIMD, MISD, MIMD |
| **Fork–join** | Thread dibuat (fork) lalu ditunggu selesai (join) |
| **Global/shared/local memory (CUDA)** | Untuk semua thread / satu block / satu thread |
| **Grid** | Kumpulan block yang menjalankan satu kernel |
| **Gustafson–Barsis' law** | Speedup jika ukuran masalah ikut membesar: N − S(N−1) |
| **ILP** | Instruction Level Parallelism (pipelining, multiple issue, speculation) |
| **Kernel** | Fungsi yang dijalankan paralel di GPU (`__global__`) |
| **Latency** | Waktu sampai byte pertama tiba |
| **Load balancing** | Membagi kerja secara merata |
| **Loop-carried dependency** | Iterasi bergantung pada hasil iterasi lain |
| **MIMD** | Banyak instruksi, banyak data — core independen |
| **Mutex** | Kunci untuk critical section |
| **Nondeterminism** | Hasil/urutan bisa berbeda antar eksekusi |
| **NUMA / UMA** | Waktu akses memori tidak sama / sama untuk semua core |
| **Peel / remainder loop** | Loop awal untuk menyelaraskan alignment / loop akhir untuk sisa elemen |
| **PGAS** | Partitioned Global Address Space — memori global yang terbagi per proses |
| **Pipelining** | Membagi operasi menjadi tahap yang berjalan tumpang tindih |
| **Pragma** | Instruksi khusus untuk compiler (`#pragma omp ...`) |
| **Race condition** | Hasil bergantung pada urutan akses thread ke data bersama |
| **Rank** | Nomor identitas proses MPI (0 … p−1) |
| **Read-write lock** | Banyak pembaca bersamaan, penulis eksklusif |
| **Reduction** | Menggabungkan banyak nilai jadi satu dengan operator yang sama |
| **SIMD / SIMT** | Satu instruksi banyak data / satu instruksi banyak thread (warp GPU) |
| **SM** | Streaming Multiprocessor, blok pemroses di GPU |
| **SMT** | Simultaneous Multithreading (mis. Hyperthreading) |
| **Snooping** | Koherensi cache dengan "menguping" broadcast di bus |
| **Semaphore** | Penghitung ≥ 0 dengan `sem_wait` (kurangi/tunggu) & `sem_post` (tambah) |
| **Speculation** | Menebak hasil cabang lalu menjalankannya lebih dulu |
| **Speedup** | T_serial / T_parallel |
| **SPMD** | Single Program Multiple Data |
| **Strong / weak scaling** | Ukuran total tetap / ukuran per prosesor tetap |
| **Superscalar** | Prosesor dengan dynamic multiple issue |
| **Tag** | Label pesan MPI |
| **Thread-safe / re-entrant** | Aman dipanggil banyak thread sekaligus (mis. `strtok_r`) |
| **Tiling** | Memecah matriks jadi ubin dan memuatnya ke shared memory |
| **Unsafe (MPI)** | Program yang benar hanya jika MPI_Send di-buffer |
| **Vector length / width** | Jumlah elemen per instruksi / lebar unit vektor (bit) |
| **Warp** | Kelompok 32 thread GPU yang berjalan serempak |

---

> 📝 **Tips belajar:** Untuk tiap model (MPI, Pthreads, OpenMP, CUDA), coba tulis ulang **3 program kunci** tanpa melihat: (1) hello world, (2) penjumlahan global / trapesium / π, (3) perkalian matriks–vektor atau matriks–matriks. Lalu jelaskan dengan kata-katamu sendiri kenapa versi naifnya lambat atau salah, dan bagaimana memperbaikinya.
