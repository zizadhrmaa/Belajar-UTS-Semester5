# 🎯 Panduan UTS IF3130/IF3230 Sistem Paralel & Terdistribusi
### Catatan materi versi ujian + contoh soal + pembahasan SELURUH soal UTS di Bundel Soal HMIF (UTS 2013/2014 – 2025/2026)

> **Cara memakai file ini**
> 1. Baca **Bagian 0** (peta soal) untuk tahu topik apa yang paling sering keluar.
> 2. Pelajari **Bagian I** per topik. Setiap topik berisi: konsep inti → "resep" langkah mengerjakan → contoh soal + pembahasan.
> 3. Kerjakan sendiri soal di **Bagian II** (pembahasan per tahun), baru buka jawabannya.
> 4. Uji diri dengan **Bagian III** (latihan baru) dan hafalkan **Bagian IV** (lembar contekan).
>
> ⚠️ **Catatan penting**
> - Pada halaman scan (UTS 2021/2022 dan 2024/2025) ada **pilihan yang sudah dicentang / coretan tangan**. Itu jawaban mahasiswa, **bukan kunci resmi**, dan beberapa di antaranya **salah**. Pembahasan di sini menghitung ulang semuanya.
> - Beberapa soal punya **ambiguitas** (salah ketik di soal, opsi yang tidak cocok). Bagian itu ditandai 🟨 beserta cara menjawab yang aman.
> - Materi yang **tidak ada di slide** tetapi dibutuhkan untuk menjawab ditandai *(tambahan)*.
> - Bagian **UAS** di bundel (RPC, NTP, Paxos, Raft, Chord, CAP, DFS) tidak dibahas karena berada di luar materi UTS.

---

