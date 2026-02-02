# 📚 Manual Penggunaan Sistem Penilaian SD (Keyword Coverage-based Grading)

## 📖 PENDAHULUAN

Sistem Penilaian SD adalah metode penilaian otomatis untuk soal isian dan uraian yang dirancang khusus untuk **anak Sekolah Dasar (Kelas 4-6)** dengan pendekatan yang lebih **longgar**, **motivatif**, dan **humanis**.

### ✨ Fitur Utama:
- ✅ **Keyword Coverage**: Menilai berdasarkan jumlah kata kunci yang cocok
- ✅ **Toleransi Typo**: Memahami kesalahan ketik hingga 3 karakter
- ✅ **Nilai Motivatif**: Skala nilai yang memberi nilai untuk usaha
- ✅ **Feedback Positif**: Selalu memberi pesan yang memotivasi siswa
- ✅ **Otomatis & Cepat**: Penilaian instan tanpa AI eksternal

---

## 📊 SKALA NILAI

| Persentase Kata Kunci yang Cocok | Skor | Status      | Feedback untuk Siswa                |
| ------------------------------- | ---- | ----------- | ---------------------------------- |
| 0-9%                            | 20   | **Mencoba**     | "Terus berusaha ya!"                |
| 10-19%                          | 30   | **Kurang**      | "Ada usaha, terus belajar!"         |
| 20-29%                          | 45   | **Cukup**       | "Mulai paham, bagus!"               |
| 30-39%                          | 60   | **Cukup Baik**  | "Bagus sekali!"                     |
| 40-49%                          | 75   | **Baik**        | "Hebat! Kamu paham sebagian besar!" |
| 50-59%                          | 90   | **Sangat Baik** | "Sangat Hebat!"                     |
| **>60%**                            | **100**  | **Luar Biasa!** | **"Luar Biasa! Kamu Pintar!"**         |

**Catatan Penting:**
- Anak SD mendapat nilai **mulai dari 20 poin** bahkan jika hanya memasukkan 1-2 kata kunci yang benar
- Nilai 100 dicapai jika **lebih dari 60% kata kunci** cocok (lebih mudah dari sistem biasa!)
- Fokus pada **motivasi** bukan hukuman

---

## 🎯 CARA KERJA SISTEM

### Langkah 1: Ekstraksi Kata Kunci dari Jawaban Benar

Sistem otomatis mengambil kata kunci dari jawaban benar dengan:
1. **Lowercase** - "Jakarta" → "jakarta"
2. **Hapus tanda baca** - "Jakarta!" → "jakarta"
3. **Hapus stopwords** - kata penghubung (dan, atau, jika, dll) dihapus
4. **Hapus duplikat** - kata yang sama hanya dihitung sekali

**Contoh:**

**Jawaban Benar:** "Ibukota Indonesia adalah Jakarta dan merupakan pusat pemerintahan."

**Kata Kunci:** `["ibukota", "indonesia", "jakarta", "pusat", "pemerintahan"]`

*(Note: "adalah", "dan", "merupakan" dihapus karena stopwords)*

---

### Langkah 2: Bandingkan dengan Jawaban Siswa

Jawaban siswa juga diproses dengan cara yang sama, kemudian dibandingkan kata-per-kata.

**Contoh Jawaban Siswa:** "Ibu kota jakarta"

**Kata Kunci Jawaban Siswa:** `["ibu", "kota", "jakarta"]`

**Pembandingan:**
- "ibu" vs "ibukota" → Tidak match (perbedaan kata terlalu besar)
- "kota" vs "indonesia" → Tidak match
- "kota" vs "jakarta" → Tidak match
- "jakarta" vs "jakarta" → **MATCH!** ✅

**Hasil:** 1 kata cocok dari 5 kata kunci = **20%** → **Nilai 30**

---

### Langkah 3: Toleransi Typo (Fuzzy Matching)

Sistem memahami kesalahan ketik hingga 3 karakter, tergantung panjang kata:

| Panjang Kata | Maks. Typo | Contoh yang Diterima                           |
| ------------ | ---------- | ---------------------------------------------- |
| ≤4 huruf     | 0 typo     | "air" ≠ "iar" (toleransi 0)                   |
| 5-7 huruf    | 1 typo     | "suhu" vs "sihu" ✅ (1 typo)                  |
| 8-12 huruf   | 2 typo     | "indonesia" vs "inodnesia" ✅ (2 typo)         |
| >12 huruf    | 3 typo     | "temperature" vs "temperatur" ✅ (3 typo)       |

**Contoh:**

