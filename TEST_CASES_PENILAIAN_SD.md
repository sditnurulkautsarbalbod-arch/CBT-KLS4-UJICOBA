# 🧪 Test Cases - Sistem Penilaian SD (Keyword Coverage-based Grading)

## 📖 PENDAHULUAN

Dokumentasi ini berisi test cases yang komprehensif untuk memvalidasi algoritma penilaian SD. Test cases ini mencakup berbagai skenario:

1. ✅ Basic matching (exact match)
2. ✅ Typo tolerance (fuzzy matching)
3. ✅ Partial matching
4. ✅ Stopword filtering
5. ✅ Case and punctuation handling
6. ✅ Edge cases (empty, very short, very long)
7. ✅ Multi-keyword answers
8. ✅ Number and unit handling
9. ✅ Real-world scenarios

---

## 🎯 FORMAT TEST CASE

Setiap test case memiliki struktur berikut:

```javascript
{
    testCase: "Nama Test Case",
    soal: "Soal yang ditampilkan",
    jawabanBenar: "Jawaban benar yang diinput guru",
    jawabanSiswa: "Jawaban yang diberikan siswa",
    expectedScore: 100, // Nilai yang diharapkan
    expectedPercentage: 100, // Persentase yang diharapkan
    description: "Penjelasan hasil",
    passed: true/false // Hasil test
}
```

---

## 📋 TEST CASES

---

### **KATEGORI 1: BASIC MATCHING (Exact Match)**

#### TC-001: Exact Match Sempurna
```javascript
{
    testCase: "TC-001: Exact Match Sempurna",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta",
    jawabanSiswa: "Jakarta",
    keywords: ["jakarta"],
    matched: ["jakarta"],
    percentage: 100,
    expectedScore: 100,
    description: "Jawaban siswa 100% sama dengan kunci jawaban"
}
```
**Expected:** ✅ SCORE: 100

---

#### TC-002: Case Insensitive Match
```javascript
{
    testCase: "TC-002: Case Insensitive Match",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta",
    jawabanSiswa: "jakarta",
    keywords: ["jakarta"],
    matched: ["jakarta"],
    percentage: 100,
    expectedScore: 100,
    description: "Huruf kapital tidak berpengaruh"
}
```
**Expected:** ✅ SCORE: 100

---

#### TC-003: With Punctuation
```javascript
{
    testCase: "TC-003: With Punctuation",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta!",
    jawabanSiswa: "Jakarta!",
    keywords: ["jakarta"],
    matched: ["jakarta"],
    percentage: 100,
    expectedScore: 100,
    description: "Tanda baca dihapus otomatis"
}
```
**Expected:** ✅ SCORE: 100

---

#### TC-004: Multiple Spaces
```javascript
{
    testCase: "TC-004: Multiple Spaces",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta  Indonesia",
    jawabanSiswa: "Jakarta   Indonesia",
    keywords: ["jakarta", "indonesia"],
    matched: ["jakarta", "indonesia"],
    percentage: 100,
    expectedScore: 100,
    description: "Multiple spaces dinormalisasi"
}
```
**Expected:** ✅ SCORE: 100

---

### **KATEGORI 2: TYPO TOLERANCE (Fuzzy Matching)**

#### TC-101: Single Typo (Kata Pendek)
```javascript
{
    testCase: "TC-101: Single Typo (Kata Pendek)",
    soal: "Sebutkan suhu tubuh!",
    jawabanBenar: "Suhu",
    jawabanSiswa: "Sihu",
    keywords: ["suhu"],
    matched: ["sihu"],
    percentage: 100,
    expectedScore: 100,
    description: "1 typo pada kata 4 huruf (toleransi 1 untuk kata 5-7 huruf, tapi suhu=4 huruf jadi 0 typo? HARUS DICEK)"
}
```
**Expected:** ⚠️ PERLU DIVERIFIKASI (suhu=4 huruf, toleransi=0, seharusnya tidak match)

