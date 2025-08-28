# MODUL HARI 1 - SESI 1: DATA CLASSIFICATION & UPLOAD STRATEGY

*Workshop: Pengolahan Data Penelitian Empiris dengan Claude AI*  
**Durasi: 90 menit | Format: Offline | Peserta: 10-15 orang**

---

## 🎯 LEARNING OBJECTIVES

Setelah sesi ini, peserta akan dapat:
1. Mengklasifikasi tipe data penelitian menggunakan Claude AI
2. Menerapkan strategi upload yang tepat sesuai metodologi
3. Memverifikasi kualitas data yang telah diupload
4. Mengidentifikasi dan mengatasi masalah upload umum

---

## 📋 PRE-SESSION CHECKLIST

### Untuk Peserta:
- [ ] Akses Claude Pro sudah aktif
- [ ] Data penelitian sudah disiapkan (raw format)
- [ ] Laptop/device dengan koneksi internet stabil
- [ ] Notepad untuk mencatat prompt dan hasil

### Untuk Fasilitator:
- [ ] Sample datasets untuk demo (kuantitatif, kualitatif, mixed)
- [ ] Troubleshooting scenarios siap
- [ ] Assistant untuk setiap track sudah briefed

---

## 🔧 BAGIAN 1: DATA TYPE IDENTIFICATION (30 MENIT)

### Opening Prompt untuk Identifikasi Tipe Data

```
Saya adalah peneliti yang memiliki dataset untuk penelitian empiris. Saya akan memberikan deskripsi singkat tentang data saya. Tolong bantu saya:

1. Mengklasifikasi tipe data (kuantitatif/kualitatif/mixed methods)
2. Mengidentifikasi karakteristik spesifik data
3. Memberikan rekomendasi pendekatan analisis yang sesuai
4. Menentukan track workshop yang paling cocok untuk saya

Deskripsi data saya:
**KONTEKS PENELITIAN:**
- Topik/bidang penelitian: [contoh: "Pengaruh work-life balance terhadap produktivitas karyawan"]
- Tujuan penelitian: [contoh: "Menganalisis hubungan kausal antara X dan Y"]
- Populasi/sampel: [contoh: "Karyawan startup di Jakarta, N=150"]

**KARAKTERISTIK DATA:**
- Jenis data yang dikumpulkan: [contoh: "Survey kuesioner + wawancara mendalam"]
- Metode pengumpulan: [contoh: "Online survey via Google Forms + Zoom interviews"]
- Periode pengumpulan: [contoh: "Januari-Maret 2024"]
- Format file saat ini: [contoh: "Excel untuk survey, Word docs untuk transkrip"]

**DETAIL TEKNIS:**
- Ukuran data: [contoh: "150 responses survey + 20 interview transcripts"]
- Variabel utama: [contoh: "Work-life balance scale, productivity metrics, demographic data"]
- Bahasa data: [contoh: "Indonesia/Inggris/campuran"]
- Status data: [contoh: "Raw data belum dibersihkan"]

**RENCANA ANALISIS:**
- Pertanyaan penelitian utama: [contoh: "Apakah work-life balance berpengaruh signifikan terhadap produktivitas?"]
- Jenis analisis yang diinginkan: [contoh: "Korelasi, regresi, analisis tema"]
- Target publikasi: [contoh: "Jurnal manajemen Q1/Q2"]

Format jawaban yang saya inginkan:
- Tipe Data: [Klasifikasi utama]
- Karakteristik: [Detail spesifik]
- Track Rekomendasi: [A/B/C dengan alasan]
- Pendekatan Analisis: [Saran metodologi]
- Potensi Tantangan: [Hal yang perlu diwaspadai]
```

### 📊 Template Klasifikasi Berdasarkan Tipe Data

