# Modul 02: Proses Investigasi Forensik Digital

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 02  
**Topik:** Proses Investigasi Forensik Digital  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Menjelaskan fase-fase investigasi forensik digital berdasarkan NIST Framework dan standar internasional
2. Menerapkan prosedur first response dan penanganan TKP digital secara sistematis
3. Melakukan dokumentasi bukti elektronik dan fotografi forensik sesuai standar
4. Memahami dan mengimplementasikan chain of custody dalam penanganan bukti digital
5. Menerapkan teknik preservasi dan pengamanan bukti digital
6. Menyusun Standard Operating Procedure (SOP) investigasi forensik dalam konteks militer
7. Memahami prinsip incident handling dalam kerangka CERT militer dan aspek legal investigasi forensik

---

## 1. Framework Investigasi Forensik Digital

### 1.1 Pendahuluan

> **Investigasi Forensik Digital** adalah proses terstruktur dan sistematis untuk mengidentifikasi, mengumpulkan, memeriksa, menganalisis, dan melaporkan bukti digital yang relevan dengan insiden keamanan atau tindak kejahatan.

Investigasi forensik digital memerlukan pendekatan yang terencana dan metodis. Tanpa kerangka kerja (framework) yang jelas, risiko kehilangan bukti, kontaminasi data, atau penolakan bukti di pengadilan akan meningkat secara signifikan.

| Aspek | Tanpa Framework | Dengan Framework |
|-------|----------------|-----------------|
| **Proses** | Ad-hoc, tidak terstruktur | Sistematis, dapat direproduksi |
| **Bukti** | Risiko kontaminasi tinggi | Integritas terjaga |
| **Legal** | Bukti ditolak di pengadilan | Admissible di pengadilan |
| **Waktu** | Tidak efisien | Terencana dan terukur |
| **Dokumentasi** | Tidak konsisten | Standar dan lengkap |

### 1.2 NIST SP 800-86 Framework

> **NIST SP 800-86** (*Guide to Integrating Forensic Techniques into Incident Response*) adalah panduan yang diterbitkan oleh National Institute of Standards and Technology yang mendefinisikan empat fase utama investigasi forensik digital.

![NIST SP 800-86 Framework](images/p02-01-nist-framework.png)

*Gambar 2.1: Empat fase investigasi forensik digital menurut NIST SP 800-86*

Empat fase NIST SP 800-86:

| Fase | Nama | Deskripsi | Output |
|------|------|-----------|--------|
| **1** | **Collection** (Pengumpulan) | Identifikasi, labeling, perekaman, dan akuisisi data dari sumber yang relevan | Data mentah dan dokumentasi |
| **2** | **Examination** (Pemeriksaan) | Pemrosesan data yang dikumpulkan menggunakan metode forensik otomatis dan manual | Data yang telah diekstrak dan difilter |
| **3** | **Analysis** (Analisis) | Analisis hasil pemeriksaan menggunakan teknik dan metode yang dapat dipertanggungjawabkan | Temuan dan korelasi |
| **4** | **Reporting** (Pelaporan) | Pelaporan hasil analisis termasuk deskripsi tindakan yang dilakukan | Laporan forensik formal |

#### 1.2.1 Fase 1: Collection (Pengumpulan)

Fase pengumpulan mencakup aktivitas identifikasi sumber bukti dan akuisisi data. Dalam konteks militer, fase ini sering kali dilakukan di bawah tekanan waktu dan kondisi operasional yang tidak ideal.

Aktivitas utama dalam fase pengumpulan:

1. **Identifikasi sumber bukti**: Menentukan perangkat dan media yang relevan
2. **Prioritisasi**: Mengurutkan pengumpulan berdasarkan volatilitas (Order of Volatility)
3. **Akuisisi**: Membuat salinan forensik dari bukti digital
4. **Preservasi**: Menjaga integritas bukti yang dikumpulkan
5. **Dokumentasi**: Mencatat setiap langkah pengumpulan

#### 1.2.2 Fase 2: Examination (Pemeriksaan)

Fase pemeriksaan berfokus pada pemrosesan data mentah menjadi informasi yang dapat dianalisis.

Aktivitas utama:
- Ekstraksi data dari forensic image
- Filtering dan reduksi data yang tidak relevan
- Dekompresi dan dekripsi data (jika diperlukan)
- Identifikasi dan pengkategorian artefak

#### 1.2.3 Fase 3: Analysis (Analisis)

Fase analisis melibatkan interpretasi data yang telah diperiksa untuk menjawab pertanyaan investigasi.

Teknik analisis meliputi:
- **Timeline Analysis**: Rekonstruksi kronologi kejadian
- **Correlation Analysis**: Menghubungkan artefak dari berbagai sumber
- **Pattern Analysis**: Identifikasi pola perilaku mencurigakan
- **Attribution Analysis**: Menentukan siapa yang bertanggung jawab

#### 1.2.4 Fase 4: Reporting (Pelaporan)

Fase pelaporan menghasilkan dokumentasi formal dari seluruh proses investigasi.

Elemen laporan forensik:
- Ringkasan eksekutif
- Deskripsi metodologi
- Temuan dan analisis
- Kesimpulan dan rekomendasi
- Lampiran teknis

#### Solved Problem 1 ⭐

**Soal:** Sebutkan empat fase investigasi forensik digital menurut NIST SP 800-86 dan jelaskan tujuan utama masing-masing fase!

**Penyelesaian:**

*Step 1:* Identifikasi keempat fase NIST

*Step 2:* Jelaskan tujuan masing-masing

| Fase | Nama | Tujuan Utama |
|------|------|-------------|
| 1 | **Collection** (Pengumpulan) | Mengidentifikasi dan mengakuisisi data dari sumber bukti yang relevan tanpa mengubah data asli |
| 2 | **Examination** (Pemeriksaan) | Memproses dan mengekstrak data relevan dari kumpulan data mentah |
| 3 | **Analysis** (Analisis) | Menganalisis data yang telah diekstrak untuk menjawab pertanyaan investigasi |
| 4 | **Reporting** (Pelaporan) | Menyusun laporan formal yang mendokumentasikan seluruh proses dan temuan |

**Jawaban:** Empat fase NIST SP 800-86 adalah: (1) Collection — akuisisi data dari sumber relevan, (2) Examination — pemrosesan dan ekstraksi data, (3) Analysis — interpretasi dan korelasi temuan, (4) Reporting — dokumentasi formal seluruh proses dan hasil investigasi.

---

### 1.3 Framework Alternatif

Selain NIST, terdapat beberapa framework investigasi forensik digital lainnya:

| Framework | Pengembang | Jumlah Fase | Keunggulan |
|-----------|-----------|-------------|-----------|
| **NIST SP 800-86** | NIST (AS) | 4 fase | Sederhana, banyak diadopsi |
| **DFRWS** | Digital Forensic Research Workshop | 6 fase | Komprehensif, akademis |
| **ISO/IEC 27037** | ISO | 4 proses | Standar internasional |
| **ACPO Guidelines** | UK Association of Chief Police Officers | 4 prinsip | Berorientasi hukum |
| **RFC 3227** | IETF | 6 langkah | Fokus evidence collection |

#### 1.3.1 DFRWS Framework

DFRWS (Digital Forensic Research Workshop) mendefinisikan enam fase:

1. **Identification**: Mendeteksi dan mengenali insiden
2. **Preservation**: Mengamankan dan melindungi bukti
3. **Collection**: Mengumpulkan dan merekam bukti
4. **Examination**: Memeriksa bukti secara mendalam
5. **Analysis**: Menganalisis temuan untuk menarik kesimpulan
6. **Presentation**: Menyajikan hasil di forum yang sesuai

#### 1.3.2 ISO/IEC 27037

ISO/IEC 27037 mendefinisikan empat proses utama penanganan bukti digital:

1. **Identification**: Pengenalan bukti digital potensial
2. **Collection**: Pengambilan perangkat yang mengandung bukti
3. **Acquisition**: Pembuatan salinan forensik bukti digital
4. **Preservation**: Perlindungan integritas bukti digital

#### Solved Problem 2 ⭐

**Soal:** Apa perbedaan utama antara framework NIST SP 800-86 dan DFRWS dalam konteks investigasi forensik digital?

**Penyelesaian:**

*Step 1:* Bandingkan struktur kedua framework

| Aspek | NIST SP 800-86 | DFRWS |
|-------|---------------|-------|
| **Jumlah Fase** | 4 fase | 6 fase |
| **Penekanan** | Teknis-operasional | Akademis-komprehensif |
| **Identifikasi** | Termasuk dalam Collection | Fase terpisah |
| **Preservasi** | Termasuk dalam Collection | Fase terpisah |
| **Scope** | Incident response integration | Standalone forensics |

*Step 2:* Analisis perbedaan kunci

**Jawaban:** Perbedaan utama adalah: (1) NIST menggunakan 4 fase sementara DFRWS memiliki 6 fase yang lebih granular, (2) NIST menggabungkan identifikasi dan preservasi ke dalam fase Collection, sedangkan DFRWS memisahkannya sebagai fase tersendiri, (3) NIST lebih berorientasi pada integrasi dengan incident response, sementara DFRWS lebih komprehensif sebagai standalone forensic framework.

---

## 2. First Response dan Penanganan TKP Digital

### 2.1 Konsep First Response

> **First Response** dalam forensik digital adalah serangkaian tindakan awal yang dilakukan saat pertama kali tiba di lokasi insiden untuk mengamankan, mendokumentasikan, dan mempreservasi bukti digital.

First response yang tepat sangat menentukan keberhasilan seluruh investigasi. Kesalahan pada tahap ini dapat mengakibatkan bukti rusak, hilang, atau tidak dapat diterima di pengadilan.

