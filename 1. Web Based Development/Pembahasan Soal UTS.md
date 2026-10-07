# 📝 Pembahasan UTS IF3110 – Pengembangan Aplikasi Berbasis Web

**Isi:** UTS Semester I 2020/2021 (40 soal pilihan ganda) dan UTS Semester I 2021/2022 (soal Benar/Salah per pernyataan).
**Sumber soal:** Bundel Soal HMIF 2024/2025.

> **Cara membaca file ini**
> - Setiap soal berisi **soal → jawaban → alasan**.
> - Jawaban di sini **dikerjakan ulang dari nol**, bukan disalin dari kunci bundel (bundelnya sendiri memperingatkan bahwa jawabannya "belum tentu benar").
> - Soal yang bisa dicoba langsung (PHP, JavaScript, HTML/CSS) **sudah aku jalankan** di PHP 8.3, Node.js 22, dan browser Chromium untuk memastikan hasilnya. Ditandai dengan ✅ **Terverifikasi**.
> - Soal kuis 2020/2021 berasal dari Quizizz dan beberapa soal **bisa punya lebih dari satu jawaban benar** ("Mana saja…"). Semua opsi yang benar aku sebutkan.
> - Untuk UTS 2021/2022, bundel sudah mencantumkan kunci (B/S). Kalau jawabanku **berbeda** dengan kunci bundel, aku tandai ⚠️ dan jelaskan alasannya. Rekap perbedaannya ada di bagian paling bawah.
> - Beberapa soal punya **typo dari pengetikan ulang** (misal tanda kutip tidak ditutup). Aku tandai 🔎 dan menjawab sesuai maksud soal.

---