**Jawaban Benar:** "suhu"
**Jawaban Siswa:** "sihu" (1 typo)
**Hasil:** ✅ MATCH (toleransi 1 untuk kata 4 huruf)

---

### Langkah 4: Threshold Similarity

Sistem menggunakan threshold **65% similarity** untuk match:

- **≥65% similarity** → Full Match (1 poin)
- **50-65% similarity** → Partial Match (0.5 poin)
- **<50% similarity** → No Match (0 poin)

**Contoh:**

| Kata Kunci | Jawaban Siswa | Similarity | Hasil      |
| ---------- | -------------- | ---------- | ---------- |
| "derajat"  | "derajit"      | 71%        | Full Match (1 poin) ✅ |
| "derajat"  | "derajt"       | 83%        | Full Match (1 poin) ✅ |
| "derajat"  | "deraj"        | 71%        | Full Match (1 poin) ✅ |
| "derajat"  | "drajat"       | 85%        | Full Match (1 poin) ✅ |
| "derajat"  | "drajat"       | 71%        | Full Match (1 poin) ✅ |
| "derajat"  | "drjt"         | 42%        | No Match ❌ |
| "derajat"  | "derajatc"     | 88%        | Full Match (1 poin) ✅ |

---

## 📝 CARA MENENTUKAN KATA KUNCI (Tips untuk Guru)

### ✅ Praktik Terbaik:

#### 1. **Gunakan Jawaban yang Jelas dan Spesifik**
```
✅ BAIK: "Suhu tubuh normal berkisar antara 36.5 hingga 37.5 derajat Celsius"
❌ KURANG: "Suhu tubuh normal"
```

#### 2. **Pentingkan Konsep Utama, Bukan Detail Kecil**
```
✅ Fokus pada konsep: "Ibukota Indonesia adalah Jakarta"
❌ Fokus pada detail: "Ibukota negara kesatuan republik indonesia yang berbentuk republik adalah Jakarta"
```

#### 3. **Hindari Jawaban yang Terlalu Panjang**
```
✅ BAIK (4-8 kata kunci): "Suhu normal tubuh manusia adalah 36-37 derajat Celsius"
❌ KURANG (15+ kata kunci): "Suhu normal tubuh manusia adalah berkisar antara tiga puluh enam sampai dengan tiga puluh tujuh derajat Celsius dengan satuan internasional"
```

#### 4. **Tulis Kata Kunci dalam Bahasa Indonesia Baku**
```
✅ BAIK: "suhu", "tubuh", "derajat", "Celsius"
❌ KURANG: "suhu2", "b4dy", "derajt", "cel"
```

#### 5. **Pisahkan Jawaban dengan Variasi Jika Perlu**
```
Jika siswa bisa menjawab dengan cara berbeda, gunakan pemisah "|":

"ibukota|capital|pusat pemerintahan Indonesia Jakarta"

Ini akan di-expand menjadi 3 varian jawaban yang diterima.
```

---

### ⚠️ Hal yang Perlu Diperhatikan:

#### 1. **Stopwords Otomatis Dihapus**
Kata-kata berikut TIDAK dihitung sebagai kata kunci:
- Kata penghubung: **dan, atau, tetapi, karena, maka, jika, sehingga, untuk, dengan, pada, di, ke, dari, dalam**
- Kata tanya: **apa, siapa, mengapa, bagaimana, kapan, dimana**
- Pronomina: **saya, aku, kamu, dia, mereka, kita, kami**
- Partikel: **yang, pun, lah, kah, tah, juga, lagi, lebih, paling**
- Preposisi: **oleh, yaitu, ialah, yakni, adalah, menjadi**

**Tips:** Jangan gunakan kata-kata ini sebagai fokus utama penilaian, karena akan dihapus otomatis.

---

#### 2. **Angka dan Satuan**
Angka dan satuan TIDAK dihapus dan DIHITUNG sebagai kata kunci:

```
✅ "36.5" → tetap dihitung
✅ "37.5" → tetap dihitung
✅ "derajat" → tetap dihitung
✅ "Celsius" → tetap dihitung
```

**Catatan:** Untuk angka dengan rentang (36-37), sistem akan mencoba mencocokkan jika siswa menulis salah satu dari angka tersebut.

---

#### 3. **Sinonim Belum Otomatis (Fitur Tambahan)**
Sistem saat ini BELUM otomatis mencocokkan sinonim.

```
Jawaban Benar: "suhu"
Jawaban Siswa: "temperatur" → Tidak match ❌

Solusi Sementara:
- Gunakan pemisah "|": "suhu|temperatur|temperature"
```