![Prosedur First Response](images/p02-02-first-response-procedure.png)

*Gambar 2.2: Alur prosedur first response di TKP digital*

### 2.2 Langkah-Langkah First Response

#### 2.2.1 Fase Awal: Pengamanan Lokasi

Langkah pertama dalam first response adalah mengamankan lokasi insiden:

1. **Perimeter Control**: Batasi akses ke area TKP
2. **Personnel Log**: Catat setiap orang yang masuk/keluar TKP
3. **Threat Assessment**: Evaluasi risiko keamanan fisik dan digital
4. **Authorization**: Pastikan otorisasi legal untuk melakukan investigasi

#### 2.2.2 Dokumentasi Kondisi Awal

Sebelum menyentuh perangkat apapun:

| Aktivitas | Tools | Tujuan |
|-----------|-------|--------|
| **Foto panorama** | Kamera digital | Mendokumentasikan tata letak ruangan |
| **Foto close-up** | Kamera digital | Mendokumentasikan setiap perangkat |
| **Catat status perangkat** | Formulir | Mencatat kondisi hidup/mati, LED, layar |
| **Identifikasi koneksi** | Formulir | Mencatat kabel, jaringan, USB terhubung |
| **Sketsa lokasi** | Kertas/tablet | Membuat denah TKP dan posisi perangkat |

#### 2.2.3 Penanganan Perangkat Berdasarkan Status

**Jika Sistem MENYALA (Live System):**
1. JANGAN matikan komputer secara langsung
2. Dokumentasikan apa yang tampil di layar (screenshot/foto)
3. Catat waktu sistem dan bandingkan dengan waktu aktual
4. Capture volatile data (RAM, proses, koneksi jaringan)
5. Pertimbangkan live acquisition sebelum mematikan

**Jika Sistem MATI (Dead System):**
1. JANGAN menyalakan komputer
2. Foto semua sisi perangkat termasuk port dan konektor
3. Cabut kabel power dari **belakang komputer** (bukan dari stop kontak)
4. Label semua kabel dan port
5. Kemas dengan anti-statis bag

#### Solved Problem 3 ⭐

**Soal:** Jelaskan mengapa komputer yang sudah dalam keadaan mati TIDAK BOLEH dinyalakan selama proses first response!

**Penyelesaian:**

*Step 1:* Identifikasi risiko menyalakan komputer

*Step 2:* Analisis dampak terhadap bukti digital

| Risiko | Penjelasan |
|--------|-----------|
| **Modifikasi timestamp** | OS akan mengubah berbagai timestamp file saat booting |
| **Overwrite data** | Proses startup menulis data ke disk, berpotensi menimpa bukti |
| **Autorun malware** | Malware atau script destruktif mungkin berjalan otomatis |
| **Perubahan registry** | Windows memodifikasi registry entries saat startup |
| **Log modification** | Event logs berubah dengan aktivitas startup baru |

**Jawaban:** Komputer yang mati tidak boleh dinyalakan karena: (1) Proses booting mengubah banyak timestamp dan metadata file, (2) Data baru ditulis ke disk saat startup yang berpotensi menimpa bukti terhapus, (3) Malware atau script destruktif mungkin aktif otomatis dan menghancurkan bukti, (4) Aktivitas startup memodifikasi registry dan event log yang merupakan bukti penting.

---

#### Solved Problem 4 ⭐⭐

**Soal:** Seorang first responder tiba di sebuah ruang server Kodam yang mengalami insiden keamanan siber. Ditemukan tiga server dalam keadaan menyala dan satu laptop dalam keadaan tertutup (sleep mode). Jelaskan prosedur penanganan yang tepat untuk setiap perangkat!

**Penyelesaian:**

*Step 1:* Klasifikasi status perangkat

| Perangkat | Status | Prioritas |
|-----------|--------|-----------|
| Server 1-3 | Menyala (live) | Tinggi — volatile data ada |
| Laptop | Sleep mode (semi-live) | Tinggi — RAM masih aktif |

*Step 2:* Tentukan prosedur untuk setiap perangkat

**Server 1-3 (Live System):**
1. Dokumentasi visual: foto layar, LED indicator, koneksi
2. Catat waktu sistem vs waktu aktual
3. Capture volatile data menggunakan tools forensik:
   - RAM dump (menggunakan FTK Imager atau DumpIt)
   - Daftar proses aktif
   - Koneksi jaringan aktif
   - Pengguna yang sedang login
4. Pertimbangkan apakah server boleh dimatikan (konsultasi dengan pimpinan operasi)
5. Jika dimatikan: cabut kabel power dari belakang server

**Laptop (Sleep Mode):**
1. JANGAN buka tutup laptop (dapat trigger wake dan menjalankan script)
2. Dokumentasi kondisi (LED, indikator power)
3. Jika memungkinkan, capture RAM melalui koneksi jaringan atau FireWire/Thunderbolt tanpa membuka tutup
4. Jika tidak memungkinkan remote capture: buka tutup, capture volatile data, lalu shutdown

**Jawaban:** Untuk server yang menyala: prioritaskan capture volatile data (RAM, proses, koneksi jaringan), lalu dokumentasi visual dan catat waktu. Untuk laptop sleep mode: hindari membuka tutup laptop tanpa persiapan capture volatile data, karena membuka tutup dapat memicu script atau proses yang mengubah/menghancurkan bukti.

---

### 2.3 Penanganan TKP Digital dalam Konteks Militer

Dalam konteks militer Indonesia, penanganan TKP digital memiliki pertimbangan tambahan:

| Aspek | Konteks Sipil | Konteks Militer |
|-------|--------------|----------------|
| **Otorisasi** | Surat perintah pengadilan | Perintah komandan satuan + aspek legal militer |
| **Akses** | Area publik/privat | Fasilitas militer dengan klasifikasi keamanan |
| **Personel** | Investigator sipil | Tim forensik militer (Pusintelad/BSSN) |
| **Informasi** | Variasi klasifikasi | Sering melibatkan informasi rahasia negara |
| **Urgency** | Sesuai prosedur standar | Dapat dipercepat untuk kebutuhan operasi |

#### 2.3.1 Pertimbangan Khusus Lingkungan Militer

1. **Security Clearance**: Investigator harus memiliki izin keamanan yang sesuai
2. **Classified Material**: Prosedur khusus untuk penanganan materi berklasifikasi
3. **Operational Security (OPSEC)**: Menjaga kerahasiaan operasi selama investigasi
4. **Command Authority**: Koordinasi dengan rantai komando
5. **Multi-Domain**: Bukti mungkin melibatkan sistem lintas matra (AD, AL, AU)

#### Solved Problem 5 ⭐⭐

**Soal:** Dalam suatu operasi, Tim CERT Kodam XIII/Merdeka menemukan indikasi intrusi pada sistem komando dan kendali. Jelaskan faktor-faktor yang membedakan first response dalam lingkungan militer dibandingkan lingkungan sipil!

**Penyelesaian:**

*Step 1:* Identifikasi faktor-faktor pembeda

*Step 2:* Analisis implikasi masing-masing faktor

| Faktor | Implikasi di Lingkungan Militer |
|--------|-------------------------------|
| **Klasifikasi Informasi** | Bukti mungkin bersifat rahasia; penanganan memerlukan personel dengan security clearance |
| **Chain of Command** | Keputusan investigasi harus melalui rantai komando; koordinasi dengan Dansatsiber atau Kadenpom |
| **Operational Continuity** | Sistem mungkin tidak boleh dimatikan karena mendukung operasi aktif |
| **Cross-Domain** | Insiden mungkin melibatkan sistem beberapa matra sekaligus |
| **Rules of Engagement** | Prosedur respons terikat pada aturan pelibatan siber militer |
| **National Security** | Implikasi terhadap keamanan nasional memerlukan eskalasi ke level strategis |

**Jawaban:** Faktor pembeda utama first response militer meliputi: (1) Klasifikasi informasi yang mensyaratkan security clearance bagi investigator, (2) Kebutuhan koordinasi dengan chain of command, (3) Pertimbangan operational continuity dimana sistem mungkin tidak boleh dimatikan, (4) Potensi insiden lintas matra, dan (5) Implikasi keamanan nasional yang memerlukan eskalasi cepat.

---

## 3. Dokumentasi Bukti Elektronik

### 3.1 Pentingnya Dokumentasi

> **Dokumentasi Forensik** adalah pencatatan terperinci dan sistematis dari setiap tindakan yang dilakukan selama proses investigasi forensik digital, mulai dari identifikasi awal hingga penyajian di pengadilan.

Dokumentasi yang baik memastikan:
- **Reproducibility**: Investigator lain dapat mengikuti dan memverifikasi langkah-langkah
- **Admissibility**: Bukti dapat diterima di pengadilan atau tribunal militer
- **Accountability**: Setiap tindakan dapat dipertanggungjawabkan
- **Continuity**: Investigasi dapat dilanjutkan oleh investigator lain jika diperlukan

### 3.2 Jenis-Jenis Dokumentasi Forensik

![Jenis Dokumentasi Forensik](images/p02-03-documentation-types.png)

*Gambar 2.3: Jenis-jenis dokumentasi dalam investigasi forensik digital*

| Jenis Dokumentasi | Deskripsi | Kapan Dibuat |
|-------------------|-----------|-------------|
| **Case Notes** | Catatan kronologis seluruh aktivitas investigasi | Sepanjang investigasi |
| **Chain of Custody Form** | Formulir pencatatan perpindahan bukti | Saat bukti berpindah tangan |
| **Acquisition Log** | Catatan proses akuisisi dan hash values | Saat akuisisi data |
| **Forensic Photography** | Foto dokumentasi TKP dan perangkat | Saat first response |
| **Scene Sketch** | Sketsa atau denah TKP digital | Saat first response |
| **Forensic Report** | Laporan lengkap hasil investigasi | Akhir investigasi |