#### TEMPLATE A: Pure Quantitative Data
```
Saya memiliki data kuantitatif dengan karakteristik berikut:
- Ukuran sampel: [N = ...]
- Variabel: [Daftar variabel utama]
- Skala pengukuran: [Nominal/ordinal/interval/rasio]
- Missing data: [Ya/tidak, berapa persen]
- Tujuan analisis: [Deskriptif/inferensial/prediktif]

Tolong validasi apakah data ini cocok untuk analisis statistik dan berikan rekomendasi preprocessing yang diperlukan.
```

#### TEMPLATE B: Pure Qualitative Data
```
Saya memiliki data kualitatif dengan karakteristik berikut:
- Jenis data: [Interview transcripts/focus groups/observations/documents]
- Jumlah: [Berapa file/partisipan/dokumen]
- Format: [Text files/Word documents/PDFs]
- Bahasa: [Indonesia/Inggris/campuran]
- Anonymisasi: [Sudah/belum dilakukan]
- Tujuan analisis: [Thematic analysis/content analysis/discourse analysis]

Tolong validasi kesiapan data untuk analisis kualitatif dan berikan panduan preprocessing.
```

#### TEMPLATE C: Mixed Methods Data
```
Saya memiliki data mixed methods dengan karakteristik berikut:
- Data kuantitatif: [Deskripsi singkat + ukuran sampel]
- Data kualitatif: [Deskripsi singkat + jumlah]
- Design: [Sequential explanatory/exploratory/concurrent]
- Integration point: [Di tahap analisis/interpretasi/both]
- Participant matching: [Ada sistem ID penghubung/tidak]

Tolong validasi kelengkapan data untuk mixed methods dan berikan strategi integrasi.
```

### ⚡ AKTIVITAS PRAKTIK (15 menit)

**Instruksi untuk Peserta:**
1. Gunakan template yang sesuai dengan data Anda
2. Input deskripsi data ke Claude AI
3. Catat hasil klasifikasi dan rekomendasi
4. Diskusikan dengan assistant track jika ada ketidakpastian

**Expected Output:**
- Klasifikasi tipe data yang akurat
- Track assignment yang tepat
- Preliminary analysis roadmap

---

## 📤 BAGIAN 2: CUSTOMIZED UPLOAD APPROACH (60 MENIT)

### 🔢 TRACK A: Quantitative Data Upload

#### Prompt Persiapan Upload Kuantitatif
```
Saya akan mengupload dataset kuantitatif untuk analisis statistik. Sebelum upload, tolong berikan checklist persiapan data dan format optimal untuk analisis dengan AI.

Informasi dataset:
- Format file saat ini: [CSV/Excel/SPSS/lainnya]
- Jumlah variabel: [...]
- Jumlah observasi: [...]
- Ada missing values: [Ya/Tidak]
- Ada categorical variables: [Ya/Tidak]

Yang saya butuhkan:
1. Checklist pre-upload preparation
2. Format file yang direkomendasikan
3. Struktur header yang optimal
4. Handling missing values dan outliers
5. Variable naming conventions
```

#### Template Upload Verification (Post-Upload)
```
Saya baru saja mengupload dataset kuantitatif. Tolong lakukan verification menyeluruh dan berikan laporan kondisi data:

VERIFICATION REQUEST:
1. STRUCTURE CHECK:
   - Apakah struktur data sudah benar (rows = cases, columns = variables)?
   - Ada masalah dengan header/column names?
   - Format data type sudah sesuai?

2. QUALITY ASSESSMENT:
   - Missing value pattern dan persentase
   - Outlier detection (mild vs extreme)
   - Data range verification untuk setiap variabel

3. STATISTICAL READINESS:
   - Variables siap untuk analisis deskriptif?
   - Ada transformasi yang diperlukan?
   - Coding issues yang perlu diperbaiki?

4. NEXT STEPS RECOMMENDATION:
   - Prioritas cleaning/preprocessing
   - Analisis awal yang bisa dilakukan
   - Potensi masalah untuk diwaspadai

Format output: Berikan executive summary + detailed findings + action items.
```

### 📝 TRACK B: Qualitative Data Upload