---

#### TC-102: Single Typo (Kata Medium)
```javascript
{
    testCase: "TC-102: Single Typo (Kata Medium)",
    soal: "Sebutkan suhu tubuh!",
    jawabanBenar: "Derajat",
    jawabanSiswa: "Derajit",
    keywords: ["derajat"],
    matched: ["derajit"],
    percentage: 100,
    expectedScore: 100,
    description: "1 typo pada kata 7 huruf (toleransi 1)"
}
```
**Expected:** ✅ SCORE: 100 (similarity ~71%, ≥65%)

---

#### TC-103: Two Typos (Kata Panjang)
```javascript
{
    testCase: "TC-103: Two Typos (Kata Panjang)",
    soal: "Sebutkan suhu tubuh!",
    jawabanBenar: "Indonesia",
    jawabanSiswa: "Inodnesia",
    keywords: ["indonesia"],
    matched: ["inodnesia"],
    percentage: 100,
    expectedScore: 100,
    description: "2 typo pada kata 9 huruf (toleransi 2)"
}
```
**Expected:** ✅ SCORE: 100 (similarity ~88%, ≥65%)

---

#### TC-104: Three Typos (Kata Sangat Panjang)
```javascript
{
    testCase: "TC-104: Three Typos (Kata Sangat Panjang)",
    soal: "Sebutkan istilah medis!",
    jawabanBenar: "Temperature",
    jawabanSiswa: "Temperatur",
    keywords: ["temperature"],
    matched: ["temperatur"],
    percentage: 100,
    expectedScore: 100,
    description: "3 typo pada kata 11 huruf (toleransi 2 untuk 8-12 huruf, jadi HARUS DICEK)"
}
```
**Expected:** ⚠️ PERLU DIVERIFIKASI (temperature=11 huruf, toleransi=2, jadi tidak match jika typo=3)

---

#### TC-105: Too Many Typos
```javascript
{
    testCase: "TC-105: Too Many Typos",
    soal: "Sebutkan suhu tubuh!",
    jawabanBenar: "Derajat",
    jawabanSiswa: "Draaajit",
    keywords: ["derajat"],
    matched: [],
    percentage: 0,
    expectedScore: 20,
    description: "3 typo pada kata 7 huruf (melebihi toleransi 1)"
}
```
**Expected:** ✅ SCORE: 20 (tidak match)

---

### **KATEGORI 3: PARTIAL MATCHING**

#### TC-201: 50% Similarity (Borderline)
```javascript
{
    testCase: "TC-201: 50% Similarity (Borderline)",
    soal: "Sebutkan suhu tubuh!",
    jawabanBenar: "Celsius",
    jawabanSiswa: "Celsus",
    keywords: ["celsius"],
    matched: ["celsus"],
    percentage: 50,
    expectedScore: 60,
    description: "Similarity 50% = partial match = 0.5 poin"
}
```
**Expected:** ✅ SCORE: 60 (0.5/1 keywords = 50%)

---

#### TC-202: 65% Similarity (Threshold)
```javascript
{
    testCase: "TC-202: 65% Similarity (Threshold)",
    soal: "Sebutkan suhu tubuh!",
    jawabanBenar: "Celsius",
    jawabanSiswa: "Celcius",
    keywords: ["celsius"],
    matched: ["celcius"],
    percentage: 100,
    expectedScore: 100,
    description: "Similarity 65%+ = full match = 1 poin"
}
```
**Expected:** ✅ SCORE: 100 (similarity ~71%, ≥65%)

---

#### TC-203: Below Threshold
```javascript
{
    testCase: "TC-203: Below Threshold",
    soal: "Sebutkan suhu tubuh!",
    jawabanBenar: "Derajat",
    jawabanSiswa: "Drat",
    keywords: ["derajat"],
    matched: [],
    percentage: 0,
    expectedScore: 20,
    description: "Similarity <50% = no match"
}
```
**Expected:** ✅ SCORE: 20 (tidak match)