*Catatan: Fitur sinonim otomatis dapat ditambahkan jika diperlukan.*

---

## 🎓 CONTOH PRAKTIS

### Contoh 1: Soal Isian Singkat

**Soal:** Apa ibukota negara Indonesia?

**Jawaban Benar:** "Jakarta"

**Kata Kunci:** `["jakarta"]`

---

| Jawaban Siswa           | Kata Kunci | Match | %    | Skor | Feedback                        |
| ----------------------- | ---------- | ----- | ---- | ---- | ------------------------------- |
| Jakarta                 | jakarta    | 1/1   | 100% | 100  | Luar Biasa! Kamu Pintar!        |
| jakarta                 | jakarta    | 1/1   | 100% | 100  | Luar Biasa! Kamu Pintar!        |
| JAKARTA                 | jakarta    | 1/1   | 100% | 100  | Luar Biasa! Kamu Pintar!        |
| JAKARTA!                | jakarta    | 1/1   | 100% | 100  | Luar Biasa! Kamu Pintar!        |
| JAKARTA INDONESIA       | jakarta    | 1/1   | 100% | 100  | Luar Biasa! Kamu Pintar!        |
| jakartaa                | jakarta    | 0/1   | 0%   | 20   | Terus berusaha ya!              |
| JAKRTA                  | jakarta    | 0/1   | 0%   | 20   | Terus berusaha ya!              |
| Jakarta Selatan          | jakarta    | 1/1   | 100% | 100  | Luar Biasa! Kamu Pintar!        |
| (kosong)                | jakarta    | 0/1   | 0%   | 20   | Terus berusaha ya!              |

---

### Contoh 2: Soal Isian dengan 3 Kata Kunci

**Soal:** Sebutkan suhu tubuh normal manusia!

**Jawaban Benar:** "Suhu tubuh normal berkisar antara 36.5 hingga 37.5 derajat Celsius"

**Kata Kunci:** `["suhu", "tubuh", "normal", "36.5", "37.5", "derajat", "celsius"]` (7 kata kunci)

---

| Jawaban Siswa                   | Match Detail                           | Total | %    | Skor | Feedback                                      |
| ------------------------------- | ------------------------------------- | ----- | ---- | ---- | --------------------------------------------- |
| Suhu tubuh normal 36.5 derajat  | suhu✅, tubuh✅, normal✅, 36.5✅, derajat✅ | 5/7   | 71%  | 100  | Luar Biasa! Kamu Pintar!                     |
| Suhu badan normal 36 derajat    | suhu✅, badan❌, normal✅, 36❌, derajat✅  | 3/7   | 43%  | 75   | Hebat! Kamu paham sebagian besar!            |
| Suhu tubuh 37 derajat Celsius    | suhu✅, tubuh✅, 37❌, derajat✅, celsius✅ | 4/7   | 57%  | 90   | Sangat Hebat!                                |
| Suhu 36 derajat                 | suhu✅, 36❌, derajat✅                   | 2/7   | 29%  | 45   | Mulai paham, bagus!                          |
| 36.5 derajat                    | 36.5✅, derajat✅                       | 2/7   | 29%  | 45   | Mulai paham, bagus!                          |
| 36.5                            | 36.5✅                                 | 1/7   | 14%  | 30   | Ada usaha, terus belajar!                    |
| suhu tubuh                      | suhu✅, tubuh✅                         | 2/7   | 29%  | 45   | Mulai paham, bagus!                          |
| Suhu tubuh normal               | suhu✅, tubuh✅, normal✅                 | 3/7   | 43%  | 75   | Hebat! Kamu paham sebagian besar!            |
| sihu badan normal 36 darajat    | suhu✅, badan❌, normal✅, 36❌, darajat✅  | 3/7   | 43%  | 75   | Hebat! Kamu paham sebagian besar! (toleransi typo) |

---

### Contoh 3: Soal Uraian dengan Multi-point

**Soal:** Jelaskan pengertian suhu tubuh normal!

**Jawaban Benar:** "Suhu tubuh normal adalah kondisi dimana suhu badan berada pada rentang 36.5 hingga 37.5 derajat Celsius yang menunjukkan tubuh dalam keadaan sehat."

**Kata Kunci:** `["suhu", "tubuh", "normal", "kondisi", "badan", "rentang", "36.5", "37.5", "derajat", "celsius", "tubuh", "sehat"]` (setelah deduplikasi: 10 kata kunci)

---