#### Prompt Persiapan Upload Kualitatif
```
Saya akan mengupload data kualitatif untuk analisis dengan AI. Tolong berikan guidance komprehensif untuk persiapan dan upload data kualitatif.

Informasi data:
- Jenis: [Interview transcripts/focus groups/documents/observations]
- Jumlah files: [...]
- Format: [Word/PDF/txt/lainnya]
- Bahasa: [Indonesia/Inggris/campuran]
- Sudah di-anonymize: [Ya/Tidak]
- Total words/pages: [Estimasi]

Yang saya butuhkan:
1. Text preparation best practices
2. Anonymization checklist (jika belum)
3. File organization strategy
4. Metadata yang perlu disertakan
5. Upload sequence recommendation
```

#### Template Post-Upload Content Analysis
```
Saya telah mengupload data kualitatif. Tolong lakukan content overview dan assessment awal:

CONTENT ANALYSIS REQUEST:
1. BASIC OVERVIEW:
   - Total word count dan document statistics
   - Language consistency check
   - Formatting issues identification

2. PRELIMINARY CONTENT MAPPING:
   - Key topics/themes yang muncul (word frequency)
   - Participant/source diversity
   - Data richness assessment

3. ANONYMIZATION VERIFICATION:
   - Ada identitas yang masih terekspos?
   - Consistency dalam pseudonym usage
   - Location/organization references check

4. ANALYSIS READINESS:
   - Data suitable untuk thematic analysis?
   - Segmentation recommendations
   - Coding approach suggestions

5. NEXT STEPS:
   - Immediate cleaning needs
   - Analysis strategy recommendations
   - Potential challenges forecast

Berikan executive summary dan detailed assessment.
```

### 🔄 TRACK C: Mixed Methods Upload Coordination

#### Prompt Koordinasi Mixed Methods
```
Saya memiliki data mixed methods yang perlu diupload dan dikoordinasikan. Tolong berikan strategi upload dan integration plan:

PROJECT DETAILS:
- Design: [Sequential explanatory/exploratory/concurrent]
- Quantitative component: [Deskripsi + ukuran]
- Qualitative component: [Deskripsi + ukuran]  
- Integration timing: [Analysis stage/interpretation/both]
- Participant ID system: [Ada/tidak ada]

COORDINATION NEEDS:
1. Upload sequence strategy
2. Data linking/matching system
3. Integration checkpoints
4. Quality assurance across data types
5. Timeline coordination

Yang saya inginkan:
- Step-by-step upload plan
- Integration verification checklist
- Potential coordination challenges
- Success metrics untuk each stage
```

#### Template Integration Verification
```
Saya telah mengupload both quantitative dan qualitative data untuk mixed methods study. Tolong lakukan integration readiness assessment:

INTEGRATION ASSESSMENT:
1. DATA LINKING VERIFICATION:
   - Participant ID consistency across datasets
   - Sample overlap dan matching rate
   - Missing participants in either dataset

2. METHODOLOGICAL ALIGNMENT:
   - Research questions coverage across methods
   - Variable-theme correspondence
   - Integration points identification

3. QUALITY CONSISTENCY:
   - Comparable data quality across methods
   - Timing synchronization issues
   - Cultural/contextual consistency

4. INTEGRATION STRATEGY:
   - Joint analysis opportunities
   - Sequential analysis workflow
   - Triangulation possibilities

5. READINESS CHECKLIST:
   - Ready untuk concurrent analysis?
   - Sequential analysis priority
   - Integration timeline feasibility

Output format: Integration readiness score + detailed recommendations + risk assessment.
```

### ⚡ AKTIVITAS PRAKTIK UPLOAD (30 menit)

**Langkah-langkah:**
1. **Persiapan** (10 menit): Gunakan prompt persiapan sesuai track
2. **Upload Execution** (10 menit): Upload data dengan guidance AI
3. **Verification** (10 menit): Jalankan post-upload verification prompts

**Troubleshooting Scenarios:**
- File terlalu besar → Chunking strategy
- Format tidak supported → Conversion guidance  
- Data quality issues → Cleaning recommendations
- Upload errors → Step-by-step debugging

---