---

### **KATEGORI 4: STOPWORD FILTERING**

#### TC-301: Stopword Removal
```javascript
{
    testCase: "TC-301: Stopword Removal",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta adalah ibukota Indonesia",
    jawabanSiswa: "Jakarta dan ibukota Indonesia",
    keywords: ["jakarta", "ibukota", "indonesia"], // "adalah", "dan" dihapus
    matched: ["jakarta", "ibukota", "indonesia"],
    percentage: 100,
    expectedScore: 100,
    description: "Stopword 'adalah' dan 'dan' dihapus otomatis"
}
```
**Expected:** ✅ SCORE: 100

---

#### TC-302: All Stopwords
```javascript
{
    testCase: "TC-302: All Stopwords",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta",
    jawabanSiswa: "dan atau karena jika",
    keywords: [], // Semua stopwords dihapus
    matched: [],
    percentage: 0,
    expectedScore: 20,
    description: "Jawaban hanya stopwords = 0 keyword"
}
```
**Expected:** ✅ SCORE: 20 (tidak ada keyword)

---

#### TC-303: Stopword in Middle
```javascript
{
    testCase: "TC-303: Stopword in Middle",
    soal: "Sebutkan suhu tubuh!",
    jawabanBenar: "Suhu tubuh normal",
    jawabanSiswa: "Suhu dan tubuh normal",
    keywords: ["suhu", "tubuh", "normal"], // "dan" dihapus
    matched: ["suhu", "tubuh", "normal"],
    percentage: 100,
    expectedScore: 100,
    description: "Stopword di tengah kalimat dihapus"
}
```
**Expected:** ✅ SCORE: 100

---

### **KATEGORI 5: EDGE CASES**

#### TC-401: Empty Answer
```javascript
{
    testCase: "TC-401: Empty Answer",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta",
    jawabanSiswa: "",
    keywords: ["jakarta"],
    matched: [],
    percentage: 0,
    expectedScore: 20,
    description: "Jawaban kosong"
}
```
**Expected:** ✅ SCORE: 20

---

#### TC-402: Very Short Answer (1 char)
```javascript
{
    testCase: "TC-402: Very Short Answer (1 char)",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta",
    jawabanSiswa: "J",
    keywords: ["jakarta"],
    matched: [], // "j" dihapus karena <3 huruf
    percentage: 0,
    expectedScore: 20,
    description: "Jawaban 1 karakter dihapus (min 2 char)"
}
```
**Expected:** ✅ SCORE: 20

---

#### TC-403: Very Long Answer
```javascript
{
    testCase: "TC-403: Very Long Answer",
    soal: "Jelaskan pengertian suhu tubuh!",
    jawabanBenar: "Suhu tubuh normal adalah kondisi dimana suhu badan berada pada rentang 36.5 hingga 37.5 derajat Celsius yang menunjukkan tubuh dalam keadaan sehat",
    jawabanSiswa: "Suhu tubuh normal adalah kondisi dimana suhu badan berada pada rentang 36.5 hingga 37.5 derajat Celsius yang menunjukkan tubuh dalam keadaan sehat",
    keywords: ["suhu", "tubuh", "normal", "kondisi", "badan", "rentang", "36.5", "37.5", "derajat", "celsius", "tubuh", "sehat"], // 10 unik
    matched: ["suhu", "tubuh", "normal", "kondisi", "badan", "rentang", "36.5", "37.5", "derajat", "celsius", "sehat"],
    percentage: 100,
    expectedScore: 100,
    description: "Jawaban sangat panjang tapi 100% match"
}
```
**Expected:** ✅ SCORE: 100

---