| Jawaban Siswa                                                   | Match                                                                 | %    | Skor | Feedback                              |
| --------------------------------------------------------------- | --------------------------------------------------------------------- | ---- | ---- | ------------------------------------- |
| Suhu tubuh normal adalah 36-37 derajat Celsius                   | suhu✅, tubuh✅, normal✅, 36❌, 37❌, derajat✅, celsius✅                   | 5/10 | 50%  | 90    | Sangat Hebat!                         |
| Suhu badan normal adalah 36 derajat                              | suhu✅, badan❌, normal✅, 36❌, derajat✅                               | 3/10 | 30%  | 60   | Bagus sekali!                         |
| Suhu tubuh 36.5 derajat Celsius                                 | suhu✅, tubuh✅, 36.5✅, derajat✅, celsius✅                           | 5/10 | 50%  | 90   | Sangat Hebat!                         |
| Normal 36.5 derajat                                             | normal✅, 36.5✅, derajat✅                                             | 3/10 | 30%  | 60   | Bagus sekali!                         |
| Suhu badan sehat                                                | suhu✅, badan❌, sehat✅                                                | 2/10 | 20%  | 45   | Mulai paham, bagus!                   |
| 36.5 derajat                                                    | 36.5✅, derajat✅                                                      | 2/10 | 20%  | 45   | Mulai paham, bagus!                   |
| Suhu normal tubuh                                               | suhu✅, normal✅, tubuh✅                                              | 3/10 | 30%  | 60   | Bagus sekali!                         |

---

## 💡 TIPS UNTUK GURU

### 1. **Jangan Terlalu Keras dalam Penilaian**
Ingat bahwa sistem ini dirancang untuk anak SD. Anak-anak sedang belajar dan butuh motivasi, bukan hukuman.

### 2. **Fokus pada Konsep Utama, Bukan Detail Kecil**
Jika siswa memahami konsep utama meskipun salah menuliskan detail kecil, beri nilai yang baik.

### 3. **Gunakan Kata Kunci yang Relevan**
Jangan gunakan kata-kata yang terlalu teknis atau jarang digunakan di kelas.

### 4. **Review Jawaban yang Skornya Rendah Secara Manual**
Sistem otomatis sangat baik untuk penilaian cepat, tapi guru masih bisa mengubah nilai manual jika tidak sesuai.

### 5. **Beri Feedback Positif**
Selalu gunakan kata-kata motivatif saat memberikan hasil penilaian.

### 6. **Gunakan Pemisah "|" untuk Variasi Jawaban**
Jika ada beberapa cara menjawab yang benar, gunakan pemisah "|":
```
Jawaban Benar: "ibukota|capital|pusat pemerintahan Indonesia Jakarta"
```

### 7. **Tulis Jawaban Benar dengan Jelas**
Semakin jelas jawaban benar, semakin akurat hasil penilaian otomatis.

---

## 🔄 CARA MENGUBAH PENILAIAN MANUAL

Sistem penilaian otomatis SD sangat membantu, namun guru masih bisa mengubah nilai secara manual jika perlu:

### Langkah-langkah:
1. Buka halaman penilaian
2. Cari jawaban yang ingin diubah nilainya
3. Klik pada field nilai
4. Ubah nilai sesuai keinginan
5. Sistem otomatis menghitung ulang nilai final

### Kapan Perlu Mengubah Manual?
- ✅ Sistem memberi nilai terlalu rendah padahal jawaban siswa benar
- ✅ Siswa memberikan jawaban unik tapi masih benar
- ✅ Ada masalah teknis dengan penilaian otomatis
- ✅ Guru ingin memberikan nilai tambahan untuk jawaban yang sangat baik

---

## 📈 PERBANDINGAN DENGAN SISTEM LAMA

### Sistem TextSimilarity (Lama):
- Fokus pada kesamaan struktur kalimat
- Toleransi typo terbatas
- Nilai 100 hanya jika >90% similarity
- Lebih ketat dan teknis

### Sistem SD Grading (Baru):
- Fokus pada coverage kata kunci
- Toleransi typo hingga 3 karakter
- Nilai 100 jika >60% kata kunci cocok
- Lebih longgar dan motivatif untuk SD

---

## 🎓 CONTOH KASUS REAL

### Kasus 1: Anak SD yang Salah Ketik

**Soal:** Sebutkan suhu tubuh normal!

**Jawaban Benar:** "36.5 derajat Celsius"

**Jawaban Siswa:** "36.5 darajat celsius"

| Sistem           | Hasil                      | Skor |
| ---------------- | -------------------------- | ---- |
| TextSimilarity   | Similarity 71% (toleransi) | 71   |
| **SD Grading**   | 2/2 keywords matched ✅    | **100** |