### 3.3 Fotografi Forensik

Fotografi forensik digital mengikuti prinsip yang sama dengan fotografi forensik tradisional:

**Tiga Level Fotografi:**

| Level | Nama | Deskripsi | Contoh |
|-------|------|-----------|--------|
| **1** | Panoramic/Overview | Foto keseluruhan ruangan/lokasi | Seluruh ruang server |
| **2** | Mid-range | Foto perangkat dengan konteks lingkungannya | Server dalam rak dengan kabel terlihat |
| **3** | Close-up/Detail | Foto detail spesifik | Serial number, port, layar monitor |

**Best Practices Fotografi Forensik:**
- Gunakan skala/ruler untuk foto close-up
- Catat timestamp dan lokasi setiap foto
- Foto tanpa flash dan dengan flash untuk variasi pencahayaan
- Pastikan foto menunjukkan hubungan spasial antar perangkat
- Simpan foto dalam format RAW jika memungkinkan

#### Solved Problem 6 ⭐

**Soal:** Sebutkan tiga level fotografi forensik dan berikan contoh penggunaannya dalam penanganan TKP digital!

**Penyelesaian:**

*Step 1:* Identifikasi tiga level fotografi

*Step 2:* Berikan contoh spesifik untuk setiap level

| Level | Nama | Contoh di TKP Digital |
|-------|------|-----------------------|
| **1** | **Overview/Panoramic** | Foto seluruh ruangan kerja tersangka, menunjukkan posisi meja, komputer, dan perangkat lain |
| **2** | **Mid-range** | Foto workstation tersangka termasuk monitor, CPU, keyboard, dan kabel yang terhubung |
| **3** | **Close-up/Detail** | Foto serial number di belakang CPU, tampilan layar yang sedang aktif, port USB yang terhubung |

**Jawaban:** Tiga level fotografi forensik adalah: (1) Overview — foto keseluruhan lokasi untuk konteks, (2) Mid-range — foto perangkat dengan lingkungan sekitarnya, (3) Close-up — foto detail spesifik seperti serial number, layar aktif, dan koneksi.

---

### 3.4 Catatan Investigasi (Case Notes)

Case notes harus mengikuti prinsip **contemporaneous notes** — catatan yang dibuat pada saat atau segera setelah kejadian.

Elemen wajib dalam case notes:

| Elemen | Format | Contoh |
|--------|--------|--------|
| **Tanggal dan Waktu** | YYYY-MM-DD HH:MM:SS (24h) | 2026-02-07 14:30:25 |
| **Nama Investigator** | Nama lengkap dan NRP/NIP | Kapten Inf. Ardi Pratama, NRP 1234567 |
| **Lokasi** | Alamat lengkap dan deskripsi | Ruang Server Lt.3, Makodam XIII/Merdeka |
| **Aktivitas** | Deskripsi detail tindakan | "Melakukan capture RAM menggunakan FTK Imager v4.7" |
| **Temuan** | Hasil observasi atau akuisisi | "Ditemukan 3 file mencurigakan di folder C:\Temp" |
| **Hash Values** | MD5 dan/atau SHA-256 | SHA-256: a1b2c3d4... |

#### Solved Problem 7 ⭐⭐

**Soal:** Buatlah contoh case notes untuk skenario berikut: Seorang investigator forensik di Lantamal V Surabaya ditugaskan untuk menangani laptop yang diduga digunakan untuk mengakses situs terlarang oleh anggota militer.

**Penyelesaian:**

*Step 1:* Identifikasi informasi yang diperlukan

*Step 2:* Susun case notes sesuai format standar

```
CASE NOTES
==========
Case Number      : LTM-V/DFIR/2026/002
Case Title       : Investigasi Akses Situs Terlarang
Classification   : TERBATAS

Tanggal/Waktu    : 2026-02-07 09:00 WIB
Investigator     : Lettu Laut (T) Budi Santoso, NRP 7654321
Lokasi           : Kantor Lantamal V Surabaya, Gedung B Lt. 2

09:00 - Tiba di lokasi. Menerima pengarahan dari 
        Danlanal tentang kasus.
09:15 - Masuk ke ruangan tersangka didampingi 2 saksi 
        (Pelda Mar. Ahmad, Serma Mar. Rudi).
09:17 - Laptop teridentifikasi: Lenovo ThinkPad T480, 
        S/N: PF1XXXXX. Status: MENYALA, layar terkunci.
09:18 - Foto panoramic ruangan (IMG_001-003)
09:20 - Foto mid-range meja kerja (IMG_004-006)
09:22 - Foto close-up laptop: serial number, port, 
        LED indicator (IMG_007-012)
09:25 - Memulai capture RAM menggunakan FTK Imager 4.7
09:47 - RAM capture selesai. File: ram_dump.mem (8GB)
        SHA-256: [hash value]
09:50 - Mematikan laptop: cabut baterai, cabut kabel power
09:52 - Label bukti: EVD-001 (Laptop), EVD-002 (Charger)
09:55 - Kemas dalam anti-static bag
10:00 - Serah terima ke Kadenpom: Chain of Custody 
        Form ditandatangani
```

**Jawaban:** Case notes harus mencakup: informasi kasus, identitas investigator, kronologi detail dengan timestamp, deskripsi setiap tindakan, identifikasi perangkat (merek, model, serial number), hash values untuk setiap akuisisi, dan pencatatan serah terima bukti.

---

## 4. Chain of Custody

### 4.1 Definisi dan Prinsip

> **Chain of Custody (Rantai Penjagaan Bukti)** adalah dokumentasi kronologis yang mencatat setiap perpindahan, penanganan, dan penyimpanan bukti dari saat pengumpulan hingga penyajian di pengadilan.

Chain of custody memastikan bahwa bukti digital tetap dalam kondisi yang sama seperti saat pertama kali dikumpulkan. Jika chain of custody terputus, bukti dapat ditolak di pengadilan atau tribunal militer.

#### 4.1.1 Prinsip Chain of Custody

| Prinsip | Deskripsi |
|---------|-----------|
| **Continuity** | Tidak ada celah waktu dalam pencatatan penanganan bukti |
| **Integrity** | Bukti tidak diubah, ditambah, atau dikurangi |
| **Accountability** | Setiap orang yang menangani bukti tercatat dan bertanggung jawab |
| **Security** | Bukti disimpan di lokasi aman dengan akses terkontrol |
| **Documentation** | Setiap perpindahan didokumentasikan secara tertulis |

### 4.2 Komponen Chain of Custody Form

![Chain of Custody Form](images/p02-04-chain-of-custody.png)

*Gambar 2.4: Contoh formulir chain of custody untuk bukti digital*

Elemen wajib dalam formulir chain of custody:

**Bagian 1: Informasi Kasus**
- Nomor kasus
- Tanggal dan waktu
- Nama investigator penanggung jawab
- Unit/satuan

**Bagian 2: Deskripsi Bukti**
- Nomor bukti (evidence number)
- Jenis perangkat/media
- Merek, model, serial number
- Kondisi saat diterima
- Hash value (MD5/SHA-256)

**Bagian 3: Catatan Perpindahan**
- Tanggal dan waktu perpindahan
- Dari siapa (nama, jabatan, tanda tangan)
- Kepada siapa (nama, jabatan, tanda tangan)
- Tujuan perpindahan
- Kondisi bukti saat perpindahan

#### Solved Problem 8 ⭐

**Soal:** Mengapa chain of custody sangat penting dalam investigasi forensik digital?

**Penyelesaian:**

*Step 1:* Identifikasi fungsi chain of custody

*Step 2:* Analisis konsekuensi jika chain of custody terputus

| Fungsi | Konsekuensi Jika Gagal |
|--------|----------------------|
| **Menjamin integritas bukti** | Bukti dapat dipertanyakan keasliannya |
| **Membuktikan tidak ada tampering** | Pihak lawan dapat mengklaim bukti telah dimanipulasi |
| **Audit trail** | Tidak ada jejak siapa yang menangani bukti |
| **Legal admissibility** | Bukti ditolak di pengadilan |
| **Akuntabilitas** | Tidak ada pihak yang bertanggung jawab atas bukti |

**Jawaban:** Chain of custody penting karena: (1) Menjamin bahwa bukti tidak diubah sejak pengumpulan, (2) Menyediakan dokumentasi lengkap setiap perpindahan bukti, (3) Memastikan bukti dapat diterima di pengadilan, (4) Memberikan akuntabilitas kepada setiap pihak yang menangani bukti.

---

#### Solved Problem 9 ⭐⭐

**Soal:** Dalam sebuah kasus investigasi di Brigif Linud 3/Kostrad, USB drive berisi bukti digital diserahkan oleh investigator kepada petugas lab forensik tanpa mengisi formulir chain of custody. Dua hari kemudian, USB drive tersebut dikembalikan dengan hasil analisis. Analisis dampak kegagalan ini terhadap validitas bukti!

**Penyelesaian:**

*Step 1:* Identifikasi pelanggaran chain of custody

| Pelanggaran | Detail |
|-------------|--------|
| Tidak ada dokumentasi serah terima awal | Investigator → Petugas lab |
| Tidak ada pencatatan kondisi bukti | Kondisi USB saat diterima lab tidak tercatat |
| Gap dalam timeline penanganan | 2 hari tanpa dokumentasi siapa yang bertanggung jawab |
| Tidak ada verifikasi integritas | Hash value tidak diverifikasi saat serah terima |

*Step 2:* Analisis dampak terhadap validitas bukti