#### TC-404: Numeric Answer
```javascript
{
    testCase: "TC-404: Numeric Answer",
    soal: "Berapa suhu tubuh normal?",
    jawabanBenar: "36.5",
    jawabanSiswa: "36.5",
    keywords: ["36.5"],
    matched: ["36.5"],
    percentage: 100,
    expectedScore: 100,
    description: "Jawaban berupa angka desimal"
}
```
**Expected:** ✅ SCORE: 100

---

### **KATEGORI 6: MULTI-KEYWORD ANSWERS**

#### TC-501: 2 Keywords - Both Match
```javascript
{
    testCase: "TC-501: 2 Keywords - Both Match",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta Indonesia",
    jawabanSiswa: "Jakarta Indonesia",
    keywords: ["jakarta", "indonesia"],
    matched: ["jakarta", "indonesia"],
    percentage: 100,
    expectedScore: 100,
    description: "2 keywords, keduanya match"
}
```
**Expected:** ✅ SCORE: 100

---

#### TC-502: 2 Keywords - 1 Match
```javascript
{
    testCase: "TC-502: 2 Keywords - 1 Match",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta Indonesia",
    jawabanSiswa: "Jakarta",
    keywords: ["jakarta", "indonesia"],
    matched: ["jakarta"],
    percentage: 50,
    expectedScore: 90,
    description: "2 keywords, 1 match = 50%"
}
```
**Expected:** ✅ SCORE: 90 (50% → 90)

---

#### TC-503: 5 Keywords - 3 Match
```javascript
{
    testCase: "TC-503: 5 Keywords - 3 Match",
    soal: "Sebutkan suhu tubuh normal!",
    jawabanBenar: "Suhu tubuh normal 36.5 derajat Celsius",
    jawabanSiswa: "Suhu tubuh 36 derajat",
    keywords: ["suhu", "tubuh", "normal", "36.5", "derajat", "celsius"],
    matched: ["suhu", "tubuh", "derajat"], // "36" vs "36.5" tidak match
    percentage: 50,
    expectedScore: 90,
    description: "5 keywords, 3 match = 50%"
}
```
**Expected:** ✅ SCORE: 90 (3/6 = 50% → 90)

---

#### TC-504: 10 Keywords - 4 Match
```javascript
{
    testCase: "TC-504: 10 Keywords - 4 Match",
    soal: "Jelaskan pengertian suhu tubuh!",
    jawabanBenar: "Suhu tubuh normal adalah kondisi dimana suhu badan berada pada rentang 36.5 hingga 37.5 derajat Celsius",
    jawabanSiswa: "Suhu tubuh normal 36 derajat",
    keywords: ["suhu", "tubuh", "normal", "kondisi", "badan", "rentang", "36.5", "37.5", "derajat", "celsius"],
    matched: ["suhu", "tubuh", "normal", "derajat"], // "36" vs "36.5" tidak match
    percentage: 40,
    expectedScore: 75,
    description: "10 keywords, 4 match = 40%"
}
```
**Expected:** ✅ SCORE: 75 (4/10 = 40% → 75)

---

### **KATEGORI 7: NUMBER AND UNIT HANDLING**

#### TC-601: Exact Number Match
```javascript
{
    testCase: "TC-601: Exact Number Match",
    soal: "Berapa suhu tubuh normal?",
    jawabanBenar: "36.5 derajat",
    jawabanSiswa: "36.5 derajat",
    keywords: ["36.5", "derajat"],
    matched: ["36.5", "derajat"],
    percentage: 100,
    expectedScore: 100,
    description: "Angka dan satuan match sempurna"
}
```
**Expected:** ✅ SCORE: 100

---

#### TC-602: Slightly Different Number
```javascript
{
    testCase: "TC-602: Slightly Different Number",
    soal: "Berapa suhu tubuh normal?",
    jawabanBenar: "36.5 derajat",
    jawabanSiswa: "36 derajat",
    keywords: ["36.5", "derajat"],
    matched: ["derajat"], // "36" vs "36.5" tidak match
    percentage: 50,
    expectedScore: 90,
    description: "Angka berbeda sedikit (36 vs 36.5)"
}
```
**Expected:** ✅ SCORE: 90 (1/2 = 50% → 90)

