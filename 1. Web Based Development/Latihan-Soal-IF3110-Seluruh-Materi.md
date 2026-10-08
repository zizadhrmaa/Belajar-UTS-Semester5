# 🧪 Latihan Soal IF3110 – Pengembangan Aplikasi Berbasis Web

**Paket latihan setingkat UTS**, dengan dua format persis seperti UTS sebelumnya:
- **Bagian A – Pilihan Ganda** (format UTS 2020/2021, gaya Quizizz): **bisa lebih dari satu jawaban benar**. Pilih **semua** opsi yang benar.
- **Bagian B – Benar/Salah per pernyataan** (format UTS 2021/2022): setiap soal berisi 4 pernyataan yang masing-masing dijawab **B** atau **S**.

**Jumlah soal:** 94 soal pilihan ganda + 45 soal Benar/Salah (180 pernyataan), mencakup **seluruh 11 file materi** (01a sampai 06-02).

> **Cara pakai**
> 1. Kerjakan dulu tanpa melihat jawaban. Setiap jawaban tersembunyi di balik tulisan **"Lihat jawaban & alasan"**; klik untuk membukanya.
> 2. Label di setiap judul soal (misal `[02a HTTP]`) menunjukkan materi asalnya, supaya kamu tahu bagian catatan mana yang perlu diulang kalau salah.
> 3. **Kunci jawaban cepat** untuk mengoreksi sekaligus ada di bagian paling bawah.
> 4. Semua soal yang berisi kode **sudah dijalankan** untuk memastikan kuncinya benar: PHP 8.3, Node.js 22, Go 1.24, dan Chromium untuk HTML/CSS. Ditandai ✅.
> 5. Soal-soal ini **buatan baru** berdasarkan isi slide, bukan soal ujian asli. Tingkat kesulitannya disamakan dengan UTS 2020/2021 dan 2021/2022.

---

## 🗺️ Peta Cakupan Materi

| File materi | Bagian A (Pilihan Ganda) | Bagian B (Benar/Salah) |
|---|---|---|
| 01a – Introductory | – | B1 (1 soal) |
| 01b – Internet App Concept | A1–A8 (8 soal) | B2–B4 (3 soal) |
| 02a – HTTP Protocol | A9–A18 (10 soal) | B5–B11 (7 soal) |
| 02b – HTML, CSS & HTML5 | A19–A30 (12 soal) | B12–B16 (5 soal) |
| 03 – JavaScript | A31–A46 (16 soal) | B17–B21 (5 soal) |
| 03b – AJAX | A47–A51 (5 soal) | B22–B23 (2 soal) |
| 04a – Server-Side Scripting (Go & PHP) | A52–A62 (11 soal) | B24–B28 (5 soal) |
| 04b – Go for Web | A63–A65 (3 soal) | B29–B30 (2 soal) |
| 05 – State Handling & Authentication | A66–A77 (12 soal) | B31–B36 (6 soal) |
| 06-01 – REST API Design | A78–A90 (13 soal) | B37–B42 (6 soal) |
| 06-02 – GraphQL | A91–A94 (4 soal) | B43–B45 (3 soal) |

---