## 📑 Daftar Isi
- [Bagian 0 – Peta Soal UTS](#bagian-0--peta-soal-uts)
- [Bagian I – Catatan Materi Versi Ujian](#bagian-i--catatan-materi-versi-ujian)
  - [T1. Kinerja: Speedup, Efisiensi, Amdahl, Scalability](#t1-kinerja-speedup-efisiensi-amdahl-scalability)
  - [T2. ILP: Pipelining, Multiple Issue, CPI, Multithreading](#t2-ilp-pipelining-multiple-issue-cpi-multithreading)
  - [T3. Roofline: FLOP/s vs Bandwidth](#t3-roofline-flops-vs-bandwidth)
  - [T4. Jaringan Interkoneksi & Bisection Width](#t4-jaringan-interkoneksi--bisection-width)
  - [T5. Cache Coherence & False Sharing](#t5-cache-coherence--false-sharing)
  - [T6. Pthreads](#t6-pthreads)
  - [T7. OpenMP](#t7-openmp)
  - [T8. MPI](#t8-mpi)
  - [T9. CUDA / GPU](#t9-cuda--gpu)
- [Bagian II – Pembahasan Soal UTS per Tahun](#bagian-ii--pembahasan-soal-uts-per-tahun)
- [Bagian III – Latihan Tambahan](#bagian-iii--latihan-tambahan)
- [Bagian IV – Lembar Contekan 1 Halaman](#bagian-iv--lembar-contekan-1-halaman)

---

## Bagian 0 – Peta Soal UTS

### 0.1 Topik × tahun

| Topik | 13/14 | 15/16 | 16/17 | 18/19 | 19/20 | 20/21 | 21/22 | 24/25 | 25/26 | Total |
|---|---|---|---|---|---|---|---|---|---|---|
| Amdahl / speedup / efisiensi / scalability | ✔ | | ✔ | ✔ | ✔ | | ✔ | | ✔ | **6** |
| ILP, pipeline, CPI, SMT, thread vs core | ✔✔ | ✔ | | | | ✔ | ✔ | | ✔ | **6** |
| Roofline (FLOP/s & bandwidth) | | | | | | | | ✔ | | 1 |
| Bisection width / interkoneksi / cache coherence / false sharing | | | | | ✔ | | ✔✔ | | ✔ | **4** |
| Pthreads (rwlock, linked list, barrier, menghitung thread) | | | | ✔ | ✔ | ✔ | ✔✔ | ✔ | | **6** |
| OpenMP (race, reduction, critical/atomic, schedule, single/task, dependensi loop) | | ✔ | ✔ | ✔✔ | ✔ | ✔✔✔✔ | ✔✔✔✔ | ✔ | ✔ | **15** |
| MPI (tracing kode, kolektif, deadlock, Sendrecv, derived type) | ✔ | ✔✔ | ✔✔ | ✔✔ | ✔ | ✔✔✔✔✔ | ✔✔ | ✔ | ✔✔ | **18** |
| CUDA (kode host, indeks, warp, divergence, coalescing, tiling, memori) | | ✔ | ✔✔ | ✔✔✔✔ | ✔✔✔✔ | ✔✔✔ | ✔✔✔✔✔✔ | ✔ | ✔ | **22** |

👉 **Prioritas belajar:** CUDA (22) → MPI (18) → OpenMP (15) — ketiganya ±70% soal → lalu Amdahl/ILP → lalu Pthreads → lalu interkoneksi/cache.

### 0.2 Sebelas tipe soal yang terus berulang
| # | Tipe soal | Contoh tahun | Resep di |
|---|---|---|---|
| 1 | Hitung speedup/efisiensi dengan Amdahl | 13/14, 16/17, 18/19, 19/20 | T1 |
| 2 | Strong vs weak scalability (dengan overhead) | 19/20, 21/22 | T1 |
| 3 | Pipeline + multiple issue → CPI / waktu | 15/16, 20/21 | T2 |
| 4 | **Jelaskan apa yang dilakukan program MPI** (ring, tree) | 13/14, 15/16, 18/19 | T8 |
| 5 | Bedakan kolektif MPI (Bcast/Scatter/Gather/Allgather/Reduce/Allreduce) + gambar | 15/16, 16/17, 18/19, 20/21 | T8 |
| 6 | Deadlock / semantik `MPI_Send` / ganti `MPI_Sendrecv` | 19/20, 20/21, 25/26 | T8 |
| 7 | Perbaiki race condition OpenMP (reduction, max, critical vs atomic) | 15/16, 19/20, 20/21, 21/22, 24/25 | T7 |
| 8 | `schedule(static/dynamic/guided)` + ilustrasi pembagian | 18/19, 20/21, 25/26 | T7 |
| 9 | **Lengkapi kode host CUDA** (malloc, memcpy, launch, free) | 16/17, 25/26 | T9 |
| 10 | Hitung **warp / divergence / coalescing / blok** | 18/19, 19/20, 20/21, 21/22 | T9 |
| 11 | Linked list paralel (Member/Insert/Delete) → rwlock | 18/19, 19/20, 20/21, 21/22 | T6 |

---

## Bagian I – Catatan Materi Versi Ujian

---

### T1. Kinerja: Speedup, Efisiensi, Amdahl, Scalability

#### 🧠 Konsep inti
```
Speedup     S = T_serial / T_parallel
Efisiensi   E = S / p
Overhead    T_parallel = T_serial/p + T_overhead
Amdahl      S(p) = 1 / (s + (1−s)/p)        s = fraksi serial, (1−s) = fraksi paralel
            S_maks (p → ∞) = 1 / s
Gustafson   S(p) = p − s·(p − 1)            (ukuran masalah ikut membesar)
```
> 🚗 **Analogi Amdahl:** bagian serial = gerbang tol yang macet. Mobil secepat apa pun tidak bisa memotong waktu antre di gerbang.

#### 🧾 Resep soal Amdahl
1. Tentukan **s** (fraksi yang TIDAK bisa diparalelkan). Hati-hati: soal sering memberi fraksi *paralel* (80%, 90%) → s = 1 − fraksi itu.
2. Maksimum (prosesor tak terbatas) → **1/s**.
3. Dengan p prosesor → **1/(s + (1−s)/p)**.
4. Jika diberi waktu (detik): T_par = s·T_ser + (1−s)·T_ser/p, lalu S = T_ser/T_par.
5. Efisiensi = S/p (boleh ditulis dalam %).

#### 🧾 Resep soal scalability
Definisi slide:
- **Strongly scalable:** efisiensi tetap ketika p dinaikkan **tanpa** menaikkan ukuran masalah n.
- **Weakly scalable:** efisiensi tetap ketika n dinaikkan **dengan laju yang sama** dengan p.

Langkah: tulis E(n, p) → substitusi p → kp:
- Jika E(n, kp) = E(n, p) → strongly scalable.
- Jika tidak, coba E(kn, kp) = E(n, p) → weakly scalable.
- Jika keduanya tidak → tidak scalable (atau scalable dengan laju n yang lain).

#### ✍️ Contoh soal T1-a
> 75% program bisa diparalelkan. T_serial = 400 s. Hitung (a) speedup maksimum, (b) T_par, S, E untuk p = 5.

**Jawab:** s = 0,25.
(a) S_maks = 1/0,25 = **4**.
(b) T_par = 0,25·400 + 0,75·400/5 = 100 + 60 = **160 s**; S = 400/160 = **2,5**; E = 2,5/5 = **0,5 (50%)**.

#### ✍️ Contoh soal T1-b (scalability dengan overhead)
> T_ser = n, program 100% paralel, overhead = log₂p. Apakah scalable?

**Jawab:** T_par = n/p + log₂p → E = T_ser/(p·T_par) = n / (n + p·log₂p).
- p → kp, n tetap: E = n/(n + kp·log₂(kp)) **turun** → bukan strongly scalable.
- n → kn: E = kn/(kn + kp·log₂(kp)) = n/(n + p·log₂(kp)) masih turun pelan (karena log₂(kp) > log₂p) → **tidak weakly scalable sempurna**; agar E tetap, n harus naik sebanding p·log p.
(Bandingkan dengan soal UTS 19/20 di mana overhead konstan = 1 → weakly scalable.)

---

### T2. ILP: Pipelining, Multiple Issue, CPI, Multithreading

#### 🧠 Konsep inti
- **ILP (Instruction Level Parallelism):** beberapa unit fungsional prosesor mengeksekusi instruksi secara bersamaan. Bentuknya: **pipelining**, **multiple issue** (static = dijadwalkan compiler; dynamic = superscalar, dijadwalkan saat runtime), **speculation** (menebak cabang lalu mengeksekusi lebih dulu; jika salah, diulang).
- **Pipelining** = tahap-tahap instruksi **berbeda** dikerjakan tumpang tindih → throughput naik, latensi satu instruksi tetap.
- **Multiple issue + pipeline:** tiap siklus, **w** instruksi masuk pipeline (w = issue width).

#### 🧮 Rumus
```
Pipeline k tahap, N instruksi, issue width w (tiap tahap 1 siklus):
    Jumlah siklus = k + (N/w − 1)
    CPI ideal     = 1/w            (CPI = cycle per instruction)
    Waktu         = jumlah siklus × waktu per siklus
Tanpa pipeline:   Jumlah siklus = k × N
```
Ilustrasi issue width 4, pipeline 5 tahap (IF, ID, EX, MEM, WB):
```
Siklus:   1     2     3     4     5     6     7
I1–I4    IF    ID    EX    MEM   WB
I5–I8          IF    ID    EX    MEM   WB
I9–I12               IF    ID    EX    MEM   WB
...
→ setelah pipeline penuh, setiap siklus 4 instruksi selesai → CPI = 1/4 = 0,25
```

#### 🧠 Hardware multithreading
| Jenis | Ciri |
|---|---|
| Fine-grained | Ganti thread setiap instruksi, lewati yang stall |
| Coarse-grained | Ganti thread hanya saat stall lama |
| **SMT** | Beberapa thread mengeluarkan instruksi **dalam siklus yang sama** ke unit-unit fungsional yang berbeda (memanfaatkan multiple issue). Contoh: Intel Hyperthreading |

#### 🧠 "Apakah ada manfaat thread > core fisik?" (soal klasik)
- **Ya, bermanfaat** jika thread sering **menunggu** (I/O, akses memori lambat, blocking) → saat satu thread menunggu, thread lain memakai core (*latency hiding*). Juga jika CPU mendukung **SMT/Hyperthreading** (thread hardware > core fisik), dan untuk load balancing yang lebih halus.
- **Tidak bermanfaat / merugikan** untuk program **compute-bound** yang tidak pernah menunggu: muncul overhead context switch, rebutan cache, dan sinkronisasi busy-wait jadi sangat buruk.
- Bukti dari slide (π, 8 core): busy-wait 8 thread 0,38 s → 64 thread **3,56 s**; mutex tetap ±0,38 s.

#### ✍️ Contoh soal T2
> Prosesor issue width 2, pipeline 6 tahap, 1 siklus = 0,5 ns. Berapa waktu untuk 1000 instruksi independen?

**Jawab:** siklus = 6 + (1000/2 − 1) = 505 → waktu = 505 × 0,5 ns = **252,5 ns**. CPI ≈ 505/1000 = 0,505 → mendekati 1/2.

---

### T3. Roofline: FLOP/s vs Bandwidth

*(Dari grafik "Performance" di slide 02 yang muncul di UTS 2024/2025.)*

#### 🧠 Konsep inti
```
Puncak komputasi (FLOP/s) = jumlah core × thread/core* × frekuensi × FLOP/siklus
Bandwidth memori (B/s)    = word/siklus × ukuran word (byte) × frekuensi
Arithmetic intensity (AI) = FLOP per byte data yang diambil
Kinerja tercapai          = min( puncak komputasi , AI × bandwidth )
```
\* Model slide (Robey & Zamora) menghitung hyperthread ikut mengalikan: "Multicore (4 core, 2 hyperthreads): 192 GFLOP/s" = 24 GFLOP/s (vektor) × 4 × 2.

> 🏭 **Analogi:** Pabrik (CPU) bisa memproses 24 ton/jam, tetapi truk bahan baku (memori) hanya mengantar 10 ton/jam → produksi hanya 10 ton/jam (**memory bound**). Kalau truk mengantar 600 ton/jam, pabrik yang menjadi batas: 24 ton/jam (**compute bound**).

#### ✍️ Contoh soal T3
> 8 core tanpa hyperthreading, 2 GHz, 2 FLOP/siklus. DRAM 1 word/siklus, 1 word = 8 byte. Program AI = 0,25 FLOP/byte.

**Jawab:** Puncak = 8 × 2·10⁹ × 2 = **32 GFLOP/s**. Bandwidth = 1 × 8 × 2·10⁹ = **16 GB/s**. AI × BW = 0,25 × 16 = 4 GFLOP/s → kinerja = min(32, 4) = **4 GFLOP/s** (memory bound).

---

### T4. Jaringan Interkoneksi & Bisection Width

#### 🧠 Konsep inti
- **Bisection width** = jumlah link minimum yang harus diputus untuk membagi node menjadi dua bagian sama besar = jumlah komunikasi simultan maksimum "melintasi pemisah" (diambil kasus terburuk).
- **Bisection bandwidth** = jumlah *bandwidth* link-link yang melintasi pemisah.
- Waktu pesan = latency + n/bandwidth.

| Topologi (p node) | Bisection width | Jumlah link |
|---|---|---|
| Bus | 1 | – |
| Ring | 2 | p |
| Toroidal mesh 2D (√p × √p) | 2√p | 2p |
| Hypercube (dimensi d, p = 2^d) | p/2 | (p/2)·log₂p |
| **Fully connected** | **(p/2)² = p²/4** | p(p−1)/2 |
| Crossbar (p×p) | p | – |

*(tambahan dari buku Pacheco; slide hanya memberi definisinya.)*

**Kenapa fully connected = (p/2)²?** Bagi menjadi dua kelompok p/2 node. Setiap node di kiri punya link langsung ke **setiap** node di kanan → (p/2) × (p/2) link melintasi pemisah.

#### ✍️ Contoh soal T4
> 10 node fully connected → bisection width = (10/2)² = **25**. Hypercube 16 node → 16/2 = **8**. Ring 16 node → **2**.

---

### T5. Cache Coherence & False Sharing

#### 🧠 Konsep inti
- **Cache coherence:** beberapa core menyimpan salinan variabel yang sama di cache masing-masing; ketika satu core mengubahnya, salinan lain menjadi basi.
  - **Snooping:** core berbagi **bus**; perubahan **di-broadcast** di bus; core lain yang "menguping" menandai salinannya **invalid**.
  - **Directory-based:** ada **directory** yang mencatat status setiap cache line; saat variabel diubah, hanya cache yang tercatat memegang line itu yang di-invalidate (lebih skalabel daripada broadcast).
  - Koherensi menimbulkan **lalu lintas/overhead komunikasi** (pesan invalidate, pengambilan ulang data).
- **False sharing:** thread menulis variabel **berbeda** yang kebetulan berada pada **satu cache line** yang sama. Setiap tulisan meng-invalidate cache line di core lain → core lain harus mengambil ulang line tersebut dari memori/cache lain → banyak *cache miss* dan lalu lintas koherensi, padahal secara logika tidak ada data yang dibagi.

```
Cache line 64 byte:  [ y[0] | y[1] | y[2] | y[3] | y[4] | y[5] | y[6] | y[7] ]
                        ↑ thread 0 menulis   ↑ thread 1 menulis
Setiap tulisan thread 0 → line di cache core 1 di-invalidate → core 1 miss → ambil ulang → dan sebaliknya (ping-pong).
```
> 📒 **Analogi:** Dua orang menulis di baris berbeda pada halaman yang sama dari satu buku; buku harus dioper bolak-balik setiap kali salah satu menulis.

**Bukti slide:** perkalian matriks–vektor 8 × 8.000.000 dengan 4 thread: efisiensi hanya **0,29** karena y hanya 8 double (1 cache line) dan ditulis berulang oleh semua thread.

**Cara mengatasi false sharing:** akumulasi ke variabel **lokal/private** lalu tulis sekali; *padding* agar data tiap thread berada di cache line berbeda; pembagian data yang membuat tiap thread menulis blok besar yang berdekatan.

#### ✍️ Contoh soal T5
> Program: `double sum[4]; #pragma omp parallel { int id = omp_get_thread_num(); for (i...) sum[id] += a[i]; }`. Mengapa lambat? Perbaiki.

**Jawab:** `sum[0..3]` berada di satu cache line → false sharing. Perbaikan: `double local = 0; for (...) local += a[i]; sum[id] = local;` atau langsung `reduction(+:total)`.

---
### T6. Pthreads

#### 🧠 Konsep inti (ringkas)
- Thread dibuat dengan `pthread_create`, ditunggu dengan `pthread_join`. Compile `gcc ... -lpthread`.
- Variabel **global = shared**, variabel lokal = **private**.
- **Race condition** → lindungi **critical section** dengan: busy-wait (boros CPU), **mutex** (`pthread_mutex_lock/unlock`), **semaphore** (`sem_wait/sem_post`, bisa mengatur urutan), **barrier**, **condition variable** (`pthread_cond_wait/signal/broadcast`), **read-write lock**.

#### 🧾 Resep: "Berapa thread yang dipakai?" (UTS 24/25)
**Tipe 1 – dari dependensi instruksi.** Kelompokkan instruksi yang saling bergantung ke thread yang sama; instruksi yang independen boleh di thread lain.
```
x++;          ┐
p = x + 5;    ├─ rantai dependensi (x → p → q): harus berurutan → 1 thread
q = p + 3;    ┘
c++;          ── tidak bergantung pada apa pun → 1 thread lain
→ total 2 thread
```
**Tipe 2 – menghitung thread dari kode.** Ingat:
- `fork()` menduplikasi **proses** (proses anak juga menjalankan sisa kode).
- Setiap proses punya **1 main thread** + thread yang dibuat dengan `pthread_create`.
```c
pthread_t thread[2];
fork();                                 // 2 proses
pthread_create(&thread[0], ...);        // tiap proses +1 thread
pthread_create(&thread[1], ...);        // tiap proses +1 thread
```
→ per proses: 1 main + 2 = 3 thread → **2 × 3 = 6 thread** (jika hanya menghitung yang dibuat oleh `pthread_create`: 4).

#### 🧾 Resep: Linked list paralel (Member/Insert/Delete)
Aturan: **masalah muncul jika minimal satu operasi MENULIS** (Insert/Delete).

| Pasangan operasi bersamaan | Masalah? | Contoh kejadian |
|---|---|---|
| Member + Member | ❌ Aman | Sama-sama hanya membaca |
| Insert + Delete | ✅ Bermasalah | Keduanya mengubah pointer `next` di sekitar node yang sama → node hilang/list rusak |
| Delete + Member | ✅ Bermasalah | Member sedang membaca node yang sedang di-`free` oleh Delete → akses memori yang sudah dibebaskan |
| Member + Insert | ✅ Bermasalah | Member bisa membaca pointer `next` yang setengah diperbarui → hasil salah |
| Delete + Delete | ✅ Bermasalah | Dua delete pada node bersebelahan/sama → double free, list rusak |

**Solusi Pthreads:** (1) satu mutex untuk seluruh list (mudah, tetapi semua operasi serial); (2) mutex per node (rumit, lambat, boros memori); (3) ✅ **read-write lock** (`pthread_rwlock_rdlock` untuk Member, `pthread_rwlock_wrlock` untuk Insert/Delete) → banyak pembaca bisa paralel.
**Nama umum masalahnya:** ***readers–writers problem***.

#### 🧾 Resep: program dominan baca, jarang tulis → read-write lock
```c
pthread_rwlock_t rwlock;                         // shared
pthread_rwlock_init(&rwlock, NULL);              // di main

/* BACA (boleh banyak thread bersamaan) */
pthread_rwlock_rdlock(&rwlock);
value = data[idx];                               // atau Member(x)
pthread_rwlock_unlock(&rwlock);

/* UBAH (eksklusif) */
pthread_rwlock_wrlock(&rwlock);
data[idx] = new_value;                           // atau Insert(x) / Delete(x)
pthread_rwlock_unlock(&rwlock);

pthread_rwlock_destroy(&rwlock);                 // di akhir
```
> 📚 **Analogi perpustakaan:** banyak orang boleh membaca buku yang sama bersamaan; saat pustakawan merevisi buku, tidak ada yang boleh membaca.

#### 🧾 Resep: mengenali barrier
```c
pthread_mutex_lock(&mutex);
counter++;
if (counter == thread_count) { counter = 0; pthread_cond_broadcast(&cond_var); }
else while (pthread_cond_wait(&cond_var, &mutex) != 0);
pthread_mutex_unlock(&mutex);
```
= **barrier dengan condition variable**: thread menunggu sampai semua thread lain tiba (belum selesai mengerjakan bagiannya), thread terakhir membangunkan semua.

---

### T7. OpenMP

#### 🧾 Resep 1: Race pada akumulasi → `reduction`
```c
// ❌ race: sum shared, banyak thread menulis
#pragma omp parallel for
for (int i = 0; i < 1000; i++) sum += i;

// ✅ perbaikan
#pragma omp parallel for reduction(+:sum)
for (int i = 0; i < 1000; i++) sum += i;        // hasil benar: 499500
```
Alternatif yang juga benar tetapi lebih lambat: `#pragma omp atomic` di depan `sum += i;` atau `critical`.

#### 🧾 Resep 2: Mencari maksimum
```c
// ❌ benar tetapi TIDAK paralel: semua iterasi masuk critical → efektif serial (plus overhead kunci)
#pragma omp parallel for
for (i = 0; i < N; ++i) {
    #pragma omp critical
    { if (arr[i] > max) max = arr[i]; }
}

// ✅ Perbaikan A (OpenMP ≥ 3.1): reduction max
#pragma omp parallel for reduction(max:max)
for (i = 0; i < N; ++i)
    if (arr[i] > max) max = arr[i];

// ✅ Perbaikan B (tanpa reduction max): maksimum lokal, critical sekali per thread
#pragma omp parallel
{
    int local_max = arr[0];                      // private
    #pragma omp for
    for (i = 0; i < N; ++i)
        if (arr[i] > local_max) local_max = arr[i];
    #pragma omp critical
    { if (local_max > max) max = local_max; }
}
```
💡 Prinsip: **jangan masuk critical section di setiap iterasi**; hitung lokal, gabung sekali.

#### 🧾 Resep 3: `critical` vs `atomic`
| | `critical` | `atomic` |
|---|---|---|
| Melindungi | Blok kode **apa pun** (banyak pernyataan, if, pemanggilan fungsi) | **Satu** pernyataan update sederhana: `x op= expr`, `x++`, `++x`, `x--`, `--x` |
| Implementasi | Lock/mutex umum | Instruksi hardware *load-modify-store* |
| Kecepatan | Lebih lambat | **Lebih cepat** |
| Hasil untuk `count++` | Sama-sama benar | Sama-sama benar |

- Pilih **atomic** untuk update satu variabel sederhana (counter, penjumlahan).
- Pilih **critical** untuk operasi kompleks: `if (a[i] > max) max = a[i];`, update beberapa variabel sekaligus, `Enqueue(...)`, I/O, dsb.
- Jangan mencampur atomic dan critical untuk variabel yang sama; keduanya tidak bisa "digabung".

#### 🧾 Resep 4: `parallel for` (satu baris) vs `parallel` + `for` (terpisah)
- **Satu baris (`#pragma omp parallel for`)**: jika region paralel **hanya berisi satu loop** for.
- **Terpisah (`#pragma omp parallel` lalu di dalamnya `#pragma omp for`)**: jika di dalam region paralel ada **lebih dari satu loop** atau **kode lain** selain loop (inisialisasi variabel private, `single`, `barrier`, `critical` setelah loop, dll.), atau jika ingin memakai **tim thread yang sama berulang kali** (menghindari fork/join berulang, mis. odd-even sort di dalam loop fase).
```c
// Satu baris
#pragma omp parallel for
for (i = 0; i < n; i++) a[i] = b[i] + c[i];

// Terpisah: ada kode lain + dua loop
#pragma omp parallel
{
    int my_sum = 0;                    // kode per thread
    #pragma omp for
    for (i = 0; i < n; i++) a[i] = b[i] + c[i];
    #pragma omp for
    for (i = 0; i < n; i++) my_sum += a[i];
    #pragma omp critical
    total += my_sum;
}
```
⚠️ `#pragma omp for` **tanpa** region `parallel` di sekitarnya hanya dijalankan oleh 1 thread.

#### 🧾 Resep 5: `schedule(type, chunk)`
| Type | Kapan iterasi dibagi | Cara |
|---|---|---|
| `static` | Sebelum loop | Potongan sebesar chunk dibagikan round-robin. Tanpa chunk: dibagi jadi ±n/t potongan berurutan |
| `dynamic` | Saat loop berjalan | Thread yang selesai meminta chunk berikutnya (default chunk = 1) |
| `guided` | Saat loop berjalan | Seperti dynamic, tetapi chunk mengecil (≈ sisa/t) sampai 1 atau sampai chunk |
| `auto` | Ditentukan compiler/runtime | |
| `runtime` | Dari variabel lingkungan `OMP_SCHEDULE` | |

**Ilustrasi 12 elemen (1..12), 3 thread:**
```
schedule(static,1):  T0: 1 4 7 10    T1: 2 5 8 11    T2: 3 6 9 12
schedule(static,2):  T0: 1 2 7 8     T1: 3 4 9 10    T2: 5 6 11 12
schedule(static,4):  T0: 1 2 3 4     T1: 5 6 7 8     T2: 9 10 11 12
```
**Static tanpa chunk dengan sisa** (cara GCC/libgomp): q = n div t, r = n mod t → **r thread pertama** mendapat q+1 iterasi, sisanya q. Contoh n = 14, t = 4: q = 3, r = 2 → T0: 1–4, T1: 5–8, T2: 9–11, T3: 12–14.

**Kapan static, kapan dynamic?**
- Beban tiap iterasi **seragam & kecil** → **static** (overhead nol saat runtime, lokalitas cache bagus). Dynamic malah lebih lambat karena setiap pengambilan chunk butuh sinkronisasi ke runtime.
- Beban tiap iterasi **tidak seragam / tidak terduga**, atau kecepatan thread berbeda (NUMA, hyperthread berbagi core, gangguan OS) → **dynamic/guided** lebih seimbang. Overhead-nya kecil bila kerja per chunk besar.
- Menaikkan **chunk size** pada dynamic = mengurangi overhead.

#### 🧾 Resep 6: Dependensi pada loop bersarang
Langkah:
1. Tulis setiap akses array dan tanyakan: "iterasi loop mana yang **membaca hasil iterasi lain** dari loop itu?"
2. Loop yang indeksnya muncul sebagai `idx-1` di sisi kanan (dan `idx` di kiri) → **loop-carried dependency** → **tidak boleh** diparalelkan.
3. Loop yang tidak membawa dependensi → **boleh** diparalelkan.
4. Untuk efisiensi: buat tim thread **sekali** di luar (`omp parallel`) dan pakai `omp for` di loop yang aman, atau tukar urutan loop (*loop interchange*) agar loop yang aman berada di **paling luar** dan pakai `parallel for` sekali saja.

Contoh: `x[i][j][k] = x[i][j][k-1] + x[i][j-1][k] * scale;` dengan loop k (luar) → j → i (dalam):
- k membawa dependensi (`k-1`) ❌, j membawa dependensi (`j-1`) ❌, **i bebas** ✅.

#### 🧾 Resep 7: `single`, `master`, `task`
- `#pragma omp single` → blok dieksekusi **sekali oleh salah satu thread** (thread mana pun), ada **barrier implisit** di akhir.
- `#pragma omp master` → hanya thread 0, tanpa barrier.
- `#pragma omp task` → membuat tugas yang bisa dikerjakan **thread mana pun, kapan pun** sebelum barrier berikutnya → urutan output task **tidak tentu**.

#### 🧾 Resep 8: Syarat worksharing `parallel for`
- Iterasi **tidak saling bergantung** (tidak ada loop-carried dependency).
- Bentuk loop *canonical*: indeks integer/pointer, batas & increment tidak berubah, jumlah iterasi bisa dihitung sebelum loop (tidak ada `break`).
- **Tidak** disyaratkan: ukuran loop kelipatan jumlah thread, indeks mulai dari 1, data harus integer.

#### ✍️ Contoh soal T7
> `#pragma omp parallel for schedule(static,3)` untuk 10 iterasi (0..9), 2 thread. Iterasi apa saja milik T1?

**Jawab:** chunk: [0,1,2] → T0, [3,4,5] → T1, [6,7,8] → T0, [9] → T1 → **T1: 3, 4, 5, 9**.

---

### T8. MPI

#### 🧾 Resep 1: Menjelaskan "apa yang dilakukan program MPI" (tracing)
1. Tentukan peran tiap rank (siapa kirim ke siapa: `next = (rank+1)%size`, `prev = (rank+size-1)%size` → pola **ring**; `rank+step`, `step*=2` → pola **pohon**).
2. Simulasikan dengan p kecil (mis. 4 atau 5) dalam tabel per langkah.
3. Jawab: apa yang **dihitung/dikirim**, berapa pesan, kapan tiap proses **keluar**, dan apakah ada **deadlock/bug**.

#### 🧾 Resep 2: Fungsi kolektif + ilustrasi (wajib bisa menggambar)
```
MPI_Bcast (root P0)          MPI_Scatter (root P0)          MPI_Gather (root P0)
P0 [A B C D] → P0 [A B C D]  P0 [A B C D] → P0 [A]          P0 [A] ┐
             → P1 [A B C D]                → P1 [B]          P1 [B] ├→ P0 [A B C D]
             → P2 [A B C D]                → P2 [C]          P2 [C] │
             → P3 [A B C D]                → P3 [D]          P3 [D] ┘
(semua dapat data UTUH yang sama)  (tiap proses dapat POTONGAN berbeda)

MPI_Allgather                      MPI_Reduce (SUM, root P0)       MPI_Allreduce (SUM)
P0 [A] ┐   P0 [A B C D]           P0 3 ┐                          P0 3 ┐   P0 10
P1 [B] ├→  P1 [A B C D]           P1 1 ├→ P0 10                   P1 1 ├→  P1 10
P2 [C] │   P2 [A B C D]           P2 4 │                          P2 4 │   P2 10
P3 [D] ┘   P3 [A B C D]           P3 2 ┘                          P3 2 ┘   P3 10
(= Gather + Bcast)                 (hasil hanya di root)           (= Reduce + Bcast)
```

| Pasangan | Perbedaan kunci |
|---|---|
| **Bcast vs Scatter** | Bcast: data root yang **sama dan utuh** dikirim ke semua. Scatter: data root **dipotong**, tiap proses menerima **potongan berbeda** |
| **Gather vs Allgather** | Gather: kumpulan potongan **hanya di root**. Allgather: **semua** proses menerima kumpulan lengkap |
| **Reduce vs Allreduce** | Reduce: hasil operasi (SUM/MAX/…) **hanya di root**. Allreduce: **semua** proses menerima hasilnya |
| **Allreduce vs Allgather** | Allreduce: data **digabung dengan operator** → 1 nilai (per elemen) di semua proses. Allgather: data **dijajarkan/dikonkatenasi** tanpa operasi → array berukuran p × n di semua proses |

**Cara cepat memilih dari signature:**
- Ada argumen **root** (angka rank, mis. `0`) → `MPI_Reduce` / `MPI_Gather` / `MPI_Scatter` / `MPI_Bcast`.
- **Tidak** ada root → `MPI_Allreduce` / `MPI_Allgather`.
- Ada **MPI_Op** (MPI_SUM, MPI_MAX) → Reduce/Allreduce.
- Jika hasil dibutuhkan **semua** proses untuk langkah berikutnya (mis. rata-rata dipakai menghitung selisih kuadrat) → **Allreduce**.

#### 🧾 Resep 3: Semantik `MPI_Send` & deadlock
- `MPI_Send` boleh **(a)** menyalin pesan ke buffer internal lalu kembali, **atau (b)** block sampai `MPI_Recv` pasangannya mulai. Pesan kecil biasanya (a), pesan besar biasanya (b).
- Setelah `MPI_Send` kembali, yang **pasti** hanyalah: **buffer kirim aman dipakai ulang** (datanya sudah disalin ke buffer internal **atau** sudah dikirim ke penerima). Tidak bisa diasumsikan mana yang terjadi.
- `MPI_Recv` **selalu** block sampai pesan cocok tiba.
- Pola **Send–Send lalu Recv–Recv** antara dua proses = **unsafe**: bisa jalan (jika di-buffer) atau **deadlock** (jika Send block).
- Solusi: urutan berlawanan (genap Send→Recv, ganjil Recv→Send), `MPI_Sendrecv`, atau non-blocking (`MPI_Isend/MPI_Irecv` + `MPI_Wait`).
- `MPI_Ssend` = synchronous send: dijamin block sampai receive pasangan dimulai (dipakai menguji safety).

**Synchronous vs asynchronous & buffer:** kalau variabel yang dikirim **langsung diubah** setelah perintah kirim, pakai pengiriman **blocking/synchronous** (`MPI_Send`/`MPI_Ssend`). Pengiriman **asynchronous/non-blocking** (`MPI_Isend`) kembali segera, sehingga buffer bisa saja berubah sebelum datanya benar-benar terkirim → data yang dikirim bisa salah, kecuali dipanggil `MPI_Wait` sebelum variabel diubah.

#### 🧾 Resep 4: Mengganti `MPI_Sendrecv` dengan Send/Recv tanpa deadlock
```c
if (my_rank % 2 != 0) {          // rank ganjil: terima dulu, lalu kirim
    MPI_Recv(temp_B, local_n, MPI_INT, partner, 0, comm, &status);
    MPI_Send(local_A, local_n, MPI_INT, partner, 0, comm);
} else {                         // rank genap: kirim dulu, lalu terima
    MPI_Send(local_A, local_n, MPI_INT, partner, 0, comm);
    MPI_Recv(temp_B, local_n, MPI_INT, partner, 0, comm, &status);
}
```
(Boleh juga ganjil kirim dulu & genap terima dulu — yang penting **berlawanan**.)

#### 🧾 Resep 5: Derived datatype (struct)
```c
typedef struct { int x; int y; } Point;
MPI_Datatype MPI_POINT;
int          blocklen[2] = {1, 1};
MPI_Aint     disp[2];
MPI_Datatype types[2] = {MPI_INT, MPI_INT};
Point p;
MPI_Aint base;
MPI_Get_address(&p,   &base);
MPI_Get_address(&p.x, &disp[0]);
MPI_Get_address(&p.y, &disp[1]);
disp[0] -= base;  disp[1] -= base;
MPI_Type_create_struct(2, blocklen, disp, types, &MPI_POINT);
MPI_Type_commit(&MPI_POINT);
/* ... pakai ... */
MPI_Type_free(&MPI_POINT);
```
(Karena dua int berurutan, alternatif sederhana: `MPI_Type_contiguous(2, MPI_INT, &MPI_POINT);`.)
**Tujuan derived datatype:** mengirim data tidak-berurutan/bercampur tipe **dalam satu pesan** (lebih efisien karena latency dibayar sekali), menghindari penyalinan manual ke buffer, kode lebih rapi. **Bukan** untuk membuat penyimpanan lebih kecil.

#### 🧾 Resep 6: Jumlah pesan minimum `MPI_Allreduce` (p proses)
- **Jumlah pesan minimum: 2(p − 1) Send dan 2(p − 1) Recv.** Fase 1 (reduce): p−1 pesan agar semua nilai terkumpul di satu proses. Fase 2 (broadcast): p−1 pesan agar semua proses menerima hasil. Ini batas bawah (*gossip problem* satu arah: 2p − 2 panggilan) *(tambahan)*.
- **Jumlah langkah (waktu) minimum: log₂p** dengan pola **butterfly** — tetapi totalnya p·log₂p pesan.
- Contoh p = 8: pohon reduce+bcast → 14 pesan, 6 langkah; butterfly → 24 pesan, 3 langkah.

#### 🧾 Resep 7: Memeriksa kode MPI yang "salah"
Checklist: ① `MPI_Init`, `MPI_Comm_rank`, `MPI_Comm_size` dipanggil? ② Pembagian data mengasumsikan jumlah proses tertentu? (Scatter 3 elemen × p harus = ukuran data) ③ Rank tujuan/sumber valid (0..size−1)? ④ Urutan Send/Recv berpotensi deadlock? ⑤ Bisa diganti kolektif (`MPI_Reduce`) yang lebih efisien (pohon, O(log p))? ⑥ `MPI_Finalize` dipanggil?

- Mengirim/menerima dari **rank yang tidak ada** (mis. rank 1 saat hanya 1 proses) → **error** `MPI_ERR_RANK` (program dihentikan oleh error handler default).

#### ✍️ Contoh soal T8
> 4 proses, nilai lokal 5, 1, 7, 3. Apa isi `x` di tiap proses setelah `MPI_Reduce(&v,&x,1,MPI_INT,MPI_MAX,2,comm)`? Setelah `MPI_Allreduce(&v,&x,1,MPI_INT,MPI_SUM,comm)`?

**Jawab:** Reduce MAX ke rank 2: **x di P2 = 7**, proses lain x tidak terdefinisi/tidak berubah. Allreduce SUM: **x = 16 di semua proses**.

---

### T9. CUDA / GPU

#### 🧾 Resep 1: Kerangka kode host (hafalkan 7 langkah)
```c
// 1. alokasi host
h_A = (float*) malloc(size);  h_B = (float*) malloc(size);  h_C = (float*) malloc(size);
// 2. alokasi device
cudaMalloc((void**)&d_A, size); cudaMalloc((void**)&d_B, size); cudaMalloc((void**)&d_C, size);
// 3. salin input host → device
cudaMemcpy(d_A, h_A, size, cudaMemcpyHostToDevice);
cudaMemcpy(d_B, h_B, size, cudaMemcpyHostToDevice);
// 4. luncurkan kernel
kernel<<<numBlocks, threadsPerBlock>>>(d_A, d_B, d_C, ...);
// 5. tunggu kernel selesai
cudaDeviceSynchronize();
// 6. salin hasil device → host
cudaMemcpy(h_C, d_C, size, cudaMemcpyDeviceToHost);
// 7. bebaskan memori
cudaFree(d_A); cudaFree(d_B); cudaFree(d_C);
free(h_A); free(h_B); free(h_C);
```
- `cudaMemcpy(tujuan, sumber, byte, arah)` — **tujuan dulu**!
- Jumlah blok agar menutupi N elemen: `(N + threads − 1) / threads` (pembulatan ke atas).

#### 🧾 Resep 2: Qualifier & ruang memori
| Deklarasi | Lokasi | Lifetime | Siapa yang akses |
|---|---|---|---|
| Variabel lokal di kernel (`float left;`) | **Register** (bisa *spill* ke local memory) | Thread | 1 thread |
| Array lokal besar di kernel | Local memory (di DRAM) | Thread | 1 thread |
| `__shared__` | **Shared memory** (on-chip) | **Block** | Semua thread dalam 1 block (**1 salinan per block**) |
| `__device__` (tanpa shared/constant) | **Global memory** | **Aplikasi** | Semua thread semua grid + host (via API) |
| `__constant__` | Constant memory | Aplikasi | Thread: baca saja; host: tulis |
| Hasil `cudaMalloc` | **Global memory** | Sampai `cudaFree` | Semua thread |

| Fungsi | Jalan di | Dipanggil dari |
|---|---|---|
| `__global__` (kernel, return `void`) | Device | Host (`<<<...>>>`) |
| `__device__` | Device | Device |
| `__host__` | Host | Host |

#### 🧾 Resep 3: Indeks thread
```c
int i   = blockIdx.x * blockDim.x + threadIdx.x;            // 1D
int Row = blockIdx.y * blockDim.y + threadIdx.y;            // 2D
int Col = blockIdx.x * blockDim.x + threadIdx.x;
int idx = Row * width + Col;                                // row-major
```
- Kernel `<<<m, n>>>` dengan m blok × n thread untuk matriks m×n: `my_ij = blockIdx.x * blockDim.x + threadIdx.x` (= baris × n + kolom).
- `kernel<<<1, 10>>>` → threadIdx.x = 0..9. `kernel<<<10, 1>>>` → threadIdx.x selalu 0 (blockIdx.x = 0..9).
- Agar `threadIdx.x == T && blockIdx.x == B` ada → butuh **≥ B+1 blok** dan **≥ T+1 thread per blok** (maks 1024 thread/blok).
- Satu thread mengerjakan **2 elemen**:
  - *Berdekatan* (i, i+1): `i = (blockIdx.x*blockDim.x + threadIdx.x)*2;` → kedua `if (i<n)` dan `if (i+1<n)`. Kurang coalesced (stride 2).
  - *Coalesced* (dua "seksi"): `i = blockIdx.x*blockDim.x*2 + threadIdx.x;` → elemen kedua `i + blockDim.x` dengan `if (i+blockDim.x < n)`.
- **Grid-stride loop**: `for (i = tid; i < n; i += total_threads)` → coalesced, menangani batas dengan benar, jumlah thread tidak harus ≥ n.

#### 🧾 Resep 4: Menghitung blok & warp
```
blok_x = ceil(lebar / blockDim.x),  blok_y = ceil(tinggi / blockDim.y)
warp per blok = (blockDim.x × blockDim.y) / 32
total warp = blok_x × blok_y × warp per blok
```
Contoh: gambar 800 (lebar) × 600 (tinggi), blok 16×16 → 50 × 38 blok, 256/32 = 8 warp/blok → **38 × 50 × 8 = 15.200 warp**.

#### 🧾 Resep 5: Warp divergence
Warp (32 thread berurutan, dilinearkan `threadIdx.y * blockDim.x + threadIdx.x`) **divergen** jika **sebagian** thread-nya mengambil cabang `if` dan sebagian tidak. Jika **semua** aktif atau **semua** tidak aktif → **tidak** divergen.

**(a) Reduksi naif** `if (t % stride == 0)` (blok 1024 thread = 32 warp):
| stride | Thread aktif per warp | Warp divergen |
|---|---|---|
| 1 | 32 (semua) | **0** |
| 2 | 16 | 32 |
| 16 | 2 | **32** |
| 32 | 1 | 32 |
| 64 | 1 di warp genap, 0 di warp ganjil | 16 |

**(b) Reduksi diperbaiki** `for (stride = blockDim.x; stride > 0; stride /= 2) if (t < stride)`: thread aktif selalu **berurutan dari 0** → warp penuh aktif atau penuh diam **selama stride ≥ 32**. Divergensi baru terjadi saat **stride < 32** (16, 8, 4, 2, 1), dan hanya di **1 warp** (warp 0).
Contoh blok 256: stride 256, 128, 64, 32 → tidak divergen; stride **16** (iterasi ke-5 jika dihitung dari 1) → mulai divergen.

**(c) Batas matriks** `if (Row < W && Col < W)`: hanya warp yang **melintasi tepi** matriks yang divergen. Untuk blok 16×16, satu warp = 2 baris × 16 kolom.

#### 🧾 Resep 6: Coalescing
Akses **coalesced** = thread-thread berurutan dalam satu warp (threadIdx.x berurutan) mengakses **alamat memori berurutan** → digabung menjadi sedikit transaksi.
- `N[k*Width + Col]` → Col berurutan antar thread → ✅ coalesced.
- `P[Row*Width + Col]` → ✅ coalesced.
- `M[Row*Width + k]` → indeks tidak bergantung pada threadIdx.x (semua thread sebaris membaca alamat yang sama; sepanjang iterasi k satu thread berjalan sepanjang baris) → dalam materi Kirk & Hwu dikategorikan **tidak coalesced**.
- `a[tid]` dengan `tid = threadIdx.x + blockIdx.x*blockDim.x` → ✅ coalesced. `a[tid*2]` → stride 2 → tidak sepenuhnya coalesced.

#### 🧾 Resep 7: Tiling dengan shared memory
- Tile T×T: setiap elemen yang dimuat dari global memory dipakai **T kali** dari shared memory → akses global memory turun menjadi **1/T**. (Tile 16×16 → 1/16; tile 32×32 → **1/32**.)
- Perlu `__syncthreads()` **setelah memuat** tile dan **setelah menghitung** (sebelum tile ditimpa).
- Variabel `__shared__` → **satu salinan per blok**. 1000 blok × 512 thread → **1000 versi**.
- Algoritma yang diuntungkan oleh shared memory/cache = yang punya **penggunaan ulang data (data reuse) tinggi** → *arithmetic intensity/density* tinggi (mis. perkalian matriks, konvolusi). Penjumlahan vektor (setiap data dipakai sekali) tidak diuntungkan.

#### 🧾 Resep 8: Kapan GPU cocok / tidak
| ✅ Cocok | ❌ Tidak cocok |
|---|---|
| Data sangat besar dan **operasi sama untuk tiap elemen** (data parallel, SIMT) — mis. tiap piksel diproses 1 thread | **Paralelisme kecil** / data kecil (overhead transfer PCIe lebih besar dari untung) |
| Ribuan–jutaan elemen **independen** (tidak ada dependensi antar elemen) | Banyak **percabangan** → thread divergence |
| Butuh **throughput** & bandwidth memori tinggi | **Rekursi** (stack GPU terbatas), alokasi memori **dinamis** |
| Komputasi floating-point intensif | Algoritma **sekuensial** dengan dependensi (mis. Fibonacci, menelusuri linked list) |

#### 🧾 Resep 9: CPU vs GPU (soal gambar arsitektur)
```
        CPU (latency-oriented)                GPU (throughput-oriented)
┌───────────────┬──────┬──────┐        ┌─┬──────────────────────────┐
│               │ ALU  │ ALU  │        │C│ ▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢ │
│   Control     ├──────┼──────┤        ├─┤ ▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢ │
│   (besar)     │ ALU  │ ALU  │        │C│ ▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢ │  ▢ = ALU/core kecil
├───────────────┴──────┴──────┤        ├─┤ ▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢ │  C = control+cache kecil
│        Cache (besar)        │        │C│ ▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢▢ │     per SM
├─────────────────────────────┤        ├─┴──────────────────────────┤
│            DRAM             │        │            DRAM            │
└─────────────────────────────┘        └────────────────────────────┘
```
- **CPU:** sedikit core yang kompleks; banyak transistor untuk **cache** dan **control** (branch prediction, out-of-order, superscalar, speculation) agar **satu** thread secepat mungkin (**latency**).
- **GPU:** ribuan core sederhana dalam SM; sebagian besar transistor untuk **ALU**; menyembunyikan latency memori dengan **berganti warp** (hardware multithreading), bukan dengan cache besar/spekulasi → **throughput** tinggi, bandwidth memori besar.

#### 🧾 Resep 10 *(tambahan)*: Histogram / bucket sort di CUDA
Struktur data: `in[]` (global), `count[B]` (jumlah per bucket), `offset[B]` (posisi awal tiap bucket = prefix sum dari count), `out[]` (hasil).
```c
// Kernel 1: hitung isi tiap bucket (1 thread = 1 elemen)
__global__ void count_k(int* in, int* count, int n, int minv, int range, int B) {
    int i = blockIdx.x * blockDim.x + threadIdx.x;
    if (i < n) {
        int b = (in[i] - minv) * B / range;      // indeks bucket
        atomicAdd(&count[b], 1);                  // atomik: banyak thread menambah counter yang sama
    }
}
// Langkah 2: exclusive prefix sum (scan) atas count → offset
// Kernel 3: sebar elemen ke posisi bucket-nya
__global__ void scatter_k(int* in, int* out, int* offset, int n, int minv, int range, int B) {
    int i = blockIdx.x * blockDim.x + threadIdx.x;
    if (i < n) {
        int b   = (in[i] - minv) * B / range;
        int pos = atomicAdd(&offset[b], 1);       // ambil slot berikutnya di bucket b
        out[pos] = in[i];
    }
}
// Kernel 4: urutkan tiap bucket (mis. 1 block per bucket, sort di shared memory)
```
Optimasi: histogram lokal per block di `__shared__` lalu digabung ke global (mengurangi rebutan atomik).

---
## Bagian II – Pembahasan Soal UTS per Tahun

> Coba jawab dulu, baru buka **Pembahasan**. Soal ditulis ulang secara ringkas; kode panjang hanya dikutip bagian pentingnya.

---

### 📘 UTS 2013/2014

**1. (10) Jelaskan ILP dan berikan contohnya.**
<details><summary>Pembahasan</summary>

ILP (*Instruction Level Parallelism*) adalah upaya meningkatkan kinerja prosesor dengan membuat beberapa komponen/unit fungsional prosesor mengeksekusi **beberapa instruksi secara bersamaan**, otomatis oleh hardware/compiler tanpa diprogram eksplisit. Contoh:
- **Pipelining:** penjumlahan floating point dipecah 7 tahap (fetch, compare exponent, shift, add, normalize, round, store); 1000 penjumlahan butuh 1006 ns, bukan 7000 ns.
- **Multiple issue:** dua adder menghitung `z[i] = x[i] + y[i]` untuk dua i berbeda sekaligus (static = dijadwalkan compiler; dynamic = superscalar).
- **Speculation:** `z = x + y; if (z > 0) w = x; else w = y;` prosesor menebak z > 0 dan langsung mengeksekusi `w = x`; jika salah, diulang.
</details>

**2. (10) Apakah ada manfaat membuat thread lebih banyak dari jumlah core fisik?**
<details><summary>Pembahasan</summary>

Tergantung jenis programnya (lihat T2):
- **Ada manfaat** jika thread sering menunggu (I/O, akses memori, blocking komunikasi): ketika satu thread menunggu, OS/hardware menjalankan thread lain → latency tersembunyi, core tidak menganggur. Juga pada CPU dengan **SMT/Hyperthreading**, satu core fisik memang bisa menjalankan 2 thread hardware yang memakai unit fungsional berbeda. Thread lebih banyak juga membantu load balancing jika beban tidak seragam.
- **Tidak ada manfaat (bahkan merugikan)** untuk program compute-bound yang selalu sibuk: thread berebut core → overhead context switch, cache saling menimpa, dan sinkronisasi busy-wait menjadi sangat lambat (slide: busy-wait 64 thread di 8 core 3,56 s vs 0,38 s). Speedup maksimum tetap dibatasi jumlah core.
</details>

**3. (10) Hanya 80% algoritma bisa diparalelkan. Speedup maksimum dengan prosesor tak terbatas?**
<details><summary>Pembahasan</summary>

s = 0,2 → S_maks = 1/s = 1/0,2 = **5×**. Alasan (Amdahl): S(p) = 1/(0,2 + 0,8/p); ketika p → ∞, 0,8/p → 0, sehingga S → 5. Bagian serial 20% tidak bisa dipercepat.
</details>

**4. (20) Jelaskan apa yang dihitung kode MPI berikut.**
```c
myvalue = rank;
for (int step = 1; step < p; step = step*2) {
  if (rank % step == 0) {
    if (isEven(rank/step)) { MPI_Recv(&received, 1, MPI_INT, rank+step, ...); myvalue += received; }
    else                   { MPI_Send(&myvalue,  1, MPI_INT, rank+step, ...); }
  } else break;
}
if (rank == 0) printf("%d\n", myvalue);
```
<details><summary>Pembahasan</summary>

Kode ini adalah **penjumlahan global berbentuk pohon (tree-structured global sum / reduksi)**. Nilai yang dijumlahkan adalah rank setiap proses, sehingga rank 0 mencetak
**0 + 1 + … + (p−1) = p(p−1)/2** (contoh p = 8 → 28) dalam **log₂p langkah**.

Simulasi p = 8 (maksud kode):
```
step 1: rank genap (0,2,4,6) menerima dari rank+1; rank ganjil mengirim lalu keluar (break di iterasi berikutnya)
        0:0+1=1   2:2+3=5   4:4+5=9   6:6+7=13
step 2: rank 0 & 4 (rank/2 genap) menerima dari 2 & 6;  2 & 6 mengirim
        0:1+5=6   4:9+13=22
step 4: rank 0 menerima dari 4 → 6+22 = 28 → dicetak
```
🟨 **Catatan teliti (nilai plus):** pada kode tertulis, pengirim mengirim ke `rank+step`, padahal penerimanya adalah `rank−step` (rank 1 seharusnya mengirim ke 0, bukan ke 2). Dengan kode apa adanya, pesan tidak cocok (rank 0 menunggu dari 1, rank 1 mengirim ke 2) → program **hang/deadlock**, dan rank p−1 bisa mengirim ke rank yang tidak ada → error. Perbaikan: `MPI_Send(&myvalue, 1, MPI_INT, rank-step, ...)`. Algoritma juga mengasumsikan **p pangkat 2** (jika tidak, `rank+step` penerima bisa ≥ p).
</details>

---

### 📘 UTS 2015/2016

**1. Hubungan multiple issue dengan pipelining. Ilustrasikan untuk issue = 4, pipeline 5 tahap (IF, ID, EX, MEM, WB). Hitung CPI.**
<details><summary>Pembahasan</summary>

Pipelining membuat tahap-tahap instruksi berbeda berjalan tumpang tindih (1 instruksi selesai per siklus setelah pipeline penuh, CPI = 1). Multiple issue **mereplikasi** pipeline/unit fungsional sehingga **w instruksi dimasukkan ke pipeline setiap siklus** → setelah pipeline penuh, w instruksi selesai per siklus. Keduanya saling melengkapi: pipeline mengisi dimensi "waktu", multiple issue mengisi dimensi "lebar".
```
Siklus:   1    2    3    4    5    6    7    8
I1–I4    IF   ID   EX   MEM  WB
I5–I8         IF   ID   EX   MEM  WB
I9–I12             IF   ID   EX   MEM  WB
I13–I16                 IF   ID   EX   MEM  WB
```
**CPI ideal = 1/4 = 0,25** (siklus per instruksi). Untuk N instruksi: siklus = 5 + N/4 − 1 → N = 100: 29 siklus (CPI 0,29); N besar → CPI → 0,25.
</details>

**2. Perbedaan `MPI_Bcast` dan `MPI_Scatter` + ilustrasi aliran data.**
<details><summary>Pembahasan</summary>

- **Bcast:** root mengirim **seluruh** data yang **sama** ke semua proses. Setelahnya, semua proses punya salinan identik.
- **Scatter:** root **memotong** array menjadi p bagian (block) dan mengirim **bagian ke-i ke proses i**. Tiap proses mendapat potongan berbeda.
```
Bcast:   P0[A B C D] → P0[A B C D], P1[A B C D], P2[A B C D], P3[A B C D]
Scatter: P0[A B C D] → P0[A],       P1[B],       P2[C],       P3[D]
```
Signature: `MPI_Bcast(buf, count, type, root, comm)` vs `MPI_Scatter(sendbuf, sendcount_per_proc, type, recvbuf, recvcount, type, root, comm)`.
</details>

**3. Program ring (`next = (rank+1)%size`, `prev = (rank+size-1)%size`, rank 0 mengirim `message = 10`, setiap proses terima dari prev, rank 0 mengurangi 1, teruskan ke next, berhenti jika message == 0; rank 0 menerima sekali lagi di akhir). Jelaskan untuk 5 proses.**
<details><summary>Pembahasan</summary>

Ini program **token ring**: sebuah "token" (bilangan) diedarkan berkeliling cincin 0 → 1 → 2 → 3 → 4 → 0 → … Setiap kali token melewati **rank 0**, nilainya **dikurangi 1**. Program berhenti setelah token bernilai 0 berkeliling satu putaran.

Jalannya (5 proses):
1. Rank 0 mengirim 10 ke rank 1. Semua proses masuk loop dan menunggu `MPI_Recv` dari `prev`.
2. Putaran 1: 1 → 2 → 3 → 4 meneruskan 10; rank 0 menerima 10, menguranginya jadi 9, meneruskan 9 ke rank 1.
3. Begitu seterusnya: rank 0 menerima 10, 9, …, 1 dan mengirim 9, 8, …, 0. Rank 1–4 meneruskan nilai apa adanya.
4. Saat rank 0 mengubah 1 → 0, ia mengirim 0 ke rank 1, lalu **keluar dari loop** (message == 0).
5. Rank 1 menerima 0, meneruskan ke rank 2, lalu keluar; rank 2, 3, 4 melakukan hal yang sama secara berurutan.
6. Rank 4 mengirim 0 ke rank 0. Rank 0 sudah di luar loop dan memanggil `MPI_Recv` terakhir untuk **menyerap** pesan 0 itu (supaya tidak ada pesan menggantung), lalu selesai.

Statistik: token berkeliling **10 putaran penuh + 1 putaran nilai 0**. Setiap proses melakukan **11 Recv dan 11 Send**.
**Cara keluar:** tidak bersamaan, melainkan **berurutan mengikuti cincin**: rank 1 keluar duluan (setelah meneruskan 0), lalu 2, 3, 4, dan **rank 0 terakhir** (setelah menerima 0 kembali). Tidak ada deadlock.
</details>

**4. OpenMP – apa yang kurang tepat & perbaiki:**
```c
#pragma omp parallel for
for (i = 0; i < N; ++i) {
  #pragma omp critical
  { if (arr[i] > max) max = arr[i]; }
}
```
<details><summary>Pembahasan</summary>

Hasilnya benar, tetapi **setiap iterasi masuk critical section** → semua thread antre bergantian → eksekusi **efektif serial**, bahkan lebih lambat dari serial karena overhead lock. Perbaikan (lihat T7 Resep 2): `reduction(max:max)`, atau hitung `local_max` per thread lalu `critical` **sekali** per thread. (Alternatif: cek dulu di luar critical `if (arr[i] > max)` lalu cek ulang di dalam critical — mengurangi frekuensi masuk critical.)
</details>

**5. CUDA – (a) struktur data bucket sort, (b) strategi membagi data ke bucket dengan thread.**
<details><summary>Pembahasan</summary>

Lihat **T9 Resep 10** *(tambahan — atomik tidak ada di slide)*.
(a) Array input di global memory; array `count[B]` (jumlah elemen per bucket); array `offset[B]` = prefix sum dari count (posisi awal bucket); array output; opsional histogram lokal `__shared__` per block.
(b) Algoritma: ① 1 thread per elemen menghitung indeks bucket `b = (x − min)·B/(max − min + 1)` lalu `atomicAdd(&count[b], 1)`; ② prefix sum count → offset; ③ 1 thread per elemen: `pos = atomicAdd(&offset[b], 1); out[pos] = x;`; ④ urutkan isi tiap bucket (mis. 1 block per bucket, sort di shared memory). Atomik diperlukan karena banyak thread menambah counter bucket yang sama (race condition).
</details>

---

### 📘 UTS 2016/2017

**1. Fungsi yang memakan 90% waktu serial diparalelkan. (a) Speedup yang mungkin, (b) speedup maksimum dengan 9 prosesor.**
<details><summary>Pembahasan</summary>

s = 0,1.
(a) Dengan prosesor tak terbatas: S = 1/0,1 = **10×**.
(b) p = 9: S = 1/(0,1 + 0,9/9) = 1/(0,1 + 0,1) = **5×** (efisiensi 5/9 ≈ 55,6%).
</details>

**2. OpenMP – kode loop 3 tingkat:**
```c
for (k=1; k<n; k++)
  for (j=1; j<n; j++)
    for (i=0; i<m; i++)
      x[i][j][k] = x[i][j][k-1] + x[i][j-1][k] * scale;     // versi 1
      // x[k][j][i] = x[k-1][j][i] + x[k][j-1][i] * scale;  // versi 2
```
(Pertanyaan di bundel terpotong; yang biasanya ditanyakan: loop mana yang bisa diparalelkan dan bagaimana caranya.)
<details><summary>Pembahasan</summary>

**Analisis dependensi** (Resep 6 T7):
- Loop **k** membawa dependensi (`k-1`) ❌.
- Loop **j** membawa dependensi (`j-1`) ❌.
- Loop **i** tidak membawa dependensi (setiap i hanya membaca elemen dengan i yang sama) ✅.

**Cara memparalelkan:**
- Sederhana: `#pragma omp parallel for` di loop i → benar, tetapi fork/join terjadi (n−1)² kali → overhead besar.
- Lebih efisien: buat tim sekali di luar k, worksharing di i:
```c
#pragma omp parallel private(j, k)      // j dan k harus private!
for (k = 1; k < n; k++)
  for (j = 1; j < n; j++) {
    #pragma omp for                      // barrier implisit di akhir menjaga urutan (k, j)
    for (i = 0; i < m; i++)
      x[i][j][k] = x[i][j][k-1] + x[i][j-1][k] * scale;
  }
```
- Paling efisien: **loop interchange** — pindahkan i ke paling luar, lalu satu `#pragma omp parallel for` di i (tiap thread mengerjakan i miliknya secara penuh untuk semua k, j; tanpa barrier berulang).

**Versi 2** (`x[k][j][i]`): dependensinya sama (k dan j membawa dependensi, i bebas), tetapi i adalah indeks **terakhir** → akses berurutan di memori (row-major) → lebih ramah cache dan bisa divektorisasi (`#pragma omp simd` di loop i). Pada versi 1, loop i terdalam melompat jauh di memori (stride besar) → kurang efisien; interchange sangat dianjurkan.
</details>

**3. MPI – (a) beda `MPI_Allreduce` dan `MPI_Allgather`; (b) program dengan tipe data `Point` (int X, int Y), input X dan Y dikirim ke seluruh proses sebagai Point.**
<details><summary>Pembahasan</summary>

(a) **Allreduce** menggabungkan data semua proses dengan **operator** (SUM, MAX, …) → setiap proses menerima **hasil reduksi** (ukurannya sama dengan input satu proses). **Allgather** **mengumpulkan/menjajarkan** data semua proses **tanpa operasi** → setiap proses menerima **seluruh** data (ukuran p × input). Contoh: nilai 3,1,4,2 → Allreduce SUM: semua dapat 10; Allgather: semua dapat [3,1,4,2].

(b)
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

    /* definisikan tipe Point */
    int          blocklen[2] = {1, 1};
    MPI_Datatype types[2]    = {MPI_INT, MPI_INT};
    MPI_Aint     disp[2], base;
    MPI_Get_address(&p,   &base);
    MPI_Get_address(&p.x, &disp[0]);
    MPI_Get_address(&p.y, &disp[1]);
    disp[0] -= base;  disp[1] -= base;
    MPI_Type_create_struct(2, blocklen, disp, types, &MPI_POINT);
    MPI_Type_commit(&MPI_POINT);

    if (rank == 0) {                       /* hanya rank 0 membaca stdin */
        printf("Masukkan X dan Y: ");
        scanf("%d %d", &p.x, &p.y);
    }
    MPI_Bcast(&p, 1, MPI_POINT, 0, MPI_COMM_WORLD);   /* dikirim sebagai 1 Point */

    printf("Proses %d menerima Point(%d, %d)\n", rank, p.x, p.y);

    MPI_Type_free(&MPI_POINT);
    MPI_Finalize();
    return 0;
}
```
</details>

**4. CUDA – (a) beda desain arsitektur CPU vs GPU + gambar; (b) lengkapi kode `Mat_add` (m = 512 blok, n = 256 thread).**
<details><summary>Pembahasan</summary>

(a) Lihat **T9 Resep 9** (gambar + penjelasan latency-oriented vs throughput-oriented).

(b)
```c
(1) my_ij = blockIdx.x * blockDim.x + threadIdx.x;        // = baris*n + kolom (blockDim.x = n)

(2) h_A = (float*) malloc(size);
    h_B = (float*) malloc(size);
    h_C = (float*) malloc(size);
    cudaMalloc((void**)&d_A, size);
    cudaMalloc((void**)&d_B, size);
    cudaMalloc((void**)&d_C, size);

(3) cudaMemcpy(d_A, h_A, size, cudaMemcpyHostToDevice);
    cudaMemcpy(d_B, h_B, size, cudaMemcpyHostToDevice);

(4) Mat_add<<<m, n>>>(d_A, d_B, d_C, m, n);               // m blok, masing-masing n thread

(5) cudaDeviceSynchronize();                              // (versi lama: cudaThreadSynchronize())

(6) cudaMemcpy(h_C, d_C, size, cudaMemcpyDeviceToHost);

(7) cudaFree(d_A); cudaFree(d_B); cudaFree(d_C);
    free(h_A); free(h_B); free(h_C);
```
n = 256 ≤ 1024 thread per blok → valid.
</details>

---

### 📘 UTS 2018/2019

**1. T_serial = 600 s, 80% bisa diparalelkan. (a) Speedup dengan prosesor tak terbatas, (b) efisiensi dengan 8 prosesor.**
<details><summary>Pembahasan</summary>

s = 0,2.
(a) S_maks = 1/0,2 = **5** (T_par → 120 s).
(b) T_par = 0,2·600 + 0,8·600/8 = 120 + 60 = **180 s**; S = 600/180 = **3,33**; E = 3,33/8 = **0,417 ≈ 41,7%**.
</details>

**2. Program Pthread didominasi pembacaan data, jarang pengubahan. Bagaimana menghindari race condition saat mengubah data, tetapi pembacaan tetap paralel? Beri contoh kode.**
<details><summary>Pembahasan</summary>

Gunakan **read-write lock** (`pthread_rwlock_t`): pembaca memakai `pthread_rwlock_rdlock` (banyak thread bisa memegang bersamaan → pembacaan tetap paralel), penulis memakai `pthread_rwlock_wrlock` (eksklusif: menunggu semua pembaca selesai dan memblokir pembaca/penulis lain). Mutex biasa akan menyerialkan pembacaan juga. Kode: lihat **T6 Resep "dominan baca"**. Data slide: 99,9% Member, 4 thread → rwlock 0,098 s vs satu mutex 0,385 s.
</details>

**3. Program ring yang sama dengan UTS 2015/2016 no. 3 (`a = next`, `b = prev`, tag 201). Jelaskan apa yang dilakukan & bagaimana tiap task keluar.**
<details><summary>Pembahasan</summary>

Sama dengan **UTS 2015/16 no. 3**: token bernilai 10 diedarkan berkeliling cincin, dikurangi 1 setiap melewati rank 0, sampai 0. Proses **tidak keluar bersamaan**: setelah meneruskan token 0, setiap proses keluar dari loop **secara berurutan** mengikuti cincin (1, 2, …, size−1), dan rank 0 keluar **terakhir** setelah `MPI_Recv` tambahan menerima token 0 dari rank size−1. Lalu semua memanggil `MPI_Finalize`.
</details>

**4. Bedakan (a) `MPI_Gather` vs `MPI_Allgather`, (b) `MPI_Reduce` vs `MPI_Allreduce` + ilustrasi.**
<details><summary>Pembahasan</summary>

Lihat ilustrasi **T8 Resep 2**.
(a) Gather: potongan dari semua proses dikumpulkan **hanya di root**. Allgather: semua proses mendapat **kumpulan lengkap** (setara Gather + Bcast), tanpa parameter root.
(b) Reduce: nilai semua proses digabung dengan operator, hasil **hanya di root**. Allreduce: hasil reduksi diterima **semua** proses (setara Reduce + Bcast, atau pola butterfly).
</details>

**5. OpenMP: kapan `parallel` dan `for` ditulis satu baris dan kapan harus terpisah? Beri contoh.**
<details><summary>Pembahasan</summary>

Lihat **T7 Resep 4**. Satu baris jika region paralel hanya berisi satu loop. Terpisah jika di dalam region ada lebih dari satu loop, ada kode lain (variabel private, `single`, `critical` setelah loop), atau tim thread ingin dipakai ulang di dalam loop luar yang serial (contoh odd-even sort: `#pragma omp parallel` di luar loop fase, `#pragma omp for` di loop dalam → fork/join hanya sekali, lebih cepat: 0,239 s vs 0,305 s untuk 4 thread).
</details>

**6. `schedule(type, chunksize)`: (a) sebutkan tipe-tipenya; (b) array 12 elemen (1..12), 3 thread — ilustrasikan static,1 / static,2 / static,4.**
<details><summary>Pembahasan</summary>

(a) static, dynamic, guided, auto, runtime (penjelasan di **T7 Resep 5**).
(b)
```
               Thread 0        Thread 1        Thread 2
static,1   :   1  4  7  10     2  5  8  11     3  6  9  12
static,2   :   1  2  7  8      3  4  9  10     5  6  11 12
static,4   :   1  2  3  4      5  6  7  8      9  10 11 12
```
</details>

**7 & 8. Kernel reduksi naif (`if (t % stride == 0) partialSum[2*t] += partialSum[2*t+stride];`), blok 1024, warp 32. Berapa warp yang divergen saat stride = 1? Saat stride = 16?** (pilihan 0 / 1 / 16 / 32)
<details><summary>Pembahasan</summary>

Blok 1024 thread = 32 warp.
- **Stride 1:** `t % 1 == 0` benar untuk **semua** thread → semua aktif → **0 warp** divergen → **(a) 0**.
- **Stride 16:** hanya thread dengan t kelipatan 16 yang aktif → di setiap warp (32 thread) ada 2 aktif dan 30 tidak → **semua 32 warp** divergen → **(d) 32**.
</details>

**9 & 10. Setiap thread menghitung 2 elemen output penjumlahan vektor. Inisialisasi i yang benar & coalesced? Statement untuk elemen kedua?**
<details><summary>Pembahasan</summary>

🟨 Soal mencampur dua versi latihan Kirk & Hwu ("adjacent" vs "coalesced"):
- Karena soal **menekankan "coalesced"** → no. 9: **(a)** `i = (blockIdx.x*blockDim.x)*2 + threadIdx.x` (opsi a salah ketik `threadIdx` tanpa `.x`); no. 10: **(d)** `if (i+blockDim.x < n) C[i+blockDim.x] = A[i+blockDim.x] + B[i+blockDim.x];`. Thread-thread berurutan mengakses alamat berurutan di kedua akses → coalesced penuh.
- Jika yang dimaksud literal "2 elemen **bersebelahan**": no. 9 **(b)** `i = (blockIdx.x*blockDim.x + threadIdx.x)*2`, no. 10 **(b)** `if (i+1 < n) C[i+1] = ...` — benar tetapi hanya setengah coalesced (stride 2).

Saran: pilih (a) & (d), lalu tulis satu kalimat alasan "coalesced" di lembar jawaban.
</details>

---

### 📘 UTS 2019/2020

**1. 20% tidak bisa diparalelkan, T_ser = n. (a) Hitung T_par dan S pada p core (overhead 0). (b) Jika 100% paralel tetapi overhead 1, dan core dinaikkan k kali (kp), apakah scalable? Strongly/weakly? Tunjukkan perhitungannya.**
<details><summary>Pembahasan</summary>

(a) T_par = 0,2n + 0,8n/p. S = n / (0,2n + 0,8n/p) = 1/(0,2 + 0,8/p) = **5p/(p + 4)**. (E = 5/(p + 4); p → ∞: S → 5.)

(b) T_par = n/p + 1 → S = n/(n/p + 1) = np/(n + p) → **E = S/p = n/(n + p)**.
- Core jadi kp, n tetap: E = n/(n + kp) < n/(n + p) → efisiensi **turun** → **tidak strongly scalable**.
- Core jadi kp dan n jadi kn: E = kn/(kn + kp) = n/(n + p) → efisiensi **tetap** → **scalable, weakly scalable**.
</details>

**2. Jelaskan terjadinya false sharing, contoh, dan mengapa menurunkan kinerja.**
<details><summary>Pembahasan</summary>

Lihat **T5**. Inti: thread di core berbeda menulis variabel **berbeda** yang berada di **satu cache line**; protokol koherensi memperlakukan seluruh line sebagai "berubah" → line di cache core lain di-invalidate setiap ada tulisan → core lain harus mengambil ulang line (cache miss + lalu lintas bus/koherensi) → ping-pong cache line → kinerja turun padahal tidak ada data yang benar-benar dibagi. Contoh: `y[8]` pada perkalian matriks–vektor 8 × 8.000.000 (efisiensi 4 thread hanya 0,29), atau `sum[thread_id] += …` di dalam loop.
</details>

**3. (a) Ganti `MPI_Sendrecv` di fase genap odd-even sort dengan `MPI_Send` dan `MPI_Recv` (isi i–iv). (b) `MPI_Sendrecv` disebut thread-safe; jelaskan maksud thread-safe.**
<details><summary>Pembahasan</summary>

(a) Pada fase genap, pasangan adalah (genap, ganjil). Urutan harus **berlawanan** agar tidak deadlock:
```c
if (my_rank % 2 != 0) {      // ganjil
  (i)   MPI_Recv(temp_B, local_n, MPI_INT, even_partner, 0, comm, &status);
  (ii)  MPI_Send(local_A, local_n, MPI_INT, even_partner, 0, comm);
        Merge_high(local_A, temp_B, temp_C, local_n);
} else {                     // genap
  (iii) MPI_Send(local_A, local_n, MPI_INT, even_partner, 0, comm);
  (iv)  MPI_Recv(temp_B, local_n, MPI_INT, even_partner, 0, comm, &status);
        Merge_low(local_A, temp_B, temp_C, local_n);
}
```
(Ganjil Send dulu & genap Recv dulu juga benar.)

(b) **Thread-safe** (definisi slide): sebuah blok kode/fungsi dapat dijalankan **bersamaan oleh beberapa thread tanpa menimbulkan masalah** (tidak memakai state bersama yang tidak dilindungi, seperti variabel `static` pada `strtok`). Dalam konteks soal, sifat penting `MPI_Sendrecv` sebenarnya adalah **"safe"**: MPI sendiri menjadwalkan kirim dan terima sehingga program **tidak deadlock** apa pun perilaku buffering `MPI_Send`. (Penggunaan MPI dari banyak thread sekaligus tergantung level `MPI_THREAD_MULTIPLE`.)
</details>

**4. Linked list paralel (IsMember, Insert, Delete). Kasus mana yang bermasalah (I), jelaskan (II), bagaimana Pthreads menanganinya (III), istilah umumnya (IV, bonus).**
a. dua IsMember, b. Insert & Delete, c. Delete & IsMember, d. IsMember & Insert.
<details><summary>Pembahasan</summary>

(I) **b, c, d** bermasalah; **a aman**.
(II) Lihat tabel **T6**: masalah muncul jika minimal satu operasi menulis (mengubah pointer / membebaskan node) sementara operasi lain membaca/menulis node yang sama.
(III) Satu mutex untuk seluruh list; mutex per node; atau **read-write lock** (rdlock untuk IsMember, wrlock untuk Insert/Delete) — terbaik jika mayoritas operasi IsMember.
(IV) **Readers–writers problem**.
</details>

**5. Kode I: `#pragma omp critical` `count++;` vs kode II: `#pragma omp atomic` `count++;`. (a) Beda hasil? (b) Keuntungan atomic? (c) Kapan critical lebih baik?**
<details><summary>Pembahasan</summary>

(a) **Tidak ada perbedaan hasil**; keduanya mencegah race sehingga count benar.
(b) **Atomic lebih cepat**: memakai instruksi hardware load-modify-store, tanpa overhead lock umum.
(c) **Critical** lebih baik/harus dipakai jika critical section lebih dari satu update sederhana: beberapa pernyataan, kondisi (`if (a[i] > max) max = a[i]`), update beberapa variabel sekaligus, pemanggilan fungsi (`Enqueue`), atau I/O.
</details>

**6. Kernel perkalian matriks dasar: mana yang coalesced?**
<details><summary>Pembahasan</summary>

**(c)** `M[Row*Width+k]` **tidak coalesced**, `N[k*Width+Col]` dan `P[Row*Width+Col]` **coalesced**. Thread-thread berurutan dalam warp berbeda di Col (threadIdx.x) → N dan P mengakses alamat berurutan. Indeks M tidak bergantung pada threadIdx.x; sepanjang iterasi k setiap thread menelusuri **baris** M (bukan antar-thread berurutan) → menurut analisis Kirk & Hwu dikategorikan tidak coalesced.
</details>

**7. Gambar 800 (n, horizontal) × 600 (m, vertikal), blok 16×16. Jumlah warp?**
<details><summary>Pembahasan</summary>

Blok x = ⌈800/16⌉ = 50; blok y = ⌈600/16⌉ = ⌈37,5⌉ = 38; warp per blok = 256/32 = 8. Total = **38 × 50 × 8 = 15.200 warp** → **(c) 38*8*50**.
</details>

**8. 1000 thread block × 512 thread. Variabel `__shared__` punya berapa versi?**
<details><summary>Pembahasan</summary>

Shared memory: **satu salinan per block** → **(b) 1000**.
</details>

**9. Tiling 32×32 pada perkalian matriks: reduksi bandwidth memory untuk input A dan B?**
<details><summary>Pembahasan</summary>

Setiap elemen yang dimuat dipakai 32 kali → akses global memory menjadi **(c) 1/32** dari semula.
</details>

---
### 📘 UTS 2020/2021

**1. `#pragma omp parallel for reduction(+:sum) schedule(runtime)`, `OMP_SCHEDULE=static`, max = 14, tiap iterasi `sleep(i < 4 ? i + 1 : 1)`. Mana yang benar?** (a) rank 0 berjalan 10 detik, (b) rank 1 5 detik, (c) rank 2 4 detik, (d) rank 3 2 detik, (e) tidak ada.
<details><summary>Pembahasan</summary>

Lama tidur: i = 1 → 2 s, i = 2 → 3 s, i = 3 → 4 s, i ≥ 4 → 1 s. Jumlah thread tidak disebut; opsi menyebut rank 3 → asumsikan **4 thread**.
`schedule(static)` tanpa chunk, 14 iterasi / 4 thread: q = 3, sisa 2 → dua thread pertama mendapat 4 iterasi:

| Thread | Iterasi | Waktu |
|---|---|---|
| 0 | 1, 2, 3, 4 | 2 + 3 + 4 + 1 = **10 s** |
| 1 | 5, 6, 7, 8 | 4 s |
| 2 | 9, 10, 11 | 3 s |
| 3 | 12, 13, 14 | 3 s |

Jawaban: **(a)**. (Rank 1 bukan 5 s, rank 2 bukan 4 s, rank 3 bukan 2 s.) Hasil `sum` = 105 (reduction benar).
🟨 Jika implementasi membagi dengan chunk ⌈14/4⌉ = 4 (T2: 9–12, T3: 13–14), maka (c) dan (d) juga benar; namun untuk soal pilihan tunggal, jawaban yang konsisten adalah (a).
</details>

**2. Kernel grid-stride: `for (i = tid; i < array_size; i += num_threads) result[i] = a[i] + b[i];` Mana yang benar?**
<details><summary>Pembahasan</summary>

**(b) Boundary condition sudah ditangani dengan benar** (`i < array_size`).
- (a) salah: thread berurutan mengakses i berurutan → **coalesced**.
- (c) salah: memperbesar blok tidak otomatis lebih cepat (kerja total sama; tergantung occupancy).
- (d) salah: divergensi hanya terjadi di iterasi terakhir sebagian warp, dampaknya kecil.
- (e) salah: setiap elemen hanya dibaca sekali (tidak ada data reuse) → shared memory tidak membantu.
</details>

**3. Pseudocode MPI dijalankan dengan 1 proses: rank 1 kirim ke proses 2, lainnya terima dari proses 1 lalu cetak "Message received!".**
<details><summary>Pembahasan</summary>

Dengan 1 proses, satu-satunya rank adalah 0 → masuk cabang else → `MPI_RECV` dari **rank 1 yang tidak ada** di `MPI_COMM_WORLD` (ukuran 1). MPI memeriksa validitas rank → **error** (`MPI_ERR_RANK`, program dihentikan oleh error handler default) → **(d) Muncul error**.
</details>

**4. `MPI_SEND` mengirim array besar. Saat `MPI_SEND` kembali, programmer dapat mengasumsikan…**
<details><summary>Pembahasan</summary>

Standar MPI: setelah `MPI_Send` kembali, **buffer kirim aman dipakai ulang**, karena data **sudah disalin ke buffer internal** ATAU **sudah diterima (dikirim ke) proses tujuan** — programmer tidak tahu yang mana. → **(d) Salah satu dari A atau B**.
(Untuk array besar, implementasi biasanya memakai cara A — Send block sampai penerima menerima — tetapi itu tidak dijamin standar.)
</details>

**5. Kode:**
```c
for (int d = 1; d < ts; d = d * 2)
  #pragma omp parallel for
  for (int t = 0; t < ts; t = t + 2 * d)
    if (t + d < ts) sums[t] = sums[t] + sums[t + d];
```
<details><summary>Pembahasan</summary>

Ini **reduksi penjumlahan berbentuk pohon**: d = 1 menjumlah pasangan (0,1), (2,3), …; d = 2 menjumlah (0,2), (4,6), …; dst. Total semua elemen terkumpul di **sums[0]** → **(a)**.
</details>

**6. Pernyataan I: vector processor (kelas SIMD) mengeksekusi instruksi yang sama pada beberapa data di lokasi memori berbeda. SEHINGGA Pernyataan II: pemrograman SIMD dilakukan dengan message passing producer–consumer antar proses dengan buffer sebagai critical section.**
<details><summary>Pembahasan</summary>

I **benar** (definisi SIMD/vector processor). II **salah**: message passing antar proses dan producer–consumer dengan critical section adalah ciri pemrograman **MIMD** (distributed/shared memory), bukan SIMD (SIMD = satu instruksi untuk banyak data, sinkron, tanpa komunikasi antar proses). → **(c) I benar, II salah**.
</details>

**7. Linked list: pasangan mana yang berpotensi bermasalah?** (dua isMember / Insert & Delete / Delete & IsMember / IsMember & Insert / dua Delete)
<details><summary>Pembahasan</summary>

Semua pasangan yang melibatkan penulisan: **Insert & Delete, Delete & IsMember, IsMember & Insert, dua Delete**. Hanya **dua isMember** yang aman.
</details>

**8. critical vs atomic untuk `count++` — pernyataan yang benar?**
<details><summary>Pembahasan</summary>

Hanya pernyataan pertama yang benar: **"Bila lingkungan & cakupan sama, keduanya memberikan hasil count yang sama."**
- "Kode I (critical) lebih cepat dari II" → salah (atomic lebih cepat).
- "Kode II (atomic) lebih baik untuk operasi kompleks" → salah (atomic hanya untuk satu update sederhana; critical untuk yang kompleks).
- "Bisa dioptimalkan dengan memakai keduanya sekaligus" → salah (tidak boleh mencampur jenis mutual exclusion untuk critical section yang sama).
</details>

**9 & 10. Program rata-rata & deviasi standar MPI. (I) `____(&local_sum, &global_sum, 1, MPI_FLOAT, MPI_SUM, MPI_COMM_WORLD); mean = global_sum / …;` (II) `____(&local_square_diff, &global_square_diff, 1, MPI_FLOAT, MPI_SUM, 0, MPI_COMM_WORLD);`**
<details><summary>Pembahasan</summary>

- **(I) → (d) `MPI_ALLREDUCE`:** signature tanpa root, dan `mean` dibutuhkan **semua** proses untuk menghitung selisih kuadrat lokal.
- **(II) → (c) `MPI_REDUCE`:** ada argumen root `0`, dan deviasi standar hanya dihitung/dicetak oleh rank 0.
</details>

**11. Pipeline multiple issue: CPI = 1/3, panjang pipeline 7 tahap, 1 ns per tahap. Waktu memproses 6000 elemen?**
<details><summary>Pembahasan</summary>

Issue width = 1/CPI = 3. Siklus = 7 + (6000/3 − 1) = 7 + 1999 = **2006** → waktu = **2006 × 10⁻⁹ s** → **(c)**.
</details>

**12. Rank 0: Send ke 1 lalu Recv dari 1. Rank 1: Send ke 0 lalu Recv dari 0. Kemungkinan yang terjadi?**
<details><summary>Pembahasan</summary>

Kedua proses **Send dulu**. Jika `MPI_Send` block (pesan besar / tidak di-buffer), keduanya menunggu receive yang tidak pernah dipanggil → **deadlock** → **(d)**. (Jika pesan kecil dan di-buffer, program bisa saja selesai — itulah sebabnya pola ini disebut **unsafe**. Jawaban yang diharapkan adalah risiko deadlock.)
</details>

**13. `parallel` → `single` { print "Mulai "; task print "task1 "; task print "task2 "; print "selesai mencetak " }, 2 thread. Output yang mungkin?**
<details><summary>Pembahasan</summary>

- Blok `single` dijalankan **sekali** oleh satu thread → semua teks muncul **sekali** → opsi (b) dan (e) (dicetak 2 kali) **mustahil**.
- `task` dapat dikerjakan oleh thread mana pun, kapan saja sebelum barrier implisit di akhir `single` → urutan task1/task2 dan posisinya relatif terhadap "selesai mencetak" **tidak tentu**.
- Jadi **(a), (c), dan (d) semuanya mungkin**. Jika harus memilih satu, pilih **(d)** "Mulai selesai mencetak task2 task1" — itu output yang menunjukkan sifat task (dibuat sekarang, dieksekusi belakangan; contoh klasik dari materi OpenMP tasks).
</details>

**14. Variabel `__device__` tanpa `__shared__`/`__constant__` berada di…?**
<details><summary>Pembahasan</summary>

**(d) Global memory, lifetime = aplikasi.**
</details>

**15. Matriks 104×104, tile 16×16, warp 32. Perkalian matriks tiling → control divergence. Penurunan performa (tidak lebih dari)?**
<details><summary>Pembahasan</summary>

**Setup:** ⌈104/16⌉ = 7 tile per dimensi (6 × 16 = 96, tile terakhir hanya 8 elemen valid: indeks 96–103). Grid 7 × 7 = 49 blok × 8 warp = **392 warp**. Setiap warp = 2 baris × 16 kolom thread. Ada **7 fase**, tiap fase memuat 1 tile M dan 1 tile N → per warp 14 operasi load → total **392 × 14 = 5488** warp-load.

**Hitung warp-load yang divergen** (sebagian thread di luar batas, sebagian di dalam):
1. **Load M** (kolom M = fase·16 + tx): hanya di **fase terakhir** (kolom 96–111, 8 valid dari 16 di tiap baris) → setiap warp yang barisnya valid menjadi divergen.
   - 42 blok bukan baris-terakhir × 8 warp = 336
   - 7 blok baris-terakhir: hanya 4 warp yang barisnya valid (baris 96–103) → 28
   - Subtotal **364**.
2. **Load N** (kolom N = Col): divergen hanya di **7 blok kolom-terakhir** (Col 96–111, 8 valid).
   - Fase 1–6: 8 warp × 6 = 48 per blok; fase 7: hanya 4 warp yang baris N-nya valid → 4 → total 52 per blok × 7 = **364**.
   - Di blok lain, fase 7 tile N: warp 0–3 baris valid semua, warp 4–7 tidak valid semua → tidak divergen.
3. Total divergen = 364 + 364 = **728** dari 5488 → **728/5488 = 13,265% ≈ 13,26%** → **(a)**.

(Jika store P ikut dihitung: (728 + 52)/(5488 + 392) = 780/5880 = 13,265% → tetap **13,26%**.)
</details>

---

### 📘 UTS 2021/2022 (soal dari scan; pilihan yang tercentang di scan BUKAN kunci)

**1. SMT — pernyataan paling tepat?** (a) tiap clock cycle context switch beberapa thread dari instruksi yang sama saja; (b) … dari instruksi yang sama atau berbeda; (c) SMT tidak mendukung multiple issue; (d) SMT selalu meningkatkan kinerja meski hardware tidak mendukung multithreading; (e) hanya 2 yang benar.
<details><summary>Pembahasan</summary>

**(b).** Pada SMT, dalam **satu siklus** beberapa thread bisa mengeluarkan instruksi (sejenis maupun berbeda) ke unit-unit fungsional yang tersedia. (c) salah karena SMT justru **memanfaatkan** multiple issue; (d) salah karena SMT butuh dukungan hardware; (a) terlalu sempit.
</details>

**2. 18 node fully connected. Bisection width?**
<details><summary>Pembahasan</summary>

(p/2)² = 9² = **81 → (c)**. Setiap dari 9 node di satu sisi terhubung langsung ke 9 node di sisi lain.
</details>

**3. Snooping cache coherence — pernyataan yang tepat?**
<details><summary>Pembahasan</summary>

**(c) Nilai cache yang diubah oleh core di-broadcast lewat bus** (core lain menguping dan meng-invalidate salinannya). (d) adalah directory-based.
</details>

**4. Strongly scalable dicapai dengan cara…?**
<details><summary>Pembahasan</summary>

**(d) Menambah jumlah prosesor dan mempertahankan problem size** (dengan efisiensi tetap). (a) = definisi weakly scalable.
</details>

**5. Sumber daya: PC + beberapa workstation terhubung LAN. Library paling sesuai?**
<details><summary>Pembahasan</summary>

**(a) MPI** — mesin-mesin terpisah dengan memori masing-masing (distributed memory) → message passing. OpenMP/Pthreads hanya untuk satu mesin shared memory; CUDA/OpenCL untuk GPU.
</details>

**6. Linked list: kasus yang TIDAK bermasalah?**
<details><summary>Pembahasan</summary>

**Dua pemanggilan IsMember** (sama-sama hanya membaca).
</details>

**7. critical vs atomic `count++` — pernyataan yang benar?**
<details><summary>Pembahasan</summary>

**"Bila lingkungan & cakupan sama, keduanya memberikan hasil count yang sama."** (sama seperti UTS 2020/21 no. 8.)
</details>

**8. Kode mutex + counter + `pthread_cond_broadcast` / `pthread_cond_wait` digunakan untuk…?**
<details><summary>Pembahasan</summary>

**(d) Menunggu thread lain yang belum selesai memproses tugasnya** — ini **barrier** dengan condition variable. Bukan busy-waiting (thread tidur di `cond_wait`).
</details>

**9. Syarat worksharing `PARALLEL FOR`?**
<details><summary>Pembahasan</summary>

**(d) Data yang diproses tidak bergantung pada data lain** (tidak ada loop-carried dependency). Ukuran loop tidak harus kelipatan jumlah thread, indeks tidak harus mulai dari 1, data tidak harus integer.
</details>

**10. 4 thread, `#pragma omp single` di dalam `parallel`, print "Hello". Output?**
<details><summary>Pembahasan</summary>

**(d) "Hello" dicetak sekali saja oleh salah satu thread** (tidak harus thread 0 — itu `master`).
</details>

**11. Kernel konvolusi 2D dasar — agar lebih efisien?**
<details><summary>Pembahasan</summary>

Data yang **dipakai berulang** layak dipindah ke memori on-chip:
- Elemen **input** dibaca oleh hingga maskwidth² thread tetangga → reuse tinggi.
- **Mask** dibaca oleh **semua** thread → reuse tinggi.
- **Output** hanya ditulis sekali per elemen → tidak ada untungnya.

Jawaban: **"Membuat shared memory untuk menyimpan matriks input (in) dan mask yang digunakan dalam 1 blok thread"**. (Dalam praktik Kirk & Hwu, mask biasanya ditaruh di `__constant__` memory dan input di-tiling ke shared memory; di antara opsi yang tersedia, input + mask adalah yang paling mengurangi akses global memory.)
</details>

**12. Algoritma yang bisa dioptimalkan dengan memori lokal (cache CPU / shared memory GPU) memiliki karakteristik…?**
<details><summary>Pembahasan</summary>

**"Memiliki arithmetic density yang besar"** — artinya banyak operasi per data yang dimuat, sehingga data yang sudah ada di memori lokal **dipakai ulang** berkali-kali (contoh: perkalian matriks dengan tiling). Algoritma yang hanya butuh bandwidth besar tanpa reuse (penjumlahan vektor) atau yang sekadar data-parallel tidak otomatis diuntungkan. (Pilihan "data paralelism besar" yang tercentang di scan kurang tepat.)
</details>

**13. Cetak "This is iteration number 0" s.d. "9" dengan `printf(..., threadIdx.x)`. Pemanggilan yang benar?**
<details><summary>Pembahasan</summary>

**`loop<<<1, 10>>>();`** → 1 blok berisi 10 thread, threadIdx.x = 0..9. (`<<<10,1>>>` mencetak 0 sepuluh kali; `<<<5,2>>>` mencetak 0,1 lima kali.)
</details>

**14. Blok 256 thread, reduksi yang diperbaiki (`for (stride = blockDim.x; stride > 0; stride /= 2) { __syncthreads(); if (t < stride) partialSum[t] += partialSum[t+stride]; }`). Divergensi terjadi pada langkah ke-?**
<details><summary>Pembahasan</summary>

Urutan stride: 256 → 128 → 64 → 32 → **16** → 8 → 4 → 2 → 1.
- Stride ≥ 32: thread aktif adalah t = 0..stride−1, kelipatan 32 → warp penuh aktif atau penuh diam → **tidak divergen**.
- Stride **16**: warp 0 hanya setengah aktif → **divergensi pertama** (dan berlanjut di stride 8, 4, 2, 1, selalu hanya di 1 warp).

🟨 Stride 16 adalah **iterasi ke-5** jika dihitung dari 1 → jawaban ketat **(e) tidak ada yang benar**. Jika iterasi dihitung mulai 0 (stride 256 = langkah 0), stride 16 adalah langkah ke-**4** → **(c)**. Tulis alasanmu: "divergensi mulai saat stride = 16 (< ukuran warp 32)".
</details>

**15. Yang BUKAN tujuan custom data type MPI (`MPI_Type_create_struct`, `MPI_Type_vector`)?**
<details><summary>Pembahasan</summary>

**"Membuat penyimpanan data menjadi lebih kecil"** — derived datatype hanya **mendeskripsikan** tata letak data di memori; tidak mengecilkan penyimpanan. Tujuan sebenarnya: pengiriman lebih efisien, menghindari penyalinan manual, menghindari banyak pesan kecil terpisah, kode lebih rapi.
</details>

**16. Agar `if (threadIdx.x == 1023 && blockIdx.x == 255) printf("Success!")` tercetak, konfigurasi `<<<?, ?>>>`?**
<details><summary>Pembahasan</summary>

Butuh minimal **256 blok** (blockIdx.x 0..255) dan **1024 thread per blok** (threadIdx.x 0..1023) → **`<<<256, 1024>>>`**. (Urutan: jumlah blok dulu, lalu thread per blok; `<<<1024, 256>>>` tidak punya threadIdx.x = 1023.)
</details>

**17. Matriks 100×100, blok 16×16, kernel perkalian matriks dasar dengan `if (Row < Width && Col < Width)`. Berapa warp yang mengalami control divergence?** (7 / 13 / 54 / 56 / 104)
<details><summary>Pembahasan</summary>

Grid: ⌈100/16⌉ = 7 → 7 × 7 = 49 blok × 8 warp = 392 warp. Satu warp = 2 baris × 16 kolom. Blok terakhir di tiap dimensi mencakup indeks 96–111, yang valid hanya 96–99 (4 elemen).
- **Blok di kolom terakhir (bukan baris terakhir), 6 blok:** setiap baris warp punya 4 Col valid + 12 tidak valid → **8 warp divergen per blok → 48**.
- **Blok sudut (kanan bawah):** baris valid hanya ty 0–3 → warp 0 & 1 punya baris valid dengan Col campuran → **2 divergen**; warp 2–7 semua thread tidak valid → tidak divergen.
- **Blok di baris terakhir (bukan sudut):** warp 0–1 semua valid, warp 2–7 semua tidak valid → **0**.
- Total ketat = 48 + 2 = **50**.

🟨 50 tidak ada di pilihan. Opsi yang paling dekat & kemungkinan kunci penyusun soal adalah **56** = 7 blok × 8 warp di **seluruh kolom blok paling kanan** (menganggap semua warp di blok tepi kanan divergen). Di lembar jawaban, pilih 56 dan tuliskan analisis singkat di atas (bahwa secara ketat 50).
</details>

**18. Kode `x[i][j][k] = x[i][j][k-1] + x[i][j-1][k]*scale` dengan loop k (a) → j (b) → i (c). Solusi OpenMP paling efisien?**
<details><summary>Pembahasan</summary>

Dependensi: loop k (`k-1`) ❌, loop j (`j-1`) ❌, loop i ✅ bebas.
- parallel for di (a) atau (b) → **salah** (melanggar dependensi).
- parallel for di (c) → benar, tetapi fork/join (n−1)² kali.
- **"omp parallel pada (a) dan omp for pada (c)"** → benar **dan** tim thread dibuat sekali → **paling efisien** ✅ (pastikan j dan k `private`).
- "omp parallel pada (a) dan omp for pada (b)" (yang tercentang di scan) → **salah**, karena loop j membawa dependensi `x[i][j-1][k]`.
</details>

---

### 📘 UTS 2024/2025 (soal dari scan; coretan tangan di scan bukan kunci)

**Q1. 4 core, hyperthreaded (2 thread/core), 3 GHz, 1 FLOP/siklus, DRAM 1 word/siklus (1 word = 8 byte), L3 600 GB/s.**
(a) Bandwidth DRAM (GB/s)? (b) Kecepatan komputasi maksimum (FLOP/s)? (c) Program dengan arithmetic intensity 1 FLOP/byte, semua data di DRAM — kecepatannya? (d) Program yang sama, semua data di L3?
<details><summary>Pembahasan</summary>

(a) 1 word/siklus × 8 byte × 3·10⁹ siklus/s = **24 GB/s** (sama dengan angka "DRAM 24 GB/s" di grafik slide).
(b) Mengikuti model slide (hyperthread ikut dikali): 4 core × 2 thread × 3·10⁹ × 1 FLOP = **24·10⁹ FLOP/s = 24 GFLOP/s**. *(Jika hyperthread tidak dihitung karena berbagi unit fungsional: 12 GFLOP/s — sebutkan asumsimu.)*
(c) Batas memori = AI × BW = 1 × 24 GB/s = 24 GFLOP/s; batas komputasi = 24 GFLOP/s → kinerja = min(24, 24) = **24 GFLOP/s** (tepat di titik pertemuan).
(d) Batas memori = 1 × 600 = 600 GFLOP/s, tetapi CPU hanya mampu 24 GFLOP/s → **24 GFLOP/s** (compute bound). Coretan "600·10⁹" di scan salah: data cepat tidak membuat CPU melampaui puncaknya.
</details>

**Q2. (a) Berapa thread untuk `x++; p = x + 5; q = p + 3; c++;`? (b) Berapa thread pada kode dengan `fork();` lalu dua `pthread_create`?**
<details><summary>Pembahasan</summary>

(a) **2 thread**: `x++ → p = x + 5 → q = p + 3` adalah rantai dependensi (RAW) sehingga harus berurutan di satu thread; `c++` independen → thread kedua.
(b) `fork()` menghasilkan 2 proses; masing-masing menjalankan 1 main thread + 2 thread baru = 3 → total **6 thread** (atau 4 jika hanya menghitung thread hasil `pthread_create`; sebutkan keduanya).
</details>

**Q3. OpenMP: identifikasi masalah & perbaiki:**
```c
int sum = 0;
#pragma omp parallel for
for (int i = 0; i < 1000; i++) { sum += i; }
printf("Sum: %d\n", sum);
```
<details><summary>Pembahasan</summary>

`sum` shared dan diubah banyak thread tanpa perlindungan → **race condition** pada `sum += i` (read-modify-write tidak atomik) → hasil bisa lebih kecil dari seharusnya dan berbeda-beda tiap eksekusi.
Perbaikan: `#pragma omp parallel for reduction(+:sum)` → hasil pasti **499500**. Alternatif lebih lambat: `#pragma omp atomic` sebelum `sum += i;`.
</details>

**Q4. Program MPI menjumlahkan array 12 elemen (data = 1..12) dengan `MPI_Scatter(data, 3, …, local_data, 3, …)`, jumlah lokal, lalu rank 0 menerima dari rank 1..size−1 dengan `MPI_Recv`. (a) Output yang diharapkan? (b) Ada masalah? Saran perbaikan.**
<details><summary>Pembahasan</summary>

(a) Jika dijalankan dengan **4 proses**: P0 {1,2,3} = 6, P1 {4,5,6} = 15, P2 {7,8,9} = 24, P3 {10,11,12} = 33 → output **"Total sum is 78"**.

(b) Masalah:
1. **Inisialisasi MPI tidak lengkap**: `MPI_Init`, `MPI_Comm_rank(…, &rank)`, `MPI_Comm_size(…, &size)` tidak ada (hanya komentar) dan variabel `rank`, `size` tidak dideklarasikan.
2. **Hanya benar jika size = 4** (3 elemen × 4 proses = 12). Jika size = 2 hanya 6 elemen yang dijumlah (hasil 21); jika size > 4 Scatter membaca melewati batas `data[12]` → hasil acak/crash. Perbaikan: `local_n = 12/size` dan tangani sisa dengan `MPI_Scatterv`, atau validasi `size`.
3. **Penjumlahan manual di rank 0 tidak efisien** (p−1 penerimaan berurutan, O(p)) → ganti dengan `MPI_Reduce(&local_sum, &total_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);` yang memakai pohon (O(log p)) dan kodenya lebih ringkas.
</details>

**Q5. GPU: citra medis 8000×8000, operasi sama di tiap piksel (sharpening, kontras, thresholding). (a) 2 alasan cocok untuk GPU. (b) 1 contoh kasus yang tidak cocok untuk GPU.**
<details><summary>Pembahasan</summary>

(a) ① **Data parallelism masif**: 64 juta piksel, setiap piksel bisa ditangani **1 thread** dengan **instruksi yang sama** (SIMT) → memanfaatkan ribuan core GPU. ② Piksel **independen** (tidak ada dependensi antar iterasi, sedikit percabangan → minim divergence) dan operasinya **throughput-oriented** dengan akses memori teratur (coalesced) yang memanfaatkan bandwidth memori GPU yang besar.
(b) Contoh tidak cocok: algoritma **sekuensial/rekursif** dengan dependensi antar langkah (mis. menghitung Fibonacci, menelusuri linked list, parsing), program dengan **banyak percabangan** berbeda per data (divergence), atau **data kecil** yang biaya transfer PCIe-nya lebih besar dari waktu komputasinya.
</details>

---

### 📘 UTS 2025/2026

**1. Benar atau salah:**
<details><summary>Pembahasan</summary>

| | Pernyataan | Jawaban | Alasan |
|---|---|---|---|
| a | Pada shared memory, cache coherence menyebabkan overhead komunikasi | **Benar** | Invalidate/broadcast/pengambilan ulang cache line = lalu lintas tambahan di interconnect (diperparah false sharing) |
| b | SPMD tidak cocok untuk shared memory karena tidak memungkinkan komunikasi antar proses | **Salah** | SPMD dipakai juga di shared memory (thread menjalankan program sama, bercabang berdasarkan rank); komunikasi lewat variabel bersama |
| c | Amdahl's law menunjukkan peningkatan prosesor secara eksponensial dapat meningkatkan speedup "hampir serial" (kemungkinan salah ketik dari "hampir linear") | **Salah** | Amdahl justru menunjukkan speedup dibatasi 1/s berapa pun jumlah prosesornya; menambah prosesor memberi hasil yang makin kecil |
| d | Pipelining di ILP mempercepat eksekusi dengan mengerjakan tahap-tahap instruksi berbeda secara paralel | **Benar** | Throughput naik (latensi satu instruksi tetap) |
| e | Speculative execution di CPU tidak relevan di GPU karena fokus latency vs throughput berbeda | **Benar** | Spekulasi adalah teknik CPU untuk menekan latency satu thread; GPU memakai core sederhana dan menyembunyikan latency dengan berganti warp |
| f | Pada shared address space, akses memori bisa dilakukan tanpa mekanisme sinkronisasi | **Salah** | Akses bersamaan dengan minimal satu penulisan → race condition; butuh mutex/atomic/barrier, dsb. |
</details>

**2. `#pragma omp for schedule(static)` penjumlahan matriks `result[i][j] = A[i][j] + B[i][j]`.**
(a) 100×100: static 5 s, dynamic 8 s — mengapa? (b) Saran mempercepat. (c) 10000×10000: static 50 s, dynamic 35 s — mengapa? Adakah hubungan ukuran matriks dan penjadwalan?
<details><summary>Pembahasan</summary>

(a) Beban setiap baris **seragam dan kecil** (100 penjumlahan). `dynamic` tanpa chunk = **chunk 1**: setiap baris harus **diminta ke runtime** (sinkronisasi/antrean bersama) → 100 kali overhead untuk pekerjaan yang sangat kecil; tidak ada manfaat load balancing karena beban sudah seimbang; baris yang diproses satu thread juga tidak berurutan → lokalitas cache/prefetch lebih buruk. `static` membagi blok baris berurutan sekali di awal, tanpa overhead saat runtime.

(b) Gunakan **`schedule(static)`** untuk beban seragam; jika tetap ingin dynamic, **perbesar chunk** (mis. `schedule(dynamic, 25)`) atau pakai `guided`; untuk matriks kecil, kurangi jumlah thread atau jalankan serial (overhead fork/join bisa lebih besar dari kerjanya). Pastikan juga ada `#pragma omp parallel` (atau tulis `#pragma omp parallel for`) — `omp for` saja di luar region paralel hanya dijalankan 1 thread.

(c) Pada 10000×10000, setiap baris berisi 10.000 operasi → overhead dynamic per chunk menjadi **sangat kecil dibanding kerjanya**. Sementara itu program besar bersifat **memory bound** dan kecepatan thread bisa **tidak sama** (efek NUMA/lokasi memori, hyperthread yang berbagi core, rebutan bandwidth, gangguan OS). Dengan static, waktu total ditentukan **thread paling lambat**; dengan dynamic, thread yang lebih cepat mengambil lebih banyak baris → beban seimbang → lebih cepat (35 s vs 50 s).
**Hubungan:** ya — rasio overhead penjadwalan terhadap kerja per chunk bergantung pada ukuran masalah. Masalah kecil & seragam → static; masalah besar atau beban/kecepatan tidak seragam → dynamic/guided dengan chunk yang wajar.
</details>

**3. `x = f1(y); MPI_XXX_Send(/* kirim x */); x = f2(z);` — sebaiknya synchronous atau asynchronous?**
<details><summary>Pembahasan</summary>

**Synchronous/blocking.** Variabel `x` langsung ditimpa oleh `f2(z)` setelah perintah kirim. Jika pengiriman **asynchronous** (non-blocking, mis. `MPI_Isend`), fungsi kembali sebelum data dijamin terkirim/tersalin, sehingga yang terkirim bisa saja nilai `f2(z)` atau data rusak. Dengan pengiriman blocking (`MPI_Send`, yang kembali setelah buffer aman dipakai ulang, atau `MPI_Ssend` yang menunggu penerima mulai menerima), nilai `f1(y)` dijamin yang terkirim. (Jika tetap ingin asynchronous: panggil `MPI_Wait` sebelum `x = f2(z)`, atau kirim salinan `x`.)
</details>

**4. Berapa banyak `MPI_Send` dan `MPI_Recv` minimum untuk mengimplementasikan `MPI_Allreduce`?**
<details><summary>Pembahasan</summary>

Dengan p proses: **2(p − 1) Send dan 2(p − 1) Recv** (lihat T8 Resep 6).
- Reduksi ke satu proses: p − 1 pesan (pohon atau linear) → semua nilai tergabung.
- Broadcast hasil: p − 1 pesan → semua proses tahu hasilnya.
- Tidak bisa kurang: (1) agar **ada satu** proses yang mengetahui gabungan semua nilai, informasi dari p proses harus disatukan; setiap pesan paling banyak menyatukan dua "kelompok informasi", jadi butuh minimal p − 1 pesan. (2) Pada saat itu, p − 1 proses lainnya belum tahu hasil lengkap, sehingga masing-masing harus menerima minimal 1 pesan lagi → p − 1 pesan. Total ≥ 2(p − 1).
Catatan: jika yang dioptimalkan **jumlah langkah**, pola **butterfly** hanya butuh **log₂p langkah** tetapi total p·log₂p pesan (p = 8: 24 pesan, 3 langkah vs 14 pesan, 6 langkah).
</details>

**5. CUDA local maximum filter: lengkapi [A]–[I], race condition?, lokasi memori variabel.**
<details><summary>Pembahasan</summary>

(a)
```c
[A] __device__                                   // dipanggil dari kernel
[B] __global__                                   // kernel, dipanggil dari host
[C] blockIdx.x * blockDim.x + threadIdx.x
[D] cudaMemcpyHostToDevice
[E] (N + blockSize.x - 1) / blockSize.x          // = (N + 255)/256 = 4096 untuk N = 2^20
[F] d_input, d_output, N
[G] cudaMemcpyDeviceToHost
[H] cudaFree(d_input)
[I] cudaFree(d_output)
```
(Opsional: `cudaDeviceSynchronize()` setelah kernel; `free(input); free(output);` di host. Kode soal juga punya salah ketik `float *input, *d_input,` yang seharusnya diakhiri `;`.)

(b) **Tidak ada race condition**: setiap thread hanya **menulis** `output[i]` miliknya sendiri (indeks unik); `input` hanya **dibaca** (boleh dibaca banyak thread bersamaan). Catatan: `output[0]` dan `output[N-1]` tidak pernah ditulis (bukan race, tetapi nilainya tidak terdefinisi).

(c)
- `left`, `center`, `right` (dan `i`): variabel lokal skalar di kernel → **register** milik masing-masing thread (lifetime = thread).
- `input`, `output`: array hasil `cudaMalloc` → berada di **global memory** (DRAM GPU), dapat diakses semua thread, lifetime sampai `cudaFree`. (Pointer-nya sendiri dikirim sebagai parameter kernel.)
</details>

---
## Bagian III – Latihan Tambahan

> Soal baru dengan pola yang sama seperti soal UTS. Kerjakan dulu, lalu cocokkan.

**L1 (Amdahl).** 95% program bisa diparalelkan. Hitung speedup & efisiensi untuk 16 prosesor, dan speedup maksimum.
<details><summary>Jawaban</summary>

s = 0,05. S₁₆ = 1/(0,05 + 0,95/16) = 1/0,109375 ≈ **9,14**; E ≈ 9,14/16 ≈ **0,57**. S_maks = 1/0,05 = **20**.
</details>

**L2 (Scalability).** T_ser = n, T_par = n/p + p (overhead naik sebanding p). Apakah strongly/weakly scalable?
<details><summary>Jawaban</summary>

E = n/(p·(n/p + p)) = n/(n + p²).
- p → kp, n tetap: n/(n + k²p²) turun → **tidak strongly scalable**.
- p → kp, n → kn: kn/(kn + k²p²) = n/(n + kp²) masih turun → **tidak weakly scalable** dalam arti slide (n naik sebanding p).
- Efisiensi baru tetap jika n naik sebanding **p²** (n → k²n): k²n/(k²n + k²p²) = n/(n + p²). Jadi program scalable, tetapi ukuran masalah harus tumbuh lebih cepat dari p.
</details>

**L3 (Pipeline).** Pipeline 8 tahap, issue width 4, 1 tahap = 0,25 ns. Waktu untuk 4000 instruksi independen? CPI?
<details><summary>Jawaban</summary>

Siklus = 8 + (4000/4 − 1) = **1007** → waktu = 1007 × 0,25 ns = **251,75 ns**. CPI ≈ 1007/4000 ≈ 0,252 → mendekati **1/4**.
</details>

**L4 (Roofline).** 2 core tanpa HT, 2,5 GHz, 4 FLOP/siklus (vektor). DRAM 2 word/siklus, 1 word = 8 byte. Kinerja untuk AI = 0,125 FLOP/byte? Untuk AI = 1?
<details><summary>Jawaban</summary>

Puncak = 2 × 2,5·10⁹ × 4 = **20 GFLOP/s**. Bandwidth = 2 × 8 × 2,5·10⁹ = **40 GB/s**.
AI 0,125 → min(20; 0,125 × 40 = 5) = **5 GFLOP/s** (memory bound). AI 1 → min(20; 40) = **20 GFLOP/s** (compute bound).
</details>

**L5 (Bisection width).** Hitung untuk: hypercube 32 node, toroidal mesh 8×8, fully connected 12 node, ring 12 node.
<details><summary>Jawaban</summary>

Hypercube: 32/2 = **16**. Toroidal mesh 8×8: 2·8 = **16**. Fully connected: (12/2)² = **36**. Ring: **2**.
</details>

**L6 (Tracing MPI).** Apa output program ini dengan 4 proses?
```c
int send = rank, recv;
MPI_Sendrecv(&send, 1, MPI_INT, (rank+1)%size, 0,
             &recv, 1, MPI_INT, (rank-1+size)%size, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
printf("rank %d menerima %d\n", rank, recv);
```
<details><summary>Jawaban</summary>

Setiap proses mengirim rank-nya ke tetangga kanan dan menerima dari tetangga kiri (pergeseran ring). Output (urutan baris bisa acak): rank 0 menerima **3**, rank 1 menerima **0**, rank 2 menerima **1**, rank 3 menerima **2**. Tidak deadlock karena memakai `MPI_Sendrecv`.
</details>

**L7 (Memilih kolektif).** Setiap proses punya array lokal; setiap proses harus menormalkan datanya dengan nilai **maksimum global**. Fungsi apa yang dipakai? Bagaimana jika hanya rank 0 yang perlu mencetak maksimum?
<details><summary>Jawaban</summary>

Semua proses butuh hasilnya → `MPI_Allreduce(&local_max, &global_max, 1, MPI_DOUBLE, MPI_MAX, comm)`. Jika hanya rank 0 → `MPI_Reduce(..., MPI_MAX, 0, comm)`.
</details>

**L8 (Deadlock).** 3 proses; setiap proses memanggil `MPI_Ssend` ke `(rank+1)%3` lalu `MPI_Recv` dari `(rank+2)%3`. Apa yang terjadi? Perbaiki.
<details><summary>Jawaban</summary>

`MPI_Ssend` selalu menunggu sampai receive pasangan dimulai, tetapi semua proses sedang berada di Ssend → **deadlock** pasti. Perbaikan: `MPI_Sendrecv`; atau urutan bergantian — tetapi dengan jumlah proses **ganjil**, pola genap/ganjil biasa tidak cukup (rank 2 dan rank 0 sama-sama genap). Cara aman: rank 0 Recv dulu lalu Send, rank lain Send dulu lalu Recv (rantai terputus di satu titik), atau pakai `MPI_Isend`/`MPI_Irecv` + `MPI_Waitall`.
</details>

**L9 (Schedule).** (a) `schedule(static,2)`, 11 iterasi (0..10), 3 thread. (b) `schedule(static)` tanpa chunk, 10 iterasi, 4 thread (aturan GCC).
<details><summary>Jawaban</summary>

(a) T0: 0, 1, 6, 7; T1: 2, 3, 8, 9; T2: 4, 5, 10.
(b) q = 2, r = 2 → T0: 0–2, T1: 3–5, T2: 6–7, T3: 8–9.
</details>

**L10 (Race OpenMP).** Perbaiki:
```c
int count = 0;
#pragma omp parallel for
for (i = 0; i < n; i++) if (a[i] % 2 == 0) count++;
```
<details><summary>Jawaban</summary>

`count` shared → race. Perbaikan terbaik: `#pragma omp parallel for reduction(+:count)`. Alternatif: `#pragma omp atomic` sebelum `count++` (benar, tetapi lebih lambat karena setiap genap memicu operasi atomik).
</details>

**L11 (Dependensi loop).** Mana yang boleh langsung diberi `#pragma omp parallel for`?
(A) `a[i] = b[i] * c[i];` (B) `a[i] = a[i-1] + 1;` (C) `a[i] = a[i+1] * 2;` (D) `a[i] = a[i] * 2 + b[i];`
<details><summary>Jawaban</summary>

(A) ✅ independen. (B) ❌ RAW loop-carried (bisa ditulis ulang tanpa dependensi: `a[i] = a[0] + i;`). (C) ❌ anti-dependensi (WAR): secara serial iterasi i membaca a[i+1] **lama**; jika paralel, iterasi i+1 bisa lebih dulu menimpanya → hasil salah (solusi: baca dari salinan array). (D) ✅ setiap iterasi hanya memakai elemennya sendiri.
</details>

**L12 (Menghitung thread).** `fork(); fork(); pthread_create(&t, NULL, f, NULL); pthread_join(t, NULL);` — berapa thread total?
<details><summary>Jawaban</summary>

Dua `fork()` → 4 proses; setiap proses: 1 main thread + 1 thread baru = 2 → **8 thread** (4 jika hanya menghitung yang dibuat `pthread_create`).
</details>

**L13 (Warp & divergence).** Gambar 1000×1000, blok `dim3(32, 8)`, kernel dengan `if (Row < 1000 && Col < 1000)`. Hitung jumlah blok, total warp, dan warp yang divergen.
<details><summary>Jawaban</summary>

Grid x = ⌈1000/32⌉ = 32, grid y = ⌈1000/8⌉ = 125 → **4000 blok**; 256/32 = 8 warp/blok → **32.000 warp**. Dengan blockDim.x = 32, satu warp = **satu baris** 32 thread. Baris: 1000/8 = 125 tepat → tidak ada baris di luar batas. Kolom: blok kolom terakhir mencakup Col 992–1023, valid hanya 992–999 → setiap warp di kolom blok terakhir divergen: 125 blok × 8 warp = **1000 warp divergen**.
</details>

**L14 (Indeks CUDA).** `kernel<<<4, 64>>>`: indeks global thread ke-10 di blok ke-3? Total thread?
<details><summary>Jawaban</summary>

i = 3 × 64 + 10 = **202**. Total = 4 × 64 = **256** thread.
</details>

**L15 (Reduksi).** Blok 512 thread. (a) Reduksi naif `t % stride == 0`, stride = 8: berapa warp divergen? (b) Reduksi diperbaiki `t < stride`, stride = 64? stride = 8?
<details><summary>Jawaban</summary>

512 thread = 16 warp.
(a) Stride 8 → 4 thread aktif per warp → **16 warp** divergen.
(b) Stride 64 → warp 0–1 penuh aktif, sisanya penuh diam → **0**. Stride 8 → hanya warp 0, sebagian aktif → **1**.
</details>

**L16 (Shared memory).** `kernel<<<dim3(10,10), dim3(16,16)>>>` dengan `__shared__ float tile[16][16];`. Berapa salinan `tile` dan total memori shared yang dipakai seluruh grid?
<details><summary>Jawaban</summary>

1 salinan per blok → 10 × 10 = **100 salinan**; masing-masing 16 × 16 × 4 byte = 1 KB → total **100 KB** (tersebar di SM-SM).
</details>

**L17 (Kode host).** Kernel `scale(float *x, float a, int n)` mengalikan setiap elemen dengan a. N = 1.000.000, 512 thread/blok. Tulis kode host & isi kernel.
<details><summary>Jawaban</summary>

```c
__global__ void scale(float *x, float a, int n) {
    int i = blockIdx.x * blockDim.x + threadIdx.x;
    if (i < n) x[i] = a * x[i];                       // wajib cek batas!
}
int main() {
    int N = 1000000; size_t size = N * sizeof(float);
    float *h_x = (float*) malloc(size), *d_x;
    /* isi h_x */
    cudaMalloc((void**)&d_x, size);
    cudaMemcpy(d_x, h_x, size, cudaMemcpyHostToDevice);
    int threads = 512, blocks = (N + threads - 1) / threads;   // = 1954
    scale<<<blocks, threads>>>(d_x, 2.0f, N);
    cudaDeviceSynchronize();
    cudaMemcpy(h_x, d_x, size, cudaMemcpyDeviceToHost);
    cudaFree(d_x); free(h_x);
    return 0;
}
```
1954 × 512 = 1.000.448 thread → 448 thread terakhir tidak melakukan apa-apa berkat `if (i < n)`.
</details>

**L18 (Tiling).** Perkalian matriks dengan tile 8×8: akses global memory turun menjadi berapa? Mengapa butuh dua `__syncthreads()`?
<details><summary>Jawaban</summary>

Menjadi **1/8**. `__syncthreads()` pertama: memastikan seluruh tile selesai dimuat sebelum dipakai menghitung. Kedua: memastikan semua thread selesai memakai tile sebelum tile ditimpa oleh tile berikutnya.
</details>

---

## Bagian IV – Lembar Contekan 1 Halaman

### Rumus
| Rumus | Pakai untuk |
|---|---|
| S = T_ser/T_par; E = S/p | Speedup, efisiensi |
| Amdahl: S = 1/(s + (1−s)/p); maks 1/s | s = fraksi **serial** |
| Gustafson: S = p − s(p − 1) | Ukuran masalah membesar |
| T_par = T_ser/p + T_overhead | Scalability: cek E(n, kp) vs E(kn, kp) |
| Siklus pipeline = k + N/w − 1; CPI = 1/w | k tahap, w issue width |
| Waktu pesan = latency + n/bandwidth | Jaringan |
| Bisection: ring 2, mesh torus 2√p, hypercube p/2, fully connected (p/2)² | Interkoneksi |
| Kinerja = min(puncak FLOP/s, AI × bandwidth) | Roofline |
| Puncak = core × thread/core × GHz × FLOP/siklus; BW = word/siklus × byte × GHz | Roofline |
| Blok = ⌈N/threads⌉ = (N + T − 1)/T; warp/blok = threads/32 | CUDA |
| i = blockIdx.x·blockDim.x + threadIdx.x; idx = Row·width + Col | CUDA |
| Tiling T×T → akses global ÷ T | CUDA |

### Keputusan cepat
| Situasi | Jawaban |
|---|---|
| Akumulasi di loop OpenMP | `reduction(op:var)` |
| Update 1 variabel sederhana | `atomic` (lebih cepat) |
| Critical section kompleks | `critical` / lock |
| Banyak baca, jarang tulis | read-write lock (Pthreads) |
| Region berisi 1 loop saja | `#pragma omp parallel for` |
| Region berisi beberapa loop/kode lain | `parallel` + `for` terpisah |
| Beban iterasi seragam | `schedule(static)` |
| Beban tidak seragam | `dynamic`/`guided` (chunk wajar) |
| Hasil dibutuhkan semua proses | `MPI_Allreduce` / `MPI_Allgather` |
| Hasil cukup di root | `MPI_Reduce` / `MPI_Gather` |
| Data sama ke semua | `MPI_Bcast` |
| Potongan berbeda ke tiap proses | `MPI_Scatter` |
| Tukar data berpasangan tanpa deadlock | `MPI_Sendrecv` / urutan berlawanan |
| Kirim lalu buffer langsung diubah | send blocking/synchronous |
| Mesin-mesin terpisah di LAN | MPI |
| Satu mesin multicore | OpenMP / Pthreads |
| Data besar, operasi sama per elemen | GPU / CUDA |
| Variabel per blok GPU | `__shared__` (1 salinan per blok) |
| `__device__` tanpa qualifier lain | global memory, lifetime aplikasi |
| Variabel lokal di kernel | register |

### Jebakan favorit dosen
1. Soal memberi fraksi **paralel**, rumus Amdahl butuh fraksi **serial**.
2. Kolektif MPI dicocokkan berdasarkan **urutan pemanggilan**, bukan nama variabel.
3. `MPI_Send` bisa buffer **atau** block → Send–Send–Recv–Recv = **unsafe/deadlock**.
4. `critical` di dalam loop = program jadi **serial**.
5. `omp for` di luar region `parallel` = **1 thread**.
6. Loop dengan `a[i-1]` / `a[j-1]` di sisi kanan → **jangan** diparalelkan di loop itu.
7. `<<<blok, thread>>>` — urutan **blok dulu**.
8. Reduksi naif `t % stride` → divergen di **semua warp** begitu stride ≥ 2; reduksi `t < stride` → divergen hanya saat **stride < 32** dan hanya **1 warp**.
9. Warp yang **semua** thread-nya di luar batas **tidak** divergen.
10. `cudaMemcpy(tujuan, sumber, ...)` — **tujuan dulu**.

---

*Semoga sukses UTS-nya! 🍀 Jika ada soal yang pembahasannya ingin diperdalam (misalnya simulasi langkah demi langkah), tinggal tanyakan.*