---

#### TC-603: Missing Unit
```javascript
{
    testCase: "TC-603: Missing Unit",
    soal: "Berapa suhu tubuh normal?",
    jawabanBenar: "36.5 derajat",
    jawabanSiswa: "36.5",
    keywords: ["36.5", "derajat"],
    matched: ["36.5"],
    percentage: 50,
    expectedScore: 90,
    description: "Tanpa satuan"
}
```
**Expected:** ✅ SCORE: 90 (1/2 = 50% → 90)

---

#### TC-604: Wrong Unit
```javascript
{
    testCase: "TC-604: Wrong Unit",
    soal: "Berapa suhu tubuh normal?",
    jawabanBenar: "36.5 derajat",
    jawabanSiswa: "36.5 meter",
    keywords: ["36.5", "derajat"],
    matched: ["36.5"], // "meter" vs "derajat" tidak match
    percentage: 50,
    expectedScore: 90,
    description: "Satuan salah"
}
```
**Expected:** ✅ SCORE: 90 (1/2 = 50% → 90)

---

### **KATEGORI 8: REAL-WORLD SCENARIOS**

#### TC-701: Anak SD Salah Ketik (Sedang)
```javascript
{
    testCase: "TC-701: Anak SD Salah Ketik (Sedang)",
    soal: "Sebutkan 3 ibukota negara ASEAN!",
    jawabanBenar: "Jakarta Bangkok Kuala Lumpur",
    jawabanSiswa: "Jakarta Bankok Kula Lumpur",
    keywords: ["jakarta", "bangkok", "kuala", "lumpur"],
    matched: ["jakarta", "bankok"], // "Bankok" vs "Bangkok" (1 typo) ✅
    percentage: 50,
    expectedScore: 90,
    description: "Anak SD salah ketik 1 kata"
}
```
**Expected:** ✅ SCORE: 90 (2/4 = 50% → 90)

---

#### TC-702: Anak SD Menjawab Singkat
```javascript
{
    testCase: "TC-702: Anak SD Menjawab Singkat",
    soal: "Sebutkan 3 ibukota negara ASEAN!",
    jawabanBenar: "Jakarta Bangkok Kuala Lumpur",
    jawabanSiswa: "Jakarta",
    keywords: ["jakarta", "bangkok", "kuala", "lumpur"],
    matched: ["jakarta"],
    percentage: 25,
    expectedScore: 60,
    description: "Anak SD hanya menjawab 1 dari 3"
}
```
**Expected:** ✅ SCORE: 60 (1/4 = 25% → 60)

---

#### TC-703: Anak SD Menjawab Sebagian Besar
```javascript
{
    testCase: "TC-703: Anak SD Menjawab Sebagian Besar",
    soal: "Sebutkan 3 ibukota negara ASEAN!",
    jawabanBenar: "Jakarta Bangkok Kuala Lumpur",
    jawabanSiswa: "Jakarta Bangkok Kuala",
    keywords: ["jakarta", "bangkok", "kuala", "lumpur"],
    matched: ["jakarta", "bangkok", "kuala"],
    percentage: 75,
    expectedScore: 100,
    description: "Anak SD menjawab 3 dari 4 kata kunci"
}
```
**Expected:** ✅ SCORE: 100 (3/4 = 75% → 100)

---

#### TC-704: Anak SD Menjawab dengan Stopwords
```javascript
{
    testCase: "TC-704: Anak SD Menjawab dengan Stopwords",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta",
    jawabanSiswa: "Ibukota Indonesia adalah Jakarta",
    keywords: ["jakarta"], // "ibukota", "indonesia", "adalah" bukan kunci
    matched: ["jakarta"],
    percentage: 100,
    expectedScore: 100,
    description: "Anak SD menjawab dengan kalimat lengkap"
}
```
**Expected:** ⚠️ PERLU DIVERIFIKASI (tergantung apakah "ibukota" dan "indonesia" dianggap keyword)