## ✅ BAGIAN 3: SUCCESS VERIFICATION & NEXT STEPS (10 MENIT)

### Final Verification Prompt
```
Saya telah menyelesaikan sesi 1 workshop data classification dan upload. Tolong lakukan final assessment dan berikan roadmap untuk sesi berikutnya:

SESSION 1 COMPLETION CHECK:
1. Data berhasil diclassify dengan tepat: [Ya/Tidak]
2. Upload strategy berhasil dieksekusi: [Ya/Tidak] 
3. Data quality verification completed: [Ya/Tidak]
4. Track assignment sudah final: [Track A/B/C]

NEXT SESSION PREPARATION:
- Apa yang perlu saya siapkan untuk sesi 2?
- Pre-processing tasks yang bisa dikerjakan sebelum sesi 2?
- Specific prompts yang perlu saya bookmark?
- Potential issues yang perlu diwaspadai?

LEARNING CONSOLIDATION:
- Key prompts yang paling useful dari sesi ini
- Personal notes yang perlu saya catat
- Questions untuk follow-up dengan assistant

Berikan actionable summary untuk bridge ke sesi 2.
```

---

## 🎯 SESSION DELIVERABLES

### Untuk Setiap Peserta:
- [ ] Data berhasil diklasifikasi dengan akurat
- [ ] Data terupload dengan format optimal
- [ ] Quality assessment report lengkap
- [ ] Track assignment yang tepat (A/B/C)
- [ ] Personalized prompt library untuk lanjutan
- [ ] Troubleshooting notes untuk reference

### Untuk Fasilitator:
- [ ] Attendance dan track distribution log
- [ ] Common issues dan solutions documented  
- [ ] Individual progress assessment per peserta
- [ ] Session 2 preparation notes

---

## 🔧 TROUBLESHOOTING GUIDE

### Issue: File Terlalu Besar untuk Upload
**Solution Prompt:**
```
File data saya terlalu besar untuk diupload sekaligus. Tolong berikan strategi chunking yang tepat untuk [tipe data] tanpa kehilangan konteks analisis.

File details: [ukuran, format, content type]
Analysis goals: [tujuan analisis utama]

Yang saya butuhkan:
1. Chunking strategy yang preserve context
2. Sequence upload recommendations  
3. Analysis coordination across chunks
4. Quality assurance checklist
```

### Issue: Data Quality Concerns
**Solution Prompt:**
```
Setelah upload, saya menemukan quality issues pada data. Tolong berikan comprehensive cleaning strategy:

Issues identified: [list specific problems]
Data type: [kuantitatif/kualitatif/mixed]
Timeline constraints: [available time untuk cleaning]

Yang saya butuhkan:
1. Prioritized cleaning tasks
2. Step-by-step cleaning prompts
3. Quality verification methods
4. Impact assessment on analysis plan
```

### Issue: Track Assignment Uncertainty  
**Solution Prompt:**
```
Saya masih ragu dengan track assignment untuk data saya. Tolong lakukan deeper assessment dan berikan clear recommendation:

Data characteristics: [detailed description]
Research objectives: [specific goals]
Analysis experience: [beginner/intermediate]
Time constraints: [available time]

Yang saya butuhkan:
1. Detailed track comparison untuk my case
2. Pros/cons setiap track option
3. Final recommendation dengan reasoning
4. Backup plan jika primary track tidak cocok
```

---

## 📚 NEXT SESSION PREVIEW

**Sesi 2 akan fokus pada:** Track-Specific Data Preparation
- Track A: Statistical assumptions testing
- Track B: Text data standardization  
- Track C: Mixed methods integration framework

**Preparation untuk Sesi 2:**
1. Data sudah clean dan terverifikasi
2. Bookmark key prompts dari sesi 1
3. Siapkan specific research questions
4. Review track-specific requirements

---

*Modul ini dirancang untuk memastikan foundation yang kuat sebelum masuk ke analisis mendalam. Setiap peserta harus menyelesaikan deliverables sesi 1 sebelum lanjut ke sesi berikutnya.*