## 📑 Daftar Isi
1. [Kunci Cepat UTS 2020/2021](#-kunci-cepat-uts-20202021)
2. [Pembahasan UTS 2020/2021](#uts-semester-i--20202021)
3. [Kunci Cepat UTS 2021/2022](#-kunci-cepat-uts-20212022)
4. [Pembahasan UTS 2021/2022](#uts-semester-i--20212022)
5. [Rekap Perbedaan dengan Kunci Bundel](#-rekap-perbedaan-dengan-kunci-bundel-uts-20212022)

---

## 🔑 Kunci Cepat UTS 2020/2021

| No | Jawaban | No | Jawaban | No | Jawaban | No | Jawaban |
|---|---|---|---|---|---|---|---|
| 1 | c (tebal, tidak miring) | 11 | b | 21 | a | 31 | a, b, c, d |
| 2 | b, c, d | 12 | a (21) | 22 | b | 32 | c |
| 3 | b, d | 13 | a, b, c | 23 | c (d dapat diterima*) | 33 | b |
| 4 | a (merah) | 14 | b ("21little birds") | 24 | c | 34 | a, b, c |
| 5 | b (biru) | 15 | a | 25 | d (a diperdebatkan*) | 35 | c |
| 6 | c | 16 | b | 26 | a, b, c | 36 | b, c, d |
| 7 | c | 17 | a, b | 27 | b, c, d | 37 | a, d |
| 8 | b, c | 18 | c | 28 | a, b, c | 38 | a, b, c |
| 9 | a, b, c | 19 | b (c jika scope="request") | 29 | a, b, c | 39 | a, b, c, d |
| 10 | c | 20 | b, c | 30 | a, b | 40 | a, b, c |

\* Lihat pembahasan masing-masing soal.

---

## UTS SEMESTER I – 2020/2021

*IF3110 Pengembangan Aplikasi Berbasis Web — Selasa, 13 Oktober 2020 — Waktu: 25 menit (soal dari Quizizz)*

---

### Soal 1
**Manakah tampilan yang merepresentasikan kode HTML ini?**
```html
<i style="font-weight: bold; font-style: normal;">Hello world</i>
```
Pilihan (berupa gambar):
- a. *Hello world* (miring, tipis)
- b. ***Hello world*** (miring + tebal)
- c. **Hello world** (tebal, tegak/tidak miring)
- d. Hello world (biasa)

**✅ Jawaban: c** (tebal, tidak miring). ✅ **Terverifikasi** (Chromium: `font-weight: 700`, `font-style: normal`).

**Alasan:**
- Secara default, `<i>` membuat teks **miring** (italic), karena gaya bawaan browser untuk `<i>` adalah `font-style: italic`.
- Tapi ada **inline style** yang menimpanya:
  - `font-weight: bold` → teks jadi **tebal**.
  - `font-style: normal` → miringnya **dibatalkan**, jadi tegak.
- Inline style punya prioritas lebih tinggi daripada gaya bawaan browser.

---

### Soal 2
**User saat ini sedang berada pada halaman `home.html`. Apa yang terjadi jika user mengklik link berikut:**
```html
<a href="home.html#section2">Section 2</a>
```
- a. Browser akan reload halaman home.html dan scroll ke lokasi `<section id="section2">`
- b. Browser akan langsung scroll ke lokasi `<a id="section2">`
- c. Browser akan langsung scroll ke lokasi `<section id="section2">`
- d. Browser akan langsung scroll ke lokasi `<a name="section2">`

**✅ Jawaban: b, c, d.** ✅ **Terverifikasi** di Chromium: halaman **tidak di-reload**, lalu scroll ke `<a id="section2">` maupun ke `<a name="section2">`.

**Alasan:**
- URL tujuan (`home.html#section2`) **sama dengan halaman yang sedang dibuka**, hanya beda di bagian **fragment** (`#section2`). Untuk kasus ini browser melakukan **"navigate to fragment"**: **tidak me-reload** halaman, langsung scroll. Jadi **a salah**.
- Menurut standar HTML, target fragment dicari dengan urutan:
  1. Elemen **apa pun** dengan `id="section2"` → jadi `<a id>` (b) dan `<section id>` (c) sama-sama valid.
  2. Jika tidak ada, elemen `<a>` dengan `name="section2"` (cara lama) → d valid.
- Pada praktiknya hanya ada satu elemen target di halaman. Pernyataan b, c, dan d semuanya benar untuk elemen target masing-masing.

🔎 *Typo di bundel:* `href="home.html#section2>` tidak punya tanda kutip penutup. Aku menjawab sesuai maksud soal, yaitu `href="home.html#section2"`.

---

### Soal 3
**Mana saja di antara tag HTML berikut yang bisa digunakan untuk menampilkan pilihan majemuk (multiple selection)?**
- a. `<input type="radio">`
- b. `<select multiple></select>`
- c. `<select></select>`
- d. `<input type="checkbox">`

**✅ Jawaban: b dan d.**

**Alasan:**
- `<select multiple>`: user bisa memilih **beberapa** option (Ctrl/Shift + klik).
- `<input type="checkbox">`: setiap checkbox bisa dicentang **secara independen**, jadi beberapa pilihan bisa dicentang sekaligus.
- `<input type="radio">`: dalam satu grup (`name` sama) **hanya satu** yang bisa dipilih → pilihan tunggal.
- `<select>` tanpa `multiple`: dropdown dengan **satu** pilihan.

---

### Soal 4
**Manakah tampilan yang merepresentasikan kode HTML ini?**
```html
<p style="color: red;">Hello world</p>
<style type="text/css"> p {color: blue;} </style>
```
Pilihan: (a) "Hello world" **merah**, (b) **biru**, (c) "Hello" merah + "world" biru, (d) semua salah.

**✅ Jawaban: a** (Hello world berwarna merah).

**Alasan:**
- Ada dua aturan untuk `<p>` yang sama: **inline style** `color: red` dan aturan di `<style>`, yaitu `p {color: blue}`.
- **Inline style punya specificity 1000**, mengalahkan selector elemen `p` (specificity 1).
- Posisi `<style>` yang ditulis setelah `<p>` **tidak berpengaruh**, karena urutan kemunculan baru dipakai jika specificity-nya sama.

---

### Soal 5
**Manakah tampilan yang merepresentasikan kode HTML ini?**
```html
<p id="myId" class="myClass">Hello world</p>
<style type="text/css">
#myId {color: blue;}
.myClass {color: red;}
</style>
```
Pilihan: (a) merah, (b) biru, (c) "Hello" merah + "world" biru, (d) semua salah.

**✅ Jawaban: b** (Hello world berwarna biru). ✅ **Terverifikasi**.

**Alasan:**
- `#myId` adalah **ID selector** (specificity **100**), sedangkan `.myClass` adalah **class selector** (specificity **10**).
- ID lebih spesifik, jadi warna **biru** yang menang. Urutan penulisan tidak berpengaruh karena specificity-nya berbeda.

🔎 *Typo di bundel:* di bundel tertulis `<p sid="myId" ...>`. Kalau benar-benar ditulis `sid` (bukan `id`), selector `#myId` **tidak cocok** dengan elemen mana pun, sehingga hanya `.myClass` yang berlaku dan hasilnya **merah**. Ini sudah aku cek di Chromium. Maksud soal aslinya hampir pasti `id`, jadi jawabannya **biru**.

---

### Soal 6
**Manakah tampilan yang merepresentasikan kode HTML ini?**
```html
<h1>This <span class="myclass">headline is <em>very</em> important</span><em>me</em></h1>
<style type="text/css">
h1 {color: red}
em {color: blue}
span em {color: green}
</style>
```
Pilihan (berupa gambar):
- a. Semua merah, "very" biru, "me" biru
- b. Semua merah
- c. Semua merah, **"very" hijau**, **"me" biru**
- d. Semua merah, "very" biru, "me" hijau

**✅ Jawaban: c.** ✅ **Terverifikasi** (h1 & span: merah, "very": hijau, "me": biru).

**Alasan:**
- Teks biasa di `h1` dan `span` → **merah**. Aturan `h1 {color: red}` berlaku, dan `span` **mewarisi (inherit)** warna merah dari `h1`.
- `<em>very</em>` berada **di dalam `span`**, jadi dua aturan cocok:
  - `em` → specificity 1 → biru
  - `span em` → specificity 2 (dua elemen) → **hijau** ✔ (lebih spesifik, menang)
- `<em>me</em>` berada **di luar `span`**, jadi hanya `em {color: blue}` yang cocok → **biru**.

---

### Soal 7
**Berikut ini adalah sebuah CSS style yang dapat memunculkan tampilan berikut ini:**
*(Gambar: sebuah kotak bergaris tepi hitam tipis. Di dalamnya tulisan "Hello world" dengan jarak lega antara teks dan garis tepi.)*
- a. `div {border: line black 1 px; padding: 20px;}`
- b. `div {border: line black 1 px; margin: 20px;}`
- c. `div {border: solid black 1 px; padding: 20px;}`
- d. `div {border: solid black 1 px; margin: 20px;}`

**✅ Jawaban: c.**

**Alasan:**
- **Jarak antara teks dan garis tepi** adalah **padding** (ruang di *dalam* border). **Margin** adalah ruang di *luar* border. Kalau memakai margin, teks akan menempel pada garis tepi. Jadi b dan d salah.
- Gaya garis yang valid adalah **`solid`**. **`line` bukan nilai `border-style` yang valid** (yang valid misalnya solid, dashed, dotted, double). Jadi a dan b salah.

🔎 *Catatan teknis:* penulisan `1 px` (pakai spasi) sebenarnya **tidak valid** di CSS, yang benar `1px`. Aku sudah mengujinya di Chromium: deklarasi `border: solid black 1 px` **diabaikan seluruhnya**, sehingga border tidak muncul. Maksud soalnya jelas opsi c, tapi saat menulis kode sungguhan, tulislah `1px` tanpa spasi.

---

### Soal 8
**Input data berikut ini, saat di-POST ke sebuah file PHP, akan memiliki nilai `array(1,2)`:**
- a. `<input type="hidden" name="data" value="1">` + `<input type="hidden" name="data" value="2">`
- b. `<input type="hidden" name="data[0]" value="1">` + `<input type="hidden" name="data[1]" value="2">`
- c. `<input type="hidden" name="data[]" value="1">` + `<input type="hidden" name="data[]" value="2">`
- d. Tidak ada yang benar

**✅ Jawaban: b dan c.** ✅ **Terverifikasi** dengan `parse_str` PHP 8.3.

**Alasan:**
- **a:** dua field dengan nama sama **tanpa `[]`** → PHP hanya menyimpan **yang terakhir**: `$_POST['data'] = "2"`, bukan array.
- **b:** `data[0]` dan `data[1]` → PHP membentuk array dengan kunci 0 dan 1: `["1", "2"]`.
- **c:** `data[]` → PHP otomatis menambahkan elemen dengan kunci 0, 1, …: `["1", "2"]`.
- Catatan kecil: nilai dari form selalu berupa **string** (`"1"`, `"2"`), bukan integer. Tapi bentuk array-nya sama dengan `array(1,2)`.

🔎 *Typo di bundel:* `value="1'` memakai tanda kutip yang tidak cocok. Aku menganggap maksudnya `value="1"`.

---

### Soal 9
**Berikut ini adalah falsy values pada PHP:**
- a. `""`
- b. `[]`
- c. `"0"`
- d. `[0]`

**✅ Jawaban: a, b, c.** ✅ **Terverifikasi**.

**Alasan:** Di PHP, nilai yang dianggap `false` adalah `false`, `0`, `0.0`, `""` (string kosong), **`"0"` (string berisi nol)**, **array kosong `[]`**, dan `null`.
- `[0]` adalah array yang **berisi satu elemen**, jadi **truthy**, meskipun elemennya 0.
- ⚠️ **Beda dengan JavaScript:** di JS, `"0"` itu **truthy** dan `[]` juga **truthy**. Bandingkan dengan soal 13.

---

### Soal 10
**Apa hasil eksekusi dari kode berikut?**
```php
<?php
    $a[] = 1;
    $a[2] = 2;
    $a[3] = 3;
    $a[] = 4;
    $a[5] = 5;
    print_r($a);
?>
```
- a. `Array ([0] => 1; [1] => 4; [2] => 2; [3] => 3; [5] => 5)`
- b. `Array ([0] => 1; [1] => 4; [2] => 2; [3] => 3; [4] => null; [5] => 5)`
- c. `Array ([0] => 1; [2] => 2; [3] => 3; [4] => 4; [5] => 5)`
- d. `Array ([0] => 1; [1] => null; [2] => 2; [3] => 3; [4] => 4; [5] => 5)`

**✅ Jawaban: c.** ✅ **Terverifikasi** (output: `[0] => 1 [2] => 2 [3] => 3 [4] => 4 [5] => 5`).

**Alasan:** Ingat, array PHP adalah **ordered map**.
1. `$a[] = 1;` → array masih kosong, jadi kuncinya **0**.
2. `$a[2] = 2;` → kunci **2** (kunci 1 tidak pernah dibuat, sehingga **tidak ada**, bukan bernilai `null`).
3. `$a[3] = 3;` → kunci **3**.
4. `$a[] = 4;` → kunci otomatis = **(kunci integer terbesar sejauh ini) + 1** = 3 + 1 = **4**. Jadi bukan "mengisi lubang" di kunci 1.
5. `$a[5] = 5;` → kunci **5**.

---

### Soal 11
**Diberikan `$person = array("name" => "Fulan")`. Cara yang benar untuk memeriksa apakah umur `$person` kurang dari 21:**
- a. `if ($person["age"] < 21)`
- b. `if (!empty($person["age"]) && $person["age"] < 21)`
- c. `if (!isset($person["age"]) && $person["age"] < 21)`
- d. Semua salah

**✅ Jawaban: b.** ✅ **Terverifikasi**.

**Alasan:**
- **a:** key `"age"` tidak ada, jadi muncul **Warning "Undefined array key"**. Nilainya dianggap `null`, dan `null < 21` bernilai **`true`**. Hasilnya **salah**: orang tanpa data umur dianggap berumur di bawah 21.
- **b:** `empty()` mengecek "ada dan tidak kosong" **tanpa warning**. Karena `age` tidak ada, `!empty(...)` bernilai false. Ekspresi `&&` berhenti di situ (*short-circuit*), sehingga `$person["age"]` tidak pernah diakses. ✔
- **c:** logikanya **terbalik**. `!isset` bernilai true justru ketika age **tidak ada**, lalu kode tetap mengakses `$person["age"]` → warning.
- Catatan: b punya celah kecil. Jika `age = 0`, `empty(0)` bernilai true sehingga dianggap tidak ada. Versi paling tepat adalah `isset($person["age"]) && $person["age"] < 21`. Tapi di antara pilihan yang ada, **b** yang benar.

---

### Soal 12
**Apakah hasil eksekusi kode berikut pada bahasa PHP: `echo 1 + "20 little birds";`**
- a. 21
- b. 21 little birds
- c. 120 little birds
- d. Terjadi TypeError

**✅ Jawaban: a (21).** ✅ **Terverifikasi** (PHP 8.3 mencetak `21` disertai *Warning: A non-numeric value encountered*).

**Alasan:**
- Di PHP, operator `+` **selalu operasi aritmatika**. Penggabungan string di PHP memakai titik (`.`), bukan `+`.
- `"20 little birds"` adalah **leading-numeric string** (diawali angka). PHP mengambil angka di depannya, yaitu **20**, lalu 1 + 20 = **21**.
- Tidak terjadi TypeError. PHP 7 memberi *Notice*, PHP 8 memberi *Warning*, tapi program tetap jalan. TypeError baru muncul untuk kasus lain, misalnya array + int.

---

### Soal 13
**Berikut ini adalah falsy values pada JavaScript:**
- a. `0`
- b. `NaN`
- c. `undefined`
- d. `"0"`

**✅ Jawaban: a, b, c.** ✅ **Terverifikasi** di Node.js.

**Alasan:** Nilai falsy di JS: `0`, `-0`, `0n`, `false`, `null`, `undefined`, `NaN`, `""`.
`"0"` adalah **string yang tidak kosong**, jadi **truthy**. Inilah bedanya dengan PHP (soal 9).

---

### Soal 14
**Apakah nilai dari ekspresi berikut pada JavaScript: `1 + 20 + "little birds";`**
- a. 21
- b. 21 little birds
- c. 120 little birds
- d. NaN

**✅ Jawaban: b.** ✅ **Terverifikasi** (hasil persisnya `"21little birds"`).

**Alasan:**
- Operator `+` dievaluasi **dari kiri ke kanan**:
  1. `1 + 20` → keduanya number → **21**.
  2. `21 + "little birds"` → salah satunya string, jadi `+` berarti **gabung teks** → `"21little birds"`.
- Kalau string-nya di depan (`"little birds" + 1 + 20`), hasilnya `"little birds120"`.
- Catatan: hasil sebenarnya **tanpa spasi** (`"21little birds"`). Opsi b adalah yang paling mendekati.

---

### Soal 15
**Pada statement `var x = person && person.age`, apa nilai `x` jika `person` bernilai `undefined`?**
- a. undefined
- b. person
- c. person.age
- d. NaN

**✅ Jawaban: a (undefined).** ✅ **Terverifikasi**.

**Alasan:**
- Di JS, `A && B` **mengembalikan operand pertama yang falsy**. Kalau semuanya truthy, yang dikembalikan operand terakhir.
- `person` bernilai `undefined` (falsy), jadi ekspresi berhenti dan mengembalikan **`undefined`**. Bagian `person.age` **tidak dieksekusi**, sehingga tidak ada error "Cannot read properties of undefined".
- Ini pola klasik untuk akses aman. Versi modernnya: `person?.age`.

---

### Soal 16
**Diberikan sebuah kelas `Person` dalam JavaScript. Bagaimana cara membuat kelas `Student` yang merupakan turunan `Person`?**
- a. `class Student extends Person;`
- b. `Student.prototype = new Person();`
- c. `Student.prototype = Person;`
- d. Semua salah

**✅ Jawaban: b.** ✅ **Terverifikasi** (`new Student() instanceof Person` → true; opsi a → `SyntaxError: Unexpected token ';'`).

**Alasan:**
- **b** adalah cara klasik pewarisan **berbasis prototype**: objek `Student` akan "mewarisi" property dan method dari sebuah instance `Person` melalui rantai prototype.
- **a** idenya benar, tapi **sintaksnya salah**. Deklarasi class wajib punya body: `class Student extends Person {}`. Dengan titik koma tanpa `{}`, hasilnya SyntaxError.
- **c** salah, karena prototype di-set ke **fungsi constructor `Person` itu sendiri**, bukan ke objek `Person`. Method instance tidak akan terwariskan.

---

### Soal 17
**Diberikan kode HTML berikut. Bagaimana cara mendapatkan elemen `<i>simple</i>`?**
```html
<html>
<body>
<h1>An HTML Document</h1>
<p>This is a <i>simple</i> but not <i>so simple</i> document.</p>
</body>
</html>
```
- a. `document.getElementsByTagName("i")[0]`
- b. `document.body.lastElementChild.firstElementChild`
- c. `document.getElementById("i")`
- d. `document.body.lastElementChild.lastElementChild`

**✅ Jawaban: a dan b.**

**Alasan:**
- **a:** `getElementsByTagName("i")` mengembalikan semua `<i>` sesuai urutan dokumen, dan indeks `[0]` adalah yang pertama, yaitu `simple` ✔.
- **b:** `document.body.lastElementChild` = elemen anak terakhir `body` = **`<p>`**. Lalu `.firstElementChild` = anak elemen pertama dari `<p>` = **`<i>simple</i>`** ✔. (`*ElementChild` mengabaikan node teks, jadi spasi dan teks tidak mengganggu.)
- **c:** `getElementById("i")` mencari elemen dengan **`id="i"`**, bukan tag `i`. Hasilnya `null`.
- **d:** `.lastElementChild` dari `<p>` adalah **`<i>so simple</i>`**, bukan yang diminta.

---

### Soal 18
**Method mana pada Java Servlet yang bisa digunakan untuk membersihkan session user saat logout?**
- a. `session.setAttribute(key, object)`
- b. `session.removeAttribute(key)`
- c. `session.invalidate()`
- d. `session.logout()`

**✅ Jawaban: c.**

**Alasan:**
- `session.invalidate()` **menghapus seluruh session**, termasuk semua atributnya. Session ID lama juga tidak berlaku lagi. Inilah yang tepat untuk logout.
- `removeAttribute(key)` hanya menghapus **satu** atribut, sedangkan session-nya tetap hidup.
- `setAttribute` justru **menambah** data.
- `session.logout()` **tidak ada** di `HttpSession`. Yang ada adalah `request.logout()`, dan itu pun untuk container-managed authentication, bukan untuk membersihkan session.

---

### Soal 19
**Di servlet didefinisikan bean `person` yang di-set dengan `request.setAttribute("key", person)`. Bagaimana menampilkan property `myProperty` dari bean tersebut di halaman JSP?**
- a. `<%=person.myProperty%>`
- b. `${key.myProperty}`
- c. `<jsp:useBean id="key" …/><jsp:getProperty name="key" property="myProperty"/>`
- d. `${person.myProperty}`

**✅ Jawaban: b** (dan **c** juga benar asalkan `useBean` memakai `scope="request"`).

**Alasan:**
- Atribut disimpan dengan **nama `"key"`**. Nama variabel Java (`person`) di servlet **tidak dikenal** di JSP.
- **b:** Expression Language `${key.myProperty}` mencari atribut bernama `key` di page, request, session, lalu application scope, kemudian memanggil getter `getMyProperty()` ✔.
- **c:** `<jsp:useBean id="key" scope="request" class="..."/>` akan **menemukan** atribut `key` yang sudah ada di request, lalu `<jsp:getProperty>` menampilkannya ✔. Tapi kalau `scope` tidak ditulis, default-nya `page`, sehingga JSP akan **membuat bean baru yang kosong**. Itulah sebabnya c bergantung pada isi "…".
- **a:** dalam scriptlet tidak ada variabel `person`, dan `person.myProperty` adalah akses field (biasanya private), bukan getter. Salah.
- **d:** tidak ada atribut bernama `person`, jadi hasilnya kosong.

---

### Soal 20
**Berikut ini adalah cara untuk set header Content-Type pada program JSP/Servlet:**
- a. `request.sendHeader("Content-Type", "application/json")`
- b. `response.addHeader("Content-Type", "application/json")`
- c. `<%@ page contentType="application/json" %>`
- d. `application.addHeader("Content-Type", "application/json")`

**✅ Jawaban: b dan c.**

**Alasan:**
- Header dikirim **dalam response**, jadi harus lewat objek **`response`**. `response.addHeader(...)` bisa (cara yang lebih umum adalah `response.setContentType("application/json")`) ✔.
- `<%@ page contentType="..." %>` adalah **page directive JSP** untuk mengatur Content-Type halaman ✔.
- `request.sendHeader` tidak ada. Selain itu, request adalah data yang **masuk**, bukan yang dikirim.
- `application` (ServletContext) tidak punya `addHeader`.

---

### Soal 21
**Manakah pernyataan yang benar?**
- a. Pemrograman aplikasi berbasis Internet selalu menggunakan arsitektur sistem terdistribusi
- b. Aplikasi berbasis Internet selalu dijalankan menggunakan web browser
- c. Pemrograman aplikasi berbasis Internet selalu menggunakan protokol Web
- d. Aplikasi berbasis Internet menggunakan internet sebagai sumber data dan informasi

**✅ Jawaban: a.**

**Alasan** (slide 01b, *Main Concept*):
- Internet App adalah aplikasi yang **tersebar di berbagai tempat** dan memakai Internet sebagai alat komunikasi. Kata kuncinya: **Distributed System** dan **Communication via Internet**. Jadi a benar.
- b salah: aplikasi Internet tidak harus memakai browser (contoh: FileZilla/FTP, WhatsApp desktop).
- c salah: aplikasi Internet boleh memakai protokol apa saja (FTP, SMTP, dll), bahkan **protokol buatan sendiri**.
- d salah: Internet dipakai sebagai **sarana komunikasi**, bukan sebagai "sumber data".

---

### Soal 22
**Ciri-ciri dari Web-based Application:**
- a. Berjalan di cloud
- b. Hanya bisa menggunakan protokol Web
- c. Selalu menggunakan browser sebagai client
- d. Menggunakan paradigma RESTful

**✅ Jawaban: b.**

**Alasan** (slide 01b, *Internet-based vs Web-based*):
- Web-based app **memakai HTTP** (protokol Web) dan berkomunikasi dengan client **melalui web server**. Jadi b benar.
- c salah: slide menyebut web app "**commonly** runs in web browser", artinya *umumnya*, bukan *selalu*. Client bisa juga aplikasi mobile atau program lain yang memakai HTTP.
- a salah: tidak harus di cloud; bisa juga di server kampus atau localhost.
- d salah: REST hanya salah satu gaya desain dan tidak wajib.

---

### Soal 23
**Pernyataan berikut terkait dengan Protokol HTTP:**
- a. Hanya bisa mengirimkan data dalam bentuk tekstual
- b. Hanya dapat digunakan pada arsitektur Client-Server
- c. Merupakan Application Protocol dari WebApp
- d. Merupakan Transport Protocol pada Web Service

**✅ Jawaban: c** (d juga dapat diterima dalam konteks Web Service).

**Alasan:**
- **c:** HTTP adalah **application layer protocol** yang dipakai Web App ✔.
- **a** salah: body HTTP bisa berisi **data biner** (gambar, file upload dengan `multipart/form-data`), dan HTTP/2 bahkan protokol biner.
- **b** salah karena kata "hanya": HTTP memang berpola request–response, tapi bisa dipakai di sistem peer-to-peer, di mana setiap node menjalankan HTTP client sekaligus HTTP server.
- **d** (diperdebatkan): secara lapisan jaringan, HTTP **bukan** transport protocol (transport-nya TCP). Tapi dalam konteks **Web Service (SOAP)**, HTTP lazim disebut sebagai "**transport**" untuk membawa pesan SOAP/XML. Slide 01b sendiri menulis "*Web Programming often uses HTTP as **transport protocol** and HTML/XML as message format*". Kalau soal ini diambil dari sudut pandang slide, d bisa dianggap benar.

---

### Soal 24
**AJAX merupakan:**
- a. Bisa diimplementasikan pada sisi server
- b. Hanya dapat mempertukarkan XML sesuai dengan namanya
- c. Tidak semua browser dapat menjalankan AJAX
- d. Apabila AJAX request yang dikerjakan terlalu lama akan menyebabkan halaman web tersebut tidak responsif

**✅ Jawaban: c.**

**Alasan:**
- **c** ✔: AJAX butuh JavaScript dan objek `XMLHttpRequest`/Fetch. Browser lama (misalnya IE versi awal tanpa XHR native), browser teks seperti **lynx**, atau browser dengan **JavaScript dimatikan** tidak bisa menjalankan AJAX. (Slide hanya menyebut "*all **modern** browsers support XMLHttpRequest*".)
- **a** salah: AJAX adalah teknik **di sisi client** (browser). Server hanya melayani request HTTP biasa.
- **b** salah: data yang dipertukarkan bisa **teks, HTML, JSON**, dll.
- **d** salah: AJAX bersifat **asinkron**, jadi selama menunggu response halaman **tetap responsif**. Halaman baru membeku jika memakai **XHR sinkron** (`async=false`), dan cara itu memang tidak direkomendasikan.

---

### Soal 25
**Pernyataan terkait HTML 5:**
- a. Standard yang sudah final dan published
- b. Geolocation hanya dapat berjalan pada device yang memiliki GPS
- c. `<video>` berfungsi untuk memutar video tanpa tombol control
- d. Memiliki sejumlah teknologi yang digunakan pada offline Web Application

**✅ Jawaban: d** (a diperdebatkan, lihat alasan).

**Alasan:**
- **d** ✔: HTML5 menyediakan **Web Storage (localStorage/sessionStorage)**, Application Cache (sekarang diganti **Service Worker** + Cache API), dan IndexedDB untuk aplikasi **offline** (slide 02b, *Offline Support for Web App*).
- **b** salah: Geolocation API bisa menentukan lokasi dari **alamat IP, WiFi, atau jaringan seluler**, tidak harus GPS.
- **c** salah: fungsi `<video>` adalah **menyisipkan video tanpa plug-in**. Tombol kontrol bawaan bisa ditampilkan dengan atribut `controls` (`<video src="a.mp4" controls>`).
- **a** (diperdebatkan): W3C memang pernah mempublikasikan HTML5 sebagai **W3C Recommendation (2014)**, dan slide menyebut HTML5 sebagai "*the fifth and last major HTML version that is a W3C recommendation*". Tapi sekarang HTML adalah **"living standard"** di WHATWG yang terus diperbarui (slide 02b: "*It is a living document*"). Jadi jawaban yang paling aman adalah **d**. Kalau dosen memakai sudut pandang "W3C Recommendation 2014", a juga bisa dianggap benar.

---

### Soal 26
**Berikut adalah tempat menyimpan state pada sebuah Web App yang berada pada sisi browser:**
- a. Cookie
- b. Local Storage
- c. Web SQL
- d. Session

**✅ Jawaban: a, b, c.**

**Alasan:**
- **Cookie** disimpan di browser (lalu ikut terkirim ke server di setiap request).
- **Local Storage** adalah Web Storage di browser.
- **Web SQL** adalah database SQL di browser. Sekarang sudah **deprecated** dan digantikan IndexedDB, tapi letaknya tetap di sisi browser.
- **Session** (server-side session) **disimpan di server**. Browser hanya memegang **session ID**-nya. *(Jangan tertukar dengan `sessionStorage`, yang memang ada di browser. Opsi "Session" di sini maksudnya session server.)*

---

### Soal 27
**Berikut adalah pernyataan terkait ECMAScript Type System:**
- a. Nilai default sebuah variabel adalah NULL
- b. Dynamic Typed
- c. Tidak mengharuskan type casting karena interpreter melakukan juggling dari tipe yang mungkin
- d. Loosely Typed

**✅ Jawaban: b, c, d.**

**Alasan** (slide 03 JavaScript):
- **a** salah: nilai default variabel adalah **`undefined`**, bukan `null`.
- **b** ✔: tipe tidak perlu ditentukan di awal dan tidak dicek saat compile (*dynamically typed*).
- **c** ✔: konversi tipe terjadi **otomatis** (*type coercion*), misalnya `"7" * "4"` menghasilkan 28.
- **d** ✔: tipe apa pun bisa disimpan di variabel atau dikirim ke fungsi mana pun (*loosely typed*).

---

### Soal 28
**Persamaan JavaScript dan Java:**
- a. Dapat berjalan di atas browser
- b. Membutuhkan interpreter untuk menjalankan program
- c. Object-oriented language
- d. Merupakan Dynamic Typed Language

**✅ Jawaban: a, b, c.**

**Alasan:**
- **a** ✔: JavaScript berjalan **langsung** di browser. Java juga bisa berjalan di browser lewat **Java Applet** (plug-in), seperti disebut di slide 01b *Web Programming*. Teknologi applet memang sudah usang, tapi secara konsep keduanya bisa.
- **b** ✔: JavaScript butuh **JS engine/interpreter** (V8, SpiderMonkey). Java di-compile ke **bytecode** yang lalu dijalankan **JVM** (interpreter + JIT). Keduanya butuh "mesin" perantara.
- **c** ✔: keduanya berorientasi objek. JavaScript memakai **prototype**, Java memakai **class**.
- **d** salah: **Java statically typed** (tipe dicek saat compile), sedangkan JavaScript dynamically typed.

---

### Soal 29
**Pernyataan terkait CGI:**
- a. CGI is an old technique to build dynamic website
- b. Cara alternatif menjalankan PHP program
- c. Bersifat stateless
- d. Hanya bisa menggunakan GET dan POST HTTP Method

**✅ Jawaban: a, b, c.**

**Alasan:**
- **a** ✔: CGI adalah teknik lama (sejak awal 1990-an). Web server menjalankan program, lalu output-nya dijadikan HTTP response (slide 01b).
- **b** ✔: PHP bisa dijalankan sebagai **CGI** (`php-cgi`), sebagai alternatif dari modul Apache (`mod_php`) atau PHP-FPM.
- **c** ✔: setiap request menjalankan **proses baru** yang langsung berakhir setelah response dikirim, sehingga tidak ada state yang tersimpan antar-request.
- **d** salah: CGI menerima method apa pun lewat variabel lingkungan `REQUEST_METHOD` (GET, POST, PUT, DELETE, HEAD, …).

---

### Soal 30
**Pernyataan terkait PHP Type System:**
- a. Dynamic typing dengan cara type juggling
- b. NULL adalah sebuah nilai yang diberikan pada sebuah variabel yang belum di-assign
- c. Variabel harus dideklarasikan terlebih dahulu
- d. Variabel hanya dapat menjadi masukan dalam sebuah fungsi

**✅ Jawaban: a dan b.**

**Alasan** (slide 04a):
- **a** ✔: *Type juggling*, yaitu tipe variabel ditentukan oleh konteks pemakaiannya.
- **b** ✔: variabel dianggap null jika **belum pernah diberi nilai**, diisi `NULL`, atau sudah di-`unset()`.
- **c** salah: PHP **tidak butuh deklarasi**. Cukup langsung `$x = 5;`.
- **d** salah: pernyataan ini tidak bermakna. Variabel bisa dipakai di mana saja, tidak hanya sebagai parameter fungsi.

---

### Soal 31
**Interaksi antara JSP dan Java mungkin dilakukan melalui:**
- a. In-line Java codes in JSP
- b. Melalui POJO Java Bean
- c. Melalui Enterprise Java Bean
- d. Melalui JSP Expression Language

**✅ Jawaban: a, b, c, d** (semuanya mungkin).

**Alasan:**
- **a** ✔: **scriptlet** `<% ... %>` dan expression `<%= ... %>` berisi kode Java langsung.
- **b** ✔: `<jsp:useBean>`, `<jsp:getProperty>`, dan `<jsp:setProperty>` untuk JavaBean (POJO).
- **c** ✔: JSP bisa memanggil **EJB** (biasanya lewat lookup JNDI di scriptlet, atau melalui servlet/controller yang meneruskannya). Arsitektur ini umum di aplikasi Java EE.
- **d** ✔: **EL** `${...}` mengakses objek dan bean Java (atribut di scope, property lewat getter).

---

### Soal 32
**Apa fungsi JSTL?**
- a. Mendefinisikan tags yang baru
- b. Kumpulan tag-tag HTML yang didukung Java Web
- c. Mendefinisikan fungsionalitas dasar yang sering dipakai dalam JSP
- d. Menentukan utiliti yang umum dibutuhkan oleh sebuah Java Web

**✅ Jawaban: c.**

**Alasan:**
- **JSTL (JSP Standard Tag Library)** adalah kumpulan tag standar yang **membungkus fungsionalitas umum di halaman JSP**: perulangan (`<c:forEach>`), kondisi (`<c:if>`, `<c:choose>`), format angka/tanggal (`fmt`), XML, dan SQL. Tujuannya mengurangi scriptlet.
- **a** salah: membuat tag baru adalah fungsi **custom tag library**, bukan JSTL.
- **b** salah: JSTL bukan tag HTML.
- **d** terlalu umum: JSTL khusus untuk **halaman JSP**, bukan utilitas untuk seluruh aplikasi Java Web.

---

### Soal 33
**Pernyataan yang benar terkait Servlet:**
- a. Umumnya digunakan sebagai Business Tier dalam Enterprise Web
- b. Hanya bisa berjalan di Web
- c. Process-based execution
- d. Asynchronous execution

**✅ Jawaban: b.**

**Alasan:**
- **b** ✔: servlet hanya bisa berjalan di dalam **web container/servlet container** (Tomcat, Jetty, dll) dan melayani request web.
- **a** salah: servlet berada di **web/presentation tier** (sebagai controller). Business tier dalam Java EE biasanya **EJB**.
- **c** salah: servlet memakai model **thread-based** (satu thread per request dalam satu proses JVM), bukan process-per-request seperti CGI. Inilah keunggulan servlet dibanding CGI.
- **d** salah sebagai karakteristik umum: secara default servlet **sinkron** (thread menunggu sampai response selesai). Dukungan asinkron (`AsyncContext`) baru ditambahkan di **Servlet 3.0** sebagai fitur opsional.

---

### Soal 34
**State handling yang bisa terjadi di sebuah Java Web App:**
- a. Session
- b. Page
- c. Request
- d. Servlet

**✅ Jawaban: a, b, c.**

**Alasan:** Java Web punya **4 scope** untuk menyimpan atribut/state:
1. **page** (`pageContext`): hanya selama satu halaman JSP dieksekusi.
2. **request**: selama satu request (termasuk saat di-*forward*).
3. **session**: selama sesi user.
4. **application** (`ServletContext`): global untuk seluruh aplikasi.

"Servlet" **bukan** sebuah scope.

---

### Soal 35
**Teknologi yang ekuivalen dengan PHP dalam pembangunan Java Web Application:**
- a. Servlet
- b. JSF
- c. JSP
- d. JScript

**✅ Jawaban: c (JSP).**

**Alasan:**
- PHP dan **JSP (JavaServer Pages)** sama-sama **menyisipkan kode server di dalam halaman HTML** (`<?php ... ?>` dibandingkan `<% ... %>`). Lihat juga Tabel 8.1 di slide 01b: `.php`, `.asp`, `.jsp` sama-sama *auxiliary processing modules/scripting*.
- Servlet adalah kelas Java murni yang menulis HTML lewat kode, jadi lebih mirip CGI.
- JSF adalah framework komponen UI.
- JScript adalah varian JavaScript buatan Microsoft yang berjalan di sisi client.

---

### Soal 36
**Persamaan JavaScript dan PHP secara bahasa:**
- a. Full feature OO-language
- b. Merupakan dynamic language
- c. Array diimplementasikan secara Map
- d. Interpreted Language

**✅ Jawaban: b, c, d.**

**Alasan:**
- **b** ✔: keduanya bertipe dinamis (tipe ditentukan saat runtime).
- **d** ✔: keduanya dijalankan oleh interpreter/engine (dengan JIT di versi modern: V8 dan PHP 8).
- **c** ✔: array PHP adalah **ordered map** (slide 04a). Array JavaScript sebenarnya adalah **objek**, yaitu kumpulan pasangan key→value dengan key berupa indeks. Contohnya, `a[100] = 1` pada array kosong tidak membuat 100 elemen sungguhan. Secara konsep keduanya berupa map.
- **a** salah untuk JavaScript: JS memakai **prototype** (slide: "*Prototypes over class-inheritance; class keyword is syntactic sugar*"), dan dulu tidak punya fitur OOP klasik seperti access modifier dan interface. PHP 5+ memang punya model objek penuh.

---

### Soal 37
**Karakteristik dari aplikasi Internet:**
- a. Paralel
- b. Client-server
- c. Asynchronous
- d. Exception handling

**✅ Jawaban: a dan d.**

**Alasan** (slide 01b, *Characteristics*): karakteristik aplikasi Internet adalah **Concurrency**, **Synchronize**, dan **Exception Handling**.
- **a** ✔: *Paralel* adalah padanan **concurrency** (menangani banyak tugas atau client sekaligus).
- **d** ✔: *Exception handling*, yaitu kegagalan pada satu user tidak boleh mengganggu user lain.
- **b** salah: client-server adalah **kelas arsitektur** sistem terdistribusi, bukan karakteristik. Aplikasi Internet juga bisa peer-to-peer.
- **c** salah: yang ada di slide adalah **Synchronize** (koordinasi aktivitas & timing), bukan asynchronous.

---

### Soal 38
**Fungsi utama web server:**
- a. Menerima request pengguna web
- b. Menyajikan dokumen
- c. Menyimpan dokumen
- d. Menjalankan aplikasi

**✅ Jawaban: a, b, c.**

**Alasan** (slide 01b, *Web server*: "*a software, runs in a server, **store web document so it can be accessed** by users across the Internet*"):
- Web server **menerima request**, **menyimpan** dokumen web, dan **menyajikannya** ke pengguna.
- **d**: menjalankan aplikasi adalah fungsi **Web *App* Server**, hasil evolusi web server (slide: "*Web Server → Web App Server*"). Karena itu d bukan fungsi utama web server klasik.

---

### Soal 39
**Perbedaan GET dan POST:**
- a. Cache-ability
- b. Security
- c. Tipe data yang bisa dikirimkan
- d. Length of data

**✅ Jawaban: a, b, c, d** (semuanya).

**Alasan** (slide 02a, *GET vs POST*):
| Aspek | GET | POST |
|---|---|---|
| **Cache-ability** | Bisa di-cache | Default tidak di-cache |
| **Security** | Data **terlihat di URL**, history, dan bookmark | Data di body, tidak tampil di URL |
| **Tipe data** | Hanya karakter **ASCII** | **Biner** juga boleh |
| **Panjang data** | **Dibatasi** (panjang URL) | Tidak ada batasan |

(Catatan: "security" di sini maksudnya visibilitas data. Enkripsi sesungguhnya tetap butuh HTTPS.)

---

### Soal 40
**Berikut adalah HTTP Method:**
- a. HEAD
- b. PATCH
- c. CONNECT
- d. BODY

**✅ Jawaban: a, b, c.**

**Alasan:** Method HTTP adalah GET, POST, HEAD, PUT, DELETE, TRACE, OPTIONS, CONNECT, PATCH, dan QUERY (baru). **BODY bukan method**; body adalah *bagian dari pesan* HTTP.

---
---

## 🔑 Kunci Cepat UTS 2021/2022

Format: urutan jawaban per pernyataan (B = Benar, S = Salah; V = Valid, E = Error). ⚠️ = berbeda dengan kunci bundel.

| No | Jawaban | No | Jawaban |
|---|---|---|---|
| 2 | B, B, B, B | 17 | S, B, S, S |
| 3 | B, B, S, B | 18 | Ada, Tidak ada, Ada, Ada |
| 4 | B, B, B, B | 19 | V, E, E, V |
| 5 | B, B, B, B | 20 | V, V, V, E |
| 6 | B, B, S, B | 21 | Tidak, Perlu, Perlu*, Tidak |
| 7 | S, S, S, S | 22 | S, B, **B ⚠️**, S |
| 8 | B, S, **B ⚠️**, B | 23 | B, B, S, S |
| 9 | S, B, B, B | 24 | S, B, S, B |
| 10 | S, B, S, S | 25 | S, B, S, S |
| 11 | S, B, B, B | 26 | B, S, B, B |
| 12 | B, B, S, S | 27 | S, B, B, S |
| 13 | B, S*, B, S* | 28 | B, S, B, B |
| 14 | B, S, B, S | 29 | B, B, S, B |
| 15 | B, B, B, **S ⚠️** | 30 | **B ⚠️**, B, B, B |
| 16 | B, **S ⚠️**, B, S | | |

\* Lihat pembahasan (jawaban bergantung pada interpretasi).
*Soal nomor 1 tidak ada di bundel; nomor pertama yang tercantum adalah nomor 2.*

---

## UTS SEMESTER I – 2021/2022

*Format: setiap soal berisi 4 pernyataan yang dijawab Benar/Salah (atau Valid/Error, Ada/Tidak Ada, Perlu/Tidak Perlu).*

> 🔎 **Penomoran:** di bundel, nomor soal ditulis pada bilah di atas setiap soal. Soal pertama (tentang protokol HTTP) tidak bernomor dan diikuti bilah "3", sehingga soal pertama adalah **nomor 2**. Soal nomor 1 tidak tercantum di bundel.

---

### Soal 2 — Pernyataan terkait Protokol HTTP
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Menggunakan paradigma request dan response | **B** | B | Satu transaksi HTTP terdiri dari satu request dari client dan satu response dari server. |
| Struktur request dan response mengandung sejumlah baris header | **B** | B | Strukturnya: start line, lalu beberapa baris header, baris kosong, dan body (opsional). |
| Menggunakan protokol TCP sebagai layer transport | **B** | B | HTTP/1.x dan HTTP/2 berjalan di atas TCP. (Catatan: HTTP/3 memakai QUIC di atas UDP, tapi di tingkat materi ini jawabannya B.) |
| Stateless protocol | **B** | B | Server tidak menyimpan state antar-request di level protokol. |

### Soal 3 — Header yang bisa digunakan di HTTP **request**
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Content-Length | **B** | B | Dipakai saat request punya body, misalnya POST/PUT. |
| Content-Type | **B** | B | Menyatakan format body request, misalnya `application/x-www-form-urlencoded` atau `application/json`. |
| Set-Cookie | **S** | S | `Set-Cookie` hanya dikirim **server di response**. Di request, browser memakai header **`Cookie`**. |
| User-Agent | **B** | B | Identitas browser/client, dikirim di request. |

### Soal 4 — Header yang bisa digunakan di HTTP **response**
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Content-Length | **B** | B | Panjang body response. |
| Content-Type | **B** | B | Jenis isi response (`text/html`, `application/json`, …). |
| Set-Cookie | **B** | B | Server menyuruh browser menyimpan cookie. |
| Last-Modified | **B** | B | Kapan resource terakhir diubah; dipakai untuk caching. |

### Soal 5 — Teknik state handling pada protokol HTTP
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Memanfaatkan hidden variable saat mengirimkan form | **B** | B | Teknik *message passing* (slide 05). |
| Menambahkan parameter pada URL | **B** | B | Query string membawa state di setiap request. |
| Menggunakan cookie | **B** | B | Disimpan browser dan otomatis dikirim di setiap request. |
| Menggunakan session | **B** | B | State disimpan di server, diidentifikasi lewat session ID. |

### Soal 6 — Kelemahan cookie sebagai teknik state handling
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Ukurannya terbatas | **B** | B | Sekitar 4KB per cookie, dan jumlah cookie per domain juga dibatasi. |
| Bisa diubah-ubah oleh user di sisi client/browser | **B** | B | User bisa mengedit atau menghapus cookie lewat DevTools, jadi isinya tidak boleh dipercaya begitu saja. |
| Hanya bisa untuk menyimpan session id | **S** | S | Cookie bisa menyimpan data apa saja (tema, bahasa, isi keranjang, dll), tidak hanya session ID. |
| Masa aktif yang terbatas | **B** | B | Cookie punya waktu kedaluwarsa (Expires/Max-Age). Session cookie hilang saat browser ditutup, browser modern membatasi umur maksimum cookie, dan user bisa menghapusnya kapan saja. |

### Soal 7 — Pernyataan terkait session
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Membutuhkan cookie untuk menyimpan session id | **S** | S | Session ID **bisa** dibawa tanpa cookie, misalnya lewat **URL rewriting** (`;jsessionid=...` di Java, `?PHPSESSID=...` di PHP) atau hidden field. Cookie hanya cara yang paling umum. |
| User bisa mengubah atau memalsukan session id | **S** | S | Session ID adalah token **acak yang panjang dan tidak bisa ditebak**. User memang bisa mengedit nilainya, tapi tidak bisa **memalsukan** session ID yang valid milik orang lain. ID palsu tidak akan ditemukan di server. *(Ancaman nyatanya adalah pencurian ID, misalnya lewat XSS, yang dicegah dengan `HttpOnly` + `Secure`.)* |
| Variabel session disimpan di sisi browser sehingga bisa diubah oleh user | **S** | S | Variabel session disimpan **di server**. Browser hanya memegang ID-nya. |
| Banyaknya variabel session yang bisa dibuat relatif terbatas | **S** | S | Jumlahnya hanya dibatasi memori/penyimpanan server, jauh lebih longgar daripada batas cookie. |

### Soal 8 — Kelebihan Single-Page Application (SPA)
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Pengembangan aplikasi menjadi lebih cepat | **B** | B | Server cukup menyediakan API (JSON) dan tidak perlu me-render HTML per halaman. Frontend dan backend bisa dikembangkan paralel, dan backend yang sama bisa dipakai ulang untuk aplikasi mobile. |
| Lebih baik dalam search engine optimization | **S** | S | Justru **kelemahan** SPA: konten dibuat oleh JavaScript, sehingga crawler mesin pencari bisa melihat halaman kosong (kecuali memakai SSR/pre-rendering). |
| Cocok digunakan untuk aplikasi yang memisahkan front-end back-end | **B** ⚠️ | S | SPA **sangat cocok** untuk arsitektur yang memisahkan frontend (SPA di browser) dan backend (REST/GraphQL API). Pemisahan ini adalah ciri khas SPA, jadi menurutku kunci bundel keliru. |
| Debugging lebih mudah dilakukan | **B** | B | Logika aplikasi ada di browser, sehingga bisa di-debug langsung dengan DevTools (tab Network untuk melihat request API, Console, Sources). Klaim ini lazim disebut sebagai kelebihan SPA. |

### Soal 9 — Teknik untuk mewujudkan SPA
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Session storage | **S** | S | Hanya tempat menyimpan data di browser. Ia tidak mengambil atau memperbarui konten dari server, jadi bukan teknik inti SPA (paling jauh hanya pelengkap). |
| AJAX | **B** | B | Inti SPA: mengambil data tanpa reload halaman. |
| WebSockets | **B** | B | Pembaruan konten real-time dua arah tanpa reload. |
| Server-sent events | **B** | B | Server mendorong (push) data ke halaman tanpa reload. |

### Soal 10 — Pernyataan terkait AJAX
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Sesuai dengan singkatannya, selalu menggunakan XML | **S** | S | Bisa berupa teks, HTML, atau JSON (paling umum). |
| Memanfaatkan objek XMLHttpRequest | **B** | B | Definisi AJAX di slide: XMLHttpRequest + JavaScript/DOM. (Sekarang juga bisa memakai Fetch API.) |
| Hanya bisa menggunakan asynchronous request | **S** | S | Bisa sinkron dengan `open(method, url, false)`, walaupun tidak direkomendasikan. |
| Tidak cocok menggunakan method POST | **S** | S | POST justru disarankan untuk mengirim data besar, input user, atau perubahan data (slide 03b, *GET or POST?*). |

### Soal 11 — AJAX cocok digunakan untuk
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Multimedia (audio/video) player control | **S** | S | Kontrol play/pause dilakukan di browser lewat API `<audio>`/`<video>` dan tidak butuh komunikasi ke server. |
| Form validation | **B** | B | Contohnya cek "username sudah dipakai" ke server tanpa reload (use case di slide). |
| Autocomplete | **B** | B | Use case klasik AJAX. |
| Multi Stage Download | **B** | B | Termasuk pola AJAX di slide 03b: halaman besar dimuat bertahap. |

### Soal 12 — Operasi file sistem yang dapat dilakukan pada aplikasi web
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Server side menuliskan data pada sebuah file di server | **B** | B | Misalnya `fwrite` di PHP atau `os.Create` di Go. |
| Server side membaca file berisi data yang dikirimkan browser | **B** | B | File upload (`$_FILES`, `multipart/form-data`). |
| Browser membaca file di lokal atas input/request server | **S** | S | Demi keamanan, browser **tidak boleh** membaca file lokal atas perintah server. File lokal hanya bisa dibaca jika **user sendiri yang memilihnya** (`<input type="file">`). |
| Browser bisa menulis file pada folder apapun di lokal | **S** | S | Browser berjalan dalam **sandbox**. Penulisan file terbatas pada download atau API dengan izin eksplisit user, tidak bisa ke sembarang folder. |

### Soal 13 — Kegunaan basis data ("best practice") untuk aplikasi web e-commerce
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Menyimpan status transaksi pembelian | **B** | B/S | Data transaksi bersifat **kritis**: harus persisten, konsisten (memakai transaction), dan bisa diaudit. Wajib disimpan di database. |
| Menyimpan barang dipilih shopping cart | **S*** | B/S | Menurut pola klasik, isi keranjang adalah **state sementara** sebelum checkout, sehingga disimpan di **session/cookie/local storage**. *Catatan:* banyak e-commerce modern juga menyimpan keranjang di database supaya tetap ada di semua perangkat. Kalau soal dimaknai seperti itu, jawabannya B. |
| Menyimpan barang katalog yang dijual | **B** | B | Data master yang persisten dan perlu dicari/difilter secara efisien. |
| Menyimpan preferensi dan konfigurasi pengguna | **S*** | S | Preferensi tampilan (tema, bahasa) cukup disimpan di **cookie/localStorage** di sisi client. *Catatan:* preferensi yang terikat akun (alamat, pengaturan notifikasi) wajar disimpan di database. |

### Soal 14 — Protokol umum untuk autentikasi dengan external service
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| SSO | **B** | B | Single Sign-On memakai identity provider eksternal (implementasinya misalnya SAML, CAS, atau OpenID Connect). Contohnya login dengan akun kampus. |
| HTTP Basic Auth | **S** | S | Basic Auth adalah autentikasi langsung ke server itu sendiri (username:password di header), bukan protokol untuk mendelegasikan autentikasi ke layanan eksternal. |
| LDAP | **B** | B | Aplikasi memverifikasi kredensial ke **direktori eksternal** (LDAP/Active Directory), misalnya akun institusi. |
| JWT | **S** | S | JWT adalah **format token**, bukan protokol autentikasi. Ia dipakai *di dalam* protokol seperti OpenID Connect (ID token berformat JWT, slide 05). |

### Soal 15 — Hal terkait basis data yang berdampak pada kinerja aplikasi web
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Close DB connection setelah selesai query | **B** | B | Koneksi yang tidak ditutup akan menumpuk dan menghabiskan resource DB (*connection leak*). Slide: "*Open and close database connection wisely*". Dengan connection pool, "close" artinya mengembalikan koneksi ke pool. |
| Mendefinisikan Limit dan Offset pada setiap query | **B** | B | Membatasi jumlah baris yang diambil, sehingga transfer lebih kecil dan memori lebih hemat (pagination). |
| Atribut yang ditampilkan/ada pada projection | **B** | B | Memilih hanya kolom yang dibutuhkan (hindari `SELECT *`) mengurangi data yang dibaca dan dikirim. Slide: "*Be careful with SQL wildcard → slow query, big result sets*". |
| Sanitasi query | **S** ⚠️ | B | Sanitasi/escape input (atau prepared statement) adalah langkah **keamanan** untuk mencegah SQL injection, bukan untuk meningkatkan kinerja. Di slide, sanitasi juga dibahas di bawah "*Never trust other inputs*". |

### Soal 16 — Fitur yang ada di HTML5
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Server-Sent Events | **B** | B | Tercantum di daftar fitur baru HTML5 (slide 02b). |
| AJAX | **S** ⚠️ | B | AJAX (XMLHttpRequest) **sudah ada jauh sebelum HTML5** (sejak sekitar 1999–2005) dan **tidak termasuk** daftar fitur baru HTML5 di slide. XMLHttpRequest dan Fetch adalah **Web Platform API** terpisah (slide 03b). |
| Persistent Local Storage | **B** | B | Fitur HTML5 (Web Storage). |
| MVC | **S** | S | MVC adalah pola arsitektur aplikasi, bukan fitur HTML5. |

### Soal 17 — Pernyataan terkait HTML5
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Untuk memanfaatkan fitur audio/video, di browser harus ditambahkan plug-in | **S** | S | Keunggulan HTML5 justru audio/video **tanpa plug-in**. |
| Selain tag `<audio>`/`<video>`, kita perlu menambahkan tombol control agar user bisa play/pause | **B*** | B | Tanpa atribut **`controls`** (atau tombol buatan sendiri lewat JavaScript), elemen audio/video **tidak menampilkan** tombol play/pause. Jadi memang perlu "menambahkan control". *Catatan:* tidak harus membuat tombol sendiri; cukup tambahkan atribut `controls`. |
| Pada WebSockets, penanganan data yang diterima harus dilakukan secara synchronous | **S** | S | Data diterima lewat **event handler `onmessage`**, yang bersifat asinkron/event-driven. |
| Komunikasi WebSockets hanya satu arah dari server ke browser | **S** | S | WebSocket bersifat **dua arah (bidirectional)**: `send()` untuk mengirim dan `onmessage` untuk menerima. Komunikasi satu arah server→browser adalah ciri **Server-Sent Events**. |

### Soal 18 — Tipe variabel di PHP (Ada/Tidak Ada)
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Boolean | **Ada** | Ada | Tipe scalar `bool`. |
| Single-Precision Floating Point | **Tidak ada** | Tidak ada | `float` di PHP selalu **double precision** (64-bit IEEE-754). ✅ Terverifikasi: `PHP_FLOAT_DIG = 15`, `PHP_FLOAT_EPSILON ≈ 2.22e-16`. |
| Function Callback | **Ada** | Ada | Pseudo-type `callback`/`callable` (slide 04a), juga objek `Closure`. |
| Database Connection Resource | **Ada** | Ada | Tipe `resource`, misalnya hasil `mysql_connect()` (slide 04a). |

### Soal 19 — Akses array PHP
```php
<?php
$arr = array("foo" => "bar", "sna" => "fu", 3 => "rand");
?>
```
| Akses | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| `$arr["foo"]` | **Valid** | V | Key `"foo"` ada, nilainya `"bar"`. |
| `$arr["bar"]` | **Error** | E | `"bar"` adalah **nilai**, bukan key. Muncul *Warning: Undefined array key "bar"* dan hasilnya `NULL`. |
| `$arr[1]` | **Error** | E | Tidak ada key 1. Key integer hanya **3**, karena `"foo"` dan `"sna"` adalah key string, tidak otomatis bernomor 0/1. |
| `$arr[3]` | **Valid** | V | Nilainya `"rand"`. |

✅ **Terverifikasi** di PHP 8.3. (Catatan: di PHP, akses key yang tidak ada menghasilkan **Warning** (PHP 8) atau *Notice* (PHP 7) dan bernilai `NULL`, bukan fatal error. Tapi dalam konteks soal, aksesnya dianggap tidak valid.)

### Soal 20 — Pemanggilan fungsi PHP
```php
<?php
function doSomething($type = "default") {
    return "A sentence containing $type parameter.\n";
}
$functionCaller = "doSomething";
?>
```
| Baris | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| `echo doSomething();` | **Valid** | V | Memakai nilai default → "A sentence containing default parameter." |
| `echo doSomething("else");` | **Valid** | V | → "A sentence containing else parameter." |
| `echo $functionCaller();` | **Valid** | V | **Variable function** (dynamic function call, slide 04a): isi variabel `"doSomething"` dipanggil sebagai fungsi. |
| `echo functionCaller("some");` | **Error** | E | Tidak ada fungsi bernama `functionCaller` (tanpa `$`, ini bukan variabel). Hasilnya *Error: Call to undefined function functionCaller()*. |

✅ **Terverifikasi** di PHP 8.3.

### Soal 21 — Komponen PHP untuk menyimpan input form teks (POST, user harus login) ke database
| Variabel/fungsi | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| `$_GET` | **Tidak perlu** | S | Data dikirim di **body**, bukan URL. |
| `$_POST` | **Perlu** | B | Data form dikirim lewat body POST. |
| `mysql_query` | **Perlu*** | S/B | Secara konsep perlu **fungsi untuk menjalankan query INSERT** ke database. *Catatan:* `mysql_query` sudah **dihapus sejak PHP 7**. Di PHP modern pakai `mysqli_query`/`PDO::prepare`, sebaiknya dengan prepared statement. Kalau soal menilai "fungsi spesifik ini di PHP modern", jawabannya Tidak (karena sudah tidak tersedia). |
| `$_FILES` | **Tidak perlu** | S | Tidak ada file yang di-upload; isinya hanya teks. |

(Tambahan yang sebenarnya juga diperlukan tapi tidak ada di pilihan: **`$_SESSION`** dan `session_start()` untuk memastikan user sudah login, seperti di slide 04a *Input from Session*.)

### Soal 22 — Klik `<a href="home.html#section2">` saat berada di home.html
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Browser akan reload home.html dan scroll ke `<section id="section2">` | **S** | S | URL-nya sama dan hanya beda fragment, jadi **tidak ada reload** (navigasi fragment). |
| Browser akan langsung scroll ke `<a name="section2">` | **B** | B | Fallback lama di standar HTML: `<a>` dengan atribut `name` yang cocok. |
| Browser akan langsung scroll ke `<a id="section2">` | **B** ⚠️ | S | Menurut standar HTML, fragment **pertama-tama dicocokkan dengan atribut `id` pada elemen APA PUN**, termasuk `<a>`. ✅ **Terverifikasi** di Chromium: halaman ter-scroll ke `<a id="section2">` tanpa reload. Kunci bundel keliru. |
| Browser akan langsung scroll ke `<section name="section2">` | **S** | S | `<section>` tidak punya atribut `name` yang berfungsi sebagai target fragment. ✅ Terverifikasi: tidak ter-scroll. |

🔎 Tanda kutip penutup di `href` hilang di bundel. Aku menjawab sesuai maksud soal.

### Soal 23 — Tag untuk masukan pilihan tunggal (single selection input)
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| `<input type="radio">` | **B** | B | Dalam satu grup, hanya satu yang bisa dipilih. |
| `<select></select>` | **B** | B | Dropdown dengan satu pilihan. |
| `<select multiple></select>` | **S** | S | Untuk pilihan **majemuk**. |
| `<input type="checkbox">` | **S** | S | Setiap checkbox independen, jadi bisa banyak yang dicentang. |

### Soal 24 — CSS untuk tampilan kotak bergaris tepi dengan "Hello world" dan jarak di dalam
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| `div { border: line black 1px; padding: 20px; }` | **S** | S | `line` bukan `border-style` yang valid. Seluruh deklarasi border diabaikan, sehingga tidak ada garis tepi. |
| `div { border: solid black 1px; padding: 20px; }` | **B** | B | Garis solid 1px dan jarak **di dalam** border (padding). |
| `div { border: solid black 1px; margin: 20px; }` | **S** | S | Margin adalah jarak di **luar** border, sehingga teks menempel ke garis. |
| `div { border: 1px black solid; padding: 20px; }` | **B** | B | Urutan nilai pada shorthand `border` **bebas**. ✅ Terverifikasi di Chromium (border 1px solid). |

### Soal 25 — Mengubah isi `<p id="demo">Ini adalah Ujian WBD</p>`
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| `document.getElement("p").innerHTML = ...` | **S** | S | Method `getElement` **tidak ada**. |
| `document.getElementById("demo").innerHTML = ...` | **B** | B | Cara standar. |
| `document.getElementByName("p").innerHTML = ...` | **S** | S | Method yang ada adalah `getElementsByName` (jamak, mengembalikan NodeList). Lagi pula `"p"` adalah nama tag, bukan nilai atribut `name`. |
| `#demo.innerHTML = ...` | **S** | S | `#demo` adalah sintaks **selector CSS**, bukan JavaScript. Hasilnya SyntaxError. (Yang valid misalnya `document.querySelector("#demo").innerHTML = ...`.) |

### Soal 26 — Cara menulis fungsi penjumlahan di JavaScript
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| `function add(a, b) { return a+b; }` | **B** | B | Function declaration. |
| `int add(a, b) { return a+b; }` | **S** | S | JavaScript tidak mengenal deklarasi tipe seperti `int` (itu gaya C/Java). Hasilnya SyntaxError. |
| `var add = function(a, b) { return a+b; }` | **B** | B | Function expression/literal. |
| `var add = (a,b) => a+b;` | **B** | B | Arrow function dengan return implisit. |

### Soal 27 — Pertimbangan mendesain responsive web
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Mengutamakan kebutuhan pengguna desktop terlebih dahulu sebelum pengguna mobile | **S** | S | Praktik yang disarankan adalah **mobile-first**: mulai dari layar kecil dengan konten esensial, lalu diperkaya untuk layar besar. |
| Mengurangi content hingga ke hal-hal yang esensial saja | **B** | B | Prioritas konten: di layar sempit, tampilkan yang penting dulu. |
| Memperhatikan bagaimana halaman tampil baik di semua ukuran layar | **B** | B | Ini definisi responsive design. |
| Tidak menggunakan JavaScript agar dapat berjalan di semua browser | **S** | S | Responsive design tidak melarang JavaScript. Yang dianjurkan adalah *progressive enhancement*, bukan menghindari JS sama sekali. |

### Soal 28 — Strategi implementasi responsive web
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| CSS berbeda untuk kriteria media berbeda dengan **media queries** | **B** | B | Contoh: `@media (max-width: 600px) { ... }`. |
| Responsive grid system dengan **HTML table** | **S** | S | Tabel untuk layout tidak fleksibel dan tidak semantik. Grid responsif dibuat dengan CSS Grid/Flexbox. |
| Menggunakan CSS **flexbox** | **B** | B | Item bisa melebar/menyusut dan *wrap* sesuai ruang (slide 02b). |
| JavaScript untuk mengubah width elemen secara dinamis berdasarkan ukuran window | **B** | B | Bisa dilakukan (event `resize`/`matchMedia`), walaupun CSS lebih disarankan. |

### Soal 29 — Hal yang dapat dilakukan Progressive Web App (PWA)
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Membuat script yang berjalan di thread terpisah dari main thread | **B** | B | **Service Worker** dan **Web Worker** berjalan di thread terpisah (slide 02b). |
| Aplikasi dapat memiliki launcher icon | **B** | B | Lewat **Web App Manifest**, PWA bisa di-install dan punya ikon di home screen. |
| Service worker di background dapat langsung mengubah tampilan halaman melalui DOM | **S** | S | Service worker **tidak punya akses ke DOM**. Ia harus berkomunikasi dengan halaman lewat `postMessage`, lalu halaman yang mengubah DOM. |
| Client dapat menerima push notification dari server | **B** | B | Lewat Push API dan Notification API yang ditangani service worker. |

### Soal 30 — Pernyataan terkait web accessibility
| Pernyataan | Jawaban | Kunci bundel | Alasan |
|---|---|---|---|
| Memudahkan pengguna menggunakan aplikasi web dalam keadaan sulit seperti ketika sedang berkendara | **B** ⚠️ | S | Menurut **W3C WAI**, aksesibilitas juga bermanfaat bagi orang dengan **"situational limitations"**, misalnya sinar matahari terang, lingkungan yang tidak memungkinkan mendengar audio, atau tangan sedang sibuk. Berkendara termasuk keterbatasan situasional. *Catatan:* kalau dosen memaknai accessibility **sempit**, hanya untuk penyandang disabilitas, jawabannya S seperti kunci bundel. |
| Memungkinkan semua penyandang disabilitas dapat mengakses aplikasi web | **B** | B | Ini tujuan utama web accessibility (pengguna tunanetra, tunarungu, keterbatasan motorik, kognitif, dll). |
| Tidak mengandalkan masukan keyboard saja sehingga dapat meningkatkan accessibility | **B** | B | Mendukung berbagai cara input (keyboard, mouse/touch, suara, switch device) membuat lebih banyak orang bisa memakai aplikasi. (WCAG juga mewajibkan semua fungsi tetap **bisa** dioperasikan dengan keyboard.) |
| Fitur high contrast display, seperti dark-mode, dapat meningkatkan accessibility | **B** | B | Membantu pengguna low vision dan yang sensitif terhadap cahaya. |

---

## ⚠️ Rekap Perbedaan dengan Kunci Bundel (UTS 2021/2022)

| No | Pernyataan | Kunci bundel | Jawabanku | Ringkasan alasan |
|---|---|---|---|---|
| 8 | SPA cocok untuk aplikasi yang memisahkan front-end dan back-end | S | **B** | Pemisahan FE (SPA) dan BE (API) adalah arsitektur khas SPA. |
| 15 | Sanitasi query meningkatkan kinerja | B | **S** | Sanitasi adalah langkah keamanan (anti SQL injection), bukan untuk kinerja. |
| 16 | AJAX adalah fitur HTML5 | B | **S** | AJAX/XHR sudah ada sebelum HTML5 dan merupakan Web API terpisah; tidak ada di daftar fitur HTML5 slide. |
| 22 | Klik `#section2` akan scroll ke `<a id="section2">` | S | **B** | Fragment dicocokkan dengan `id` pada elemen apa pun (terverifikasi di Chromium). |
| 30 | Accessibility membantu pengguna dalam keadaan sulit seperti berkendara | S | **B** | W3C WAI menyebut manfaat untuk *situational limitations*. Bisa S jika accessibility dimaknai sempit. |

Selain itu, ada beberapa yang **tergantung interpretasi** (ditandai \* di tabel kunci):
- **No. 13** (keranjang belanja & preferensi): pola klasik menyimpannya di session/client (S), tapi e-commerce modern sering menyimpannya di database (B).
- **No. 17b** (tombol kontrol audio/video): B jika maksudnya "perlu atribut `controls`".
- **No. 21** (`mysql_query`): secara konsep perlu fungsi query, tapi fungsi itu sudah dihapus sejak PHP 7.

> 💡 **Saran:** untuk soal yang diperdebatkan, pahami **kedua sudut pandangnya**. Di ujian sungguhan, pilih jawaban yang paling sesuai dengan materi slide dosen (lihat file catatan materi). Kalau jawabanku dan kunci bundel berbeda, alasan di atas bisa jadi bahan diskusi dengan asisten atau dosen.