## 📑 Daftar Isi
- [Bagian A – Pilihan Ganda](#bagian-a--pilihan-ganda)
- [Bagian B – Benar/Salah](#bagian-b--benarsalah)
- [Kunci Jawaban Cepat](#-kunci-jawaban-cepat)

---

## Bagian A – Pilihan Ganda

> Petunjuk: pilih **semua** jawaban yang benar (bisa satu, bisa lebih).

---

### A1 · [01b Konsep Aplikasi Internet]
Berikut adalah karakteristik aplikasi Internet menurut materi kuliah:
- a. Concurrency
- b. Synchronize
- c. Stateless
- d. Exception handling

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b, d**

**Alasan:** Slide *Characteristics* menyebut tiga karakteristik: **Concurrency** (menangani banyak tugas/client sekaligus), **Synchronize** (koordinasi aktivitas dan pengaturan timing), dan **Exception Handling** (kegagalan pada satu user tidak mengganggu user lain). *Stateless* adalah salah satu **varian client-server**, bukan karakteristik aplikasi Internet.

</details>

---

### A2 · [01b Konsep Aplikasi Internet]
Pernyataan yang benar terkait sistem terdistribusi:
- a. Pada peer-to-peer, setiap komponen berperan sebagai client sekaligus server
- b. Pada client-server, peran client dan server selalu bertukar di setiap interaksi
- c. Pada stateful client-server, client dapat mengirim pesan berdasarkan konteks interaksi sebelumnya
- d. N-tier system memecah sebuah server menjadi beberapa tier (misalnya presentation, business, database)

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, c, d**

**Alasan:**
- a ✔: di P2P semua komponen setara, jadi masing-masing bisa meminta dan menyediakan layanan.
- b ✘: di client-server perannya **tetap**: client meminta, server melayani.
- c ✔: definisi *stateful*. Server menyimpan info interaksi, sehingga client tidak perlu mengulang konteks di setiap pesan.
- d ✔: definisi N-tier di slide *Other Flavour of Client-Server*.

</details>

---

### A3 · [01b Konsep Aplikasi Internet]
Pasangan layanan dan protokol aplikasi Internet yang benar:
- a. Directory service – SNMP
- b. Mengirim e-mail – SMTP
- c. Naming – DNS
- d. Remote terminal – telnet

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, c, d**

**Alasan:** Directory service memakai **LDAP**. **SNMP** adalah protokol untuk **network monitoring**. Pasangan lainnya sesuai dengan daftar di slide *Application Protocol using Internet*.

</details>

---

### A4 · [01b Konsep Aplikasi Internet]
Sebuah aplikasi chat desktop memakai **protokol buatan sendiri** di atas TCP dan berkomunikasi **langsung** dengan program server tanpa web server. Aplikasi ini termasuk:
- a. Internet-based application
- b. Web-based application
- c. Bukan aplikasi terdistribusi
- d. Aplikasi yang harus dijalankan di web browser

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:** Ciri Internet-based app: boleh memakai protokol aplikasi Internet apa saja **atau protokol sendiri**, server berkomunikasi **langsung** dengan client, dan bisa berupa aplikasi standalone. Web-based app wajib memakai **HTTP** dan berkomunikasi **lewat web server**. Aplikasi ini tetap terdistribusi (client dan server di tempat berbeda) dan tidak harus berjalan di browser.

</details>

---

### A5 · [01b Konsep Aplikasi Internet]
Teknik *web programming* yang kodenya dieksekusi **di sisi browser (client)**:
- a. CGI
- b. Server-side scripting
- c. Client-side scripting
- d. Plug-in

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c, d**

**Alasan:** **CGI** (web server menjalankan program, misalnya Perl/C) dan **server-side scripting** (PHP, ASP, JSP, Python) dieksekusi di **server**. **Client-side scripting** (JavaScript) dan **plug-in** (Applet, ActiveX, Flash) dieksekusi oleh **browser**.

</details>

---

### A6 · [01b Konsep Aplikasi Internet]
Diberikan URL: `https://shop.example.com:8443/produk/list?kategori=buku&page=2#ulasan`. Pernyataan yang benar:
- a. Port yang dituju adalah 8443
- b. Query string berisi 2 parameter
- c. Bagian `#ulasan` ikut dikirim ke server di dalam request line HTTP
- d. Path resource-nya adalah `/produk/list`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b, d**

**Alasan:**
- Struktur URL: `scheme://host:port/path?query#fragment`.
- Port **8443** ditulis eksplisit (a ✔).
- Query string `kategori=buku&page=2` berisi **2 parameter** yang dipisah `&` (b ✔).
- Path-nya `/produk/list` (d ✔).
- **Fragment (`#ulasan`) tidak dikirim ke server.** Fragment hanya dipakai browser untuk menggulir ke bagian halaman, sehingga request line-nya `GET /produk/list?kategori=buku&page=2 HTTP/1.1` (c ✘).

</details>

---

### A7 · [01b Konsep Aplikasi Internet]
Pada diagram *Typical Web App Processing* (Shklar), tahap **"Customizing content for presentation"** mencakup:
- a. Aggregation
- b. Syndication
- c. Personalization
- d. Authentication

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b, c**

**Alasan:** Diagram mencantumkan **Aggregation, Syndication, Personalization**, ditambah *final transformation* opsional (XSLT → HTML/WML/VoiceXML/SMIL). Autentikasi termasuk tahap **"Controlling access to the application"**, bukan tahap kustomisasi konten.

</details>

---

### A8 · [01b Konsep Aplikasi Internet]
Menurut Tabel 8.1 (*Selection of server processing modules*), URL seperti `http://host/.../modulename.php?...`, `.cfm?...`, atau `.asp?...` umumnya diproses oleh pendekatan:
- a. CGI
- b. Auxiliary processing modules (scripting, template, hybrid)
- c. J2EE Webapp
- d. Client-side scripting

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b**

**Alasan:** Pada *auxiliary processing modules*, server mendaftarkan modul pemroses untuk file dengan **ekstensi tertentu** (`.php`, `.cfm`, `.asp`), dan dukungannya bisa sudah tertanam (misal ASP di IIS). **CGI** memakai path seperti `/cgi-bin/script` atau ekstensi `.cgi`. **J2EE Webapp** memakai path `servlet/...` atau halaman `.jsp`. Client-side scripting tidak diproses oleh server.

</details>

---

### A9 · [02a HTTP]
Pada pesan HTTP (request maupun response), bagian **header** dipisahkan dari **body** oleh:
- a. Tanda titik koma (`;`)
- b. Header `Content-Length`
- c. Tag `<body>`
- d. Satu baris kosong (CRLF)

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d**

**Alasan:** Strukturnya: start line, beberapa baris header, **baris kosong (CRLF)**, lalu message body. `Content-Length` memberi tahu **panjang** body, bukan pemisahnya. `<body>` adalah tag HTML di dalam body, bukan bagian dari protokol HTTP.

</details>

---

### A10 · [02a HTTP]
Pasangan kelas status code HTTP dan artinya yang benar:
- a. 3xx – redirection
- b. 4xx – kesalahan pada request client
- c. 5xx – kesalahan di server
- d. 1xx – sukses

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b, c**

**Alasan:** Menurut slide *HTTP Response Status Code*: 1xx = **information**, 2xx = **success**, 3xx = redirection, 4xx = client request error, 5xx = server error. Kelas sukses adalah **2xx**, bukan 1xx.

</details>

---

### A11 · [02a HTTP]
Method HTTP yang **idempotent tetapi TIDAK safe**:
- a. GET
- b. PUT
- c. DELETE
- d. POST

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, c**

**Alasan:** Menurut tabel method di slide: GET itu safe dan idempotent. **PUT** dan **DELETE** tidak safe karena mengubah state, tapi **idempotent**: mengulangnya menghasilkan efek akhir yang sama. POST tidak safe dan tidak idempotent.

</details>

---

### A12 · [02a HTTP]
Arti response header `Cache-Control: no-cache` adalah:
- a. Response boleh disimpan, tetapi harus divalidasi ke server asal sebelum setiap pemakaian ulang
- b. Response tidak boleh disimpan oleh cache mana pun
- c. Response hanya boleh disimpan di browser, tidak di proxy/CDN
- d. Response dianggap segar selamanya

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:** Ini jebakan klasik. "Tidak boleh disimpan sama sekali" adalah arti **`no-store`** (b). Hanya di perangkat client adalah arti **`private`** (c). **`no-cache`** artinya boleh disimpan, tapi **wajib revalidasi** dulu sebelum dipakai ulang.

</details>

---

### A13 · [02a HTTP]
Browser mengirim request dengan header `If-None-Match: "v7"`, dan resource di server **belum berubah** (ETag-nya masih `"v7"`). Response yang paling tepat:
- a. `412 Precondition Failed`
- b. `200 OK` dengan body lengkap
- c. `204 No Content`
- d. `304 Not Modified` tanpa body

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d**

**Alasan:** Ini **revalidasi cache**. Server cukup menjawab **304 Not Modified** tanpa body, lalu browser memakai salinan yang sudah ada di cache. Status **412** dipakai untuk kondisi `If-Match` yang **gagal** saat update (lihat materi REST), bukan untuk cache yang masih valid.

</details>

---

### A14 · [02a HTTP]
Dua domain `toko-a.com` dan `toko-b.com` di-*resolve* ke **alamat IP dan port yang sama**. Server dapat membedakan situs mana yang diminta berdasarkan:
- a. Header `Host`
- b. Alamat IP tujuan
- c. Header `Referer`
- d. Header `User-Agent`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:** Inilah **virtual hosting** (contoh dua subdomain wordpress.com di slide). Karena IP dan port sama, satu-satunya pembeda adalah header **`Host`**. Itu sebabnya header `Host` wajib di HTTP/1.1.

</details>

---

### A15 · [02a HTTP]
Agar sebuah form dapat meng-upload **file foto** ke server, form tersebut harus memakai:
- a. `method="GET"`
- b. `method="POST"`
- c. `enctype="application/x-www-form-urlencoded"`
- d. `enctype="multipart/form-data"`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, d**

**Alasan:** Data biner hanya bisa dikirim di **body** (POST), karena GET hanya membawa data ASCII di URL. Upload file memakai **`multipart/form-data`**, di mana setiap bagian dipisahkan *boundary* (contoh 2 di slide *Form Processing*). `x-www-form-urlencoded` hanya untuk pasangan `nama=nilai` berupa teks.

</details>

---

### A16 · [02a HTTP]
Kelebihan HTTP/2 dibanding HTTP/1.1:
- a. Menghilangkan head-of-line blocking di level TCP ketika ada paket yang hilang
- b. Banyak stream (request) bisa di-*multiplex* dalam satu koneksi TCP
- c. Protokol biner, bukan teks
- d. Kompresi header dengan HPACK

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, c, d**

**Alasan:** HTTP/2 tetap berjalan di atas **satu aliran byte TCP**. Kalau satu paket TCP hilang, **semua stream ikut tertahan**. Masalah ini baru diatasi oleh **HTTP/3 (QUIC)** (slide *Why HTTP/3?*). Opsi lainnya adalah fitur HTTP/2.

</details>

---

### A17 · [02a HTTP]
Pernyataan yang benar tentang HTTP/3:
- a. Berjalan di atas QUIC yang dibawa dalam datagram UDP
- b. Mengubah semantik HTTP (method dan status code menjadi berbeda)
- c. Connection ID memungkinkan koneksi tetap hidup ketika alamat IP/port berubah
- d. Data 0-RTT aman dipakai untuk operasi apa pun, termasuk transfer uang

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, c**

**Alasan:**
- b ✘: semantik HTTP **tidak berubah**. Method, status, field, dan body tetap sama.
- d ✘: data 0-RTT **bisa di-replay**, jadi hanya boleh untuk operasi yang aman diulang.
- a dan c ✔ sesuai slide *HTTP/3 runs over QUIC* dan *Faster handshakes and migration*.

</details>

---

### A18 · [02a HTTP]
Sebuah request berisi header `Authorization: Basic dXNlcjpwYXNz`. Pernyataan yang benar:
- a. Nilai tersebut terenkripsi sehingga aman dikirim tanpa HTTPS
- b. Nilai tersebut adalah Base64 dari string `username:password` (di sini `user:pass`)
- c. Server yang meminta Basic Auth biasanya membalas `401` dengan header `WWW-Authenticate: Basic realm="..."`
- d. Kredensial cukup dikirim sekali; setelah itu server mengingatnya tanpa perlu dikirim lagi

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, c**

**Alasan:**
- `dXNlcjpwYXNz` jika di-decode dari Base64 menjadi `user:pass`. **Base64 adalah encoding, bukan enkripsi**, sehingga Basic Auth wajib memakai HTTPS (a ✘).
- Alur Basic Auth: server membalas 401 + `WWW-Authenticate`, lalu browser menampilkan pop-up login (c ✔).
- Karena HTTP stateless, browser **mengirim ulang** header `Authorization` di **setiap** request (d ✘).

</details>

---

### A19 · [02b HTML & CSS]
Berikut ini yang termasuk elemen **block-level** secara default:
- a. `<a>`
- b. `<span>`
- c. `<p>`
- d. `<div>`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c, d**

**Alasan:** Elemen block selalu mulai di baris baru dan mengambil lebar penuh; contoh di slide adalah `<p>` dan `<div>`. `<span>` dan `<a>` adalah elemen **inline**.

</details>

---

### A20 · [02b HTML & CSS]
Penulisan HTML berikut yang **benar**:
- a. `<img src="pusheen.gif"></img>`
- b. `<img src="pusheen.gif" />`
- c. `<ol><li>Satu</li><li>Dua</li></ol>` untuk daftar bernomor
- d. `<label for="fname">Nama</label>` dipasangkan dengan `<input id="fname" name="fname">`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, c, d**

**Alasan:** `<img>` adalah **void element** tanpa konten dan tanpa tag penutup, sehingga bentuk a dicoret di slide. `<ol>` = ordered list dan `<li>` = list item. Atribut `for` pada `<label>` harus sama dengan **`id`** input yang dituju.

</details>

---

### A21 · [02b HTML & CSS]
Keempat selector berikut sama-sama cocok dengan sebuah link `<a>` di dalam `<ul id="nav"><li class="item">`. Selector dengan **specificity tertinggi** adalah:
- a. `#nav .item a`
- b. `.item a`
- c. `ul#nav a`
- d. `a:hover`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a** ✅ Terverifikasi di Chromium.

**Alasan:** Hitung skornya (ID = 100, class/pseudo-class = 10, elemen = 1):
- `#nav .item a` = 100 + 10 + 1 = **111**
- `.item a` = 10 + 1 = 11
- `ul#nav a` = 1 + 100 + 1 = 102
- `a:hover` = 1 + 10 = 11

</details>

---

### A22 · [02b HTML & CSS]
Apa warna teks "Halo" pada kode berikut?
```html
<p class="note" id="info" style="color: green">Halo</p>
<style>
  #info   { color: blue; }
  .note   { color: red !important; }
  p.note  { color: black; }
</style>
```
- a. Hijau
- b. Biru
- c. Merah
- d. Hitam

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c (merah)** ✅ Terverifikasi di Chromium.

**Alasan:** Biasanya inline style (specificity 1000) menang. Tapi **`!important` mengalahkan bahkan inline style**, dan hanya bisa ditimpa oleh `!important` lain dengan specificity yang sama atau lebih tinggi. Di sini hanya `.note` yang memakai `!important`, jadi warnanya **merah**.

</details>

---

### A23 · [02b HTML & CSS]
Paragraf mana yang berwarna merah?
```html
<div class="box">
  <p>A</p>
  <section><p>B</p></section>
  <p>C</p>
  <p>D</p>
</div>
<style> .box > p + p { color: red; } </style>
```
- a. A
- b. B
- c. C
- d. D

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d** ✅ Terverifikasi di Chromium.

**Alasan:** `.box > p + p` artinya: `<p>` yang merupakan **anak langsung** `.box` **dan** **tepat didahului** oleh saudara `<p>`.
- A tidak punya saudara sebelumnya.
- B bukan anak langsung `.box` (ia cucu, di dalam section).
- C tepat didahului oleh `<section>`, bukan `<p>`.
- **D** tepat didahului oleh `<p>C</p>` → merah.

</details>

---

### A24 · [02b HTML & CSS]
Sebuah `div` memiliki style `width: 200px; padding: 10px; border: 5px solid black; margin: 20px;` dengan `box-sizing` default (`content-box`). Berapa **lebar kotak yang terlihat**, diukur dari tepi luar border kiri ke tepi luar border kanan?
- a. 200px
- b. 220px
- c. 230px
- d. 270px

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c (230px)** ✅ Terverifikasi (`offsetWidth = 230`).

**Alasan:** Ingat urutan box model: content → padding → border → margin. Dengan `content-box`, `width` hanya mengukur **content**. Lebar sampai border = 200 + 2×10 (padding) + 2×5 (border) = **230px**. Margin (2×20) tidak termasuk karena berada di luar border; kalau dihitung, total ruang yang dipakai menjadi 270px.

</details>

---

### A25 · [02b HTML & CSS]
Properti CSS yang membuat elemen **tidak terlihat tetapi tetap memakan ruang** di layout:
- a. `overflow: auto`
- b. `display: none`
- c. `display: flex`
- d. `visibility: hidden`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d**

**Alasan:** `display: none` menghapus elemen dari layout sehingga tidak memakan ruang. `visibility: hidden` menyembunyikan elemen, tapi **ruangnya tetap ada** (slide *display: none*).

</details>

---

### A26 · [02b HTML & CSS]
Pernyataan yang benar terkait layout CSS:
- a. Flexbox adalah metode layout satu dimensi (baris **atau** kolom)
- b. Grid adalah metode layout dua dimensi (baris **dan** kolom)
- c. Nilai default `grid-auto-flow` adalah `column`
- d. Sumbu yang tegak lurus terhadap *main axis* pada flexbox disebut *cross axis*

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b, d**

**Alasan:** Default `grid-auto-flow` adalah **`row`** ("*By default, grid will prioritize rows*"). Pernyataan lainnya sesuai slide *Flexbox*, *display: grid*, dan *flex model*.

</details>

---

### A27 · [02b HTML5]
Elemen semantik HTML5 yang tepat untuk **konten independen yang bisa berdiri sendiri**, seperti satu entri blog atau artikel berita:
- a. `<article>`
- b. `<section>`
- c. `<aside>`
- d. `<div>`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:** `<article>` = konten independen. `<section>` = bagian generik dokumen. `<aside>` = konten yang hanya sedikit berkaitan (sidebar). `<div>` bukan elemen semantik.

</details>

---

### A28 · [02b HTML5]
Berikut yang merupakan **tipe `<input>`** baru di HTML5:
- a. `currency`
- b. `range`
- c. `email`
- d. `date`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, c, d**

**Alasan:** Tipe input baru di slide *Web Forms 2.0*: datetime, datetime-local, date, month, week, time, number, range, email, url. Tipe `currency` **tidak ada**.

</details>

---

### A29 · [02b HTML5 / 05 State]
Data yang disimpan dengan mekanisme berikut akan **tetap ada setelah browser ditutup lalu dibuka kembali**:
- a. `sessionStorage`
- b. Variabel JavaScript biasa
- c. Cookie tanpa atribut `Expires`/`Max-Age`
- d. `localStorage`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d**

**Alasan:** `localStorage` bertahan **sampai dihapus**. `sessionStorage` hilang saat tab/sesi halaman ditutup. Variabel JS bahkan hilang saat reload. Cookie tanpa `Expires`/`Max-Age` adalah **session cookie** yang terhapus saat browser ditutup.

</details>

---

### A30 · [02b HTML5]
Pernyataan yang benar tentang **Service Worker**:
- a. Berjalan di background, pada thread terpisah dari UI browser
- b. Bertindak seperti proxy di antara browser dan jaringan
- c. Dapat langsung memanipulasi DOM halaman
- d. Memungkinkan web app tetap berfungsi saat offline lewat cache

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b, d**

**Alasan:** Service worker menangani tugas **non-UI** dan **tidak punya akses ke DOM**. Untuk mengubah tampilan, ia harus berkirim pesan ke halaman. Pernyataan lainnya sesuai slide *Offline Support for Web App*.

</details>

---
### A31 · [03 JavaScript]
Apa yang terjadi ketika kode berikut dijalankan?
```javascript
console.log(a);
var a = 5;
console.log(b);
let b = 6;
```
- a. Mencetak `5` lalu `6`
- b. Mencetak `undefined` lalu `6`
- c. Mencetak `undefined`, lalu terjadi `ReferenceError`
- d. Langsung terjadi `ReferenceError` di baris pertama

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c** ✅ Terverifikasi di Node.js.

**Alasan:** Deklarasi `var a` di-**hoist** ke atas, sehingga baris pertama mencetak `undefined` tanpa error. Variabel `let b` tidak bisa diakses sebelum baris deklarasinya, sehingga muncul **ReferenceError** di baris ketiga. Inilah alasan slide menyarankan `let`/`const` untuk mencegah perilaku hoisting.

</details>

---

### A32 · [03 JavaScript]
Apa yang terjadi pada kode berikut?
```javascript
const arr = [1, 2];
arr.push(3);
arr = [4];
```
- a. `push` berhasil (`arr` menjadi `[1, 2, 3]`), lalu `TypeError` di baris `arr = [4]`
- b. Error di baris `arr.push(3)` karena `arr` adalah const
- c. `arr` menjadi `[4]` tanpa error
- d. Tidak ada error sama sekali

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a** ✅ Terverifikasi (`TypeError: Assignment to constant variable.`).

**Alasan:** `const` melarang **re-assignment** (mengarahkan variabel ke nilai lain), tapi **isi array/objek tetap boleh diubah**. Contoh di slide: `topics.push("DOM")` diperbolehkan.

</details>

---

### A33 · [03 JavaScript]
Ekspresi mana yang bernilai `true`?
- a. `"5" == 5`
- b. `"5" === 5`
- c. `null == undefined`
- d. `null === undefined`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, c** ✅ Terverifikasi.

**Alasan:** `==` melakukan **type coercion**: `"5"` diubah jadi 5, dan `null == undefined` memang didefinisikan sama. `===` (strict) **tidak** melakukan konversi: string ≠ number, dan seperti di slide, *undefined !== null*. Saran slide: gunakan `===`/`!==`.

</details>

---

### A34 · [03 JavaScript]
Berapa nilai ekspresi `"3" * "4" + "1"`?
- a. `13`
- b. `"121"`
- c. `"341"`
- d. `NaN`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b** ✅ Terverifikasi.

**Alasan:** Evaluasi dari kiri. Operator `*` hanya untuk angka, jadi `"3" * "4"` = **12** (number). Lalu `12 + "1"`: salah satunya string, sehingga `+` menjadi **penggabungan teks** dan hasilnya `"121"`.

</details>

---

### A35 · [03 JavaScript]
Ekspresi `typeof` mana yang menghasilkan `"object"`?
- a. `typeof null`
- b. `typeof []`
- c. `typeof undefined`
- d. `typeof NaN`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b** ✅ Terverifikasi.

**Alasan:** `typeof null` adalah `'object'` (bug historis yang dipertahankan). Array juga objek, jadi `typeof []` adalah `'object'`; gunakan `Array.isArray` untuk mengecek array. `typeof undefined` adalah `'undefined'`, dan `typeof NaN` adalah `'number'`.

</details>

---

### A36 · [03 JavaScript]
Apa output kode berikut?
```javascript
function counter() {
  let n = 0;
  return () => ++n;
}
const c1 = counter();
const c2 = counter();
c1(); c1();
console.log(c1(), c2());
```
- a. `3 3`
- b. `1 1`
- c. `2 1`
- d. `3 1`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d** ✅ Terverifikasi.

**Alasan:** Ini **closure**. Setiap pemanggilan `counter()` membuat variabel `n` **baru** yang "diingat" oleh fungsi yang dikembalikan. `c1` dipanggil 3 kali sehingga mencetak 3. `c2` punya `n` sendiri dan baru dipanggil sekali sehingga mencetak 1 (mirip contoh `makeAdder` di slide).

</details>

---

### A37 · [03 JavaScript]
Perhatikan kode dari slide *Runtime Concept: Call Stack*:
```javascript
function foo(b) {
  const a = 10;
  return a + b + 11;
}
function bar(x) {
  const y = 3;
  return foo(x * y);
}
const baz = bar(7);
```
Berapa nilai `baz`, dan berapa jumlah **frame fungsi** paling banyak yang bertumpuk di call stack (tidak menghitung konteks global)?
- a. 21 dan 2
- b. 42 dan 2
- c. 42 dan 1
- d. 32 dan 2

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b** ✅ Terverifikasi di Node.js.

**Alasan:** `bar(7)` membuat frame pertama dengan x = 7 dan y = 3. `bar` lalu memanggil `foo(21)`, sehingga frame kedua ditumpuk di atasnya. Jadi maksimum ada **2 frame**. `foo` mengembalikan 10 + 21 + 11 = **42** lalu di-pop; setelah `bar` mengembalikan 42, stack kosong.

</details>

---

### A38 · [03 JavaScript]
Apa urutan output kode berikut?
```javascript
console.log("A");
setTimeout(() => console.log("B"), 0);
Promise.resolve().then(() => console.log("C"));
queueMicrotask(() => console.log("D"));
console.log("E");
```
- a. A B C D E
- b. A E B C D
- c. A E C D B
- d. A C D E B

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c** ✅ Terverifikasi.

**Alasan:**
1. Kode sinkron di call stack jalan dulu: **A, E**.
2. Setelah stack kosong, **semua microtask** dijalankan (callback Promise dan `queueMicrotask`) sesuai urutan masuk: **C, D**.
3. Baru setelah itu **macrotask** (callback `setTimeout`) dijalankan: **B**.

`setTimeout(..., 0)` tetap menunggu di task queue.

</details>

---

### A39 · [03 JavaScript]
Apa output kode berikut?
```javascript
Promise.resolve(1)
  .then(x => x + 1)
  .then(x => { throw new Error("x=" + x); })
  .then(x => console.log("then", x))
  .catch(e => { console.log("catch", e.message); return 10; })
  .then(x => console.log("akhir", x));
```
- a. `then 2` lalu `akhir 10`
- b. `catch x=2` saja
- c. `catch x=2` lalu `akhir 10`
- d. `catch x=1` lalu `akhir undefined`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c** ✅ Terverifikasi.

**Alasan:**
- Nilai 1 menjadi 2 di `then` pertama. `then` kedua **melempar error**, sehingga promise berikutnya **rejected** dan `then(x => console.log("then", x))` **dilewati**.
- `catch` menangkap error (mencetak `catch x=2`) dan **mengembalikan nilai 10**, sehingga promise hasil `catch` menjadi **fulfilled dengan nilai 10** (aturan "*returns a value → p gets fulfilled*" di slide).
- Karena itu `then` terakhir mencetak `akhir 10`.

</details>

---

### A40 · [03 JavaScript]
Urutan fase propagasi event DOM standar yang benar adalah:
- a. Capturing → Target → Bubbling
- b. Bubbling → Target → Capturing
- c. Target → Capturing → Bubbling
- d. Capturing → Bubbling → Target

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:** Event **turun** dari `window` ke elemen target (capturing), **sampai** di target (target phase), lalu **naik** kembali ke atas (bubbling).

</details>

---

### A41 · [03 JavaScript]
Diberikan `<ul id="list"><li>Satu</li><li>Dua</li><li>Tiga</li></ul>`. Ekspresi yang menghasilkan elemen `<li>Dua</li>`:
- a. `document.getElementById("list").children[1]`
- b. `document.getElementsByTagName("li")[2]`
- c. `document.querySelector("#list li:nth-child(2)")`
- d. `document.getElementById("list").firstElementChild.nextElementSibling`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, c, d** ✅ Terverifikasi di Chromium.

**Alasan:** Indeks mulai dari 0, jadi `children[1]` adalah elemen kedua (Dua). `getElementsByTagName("li")[2]` adalah elemen **ketiga** (Tiga). `:nth-child(2)` mulai dari 1, jadi elemen kedua. `firstElementChild` (Satu) diikuti `.nextElementSibling` menghasilkan Dua.

</details>

---

### A42 · [03 JavaScript]
Pernyataan yang benar tentang objek browser:
- a. `navigator`, `screen`, dan `history` adalah bagian dari standar ECMAScript
- b. Objek `location` dapat dipakai untuk mengetahui atau mengubah URL halaman
- c. Di level global, `var x = 5;` sama dengan `window.x = 5;`
- d. Objek `document` merepresentasikan halaman yang sedang ditampilkan

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, c, d**

**Alasan:** Slide *Browser object*: `window` adalah konteks eksekusi global, dan `document` (`window.document`) adalah halaman yang sedang dirender. Objek seperti `navigator`, `screen`, `history`, dan `location` "*Not standard, but provided in every browser*", jadi bukan bagian ECMAScript.

</details>

---

### A43 · [03 JavaScript]
Cara agar sebuah script baru dijalankan **setelah elemen-elemen HTML halaman selesai diproses**:
- a. Menambahkan atribut `defer` pada `<script src="...">`
- b. Meletakkan `<script>` di akhir dokumen, tepat sebelum `</body>`
- c. Membungkus kode dalam fungsi yang dipanggil lewat `window.onload` atau `<body onload="...">`
- d. Meletakkan `<script>` biasa (tanpa atribut) di dalam `<head>`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b, c**

**Alasan:** Slide *JavaScript Execution in HTML* menyebut atribut **defer** dan pemanggilan lewat **onload**. Tips di slide *How JavaScript works in Web* menyarankan menaruh script **di akhir HTML**. Script biasa di `<head>` dieksekusi **segera** dan **menghentikan parsing HTML**, sehingga elemen body belum ada saat script berjalan.

</details>

---

### A44 · [03 JavaScript]
Pernyataan yang benar terkait manajemen memori JavaScript:
- a. Programmer JavaScript harus memanggil `free()` untuk melepas memori
- b. Reference counting gagal membersihkan objek yang saling mereferensikan (*circular reference*)
- c. Variabel yang di-assign di dalam fungsi **tanpa** `var`/`let`/`const` bisa menyebabkan memory leak
- d. Engine JavaScript modern memakai algoritma mark-and-sweep

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, c, d**

**Alasan:** Memori dilepas **otomatis** oleh garbage collector, jadi tidak ada `free()` (a ✘). Mark-and-sweep menganggap objek yang **tidak terjangkau** sebagai sampah, sehingga circular reference yang tak terjangkau tetap dibersihkan. Itulah kelemahan reference counting yang membuatnya tidak dipakai lagi. Contoh `leakyVar` di slide menunjukkan variabel tanpa deklarasi berubah menjadi global (`window.leakyVar`) dan hidup sampai halaman ditutup.

</details>

---

### A45 · [03 JavaScript]
Pernyataan yang benar tentang TypeScript:
- a. Kesalahan tipe ditemukan saat compile, bukan saat runtime
- b. Browser dapat menjalankan file `.ts` secara langsung tanpa proses apa pun
- c. TypeScript mencakup seluruh sintaks JavaScript ditambah sintaks tipe
- d. Compiler/transpiler TypeScript menghasilkan kode JavaScript

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, c, d**

**Alasan:** Browser hanya menjalankan JavaScript. Kode TS harus **di-transpile** dulu ke JS. Contoh `squareOf('z')` di slide menghasilkan *Error TS2345* saat compile, sedangkan di JS hanya menghasilkan NaN diam-diam.

</details>

---

### A46 · [03 JavaScript]
Pasangan JavaScript engine dan pemakainya yang benar:
- a. V8 – Chrome dan Node.js
- b. SpiderMonkey – Mozilla Firefox
- c. JavaScriptCore – WebKit/Safari
- d. V8 – Mozilla Firefox

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b, c**

**Alasan:** Sesuai slide *Introduction*. Firefox memakai SpiderMonkey, bukan V8.

</details>

---

### A47 · [03b AJAX]
Pada objek `XMLHttpRequest`, nilai `readyState == 4` berarti:
- a. Request belum diinisialisasi
- b. Koneksi ke server sudah dibuat
- c. Request sedang diproses
- d. Request selesai dan response sudah siap

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d**

**Alasan:** 0 = belum diinisialisasi, 1 = koneksi dibuat, 2 = request diterima, 3 = sedang diproses, **4 = selesai & response siap**. Karena itu kondisi klasiknya adalah `this.readyState == 4 && this.status == 200`.

</details>

---

### A48 · [03b AJAX]
Sebuah halaman dibuka dari `https://app.itb.ac.id/home`. Request AJAX ke URL mana yang termasuk **same-origin** (tidak perlu CORS)?
- a. `https://app.itb.ac.id/api/data`
- b. `http://app.itb.ac.id/api/data`
- c. `https://api.itb.ac.id/data`
- d. `https://app.itb.ac.id:443/api/data`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, d**

**Alasan:** Origin = **skema + domain + port**.
- b berbeda **skema** (http vs https).
- c berbeda **domain** (subdomain api vs app).
- d menulis port 443 secara eksplisit, tapi **443 adalah port default HTTPS**, sehingga origin-nya sama dengan halaman.

</details>

---

### A49 · [03b AJAX]
Kode XHR mana yang mengirim `nama=Budi` **di body request** dengan format yang bisa dibaca PHP melalui `$_POST["nama"]`?
- a. `xhttp.open("POST", "save.php", true); xhttp.send("nama=Budi");`
- b. `xhttp.open("POST", "save.php", true); xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded"); xhttp.send("nama=Budi");`
- c. `xhttp.open("GET", "save.php?nama=Budi", true); xhttp.send();`
- d. `xhttp.open("POST", "save.php?nama=Budi", true); xhttp.send();`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b**

**Alasan:** Sesuai slide *Sending information with GET/POST*: POST harus menyertakan `setRequestHeader("Content-type", "application/x-www-form-urlencoded")`. Tanpa header itu (a), body string dikirim sebagai `text/plain` sehingga PHP **tidak** mengisi `$_POST`. Opsi c dan d menaruh data di **URL** (query string), sehingga data masuk ke `$_GET`, bukan body.

</details>

---

### A50 · [03b AJAX]
Pernyataan yang benar tentang Fetch API:
- a. `fetch(url)` mengembalikan Promise yang resolve menjadi objek `Response`
- b. Promise dari `fetch` otomatis **reject** jika server membalas status 404
- c. Untuk mengetahui apakah status termasuk 2xx, kita perlu memeriksa `response.ok`
- d. Fetch API adalah bagian dari standar ECMAScript

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, c**

**Alasan:** `fetch` hanya reject jika terjadi **kegagalan jaringan**. Response 404/500 tetap resolve. Itulah sebabnya contoh di slide memakai `if (!response.ok) throw new Error(...)`. XHR dan Fetch adalah **Web Platform API** (WHATWG/W3C), **bukan** ECMAScript.

</details>

---

### A51 · [03b AJAX]
Sebuah aplikasi membaca berita: saat pengguna sedang membaca halaman 1, aplikasi **diam-diam sudah mengambil data halaman 2**. Pola AJAX ini disebut:
- a. Multi Stage Download
- b. Periodic Refresh
- c. Predictive Fetch
- d. Submission Throttling

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c**

**Alasan:** *Predictive Fetch* berarti menebak apa yang akan dilakukan pengguna berikutnya lalu mengambil datanya lebih dulu.
- *Multi Stage Download*: halaman besar dimuat bertahap.
- *Periodic Refresh*: polling berkala.
- *Submission Throttling*: mengirim data besar dalam beberapa tahap.

</details>

---

### A52 · [04a Server-Side – Go]
Diberikan router Go 1.22+ berikut:
```go
mux := http.NewServeMux()
mux.HandleFunc("GET /login", app.loginPage)    // menulis "form"
mux.HandleFunc("POST /login", app.loginSubmit) // menulis "submit"
```
Apa respons untuk request `PUT /login`?
- a. `200` dengan isi "form"
- b. `200` dengan isi "submit"
- c. `404 Not Found`
- d. `405 Method Not Allowed`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d** ✅ Terverifikasi di Go 1.24 (`405`, header `Allow: GET, HEAD, POST`).

**Alasan:** Path `/login` **ada**, tapi tidak ada pattern untuk method PUT, sehingga ServeMux membalas **405 Method Not Allowed** (bukan 404). Routing memetakan **method + URL** ke fungsi, jadi URL yang sama dengan method berbeda bisa memanggil fungsi berbeda (seperti `main.go` di slide *Form Processing*).

</details>

---

### A53 · [04a Server-Side – Go]
Template Go `<p>{{ .Message }}</p>` di-render memakai paket `html/template` dengan data `Message = "<b>hai</b>"`. Hasil HTML-nya adalah:
- a. `<p><b>hai</b></p>` (teks "hai" tampil tebal)
- b. `<p>&lt;b&gt;hai&lt;/b&gt;</p>` (tag tampil sebagai teks biasa)
- c. `<p></p>` (data diabaikan)
- d. Terjadi error saat eksekusi template

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b** ✅ Terverifikasi di Go 1.24.

**Alasan:** `html/template` **otomatis meng-escape** data sesuai konteks HTML. Karakter `<` dan `>` diubah menjadi `&lt;` dan `&gt;`, sehingga data dari user tidak bisa menyisipkan tag atau script. Inilah perlindungan dari **XSS**.

</details>

---

### A54 · [04a Server-Side – Go]
Pada pola layout template Go di slide, sintaks yang dipakai `base.html` untuk menyisipkan template halaman (misal `home.html`) **sekaligus mengoper data** adalah:
- a. `{{ define "content" }}`
- b. `{{ .Content }}`
- c. `{{ template "content" . }}`
- d. `{{ include "home.html" }}`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c**

**Alasan:** `{{ template "nama" . }}` menyisipkan template bernama "nama", dan titik (`.`) mengoper data saat ini. `{{ define "content" }} ... {{ end }}` dipakai di `home.html` untuk **mendefinisikan** template itu. `{{ .Content }}` hanya menampilkan sebuah field data. `include` bukan sintaks template Go.

</details>

---

### A55 · [04a Server-Side – PHP]
Pada array `$x = array(3 => "a", "b", "k" => "c", "d");`, elemen `"d"` memiliki kunci:
- a. `2`
- b. `4`
- c. `5`
- d. `"k1"`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c** ✅ Terverifikasi (`[3]=>a [4]=>b [k]=>c [5]=>d`).

**Alasan:** Kunci otomatis = **kunci integer terbesar sebelumnya + 1**. `"a"` mendapat kunci 3, `"b"` mendapat 4, dan `"c"` mendapat kunci string `"k"` (tidak memengaruhi penomoran). Jadi `"d"` mendapat 4 + 1 = **5**. Lihat juga contoh `array(5 => 43, 32, 56, "b" => 12)` di slide.

</details>

---

### A56 · [04a Server-Side – PHP]
Apa output kode berikut?
```php
<?php
function hitung() {
    static $n = 0;
    $n++;
    return $n;
}
hitung(); hitung();
echo hitung();
?>
```
- a. `1`
- b. `2`
- c. `3`
- d. `0`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c** ✅ Terverifikasi.

**Alasan:** Variabel **static** hanya diinisialisasi **sekali** dan nilainya **diingat** antar-pemanggilan (lihat contoh `useColor()` di slide). Tiga kali pemanggilan menghasilkan 1, 2, 3.

</details>

---

### A57 · [04a Server-Side – PHP]
Apa output kode berikut?
```php
<?php
function tambah(&$a) { $a += 5; }
function tambah2($b) { $b += 5; }
$x = 1; $y = 1;
tambah($x);
tambah2($y);
echo "$x $y";
?>
```
- a. `6 6`
- b. `1 6`
- c. `1 1`
- d. `6 1`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d** ✅ Terverifikasi.

**Alasan:** `&$a` adalah **parameter by reference**: fungsi mengubah variabel **asli** `$x`, sehingga nilainya menjadi 6 (seperti `stripCommas(&$text)` di slide). `tambah2` menerima **salinan** (by value), sehingga `$y` tetap 1.

</details>

---

### A58 · [04a Server-Side – PHP]
Apa output kode berikut?
```php
<?php
$a = "hallo";
$$a = "world";
echo $a . " " . $$a . " " . $hallo;
?>
```
- a. `hallo world world`
- b. `hallo hallo hallo`
- c. `world world world`
- d. Error: variabel `$hallo` tidak didefinisikan

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a** ✅ Terverifikasi.

**Alasan:** **Variable variables**: `$$a` artinya variabel yang **namanya** adalah isi `$a`, yaitu `$hallo`. Jadi `$$a = "world"` sama dengan `$hallo = "world"` (slide *Variable*).

</details>

---

### A59 · [04a Server-Side – PHP]
Apa output kode berikut? (Variabel `$u` tidak pernah didefinisikan.)
```php
<?php echo (2 <=> 2) . ($u ?? "X"); ?>
```
- a. `1X`
- b. `-1X`
- c. `0` disertai warning *Undefined variable*
- d. `0X`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d** ✅ Terverifikasi (tanpa warning).

**Alasan:** **Spaceship operator** `<=>` menghasilkan -1, 0, atau 1; karena 2 = 2, hasilnya **0**. **Null coalescing** `??` mengembalikan operand kanan jika operand kiri **tidak ada atau null**, dan **tidak memunculkan warning** untuk variabel yang belum didefinisikan. Kedua operator ini adalah fitur PHP 7.

</details>

---

### A60 · [04a Server-Side – PHP]
Sebuah form `<form method="post" enctype="multipart/form-data">` berisi `<input type="text" name="nama">` dan `<input type="file" name="foto">`. Di `save.php`, data dapat diakses melalui:
- a. `$_POST["nama"]`
- b. `$_FILES["foto"]`
- c. `$_GET["nama"]`
- d. `$_POST["foto"]`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b**

**Alasan:** Field teks dari form POST ada di **`$_POST`**. File yang di-upload ada di **`$_FILES`** (berisi nama, tipe, ukuran, dan lokasi file sementara). `$_GET` hanya berisi parameter dari URL.

</details>

---

### A61 · [04a Server-Side – PHP]
Pernyataan yang benar tentang model eksekusi **Nginx** di slide:
- a. Bersifat event-driven, asynchronous, dan non-blocking
- b. Membuat satu thread/proses baru yang berat untuk setiap request
- c. Meneruskan request PHP ke application server, misalnya lewat FastCGI
- d. Memiliki master process yang memuat konfigurasi dan meluncurkan worker

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, c, d**

**Alasan:** Opsi b adalah ciri **Apache HTTPD klasik** ("*process-driven approach, create a new thread for each request*"). Pada Nginx, sedikit worker menangani banyak koneksi lewat multiplexing (epoll/kqueue). Request dinamis diteruskan ke backend seperti application server lewat FastCGI.

</details>

---

### A62 · [04a Server-Side – PHP]
Nilai berikut yang dianggap **falsy** (bernilai `false` dalam kondisi) di **PHP**:
- a. `"0"`
- b. `"0.0"`
- c. `""`
- d. `[]` (array kosong)

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, c, d** ✅ Terverifikasi.

**Alasan:** Di PHP, hanya string `"0"` dan `""` yang falsy di antara string. `"0.0"` adalah string yang **bukan** persis `"0"`, sehingga **truthy**. Array kosong falsy, sedangkan array berisi elemen (termasuk `[0]`) truthy. ⚠️ Bedakan dengan JavaScript, di mana `"0"` dan `[]` keduanya **truthy**.

</details>

---

### A63 · [04b Go for Web]
Server Go minimal di slide mendaftarkan `http.HandleFunc("/", ...)` (menulis "Hello, World!") dan `http.HandleFunc("/products", ...)` (menulis "Product list"), lalu menjalankan `http.ListenAndServe(":8080", nil)`. Apa respons untuk `GET /products/7`?
- a. `200` dengan isi "Hello, World!"
- b. `404 Not Found`
- c. `200` dengan isi "Product list"
- d. Server panic

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a** ✅ Terverifikasi di Go 1.24.

**Alasan:** Pattern `"/products"` (tanpa garis miring di akhir) hanya cocok dengan path **persis** `/products`. Pattern `"/"` berlaku sebagai **penangkap semua path** yang tidak cocok dengan pattern lain, sehingga `/products/7` (dan juga `/about`) ditangani handler `/`. Argumen `nil` di `ListenAndServe` berarti memakai **DefaultServeMux**, tempat `http.HandleFunc` mendaftarkan route.

</details>

---

### A64 · [04b Go for Web]
Pasangan model eksekusi server dan contohnya yang benar:
- a. Process-per-request – PHP klasik (Apache `mod_cgi`)
- b. Thread-per-request – Java (Tomcat), Python (WSGI)
- c. Goroutines (M:N) – Node.js
- d. Event loop – Node.js, nginx

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b, d**

**Alasan:** Model **goroutines M:N** (thread ringan yang di-multiplex ke thread OS) adalah milik **Go**. Node.js memakai **event loop** satu thread dengan I/O asinkron (tabel *Execution Models* di slide).

</details>

---

### A65 · [04b Go for Web]
Manakah yang **BUKAN** kelebihan Go untuk web menurut materi?
- a. Di-compile menjadi satu file binary sehingga deploy mudah
- b. Statically typed sehingga bug tertangkap saat compile
- c. Ekosistem library lebih besar daripada Node.js dan Python
- d. Goroutine yang ringan (~2KB) memungkinkan menangani banyak request bersamaan dengan murah

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c**

**Alasan:** Ekosistem yang **lebih kecil** dibanding Node/Python justru tercantum sebagai **trade-off** Go. Trade-off lainnya: kurang "batteries-included" dan error handling yang verbose (`if err != nil`).

</details>

---
### A66 · [05 State Handling]
Mekanisme penyimpanan di browser yang datanya **bertahan saat halaman di-reload**, tetapi **hilang ketika tab ditutup**:
- a. Variabel JavaScript
- b. Store Zustand/Redux di memori
- c. `sessionStorage`
- d. `localStorage`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c**

**Alasan:** Tabel *Browser storage lifetimes*:
- Variabel JS (termasuk store di memori seperti Zustand): **tidak bertahan** saat reload.
- `sessionStorage`: bertahan saat reload, masa hidupnya sepanjang **sesi halaman di tab itu**.
- `localStorage`: bertahan **sampai dihapus**.

</details>

---

### A67 · [05 State Handling]
Agar cookie `session_id` **tidak bisa dibaca oleh JavaScript** dan **hanya dikirim lewat HTTPS**, atribut yang harus dipasang adalah:
- a. `Path`
- b. `Secure`
- c. `SameSite`
- d. `HttpOnly`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, d**

**Alasan:** **`HttpOnly`** memblokir akses lewat `document.cookie`, sehingga cookie terlindung dari pencurian lewat XSS. **`Secure`** membuat cookie hanya dikirim lewat HTTPS. `SameSite` mengatur pengiriman cookie dalam konteks lintas situs, dan `Path` mengatur URL mana yang menerima cookie.

</details>

---

### A68 · [05 State Handling]
Cara terbaik membuat **session ID** setelah login berhasil:
- a. Memakai user ID secara langsung, misalnya `session_id=123`
- b. Memakai timestamp waktu login
- c. Membangkitkan 32 byte acak dengan generator acak kriptografis (`crypto/rand`) lalu meng-encode-nya ke hex
- d. Memakai hash MD5 dari username

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c**

**Alasan:** Slide menegaskan "*Use unpredictable IDs. Never hard-code a shared session ID.*" User ID, timestamp, dan hash username semuanya **bisa ditebak**, sehingga penyerang dapat membajak session orang lain. Kode di slide memakai `rand.Read(token[:])` dengan `token [32]byte`, lalu `hex.EncodeToString`.

</details>

---

### A69 · [05 State Handling]
Potongan kode mana yang **aman dari SQL injection**? (`input` berasal dari user.)
- a. `db.QueryRowContext(ctx, "SELECT nama FROM mhs WHERE nim = '" + input + "'")`
- b. `db.QueryRowContext(ctx, "SELECT nama FROM mhs WHERE nim = $1", input)`
- c. `db.QueryRowContext(ctx, fmt.Sprintf("SELECT nama FROM mhs WHERE nim = '%s'", input))`
- d. `$stmt = $pdo->prepare("SELECT nama FROM mhs WHERE nim = ?"); $stmt->execute([$input]);`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, d**

**Alasan:** **Prepared statement / placeholder** (`$1` di PostgreSQL/pgx, `?` di PDO) mengirim input **terpisah** dari perintah SQL, sehingga input selalu diperlakukan sebagai data. Opsi a dan c **menyambung string**. Input seperti `' OR '1'='1` akan mengubah logika query.

</details>

---

### A70 · [05 State Handling]
Sebuah halaman menampilkan **50 pesanan**. Kode mengambil daftar pesanan dengan 1 query, lalu untuk **setiap** pesanan menjalankan 1 query lagi untuk mengambil data customer-nya. Berapa jumlah query totalnya, dan berapa jumlahnya jika data customer diambil sekaligus dengan `WHERE id IN (...)`?
- a. 51 dan 2
- b. 50 dan 1
- c. 50 dan 2
- d. 51 dan 51

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:** Ini **masalah N+1**: 1 query daftar + N (50) query per baris = **51**. Dengan batching, cukup 1 query daftar + 1 query customer = **2**. Slide ORM memperingatkan "*Watch for N+1 queries*", dan masalah yang sama muncul di resolver GraphQL.

</details>

---

### A71 · [05 State Handling – Database]
Pada `sql.DB` di Go, pengaturan connection pool yang dipakai untuk **membatasi jumlah koneksi yang terbuka** adalah:
- a. `SetMaxIdleConns`
- b. `DB.Stats`
- c. `SetConnMaxLifetime`
- d. `SetMaxOpenConns`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d**

**Alasan:** Tabel *Connection Pooling*:
- `SetMaxOpenConns`: membatasi koneksi terbuka.
- `SetMaxIdleConns`: menjaga koneksi idle tetap siap.
- `SetConnMaxLifetime` / `SetConnMaxIdleTime`: "memensiunkan" koneksi berdasarkan umur atau lama menganggur.
- `DB.Stats`: mengamati pemakaian.

Membuka koneksi baru untuk setiap request itu mahal, karena itu dipakai pool.

</details>

---

### A72 · [05 Authentication]
Perhatikan dua skenario: (i) pengunjung **tanpa login** membuka `/grades`; (ii) user yang **sudah login sebagai student 123** membuka nilai milik student 456. Status yang tepat untuk (i) dan (ii) berturut-turut:
- a. 401 dan 403
- b. 403 dan 401
- c. 401 dan 401
- d. 404 dan 500

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:** **401 Unauthorized** = autentikasi gagal atau belum dilakukan ("kamu siapa?"). **403 Forbidden** = sudah terautentikasi, tapi **tidak punya izin** untuk resource ini. Contoh di slide: `if grade.StudentID != sess.UserID { ... StatusForbidden }`.

*(Catatan materi REST: sebuah API boleh juga memilih membalas **404** untuk skenario (ii) sebagai **concealment policy**, supaya keberadaan data orang lain tidak bocor. Tapi di antara pilihan yang ada, jawaban yang tepat adalah a.)*

</details>

---

### A73 · [05 Authentication]
Cara menyimpan password user yang sesuai praktik baik di slide:
- a. Plaintext, karena database sudah diberi password
- b. Hash MD5 tanpa salt
- c. Hash SHA-256 biasa tanpa salt
- d. Adaptive password hash (misal Argon2id) dengan salt unik per user, memakai library yang terpelihara

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d**

**Alasan:** Slide *Basic password verification*: buat **salt unik**, hitung **adaptive password hash** (Argon2id, scrypt, PBKDF2), simpan hash beserta parameternya, dan "*Never store plaintext*". MD5/SHA-256 biasa terlalu **cepat**, sehingga mudah ditebak secara brute force.

</details>

---

### A74 · [05 Authentication]
Langkah yang harus dilakukan server untuk **memvalidasi** sebuah JWT:
- a. Memeriksa tanda tangan (signature) dengan kunci tepercaya
- b. Memastikan algoritma (`alg`) termasuk yang diizinkan
- c. Memeriksa issuer (`iss`), audience (`aud`), dan expiry (`exp`)
- d. Cukup men-decode payload Base64url, karena isinya terenkripsi dan hanya bisa dibaca server

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b, c**

**Alasan:** Slide *Decoding and validating a JWT*: decoding bisa dilakukan **siapa saja** yang memegang token, karena payload **tidak terenkripsi**, hanya ditandatangani. "*Parsing JSON does not authenticate a user.*" Validasi memerlukan pengecekan signature, algoritma, klaim `iss`/`aud`/`exp`, dan *token profile*, sebaiknya dengan validator yang terpelihara.

</details>

---

### A75 · [05 Authentication]
Dalam OpenID Connect, token yang **ditujukan untuk resource server/API** agar aplikasi bisa mengakses data sesuai izin adalah:
- a. ID token
- b. Access token
- c. Refresh token
- d. Session ID cookie

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b**

**Alasan:**
- **ID token**: dibaca **aplikasi client**, sebagai bukti tentang autentikasi user.
- **Access token**: dibaca **resource server/API**, untuk akses sesuai izin.
- **Refresh token**: dikirim ke **authorization server**, untuk mendapatkan access token pengganti.

Slide menegaskan "*An ID token is not a substitute for an API access token.*"

</details>

---

### A76 · [05 Authentication]
Urutan yang benar pada alur OAuth (authorization code) di slide *OAuth – An Overview*:
- a. Request token → User login & consent → Authorization code → Exchange code for token → Token response → Use token to call API
- b. User login & consent → Token response → Authorization code → Use token to call API
- c. Exchange code for token → Request token → User login & consent → Use token to call API
- d. Use token to call API → Request token → Authorization code → Token response

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:** Aplikasi meminta otorisasi, lalu user login di **provider** (bukan di aplikasi) dan memberi persetujuan. Provider mengembalikan **authorization code**, aplikasi **menukarnya** dengan token, menerima token, lalu memakainya untuk memanggil API. Dengan cara ini password user **tidak pernah** dibagikan ke aplikasi pihak ketiga.

</details>

---

### A77 · [05 State Handling]
Pada transaksi berikut, apa fungsi klausa `FOR UPDATE`?
```go
tx, _ := db.BeginTx(ctx, nil)
defer tx.Rollback()
tx.QueryRowContext(ctx, `SELECT stock FROM products WHERE id = $1 FOR UPDATE`, id).Scan(&stock)
if stock <= 0 { return errors.New("Out of Stock") }
tx.ExecContext(ctx, `UPDATE products SET stock = stock - 1 WHERE id = $1`, id)
return tx.Commit()
```
- a. Membuat query berjalan lebih cepat dengan index
- b. Otomatis melakukan COMMIT setelah SELECT
- c. Menghapus baris setelah dibaca
- d. Mengunci baris produk tersebut sampai transaksi selesai, sehingga transaksi lain yang ingin mengubahnya harus menunggu

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d**

**Alasan:** `FOR UPDATE` **mengunci baris** yang dibaca, sehingga dua pembeli tidak bisa sama-sama membaca stok = 1 lalu sama-sama menguranginya (*race condition*). Ini berkaitan dengan karakteristik "synchronize" aplikasi Internet. `defer tx.Rollback()` memastikan perubahan dibatalkan jika fungsi keluar sebelum `Commit`.

</details>

---

### A78 · [06-01 REST API Design]
Urutan **empat lapis desain API** menurut Arnaud Lauret yang benar adalah:
- a. Batasan dunia nyata → Kemudahan pakai → Pekerjaan yang benar → Proses yang terus membaik
- b. Melakukan pekerjaan yang benar → Ramah pengguna & interoperable → Mempertimbangkan batasan dunia nyata → Proses desain yang beralasan & terus membaik
- c. Ramah pengguna → Keamanan → Versioning → Dokumentasi
- d. Implementasi → Pengujian → Deployment → Desain

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b**

**Alasan:** Slide *Layered design approach*: "*Four layers: the right job, usability, real-world constraints, and continuous improvement.*" Lapis 3 (batasan dunia nyata) mencakup keamanan, efisiensi, konteks teknis, backward compatibility, dan ekstensibilitas.

</details>

---

### A79 · [06-01 REST API Design]
Kolom-kolom yang terdapat pada **Capabilities Canvas** (Lauret, Figure 2.3):
- a. User dan Use case
- b. Input, Success, dan Failure
- c. Database schema
- d. Step dan Operation

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, b, d**

**Alasan:** Kolom canvas: **USER, USE CASE, STEP, INPUT, SUCCESS, FAILURE, OPERATION**. Lapis 1 menekankan "*capabilities before HTTP paths*": pahami dulu kebutuhan user. Skema database adalah detail implementasi internal dan sebaiknya tidak menentukan kontrak API.

</details>

---

### A80 · [06-01 REST API Design]
Desain path REST mana yang **baik** menurut aturan penamaan di slide?
- a. `POST /createOrder`
- b. `GET /orders/O55`
- c. `GET /customers/C7/orders`
- d. `GET /products/travel-mug` (memakai nama produk yang bisa diedit seller)

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b, c**

**Alasan:**
- a ✘: memakai **kata kerja/aksi** di URI. Seharusnya `POST /orders`.
- b ✔: item yang bisa dialamatkan secara independen diberi path pendek dan stabil.
- c ✔: nesting yang wajar ("*pesanan milik customer C7*").
- d ✘: rapuh, karena nama produk bisa berubah. Gunakan **identifier stabil** seperti `/products/P101`.

</details>

---

### A81 · [06-01 REST API Design]
Request `POST /loans` berhasil membuat peminjaman buku baru. Response yang paling sesuai kontrak di slide:
- a. `201 Created` dengan data loan dan header `Location` ke resource baru
- b. `200 OK` tanpa header tambahan
- c. `204 No Content`
- d. `202 Accepted`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:** Tabel *Layer 1: resources and HTTP semantics*: "Borrow a book – POST /loans – **201 with loan and Location**". Status 202 dipakai jika pemrosesan **belum selesai** (operasi asinkron), dan 204 jika memang tidak ada isi response.

</details>

---

### A82 · [06-01 REST API Design]
Consumer A dan B sama-sama membaca `GET /owners/123` dan mendapat `ETag: "z567dff"`. A lebih dulu mengupdate data. Kemudian B mengirim `PUT /owners/123` dengan `If-Match: "z567dff"`. Response untuk B:
- a. `200 OK`, dan data A tertimpa
- b. `304 Not Modified`
- c. `409 Conflict` tanpa pengecekan header
- d. `412 Precondition Failed`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d**

**Alasan:** Setelah update dari A, versi data (ETag) berubah. Kondisi `If-Match` dari B **tidak cocok lagi**, sehingga server menolak dengan **412 Precondition Failed**. Ini mencegah *lost update*. B harus membaca ulang data lalu menyelesaikan konfliknya (Lauret, Figure 12.5).

</details>

---

### A83 · [06-01 REST API Design]
Request berikut dikirim ke produk yang semula berisi `{"name":"Travel Mug","description":"Insulated","active":true}`:
```
PATCH /seller-products/P101
Content-Type: application/merge-patch+json

{"name":"Larger Travel Mug"}
```
Pernyataan yang benar:
- a. Hanya field `name` yang berubah; `description` dan `active` tetap
- b. Field `description` dan `active` terhapus karena tidak dikirim
- c. Jika body yang sama dikirim dengan `PUT`, efeknya juga hanya mengubah `name`
- d. `application/merge-patch+json` menandakan body berisi instruksi perubahan berformat JSON Merge Patch

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, d**

**Alasan:** "*PATCH serves as a set of instructions for modifying a resource, whereas PUT represents a **complete replacement**.*" Dengan PATCH merge-patch, hanya field yang dikirim yang berubah. Dengan **PUT**, body menjadi representasi **baru secara utuh**, sehingga field yang tidak dikirim dianggap tidak ada lagi. Karena itu c salah.

</details>

---

### A84 · [06-01 REST API Design]
Pencarian valid `GET /books?q=xyz` tidak menemukan buku apa pun. Response yang paling tepat:
- a. `200 OK` dengan body `{ "items": [] }`
- b. `404 Not Found`
- c. `200 OK` dengan body `"No books"`
- d. `500 Internal Server Error`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:** Slide *Why design an API?*: mengembalikan string "No products" membuat client harus menangani dua tipe response yang berbeda. Hasil kosong yang dapat diprediksi adalah **koleksi kosong** dengan bentuk yang sama: `{ "items": [] }`. Pencarian yang valid tidak gagal, jadi bukan 404 atau 500.

</details>

---

### A85 · [06-01 REST API Design]
Seorang client mengirim terlalu banyak request melebihi kuota. Response yang dianjurkan:
- a. `400 Bad Request`
- b. `403 Forbidden`
- c. `429 Too Many Requests` dengan header `Retry-After`
- d. `503 Service Unavailable` tanpa keterangan

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c**

**Alasan:** Slide *Efficiency, quotas, and HTTP caching*: "Request quota – **429 with Retry-After** – follow the documented retry policy". Di APISIX, plugin `limit-count` bisa dikonfigurasi untuk menolak dengan 429.

</details>

---

### A86 · [06-01 REST API Design]
Pembuatan laporan tahunan butuh waktu beberapa menit. Desain API yang sesuai materi:
- a. Menahan koneksi sampai laporan selesai lalu membalas 200
- b. Membalas `202 Accepted` beserta URI *job resource* yang bisa di-polling, atau memanggil callback/webhook saat selesai
- c. Membalas `201 Created` padahal laporan belum dibuat
- d. Membalas `500` lalu meminta client mengulang terus

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b**

**Alasan:** Slide *Long operations, callbacks, and events*: **202 Accepted** berarti pemrosesan belum selesai. Sebuah **job resource** bisa menampilkan progres dan hasil akhir, dan client dapat **polling** status atau menerima **callback/webhook**. Format event bisa memakai standar **CloudEvents**.

</details>

---

### A87 · [06-01 REST API Design]
Perubahan berikut yang termasuk **breaking change** bagi consumer yang sudah ada:
- a. Mengganti nama field `sts` menjadi `status`
- b. Menambah kolom baru di database internal tanpa mengubah response API
- c. Mengubah `price` dari angka (`25.00`) menjadi objek (`{"amountMinor":2500,"currency":"USD"}`)
- d. Mewajibkan field input baru yang sebelumnya tidak ada

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a, c, d**

**Alasan:**
- a: kode lama tidak menemukan `sts`.
- c: client yang mengharapkan angka akan error. Itulah sebabnya contoh di slide membuat `/v2`.
- d: request lama menjadi tidak valid.
- b ✘: "*A new database column or deployment number does not require a public API version*". Refactor penyimpanan juga tidak mengubah API.

</details>

---

### A88 · [06-01 REST API Design]
Pada file OpenAPI, bagian yang dipakai untuk menentukan **alamat server API dan base URL** (misal production dan staging) adalah:
- a. `info`
- b. `servers`
- c. `paths`
- d. `openapi`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b**

**Alasan:**
- `openapi`: versi OAS (misal `3.0.0`).
- `info`: judul, deskripsi, versi API, kontak, lisensi.
- **`servers`**: URL server dan base URL.
- `paths`: endpoint dan operasinya.

</details>

---

### A89 · [06-01 REST API Design – API Management]
Pada Apache APISIX, komponen yang **menentukan ke mana trafik yang cocok diteruskan** (alamat backend), dan plugin yang dipakai untuk **memeriksa API key**, berturut-turut adalah:
- a. Route dan `limit-count`
- b. Upstream dan `key-auth`
- c. Consumer dan `key-auth`
- d. Plugin dan `limit-count`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b**

**Alasan:**
- **Route** mencocokkan request yang masuk.
- **Upstream** menentukan **tujuan** trafik.
- **Consumer** merepresentasikan pemanggil API.
- **Plugin** menerapkan perilaku tambahan, misalnya **`key-auth`** (cek API key) dan **`limit-count`** (rate limit).

</details>

---

### A90 · [06-01 REST API Design]
Client AJAX ingin menerima representasi **JSON** dari `GET /products/P101`. Header yang dikirim client, dan header yang dipakai server untuk menyatakan format body yang dikirim, berturut-turut adalah:
- a. `Content-Type: application/json` dan `Accept: application/json`
- b. `Host: application/json` dan `Server: application/json`
- c. `Accept: json` dan `Format: json`
- d. `Accept: application/json` dan `Content-Type: application/json`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: d**

**Alasan:** Ini **content negotiation** (prinsip *representation oriented*). Client menyatakan format yang **diinginkan** lewat **`Accept`**, sedangkan server menyatakan format yang **dikirim** lewat **`Content-Type`** dengan nilai berupa MIME type.

</details>

---

### A91 · [06-02 GraphQL]
Halaman produk menerima `sellerId` dari endpoint produk, lalu **harus mengirim request lain** untuk mengetahui nama seller. Masalah ini disebut:
- a. Under-fetching
- b. Over-fetching
- c. N+1 query
- d. Introspection

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:**
- **Under-fetching**: data yang diterima **kurang**, sehingga perlu request tambahan.
- **Over-fetching**: menerima representasi besar padahal hanya sebagian field yang dipakai.
- **N+1**: masalah performa **di backend** (resolver), bukan masalah jumlah request client.
- **Introspection**: fitur untuk menemukan skema.

</details>

---

### A92 · [06-02 GraphQL]
Field GraphQL bertipe `[Post!]` berarti:
- a. List wajib ada, dan setiap item wajib ada
- b. List boleh null, tetapi jika ada, setiap item tidak boleh null
- c. List wajib ada, tetapi item boleh null
- d. Hanya satu Post yang wajib ada

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: b**

**Alasan:** Tanda `!` berlaku pada apa yang ada **tepat di depannya**. `Post!` artinya setiap item tidak boleh null. Tidak ada `!` di luar kurung siku, sehingga list-nya sendiri **boleh null**. Bandingkan dengan `[Product!]!` di slide, yang berarti list dan semua item wajib ada.

</details>

---

### A93 · [06-02 GraphQL]
Sebuah query mengembalikan **30 produk** beserta `seller { name }`. Jika setiap resolver `Product.seller` melakukan lookup sendiri, berapa jumlah lookup backend, dan berapa jumlahnya jika ID seller di-*batch*?
- a. 30 dan 1
- b. 1 dan 1
- c. 31 dan 2
- d. 31 dan 31

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: c**

**Alasan:** Tanpa batching: 1 lookup produk + 30 lookup seller = **31** (masalah N+1, seperti contoh "1 + 20" di slide). Dengan batching: 1 lookup produk + 1 lookup semua seller sekaligus = **2**.

</details>

---

### A94 · [06-02 GraphQL]
Saat mendesain mutation `placeOrder(input: PlaceOrderInput!)`, field yang **sebaiknya** ada di `PlaceOrderInput`:
- a. `cartId`
- b. `totalPrice`
- c. `customerId`
- d. `discountPercent`

<details>
<summary>Lihat jawaban & alasan</summary>

**Jawaban: a**

**Alasan:** Slide *Mutation: design the schema*: "*The input contains a cart ID, while the server obtains customer identity from **authentication** and **calculates prices itself**.*" Harga, diskon, dan identitas yang dikirim client bisa dimanipulasi, sehingga server tidak boleh mempercayainya.

</details>

---
## Bagian B – Benar/Salah

> Petunjuk: tentukan apakah setiap pernyataan **Benar (B)** atau **Salah (S)**.

---

### B1 · [01a Pengantar Kuliah]
Pernyataan berikut terkait aturan mata kuliah IF3110: (Benar/Salah)
1. Tugas besar dikerjakan berkelompok 3 orang yang ditentukan oleh dosen
2. Bobot ujian lebih dominan daripada bobot tugas
3. Meminta LLM (seperti GPT/Claude) menuliskan kode proyek dianggap kecurangan
4. Mata kuliah 3 SKS ini terdiri dari 3 jam kuliah dan 6 jam aktivitas mandiri per minggu

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | "*done in a group of 3 students (defined by lecturers)*". |
| 2 | **S** | Tugas yang lebih dominan, kira-kira **55% tugas : 45% ujian**. |
| 3 | **B** | Tercantum di slide *Noteworthy* sebagai bentuk kecurangan. |
| 4 | **B** | 3 jam kuliah (Senin & Rabu) + 6 jam independent activities. |

</details>

---

### B2 · [01b Konsep Aplikasi Internet]
Pernyataan berikut terkait Internet-based dan Web-based application: (Benar/Salah)
1. Semua Internet-based application adalah Web-based application
2. Internet-based application dapat memakai protokol buatan sendiri
3. Web-based application berkomunikasi dengan client melalui web server
4. Web-based application selalu dan hanya dapat dijalankan di web browser

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **S** | Kebalikannya. Web app adalah **bagian** dari Internet app, sehingga semua web app adalah Internet app, tapi tidak sebaliknya. |
| 2 | **B** | "*Using Internet Application Protocol or defining own protocol*". |
| 3 | **B** | "*Application communicate to Client via Web Server*". |
| 4 | **S** | Slide menulis "*commonly runs in web browser*", artinya umumnya, bukan selalu. |

</details>

---

### B3 · [01b Konsep Aplikasi Internet]
Pernyataan berikut terkait web stack dan arsitektur aplikasi web: (Benar/Salah)
1. Web stack mencakup sistem operasi, web server, bahasa pemrograman, web framework, dan database server
2. Web stack dan web framework adalah istilah untuk hal yang sama
3. Pada *distributed deployment*, presentation layer dapat berada di web server, sedangkan business & data layer berada di application server
4. *Cross-cutting concerns* pada arsitektur aplikasi meliputi security, operational management, dan communication

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Definisi web stack di slide. |
| 2 | **S** | "*It is different from Web Framework*". Framework hanya satu lapisan di dalam stack. |
| 3 | **B** | Figure 3 *Distributed deployment*: Web Server (Presentation) → Application Server (Business, Data) → Database Server. |
| 4 | **B** | Figure 1 *Common application architecture*: kolom Cross-cutting berisi Security, Operational Management, Communication. |

</details>

---

### B4 · [01b Konsep Aplikasi Internet]
Pernyataan berikut terkait client-server dan protokol komunikasi: (Benar/Salah)
1. Client dan server dapat dirancang dan dikembangkan secara terpisah
2. Pada arsitektur client-server, server hanya dapat memproses satu request dalam satu waktu
3. Transport protocol memecah pesan menjadi paket, mengirimnya lewat berbagai rute jaringan, lalu menyusunnya kembali di tujuan
4. *Request context* yang dikirim browser hanya terdiri dari URL

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Termasuk keuntungan client-server. |
| 2 | **S** | Justru keuntungan client-server adalah "*Server can process many simultaneous requests*". |
| 3 | **B** | Definisi transport protocol di slide *Protocol Communication*. |
| 4 | **S** | Request context = **URL, query string, request headers, request body, session information**. |

</details>

---

### B5 · [02a HTTP]
Pernyataan berikut terkait dasar protokol HTTP: (Benar/Salah)
1. HTTP adalah application layer protocol yang (pada HTTP/1.x dan HTTP/2) berjalan di atas TCP
2. Pada HTTP/1.0, koneksi TCP ditutup setelah satu response
3. HTTP menyimpan state antar-request di level protokol
4. Header `Referer` berisi URL halaman asal yang mereferensikan request tersebut

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | "*Application Layer Protocol founded by TCP*". HTTP/3 memakai QUIC/UDP. |
| 2 | **B** | Anatomi HTTP 1.0: Connect → Request → Response → **Close**. |
| 3 | **S** | HTTP **stateless**: satu transaksi = satu request + satu response. |
| 4 | **B** | Contoh di slide: `Referer: http://www.cs.rutgers.edu/index.html`. |

</details>

---

### B6 · [02a HTTP]
Pernyataan berikut terkait HTTP method: (Benar/Salah)
1. HEAD mengembalikan header saja, tanpa message body
2. TRACE dipakai untuk menghapus resource di server
3. OPTIONS mengembalikan method HTTP yang didukung server untuk URL tertentu
4. QUERY mirip GET tetapi memiliki request body

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | "*Server MUST NOT return a message body*". Berguna untuk mengecek `Last-Modified`. |
| 2 | **S** | TRACE **mengembalikan request** yang diterima server, untuk melihat info yang ditambahkan proxy. Penghapusan memakai **DELETE**. |
| 3 | **B** | Definisi OPTIONS di slide. |
| 4 | **B** | "*Similar to GET, but it has request body content*". Juga safe dan idempotent. |

</details>

---

### B7 · [02a HTTP]
Pernyataan berikut terkait GET dan POST: (Benar/Salah)
1. Request GET dapat di-bookmark
2. Data yang dikirim dengan POST tersimpan di riwayat (history) browser
3. Data GET hanya boleh berisi karakter ASCII
4. POST mengenkripsi data sehingga aman dikirim tanpa HTTPS

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Tabel GET vs POST: "*can be bookmarked*". |
| 2 | **S** | POST "*do not remain in the browser history*". |
| 3 | **B** | "*Only ASCII characters allowed*". |
| 4 | **S** | POST hanya menaruh data di body, bukan di URL. **Tidak ada enkripsi**; tanpa HTTPS, body tetap bisa disadap. |

</details>

---

### B8 · [02a HTTP]
Pernyataan berikut terkait header dan caching HTTP: (Benar/Salah)
1. `Set-Cookie` adalah header yang dikirim browser di dalam request
2. `Cache-Control: private` berarti resource hanya boleh di-cache di perangkat client
3. `Cache-Control: max-age=600` berarti response dianggap segar selama 600 menit
4. Header `Last-Modified` berisi waktu ketika server asal menganggap resource terakhir diubah

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **S** | `Set-Cookie` dikirim **server di response**. Browser mengirim balik cookie dengan header **`Cookie`**. |
| 2 | **B** | "*resource is user specific — it can still be cached, but only on a client device*". |
| 3 | **S** | `max-age` dihitung dalam **detik**: 600 detik = 10 menit. |
| 4 | **B** | Definisi `Last-Modified` di slide. |

</details>

---

### B9 · [02a HTTP]
Pernyataan berikut terkait evolusi HTTP/1.1 dan HTTP/2: (Benar/Salah)
1. HTTP/1.1 memperkenalkan persistent connection dan chunked transfer encoding
2. Image spriting dan domain sharding adalah teknik untuk mengakali keterbatasan HTTP/1.1
3. HTTP/2 memakai SPDY sebagai draft pertamanya
4. HTTP/3 memakai HPACK untuk kompresi header

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *HTTP 1.1 vs 1.0*: transfer coding (chunk encoding) dan persistent connections. |
| 2 | **B** | Termasuk "*many hacks because requests are evil*". |
| 3 | **B** | "*Used SPDY3 as its first draft*". |
| 4 | **S** | HPACK milik HTTP/2. HTTP/3 memakai **QPACK**, yang dirancang untuk stream QUIC yang bisa datang tidak berurutan. |

</details>

---

### B10 · [02a HTTP]
Pernyataan berikut terkait HTTPS dan HTTP/3: (Benar/Salah)
1. HTTPS adalah HTTP di atas TLS di atas TCP
2. Pada HTTPS berbasis TCP, TCP handshake dan pertukaran sertifikat TLS dilakukan sebelum data HTTP dikirim
3. Pada HTTP/3, satu paket yang hilang akan menahan seluruh stream di koneksi tersebut
4. Jika QUIC gagal (misalnya UDP diblokir), client sebaiknya mencoba versi HTTP berbasis TCP

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Definisi di slide *HTTPS*. |
| 2 | **B** | Diagram hpbn.co: TCP 56 ms + TLS 112 ms, baru *Application Data*. |
| 3 | **S** | Itu masalah **HTTP/2 di atas TCP**. QUIC memulihkan data **per stream**, sehingga hanya stream yang terdampak yang tertahan. |
| 4 | **B** | Slide *HTTP/3 adoption still needs fallback*. |

</details>

---

### B11 · [02a HTTP]
Pernyataan berikut terkait jalur request dan keandalan HTTP/2: (Benar/Salah)
1. Request dari browser dapat melewati satu atau lebih proxy sebelum sampai ke web server
2. Frame `GOAWAY` memberi tahu nomor stream tertinggi yang sudah diproses server
3. Kode error `REFUSED_STREAM` dapat dikirim dalam frame `RST_STREAM` untuk menandakan stream belum diproses
4. Pada HPACK, header yang sensitif sebaiknya selalu ditambahkan ke dynamic table

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *Request Response Virtual Circuit*: Browser → Proxy (ISP) → Proxy (firewall) → Web server. |
| 2 | **B** | Slide *Request Reliability*. Stream dengan nomor lebih besar dijamin belum diproses, sehingga aman diulang. |
| 3 | **B** | Mekanisme kedua di slide yang sama. |
| 4 | **S** | HPACK punya opsi literal "**NEVER add to dynamic table**" untuk nilai yang tidak boleh disimpan dalam tabel kompresi. Jadi tidak "selalu" ditambahkan. |

</details>

---

### B12 · [02b HTML & CSS]
Pernyataan berikut terkait dasar HTML: (Benar/Salah)
1. Tag dan elemen adalah hal yang sama
2. `<!DOCTYPE html>` memastikan halaman dirender dengan HTML terbaru, bukan mode fallback lama
3. Elemen inline tidak boleh berisi elemen block-level
4. `<head>` adalah tempat konten yang tampil di halaman, seperti judul utama dan nama penulis

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **S** | Tag memulai atau mengakhiri elemen di kode sumber. Elemen adalah bagian dari **DOM**. |
| 2 | **B** | "*this makes sure page uses latest html and not some random fallback version*". |
| 3 | **B** | "*An inline element cannot contain a block-level element!*". |
| 4 | **S** | `<head>` berisi **metadata untuk mesin**. Konten yang terlihat seperti judul utama dan penulis ditaruh di **`<header>`**. |

</details>

---

### B13 · [02b HTML & CSS]
Pernyataan berikut terkait cascade dan specificity CSS: (Benar/Salah)
1. Inline style memiliki nilai specificity 1000
2. Jika dua aturan memiliki specificity sama, aturan yang muncul paling akhir yang berlaku
3. `!important` hanya dapat dikalahkan oleh inline style
4. Satu elemen boleh memiliki beberapa class, tetapi sebuah ID harus unik dalam satu dokumen

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Note 1 di slide *CSS Specificity (2)*. |
| 2 | **B** | Tahap *position and order of appearance* pada cascade (contoh tombol biru di slide). |
| 3 | **S** | `!important` justru **mengalahkan** inline style. Ia hanya bisa ditimpa oleh `!important` lain dengan specificity sama atau lebih tinggi. |
| 4 | **B** | Slide *ID vs Class*. |

</details>

---

### B14 · [02b HTML & CSS]
Diberikan HTML berikut:
```html
<div class="container">
  <section class="child" id="c1"><div>subchild 1</div></section>
  <div class="child" id="c2">child 2</div>
  <div class="child" id="c3">child 3</div>
  <div class="child" id="c4">child 4</div>
</div>
```
Pernyataan berikut terkait combinator CSS: (Benar/Salah)
1. `.container div` memilih subchild 1
2. `.container > div` memilih subchild 1
3. `#c1 + div` hanya memilih child 2
4. `#c1 ~ div` memilih subchild 1

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Spasi = **keturunan** sedalam apa pun, dan subchild 1 adalah cucu `.container`. |
| 2 | **S** | `>` = **anak langsung** saja, sedangkan subchild 1 adalah cucu. |
| 3 | **B** | `+` = **satu** saudara **tepat setelah** `#c1`. |
| 4 | **S** | `~` memilih **saudara** setelah `#c1` (child 2, 3, 4). Subchild 1 adalah **anak** `#c1`, bukan saudaranya. |

✅ Keempat pernyataan terverifikasi dengan `querySelectorAll` di Chromium.

</details>

---

### B15 · [02b HTML & CSS]
Pernyataan berikut terkait box model dan overflow: (Benar/Salah)
1. Padding adalah ruang di antara konten dan border
2. Margin adalah ruang di antara konten dan border
3. `overflow: hidden` memotong konten yang meluber, dan isinya tidak dapat di-scroll
4. `overflow: visible` adalah nilai default

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Padding box = "*The space around the content*" (di dalam border). |
| 2 | **S** | Margin adalah ruang **terluar**, antara elemen ini dan elemen lain. |
| 3 | **B** | "*clip the overflowing content → cannot scroll within the parent element*". |
| 4 | **B** | "*Default behavior → display the overflowing content*". |

</details>

---

### B16 · [02b HTML5]
Pernyataan berikut terkait fitur HTML5: (Benar/Salah)
1. Canvas menyediakan permukaan gambar 2D yang dapat diprogram dengan JavaScript
2. WebSocket hanya dapat mengirim data dari server ke browser
3. Server-Sent Events adalah event yang mengalir dari server ke browser
4. Web worker menjalankan JavaScript di background tanpa mengganggu performa halaman

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *HTML 5 New Features*. |
| 2 | **S** | WebSocket bersifat **bidirectional**: `send()` mengirim ke server, `onmessage` menerima dari server. |
| 3 | **B** | Definisi SSE. |
| 4 | **B** | Slide *Web Worker*. |

</details>

---

### B17 · [03 JavaScript]
Pernyataan berikut terkait JavaScript secara umum: (Benar/Salah)
1. JavaScript dikembangkan dari bahasa Java sehingga sintaks dan konsepnya sama
2. ECMAScript adalah standar resmi JavaScript (ECMA-262)
3. Keyword `class` pada ES6 adalah *syntactic sugar* di atas prototype
4. DOM biasanya disediakan oleh JavaScript engine, bukan oleh browser

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **S** | "*JavaScript != Java, and it is not came from java*". Sintaksnya hanya **mirip**. |
| 2 | **B** | "*Formalized & standardised as ECMAScript (ECMA-262 and ISO/IEC 16262)*". |
| 3 | **B** | Tertulis di slide *Introduction*. |
| 4 | **S** | "*The DOM is not typically provided by the JavaScript engine but instead by a browser*". |

</details>

---

### B18 · [03 JavaScript]
Pernyataan berikut terkait tipe data JavaScript: (Benar/Salah)
1. String `"false"` bernilai truthy
2. Array kosong `[]` bernilai truthy
3. `NaN === NaN` bernilai `true`
4. `typeof function(){}` menghasilkan `"function"`

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | String yang tidak kosong selalu truthy; slide menyebut "*including "null", "0", "false"*". ✅ |
| 2 | **B** | Objek dan array **selalu truthy** di JS (berbeda dengan PHP). ✅ |
| 3 | **S** | NaN tidak sama dengan apa pun, **termasuk dirinya sendiri**. ✅ |
| 4 | **B** | ✅ Terverifikasi di Node.js. |

</details>

---

### B19 · [03 JavaScript]
Pernyataan berikut terkait runtime model JavaScript: (Benar/Salah)
1. Program JavaScript bersifat single-threaded
2. Callback `Promise.then` dimasukkan ke microtask queue
3. Callback `setTimeout(fn, 0)` dijalankan lebih dulu daripada callback `Promise.then` yang dijadwalkan pada saat yang sama
4. Event handler yang berjalan sangat lama dapat membuat browser *freeze*

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *Javascript Runtime Model*. |
| 2 | **B** | Microtask queue berisi `.then()`, `.catch()`, `.finally()`. |
| 3 | **S** | Semua **microtask** dijalankan **sebelum** macrotask berikutnya, jadi `then` jalan lebih dulu. ✅ Terverifikasi. |
| 4 | **B** | "*event-handler javascript SHOULD NOT be long, it creates a browser freeze*". |

</details>

---

### B20 · [03 JavaScript]
Pernyataan berikut terkait fungsi dan array JavaScript: (Benar/Salah)
1. `var f = (x, y) => x * y;` adalah arrow function yang valid
2. Argumen fungsi dapat diakses melalui objek `arguments`
3. JavaScript memeriksa jumlah argumen yang dikirim ke fungsi pada saat compile
4. `[10, 9, 1].sort()` menghasilkan `[1, 9, 10]`

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Bentuk singkat dengan return implisit. (Bentuk `=> return x*y` tanpa kurung kurawal justru error.) |
| 2 | **B** | Contoh slide: `arguments[0]*x`. |
| 3 | **S** | "*argument checking/verification in Javascript done at runtime*". |
| 4 | **S** | `sort()` default mengurutkan **sebagai string**, sehingga hasilnya `[1, 10, 9]`. Untuk angka gunakan `sort((a, b) => a - b)`. ✅ Terverifikasi. |

</details>

---

### B21 · [03 JavaScript]
Pernyataan berikut terkait W3C DOM dan penanganan error JavaScript: (Benar/Salah)
1. Elemen HTML di DOM merupakan turunan dari `Node`
2. `Text` dan `Comment` merupakan turunan dari `CharacterData`
3. `TypeError` dan `RangeError` adalah nama exception bawaan JavaScript
4. Nilai yang diperiksa oleh `switch` di JavaScript harus berupa number

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | "*HTML element in DOM is descendant of Node*" (Node → Element → HTMLElement → HTMLParagraphElement, dll). |
| 2 | **B** | Diagram hierarki Node: CharacterData → Text, Comment. |
| 3 | **B** | Daftar di slide: Error, EvalError, RangeError, SyntaxError, TypeError, URIError. |
| 4 | **S** | "*The switch value does not need to a number. It can be a string.*" |

</details>

---

### B22 · [03b AJAX]
Pernyataan berikut terkait AJAX dan XMLHttpRequest: (Benar/Salah)
1. AJAX adalah teknik yang menggabungkan objek XMLHttpRequest dengan JavaScript dan HTML DOM
2. Property `responseXML` mengembalikan response sebagai objek XML DOM
3. Synchronous XMLHttpRequest dianjurkan karena kodenya lebih sederhana
4. Method `getResponseHeader("Last-Modified")` mengembalikan satu header tertentu dari response

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Definisi slide *What is AJAX*. |
| 2 | **B** | XHR punya parser XML bawaan. |
| 3 | **S** | "*Synchronous XMLHttpRequest is not recommended*", karena JavaScript berhenti sampai response siap. |
| 4 | **B** | Contoh di slide *getResponseHeader()*. |

</details>

---

### B23 · [03b AJAX]
Pernyataan berikut terkait same-origin policy dan CORS: (Benar/Salah)
1. Origin ditentukan oleh skema, domain, dan port
2. `http://itb.ac.id` dan `https://itb.ac.id` memiliki origin yang sama
3. Server dapat mengizinkan origin lain mengakses response-nya dengan header `Access-Control-Allow-Origin`
4. Same-origin policy diberlakukan oleh browser

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | "*same origin (same domain & scheme & port)*". |
| 2 | **S** | Skemanya berbeda (http vs https), dan port default-nya juga berbeda (80 vs 443). |
| 3 | **B** | Mekanisme CORS di slide *Access Across Domains*. |
| 4 | **B** | "*modern browsers do not allow access across domains*". Server hanya **memberi izin** lewat header CORS; yang menegakkan aturannya adalah browser. |

</details>

---
### B24 · [04a Server-Side Scripting]
Pernyataan berikut terkait aspek server-side: (Benar/Salah)
1. Routing memetakan URL tertentu ke fungsi yang terkait
2. Template berisi data statis dan placeholder untuk data dinamis
3. Kode server-side scripting dijalankan oleh browser
4. Pada web application, sebuah route harus selalu mengembalikan file HTML

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | "*Map the specific URL with an associated function*". |
| 2 | **B** | Definisi template di slide. |
| 3 | **S** | Dijalankan di **server**. Browser hanya menerima hasilnya (lihat contoh `hello.php`). |
| 4 | **S** | "*Using a web application, an HTML file may be returned, **among other return types***", misalnya JSON, gambar, atau redirect. |

</details>

---

### B25 · [04a Server-Side – PHP]
Pernyataan berikut terkait bahasa PHP: (Benar/Salah)
1. PHP adalah singkatan rekursif dari "PHP: Hypertext Preprocessor"
2. Variabel di dalam fungsi PHP secara default dapat mengakses variabel global tanpa keyword `global`
3. String dengan tanda petik satu (`'Halo $nama'`) tidak mengganti `$nama` dengan nilainya
4. Tipe `float` di PHP adalah single-precision floating point

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *What is PHP?*. |
| 2 | **S** | Scope default **lokal**; harus memakai `global`. ✅ Terverifikasi: tanpa `global`, variabel luar tidak berubah. |
| 3 | **B** | ✅ Terverifikasi: `'Halo $nama'` dicetak apa adanya, sedangkan `"Halo $nama"` menjadi `Halo Budi`. |
| 4 | **S** | Float PHP adalah **double precision** (64-bit). |

</details>

---

### B26 · [04a Server-Side – PHP]
Pernyataan berikut terkait input/output PHP: (Benar/Salah)
1. Nilai `$_COOKIE` dapat diubah pengguna, sehingga tidak boleh langsung dipercaya sebagai bukti login
2. Data `$_SESSION` disimpan di server, sedangkan browser hanya menyimpan session ID
3. `header("Location: login.html")` dipakai untuk mengalihkan (redirect) pengguna
4. Fungsi `mysql_connect` masih tersedia di PHP 8

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Cookie ada di browser dan bisa diedit lewat DevTools. Contoh *Input from Cookie* di slide rawan dipalsukan. |
| 2 | **B** | Inilah yang membuat session lebih aman daripada cookie biasa. |
| 3 | **B** | Dipakai di contoh `anypage.php` di slide. |
| 4 | **S** | Fungsi `mysql_*` **dihapus sejak PHP 7**. Gunakan `mysqli` atau PDO. |

</details>

---

### B27 · [04a Server-Side – PHP]
Pernyataan berikut terkait versi dan array PHP: (Benar/Salah)
1. PHP 6 tidak pernah dirilis
2. Null coalescing operator `??` pertama kali diperkenalkan di PHP 8
3. Array PHP sebenarnya adalah *ordered map*
4. Setelah `$arr = [10, 20, 30]; $arr[] = 40; unset($arr[3]); $arr[] = 50;`, nilai 50 berada di kunci 3

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | "*PHP 6 was experimental, and never released*". |
| 2 | **S** | `??` (dan `<=>`) diperkenalkan di **PHP 7** (slide *Additional Stuffs in PHP 7*). |
| 3 | **B** | "*An array in PHP is actually an ordered map*". |
| 4 | **S** | Kunci otomatis melanjutkan dari kunci integer **terbesar yang pernah dipakai**. Setelah kunci 3 di-unset, elemen berikutnya mendapat kunci **4**. ✅ Terverifikasi. |

</details>

---

### B28 · [04a Server-Side – PHP]
Pernyataan berikut terkait objek, resource, dan output PHP: (Benar/Salah)
1. Method objek PHP dipanggil dengan operator `->`, misalnya `$bar->do_foo();`
2. Variabel bertipe resource memegang handler ke sumber eksternal seperti file yang dibuka atau koneksi database
3. PHP dapat menghasilkan gambar dengan mengirim header `Content-type: image/gif` lalu memanggil `imagegif()`
4. Sejak PHP 5, meng-assign sebuah objek ke variabel lain akan menyalin seluruh isi objek, bukan referensinya

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Contoh `class foo` di slide. |
| 2 | **B** | Slide *Resource*. |
| 3 | **B** | Slide *Image* (`imagecreate`, `imagefilledrectangle`, `imagegif`, `imagedestroy`). |
| 4 | **S** | "*PHP treats objects in the same way as references or handles … each variable contains an object reference rather than a copy of the entire object.*" |

</details>

---

### B29 · [04b Go for Web]
Pernyataan berikut terkait bahasa Go: (Benar/Salah)
1. Go dibuat di Google pada tahun 2007 oleh Robert Griesemer, Rob Pike, dan Ken Thompson
2. Go memiliki sekitar 25 keyword
3. Standard library Go menyediakan ORM bawaan
4. `gofmt` memaksa gaya penulisan kode yang konsisten

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *The Origin Story*. |
| 2 | **B** | "*Go has ~25 keywords (vs Java's ~50, C++'s ~90)*". |
| 3 | **S** | "*No built-in ORM (use database/sql or GORM)*". |
| 4 | **B** | "*Consistent style (gofmt enforces formatting)*". |

</details>

---

### B30 · [04b Go for Web]
Pernyataan berikut terkait Go untuk web: (Benar/Salah)
1. Paket `net/http` di standard library dianggap production-grade
2. Setiap request pada server `net/http` dilayani oleh goroutine
3. Jika satu goroutine terblokir karena I/O, seluruh goroutine lain ikut berhenti
4. Gin, Echo, dan Fiber adalah web framework untuk Go

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | "*net/http is production-grade*". |
| 2 | **B** | "*Each request spawns a goroutine*". |
| 3 | **S** | "*If a goroutine blocks (I/O), others continue*". |
| 4 | **B** | Slide *Popular Go web frameworks*. |

</details>

---

### B31 · [05 State Handling]
Pernyataan berikut terkait teknik state handling di sisi client: (Benar/Salah)
1. Menyimpan filter dan nomor halaman di URL membuat state bertahan saat refresh dan bisa dibagikan lewat link
2. Hidden field adalah batas keamanan karena tidak terlihat oleh pengguna
3. Store Zustand otomatis menyimpan data secara permanen walaupun halaman di-reload
4. Data `localStorage` otomatis ikut terkirim di setiap request ke server

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *URL state survives refresh*. |
| 2 | **S** | "*Hidden describes the normal page UI, not a security boundary. Users can modify it via DevTools.*" |
| 3 | **S** | "*The shared store keeps the page consistent, but it persists nothing.*" Reload mengembalikan count ke 0. |
| 4 | **S** | "*Web Storage belongs to an origin. It does not automatically travel in requests.*" Harus dikirim manual dengan `fetch`. |

</details>

---

### B32 · [05 State Handling]
Pernyataan berikut terkait cookie dan server-side session di Go: (Benar/Salah)
1. Cookie harus di-set sebelum body response ditulis
2. Session yang disimpan di map memori server akan hilang saat aplikasi di-restart
3. Jika aplikasi berjalan di beberapa instance, dibutuhkan shared session store
4. Cukup mengandalkan masa berlaku cookie di browser; server tidak perlu lagi mengecek masa berlaku session

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | "*Set the cookie before writing the response*". Cookie adalah header, dan header dikirim sebelum body. |
| 2 | **B** | Termasuk keterbatasan session di slide *Session Limitation*. |
| 3 | **B** | "*Multiple servers need a shared store*". |
| 4 | **S** | "*Browser cookie expiry and server session expiry are **independent checks***". Server tetap harus mengecek `sess.Expires`. |

</details>

---

### B33 · [05 State Handling – Database]
Pernyataan berikut terkait akses database di Go: (Benar/Salah)
1. `sql.Open` selalu langsung membuka koneksi ke database
2. `rows.Close()` perlu dipanggil (misalnya dengan `defer`) setelah membaca hasil query banyak baris
3. Perintah `UPDATE` atau `DELETE` tanpa `WHERE` akan memengaruhi seluruh baris tabel
4. `result.RowsAffected()` bernilai 0 dapat menandakan record yang dimaksud tidak ditemukan

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **S** | "*sql.Open: creates a handle; **may not connect yet***". Konektivitas dicek dengan `PingContext`. |
| 2 | **B** | Contoh di slide memakai `defer rows.Close()` agar koneksi kembali ke pool. |
| 3 | **B** | Karena itu slide menulis "*Always use WHERE to control which records are changed/deleted*". |
| 4 | **B** | Contoh slide: `if rows == 0 { return errOfferingNotFound }`. |

</details>

---

### B34 · [05 State Handling – File]
Pernyataan berikut terkait penanganan file di aplikasi web: (Benar/Salah)
1. Nama file yang diberikan user harus divalidasi untuk mencegah *path traversal* (misal `../../etc/passwd`)
2. Membaca CSV secara streaming langsung dari body request dapat menghindari penyimpanan file sementara di disk
3. Signed URL untuk object storage berlaku selamanya agar bisa dipakai ulang
4. Dengan signed URL, client dapat meng-upload file langsung ke object storage tanpa melewati server aplikasi

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Contoh slide memakai regex `^[a-zA-Z0-9_-]+$` sebelum `filepath.Join`. |
| 2 | **B** | Alternatif *Stream through backend*: "*No file saved to disk, Memory-efficient streaming*". |
| 3 | **S** | "*Signed URL is only valid for a short of time*". Contoh di slide memakai 10 menit. |
| 4 | **B** | Alur upload: WebApp meminta signed URL, lalu client meng-upload langsung lewat URL itu. |

</details>

---

### B35 · [05 Authentication]
Pernyataan berikut terkait authentication dan authorization: (Benar/Salah)
1. Authentication dilakukan di setiap request yang dilindungi, sedangkan authorization hanya sekali saat login
2. User yang sudah login tetap dapat menerima respons *access denied* (403)
3. Pada opaque token, makna token ada di catatan server, sedangkan client hanya memegang identifier
4. Bearer token sebaiknya tidak dimasukkan ke URL, log, atau source code

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **S** | **Terbalik**. Authentication terjadi sekali (saat login), sedangkan authorization terjadi di setiap request yang dilindungi. |
| 2 | **B** | "*A signed-in user can still receive an access-denied response.*" |
| 3 | **B** | "*The server record supplies the meaning. The client holds an identifier.*" |
| 4 | **B** | "*Possession matters. Keep tokens out of URLs, logs, and source code.*" |

</details>

---

### B36 · [05 Authentication]
Pernyataan berikut terkait JWT, OAuth, dan OpenID Connect: (Benar/Salah)
1. Siapa pun yang memegang sebuah JWT biasanya dapat membaca isi header dan payload-nya
2. OAuth adalah protokol *delegated authorization* yang mewajibkan persetujuan eksplisit user
3. ID token OIDC dapat dipakai sebagai pengganti access token untuk memanggil API
4. Dalam satu alur login, JWT, OIDC, dan server-side session dapat muncul bersamaan

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Payload hanya di-encode Base64url, bukan dienkripsi. |
| 2 | **B** | Slide *OAuth – Features*: "*Delegated authorization protocol. Explicit user consent is mandatory.*" |
| 3 | **S** | "*An ID token is not a substitute for an API access token.*" |
| 4 | **B** | Tertulis di slide *OIDC Flow – An Overview with Go*. |

</details>

---

### B37 · [06-01 REST API Design]
Pernyataan berikut terkait prinsip REST: (Benar/Salah)
1. REST adalah gaya arsitektur untuk sistem jaringan yang diperkenalkan oleh Roy Fielding
2. Sekumpulan endpoint CRUD otomatis memenuhi semua batasan REST
3. HATEOAS berarti response memberi tahu interaksi apa yang bisa dilakukan selanjutnya melalui link
4. Prinsip stateless pada REST berarti server tidak boleh menyimpan data resource apa pun di database

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *REST API*. |
| 2 | **S** | "*A list of CRUD endpoints alone does not establish those constraints.*" |
| 3 | **B** | Definisi HATEOAS dan contoh `<order>` dengan `customer-uri`/`product-uri`. |
| 4 | **S** | Yang tidak boleh disimpan adalah **data sesi client**. State **resource** tetap persisten (slide: "*Stateless interaction still permits persistent resource state*"). |

</details>

---

### B38 · [06-01 REST API Design]
Pernyataan berikut terkait properti method HTTP pada REST: (Benar/Salah)
1. Safe berarti client tidak bermaksud mengubah state; logging insidental di server tidak mengubah sifat tersebut
2. Idempotent berarti setiap pengulangan request harus menghasilkan status code yang identik
3. Idempotensi PATCH bergantung pada semantik patch yang dipakai
4. `GET /buy?product=P101` adalah desain yang baik untuk aksi pembelian

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Definisi *safe* di slide *HTTP Method Properties*. |
| 2 | **S** | "*The response code need not be identical each time.*" Yang sama adalah **efek yang dimaksudkan**. |
| 3 | **B** | Tabel *Uniform Interface*: PATCH idempotent-nya "*Depends on patch semantics*". |
| 4 | **S** | "*Don't use it for state changing actions, such as GET /buy?product=P101*". |

</details>

---

### B39 · [06-01 REST API Design]
Pernyataan berikut terkait usability dan format data API: (Benar/Salah)
1. Timestamp sebaiknya memakai format RFC 3339, misalnya `2026-09-27T09:00:00Z`
2. Uang sebaiknya direpresentasikan dengan satuan terkecil dan kode mata uang, misalnya `amountMinor` dan `currency`
3. Kegagalan sebaiknya dikembalikan sebagai `200 OK` dengan body `{"error": true}` agar client tidak bingung
4. RFC 9457 mendefinisikan media type `application/problem+json` untuk detail error

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *Layer 2: predictable, usable data*. |
| 2 | **B** | Contoh `"fine": {"amountMinor": 50000, "currency": "IDR"}`. |
| 3 | **S** | "*Use a status that reflects the failed operation instead of disguising failure as success.*" |
| 4 | **B** | Field-nya: `type`, `title`, `status`, `detail`, `instance`. |

</details>

---

### B40 · [06-01 REST API Design]
Pernyataan berikut terkait keamanan API: (Benar/Salah)
1. OAuth scope seperti `loans:read` sudah cukup untuk memutuskan apakah pemanggil memiliki sebuah loan tertentu
2. BOLA dicegah dengan memeriksa izin terhadap objek spesifik pada setiap operasi yang relevan
3. OpenAPI dapat menegakkan aturan kepemilikan objek secara otomatis tanpa implementasi di kode
4. *Data minimization* berarti hanya menerima property yang boleh ditulis dan hanya mengembalikan data yang boleh dilihat pemanggil

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **S** | "*A scope grants a class of access. It does not decide whether this caller owns this particular loan.*" |
| 2 | **B** | Slide *preventing BOLA*. |
| 3 | **S** | "*OpenAPI cannot enforce ownership by itself.*" |
| 4 | **B** | Slide *Security by design and least exposure*. |

</details>

---

### B41 · [06-01 REST API Design]
Pernyataan berikut terkait versioning, pagination, dan proses desain: (Benar/Salah)
1. Menambah kolom baru di database selalu membutuhkan versi API publik yang baru
2. Cursor pagination memakai token posisi yang opaque, misalnya `?after=token&limit=20`
3. Spectral dapat dipakai di CI untuk melakukan lint terhadap deskripsi OpenAPI
4. Linting OpenAPI dapat membuktikan bahwa otorisasi dan perilaku runtime sudah benar

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **S** | "*A new database column or deployment number does not require a public API version.*" |
| 2 | **B** | Tabel *Filtering, sorting, and pagination*. |
| 3 | **B** | Contoh `spectral lint openapi.yaml`. |
| 4 | **S** | "*Linting cannot prove correct user needs, authorization or runtime behavior.*" |

</details>

---

### B42 · [06-01 REST API Design – API Management]
Pernyataan berikut terkait API management dan operasi bulk: (Benar/Salah)
1. API gateway adalah titik masuk runtime yang dapat menerapkan aturan trafik bersama
2. API key yang mengidentifikasi sebuah integrasi sama dengan login end user di toko
3. Akses Admin API atau dashboard APISIX perlu dilindungi secara terpisah dari trafik API publik
4. Status 207 berasal dari WebDAV dan tidak menstandarkan format envelope batch JSON sembarang

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *What is API management?*. |
| 2 | **S** | "*An API key identifying an integration is different from an end user's store login.*" |
| 3 | **B** | "*Protect management access separately from public API traffic.*" |
| 4 | **B** | Slide *Bulk operations and mixed outcomes*. |

</details>

---

### B43 · [06-02 GraphQL]
Pernyataan berikut terkait dasar GraphQL: (Benar/Salah)
1. GraphQL memiliki tiga operasi: query, mutation, dan subscription
2. GraphQL mewajibkan penggunaan graph database
3. Client GraphQL dapat memilih kolom database apa pun secara bebas
4. Struktur JSON di bawah `data` pada response mengikuti bentuk *selection* pada operasi

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *What is GraphQL?*. |
| 2 | **S** | "*GraphQL does not require a graph database.*" |
| 3 | **S** | "*Resolvers supply data behind the schema; clients do not choose arbitrary database columns.*" |
| 4 | **B** | Slide *Query: response*. |

</details>

---

### B44 · [06-02 GraphQL]
Pernyataan berikut terkait sistem tipe GraphQL: (Benar/Salah)
1. Scalar bawaan GraphQL meliputi String, Int, Float, Boolean, dan ID
2. `[Product!]!` berarti list pasti ada dan setiap item di dalamnya pasti ada
3. *Input object* mengelompokkan nilai-nilai yang boleh dikirim client ke sebuah operasi
4. Lebih baik memakai string JSON buram (opaque) daripada field bertipe, agar skema lebih sederhana

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *Scalar, object, enum, and input types*. |
| 2 | **B** | Tabel *Lists and nullability*. |
| 3 | **B** | Definisi input object. |
| 4 | **S** | "*Prefer a meaningful, typed field to an opaque JSON string when clients need to explore and select its parts.*" |

</details>

---

### B45 · [06-02 GraphQL]
Pernyataan berikut terkait praktik baik GraphQL: (Benar/Salah)
1. Mengumpulkan (*batch*) ID dalam satu request dapat mengatasi masalah N+1 pada resolver
2. Rate limit berdasarkan jumlah request saja sudah cukup untuk membatasi beban kerja satu request GraphQL
3. Setelah subscription tersambung kembali (*reconnect*), client sebaiknya mengambil status terkini karena event bisa saja terlewat
4. Field lama boleh langsung dihapus tanpa tanda `@deprecated`, asalkan sudah ada field pengganti

<details>
<summary>Lihat jawaban & alasan</summary>

| No | Jawaban | Alasan |
|---|---|---|
| 1 | **B** | Slide *Resolver performance: the N+1 problem*. |
| 2 | **S** | "*A request-count rate limit alone does not bound the work in one GraphQL request.*" Perlu batas kedalaman/biaya, ukuran koleksi, dan deadline. |
| 3 | **B** | "*After reconnecting, fetch the current status because events may have been missed.*" |
| 4 | **S** | "*Deprecate old fields with a replacement and migration guidance before removing them*", contohnya `@deprecated(reason: "Use seller { name }")`. |

</details>

---
## 🔑 Kunci Jawaban Cepat

### Bagian A – Pilihan Ganda

| No | Jawaban | No | Jawaban | No | Jawaban | No | Jawaban | No | Jawaban |
|---|---|---|---|---|---|---|---|---|---|
| 1 | a, b, d | 20 | b, c, d | 39 | c | 58 | a | 77 | d |
| 2 | a, c, d | 21 | a | 40 | a | 59 | d | 78 | b |
| 3 | b, c, d | 22 | c | 41 | a, c, d | 60 | a, b | 79 | a, b, d |
| 4 | a | 23 | d | 42 | b, c, d | 61 | a, c, d | 80 | b, c |
| 5 | c, d | 24 | c | 43 | a, b, c | 62 | a, c, d | 81 | a |
| 6 | a, b, d | 25 | d | 44 | b, c, d | 63 | a | 82 | d |
| 7 | a, b, c | 26 | a, b, d | 45 | a, c, d | 64 | a, b, d | 83 | a, d |
| 8 | b | 27 | a | 46 | a, b, c | 65 | c | 84 | a |
| 9 | d | 28 | b, c, d | 47 | d | 66 | c | 85 | c |
| 10 | a, b, c | 29 | d | 48 | a, d | 67 | b, d | 86 | b |
| 11 | b, c | 30 | a, b, d | 49 | b | 68 | c | 87 | a, c, d |
| 12 | a | 31 | c | 50 | a, c | 69 | b, d | 88 | b |
| 13 | d | 32 | a | 51 | c | 70 | a | 89 | b |
| 14 | a | 33 | a, c | 52 | d | 71 | d | 90 | d |
| 15 | b, d | 34 | b | 53 | b | 72 | a | 91 | a |
| 16 | b, c, d | 35 | a, b | 54 | c | 73 | d | 92 | b |
| 17 | a, c | 36 | d | 55 | c | 74 | a, b, c | 93 | c |
| 18 | b, c | 37 | b | 56 | c | 75 | b | 94 | a |
| 19 | c, d | 38 | c | 57 | d | 76 | a |  |  |

### Bagian B – Benar/Salah

Urutan jawaban mengikuti pernyataan 1–4.

| No | Jawaban | No | Jawaban | No | Jawaban |
|---|---|---|---|---|---|
| 1 | B, S, B, B | 16 | B, S, B, B | 31 | B, S, S, S |
| 2 | S, B, B, S | 17 | S, B, B, S | 32 | B, B, B, S |
| 3 | B, S, B, B | 18 | B, B, S, B | 33 | S, B, B, B |
| 4 | B, S, B, S | 19 | B, B, S, B | 34 | B, B, S, B |
| 5 | B, B, S, B | 20 | B, B, S, S | 35 | S, B, B, B |
| 6 | B, S, B, B | 21 | B, B, B, S | 36 | B, B, S, B |
| 7 | B, S, B, S | 22 | B, B, S, B | 37 | B, S, B, S |
| 8 | S, B, S, B | 23 | B, S, B, B | 38 | B, S, B, S |
| 9 | B, B, B, S | 24 | B, B, S, S | 39 | B, B, S, B |
| 10 | B, B, S, B | 25 | B, S, B, S | 40 | S, B, S, B |
| 11 | B, B, B, S | 26 | B, B, B, S | 41 | S, B, B, S |
| 12 | S, B, B, S | 27 | B, S, B, S | 42 | B, S, B, B |
| 13 | B, B, S, B | 28 | B, B, B, S | 43 | B, S, S, B |
| 14 | B, S, B, S | 29 | B, B, S, B | 44 | B, B, B, S |
| 15 | B, S, B, B | 30 | B, B, S, B | 45 | B, S, B, S |

---

## 📊 Cara Menilai Diri Sendiri

- **Bagian A:** beri nilai 1 untuk setiap soal yang **semua** pilihannya tepat (tidak kurang dan tidak lebih).
- **Bagian B:** beri nilai 1 untuk setiap **pernyataan** yang benar (maksimal 4 per soal, total 180).
- Hitung persentase per materi memakai **Peta Cakupan Materi** di atas. Materi dengan skor di bawah 70% adalah prioritas untuk diulang di file catatan.

> 🎯 Target realistis sebelum UTS: **≥ 80%** di kedua bagian. Kalau sudah tercapai, ulangi soal yang salah sampai kamu bisa menjelaskan alasannya dengan kata-katamu sendiri.
