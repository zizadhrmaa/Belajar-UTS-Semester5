# 📘 Catatan Lengkap IF3110 – Pengembangan Aplikasi Berbasis Web

> Catatan ini disusun dari **11 file slide kuliah** (01a sampai 06-02) dan ditulis untuk orang yang **belum pernah masuk kuliah sama sekali**. Semua slide dibahas, tidak ada yang di-skip. Kalau di slide ada contoh kode berupa gambar, kodenya sudah aku ketik ulang di sini dan aku jelaskan baris per baris.

---

## 🧭 Cara Memakai Catatan Ini

1. **Baca berurutan.** Materi kuliah ini seperti tangga: HTTP dipakai di semua bab, HTML/CSS dipakai oleh JavaScript, JavaScript dipakai oleh AJAX, dan seterusnya.
2. **Perhatikan kotak-kotak khusus:**
   - 💡 **Analogi** → perumpamaan sehari-hari supaya konsep abstrak gampang dibayangkan.
   - ⚠️ **Hati-hati / Koreksi slide** → bagian yang sering bikin salah paham, atau ada typo/kesalahan kecil di slide aslinya.
   - 🎯 **Inti yang harus diingat** → ringkasan yang kemungkinan besar keluar di ujian.
3. **Di akhir tiap bab ada "Cek Pemahaman"** dengan jawaban tersembunyi (klik tulisan "Lihat jawaban").
4. Di bagian paling akhir ada **Glosarium** (kamus istilah) dan **Cheat Sheet** untuk belajar kilat sebelum ujian.

---

## 📑 Daftar Isi

0. [Gambaran Besar: Apa yang Terjadi Saat Kamu Membuka Sebuah Website?](#bab-0--gambaran-besar-apa-yang-terjadi-saat-kamu-membuka-sebuah-website)
1. [Bab 1 – Pengantar Kuliah (01a-Introductory)](#bab-1--pengantar-kuliah-01a-introductory)
2. [Bab 2 – Konsep Aplikasi Internet (01b-InternetApp)](#bab-2--konsep-aplikasi-internet-01b-internetapp)
3. [Bab 3 – Protokol HTTP (02a-HTTP-Protocol)](#bab-3--protokol-http-02a-http-protocol)
4. [Bab 4 – HTML, CSS, dan HTML5 (02b-HTML-CSS)](#bab-4--html-css-dan-html5-02b-html-css)
5. [Bab 5 – JavaScript (03-JavaScript)](#bab-5--javascript-03-javascript)
6. [Bab 6 – AJAX (03b-AJAX)](#bab-6--ajax-03b-ajax)
7. [Bab 7 – Server-Side Scripting: Go & PHP (04a-Server-Side-Scripting)](#bab-7--server-side-scripting-go--php-04a-server-side-scripting)
8. [Bab 8 – Go untuk Web Development (04b-Go-for-Web)](#bab-8--go-untuk-web-development-04b-go-for-web)
9. [Bab 9 – State Handling & Autentikasi (05-State-Handling-and-Authentication)](#bab-9--state-handling--autentikasi-05-state-handling-and-authentication)
10. [Bab 10 – Desain REST API (06-01-REST-API-Design)](#bab-10--desain-rest-api-06-01-rest-api-design)
11. [Bab 11 – GraphQL (06-02-GraphQL)](#bab-11--graphql-06-02-graphql)
12. [Glosarium (Kamus Istilah)](#-glosarium-kamus-istilah)
13. [Cheat Sheet Sebelum Ujian](#-cheat-sheet-sebelum-ujian)

---

## Bab 0 – Gambaran Besar: Apa yang Terjadi Saat Kamu Membuka Sebuah Website?

Sebelum masuk ke detail, kamu perlu punya "peta" dulu. Bayangkan kamu mengetik `https://www.itb.ac.id` di browser lalu menekan Enter. Ini yang terjadi secara garis besar (dan setiap langkah adalah materi salah satu bab):

```
 KAMU (pengguna)
   │ mengetik URL di browser
   ▼
 ┌──────────────────────────┐
 │ BROWSER (web client)     │  ← Bab 2
 │ - Chrome, Firefox, dll   │
 └──────────────────────────┘
   │ 1. Mencari alamat IP server (lewat DNS)          ← Bab 2
   │ 2. Membuka koneksi TCP (+TLS kalau HTTPS)        ← Bab 3
   │ 3. Mengirim "HTTP Request": GET / HTTP/1.1       ← Bab 3
   ▼
 ┌──────────────────────────┐
 │ WEB SERVER               │  ← Bab 2, 7, 8
 │ - Nginx, Apache, program │
 │   Go, PHP, dll           │
 │ - Menjalankan logika     │  ← Bab 7, 8 (server-side scripting)
 │ - Membaca database/file  │  ← Bab 9 (state, database, file)
 │ - Cek login & hak akses  │  ← Bab 9 (autentikasi & otorisasi)
 └──────────────────────────┘
   │ 4. Mengirim "HTTP Response": 200 OK + isi HTML   ← Bab 3
   ▼
 ┌──────────────────────────┐
 │ BROWSER                  │
 │ - Membaca HTML (struktur)│  ← Bab 4
 │ - Menerapkan CSS (gaya)  │  ← Bab 4
 │ - Menjalankan JavaScript │  ← Bab 5
 │   (interaksi, logika)    │
 │ - JS bisa minta data lagi│  ← Bab 6 (AJAX/Fetch)
 │   tanpa reload halaman   │
 └──────────────────────────┘
   │
   ▼
 Halaman tampil di layar kamu 🎉
```

Lalu, ketika aplikasi makin besar dan perlu dipakai oleh aplikasi lain (misalnya aplikasi HP), server menyediakan **API** — "pintu layanan" yang rapi dan terdokumentasi. Cara mendesain API ada di **Bab 10 (REST)** dan **Bab 11 (GraphQL)**.

💡 **Analogi besar: Restoran.**
- **Kamu** = pelanggan.
- **Browser** = pelayan yang mengantarkan pesananmu dan menyajikan makanan dengan rapi di meja.
- **HTTP** = bahasa/format "kertas pesanan" yang disepakati pelayan dan dapur.
- **Web server** = dapur.
- **Database** = gudang bahan makanan.
- **HTML** = isi makanannya (nasi, lauk), **CSS** = penataan/plating-nya (warna, posisi), **JavaScript** = "aksi" di meja (misalnya kompor kecil untuk shabu-shabu yang bisa kamu atur sendiri).
- **API** = menu resmi + aturan pemesanan, sehingga siapa pun (pelanggan langsung, ojek online, katering) bisa memesan dengan cara yang sama.

Simpan peta ini di kepala. Setiap kali bingung "ini materi buat apa sih?", kembali ke gambar di atas.

---
## Bab 1 – Pengantar Kuliah (01a-Introductory)

Bab ini isinya "aturan main" mata kuliah **IF3110 – Web-based Application Development** (Pengembangan Aplikasi Berbasis Web), Semester 1 2026/2027. Tidak terlalu teknis, tapi penting supaya kamu tahu apa yang diharapkan.

### 1.1 Tujuan Mata Kuliah (Objectives)

Setelah kuliah ini, kamu diharapkan:

| No | Tujuan (bahasa slide) | Artinya dalam bahasa sederhana |
|----|----------------------|--------------------------------|
| 1 | *Understanding on how Internet works, how Internet-based Application works* | Paham cara kerja Internet dan cara kerja aplikasi yang memakai Internet. |
| 2 | *Ability to develop an application using various technologies at server & client side* | Bisa membuat aplikasi dengan teknologi di **sisi server** (komputer penyedia layanan) dan **sisi client** (browser pengguna). |
| 3 | *Ability to design an Internet application w/ considering various qualities (e.g., security, performance, usability)* | Bisa merancang aplikasi dengan memperhatikan **kualitas**: keamanan (*security*), kecepatan (*performance*), dan kemudahan dipakai (*usability*). |

### 1.2 Aktivitas Kuliah (Course Activity)

- **Bobot: 3 SKS (credits).**
  - **3 jam kuliah tatap muka** per minggu – Senin 07.00–08.40 dan Rabu 09.00–10.00.
  - **6 jam aktivitas mandiri** per minggu (eksplorasi, mengerjakan tugas). Artinya kamu memang diharapkan belajar sendiri dua kali lipat dari jam kuliah.
- **Dosen per kelas:**
  - **K1:** Yudistira Asnar (yudis@itb.ac.id) & Bayu Samudra (bayu.samudra@itb.ac.id)
  - **K2:** Mohammad Koyyim (koyimatu@itb.ac.id) & Muhammad Zuhri Catur Candra (mzc@itb.ac.id)
  - **K3:** Riza Satria Perdana (riza@itb.ac.id) & Galuh Dipa Bharata (galuhdb@itb.ac.id)

### 1.3 Penilaian (Assessment)

- **Tugas (Assignments):** 1 tugas besar yang **berkelanjutan** (*continuous project*) – artinya satu proyek yang dikembangkan terus sepanjang semester.
- **Ujian (Exams):** Kuis, UTS (*Mid*), dan UAS (*Final*).
- **Perbandingan bobot:** Tugas lebih dominan, kira-kira **55% tugas : 45% ujian**. Bisa sedikit disesuaikan tergantung performa peserta.

### 1.4 Aturan Tugas

- Dikerjakan **berkelompok 3 orang**, dan **kelompok ditentukan oleh dosen** (bukan pilih sendiri).
- Pengumpulan **harus mengikuti prosedur dari asisten**.
- Beberapa tugas harus **dipresentasikan ke asisten**.
- Detail lengkap ada di dokumen **Term of Reference (ToR)** – semacam "spesifikasi tugas".

### 1.5 Hal Penting (Noteworthy) – Kecurangan

⚠️ **Semua bentuk kecurangan dihukum berat**, termasuk:
- Memalsukan kehadiran (titip absen).
- Menyalin pekerjaan orang lain.
- **Meminta LLM (seperti GPT/Claude atau sejenisnya) untuk menuliskan kode proyekmu.**

> Jadi catatan ini boleh kamu pakai untuk **memahami konsep**, tapi kode tugasmu harus kamu tulis sendiri.

### 1.6 Topik-Topik Sepanjang Semester

Slide menyebutkan topik berikut (catatan ini mencakup topik yang sudah ada slide-nya, yaitu sampai GraphQL):

| Kolom kiri | Kolom kanan |
|-----------|-------------|
| Konsep dasar Internet & aplikasi Internet | Web berbasis event & pemrosesan asinkron |
| HTTP | Keamanan web (*Web Security*) |
| HTML + JavaScript + CSS | Pengantar: *Performance & Security Engineering* |
| Client side: JS dan pengembangannya | Pengantar: *Web Testing* |
| Server side | Pengantar: *Usability* |
| Akses data (*Data Access*) | Pengantar: *Microservice* |
| AJAX, *Single Page Application* | |
| Web Framework: Node.js++ | |
| Cloud & Cloud Native | |
| Web Service | |
| GraphQL, APIM (*API Management*) | |

### 1.7 Referensi

- *Full Stack Web Development: The Comprehensive Guide* – Philip Ackermann (2023)
- *Web Technologies: A Computer Science Perspective* – Jeffrey C. Jackson (2006)
- *Developing Large Web Applications* – Kyle Loudon (2010)
- *HTML5: Up and Running* – Mark Pilgrim (2010)
- *JavaScript: The Definitive Guide* – David Flanagan (2011)
- Catatan dari slide: **slide dan buku hanya panduan**. Untuk referensi lengkap, gunakan Internet (misalnya MDN – developer.mozilla.org).

### 1.8 Komunikasi

- **Sinkron** (langsung) dilakukan di kelas.
- **Edunex** (https://edunex.itb.ac.id/) – Parent Class.
- **Microsoft Teams** (kode join: `cg1y8rj`) – anggotanya dosen, asisten, mahasiswa; dipakai untuk pengumuman dan diskusi.
- **Email dosen** dengan subjek berformat: `[IF3110] xxxx`.
- Cara komunikasi bisa diubah sesuai kebutuhan dosen.

---
## Bab 2 – Konsep Aplikasi Internet (01b-InternetApp)

### 2.0 Tujuan Bab Ini
Setelah bab ini kamu harus:
1. Paham **beda Internet App vs Web App**.
2. Tahu **komponen-komponen penyusun** sebuah Web App.
3. Paham **proses-proses umum** yang terjadi di dalam Web App.

---

### 2.1 Konsep Utama: Apa itu "Internet Programming"?

**Internet Programming** = membuat **aplikasi berbasis Internet** (*Internet App*).

**Internet App** adalah aplikasi yang **bagian-bagiannya tersebar di tempat berbeda** (misalnya satu bagian di HP-mu, satu bagian di server Google di Amerika), dan bagian-bagian itu **berkomunikasi lewat Internet**.

Dua kata kunci:
- **Distributed System (Sistem Terdistribusi)** → aplikasinya tersebar di banyak komputer.
- **Communication via Internet** → komunikasinya lewat jaringan Internet.

💡 **Analogi:** Aplikasi biasa (misalnya Kalkulator di laptop) itu seperti memasak sendiri di dapurmu: semuanya di satu tempat. Aplikasi Internet itu seperti pesan makanan lewat ojek online: kamu (di rumah), restoran (di tempat lain), dan driver (di jalan) harus berkoordinasi lewat aplikasi.

---

### 2.2 Karakteristik Aplikasi Internet

| Karakteristik | Penjelasan | Contoh |
|---|---|---|
| **Concurrency (konkurensi)** | Harus bisa menangani **banyak pekerjaan sekaligus**: kirim/terima data, memproses permintaan pengguna, melayani banyak client bersamaan. | Server Shopee melayani jutaan pembeli di waktu yang sama saat flash sale. |
| **Synchronize (sinkronisasi)** | Harus ada **koordinasi aktivitas** dan **pengaturan waktu/timing**. | Dua orang membeli barang terakhir di detik yang sama → hanya satu yang boleh berhasil. |
| **Exception Handling (penanganan kesalahan)** | Jika terjadi **kegagalan saat melayani satu pengguna, pengguna lain tidak boleh ikut terganggu**. | Kalau upload foto si A gagal, si B tetap bisa chat dengan normal. |

---

### 2.3 Sistem Terdistribusi (Distributed System)

Ada dua kelas utama:

#### a) Client–Server
- **Client** = pihak yang **meminta layanan** (*service requester*). Contoh: browser-mu.
- **Server** = pihak yang **menyediakan layanan** (*service provider*). Contoh: komputer milik Google.
- Perannya **tetap**: client selalu meminta, server selalu melayani.

#### b) Peer-to-Peer (P2P)
- **Setiap komponen setara** (*same level*). Setiap komponen bisa **meminta sekaligus menyediakan** layanan.
- Artinya, setiap komponen berperan **sebagai client DAN server sekaligus**.
- Contoh: BitTorrent – komputermu mengunduh potongan file dari orang lain sekaligus mengirimkan potongan file ke orang lain.

```
CLIENT–SERVER                    PEER-TO-PEER
                                    
 [Client]──┐                     [Peer]────[Peer]
 [Client]──┼──► [SERVER]            │  \    /  │
 [Client]──┘                        │   \  /   │
                                 [Peer]────[Peer]
 (server = pusat)                (semua setara)
```

---

### 2.4 Varian Client–Server: Stateless vs Stateful

Ini **SANGAT PENTING** karena akan muncul lagi di HTTP (Bab 3) dan State Handling (Bab 9).

**"State"** = *keadaan* / *ingatan* tentang interaksi sebelumnya.

| | **Stateless Client-Server** | **Stateful Client-Server** |
|---|---|---|
| Apakah server "ingat" client? | **Tidak.** Client dan server tidak menyimpan status satu sama lain. | **Ya.** Menyimpan info tentang interaksi client–server. |
| Hubungan antar-pesan | Setiap interaksi **berdiri sendiri (independen)**. | Client bisa mengirim pesan **berdasarkan konteks interaksi sebelumnya**, tanpa harus mengulang info itu di pesannya. |
| Analogi | Pelayan **minimarket**: setiap kali kamu ke kasir, kamu dianggap pembeli baru. Kalau mau sesuatu, sebutkan lengkap. | **Barista langganan**: cukup bilang "seperti biasa ya", dia sudah ingat pesananmu. |

---

### 2.5 "Rasa" Lain dari Client–Server

- **RPC (Remote Procedure Call):** interaksi client–server dipandang sebagai **memanggil fungsi (prosedur) yang letaknya di komputer lain**. Bagi programmer, rasanya seperti memanggil fungsi biasa `hitungGaji(id)`, padahal fungsi itu dijalankan di server.
- **N-tier systems (sistem N-lapis):** server "dipecah" menjadi **beberapa lapisan (tier) server**. Contoh paling umum adalah **3-tier**:

```
┌──────────────────┐      ┌──────────────────┐      ┌──────────────────┐
│ Presentation Tier│ ◄──► │  Business Tier   │ ◄──► │  Database Tier   │
│ (tampilan/UI)    │      │ (logika bisnis)  │      │ (penyimpanan)    │
└──────────────────┘      └──────────────────┘      └──────────────────┘
```
- **Presentation tier:** yang dilihat pengguna (halaman web).
- **Business tier:** aturan bisnis (misal: "diskon 10% jika belanja > 100rb").
- **Database tier:** tempat data disimpan permanen.

---

### 2.6 Untung-Rugi (Trade-Off) Client–Server

**✅ Keuntungan:**
1. Komputasi bisa **dibagi ke beberapa mesin**.
2. Client bisa **mengakses layanan dari jauh** (*remotely*).
3. Client dan server bisa **dirancang & dikembangkan secara terpisah** (tim frontend dan backend bisa kerja paralel).
4. Server bisa **memproses banyak permintaan sekaligus**.
5. Data bisa disimpan **terpusat di server** atau **tersebar di client-client**.

**❌ Kerugian:**
1. Ada **delay (jeda) komunikasi** – data harus "jalan" lewat jaringan.
2. Harus memikirkan **sinkronisasi dan proses paralel/konkuren di server** (lebih rumit).

---

### 2.7 Protokol Komunikasi

**Protokol** = **sekumpulan aturan yang disepakati** client dan server dalam berkomunikasi.

💡 **Analogi:** Protokol itu seperti tata cara surat-menyurat resmi: harus ada kop, tanggal, perihal, salam pembuka, isi, salam penutup, tanda tangan — **dengan urutan tertentu**. Kalau tidak sesuai, penerima bingung.

Ada dua jenis yang disebut di slide:

| Jenis | Tugasnya |
|---|---|
| **Application protocol** | Client dan server saling mengirim pesan dengan **format/sintaks tertentu** dan **urutan tertentu**. (Contoh: HTTP.) |
| **Transport protocol** | (1) Pesan **dipotong-potong menjadi banyak paket**, (2) paket dikirim lewat **berbagai rute jaringan**, (3) di tujuan, paket **disusun kembali** menjadi pesan asli. (Contoh: TCP.) |

🎯 **Mata kuliah ini fokus pada *application protocol*.**

#### Contoh-contoh protokol aplikasi di Internet

| Layanan | Protokol |
|---|---|
| Web | **HTTP** |
| Penamaan (mengubah nama domain → alamat IP) | **DNS** |
| E-mail | **IMAP, POP** (mengambil email), **SMTP** (mengirim email) |
| Chatting | Standar terbuka: **IRC**; Non-standar: YM, ICQ, MSN chat, AOL, dll |
| Transfer file | **FTP** |
| Terminal jarak jauh | **telnet** |
| Layanan direktori | **LDAP** |
| Monitoring jaringan | **SNMP** |
| Web service | **SOAP** |
| Suara (VoIP) | **SIP, XMPP, Asterisk** |

---

### 2.8 Internet-based App vs Web-based App

Ini sering ditanyakan!

| **Internet-based Application** | **Web-based Application** |
|---|---|
| Memakai protokol aplikasi Internet apa saja (FTP, SMTP, dll) **atau bahkan membuat protokol sendiri**. | **Memakai HTTP**. |
| Aplikasi di server **berkomunikasi langsung dengan client**. | Aplikasi berkomunikasi dengan client **melalui Web Server**. |
| Bisa berupa aplikasi **standalone** atau **komponen dari aplikasi lain**. | Umumnya **berjalan di web browser**. |
| Contoh: aplikasi FileZilla (FTP), game online dengan protokol khusus, WhatsApp. | Contoh: Gmail di browser, Edunex, Shopee versi web. |

🎯 **Web App adalah bagian (subset) dari Internet App.** Semua web app adalah internet app, tapi tidak semua internet app adalah web app.

---

### 2.9 Teknologi dalam Pengembangan Aplikasi Web

Daftar komponennya:
- **Web client** (web browser)
- **Web server**
- **URL** – Uniform Resource Locator
- **HTTP** – HyperText Transfer Protocol
- **HTML** – HyperText Markup Language
- **CSS** – Cascading Style Sheet
- **Web Programming:** CGI, server-side scripting, client-side scripting, plug-in

Mari bahas satu per satu.

#### 2.9.1 Web Client (Web Browser)
- Sebuah **perangkat lunak (software)**.
- Berjalan **di komputer client/pengguna**.
- Fungsinya: **menjelajah (navigate) web** dan **menampilkan (render) halaman web**.
- Contoh: **Chrome** (Windows), **Internet Explorer** (Windows), **Mozilla Firefox** (Windows & Linux), **Opera** (Windows & Linux), **Safari** (Mac), **lynx** (browser berbasis teks di Linux – tanpa gambar, hanya tulisan).

#### 2.9.2 Web Server
- Sebuah **software** yang berjalan **di server**.
- Fungsinya: **menyimpan dokumen web** supaya bisa diakses pengguna dari seluruh Internet.
- Contoh: **Apache** (Linux & Windows), **Nginx**, **Microsoft IIS** (Windows), **Tomcat** (untuk Java, Windows & Linux).

⚠️ Kata "server" bisa berarti dua hal: (1) **komputer fisiknya**, atau (2) **program software-nya** (Apache/Nginx). Perhatikan konteks.

#### 2.9.3 URL (Uniform Resource Locator)
- URL adalah **alamat untuk menemukan sebuah resource (file/sumber daya) di Internet**.
- Formatnya didefinisikan di **RFC 1738**. (RFC = *Request for Comments*, dokumen standar resmi Internet.)
- URL **diawali jenis protokol**. Contoh:
  - `http://www.if.itb.ac.id/` → protokol HTTP
  - `mailto:fulan@informatika.org` → membuka aplikasi email
  - `ftp://ftp.itb.ac.id/` → protokol FTP
- Contoh URL web:
  - `http://www.itb.ac.id/campus-life/index.html` → meminta file `index.html` di folder `campus-life`.
  - `http://www.google.com/search?hl=en&q=URL+RFC` → bagian setelah `?` disebut **query string**: parameter `hl=en` (bahasa Inggris) dan `q=URL+RFC` (kata kunci pencarian; `+` = spasi).
  - `http://www.indymedia.org:8081/` → `:8081` adalah **nomor port** (default HTTP adalah port 80, HTTPS port 443).

**Membedah URL:**
```
https://www.google.com:443/search?hl=en&q=URL+RFC#hasil
└─┬──┘  └──────┬─────┘└┬┘└──┬──┘└───────┬───────┘└─┬─┘
scheme       host    port  path     query string  fragment
(protokol) (nama server)  (lokasi   (parameter)   (bagian di
                           resource)               halaman)
```

#### 2.9.4 HTTP (HyperText Transfer Protocol)
- HTTP adalah **protokol komunikasi standar di web**.
- Standar HTTP/1.1 (versi lama) didefinisikan di **RFC 2616**.
- Contoh dari slide – apa yang terjadi saat browser membuka Google:

```
① Browser → Server  (HTTP request 1: minta halaman utama)
   GET / HTTP/1.1
   Accept: image/gif, image/jpeg, application/x-shockwave-flash, */*
   User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)

② Server → Browser  (HTTP response 1: kirim HTML)
   HTTP/1.1 200 OK
   Content-type: text/html

   <html>
   <head><title> Google </title></head>
   <body>Welcome to google <img src='logo.gif'></body>
   </html>

③ Browser membaca HTML, menemukan <img src='logo.gif'>, lalu meminta gambarnya
   GET /logo.gif HTTP/1.1
   Accept: image/gif, image/jpeg, ...
   User-Agent: Mozilla/4.0 (...)

④ Server → Browser  (HTTP response 2: kirim gambar)
   HTTP/1.1 200 OK
   Content-type: image/gif

   GIF89a€6÷aD&nˆ†!VR...   ← data biner gambar (terlihat "acak")
```

🎯 **Pelajaran penting:** satu halaman web = **banyak request**. Satu request untuk HTML, lalu satu request untuk **setiap** gambar, file CSS, file JS, dll.

#### 2.9.5 HTML (HyperText Markup Language)
- HTML adalah **format dokumen standar di web** – mendeskripsikan **isi dan struktur** halaman.
- Standar: HTML 4.01 (http://www.w3.org/TR/html4/) dan **HTML5** (https://html.spec.whatwg.org/multipage/).
- Contoh:
```html
<html>
<head>
    <title>My first HTML document</title>
</head>
<body>
    <p>Hello world!<br>Welcome to my <b>first</b> HTML page.
    </p>
</body>
</html>
```
- Output: "Hello world!" lalu pindah baris (`<br>` = *line break*), "Welcome to my **first** HTML page." (`<b>` = *bold*/tebal).

#### 2.9.6 CSS (Cascading Style Sheet)
- CSS adalah **mekanisme untuk mengatur gaya** (font, tata letak, warna) dokumen web.
- Standar CSS2: http://www.w3.org/TR/REC-CSS2/
- Contoh:
```html
<style type='text/css'>
    p { font-family: Arial; text-decoration: underline; }  /* semua paragraf: font Arial, bergaris bawah */
    p:first-letter { font-size: 20pt; }                     /* huruf pertama paragraf: ukuran 20pt */
    b { font-size: 20pt; }                                   /* teks tebal: ukuran 20pt */
</style>
```
- Hasilnya: teks yang sama seperti contoh HTML, tapi dengan font Arial, bergaris bawah, huruf "H" pertama besar, dan kata "first" besar.

---

### 2.10 Pemrograman TCP/IP (Internet)

- **Web programming** biasanya memakai **HTTP sebagai protokol pengantar** dan **HTML/XML sebagai format pesan**. Sifatnya:
  - **Berorientasi teks** (pesannya berupa teks yang bisa dibaca manusia).
  - **Stateless** dan berorientasi client–server.
- **Fleksibilitas lebih tinggi** bisa didapat dengan **langsung memakai protokol transport/network** (tanpa HTTP), misalnya untuk game online.
- Di jaringan IP (lapisan transport) ada dua pilihan:

| | **TCP** | **UDP** |
|---|---|---|
| Sifat | **Connection oriented** – harus "menyambung" dulu sebelum kirim data. | **Datagram oriented** – langsung kirim paket-paket tanpa membuat sambungan. |
| Keandalan | Data **dijamin sampai & urut**; kalau hilang dikirim ulang. | **Tidak dijamin** sampai/urut, tapi **lebih cepat**. |
| Analogi | **Telepon**: sambung dulu, ngobrol, saling konfirmasi "halo, dengar?" | **Kirim kartu pos**: langsung kirim, tidak tahu pasti sampai atau tidak. |
| Contoh | Web (HTTP/1.1, HTTP/2), email, FTP. | Video call, game online, DNS, **HTTP/3 (lewat QUIC)**. |

- **Mode komunikasi:**
  - **Unicast** – dari satu pengirim ke **satu** penerima.
  - **Multicast** – dari satu pengirim ke **sekelompok** penerima tertentu.
  - **Broadcast** – dari satu pengirim ke **semua** penerima di jaringan.

---

### 2.11 Web Programming – 4 Cara Membuat Web Menjadi "Dinamis"

Halaman web statis = file HTML yang isinya selalu sama. Agar isinya bisa berubah (misalnya menampilkan nama user yang login), dipakailah *web programming*:

| Cara | Di mana kode dijalankan? | Cara kerja | Contoh bahasa/teknologi |
|---|---|---|---|
| **CGI (Common Gateway Interface)** | **Server** | Web server **menjalankan sebuah program**, lalu **output program itu "dialirkan" (piped) menjadi HTTP response**. | Perl, C |
| **Server-side scripting** | **Server** | Web server **mengenali dan menjalankan script**, lalu **menyisipkan output-nya menjadi bagian dokumen web**. | PHP, ASP, JSP, Python |
| **Client-side scripting** | **Browser (client)** | Browser **mengenali dan menjalankan script**, lalu menyisipkan output ke dokumen, **memodifikasi dokumen**, atau melakukan perintah lain (misalnya **XHR** = request ke server, atau **membuka socket**). | JavaScript, JScript, VBScript |
| **Plug-in** | **Browser (client)** | Browser menjalankan program **dengan bantuan plug-in**, dan menampilkan output-nya di tempat tertentu di halaman. | Java Applet, ActiveX, Flash (semuanya sekarang sudah usang/mati) |

---

### 2.12 Evolusi Aplikasi Web

- **Web site** = **kumpulan dokumen** yang bisa diakses lewat Internet. (Statis, seperti brosur online.)
- **Web app** = **aplikasi client/server** yang **memakai web (browser) sebagai client** dan Internet untuk berkomunikasi. (Dinamis, interaktif, seperti Gmail.)
- Seiring evolusi ini: **Web Server → berkembang menjadi Web *App* Server** (server yang tidak hanya menyimpan file, tapi juga menjalankan aplikasi).

---

### 2.13 Proses Umum (Typical) dalam Web App

Diagram dari buku Shklar, *Web Application Architecture* (2003), **Figure 8.3**. Garis abu-abu horizontal memisahkan: **di atas garis = tugas Web Server**, **di bawah garis = tugas Web Application**.

```
       (WEB SERVER - di atas garis)
 ① Interpreting & routing          ⑥ Transmitting
    user requests                      responses
         │                                 ▲
         ▼                                 │
 ② Controlling access ─────────────────────│───── garis abu-abu ─────
    to the application                     │
         │      (WEB APPLICATION - di bawah garis)
         ▼                                 │
 ③ Enabling      ──► ④ Accessing and ──► ⑤ Customizing content
    data access         modifying            for presentation
         │              content              • Aggregation
         ▼                                   • Syndication
   [Data source abstraction layer]           • Personalization
    Prod DB | Config files | LDAP server     Final transformation (opsional):
    | Dev DB | ...                           XSLT → HTML, WML, VoiceXML, SMIL
         │
    Database, File system
```

Penjelasan langkah:
1. **Interpreting & routing user requests** – server membaca request dan menentukan request ini harus diteruskan ke bagian mana.
2. **Controlling access to the application** – cek: boleh tidak orang ini mengakses? (login, izin)
3. **Enabling data access** – menyiapkan akses ke sumber data lewat **data source abstraction layer** (lapisan yang menyembunyikan detail: data dari database produksi, file konfigurasi, server LDAP, database development, dll).
4. **Accessing and modifying content** – membaca/mengubah data.
5. **Customizing content for presentation** – menyesuaikan tampilan:
   - **Aggregation** = menggabungkan data dari berbagai sumber.
   - **Syndication** = mengambil/mendistribusikan konten dari/ke pihak lain (misal RSS).
   - **Personalization** = menyesuaikan dengan pengguna ("Halo, Budi!").
   - **Final transformation (opsional)** = mengubah format, misal dengan XSLT menjadi HTML, WML (untuk HP jadul), VoiceXML (suara), SMIL (multimedia).
6. **Transmitting responses** – mengirim hasil ke browser.

---

### 2.14 Web Stack

**Web stack** = **kumpulan software yang dibutuhkan untuk membangun dan menjalankan aplikasi web**. Isinya **semua lapisan software**:
- **Sistem Operasi** (Linux, Windows)
- **Web Server** (Apache, Nginx)
- **Bahasa Pemrograman** (frontend & backend)
- **Web Framework** (frontend & backend)
- **Database Server** (MySQL, MongoDB)

Contoh: **LAMP, MEAN, MERN**, dll. (Detail di Bab 7.)

⚠️ **Web Stack ≠ Web Framework.** Framework hanya **satu lapisan** di dalam stack (misalnya Laravel atau Express). Stack adalah **seluruh tumpukan**-nya.

---

### 2.15 Menafsirkan & Memproses Request (Request Context)

Browser mengirim request lewat HTTP. **Semua informasi yang dikirim browser** disebut **request context**, yang terdiri dari:
- **URL**
- **Query string**
- **Request headers**
- **Request body**
- **Informasi session**

Contoh dari slide:
```
POST /servlet/myservlet/action/subaction?param1=value1&param2=value2 HTTP/1.1
     └──────┬────────┘└──────┬───────┘└──────────────┬──────────────┘
      path aplikasi   EXTRA PATH INFO           QUERY STRING
Host: www.myhost.com                         ┐
Cookie: IDENTIFIER=xyz1234abc9876p5          ├ REQUEST HEADERS
Authorization: BASIC 9af95c84b4263bd         ┘
                                              
[request body]                               ← REQUEST BODY (untuk POST)
```
- Cookie `IDENTIFIER=xyz1234abc9876p5` dipakai server untuk **mencari data session** pengguna ini:

| SESSION ATTRIBUTES | |
|---|---|
| sessionattribute1 | value1 |
| sessionattribute2 | value2 |
| sessionattribute3 | *custom object* |

💡 Jadi cookie berisi "nomor tiket", dan server punya "lemari penitipan" (session) berisi barang-barangmu. (Dibahas tuntas di Bab 9.)

#### Tabel 8.1 – Pilihan Modul Pemrosesan di Server

| Pendekatan | Konfigurasi | Contoh URL |
|---|---|---|
| **CGI** | (1) Server menyediakan mekanisme untuk mendefinisikan **path CGI**. (2) Server memetakan **ekstensi file** di URL ke pemrosesan CGI. | `http://host/cgi-bin/script?...` <br> `http://host/.../script.cgi?...` |
| **Auxiliary processing modules** (scripting, template, hybrid) | (1) Server menyediakan mekanisme untuk **mendaftarkan modul pemroses** untuk file dengan ekstensi tertentu. (2) Dukungan bawaan bisa sudah tertanam di server (misalnya ASP di IIS). | `http://host/.../modulename.php?...` <br> `.cfm?...` <br> `.asp?...` |
| **J2EE Webapp** (Java) | (1) Server menyediakan mekanisme untuk mendefinisikan **path aplikasi**. (2) Server memetakan ekstensi file untuk diproses sebagai halaman **JSP**. | `http://host/servlet/servletname/...` <br> `http://host/webappname/servletname/...` <br> `http://host/webappname/modulename.jsp?...` |

Artinya: dari URL saja kita bisa menebak teknologi server yang dipakai (misalnya URL yang berakhiran `.php` → server memakai PHP).

---

### 2.16 Mengontrol Akses Pengguna

- **HTTP sudah menyediakan mekanisme autentikasi bawaan.**
- Cara kerjanya: **web server meminta info autentikasi** (username/password) **jika request yang datang belum menyertakan info tersebut**. (Inilah *Basic Auth*, dibahas di Bab 3 dan Bab 9.)

---

### 2.17 Arsitektur Aplikasi Web

Aplikasi web biasanya dibagi menjadi lapisan-lapisan:

1. **Presentation layer** (lapisan tampilan):
   - **Markup** – HTML/XHTML
     - **Templating system** – template HTML dengan "lubang" untuk data dinamis.
     - **Code segregation** – memisahkan kode logika dari kode tampilan (supaya tidak campur aduk).
   - **CSS**
2. **Presentation/page logic** – logika khusus tampilan (misal: "kalau belum login, tampilkan tombol Login").
3. **Business/application logic** – aturan bisnis (misal: hitung total belanja + ongkir).
4. **Persistent store** – penyimpanan permanen (database).

#### Evolusi arsitektur (gambar dari *Microsoft Application Architecture Guide*, 2nd Ed., 2009):

**a) Web Site Architecture (Lama):**
```
[Browser] ──req──► [Web Server]
          ◄─web docs──
```
Browser minta, server kirim dokumen. Selesai. Statis.

**b) Nondistributed deployment (Figure 2):** satu Web Server berisi semua lapisan.
```
──► ┌─────────────────┐       ┌─────────────────┐
    │   Web Server    │       │ Database Server │
    │ ┌─────────────┐ │       │                 │
    │ │Presentation │ │──────►│   [Database]    │
    │ │Business     │ │       │                 │
    │ │Data         │ │       │                 │
    │ └─────────────┘ │       │                 │
    └─────────────────┘       └─────────────────┘
```

**c) Distributed deployment (Figure 3):** lapisan dipisah ke server berbeda.
```
──► [Web Server: Presentation] ──► [Application Server: Business + Data] ──► [Database Server]
```
Keuntungan distributed: tiap bagian bisa di-*scale* (diperbesar) sendiri-sendiri dan lebih aman (database tidak langsung terekspos).

**d) Common Application Architecture (Figure 1):**
```
   USERS                    EXTERNAL SYSTEMS (Service Consumers)
     │                               │
┌────▼──────────────────────┐        │          ┌──────────────────────┐
│ PRESENTATION LAYER        │        │          │    CROSS-CUTTING     │
│  - UI Components          │        │          │  (berlaku di semua   │
│  - Presentation Logic Comp│        │          │   lapisan)           │
├───────────────────────────┤        │          │  • Security          │
│ SERVICES LAYER            │◄───────┘          │  • Operational       │
│  - Service Interfaces     │                   │    Management        │
│  - Message Types          │                   │  • Communication     │
├───────────────────────────┤                   │                      │
│ BUSINESS LAYER            │                   │                      │
│  - Application Façade     │                   │                      │
│  - Business Workflow      │                   │                      │
│  - Business Components    │                   │                      │
│  - Business Entities      │                   │                      │
├───────────────────────────┤                   │                      │
│ DATA LAYER                │                   │                      │
│  - Data Access Components │                   │                      │
│  - Data Helpers/Utilities │                   │                      │
│  - Service Agents         │                   └──────────────────────┘
└────┬───────────────┬──────┘
     ▼               ▼
 Data Sources     Services (layanan eksternal)
```
- **Services layer** dipakai kalau aplikasimu juga diakses oleh **sistem lain** (bukan manusia) – cikal bakal API (Bab 10).
- **Cross-cutting concerns** = urusan yang "memotong" semua lapisan: **keamanan**, **manajemen operasional** (logging, monitoring), dan **komunikasi**.

---

### 2.18 Aspek Umum di Setiap Web App

Hal-hal yang hampir selalu harus dipikirkan saat membangun web app:

| Aspek | Artinya |
|---|---|
| App request processing | Cara memproses request masuk |
| **Authentication** | Memastikan **siapa** penggunanya (login) |
| **Authorization** | Memastikan pengguna **boleh melakukan apa** |
| **Caching** | Menyimpan hasil sementara supaya lebih cepat |
| Exception Management | Menangani error dengan baik |
| Logging & Instrumentation | Mencatat aktivitas & mengukur performa |
| Navigation | Alur perpindahan antar-halaman |
| Page layout | Tata letak halaman |
| Page rendering | Proses menampilkan halaman |
| **Session management** | Mengingat pengguna antar-request |
| **Validation** | Memeriksa input pengguna valid/tidak |
| Internationalization | Dukungan multi-bahasa (i18n) |
| User Experience | Kenyamanan pengguna (UX) |

---

### 2.19 Latihan: "Di Balik Layar" Aplikasi Internet (FTP vs Web App)

**Tujuan:** memahami bagaimana client aplikasi Internet berkomunikasi dengan server.

**Alat:**
- FTP Client **FileZilla** – https://wiki.filezilla-project.org/Using
- HTTP server sederhana untuk tes: **http://neverssl.com** (sengaja tidak pakai HTTPS supaya lalu lintasnya bisa dibaca)
- FTP server gratis untuk tes: https://dlptest.com/ftp-test/ (FTP URL: `ftp.dlptest.com`, User: `dlpuser`, Password: `rNrKYTX9g7z3RgJRmxWuGHbeu` – sesuai slide)
- **Wireshark** – untuk menangkap paket jaringan secara langsung.
- **Chrome DevTools** (tab Network) – untuk melihat request/response web.

**Instruksi:** Akses layanan lewat client (HTTP/FTP), lalu tangkap lalu lintasnya dengan Wireshark.

**Pertanyaan & panduan jawaban:**

1. **Apa yang terjadi "di balik layar" (di jaringan)?**
   - Pertama ada **resolusi DNS** (nama domain → IP).
   - Lalu **TCP 3-way handshake** (SYN → SYN-ACK → ACK) untuk membuka koneksi.
   - Lalu pesan-pesan protokol aplikasi (HTTP: `GET / HTTP/1.1` ... `HTTP/1.1 200 OK`; FTP: `USER dlpuser`, `PASS ...`, `LIST`, `RETR file`).
   - Lalu koneksi ditutup (FIN).
2. **Apa beda FTP dan HTTP yang kamu temukan?**
   - **FTP memakai 2 koneksi**: *control connection* (port 21, untuk perintah seperti USER, PASS, LIST) dan *data connection* (untuk isi file). **HTTP memakai 1 koneksi** untuk perintah dan data sekaligus.
   - **FTP stateful**: setelah login, server ingat siapa kamu dan kamu sedang di folder mana (`CWD`). **HTTP stateless**: setiap request berdiri sendiri.
   - Keduanya berbasis teks, sehingga password FTP terlihat jelas di Wireshark (tidak terenkripsi!).
3. **Apa beda protokol stateful dan stateless?**
   - **Stateful**: server menyimpan konteks percakapan (contoh FTP: sesi login, direktori aktif).
   - **Stateless**: server tidak menyimpan konteks; setiap request harus membawa semua info yang dibutuhkan (contoh HTTP). Untuk "mengingat" pengguna, HTTP butuh trik tambahan seperti cookie & session (Bab 9).

---

### ✅ Cek Pemahaman Bab 2

1. Sebutkan 3 karakteristik aplikasi Internet!
2. Apa beda client-server dan peer-to-peer?
3. Apa beda Internet-based app dan Web-based app?
4. Sebutkan 4 cara web programming dan di mana kodenya dijalankan!
5. Apa beda Web Stack dan Web Framework?

<details>
<summary>Lihat jawaban</summary>

1. Concurrency, Synchronize, Exception Handling.
2. Client-server: peran tetap (client meminta, server melayani). P2P: setiap komponen setara, bisa jadi client sekaligus server.
3. Internet-based: bisa pakai protokol apa saja/protokol sendiri, berkomunikasi langsung dengan client, bisa standalone. Web-based: pakai HTTP, lewat web server, umumnya berjalan di browser.
4. CGI (server), server-side scripting (server), client-side scripting (browser), plug-in (browser).
5. Web stack = seluruh tumpukan software (OS, web server, bahasa, framework, database). Web framework = hanya satu lapisan di dalamnya.
</details>

---
## Bab 3 – Protokol HTTP (02a-HTTP-Protocol)

> Referensi slide: Leon Shklar & Richard Rosen, *Web Application Architecture* (2003) dan https://httpwg.org/specs/

HTTP adalah "bahasa" yang dipakai browser dan server untuk ngobrol. **Hampir semua bab setelah ini bergantung pada HTTP**, jadi bab ini wajib benar-benar dikuasai.

---

### 3.1 Tentang HTTP

- **Bahan dasar (basic ingredient) World Wide Web.** Tanpa HTTP, tidak ada web.
- **Sederhana** – ini **kekuatan** sekaligus **kelemahan**. Kuat karena mudah dipahami dan diimplementasikan; lemah karena fiturnya terbatas.
- **Tidak mengelola state** (*stateless*), fungsinya terbatas.
- Merupakan **Application Layer Protocol** yang **berjalan di atas TCP** (TCP menjamin data sampai utuh dan urut; HTTP tinggal fokus ke "isi percakapan").

---

### 3.2 Struktur Protokol HTTP

- Memakai paradigma **request dan response** (permintaan dan jawaban).
- **Request dan response punya struktur yang mirip:**
  1. Beberapa baris **header**
  2. **Satu baris kosong**
  3. Diikuti **message body** (isi pesan) – opsional
- **Stateless protocol:** satu transaksi = **satu request dari client + satu response dari server**. Setelah itu selesai; server tidak mengingat apa-apa.

#### Struktur umum (slide 7)

```
REQUEST
┌──────────────────────────────────────────────┐
│ METHOD /path-to-resource HTTP/version-number │  ← request line
│ Header-Name-1: value                         │  ← headers
│ Header-Name-2: value                         │
│                                              │  ← baris kosong
│ [ optional request body ]                    │  ← body (opsional)
└──────────────────────────────────────────────┘

RESPONSE
┌──────────────────────────────────────────────┐
│ HTTP/version-number  status-code  message    │  ← status line
│ Header-Name-1: value                         │  ← headers
│ Header-Name-2: value                         │
│                                              │  ← baris kosong
│ [ response body ]                            │  ← body
└──────────────────────────────────────────────┘
```

**Request Methods** (jenis permintaan): **GET, POST, HEAD**, PUT, DELETE, TRACE, OPTION(S), CONNECT, PATCH, **QUERY (baru)**.

---

### 3.3 Request–Response "Virtual Circuit"

Di dunia nyata, request dari browser **tidak selalu langsung** ke web server. Bisa melewati beberapa **proxy** (perantara):

```
                 ┌────────── request (dari sudut pandang browser: "langsung") ──────────┐
                 │                                                                       ▼
 [Browser] ──request──► [Proxy]                ──request──► [Proxy]               ──request──► [Web server]
           ◄─response── (gateway.myisp.net)    ◄─response── (firewall.neurozen.com) ◄─response── (www.neurozen.com)
                 ▲                                                                       │
                 └────────────────────────────── response ──────────────────────────────┘
```

- Proxy pertama misalnya milik **ISP** (penyedia internet), proxy kedua misalnya **firewall perusahaan**.
- Disebut **"virtual circuit"** karena bagi browser rasanya seperti terhubung langsung ke server, padahal sebenarnya melewati beberapa "pos".
- 💡 **Analogi:** Kamu kirim paket lewat JNE. Kamu cukup tahu alamat tujuan, padahal paketnya transit di beberapa gudang sortir.

---

### 3.4 Anatomi HTTP 1.0

```
 Web Client                                         Web Server
     │──────────── Connect ──────────────────────────►│
     │  Request Line      GET / HTTP/1.0              │
     │  Request Header    Host: www.google.com        │
     │  Request Delimiter CRLF (baris kosong)         │
     │                                                │
     │◄──────────── Response ─────────────────────────│
     │  Response Status   HTTP/1.0 200 OK             │
     │  Response Header   Date: Tue, 16 Feb 2010 19:21:24 GMT
     │                    Content-Type: text/html;    │
     │  Response Delimiter CRLF                       │
     │  Response Body     <html><head><title>Google!</title>...
     │◄──────────── Close ────────────────────────────│
```
- **CRLF** = *Carriage Return + Line Feed* (`\r\n`) = karakter "enter". Baris kosong (CRLF) menandai **akhir header**.
- Di HTTP 1.0, **setelah satu response, koneksi langsung ditutup (Close)**.

---

### 3.5 Header HTTP

Header adalah "keterangan tambahan" dalam pesan HTTP, berformat `Nama-Header: nilai`.

#### a) General Header (bisa ada di request maupun response)
| Header | Contoh | Arti |
|---|---|---|
| `Date` | `Date: Sun, 11 Feb 2001 22:28:31 GMT` | Waktu pesan dibuat. |
| `Connection` | `Connection: Close` | Client/server menentukan apakah koneksi **dipertahankan atau ditutup**. |

#### b) Request Header (hanya di request)
| Header | Contoh | Arti |
|---|---|---|
| `User-Agent` | `User-Agent: Mozilla/4.75 [en] (WinNT; U)` | Identitas **browser/aplikasi** pengirim. |
| `Host` | `Host: www.neurozen.com` | Nama domain yang dituju – **untuk mendukung virtual host** (lihat 3.12). |
| `Referer` | `Referer: http://www.cs.rutgers.edu/index.html` | URL halaman **asal** (halaman yang link-nya kamu klik). *(Ejaan "Referer" memang salah ketik historis di standar HTTP.)* |

#### c) Response Header (hanya di response)
| Header | Contoh | Arti |
|---|---|---|
| `Location` | `Location: http://www.mywebsite.com/Page.html` | Halaman tujuan **redirect** (dialihkan). |
| `Server` | `Server: Apache/1.2.5` | Identitas software server. |

#### d) Entity Header (menjelaskan isi body)
| Header | Contoh | Arti |
|---|---|---|
| `Content-Type` | `Content-Type: text/html` | **Jenis isi body** dalam format **MIME type** `tipe/subtipe` (misal `text/html`, `image/png`, `application/json`). |
| `Content-Length` | `Content-Length: 219` | **Panjang body** dalam byte. |
| `Last-Modified` | `Last-Modified: Sun, 11 Feb 2001 22:28:31 GMT` | Kapan konten terakhir diubah. |

---

### 3.6 Kode Status Response (Status Code)

Kode 3 digit yang memberi tahu browser/proxy **apakah request berhasil sesuai harapan**:

| Kelas | Arti | Contoh yang wajib hafal |
|---|---|---|
| **1xx** | **Informasi** (request diterima, proses berlanjut) | 100 Continue, 101 Switching Protocols |
| **2xx** | **Sukses** | **200 OK**, **201 Created**, **202 Accepted**, **204 No Content** |
| **3xx** | **Redirect (pengalihan)** | **301 Moved Permanently**, 302 Found, 303 See Other, **304 Not Modified** |
| **4xx** | **Kesalahan dari sisi client** (request-nya salah) | **400 Bad Request**, **401 Unauthorized**, **403 Forbidden**, **404 Not Found**, 405 Method Not Allowed, **409 Conflict**, **412 Precondition Failed**, **429 Too Many Requests** |
| **5xx** | **Kesalahan dari sisi server** | **500 Internal Server Error**, 502 Bad Gateway, 503 Service Unavailable |

💡 **Cara gampang ingat:** 2xx = "Siap, beres!"; 3xx = "Pindah ke sana ya"; 4xx = "Kamu yang salah"; 5xx = "Maaf, kami yang salah".

---

### 3.7 Contoh Nyata Request & Response

#### a) Contoh HTML (pakai `curl` ke google.com)
```
GET / HTTP/1.1                          ← REQUEST
Host: google.com
User-Agent: curl/8.7.1
Accept: */*

HTTP/1.1 301 Moved Permanently          ← RESPONSE
Location: http://www.google.com/        ← "pindah ke www.google.com ya"
Content-Type: text/html; charset=UTF-8  ← isinya HTML, encoding UTF-8
Content-Security-Policy-Report-Only: object-src 'none';base-uri 'self';script-src 'nonce-...' ...
Date: Sun, 30 Aug 2026 07:03:35 GMT
Expires: Tue, 29 Sep 2026 07:03:35 GMT
Cache-Control: public, max-age=2592000  ← boleh di-cache 30 hari
Server: gws                             ← Google Web Server
Content-Length: 219
X-XSS-Protection: 0
X-Frame-Options: SAMEORIGIN             ← halaman tidak boleh di-embed situs lain (anti clickjacking)

<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```
Artinya: kamu minta `google.com`, server bilang "sudah pindah permanen ke `www.google.com`". Browser otomatis mengikuti header `Location`.

#### b) Contoh JSON (dari Chrome DevTools, situs Edunex)
Di slide ditunjukkan tab **Network** di DevTools:
- Request URL ke API Edunex (`.../login/me`), **Request Method: GET**, **Status Code: 200 OK**.
- Response header **`Content-Type: application/json`** (digarisbawahi merah) → isinya **bukan HTML**, melainkan **data JSON**.
- Tab *Response* menampilkan data seperti `{"id": ..., "username": ..., "name": ..., "email": ..., "level": "STUDENT", ...}`.

🎯 **Pelajaran:** HTTP tidak hanya untuk mengirim halaman HTML. Aplikasi web modern sering memakai HTTP untuk mengirim **data JSON** ke JavaScript (inilah dasar AJAX dan API – Bab 6 & 10).

**Tips praktis:** buka situs apa saja → klik kanan → *Inspect* → tab **Network** → refresh. Kamu akan melihat semua request & response seperti di slide.

---

### 3.8 Sifat-Sifat Method HTTP: Safe, Idempotent, Cacheable

| Method | **Safe?** | **Idempotent?** | **Cacheable?** |
|---|---|---|---|
| GET | Ya | Ya | Ya |
| HEAD | Ya | Ya | Ya |
| OPTIONS | Ya | Ya | Tidak |
| TRACE | Ya | Ya | Tidak |
| PUT | Tidak | Ya | Tidak |
| DELETE | Tidak | Ya | Tidak |
| POST | Tidak | Tidak | Bersyarat* |
| PATCH | Tidak | Tidak | Bersyarat* |
| CONNECT | Tidak | Tidak | Tidak |
| QUERY | Ya | Ya | Ya |

\* POST dan PATCH bisa di-cache **hanya jika** response-nya secara eksplisit menyertakan info kesegaran (*freshness*) dan header `Content-Location` yang cocok. (Sumber: MDN.)

**Arti istilahnya:**
- **Safe (aman)** = method ini **tidak dimaksudkan untuk mengubah data** di server. Hanya "melihat".
  💡 Seperti **melihat-lihat etalase** – tidak mengubah apa pun di toko.
- **Idempotent** = **mengirim request yang sama berkali-kali hasil akhirnya sama dengan mengirim sekali**.
  💡 Seperti **menekan tombol lift lantai 5** berkali-kali – liftnya tetap ke lantai 5, tidak jadi ke lantai 25. Sedangkan **POST** itu seperti **memasukkan koin ke mesin** – masukkan 3 kali, saldo bertambah 3 kali.
  - `DELETE /foto/7` dua kali → foto 7 tetap terhapus (yang kedua mungkin dapat 404, tapi **efek akhirnya sama**). Jadi idempotent.
  - `POST /pesanan` dua kali → **dua pesanan** terbuat. Tidak idempotent.
- **Cacheable** = response-nya boleh disimpan (cache) untuk dipakai ulang.

---

### 3.9 GET vs POST (sering keluar ujian!)

| **GET** | **POST** |
|---|---|
| **Bisa di-cache** | Secara default **tidak pernah di-cache** |
| **Tersimpan di riwayat (history) browser** | **Tidak** tersimpan di history |
| **Bisa di-bookmark** | **Tidak bisa** di-bookmark |
| **Data terlihat oleh semua orang di URL** (`?nama=budi`) | Data **tidak ditampilkan di URL** (ada di body) |
| **Panjang data dibatasi** (karena URL ada batas panjangnya) | **Tidak ada batasan** panjang data (dari sisi protokol) |
| **Hanya karakter ASCII** | **Tidak ada batasan** tipe data – **data biner juga boleh** (misal upload foto) |
| **Seharusnya hanya untuk mengambil data** | Untuk **mengirim data yang akan diproses** |

⚠️ **Hati-hati:** "Data POST tidak tampil di URL" **bukan berarti POST aman/terenkripsi**. Tanpa HTTPS, isi body POST tetap bisa dibaca orang yang menyadap jaringan. Keamanan sesungguhnya datang dari **HTTPS**.

---

### 3.10 Penjelasan Setiap Method

| Method | Fungsi |
|---|---|
| **GET** | Paling sederhana. **Tidak punya request body.** Parameter ditambahkan di URL sebagai **query string** (setelah `?`). **Hanya mengambil** resource/data, **tanpa efek lain** (tidak mengubah/menghapus). |
| **POST** | **Request body berisi parameter.** Mengirim data untuk **diproses** oleh resource tertentu. URL tidak berisi data → **cocok untuk submit FORM**. |
| **HEAD** | Mirip GET, tapi server **TIDAK BOLEH mengembalikan body** – **hanya header**. Berguna untuk **cache**: cek `Last-Modified` dulu, kalau belum berubah tidak perlu download ulang. 💡 Seperti menanyakan "bukunya edisi tahun berapa?" tanpa meminjam bukunya. |
| **PUT** | **Menyimpan resource di URI tertentu.** Jika URI sudah ada resource-nya, maka resource itu **di-update (diganti)**. |
| **DELETE** | **Menghapus** resource. |
| **TRACE** | Server **mengirim balik request yang ia terima**. Gunanya: client bisa melihat **info tambahan apa yang disisipkan** ke request di tengah jalan (misalnya oleh proxy). (Untuk debugging.) |
| **OPTIONS** | Mengembalikan **method HTTP apa saja yang didukung** server untuk URL tertentu. |
| **CONNECT** | Mengubah request menjadi **terowongan (tunnel) TCP/IP transparan**. Dipakai untuk **HTTPS (SSL) lewat proxy**. |
| **PATCH** | Memodifikasi **sebagian** resource (bukan mengganti seluruhnya seperti PUT). |
| **QUERY** | **Mirip GET, tapi punya request body.** Berguna untuk pencarian yang kompleks/panjang (dibahas lagi di Bab 10). |

---

### 3.11 Pemrosesan Form

- Form HTML dengan `method="POST"` akan memakai HTTP POST dengan **`Content-Type: application/x-www-form-urlencoded`**.
- Parameter dikirim sebagai pasangan **`nama=nilai`**, dipisah `&`.
- **Upload file** memakai **`Content-Type: multipart/form-data`**.

#### Contoh 1 – Form biasa (urlencoded)
```
POST /enlighten/calais.asmx/Enlighten HTTP/1.1
Host: api.opencalais.com
Content-Type: application/x-www-form-urlencoded
Content-Length: length

licenseID=string&content=string&paramsXML=string
```

#### Contoh 2 – Upload file (multipart/form-data)
```
POST /upload HTTP/1.1
Host: example.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

john_doe
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_picture"; filename="profile.jpg"
Content-Type: image/jpeg

[Binary data of the JPEG file]
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```
- **`boundary`** = "garis pembatas" unik yang memisahkan tiap bagian (part) form. Karena file biner bisa berisi karakter apa saja, kita perlu pembatas yang tidak mungkin muncul di isi.
- Setiap bagian punya header sendiri (`Content-Disposition` berisi nama field, `Content-Type` untuk tipe file).
- 💡 Seperti mengirim **satu kardus berisi beberapa paket**, masing-masing dibungkus dan diberi label.

#### Contoh 3 – Data JSON (dipakai oleh API/AJAX)
```
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 53

{
    "name": "Alex",
    "email": "alex@example.com"
}
```

---

### 3.12 Virtual Hosting

**Virtual hosting** = **satu server (satu alamat IP) melayani banyak website/domain**. Dipakai oleh Apache, Nginx, dll.

Contoh di slide (pakai `curl`):
- `eatingbirdfood.wordpress.com` → di-*resolve* ke IP **192.0.78.12, 192.0.78.13**
- `onepiecethenewworld.wordpress.com` → di-*resolve* ke IP **yang sama: 192.0.78.12, 192.0.78.13**

Keduanya terhubung ke IP yang sama di port 80, tapi server tahu situs mana yang diminta dari **header `Host`**:
```
GET / HTTP/1.1
Host: eatingbirdfood.wordpress.com        ← ini pembedanya!
```
Server (nginx) menjawab masing-masing dengan `301 Moved Permanently` ke `https://...` (versi HTTPS situs yang benar).

💡 **Analogi:** Satu gedung apartemen (IP) punya banyak unit. Kurir (request) tahu unit mana yang dituju dari **nomor unit di alamat** (header `Host`).

🎯 Inilah alasan header **`Host` wajib** di HTTP/1.1.

---

### 3.13 Session Management (Manajemen Sesi) dengan Cookie

HTTP itu stateless, jadi bagaimana server "ingat" kamu? Dengan **cookie**:

```
① Browser → Server
   GET /movies/register HTTP/1.1
   Host: www.sample-movie-rental.com
   Authorization: ...

② Server → Browser
   HTTP/1.1 200 OK
   Set-Cookie: CLIENT=Rich; path=/movies      ← "Tolong simpan cookie ini ya"
   ...

③ Browser → Server (request berikutnya)
   GET /movies/rent-recommended HTTP/1.1
   Host: www.sample-movie-rental.com
   Cookie: CLIENT=Rich                        ← browser otomatis mengirim balik
```
- `Set-Cookie` (dari server) = menyuruh browser menyimpan cookie.
- `path=/movies` = cookie ini hanya dikirim untuk URL yang diawali `/movies`.
- `Cookie` (dari browser) = browser **otomatis** melampirkan cookie di setiap request ke situs tersebut.

💡 **Analogi:** Cookie itu seperti **cap tangan di acara konser**. Setelah masuk sekali, penjaga memberimu cap. Kalau kamu keluar-masuk, cukup tunjukkan cap, tidak perlu beli tiket lagi.

#### Contoh cookie nyata (DevTools)
- **Response header** dari server: `Set-Cookie: token=...; Path=/; Expires=Mon, 31 Aug 2026 10:59:38 GMT; HttpOnly; Secure; SameSite=None`
  - `Expires` = kapan cookie kedaluwarsa.
  - `HttpOnly` = cookie **tidak bisa dibaca JavaScript** (mencegah pencurian lewat XSS).
  - `Secure` = hanya dikirim lewat **HTTPS**.
  - `SameSite` = mengatur apakah cookie dikirim pada request lintas situs.
- **Request header** di request berikutnya: `Cookie: _ga=GA1.1...; _ga_...; token=...` (browser mengirim semua cookie yang relevan).
- Di response juga terlihat header keamanan lain seperti `Strict-Transport-Security`, `X-Content-Type-Options: nosniff`, `X-Frame-Options: SAMEORIGIN`, `X-Powered-By: Express`.

---

### 3.14 Basic Auth (Autentikasi Dasar)

```
① Browser minta halaman yang dilindungi (tanpa login)

② Server → Browser
   HTTP/1.1 401 Authenticate            ← (reason phrase standarnya "Unauthorized")
   Date: Mon, 05 Feb 2001 03:41:23 GMT
   Server: Apache/1.2.5
   WWW-Authenticate: Basic realm="Chapter3"    ← "Butuh login Basic untuk area 'Chapter3'"

   → Browser menampilkan pop-up username/password

③ Browser → Server (mengulang request dengan kredensial)
   GET /book/chapter3/index.html HTTP/1.1
   Date: Mon, 05 Feb 2001 03:41:24 GMT
   Host: www.neurozen.com
   Authorization: Basic eNCoDEd-uSErId:pASswORd
```
- `realm` = nama "area" yang dilindungi.
- Nilai setelah `Basic` adalah **`username:password` yang di-encode dengan Base64**.

⚠️ **PENTING:** **Base64 itu *encoding*, BUKAN enkripsi.** Siapa pun bisa men-decode-nya kembali dengan mudah. Contoh: `Basic YWxpY2U6cmFoYXNpYQ==` → decode → `alice:rahasia`. Karena itu **Basic Auth wajib dipakai bersama HTTPS**.

---

### 3.15 Bearer Auth

```
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIs...
```
- **"Bearer"** artinya "**pembawa**": **siapa pun yang membawa token ini dianggap berhak**.
- Token biasanya didapat setelah login (sering berbentuk **JWT**, dibahas di Bab 9).
- Di slide: contoh request ke `api-edunex.cognisia.id` dengan header `Authorization: Bearer eyJ0eXAi...` (token disensor).
- 💡 Seperti **tiket bioskop**: petugas tidak peduli siapa kamu, yang penting kamu membawa tiket yang sah. Kalau tiketmu dicuri, pencurinya bisa masuk. → **Jaga token baik-baik.**

---

### 3.16 HTTP Caching

**Cache** = **tempat penyimpanan sementara** salinan response, supaya request berikutnya tidak perlu diambil ulang dari server asal (lebih cepat, hemat bandwidth).

Jenis cache (dari MDN):
```
[Browser + cache]  ───  [Shared Cache (proxy) + cache]  ───  [Reverse Proxy + cache]  ───  [Origin + cache]
  PRIVATE CACHE           SHARED (PROXY) CACHE                 [CDN: cache cache cache]      (server asal)
                                                               SHARED (MANAGED) CACHE
```

| Jenis | Letak | Untuk siapa? |
|---|---|---|
| **Private cache** | Di **browser** pengguna | Hanya untuk **satu pengguna** itu. |
| **Shared (proxy) cache** | Di proxy (misal milik ISP/kantor) | Dipakai **bersama** banyak pengguna. |
| **Shared (managed) cache** | **Reverse proxy** dan **CDN** (Content Delivery Network) – dikelola oleh pemilik website | Banyak pengguna; dikontrol developer. |
| **Origin** | Server asal | Sumber data asli. |

💡 **Analogi:** Kamu suka baca koran X.
- **Private cache** = kamu fotokopi artikelnya dan simpan di laci sendiri.
- **Shared cache** = perpustakaan kampus berlangganan koran itu, semua mahasiswa baca dari sana.
- **CDN** = penerbit koran punya kios di setiap kota supaya kamu tidak perlu ke kantor pusat.

---

### 3.17 Browser Caching

- Browser **memeriksa header** dari HTTP response. Ada **4 header yang umum dipakai untuk caching**:
  1. **`Cache-Control`**
  2. **`ETag`**
  3. **`Expires`**
  4. **`Last-Modified`**
- Browser juga punya **Cache API** – untuk menyimpan dan mengambil request jaringan beserta response-nya (dipakai oleh Service Worker, lihat Bab 4).

Di DevTools (slide) terlihat kolom **Size**:
- `(memory cache)` = diambil dari **cache di RAM** (paling cepat).
- `(disk cache)` = diambil dari **cache di hard disk**.
- Status **`304`** = "**Not Modified**": browser bertanya ke server "apakah sudah berubah?", server jawab "belum, pakai yang lama saja" → tanpa body, hemat data.
- Tab **Application → Cache storage** menunjukkan file-file yang disimpan oleh Cache API (contoh: `workbox-precache` di situs pasti.kemenkum.go.id).

---

### 3.18 Header `Cache-Control`

`Cache-Control` adalah header untuk menentukan **kebijakan caching**, baik di **request client** maupun di **shared cache** (CDN, proxy). Contoh: `cache-control: public, max-age=31536000`.

| Directive | Arti |
|---|---|
| **`public`** | Resource **boleh di-cache oleh cache mana pun** (browser, proxy, CDN). |
| **`private`** | Resource **khusus untuk satu pengguna** – boleh di-cache, tapi **hanya di perangkat client** (bukan di proxy/CDN). Contoh: halaman profil. |
| **`no-cache`** | Response **boleh disimpan**, tapi **WAJIB divalidasi ke server asal sebelum dipakai ulang**. |
| **`no-store`** | **Tidak boleh disimpan sama sekali** oleh cache apa pun. Contoh: data rekening bank. |
| **`max-age=N`** | Response dianggap **masih segar (fresh) selama N detik** sejak dibuat. `max-age=31536000` = 1 tahun. |

⚠️ **Jebakan klasik:** `no-cache` **BUKAN** berarti "jangan di-cache"! Artinya "**simpan boleh, tapi tanya dulu ke server sebelum pakai**". Yang benar-benar "jangan simpan" adalah **`no-store`**.

---

### 3.19 Header Cache Lainnya

| Header | Arti | Contoh |
|---|---|---|
| **`ETag`** | **Identitas (sidik jari) versi tertentu** dari sebuah resource. Kalau isinya berubah, ETag-nya berubah. | `Etag: "a8e51e87b9fe7dc09becf56f67707250"` |
| **`Expires`** | **Tanggal/waktu** setelah itu response dianggap **kedaluwarsa**. | `Expires: Sun, 30 Aug 2026 02:31:39 GMT` |
| **`Last-Modified`** | Tanggal/waktu **server asal yakin resource terakhir diubah**. | `Last-Modified: Sun, 30 Aug 2026 01:15:36 GMT` |

**Cara kerja validasi (revalidation):**
```
Request 1:  GET /logo.png
Response:   200 OK, ETag: "v7", body = [gambar]          ← browser simpan gambar + ETag

Request 2:  GET /logo.png
            If-None-Match: "v7"                          ← "aku punya versi v7, masih sama?"
Response:   304 Not Modified (tanpa body)                ← "masih sama, pakai yang kamu punya"
```
(Dengan `Last-Modified`, browser mengirim `If-Modified-Since: <tanggal>`.)

---

### 3.20 Persistent Connection

- **HTTP 1.0** membuka **koneksi TCP terpisah untuk setiap request** → **tidak efisien** dan **lambat (latensi tinggi)**, karena setiap koneksi TCP butuh *handshake*.
- **HTTP 1.1** memakai **persistent connection** – satu koneksi bisa dipakai **untuk beberapa request**.
- Header terkait:
  ```
  Connection: Close        ← tutup koneksi setelah response ini
  Connection: Keep-Alive   ← pertahankan koneksi untuk request berikutnya
  ```

💡 **Analogi:** HTTP 1.0 = setiap mau tanya satu hal, kamu menelepon, tanya, lalu tutup telepon. Mau tanya lagi? Telepon ulang. HTTP 1.1 = telepon sekali, tanya banyak hal, baru tutup.

---

### 3.21 HTTP 1.1 vs HTTP 1.0

HTTP 1.1 menambahkan:
1. **Method tambahan:** PUT, DELETE, TRACE, CONNECT (HTTP 1.0 hanya GET, HEAD, POST).
2. **Header tambahan** (misalnya `Host` yang wajib, sehingga virtual hosting bisa jalan).
3. **Transfer Coding – chunked encoding:** server bisa mengirim body **sepotong-sepotong** (chunk) tanpa harus tahu total panjangnya di awal.
4. **Persistent Connections** – karena koneksi tidak langsung ditutup, **`Content-Length` jadi penting** (supaya client tahu kapan satu response selesai).
5. **Request Pipelining** – client bisa mengirim beberapa request berturut-turut **tanpa menunggu** response sebelumnya (tapi response tetap harus datang **berurutan**).

---

### 3.22 Masalah HTTP 1.1 "Zaman Dulu"

HTTP 1.1 dipakai sejak **1997/1999**. Masalahnya:
- Ada `Connection: keep-alive`, **tapi** muncul **Head-of-Line (HOL) Blocking**: request berikutnya harus menunggu request di depannya selesai.
  💡 Seperti antre di kasir dengan satu jalur: kalau orang di depan belanja 200 item, kamu yang cuma beli 1 permen ikut menunggu.
- Karena itu browser tetap membuka **N koneksi TCP per origin** (biasanya 6).
- **Tidak ada kompresi header** – header yang sama (User-Agent, Cookie, dll) dikirim berulang-ulang di setiap request.

Karena request dianggap "jahat" (mahal), developer membuat **banyak "hack"** untuk mengurangi jumlah request:
| Hack | Penjelasan |
|---|---|
| **Spriting of images** | Menggabungkan banyak ikon kecil jadi **satu gambar besar**, lalu menampilkan potongannya dengan CSS. |
| **Resource inlining** | Menyisipkan CSS/JS/gambar langsung ke dalam HTML (misal gambar sebagai `data:` URI). |
| **Concatenation of files** | Menggabungkan banyak file JS/CSS menjadi satu file. |
| **Domain sharding** | Membagi resource ke **banyak subdomain** (img1.situs.com, img2.situs.com) supaya browser bisa membuka lebih banyak koneksi paralel. |
| **CDNs** | Menaruh file di server-server yang dekat dengan pengguna. |

---

### 3.23 HTTP/2

HTTP/2 dibuat untuk **mengatasi masalah performa HTTP 1.1**:

| Fitur | Penjelasan |
|---|---|
| **Binary, bukan teks** | Pesan dikodekan dalam format biner (lebih efisien dan mudah di-*parse* mesin; tidak bisa dibaca langsung oleh manusia). |
| **Multiplexed, bukan sinkron** | Banyak request/response bisa berjalan **bersamaan dalam satu koneksi TCP**, potongan-potongannya (*frames*) diselang-seling. |
| **Flow control** | Mengatur laju data agar penerima tidak kebanjiran. |
| **Stream prioritization** | Request bisa diberi **prioritas** (misal CSS lebih penting dari gambar di bawah halaman). |
| **Header compression** | Header dikompres (dengan **HPACK**). |
| **Server push** | Server bisa **mengirim resource sebelum diminta** (misal: browser minta HTML, server sekalian kirim CSS-nya). *(Catatan: fitur ini belakangan dinonaktifkan oleh browser besar karena jarang efektif.)* |

#### Multiple TCP Connection vs Single Connection
- Banyak request HTTP/1 secara paralel **butuh banyak koneksi TCP**.
- Kebanyakan browser membuka **hingga 6 koneksi per domain** secara paralel.
- Kalau batas tercapai → **terjadi blocking**. Selain itu, **koneksi TCP itu mahal**.
- **Domain sharding** dipakai untuk mengakali batas ini.
- HTTP/1 bisa memakai *persistent connection* (`Connection: keep-alive`), **tapi** menimbulkan **head-of-line blocking**: request saat ini harus selesai sebelum request berikutnya bisa dikirim.
- **HTTP/2 memungkinkan satu koneksi TCP me-*multiplex* banyak *stream* resource** dalam bentuk **frame-frame yang bisa diselang-seling**.

```
HTTP/1.1 (3 koneksi TCP terpisah)            HTTP/2 (1 koneksi TCP, banyak stream)
Client                Server                 Client                         Server
Req2 GET /styles.css ═TCP1═► Req2            Req2 ┐                        ┌ Req2
Req3 GET /script.js  ═TCP2═► Req3            Req3 ├═ [S9 hdr][S7 hdr][S5 hdr] ═► Req3
Req4 GET /image.jpg  ═TCP3═► Req4            Req4 ┘ ◄═ [S5 hdr][S7 hdr][S5 body][S7 body][S9 hdr][S9 body] ═ Req4
                                                     (frame tiap stream diselang-seling)
```

---

### 3.24 HTTPS

- **HTTPS = HTTP di atas TLS di atas TCP.** (TLS = *Transport Layer Security*, lapisan enkripsi.)
- Karena itu, **sebelum data HTTP dikirim**, harus dilakukan:
  1. **TCP 3-way handshake** (SYN → SYN ACK → ACK) – dalam contoh memakan **56 ms**.
  2. **Pertukaran sertifikat & kunci TLS** (ClientHello → ServerHello, Certificate, ServerHelloDone → ClientKeyExchange, ChangeCipherSpec, Finished → ChangeCipherSpec, Finished) – dalam contoh memakan **112 ms**.
  3. Baru **Application Data** (HTTP request/response) dikirim (sekitar ms ke-168 sampai 224).

```
Sender (browser)                         Receiver (server)
   SYN ──────────────────────────────────►          0 ms   ┐
       ◄───────────────────────────────── SYN ACK   28 ms  │ TCP: 56 ms
   ACK ──────────────────────────────────►          56 ms  ┘
   ClientHello ──────────────────────────►                 ┐
       ◄────── ServerHello, Certificate, ServerHelloDone   │
   ClientKeyExchange, ChangeCipherSpec, Finished ──►       │ TLS: 112 ms
       ◄────────────── ChangeCipherSpec, Finished  168 ms  ┘
   Application Data ─────────────────────►          196 ms
       ◄──────────────────────────────── Application Data  224 ms
```
(Sumber diagram: https://hpbn.co/ – *High Performance Browser Networking*.)

🎯 **Pelajaran:** HTTPS lebih aman tapi ada **biaya waktu** di awal koneksi. Karena itu, memakai ulang koneksi (persistent connection, HTTP/2) sangat penting.

---

### 3.25 Mengapa HTTP/2?

- **Performa itu penting** – HTTP/2 **mengurangi dampak latensi** pada aplikasi web.
- **TLS jadi default** – HTTP/2 **"mencicil" biaya TLS** untuk seluruh aplikasi (cukup satu koneksi TLS untuk semua request).
- **Memungkinkan cara baru mengembangkan web** (tidak perlu hack seperti spriting/sharding).
- **Peningkatan performa yang terasa** oleh pengguna.
- **Bekerja dengan Internet saat ini.**
- **Tetap kompatibel dengan konten yang sudah ada** (method, header, status code tetap sama).

#### Apa itu HTTP/2?
- Memakai **SPDY** (protokol buatan Google, dibaca "speedy") sebagai **draft pertamanya**.
- **Utamanya didorong oleh performa**, tapi juga mencakup **keamanan (security)** dan **keandalan (reliability)**.

#### Tabel HTTP 1.x vs HTTP/2

| HTTP 1.x | HTTP/2 |
|---|---|
| Protokol **teks** | Protokol **biner** |
| Semantik verb & resource | Semantik verb & resource **(sama)** |
| Header dan body | Header dan body **(sama)** |
| **Header diulang-ulang** | **Header dioptimasi** |
| SSL opsional | SSL opsional (**tapi hampir selalu dipakai**) |
| **Satu koneksi – satu request** | **Satu koneksi – banyak request** |
| – | **Kompresi** |

#### HTTP/2: Connections dan Streams

| HTTP/1.1 – **Request = Connection** | HTTP/2 – **Request = Stream** |
|---|---|
| Setiap request butuh **koneksi TCP khusus** | Satu koneksi TCP bisa punya **banyak stream (request)** |
| Response datang **berurutan** per koneksi | Response bisa datang **tidak berurutan**; server bisa mengoptimasi |
| Setiap koneksi butuh **setup + slow start** | Tidak perlu setup koneksi untuk stream baru, **tanpa slow start** |
| Aplikasi melihat "connections" | Stream direpresentasikan sebagai "connections" ke aplikasi |

(*Slow start* = mekanisme TCP yang memulai pengiriman data pelan-pelan lalu makin cepat. Koneksi baru selalu "lambat di awal".)

---

### 3.26 Kompresi Header: HPACK (HTTP/2)

Masalah: header seperti `user-agent`, `accept-encoding`, `cookie` dikirim **berulang-ulang** di setiap request. HPACK mengatasinya dengan **tabel**:

- **Static Table** – daftar header umum yang **sudah disepakati sejak awal** (sudah tertanam di standar). Contoh: `:path: /`, `accept-encoding: gzip,deflate`, `:method: GET`, `:scheme: http`.
- **Dynamic Table** – tabel yang **tumbuh selama koneksi berlangsung**, berisi header yang pernah dikirim (contoh: `:authority: www.example.com`).

Setiap header diekspresikan sebagai salah satu dari:
- **Indexed:** cukup **kirim nomor indeks** yang merujuk ke header lengkap di static/dynamic table. (Super hemat!)
- **Literal:**
  - **Nama** header berupa referensi ke entri static/dynamic table, **atau** string ter-*encode* Huffman.
  - **Nilai** header berupa string ter-*encode* **Huffman** (kompresi teks).
  - **Perilaku indexing:**
    - **Tambahkan** ke dynamic table (supaya request berikutnya cukup kirim indeks).
    - **Jangan tambahkan** ke dynamic table.
    - **JANGAN PERNAH tambahkan** ke dynamic table (untuk data sensitif; aturan ini harus dipertahankan juga oleh perantara/proxy).

💡 **Analogi:** Seperti pesan di warung langganan: "Bu, **yang nomor 3**" (indexed) daripada menyebut lengkap "nasi, ayam goreng, sambal ijo, es teh manis".

---

### 3.27 Keandalan Request (Request Reliability)

- Di HTTP lama, **tidak ada cara aman untuk mengulang (retry)** request ketika terjadi error. Masalahnya: mungkin server **sudah sempat memproses sebagian** sebelum error, sehingga mengulang bisa menimbulkan efek yang tidak diinginkan (misal: transfer uang dobel).
- HTTP/2 menyediakan **2 mekanisme** untuk menjamin ke client bahwa sebuah request **belum diproses** (sehingga aman diulang):
  1. **`GOAWAY`** frame – memberi tahu **nomor stream tertinggi yang sudah diproses**. Stream dengan nomor lebih besar dijamin belum diproses.
  2. **`REFUSED_STREAM`** – kode error yang bisa dimasukkan dalam frame **`RST_STREAM`**, menandakan stream itu ditolak sebelum diproses.

---

### 3.28 HTTP/2 vs HTTP/3

| HTTP/2 | HTTP/3 |
|---|---|
| **TCP + TLS** | **QUIC di atas UDP**; **TLS 1.3 terintegrasi** |
| Multiplexing **di lapisan HTTP** | Stream dikelola **di lapisan transport** (QUIC) |
| **Satu paket TCP hilang bisa menahan SEMUA stream** | Paket hilang **terutama hanya menahan stream yang terdampak** |
| Kompresi header: **HPACK** | Kompresi field: **QPACK** |
| Koneksi terikat ke **alamat IP dan port** | **Connection ID** memungkinkan **migrasi jalur** (pindah jaringan tanpa putus) |

#### Tumpukan protokol (dari slide):
```
  HTTP/1.1     HTTPS        HTTP/2              HTTP/3
 ┌────────┐  ┌────────┐  ┌──────────────┐  ┌──────────────┐
 │  HTTP  │  │  HTTP  │  │     HTTP     │  │     HTTP     │
 │        │  ├────────┤  ├──────┬───────┤  ├──────┬───────┤
 │        │  │  TLS   │  │HPACK │Stream │  │QPACK │Stream │
 ├────────┤  ├────────┤  ├──────┴───────┤  ├──────┴───────┤
 │  TCP   │  │  TCP   │  │   TLS 1.2+   │  │ QUIC (TLS1.3+│
 │        │  │        │  ├──────────────┤  │  di dalamnya)│
 │        │  │        │  │     TCP      │  ├──────────────┤
 │        │  │        │  │              │  │     UDP      │
 ├────────┤  ├────────┤  ├──────────────┤  ├──────────────┤
 │   IP   │  │   IP   │  │      IP      │  │      IP      │
 │  MAC   │  │  MAC   │  │     MAC      │  │     MAC      │
 └────────┘  └────────┘  └──────────────┘  └──────────────┘
```

#### HTTP/3 berjalan di atas QUIC
- **Semantik HTTP tidak berubah:** method, status, field (header), dan body **tetap sama**. Yang berubah hanya "cara pengirimannya".
- **QUIC** menyediakan **multiplexing stream** dan **flow control per stream**.
- **Handshake QUIC menggabungkan parameter transport dengan TLS 1.3** (jadi lebih cepat dari TCP lalu TLS).
- **Paket QUIC dibawa dalam datagram UDP.**
- **ALPN** (negosiasi protokol) memakai token **`h3`**; server bisa **mengiklankan** dukungan HTTP/3 lewat header **`Alt-Svc`** (contohnya terlihat di screenshot slide Cache-Control, Bab 3.18: `alt-svc: quic=":443"; ma=2592000; v="39,38,37,36,35"`).

#### Mengapa HTTP/3?
- HTTP/2 membawa banyak stream lewat **satu aliran byte TCP**, dan **TCP harus mengirim byte secara berurutan**.
- Akibatnya, **satu paket hilang bisa menunda semua transaksi** di koneksi HTTP/2 tersebut (*TCP head-of-line blocking*).
- **QUIC memulihkan data per stream**, jadi **stream lain yang tidak terkait bisa terus berjalan**.
- (Catatan: **congestion control** tetap bekerja di **level koneksi**.)

💡 **Analogi:** HTTP/2 di atas TCP = beberapa mobil (stream) dalam **satu gerbong kereta**: kalau satu mobil bermasalah, seluruh kereta berhenti. HTTP/3 di atas QUIC = beberapa mobil di **jalan tol berlajur banyak**: kalau satu mogok, hanya lajurnya yang tersendat.

#### Ilustrasi evolusi (slide video https://www.youtube.com/watch?v=UMwQjFzTQXw):
- **HTTP 1.0:** TCP SYN → TCP SYN+ACK → TCP ACK → HTTP Request → HTTP Response, lalu koneksi ditutup. (*HTTP over TCP*)
- **HTTP 1.1:** satu koneksi TCP, request/stream dikirim berurutan.
- **HTTP 2.0:** *persistent connection*, banyak request dalam satu koneksi (open/close lebih jarang).
- **HTTP 3.0 on QUIC:** di atas **UDP**, stream-stream independen.

---

### 3.29 QPACK – Kompresi Field untuk HTTP/3

- **QPACK dirancang untuk stream QUIC yang bisa datang tidak berurutan.** (HPACK mengandalkan urutan TCP, jadi tidak cocok.)
- **Static table** menyimpan field umum; **dynamic table** tumbuh selama koneksi.
- **Encoder stream dan decoder stream** membawa instruksi kompresi **secara terpisah** dari data.
  - Referensi ke dynamic table yang **belum diterima** bisa **memblokir satu stream** tertentu.
  - Encoder harus **menyeimbangkan efisiensi kompresi vs risiko blocking**.

---

### 3.30 Handshake Lebih Cepat dan Migrasi Koneksi

- Handshake QUIC **menggabungkan negosiasi kriptografi dan transport** → koneksi baru bisa mengirim data aplikasi **lebih cepat** dibanding urutan TCP lalu TLS.
- Koneksi yang **pernah dipakai sebelumnya** bisa mengirim data **0-RTT** (*zero round-trip time* – langsung kirim data di paket pertama).
  - ⚠️ **Data 0-RTT bisa di-*replay*** (diputar ulang oleh penyerang) → **hanya pakai untuk operasi yang aman diulang** (misal GET), jangan untuk transfer uang.
- **Connection ID** menjaga koneksi tetap hidup **saat alamat IP atau port berubah** (misal HP-mu pindah dari WiFi ke data seluler – video call tidak putus).

### 3.31 Adopsi HTTP/3 Masih Butuh Fallback

- **Iklankan dukungan h3** lewat `Alt-Svc` atau mekanisme penemuan lain.
- **Siapkan infrastruktur:** UDP, TLS 1.3, load balancer, observability (monitoring), dan rate limiting.
- **Beberapa jaringan memblokir/membatasi UDP** (misalnya jaringan kantor tertentu).
- **Kalau QUIC gagal, client harus mencoba versi HTTP berbasis TCP.**
- **Tetap sediakan HTTP/2 atau HTTP/1.1**, dan **ukur hasilnya di jaringan nyata**.

---

### ✅ Cek Pemahaman Bab 3

1. Apa arti "HTTP itu stateless"?
2. Apa beda method *safe* dan *idempotent*? Beri contoh method yang idempotent tapi tidak safe.
3. Apa beda `no-cache` dan `no-store`?
4. Mengapa header `Host` penting?
5. Apa masalah utama HTTP/1.1 yang diselesaikan HTTP/2, dan masalah HTTP/2 apa yang diselesaikan HTTP/3?
6. Basic Auth mengirim `username:password` dalam Base64. Apakah itu aman tanpa HTTPS?

<details>
<summary>Lihat jawaban</summary>

1. Setiap transaksi (1 request + 1 response) berdiri sendiri; server tidak menyimpan ingatan tentang request sebelumnya.
2. Safe = tidak dimaksudkan mengubah data. Idempotent = diulang berkali-kali efek akhirnya sama dengan sekali. Contoh idempotent tapi tidak safe: **PUT** dan **DELETE**.
3. `no-cache` = boleh disimpan tapi wajib divalidasi ke server sebelum dipakai ulang. `no-store` = tidak boleh disimpan sama sekali.
4. Untuk virtual hosting – satu IP melayani banyak domain; server tahu domain yang diminta dari `Host`.
5. HTTP/2 mengatasi HOL blocking di level HTTP (1 request per koneksi) lewat multiplexing stream di satu koneksi + kompresi header. HTTP/3 (QUIC/UDP) mengatasi HOL blocking di level TCP (satu paket hilang menahan semua stream).
6. Tidak aman. Base64 hanya encoding, mudah di-decode. Harus pakai HTTPS.
</details>

---
## Bab 4 – HTML, CSS, dan HTML5 (02b-HTML-CSS)

> Referensi: standar HTML adalah **"living document"** (dokumen hidup, terus diperbarui) di https://html.spec.whatwg.org/multipage/. Referensi tambahan: https://developer.mozilla.org/en-US/ (MDN) dan https://weblab.mit.edu/schedule

---

### 4.1 Apa itu HTML?

**HTML = HyperText Markup Language** – bahasa yang dipakai browser untuk **mendeskripsikan ISI dan STRUKTUR** halaman web.

- **HyperText** = teks yang bisa berisi **link** ke dokumen lain.
- **Markup** = "penanda". Kita menandai bagian-bagian teks dengan **tag**, misalnya "ini judul", "ini paragraf", "ini gambar".
- ⚠️ HTML **bukan bahasa pemrograman** (tidak ada if/loop). HTML adalah **bahasa markup**.

#### Analogi HTML & CSS (dari weblab.mit.edu)
- Gambar di slide: **burung tanpa bulu** (= HTML saja) vs **burung berbulu warna-warni** (= HTML + CSS).
- Gambar kedua: **kerangka tulang** (= HTML: `<head>`, `<body>`) vs **orang berjas** (= CSS: `head{ }`, `body{ }`).
- 🎯 **HTML = kerangka/struktur. CSS = penampilan/pakaian.** (Dan nanti JavaScript = otot/gerakan.)

### 4.2 HTML = Kotak-Kotak Bersarang (Nested Boxes)

Slide menampilkan halaman **TikTok Explore** yang diberi garis merah di setiap elemen: logo, kotak pencarian, tombol Log in, menu (For You, Explore, Following, LIVE, Profile), kategori (All, Singing & Dancing, Comedy, ...), dan kotak-kotak video.

🎯 **Setiap halaman web sebenarnya adalah kotak di dalam kotak di dalam kotak.** Halaman → bagian kiri (menu) & kanan (konten) → di dalam konten ada baris kategori & grid video → di dalam grid ada kotak-kotak video.

---

### 4.3 Struktur Dokumen HTML

File `hello.html`:
```html
<!DOCTYPE html>          <!-- Document Type Declaration -->
<html>                   <!-- Awal dokumen HTML -->
  <head>                 <!-- Info TENTANG dokumen (tidak tampil di halaman) -->
    <title>Title!</title><!-- Judul di tab browser -->
  </head>
  <body>                 <!-- ISI yang tampil di halaman -->
    <h1>Heading!</h1>    <!-- Judul besar -->
    <p>Paragraph!</p>    <!-- Paragraf -->
  </body>
</html>
```
- **`<!DOCTYPE html>`** = **Document Type Declaration**. Fungsinya: memastikan halaman dirender memakai **HTML terbaru (HTML5)**, **bukan mode "fallback" lama** yang aneh-aneh (*quirks mode*).
- Hasil di browser: tab bertuliskan **"Title!"**, halaman berisi **"Heading!"** (besar, tebal) dan **"Paragraph!"**.

### 4.4 Anatomi Elemen HTML

```
     nama elemen   atribut  nilai
           │          │      │
         <elname     foo  = "bar">   ← Opening tag (tag pembuka)
           content goes here          ← Konten
         </elname>                    ← Closing tag (tag penutup, ada garis miring "/")
```

⚠️ **Elemen dan tag itu BERBEDA:**
- **Tag** = penanda **di kode sumber** yang **memulai atau mengakhiri** elemen (`<p>` dan `</p>`).
- **Elemen** = bagian dari **DOM** (*Document Object Model*) – model dokumen yang dipakai browser untuk menampilkan halaman. Elemen = tag pembuka + konten + tag penutup, yang sudah "hidup" di memori browser.

### 4.5 Elemen-Elemen Dasar HTML

| Elemen | Fungsi |
|---|---|
| `html` | **Akar (root)** dokumen HTML |
| `head` | **Informasi tentang** dokumen |
| `body` | **Badan/isi** dokumen |
| `h1`, `h2`, `h3`, … `h6` | **Judul (header)** – h1 paling besar/penting |
| `p` | **Paragraf** |
| `div` | **Bagian generik bertipe block** |
| `span` | **Bagian generik bertipe inline** |

### 4.6 Tipe Elemen: Block vs Inline

| **Block-level** | **Inline** |
|---|---|
| **Selalu mulai di baris baru.** | **Tidak** mulai di baris baru. |
| Browser otomatis memberi **jarak (margin) sebelum & sesudahnya**. | – |
| **Selalu mengambil lebar penuh** yang tersedia (melebar ke kiri-kanan sejauh mungkin). | **Hanya selebar yang dibutuhkan.** |
| Contoh: `<p>`, `<div>`, `<h1>`, `<ul>` | Contoh: `<span>`, `<a>`, `<b>`, `<img>` |

⚠️ **Elemen inline TIDAK BOLEH berisi elemen block.** (Contoh salah: `<span><div>...</div></span>`.)

💡 **Analogi:** Block = **paragraf di buku** (selalu baris baru, selebar halaman). Inline = **kata yang di-highlight stabilo** di tengah kalimat (tetap di baris yang sama, selebar katanya saja).

### 4.7 Contoh Elemen

**a) Link (tautan)**
```html
<a href="https://itb.ac.id">Link to itb.ac.id!</a>
```
- `a` = *anchor*. Atribut **`href`** = alamat tujuan. Tampil sebagai teks biru bergaris bawah "Link to itb.ac.id!".

**b) Gambar**
```html
<img src="pusheen.gif"></img>   <!-- ❌ SALAH (dicoret X di slide) -->
<img src="pusheen.gif" />       <!-- ✅ BENAR -->
```
- `<img>` adalah **void element** (elemen kosong) – **tidak punya konten dan tidak punya tag penutup**. Atribut `src` = sumber file gambar. (Di HTML5, `<img src="pusheen.gif">` tanpa `/` juga sah.)

**c) List (daftar)**
| Tag | Arti |
|---|---|
| `<ol>` | **Ordered List** – daftar bernomor (1, 2, 3…) |
| `<ul>` | **Unordered List** – daftar berpoin (bullet) |
| `<li>` | **List Item** – satu butir dalam daftar |
```html
<ul>
  <li>Apel</li>
  <li>Jeruk</li>
</ul>
```

**d) div & span**
| Tag | Arti |
|---|---|
| `<div>` | **Bagian block** dalam dokumen |
| `<span>` | **Bagian inline** dalam dokumen |
```html
<div id="div-0">
  <h2>Div 1!</h2>
  <p>Paragraph</p>
</div>

<div id="div-1">
  <h2>Div 2!</h2>
  <p>Paragraph!!</p>
</div>

<span id="span-0">This text is inside a span element.</span>
<span id="span-1">This text is inside another span element.</span>
```
Hasilnya: dua `div` tampil sebagai **dua kotak bertumpuk vertikal**, masing-masing selebar layar. Dua `span` tampil **bersebelahan dalam satu baris**.

### 4.8 Elemen Form

Form = tempat pengguna mengisi data. Elemen-elemennya:
`<input>`, `<label>`, `<select>` (dropdown), `<textarea>` (teks panjang), `<button>`, `<fieldset>` (kelompok field), `<legend>` (judul fieldset), `<datalist>` (daftar saran untuk input), `<output>` (hasil perhitungan), `<option>` (pilihan dalam select), `<optgroup>` (kelompok option).

Contoh:
```html
<!DOCTYPE html>
<html>
<body>
<h2>Sample Form - input Element</h2>
<form action="/action_page.php">               <!-- data dikirim ke /action_page.php -->
  <label for="fname">Nama Lengkap:</label><br> <!-- label untuk input ber-id "fname" -->
  <input type="text" id="fname" name="fname"><br><br>
  <input type="submit" value="Submit">         <!-- tombol kirim -->
</form>
</body>
</html>
```
- **`action`** = URL tujuan pengiriman data.
- **`name`** = nama field yang akan dikirim ke server (`fname=...`).
- **`for`** di label harus sama dengan **`id`** input (supaya klik label = fokus ke input).
- ⚠️ Di slide tertulis `id="name"` sedangkan label `for="fname"` – seharusnya **id-nya `fname`** supaya cocok.

### 4.9 Masih Banyak Elemen Lain!
Referensi lengkap:
- https://html.spec.whatwg.org/multipage/#toc-semantics
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element

---

### 4.10 Apa itu CSS?

**CSS = Cascading Style Sheets** – **aturan-aturan yang memberi tahu browser bagaimana tampilan sesuatu**.

- **CSS = daftar deskripsi** ("*making things look pretty*"). CSS dipakai untuk memberi **deskripsi atau aturan tentang bagaimana elemen HTML ditampilkan** di browser.
- Gambar slide: **HTML saja** = kerangka tulang; **HTML+CSS** = orang berkemeja dan berdasi.

### 4.11 Bagaimana CSS Bekerja?

Saat browser menampilkan halaman, ia **menggabungkan konten dengan informasi gayanya**:

```
 Load HTML ──► Parse HTML ──► Create DOM tree ──► Display
                   │                 ▲
                   ▼                 │ Attach style to DOM nodes
              Load CSS ──► Parse CSS ┘
```
1. **Load HTML** – unduh file HTML.
2. **Parse HTML** – baca & pahami HTML.
3. Saat parsing, browser menemukan link CSS → **Load CSS** → **Parse CSS**.
4. **Create DOM tree** – bangun pohon DOM.
5. **Attach style to DOM nodes** – tempelkan gaya CSS ke setiap node DOM.
6. **Display** – tampilkan di layar.

(Browser berbeda bisa sedikit berbeda prosesnya; ini versi sederhananya.)

### 4.12 CSS dan DOM

- **DOM berbentuk pohon (tree).** Setiap **elemen, atribut, dan potongan teks** di markup menjadi **node DOM**.
- Node didefinisikan oleh **hubungannya** dengan node lain: ada **parent (induk)**, **child (anak)**, dan **sibling (saudara)**.
- Saat CSS diterapkan, gaya **dikaitkan dengan node DOM** – menentukan warna, ukuran, posisi – **tanpa mengubah struktur DOM**.

Contoh:
```html
<p>
  Let's use:
  <span>Cascading</span>
  <span>Style</span>
  <span>Sheets</span>
</p>
```
Pohon DOM-nya:
```
P
├── "Let's use:"   (node teks)
├── SPAN
│   └── "Cascading"
├── SPAN
│   └── "Style"
└── SPAN
    └── "Sheets"
```

### 4.13 Menerapkan CSS ke DOM

```css
span {
  border: 1px solid black;    /* garis tepi hitam 1 pixel */
  background-color: lime;     /* latar hijau terang */
}
```
**Hasil:** "Let's use: [Cascading] [Style] [Sheets]" – ketiga kata dalam span diberi kotak bergaris hitam berlatar hijau lime. Teks "Let's use:" tidak terpengaruh karena bukan span.

### 4.14 The CSS Cascade (Kaskade)

**Cascade** = **algoritma untuk menyelesaikan konflik** ketika **beberapa aturan CSS berlaku pada satu elemen**.

Contoh:
```css
button { color: red; }
button { color: blue; }
```
Hasil: tombol **"Hello, I have blue text"** berwarna **biru**, karena aturan biru **muncul belakangan**.

Algoritma cascade punya **4 tahap**:
1. **Position and order of appearance** – **urutan kemunculan** aturan CSS (yang belakangan menang jika setara).
2. **Specificity** – algoritma untuk menentukan **selector mana yang paling "spesifik"** (paling kuat cocoknya).
3. **Origin** – **dari mana** CSS berasal: gaya bawaan browser, CSS dari ekstensi browser, atau CSS buatanmu (*authored*).
4. **Importance** – beberapa aturan **lebih berat bobotnya**, terutama yang memakai **`!important`**.

(Sumber: https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade)

⚠️ **Cara membaca daftar di atas:** urutannya dari **yang paling lemah ke yang paling kuat**. Saat browser memutuskan pemenang, ia mengecek dari bawah ke atas:
1. Adakah yang `!important`? (**Importance**) → yang important menang.
2. Kalau setara, dari mana asalnya? (**Origin**) → CSS buatanmu mengalahkan gaya bawaan browser.
3. Kalau masih setara, mana yang lebih spesifik? (**Specificity**).
4. Kalau masih setara juga, **yang ditulis paling akhir menang** (**Order of appearance**) – seperti contoh tombol biru di atas.

💡 Seperti lomba: urutan kemunculan hanya dipakai sebagai **"adu penalti"** kalau semua kriteria lain seri.

### 4.15 Ruleset (Aturan) CSS

```css
div {                    ← SELECTOR: elemen mana yang dikenai aturan
  color: red;            ← PROPERTY: color, VALUE: red
  font-family: Arial;    ← setiap deklarasi diakhiri titik koma (;)
  font-size: 24pt;
}
```
- **Selector** = memilih elemen yang mau diberi gaya.
- **Property** = aspek yang diatur (warna, font, ukuran).
- **Value** = nilai untuk property tersebut.
- Daftar lengkap selector & property: https://developer.mozilla.org/en-US/docs/Web/CSS/Reference

### 4.16 Tiga Selector Dasar (dengan contoh)

HTML yang dipakai:
```html
<h1>Heading!</h1>
<div>Paragraph!</div>
<div ...>Info</div>
```

**Contoh 1 – Element Type Selector** (pilih berdasarkan **nama tag**)
```css
div { color: red; font-family: Arial; font-size: 24pt; }
```
→ **"Paragraph!" dan "Info" keduanya merah** (karena keduanya `div`). "Heading" tetap hitam.

**Contoh 2 – Class Selector** (pilih berdasarkan **class**, pakai titik `.`)
```html
<div class="info">Info</div>
```
```css
.info { color: red; font-family: Arial; font-size: 24pt; }
```
→ **Hanya "Info" yang merah.**

**Contoh 3 – ID Selector** (pilih berdasarkan **id**, pakai pagar `#`)
```html
<div id="unique">Info</div>
```
```css
#unique { color: red; font-family: Arial; font-size: 24pt; }
```
→ **Hanya "Info" yang merah.**

### 4.17 ID vs Class

| **ID** | **Class** |
|---|---|
| Satu elemen **hanya boleh punya satu ID**. | Satu elemen **boleh punya banyak class** (`class="tombol besar merah"`). |
| ID **harus unik** dalam satu dokumen HTML. | Class yang sama **boleh dipakai di banyak elemen**. |
| Selector: `#id { … }` | Selector: `.classname { … }` |

💡 **Analogi:** ID = **NIK/KTP** (unik, satu orang satu). Class = **"jurusan"** (banyak orang bisa sama-sama "Informatika", dan satu orang bisa ikut beberapa unit kegiatan).

### 4.18 CSS Specificity (Spesifisitas)

Jika ada dua atau lebih aturan yang menunjuk elemen yang sama, **selector dengan nilai specificity tertinggi "menang"**. Anggap specificity sebagai **skor/peringkat**.

**Contoh 1:**
```html
<style> p {color: red;} </style>
<p>Hello World!</p>
```
→ **Merah** (hanya ada satu aturan).

**Contoh 2:**
```html
<style>
  .test {color: green;}
  p {color: red;}
</style>
<p class="test">Hello World!</p>
```
→ **Hijau!** Walaupun `p {color: red}` ditulis belakangan, **class lebih spesifik** daripada elemen. (Specificity dicek **sebelum** urutan kemunculan.)

#### Cara menghitung skor specificity
```
  Style attribute ,   ID   ,  Class/pseudo-class/attribute ,  Elements
   (paling kuat)                                               (paling lemah)
```
- Mulai dari **0**.
- **+100** untuk setiap **ID**.
- **+10** untuk setiap **class**, **pseudo-class** (misal `:hover`), atau **attribute selector** (misal `[type="text"]`).
- **+1** untuk setiap **elemen** atau **pseudo-element** (misal `::first-letter`).
- **Catatan 1:** **Inline style** (`<p style="color:red">`) bernilai **1000** – selalu prioritas tertinggi.
- **Catatan 2:** Satu-satunya pengecualian: **`!important`** bahkan **mengalahkan inline style**.

**Latihan hitung:**
| Selector | Skor |
|---|---|
| `p` | 1 |
| `div p` | 2 |
| `.info` | 10 |
| `p.info` | 11 |
| `#unique` | 100 |
| `#unique .info p` | 100 + 10 + 1 = 111 |
| `style="..."` (inline) | 1000 |

### 4.19 Hierarki CSS

```
  ▲ Specificity tertinggi
  │   INLINE STYLE
  │   ID ATTRIBUTES
  │   CLASSES
  │   ELEMENTS
  ▼ Specificity terendah
```
🎯 Saran dari slide: **"Only use classes for CSS styling!"** – gunakan **class** untuk styling. Alasannya: specificity class sedang-sedang saja, mudah di-*override*, dan bisa dipakai ulang. ID terlalu kuat (susah ditimpa) dan inline style membuat kode berantakan.

### 4.20 `!important`

- `!important` dipakai untuk **memberi kepentingan lebih** pada sebuah property/value. Jika dipakai, ia akan **menimpa SEMUA aturan styling sebelumnya** untuk property itu di elemen itu!
- **Satu-satunya cara menimpa `!important`** adalah dengan `!important` lain pada deklarasi dengan **specificity yang sama atau lebih tinggi** (atau sama tapi muncul belakangan).

#### Contoh dari slide
```css
#myid    { background-color: blue; }
.myclass { background-color: gray; }
p        { background-color: red !important; }
```
```html
<p>Hello World!</p>
<p class="myclass">Hello World!</p>
<p id="myid">Hello World!</p>
```
**Hasil: KETIGA paragraf berlatar MERAH.** Karena aturan `p` memakai `!important`, ia mengalahkan `#myid` dan `.myclass` yang biasa – walaupun ID dan class lebih spesifik.

Versi kedua di slide memberi `!important` juga pada ID dan class.
⚠️ **Koreksi penulisan:** di slide tertulis `background-color: blue; !important;` – **ini salah tulis**. Titik koma sebelum `!important` membuat `!important` jadi deklarasi tidak sah dan **diabaikan browser**. Penulisan yang benar:
```css
#myid    { background-color: blue !important; }
.myclass { background-color: gray !important; }
p        { background-color: red  !important; }
```
**Hasil (jika ditulis benar):** semuanya `!important`, jadi ditentukan oleh **specificity**:
- Paragraf 1 (`<p>`) → hanya kena aturan `p` → **merah**.
- Paragraf 2 (`class="myclass"`) → `.myclass` (10) > `p` (1) → **abu-abu**.
- Paragraf 3 (`id="myid"`) → `#myid` (100) > `p` (1) → **biru**.

### 4.21 CSS Combinators (Penggabung Selector)

Combinator menyatakan **hubungan antar selector**. Ada **4**:
| Combinator | Simbol | Arti |
|---|---|---|
| Descendant selector | **spasi** | Semua **keturunan** (anak, cucu, cicit, …) |
| Child selector | **`>`** | Hanya **anak langsung** |
| Adjacent sibling selector | **`+`** | **Satu** saudara yang **tepat sesudahnya** |
| General sibling selector | **`~`** | **Semua** saudara **sesudahnya** |

HTML contoh yang dipakai di slide:
```html
<div class="container">
  <section class="child" id="c1">
    <div>subchild 1</div>         <!-- cucu dari .container -->
  </section>
  <div class="child" id="c2">child 2</div>
  <div class="child" id="c3">child 3</div>
  <div class="child" id="c4">child 4</div>
</div>
```
Struktur pohonnya:
```
.container
├── section#c1
│   └── div "subchild 1"
├── div#c2 "child 2"
├── div#c3 "child 3"
└── div#c4 "child 4"
```

**a) Descendant (spasi)**
```css
.container div { font-size: 20px; }
```
→ Memilih **semua `div` di dalam `.container`, sedalam apa pun**. Hasil: **subchild 1, child 2, child 3, child 4 semuanya besar** (subchild 1 juga terpilih karena ia cucu).

**b) Child (`>`)**
```css
.container > div { font-size: 20px; }
```
→ Hanya `div` yang **anak langsung** `.container`. Hasil: **child 2, 3, 4 besar**; **subchild 1 tidak** (ia cucu, bukan anak; dan `#c1` adalah `section`, bukan `div`).

**c) Adjacent sibling (`+`)**
```css
#c1 + div { color: red; }
```
→ `div` yang **tepat setelah** `#c1`. Hasil: **hanya child 2 merah**.

**d) General sibling (`~`)**
```css
#c1 ~ div { color: red; }
```
→ **Semua** `div` saudara **setelah** `#c1`. Hasil: **child 2, child 3, child 4 merah**.

💡 **Analogi keluarga:** spasi = "semua keturunan kakek" (anak + cucu); `>` = "anak-anaknya saja"; `+` = "adik yang lahir persis setelahmu"; `~` = "semua adikmu".

---

### 4.22 CSS Layout (Tata Letak)

Teknik layout CSS memungkinkan **kontrol presisi atas posisi elemen**, berdasarkan: posisi default di *normal flow*, hubungannya dengan elemen sekitar, container induknya, dan viewport/jendela utama.

Teknik-teknik layout:
- **Normal flow** (aliran normal – default: block bertumpuk, inline berjajar)
- **Properti `display`**
- **Flexbox**
- **Grid**
- **Floats**
- **Positioning** (`position: relative/absolute/fixed/sticky`)
- **Table layout**
- **Multiple-column layout**

### 4.23 Tipe `display`

Properti **`display`** memberi tahu browser **cara menampilkan sebuah elemen dan anak-anaknya**. Nilai yang dibahas: **`block`**, **`flex`**, **`grid`**, **`none`** (juga `inline`).

### 4.24 Box Model

- HTML mengelompokkan elemen menjadi **block-level** (mulai baris baru, bertumpuk **vertikal**) atau **inline-level** (masuk ke aliran teks, berjajar **horizontal**).
- Dengan `display`, kita bisa mengubah perilaku ini: **`display: block`** → jadi block; **`display: inline`** → jadi inline.

```
Block-Level Elements:          Inline-Level Elements:
┌────────────────────────┐     ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐
└────────────────────────┘     └────┘ └────┘ └────┘ └────┘ └────┘
┌────────────────────────┐
└────────────────────────┘
┌────────────────────────┐
└────────────────────────┘
```

#### CSS Box Model
Setiap elemen block adalah **kotak berlapis 4**:
```
┌──────────────────── MARGIN (ruang di luar, jarak ke elemen lain) ──────┐
│  ┏━━━━━━━━━━━━━━━━━ BORDER (garis tepi) ━━━━━━━━━━━━━━━━━━━━━━━━━┓      │
│  ┃  ┌─────────────── PADDING (ruang di dalam border) ─────────┐  ┃      │
│  ┃  │   ┌───────────────────────────────────────────┐        │  ┃      │
│  ┃  │   │          CONTENT (isi: teks/gambar)       │        │  ┃      │
│  ┃  │   └───────────────────────────────────────────┘        │  ┃      │
│  ┃  └─────────────────────────────────────────────────────────┘  ┃      │
│  ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛      │
└─────────────────────────────────────────────────────────────────────────┘
```
| Bagian | Fungsi | Diatur dengan |
|---|---|---|
| **Content box** | Area tempat konten tampil | `width`, `height` |
| **Padding box** | Ruang **di sekitar konten** (di dalam border) | `padding` |
| **Border box** | Lapisan **mengelilingi konten + padding** | `border` |
| **Margin box** | Ruang **terluar** antara elemen ini dan elemen lain | `margin` |

Box model sepenuhnya berlaku untuk **block box**; **inline box hanya memakai sebagian** perilakunya (misal margin atas-bawah tidak berpengaruh pada inline).

💡 **Analogi:** Bingkai foto di dinding: **foto** = content, **passepartout/karton putih** = padding, **bingkai kayu** = border, **jarak ke bingkai lain di dinding** = margin.

Contoh HTML untuk materi layout berikutnya:
```html
<div class="container">
  <div class="child" id="c1">child 1</div>
  <div class="child" id="c2">child 2</div>
  <div class="child" id="c3">child 3</div>
  <div class="child" id="c4">child 4</div>
</div>
```

### 4.25 `display: grid`

```css
.container { display: grid; }
```
Memberi tahu browser untuk **menampilkan anak-anak elemen dalam layout grid DUA DIMENSI** (baris dan kolom).

Grid 3×3 punya **garis kolom (column lines) 1–4** dan **garis baris (row lines) 1–4** – garis-garis inilah yang dipakai untuk memposisikan item.

#### `grid-auto-flow`
Mengatur **bagaimana browser otomatis menempatkan anak-anak** yang "melewati" batas grid:
- **`row`** – utamakan **menambah baris** baru. **Ini default.**
- **`column`** – utamakan **menambah kolom** baru.

```css
.container { display: grid; grid-auto-flow: row; }
```
→ child 1, child 2, child 3, child 4 **bertumpuk ke bawah** (satu per baris).

```css
.container { display: grid; grid-auto-flow: column; }
```
→ child 1 | child 2 | child 3 | child 4 **berjajar ke samping** dalam satu baris (satu per kolom).

### 4.26 Flexbox

**Flexbox** = metode layout **SATU DIMENSI** untuk mengatur item dalam **baris atau kolom**. Item bisa **"flex" (melebar) mengisi ruang tambahan** atau **menyusut** agar muat di ruang sempit.

Dengan Flexbox kamu bisa:
- **Menengahkan blok konten secara vertikal** di dalam induknya (dulu ini sangat sulit!).
- Membuat **semua anak container mengambil lebar/tinggi yang sama**, berapa pun ruang yang tersedia.
- Membuat **semua kolom dalam layout multi-kolom setinggi sama**, walaupun isinya berbeda panjang.

**Contoh:**
```css
section { display: flex; }
```
Hasilnya (slide): tiga `<article>` ("First article", "Second article", "Third article") **berjajar ke samping** dengan **tinggi yang sama** – walaupun "Third article" isinya lebih panjang, dua article lainnya ikut setinggi itu.

#### Flex Model – dua sumbu
```
               cross start
  ┌───────────────── │ ────────────────────────────── flex container ┐
  │ main start ──────┼──────── main axis ─────────────────► main end │
  │  ┌──────────┐    │  ┌──────────┐      ┌──────────┐ ▲             │
  │  │flex item │    │  │flex item │      │flex item │ │ cross size  │
  │  │          │    │  │◄main size►│     │          │ ▼             │
  │  └──────────┘    ▼  └──────────┘      └──────────┘               │
  └──────────────── cross end (cross axis ↓) ─────────────────────────┘
```
- **Main axis (sumbu utama)** = arah item disusun (baris atau kolom). Ujung-ujungnya **main start** dan **main end**; panjangnya **main size**.
- **Cross axis (sumbu silang)** = **tegak lurus** main axis. Ujungnya **cross start** dan **cross end**; panjangnya **cross size**.
- Elemen dengan `display: flex` disebut **flex container**.
- Elemen di dalamnya disebut **flex items**.

🎯 **Grid vs Flexbox:** Grid = **2 dimensi** (baris DAN kolom sekaligus, cocok untuk layout halaman). Flexbox = **1 dimensi** (baris ATAU kolom, cocok untuk menyusun komponen seperti navbar).

### 4.27 `display: none` vs `visibility: hidden`

```css
#c1 { display: none; }
```
→ Elemen **dihapus dari tampilan** dan **tidak memakan ruang** dalam layout. Hasil: child 2, child 3, child 4 bergeser mengisi tempat child 1.

```css
#c1 { visibility: hidden; }
```
→ Elemen **disembunyikan tapi TETAP memakan ruang**. Hasil: ada **ruang kosong** di tempat child 1; child 2, 3, 4 tetap di posisinya.

💡 **Analogi:** `display: none` = **kursinya diangkat dari kelas**. `visibility: hidden` = **orangnya pergi, tapi kursinya tetap ada (kosong)**.

### 4.28 Content Overflow

Properti **`overflow`** mengatur **apa yang terjadi jika isi anak lebih besar dari induknya**. Contoh: kotak 50×50 px berlatar abu-abu berisi teks "the quick brown fox jumps over the lazy dog".

```css
.container {
  width: 50px;
  height: 50px;
  background-color: gray;
  overflow: ...;
}
```

| Nilai | Perilaku | Hasil di contoh |
|---|---|---|
| **`visible`** | **Default.** Isi yang meluber **tetap ditampilkan** (keluar dari kotak). | Hanya "the quick" di dalam kotak abu-abu, sisanya ("brown fox jumps over the lazy dog") meluber ke bawah kotak. |
| **`hidden`** | Isi yang meluber **dipotong (clip)**; **tidak bisa di-scroll**. | Hanya "the quick" terlihat, sisanya hilang. |
| **`scroll`** | **Selalu** menampilkan scrollbar (ada atau tidak ada luberan). | (Tidak ada contoh di slide.) |
| **`auto`** | Scrollbar **hanya muncul jika perlu** (ada isi yang meluber). | Kotak dengan teks panjang → ada scrollbar; kotak berisi "the" saja → tanpa scrollbar. |

---

### 4.29 HTML5

- **HTML5** adalah bahasa markup untuk menyusun & menyajikan konten di World Wide Web. Ini adalah **versi besar kelima dan terakhir** HTML yang menjadi **rekomendasi W3C**.
- HTML5 adalah revisi besar yang **menggantikan HTML 4.01, XHTML 1.0, dan XHTML 1.1**.
- Standar HTML5 adalah **kerja sama antara W3C** (World Wide Web Consortium) **dan WHATWG** (Web Hypertext Application Technology Working Group).

#### Fitur-fitur baru HTML5

| Fitur | Penjelasan |
|---|---|
| **New Semantic Elements** | Elemen bermakna seperti `<header>`, `<footer>`, `<section>`. |
| **Forms 2.0** | Peningkatan form – atribut & tipe baru untuk `<input>`. |
| **Persistent Local Storage** | Menyimpan data di browser **tanpa plugin pihak ketiga**. |
| **WebSocket** | Teknologi komunikasi **dua arah (bidirectional)** generasi baru. |
| **Server-Sent Events (SSE)** | **Event yang mengalir dari server ke browser** (server "mendorong" data). |
| **Canvas** | **Permukaan gambar 2D (bitmap)** yang bisa diprogram dengan JavaScript (untuk game, grafik). |
| **Audio & Video** | Menyisipkan audio/video **tanpa plugin** (dulu harus Flash). |
| **Geolocation** | Pengunjung bisa **berbagi lokasi fisiknya** dengan aplikasi web. |
| **Microdata** | Membuat **kosakata sendiri** di luar HTML5 untuk memberi makna khusus (membantu mesin pencari). |
| **Drag and drop** | Menyeret & melepas item dari satu tempat ke tempat lain di halaman yang sama. |

### 4.30 Elemen Semantik HTML5

"Semantik" = **bermakna**. Daripada `<div>` di mana-mana, HTML5 menyediakan tag yang **menjelaskan perannya**:

| Tag | Makna |
|---|---|
| `<section>` | **Bagian generik** dokumen/aplikasi. Bisa dipakai bersama h1–h6 untuk menunjukkan struktur. |
| `<article>` | **Konten independen** yang bisa berdiri sendiri, misal **entri blog** atau **artikel koran**. |
| `<aside>` | Konten yang **hanya sedikit berkaitan** dengan isi halaman (misal sidebar, iklan). |
| `<header>` | **Kepala/header** dari sebuah section. |
| `<footer>` | **Kaki/footer** section – bisa berisi info penulis, hak cipta, dll. |
| `<nav>` | Bagian dokumen untuk **navigasi** (menu). |
| `<dialog>` | Untuk menandai **percakapan/dialog** (sekarang dipakai juga untuk kotak dialog/modal). |
| `<figure>` | Mengaitkan **caption** dengan konten tertanam seperti **gambar atau video**. |

#### Contoh struktur halaman semantik
```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="utf-8">
      <title>...</title>
   </head>
   <body>
      <header>...</header>
      <nav>...</nav>
      <article>
         <section>
            ...
         </section>
      </article>
      <aside>...</aside>
      <footer>...</footer>
   </body>
</html>
```
```
┌─────────────── <header> ───────────────┐
├─────────────── <nav> ──────────────────┤
│ ┌──── <article> ────────┐ ┌─<aside>──┐ │
│ │  ┌── <section> ────┐  │ │          │ │
│ │  └─────────────────┘  │ │          │ │
│ └───────────────────────┘ └──────────┘ │
├─────────────── <footer> ───────────────┤
```
⚠️ **Jangan tertukar `<head>` dan `<header>`!** `<head>` berisi **metadata untuk mesin** (title, charset, link CSS) – **tidak tampil**. `<header>` adalah **bagian yang terlihat** manusia (judul utama, logo, nama penulis).

Manfaat semantik: lebih mudah dibaca developer, lebih baik untuk **SEO** (mesin pencari) dan **aksesibilitas** (pembaca layar untuk tunanetra).

### 4.31 Web Forms 2.0

Web Forms 2.0 adalah **perluasan fitur form HTML4**. Elemen & atribut form di HTML5 lebih **semantik** dan **membebaskan kita dari banyak scripting & styling yang merepotkan** (misalnya validasi email dulu harus pakai JavaScript, sekarang cukup `type="email"`).

#### Tipe `<input>` baru di HTML5

| Type | Arti |
|---|---|
| `datetime` | Tanggal + waktu (tahun, bulan, hari, jam, menit, detik, pecahan detik) format **ISO 8601** dengan zona waktu **UTC**. *(Sudah usang/tidak didukung browser modern.)* |
| `datetime-local` | Tanggal + waktu **tanpa info zona waktu**. |
| `date` | Tanggal (tahun, bulan, hari). |
| `month` | Tahun + bulan. |
| `week` | Tahun + nomor minggu. |
| `time` | Waktu (jam, menit, detik, pecahan detik). |
| `number` | **Hanya menerima angka.** Atribut `step` menentukan presisi (default 1). |
| `range` | Slider untuk memilih nilai **dari rentang angka**. |
| `email` | **Hanya menerima format email.** |
| `url` | **Hanya menerima format URL.** |

```html
<input type="email" name="email">        <!-- browser cek ada @ -->
<input type="number" step="0.5">         <!-- angka kelipatan 0.5 -->
<input type="range" min="0" max="100">   <!-- slider 0–100 -->
<input type="date">                      <!-- muncul kalender -->
```

### 4.32 Web Storage

HTML5 **web storage** = fitur untuk **menyimpan informasi secara lokal di komputer pengguna**, mirip cookie, tapi **lebih cepat dan jauh lebih baik dari cookie** (kapasitas lebih besar, tidak ikut terkirim di setiap request).

Dua mekanisme:
| | **Session Storage** | **Local Storage** |
|---|---|---|
| Dirancang untuk | Skenario pengguna melakukan **satu transaksi**, tapi bisa melakukan **beberapa transaksi di jendela berbeda** secara bersamaan. | Penyimpanan yang **berlaku lintas banyak jendela** dan **bertahan melewati sesi saat ini**. |
| Kapan hilang? | Saat **tab ditutup**. | Bertahan **sampai dihapus** (oleh kode atau pengguna). |

#### Contoh Local Storage
```html
<script>
// Cek apakah objek localStorage tersedia
if (localStorage) {
    // Simpan data
    localStorage.setItem("first_name", "Peter");

    // Ambil data
    alert("Hi, " + localStorage.getItem("first_name"));   // → "Hi, Peter"
} else {
    alert("Sorry, your browser do not support local storage.");
}
</script>
```
- **`localStorage.setItem(key, value)`** – menyimpan **value** dengan kunci **key**.
- **`localStorage.getItem(key)`** – mengambil value berdasarkan **key**.
- (Tambahan: `removeItem(key)` untuk menghapus, `clear()` untuk menghapus semua.)

### 4.33 WebSockets

- **WebSockets** = teknologi komunikasi **dua arah (bidirectional)** generasi baru yang bekerja **lewat satu socket (koneksi)**, diakses lewat **antarmuka JavaScript** di browser yang mendukung HTML5.
- **Kirim data browser → server:** method **`send()`**.
- **Terima data server → browser:** event handler **`onmessage`**.

💡 **Bedanya dengan HTTP biasa:** HTTP itu seperti **SMS** (kirim pesan, tunggu balasan, server tidak bisa mengirim duluan). WebSocket itu seperti **telepon yang terus tersambung** – kedua pihak bisa bicara kapan saja. Cocok untuk chat, notifikasi real-time, game multiplayer.

#### Event WebSocket
| Event | Event Handler | Kapan terjadi |
|---|---|---|
| `open` | `Socket.onopen` | Koneksi socket **berhasil dibuat**. |
| `message` | `Socket.onmessage` | Client **menerima data dari server**. |
| `error` | `Socket.onerror` | Terjadi **error** dalam komunikasi. |
| `close` | `Socket.onclose` | Koneksi **ditutup**. |

```javascript
const socket = new WebSocket("wss://contoh.com/chat");
socket.onopen    = () => socket.send("Halo server!");
socket.onmessage = (event) => console.log("Pesan masuk:", event.data);
socket.onerror   = (err) => console.error("Error:", err);
socket.onclose   = () => console.log("Koneksi ditutup");
```

### 4.34 Web Worker

- **Web worker** = JavaScript yang **berjalan di latar belakang (background)**, **terpisah dari script lain**, **tanpa mengganggu performa halaman**.
- Kamu tetap bisa melakukan apa pun (klik, memilih sesuatu, dll) **sementara web worker bekerja** di belakang.
- 💡 Kenapa penting? Karena JavaScript itu **single-threaded** (Bab 5). Kalau ada perhitungan berat di thread utama, halaman "membeku". Web worker = **asisten yang mengerjakan tugas berat di ruangan lain**, jadi kamu (thread utama) tetap bisa melayani tamu.

### 4.35 Dukungan Offline untuk Web App (Service Worker)

- Gunakan **Service Worker** untuk mengambil konten/file dan menyimpan data di penyimpanan lokal.
- **Service Worker** = script yang berjalan **di latar belakang, di thread terpisah dari UI browser**.
- **Service worker cache** memungkinkan website **berfungsi saat offline**.
- Service worker adalah "mesin" yang **mengubah website menjadi Progressive Web App (PWA)**.
- Memungkinkan integrasi mendalam dengan platform: **caching kaya**, **push notification**, dan **background sync**.
- Service worker **berada di antara browser dan jaringan**, bertindak **seperti proxy server**, menangani tugas-tugas non-UI.
- Bersifat **event-driven** dan **hidup di luar proses browser**, sehingga bisa bekerja **tanpa sesi browser aktif**.

```
[Halaman web] ──request──► [SERVICE WORKER] ──(kalau perlu)──► [Jaringan/Server]
                               │
                               └──► [Cache] (kalau offline, ambil dari sini)
```

---

### ✅ Cek Pemahaman Bab 4

1. Apa beda tag dan elemen?
2. Hitung specificity: `#nav .item a`.
3. `p {color:red}` ditulis setelah `.test {color:green}`. Elemen `<p class="test">` berwarna apa? Kenapa?
4. Apa beda `.a .b`, `.a > .b`, `.a + .b`, `.a ~ .b`?
5. Apa beda `display: none` dan `visibility: hidden`?
6. Apa beda sessionStorage dan localStorage?
7. Grid vs Flexbox?

<details>
<summary>Lihat jawaban</summary>

1. Tag = penanda di kode sumber (pembuka/penutup). Elemen = bagian DOM (tag + isi) yang ada di memori browser.
2. 100 + 10 + 1 = **111**.
3. **Hijau**, karena class (10) lebih spesifik dari elemen (1); specificity dicek sebelum urutan.
4. `.a .b` = semua keturunan; `.a > .b` = anak langsung; `.a + .b` = satu saudara tepat setelahnya; `.a ~ .b` = semua saudara setelahnya.
5. `none` = dihapus dari layout, tidak makan tempat. `hidden` = tak terlihat tapi tetap makan tempat.
6. sessionStorage hilang saat tab ditutup; localStorage bertahan sampai dihapus.
7. Grid = 2 dimensi (baris & kolom); Flexbox = 1 dimensi (baris atau kolom).
</details>

---
## Bab 5 – JavaScript (03-JavaScript)

Kalau HTML adalah kerangka dan CSS adalah pakaian, maka **JavaScript adalah otak dan ototnya** – yang membuat halaman bisa **bereaksi, berubah, dan berpikir**.

---

### 5.1 Pengenalan JavaScript

| Ciri | Penjelasan sederhana |
|---|---|
| **Berbasis interpreter** | Kode dijalankan langsung baris per baris oleh "penerjemah" (tidak perlu di-compile manual jadi file .exe). Dulu **hanya berjalan di browser**, sekarang bisa juga di **server (Node.js)** atau di **JVM (Rhino)**. |
| **Sintaks mirip Java, tapi BUKAN berbasis Java** | Namanya mirip karena alasan pemasaran tahun 1995. Bahasanya sangat berbeda. *(JavaScript ≠ Java, seperti "mobil" ≠ "mobil-mobilan".)* |
| **Dynamic typing & duck typing** | Tipe variabel ditentukan saat program jalan, bukan saat ditulis. *Duck typing:* "Kalau ia berjalan seperti bebek dan bersuara seperti bebek, anggap saja bebek" – yang penting objek punya method/property yang dibutuhkan, bukan "kelasnya" apa. |
| **Objek sebagai wadah umum** | Objek dipakai untuk menampung apa saja (seperti "kamus" kunci–nilai). |
| **Fungsi adalah *first-class*** | Fungsi bisa **disimpan di variabel, dikirim sebagai parameter, dan dikembalikan dari fungsi lain** (konsep lambda/closure). |
| **Linkage via global variables** | Antar-file script "terhubung" lewat variabel global (ini sering jadi sumber bug). |
| **Prototype, bukan class-inheritance** | Pewarisan memakai **prototype**. Kata kunci `class` (ES6) hanyalah **"gula sintaks" (syntactic sugar)** di atas prototype. |
| **Distandarkan sebagai ECMAScript** | Standar resminya bernama **ECMAScript (ECMA-262 dan ISO/IEC 16262)**. ES6 = ECMAScript 2015. |

#### JavaScript di browser
- JS di browser = **sintaks inti + DOM**.
- Ada beberapa **implementasi (JavaScript Engine)** – program khusus yang **ditanam di browser** untuk menjalankan kode JS, dan mengimplementasikan standar ECMAScript:
  - **V8** → Chrome, Edge, Opera, dan **Node.js** (bukan browser)
  - **SpiderMonkey** → Mozilla Firefox
  - **JavaScriptCore** → WebKit/Safari
- ⚠️ **DOM biasanya TIDAK disediakan oleh JavaScript engine, melainkan oleh browser.** (Makanya di Node.js tidak ada `document`.)

### 5.2 Kenapa Kita Butuh JavaScript?

- **HTML + CSS saja tidak bisa menangani logika dinamis dan interaksi pengguna yang kompleks.** HTML mendefinisikan **struktur**, CSS hanya mendefinisikan **tampilan**.
- **JavaScript memungkinkan halaman web merespons pengguna, mengubah konten, memproses data, berkomunikasi dengan server, dan membuat keputusan saat halaman berjalan.**
- Ilustrasi dari scrimba: **HTML = komponen mobil** (rangka, roda, pintu), **CSS = cat**, **JS = mesin** (yang membuat mobil bisa bergerak).

### 5.3 Di Dalam JavaScript Engine (V8)

**V8 dari Google** adalah engine JS yang paling banyak dipakai, **ditulis dengan C++**.

```
JavaScript Source Code
        │
        ▼
     Parser  ──►  Abstract Syntax Tree (AST)
                        │
                        ▼
              Interpreter "Ignition" ──────(kode sering dipakai / "panas")────► Compiler "TurboFan"
                        │                                                           │
                        ▼                                                           ▼
                    Bytecode  ◄──────────(deoptimisasi kalau asumsi salah)──── Optimized Machine Code
```
1. **Parser** membaca kode dan membentuk **AST** (pohon yang merepresentasikan struktur kode).
2. **Ignition (interpreter)** "meng-compile" JS **just in time (JIT)** menjadi **bytecode**, lalu bytecode itu diinterpretasikan.
3. Kode yang sering dijalankan ("panas") dikirim ke **TurboFan (compiler pengoptimasi)** → menjadi **machine code teroptimasi** (sangat cepat).
4. Jika asumsi optimasi ternyata salah (misal tipe data berubah), kode **di-deoptimasi** kembali ke bytecode (panah merah di diagram).

Poin lain dari slide:
- **JavaScript != Java**, dan **tidak berasal dari Java**.
- JS berjalan di **browser** dan lingkungan **server-side** (misal Node.js).

### 5.4 Cara Menaruh JavaScript di HTML

**a) Internal (di dalam dokumen HTML)**
```html
<!-- Standar HTML 4.01 -->
<script type="text/javascript">
  ...statement...
</script>

<!-- Tag lama (usang) -->
<script language="JavaScript">
  ...statement...
</script>

<!-- HTML5: cukup -->
<script> ... </script>
```

**b) File eksternal (DIREKOMENDASIKAN)**
- Script ditulis di file tersendiri berekstensi **`.js`**.
- Dipanggil dengan:
```html
<script src="namafile.js"></script>
```
Keuntungan: kode rapi, bisa dipakai banyak halaman, dan bisa di-cache browser.

💡 **Tips:** Taruh `<script>` **di akhir HTML** (sebelum `</body>`) untuk **meningkatkan performa** – karena saat browser menemukan `<script>`, ia **berhenti memproses HTML** sampai script selesai diunduh & dijalankan. Kalau script di akhir, konten sudah tampil duluan.

### 5.5 DOM dan Kode JavaScript

```html
<html>
<body>

<h1 id="demo"></h1>

<script>
// Akses sebuah elemen
const myPara = document.getElementById("demo");

// Ubah isi elemen tersebut
myPara.innerHTML = "Hello World";
</script>

</body>
</html>
```
- `document.getElementById("demo")` → mencari elemen dengan `id="demo"` di DOM.
- `.innerHTML = "Hello World"` → mengganti isi elemen. Hasilnya, halaman menampilkan **"Hello World"** sebagai judul h1.

Pohon DOM (dari slide):
```
              Document
                 │
          Root Element <html>
          ┌──────┴────────┐
   Element <head>     Element <body>
         │              ┌───┴──────┐
   Element <title>  Element <a>  Element <h1>
         │              │            │
   Text "My title"  Atribut "href"  Text "My header"
```

💡 **Kamu juga bisa menjalankan JavaScript langsung di Console DevTools browser** (karena browser seperti Chrome punya engine V8). Contoh di slide: ketik `console.log("Hello WBD");` → muncul `Hello WBD`, lalu `undefined` (karena `console.log` tidak mengembalikan nilai).

---

### 5.6 Variabel

- **Case sensitive:** `nama` ≠ `Nama` ≠ `NAMA`.
- Dideklarasikan dengan **`var`**, **`let`**, atau **`const`** (`let` & `const` sejak **ES6, 2015**):
```javascript
var i;  var x = 2;     // var: gaya lama, scope fungsi
let y = 3;             // let: scope BLOK, tidak boleh dideklarasikan ulang
const z = 4;           // const: scope BLOK, tidak boleh dideklarasikan ulang & tidak boleh di-assign ulang
```
- **Variabel tanpa deklarasi otomatis menjadi variabel GLOBAL:**
```javascript
a = 5;   // tanpa var/let/const → jadi global (window.a). HINDARI!
```
- **Scope:** global dan lokal. Variabel jadi **lokal** jika dideklarasikan di dalam fungsi dengan `var`.
- **Konvensi penamaan:** nama variabel, parameter, member, dan fungsi **diawali huruf kecil** (`namaMahasiswa`). **Constructor diawali huruf besar** (`Mahasiswa`, `Date`).

Contoh di console (slide):
```javascript
const course = "WBD";   // ok
let week = 4;           // ok
week = 5;               // ✅ allowed (let boleh diubah nilainya)
week;                   // → 5
const topics = [];
topics.push("DOM");     // ✅ allowed! const melarang assign ULANG, bukan mengubah ISI array
topics;                 // → ['DOM']
// course = "PBO";      // ❌ TypeError: Assignment to constant variable.
```
⚠️ **Jebakan:** `const` artinya **variabel tidak bisa diarahkan ke nilai lain**, tapi **isi objek/array-nya tetap bisa diubah**. 💡 Seperti **nomor loker yang dipermanenkan**: kamu tidak boleh pindah loker, tapi isi lokernya boleh kamu tambah/kurangi.

**Apa itu "block scope"?** Blok = kode di antara `{ }`.
```javascript
if (true) {
  var a = 1;   // var tidak peduli blok → bocor keluar
  let b = 2;   // let hanya hidup di dalam blok ini
}
console.log(a); // 1
console.log(b); // ReferenceError: b is not defined
```

### 5.7 Hoisting

**Hoisting** = proses di mana interpreter **seolah-olah memindahkan deklarasi fungsi dan variabel (yang memakai `var`) ke atas scope-nya**, sebelum kode dijalankan.

```javascript
// Bagaimana kode ditulis:
console.log(x);            // undefined (bukan error!)
var x = "Hello, World";

// Bagaimana sebenarnya dieksekusi:
var x;                     // deklarasi "diangkat" ke atas
console.log(x);            // undefined
x = "Hello, World";        // assignment tetap di tempat asalnya
```
Perilaku hoisting:
- Bisa **memakai variabel sebelum baris deklarasinya**.
- Bisa **mereferensikan variabel tanpa error** (nilainya `undefined`).

🎯 **Untuk mencegah ini, gunakan `let` atau `const`.** (Dengan `let`/`const`, mengakses sebelum deklarasi menghasilkan `ReferenceError` – lebih aman karena bug langsung ketahuan.)

💡 **Analogi:** `var` itu seperti nama tamu yang sudah tertulis di daftar hadir sejak awal acara (jadi kalau dipanggil, "ada namanya" tapi orangnya belum datang = `undefined`).

---

### 5.8 Tipe Data JavaScript

#### a) Number
- **Semua angka** direpresentasikan sebagai **floating-point 64-bit IEEE-754 (double)**, tapi bisa dioperasikan sebagai integer.
- Contoh: `0`, `3`, `10000000`, `0xF3` (heksadesimal), `031` (oktal gaya lama), `3.14`, `1.23e10` (1.23 × 10¹⁰), `3.14E-14`.
- **`NaN` = Not a Number.** Setiap operasi dengan NaN hasilnya NaN. (Uniknya, `typeof NaN` adalah `"number"`, dan `NaN === NaN` adalah `false`!)

#### b) String
- **Immutable** (tidak bisa diubah – setiap operasi menghasilkan string baru).
- Operasi dasar:
```javascript
let s = "hello";
s.length;                                  // 5
s.charAt(2);                               // "l"  (indeks mulai 0)
let sub = s.substring(2, 3);               // "l"  (dari indeks 2 sampai sebelum 3)
let i = s.indexOf('e');                    // 1
"hello, world".replace("hello", "goodbye") // "goodbye, world"
"hello".toUpperCase()                      // "HELLO"
```

#### c) Boolean
- Nilainya **`true`** atau **`false`**.
- **`Boolean(value)`** mengubah nilai apa pun menjadi boolean. Alternatif: **`!!value`**.
- **Falsy** (dianggap false): **`0`, `false`, `null`, `undefined`, `""` (string kosong), `NaN`**.
- **Truthy** = **selain falsy**, **termasuk string `"null"`, `"0"`, `"false"`** (karena itu string yang tidak kosong!).
```javascript
if ("0")  { console.log("masuk!"); }  // masuk! karena "0" adalah string tidak kosong → truthy
if (0)    { console.log("tidak"); }   // tidak masuk, 0 falsy
```

#### d) Object
Contoh objek: **Function, Array, Date, RegExp**, dll.

#### e) Null
- **"Nilai yang bukan apa-apa"** – sengaja dikosongkan oleh programmer.

#### f) Undefined
- **"Nilai yang bahkan bukan itu"** – belum pernah diberi nilai.
- **Nilai default** untuk variabel dan parameter yang belum diisi.
- Nilai untuk **member objek yang tidak ada**.
- **`undefined !== null`** – keduanya berbeda.
- `typeof undefined;` → `'undefined'`
- `typeof null;` → `'object'` ← ini **bug historis** JavaScript yang dipertahankan demi kompatibilitas.

Contoh di console (slide):
```javascript
let wbd;              // undefined (belum diisi)
let sbd = null;       // sengaja dikosongkan
wbd !== sbd;          // true  (berbeda!)
typeof undefined;     // 'undefined'
typeof null;          // 'object'
typeof [];            // 'object'  ← array juga objek! (pakai Array.isArray([]) → true untuk cek array)
```

💡 **Analogi null vs undefined:** `undefined` = **kotak surat yang belum pernah dipasang**. `null` = **kotak surat terpasang tapi sengaja dikosongkan**.

---

### 5.9 Operator

#### Aritmatika
| Operator | Arti | Contoh | Hasil |
|---|---|---|---|
| `+` | tambah | `3+4` | 7 |
| `-` | kurang | `4-3` | 1 |
| `*` | kali | `4*3` | 12 |
| `/` | bagi | `4/3` | 1.33333333 |
| `%` | modulus (sisa bagi) | `4%3` | 1 |
| `++` | increment (tambah 1) | `x=5; x++` | x=6 |
| `--` | decrement (kurang 1) | `x=5; x--` | x=4 |

#### Objek `Math` – kumpulan fungsi matematika standar
| Fungsi | Arti | Contoh |
|---|---|---|
| `Math.abs` | nilai absolut | `Math.abs(-5)` → 5 |
| `Math.floor` | pembulatan ke bawah (jadi integer) | `Math.floor(4.9)` → 4 |
| `Math.log` | logaritma natural | `Math.log(Math.E)` → 1 |
| `Math.max` | nilai maksimum | `Math.max(3, 9)` → 9 |
| `Math.pow` | pangkat | `Math.pow(2, 3)` → 8 |
| `Math.random` | angka acak 0 ≤ x < 1 | `Math.random()` → 0.42... |
| `Math.round` | pembulatan terdekat | `Math.round(4.5)` → 5 |
| `Math.sin` | sinus | `Math.sin(0)` → 0 |
| `Math.sqrt` | akar kuadrat | `Math.sqrt(16)` → 4 |

#### Assignment (penugasan)
| Operator | Contoh | Sama dengan |
|---|---|---|
| `=` | `a = b` | isi a dengan b |
| `+=` | `a += b` | `a = a + b` |
| `-=` | `a -= b` | `a = a - b` |
| `*=` | `a *= b` | `a = a * b` |
| `/=` | `a /= b` | `a = a / b` |
| `%=` | `a %= b` | `a = a % b` |

#### Perbandingan (comparison)
⚠️ Slide "Comparative Operator" sebenarnya mengulang tabel assignment. Ini operator perbandingan yang sebenarnya:
| Operator | Arti | Contoh (x=6) | Hasil |
|---|---|---|---|
| `==` | sama dengan (**dengan** konversi tipe) | `x == "6"` | true |
| `===` | sama dengan (**tanpa** konversi tipe, *strict*) | `x === "6"` | false |
| `!=` | tidak sama | `x != 3` | true |
| `!==` | tidak sama (strict) | `x !== "6"` | true |
| `<`, `>` | kurang/lebih dari | `x > 3` | true |
| `<=`, `>=` | kurang/lebih dari atau sama | `x <= 6` | true |

#### Logika
| Operator | Arti | Contoh (x=6, y=3) | Hasil |
|---|---|---|---|
| `&&` | **and** (dan) | `(x < 7 && y < 4)` | true (keduanya benar) |
| `\|\|` | **or** (atau) | `(x < 7 \|\| y < 2)` | true (salah satu benar) |
| `!` | **not** (bukan) | `!(x == y)` | true |

(Slide menulis contoh `!` sebagai `x != y` – itu operator "tidak sama dengan", yang memakai tanda `!` juga.)

### 5.10 Loosely & Dynamically Typed

**Loosely typed (bertipe longgar):**
- Tipe apa pun bisa **disimpan di variabel** atau **dikirim sebagai parameter** ke fungsi apa pun.
- Bahasa ini **bukan "tanpa tipe"** – nilainya tetap punya tipe.
- **Konversi tipe terjadi otomatis.**

**Dynamically typed (bertipe dinamis):**
- Tipe **tidak perlu ditentukan di awal**.
- Tipe **tidak diperiksa saat compile** (baru ketahuan saat program jalan).

```javascript
let data = 5;        // number
data = "lima";       // sekarang string – boleh!
data = [5];          // sekarang array – boleh juga!
```

### 5.11 Konversi Tipe (Type Conversion)

```javascript
10 + " objects"    // => "10 objects"  – angka 10 diubah jadi string (karena + dengan string = gabung teks)
"7" * "4"          // => 28            – kedua string diubah jadi angka (karena * hanya untuk angka)
var n = 1 - "x";   // => NaN           – string "x" tidak bisa diubah jadi angka
n + " objects"     // => "NaN objects" – NaN diubah jadi string "NaN"
```
🎯 **Aturan praktis:** Operator `+` dengan string → **menggabungkan teks**. Operator `-`, `*`, `/` → **mencoba mengubah ke angka**.

### 5.12 Type Coercion: `==` vs `===`

- Operator **`==`** dan **`!=`** melakukan **"type coercion"** (memaksa konversi tipe sebelum membandingkan).
- **Lebih baik pakai `===` (strict equality) dan `!==`**, yang **tidak melakukan konversi tipe**.
```javascript
"5" == 5     // true,  karena "5" dikonversi jadi 5 dulu
"5" === 5    // false, tidak ada konversi; string ≠ number
0 == false   // true   (aneh, kan?)
0 === false  // false
```

---

### 5.13 Perulangan `for`

**Iterasi semua elemen array:**
```javascript
for (var i = 0; i < array.length; i += 1) {
    // di dalam loop:
    // i adalah indeks elemen saat ini
    // array[i] adalah elemen saat ini
}
```

**Iterasi semua member (property) objek:**
```javascript
for (var name in object) {
    if (object.hasOwnProperty(name)) {
        // name adalah KUNCI member saat ini
        // object[name] adalah NILAI saat ini
    }
}
```
- `hasOwnProperty` dipakai untuk memastikan property itu **milik objek itu sendiri**, bukan warisan dari prototype.

### 5.14 `switch`

- **Percabangan banyak arah** (*multiway branch*).
- **Nilai switch tidak harus angka – boleh string.**
- **Nilai `case` boleh berupa ekspresi.**
```javascript
switch (expression) {
case ';':
case ',':
case '.':
    punctuation();        // dijalankan jika expression adalah ; atau , atau .
    break;                // break: keluar dari switch (tanpa ini, eksekusi "jatuh" ke case berikutnya)
default:
    noneOfTheAbove();     // jika tidak ada case yang cocok
}
```

### 5.15 `throw` dan `try`

**Melempar error:**
```javascript
throw new Error(reason);          // cara standar

throw {                           // atau melempar objek sendiri
    name: exceptionName,
    message: reason
};
```
**Menangkap error:**
```javascript
try {
    ...                           // kode yang mungkin error
} catch (e) {
    switch (e.name) {
    case 'Error':
        ...                       // tangani error jenis 'Error'
        break;
    default:
        throw e;                  // error lain: lempar lagi ke atas
    }
}
```
Nama-nama exception bawaan JavaScript: **`Error`, `EvalError`, `RangeError`, `SyntaxError`, `TypeError`, `URIError`**.

---

### 5.16 Fungsi (Function)

- **Mekanisme untuk menyusun program** agar **modular** (terbagi rapi) dan **reusable** (bisa dipakai ulang).
- **Jenis:** **built-in** (bawaan, misal `alert()`, `parseInt()`) dan **user-defined** (buatan sendiri).
- Fungsi **bisa punya parameter** dan **bisa mengembalikan nilai**.
- **Fungsi bisa didefinisikan di dalam fungsi lain.**

Sintaks:
```javascript
function namaFungsi([parameter]) {
    ... statements
    [return value]
}
```

#### 4 cara mendefinisikan fungsi
```javascript
// 1. Definisi biasa (function declaration)
function f(x, y) { return x * y; }

// 2. Constructor Function() – jarang dipakai
var f = new Function("x", "y", "return x*y;");

// 3. Function literal (function expression)
var f = function(x, y) { return x * y; };

// 4. Arrow function (ES6)
var f = (x, y) => x * y;               // ✅ bentuk singkat, otomatis return
var f = (x, y) => { return x * y; };   // ✅ bentuk dengan kurung kurawal, return manual
```
⚠️ **Koreksi slide:** di slide tertulis `var f = (x, y) => return x*y;` – ini **error sintaks**. Kalau pakai kata `return`, wajib pakai `{ }`. Tanpa `{ }`, langsung tulis ekspresinya saja.

#### Fungsi bisa diperlakukan sebagai data (disimpan di variabel)
```javascript
function square(x) { return x * x; }
var b = square;     // b sekarang menunjuk ke fungsi square (TANPA tanda kurung!)
var c = b(5);       // c = 25
```

#### Mengakses argumen
- Argumen bisa diakses **lewat nama parameter** atau lewat **objek `arguments`**:
```javascript
function square(x) { return arguments[0] * x; }   // arguments[0] sama dengan x
```
- **Pemeriksaan argumen di JavaScript dilakukan saat runtime** (JS tidak protes kalau kamu memanggil fungsi dengan jumlah argumen yang salah; parameter yang kurang akan bernilai `undefined`).

### 5.17 Closure

**Closure** = **kombinasi sebuah fungsi yang "dibungkus" bersama referensi ke lingkungan sekitarnya (*lexical environment*)**. Dengan kata lain, **closure memberi fungsi akses ke scope luarnya**. Di JS, closure **tercipta setiap kali fungsi dibuat**, pada saat fungsi dibuat.

**Contoh 1:**
```javascript
function makeFunc() {
  const name = "Mozilla";       // variabel lokal makeFunc
  function displayName() {
    console.log(name);          // displayName "mengingat" name dari luar
  }
  return displayName;           // kembalikan FUNGSINYA (bukan hasil panggilan)
}

const myFunc = makeFunc();      // makeFunc sudah selesai dijalankan...
myFunc();                       // ...tapi ini tetap mencetak "Mozilla"!
```
Normalnya, variabel lokal hilang setelah fungsi selesai. Tapi karena `displayName` masih memegang referensi ke `name`, variabel itu **tetap hidup**. Itulah closure.

**Contoh 2 – "pabrik fungsi":**
```javascript
function makeAdder(x) {
  return function (y) {
    return x + y;               // x "diingat" dari saat makeAdder dipanggil
  };
}

const add5 = makeAdder(5);      // fungsi yang selalu menambah 5
const add10 = makeAdder(10);    // fungsi yang selalu menambah 10

console.log(add5(2));  // 7
console.log(add10(2)); // 12
```

💡 **Analogi:** Closure seperti **ransel**. Saat fungsi "lahir" di dalam fungsi lain, ia membawa ransel berisi variabel-variabel dari tempat kelahirannya. Ke mana pun fungsi itu pergi, ranselnya ikut.

### 5.18 Array & Method-nya

**Membuat array:**
```javascript
var a = new Array();               // dengan constructor (kosong)
var a = new Array(1, 2, "tiga");   // dengan constructor (berisi)
var a = [1, 2, "tiga"];            // dengan literal (paling umum) – boleh campur tipe!
```
**Akses:**
```javascript
a[0] = 1;
a[7] = 8;      // boleh! indeks 3–6 jadi "kosong" (empty/undefined), panjang jadi 8
a.length;      // panjang array
```
**Method penting:**
| Method | Fungsi |
|---|---|
| `join()` | Mengubah isi array menjadi **string** (`["a","b"].join("-")` → `"a-b"`) |
| `reverse()` | **Membalik** urutan array |
| `sort()` | **Mengurutkan** isi array (default: **secara alfabetis**/sebagai string!) |
| `concat()` | **Menggabungkan** array dengan nilai/array dari parameter |
| `slice()` | **Mengambil potongan** array (tidak mengubah array asli) |
| `splice()` | **Menghapus dan/atau menambah** elemen (mengubah array asli) |
| `push()` / `pop()` | Tambah / ambil elemen **di akhir** |
| `shift()` / `unshift()` | Ambil / tambah elemen **di awal** |

⚠️ `sort()` default mengurutkan sebagai teks: `[10, 9, 1].sort()` → `[1, 10, 9]`. Untuk angka: `arr.sort((a, b) => a - b)`.

**Contoh lengkap dari slide:**
```html
<html>
<script type="text/javascript">
// cara 1 pendefinisian array
mhs = new Array();
mhs[0] = "Bevin";
mhs[1] = "Andini";
mhs[2] = "Citra";
// cara 2 pendefinisian array
mhs = new Array("Bevin", "Andini", "Citra");
// cara 3 pendefinisian array
mhs = ["Bevin", "Andini", "Citra"];
// cara pengaksesan array
document.write("Mahasiswa pertama adalah " + mhs[0] + ".<br>");   // Bevin
document.write("Mahasiswa terakhir adalah " + mhs[2] + ".<br>");  // Citra
// cara pengaksesan array menggunakan loop
for (i = 0; i < mhs.length; i++) {
  document.write(mhs[i] + "<br>");
}
// mengurutkan array
mhs.sort();
// menggabungkan array
document.write(mhs.join("-"));   // Andini-Bevin-Citra
</script>
</html>
```

### 5.19 Object

- JavaScript adalah bahasa **berbasis prototype**.
- Objek bisa dibuat dengan **constructor**: `var now = new Date();`
- Objek bisa dibuat dari **literal**: `var circle = { x: 0, y: 0, radius: 2 };`
- Kata kunci **`class`** diperkenalkan di **ES6 (2015)**.
- Atribut & method diakses dengan **titik (`.`)**:
```javascript
var book = new Object();
book.title = "Javascript: The Definitive Guide";
book.author = "David Flanagan";
```
- Akses property bisa dengan **titik** atau seperti **array asosiatif**:
```javascript
book.title       // cara titik
book["title"]    // cara kurung siku – berguna jika nama property disimpan di variabel
```
- Property bisa **di-enumerasi (dijelajahi) dengan for-in**:
```javascript
for (var name in obj) {
  document.write(name + "<BR>");
}
```
- Property bisa **dihapus**:
```javascript
delete book.title;
```

Contoh `class` (ES6):
```javascript
class Mahasiswa {
  constructor(nama, nim) { this.nama = nama; this.nim = nim; }
  sapa() { return "Halo, saya " + this.nama; }
}
const m = new Mahasiswa("Andini", "13523001");
m.sapa();   // "Halo, saya Andini"
```

---

### 5.20 Objek-Objek Browser

| Objek | Fungsi |
|---|---|
| `window` | Jendela browser (objek paling atas) |
| `navigator` | Info tentang browser (nama, versi, bahasa) |
| `screen` | Info layar (lebar, tinggi) |
| `history` | Riwayat navigasi (`history.back()`) |
| `location` | URL saat ini (`location.href`) – bisa dipakai untuk pindah halaman |

**Bukan standar ECMAScript, tapi disediakan di setiap browser.**

- **`window`** adalah **konteks eksekusi global**:
```javascript
var x = 5;
// sama dengan
window.x = 5;
```
  `window` merujuk ke **jendela browser (atau frame)** yang menampilkan halaman.
- **`document`** = **halaman yang sedang ditampilkan**, diakses sebagai `window.document` (atau cukup `document`).

### 5.21 DOM (Document Object Model)

**DOM** adalah **antarmuka pemrograman** yang **merepresentasikan struktur HTML halaman web sebagai pohon objek di memori browser**.

Contoh method:
```javascript
getElementById(id)            // cari elemen berdasarkan id
getElementsByTagName(tag)     // cari semua elemen dengan tag tertentu
createElement(tag)            // buat elemen baru
```

**`HTMLDocument`** (turunan dari `Document`) punya property & method seperti:
- `body`, `forms`, `anchors`, `images`, `links`
- `title`, `lastModified`, `referrer`
- `close()`, `open()`, `write()`

Hierarki objek browser (Flanagan, *JavaScript: The Definitive Guide*, 5th Ed.):
```
The Current Window
├── self, window, parent, top   (berbagai objek Window)
├── navigator                   (objek Navigator)
├── frames[]                    (array objek Window)
├── location                    (objek Location)
├── history                     (objek History)
├── document                    (objek Document)
│   ├── forms[]    (array objek Form)
│   │   └── elements[]  (array elemen form HTML: Input, Select, Textarea)
│   │        └── options[]  (array objek Option)
│   ├── anchors[]  (array objek Anchor)
│   ├── links[]    (array objek Link)
│   ├── images[]   (array objek Image)
│   └── applets[]  (array applet)
└── screen                      (objek Screen)
```

### 5.22 JavaScript sebagai Event Handler

**Event** = **kejadian** di halaman (klik, ketik, halaman selesai dimuat, dll). Elemen HTML bisa diatur untuk **menjalankan script ketika sebuah event terjadi**:
- `onclick` (diklik), `onmousedown` (tombol mouse ditekan), `onmouseup` (dilepas), `onchange` (nilai berubah), `onload` (selesai dimuat).

**Inline di HTML:**
```html
<input type="checkbox" name="options" value="giftwrap"
       onclick="giftwrap = this.checked;">
```
→ Saat checkbox diklik, variabel `giftwrap` diisi status centangnya (`this` = elemen checkbox itu).

### 5.23 Event Listener

- **Event handler** = fungsi yang **menangani** ketika event tertentu terjadi **pada target tertentu**.
- **Objek yang merepresentasikan event dikirim sebagai argumen pertama** ke event handler. Objek ini mengimplementasikan/diturunkan dari **interface `Event`** (berisi info seperti elemen mana yang diklik, posisi mouse, dll).
- Event **tidak hanya untuk UI**, tapi juga dipakai di **worker, WebSocket**, dll.

**Dua pendekatan:**

1. **Properti `onevent`** (misal `onclick`) – **sederhana**, **satu-ke-satu** (satu event satu handler):
```javascript
const btn = document.querySelector('button');
function greet(event) {
  console.log('greet:', event);
}
btn.onclick = greet;          // kalau di-assign lagi, handler lama tertimpa
```

2. **`addEventListener` dan `removeEventListener`** – **sangat fleksibel dan spesifik**, **bisa memasang banyak listener** pada satu target, dan bisa menentukan **fase bubbling/capture** (untuk event DOM):
```javascript
const btn = document.querySelector('button');
function greet(event) {
  console.log('greet:', event);
}
btn.addEventListener('click', greet);
// btn.removeEventListener('click', greet);   // untuk melepas
```

#### Contoh dari slide (`window.onload`)
Kode di slide terpotong; versi lengkapnya kira-kira begini:
```html
<p id="clock"></p>
<script>
// Definisikan fungsi untuk menampilkan waktu sekarang
function displayTime() {
  var elt = document.getElementById("clock");   // cari elemen dengan id="clock"
  var now = new Date();                          // ambil waktu sekarang
  elt.innerHTML = now.toLocaleTimeString();      // tampilkan
  setTimeout(displayTime, 1000);                 // ulangi tiap 1 detik
}
window.onload = displayTime;   // mulai menampilkan waktu saat dokumen selesai dimuat
</script>
```

### 5.24 Event Bubbling & Capturing

Standar DOM Events mendeskripsikan **3 fase perambatan (propagasi) event**:
1. **Capturing phase** – event **turun** dari `window` menuju elemen target.
2. **Target phase** – event **sampai di elemen target**.
3. **Bubbling phase** – event **naik (menggelembung)** dari elemen target kembali ke atas.

```
   window        ▲
     │     │     │
  document │     │
     │     │     │
   html    │     │         Contoh: kamu klik #buttonOne
     │   CAPTURE BUBBLE
   body    │     │         Capture: window → document → html → body → #one_a → #two → #three_a → #buttonOne
     │     │     │         Bubble : #buttonOne → #three_a → #two → #one_a → body → html → document → window
  #one_a   │     │
     │     │     │
   #two    │     │
     │     │     │
 #three_a  ▼     │
     │
 #buttonOne  (TARGET)
```
💡 **Analogi:** Kamu melempar batu ke kolam di dalam taman di dalam kompleks. Capture = berita "ada lemparan" turun dari satpam kompleks → penjaga taman → ke kolam. Bubble = riak air dari kolam menyebar kembali ke atas. Secara default, `addEventListener` mendengarkan di **fase bubbling**; untuk capturing pakai `addEventListener('click', fn, true)`.

### 5.25 Eksekusi JavaScript di HTML

- JavaScript **dieksekusi segera setelah elemen `<script>` selesai diproses** browser.
- Untuk **menunda eksekusi**:
  - Gunakan atribut **`defer`**: `<script src="app.js" defer></script>` → script diunduh paralel, dijalankan **setelah HTML selesai di-parse**.
  - **ATAU** tulis script sebagai fungsi yang dipanggil oleh atribut **`onload`** di elemen `body`: `<body onload="init()">`.

### 5.26 Model Threading JavaScript

- JavaScript berjalan **berurutan**, dengan model **single thread** (satu jalur eksekusi).
- Saat memuat dokumen HTML, **menjalankan script akan menghentikan sementara proses loading** sampai eksekusinya selesai.
- **Event handler JavaScript TIDAK BOLEH lama**, karena akan membuat **browser membeku (freeze)**.

---

### 5.27 W3C DOM

- **W3C (World Wide Web Consortium)** adalah kelompok internasional utama yang **membuat standar terbuka dan panduan untuk Internet**.
- **Dokumen HTML diakses JavaScript melalui DOM.**

Contoh:
```html
<html> <head> <title>Sample Document</title> </head> <body> <h1>An HTML Document</h1> <p>This is a <i>simple</i> document.</p> </html>
```
Pohon DOM-nya:
```
Document
└── <html>
    ├── <head>
    │   └── <title>
    │       └── "Sample Document"
    └── <body>
        ├── <h1>
        │   └── "An HTML Document"
        └── <p>
            ├── "This is a"
            ├── <i>
            │   └── "simple"
            └── "document."
```

#### Hubungan antar-node
Contoh: `<p><strong>Hello</strong> how are you doing?</p>`
```
                 ┌──────── p ────────┐
      firstChild │                   │ lastChild
                 ▼                   ▼
             strong  ──nextSibling──►  "how are you doing?"
                     ◄─previousSibling─
                 │
     firstChild, │ lastChild
                 ▼
              "hello"
(Setiap anak punya parentNode yang menunjuk ke induknya)
```
| Property | Arti |
|---|---|
| `parentNode` | Induk |
| `firstChild` | Anak pertama |
| `lastChild` | Anak terakhir |
| `nextSibling` | Saudara berikutnya |
| `previousSibling` | Saudara sebelumnya |

#### Hierarki tipe Node
**Elemen HTML di DOM adalah turunan dari `Node`:**
```
Node
├── Document ──── HTMLDocument
├── CharacterData
│   ├── Text
│   └── Comment
├── Element ───── HTMLElement
│                 ├── HTMLHeadElement
│                 ├── HTMLBodyElement
│                 ├── HTMLTitleElement
│                 ├── HTMLParagraphElement
│                 ├── HTMLInputElement
│                 ├── HTMLTableElement
│                 └── ... etc.
└── Attr
```

#### Mengakses elemen
```javascript
document.getElementById(id_name);
// → mengembalikan objek Node (jika ada), atau null

document.getElementsByTagName(tag_name);
// → mengembalikan NodeList, bisa diakses seperti array

document.getElementsByTagName("h1")[0];        // h1 pertama

var li = document.getElementsByTagName("li");  // semua <li>
for (var j = 0; j < li.length; j++) {
  li[j].style.border = "1px solid #000";       // beri border hitam ke setiap <li>
}
```
(Cara modern: `document.querySelector("css-selector")` dan `document.querySelectorAll(...)`.)

---

### 5.28 Manajemen Memori

Di bahasa apa pun, **siklus hidup memori** hampir selalu sama:
1. **Alokasi** memori yang dibutuhkan. Di JS, memori **dialokasikan otomatis** saat nilai pertama kali dideklarasikan.
2. **Gunakan** memori yang dialokasikan (baca, tulis) – lewat membaca/menulis variabel, property objek, atau mengirim argumen ke fungsi.
3. **Lepaskan** memori saat tidak dibutuhkan lagi. JS **otomatis membebaskannya** saat tidak dipakai (**garbage collection**).
```javascript
let text = "Hello";   // alokasi memori untuk string
console.log(text);    // string dibaca
// (string akan dibebaskan otomatis saat tidak dipakai lagi)
```

### 5.29 Garbage Collection (Pengumpulan Sampah)

- Konsep utama: **referensi**. Sebuah objek dikatakan **mereferensikan** objek lain jika objek pertama **punya akses** ke objek kedua (implisit maupun eksplisit).
- **Algoritma Reference-counting:** objek dianggap "tidak dibutuhkan" jika **tidak ada lagi objek lain yang mereferensikannya**. (**Sudah tidak dipakai di engine JS modern** – masalahnya: dua objek yang saling mereferensikan (*circular reference*) tidak akan pernah dibersihkan.)
- **Algoritma Mark-and-sweep:** menyederhanakan definisi "objek tidak dibutuhkan" menjadi "**objek tidak bisa dijangkau (unreachable)**". GC mulai dari "akar" (objek global), **menandai (mark)** semua yang bisa dijangkau, lalu **menyapu (sweep)** sisanya. **Semua engine modern memakai mark-and-sweep.**

💡 **Analogi mark-and-sweep:** Petugas kebersihan memegang benang dari pintu kantor (akar), menyusuri semua meja yang terhubung benang dan memberi stiker "masih dipakai". Meja tanpa stiker = dibuang.

### 5.30 Memory Leak (Kebocoran Memori)

**Memory leak** terjadi saat program **tetap memegang referensi ke objek yang sudah tidak dibutuhkan**, sehingga garbage collector **tidak bisa membersihkannya**.

```javascript
function leakyFunction() {
  leakyVar = "I'm a global now";   // Oops! Tanpa 'var', 'let', atau 'const'
}
```
Fungsi ini membuat **variabel global** (`window.leakyVar`) yang **akan hidup sampai halaman ditutup**. (Pakai `"use strict";` agar kesalahan seperti ini langsung jadi error.)

---

### 5.31 JavaScript Runtime Model

- JavaScript punya **runtime model berbasis *event loop***, yang bertanggung jawab untuk **mengeksekusi kode, mengumpulkan & memproses event, dan menjalankan sub-tugas yang mengantre**. Model ini **cukup berbeda dari bahasa lain seperti C dan Java**.
- Program JavaScript bersifat ***single-threaded***. **Thread** = urutan instruksi yang diikuti program. Karena hanya ada satu thread, JS **hanya bisa melakukan satu hal dalam satu waktu**: kalau sedang menunggu panggilan sinkron yang lama, ia **tidak bisa melakukan hal lain**.

```
┌───────────── JS Engine ─────────────┐        ┌──────── Web APIs ────────┐
│  ┌──────┐        ┌────────────┐     │        │  DOM (document)          │
│  │ Heap │        │ Call stack │     │ ─────► │  AJAX (XMLHttpRequest)   │
│  │(objek│        │ (fungsi yg │     │        │  Timeout (setTimeout)    │
│  │  di  │        │  sedang    │     │        └────────────┬─────────────┘
│  │memori│        │  jalan)    │     │                     │ callback selesai
│  └──────┘        └────────────┘     │                     ▼
└─────────────────────▲───────────────┘        ┌────────────────────────────┐
                      │                        │      Callback Queue        │
                      └──── Event Loop ◄────── │  [onClick][onDone][onLoad] │
                                               └────────────────────────────┘
```
(Sumber: https://felixgerschau.com/javascript-event-loop-call-stack/)

### 5.32 Konsep Runtime: Call Stack

**Pemanggilan fungsi membentuk tumpukan (stack) frame.**
```javascript
function foo(b) {
  const a = 10;
  return a + b + 11;
}

function bar(x) {
  const y = 3;
  return foo(x * y);
}

const baz = bar(7); // assigns 42 to baz
```
Urutan operasi:
1. Saat memanggil **`bar`**, **frame pertama** dibuat berisi referensi ke argumen & variabel lokal `bar` (x=7, y=3).
2. Saat `bar` memanggil **`foo`**, **frame kedua** dibuat dan **ditumpuk di atas** frame pertama, berisi argumen & variabel lokal `foo` (b=21, a=10).
3. Saat `foo` **return** (10 + 21 + 11 = 42), frame teratas **di-pop (dikeluarkan)** dari stack, menyisakan frame `bar`.
4. Saat `bar` return (42), **stack kosong**. `baz = 42`.

```
Langkah:     1            2            3            4
                       ┌───────┐
                       │foo b=21│
           ┌───────┐   ├───────┤   ┌───────┐
           │bar x=7│   │bar x=7│   │bar x=7│      (kosong)
           └───────┘   └───────┘   └───────┘
```
- Catatan: **argumen dan variabel lokal bisa tetap ada** setelah frame di-pop, karena **disimpan di luar stack** (misalnya jika ditangkap closure).
- Lihat animasinya: https://s.id/nu3r7

💡 **Analogi:** Tumpukan piring – piring terakhir yang ditaruh adalah yang pertama diambil (**LIFO: Last In First Out**).

### 5.33 Konsep Runtime: Heap

**Objek dialokasikan di *heap*** – sebutan untuk **wilayah memori yang besar dan (sebagian besar) tidak terstruktur**.
```
┌Stack┐┌──────────── Heap ────────────┐
│     ││   Object    Object           │
│     ││        Object    Object      │
│Frame││     Object     Object        │
│Frame││   Object                     │
│Frame││                              │
│Frame││                              │
├─────┴┴──────────────────────────────┤
│ [Message][Message][Message]  Queue  │
└─────────────────────────────────────┘
```

### 5.34 Konsep Runtime: Queue

- Runtime JS memakai **message queue** – **daftar pesan yang menunggu diproses**. Setiap pesan punya **fungsi terkait** yang dipanggil untuk menangani pesan itu.
- Event loop memproses **dua antrean**:
  - **Task Queue (Macrotask):** berisi unit kerja terjadwal seperti **script, event pengguna, timer, dan callback asinkron lainnya**. Contoh: menjalankan **event klik tombol**, callback **`setTimeout()` / `setInterval()`**.
  - **Microtask Queue:** berisi **pekerjaan lanjutan pendek berprioritas tinggi** yang dijalankan **setelah task saat ini selesai, tapi sebelum task berikutnya dimulai**. Contoh: callback **Promise** seperti **`.then()`, `.catch()`, `.finally()`**.

### 5.35 Event Loop

Event loop dinamai begitu karena implementasinya biasanya mirip:
```javascript
while (queue.waitForMessage()) {   // tunggu sampai ada pesan
  queue.processNextMessage();       // proses pesan berikutnya
}
```
- Visualisasi bagus: https://www.youtube.com/watch?v=eiC58R16hb8
- **Sifat menarik event loop:** JavaScript, tidak seperti banyak bahasa lain, **tidak pernah memblokir (never blocks)**. Operasi I/O (seperti request jaringan) dilakukan lewat event & callback, jadi sambil menunggu, JS tetap bisa memproses input pengguna.
- Pengecualian lama (legacy) ada, seperti **`alert`** atau **XHR sinkron**, tapi **sebaiknya dihindari**.

#### Diagram "JavaScript Event Loop in the Browser"
```
 ┌ Call Stack ┐                           ┌─ Microtask Queue ─┐
 │Function Call│ ───────►  ( EVENT  ) ───►│ Promise.then      │
 │Function Call│           (  LOOP  )     │ Promise.catch     │
 │Function Call│        Checks & Moves    │ queueMicrotask    │
 └──────▲──────┘           Tasks          └─────────┬─────────┘
        │ Async Operations   │  ╲ Process Microtasks │
        │ (timer, AJAX,      │   ╲                   ▼
        │  DOM Events)       │    ╲─────────►┌─ Callback Queue (Macrotasks) ─┐
 ┌──────┴──────┐             │ Process      │ setTimeout                    │
 │  Web APIs   │◄────────────┘ Macrotasks   │ Fetch Callback                │
 │(Async Tasks)│      [JavaScript Engine]   │ Event Handler                 │
 └─────────────┘                            └───────────────────────────────┘
```

#### 🧩 Uji pemahaman event loop (contoh tambahan)
```javascript
console.log("1");                                   // sinkron
setTimeout(() => console.log("2"), 0);              // macrotask
Promise.resolve().then(() => console.log("3"));     // microtask
console.log("4");                                   // sinkron
```
**Output: 1, 4, 3, 2.**
Alasan: kode sinkron (1, 4) jalan dulu di call stack → setelah stack kosong, **semua microtask** dijalankan (3) → baru **macrotask** berikutnya (2). Walaupun `setTimeout` bernilai 0 ms, ia tetap menunggu di antrean macrotask.

---

### 5.36 JavaScript Asinkron

**Asinkron** = ketika beberapa hal terkait terjadi **tanpa harus menunggu hal sebelumnya selesai**.

Contoh di slide: program **sinkron** dengan algoritma yang **sangat tidak efisien** untuk menghasilkan banyak bilangan prima besar:
```javascript
const MAX_PRIME = 1000000;

function isPrime(n) {
  for (let i = 2; i <= Math.sqrt(n); i++) {
    if (n % i === 0) {
      return false;
    }
  }
  return n > 1;
}

const random = (max) => Math.floor(Math.random() * max);

function generatePrimes(quota) {
  const primes = [];
  while (primes.length < quota) {
    const candidate = random(MAX_PRIME);
    if (isPrime(candidate)) {
      primes.push(candidate);
    }
  }
  return primes;
}
```
**Bagaimana jika fungsi sinkron ini butuh waktu lama?** Kelemahan model runtime JS: **jika satu fungsi terlalu lama, aplikasi web tidak bisa memproses interaksi pengguna** seperti klik atau scroll (halaman "hang"). Browser mengatasinya dengan dialog **"a script is taking too long to run"**.

Solusinya: pemrograman asinkron (callback, Promise, async/await) dan/atau Web Worker.

### 5.37 Callback

**Callback** = **fungsi yang dikirim ke fungsi lain**, dengan harapan callback itu **dipanggil pada waktu yang tepat**.

Masalahnya: kode berbasis callback **sulit dipahami** ketika callback itu sendiri harus memanggil fungsi yang juga menerima callback. Ini umum terjadi jika operasimu terdiri dari **serangkaian fungsi asinkron**.
```javascript
function doStep1(init, callback) {
  const result = init + 1;
  callback(result);
}

function doStep2(init, callback) {
  const result = init + 2;
  callback(result);
}

function doStep3(init, callback) {
  const result = init + 3;
  callback(result);
}

function doOperation() {
  doStep1(0, (result1) => {
    doStep2(result1, (result2) => {
      doStep3(result2, (result3) => {
        console.log(`result: ${result3}`);   // result: 6
      });
    });
  });
}

doOperation();
```
Kode yang menjorok ke kanan seperti ini disebut **"callback hell"** atau **"pyramid of doom"** (piramida malapetaka).

💡 **Analogi callback:** Kamu titip pesan ke tukang laundry: "Kalau sudah selesai, **telepon saya** ya." Nomor teleponmu = callback.

### 5.38 Promise

**Promise** = objek yang **merepresentasikan penyelesaian (atau kegagalan) dari operasi asinkron di masa depan** beserta nilai hasilnya. Promise memungkinkanmu **mengaitkan handler** dengan nilai sukses atau alasan gagal dari aksi asinkron.

**3 status Promise:**
| State | Arti |
|---|---|
| **pending** | Status awal – **belum** fulfilled maupun rejected. |
| **fulfilled** | Operasi **berhasil** diselesaikan. |
| **rejected** | Operasi **gagal**. |

```
           fulfill ──► .then(onFulfillment) ──► (async actions) ──┐
 Promise                                                          │ return
 (pending)                                                        ▼
           reject  ──► .then(..., onRejection) / .catch(onRejection) ──return──► Promise (pending) ──► .then() .catch() ...
                              │
                              └──► (error handling)
```
(Setelah fulfilled atau rejected, promise disebut ***settled***.)

💡 **Analogi Promise:** **Struk/nomor antrean** di restoran cepat saji. Saat pesan, kamu dapat nomor (promise, status *pending*). Nanti salah satunya terjadi: makanan siap (*fulfilled*) atau "maaf, habis" (*rejected*).

### 5.39 Membuat Promise dan Chaining

```javascript
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("foo");          // setelah 300 ms, promise fulfilled dengan nilai "foo"
  }, 300);
});

myPromise
  .then(handleFulfilledA, handleRejectedA)
  .then(handleFulfilledB, handleRejectedB)
  .then(handleFulfilledC, handleRejectedC);
```
- **Promise chain** dibutuhkan saat operasimu terdiri dari **beberapa fungsi asinkron** dan **setiap fungsi harus selesai sebelum yang berikutnya dimulai**.
- Fungsi khusus **`then()` mengembalikan promise baru `p`**. Status `p` bergantung pada apa yang dilakukan handler:
  - **Mengembalikan nilai** → `p` **fulfilled** dengan nilai itu.
  - **Tidak mengembalikan apa-apa** → `p` **fulfilled** dengan nilai `undefined`.
  - **Melempar error** → `p` **rejected** dengan error itu.
  - **Mengembalikan promise lain yang masih pending** → `p` **ikut pending**, lalu fulfilled/rejected **mengikuti promise itu** segera setelah promise itu selesai.

Contoh callback hell tadi ditulis ulang dengan Promise:
```javascript
const step = (init, add) => Promise.resolve(init + add);
step(0, 1)
  .then(r1 => step(r1, 2))
  .then(r2 => step(r2, 3))
  .then(r3 => console.log(`result: ${r3}`))   // result: 6
  .catch(err => console.error(err));          // satu catch untuk semua error
```

### 5.40 Async / Await

- Kata kunci **`async`** memberi cara **lebih sederhana** untuk bekerja dengan kode asinkron berbasis promise. Menambahkan `async` di depan fungsi membuatnya jadi **async function** (selalu mengembalikan Promise).
- **Di dalam async function**, kamu bisa memakai **`await`** sebelum memanggil fungsi yang mengembalikan promise – artinya "**tunggu sampai promise ini selesai**, lalu lanjut" (tanpa memblokir thread utama).
- `async`/`await` membuat lebih mudah menyusun operasi dari **serangkaian fungsi asinkron berurutan**, **tanpa perlu promise chain eksplisit**, dan kodenya **terlihat seperti kode sinkron** (gaya lebih bersih).

```javascript
async function fetchProducts() {
  try {
    // setelah baris ini, fungsi akan menunggu panggilan `fetch()` selesai
    // `fetch()` akan mengembalikan Response atau melempar error
    const response = await fetch(
      "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
    );
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    // setelah baris ini, fungsi akan menunggu `response.json()` selesai
    // `response.json()` akan mengembalikan objek JSON hasil parsing atau melempar error
    const data = await response.json();
    console.log(data[0].name);
  } catch (error) {
    console.error(`Could not get products: ${error}`);
  }
}

fetchProducts();
```

🎯 **Ringkas evolusi:** Callback (rawan callback hell) → Promise (`.then().catch()`, lebih rapi) → async/await (paling mudah dibaca, tapi di baliknya tetap Promise).

---

### 5.41 TypeScript

**Apa itu TypeScript?**
| Komponen | Penjelasan |
|---|---|
| **Bahasa pemrograman** | Mencakup **semua sintaks JavaScript**, **ditambah sintaks khusus TypeScript** untuk mendefinisikan dan memakai **tipe**. |
| **Type Checker** | Program yang membaca file JavaScript dan/atau TypeScript, lalu **memeriksa kesalahan** (terutama kesalahan tipe). |
| **Compiler (Transpiler)** | Program yang menjalankan type checker, **melaporkan masalah**, lalu **menghasilkan kode JavaScript yang setara** (karena browser hanya mengerti JS). |
| **Language Service** | Program yang memakai type checker untuk memberi tahu editor (seperti **VS Code**) cara menyediakan bantuan (autocomplete, peringatan merah, dll). |

**JS vs TS:**
| **JavaScript** | **TypeScript** |
|---|---|
| **Tidak dibatasi** (*unconstrained*) | **Dibatasi/terikat secara statis** |
| **Diperiksa saat runtime** | **Diperiksa saat compile** |
| Error **muncul saat runtime** (kebanyakan) | Error **muncul saat compile** (kebanyakan) |

```javascript
// JavaScript
function squareOf(n) {
  return n * n
}
squareOf(2)     // evaluates to 4
squareOf('z')   // evaluates to NaN   ← bug diam-diam, baru ketahuan saat jalan
```
```typescript
// TypeScript
function squareOf(n: number) {
  return n * n
}
squareOf(2)     // evaluates to 4
squareOf('z')   // Error TS2345: Argument of type '"z"' is not assignable to
                //               parameter of type 'number'.   ← ketahuan SEBELUM jalan
```
💡 **Analogi:** JS = menulis surat tanpa spell-checker (salah ketik baru ketahuan setelah dibaca penerima). TS = menulis dengan spell-checker (garis merah muncul saat kamu mengetik).

---

### ✅ Cek Pemahaman Bab 5

1. Apa beda `var`, `let`, `const`?
2. Apa output `console.log(x); var x = 3;`? Dan kalau `var` diganti `let`?
3. Sebutkan nilai-nilai falsy!
4. `"5" == 5` dan `"5" === 5` hasilnya apa?
5. Apa itu closure? Apa output `makeAdder(3)(4)`?
6. Urutkan output: `console.log('A'); setTimeout(()=>console.log('B')); Promise.resolve().then(()=>console.log('C')); console.log('D');`
7. Sebutkan 3 status Promise.
8. Apa beda `onclick = fn` dan `addEventListener('click', fn)`?

<details>
<summary>Lihat jawaban</summary>

1. `var`: scope fungsi, bisa dideklarasi ulang, di-hoist (undefined). `let`: scope blok, bisa diubah nilainya, tidak bisa dideklarasi ulang. `const`: scope blok, tidak bisa di-assign ulang (isi objek/array tetap bisa diubah).
2. `undefined` (karena hoisting). Dengan `let` → `ReferenceError`.
3. `0`, `false`, `null`, `undefined`, `""`, `NaN`.
4. `true` (ada type coercion) dan `false` (strict).
5. Fungsi + referensi ke lingkungan leksikal (scope luar) tempat ia dibuat. `makeAdder(3)(4)` = **7**.
6. **A, D, C, B**.
7. pending, fulfilled, rejected.
8. `onclick` hanya bisa satu handler (yang baru menimpa yang lama). `addEventListener` bisa banyak listener, bisa dilepas dengan `removeEventListener`, dan bisa memilih fase capture/bubble.
</details>

---
## Bab 6 – AJAX (03b-AJAX)

### 6.1 Apa itu AJAX?

**AJAX = Asynchronous JavaScript And XML.**

- ⚠️ **AJAX BUKAN bahasa pemrograman.** AJAX adalah **teknik** (cara).
- AJAX hanya **menggabungkan**:
  - Objek bawaan browser **`XMLHttpRequest`** (untuk **meminta data dari web server**), dan
  - **JavaScript + HTML DOM** (untuk **menampilkan atau memakai data** tersebut).
- Walaupun namanya ada "XML", data yang dikirim bisa berupa **teks biasa, HTML, atau JSON** (JSON paling umum sekarang).

### 6.2 Kenapa AJAX Disebut "Mimpi Developer"?

Dengan AJAX kamu bisa:
- **Mengupdate halaman web tanpa me-reload seluruh halaman.**
- **Meminta data dari server – setelah halaman selesai dimuat.**
- **Menerima data dari server – setelah halaman selesai dimuat.**
- **Mengirim data ke server – di latar belakang (background).**

💡 **Analogi:** Tanpa AJAX, setiap kali kamu ingin menambah satu lauk, pelayan **membawa pergi seluruh meja dan menggantinya dengan meja baru** (reload halaman penuh). Dengan AJAX, pelayan **cukup mengantar satu piring lauk tambahan** ke mejamu.

Contoh nyata: ketik di kotak pencarian Google → saran muncul tanpa reload; klik "like" di Instagram → angka like bertambah tanpa reload.

### 6.3 Interaksi AJAX

```
┌──────────── Browser ────────────┐                   ┌────────── Web Server ──────────┐
│  ┌──────────────────────────┐   │ 2) HTTP Request   │  ┌──────────────────────────┐  │
│  │     XMLHttpRequest       │───┼──────────────────►│  │ Business Logic           │  │
│  │   ┌──────────────┐       │   │                   │  │ Implementation           │  │
│  │   │  callback()  │◄──────┼───┼───────────────────│  │ (PHP, servlet, etc.)     │  │
│  │   └──────────────┘       │   │ 5) XML Data       │  └──────────┬───────▲───────┘  │
│  └────▲──────────┬──────────┘   │                   │   3) data   │       │ 4)       │
│       │1)        │6) HTML &     │                   │   exchange  ▼       │          │
│  JavaScript      │   CSS data   │                   │  ┌──────────────────────────┐  │
│     call         ▼              │                   │  │       Data Store         │  │
│  ┌──────────────────────────┐   │                   │  └──────────────────────────┘  │
│  │      Presentation        │   │                   └────────────────────────────────┘
│  └──────────────────────────┘   │
└─────────────────────────────────┘
```
1. **JavaScript call** – event di halaman (misal klik) memanggil fungsi JS yang membuat objek XMLHttpRequest.
2. **HTTP Request** – XMLHttpRequest mengirim request ke web server.
3. **Data exchange** – server (logika bisnis: PHP, servlet, dll) mengambil data dari **Data Store**.
4. Data dikembalikan ke logika bisnis.
5. **XML Data** (atau JSON/teks) dikirim balik ke browser → **`callback()`** dipanggil.
6. **HTML & CSS data** – callback memperbarui bagian **Presentation** (DOM) halaman.

### 6.4 Contoh AJAX

**HTML:**
```html
<!DOCTYPE html>
<html>
<body>

<div id="demo">
  <h2>Let AJAX change this text</h2>
  <button type="button" onclick="loadDoc()">Change Content</button>
</div>

</body>
</html>
```
Tampilan awal: judul **"Let AJAX change this text"** dan tombol **"Change Content"**.

**JavaScript:**
```javascript
function loadDoc() {
  var xhttp = new XMLHttpRequest();              // 1. buat objek XHR
  xhttp.onreadystatechange = function() {         // 2. siapkan "callback" saat status berubah
    if (this.readyState == 4 && this.status == 200) {   // 4 = selesai, 200 = OK
      document.getElementById("demo").innerHTML = this.responseText;  // 5. ganti isi div
    }
  };
  xhttp.open("GET", "ajax_info.txt", true);       // 3. siapkan request: GET, file, async=true
  xhttp.send();                                   // 4. kirim!
}
```
**File di server `ajax_info.txt`:**
```html
<h1>AJAX</h1>
<p>AJAX is not a programming language.</p>
<p>AJAX is a technique for accessing web servers from a web page.</p>
<p>AJAX stands for Asynchronous JavaScript And XML.</p>
```
**Hasil:** setelah tombol diklik, isi `div#demo` (judul + tombol) **diganti** dengan isi `ajax_info.txt` – **tanpa reload halaman**.

### 6.5 Objek `XMLHttpRequest`

- **Semua browser modern mendukung** objek XMLHttpRequest.
- XMLHttpRequest bisa dipakai untuk **bertukar data dengan server "di balik layar"**. Artinya kita bisa **mengupdate sebagian halaman tanpa me-reload seluruh halaman**.
- **Browser modern bisa memakai Fetch API** dengan cara yang **lebih sederhana** (lihat 6.15).

### 6.6 Akses Lintas Domain (Same-Origin Policy & CORS)

- Demi keamanan, browser modern **tidak mengizinkan akses lintas domain** → disebut **same-origin policy**.
- Artinya, halaman web dan file (misal XML) yang ingin dimuatnya **harus berada di *origin* yang sama**.
- **Origin = kombinasi skema (protokol) + domain + port.**

| URL halaman | URL yang diminta | Origin sama? |
|---|---|---|
| `https://itb.ac.id/a` | `https://itb.ac.id/b` | ✅ Ya |
| `https://itb.ac.id` | `http://itb.ac.id` | ❌ Skema beda |
| `https://itb.ac.id` | `https://api.itb.ac.id` | ❌ Domain beda |
| `https://itb.ac.id` | `https://itb.ac.id:8080` | ❌ Port beda |

- **Pengecualian** bisa diatur oleh server tujuan dengan mekanisme **CORS** (*Cross-Origin Resource Sharing*), misalnya server mengirim header:
```
Access-Control-Allow-Origin: https://example.com
```
→ "Saya (server API) mengizinkan halaman dari `https://example.com` membaca response saya."

💡 **Analogi:** Same-origin policy seperti satpam asrama yang hanya mengizinkan penghuni asrama yang sama masuk ke kamar. CORS = **surat izin tamu** dari pemilik kamar.

### 6.7 Method Objek XMLHttpRequest

| Method | Deskripsi |
|---|---|
| `new XMLHttpRequest()` | **Membuat** objek XMLHttpRequest baru |
| `abort()` | **Membatalkan** request saat ini |
| `getAllResponseHeaders()` | Mengembalikan **semua info header** response |
| `getResponseHeader()` | Mengembalikan **header tertentu** |
| `open(method, url, async, user, psw)` | **Menyiapkan** request:<br>• `method`: tipe request GET atau POST<br>• `url`: lokasi file<br>• `async`: `true` (asinkron) atau `false` (sinkron)<br>• `user`: username (opsional)<br>• `psw`: password (opsional) |
| `send()` | **Mengirim** request ke server – **dipakai untuk GET** |
| `send(string)` | **Mengirim** request ke server – **dipakai untuk POST** (string = isi body) |
| `setRequestHeader()` | **Menambahkan pasangan label/nilai ke header** yang akan dikirim |

### 6.8 Property Objek XMLHttpRequest

| Property | Deskripsi |
|---|---|
| `onreadystatechange` | Fungsi yang **dipanggil setiap kali `readyState` berubah** |
| `readyState` | **Status XMLHttpRequest:**<br>**0**: request belum diinisialisasi<br>**1**: koneksi ke server sudah dibuat<br>**2**: request diterima<br>**3**: request sedang diproses<br>**4**: **request selesai dan response siap** |
| `responseText` | Data response sebagai **string** |
| `responseXML` | Data response sebagai **data XML** |
| `status` | **Nomor status** request: **200**: "OK", **403**: "Forbidden", **404**: "Not Found" |
| `statusText` | **Teks status** (misal "OK" atau "Not Found") |

🎯 Karena itu kondisi klasik di callback adalah: `if (this.readyState == 4 && this.status == 200)` → "**selesai DAN sukses**".

### 6.9 GET atau POST?

- **GET lebih sederhana dan lebih cepat** daripada POST, dan bisa dipakai di kebanyakan kasus.
- **Tapi selalu gunakan POST** jika:
  - **File cache bukan pilihan** (misalnya meng-update file atau database di server).
  - **Mengirim data dalam jumlah besar** ke server (POST tidak punya batasan ukuran).
  - **Mengirim input pengguna** (yang bisa berisi karakter aneh/tak terduga) – POST **lebih tangguh dan "lebih aman"** daripada GET. *(Ingat dari Bab 3: "lebih aman" di sini maksudnya data tidak tampil di URL/history/log; enkripsi sesungguhnya tetap butuh HTTPS.)*

### 6.10 Mengirim Informasi dengan GET/POST

**GET** – data ditaruh di URL sebagai query string:
```javascript
xhttp.open("GET", "demo_get2.asp?fname=Henry&lname=Ford", true);
xhttp.send();
```
**POST** – data ditaruh di body, dan **wajib memberi tahu format datanya lewat header**:
```javascript
xhttp.open("POST", "demo_post2.asp", true);
xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
xhttp.send("fname=Henry&lname=Ford");
```

### 6.11 Property `onreadystatechange`

Dengan objek XMLHttpRequest, kamu bisa **mendefinisikan fungsi yang dijalankan saat request menerima jawaban**. Fungsi itu ditaruh di property `onreadystatechange`:
```javascript
xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    document.getElementById("demo").innerHTML = this.responseText;
  }
};
xhttp.open("GET", "ajax_info.txt", true);
xhttp.send();
```

### 6.12 Request Sinkron

- Untuk request **sinkron**, ubah **parameter ketiga `open()` menjadi `false`**.
- Kadang `async = false` dipakai untuk **tes cepat**.
- Karena kode akan **menunggu server selesai**, **tidak perlu fungsi `onreadystatechange`**:
```javascript
xhttp.open("GET", "ajax_info.txt", false);
xhttp.send();                                   // ← kode BERHENTI di sini sampai response datang
document.getElementById("demo").innerHTML = xhttp.responseText;
```
- ⚠️ **XMLHttpRequest sinkron (`async = false`) TIDAK direkomendasikan**, karena **JavaScript berhenti dieksekusi sampai response server siap** → halaman membeku (ingat Bab 5: JS single-threaded!).

### 6.13 Property `responseXML`

- XMLHttpRequest punya **parser XML bawaan**.
- `responseXML` mengembalikan response server sebagai **objek XML DOM** (bisa dijelajahi seperti DOM HTML).
```javascript
xmlDoc = xhttp.responseXML;
txt = "";
x = xmlDoc.getElementsByTagName("ARTIST");            // ambil semua elemen <ARTIST>
for (i = 0; i < x.length; i++) {
  txt += x[i].childNodes[0].nodeValue + "<br>";      // ambil teks di dalamnya
}
document.getElementById("demo").innerHTML = txt;
xhttp.open("GET", "cd_catalog.xml", true);
xhttp.send();
```

**Contoh lebih lengkap – membuat tabel dari XML:**
```javascript
function loadDoc() {
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      myFunction(this);                  // kirim objek xhttp ke fungsi pemroses
    }
  };
  xhttp.open("GET", "cd_catalog.xml", true);
  xhttp.send();
}

function myFunction(xml) {
  var i;
  var xmlDoc = xml.responseXML;
  var table = "<tr><th>Title</th><th>Artist</th></tr>";
  var x = xmlDoc.getElementsByTagName("CD");          // setiap <CD> di katalog
  for (i = 0; i < x.length; i++) {
    table += "<tr><td>" +
      x[i].getElementsByTagName("TITLE")[0].childNodes[0].nodeValue +
      "</td><td>" +
      x[i].getElementsByTagName("ARTIST")[0].childNodes[0].nodeValue +
      "</td></tr>";
  }
  document.getElementById("demo").innerHTML = table;
}
```
Misal `cd_catalog.xml` berisi:
```xml
<CATALOG>
  <CD><TITLE>Empire Burlesque</TITLE><ARTIST>Bob Dylan</ARTIST></CD>
  <CD><TITLE>Hide your heart</TITLE><ARTIST>Bonnie Tyler</ARTIST></CD>
</CATALOG>
```
Hasilnya tabel dengan kolom Title & Artist.

### 6.14 Method `getAllResponseHeaders()` dan `getResponseHeader()`

**`getAllResponseHeaders()`** – mengembalikan **semua informasi header** dari response server:
```javascript
var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    document.getElementById("demo").innerHTML = this.getAllResponseHeaders();
  }
};
```

**`getResponseHeader()`** – mengembalikan **header tertentu**:
```javascript
var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    document.getElementById("demo").innerHTML =
      this.getResponseHeader("Last-Modified");    // kapan file terakhir diubah
  }
};
xhttp.open("GET", "ajax_info.txt", true);
xhttp.send();
```

**Contoh untuk dicoba langsung:**
- AJAX sederhana: https://www.w3schools.com/xml/tryit.asp?filename=tryajax_first
- AJAX dengan fungsi callback: https://www.w3schools.com/xml/tryit.asp?filename=tryajax_callback

### 6.15 Fetch API

```javascript
async function getData() {
  const url = "https://example.org/products.json";
  try {
    const response = await fetch(url);                 // kirim request, tunggu Response
    if (!response.ok) {                                // ok = status 200–299
      throw new Error(`Response status: ${response.status}`);
    }

    const json = await response.json();                // ubah body jadi objek JS
    console.log(json);
  } catch (error) {
    console.error(error.message);
  }
}
```
- **Fetch API** menyediakan antarmuka untuk **mengambil resource (termasuk lewat jaringan)**. Ini adalah **pengganti XMLHttpRequest yang lebih powerful dan fleksibel**.
- Method **`fetch()` menerima satu argumen wajib: path ke resource** yang ingin diambil. Ia **mengembalikan Promise** yang akan *resolve* menjadi **Response** untuk request tersebut.

**Bandingkan XHR vs Fetch:**
| XMLHttpRequest | Fetch API |
|---|---|
| Berbasis callback (`onreadystatechange`) | Berbasis **Promise** (cocok dengan async/await) |
| Kode lebih panjang (cek readyState, status) | Lebih ringkas |
| Ada mode sinkron (tidak disarankan) | Selalu asinkron |

Contoh POST dengan fetch:
```javascript
await fetch("/api/users", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Alex", email: "alex@example.com" })
});
```

### 6.16 Web Platform APIs

- **XMLHttpRequest dan Fetch API adalah bagian dari Web Platform API** yang didefinisikan oleh badan standar **WHATWG** dan **W3C**.
  - https://fetch.spec.whatwg.org/
  - https://developer.mozilla.org/en-US/docs/Web/API
- ⚠️ **Mereka BUKAN bagian dari standar ECMAScript.** (Sama seperti DOM – disediakan oleh browser, bukan oleh bahasa JavaScript-nya. Ingat: di Bab 5, `fetch` muncul di bagian "Web APIs" pada diagram event loop.)

### 6.17 Use Case AJAX

| Use case | Contoh |
|---|---|
| **Autocomplete** | Saran pencarian Google |
| **Voting and Rating** | Tombol like/bintang |
| **Dynamically updating content** | Feed berita yang bertambah otomatis |
| **Form Submission & Validation** | Cek "username sudah dipakai" saat mengetik |
| **Chat Rooms and Instant Messaging** | Web chat |
| **Slicker UIs** | Antarmuka yang lebih mulus |
| **Widgets and ads** | Widget cuaca, iklan |
| **JIT (Just-in-time) Help system** | Bantuan muncul saat dibutuhkan |
| **Tooltips** | Info kecil saat mouse hover |
| **Accordion** | Panel buka-tutup yang isinya dimuat saat dibuka |
| **Tabbed pages** | Isi tab dimuat saat tab diklik |

### 6.18 Pola-Pola AJAX (AJAX Patterns)

| Pola | Penjelasan | Contoh |
|---|---|---|
| **Multi Stage Download** | Halaman besar **tidak dimuat sekaligus, tapi bertahap**. | Bagian utama tampil dulu, komentar dimuat belakangan. |
| **Periodic Refresh (polling)** | AJAX **secara berkala mengecek** apakah ada data baru di server. | Cek notifikasi tiap 30 detik. |
| **Predictive Fetch** | Aplikasi AJAX **menebak apa yang akan dilakukan pengguna berikutnya** dan mengambil datanya duluan. | Saat membaca halaman 1, halaman 2 sudah dimuat diam-diam. |
| **Fallback Pattern** | Jika terjadi error di server (status **selain 200**), AJAX harus **memutuskan apa yang dilakukan**. | Tampilkan pesan "Gagal memuat, coba lagi" atau pakai data cache. |
| **Submission Throttling** | AJAX **mengirim data besar dalam beberapa tahap**. | Autosave dokumen tiap beberapa detik, bukan setiap ketukan tombol. |

---

### ✅ Cek Pemahaman Bab 6

1. Apakah AJAX bahasa pemrograman? Terdiri dari apa saja?
2. Apa arti `readyState == 4` dan `status == 200`?
3. Apa itu same-origin policy? Bagaimana cara mengizinkan akses lintas origin?
4. Mengapa XHR sinkron tidak disarankan?
5. Apa kelebihan Fetch dibanding XMLHttpRequest?
6. Apakah `fetch` bagian dari ECMAScript?

<details>
<summary>Lihat jawaban</summary>

1. Bukan. Teknik yang menggabungkan XMLHttpRequest (meminta data) + JavaScript & HTML DOM (menampilkan data).
2. 4 = request selesai & response siap; 200 = OK (sukses).
3. Browser hanya mengizinkan request ke origin yang sama (skema + domain + port sama). Diizinkan lewat CORS, misal server mengirim header `Access-Control-Allow-Origin`.
4. Karena JS berhenti sampai response datang → halaman membeku.
5. Berbasis Promise (bisa async/await), lebih sederhana, lebih fleksibel.
6. Bukan. Bagian dari Web Platform API (WHATWG/W3C).
</details>

---
## Bab 7 – Server-Side Scripting: Go & PHP (04a-Server-Side-Scripting)

**Server-side scripting** = program yang **berjalan di server**, bukan di browser. Tugasnya: menerima request, menjalankan logika (cek login, ambil data dari database), lalu **membuat response** (HTML atau JSON) untuk dikirim ke browser.

💡 **Analogi:** Kalau JavaScript di browser itu "pelayan di meja", server-side script adalah **koki di dapur** – pengunjung tidak melihatnya, tapi dialah yang memasak hidangan sesuai pesanan.

---

### 7.1 Proses Umum Web App (Pengulangan)

Slide pembuka menampilkan lagi diagram **Typical Web Application Processing** (Shklar, Figure 8.3) – lihat penjelasan lengkapnya di **Bab 2.13**. Intinya, server-side scripting bertugas di bagian **bawah garis abu-abu**: *enabling data access → accessing/modifying content → customizing content for presentation*.

### 7.2 Web Stack (Pengulangan + Contoh)

**Web stack** = kumpulan software yang dibutuhkan untuk membangun dan menjalankan web app; berisi semua lapisan: **OS, Web Server, Bahasa Pemrograman (& Markup) frontend & backend, Web Framework frontend & backend, Database Server**. Contoh: **LAMP stack, MEAN stack, JAMstack**, …

#### Stack-stack populer

| Stack | Komponen |
|---|---|
| **MEAN / MERN / MEVN** | **M**ongoDB (database), **E**xpressJS (framework backend), **A**ngular / **R**eact / **V**ue.js (framework frontend), **N**odeJS (runtime JS di server) |
| **WINS** | **W**indows Server, **I**IS (web server), .**N**ET (framework), Microsoft **S**QL Server |
| **LAMP** | **L**inux, **A**pache, **M**ySQL/MariaDB, **P**HP |
| **XAMPP** | **X** (cross-platform: bisa di OS apa saja), **A**pache, **M**ySQL/MariaDB, **P**HP & **P**erl |
| **GoTTH** | **Go**lang (ServeMux), **T**ailwind (CSS), **T**urso LibSQL (database), **H**TMX + Templ (template engine) |
| **JAMstack** | **J**avaScript, **A**PIs, **M**arkup (situs statis + API) |

### 7.3 Bahasa Server-Side Scripting & Framework-nya

| Bahasa | Framework populer |
|---|---|
| **PHP** | Laravel, Zend, CodeIgniter |
| **JavaScript/TypeScript** | Express, Nest, Hapi |
| **Python** | Flask, Django |
| **Ruby** | Rails |
| **Java** | Spring |
| **C#** | ASP.NET |
| **Scala** | Play |
| **Go** | (standard library `net/http`, Gin, Echo, Fiber – lihat Bab 8) |

### 7.4 Empat Aspek Server-Side

1. **Routing** – URL mana diarahkan ke fungsi mana.
2. **Templating** – membuat HTML dari template + data.
3. **Form Processing** – memproses data yang dikirim form.
4. **Data Access** – mengakses database/file (dibahas detail di Bab 9).

### 7.5 Menafsirkan & Memproses Request (Pengulangan)

Browser mengirim request lewat HTTP; info yang dikirim disebut **request context**: URL, query string, request headers, request body, session information. (Detail dan Tabel 8.1 CGI/Auxiliary/J2EE ada di **Bab 2.15**.)

---

### 7.6 Routing

**Routing** = **memetakan URL tertentu ke fungsi yang terkait**.

💡 **Analogi:** Routing seperti **resepsionis gedung**: "Mau ke bagian keuangan? Lantai 3. Mau ke HRD? Lantai 5." URL = tujuan, fungsi = ruangan.

**Sintaks URI:**
```
scheme:[//authority]path[?query][#fragment]
authority = [userinfo@]host[:port]
```
Contoh:
- `https://website.org/one/certain/path.html`
- `https://myweb.app/view`

Perbedaan:
- **Tanpa routing** (web server statis biasa): URL langsung dipetakan ke **file HTML** di folder. `/one/certain/path.html` → file `one/certain/path.html`.
- **Dengan web application**: URL dipetakan ke **fungsi**, yang bisa mengembalikan **HTML, atau tipe lain** (JSON, gambar, file download, redirect…). `/view` tidak harus ada file bernama "view".

#### Contoh routing di Go
```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    // Daftarkan handler untuk route root "/"
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        // 1. Beri tahu browser bahwa output adalah halaman HTML
        w.Header().Set("Content-Type", "text/html; charset=utf-8")
        w.WriteHeader(http.StatusOK)                     // status 200

        // 2. Definisikan struktur HTML langsung di dalam string memakai backtick (``)
        htmlContent := `
        <!DOCTYPE html>
        <html>
        <head>
            <title>Simple Go Server</title>
            <style>
                body { font-family: sans-serif; text-align: center; margin-top: 10%; }
                h1 { color: #007d9c; }
            </style>
        </head>
        <body>
            <h1>Hello from Go!</h1>
            <p>This simple HTML document is served directly from memory.</p>
        </body>
        </html>
        `

        // 3. Tulis string HTML langsung ke response
        fmt.Fprint(w, htmlContent)
    })

    // Jalankan server di port 8080
    http.ListenAndServe(":8080", nil)
}
```
Penjelasan istilah Go:
- `http.HandleFunc(pola, fungsi)` → mendaftarkan route.
- Handler selalu punya 2 parameter: **`w http.ResponseWriter`** (untuk **menulis response**) dan **`r *http.Request`** (berisi **request** yang masuk).
- `w.Header().Set(...)` → set header response. `w.WriteHeader(...)` → set status code.
- `fmt.Fprint(w, ...)` → tulis teks ke response.
- `http.ListenAndServe(":8080", nil)` → server mendengarkan di port 8080. Buka `http://localhost:8080` di browser.

---

### 7.7 Template

- **Template** = file yang berisi **data statis** dan **"placeholder" (tempat kosong) untuk data dinamis**.
- Template **dirender dengan data tertentu** untuk menghasilkan **dokumen akhir**.

💡 **Analogi:** Template = **formulir surat undangan** dengan titik-titik "Kepada Yth. ........". Rendering = mengisi titik-titik itu dengan nama yang berbeda-beda.

#### `app.go` (bagian Template)
```go
package main

import (
    "html/template"
    "net/http"
)

type App struct {
    templates *template.Template      // menyimpan semua template yang sudah di-parse
}

func NewApp() *App {
    templates := template.Must(          // Must: kalau gagal parse, langsung panic (crash) saat start
        template.ParseGlob("templates/*.html"),   // baca SEMUA file .html di folder templates/
    )

    return &App{
        templates: templates,
    }
}

func (app *App) home(w http.ResponseWriter, r *http.Request) {
    data := map[string]any{              // data yang akan "dituangkan" ke template
        "Title":   "Home",
        "Heading": "Go Templating",
        "Message": "Server-side HTML rendering with Go templates.",
    }

    // render template bernama "base" dengan data di atas
    if err := app.templates.ExecuteTemplate(w, "base", data); err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)   // 500 jika gagal
    }
}
```

#### `templates/base.html`
```html
{{ define "base" }}
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta
        name="viewport"
        content="width=device-width, initial-scale=1"
    >

    <title>{{ .Title }}</title>          <!-- diisi data["Title"] → "Home" -->

    <style>
        body {
            font-family: sans-serif;
            max-width: 720px;
            margin: 4rem auto;
            padding: 0 1rem;
        }

        nav {
            margin-bottom: 2rem;
        }
    </style>
</head>

<body>
    <nav>
        <a href="/">Home</a>
    </nav>

    {{ template "content" . }}           <!-- sisipkan template "content", oper data (.) -->
</body>
</html>
{{ end }}
```

#### `templates/home.html`
```html
{{ define "content" }}
<main>
    <h1>{{ .Heading }}</h1>              <!-- "Go Templating" -->
    <p>{{ .Message }}</p>                <!-- "Server-side HTML rendering with Go templates." -->
</main>
{{ end }}
```

**Sintaks template Go:**
| Sintaks | Arti |
|---|---|
| `{{ define "nama" }} ... {{ end }}` | Mendefinisikan template bernama "nama" |
| `{{ .Field }}` | Menampilkan nilai field dari data (titik `.` = data saat ini) |
| `{{ template "nama" . }}` | Menyisipkan template lain dan mengoper data |
| `{{ if .X }} ... {{ end }}` | Kondisi |

🎯 **Pola "layout":** `base.html` = **kerangka umum** (head, navbar) yang dipakai semua halaman; `home.html` = **isi khusus** halaman. Jadi tidak perlu menulis ulang `<head>` dan navbar di setiap halaman.

⚠️ **Keamanan:** `html/template` **otomatis meng-escape** data. Kalau `.Message` berisi `<script>alert(1)</script>`, yang tampil adalah teksnya, bukan script yang dijalankan → mencegah serangan **XSS**.

---

### 7.8 Form Processing

- Sudah jadi praktik umum bahwa **request form bertipe POST diproses oleh program server-side**.
- **Nilai-nilai dari form diteruskan ke fungsi** untuk diproses lebih lanjut.

Contoh di slide terdiri dari 4 file:

#### `main.go`
```go
package main

import (
    "html/template"
    "log"
    "net/http"
)

func main() {
    app := NewApp()

    mux := http.NewServeMux()                       // router

    mux.HandleFunc("GET /", app.home)               // GET  /       → halaman home
    mux.HandleFunc("GET /login", app.loginPage)     // GET  /login  → tampilkan form login
    mux.HandleFunc("POST /login", app.loginSubmit)  // POST /login  → proses form login

    log.Println("running on http://localhost:8080")
    log.Fatal(http.ListenAndServe(":8080", mux))
}
```
- `http.NewServeMux()` = membuat **router** (ServeMux = *HTTP request multiplexer*).
- Pola `"GET /login"` (method + path) didukung sejak **Go 1.22**. Perhatikan: **URL sama (`/login`) tapi method beda → fungsi beda.**
- ⚠️ Catatan kecil: di slide, `main.go` meng-import `"html/template"` padahal tidak dipakai di file ini – di Go, import yang tidak dipakai menyebabkan **error kompilasi**. Hapus import itu jika kamu mencoba kodenya.

#### `login_page.go` – menampilkan form
```go
package main

import (
    "net/http"
)

type LoginData struct {
    Username string
    Error    string
}

func (app *App) loginPage(w http.ResponseWriter, r *http.Request) {
    data := LoginData{}                               // data kosong (belum ada error)

    if err := app.templates.ExecuteTemplate(w, "login", data); err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
    }
}
```

#### `login_submit.go` – memproses form
```go
package main

import (
    "net/http"
)

type LoginData struct {
    Username string
    Error    string
}

func (app *App) loginSubmit(w http.ResponseWriter, r *http.Request) {
    username := r.FormValue("username")     // ambil field "username" dari form
    password := r.FormValue("password")     // ambil field "password" dari form

    // Demo only. Don't do this in real life >:D
    if username == "admin" && password == "secret" {
        http.Redirect(w, r, "/", http.StatusSeeOther)   // sukses → redirect ke home (303)
        return
    }

    data := LoginData{
        Username: username,                              // isi ulang username supaya user tak perlu ketik lagi
        Error:    "Invalid username or password",
    }

    w.WriteHeader(http.StatusUnauthorized)               // status 401

    if err := app.templates.ExecuteTemplate(w, "login.html", data); err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
    }
}
```

#### `templates/login.html`
```html
{{ define "login" }}
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Login</title>
</head>

<body>
    <h1>Login</h1>

    {{ if .Error }}
        <p style="color: red;">{{ .Error }}</p>       <!-- tampil hanya jika ada error -->
    {{ end }}

    <form method="POST" action="/login">
        <div>
            <label>
                Username:
                <input type="text" name="username" value="{{ .Username }}">
            </label>
        </div>

        <div>
            <label>
                Password:
                <input type="password" name="password">
            </label>
        </div>

        <button type="submit">Login</button>
    </form>
</body>
</html>
{{ end }}
```

**Alur lengkap:**
```
1. User buka  GET /login          → loginPage → render template "login" (form kosong)
2. User isi form, klik Login      → browser kirim POST /login, body: username=...&password=...
3. loginSubmit membaca FormValue
   ├─ benar (admin/secret) → 303 Redirect ke "/" → browser GET / → home
   └─ salah                → 401 + render form lagi dengan pesan merah & username terisi
```

⚠️ **Catatan untuk dipahami (bukan untuk ditiru):**
- Komentar "*Demo only. Don't do this in real life*": password **tidak boleh di-hardcode** dan harus disimpan sebagai **hash** (Bab 9).
- Struct `LoginData` di slide ditulis di **dua file** dalam package yang sama – di Go ini **error "redeclared"**; cukup definisikan sekali.
- `loginSubmit` memanggil template `"login.html"`, sedangkan `loginPage` memanggil `"login"`. Karena isi file `login.html` dibungkus `{{ define "login" }}`, nama yang benar untuk dirender adalah **`"login"`** (memanggil nama `"login.html"` akan menghasilkan halaman kosong atau error *"incomplete or empty template"*, karena isi file di luar blok `define` kosong). Pakai nama yang konsisten.
- Setelah login sukses, seharusnya server **membuat session** supaya user tetap dikenali di request berikutnya – ini topik **Bab 9**.
- Pola **POST → Redirect → GET** (303 See Other) mencegah form terkirim ulang jika user menekan refresh.

---

### 7.9 Pemrograman PHP

#### Apa itu PHP?
- **PHP** = singkatan **rekursif** dari **"PHP: Hypertext Preprocessor"** (huruf P pertama merujuk ke dirinya sendiri).
- Bahasa scripting **open source**, **serbaguna**, dipakai luas, **sangat cocok untuk web development**, dan **bisa disisipkan langsung ke dalam HTML**.
- Bedanya dengan JavaScript client-side: **kode PHP dijalankan di SERVER**, menghasilkan HTML yang **kemudian dikirim ke client**. Browser **tidak pernah melihat kode PHP-nya**, hanya hasilnya.

#### Sejarah versi
| Versi | Catatan |
|---|---|
| **PHP/FI 2.0** | Versi awal, **sudah tidak didukung**. |
| **PHP 3** | Penerus PHP/FI 2.0, **jauh lebih baik**. |
| **PHP 5** | Memakai **Zend Engine 2** – banyak **fitur OOP** tambahan. |
| **PHP 6** | **Eksperimental, tidak pernah dirilis.** |
| **PHP 7** | Peningkatan besar – **2x lebih cepat dari PHP 5**. |
| **PHP 8** | Versi mayor terbaru – punya **JIT compiler**, **manajemen memori lebih baik**, **10–20% lebih cepat dari PHP 7**. |

#### Apa yang bisa dilakukan PHP?
**"Apa saja."** PHP fokus pada **server-side scripting**, jadi bisa melakukan apa pun yang bisa dilakukan program CGI: **mengumpulkan data form**, **membuat konten halaman dinamis**, **mengirim & menerima cookie**. Tapi PHP bisa lebih dari itu (script command line, membuat gambar, PDF, dll).

#### Situs-situs besar yang (pernah) memakai PHP (Tabel 1-1)
| Situs | Deskripsi | URL |
|---|---|---|
| Facebook | Jejaring sosial | http://www.facebook.com |
| Flickr | Berbagi foto | http://www.flickr.com |
| Wikipedia | Ensiklopedia kolaboratif online | http://www.wikipedia.org |
| SugarCRM | Tool manajemen hubungan pelanggan | http://www.sugarcrm.com |
| Dotproject | Tool manajemen proyek | http://www.dotproject.org |
| Drupal | Mesin template pembuat website | http://drupal.org |
| Interspire | Produk newsletter & email marketing | http://www.interspire.com |

### 7.10 Contoh PHP Pertama

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
    "http://www.w3.org/TR/html4/loose.dtd">
<html>
    <head>
        <title>Example</title>
    </head>
    <body>

        <?php
            echo "Hi, I'm a PHP script!";
        ?>

    </body>
</html>
```
- Kode PHP diletakkan di antara **`<?php`** dan **`?>`**.
- `echo` = mencetak teks ke output (ke halaman HTML).

#### `hello.php` – apa yang ditulis vs apa yang diterima browser
**Ditulis di server:**
```php
<html>
 <head>
  <title>PHP Test</title>
 </head>
 <body>
 <?php echo '<p>Hello World</p>'; ?>
 </body>
</html>
```
**Yang diterima browser (view source):**
```html
<html>
 <head>
  <title>PHP Test</title>
 </head>
 <body>
 <p>Hello World</p>
 </body>
</html>
```
🎯 **Kode PHP-nya hilang**, diganti hasilnya. Inilah inti "server-side".

### 7.11 Contoh-Contoh Lain

**Contoh #1 – Mencetak variabel (elemen array)**
```php
<?php
echo $_SERVER['HTTP_USER_AGENT'];
?>
```
Output contoh: `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)`
→ `$_SERVER` adalah array bawaan berisi info server & request; `HTTP_USER_AGENT` = header User-Agent dari browser.

**Contoh #2 – Struktur kontrol dan fungsi**
```php
<?php
if (strpos($_SERVER['HTTP_USER_AGENT'], 'MSIE') !== FALSE) {
    echo 'You are using Internet Explorer.<br />';
}
?>
```
Output contoh: `You are using Internet Explorer.<br />`
→ `strpos(teks, cari)` mencari posisi "MSIE" di dalam User-Agent; mengembalikan `FALSE` jika tidak ketemu. Pakai `!==` (strict) karena posisi `0` juga "falsy".

**Contoh #3 – Mencampur mode HTML dan PHP**
```php
<?php
if (strpos($_SERVER['HTTP_USER_AGENT'], 'MSIE') !== FALSE) {
?>
<h3>strpos() must have returned non-false</h3>
<p>You are using Internet Explorer</p>
<?php
} else {
?>
<h3>strpos() must have returned false</h3>
<p>You are not using Internet Explorer</p>
<?php
}
?>
```
Output contoh:
```html
<h3>strpos() must have returned non-false</h3>
<p>You are using Internet Explorer</p>
```
→ Kita bisa "keluar-masuk" PHP. HTML di antara blok `if` hanya dikirim jika kondisinya terpenuhi.

**Contoh form – #1 Form HTML sederhana**
```html
<form action="action.php" method="post">
 <p>Your name: <input type="text" name="name" /></p>
 <p>Your age: <input type="text" name="age" /></p>
 <p><input type="submit" /></p>
</form>
```
**#2 Menampilkan data dari form (`action.php`)**
```php
Hi <?php echo htmlspecialchars($_POST['name']); ?>.
You are <?php echo (int)$_POST['age']; ?> years old.
```
Output contoh: `Hi Joe. You are 22 years old.`
- `$_POST['name']` = isi field `name` dari form POST.
- **`htmlspecialchars()`** mengubah karakter berbahaya (`<`, `>`, `&`, `"`) menjadi aman → **mencegah XSS** (kalau user mengetik `<script>` sebagai nama).
- **`(int)`** memaksa nilai menjadi integer (kalau user mengetik "abc", hasilnya 0).

### 7.12 Gambaran Umum Bahasa PHP

**Tag script:**
```php
<?php php_statements..   ?>    // bentuk standar (disarankan)
<? php_statements.. ?>          // short tag (bisa dimatikan di konfigurasi, hindari)
```
**Komentar:**
```php
// komentar satu baris
/* komentar
   banyak baris */
# komentar gaya shell
```
**Statement** diakhiri **titik koma `;`**:
```php
$nama = "amir";     // variabel PHP selalu diawali tanda $
```

### 7.13 Tipe Data PHP

| Kategori | Tipe |
|---|---|
| **Scalar** (nilai tunggal) | `boolean`, `integer`, `float` (atau `double`), `string` |
| **Compound** (gabungan) | `array`, `object` |
| **Special** (khusus) | `resource`, `NULL` |
| **Pseudo-types** (hanya untuk dokumentasi) | `mixed`, `number`, `callback` |

**Contoh:**
```php
<?php
$a_bool = TRUE;    // a boolean
$a_str  = "foo";   // a string
$a_str2 = 'foo';   // a string
$an_int = 12;      // an integer

echo gettype($a_bool); // prints out: boolean
echo gettype($a_str);  // prints out: string

// Jika ini integer, tambahkan 4
if (is_int($an_int)) {
    $an_int += 4;
}

// Jika $a_bool adalah string, cetak
// (tidak mencetak apa-apa, karena $a_bool boolean)
if (is_string($a_bool)) {
    echo "String: $a_bool";
}
?>
```
- `gettype()` → nama tipe variabel. `is_int()`, `is_string()` → cek tipe.
- 💡 String dengan **petik dua `"..."`** akan **mengganti variabel di dalamnya** dengan nilainya (`"Halo $nama"` → `Halo amir`). String dengan **petik satu `'...'`** **tidak** (dicetak apa adanya).

### 7.14 Array di PHP

- **Array PHP sebenarnya adalah *ordered map*** (peta terurut).
- **Map** = tipe yang **memasangkan nilai (value) ke kunci (key)**.
- Dioptimasi untuk banyak kegunaan; bisa diperlakukan sebagai: **array, list (vector), hash table** (implementasi map), **dictionary, collection, stack, queue**, dan lainnya.
- Karena nilai array bisa berupa array lain, **pohon (tree) dan array multidimensi** juga mungkin.

💡 **Analogi:** Array PHP = **lemari arsip berlabel**. Labelnya bisa nomor (0, 1, 2) atau nama ("mon", "nama"), dan urutan memasukkan tetap terjaga.

**Contoh (slide 33):**
```php
<?php
$arr = array("foo" => "bar", 12 => true);   // kunci string "foo" dan kunci integer 12

echo $arr["foo"]; // bar
echo $arr[12];    // 1   ← true dicetak sebagai "1"
?>
```
```php
<?php
$arr = array("somearray" => array(6 => 5, 13 => 9, "a" => 42));   // array di dalam array

echo $arr["somearray"][6];    // 5
echo $arr["somearray"][13];   // 9
echo $arr["somearray"]["a"];  // 42
?>
```
```php
<?php
// Array ini sama dengan ...
array(5 => 43, 32, 56, "b" => 12);

// ...array ini
array(5 => 43, 6 => 32, 7 => 56, "b" => 12);
?>
```
→ Jika kunci tidak ditulis, PHP memakai **(kunci integer terbesar sebelumnya + 1)**. Karena ada `5 => 43`, maka 32 dapat kunci 6, dan 56 dapat kunci 7.

**Operasi array:**
```php
// Akses dengan indeks
$buah[0] = "apel";
$buah[1] = "mangga";

// Menambah elemen (otomatis di akhir)
$buah[] = "apel";
$buah[] = "mangga";

// Panjang array
$len = count($buah);

// Array asosiatif (kunci berupa string)
$pengguna["nama"] = "amir";
$pengguna["alamat"] = "ganesha 10";
```

**Inisialisasi:**
```php
$hari = array("senin", "selasa", "rabu", "kamis", "jumat", "sabtu", "minggu");
// indeks 0..6

$hari = array(1 => "senin", "selasa", "rabu", "kamis", "jumat", "sabtu", "minggu");
// indeks 1..7 (karena mulai dari 1)

$days = array("mon" => "monday", "tue" => "tuesday", "wed" => "wednesday",
              "thu" => "thursday", "fri" => "friday",
              "sat" => "saturday", "sun" => "sunday");
// kunci string
```
(Sejak PHP 5.4 boleh juga pakai `[...]` sebagai pengganti `array(...)`.)

**Akses array (1) – multidimensi:**
```php
<?php
$Cities = array(
    "Jawa Barat" => array(
        "Bandung",
        "Cianjur",
        "Cirebon"
    ),
    "Jawa Tengah" => array(
        "Semarang",
        "Magelang"
    )
);
print($Cities["Jawa Barat"][1]);   // Cianjur  (indeks 1 = elemen kedua)
?>
```
**Akses array (2) – loop for:**
```php
<?php
$buah[0] = "apel";
$buah[1] = "mangga";
$buah[2] = "jambu";
$len = count($buah);
for ($i = 0; $i < $len; $i++) {
    echo $buah[$i], "<br>";
}
// apel<br>mangga<br>jambu<br>
?>
```
**Akses array (3) – loop foreach:**
```php
<?php
$days = array("mon" => "monday", "tue" => "tuesday", "wed" => "wednesday",
              "thu" => "thursday", "fri" => "friday", "sat" => "saturday",
              "sun" => "sunday");

foreach ($days as $key => $value) {
    echo "key: ", $key, ", value: ", $value, "<br>";
}
// key: mon, value: monday
// key: tue, value: tuesday  ... dst
?>
```

### 7.15 Fungsi di PHP

| Konsep | Sintaks |
|---|---|
| **Definisi** | `function f($param1, $param2 ..) { statements.. }` |
| **Return value** | `return $val;` |
| **Parameter by reference** (fungsi bisa mengubah variabel aslinya) | `function f(&$param1, &$param2 ..) { statements.. }` |
| **Return by reference** | `function &f($param1, $param2 ..) { .. return $v }` |
| **Akses parameter dinamis** | `func_get_arg($i)`, `func_num_args()` |

#### Scope variabel – keyword `global`
Di PHP, variabel di dalam fungsi **default-nya lokal** – fungsi **tidak bisa melihat variabel global** kecuali memakai `global`. (Kode di slide sedikit berantakan; ini versi yang dimaksud:)
```php
<?php
function assignName() {
    global $name;          // "saya mau pakai $name yang global"
    $name = "Zeev";        // mengubah variabel global
}

$name = "Leon";
assignName();
print($name);              // Zeev
?>
```
Tanpa `global $name;`, fungsi akan membuat variabel lokal baru dan `print($name)` tetap mencetak **Leon**.

⚠️ **Beda dengan JavaScript!** Di JS, fungsi bisa langsung membaca variabel luar (closure). Di PHP, harus eksplisit pakai `global` (atau `use` untuk closure).

#### Static variable & dynamic function call
- **Static variable** – **menyimpan state variabel di dalam fungsi** (nilainya **tidak hilang** di antara pemanggilan).
- **Dynamic function call** – fungsi bisa dipanggil secara dinamis dengan **menyimpan nama fungsi di variabel**, lalu **memanggil variabel itu seperti fungsi**.

#### Contoh fungsi 1 – fungsi yang mencetak
```php
<?php
function printBold($text)
{
    print("<b>$text</b>");
}

print("This Line is not Bold<br>\n");
printBold("This Line is Bold");
print("<br>\n");
print("This Line is not Bold<br>\n");
?>
```
#### Contoh fungsi 2 – fungsi yang mengembalikan nilai
```php
<?php
function makeBold($text)
{
    $text = "<b>$text</b>";
    return($text);
}

print("This Line is not Bold<br>\n");
print(makeBold("This Line is Bold") . "<br>\n");   // titik (.) = operator gabung string di PHP
print("This Line is not Bold<br>\n");
?>
```
#### Contoh fungsi 3 – parameter by reference (`&`)
```php
<?php
function stripCommas(&$text)        // & = yang dikirim adalah "alamat" variabel aslinya
{
    $text = str_replace(",", "", $text);
}

$myNumber = "10,000";

stripCommas($myNumber);
print($myNumber);                   // 10000  ← variabel asli ikut berubah!
?>
```
💡 **By value** = memberi **fotokopi** dokumen (aslinya aman). **By reference** = memberi **dokumen aslinya** (kalau dicoret, aslinya ikut tercoret).

#### Contoh fungsi 4 – static variable (warna baris tabel selang-seling)
```php
<?
function useColor()
{
    static $ColorValue = "#00FF00";    // hanya diinisialisasi SEKALI, nilainya diingat

    if ($ColorValue == "#00FF00") {
        $ColorValue = "#CCFFCC";
    } else {
        $ColorValue = "#00FF00";
    }

    return($ColorValue);
}

print("<table width=\"300\">\n");
for ($count = 0; $count < 10; $count++) {
    $RowColor = useColor();             // bergantian: #CCFFCC, #00FF00, #CCFFCC, ...

    print("<tr>" .
          "<td style=\"background: $RowColor\">" .
          "Row number $count" .
          "</td>" .
          "</tr>\n");
}
print("</table>\n");
?>
```
→ Menghasilkan tabel 10 baris dengan warna latar **bergantian** hijau muda & hijau.

#### Contoh fungsi 5 – dynamic function call
```php
<?php
function write($text)
{
    print($text);
}

function writeBold($text)
{
    print("<b>$text</b>");
}

$myFunction = "write";        // simpan NAMA fungsi di variabel
$myFunction("Hello!");        // = write("Hello!")
print("<br>\n");

$myFunction = "writeBold";
$myFunction("Goodbye!");      // = writeBold("Goodbye!") → tebal
print("<br>\n");
?>
```

### 7.16 Object di PHP

- Sejak **PHP 5**, model objek **ditulis ulang** untuk performa lebih baik dan fitur lebih banyak (perubahan besar dari PHP 4). **PHP 5 punya model objek penuh.**
- Fitur PHP 5: **visibility** (public/private/protected), **class & method abstract dan final**, **magic methods** tambahan (`__construct`, `__toString`, dll), **interface**, **cloning**, dan **type hinting**.
- PHP memperlakukan objek seperti **referensi/handle** – setiap variabel berisi **referensi ke objek**, **bukan salinan seluruh objek**.

```php
<?php
class foo
{
    function do_foo()
    {
        echo "Doing foo.";
    }
}

$bar = new foo;         // buat objek dari class foo
$bar->do_foo();         // panggil method → "Doing foo."   (PHP pakai -> bukan titik)
?>
```

### 7.17 Resource

- **Resource** = variabel khusus yang **memegang referensi ke sumber daya eksternal**.
- Resource **dibuat dan dipakai oleh fungsi-fungsi khusus**.
- Variabel resource memegang **"handler"** ke: **file yang dibuka**, **koneksi database**, **area kanvas gambar**, dan sejenisnya.

**Contoh #1 – `mysql_connect()`:**
```php
<?php
$link = mysql_connect('localhost', 'mysql_user', 'mysql_password');   // $link adalah resource
if (!$link) {
    die('Could not connect: ' . mysql_error());   // die = hentikan script dengan pesan
}
echo 'Connected successfully';
mysql_close($link);
?>
```
⚠️ **Penting:** fungsi `mysql_*` **sudah dihapus sejak PHP 7**. Sekarang pakai **`mysqli`** atau **PDO** (lihat 7.24).

### 7.18 NULL

- Nilai khusus **NULL** merepresentasikan **variabel tanpa nilai**. NULL adalah **satu-satunya nilai** dari tipe NULL.
- Variabel dianggap null jika:
  - **diisi konstanta `NULL`**,
  - **belum pernah diisi nilai apa pun**, atau
  - **sudah di-`unset()`**.

### 7.19 Pseudo-types

Bukan tipe sungguhan, hanya dipakai di **dokumentasi** untuk menjelaskan parameter:
- **`mixed`** – parameter bisa menerima **beberapa tipe** (tapi tidak harus semua).
- **`number`** – parameter bisa **integer atau float**.
- **`callback`** – **fungsi atau method yang bisa dipanggil**. Beberapa fungsi seperti **`call_user_func()`** atau **`usort()`** menerima fungsi buatan user sebagai parameter.
- **`void`** – menandakan **tidak ada nilai yang dikembalikan**.

### 7.20 Type Juggling

- PHP **tidak membutuhkan (dan dulu tidak mendukung) definisi tipe eksplisit** saat deklarasi variabel. **Tipe variabel ditentukan oleh konteks pemakaiannya.**
- Artinya: jika string diisi ke `$var`, `$var` jadi string. Jika kemudian integer diisi ke `$var`, ia jadi integer.

```php
<?php
$foo = "0";                     // $foo is string (ASCII 48)
$foo += 2;                      // $foo is now an integer (2)
$foo = $foo + 1.3;              // $foo is now a float (3.3)
$foo = 5 + "10 Little Piggies"; // $foo is integer (15)
$foo = 5 + "10 Small Pigs";     // $foo is integer (15)
?>
```
(Di PHP 8, string seperti `"10 Little Piggies"` tetap dibaca angka depannya (10), tapi PHP akan memberi **peringatan/warning** karena string itu tidak sepenuhnya angka.)

**Type casting (memaksa tipe):**
```php
<?php
$foo = 10;              // $foo is an integer
$bar = (boolean) $foo;  // $bar is a boolean (true)
?>
```

### 7.21 Variabel di PHP

- **Scope default: lokal.**
- Untuk mengakses variabel global, pakai **keyword `global`**.
- Mendukung **variabel static**.
- Bisa membuat **"variable variables"** (variabel yang namanya diambil dari isi variabel lain):
```php
$a = "hallo";
$$a = "world";      // sama dengan $hallo = "world";
echo $hallo;        // world
```

### 7.22 Predefined Variables (Variabel Bawaan / Superglobal)

| Variabel | Isi |
|---|---|
| `$_REQUEST` | Variabel **HTTP request** (gabungan GET, POST, COOKIE) |
| `$_GET` | Variabel dari **HTTP GET** (query string di URL) |
| `$_POST` | Variabel dari **HTTP POST** (body form) |
| `$_FILES` | **File yang di-upload** lewat HTTP |
| `$_SESSION` | Variabel **session** |
| `$_COOKIE` | **Cookie** HTTP |
| `$_ENV` | Variabel **environment** (lingkungan sistem) |
| `$_SERVER` | Variabel **server** (info request, header, path, dll) |

### 7.23 Sumber Input dan Output

**Alternatif sumber INPUT data:**
| Sumber | Cara di PHP |
|---|---|
| Parameter URL | `$_GET`, `$_REQUEST` |
| Form | `$_POST`, `$_REQUEST`, `$_FILES` |
| Cookie | `$_COOKIE` |
| Session | `session_start()`, `$_SESSION` |
| File | `fopen()`, `fread()`, `fclose()`, dll |
| Database | connect, select_db, query, fetch |

**Alternatif OUTPUT:**
| Output | Cara di PHP |
|---|---|
| HTML | `echo` |
| Gambar | `imagejpeg()`, `imagegif()`, `imagepng()` |
| File | `fopen()`, `fwrite()`, `fclose()` |
| Cookie | `setcookie()` |
| Session | `session_start()`, `$_SESSION` |
| Database | connect, select_db, query |
| **Data JSON atau XML (untuk API)** | `echo json_encode($data);` |

#### Input dari parameter URL
- **Dipakai untuk menandai link mana yang diklik user.**
- **Setiap link merepresentasikan data/aksi.**

HTML:
```html
<html>
1. Jaket  <a href='go.php?id=1&act=edit'>Edit</a>
          <a href='go.php?id=1&act=delete'>Delete</a><br>
2. Sepatu <a href='go.php?id=2&act=edit'>Edit</a>
          <a href='go.php?id=2&act=delete'>Delete</a>
</html>
```
Tampil di browser: "1. Jaket Edit Delete / 2. Sepatu Edit Delete".

PHP `go.php`:
```php
<?
$aksi = $_GET["act"];
$id = $_GET["id"];
if ($aksi == "edit") {
    // lakukan edit terhadap data dengan ID = $id
}
else if ($aksi == "Delete") {
    // lakukan delete terhadap data dengan ID = $id
}
?>
```
⚠️ Perhatikan: di slide tertulis `"Delete"` (D besar) padahal link mengirim `act=delete` (d kecil) – perbandingan string di PHP **case-sensitive**, jadi cabang delete tidak akan jalan. Selain itu, **aksi yang mengubah data (delete) seharusnya tidak lewat GET** (ingat Bab 3: GET harus *safe*) – lebih baik pakai form POST.

#### Input dari form HTML
HTML:
```html
<html>
 <form action='save.php' method='POST'>
   Nama<br>
     <input type='text' name='nama'><br>
   Jenis<br>
     <input type='radio' name='jenis' value='L'>Laki-laki<br>
     <input type='radio' name='jenis' value='P'>Perempuan<br>
   <input type='submit' value='Simpan'>
 </form>
</html>
```
PHP `save.php`:
```php
<?
$nama = $_POST["nama"];    // berisi string nama
$jenis = $_POST["jenis"];  // berisi "L" atau "P"

// simpan data $nama dan $jenis
?>
```

#### Input dari Cookie
- **Dapat digunakan untuk mendapatkan data yang dimasukkan user pada halaman sebelumnya.**

`login.html`:
```html
<html>
 <form action='login.php' method='POST'>
   User <input type='text' name='user'><br>
   Password <input type='text' name='pass'><br>
   <input type='submit' value='Login'>
 </form>
</html>
```
`login.php`:
```php
<?
$user = $_POST["user"];   // berisi string username
$pass = $_POST["pass"];   // berisi string password

if (UserDanPasswordOK($user, $pass)) {
    // simpan $user di cookie
    setcookie("login", $user);
}
?>
```
`anypage.php`:
```php
<?
$user = $_COOKIE["login"];        // berisi string username
if ($user == "") {                // belum melakukan login
    header("Location: login.html");   // redirect ke halaman login
}
else {
    // User sudah login, boleh melakukan sesuatu
}
?>
```
⚠️ **Masalah keamanan besar:** cookie disimpan di browser, jadi **user bisa mengedit cookie `login` menjadi nama siapa saja** (misal "admin") lewat DevTools! Cookie berisi username polos **tidak boleh dipercaya** untuk autentikasi. Solusinya: session (di bawah) atau token bertanda tangan (Bab 9). Juga, input password sebaiknya `type='password'`.

#### Input dari Session
`login.php`:
```php
<?
$user = $_POST["user"];   // berisi string username
$pass = $_POST["pass"];   // berisi string password

if (UserDanPasswordOK($user, $pass)) {
    // simpan $user di session
    session_start();
    $_SESSION["login"] = $user;
}
?>
```
`anypage.php`:
```php
<?
session_start();
$user = $_SESSION["login"];       // berisi string username
if ($user == "") {                // belum melakukan login
    header("Location: login.html");   // redirect ke halaman login
}
else {
    // User sudah login, boleh melakukan sesuatu
}
?>
```
🎯 **Beda cookie vs session:** dengan session, **data (`login = budi`) disimpan di SERVER**. Browser hanya memegang **ID session acak** (PHP otomatis mengirim cookie `PHPSESSID`). User tidak bisa mengubah isi session, hanya bisa mengirim ID – jauh lebih aman.

#### Akses File
Alur: **`fopen` → `fread`/`fwrite` (berulang) → `fclose`**
```php
<?
$namafile = "log.txt";

// Contoh menulis ke file
$fw = fopen($namafile, "w");   // buka untuk ditulis ("w" = write, menimpa isi lama)
fwrite($fw, "2006-02-12 User Budi melakukan login\n");
fwrite($fw, "2006-02-15 User Toni melakukan login\n");
fwrite($fw, "2006-02-17 User Budi menambah data\n");
fclose($fw);

// Contoh membaca file
$fr = fopen($namafile, "r");   // buka untuk dibaca ("r" = read)
while ($line = fread($fr)) {
    echo $line;
}
fclose($fr);

// Contoh membaca isi file dan memasukkan isinya ke sebuah variabel
$isifile = file_get_contents($namafile);
?>
```
(Catatan: di PHP asli, `fread` butuh parameter panjang, misalnya `fread($fr, 1024)`; untuk membaca per baris biasanya pakai `fgets($fr)`.)

Mode `fopen` lain: `"a"` (append – tambah di akhir), `"r+"` (baca & tulis).

#### Akses Database
Alur: **connect → select_db → (query → fetch rows, berulang) → close**
```php
$server = "167.205.1.2";    // database server
$userid = "tedi";
$password = "asdf";
$basisdata = "mhs";
$link = mysql_connect($server, $userid, $password);

mysql_select_db($basisdata, $link);

// contoh menyimpan data
$query = "insert into t_mahasiswa values('135', 'Budi', 'L')";
mysql_query($query);

// contoh membaca data
$query = "select nim, nama, jenis from t_mahasiswa";
$result = mysql_query($query, $link);
while ($row = mysql_fetch_array($result)) {
    echo $row["nama"] . "<br>";
}

mysql_close($link);
```

### 7.24 ⚠️ Versi Modern Akses Database (PDO + Prepared Statement)

Karena `mysql_*` sudah dihapus dan rawan **SQL injection** (jika query dibuat dengan menyambung string input user), versi yang benar sekarang:
```php
<?php
$pdo = new PDO("mysql:host=167.205.1.2;dbname=mhs", "tedi", "asdf");

// menyimpan data – pakai placeholder ?, BUKAN menyambung string
$stmt = $pdo->prepare("INSERT INTO t_mahasiswa VALUES (?, ?, ?)");
$stmt->execute(['135', 'Budi', 'L']);

// membaca data
$stmt = $pdo->query("SELECT nim, nama, jenis FROM t_mahasiswa");
foreach ($stmt as $row) {
    echo htmlspecialchars($row["nama"]) . "<br>";
}
?>
```
(Konsep *prepared statement* dibahas lagi di Bab 9 dengan Go.)

### 7.25 Membuat Gambar dengan PHP
```php
<?
// HTTP header yang menyatakan bahwa output mempunyai format GIF
header("Content-type: image/gif");
// membuat image baru di memory
$im = imagecreate(100, 100);   // width, height
// definisi warna pertama untuk background
$backgroundcolor = imagecolorallocate($im, 255, 0, 255);   // purple (magenta)
// contoh definisi warna lainnya sesuai kebutuhan
$redcolor    = imagecolorallocate($im, 255, 0, 0);     // red
$greencolor  = imagecolorallocate($im, 0, 200, 0);     // green
$bluecolor   = imagecolorallocate($im, 0, 0, 255);     // blue
$yellowcolor = imagecolorallocate($im, 255, 255, 0);   // yellow
// contoh menggambar persegi panjang
imagefilledrectangle($im, 5, 5, 80, 25, $redcolor);    // x1, y1, x2, y2, color
// contoh menggambar lingkaran
imagefilledellipse($im, 60, 40, 50, 50, $greencolor);  // xcenter, ycenter, width, height
// contoh menggambar poligon
$points = array(30, 10, 60, 60, 30, 50, 10, 70);       // x1,y1, x2,y2, x3,y3, x4,y4
imagefilledpolygon($im, $points, 4, $bluecolor);       // arraypoints, numpoints, color
// contoh menggambar teks
imagestring($im, 5, 8, 8, "Contoh", $yellowcolor);     // fontsize, x, y, color
// outputkan ke browser
imagegif($im);
// hapus dari memory
imagedestroy($im);
?>
```
Hasil di browser: gambar 100×100 berlatar ungu/magenta, ada persegi merah, lingkaran hijau, segitiga/poligon biru, dan tulisan kuning "Contoh".
🎯 **Pelajaran:** response HTTP tidak harus HTML – dengan header `Content-type: image/gif`, PHP bisa mengirim **gambar**.

---

### 7.26 Model Eksekusi PHP

#### a) Apache HTTPD
```
 ═══►  ┌──┐┌──┐┌──┐┌──┐
 ═══►  │ ⟳││ ⟳││ ⟳││ ⟳│
 ═══►  ├──┤├──┤├──┤├──┤        ───►  [CPU]
 ═══►  │ ⟳││ ⟳││ ⟳││ ⟳│
 ═══►  └──┘└──┘└──┘└──┘
Hundreds of concurrent    require hundreds of heavyweight     competing for limited
connections...            threads or processes...             CPU and memory
```
- **Pendekatan berbasis proses (process-driven).**
- **Membuat thread/proses baru untuk setiap request.**
- Masalah: ratusan koneksi bersamaan → butuh ratusan thread/proses yang **berat** → **berebut CPU dan memori** yang terbatas.

💡 Seperti restoran yang **merekrut satu pelayan baru untuk setiap tamu** – kalau tamunya 500, butuh 500 pelayan, dapur jadi sesak.

#### b) NGINX
```
            ┌──────── Master ────────┐     (Load configuration, Launch workers, Non-stop upgrade)
            ▼            ▼           ▼
 Users ⇄ [Worker]    [Worker]    [Worker] ──HTTP──────► Web server        ┐
  HTTP/   (ht_core,   ...          ...    ──FastCGI───► Application server ├ Backend
  HTTPS   ht_upstream,                    ──memcache──► Memcached         ┘
          ht_proxy,
          ht_fastcgi)
             │ Multiplexing via kevent/epoll/select
             ▼
        [ proxy cache ] ◄── Cache loader, Cache manager
        (Advanced I/O: sendfile, AIO, mmap, etc.)
```
- **Event-driven, Asynchronous, Non-blocking.**
- Sedikit **worker process** (biasanya satu per core CPU), masing-masing bisa menangani **ribuan koneksi** sekaligus memakai multiplexing (`epoll`/`kqueue`/`select`).
- Nginx sendiri **tidak menjalankan PHP**; ia meneruskan request PHP ke **application server** lewat **FastCGI** (biasanya **PHP-FPM**).
- Ada **proxy cache** dengan *cache loader* dan *cache manager*.
- **Master process**: memuat konfigurasi, meluncurkan worker, upgrade tanpa downtime.

💡 Seperti restoran dengan **sedikit pelayan super-sigap**: setiap pelayan mencatat pesanan banyak meja, menyerahkan ke dapur, lalu langsung melayani meja lain sambil menunggu masakan matang (tidak berdiri menunggu).

### 7.27 Tambahan di PHP 7
- **Kecepatan:** benchmark PHP 7 konsisten menunjukkan **2x lebih cepat dari PHP 5.6**, bahkan lebih.
- **Strict typing opsional** (`declare(strict_types=1);`).
- **Penanganan error/exception** yang lebih baik (banyak error fatal kini bisa ditangkap sebagai exception `Error`).
- **Operator baru:**
  - **"Spaceship" operator `<=>`** – membandingkan dua nilai:
    ```php
    $compare = 2 <=> 1;
    // 2 < 1 ? return -1
    // 2 = 1 ? return 0
    // 2 > 1 ? return 1      → $compare = 1
    ```
    (Berguna untuk fungsi pengurutan `usort`.)
  - **Null coalescing operator `??`**:
    ```php
    $name = $firstName ?? "Guest";   // pakai $firstName jika ada & tidak null; kalau tidak, "Guest"
    ```

### 7.28 PHP 8
PHP 8 kembali meningkatkan performa (tidak sedrastis lompatan PHP 5 → 7, tapi terus naik di setiap rilis). Poin-poin di slide:
1. **New Language Syntax** – sintaks baru, contoh: *named arguments* `foo(nama: "x")`, ekspresi `match`, *constructor property promotion*.
2. **Excellent Load Bearing Capacity** – daya tahan beban tinggi (dibantu **JIT compiler**).
3. **Help with Null** – misalnya **nullsafe operator `?->`**: `$user?->getAddress()?->city` (tidak error jika ada yang null).
4. **Return Types** – deklarasi tipe kembalian lebih kaya (union types `int|string`, `mixed`, `static`).
5. **Asynchronous Programming Support** – dukungan pemrograman asinkron (misalnya **Fibers** di PHP 8.1).

---

### ✅ Cek Pemahaman Bab 7

1. Sebutkan 4 aspek server-side!
2. Apa kepanjangan komponen stack LAMP dan MERN?
3. Apa fungsi `{{ .Title }}` dan `{{ template "content" . }}` di template Go?
4. Apa beda `$_GET`, `$_POST`, `$_SESSION`, `$_COOKIE`?
5. Mengapa menyimpan username di cookie untuk menandai login itu berbahaya?
6. Apa beda parameter by value dan by reference di PHP?
7. Apa beda model eksekusi Apache (klasik) dan Nginx?
8. Apa hasil `5 <=> 3` dan `null ?? "x"`?

<details>
<summary>Lihat jawaban</summary>

1. Routing, Templating, Form Processing, Data Access.
2. LAMP: Linux, Apache, MySQL/MariaDB, PHP. MERN: MongoDB, Express, React, Node.js.
3. `{{ .Title }}` menampilkan field Title dari data; `{{ template "content" . }}` menyisipkan template bernama "content" dan mengoper data.
4. `$_GET`: data dari query string URL; `$_POST`: data dari body form POST; `$_SESSION`: data session yang disimpan di server; `$_COOKIE`: cookie yang dikirim browser.
5. Cookie ada di browser dan bisa diubah user, jadi user bisa mengaku sebagai orang lain.
6. By value: fungsi menerima salinan, variabel asli tidak berubah. By reference (`&`): fungsi menerima variabel aslinya, perubahan ikut terjadi pada variabel asli.
7. Apache klasik: process/thread per request (berat). Nginx: event-driven, asynchronous, non-blocking, sedikit worker melayani banyak koneksi.
8. `1` dan `"x"`.
</details>

---
## Bab 8 – Go untuk Web Development (04b-Go-for-Web)

**Go** (sering disebut **Golang**) adalah bahasa yang dipakai di banyak contoh kuliah ini (Bab 7 dan Bab 9). Bab ini menjelaskan **kenapa Go**, kelebihan-kekurangannya, dan model eksekusinya.

### 8.1 Kenapa Go?

**Kelebihan:**
| Kelebihan | Penjelasan |
|---|---|
| **Compiled** → cepat, **deploy cukup satu file binary** | Kode Go di-compile jadi satu file program. Di server tinggal copy satu file itu, tidak perlu install runtime/interpreter. |
| **Statically typed** → bug tertangkap saat compile | Tipe data dicek sebelum program jalan (mirip TypeScript, Bab 5). |
| **Goroutines** → menangani **ribuan request bersamaan dengan murah** | "Thread ringan" bawaan Go. |
| **Standard library sangat baik** → `net/http` sudah **production-grade** | Tanpa framework pun sudah bisa membuat server serius. |
| **Sederhana** → bahasanya kecil, mudah dipelajari | |

**Trade-off (kekurangan):**
- **Ekosistem lebih kecil** dibanding Node/Python.
- **Kurang "batteries-included"** dibanding Django/Rails (banyak hal harus dirakit sendiri).
- **Verbose (bertele-tele)** – terutama penanganan error: `if err != nil { ... }` di mana-mana.

### 8.2 Kisah Asal-Usul

Go dibuat di **Google tahun 2007** oleh **Robert Griesemer, Rob Pike, dan Ken Thompson** (salah satu pencipta **Unix**).

**Masalah yang ingin mereka selesaikan:**
- Sistem terdistribusi **berskala besar** di Google.
- **Kompilasi lambat** (C++ butuh **menit-an**).
- **Konkurensi sulit** (thread, lock, *race condition*).
- **Dependensi jadi mimpi buruk.**

**Tujuan desain Go:**
1. **Kompilasi cepat** (hitungan detik, bukan menit).
2. **Sintaks sederhana** (mudah dipelajari, mudah dibaca).
3. **Konkurensi bawaan** (*goroutines*, *channels*).
4. **Static typing** (bug tertangkap saat compile).
5. **Eksekusi cepat** (di-compile ke *machine code*).

### 8.3 Filosofi Go: Kesederhanaan

- Go hanya punya **~25 keyword** (bandingkan Java **~50**, C++ **~90**).
- **"Satu cara untuk melakukan sesuatu":**
```go
// Go: satu cara menangani error
if err != nil {
    return err
}
```
```python
# Python: banyak cara
try:
    ...
except:
    ...

# atau
if ...:
    ...
else:
    ...
```
**Manfaat:**
- **Mudah dipelajari** (bisa dipelajari dalam akhir pekan).
- **Kode mudah dibaca** (tidak ada trik "pintar" yang membingungkan).
- **Gaya konsisten** (`gofmt` memaksa format kode yang sama untuk semua orang).
- **Onboarding cepat** (anggota tim baru cepat produktif).

### 8.4 Kekuatan Go untuk Web

| 1. Performa | 2. Konkurensi |
|---|---|
| Di-compile ke **machine code** (bukan diinterpretasi). | **Goroutines:** thread ringan (stack awal **~2KB**). |
| **Deploy satu binary** (tanpa dependensi runtime). | Bisa menjalankan **100.000 goroutine di laptop**. |
| **Startup ~10ms** (vs Node **~100ms**, Java **~1s**). | **Sempurna untuk menangani banyak request bersamaan.** |

| 3. Standard Library | 4. Type Safety |
|---|---|
| `net/http` **production-grade**. | **Bug tertangkap saat compile.** |
| **Tidak butuh framework** (walau framework membantu). | **Autocomplete IDE** bekerja sangat baik. |
| **Batteries included** untuk JSON, template, kriptografi, dll. | **Refactoring aman.** |

### 8.5 Trade-Off Go untuk Web

**1. Error handling yang verbose:**
```go
// Go: pengecekan error eksplisit
result, err := doSomething()
if err != nil {
    return err
}
// ... pakai result ...
```
```python
# Python: exceptions
try:
    result = do_something()
except Exception as e:
    return e
```
💡 Go sengaja membuat error **terlihat jelas** (tidak "terbang" diam-diam seperti exception). Konsekuensinya: kodenya lebih panjang.

**2. Ekosistem lebih kecil:**
- **Library lebih sedikit** dibanding Node/Python.
- Beberapa web framework **kurang matang**.
- **Pilihan ORM terbatas** (**GORM** yang utama).

**3. Kurang "batteries-included":**
- **Tidak ada ORM bawaan** (pakai `database/sql` atau GORM).
- **Tidak ada library validasi bawaan.**
- **Tidak ada form handling bawaan** (parse manual).

### 8.6 Adopsi di Dunia Nyata

| Perusahaan | Pemakaian |
|---|---|
| **Google** | Infrastruktur Search, backend YouTube |
| **Uber** | Layanan geolokasi, update real-time |
| **Twitch** | Sistem chat, live streaming |
| **Dropbox** | Layanan penyimpanan file |
| **Cloudflare** | CDN, edge computing |
| **American Express** | Pemrosesan pembayaran |
| **Mercado Libre** | Platform e-commerce (Amerika Latin) |

### 8.7 Framework Web Go Populer
- **Standard library (`net/http`)** — **pilihan paling populer**.
- **Gin** — cepat, minimal, mirip Express.
- **Echo** — ringan, cepat.
- **Fiber** — terinspirasi Express, sangat cepat.

### 8.8 Go vs Bahasa Lain untuk Web

| Bahasa | Kelebihan | Kekurangan |
|---|---|---|
| **Go** | Cepat, aman (type-safe), konkuren | Verbose, ekosistem lebih kecil |
| **Node.js** | Ekosistem sangat besar, asinkron | Single-threaded, type safety (tanpa TS) |
| **Python** | Mudah, banyak library | Lambat, **GIL** membatasi konkurensi |
| **Java** | Enterprise, matang | Startup lambat, verbose |
| **Rust** | Paling cepat, paling aman | Kurva belajar curam |

(GIL = *Global Interpreter Lock*: di CPython, hanya satu thread yang bisa menjalankan kode Python pada satu waktu.)

#### Kapan memilih Go?
- **Layanan dengan konkurensi tinggi** (chat, real-time).
- **Microservices** (startup cepat, binary kecil).
- **API yang kritis performa.**
- **Tim yang menghargai kesederhanaan dan type safety.**

### 8.9 Model Eksekusi Go

**Cara kerjanya:**
- **Setiap request memunculkan (spawn) satu goroutine** (thread ringan).
- **Goroutine-goroutine di-*multiplex* ke thread OS** (banyak goroutine berbagi sedikit thread OS).
- **Jika satu goroutine terblokir (misal menunggu I/O/database), goroutine lain tetap jalan.**
- **Jutaan goroutine dimungkinkan.**

```
                 Incoming Request
                        │
                   Main Thread
          ┌─────────────┼──────────────┐
  Spawn Goroutine 1  Spawn Goroutine 2  Spawn Goroutine N
          │             │              │
  Handle Request 1  Handle Request 2  Handle Request N
          │             │              │
      Response 1    Response 2     Response N
```

### 8.10 Perbandingan Model Eksekusi Server

| Model | Cara kerja | Contoh |
|---|---|---|
| **Process-per-request** | **Spawn satu proses** untuk setiap request | PHP klasik (Apache `mod_cgi`) |
| **Thread-per-request** | **Satu thread** per request | Java (Tomcat), Python (WSGI) |
| **Event loop** | **Satu thread**, I/O asinkron | Node.js, nginx |
| **Goroutines (M:N)** | **Thread ringan di-multiplex ke thread OS** (M goroutine dipetakan ke N thread OS) | Go |

💡 **Analogi restoran:**
- **Process-per-request** = setiap tamu dibuatkan **dapur baru**. (Sangat boros.)
- **Thread-per-request** = setiap tamu dapat **satu koki khusus**. (Boros kalau tamunya ribuan.)
- **Event loop** = **satu koki super cepat** yang tidak pernah menunggu: sambil air mendidih, ia memotong sayur untuk pesanan lain. (Efisien, tapi kalau ada satu tugas yang lama dikerjakan sendiri, semua tertunda.)
- **Goroutines (M:N)** = **ribuan "kartu tugas"** dibagikan ke **beberapa koki**; kalau satu tugas sedang menunggu oven, koki langsung mengambil kartu lain. (Efisien DAN bisa memakai banyak CPU.)

### 8.11 Web Server Go Minimal

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

func main() {
    // Definisikan route
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprint(w, "Hello, World!")
    })

    http.HandleFunc("/products", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprint(w, "Product list")
    })

    // Jalankan server
    log.Println("Server starting on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```
**"Itu saja. 20 baris. Production-ready. Tanpa framework."**

Cara mencoba: simpan sebagai `main.go`, jalankan `go run main.go`, lalu buka:
- `http://localhost:8080/` → "Hello, World!"
- `http://localhost:8080/products` → "Product list"

Penjelasan:
- `package main` + `func main()` = titik awal program Go.
- `import` = memakai paket standar: `fmt` (format/cetak), `log` (logging), `net/http` (server HTTP).
- `log.Fatal(...)` = jika `ListenAndServe` gagal (misal port sudah dipakai), cetak error lalu hentikan program.
- Setiap request yang masuk otomatis dilayani di **goroutine terpisah** oleh `net/http`.

---

### ✅ Cek Pemahaman Bab 8

1. Sebutkan 3 kelebihan dan 3 kekurangan Go untuk web.
2. Siapa pembuat Go dan masalah apa yang ingin diselesaikan?
3. Apa itu goroutine dan apa artinya model "M:N"?
4. Bandingkan 4 model eksekusi server.

<details>
<summary>Lihat jawaban</summary>

1. Kelebihan: compiled & cepat (single binary), static typing, goroutines (konkurensi murah), stdlib kuat, sederhana. Kekurangan: ekosistem lebih kecil, kurang batteries-included, error handling verbose.
2. Robert Griesemer, Rob Pike, Ken Thompson (Google, 2007) – kompilasi C++ lambat, konkurensi sulit, dependensi rumit, sistem terdistribusi besar.
3. Goroutine = thread ringan (stack ~2KB). M:N = M goroutine dijalankan di atas N thread OS.
4. Process-per-request (PHP klasik), thread-per-request (Tomcat/WSGI), event loop (Node/nginx), goroutines M:N (Go).
</details>

---
## Bab 9 – State Handling & Autentikasi (05-State-Handling-and-Authentication)

Bab ini menjawab dua pertanyaan besar:
1. **Bagaimana aplikasi web "mengingat" sesuatu**, padahal HTTP itu stateless? (*State handling*: URL, form, memori browser, storage, cookie, session, database, file.)
2. **Bagaimana aplikasi tahu siapa penggunanya dan apa yang boleh ia lakukan?** (*Authentication & Authorization*: password, session, Basic/Bearer, JWT, OAuth, OpenID Connect.)

---

### 9.1 Tugas (Task) dalam Aplikasi Web

- Fitur aplikasi memungkinkan pengguna melakukan berbagai **tugas (task)**.
- Satu tugas bisa melibatkan **satu atau beberapa halaman**:
  - **1 halaman:** membaca artikel, mengisi buku tamu.
  - **Banyak halaman:** **belanja online**.

```
┌──────────────┐     ┌────────────────────┐     ┌───────────────────┐
│ Pilih barang:│     │ Masukkan alamat    │     │ Anda membeli:     │
│ - buku       │ ──► │ pengiriman:        │ ──► │ mainan            │
│ - mainan  ✓  │     │ ................   │     │ akan dikirim ke:  │
│ - alat tulis │     │ ................   │     │ Jl. Ganesha       │
│ - pakaian    │     └────────────────────┘     └───────────────────┘
│ - makanan    │
└──────────────┘
```
Halaman ketiga harus **tahu** barang yang dipilih di halaman pertama dan alamat dari halaman kedua. → **Antar-halaman harus ada mekanisme untuk mengelola *state* (keadaan) tugas.**

### 9.2 HTTP itu Stateless

**HTTP stateless: tidak ada state yang dipertahankan antar-request (di level protokol).**
```
 page1.php                  page2.php                  page3.php
 $a = 1;     ──link──►      echo $a;      ──link──►    echo $a;
             <a href=        // nothing!               // nothing!
             'page2.php'>
```
Variabel `$a` yang dibuat di page1 **tidak ada** di page2 dan page3. Setiap request = dunia baru.

### 9.3 State Management

- **Definisi:** proses untuk **mempertahankan state dan informasi halaman melewati beberapa request**, untuk halaman yang sama maupun berbeda.
- State bisa disimpan di **sisi client**, **sisi server**, atau **kombinasi keduanya** (*kebanyakan kasus memakai kombinasi*).

### 9.4 Teknik-Teknik Umum

| Kelompok | Teknik |
|---|---|
| **Message Passing** (state ikut di pesan) | URL/form, **hidden field** |
| **Browser Memory** | Variabel JavaScript, store (Zustand/Redux) |
| **Browser Storage** | **Cookie**, **Local Storage / Session Storage** |
| **Server Side** | **Session**, **Database**, **File** (di server) |

🎯 **Prinsip "Server-Side Authority":** perlakukan **client (browser) sebagai lingkungan yang bermusuhan (hostile)**. Browser **hanya memegang referensi** (token, ID, hidden field), sedangkan **server memegang kebenaran (the truth)**.

💡 **Analogi:** Browser = pelanggan yang memegang **nomor antrean**. Server = bank yang memegang **saldo sebenarnya**. Pelanggan boleh mencoret-coret nomor antreannya, tapi tidak bisa mengubah saldo di sistem bank.

---

### 9.5 URL: Membawa State di Setiap Request

Contoh: website **Portal Akademik**.
```
Browser                                        Server: Go handler
department = STEI                              membaca request ini
page = 2
          ── GET /courses?department=STEI&page=2 ──►
```
- **Client mengirim ulang nilai-nilai itu setiap kali dibutuhkan.**
- ⚠️ **Jangan taruh password dan session ID di URL** (URL tersimpan di history, log server, dan bisa terbagikan).

#### Membaca query parameter di Go
```go
func products(w http.ResponseWriter, r *http.Request) {
    q := r.URL.Query()                     // ambil semua query parameter
    category := q.Get("department")        // "STEI"
    page := q.Get("page")                  // "2"

    w.Header().Set("Content-Type", "text/plain; charset=utf-8")
    fmt.Fprintln(w, category, page)        // tulis "STEI 2"
}
// diakses lewat: /courses?department=STEI&page=2
```

#### State di URL bertahan saat refresh
```javascript
const params = new URLSearchParams({
  department: "STEI",
  page: "2"
});
location.href = "/courses?" + params.toString();   // pindah ke /courses?department=STEI&page=2
```
- **Refresh akan meminta query yang sama lagi.**
- **Bookmark atau link yang dibagikan** bisa **mempertahankan filter dan pagination**.
- **Berguna untuk state navigasi** (filter, halaman, urutan).

### 9.6 Form HTML Membawa Nilai Bernama

```html
<form action="/register" method="post">
  <label>Username
    <input name="username" required>
  </label>
  <button type="submit">Register</button>
</form>
```
Yang terkirim:
```
POST /register
Content-Type: application/x-www-form-urlencoded

username=Arya
```

#### Membaca data form di Go
```go
r.Body = http.MaxBytesReader(w, r.Body, 4096) // WAJIB batasi ukuran body (maks 4096 byte)
if err := r.ParseForm(); err != nil {
    http.Error(w, "Invalid form", http.StatusBadRequest)      // 400
    return
}
username := r.PostForm.Get("username")
if username == "" {
    http.Error(w, "Username required", http.StatusBadRequest) // 400
    return
}
```
- `MaxBytesReader` mencegah penyerang mengirim body raksasa yang menghabiskan memori server.
- **Hasil parsing selalu berupa string. Aplikasi tetap harus memvalidasinya** (panjang, format, karakter yang diizinkan).

### 9.7 Hidden Field

```html
<form action="/enroll" method="post">
  <input type="hidden" name="offering_id" value="123">   <!-- tidak terlihat di halaman -->
  <button type="submit">Take Course</button>
</form>
```
```go
// Setelah mengecek r.ParseForm()
productID := r.PostForm.Get("offering_id") // WAJIB diverifikasi di server
```
⚠️ **Hidden field hanya untuk tampilan UI normal, BUKAN batas keamanan.** User bisa **mengubahnya lewat DevTools** (misal mengganti 123 jadi 999). Server harus mengecek: apakah offering 999 ada? apakah user ini boleh mengambilnya?

---

### 9.8 Browser Memory (Memori JavaScript)

Sebuah variabel JS **mengingat pilihan selama halaman berjalan**:
```javascript
let selectedCourseIds = [];

function render() {
  document.querySelector("#count").textContent = String(
     selectedCourseIds.length,
  );
}
```
⚠️ **Hilang saat halaman di-reload.**

### 9.9 Memory Store (Zustand, Redux, MobX)

- **Store** berguna ketika **koordinasi antar-bagian halaman menjadi lebih rumit** daripada memakai beberapa variabel biasa. **Tidak wajib untuk setiap halaman.**
- **Shared store** memungkinkan bagian-bagian halaman:
  - **Membaca state yang sama.**
  - **Mengubahnya lewat *actions*.**
  - **Berlangganan (subscribe) perubahan** sehingga tampilan bisa ikut diperbarui.
- Contoh library: **Zustand, Redux, MobX**.

💡 **Analogi:** Store = **papan pengumuman bersama** di kantor. Semua divisi membaca dari papan yang sama; perubahan hanya boleh lewat petugas resmi (action); siapa pun yang "berlangganan" langsung tahu kalau ada pengumuman baru.

#### Zustand: mendefinisikan shared store
Simpan ID yang dipilih dan action untuk mengubahnya **bersama-sama**:
```javascript
import { createStore } from "zustand/vanilla";

const store = createStore((set) => ({
  selectedCourseIds: [],                       // state
  addCourse: (id) =>                           // action
     set((state) => ({
       selectedCourseIds: state.selectedCourseIds.includes(id)
          ? state.selectedCourseIds            // sudah ada → tidak berubah
          : [...state.selectedCourseIds, id],  // belum ada → buat array baru + id
     })),
}));
```
- `addCourse` **menambahkan ID hanya jika belum dipilih** (tidak ada duplikat).
- `[...array, id]` = *spread syntax*: membuat **array baru** berisi isi lama + id (state tidak diubah langsung – prinsip *immutability*).

#### Zustand: menghubungkan store ke halaman
```javascript
function render(state) {
  document.querySelector("#count").textContent = String(
     state.selectedCourseIds.length,
  );
}

document.querySelector("#add42")
  .addEventListener("click", () => {
     store.getState().addCourse(42);          // jalankan action
  });

store.subscribe(render);                      // setiap state berubah → render ulang

render(store.getState());                     // render pertama kali
```
**Saat reload, semua state di memori hilang:**
1. Klik "add course 42": **count menjadi 1**.
2. **Reload**: halaman restart dan memori direset.
3. **Count kembali ke 0.**

🎯 **Shared store menjaga halaman tetap konsisten, tapi TIDAK menyimpan apa-apa secara permanen. Persistensi membutuhkan menyimpan (save) dan memulihkan (restore).**

---

### 9.10 Browser Storage: localStorage

#### Contoh: preferensi tema
```css
/* style.css */
body.dark { background: #16202b; color: white; }
```
```javascript
// app.js
localStorage.setItem("theme", "dark");                       // simpan
const theme = localStorage.getItem("theme");                 // baca
document.body.classList.toggle("dark", theme === "dark");    // pasang class "dark" jika tema dark
// localStorage.removeItem("theme");                         // hapus
```
**Reload membuat memori JS baru**, lalu kode **membaca preferensi yang tersimpan** → tema tetap gelap.

#### Menyimpan pilihan kursus sebagai JSON
localStorage **hanya bisa menyimpan string**, jadi array/objek harus diubah ke JSON:
```javascript
localStorage.setItem(
    "selectedCourses",
    JSON.stringify(selectedCourses)          // array → string JSON, misal "[42,7]"
);

try {
    const saved = JSON.parse(
        localStorage.getItem("selectedCourses") ?? "[]"   // ?? : kalau null, pakai "[]"
    );

    selectedCourses = Array.isArray(saved) ? saved : [];  // pastikan hasilnya array
} catch {
    selectedCourses = [];                    // JSON rusak / storage tak bisa diakses
}
```
⚠️ **Penulisan ke storage bisa gagal** (penuh, mode private, dinonaktifkan). **Pastikan antarmuka tetap bisa dipakai walau persistensi tidak tersedia.**

#### Masa hidup penyimpanan browser
| Mekanisme | Saat reload | Masa hidup biasanya |
|---|---|---|
| **Variabel JS** | **Tidak bertahan** | Selama dokumen (halaman) saat ini |
| **sessionStorage** | Bertahan | Selama **sesi halaman di tab itu** |
| **localStorage** | Bertahan | **Sampai dihapus / dikosongkan (evicted)** |

🎯 **Web Storage milik sebuah *origin*. Ia TIDAK otomatis ikut terkirim di request** (berbeda dengan cookie!).

#### Mengirim state browser secara eksplisit
```javascript
const theme = localStorage.getItem("theme");

await fetch("/api/preferences", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ theme })            // { "theme": "dark" }
});
```
**JavaScript harus menyalin nilainya ke body request** jika server membutuhkannya.

---

### 9.11 Cookie: Menghubungkan Browser Storage dengan HTTP

```
Browser                                   Go Server
   │────────── GET /dashboard ──────────────►│
   │◄───── Set-Cookie: theme=dark; Path=/ ───│   (server menyuruh simpan)
   │────────── GET /profile ────────────────►│
   │   [Cookie: theme=dark]                  │   (browser OTOMATIS melampirkan)
```
Beda utama dengan localStorage: **cookie otomatis dikirim browser di setiap request** ke situs yang sesuai.

#### Menyetel cookie di Go
```go
http.SetCookie(w, &http.Cookie{
    Name:     "theme",
    Value:    "dark",
    Path:     "/",                       // berlaku untuk semua path
    MaxAge:   60 * 60 * 24 * 30,         // 30 hari (dalam detik)
    Secure:   true,                      // hanya lewat HTTPS
    SameSite: http.SameSiteLaxMode,      // batasi pengiriman lintas situs
})
fmt.Fprintln(w, "Preference saved")
```
⚠️ **Set cookie SEBELUM menulis body response** (karena cookie adalah header, dan header harus dikirim sebelum body). Contoh ini **mengasumsikan HTTPS** (karena `Secure: true`).

#### Membaca dan menghapus cookie di Go
```go
theme := "light"                                          // nilai default
if c, err := r.Cookie("theme"); err == nil && c.Value == "dark" {
    theme = "dark"
}

// Untuk menghapus cookie di response berikutnya:
http.SetCookie(w, &http.Cookie{
    Name: "theme", Value: "", Path: "/", MaxAge: -1,      // MaxAge negatif = hapus sekarang
    Secure: true, SameSite: http.SameSiteLaxMode,
})
```
⚠️ **Tangani cookie yang tidak ada. Client bisa menghapus atau mengubah nilai cookie.** (Jangan percaya isinya tanpa verifikasi.)

#### Atribut cakupan & keamanan cookie
| Atribut | Efek |
|---|---|
| **`Secure`** | Hanya dikirim lewat **HTTPS** |
| **`HttpOnly`** | **Memblokir akses `document.cookie`** (JavaScript tidak bisa membaca → melindungi dari pencurian lewat XSS) |
| **`SameSite`** | **Membatasi pengiriman pada konteks lintas situs** (melindungi dari CSRF). Nilai: `Strict`, `Lax`, `None` |
| **`Path` / `Domain`** | Mengatur **request mana saja yang menerima cookie** |
| **`Max-Age` / `Expires`** | Mengatur **persistensi dan kedaluwarsa** di browser (tanpa ini = *session cookie*, hilang saat browser ditutup) |

**Jaga cookie tetap kecil** (batas ~4KB per cookie). **Aturan privasi browser bisa memblokir *third-party cookie*** (cookie dari domain lain, biasanya untuk pelacakan iklan).

---

### 9.12 Server-Side Session

```
Browser                                      Server session store
session_id = opaque token     ──────►        user ID, cart, expiry
(token acak tak bermakna)                    (data sebenarnya)
```
- **Browser menunjukkan sebuah identifier (ID).**
- **Server mencari state yang sesuai** dengan ID itu.
- **Data session tetap di server. Tapi identifier-nya tetap perlu "diangkut"** (biasanya lewat cookie).

💡 **Analogi:** Penitipan barang di mal. Kamu menitipkan tas (data) dan mendapat **kartu bernomor** (session ID). Kartunya tidak berisi tasmu – hanya nomor. Petugas (server) memakai nomor itu untuk mengambil tasmu dari rak.

#### Session ID di dalam cookie
```
Browser                                    Go server + session store
  │ 1  POST /login ───────────────────────────►│
  │                                            │ 2  Verifikasi kredensial, lalu buat session
  │◄────────── 3  Set-Cookie: session_id=<random> │
  │ 4  GET /profile + Cookie: session_id=... ──►│
```

#### Session store kecil di Go
```go
type Session struct {
    UserID  string
    Expires time.Time
}

var (
    mu       sync.Mutex                  // kunci, agar map aman diakses banyak goroutine
    sessions = map[string]Session{}      // session_id → data session
)
```
- **Map di memori cocok untuk demo pengajaran. Banyak server butuh *shared store*** (misal Redis atau database).
- `sync.Mutex` diperlukan karena Go melayani setiap request di **goroutine berbeda** (Bab 8) – tanpa kunci, dua goroutine bisa menulis map bersamaan → crash / data rusak (*race condition*).

#### Membuat session setelah login
```go
var token [32]byte                                       // 32 byte acak = 256 bit
if _, err := rand.Read(token[:]); err != nil {           // crypto/rand: acak yang aman secara kriptografi
    http.Error(w, "Session error", http.StatusInternalServerError)
    return
}

sid := hex.EncodeToString(token[:])                      // ubah ke teks hex (64 karakter)
mu.Lock()
sessions[sid] = Session{
    UserID: userID, Expires: time.Now().Add(30 * time.Minute),   // berlaku 30 menit
}
mu.Unlock()
```
⚠️ **Gunakan ID yang tidak bisa ditebak. Jangan pernah meng-hardcode session ID bersama.** (Kalau ID-nya bisa ditebak – misal 1, 2, 3 – penyerang tinggal mencoba nomor lain untuk membajak session orang.)

#### Mengirim dan membaca session ID
```go
// Response setelah login
http.SetCookie(w, &http.Cookie{
    Name: "session_id", Value: sid, Path: "/",
    HttpOnly: true, Secure: true,                      // tidak bisa dibaca JS, hanya HTTPS
    SameSite: http.SameSiteLaxMode, MaxAge: 1800,      // 30 menit
})

// Request berikutnya
c, err := r.Cookie("session_id")
if err != nil {
    http.Error(w, "Sign in", http.StatusUnauthorized)  // tidak ada cookie → 401
    return
}
```

#### Mengecek session dan masa berlakunya
```go
mu.Lock()
sess, ok := sessions[c.Value]                           // cari session berdasarkan ID
mu.Unlock()

if !ok || !time.Now().Before(sess.Expires) {            // tidak ada, ATAU sudah kedaluwarsa
    http.Error(w, "Sign in", http.StatusUnauthorized)
    return
}
// Gunakan sess.UserID, lalu lakukan otorisasi untuk aksi yang diminta.
```
🎯 **Kedaluwarsa cookie di browser dan kedaluwarsa session di server adalah DUA pengecekan yang independen.** Jangan hanya mengandalkan `MaxAge` cookie – penyerang bisa saja terus mengirim ID lama; server harus tetap mengecek `Expires`.

#### Keterbatasan session
Session sederhana, tapi punya keterbatasan.

**Di setup kelas saat ini (map di memori):**
- **Data hidup di satu server** — server lain tidak bisa mengakses data session yang sama.
- **Data bisa hilang saat restart** — session di memori hilang ketika aplikasi berhenti.

**Secara umum:**
- **Sulit di-*scale*** — banyak instance aplikasi butuh **shared session store**.
- **Persistensi terbatas** — data session tidak dirancang untuk data aplikasi jangka panjang.
- **Konkurensi dan sinkronisasi** makin rumit seiring aplikasi bertumbuh.

---

### 9.13 Database

**Database** = sistem untuk **menyimpan, mengorganisasi, dan mengambil data secara andal**.

Daripada menyimpan data hanya di memori aplikasi, kita simpan di database supaya data:
- **Tetap ada walau aplikasi restart** (*persist*).
- **Bisa menampung data dalam jumlah besar.**
- **Bisa dibagi ke banyak instance aplikasi.**
- **Bisa dicari dan diupdate secara efisien.**
- **Tetap terstruktur dan konsisten.**

### 9.14 Membuka Database di Go

- **`database/sql`** menyediakan **antarmuka (interface)**; sebuah **driver** yang berbicara dengan PostgreSQL (di sini **pgx**).
- **`sql.Open`**: membuat **handle**; **mungkin belum benar-benar terhubung**.
- **`PingContext`**: **mengecek konektivitas** sungguhan.
- **Buat SATU handle bersama saat startup; tutup saat shutdown.**

```go
import (
    "context"
    "database/sql"
    "time"
    _ "github.com/jackc/pgx/v5/stdlib"     // "_" = import hanya untuk mendaftarkan driver "pgx"
)

func openDB(ctx context.Context, dsn string) (*sql.DB, error) {
    db, err := sql.Open("pgx", dsn)          // dsn = alamat database, misal "postgres://user:pass@host/db"
    if err != nil { return nil, err }
    pingCtx, cancel := context.WithTimeout(ctx, 3*time.Second)   // batas waktu 3 detik
    defer cancel()                           // defer = jalankan saat fungsi selesai
    if err := db.PingContext(pingCtx); err != nil {
        db.Close()
        return nil, err
    }
    return db, nil
}
```
- **`context`** dipakai untuk **membatasi waktu** dan **membatalkan** operasi (misal jika user menutup browser, query ikut dibatalkan).

### 9.15 Mengambil Data (Query)

**Query berdasarkan ID, dan bedakan "data tidak ditemukan" dari "database error":**
```go
var title string
err := db.QueryRowContext(ctx,
    "SELECT title FROM course_offerings WHERE id = $1",   // $1 = placeholder parameter pertama
    offeringID,
).Scan(&title)                                             // salin kolom hasil ke variabel title
if errors.Is(err, sql.ErrNoRows) {
    return "", errOfferingNotFound                         // tidak ada baris → "not found" (404)
}
if err != nil { return "", err }                           // error lain → masalah database (500)
return title, nil
```
**`Scan` menyalin kolom yang dikembalikan ke variabel Go.** (`&title` = alamat variabel, supaya Scan bisa mengisinya.)

### 9.16 Prepared Statements

**Prepared statement** memungkinkan kita **mendefinisikan query SQL sekali** dan **menjalankannya berkali-kali dengan nilai berbeda**:
```go
db.QueryRowContext(ctx,
    "SELECT title FROM course_offerings WHERE id = $1", offeringID)
```
**Kenapa pakai prepared statement?**
- **Lebih aman** — membantu **mencegah SQL injection**.
- **Bisa dipakai ulang** — jalankan query yang sama dengan nilai berbeda.
- **Lebih rapi** — **SQL dan input user tetap terpisah**.
- **Berpotensi lebih efisien** untuk query yang sering diulang.

**Apa itu SQL injection?** Bayangkan query dibuat dengan menyambung string:
```go
// ❌ BERBAHAYA
query := "SELECT * FROM users WHERE name = '" + input + "'"
// jika input = ' OR '1'='1
// query jadi: SELECT * FROM users WHERE name = '' OR '1'='1'   → mengembalikan SEMUA user!
```
Dengan placeholder `$1`, input dikirim **terpisah** sebagai **data**, bukan sebagai **bagian dari perintah SQL**, jadi tidak bisa "membajak" query.

💡 **Analogi:** Menyambung string = membiarkan tamu **menulis sendiri di formulir perintah** ("ambilkan berkas atas nama ____") – tamu nakal bisa menulis "Budi, dan juga semua berkas lain". Prepared statement = formulirnya sudah tercetak, tamu **hanya boleh mengisi kotak nama**, dan isinya selalu diperlakukan sebagai nama.

### 9.17 Mengambil Banyak Baris
```go
rows, err := db.QueryContext(ctx,
    `SELECT id, title FROM course_offerings`,
)
if err != nil {
    return err
}
defer rows.Close()                            // WAJIB ditutup agar koneksi dikembalikan ke pool

var offerings []CourseOffering

for rows.Next() {                             // iterasi baris per baris
    var o CourseOffering

    if err := rows.Scan(&o.ID, &o.Title); err != nil {
        return err
    }

    offerings = append(offerings, o)          // tambahkan ke slice
}

if err := rows.Err(); err != nil {            // cek error yang terjadi selama iterasi
    return err
}
```

### 9.18 Menyisipkan Data (INSERT)
**Gunakan INSERT untuk membuat record baru.**
```go
_, err := db.ExecContext(ctx,
     `INSERT INTO enrollments (student_id, offering_id)
      VALUES ($1, $2)`,
     studentID, offeringID,
)

if err != nil {
    return err
}
```
- `ExecContext` dipakai untuk perintah yang **tidak mengembalikan baris** (INSERT/UPDATE/DELETE).

### 9.19 Mengubah Data (UPDATE)
```go
result, err := db.ExecContext(ctx,
    `UPDATE course_offerings SET capacity = $1 WHERE id = $2`, capacity, offeringID)
if err != nil {
    return err
}

rows, err := result.RowsAffected() // memberi tahu apakah ada record yang benar-benar terupdate
if err != nil {
    return err
}

if rows == 0 {
    return errOfferingNotFound      // tidak ada baris dengan id itu
}
```
⚠️ **SELALU gunakan `WHERE` untuk mengontrol record mana yang diubah.** (Tanpa WHERE, **semua** baris di tabel akan diubah!)

### 9.20 Menghapus Data (DELETE)
```go
result, err := db.ExecContext(ctx,
    `DELETE FROM enrollments WHERE student_id = $1 AND offering_id = $2`,
    studentID, offeringID,
)

if err != nil {
    return err
}

rows, err := result.RowsAffected()
if err != nil {
    return err
}

if rows == 0 {
    return errEnrollmentNotFound
}
```
⚠️ **SELALU gunakan `WHERE` untuk mengontrol record mana yang dihapus.**

### 9.21 Transaction (Transaksi)

**Transaksi** = sekelompok operasi database yang **dianggap satu kesatuan**: **semua berhasil, atau semua dibatalkan**.
```go
tx, err := db.BeginTx(ctx, nil)              // mulai transaksi
if err != nil {
   return err
}
defer tx.Rollback()                          // jika fungsi keluar sebelum Commit → batalkan semua

var stock int
err = tx.QueryRowContext(ctx,
   `SELECT stock FROM products WHERE id = $1 FOR UPDATE`, productID).Scan(&stock)
   //                                       ^^^^^^^^^^ kunci baris ini agar transaksi lain menunggu
if err != nil {
    return err
}

if stock <= 0 {
    return errors.New("Out of Stock")        // keluar → Rollback otomatis via defer
}

_, err = tx.ExecContext(ctx,
   `UPDATE products SET stock = stock - 1 WHERE id = $1`, productID)
if err != nil {
   return err
}

return tx.Commit()                           // semua sukses → simpan permanen
```
- **`FOR UPDATE`** mengunci baris produk sampai transaksi selesai, sehingga **dua pembeli tidak bisa membeli stok terakhir bersamaan** (ingat karakteristik "synchronize" di Bab 2!).
- `defer tx.Rollback()` aman dipanggil walaupun sudah Commit (akan diabaikan).

💡 **Analogi:** Transfer uang: "kurangi saldo A" dan "tambah saldo B" harus **sukses berdua atau gagal berdua**. Jangan sampai saldo A berkurang tapi B tidak bertambah.

### 9.22 Connection Pooling

**Membuka koneksi database baru untuk setiap request itu mahal.** **Connection pool** menyimpan **sekumpulan koneksi database yang siap dipakai ulang**.

| Setting | Tujuan |
|---|---|
| `SetMaxOpenConns` | **Membatasi** jumlah koneksi terbuka |
| `SetMaxIdleConns` | Menjaga koneksi **idle (menganggur) tetap siap** |
| `SetConnMaxLifetime` | **"Pensiunkan"** koneksi berdasarkan **umur** |
| `SetConnMaxIdleTime` | **"Pensiunkan"** koneksi berdasarkan **lama menganggur** |
| `DB.Stats` | **Mengamati** penggunaan dan antrean (waiting) |

**`sql.DB` mengelola pool koneksi yang bisa dipakai ulang dan dibagi oleh semua handler.** (Makanya cukup satu `sql.DB` untuk seluruh aplikasi – Bab 9.14.)

💡 **Analogi:** Pool koneksi = **pangkalan taksi**. Daripada memesan & menunggu taksi baru datang dari pool pusat setiap kali (lama), selalu ada beberapa taksi standby di depan gedung.

### 9.23 ORM (Object-Relational Mapper)

**ORM** **memetakan struct Go dan operasinya ke record database** (tidak perlu menulis SQL manual).
```go
type Registration struct {
    ID         int64
    StudentID  int64
    OfferingID int64
}
registration := Registration{
    StudentID: studentID, OfferingID: offeringID,
}
err := gormDB.WithContext(ctx).Create(&registration).Error
```
- **GORM menghasilkan perintah INSERT** secara otomatis.
- ⚠️ **Kamu tetap butuh *constraint* (di database), pengecekan izin (permission), dan transaksi.**
- ⚠️ **Waspadai "N+1 query":** satu query untuk mengambil daftar, **lalu satu query lagi untuk SETIAP baris**. Contoh: ambil 100 mahasiswa (1 query), lalu untuk setiap mahasiswa ambil jurusannya (100 query) = **101 query**. Solusi: JOIN atau preload (*eager loading*). (Masalah yang sama muncul di GraphQL, Bab 11.)

### 9.24 Catatan Penting soal Database

- **Sering kali kamu hanya punya SATU instance DBMS SQL:**
  - Bisa jadi **titik sempit (choking point)** performa aplikasi web, dan **Single Point of Failure** (kalau mati, semua mati).
- **Hati-hati dengan SQL wildcard** (`SELECT *`, `LIKE '%kata%'`):
  - Bisa menyebabkan **query lambat**.
  - **Hasil (result set) besar yang tidak perlu.**
- **Jangan pernah percaya input apa pun:**
  - **Sanitasi sebelum disimpan** ke database.
  - **Sanitasi/escape sebelum dicetak ke client** (setelah diambil dari database) → mencegah XSS.
- **Buka dan tutup koneksi database dengan bijak.**

---

### 9.25 File

Hal-hal yang umum dilakukan:
- **Membaca file**
- **Menulis file**
- **Bekerja dengan CSV** (*Comma-Separated Values* – format tabel teks: `101,Web Programming,3`)

#### Membuka file
```go
import (
    "encoding/csv"
    "fmt"
    "io"
    "os"
)

func main() {
   file, err := os.Open("courses.csv")      // buka file untuk dibaca
   if err != nil {
      return err
   }

   defer file.Close()                        // pastikan ditutup di akhir
```
#### Membaca file CSV
```go
   reader := csv.NewReader(file)             // buat CSV Reader
   for {
       row, err := reader.Read()             // baca SATU record (baris)
       if err == io.EOF {                    // EOF = End Of File → selesai
           break
       }
       if err != nil {
           return err
       }
       fmt.Println(row)                      // row adalah []string, misal [101 Web Programming 3]
   }
}
```
**Catatan:** bisa juga pakai **`reader.ReadAll()`** untuk mengambil semua baris sekaligus (hati-hati untuk file besar – semua masuk memori).

#### Menulis file CSV
```go
func main() {
  file, err := os.Create("courses.csv")      // buat (atau timpa) file
  if err != nil {
     return err
  }
  defer file.Close()

  writer := csv.NewWriter(file)
  defer writer.Flush()                       // tulis data yang masih di buffer ke file

  writer.Write([]string{"id", "title", "credits"})
  writer.Write([]string{"101", "Web Programming", "3"})
  writer.Write([]string{"102", "Database Systems", "4"})
}
```
(Catatan: `main()` di Go tidak bisa `return err`; ini potongan kode untuk ilustrasi – di program nyata letakkan di fungsi yang mengembalikan `error`.)

#### Catatan penting soal file
**1. Tangani error file dengan benar** (gagal buka, izin akses, ruang disk penuh, dll):
```go
file, err := os.Create("courses.csv")
if err != nil {
   return nil, fmt.Errorf("failed to create courses.csv: %w", err)   // bungkus error dengan konteks
}

defer file.Close() // Jangan lupa tutup file
```
**2. Sanitasi nama file yang diberikan user:**
```go
user := r.FormValue("user")

// Hanya izinkan karakter yang aman
validUser := regexp.MustCompile(`^[a-zA-Z0-9_-]+$`)
if !validUser.MatchString(user) {
    http.Error(w, "invalid user", http.StatusBadRequest)
    return
}

path := filepath.Join("/usr/local/data", user)
data, err := os.ReadFile(path)
```
⚠️ Kenapa? Tanpa validasi, penyerang bisa mengirim `user=../../etc/passwd` → membaca file sistem rahasia. Serangan ini disebut **path traversal**.

### 9.26 Penanganan File di Web App (Upload CSV ke Database)

Alur sederhana:
1. **Upload file ke server.**
2. **Simpan file di server.**
3. **Buka file di server.**
4. **Parse file dan insert ke database.**
5. **Bersihkan file sementara (temp)** jika ada.

```
Client          Server                         Local Disk            Database
  │─POST /upload (file in body)─►│                  │                    │
  │               │ r.Body = http.MaxBytesReader(...)                    │
  │               │ Parse multipart form             │                    │
  │               │──os.Create("/data/file.csv")────►│                    │
  │               │◄─────────── File handle ─────────│                    │
  │               │──io.Copy(dst, file)─────────────►│                    │
  │               │◄─────────── Write complete ──────│                    │
  │               │ [File now on disk]               │                    │
  │               │──os.Open("/data/file.csv")──────►│                    │
  │               │◄─────────── File handle ─────────│                    │
  │               │ csv.NewReader(file)              │                    │
  │               │ loop [For each row]:             │                    │
  │               │   reader.Read()                  │                    │
  │               │   ──INSERT INTO courses...──────────────────────────►│
  │               │   ◄────────────────────── Row inserted ──────────────│
  │               │──os.Remove("/data/file.csv")────►│ [Cleanup temp file]│
  │◄─200 OK "Import successful"─│                    │                    │
```

**Pertanyaan untuk dipikirkan (dari slide):**
- **Bagaimana jika CSV berisi 1000 baris sudah ter-insert 900 baris lalu gagal di baris ke-901?**
  → Data jadi setengah masuk (tidak konsisten). **Solusi:** bungkus semua insert dalam **satu transaksi** (Bab 9.21) → gagal di baris 901 = semua 900 baris dibatalkan (rollback); atau validasi semua baris dulu sebelum insert.
- **Bagaimana jika ada 100 user meng-upload CSV 200MB bersamaan?**
  → 20GB data membanjiri disk & memori server, request lama memblokir. **Solusi:** lihat alternatif di bawah (streaming, object storage, proses di background).

### 9.27 Alternatif 1: Stream Melalui Backend

```
Client          Server                                   Database
  │─POST /upload (file in r.Body)─►│                        │
  │               │ r.Body = http.MaxBytesReader(...)       │
  │               │ csv.NewReader(r.Body)                   │
  │               │ [Stream directly from HTTP body]        │
  │               │──BEGIN TRANSACTION────────────────────►│
  │               │ loop [For each row]:                    │
  │               │   reader.Read()                         │
  │               │   [Validate row data]                   │
  │               │   ──INSERT INTO courses...─────────────►│
  │               │   ◄───────────── Row inserted ──────────│
  │               │ [All rows processed]                    │
  │               │──COMMIT───────────────────────────────►│
  │               │ [No file saved to disk]                 │
  │               │ [Memory-efficient streaming]            │
  │◄─200 OK "Import successful"─│                           │
```
- File **dibaca langsung dari body HTTP** baris demi baris (*streaming*) – **tidak disimpan ke disk**, **hemat memori**.
- Setiap baris **divalidasi**, dan semuanya dalam **satu transaksi** (BEGIN ... COMMIT).

### 9.28 Alternatif 2: Off-load ke Object Storage

**Pindahkan beban upload file ke server lain (object storage).**
```
Client          Server                  Object Storage           Database
  ════ Phase 1: Get Upload URL ═══════════════════════════════════════════
  │─GET /upload-url─►│                        │                      │
  │                  │─Generate signed URL───►│ (10 min expiry)      │
  │                  │◄──Return signed URL────│                      │
  │◄─Return signed URL│                       │                      │
  ════ Phase 2: Direct Upload ════════════════════════════════════════════
  │─PUT file (using signed URL)──────────────►│                      │
  │◄─200 OK (upload complete)─────────────────│                      │
  ════ Phase 3: Process and Insert to Database ═══════════════════════════
  │─POST /process {file_key: "uploads/report.csv"}─►│                │
  │                  │──Download file────────►│                      │
  │                  │◄──File content─────────│                      │
  │                  │ csv.NewReader(file)    │                      │
  │                  │──BEGIN TRANSACTION──────────────────────────►│
  │                  │ loop [For each row]: reader.Read() → INSERT ─►│
  │                  │──COMMIT─────────────────────────────────────►│
  │◄─200 OK "Processing complete"│            │                      │
```
**Peningkatan:** **parsing file bisa dilakukan di background** (tidak di alur utama request) – misalnya server langsung membalas "202 Accepted, sedang diproses" lalu worker terpisah yang memproses.

### 9.29 Object Storage

**Object storage** = penyimpanan data komputer yang **mengelola data sebagai *objek*** (file + metadata + kunci unik), **berbeda dengan file system (folder/hierarki), data block, atau record**. Contoh: **Amazon S3**, Google Cloud Storage, MinIO.

**Alur umum UPLOAD:**
1. WebApp meminta ke Object Storage sebuah **Signed URL** untuk upload file.
2. **Client meng-upload file lewat Signed URL itu** (langsung ke object storage, tidak lewat server aplikasi).
3. **Signed URL hanya berlaku sebentar.**

**Alur DOWNLOAD:**
1. WebApp meminta ke Object Storage sebuah **Signed URL** untuk download file.
2. Client **menampilkan/mengunduh** file lewat Signed URL itu.

💡 **Analogi Signed URL:** **Kartu akses sekali pakai** dari resepsionis hotel yang hanya membuka satu pintu (satu file) dan **kedaluwarsa dalam 10 menit**. Tamu bisa langsung ke kamar tanpa ditemani resepsionis (server aplikasi tidak terbebani), tapi tetap aman karena kartunya terbatas.

#### Upload menggunakan Signed URL
**Server (Go, AWS SDK) – membuat signed URL:**
```go
import (
    "context"
    "time"
    "github.com/aws/aws-sdk-go-v2/aws"
    "github.com/aws/aws-sdk-go-v2/service/s3"
)

func createUploadURL(
    ctx context.Context,
    client *s3.Client,
    bucket, key string,               // bucket = "wadah", key = nama/path objek
) (string, error) {
    presigner := s3.NewPresignClient(client)
    req, err := presigner.PresignPutObject(      // PUT = untuk upload
        ctx,
        &s3.PutObjectInput{
            Bucket:      aws.String(bucket),
            Key:         aws.String(key),
            ContentType: aws.String("application/pdf"),   // hanya boleh PDF
        },
        func(o *s3.PresignOptions) {
            o.Expires = 10 * time.Minute                   // berlaku 10 menit
        },
    )
    if err != nil {
        return "", err
    }
    return req.URL, nil
}
```
**Browser – upload langsung:**
```javascript
await fetch(uploadURL, {
    method: "PUT",
    headers: {
        "Content-Type": "application/pdf",
    },
    body: file
});
```

#### Download menggunakan Signed URL
**Server:**
```go
func createDownloadURL(
    ctx context.Context,
    client *s3.Client,
    bucket, key string,
) (string, error) {
    presigner := s3.NewPresignClient(client)

    req, err := presigner.PresignGetObject(      // GET = untuk download
        ctx,
        &s3.GetObjectInput{
            Bucket: aws.String(bucket),
            Key:    aws.String(key),
        },
        func(o *s3.PresignOptions) {
            o.Expires = 10 * time.Minute
        },
    )
    if err != nil {
        return "", err
    }
    return req.URL, nil
}
```
**Browser bisa download langsung:**
```javascript
window.location.href = downloadURL;
```
**Atau lewat Fetch API:**
```javascript
const response = await fetch(downloadURL);
```

---

### 9.30 Authentication vs Authorization

Dua pertanyaan berbeda:
- **Siapa yang memakai aplikasi?** → **Authentication**
- **Apa yang boleh mereka lakukan?** → **Authorization**

| Aspek | **Authentication (AuthN)** | **Authorization (AuthZ)** |
|---|---|---|
| Definisi | **Membuktikan klaim (identitas)** | **Memberikan izin** |
| Pertanyaan | **"Kamu siapa?"** | **"Kamu boleh melakukan apa?"** |
| Tujuan | **Verifikasi identitas** | **Cek izin/hak akses** |
| Kapan | **Sekali (saat login)** | **Di setiap request yang dilindungi** |
| Contoh | "Kamu adalah student 123" | "Student 123 boleh melihat nilainya **sendiri**" |
| Response jika gagal | **401 Unauthorized** | **403 Forbidden** |
| Mekanisme | Password, OIDC, biometrik | Access control list (ACL), pengecekan kepemilikan (ownership) |

Topik yang dibahas (lanjutan dari State Handling): **registrasi & password**, **session & token**, **JWT, OAuth 2.0, dan OpenID Connect**.

💡 **Analogi bandara:** **Authentication** = petugas mencocokkan wajahmu dengan **paspor** ("benar kamu Budi"). **Authorization** = **boarding pass** menentukan kamu **boleh naik pesawat mana dan duduk di kursi mana**. Punya paspor sah (sudah terautentikasi) tidak berarti boleh naik semua pesawat.

⚠️ **Jebakan istilah:** status **401** namanya "Unauthorized" tapi artinya sebenarnya **"belum terautentikasi"** (belum login). **403 Forbidden** = sudah login, tapi **tidak punya izin**.

### 9.31 Verifikasi Password Dasar

| **Registrasi** | **Login** |
|---|---|
| **Buat *salt* unik.** | **Ambil hash yang tersimpan.** |
| **Hitung *adaptive password hash*** (**Argon2id, scrypt, PBKDF2**). | **Verifikasi password yang diberikan** (hash ulang dengan salt yang sama, lalu bandingkan). |
| **Simpan hash beserta parameternya.** | **Buat session HANYA jika sukses.** |

⚠️ **Gunakan implementasi password-hashing yang terpelihara (library resmi). JANGAN PERNAH menyimpan password dalam bentuk plaintext (teks asli).**

**Istilah:**
- **Hash** = fungsi satu arah: `password123` → `a9f3...` (tidak bisa dibalik). Saat login, password yang diketik di-hash lagi lalu dibandingkan.
- **Salt** = data acak unik per user yang dicampurkan sebelum hashing → dua user dengan password sama akan punya hash berbeda, dan mencegah serangan tabel pelangi (*rainbow table*).
- **Adaptive** = sengaja **lambat & bisa diatur tingkat kesulitannya** (misalnya 100ms per percobaan) → login normal tidak terasa, tapi penyerang yang menebak jutaan password jadi sangat lambat. (Karena itu jangan pakai MD5/SHA-256 biasa untuk password – terlalu cepat.)

### 9.32 Login dengan Server Session

```
[Verify credentials] ──► [Create session] ──► [Set ID cookie]
```
- **Request berikutnya: cookie mengidentifikasi session.**
- **Server memuat data user dan mengecek akses ke resource yang diminta.**
- 🎯 **Browser menjaga kesinambungan lewat cookie session-ID.**

### 9.33 Authentication, Authorization, Session, dan Token

| | Penjelasan | Contoh |
|---|---|---|
| **Authentication** | **Menetapkan identitas** user. | User yang login adalah student 123. |
| **Authorization** | **Mengecek akses ke aksi ini.** | Student 123 boleh melihat nilainya sendiri. |
| **Server-side session** | **State yang dipertahankan antar-request.** | User 123 login sampai waktu kedaluwarsa. |
| **Token** | **Sebuah nilai yang ditunjukkan client.** | ID acak yang mengidentifikasi session itu. |

🎯 **User yang sudah login tetap bisa menerima response "access denied" (403).**

### 9.34 Opaque Token Mengidentifikasi State di Server

```
Cookie: session_id=a8f3c9...

Browser menunjukkan          Server lookup
   a8f3c9…        ──────►    userID: 123
                             expires: ...
```
- **"Opaque"** = token **tidak bermakna** bagi siapa pun yang melihatnya (hanya deretan acak).
- **Catatan di server yang memberi makna. Client hanya memegang identifier.**

### 9.35 Basic Auth

Saat membuka halaman yang dilindungi (misal `http://localhost/members`), browser menampilkan **pop-up "Authentication Required"**: "*The server localhost:80 at Restricted Area requires a username and password*", berisi kolom User Name, Password, dan tombol Log In / Cancel. (Ini dialog bawaan browser, bukan buatan website.)

#### Implementasi Basic Auth di Go
```
GET /members
Authorization: Basic <username>:<password>      ← (dalam praktiknya: Base64 dari "username:password")
```
```go
func protectedHandler(w http.ResponseWriter, r *http.Request) {
  username, password, ok := r.BasicAuth()       // Go otomatis decode Base64 dari header Authorization

  if !ok || !checkCredential(username, password) {
      w.Header().Set(
          "WWW-Authenticate", `Basic realm="restricted"`)   // minta browser menampilkan pop-up login

      http.Error(w, "Unauthorized", http.StatusUnauthorized)   // 401
      return
  }

  fmt.Fprintln(w, "Welcome!")
}
```
(Lihat juga Bab 3.14. Ingat: **Base64 bukan enkripsi → wajib HTTPS.** Kelemahan lain Basic Auth: kredensial dikirim **di setiap request**, dan **tidak ada tombol logout** yang bersih.)

### 9.36 Bearer Token

```
GET /api/profile
Authorization: Bearer <access-token>
```
```javascript
const response = await fetch("/api/profile", {
  headers: { Authorization: `Bearer ${accessToken}` }
});
```
⚠️ **Kepemilikan (possession) itu yang menentukan** – siapa pun yang memegang token dianggap sah. Karena itu **jauhkan token dari URL, log, dan source code.**

### 9.37 Otorisasi di Handler Go

```go
// sess berasal dari server session yang sudah divalidasi.
// grade berasal dari data store milik server.
if grade.StudentID != sess.UserID {
    http.Error(w, "Forbidden", http.StatusForbidden)   // 403
    return
}

// Sekarang render data nilai yang diizinkan.
```
🎯 **Cek kepemilikan (ownership) di server untuk SETIAP resource yang dilindungi.** (Kalau tidak, user bisa mengganti ID di URL `/grades/124` dan melihat nilai orang lain → celah **BOLA/IDOR**, dibahas di Bab 10.)

### 9.38 Token Tidak Harus Berbentuk JWT

| Representasi | Yang dipegang client | Cara server mengecek |
|---|---|---|
| **Opaque token** | **ID acak** | **Lookup** (cari di database/session store) atau **introspection** (tanya ke server penerbit) |
| **Signed JWT** | **Klaim ter-encode + tanda tangan** | Cek **tanda tangan** dan **klaim yang diwajibkan** (tanpa perlu lookup) |
| **Format lain** | Representasi khusus protokol | Aturan yang didefinisikan protokol tersebut |

- **OAuth access token: bisa opaque ATAU JWT.**
- **OpenID Connect ID token: memakai format JWT.**

### 9.39 Struktur JWT (JSON Web Token)

```
base64url(header).base64url(payload).signature
```
Contoh JWT (dipotong): `eyJhbGciOi...` **.** `eyJzdWIiOi...` **.** `SflKxwRJ...`

| **Header** (algoritma + tipe) | **Payload** (klaim) | **Signature** (integritas) |
|---|---|---|
| `{` <br>`"alg": "RS256",` <br>`"typ": "JWT",` <br>`"kid": "ZqRwpX…"` <br>`}` | `{` <br>`"sub": "student-123",` <br>`"iss": "https://sso.app.id",` <br>`"aud": "course-api",` <br>`"exp": 1516239022,` <br>`"iat": 1516200000` <br>`}` | Kotak ketiga di slide menampilkan **kunci publik (format JWK)** yang dipakai untuk **memverifikasi** tanda tangan: <br>`{ "e": "AQAB", "kty": "RSA", "n": "0wN4ZU..." }` |

Arti field:
- **Header:**
  - `alg` = algoritma tanda tangan (**RS256** = RSA + SHA-256).
  - `typ` = tipe token (JWT).
  - `kid` = *key ID* – ID kunci yang dipakai (supaya server tahu kunci publik mana untuk verifikasi).
- **Payload (klaim/claims):**
  - `sub` (*subject*) = **siapa** yang dimaksud token ini (student-123).
  - `iss` (*issuer*) = **siapa yang menerbitkan** token (server SSO).
  - `aud` (*audience*) = token ini **ditujukan untuk siapa** (course-api).
  - `exp` (*expiration*) = **kapan kedaluwarsa** (Unix timestamp, detik sejak 1 Jan 1970).
  - `iat` (*issued at*) = **kapan diterbitkan**.
- **Signature:** tanda tangan digital atas header + payload, dibuat dengan **kunci privat** penerbit. Diverifikasi dengan **kunci publik** (JWK: `kty` = jenis kunci RSA, `n` = modulus, `e` = eksponen publik).

💡 **Analogi:** JWT = **ijazah bercap basah**. Siapa pun bisa **membaca** isinya (nama, tanggal lulus), tapi **tidak bisa memalsukan** karena ada **stempel/tanda tangan** resmi. Kalau isinya diubah sedikit saja, tanda tangannya tidak cocok lagi.

### 9.40 Decoding vs Validating JWT

| **Decoding JWT** | **Validating JWT** |
|---|---|
| **Membaca** header & klaim yang ter-encode. | **Cek tanda tangan dengan kunci tepercaya.** |
| **Siapa pun yang memegang JWT biasanya bisa melakukannya** (Base64url bukan enkripsi!). | **Wajibkan algoritma yang diizinkan** (tolak `alg: none` atau algoritma tak terduga). |
| | **Cek issuer (`iss`), audience (`aud`), dan expiry (`exp`).** |
| | **Terapkan *token profile* yang benar** (aturan khusus jenis token). |

⚠️ **Gunakan validator yang terpelihara (library resmi). Mem-*parse* JSON BUKAN berarti mengautentikasi user.** Jangan pernah menaruh rahasia (password, data sensitif) di payload JWT – semua orang bisa membacanya.

### 9.41 OAuth – Fitur

- **Aplikasi pihak ketiga bisa mengakses data user yang tersimpan di *service provider* TANPA membutuhkan username dan password user.**
- **Protokol *delegated authorization*** (otorisasi yang didelegasikan).
- **Persetujuan eksplisit user (*consent*) wajib.**
- **Ringan (light-weight).**
- **Use case:** **Website X bisa mengakses data terlindungi milikmu di API Y**:
  - **Semuanya tanpa membagikan password-mu ke luar situs.**
  - **Terutama ketika memang tidak ada password**, seperti dengan OpenID.
- **Pendekatan:** **Signed HTTP Requests**, **pertukaran token yang aman tanpa password**.

💡 **Analogi:** OAuth = **kunci valet parkir**. Kamu memberi petugas valet kunci khusus yang **hanya bisa menyalakan dan memarkir mobil**, tapi **tidak bisa membuka bagasi atau laci**. Kamu tidak memberikan kunci utama (password).

### 9.42 OAuth – Gambaran Umum (Authorization Code Flow)

```
 User             Your App                         Google Servers
  │                  │──── 1. Request token ──────────►│
  │──────────────────┼──── 2. User login & consent ───►│   (user login di Google & klik "Izinkan")
  │                  │◄─── 3. Authorization code ──────│
  │                  │──── 4. Exchange code for token ►│
  │                  │◄─── 5. Token response ──────────│   (access token, refresh token)
  │                  │──── 6. Use token to call ──────►│
  │                  │       Google API                │
```
1. Aplikasimu meminta token (mengarahkan user ke Google).
2. **User login di Google** (bukan di aplikasimu!) dan **memberi persetujuan**.
3. Google mengembalikan **authorization code** (kode sementara) ke aplikasimu.
4. Aplikasimu **menukar kode itu dengan token** (dari server ke server, menyertakan *client secret*).
5. Google mengirim **token response** (access token, dan biasanya refresh token).
6. Aplikasimu memakai **access token** untuk memanggil Google API (misal membaca Google Calendar user).

### 9.43 OpenID Connect (OIDC): Login Lewat Provider

**"Sign in using my campus account."** (Contoh: halaman Sign in akun ITB `...@itb.ac.id`.)
- **Provider mengautentikasi mahasiswa.**
- **Aplikasi menerima *ID token* yang mendeskripsikan autentikasi itu.**
- **Aplikasi memvalidasinya dan membuat session login-nya sendiri.**

🎯 **OAuth mendelegasikan AKSES. OpenID Connect menstandarkan informasi IDENTITAS.** (OIDC = lapisan identitas di atas OAuth 2.0.)

| | **OAuth 2.0** | **OpenID Connect** |
|---|---|---|
| Untuk | **Authorization** (akses ke data/API) | **Authentication** (login, siapa user-nya) |
| Hasil utama | **Access token** | **ID token** (JWT) + access token |
| Contoh | "Izinkan aplikasi X membaca Google Drive-mu" | "Login dengan Google / akun kampus" |

### 9.44 OIDC Memberikan ID Token, Access Token, dan Refresh Token

| Token | Konsumen (siapa yang memakai) | Tujuan |
|---|---|---|
| **ID token** | **Aplikasi client** | **Bukti tentang autentikasi user** (siapa yang login, kapan) |
| **Access token** | **Resource server / API** | **Akses sesuai izin yang diberikan** |
| **Refresh token** | **Authorization server** | **Mendapatkan access token pengganti** (saat yang lama kedaluwarsa) |

⚠️ **ID token BUKAN pengganti API access token.** ID token untuk aplikasimu ("siapa yang login"), access token untuk API ("boleh akses apa").

💡 **Analogi konser:** ID token = **KTP** yang ditunjukkan saat registrasi ulang (bukti identitas). Access token = **gelang konser** untuk masuk area tertentu (berlaku sebentar). Refresh token = **voucher** untuk menukar gelang baru jika gelang lama kedaluwarsa, tanpa antre registrasi lagi.

### 9.45 Alur OIDC dengan Go Server-Side Script

```
[Identity provider]          [Go backend]                 [Local session]
 OIDC authentication  ──►    validate token        ──►    user + expiry
```
- **Browser menerima cookie session-ID yang `HttpOnly`.**
- **Go bisa menyimpan token dari provider untuk panggilan API server-ke-server.**
- 🎯 **JWT, OIDC, dan server-side session bisa muncul BERSAMA dalam satu alur login:**
  1. User login di provider (OIDC).
  2. Provider mengirim ID token (format JWT) ke backend Go.
  3. Backend memvalidasi JWT (tanda tangan, iss, aud, exp).
  4. Backend membuat **session lokal**-nya sendiri dan mengirim cookie `session_id` HttpOnly ke browser.
  5. Request berikutnya cukup memakai cookie session (seperti Bab 9.12).

---

### ✅ Cek Pemahaman Bab 9

1. Sebutkan teknik-teknik state management dan di mana datanya disimpan.
2. Mengapa hidden field tidak aman sebagai batas keamanan?
3. Apa beda localStorage dan cookie dalam hal pengiriman ke server?
4. Apa fungsi atribut `HttpOnly`, `Secure`, `SameSite`?
5. Mengapa session ID harus acak dan tidak bisa ditebak?
6. Apa itu prepared statement dan mengapa penting?
7. Apa itu N+1 query?
8. Apa beda 401 dan 403? Apa beda AuthN dan AuthZ?
9. Sebutkan 3 bagian JWT. Apakah isi payload JWT rahasia?
10. Apa beda OAuth dan OpenID Connect? Apa beda ID token dan access token?

<details>
<summary>Lihat jawaban</summary>

1. Message passing (URL, form, hidden field – di request), browser memory (variabel JS/store – di RAM browser), browser storage (cookie, local/sessionStorage – di browser), server side (session, database, file – di server).
2. User bisa mengubah nilainya lewat DevTools; server tetap harus memverifikasi.
3. Cookie otomatis dikirim browser di setiap request; localStorage tidak, harus dikirim manual oleh JavaScript.
4. HttpOnly: JS tidak bisa membaca cookie. Secure: hanya dikirim lewat HTTPS. SameSite: membatasi pengiriman pada request lintas situs.
5. Agar penyerang tidak bisa menebak ID session orang lain dan membajak session-nya.
6. Query SQL dengan placeholder; data dikirim terpisah dari perintah SQL → mencegah SQL injection, bisa dipakai ulang, lebih rapi.
7. 1 query untuk daftar + 1 query per baris → total N+1 query; boros.
8. 401 = belum terautentikasi; 403 = sudah terautentikasi tapi tidak berizin. AuthN = membuktikan identitas; AuthZ = memeriksa izin.
9. Header, payload, signature. Tidak rahasia – bisa di-decode siapa pun; yang dijamin hanya integritasnya (tidak bisa diubah tanpa ketahuan).
10. OAuth = otorisasi/delegasi akses; OIDC = autentikasi/identitas di atas OAuth. ID token untuk aplikasi client (bukti login); access token untuk API (izin akses).
</details>

---
## Bab 10 – Desain REST API (06-01-REST-API-Design)

**API (Application Programming Interface)** = "pintu layanan" yang disediakan sebuah program agar **program lain** bisa memakainya. Web API = API yang diakses lewat HTTP. Contoh: aplikasi Gojek di HP memanggil `GET https://api.gojek.com/orders/123` untuk mendapat data pesanan (dalam JSON), lalu menampilkannya sendiri.

Banyak contoh di bab ini diambil dari buku **Arnaud Lauret, *The Design of Web APIs*, edisi ke-2**.

---

### 10.1 Kenapa API Perlu Didesain?

Misalkan `GET /products` mengembalikan:
- **Array produk** jika ada produk.
- **String `"No products"`** jika tidak ada produk.

Ini bermasalah karena:
- **Client harus menangani dua tipe response yang berbeda.**
- **Client tidak bisa mengandalkan satu skema yang terdokumentasi.**
- **Kode di sisi client jadi lebih rumit dan rawan error.**

🎯 **Kontrak yang didesain** memberi setiap operasi **input yang stabil, output yang stabil, arti error yang jelas, dan jalur migrasi**. Untuk contoh ini, koleksi kosong tetap dikembalikan sebagai `{ "items": [] }`.

```javascript
// Client jadi mudah: selalu bentuk yang sama
const { items } = await (await fetch("/products")).json();
items.forEach(tampilkanProduk);   // kalau kosong, loop tidak jalan – tanpa kasus khusus
```

### 10.2 Kenapa Desain API Itu Penting

| Manfaat | Mekanisme desain | Efek praktis |
|---|---|---|
| **Produktivitas developer** | Nama yang jelas & perilaku konsisten | Lebih sedikit tebak-tebakan dan kerja ulang integrasi |
| **Fleksibilitas arsitektur** | Kontrak stabil yang berorientasi bisnis | Perubahan internal memengaruhi lebih sedikit client |
| **Operasi yang efisien** | Data dibatasi (bounded) & caching yang tepat | Lebih sedikit trafik dan pemrosesan yang sia-sia |
| **Evolusi lebih aman** | Review kompatibilitas & migrasi | Risiko lebih rendah bagi pemakai yang sudah ada |

### 10.3 Antarmuka yang Stabil Menyembunyikan Kompleksitas Internal

(Lauret, Figure 1.3)
```
RESTORAN:
 CUSTOMERS ──order dish──► [WAITPERSON] ┄┄► ┊ cooks dish   KITCHEN ┊
           ◄──serves dish──                  (yang terjadi di belakang tersembunyi)
   ▲ hanya berinteraksi dengan antarmuka ▲ hanya antarmuka yang terlihat

API:
 DEVELOPERS ──code──► CONSUMER APPLICATIONS ──request to do something──► [API] ┄┄► ┊ does something  IMPLEMENTATION ┊
                                            ◄──────returns result────────        PROVIDER APPLICATION/SERVER
```
- Pelanggan hanya berinteraksi dengan **pelayan** (antarmuka); apa yang terjadi di dapur **tersembunyi**.
- Sama halnya, aplikasi consumer hanya berinteraksi dengan **API**; implementasinya tersembunyi.
- 🎯 **Backend boleh berubah (ganti database, ganti bahasa), sementara consumer tetap memakai kontrak yang sama dan kompatibel.**

### 10.4 Pendekatan Desain Berlapis (Lauret)

(Lauret, Figure 1.8)
```
 DEFINE              DESIGN                                                       DEVELOP
 (Vague) ──► Identify the ──► Design the   ──► Describe the ──► Enrich the ──► API design ──► TEST
  Needs       API              programming     programming      API design     ref. kit       DEPLOY
              capabilities     interface       interface        artifacts                     PROVIDE/CONSUME
            (bahasa natural) (bahasa pemrograman: (format deskripsi  (memudahkan desain
                              POST /statuses)    API standar)       & tahap berikutnya)

 DESIGN LAYERS (perspektif yang dipertimbangkan):
  ┌─────────────────────────────────────────────────────────────┐
  │ 1. A versatile API design that does the right job           │
  │ 2. An API design that is user-friendly and interoperable    │
  │ 3. An API design that considers constraints                 │  + API design toolbox
  │    (context, security, efficiency, backward-compatibility,  │    (dipakai ulang lintas API)
  │     and extensibility)                                      │
  │ 4. A reasoned and continuously improving API design process │
  └─────────────────────────────────────────────────────────────┘
```
**Empat lapis: (1) pekerjaan yang benar, (2) kemudahan pakai (usability), (3) batasan dunia nyata, (4) perbaikan berkelanjutan.**

---

### 10.5 LAPIS 1 – Melakukan Pekerjaan yang Benar (*Does the Right Job*)

> **Pahami kebutuhan user, lalu desain *capabilities*, *resources*, dan *operations* yang memenuhinya.**

#### Kapabilitas dulu, baru path HTTP
Jangan langsung memikirkan `GET /...`. Pikirkan dulu: **user ingin mencapai apa?**

Contoh perpustakaan: **seorang anggota mencari buku, meminjamnya, dan menerima konfirmasi peminjaman.**

#### Capabilities Canvas (Lauret, Figure 2.3)
| USER | USE CASE | STEP | INPUT | SUCCESS | FAILURE | OPERATION |
|---|---|---|---|---|---|---|
| Siapa user-nya? | Apa yang mereka lakukan? | Bagaimana caranya? | Apa yang dibutuhkan untuk langkah ini? Dari mana asalnya? | Apa yang terjadi jika berhasil? Bagaimana hasilnya dipakai? | Apa yang terjadi jika gagal? Bagaimana memperbaikinya? | Operasi unik & generik apa untuk mencapai langkah ini? |

Capabilities Canvas membantu memahami kebutuhan user:
1. **Ungkap apa yang ingin dicapai consumer** (telusuri **jalur nominal** atau **"happy path"** – jalur ketika semua berjalan lancar).
2. **Telusuri dulu use case yang sukses, baru lihat alternatif dan kegagalan.**
3. **Lacak dari mana input berasal, dan bagaimana consumer memakai output.**
4. **Gabungkan langkah-langkah berulang menjadi operasi yang serbaguna.**
5. (Di gambar juga ada langkah 4: **fokus pada kebutuhan yang tepat** – apakah butuh scope? perspektif provider? perspektif consumer yang terlalu spesifik?)

#### Contoh Capabilities Canvas: meminjam buku
| User | Use case | Step | Input | Success | Failure / recovery | Operation |
|---|---|---|---|---|---|---|
| Member | Borrow a book | **Find a book** | Search terms dari member | Daftar buku yang cocok → pilih satu | Tidak ada hasil → ubah kata kunci | **Search books** |
| Member | Borrow a book | **Confirm borrowing** | Book ID dari pilihan; Member ID dari aplikasi | Loan ID dan tanggal jatuh tempo → tampilkan konfirmasi | Buku tidak tersedia → pilih buku lain; Member ID hilang → lengkapi | **Create a loan** |

- **Alternatif:** jika ID buku sudah diketahui, member bisa **melewati pencarian** dan langsung meminjam.
- **Scope:** hanya meminjam buku. **Mengembalikan buku dan membayar denda adalah use case terpisah.**

#### Resource dan semantik HTTP
| Niat | Contoh | Hasil kontrak |
|---|---|---|
| **Membaca buku** | `GET /books/b1` | **200** dengan data buku, atau **404** |
| **Meminjam buku** | `POST /loans` | **201** dengan data loan dan header **`Location`** (URL loan baru) |
| **Mengganti / mengubah** | `PUT /books/b1` atau `PATCH /books/b1` | Definisikan state yang bisa ditulis & format patch |
| **Menghapus resource** | `DELETE /reservations/r1` | **204** jika tidak ada isi response |
| **Menjelaskan kegagalan** | Input tidak valid / buku tidak tersedia | **400** untuk input salah, **409** untuk konflik (buku sudah dipinjam) |

---

### 10.6 LAPIS 2 – Ramah Pengguna & Interoperable (*User Friendly*)

> **Buat API mudah dipahami dan dipakai lewat penamaan yang konsisten, format standar, error yang berguna, dan perilaku yang dapat diprediksi.**

#### Data yang dapat diprediksi dan mudah dipakai
```json
{
  "status": "active",
  "dueDate": "2026-10-17",
  "createdAt": "2026-09-27T09:00:00Z",
  "fine": {
    "amountMinor": 50000,
    "currency": "IDR"
  },
  "countryCode": "ID"
}
```
- **Kosakata jelas:** pakai **nama bermakna** dan **konvensi konsisten** seperti **camelCase** (`dueDate`, bukan campur `due_date` dan `DueDate`).
- **Format tidak ambigu:** pakai **timestamp RFC 3339** (`2026-09-27T09:00:00Z` – `Z` = UTC) dan **kode mata uang & negara yang eksplisit** (`IDR`, `ID`).
- **Data siap pakai:** pisahkan nilai ke field-field terpisah; **dokumentasikan satuan, presisi, dan *nullability*** (boleh null atau tidak).
- **Dokumentasikan satuan dan skala uang:** `amountMinor: 50000` dengan `currency: IDR` → satuan terkecil (untuk IDR biasanya rupiah tanpa desimal; untuk USD `amountMinor: 4999` = $49.99 karena satuan terkecilnya sen). Memakai integer menghindari error pembulatan *floating point*.

#### Koleksi yang dibatasi dan filter yang berguna
- **Response yang dibatasi (*bounded*):** tetapkan **ukuran halaman default dan maksimum**. Kembalikan **cara untuk meminta halaman berikutnya**.
- **Model paging yang disengaja:** pilih **offset** atau **cursor** pagination berdasarkan urutan data, laju perubahan, dan pola akses.
- **Perilaku filter yang eksplisit:** definisikan filter yang didukung, kombinasinya, nilai tidak valid, dan apakah wajib.
- **Hasil kosong yang dapat diprediksi:** kembalikan **koleksi kosong** jika pencarian valid tidak menemukan buku (bukan 404, bukan string).

#### Response error yang bisa ditindaklanjuti
```
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json

{
    "type": "urn:problem:invalid-input",
    "title": "Invalid input",
    "status": 400,
    "detail": "memberId is required"
}
```
- **Hasil HTTP yang jujur:** pakai status yang **mencerminkan kegagalan**, **jangan menyamarkan kegagalan sebagai sukses** (❌ `200 OK` dengan body `{"error": true}`).
- **Masalah yang terstruktur:** **RFC 9457** mendefinisikan **`application/problem+json`** dan field-field umumnya.
- **Info pemulihan yang berguna:** identifikasi masalah **secara aman**. Definisikan ekstensi error per-field di kontrak.

---

### 10.7 LAPIS 3 – Mempertimbangkan Batasan Dunia Nyata (*Real-World Constraints*)

> **Tangani keamanan, efisiensi, konteks teknis, kompatibilitas mundur (backward compatibility), dan ekstensibilitas.**

#### Scope dan izin user (Lauret, Figure 12.7)
```
Bob ──► Mobile Banking ─┐                           ┌─ API gateway ─┐   ┌─ Implementation ─┐
Bob ──► SpendAdvize   ──┼─► POST /transfers         │ ✔ Mobile Bank │   │ ✘ Bob doesn't    │
Alice ► Admin Website ──┘   Authorization: HPOI9    │ ✘ SpendAdvize │   │   have "unlimited│
                            {"source":"123",        │   doesn't have│   │   transfer"      │
                             "destination":"456",   │   transfer    │   │   permission     │
                             "amount":100001}       │   scope       │   │ ✔ Alice          │
                                                    │ ✔ Admin Web   │   │                  │
                                                    └───────────────┘   └──────────────────┘
APPLICATION     SCOPE              OPERATION        SCOPE        USER   PROFILE         PERMISSION
Mobile Banking  account, transfer  POST /transfers  transfer     Bob    Account owner   –
Admin Website   account, transfer  GET /accounts    account      Alice  Bank advisor    unlimited transfer
SpendAdvize     account
```
- **Access token** memungkinkan gateway & implementasi mendapat info **scope consumer** dan **izin user**.
- **API gateway** mengecek **scope aplikasi**: SpendAdvize (aplikasi analisis pengeluaran) **tidak punya scope `transfer`** → ditolak, walaupun user-nya Bob.
- **Implementation** mengecek **izin user**: Bob (pemilik akun biasa) mentransfer 100.001 → ditolak karena butuh izin "unlimited transfer"; Alice (bank advisor) punya izin itu → diizinkan.
- 🎯 **Scope membatasi akses sebuah APLIKASI. Izin user dan izin per-objek tetap harus ditegakkan.**

#### Mencegah BOLA (Broken Object-Level Authorization)
```
# Alice owns loan l1
GET /loans/l1
# Allowed by the access policy          ✔

# Bob owns loan l2
GET /loans/l2
# Alice must not receive Bob's loan     ✘

# Example concealment policy:
HTTP/1.1 404 Not Found
```
- **Kerentanannya:** BOLA memungkinkan pemanggil **mengakses objek tanpa izin** (cukup dengan **mengganti ID** di URL).
- **Keputusan yang diwajibkan:** cek **user, aksi yang diminta, dan loan tertentu** itu di **setiap operasi yang relevan**.
- **Aturan akses yang bisa dites:** Alice boleh membaca loan-nya sendiri. **Mengganti ID tidak boleh mengekspos loan Bob.**
- **Concealment policy:** API bisa memilih menjawab **404** (bukan 403) supaya penyerang **tidak tahu bahwa loan l2 itu ada**.

💡 **Analogi BOLA:** Loker di stasiun yang semua kuncinya sama – kamu cukup ganti nomor loker untuk membuka loker orang lain. Sistem yang benar mengecek: "Apakah loker nomor ini milik orang yang memegang kunci ini?"

#### Melindungi update dengan kondisi (Lauret, Figure 12.5)
```
Consumers A dan B:  GET /owners/123
                    → 200 OK
                      Etag: "z567dff"          ← "versi" data yang dikembalikan
                      {"email": "old@email.com", "jobTitle": "Old Title"}

Consumer A:  PUT /owners/123                    Consumer B:  PUT /owners/123
             If-Match: "z567dff"                             If-Match: "z567dff"
             {"email": "new@email.com",                      {"email": "old@email.com",
              "jobTitle": "Old Title"}                        "jobTitle": "New Title"}
             → Data matched!                                 → Data doesn't match anymore
             → 200 OK  (versi berubah jadi baru)             → 412 Precondition Failed
```
- A dan B membaca data yang sama (ETag `z567dff`). A mengupdate duluan → versi data berubah.
- B mencoba update dengan `If-Match: "z567dff"` (versi lama) → server menolak dengan **412 Precondition Failed**.
- 🎯 **`If-Match` mencegah update menimpa diam-diam representasi yang lebih baru.** Tanpa ini, perubahan email dari A akan **hilang** ditimpa B (*lost update problem*). B harus membaca ulang lalu menyelesaikan konflik.

#### Biaya API yang tidak efisien (Lauret, Figure 13.1)
Gambar menunjukkan rantai masalah:
- Di sisi **consumer** (HP): jaringan memengaruhi performa, aplikasi lambat, **baterai terkuras**, **banyak data dimuat (ulang)**, kuota data habis → **user marah**.
- Di sisi **panggilan API**: satu panggilan tapi lama, atau **banyak panggilan berurutan = total waktu respons lama** (Search 3s, Details 2s, Search 4s…), **error & retry meningkat**, waktu respons naik.
- Di sisi **infrastruktur provider** (API gateway → implementation → database): **load, throttling, dan rate limiting** bisa menambah waktu respons dan error; **infrastruktur kelebihan beban = data keluar (outbound) sangat besar + biaya tinggi**.
- 🎯 **Data berlebihan dan panggilan berulang meningkatkan latensi client, beban infrastruktur, dan trafik keluar.**

#### Kontrol efisiensi dan batasannya
| Kontrol | Contoh | Batasan penting |
|---|---|---|
| **Freshness** | `Cache-Control: max-age=60` | Hanya pakai ulang jika kebijakan cache mengizinkan |
| **Revalidation** | `If-None-Match` dengan ETag yang tersimpan | **304** tidak mentransfer body representasi |
| **Rate limits** | **429** dan kebijakan retry yang terdokumentasi | Batasi **pekerjaan yang mahal**, bukan hanya jumlah panggilan |
| **Bulk operations** | Membuat beberapa record per request | Batasi ukuran batch dan definisikan hasilnya |

#### Evolusi yang kompatibel
| Perubahan | Kemungkinan dampak ke consumer | Pendekatan yang lebih aman |
|---|---|---|
| **Rename `sts` menjadi `status`** | Kode lama **tidak menemukan `sts`** | Rencanakan **migrasi terdokumentasi** |
| **Mewajibkan input baru** | Request lama **jadi tidak valid** | Pertahankan perilaku lama atau **buat versi baru secara sengaja** |
| **Menambah nilai enum** | Logika yang **exhaustive** (mengecek semua kemungkinan) bisa gagal | Definisikan ekstensibilitas dan tes consumer |
| **Refactor penyimpanan** | **Tidak perlu perubahan API** | Pertahankan makna dan perilaku |

---

### 10.8 LAPIS 4 – Proses Desain yang Beralasan & Terus Membaik

> **Catat keputusan, tetapkan panduan (guidelines), review desain, otomatiskan pengecekan, dan masukkan feedback.**

#### Kontrak OpenAPI yang di-review
```yaml
# OpenAPI 3.1 fragment
paths:
  /loans:
    post:
      operationId: createLoan
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LoanCreate'
      responses:
        '201':
          description: Loan created
```
- **Specification-first design:** jelaskan operasi & skema **sejak awal** sehingga consumer bisa **me-review rancangan antarmuka** sebelum kode dibuat.
- **Bukti desain bersama:** pakai kontrak untuk **dokumentasi, contoh, mock (server tiruan), dan tooling** yang sesuai.
- **Kesesuaian dengan server:** review perubahan dan cek perilaku. **Dokumen saja tidak bisa mencegah *contract drift*** (implementasi menyimpang dari dokumen).

#### Panduan dan pengecekan otomatis
```yaml
rules:
  operation-id-required:
    description: Add an operationId
    severity: error
    given: $.paths.*[get,post]       # untuk setiap operasi GET & POST di semua path
    then:
      field: operationId
      function: truthy               # field operationId wajib ada
# Contoh perintah di CI:
# spectral lint openapi.yaml
```
- **Konvensi bersama:** sepakati penamaan, error, pagination – **beserta alasan dan contoh**.
- **Spectral di CI:** **lint** (periksa otomatis) deskripsi API dengan aturan tertentu **sebelum menerima perubahan**. (CI = *Continuous Integration*, pengecekan otomatis setiap ada perubahan kode.)
- **Review manusia tetap penting:** linting **tidak bisa membuktikan** kebutuhan user yang benar, otorisasi, atau perilaku runtime.

#### Desain buruk menimbulkan biaya di hilir
| Kelemahan desain | Kemungkinan akibat | Praktik pencegahan |
|---|---|---|
| **Detail internal di kontrak** | Client bergantung pada struktur backend | **Resource berorientasi bisnis** |
| **Tidak ada cek izin per-objek** | Data bocor atau diubah tanpa izin | **Object-level authorization** |
| **Hasil koleksi tidak dibatasi** | Biaya transfer tinggi & tekanan memori | **Pagination dan batas query** |
| **Breaking change tak terencana** | Perilaku client lama rusak | **Review kompatibilitas** |
| **Error samar/menyesatkan** | Debugging lambat & integrator frustrasi | **Kontrak error yang actionable** |

---

### 10.9 REST Lebih Dalam

#### Apa itu REST?
**REST = Representational State Transfer.** Diperkenalkan oleh **Roy Fielding** sebagai **gaya arsitektur (*architectural style*) untuk sistem jaringan**.
- **URI mengidentifikasi resource**, misal `/products/P101`.
- Client **mengambil atau mengubah** resource melalui **antarmuka yang seragam (uniform interface)**.
- Server mengirim **representasi**, misal **JSON yang mendeskripsikan produk**.

⚠️ **REST mendeskripsikan *batasan (constraints)* pada interaksi. Sekadar daftar endpoint CRUD BELUM menjamin batasan-batasan itu terpenuhi.**

#### REST & HTTP
**REST adalah gaya arsitektur, sedangkan HTTP adalah protokol** yang sering dipakai untuk mengimplementasikannya. HTTP menyediakan **lebih dari sekadar pengangkut JSON**:
- **Method** menyatakan **niat** (membaca atau mengubah resource).
- **Status code** mengkomunikasikan **hasil**.
- **Header** membawa **tipe konten, aturan cache, prasyarat (preconditions)**, dan metadata lain.
- **Body response** bisa berisi **JSON, XML, data biner, atau representasi lain**.

🎯 **Client harus membaca response HTTP SELENGKAPNYA, termasuk status dan header**, bukan hanya body.

#### HTTP Request/Response sebagai REST
```
Request                                                     
GET /music/artists/beatles/recordings HTTP/1.1      ← METHOD (GET) + RESOURCE (/music/.../recordings)
Host: media.example.com
Accept: application/xml                             ← minta representasi XML

Response                                            ┐
HTTP/1.1 200 OK                                     │
Date: Tue, 08 May 2007 16:41:58 GMT                 │ STATE TRANSFER
Server: Apache/1.3.6                                │ (pemindahan keadaan resource
Content-Type: application/xml; charset=UTF-8        │  dari server ke client)
                                                    │
<?xml version="1.0"?>                               │ ┐
<recordings xmlns="...">                            │ │ REPRESENTATION
  <recording>...</recording>                        │ │ (wujud resource dalam
  ...                                               │ │  format XML)
</recordings>                                       ┘ ┘
```
→ Inilah arti nama "**RE**presentational **S**tate **T**ransfer": **memindahkan (transfer) keadaan (state) sebuah resource dalam bentuk representasi** (XML/JSON).

### 10.10 Prinsip-Prinsip Desain REST

1. **Web punya resource yang bisa dialamatkan (*addressable*).** Setiap resource punya URI.
2. **Web punya antarmuka yang seragam dan terbatas (*uniform & constrained interface*).** HTTP misalnya punya sedikit method; gunakan itu untuk memanipulasi resource.
3. **Web berorientasi representasi** – menyediakan beragam format.
   - Berinteraksi dengan layanan **memakai representasi** layanan itu.
   - Platform berbeda, format berbeda: **browser → HTML**, **JavaScript → JSON**, **aplikasi Java → XML?**
4. **Web bisa dipakai berkomunikasi secara stateless** – memberikan **skalabilitas**.
5. **Hypermedia dipakai sebagai mesin state aplikasi** – **HATEOAS** (*Hypermedia As The Engine Of Application State*).
   - **Biarkan format datamu yang menggerakkan transisi state di aplikasi.**

### 10.11 Prinsip: Addressability

- **Setiap potongan data atau objek sistem diperlakukan sebagai resource** yang diekspos lewat **URI unik**.
```
GET /products/P101 HTTP/1.1
    └───┬───┘└─┬─┘
  the collection  the product
Accept: application/json
```
🎯 **Perbedaan kunci: URI menyebutkan APA yang dialamatkan client; method HTTP menyatakan APA yang ingin dilakukan terhadap resource itu.**

#### Aturan penamaan path
- **Gunakan kata benda (noun)** untuk resource dan **kata benda jamak (plural)** untuk koleksi: `/products`, `/orders`.
- **Jangan pakai parameter aksi (kata kerja) di URI**, seperti `/getOrders`. (Kata kerjanya sudah ada di method HTTP: `GET /orders`.)
- **Gunakan satu ejaan & huruf yang konsisten** untuk segmen path tetap: `/order_items`, **bukan** campuran `/OrderItems` dan `/order_items`.
- **Letakkan identifier resource setelah koleksinya:** `/products/{productId}`.
- **Gunakan identifier yang stabil**, bukan nama yang bisa diedit customer/seller:
```
# Identitas stabil
GET /products/P101

# Rapuh jika nama berubah
GET /products/travel-mug
```

| ❌ Kurang baik | ✅ Baik |
|---|---|
| `GET /getAllProducts` | `GET /products` |
| `POST /createOrder` | `POST /orders` |
| `GET /product?id=5` | `GET /products/5` |
| `POST /deleteUser/5` | `DELETE /users/5` |

#### Relasi path
**Gunakan nesting (bersarang) jika induk memberi koleksi itu arti yang jelas:**
```
GET /v1/customers/C7/orders      ← pesanan milik customer C7
GET /v1/carts/K12/items          ← item di keranjang K12
```
**Beri item yang bisa dialamatkan secara independen path yang pendek dan stabil** jika berguna:
```
GET /v1/orders/O55
```
**Hindari** mewajibkan `/v1/customers/C7/orders/O55/items/I8/product` hanya untuk mencapai sebuah produk. **Path yang dalam mengikat client ke rantai relasi yang panjang.** Kembalikan **URI atau ID produk terkait**, atau sediakan path yang lebih sederhana.

#### Path parameter vs Query parameter
- **Path parameter** → untuk **mengidentifikasi resource** yang dituju. `/products/P101` = satu produk.
- **Query parameter** → untuk **mengubah cara koleksi dicari atau ditampilkan**. `/products?category=mugs` = tampilan terfilter dari koleksi produk.
- **Catatan:** definisikan **filter yang diizinkan, kunci sort, default, dan batas maksimum** di kontrak API.

### 10.12 Prinsip: Uniform Interface

**Client berinteraksi dengan resource melalui sekumpulan aturan yang konsisten, terlepas dari bagaimana server menyimpannya.**

| Method | Safe? | Idempotent? | Pemakaian khas di toko online |
|---|---|---|---|
| **GET, HEAD** | Ya | Ya | Membaca produk atau metadata |
| **QUERY** | Ya | Ya | Pencarian kompleks read-only dengan body |
| **POST** | Tidak | **Tidak secara inheren** | Submit checkout |
| **PUT** | Tidak | Ya | Mengganti representasi produk yang diketahui |
| **PATCH** | Tidak | **Tergantung semantik patch** | Mengubah field tertentu produk |
| **DELETE** | Tidak | Ya | Menghapus item dari keranjang |

#### Properti method HTTP
- **Safe** = client meminta operasi **tanpa bermaksud mengubah state**. Logging insidental (server mencatat akses) **tidak mengubah niat itu**.
- **Idempotent** = **mengulang request yang identik punya efek yang dimaksudkan sama** seperti mengirimnya sekali. **Kode response tidak harus identik** setiap kali (DELETE pertama 204, kedua 404 – tetap idempotent).
- Properti ini **membantu client dan perantara (proxy) memutuskan cara menangani *retry*** setelah hasil jaringan yang tidak pasti (misal timeout: apakah aman mengirim ulang? Kalau idempotent → aman).

### 10.13 Detail Setiap Method dalam REST

**GET**
- Meminta **representasi tanpa meminta perubahan state**:
  ```
  GET /products/P101
  GET /products?category=mugs&limit=20
  ```
- **Cacheability tergantung header response dan konteks**; "safe" **tidak berarti "selalu di-cache"**.
- Boleh menyertakan **parameter di URI**.
- ❌ **Jangan pakai untuk aksi yang mengubah state**, seperti `GET /buy?product=P101`. (Bahaya: crawler mesin pencari atau prefetch browser bisa "membeli" tanpa sengaja!)

**QUERY**
- **RFC 10008** (menurut slide) mendefinisikan **QUERY** untuk request yang **safe & idempotent** dengan **input query di body**:
  ```
  QUERY /products
  Content-Type: application/json
  Accept: application/json

  {"category":"mugs","maxPriceMinor":2000}
  ```
- Gunakan saat **pencarian read-only terlalu kompleks atau panjang untuk URI yang wajar**. **Resource target yang mendefinisikan format dan cakupan query.**
- (Sebelum ada QUERY, orang sering terpaksa memakai `POST /products/search` – padahal POST tidak safe & tidak idempotent, sehingga tidak bisa di-cache/di-retry dengan aman.)

**POST**
- Setiap pemanggilan **bisa mengubah resource dengan cara yang unik** (tidak idempotent).
- Request **boleh/tidak** berisi informasi tambahan; response juga **boleh/tidak**.
- **Parameter ada di body request** (bukan di URI).
  ```
  POST /seller-products
  Content-Type: application/json

  {"name":"Travel Mug","description":"Insulated","active":true}
  ```
  → Server membuat produk baru dan menentukan ID-nya sendiri (respons **201 Created** + `Location: /seller-products/P102`).

**PUT**
- **Membuat resource baru atau MENGGANTI** representasi resource target dengan isi request.
- **Penggantian yang identik punya efek yang sama jika diulang** (idempotent).
  ```
  PUT /seller-products/P101
  Content-Type: application/json

  {"name":"Travel Mug","description":"Insulated","active":true}
  ```
  → Client menentukan URI-nya (`P101`). Field yang tidak dikirim dianggap hilang (karena ini **penggantian penuh**).

**PATCH**
- Menerapkan **modifikasi SEBAGIAN** pada resource.
- PATCH berfungsi sebagai **sekumpulan instruksi untuk memodifikasi** resource, sedangkan **PUT merepresentasikan penggantian lengkap**.
  ```
  PATCH /seller-products/P101
  Content-Type: application/merge-patch+json

  {"name":"Larger Travel Mug"}
  ```
  → Hanya `name` yang berubah; `description` dan `active` tetap. (`merge-patch+json` = format JSON Merge Patch, RFC 7396.)

**DELETE**
- Meminta server **menghapus** resource tertentu.
- Request/response **boleh/tidak** berisi informasi tambahan.
  ```
  DELETE /items/K12
  ```

💡 **PUT vs PATCH:** PUT = **mengganti seluruh formulir** dengan formulir baru. PATCH = **memakai tip-ex di satu kolom** saja.

### 10.14 Response Code Punya Makna

| Kode | Arti |
|---|---|
| **200** | Operasi baca/tulis **sukses dengan representasi** di response |
| **201** | **Data baru berhasil dibuat** |
| **400** | Input request **rusak atau tidak valid** |
| **401 / 403** | **Butuh autentikasi** / pemanggil yang sudah terautentikasi **dilarang** |
| **404** | **Resource tidak tersedia** |
| **500** | **Terjadi sesuatu yang buruk di server kita** |

(Tambahan dari bagian lain bab ini: **202** Accepted, **204** No Content, **304** Not Modified, **409** Conflict, **412** Precondition Failed, **429** Too Many Requests.)

#### Response error yang konsisten
Gunakan **HTTP Problem Details** untuk struktur error bersama jika cocok untuk API:
```
HTTP/1.1 409 Conflict
Content-Type: application/json

{
  "type": "https://api.example.com/problems/insufficient-stock",
  "title": "Insufficient stock",
  "status": 409,
  "detail": "The cart requests more units than are available."
}
```
- **Beri client tipe masalah (`type`) yang stabil** (supaya bisa ditangani secara program).
- ⚠️ **Jangan mengekspos stack trace, perintah SQL, atau rahasia** di pesan error (membantu penyerang).

### 10.15 Operasi CRUD

**Operasi CRUD (Create, Read, Update, Delete) pada resource dilakukan lewat method HTTP + URI.**

| Operasi CRUD | Verb (method) | Noun (URI) | Contoh |
|---|---|---|---|
| **Create** (single) | `POST` | **Collection URI** | `POST /products` |
| **Read** (multiple) | `GET` | **Collection URI** | `GET /products` |
| **Read** (single) | `GET` | **Entry URI** | `GET /products/123` |
| **Update** (single) | `PUT` | **Entry URI** | `PUT /products/123` |
| **Delete** (single) | `DELETE` | **Entry URI** | `DELETE /products/123` |

(4 method HTTP utama: POST, GET, PUT, DELETE.)

### 10.16 Prinsip: Representation Oriented

**(1)**
- **Representasi resource-lah yang dipertukarkan** (bukan resource-nya langsung).
- **GET mengembalikan representasi.**
- **PUT dan POST mengirim representasi ke server** supaya resource di baliknya bisa berubah.
- Representasi bisa dalam **banyak format: XML, JSON, YAML**, dll.

**(2)**
- HTTP memakai header **`Content-Type`** untuk menyatakan **format pesan yang dikirim**.
- Nilai `Content-Type` adalah **string MIME type**. **Info versi bisa disertakan.**
- Contoh: `text/plain`, `text/html`, `application/json`, `application/vnd+xml;version=1.1`
- **`vnd`** menandakan **MIME type khusus vendor** (buatan perusahaan tertentu, misal `application/vnd.github+json`).

**(3)**
- Header **`Accept`** dipakai dalam **content negotiation** (negosiasi konten): client memberi tahu format apa yang ia inginkan.
- Request AJAX misalnya bisa **meminta JSON**:
  ```
  GET /products/P101
  Accept: application/json        ← "tolong kirim dalam JSON"
  ```
  Server menjawab dengan `Content-Type: application/json` (atau **406 Not Acceptable** jika tidak bisa).

💡 **Analogi:** Resource = **sebuah lagu**. Representasinya bisa MP3, WAV, atau partitur. Kamu (client) bilang "saya mau MP3" (`Accept`), dan toko memberi tahu "ini MP3-nya" (`Content-Type`).

### 10.17 Prinsip: Berkomunikasi Secara Stateless

- Aplikasi **boleh punya state**, tapi **tidak ada data sesi client yang disimpan di server**.
- Jika ada data spesifik sesi, **data itu harus dipegang dan dikelola client** dan **dikirim ke server bersama setiap request** sesuai kebutuhan (misal token di header `Authorization`).
- **Server lebih mudah di-scale.** Tidak ada masalah replikasi data sesi (request bisa dilayani server mana pun).

⚠️ "Stateless" di sini maksudnya **state interaksi/sesi**, bukan berarti tidak ada database. **State resource** (data produk, pesanan) tetap tersimpan permanen di server.

### 10.18 Prinsip: HATEOAS

- **Hypermedia As The Engine Of Application State.**
- **Hypermedia** = berorientasi dokumen **dengan tambahan fitur link**.
- **Setiap response dari server memberi tahu interaksi apa yang bisa kamu lakukan selanjutnya**, serta **ke mana kamu bisa pergi** untuk mengubah state aplikasimu.

**✅ Dengan HATEOAS (ada link):**
```xml
<order id="111">
  <order-uri>http://…/order/111</order-uri>
  <customer>Alice
    <customer-uri>http://…/customers/3214</customer-uri>
  </customer>
  <order-entries>
    <order-entry>
      <qty>5</qty>
      <product>Noodle
        <product-uri>http://…/products/111</product-uri>
      </product>
    </order-entry>
  </order-entries>
</order>
```
**❌ Tanpa HATEOAS (tanpa link – dicoret X di slide):**
```xml
<order id="111">
  <customer>Alice</customer>
  <order-entries>
    <order-entry>
      <qty>5</qty>
      <product>Noodle</product>
    </order-entry>
  </order-entries>
</order>
```
Versi pertama memberi tahu client **di mana menemukan detail customer dan produk** – client tidak perlu "menebak" atau meng-hardcode URL.

💡 **Analogi:** HATEOAS seperti **website biasa**: kamu tidak perlu menghafal URL, cukup klik link yang tersedia di halaman. API dengan HATEOAS memberi "link" yang sama untuk program.

### 10.19 API Versioning

- **Developer backend bisa deploy setiap hari, tapi client mungkin hanya update beberapa kali setahun** (misal aplikasi HP yang user malas update). **Versi mengidentifikasi sebuah kontrak** yang bisa terus dipakai partner selama masa migrasi.
- **Kolom database baru atau nomor deployment TIDAK memerlukan versi API publik.**
- **Perubahan API yang kompatibel biasanya tetap di versi publik yang sama.**
- **Breaking change** mungkin butuh **kontrak paralel** ketika client yang ada tidak bisa bermigrasi bersamaan.
- **Versi menambah pekerjaan:** routing, dokumentasi, dukungan, dan akhirnya **pensiun (retirement)**.

🎯 **Pertanyaan desain: "Apa yang akan dikirim, di-*parse*, dan dilakukan client yang sudah ada setelah perubahan ini?"**

#### Ilustrasi
**Version 1:**
```
GET /v1/products/P101 HTTP/1.1
Host: api.store.example
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{"id":"P101","name":"Canvas Bag","price":25.00}
```
**Version 2:**
```
GET /v2/products/P101 HTTP/1.1
Host: api.store.example
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{"id":"P101","name":"Canvas Bag","price":{"amountMinor":2500,"currency":"USD"}}
```
→ Di v2, `price` berubah dari **angka** menjadi **objek** (amountMinor + currency). Ini **breaking change** (client v1 yang mengharapkan angka akan error), jadi dibuat **v2 paralel**, dan v1 tetap jalan sampai client lama bermigrasi.

### 10.20 OpenAPI Specification (OAS)

**OpenAPI Specification** (dulu bernama **Swagger Specification**) adalah **format deskripsi API untuk REST API**. File OpenAPI bisa mendeskripsikan **seluruh API**, termasuk:
- **Endpoint yang tersedia** (misal `/users`) dan **operasi di setiap endpoint** (`GET /users`, `POST /users`).
- **Parameter operasi**, **input dan output** setiap operasi.
- **Metode autentikasi.**
- **Info kontak, lisensi, syarat penggunaan**, dan info lain.

Spesifikasi API bisa ditulis dalam **YAML atau JSON**. Formatnya **mudah dipelajari** dan **bisa dibaca manusia maupun mesin**.
Referensi: https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.1.0.md

#### Struktur dasar: Metadata
**Setiap definisi API harus menyertakan versi OAS:**
```yaml
openapi: 3.0.0
```
**Info** – berisi informasi API: **title, description (opsional), version, contact, license, terms of service**, dll.
```yaml
info:
  title: Sample API
  description: Optional multiline or single-line description in [CommonMark](http://commonmark.org/help/) or HTML.
  version: 0.1.9
```

#### Struktur dasar: Servers
**Bagian `servers` menentukan server API dan base URL.**
```yaml
servers:
  - url: http://api.example.com/v1
    description: Optional server description, e.g. Main (production) server
  - url: http://staging-api.example.com
    description: Optional server description, e.g. Internal staging server for testing
```

#### Struktur dasar: Paths
**Bagian `paths` mendefinisikan endpoint (path) dan method HTTP (operasi) yang didukung.** Contoh `GET /users`:
```yaml
paths:
  /users:
    get:
      summary: Returns a list of users.
      description: Optional extended description in CommonMark or HTML
      responses:
        "200":
          description: A JSON array of user names
          content:
            application/json:
              schema:
                type: array
                items:
                  type: string
```
→ Artinya: `GET /users` mengembalikan **200** berisi **array of string** dalam JSON, misal `["andini", "bevin", "citra"]`.

#### Panduan lengkap OAS
Baca: https://swagger.io/docs/specification/v3_0/about/ – topik-topiknya: API Server and Base Path, Media Types, Paths and Operations, Describing Parameters, Parameter Serialization, Describing Request Body, Describing Responses, Data Models, Adding Examples, Authentication, Links, Callbacks, Components Section, Using `$ref`, API General Info, Grouping Operations With Tags, OpenAPI Extensions.

💡 Dari file OpenAPI, tool seperti **Swagger UI** bisa otomatis membuat **halaman dokumentasi interaktif** (bisa langsung mencoba API), dan tool lain bisa membuat **kode client/server** dan **mock server**.

---

### 10.21 Usability, Keamanan, dan Efisiensi API

#### Buat jelas – sering bergantung pada *Developer Experience* (DX)
```
# Sulit dipahami
{"st": 1, "d": "20261017"}

# Kosakata domain yang jelas
{
  "status": "active",
  "dueDate": "2026-10-17"
}
```
- **Kosakata jelas:** pilih nama dan nilai yang **mengkomunikasikan makna domain**. **Jelaskan kode & identifier yang tidak bisa dihindari.**
- **Konteks yang berguna:** kembalikan info yang dibutuhkan consumer untuk **menafsirkan hasil**, seperti **ketersediaan** atau **mata uang**.
- **Kurangi kerja berulang:** pertimbangkan **data terhitung (computed)** jika banyak consumer akan menghitung hal yang sama (misal kembalikan `totalPrice`, jangan biarkan setiap client menjumlahkan sendiri).

#### Tipe, nama, dan format yang interoperable
| Kategori | Contoh | Keputusan kontrak |
|---|---|---|
| **Nama property** | `bookId`, `dueDate` | Casing dan terminologi konsisten |
| **Timestamp / tanggal** | `2026-09-26T08:00:00Z` / `2026-10-17` | Zona waktu dan granularitas |
| **Mata uang / negara** | `USD` / `US` atau `USA` | Set kode ISO dan representasinya (ISO 4217 untuk mata uang, ISO 3166 untuk negara) |
| **Uang** | `amountMinor: 4999, currency: USD` | Satuan, presisi, dan skala |
| **Status** | `active` | Dokumentasikan nilai-nilai yang diizinkan |

#### Path, query parameter, dan lokasi input
- **Path parameter:** mengidentifikasi target, misal `/loans/{loanId}`. **Path parameter di OpenAPI selalu wajib (required).**
- **Query parameter:** menyatakan **filter, urutan, atau modifier lain**, misal `/books?q=web&available=true`.
- **Query wajib bisa saja valid:** kata kunci pencarian atau rentang tanggal yang wajib bisa jadi pilihan kontrak yang disengaja. **Dokumentasikan jika tidak ada dan jika nilainya tidak valid.**
- **Body dan header:** **body membawa representasi atau instruksi perubahan**. **Header membawa metadata** seperti tipe konten dan kredensial.

#### Filtering, sorting, dan pagination
| Concern | Contoh kontrak | Keputusan penting |
|---|---|---|
| **Filter** | `?category=software` | Cocokkan nama & definisikan kombinasi |
| **Sort** | `?sort=title:asc` | Urutan default dan **tie-breaker yang stabil** (misal urutkan juga by id jika judul sama) |
| **Offset** | `?offset=40&limit=20` | **Biaya** (database tetap melewati 40 baris) dan **perubahan data antar-halaman** (bisa ada data terlewat/dobel) |
| **Cursor** | `?after=token&limit=20` | Token yang **opaque** dan semantik urutan |
| **Envelope** | `items` plus `next` | Hasil kosong dan **penanda akhir daftar** |

Contoh response dengan envelope:
```json
{
  "items": [ {"id": "b41", "title": "Web Design"}, ... ],
  "next": "/books?after=eyJpZCI6ImI2MCJ9&limit=20"
}
```
(Jika `next` null → sudah halaman terakhir.)

💡 **Offset vs cursor:** Offset = "lewati 40 orang pertama di antrean" (kalau ada yang keluar/masuk antrean, hitunganmu kacau). Cursor = "mulai setelah orang berbaju merah" (tetap tepat walau antrean berubah).

#### Error yang actionable dan Problem Details
```
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json

{
    "type": "urn:problem:invalid-input",
    "title": "Invalid input",
    "status": 400,
    "errors": [{
       "pointer": "/memberId",
       "detail": "Member ID is required"
    }]
}
```
- **Envelope standar:** **RFC 9457** mendefinisikan `application/problem+json` dengan field **`type`, `title`, `status`, `detail`, dan `instance`**.
- **Ekstensi yang berguna:** API bisa mendefinisikan **array `errors` dengan *field pointer*** (JSON Pointer `/memberId` menunjuk field yang salah). **Field ekstensi ini butuh kontraknya sendiri.**
- **Feedback yang aman:** jelaskan cara pemulihan **tanpa mengekspos internal**. **Kumpulkan beberapa masalah validasi independen sekaligus** jika aman dan berguna (supaya user tidak bolak-balik memperbaiki satu per satu).

#### Granularitas, statelessness, dan draft
- **Batas operasi yang berguna:** hindari memaksa setiap consumer melalui **urutan panggilan kecil-kecil yang spesifik untuk satu layar**. **Buat transisi bisnis tetap jelas.**
- **Request yang mandiri (self-contained):** **setiap request membawa konteks yang dibutuhkan** untuk memprosesnya. **Interaksi stateless tetap mengizinkan state resource yang persisten.**
- **Menyimpan progres:** sebuah **draft aplikasi bisa menjadi resource** yang menerima update sebelum submit & validasi final.
- **Transisi eksplisit:** operasi **submit** bisa memvalidasi kesiapan dan **memindahkan draft ke state bisnis baru**.
  ```
  POST  /applications                 → buat draft (status: "draft")
  PATCH /applications/A1              → simpan progres (berkali-kali)
  POST  /applications/A1/submission   → validasi & ubah status jadi "submitted"
  ```

#### Batas API dan penemuan saat runtime
- **Batas domain:** **kelompokkan kapabilitas terkait secara koheren**. **Batas API dan batas deployment backend tidak harus sama** (satu API bisa dilayani beberapa microservice, atau sebaliknya).
- **Link dan relasi:** **header `Link`** atau **link JSON yang terdokumentasi** bisa menghubungkan resource dan hasil pagination.
- **OPTIONS dan Allow:** server bisa mengekspos opsi komunikasi dan **method yang didukung** (`Allow: GET, PUT, DELETE`). **Verifikasi perilakunya**, jangan mengasumsikan semua server mendukung penemuan ini.

#### Keamanan sejak desain & paparan minimal (*least exposure*)
- **Authentication:** tetapkan identitas atau konteks kredensial pemanggil dengan **mekanisme yang sesuai**.
- **Access scope:** cek **izin token** jika skema keamanan memakai scope. **Scope membatasi apa yang boleh diminta sebuah aplikasi.**
- **Resource authorization:** tegakkan aturan **kepemilikan, tenant, peran (role), dan aturan bisnis** untuk operasi & objek spesifik.
- **Data minimization:** **terima hanya property yang boleh ditulis**, dan **kembalikan hanya data yang boleh dilihat** pemanggil. (Contoh bahaya *mass assignment*: user mengirim `{"name":"x","role":"admin"}` lalu server menyimpan semua field mentah-mentah → user jadi admin.)

#### Object-level authorization dan BOLA (versi kontrak)
```yaml
/loans/{loanId}:
  get:
    summary: Read a loan
    description: |
      Return a loan only when the
      caller may access it.
      Apply ownership and tenant
      rules in the implementation.
      This API uses 404 to conceal
      inaccessible loan IDs.
```
- **Risikonya:** pemanggil yang **sudah terautentikasi mengganti ID objek** dan mendapat akses ke data user lain.
- **Pengecekan yang wajib:** validasi izin untuk **objek dan operasi yang diminta**, termasuk **baca, tulis, dan hasil list**.
- **Kontrak dan tes:** **dokumentasikan aturan akses** dan **tes dengan ID objek yang tidak berhak**. ⚠️ **OpenAPI sendiri TIDAK bisa menegakkan kepemilikan** – harus diimplementasikan di kode.

#### Retry dan update bersyarat
| Masalah | Mekanisme | Perilaku kontrak |
|---|---|---|
| **Pembuatan berulang setelah timeout** | **Idempotency key** yang terdokumentasi | Kenali sebagai **satu operasi logis** |
| **Client membaca resource** | `ETag: "v7"` | Kembalikan **validator representasi yang opaque** |
| **Client mengubah resource** | `If-Match: "v7"` | Update **hanya jika validator saat ini cocok** |
| **Update lain terjadi duluan** | **412 Precondition Failed** | **Baca ulang dan selesaikan konflik** |

**Idempotency key:** POST tidak idempotent, jadi kalau koneksi timeout saat checkout, client tidak tahu apakah pesanan sudah terbuat. Solusinya, client mengirim header unik, misal `Idempotency-Key: 7f3a...`. Jika request yang sama dikirim ulang dengan key yang sama, server **tidak membuat pesanan baru**, tapi mengembalikan hasil yang pertama.

#### OAuth scopes dan partisi akses
```yaml
# Di dalam securitySchemes.OAuth2
type: oauth2
flows:
  authorizationCode:
    authorizationUrl: https://id.example/authorize
    tokenUrl: https://id.example/token
    scopes:
      loans:read: Read permitted loans

# Kebutuhan pada operasi
security:
  - OAuth2: [loans:read]
```
- **Least privilege (hak minimal):** berikan **hanya kapabilitas yang dibutuhkan client**, dengan izin yang mudah dipahami seperti **`loans:read`**.
- **Kebutuhan eksplisit:** OpenAPI bisa mendefinisikan **security scheme** dan **scope yang dibutuhkan sebuah operasi**.
- **Pengecekan domain tetap ada:** scope memberi **satu kelas akses**. Scope **tidak memutuskan apakah pemanggil ini memiliki loan tertentu ini** (itu tugas object-level authorization).

#### Efisiensi, kuota, dan HTTP caching
| Mekanisme | Contoh | Arti |
|---|---|---|
| **Freshness** | `Cache-Control: max-age=600` | **Boleh dipakai ulang selama masih segar** 600 detik |
| **Revalidation** | `Cache-Control: no-cache` | **Validasi dulu sebelum memakai** data tersimpan |
| **No storage** | `Cache-Control: no-store` | **Jangan simpan** response ini |
| **Conditional read** | `If-None-Match: "v7"` | **304 jika tidak berubah**, tanpa body |
| **Request quota** | **429** dengan `Retry-After` | Ikuti **kebijakan retry** yang terdokumentasi |

#### Operasi bulk dan hasil campuran
```
HTTP/1.1 200 OK
Content-Type: application/json

{"results": [
   {"requestId":"a", "status":201, "location":"/loans/l1"},
   {"requestId":"b", "status":409, "code":"BOOK_UNAVAILABLE"}
]}
# envelope khusus API ini
```
- **Lebih sedikit round trip:** operasi bulk bisa mengurangi overhead request, tapi **butuh batas ukuran batch dan biaya pemrosesan**.
- **Kebijakan eksekusi eksplisit:** pilih **atomic (all-or-nothing)** atau **hasil per item yang independen**. Definisikan **urutan dan perilaku retry**.
- **Format hasil terdokumentasi:** status **207** (*Multi-Status*) **berasal dari WebDAV**. 207 **tidak menstandarkan** envelope batch JSON sembarang – jadi format seperti contoh di atas harus kamu dokumentasikan sendiri.

#### Operasi panjang, callback, dan event
```json
{
    "specversion": "1.0",
    "type": "org.example.loan.created",
    "source": "/library",
    "id": "evt-123",
    "time": "2026-09-26T08:00:00Z",
    "data": {"loanId": "l1"}
}
```
- **Pekerjaan asinkron:** **202 Accepted** berarti **pemrosesan belum selesai**. Sebuah **job resource** yang didesain bisa mengekspos **progres dan hasil akhirnya**.
  ```
  POST /reports           → 202 Accepted, Location: /jobs/J9
  GET  /jobs/J9           → {"status":"running","progress":40}
  GET  /jobs/J9           → {"status":"done","result":"/reports/R3"}
  ```
- **Polling dan notifikasi:** client bisa **polling URI status** (bertanya berkala) atau **menerima callback/webhook** (server yang menghubungi client saat selesai). **Definisikan autentikasi, retry, dan penanganan duplikat.**
- **Metadata CloudEvents:** **envelope event standar** yang mengidentifikasi **event, sumber (source), dan tipe (type)**. **Jaminan pengiriman (delivery guarantees) tetap urusan terpisah.**

---

### 10.22 API Management

#### Apa itu API management?
**API management** menyangkut **mempublikasikan, mengontrol, melindungi, dan mengamati (observe) API** untuk para consumer-nya.
- **API gateway** = **titik masuk runtime** yang bisa menerapkan **aturan trafik bersama**.
- **Dokumentasi, onboarding, inventaris, dan proses lifecycle** juga bagian dari API management yang lebih luas.

💡 **Analogi:** API gateway = **pos satpam + resepsionis di pintu masuk gedung perkantoran**. Semua tamu lewat sini: dicek ID-nya, dibatasi jumlahnya, dicatat, lalu diarahkan ke kantor (layanan backend) yang tepat. Kantor-kantor tidak perlu masing-masing punya satpam.

#### Apache APISIX di jalur request
```
Client ──► [ APISIX Gateway: Route → Plugins (auth, rate limit, log) ] ──► Upstream (backend service)
```
- **Route** – **mencocokkan request yang masuk** (misal path `/catalog/*`).
- **Upstream** – **menentukan ke mana trafik yang cocok diteruskan** (alamat server backend).
- **Consumer** – merepresentasikan **pemanggil API yang dikonfigurasi** untuk kebijakan tertentu.
- **Plugins** – menerapkan **perilaku gateway** seperti **autentikasi, rate limit, dan observability**.

**Admin API atau dashboard** dipakai untuk mengonfigurasi APISIX. ⚠️ **Lindungi akses manajemen secara terpisah dari trafik API publik.**

#### Contoh: mengautentikasi consumer API
Plugin **`key-auth`** APISIX bisa **mengecek API key sebelum meneruskan request** ke toko:
1. **Request tanpa kredensial yang diharapkan ditolak di gateway.**
2. **Request dengan kredensial yang terkonfigurasi diteruskan ke backend.**
3. **Backend tetap mengecek izin pesanan milik customer yang login.**

🎯 **API key yang mengidentifikasi sebuah *integrasi* (aplikasi) BERBEDA dengan login end user di toko.** Definisikan dengan tepat **identitas mana yang dicek oleh setiap lapisan**.

#### Contoh: rate-limit API katalog
Plugin **`limit-count`** bisa **membatasi jumlah request** untuk **key dan jendela waktu** tertentu (misal maks 100 request per menit per API key). Untuk demonstrasi instruktur, set batas kecil dan **konfigurasikan secara eksplisit response penolakan 429**.
- **Keuntungan:** **satu kebijakan gateway bisa melindungi beberapa route** dari trafik berlebihan.
- **Biaya:** **komponen terpusat** butuh **konfigurasi aman dan monitoring** (kalau gateway mati, semua API ikut tak bisa diakses).

#### Bacaan lanjutan
- Microsoft Learn, *Web API Design Best Practices*.
- Mark Massé, *REST API Design Rulebook*, Bab 2–6.
- OpenAPI 3.2 Specification dan OpenAPI HTTP Methods.
- RFC 10008: The HTTP QUERY Method.
- Dokumentasi Apache APISIX.
- Referensi kelas: slide web service tahun lalu dan *Web Design: The Missing Link*.

---

### ✅ Cek Pemahaman Bab 10

1. Sebutkan 4 lapis desain API menurut Lauret.
2. Apa itu Capabilities Canvas dan kolom-kolomnya?
3. Mengapa `/getOrders` dianggap desain buruk?
4. Kapan pakai path parameter dan kapan query parameter?
5. Apa beda PUT dan PATCH? Mana yang idempotent?
6. Apa itu BOLA dan bagaimana mencegahnya?
7. Apa fungsi `If-Match` dan status 412?
8. Sebutkan 5 prinsip REST.
9. Apa beda offset dan cursor pagination?
10. Kapan perlu membuat versi API baru?
11. Apa beda scope OAuth dan object-level authorization?

<details>
<summary>Lihat jawaban</summary>

1. (1) Melakukan pekerjaan yang benar, (2) user-friendly & interoperable, (3) mempertimbangkan batasan dunia nyata, (4) proses desain yang beralasan & terus membaik.
2. Alat untuk memetakan kebutuhan user: USER, USE CASE, STEP, INPUT, SUCCESS, FAILURE, OPERATION.
3. Ada kata kerja (aksi) di URI; seharusnya kata benda (resource) + method HTTP: `GET /orders`.
4. Path parameter: mengidentifikasi resource (`/products/P101`). Query parameter: filter/sort/pagination koleksi (`/products?category=mugs`).
5. PUT = ganti seluruh representasi (idempotent). PATCH = ubah sebagian (idempotensinya tergantung semantik patch).
6. Broken Object-Level Authorization: user mengganti ID objek dan mengakses data orang lain. Cegah dengan mengecek kepemilikan/izin objek di setiap operasi (dan tes dengan ID tak berhak).
7. `If-Match` = update hanya jika versi (ETag) masih sama; jika data sudah diubah orang lain, server menjawab 412 Precondition Failed → mencegah lost update.
8. Addressable resources, uniform interface, representation oriented, stateless communication, HATEOAS.
9. Offset: lompati N baris (bisa mahal & tidak stabil jika data berubah). Cursor: mulai setelah token posisi tertentu (stabil & efisien).
10. Saat ada breaking change yang tidak bisa dimigrasikan bersamaan oleh client yang ada.
11. Scope membatasi kelas akses sebuah aplikasi (misal `loans:read`); object-level authorization memastikan pemanggil memang berhak atas objek spesifik (loan milik siapa).
</details>

---
## Bab 11 – GraphQL (06-02-GraphQL)

### 11.1 Motivasi: Masalah REST

REST API masih punya beberapa masalah:
- **Over-fetching (kelebihan data):** halaman menerima **representasi produk yang besar**, padahal **hanya memakai sebagian field**. (Contoh: kamu hanya butuh nama produk, tapi server mengirim 50 field termasuk deskripsi panjang, spesifikasi, ulasan…)
- **Under-fetching (kekurangan data):** halaman menerima **ID seller**, lalu **harus membuat request lagi** untuk tahu **nama seller**. (Banyak bolak-balik request.)

```
REST (under-fetching):
  GET /products/P101    → {"id":"P101","name":"Travel Mug","sellerId":"S9", ...+40 field lain}
  GET /sellers/S9       → {"id":"S9","name":"Maya's Shop", ...}
  (2 request, banyak data tak terpakai)

GraphQL:
  1 request → { product(id:"P101") { name seller { name } } }
  → {"product":{"name":"Travel Mug","seller":{"name":"Maya's Shop"}}}
  (1 request, persis data yang diminta)
```

- **GraphQL bisa mengatasi keduanya** lewat **pemilihan field (*field selection*)** dan **penelusuran field-field terkait di skema (*traversal*)**.
- (Adil juga dicatat: **REST pun bisa** menawarkan field selection (`?fields=name,price`), expansion (`?expand=seller`), atau endpoint khusus.)

💡 **Analogi:** REST = **paket menu tetap** di restoran (Paket A selalu berisi nasi + ayam + sup + es teh, mau tidak mau). GraphQL = **prasmanan/buffet dengan piring sendiri** – kamu ambil persis yang kamu mau, dari beberapa "meja" sekaligus, dalam satu kali jalan.

### 11.2 Apa itu GraphQL?

GraphQL menyediakan:
- **Skema bertipe (*typed schema*)**,
- **Bahasa untuk mengekspresikan operasi**, dan
- **Aturan eksekusi** untuk menghasilkan hasil.

Punya **tiga operasi**:
| Operasi | Fungsi | Mirip di REST |
|---|---|---|
| **Query** | **Membaca data** | GET |
| **Mutation** | **Meminta perubahan** | POST/PUT/PATCH/DELETE |
| **Subscription** | **Menerima hasil dari waktu ke waktu** ketika event relevan terjadi (real-time) | (WebSocket/SSE) |

- **Server mengimplementasikan operasi-operasi ini di atas database dan layanannya sendiri.**
- ⚠️ **GraphQL TIDAK membutuhkan *graph database*.** ("Graph" di namanya merujuk pada cara data saling terhubung di skema, bukan jenis database.)
- Biasanya hanya ada **satu endpoint**, misal `POST /graphql`, dan semua operasi dikirim ke sana.

### 11.3 Skema adalah Kontrak Publik

**Skema menyebutkan tipe, field, dan relasi yang tersedia.** Client bisa meminta `product.seller.name` **hanya karena field-field itu didefinisikan**.
```graphql
type Seller {
  id: ID!
  name: String!
}
type Product {
  id: ID!
  name: String!
  seller: Seller          # relasi: Product punya field seller bertipe Seller
}
```
🎯 **Resolver** = fungsi di server yang **menyediakan data di balik setiap field skema**. **Client tidak bisa memilih kolom database sembarangan** – hanya field yang ada di skema.

💡 **Analogi:** Skema = **menu restoran yang lengkap**. Kamu hanya bisa memesan yang tertulis di menu. Resolver = **koki** yang tahu cara membuat setiap item di menu (dari bahan di gudang mana pun).

### 11.4 Tipe Scalar, Object, Enum, dan Input

**Scalar bawaan:** `String`, `Int`, `Float`, `Boolean`, dan `ID`.
- **Object** – mengelompokkan **field output** yang bisa dipilih (misal `Product`, `Seller`).
- **Enum** – membatasi ke **sekumpulan nilai bernama**, seperti `PLACED` atau `SHIPPED`.
- **Input object** – mengelompokkan **nilai-nilai yang boleh dikirim client** ke sebuah operasi.
- **Custom scalar**, seperti `DateTime`, **butuh dokumentasi cara parsing dan serialisasinya**.

```graphql
enum OrderStatus { PLACED SHIPPED DELIVERED }

input PlaceOrderInput {     # input = untuk DIKIRIM client
  cartId: ID!
}

type Order {                # type = untuk DIBACA client
  id: ID!
  status: OrderStatus!
}

scalar DateTime             # custom scalar
```
🎯 **Lebih baik memakai field bertipe yang bermakna daripada string JSON yang "buram" (opaque)** ketika client perlu menjelajahi dan memilih bagian-bagiannya. (❌ `details: String` berisi `"{\"color\":\"red\"}"` → client tidak bisa memilih `color` saja.)

### 11.5 List dan Nullability

Tanda **`!`** artinya **tidak boleh null (wajib ada)**. Kurung siku **`[ ]`** artinya **list (daftar)**.

| Tipe GraphQL | Arti |
|---|---|
| `Product` | Sebuah nilai product **atau null** |
| `Product!` | Sebuah nilai product **dijanjikan ada** (tidak null) |
| `[Product]` | **List atau null**; **item-itemnya juga boleh null** |
| `[Product!]!` | **List-nya pasti ada**; **setiap item pasti ada** |

💡 Cara membaca `[Product!]!` dari luar ke dalam: `]!` = "kotaknya pasti ada", `Product!` = "setiap isi kotak pasti produk, bukan kosong". (Kotaknya boleh berisi 0 item: `[]`.)

### 11.6 Query

#### Mulai dari skema
```graphql
type Query {
  product(id: ID!): Product       # field root "product", argumen id wajib, hasil Product atau null
}
type Product {
  id: ID!
  name: String!
  seller: Seller!
}
```
- `product` **menerima ID yang wajib**. Ia **mengembalikan Product atau null** – misalnya ketika **tidak ada produk yang bisa diakses** dengan ID itu.
- **Client hanya bisa memilih field yang didefinisikan** di `Product` dan tipe-tipe terkaitnya.

#### Operasi dan variabel
```graphql
query ProductPage($productId: ID!) {      # nama operasi: ProductPage, variabel: $productId
  product(id: $productId) {
    id
    name
    seller { name }                       # traversal ke relasi seller, ambil name saja
  }
}
```
**Variables:**
```json
{"productId":"P101"}
```
- **Variabel menyediakan ID produk tanpa mengubah teks operasinya** (teks query bisa dipakai ulang & di-cache; mirip prepared statement di SQL).
- **Selection (bagian dalam `{ }`) menyebutkan field-field hasil yang persis diminta client ini.**

#### Response
```json
{
  "data": {
    "product": {
      "id": "P101",
      "name": "Travel Mug",
      "seller": { "name": "Maya's Shop" }
    }
  }
}
```
- **Struktur JSON di bawah `data` mengikuti bentuk *selection* di operasi** (bentuk query = bentuk jawaban).
- Jika **tidak ada produk yang bisa diakses**, `data.product` **bisa null**.
- ⚠️ **Field yang dipilih tetap bisa membutuhkan beberapa lookup di backend** (satu request GraphQL ≠ satu query database).

### 11.7 Mutation

#### Desain skemanya
```graphql
input PlaceOrderInput {
  cartId: ID!
}
type PlaceOrderPayload {
  order: Order                         # bisa null jika gagal
  userErrors: [CheckoutError!]!        # daftar error bisnis (bisa kosong)
}
type Order { id: ID! status: String! }
type CheckoutError { code: String! message: String! }
type Mutation {
  placeOrder(input: PlaceOrderInput!): PlaceOrderPayload!
}
```
- **Input hanya berisi ID keranjang**, sedangkan **server mengambil identitas customer dari autentikasi** dan **menghitung harga sendiri**. (Jangan pernah terima harga dari client – client bisa memalsukan!)
- **Payload membawa hasil yang bisa diperiksa client** (order atau userErrors).

#### Operasi dan variabel
```graphql
mutation Checkout($input: PlaceOrderInput!) {
  placeOrder(input: $input) {
    order { id status }
    userErrors { code message }
  }
}
```
**Variables:**
```json
{"input":{"cartId":"K12"}}
```
- **Client meminta field order yang ia butuhkan setelah checkout.**
- **Server tetap mengecek kepemilikan keranjang, stok, dan jumlah akhir.**

#### Sukses vs gagal karena aturan bisnis
**Checkout sukses:**
```json
{"data":{"placeOrder":{"order":{"id":"O55","status":"PLACED"},"userErrors":[]}}}
```
**Hasil stok habis:**
```json
{"data":{"placeOrder":{"order":null,"userErrors":[{"code":"OUT_OF_STOCK","message":"Travel Mug is unavailable"}]}}}
```
- **Dokumentasikan invarian hasilnya** (misal: "tepat salah satu dari `order` atau `userErrors` yang terisi").
- ⚠️ **Response yang hilang bisa membuat client mengulang (retry)**, jadi checkout juga butuh **strategi di server untuk mencegah pesanan dobel** (misal idempotency key – Bab 10).

### 11.8 Subscription: Update Pesanan

**Skema:**
```graphql
type Subscription {
  orderStatusChanged(orderId: ID!): OrderStatusEvent!
}
type OrderStatusEvent { status: String! occurredAt: String! }
```
**Operasi:**
```graphql
subscription TrackOrder($id: ID!) {
  orderStatusChanged(orderId: $id) { status occurredAt }
}
```
Ketika pesanan berubah, client bisa menerima hasil seperti:
```json
{"data":{"orderStatusChanged":{"status":"SHIPPED","occurredAt":"2026-09-26T09:15:00Z"}}}
```
- **Server butuh sumber event (*event source*) dan transport streaming** (biasanya **WebSocket**, Bab 4).
- **Cek akses pesanan untuk setiap subscriber** (jangan sampai orang lain bisa "berlangganan" pesananmu).
- **Setelah reconnect, ambil status terkini (dengan query biasa)**, karena **event bisa saja terlewat** selama koneksi putus.

### 11.9 Performa Resolver: Masalah N+1

- Sebuah query mengembalikan **20 produk**. Jika **setiap resolver `Product.seller` melakukan lookup sendiri**, backend bisa melakukan **1 lookup produk + 20 lookup seller = 21 lookup**.
- **Solusi:** **kumpulkan (batch) ID-ID seller dalam satu request**, lalu **petakan hasilnya kembali ke produk masing-masing**. Ini bisa **mengurangi contoh tadi menjadi 2 lookup**. (Teknik populernya disebut **DataLoader**.)

```
TANPA batching:                         DENGAN batching:
SELECT * FROM products LIMIT 20         SELECT * FROM products LIMIT 20
SELECT * FROM sellers WHERE id=S1       SELECT * FROM sellers WHERE id IN (S1,S2,...,S20)
SELECT * FROM sellers WHERE id=S2       → total 2 query
... (20 kali)
→ total 21 query
```
- ⚠️ **Satu request jaringan GraphQL tetap bisa melakukan pekerjaan backend yang besar.** **Ukur jumlah panggilan ke sumber data dan waktu resolver.**
- **Cache yang berlaku per-request harus terisolasi antar-user** (jangan sampai data user A ikut terkirim ke user B).

### 11.10 Otorisasi dan Biaya Query

- **Tegakkan kepemilikan pesanan di logika bisnis bersama (shared business logic).** Sebuah order bisa dijangkau lewat **`Query.order`**, **`Customer.orders`**, atau **subscription** – jadi pengecekan izin harus ada di **semua jalur**, bukan hanya di satu.
- **Validasi input** dan **tegakkan batas**: **ukuran koleksi maksimum**, **kedalaman query** atau **estimasi biaya**, dan **batas waktu eksekusi**.
- **Pantau field-field yang mahal.** ⚠️ **Rate limit berdasarkan jumlah request saja TIDAK membatasi beban kerja di dalam SATU request GraphQL.**

Contoh query "jahat" yang dalam:
```graphql
{ product(id:"P1") { seller { products { seller { products { seller { products { name } } } } } } } }
```
Satu request ini bisa memicu ribuan lookup → perlu **batas kedalaman (depth limit)** dan **analisis biaya (cost analysis)**.

🎯 **Skema memverifikasi tipe dan bentuk. Skema TIDAK memberi izin untuk mengakses sebuah field.**

### 11.11 Error, Dokumentasi, dan Perubahan

- Gunakan **hasil bertipe yang konsisten** untuk **kegagalan checkout yang memang diharapkan** (seperti `userErrors`). **Kegagalan resolver yang tidak terduga** bisa muncul di **array `errors` GraphQL**, **kadang bersamaan dengan data parsial**:
  ```json
  {
    "data": { "product": { "name": "Travel Mug", "seller": null } },
    "errors": [ { "message": "Seller service timeout", "path": ["product", "seller"] } ]
  }
  ```
  (Perhatikan: GraphQL biasanya tetap membalas **HTTP 200** walaupun ada error – client harus memeriksa `errors`.)
- **Dokumentasikan** tipe, field, argumen, input, pagination, dan otorisasi. **Sediakan contoh operasi dan response.**
- **Tambahkan field baru secara kompatibel** jika memungkinkan. **Deprecate (tandai usang) field lama** dengan **pengganti dan panduan migrasi** sebelum menghapusnya:
  ```graphql
  sellerName: String @deprecated(reason: "Use seller { name }")
  ```
- **Introspection** membantu client **menemukan (discover) skema** (client bisa bertanya "field apa saja yang ada?"); tapi **tidak menjelaskan setiap aturan bisnis**.

### 11.12 Praktik GraphQL

| ✅ Lebih baik (Prefer) | ❌ Hindari (Avoid) |
|---|---|
| **Skema domain yang kecil & bertipe** | **String JSON buram** dan **mengekspos tabel database** mentah |
| **Koleksi yang dibatasi** | Field `products` **tanpa batas** |
| **Akses data bersarang yang di-batch** | **Satu lookup seller per produk** (N+1) |
| **Otorisasi di setiap jalur data** | **Cek izin hanya di satu root query** |
| **Evolusi yang terdokumentasi** | **Menghapus field yang dipakai tanpa pemberitahuan** |

### 11.13 Ringkasan: REST vs GraphQL

| Aspek | REST | GraphQL |
|---|---|---|
| Endpoint | **Banyak** (`/products`, `/sellers/S9`) | Biasanya **satu** (`/graphql`) |
| Bentuk data | **Ditentukan server** | **Ditentukan client** (selection) |
| Over/under-fetching | Rawan (bisa diatasi dengan desain) | Diatasi lewat field selection & traversal |
| Caching HTTP | **Mudah** (GET + Cache-Control/ETag) | Lebih sulit (biasanya POST ke satu URL) |
| Status error | Memakai **status HTTP** | Sering **200 + array `errors`** |
| Kontrak | OpenAPI | **Skema GraphQL** (+ introspection) |
| Risiko khusus | BOLA, koleksi tak terbatas | **N+1 resolver**, **query mahal/dalam**, otorisasi di banyak jalur |
| Real-time | Webhook/SSE/WebSocket terpisah | **Subscription** bawaan di spesifikasi |

---

### ✅ Cek Pemahaman Bab 11

1. Apa itu over-fetching dan under-fetching?
2. Sebutkan 3 operasi GraphQL dan fungsinya.
3. Apa arti `[Product!]!`?
4. Mengapa input mutation `placeOrder` hanya berisi `cartId`, bukan harga?
5. Apa itu masalah N+1 di GraphQL dan solusinya?
6. Mengapa rate limit berbasis jumlah request kurang cukup untuk GraphQL?
7. Apakah GraphQL butuh graph database?

<details>
<summary>Lihat jawaban</summary>

1. Over-fetching: menerima lebih banyak data dari yang dibutuhkan. Under-fetching: data kurang sehingga butuh request tambahan.
2. Query (membaca), Mutation (mengubah), Subscription (menerima update real-time saat event terjadi).
3. List wajib ada (tidak null), dan setiap item di dalamnya juga tidak null.
4. Identitas customer diambil dari autentikasi dan harga dihitung server – client tidak boleh dipercaya untuk menentukan harga.
5. Satu query daftar + satu lookup per item (misal seller per produk). Solusi: batch ID dalam satu lookup lalu petakan kembali (DataLoader).
6. Satu request GraphQL bisa sangat mahal (query dalam/besar). Perlu batas kedalaman, ukuran koleksi, estimasi biaya, dan deadline eksekusi.
7. Tidak. Server bisa memakai database & layanan apa pun di balik resolver.
</details>

---
## 📖 Glosarium (Kamus Istilah)

Diurutkan sesuai abjad. Angka dalam kurung = bab tempat istilah dibahas.

| Istilah | Arti singkat |
|---|---|
| **0-RTT** | Mengirim data aplikasi di paket pertama koneksi QUIC yang pernah dipakai; bisa di-*replay*, jadi hanya untuk operasi aman (3) |
| **AJAX** | Teknik meminta/mengirim data ke server tanpa reload halaman (XHR/Fetch + DOM) (6) |
| **Alt-Svc** | Header untuk mengiklankan protokol alternatif, misal HTTP/3 (`h3`) (3) |
| **API** | Antarmuka agar program lain bisa memakai layanan sebuah sistem (10) |
| **API Gateway** | Titik masuk runtime yang menerapkan aturan bersama (auth, rate limit, log) (10) |
| **APISIX** | API gateway open source dari Apache: Route, Upstream, Consumer, Plugins (10) |
| **Arrow function** | Sintaks fungsi ringkas di JS: `(x) => x * 2` (5) |
| **async/await** | Sintaks JS untuk menulis kode asinkron berbasis Promise seperti kode sinkron (5) |
| **Authentication (AuthN)** | Membuktikan identitas ("kamu siapa?"); gagal → 401 (9) |
| **Authorization (AuthZ)** | Memeriksa izin ("boleh apa?"); gagal → 403 (9) |
| **Base64** | Encoding (bukan enkripsi!) teks/biner menjadi karakter aman (3, 9) |
| **Basic Auth** | Autentikasi HTTP dengan `Authorization: Basic base64(user:pass)` (3, 9) |
| **Bearer token** | Token yang memberi akses ke siapa pun yang membawanya (3, 9) |
| **Block element** | Elemen yang mulai baris baru & selebar penuh (`div`, `p`) (4) |
| **BOLA** | *Broken Object-Level Authorization* – akses objek orang lain dengan mengganti ID (10) |
| **Box model** | Content → padding → border → margin (4) |
| **Cache-Control** | Header kebijakan cache: public, private, no-cache, no-store, max-age (3) |
| **Callback** | Fungsi yang dikirim ke fungsi lain untuk dipanggil nanti (5) |
| **Callback hell** | Callback bersarang dalam-dalam, sulit dibaca ("pyramid of doom") (5) |
| **Call stack** | Tumpukan frame pemanggilan fungsi (LIFO) (5) |
| **Capabilities Canvas** | Tabel User–Use case–Step–Input–Success–Failure–Operation untuk desain API (10) |
| **Cascade** | Algoritma CSS untuk menyelesaikan konflik aturan (urutan, specificity, origin, importance) (4) |
| **CDN** | Jaringan server cache yang tersebar dekat pengguna (3) |
| **CGI** | Common Gateway Interface – server menjalankan program, output jadi response (2) |
| **Client-server** | Model terdistribusi: client meminta, server melayani (2) |
| **Closure** | Fungsi + referensi ke scope luar tempat ia dibuat (5) |
| **CloudEvents** | Format envelope event standar (id, source, type, time, data) (10) |
| **Connection pool** | Kumpulan koneksi database siap pakai ulang (9) |
| **Content negotiation** | Client memilih format lewat `Accept`, server menjawab dengan `Content-Type` (10) |
| **Cookie** | Data kecil yang disimpan browser dan otomatis dikirim di setiap request (3, 9) |
| **CORS** | Mekanisme server mengizinkan akses lintas origin (`Access-Control-Allow-Origin`) (6) |
| **CRUD** | Create, Read, Update, Delete ↔ POST, GET, PUT/PATCH, DELETE (10) |
| **Cursor pagination** | Paging dengan token posisi (`?after=token`) (10) |
| **DataLoader / batching** | Mengumpulkan lookup per-item menjadi satu lookup untuk atasi N+1 (11) |
| **DNS** | Sistem penamaan: domain → alamat IP (2) |
| **DOM** | Representasi dokumen HTML sebagai pohon objek di memori browser (4, 5) |
| **Domain sharding** | Membagi resource ke banyak subdomain agar lebih banyak koneksi paralel (3) |
| **ECMAScript** | Nama standar resmi bahasa JavaScript (5) |
| **ETag** | Penanda versi resource; dipakai dengan `If-None-Match`/`If-Match` (3, 10) |
| **Event bubbling / capturing** | Fase perambatan event naik / turun di pohon DOM (5) |
| **Event loop** | Mekanisme JS yang mengambil tugas dari antrean ke call stack (5) |
| **Fetch API** | API modern berbasis Promise untuk request HTTP (6) |
| **Flexbox** | Layout CSS 1 dimensi (baris/kolom) dengan main & cross axis (4) |
| **Garbage collection** | Pembersihan memori otomatis (mark-and-sweep) (5) |
| **Goroutine** | Thread ringan Go (~2KB), di-multiplex M:N ke thread OS (8) |
| **GraphQL** | Bahasa query API bertipe: Query, Mutation, Subscription (11) |
| **Grid** | Layout CSS 2 dimensi (baris & kolom) (4) |
| **Hash (password)** | Fungsi satu arah; untuk password pakai Argon2id/scrypt/PBKDF2 + salt (9) |
| **HATEOAS** | Response berisi link ke aksi/resource berikutnya (10) |
| **Head-of-line blocking** | Request di belakang tertahan oleh request di depan (3) |
| **Hidden field** | `<input type="hidden">` – bisa diubah user, bukan batas keamanan (9) |
| **Hoisting** | Deklarasi `var`/fungsi "diangkat" ke atas scope (5) |
| **HPACK / QPACK** | Kompresi header HTTP/2 / HTTP/3 (3) |
| **HTTP** | Protokol aplikasi web, request–response, stateless (3) |
| **HTTPS** | HTTP di atas TLS di atas TCP (3) |
| **HttpOnly** | Atribut cookie: tidak bisa dibaca JavaScript (9) |
| **Idempotent** | Diulang berkali-kali efeknya sama dengan sekali (GET, PUT, DELETE) (3, 10) |
| **Idempotency key** | Header unik agar retry POST tidak membuat data ganda (10) |
| **If-Match / 412** | Update bersyarat; tolak jika versi sudah berubah (10) |
| **Inline element** | Elemen yang tidak mulai baris baru, selebar isinya (`span`, `a`) (4) |
| **Introspection** | Kemampuan client GraphQL menanyakan isi skema (11) |
| **JIT** | Just-In-Time compilation – compile saat runtime (V8, PHP 8) (5, 7) |
| **JWT** | `header.payload.signature` – token berisi klaim yang ditandatangani (9) |
| **localStorage / sessionStorage** | Web Storage per-origin; local bertahan, session hilang saat tab ditutup (4, 9) |
| **Macrotask / Microtask** | Antrean event loop: setTimeout/event vs Promise.then (5) |
| **MIME type** | Jenis konten `tipe/subtipe`, misal `application/json` (3, 10) |
| **Multiplexing** | Banyak stream dalam satu koneksi (HTTP/2, QUIC) (3) |
| **N+1 query** | 1 query daftar + 1 query per item (9, 11) |
| **N-tier** | Server dipecah jadi lapisan: presentation, business, database (2) |
| **NaN** | "Not a Number" di JS (5) |
| **OAuth 2.0** | Protokol delegasi otorisasi (akses tanpa berbagi password) (9) |
| **Object storage** | Penyimpanan data sebagai objek (misal S3), akses via signed URL (9) |
| **Offset pagination** | Paging dengan `?offset=40&limit=20` (10) |
| **OpenAPI (OAS)** | Format deskripsi REST API dalam YAML/JSON (dulu Swagger) (10) |
| **OpenID Connect (OIDC)** | Lapisan identitas di atas OAuth; memberi ID token (JWT) (9) |
| **Opaque token** | Token acak tanpa makna; server melakukan lookup (9) |
| **ORM** | Pemetaan struct/objek ↔ tabel database (GORM) (8, 9) |
| **Origin** | Skema + domain + port (6, 9) |
| **Over-/Under-fetching** | Data terlalu banyak / terlalu sedikit dari satu request (11) |
| **Peer-to-peer** | Semua node setara, bisa client sekaligus server (2) |
| **Persistent connection** | Satu koneksi TCP untuk banyak request (HTTP/1.1 keep-alive) (3) |
| **Prepared statement** | Query SQL dengan placeholder; mencegah SQL injection (9) |
| **Problem Details (RFC 9457)** | Format error standar `application/problem+json` (10) |
| **Promise** | Objek hasil operasi async: pending, fulfilled, rejected (5) |
| **Protokol** | Aturan komunikasi yang disepakati (2) |
| **Proxy / Reverse proxy** | Perantara di sisi client / di depan server (3) |
| **QUERY (method)** | Method HTTP safe & idempotent dengan body untuk pencarian kompleks (3, 10) |
| **QUIC** | Protokol transport di atas UDP, dasar HTTP/3 (3) |
| **Rate limiting** | Membatasi jumlah request; respons 429 (10) |
| **Resolver** | Fungsi server GraphQL yang menyediakan data untuk satu field (11) |
| **REST** | Representational State Transfer – gaya arsitektur (Roy Fielding) (10) |
| **Routing** | Memetakan URL (dan method) ke fungsi handler (7) |
| **Safe method** | Method yang tidak bermaksud mengubah state (GET, HEAD) (3, 10) |
| **Salt** | Data acak unik per user yang dicampur sebelum hashing password (9) |
| **SameSite** | Atribut cookie untuk membatasi pengiriman lintas situs (9) |
| **Scope (OAuth)** | Batas kelas akses sebuah aplikasi, misal `loans:read` (10) |
| **Secure (cookie)** | Cookie hanya dikirim lewat HTTPS (9) |
| **Semantic elements** | Tag HTML5 bermakna: header, nav, article, section, aside, footer (4) |
| **Service Worker** | Script background yang bertindak seperti proxy; dukungan offline/PWA (4) |
| **Session** | Data user disimpan di server, diidentifikasi session ID di cookie (7, 9) |
| **Signed URL** | URL sementara bertanda tangan untuk upload/download langsung ke object storage (9) |
| **Specificity** | Skor kekuatan selector CSS: inline 1000, ID 100, class 10, elemen 1 (4) |
| **SQL injection** | Serangan menyisipkan perintah SQL lewat input (7, 9) |
| **Stateless / Stateful** | Tidak menyimpan / menyimpan konteks interaksi (2, 3, 10) |
| **Status code** | Kode hasil HTTP: 1xx–5xx (3, 10) |
| **TCP / UDP** | Transport andal & berurutan / cepat tanpa jaminan (2, 3) |
| **Template** | File berisi teks statis + placeholder data dinamis (7) |
| **TLS** | Lapisan enkripsi di bawah HTTPS (3) |
| **Transaction** | Sekelompok operasi DB: semua sukses (commit) atau semua batal (rollback) (9) |
| **Type coercion** | Konversi tipe otomatis (`==`) di JS (5) |
| **TypeScript** | JS + tipe statis; dicek saat compile (5) |
| **URL / URI** | Alamat resource: `scheme://host:port/path?query#fragment` (2, 7) |
| **V8** | Engine JavaScript Google (Chrome, Node.js) (5) |
| **Virtual hosting** | Satu IP melayani banyak domain, dibedakan header `Host` (3) |
| **Web stack** | Seluruh tumpukan software web: OS, web server, bahasa, framework, DB (2, 7) |
| **WebSocket** | Komunikasi dua arah terus-menerus antara browser & server (4) |
| **Web Worker** | JS yang berjalan di thread background (4) |
| **XMLHttpRequest (XHR)** | Objek browser untuk request HTTP dari JS (6) |
| **XSS** | Cross-Site Scripting – menyisipkan script jahat ke halaman; dicegah dengan escaping (7, 9) |

---

## ⚡ Cheat Sheet Sebelum Ujian

### A. Peta 1 Halaman
```
Browser ──DNS──► IP ──TCP(+TLS)──► Server
  HTML (struktur) + CSS (gaya) + JS (perilaku)
  JS ── AJAX/Fetch (JSON) ──► API (REST / GraphQL)
Server: routing → auth → logika → data (DB/file/session) → template/JSON → response
State: URL | form/hidden | memori JS | local/sessionStorage | cookie | session | DB | file
```

### B. HTTP Methods
| Method | Safe | Idempotent | Body | Untuk |
|---|---|---|---|---|
| GET | ✅ | ✅ | ❌ | Ambil data |
| HEAD | ✅ | ✅ | ❌ | Ambil header saja |
| OPTIONS | ✅ | ✅ | – | Method apa yang didukung |
| TRACE | ✅ | ✅ | – | Debug: kembalikan request |
| QUERY | ✅ | ✅ | ✅ | Pencarian kompleks read-only |
| POST | ❌ | ❌ | ✅ | Buat / proses data |
| PUT | ❌ | ✅ | ✅ | Ganti seluruh resource |
| PATCH | ❌ | ❌* | ✅ | Ubah sebagian |
| DELETE | ❌ | ✅ | – | Hapus |
| CONNECT | ❌ | ❌ | – | Tunnel TCP (HTTPS via proxy) |

\* tergantung semantik patch.

### C. Status Code Wajib Hafal
`200 OK` · `201 Created` · `202 Accepted` · `204 No Content` · `301 Moved Permanently` · `303 See Other` · `304 Not Modified` · `400 Bad Request` · `401 Unauthorized (belum login)` · `403 Forbidden (tidak berizin)` · `404 Not Found` · `409 Conflict` · `412 Precondition Failed` · `429 Too Many Requests` · `500 Internal Server Error`

### D. Cache-Control
`public` (cache mana saja) · `private` (hanya browser) · `no-cache` (simpan tapi validasi dulu!) · `no-store` (jangan simpan) · `max-age=N` (segar N detik). Validasi: `ETag` + `If-None-Match` → `304`.

### E. Evolusi HTTP
| Versi | Kunci |
|---|---|
| 1.0 | 1 koneksi TCP per request, lalu close |
| 1.1 | Persistent connection, Host wajib, chunked, pipelining; masalah HOL blocking & 6 koneksi/domain |
| 2 | Biner, multiplexing stream di 1 koneksi TCP, HPACK, prioritas, server push; masalah TCP HOL |
| 3 | QUIC di atas UDP, TLS 1.3 terintegrasi, QPACK, connection ID (migrasi), 0-RTT; butuh fallback |

### F. CSS
- Specificity: **inline 1000 > ID 100 > class/pseudo-class/attribute 10 > elemen/pseudo-element 1**; `!important` mengalahkan semuanya.
- Cascade: **urutan, specificity, origin, importance**.
- Combinator: **spasi** (keturunan) · **`>`** (anak langsung) · **`+`** (saudara tepat sesudah) · **`~`** (semua saudara sesudah).
- Box model: **content → padding → border → margin**.
- Grid = 2D; Flexbox = 1D (main axis & cross axis).
- `display:none` (hilang + tanpa ruang) vs `visibility:hidden` (hilang + ruang tetap).
- overflow: **visible** (default) · **hidden** (potong) · **scroll** (selalu scrollbar) · **auto** (scrollbar jika perlu).

### G. JavaScript
- `var` (fungsi, hoisted) · `let` (blok) · `const` (blok, tak bisa re-assign).
- Falsy: `0, false, null, undefined, "", NaN`.
- `==` (coercion) vs `===` (strict). `typeof null === "object"`.
- Runtime: **heap + call stack + Web APIs + queue + event loop**. Urutan: **sinkron → microtask (Promise) → macrotask (setTimeout/event)**.
- Async: **callback → Promise (pending/fulfilled/rejected) → async/await**.
- GC: **mark-and-sweep** (reference counting sudah tidak dipakai).
- Event: **capturing → target → bubbling**; `addEventListener` > `onclick`.

### H. AJAX
- `readyState`: 0 belum init · 1 koneksi dibuat · 2 request diterima · 3 diproses · **4 selesai**.
- Pola: `if (this.readyState == 4 && this.status == 200)`.
- Same-origin = **skema + domain + port** sama; pengecualian via **CORS**.
- Fetch = berbasis Promise; XHR & Fetch = Web Platform API (bukan ECMAScript).
- Pola AJAX: multi-stage download, periodic refresh, predictive fetch, fallback, submission throttling.

### I. Server-Side
- 4 aspek: **routing, templating, form processing, data access**.
- Stack: **LAMP, MEAN/MERN/MEVN, WINS, XAMPP, GoTTH, JAMstack**.
- PHP superglobal: `$_GET $_POST $_REQUEST $_FILES $_COOKIE $_SESSION $_SERVER $_ENV`.
- PHP 7: 2x lebih cepat, strict typing opsional, `<=>`, `??`. PHP 8: JIT, nullsafe `?->`, union/return types.
- Model eksekusi: **process-per-request** (PHP CGI) · **thread-per-request** (Tomcat, WSGI) · **event loop** (Node, nginx) · **goroutine M:N** (Go).

### J. State & Auth
- Server-side authority: **browser hanya pegang referensi, server pegang kebenaran**.
- Cookie attributes: **Secure, HttpOnly, SameSite, Path/Domain, Max-Age/Expires**.
- Session ID: **acak (crypto/rand), HttpOnly, Secure, dicek expiry di server**.
- DB: **prepared statement**, **WHERE** saat UPDATE/DELETE, **transaction** (BEGIN/COMMIT/ROLLBACK, `FOR UPDATE`), **connection pool**, waspada **N+1**.
- File: tangani error, **sanitasi nama file** (path traversal), alternatif **streaming** / **object storage + signed URL** / proses di background.
- Password: **salt + adaptive hash (Argon2id/scrypt/PBKDF2)**, jangan plaintext.
- JWT = `base64url(header).base64url(payload).signature` – **bisa dibaca siapa saja, tapi tak bisa dipalsukan**; validasi signature, alg, iss, aud, exp.
- OAuth = **delegasi akses** (access token); OIDC = **identitas** (ID token). Refresh token → minta access token baru.

### K. REST API Design
- 4 lapis Lauret: **right job → user-friendly → constraints → continuous improvement**.
- 5 prinsip REST: **addressable, uniform interface, representation-oriented, stateless, HATEOAS**.
- Path: **kata benda jamak**, tanpa kata kerja, ID stabil, nesting secukupnya; **path param = identitas**, **query param = filter/sort/paging**.
- Error: **status jujur + `application/problem+json` (RFC 9457)**, tanpa stack trace/SQL.
- Keamanan: **AuthN → scope → object-level authorization (cegah BOLA) → data minimization**.
- Concurrency: **ETag + If-Match → 412**. Retry POST: **idempotency key**.
- Efisiensi: cache, `304`, **429 + Retry-After**, bulk (atomic vs per item; 207 dari WebDAV), **202 + job resource**, webhook, CloudEvents.
- Versioning hanya untuk **breaking change**. Kontrak: **OpenAPI** (openapi, info, servers, paths) + **Spectral** lint di CI.
- API Management: **gateway (APISIX: route, upstream, consumer, plugins: key-auth, limit-count)**.

### L. GraphQL
- Operasi: **Query, Mutation, Subscription**. Satu endpoint. Tidak butuh graph DB.
- Tipe: scalar (String, Int, Float, Boolean, ID), object, enum, input, custom scalar. `!` = non-null, `[ ]` = list.
- Mutation payload: **`order` + `userErrors`**; server hitung harga & cek kepemilikan.
- Risiko: **N+1 (pakai batching)**, **query mahal (batas kedalaman/biaya/deadline)**, **otorisasi di semua jalur**.
- Evolusi: tambah field kompatibel, `@deprecated(reason: ...)` sebelum hapus.

---

> 🎓 **Penutup:** Kalau kamu sudah bisa menjelaskan ulang setiap baris di Cheat Sheet ini dengan kata-katamu sendiri, berarti kamu sudah memahami seluruh materi slide 01a–06-02. Semangat belajarnya! 💪