---

#### TC-705: Anak SD Salah Ketik (Banyak)
```javascript
{
    testCase: "TC-705: Anak SD Salah Ketik (Banyak)",
    soal: "Sebutkan 3 ibukota negara ASEAN!",
    jawabanBenar: "Jakarta Bangkok Kuala Lumpur",
    jawabanSiswa: "Jakrta Bngkok Kla Lumpur",
    keywords: ["jakarta", "bangkok", "kuala", "lumpur"],
    matched: ["kuala", "lumpur"], // "Jakrta" 2 typo (melebihi 1), "Bngkok" 2 typo (melebihi 1)
    percentage: 50,
    expectedScore: 90,
    description: "Anak SD salah ketik banyak tapi masih ada yang match"
}
```
**Expected:** ✅ SCORE: 90 (2/4 = 50% → 90)

---

#### TC-706: Anak SD Menjawab Salah
```javascript
{
    testCase: "TC-706: Anak SD Menjawab Salah",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta",
    jawabanSiswa: "Bandung",
    keywords: ["jakarta"],
    matched: [], // "bandung" vs "jakarta" tidak match
    percentage: 0,
    expectedScore: 20,
    description: "Anak SD menjawab kota yang salah"
}
```
**Expected:** ✅ SCORE: 20 (tidak match sama sekali)

---

#### TC-707: Anak SD Menjawab Singkat tapi Benar
```javascript
{
    testCase: "TC-707: Anak SD Menjawab Singkat tapi Benar",
    soal: "Apa suhu tubuh normal?",
    jawabanBenar: "36.5 derajat Celsius",
    jawabanSiswa: "36.5",
    keywords: ["36.5", "derajat", "celsius"],
    matched: ["36.5"],
    percentage: 33.33,
    expectedScore: 60,
    description: "Anak SD menjawab angka saja tapi benar"
}
```
**Expected:** ✅ SCORE: 60 (1/3 = 33% → 60)

---

#### TC-708: Anak SD Menjawab dengan Kalimat Panjang
```javascript
{
    testCase: "TC-708: Anak SD Menjawab dengan Kalimat Panjang",
    soal: "Apa ibukota Indonesia?",
    jawabanBenar: "Jakarta",
    jawabanSiswa: "Ibukota negara Indonesia adalah Jakarta dan merupakan pusat pemerintahan negara tersebut",
    keywords: ["jakarta"], // "ibukota", "negara", "indonesia", "adalah", "dan", "merupakan", "pusat", "pemerintahan", "tersebut" semua stopwords
    matched: ["jakarta"],
    percentage: 100,
    expectedScore: 100,
    description: "Anak SD menjawab dengan kalimat sangat panjang"
}
```
**Expected:** ⚠️ PERLU DIVERIFIKASI (hanya "jakarta" yang match)

---

## 📊 SUMMARY TABLE