1. **Legal**: Pihak pembela dapat mempertanyakan integritas bukti di tribunal militer
2. **Teknis**: Tidak ada bukti bahwa data di USB tidak dimodifikasi selama di lab
3. **Prosedural**: Pelanggaran SOP yang dapat mengakibatkan sanksi administratif
4. **Investigasi**: Seluruh temuan dari USB drive berpotensi tidak admissible

*Step 3:* Tindakan perbaikan

1. Segera buat catatan retrospektif dengan penjelasan gap
2. Verifikasi hash value USB drive saat ini dengan hash saat akuisisi awal
3. Kumpulkan keterangan dari semua pihak yang terlibat
4. Laporkan insiden kepada pimpinan investigasi

**Jawaban:** Kegagalan mengisi chain of custody form menyebabkan: (1) Gap dokumentasi selama 2 hari membuat pihak pembela dapat mengklaim bukti dimanipulasi, (2) Tidak ada verifikasi integritas data saat perpindahan, (3) Bukti berpotensi tidak diterima di tribunal militer. Tindakan perbaikan meliputi pembuatan catatan retrospektif, verifikasi hash, dan pengumpulan keterangan saksi.

---

## 5. Preservasi dan Pengamanan Bukti Digital

### 5.1 Prinsip Preservasi

> **Preservasi Bukti Digital** adalah serangkaian tindakan untuk melindungi bukti digital dari modifikasi, kerusakan, atau kehilangan selama proses investigasi.

Preservasi mencakup aspek fisik (perlindungan perangkat keras) dan logis (perlindungan data/informasi).

### 5.2 Teknik Preservasi Bukti Digital

| Teknik | Deskripsi | Tools |
|--------|-----------|-------|
| **Forensic Imaging** | Membuat salinan bit-by-bit dari media penyimpanan | FTK Imager, dd |
| **Hash Verification** | Menghitung dan memverifikasi hash untuk integritas | HashMyFiles, md5sum |
| **Write Blocking** | Mencegah penulisan ke media bukti asli | Hardware/software write blocker |
| **Secure Storage** | Menyimpan bukti di lokasi aman dan terkontrol | Evidence locker |
| **Environmental Control** | Menjaga kondisi lingkungan penyimpanan | Suhu, kelembaban terkontrol |

### 5.3 Forensic Imaging

> **Forensic Image** adalah salinan bit-by-bit (exact duplicate) dari seluruh media penyimpanan, termasuk area yang tidak terisi (unallocated space), deleted files, dan file system metadata.

Jenis forensic image:

| Jenis | Format | Deskripsi | Tools |
|-------|--------|-----------|-------|
| **Physical Image** | dd, E01, AFF4 | Salinan seluruh disk termasuk unallocated space | FTK Imager, dd |
| **Logical Image** | L01, zip | Salinan hanya file dan folder yang ada | FTK Imager |
| **Targeted Collection** | - | Hanya file tertentu yang relevan | Robocopy, xcopy |

#### 5.3.1 Write Blocker

> **Write Blocker** adalah perangkat keras atau perangkat lunak yang mencegah penulisan data ke media penyimpanan bukti, sehingga menjaga integritas bukti asli.

| Tipe | Hardware | Software |
|------|----------|----------|
| **Cara Kerja** | Memblok sinyal write secara fisik | Memodifikasi driver storage |
| **Kelebihan** | Lebih reliabel, tidak bergantung OS | Murah, mudah digunakan |
| **Kekurangan** | Mahal, perlu dibawa fisik | Bergantung pada OS |
| **Contoh** | Tableau T356789, WiebeTech | SAFE Block, Windows Registry |

#### Solved Problem 10 ⭐

**Soal:** Jelaskan perbedaan antara physical image dan logical image! Kapan masing-masing digunakan?

**Penyelesaian:**

*Step 1:* Bandingkan kedua jenis image

| Aspek | Physical Image | Logical Image |
|-------|---------------|---------------|
| **Cakupan** | Seluruh disk (bit-by-bit) | Hanya file dan folder aktif |
| **Unallocated Space** | Termasuk | Tidak termasuk |
| **Deleted Files** | Dapat di-recover | Tidak tersedia |
| **Ukuran** | Sama dengan ukuran disk asli | Lebih kecil |
| **Waktu** | Lebih lama | Lebih cepat |

*Step 2:* Identifikasi penggunaan masing-masing

**Jawaban:** Physical image menyalin seluruh disk termasuk unallocated space dan deleted files, digunakan untuk investigasi menyeluruh. Logical image hanya menyalin file dan folder aktif, digunakan ketika waktu terbatas atau hanya perlu file tertentu. Dalam investigasi forensik militer, physical image lebih direkomendasikan untuk memastikan tidak ada bukti yang terlewat.

---

### 5.4 Hash Verification

> **Hash Verification** adalah proses menghitung nilai hash (sidik jari digital) dari data untuk membuktikan bahwa data tidak berubah.

Algoritma hash yang umum digunakan:

| Algoritma | Panjang Output | Status | Penggunaan |
|-----------|---------------|--------|-----------|
| **MD5** | 128-bit (32 hex) | Deprecated untuk keamanan, masih untuk integritas | Legacy, referensi cepat |
| **SHA-1** | 160-bit (40 hex) | Deprecated | Transisi |
| **SHA-256** | 256-bit (64 hex) | Direkomendasikan | Standar saat ini |
| **SHA-512** | 512-bit (128 hex) | Sangat aman | High-security |

#### Solved Problem 11 ⭐⭐

**Soal:** Seorang investigator melakukan forensic imaging terhadap hard disk tersangka. Hash SHA-256 saat akuisisi adalah `a1b2c3d4...`. Setelah 6 bulan di evidence storage, hash diverifikasi dan menunjukkan `a1b2c3d4...` (sama). Apa makna dari hasil ini dan mengapa hashing penting?

**Penyelesaian:**

*Step 1:* Interpretasi hasil hash verification

Hash identik menunjukkan bahwa:
- Data tidak berubah selama 6 bulan penyimpanan
- Tidak ada modifikasi, penambahan, atau penghapusan data
- Integritas bukti terjaga sempurna

*Step 2:* Analisis pentingnya hashing

| Aspek | Penjelasan |
|-------|-----------|
| **Integritas** | Membuktikan data identical bit-by-bit |
| **Non-repudiation** | Tidak ada pihak yang dapat menyangkal keaslian data |
| **Legal proof** | Bukti admissible di pengadilan |
| **Tamper detection** | Perubahan sekecil 1 bit akan menghasilkan hash berbeda total |

**Jawaban:** Hash SHA-256 yang identik membuktikan bahwa forensic image tidak berubah selama 6 bulan penyimpanan — integritas bukti terjaga. Hashing penting karena: (1) Menjadi bukti matematis bahwa data tidak dimanipulasi, (2) Perubahan 1 bit saja akan menghasilkan hash yang sama sekali berbeda (avalanche effect), (3) Memenuhi persyaratan legal untuk admissibility bukti digital.

---

## 6. Standard Operating Procedure (SOP) Investigasi Forensik Militer

### 6.1 Konsep SOP

> **Standard Operating Procedure (SOP)** adalah dokumen yang berisi instruksi langkah demi langkah untuk melaksanakan operasi atau prosedur tertentu secara konsisten dan sesuai standar.

SOP investigasi forensik dalam konteks militer memiliki karakteristik tambahan:
- Mengikuti hierarki komando militer
- Mempertimbangkan klasifikasi keamanan
- Selaras dengan doktrin operasi militer
- Terintegrasi dengan sistem pelaporan militer

### 6.2 Struktur SOP Forensik Militer

![Struktur SOP Forensik Militer](images/p02-05-sop-structure.png)

*Gambar 2.5: Struktur Standard Operating Procedure forensik militer*

Tahapan SOP investigasi forensik militer:

| No | Tahap | Penanggung Jawab | Output |
|----|-------|-----------------|--------|
| 1 | Penerimaan Kasus | Unit yang melaporkan | Laporan insiden awal |
| 2 | Otorisasi & Penugasan | Komandan satuan | Surat perintah investigasi |
| 3 | Persiapan Tim & Peralatan | Ketua tim forensik | Checklist kesiapan |
| 4 | Penanganan TKP | First responder | Case notes, foto TKP |
| 5 | Akuisisi Bukti Digital | Forensic examiner | Forensic images, hash logs |
| 6 | Pemeriksaan & Analisis | Forensic analyst | Temuan dan korelasi |
| 7 | Dokumentasi & Pelaporan | Ketua tim forensik | Laporan forensik lengkap |
| 8 | Penyerahan Hasil | Ketua tim forensik | Bukti dan laporan ke komandan |

### 6.3 Forensic Readiness dalam SOP Militer

Forensic readiness (kesiapan forensik) dalam organisasi militer mencakup:

1. **Personnel**: Personel terlatih dan memiliki sertifikasi
2. **Equipment**: Peralatan forensik yang terawat dan terkalibrasi
3. **Procedures**: SOP yang terdokumentasi dan dilatih secara berkala
4. **Legal Framework**: Landasan hukum untuk melakukan investigasi
5. **Storage**: Fasilitas penyimpanan bukti yang aman

#### Solved Problem 12 ⭐⭐

**Soal:** Rancang kerangka SOP singkat untuk Tim Forensik Digital di sebuah Kodam yang baru dibentuk! Sertakan minimal 5 tahap utama dengan penanggung jawab masing-masing.

**Penyelesaian:**

*Step 1:* Identifikasi tahap-tahap kritis dalam SOP forensik militer

*Step 2:* Tentukan penanggung jawab sesuai struktur organisasi militer