**Analisis:** Anak SD salah ketik "derajat" → "darajat". SD Grading memberi nilai maksimal karena konsepnya benar.

---

### Kasus 2: Anak SD yang Menjawab Singkat

**Soal:** Apa ibukota Indonesia?

**Jawaban Benar:** "Jakarta"

**Jawaban Siswa:** "Jakarta"

| Sistem           | Hasil                   | Skor |
| ---------------- | ----------------------- | ---- |
| TextSimilarity   | Exact match             | 100  |
| **SD Grading**   | 1/1 keyword matched ✅  | **100** |

**Analisis:** Keduanya memberi nilai maksimal karena jawaban siswa sesuai.

---

### Kasus 3: Anak SD yang Hanya Menjawab Sebagian

**Soal:** Sebutkan 3 ibukota negara ASEAN!

**Jawaban Benar:** "Jakarta, Bangkok, Kuala Lumpur"

**Jawaban Siswa:** "Jakarta Bangkok"

**Kata Kunci Jawaban Benar:** `["jakarta", "bangkok", "kuala", "lumpur"]`

| Sistem           | Hasil                   | Skor |
| ---------------- | ----------------------- | ---- |
| TextSimilarity   | Similarity 45%          | 45   |
| **SD Grading**   | 2/4 keywords matched ✅  | **75** |

**Analisis:** SD Grading memberi nilai lebih tinggi (75) dibanding TextSimilarity (45) karena mengapresiasi usaha siswa.

---

## ⚙️ KONFIGURASI LANJUTAN

Sistem penilaian SD dapat dikonfigurasi sesuai kebutuhan:

### Mengubah Skala Nilai:
Jika ingin mengubah skala nilai, edit bagian ini di kode:
```javascript
config: {
    scoreRanges: [
        { min: 0, max: 9, score: 20, status: 'Mencoba', feedback: 'Terus berusaha ya!' },
        // ... dsb
    ]
}
```

### Mengubah Toleransi Typo:
```javascript
typoTolerance: {
    minLength: 4,    // Kata ≤ 4 huruf: 0 typo
    shortWord: 7,   // Kata 5-7 huruf: 1 typo
    mediumWord: 12, // Kata 8-12 huruf: 2 typo
    longWord: 99    // Kata >12 huruf: 3 typo
}
```

### Mengubah Threshold Similarity:
```javascript
fuzzyThreshold: 0.65,  // Ubah untuk lebih ketat/longgar
```

---

## ❓ PERTANYAAN UMUM (FAQ)

### Q: Apakah sistem ini bisa digunakan untuk kelas lain selain SD?
**A:** Sistem ini dirancang khusus untuk SD dengan toleransi yang lebih longgar. Untuk SMP/SMA, disarankan menggunakan sistem TextSimilarity yang lebih ketat.

### Q: Apakah nilai otomatis bisa diubah oleh guru?
**A:** Ya! Guru dapat mengubah nilai secara manual kapan saja jika penilaian otomatis tidak sesuai.

### Q: Apakah sistem ini memahami sinonim?
**A:** Saat ini belum otomatis. Untuk sementara, gunakan pemisah "|" untuk variasi jawaban: "suhu|temperatur".

### Q: Bagaimana jika siswa menjawab dalam bahasa Inggris?
**A:** Sistem akan mencoba mencocokkan, tetapi karena stopwords bahasa Indonesia dihapus, kata-kata Inggris mungkin tidak match sebaik bahasa Indonesia.

### Q: Apakah sistem ini perlu internet?
**A:** Tidak! Sistem ini berjalan sepenuhnya offline di browser.

### Q: Apakah bisa menambahkan feedback kustom?
**A:** Ya! Edit bagian `feedback` di konfigurasi `scoreRanges`.

---

## 📞 SUPPORT

Jika mengalami masalah atau memiliki pertanyaan:
1. Cek manual ini
2. Coba uji dengan jawaban contoh
3. Ubah nilai manual jika tidak sesuai
4. Konsultasikan dengan tim IT sekolah

---

## 🎉 PENUTUP

Sistem Penilaian SD dirancang untuk:
- ✅ Membantu guru dalam penilaian otomatis
- ✅ Memberikan nilai yang objektif tapi humanis
- ✅ Mempromosikan pembelajaran positif
- ✅ Mengurangi beban kerja guru dalam penilaian

**Semoga bermanfaat untuk kemajuan pendidikan anak-anak kita!** 🌟

---

*Dokumentasi ini dibuat untuk CBT (Computer Based Test) Kelas 4 SD*
*Versi: 1.0*
*Tanggal: 2026*