| ID | Kategori | Persentase | Skor | Status |
| -- | -------- | ---------- | ----- | ------ |
| TC-001 | Basic | 100% | 100 | ✅ |
| TC-002 | Basic | 100% | 100 | ✅ |
| TC-003 | Basic | 100% | 100 | ✅ |
| TC-004 | Basic | 100% | 100 | ✅ |
| TC-101 | Typo | 0% | 20 | ⚠️ |
| TC-102 | Typo | 100% | 100 | ✅ |
| TC-103 | Typo | 100% | 100 | ✅ |
| TC-104 | Typo | 0% | 20 | ⚠️ |
| TC-105 | Typo | 0% | 20 | ✅ |
| TC-201 | Partial | 50% | 60 | ✅ |
| TC-202 | Partial | 100% | 100 | ✅ |
| TC-203 | Partial | 0% | 20 | ✅ |
| TC-301 | Stopword | 100% | 100 | ✅ |
| TC-302 | Stopword | 0% | 20 | ✅ |
| TC-303 | Stopword | 100% | 100 | ✅ |
| TC-401 | Edge | 0% | 20 | ✅ |
| TC-402 | Edge | 0% | 20 | ✅ |
| TC-403 | Edge | 100% | 100 | ✅ |
| TC-404 | Edge | 100% | 100 | ✅ |
| TC-501 | Multi | 100% | 100 | ✅ |
| TC-502 | Multi | 50% | 90 | ✅ |
| TC-503 | Multi | 50% | 90 | ✅ |
| TC-504 | Multi | 40% | 75 | ✅ |
| TC-601 | Number | 100% | 100 | ✅ |
| TC-602 | Number | 50% | 90 | ✅ |
| TC-603 | Number | 50% | 90 | ✅ |
| TC-604 | Number | 50% | 90 | ✅ |
| TC-701 | Real | 50% | 90 | ✅ |
| TC-702 | Real | 25% | 60 | ✅ |
| TC-703 | Real | 75% | 100 | ✅ |
| TC-704 | Real | 100% | 100 | ⚠️ |
| TC-705 | Real | 50% | 90 | ✅ |
| TC-706 | Real | 0% | 20 | ✅ |
| TC-707 | Real | 33% | 60 | ✅ |
| TC-708 | Real | 100% | 100 | ⚠️ |

**Total Test Cases:** 37
**Expected Pass:** 34 (91.9%)
**Needs Verification:** 3 (TC-101, TC-104, TC-704, TC-708)

---

## 🧪 HOW TO RUN TESTS

### Manual Testing:
1. Buka file index.html di browser
2. Buka Console (F12)
3. Jalankan fungsi test:

```javascript
// Jalankan satu test case
const result = SDGrading.gradeAnswerForSD("Jakarta", "Jakarta");
console.log(result);

// Jalankan semua test cases
runAllTests();
```

### Automated Testing (Future):
Membuat file test.html yang berisi:
- Form untuk input soal, jawaban benar, jawaban siswa
- Button untuk menjalankan test
- Tabel hasil test yang lengkap

---

## ⚠️ NOTES & CONSIDERATIONS

### 1. Typo Tolerance vs Word Length
- Kata ≤4 huruf: Toleransi 0 typo → "suhu" (4 huruf) harus exact match
- Ini bisa terlalu ketat untuk anak SD

**Suggestion:** Pertimbangkan toleransi 1 typo untuk kata ≥3 huruf

---

### 2. Similarity Threshold
- Current: 65%
- Ini memberi full match untuk similarity yang cukup rendah

**Consideration:** Bisa dinaikkan ke 70% untuk lebih ketat, atau diturunkan ke 60% untuk lebih longgar

---

### 3. Stopword List
- Stopword list yang digunakan cukup komprehensif
- Pastikan tidak menghapus kata-kata penting yang seharusnya dianggap keyword

**Consideration:** Review stopword list untuk setiap mata pelajaran yang berbeda

---

### 4. Number Matching
- Angka "36" vs "36.5" tidak dianggap match
- Untuk SD, mungkin perlu range matching

**Suggestion:** Implementasi number range matching jika diperlukan

---

### 5. Synonyms
- Saat ini tidak ada synonym matching otomatis
- Gunakan pemisah "|" untuk variasi jawaban

**Future Work:** Implementasi synonym database

---

## 📞 NEXT STEPS

1. ✅ Review test cases di atas
2. ⏳ Jalankan test cases di sistem
3. ⏳ Verifikasi hasil untuk 3 test cases yang perlu diverifikasi
4. ⏳ Buat file test.html untuk automated testing
5. ⏳ Update dokumentasi berdasarkan hasil test
6. ⏳ Buat training manual untuk guru

---

*Dokumentasi Test Cases dibuat untuk CBT Kelas 4 SD*
*Versi: 1.0*
*Tanggal: 2026*