| Tahap | Aktivitas | Penanggung Jawab | Dokumen Output |
|-------|-----------|-----------------|---------------|
| **1. Inisiasi** | Penerimaan laporan insiden, verifikasi, dan eskalasi | Staf Intel Kodam | Formulir Laporan Insiden |
| **2. Otorisasi** | Penerbitan surat perintah investigasi dan penunjukan tim | Asisten Intel (Asintel) Kodam | Surat Perintah Investigasi |
| **3. Mobilisasi** | Persiapan tim, peralatan, dan koordinasi logistik | Ketua Tim Forensik | Checklist Kesiapan Tim |
| **4. Eksekusi** | First response, akuisisi, pemeriksaan, dan analisis | Tim Forensik | Case Notes, Forensic Images |
| **5. Pelaporan** | Penyusunan laporan dan penyerahan bukti | Ketua Tim Forensik | Laporan Forensik Lengkap |
| **6. Evaluasi** | Review proses, lessons learned, perbaikan SOP | Asintel Kodam | After Action Review |

**Jawaban:** SOP Tim Forensik Digital Kodam minimal mencakup: (1) Inisiasi — penerimaan dan verifikasi laporan, (2) Otorisasi — penerbitan Sprin oleh Asintel, (3) Mobilisasi — persiapan tim dan peralatan, (4) Eksekusi — pelaksanaan investigasi forensik, (5) Pelaporan — dokumentasi dan penyerahan hasil, (6) Evaluasi — lessons learned untuk perbaikan SOP.

---

## 7. Incident Handling dan CERT Militer

### 7.1 Konsep Incident Handling

> **Incident Handling** (Penanganan Insiden) adalah proses terstruktur untuk mendeteksi, menganalisis, membatasi dampak, dan memulihkan sistem dari insiden keamanan siber.

Incident handling berbeda dari forensik digital, namun keduanya saling terkait:

| Aspek | Incident Handling | Digital Forensics |
|-------|------------------|-------------------|
| **Tujuan Utama** | Pemulihan operasional | Pengumpulan bukti |
| **Prioritas** | Menghentikan serangan | Menjaga integritas bukti |
| **Waktu** | Secepat mungkin | Teliti dan menyeluruh |
| **Scope** | Seluruh infrastruktur | Perangkat terdampak |
| **Output** | Sistem pulih | Laporan forensik |

### 7.2 NIST Incident Response Lifecycle

![NIST Incident Response Lifecycle](images/p02-06-incident-response-lifecycle.png)

*Gambar 2.6: Siklus hidup incident response menurut NIST SP 800-61*

NIST SP 800-61 mendefinisikan empat fase incident response:

| Fase | Nama | Aktivitas Kunci |
|------|------|----------------|
| **1** | **Preparation** (Persiapan) | Membangun kapabilitas, menyiapkan tools, melatih tim |
| **2** | **Detection & Analysis** (Deteksi & Analisis) | Mendeteksi insiden, menentukan scope dan severity |
| **3** | **Containment, Eradication & Recovery** | Membatasi dampak, menghapus ancaman, memulihkan sistem |
| **4** | **Post-Incident Activity** (Pasca-Insiden) | Lessons learned, perbaikan, dokumentasi |

### 7.3 CERT/CSIRT Militer Indonesia

> **CERT (Computer Emergency Response Team)** atau **CSIRT (Computer Security Incident Response Team)** adalah tim yang bertanggung jawab untuk merespons insiden keamanan siber dalam suatu organisasi.

Dalam konteks pertahanan Indonesia:

| Organisasi | Peran | Cakupan |
|-----------|-------|---------|
| **BSSN** | Koordinator keamanan siber nasional | Seluruh infrastruktur kritis nasional |
| **Pusdiksus TNI** | Pelatihan dan pengembangan kapabilitas siber | Seluruh matra TNI |
| **Satsiber TNI** | Operasi siber militer | Pertahanan siber militer |
| **CERT Kodam** | Respons insiden tingkat regional | Wilayah pertahanan Kodam |

### 7.4 Integrasi Forensik Digital dengan Incident Response

Forensik digital terintegrasi dalam setiap fase incident response:

| Fase IR | Peran Forensik Digital |
|---------|----------------------|
| **Preparation** | Menyiapkan tools forensik, melatih kemampuan forensik |
| **Detection** | Analisis artefak untuk konfirmasi insiden |
| **Containment** | Capture volatile data sebelum isolasi sistem |
| **Eradication** | Analisis root cause dan indicators of compromise |
| **Recovery** | Verifikasi bahwa ancaman telah dihilangkan |
| **Post-Incident** | Laporan forensik lengkap untuk lessons learned |

#### Solved Problem 13 ⭐⭐

**Soal:** Tim Satsiber TNI mendeteksi adanya beacon traffic dari workstation di markas besar yang berkomunikasi dengan server C2 (Command and Control) di luar negeri. Jelaskan bagaimana incident handling dan forensik digital bekerja bersama dalam menangani kasus ini!

**Penyelesaian:**

*Step 1:* Petakan aktivitas incident handling dan forensik pada setiap fase

| Fase | Incident Handling | Forensik Digital |
|------|------------------|-----------------|
| **Detection** | IDS/IPS mendeteksi beacon traffic anomali | Capture network traffic (PCAP) untuk analisis |
| **Analysis** | Tentukan severity: HIGH (C2 communication) | Analisis PCAP: frekuensi beacon, protokol, IP tujuan |
| **Containment** | Isolasi workstation dari jaringan | Capture RAM dump sebelum isolasi; image hard disk |
| **Eradication** | Hapus malware/backdoor dari workstation | Analisis malware: jenis, capabilities, persistence mechanism |
| **Recovery** | Rebuild workstation, monitor kembali | Verifikasi clean state; buat baseline hash |
| **Post-Incident** | Lessons learned, update rules IDS/IPS | Laporan forensik lengkap dengan IOC (Indicators of Compromise) |

*Step 2:* Identifikasi titik kritis koordinasi

Titik kritis:
1. **Sebelum containment**: Forensik harus capture volatile data terlebih dahulu
2. **Selama eradication**: Forensik menyediakan IOC untuk memastikan semua ancaman dihapus
3. **Post-incident**: Forensik menyediakan informasi untuk pencegahan insiden serupa

**Jawaban:** Incident handling dan forensik digital saling melengkapi: IR berfokus pada pemulihan operasional sementara forensik berfokus pada pengumpulan bukti. Kunci koordinasi adalah: (1) Forensik capture volatile data SEBELUM containment, (2) Forensik menyediakan IOC untuk eradication, (3) Laporan forensik menjadi input untuk lessons learned dan perbaikan sistem.

---

#### Solved Problem 14 ⭐⭐⭐

**Soal:** Sebagai Ketua Tim Forensik di Satsiber TNI, Anda ditugaskan untuk menyusun prosedur integrasi forensik digital dengan incident response untuk seluruh jajaran TNI. Rancang framework integrasi yang mencakup: (a) pembagian peran, (b) prioritas konflik antara recovery dan evidence preservation, dan (c) mekanisme eskalasi!

**Penyelesaian:**

*Step 1:* Rancang pembagian peran

| Peran | Tanggung Jawab | Subordinasi |
|-------|---------------|-------------|
| **Incident Commander (IC)** | Pengambil keputusan utama | Kadensatsiber TNI |
| **IR Team Lead** | Koordinasi respons dan pemulihan | Di bawah IC |
| **Forensic Team Lead** | Koordinasi pengumpulan dan analisis bukti | Di bawah IC, paralel dengan IR Lead |
| **IR Analyst** | Analisis dan containment ancaman | Di bawah IR Lead |
| **Forensic Examiner** | Akuisisi dan analisis bukti digital | Di bawah Forensic Lead |
| **Legal Advisor** | Aspek hukum dan chain of custody | Advisory ke IC |

*Step 2:* Tentukan aturan prioritas recovery vs. evidence preservation

| Kondisi | Prioritas | Alasan |
|---------|-----------|--------|
| Ancaman aktif terhadap keselamatan jiwa | Recovery DULU | Keselamatan di atas segalanya |
| Ancaman terhadap sistem senjata/C2 aktif | Recovery DULU, forensik paralel | Operational necessity |
| Intrusi tanpa ancaman langsung | Evidence preservation DULU | Bukti lebih penting jika tidak urgent |
| Post-incident (ancaman sudah contained) | Evidence preservation | Fokus pada investigasi mendalam |

*Step 3:* Rancang mekanisme eskalasi

| Level | Trigger | Eskalasi Ke | Response Time |
|-------|---------|-------------|---------------|
| **Level 1 (Low)** | Malware terdeteksi pada 1 workstation | Tim CERT Kodam | 4 jam |
| **Level 2 (Medium)** | Intrusi pada server non-kritis | Satsiber Matra | 2 jam |
| **Level 3 (High)** | Intrusi pada sistem C2 atau komunikasi | Satsiber TNI | 1 jam |
| **Level 4 (Critical)** | Serangan koordinasi pada infrastruktur kritis pertahanan | Panglima TNI + BSSN | 30 menit |

**Jawaban:** Framework integrasi mencakup: (a) Struktur paralel dimana IR Lead dan Forensic Lead bekerja di bawah Incident Commander, (b) Prioritas ditentukan oleh tingkat ancaman — recovery didahulukan saat ada ancaman langsung terhadap keselamatan atau sistem kritis, evidence preservation didahulukan saat ancaman sudah contained, (c) Eskalasi 4 level berdasarkan severity dengan response time yang semakin ketat.

---

## 8. Legal Considerations dalam Investigasi Forensik Militer

### 8.1 Kerangka Hukum di Indonesia

Investigasi forensik digital di Indonesia, khususnya dalam konteks militer, diatur oleh beberapa regulasi:

| Regulasi | Konten Relevan |
|----------|---------------|
| **UU No. 1 Tahun 2024 tentang ITE** | Pengakuan bukti elektronik, ketentuan akses dan intersepsi |
| **UU No. 3 Tahun 2002 tentang Pertahanan Negara** | Kewenangan militer dalam pertahanan negara |
| **UU No. 31 Tahun 1997 tentang Peradilan Militer** | Prosedur peradilan militer termasuk pembuktian |
| **PP No. 82 Tahun 2012** | Penyelenggaraan sistem dan transaksi elektronik |
| **ISO/IEC 27037:2012** | Standar internasional penanganan bukti digital |

### 8.2 Admissibility Bukti Digital

Agar bukti digital dapat diterima di pengadilan atau tribunal militer, harus memenuhi kriteria:

| Kriteria | Deskripsi | Cara Memenuhi |
|----------|-----------|--------------|
| **Authenticity** | Bukti asli dan tidak dimanipulasi | Hash verification, chain of custody |
| **Reliability** | Proses pengumpulan dapat dipercaya | Mengikuti SOP dan standar (NIST, ISO) |
| **Completeness** | Bukti lengkap dan kontekstual | Dokumentasi menyeluruh |
| **Relevance** | Bukti relevan dengan kasus | Analisis dan filtering yang tepat |
| **Proportionality** | Proporsional dengan investigasi | Tidak excessive collection |

#### Solved Problem 15 ⭐⭐

**Soal:** Seorang perwira di Lanud Halim Perdanakusuma dituduh membocorkan informasi rahasia melalui email pribadi. Jelaskan pertimbangan legal yang harus diperhatikan dalam melakukan forensik digital terhadap perangkat pribadinya!

**Penyelesaian:**

*Step 1:* Identifikasi isu legal yang relevan

| Isu | Pertimbangan |
|-----|-------------|
| **Privasi** | Perangkat pribadi memiliki perlindungan privasi yang lebih kuat |
| **Otorisasi** | Memerlukan dasar hukum yang jelas untuk mengakses perangkat pribadi |
| **Scope** | Akses harus terbatas pada data yang relevan dengan kasus |
| **Peradilan Militer** | Tunduk pada UU Peradilan Militer (UU 31/1997) |
| **Bukti Elektronik** | Harus memenuhi ketentuan UU ITE tentang bukti elektronik |

*Step 2:* Tentukan langkah-langkah legal yang diperlukan

1. **Dapatkan otorisasi**: Surat perintah investigasi dari Papera (Perwira Penyerah Perkara)
2. **Konsultasi hukum**: Koordinasi dengan Oditur Militer atau Kumdam
3. **Scope limitation**: Batasi akuisisi hanya pada data relevan (email terkait kebocoran)
4. **Witness presence**: Hadirkan saksi saat akuisisi untuk validitas
5. **Informed notice**: Dokumentasikan pemberitahuan kepada tersangka (sesuai prosedur militer)

**Jawaban:** Pertimbangan legal meliputi: (1) Otorisasi harus melalui Papera sesuai hukum peradilan militer, (2) Scope akuisisi terbatas pada data relevan untuk menghormati privasi, (3) Proses harus disaksikan dan didokumentasikan, (4) Bukti digital harus memenuhi persyaratan UU ITE tentang bukti elektronik, (5) Koordinasi dengan penasihat hukum militer wajib dilakukan.

---

#### Solved Problem 16 ⭐⭐⭐

**Soal:** Tim forensik digital TNI menemukan bukti serangan siber yang berasal dari server di luar negeri. Data yang relevan berada di cloud provider yang berlokasi di negara asing. Analisis tantangan legal dan opsi yang tersedia untuk mendapatkan bukti tersebut!

**Penyelesaian:**

*Step 1:* Identifikasi tantangan legal

| Tantangan | Detail |
|-----------|--------|
| **Jurisdiksi** | Data berada di luar yurisdiksi hukum Indonesia |
| **Sovereignty** | Negara lain memiliki kedaulatan atas server di wilayahnya |
| **Privacy Laws** | Cloud provider tunduk pada hukum privasi negara setempat (misal GDPR) |
| **Time Sensitivity** | Bukti digital dapat dihapus atau dimodifikasi kapan saja |
| **Cooperation** | Memerlukan kerjasama internasional yang memakan waktu |

*Step 2:* Evaluasi opsi yang tersedia

| Opsi | Deskripsi | Kelebihan | Kekurangan |
|------|-----------|-----------|-----------|
| **MLAT** (Mutual Legal Assistance Treaty) | Permintaan bantuan hukum melalui jalur diplomatik | Resmi dan legally binding | Proses lama (berbulan-bulan) |
| **Government-to-Government** | Koordinasi langsung antar pemerintah | Lebih cepat dari MLAT | Bergantung pada hubungan diplomatik |
| **Cloud Provider Request** | Permintaan langsung ke provider | Relatif cepat | Provider mungkin menolak tanpa court order lokal |
| **INTERPOL** | Koordinasi melalui jalur INTERPOL | Network internasional luas | Proses birokrasi |
| **Preservation Request** | Minta provider mempreservasi data | Mencegah penghapusan | Tidak memberikan akses ke data |

*Step 3:* Rekomendasi strategi

1. **Segera**: Kirim preservation request ke cloud provider
2. **Paralel**: Inisiasi MLAT melalui Kementerian Hukum dan HAM
3. **Diplomatik**: Koordinasi melalui kementerian pertahanan dan luar negeri
4. **Teknis**: Kumpulkan semua bukti yang tersedia di dalam yurisdiksi terlebih dahulu

**Jawaban:** Tantangan utama adalah jurisdiksi lintas negara dan kedaulatan data. Strategi yang direkomendasikan: (1) Kirim preservation request segera untuk mencegah penghapusan data, (2) Inisiasi MLAT untuk proses formal, (3) Koordinasi diplomatik government-to-government untuk percepatan, (4) Kumpulkan semua bukti domestik terlebih dahulu sebagai bukti pendukung.

---

## 9. Laporan Investigasi Forensik

### 9.1 Struktur Laporan Forensik

> **Laporan Forensik** adalah dokumen formal yang mendokumentasikan seluruh proses investigasi forensik digital, temuan, analisis, dan kesimpulan.

Struktur standar laporan forensik:

| Bagian | Konten |
|--------|--------|
| **1. Halaman Judul** | Judul kasus, nomor kasus, tanggal, klasifikasi |
| **2. Executive Summary** | Ringkasan untuk non-teknis (1-2 halaman) |
| **3. Pendahuluan** | Latar belakang kasus, tujuan investigasi |
| **4. Metodologi** | Tools, teknik, dan prosedur yang digunakan |
| **5. Temuan** | Artefak dan bukti yang ditemukan |
| **6. Analisis** | Interpretasi dan korelasi temuan |
| **7. Timeline** | Rekonstruksi kronologi kejadian |
| **8. Kesimpulan** | Jawaban atas pertanyaan investigasi |
| **9. Rekomendasi** | Tindakan yang disarankan |
| **10. Lampiran** | Evidence list, hash values, screenshots, chain of custody |

### 9.2 Prinsip Penulisan Laporan

| Prinsip | Deskripsi |
|---------|-----------|
| **Accuracy** | Semua informasi akurat dan dapat diverifikasi |
| **Objectivity** | Tidak bias, melaporkan semua temuan |
| **Clarity** | Dapat dipahami oleh audience yang tepat |
| **Completeness** | Mencakup seluruh proses dan temuan |
| **Relevance** | Fokus pada informasi yang relevan dengan kasus |

### 9.3 Executive Summary vs. Technical Report

| Aspek | Executive Summary | Technical Report |
|-------|------------------|-----------------|
| **Audience** | Komandan, pejabat non-teknis | Investigator, analisis teknis |
| **Bahasa** | Non-teknis, lugas | Teknis, detail |
| **Panjang** | 1-2 halaman | 10-50+ halaman |
| **Detail** | Ringkasan temuan utama | Langkah-langkah detail |
| **Tujuan** | Pengambilan keputusan | Referensi teknis |

#### Solved Problem 17 ⭐

**Soal:** Sebutkan minimal 5 bagian wajib yang harus ada dalam laporan forensik digital!

**Penyelesaian:**

*Step 1:* Identifikasi bagian-bagian wajib

| No | Bagian | Alasan Wajib |
|----|--------|-------------|
| 1 | **Executive Summary** | Ringkasan untuk pengambil keputusan |
| 2 | **Metodologi** | Menunjukkan proses yang terstandarisasi |
| 3 | **Temuan** | Inti dari investigasi |
| 4 | **Analisis/Timeline** | Rekonstruksi kejadian |
| 5 | **Kesimpulan** | Jawaban atas pertanyaan investigasi |
| 6 | **Lampiran** | Hash values, chain of custody, evidence list |

**Jawaban:** Lima bagian wajib dalam laporan forensik: (1) Executive Summary, (2) Metodologi yang digunakan, (3) Temuan (findings), (4) Analisis dan timeline kejadian, (5) Kesimpulan. Bagian tambahan yang penting adalah lampiran yang berisi hash values dan chain of custody documentation.

---

#### Solved Problem 18 ⭐⭐⭐

**Soal:** Buatlah contoh executive summary untuk kasus berikut: Tim forensik Satsiber TNI menginvestigasi kebocoran dokumen rencana operasi militer dari email seorang perwira senior di Mabes TNI. Investigasi menemukan bahwa laptop perwira tersebut telah terinfeksi spyware yang mengirimkan data ke server eksternal.

**Penyelesaian:**

*Step 1:* Identifikasi elemen kunci executive summary

*Step 2:* Susun dalam bahasa non-teknis

```
EXECUTIVE SUMMARY
=================
Nomor Kasus  : STB-TNI/DFIR/2026/015
Klasifikasi  : RAHASIA
Tanggal      : 7 Februari 2026
Investigator : Mayor Tek [Nama], Kepala Tim Forensik Satsiber TNI

LATAR BELAKANG
Investigasi ini dilakukan berdasarkan Surat Perintah 
No. Sprin/XXX/II/2026 terkait insiden kebocoran dokumen 
rencana operasi militer yang terdeteksi oleh Tim Intel 
pada tanggal [tanggal].

RINGKASAN INVESTIGASI
Tim forensik melakukan analisis mendalam terhadap laptop 
dinas perwira senior (Barang Bukti EVD-001) selama 
periode [tanggal mulai] hingga [tanggal selesai]. 
Analisis mencakup forensic imaging, memory analysis, 
network traffic analysis, dan malware reverse engineering.

TEMUAN UTAMA
1. Laptop dinas (EVD-001) telah terinfeksi perangkat 
   lunak pengintai (spyware) sejak [tanggal estimasi]

2. Spyware terinstal melalui lampiran email phishing 
   yang menyamar sebagai dokumen resmi dari instansi 
   pemerintah

3. Spyware memiliki kemampuan untuk:
   - Merekam ketikan keyboard (keylogger)
   - Mengambil tangkapan layar secara berkala
   - Mengirimkan file dokumen ke server eksternal
   - Mengaktifkan mikrofon dan kamera

4. Teridentifikasi komunikasi keluar ke server di 
   [negara/region] melalui protokol terenkripsi

5. Minimal [jumlah] dokumen berklasifikasi RAHASIA 
   terindikasi telah dieksfiltrasi selama periode 
   infeksi

KESIMPULAN
Dengan tingkat kepercayaan TINGGI, laptop dinas perwira 
telah dikompromi melalui serangan spear-phishing yang 
ditargetkan. Serangan ini memiliki karakteristik 
Advanced Persistent Threat (APT) yang mengindikasikan 
keterlibatan aktor negara.

REKOMENDASI
1. Segera isolasi dan ganti seluruh perangkat TI 
   perwira terkait
2. Reset kredensial dan akses ke seluruh sistem yang 
   pernah diakses dari laptop tersebut
3. Audit seluruh email dan komunikasi perwira selama 
   periode infeksi
4. Tingkatkan proteksi email (anti-phishing) di 
   seluruh Mabes TNI
5. Lakukan briefing kesadaran keamanan siber untuk 
   seluruh perwira senior
6. Koordinasi dengan BSSN untuk threat intelligence 
   terkait aktor ancaman
```

**Jawaban:** Executive summary harus mencakup: (1) Informasi kasus dan klasifikasi, (2) Latar belakang investigasi, (3) Ringkasan metodologi, (4) Temuan utama dalam bahasa non-teknis, (5) Kesimpulan dengan tingkat kepercayaan, (6) Rekomendasi tindakan yang actionable. Bahasa harus lugas agar dapat dipahami oleh komandan yang tidak memiliki latar belakang teknis.

---

## 10. Manajemen Kualitas Investigasi

### 10.1 Quality Assurance dalam Forensik Digital

> **Quality Assurance (QA)** dalam forensik digital adalah sistem proses yang memastikan bahwa investigasi dilakukan secara konsisten, akurat, dan sesuai standar.

Elemen QA forensik digital:

| Elemen | Deskripsi |
|--------|-----------|
| **Peer Review** | Investigator lain mereview proses dan temuan |
| **Tool Validation** | Verifikasi bahwa tools forensik berfungsi dengan benar |
| **Proficiency Testing** | Pengujian berkala kemampuan investigator |
| **Documentation Review** | Audit kelengkapan dan kualitas dokumentasi |
| **Chain of Custody Audit** | Verifikasi kelengkapan chain of custody |

### 10.2 Common Errors dan Pencegahan

| Error | Dampak | Pencegahan |
|-------|--------|-----------|
| **Tidak membuat hash sebelum analisis** | Integritas bukti dipertanyakan | SOP wajib hash di awal |
| **Chain of custody gap** | Bukti tidak admissible | Template dan checklist |
| **Analisis pada bukti asli** | Bukti asli termodifikasi | Selalu gunakan working copy |
| **Dokumentasi tidak lengkap** | Proses tidak dapat direproduksi | Template standar |
| **Bias konfirmasi** | Kesimpulan tidak objektif | Peer review wajib |

#### Solved Problem 19 ⭐⭐

**Soal:** Seorang analis forensik junior di Pusdiksus TNI melakukan analisis langsung pada hard disk bukti asli tanpa membuat forensic image terlebih dahulu. Jelaskan dampak kesalahan ini dan bagaimana cara memitigasinya!

**Penyelesaian:**

*Step 1:* Identifikasi dampak analisis langsung pada bukti asli

| Dampak | Penjelasan |
|--------|-----------|
| **Modifikasi timestamp** | Mengakses file mengubah access time |
| **Write operations** | OS mungkin menulis data ke disk (swap, temp files) |
| **Overwrite deleted data** | Data baru dapat menimpa deleted files di unallocated space |
| **Chain of custody violation** | Bukti tidak lagi dalam kondisi original |
| **Legal inadmissibility** | Bukti berpotensi ditolak di pengadilan militer |

*Step 2:* Mitigasi

1. **Segera**: Hentikan analisis dan cabut hard disk
2. **Dokumentasi**: Catat semua tindakan yang sudah dilakukan pada bukti asli
3. **Hash**: Hitung hash saat ini dan bandingkan dengan hash awal (jika ada)
4. **Imaging**: Segera buat forensic image dari kondisi saat ini
5. **Reporting**: Laporkan insiden ke ketua tim forensik
6. **Assessment**: Evaluasi seberapa besar kerusakan terhadap bukti

**Jawaban:** Dampak utama: modifikasi timestamp, potensi overwrite deleted data, dan ancaman legal inadmissibility. Mitigasi: (1) Segera hentikan analisis, (2) Dokumentasikan semua tindakan, (3) Buat forensic image segera, (4) Laporkan ke ketua tim. Untuk pencegahan, SOP harus mewajibkan forensic imaging SEBELUM analisis apapun dilakukan.

---

#### Solved Problem 20 ⭐⭐⭐

**Soal:** Anda ditugaskan untuk melakukan audit kualitas terhadap laboratorium forensik digital baru di Kodam Jaya. Rancang checklist audit yang mencakup aspek personel, peralatan, prosedur, dan fasilitas!

**Penyelesaian:**

*Step 1:* Identifikasi aspek-aspek yang perlu diaudit

*Step 2:* Rancang checklist untuk setiap aspek

**A. Personel**

| No | Item Audit | Status |
|----|-----------|--------|
| 1 | Investigator memiliki sertifikasi forensik yang relevan | ☐ |
| 2 | Training record terdokumentasi dan up-to-date | ☐ |
| 3 | Security clearance sesuai dengan level kasus yang ditangani | ☐ |
| 4 | Proficiency testing dilakukan minimal setahun sekali | ☐ |
| 5 | Rasio investigator terhadap caseload memadai | ☐ |

**B. Peralatan**

| No | Item Audit | Status |
|----|-----------|--------|
| 1 | Write blockers tersedia dan divalidasi secara berkala | ☐ |
| 2 | Tools forensik (software) berlisensi dan versi terbaru | ☐ |
| 3 | Workstation forensik memenuhi spesifikasi minimum | ☐ |
| 4 | Media penyimpanan bukti tersedia dalam jumlah cukup | ☐ |
| 5 | Kit fotografi forensik lengkap | ☐ |

**C. Prosedur**

| No | Item Audit | Status |
|----|-----------|--------|
| 1 | SOP terdokumentasi untuk setiap jenis investigasi | ☐ |
| 2 | Chain of custody form standar tersedia dan digunakan | ☐ |
| 3 | Peer review dilakukan untuk setiap kasus | ☐ |
| 4 | Evidence handling procedures sesuai ISO 27037 | ☐ |
| 5 | Reporting template standar tersedia | ☐ |

**D. Fasilitas**

| No | Item Audit | Status |
|----|-----------|--------|
| 1 | Evidence storage room dengan akses terkontrol | ☐ |
| 2 | Log akses ke evidence room terdokumentasi | ☐ |
| 3 | Kontrol suhu dan kelembaban di evidence storage | ☐ |
| 4 | Area analisis terpisah dari area penyimpanan | ☐ |
| 5 | CCTV dan alarm keamanan terpasang | ☐ |

**Jawaban:** Checklist audit lab forensik digital meliputi 4 aspek utama: (1) Personel — sertifikasi, training, dan security clearance, (2) Peralatan — write blockers, tools forensik, dan workstation, (3) Prosedur — SOP, chain of custody, dan peer review, (4) Fasilitas — evidence storage, akses terkontrol, dan keamanan fisik. Setiap aspek harus diaudit secara berkala minimal setahun sekali.

---

#### Solved Problem 21 ⭐

**Soal:** Jelaskan apa yang dimaksud dengan "tool validation" dalam forensik digital dan mengapa hal ini penting!

**Penyelesaian:**

*Step 1:* Definisikan tool validation

> **Tool Validation** adalah proses memverifikasi bahwa tools forensik menghasilkan output yang akurat dan konsisten dengan menggunakan dataset yang diketahui hasilnya.

*Step 2:* Jelaskan pentingnya

| Alasan | Penjelasan |
|--------|-----------|
| **Akurasi** | Memastikan tools tidak menghasilkan false positives/negatives |
| **Admissibility** | Bukti yang dihasilkan oleh tools tervalidasi lebih kuat di pengadilan |
| **Reproducibility** | Hasil dapat direproduksi oleh investigator lain |
| **Defense Challenge** | Dapat menghadapi tantangan dari pihak pembela tentang reliabilitas tools |

Contoh validasi:
- Gunakan NIST CFTT (Computer Forensics Tool Testing) reference dataset
- Buat test image dengan konten yang diketahui, lalu verifikasi tools dapat menemukan semua konten
- Bandingkan hasil antar tools forensik yang berbeda

**Jawaban:** Tool validation adalah proses verifikasi bahwa tools forensik bekerja akurat menggunakan dataset referensi. Ini penting karena: (1) Memastikan akurasi hasil analisis, (2) Memperkuat admissibility bukti di pengadilan, (3) Memungkinkan reproducibility, (4) Menghadapi tantangan dari pihak pembela tentang reliabilitas tools yang digunakan.

---

#### Solved Problem 22 ⭐⭐⭐

**Soal:** Dalam sebuah investigasi forensik di Kodam Iskandar Muda, tim forensik menemukan bahwa dua tools forensik berbeda memberikan hasil yang berbeda saat menganalisis artefak yang sama pada sebuah forensic image. Tool A menemukan 150 file yang dihapus, sementara Tool B menemukan 175 file. Bagaimana investigator harus menangani situasi ini?

**Penyelesaian:**

*Step 1:* Analisis kemungkinan penyebab perbedaan

| Penyebab Potensial | Penjelasan |
|--------------------|-----------| 
| **Perbedaan algoritma carving** | Setiap tool menggunakan teknik recovery yang berbeda |
| **File signature database** | Satu tool mungkin mengenali lebih banyak jenis file |
| **Threshold sensitivity** | Perbedaan dalam threshold minimum file size |
| **Unallocated space handling** | Cara memproses unallocated space berbeda |
| **False positives** | Satu tool mungkin menghasilkan lebih banyak false positive |

*Step 2:* Tentukan langkah penanganan

1. **Dokumentasikan**: Catat hasil kedua tools secara lengkap
2. **Analisis delta**: Identifikasi 25 file yang hanya ditemukan Tool B
   - Apakah file tersebut valid atau false positive?
   - Gunakan hex editor untuk verifikasi manual
3. **Cross-validate**: Gunakan tool ketiga (misalnya Autopsy) sebagai validator
4. **Report both**: Dalam laporan, sertakan hasil kedua tools
5. **Explain discrepancy**: Jelaskan penyebab perbedaan secara teknis
6. **Validate tools**: Lakukan tool validation menggunakan NIST CFTT dataset

*Step 3:* Rekomendasi untuk laporan

```
Dalam laporan forensik:
- Sebutkan bahwa analisis dilakukan menggunakan Tool A 
  dan Tool B (versi dan konfigurasi dicantumkan)
- Tool A menemukan 150 deleted files
- Tool B menemukan 175 deleted files
- Cross-validation menunjukkan 150 file identik, 
  25 file tambahan dari Tool B diverifikasi sebagai 
  [valid/false positive/partial recovery]
- Tidak ada perbedaan yang mempengaruhi kesimpulan 
  utama investigasi
```

**Jawaban:** Investigator harus: (1) Mendokumentasikan hasil kedua tools secara lengkap, (2) Menganalisis perbedaan 25 file menggunakan verifikasi manual (hex editor), (3) Cross-validate menggunakan tool ketiga, (4) Melaporkan hasil kedua tools beserta penjelasan perbedaan. Perbedaan antar tools adalah hal yang normal dan harus ditangani secara transparan dalam laporan.

---

## Supplementary Problems

### Problem S1 ⭐
**Soal:** Sebutkan lima aktivitas utama dalam fase Collection menurut NIST SP 800-86!

**Jawaban:** (1) Identifikasi sumber bukti potensial, (2) Prioritisasi berdasarkan volatilitas, (3) Akuisisi data menggunakan metode forensik, (4) Preservasi integritas bukti, (5) Dokumentasi setiap langkah pengumpulan.

---

### Problem S2 ⭐⭐
**Soal:** Apa perbedaan antara forensic image format E01 (EnCase) dan format dd (raw)?

**Jawaban:** 
- **E01**: Format proprietary EnCase yang mendukung kompresi, metadata internal (case info, hash), dan segmentasi file. Lebih efisien dalam penyimpanan.
- **dd (raw)**: Format mentah bit-by-bit tanpa kompresi atau metadata. Universal dan dapat dibaca semua tools, namun ukurannya sama dengan disk asli.
- E01 lebih praktis untuk penyimpanan, dd lebih universal untuk kompatibilitas.

---

### Problem S3 ⭐⭐
**Soal:** Mengapa capture volatile data harus dilakukan SEBELUM mematikan komputer?

**Jawaban:** Volatile data (RAM, proses, koneksi jaringan) hilang permanen saat komputer dimatikan. Data ini penting karena: (1) RAM berisi malware yang mungkin hanya ada di memori (fileless malware), (2) Koneksi jaringan aktif menunjukkan komunikasi C2, (3) Running processes menunjukkan aktivitas tersangka saat itu, (4) Encryption keys di memori dapat mendekripsi data terenkripsi.

---

### Problem S4 ⭐⭐⭐
**Soal:** Bandingkan kelebihan dan kekurangan antara live forensics dan dead forensics dalam konteks investigasi militer!

**Jawaban:** 
- **Live Forensics**: Kelebihan — menangkap volatile data, encryption keys, dan malware di memori. Kekurangan — berisiko mengubah state sistem, memerlukan kecepatan, dan tools yang berjalan memodifikasi memori.
- **Dead Forensics**: Kelebihan — tidak mengubah bukti (dengan write blocker), analisis lebih teliti tanpa tekanan waktu. Kekurangan — kehilangan volatile data, encrypted drive tidak dapat diakses tanpa key.
- Dalam konteks militer, sering diperlukan kombinasi keduanya: live capture volatile data terlebih dahulu, kemudian dead analysis untuk analisis mendalam.

---

### Problem S5 ⭐⭐⭐
**Soal:** Jelaskan tantangan utama dalam melakukan investigasi forensik digital pada sistem militer yang menggunakan jaringan tertutup (air-gapped network)!

**Jawaban:** 
Tantangan pada air-gapped network:
1. **Akses terbatas**: Investigator mungkin perlu masuk ke fasilitas dengan keamanan tinggi
2. **Tools deployment**: Tools forensik harus diverifikasi dan di-approve sebelum dimasukkan ke jaringan
3. **Data extraction**: Hasil investigasi harus dikeluarkan melalui prosedur keamanan ketat
4. **Limited connectivity**: Tidak bisa mengakses online resources atau update tools selama investigasi
5. **Classification handling**: Semua media yang masuk/keluar harus melalui prosedur declassification
6. **Insider focus**: Serangan pada air-gapped network biasanya melibatkan insider atau supply chain compromise

---

## Ringkasan

| Konsep | Deskripsi Singkat |
|--------|-------------------|
| **NIST SP 800-86** | Framework 4 fase: Collection, Examination, Analysis, Reporting |
| **DFRWS** | Framework 6 fase yang lebih granular (termasuk Identification dan Preservation terpisah) |
| **ISO/IEC 27037** | Standar internasional: Identification, Collection, Acquisition, Preservation |
| **First Response** | Tindakan awal di TKP: amankan lokasi, dokumentasi, preservasi bukti |
| **Live vs. Dead System** | Live: capture volatile data dulu; Dead: jangan nyalakan |
| **Dokumentasi** | Case notes, fotografi forensik (3 level), sketsa TKP |
| **Chain of Custody** | Pencatatan kronologis setiap perpindahan dan penanganan bukti |
| **Forensic Imaging** | Physical image (seluruh disk) vs Logical image (file/folder aktif) |
| **Hash Verification** | MD5/SHA-256 untuk membuktikan integritas bukti |
| **SOP Militer** | Prosedur terstruktur sesuai hierarki komando dan klasifikasi keamanan |
| **Incident Handling** | NIST 4 fase: Preparation, Detection, Containment, Post-Incident |
| **CERT/CSIRT** | Tim respons insiden: BSSN, Satsiber TNI, CERT Kodam |
| **Legal Framework** | UU ITE No. 1/2024, UU Peradilan Militer No. 31/1997 |
| **Admissibility** | Authenticity, reliability, completeness, relevance, proportionality |
| **Laporan Forensik** | Executive summary (non-teknis) dan technical report (detail) |
| **Quality Assurance** | Peer review, tool validation, proficiency testing |

---

## Referensi

1. Casey, E. (2022). *Digital Evidence and Computer Crime: Forensic Science, Computers, and the Internet* (4th Ed.). Academic Press. Chapter 3-4.
2. Phillips, A., Nelson, B., & Steuart, C. (2022). *Guide to Computer Forensics and Investigations* (6th Ed.). Cengage Learning. Chapter 2-3.
3. Johansen, G. (2020). *Digital Forensics and Incident Response* (2nd Ed.). Packt. Chapter 2-3.
4. NIST SP 800-86: Guide to Integrating Forensic Techniques into Incident Response.
5. NIST SP 800-61 Rev. 2: Computer Security Incident Handling Guide.
6. ISO/IEC 27037:2012 — Guidelines for Identification, Collection, Acquisition and Preservation of Digital Evidence.
7. Carrier, B. (2005). *File System Forensic Analysis*. Addison-Wesley. Chapter 1.
8. UU No. 1 Tahun 2024 tentang Informasi dan Transaksi Elektronik.
9. UU No. 31 Tahun 1997 tentang Peradilan Militer.
10. UU No. 3 Tahun 2002 tentang Pertahanan Negara.

---

## License / Lisensi

This repository is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

Commercial use is permitted, provided attribution is given to the author.

© 2026 Anindito
