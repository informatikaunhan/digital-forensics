# Modul 04: Akuisisi dan Duplikasi Data Forensik

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 04  
**Topik:** Akuisisi dan Duplikasi Data Forensik  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Menjelaskan prinsip dasar akuisisi forensik dan pentingnya write-blocking
2. Membedakan metode akuisisi physical, logical, dan sparse serta kapan masing-masing digunakan
3. Mengidentifikasi format image forensik (raw/dd, E01, AFF4) beserta kelebihan dan kekurangannya
4. Memahami perbedaan live acquisition dan dead acquisition serta pertimbangan penggunaannya
5. Melakukan akuisisi volatile data dan memory forensics menggunakan tools standar
6. Menerapkan teknik verifikasi integritas menggunakan fungsi hash (MD5, SHA-1, SHA-256)
7. Menjelaskan tantangan akuisisi pada SSD, media terenkripsi, RAID array, dan remote acquisition dalam konteks operasi militer

---

## 1. Prinsip Dasar Akuisisi Forensik

### 1.1 Definisi Akuisisi Forensik

> **Akuisisi Forensik (Forensic Acquisition)** adalah proses pembuatan salinan data yang akurat secara bitwise dari media penyimpanan atau sumber data digital lainnya, dengan tetap menjaga integritas dan keaslian bukti asli.

Akuisisi forensik merupakan tahap kritis dalam investigasi forensik digital. Kegagalan atau kesalahan pada tahap ini dapat menyebabkan bukti digital tidak dapat diterima di pengadilan atau tribunal militer. Dalam konteks militer, akuisisi forensik sering dilakukan dalam kondisi tekanan waktu tinggi, seperti saat merespons insiden siber di jaringan Kodam atau saat mengamankan bukti digital di medan operasi.

| Aspek | Salinan Biasa (Copy) | Akuisisi Forensik (Imaging) |
|-------|---------------------|-----------------------------|
| **Cakupan** | Hanya file yang terlihat | Seluruh bit termasuk deleted data, slack space |
| **Metode** | Copy-paste standar | Bit-by-bit duplication |
| **Verifikasi** | Tidak ada | Hash verification (MD5/SHA) |
| **Metadata** | Berubah (timestamp) | Terjaga (dengan write-blocker) |
| **Unallocated Space** | Tidak tercakup | Tercakup |
| **Admissibility** | Tidak diterima | Diterima sebagai bukti |

### 1.2 Prinsip-Prinsip Akuisisi

Akuisisi forensik harus memenuhi prinsip-prinsip berikut:

1. **Keutuhan Bukti Asli (Evidence Integrity)**: Data asli tidak boleh berubah selama proses akuisisi
2. **Verifikasi (Verification)**: Setiap salinan harus diverifikasi keasliannya menggunakan hash cryptographic
3. **Dokumentasi (Documentation)**: Seluruh proses harus didokumentasikan secara lengkap
4. **Repeatability**: Proses harus dapat diulangi dengan hasil identik
5. **Chain of Custody**: Alur penanganan bukti harus terdokumentasi

![Prinsip Akuisisi Forensik](images/p04-01-acquisition-principles.png)

*Gambar 4.1: Lima prinsip dasar akuisisi forensik*

#### Solved Problem 1 ⭐

**Soal:** Jelaskan mengapa akuisisi forensik tidak dapat digantikan dengan proses copy-paste biasa!

**Penyelesaian:**

*Step 1:* Identifikasi keterbatasan copy-paste biasa

*Step 2:* Bandingkan dengan kebutuhan forensik

| Aspek | Copy-Paste | Akuisisi Forensik |
|-------|-----------|-------------------|
| **Deleted files** | Tidak tercakup | Tercakup (dari unallocated space) |
| **Slack space** | Tidak tercakup | Tercakup |
| **Metadata** | Berubah (timestamp update) | Terjaga dengan write-blocker |
| **Verifikasi** | Tidak ada jaminan integritas | Hash verification membuktikan kesamaan |
| **Hukum** | Tidak diterima sebagai bukti | Diterima di pengadilan |

**Jawaban:** Copy-paste biasa tidak dapat menggantikan akuisisi forensik karena: (1) Tidak mencakup deleted files dan unallocated space yang mungkin berisi bukti penting, (2) Mengubah metadata seperti timestamp, (3) Tidak menyediakan mekanisme verifikasi integritas, dan (4) Hasilnya tidak dapat diterima sebagai bukti di pengadilan atau tribunal militer.

---

### 1.3 Write-Blocking

> **Write Blocker** adalah perangkat keras atau perangkat lunak yang mencegah penulisan data ke media penyimpanan yang sedang diakses, sehingga menjaga integritas bukti asli.

Write-blocking merupakan komponen wajib dalam setiap proses akuisisi forensik. Tanpa write-blocker, sistem operasi secara otomatis dapat memodifikasi media penyimpanan saat diakses (misalnya update timestamp, journal entries, atau autorun).

#### 1.3.1 Jenis Write Blocker

| Jenis | Deskripsi | Kelebihan | Kekurangan |
|-------|-----------|-----------|------------|
| **Hardware Write Blocker** | Perangkat fisik antara media dan komputer | Independen dari OS, terpercaya | Mahal, perlu perangkat tambahan |
| **Software Write Blocker** | Utilitas software yang mencegah penulisan | Gratis/murah, fleksibel | Bergantung pada OS, perlu validasi |
| **Firmware Write Blocker** | Built-in pada beberapa forensic workstation | Terintegrasi | Terbatas pada perangkat tertentu |

#### 1.3.2 Cara Kerja Hardware Write Blocker

Hardware write blocker bekerja dengan cara:
1. **Intercept Commands**: Mencegat semua perintah write dari host ke media
2. **Allow Read**: Hanya mengizinkan perintah read untuk lewat
3. **Return Status**: Mengembalikan status sukses palsu untuk perintah write yang dicegat
4. **Interface Support**: Mendukung berbagai interface (SATA, USB, IDE, NVMe)

> **Konteks Militer:** Di lingkungan TNI, hardware write blocker sangat disarankan karena memberikan jaminan independen dari sistem operasi. Dalam skenario akuisisi di lapangan (misalnya saat investigasi di Kodam atau pangkalan), ketersediaan write blocker harus menjadi bagian dari forensic kit standar.

#### Solved Problem 2 ⭐

**Soal:** Apa yang dapat terjadi jika akuisisi dilakukan tanpa write blocker pada hard disk tersangka?

**Penyelesaian:**

*Step 1:* Identifikasi aktivitas otomatis OS saat media dikoneksikan

*Step 2:* Evaluasi dampak terhadap integritas bukti

**Aktivitas yang dapat memodifikasi bukti:**
1. **Windows autoplay/autorun**: Sistem membaca dan menulis data autorun
2. **Filesystem journal update**: NTFS journal ter-update saat mounting
3. **Timestamp modification**: Last accessed timestamp berubah
4. **Antivirus scanning**: AV melakukan scan otomatis yang mengubah metadata
5. **Indexing service**: Windows Search Indexer mengakses dan mengindex file

**Jawaban:** Tanpa write blocker, sistem operasi akan melakukan aktivitas seperti autoplay, journal update, timestamp modification, antivirus scanning, dan indexing yang semuanya memodifikasi bukti asli. Hal ini membuat hash value berubah, sehingga integritas bukti tidak lagi dapat dibuktikan dan bukti dapat ditolak di pengadilan.

---

## 2. Metode Akuisisi Forensik

### 2.1 Klasifikasi Metode Akuisisi

![Metode Akuisisi Forensik](images/p04-02-acquisition-methods.png)

*Gambar 4.2: Tiga metode utama akuisisi forensik*

#### 2.1.1 Physical Acquisition (Akuisisi Fisik)

> **Physical Acquisition** adalah proses penyalinan bit-by-bit dari seluruh media penyimpanan, termasuk semua partisi, unallocated space, slack space, dan area tersembunyi (Host Protected Area/HPA dan Device Configuration Overlay/DCO).

Karakteristik physical acquisition:
- Menyalin seluruh konten disk dari sektor pertama hingga terakhir
- Mencakup deleted files, slack space, dan unallocated space
- Menyalin HPA (Host Protected Area) dan DCO (Device Configuration Overlay)
- Menghasilkan salinan identik dari media asli
- Memerlukan waktu paling lama dan ruang penyimpanan terbesar

#### 2.1.2 Logical Acquisition (Akuisisi Logis)

> **Logical Acquisition** adalah proses penyalinan file dan folder yang terlihat pada sistem file aktif, tanpa mencakup deleted data, unallocated space, atau slack space.

Kapan menggunakan logical acquisition:
- Ketika waktu terbatas dan hanya file tertentu yang relevan
- Pada sistem yang sedang berjalan (live system) dimana shutdown tidak memungkinkan
- Untuk akuisisi dari remote system melalui jaringan
- Saat volume data sangat besar dan hanya subset yang diperlukan

#### 2.1.3 Sparse Acquisition (Akuisisi Terpilih)

> **Sparse Acquisition** adalah proses penyalinan area tertentu dari media penyimpanan berdasarkan kriteria spesifik, seperti rentang sektor tertentu atau tipe file tertentu.

Sparse acquisition berguna ketika:
- Hanya data dari partisi tertentu yang relevan
- Disk terlalu besar untuk di-image seluruhnya dalam waktu yang tersedia
- Investigasi memfokuskan pada area tertentu (misalnya, MBR atau partisi tertentu)

| Metode | Cakupan | Waktu | Ruang | Deleted Data | Kasus Penggunaan |
|--------|---------|-------|-------|-------------|-----------------|
| **Physical** | 100% disk | Lama | Besar | Ya | Investigasi lengkap |
| **Logical** | File aktif | Cepat | Kecil | Tidak | Triage awal, live system |
| **Sparse** | Area tertentu | Sedang | Sedang | Tergantung | Fokus pada area spesifik |

#### Solved Problem 3 ⭐

**Soal:** Sebuah laptop yang diduga berisi komunikasi terkait kebocoran informasi militer disita. Laptop tersebut dalam keadaan mati. Metode akuisisi apa yang paling tepat dan mengapa?

**Penyelesaian:**

*Step 1:* Analisis situasi
- Laptop dalam keadaan mati (dead system)
- Diduga berisi komunikasi sensitif (perlu pemeriksaan menyeluruh)
- Potensi adanya data yang sudah dihapus

*Step 2:* Evaluasi metode akuisisi

| Metode | Cocok? | Alasan |
|--------|--------|--------|
| Physical | ✅ | Mencakup semua data termasuk deleted |
| Logical | ❌ | Tidak mencakup deleted files |
| Sparse | ❌ | Tidak mencakup seluruh area disk |

**Jawaban:** **Physical acquisition** adalah metode yang paling tepat karena: (1) Laptop dalam keadaan mati sehingga memungkinkan dead acquisition yang aman, (2) Investigasi kebocoran memerlukan pemeriksaan menyeluruh termasuk file yang mungkin sudah dihapus tersangka, (3) Physical image mencakup unallocated space dan slack space yang dapat berisi fragmen komunikasi yang dihapus.

---

### 2.2 Live Acquisition vs Dead Acquisition

#### 2.2.1 Dead Acquisition

> **Dead Acquisition** (juga disebut *static acquisition*) adalah akuisisi yang dilakukan pada media penyimpanan yang telah dilepas dari sistem atau pada sistem yang sudah dimatikan.

Keunggulan dead acquisition:
- Media tidak berubah selama proses (dengan write-blocker)
- Hasil konsisten dan repeatable
- Standar yang paling diterima di pengadilan

Kelemahan dead acquisition:
- Kehilangan volatile data (RAM, network connections, running processes)
- Kehilangan data dari full disk encryption yang memerlukan key di RAM
- Tidak mungkin dilakukan jika sistem tidak boleh dimatikan

#### 2.2.2 Live Acquisition

> **Live Acquisition** adalah akuisisi yang dilakukan pada sistem yang masih berjalan, memungkinkan pengambilan data volatile dan data yang hanya tersedia saat sistem aktif.

| Aspek | Dead Acquisition | Live Acquisition |
|-------|-----------------|-----------------|
| **Volatile data** | Hilang | Terjaga |
| **Encryption** | Mungkin terkunci | Key tersedia di RAM |
| **Integritas** | Tinggi | Lebih rendah (sistem berubah) |
| **Repeatability** | Ya | Tidak (state berubah) |
| **Kompleksitas** | Rendah | Tinggi |
| **Waktu** | Fleksibel | Mendesak |

> **Konteks Militer:** Dalam operasi militer, live acquisition sering menjadi pilihan utama. Server komando dan kontrol (C2) militer tidak dapat dimatikan begitu saja karena dapat mengganggu operasi. Tim forensik TNI harus mampu melakukan live acquisition pada sistem aktif tanpa mengganggu operasional.

#### Solved Problem 4 ⭐⭐

**Soal:** Server data di markas Kodam XIV/Hasanuddin terdeteksi mengalami intrusi. Server tersebut menjalankan sistem manajemen personel dan tidak boleh dimatikan. Jelaskan langkah akuisisi yang harus dilakukan!

**Penyelesaian:**

*Step 1:* Evaluasi situasi
- Server aktif dan tidak boleh dimatikan → Live acquisition wajib
- Perlu mengamankan bukti volatile dan persistent
- Harus meminimalkan dampak terhadap operasi

*Step 2:* Tentukan urutan akuisisi berdasarkan order of volatility (RFC 3227)

1. **Akuisisi RAM** (paling volatile)
   - Gunakan Belkasoft RAM Capturer atau WinPMEM
   - Simpan memory dump ke external drive

2. **Network State**
   - Capture koneksi aktif: `netstat -anob`
   - Running processes: `tasklist /v`
   - Logged-on users: `query user`

3. **Volatile System Data**
   - Clipboard content
   - Temporary files
   - Current time/timezone

4. **Logical Acquisition** (data persistent)
   - Akuisisi file dan folder relevan
   - Export event logs
   - Copy registry hives

5. **Physical Image** (jika memungkinkan di lain waktu)
   - Jadwalkan maintenance window untuk full image

**Jawaban:** Pada server aktif yang tidak boleh dimatikan, lakukan live acquisition dengan urutan: (1) Akuisisi RAM menggunakan RAM Capturer, (2) Capture network state dan running processes, (3) Kumpulkan volatile system data, (4) Logical acquisition untuk file dan log relevan, (5) Jadwalkan physical acquisition saat maintenance window. Seluruh proses harus didokumentasikan termasuk hash setiap file yang diambil.

---

#### Solved Problem 5 ⭐

**Soal:** Sebutkan tiga kondisi di mana live acquisition lebih diprioritaskan dibanding dead acquisition!

**Penyelesaian:**

*Step 1:* Identifikasi skenario yang memerlukan live acquisition

**Tiga kondisi utama:**

1. **Full Disk Encryption (FDE) aktif**: Jika BitLocker, VeraCrypt, atau enkripsi lain aktif, mematikan sistem akan mengunci akses ke data. Key enkripsi hanya tersedia di RAM saat sistem berjalan.

2. **Sistem kritis yang tidak boleh mati**: Server militer, sistem komunikasi operasional, atau infrastruktur kritis yang downtime-nya berdampak pada operasi.

3. **Bukti volatile yang dibutuhkan**: Investigasi malware memerlukan analisis proses berjalan, koneksi jaringan aktif, atau injeksi memori yang hilang saat shutdown.

**Jawaban:** Live acquisition diprioritaskan saat: (1) Disk terenkripsi penuh dan key hanya di RAM, (2) Sistem kritis yang tidak boleh dimatikan (server militer, sistem C2), dan (3) Bukti volatile seperti RAM content, network connections, dan running processes diperlukan untuk investigasi.

---

## 3. Format Image Forensik

### 3.1 Jenis Format Image

![Format Image Forensik](images/p04-03-forensic-image-formats.png)

*Gambar 4.3: Perbandingan format image forensik*

#### 3.1.1 Format Raw (dd)

> **Format Raw** (juga dikenal sebagai *dd format*) adalah format image paling dasar yang merupakan salinan bit-by-bit exact dari media sumber, tanpa metadata tambahan atau kompresi.

Karakteristik format raw:
- Salinan identik bit-per-bit dari media sumber
- Tidak ada header, footer, atau metadata
- Ukuran image sama dengan ukuran media sumber
- Kompatibilitas tertinggi dengan berbagai tools
- Dapat di-mount langsung sebagai virtual disk

Variasi format raw:
- `.dd` — ekstensi standar dari perintah dd
- `.raw` — ekstensi generik
- `.img` — ekstensi umum untuk disk image
- `.001, .002, ...` — format raw yang dipecah (split)

#### 3.1.2 Format E01 (EnCase Evidence File)

> **Format E01** adalah format proprietary yang dikembangkan oleh Guidance Software (sekarang OpenText) untuk EnCase forensic suite, dan menjadi format standar de facto dalam forensik digital.

Struktur format E01:
1. **Header**: Informasi kasus, examiner, catatan
2. **Data Blocks**: Data yang dikompresi dalam chunk 32KB atau 64KB
3. **CRC per Block**: Verifikasi integritas per chunk
4. **Hash Footer**: MD5 dan/atau SHA-1 dari keseluruhan data

Keunggulan E01:
- Kompresi bawaan (biasanya 30-50% lebih kecil)
- Metadata kasus tersimpan dalam file
- CRC per block untuk deteksi korupsi
- Dapat dipecah menjadi segmen (split) untuk media kecil
- Diterima luas di pengadilan

#### 3.1.3 Format AFF4 (Advanced Forensics Format 4)

> **AFF4** adalah format forensik open-source generasi terbaru yang mendukung kompresi, signing digital, dan enkripsi, dengan desain arsitektur modern berbasis linked data.

Fitur unggulan AFF4:
- Open source dan bebas lisensi
- Kompresi efisien (snappy, zlib, lz4)
- Digital signatures untuk autentikasi
- Mendukung sparse image dan multiple streams
- Dapat menyimpan multiple image sources dalam satu container

| Fitur | Raw (dd) | E01 | AFF4 |
|-------|---------|-----|------|
| **Kompresi** | Tidak | Ya (zlib) | Ya (multi-algoritma) |
| **Metadata** | Tidak | Ya | Ya (extensible) |
| **Hash terintegrasi** | Tidak | Ya (MD5/SHA1) | Ya (SHA-256+) |
| **Split file** | Manual | Built-in | Built-in |
| **Open source** | Ya | Tidak | Ya |
| **Enkripsi** | Tidak | Tidak | Ya |
| **Digital signature** | Tidak | Tidak | Ya |
| **Ukuran relatif** | 100% | ~50-70% | ~50-70% |
| **Kompatibilitas** | Sangat tinggi | Tinggi | Sedang |

#### Solved Problem 6 ⭐

**Soal:** Tim forensik akan mengakuisisi hard disk 1TB dan harus mengirim image melalui jaringan militer dengan bandwidth terbatas. Format image apa yang paling tepat?

**Penyelesaian:**

*Step 1:* Identifikasi kebutuhan
- Ukuran besar (1TB) harus dikirim melalui jaringan
- Bandwidth terbatas → perlu kompresi
- Konteks militer → perlu integritas dan keamanan

*Step 2:* Evaluasi format

| Format | Ukuran Estimasi | Kompresi | Keamanan | Pilihan |
|--------|----------------|----------|----------|---------|
| Raw (dd) | 1TB | Tidak | Tidak | ❌ |
| E01 | ~500-700GB | Ya | CRC per block | ✅ |
| AFF4 | ~500-700GB | Ya | Signing + enkripsi | ✅✅ |

**Jawaban:** **Format E01 atau AFF4** adalah pilihan paling tepat. E01 dipilih jika kompatibilitas dengan tools standar lebih penting, karena kompresinya dapat mengurangi ukuran hingga 50-70%. AFF4 lebih ideal jika keamanan menjadi prioritas karena mendukung digital signature dan enkripsi, yang krusial untuk pengiriman melalui jaringan militer.

---

#### Solved Problem 7 ⭐⭐

**Soal:** Jelaskan mengapa format raw (dd) masih digunakan meskipun tidak memiliki fitur kompresi dan metadata!

**Penyelesaian:**

*Step 1:* Identifikasi keunggulan unik format raw

*Step 2:* Analisis kasus penggunaan

**Alasan format raw masih relevan:**

1. **Kompatibilitas universal**: Dapat dibaca oleh hampir semua tools forensik dan utilitas OS
2. **Simplicity**: Tidak ada overhead format, data persis sama dengan aslinya
3. **Direct mounting**: Dapat di-mount langsung sebagai volume tanpa konversi
4. **No dependency**: Tidak bergantung pada software vendor tertentu
5. **Hashing sederhana**: Hash dari image identik dengan hash dari media asli

**Jawaban:** Format raw masih digunakan karena: (1) Kompatibilitas universal dengan semua tools, (2) Simplicity tanpa overhead format, (3) Kemampuan direct mounting, (4) Tidak bergantung pada vendor tertentu, dan (5) Hash image identik dengan hash media asli yang memudahkan verifikasi. Format raw ideal untuk backup format atau ketika interoperabilitas antar tools menjadi prioritas.

---

## 4. Akuisisi Volatile Data dan Memory Forensics

### 4.1 Pentingnya Akuisisi Volatile Data

> **Volatile Data** adalah data yang hilang ketika sumber tenaganya terputus, terutama data yang tersimpan di Random Access Memory (RAM), CPU registers, dan cache.

Menurut RFC 3227 (*Guidelines for Evidence Collection and Archiving*), pengumpulan bukti harus mengikuti **Order of Volatility** — dimulai dari data paling volatile:

| Urutan | Sumber Data | Volatilitas | Metode Akuisisi |
|--------|-------------|-------------|-----------------|
| 1 | CPU Registers, Cache | Sangat tinggi | Tidak praktis untuk diakuisisi |
| 2 | RAM (Memory) | Tinggi | Memory dump tools |
| 3 | Network State | Tinggi | Command capture |
| 4 | Running Processes | Tinggi | Process listing tools |
| 5 | Temporary Files | Sedang | File system tools |
| 6 | Disk/Storage | Rendah | Forensic imaging |
| 7 | Remote Logging | Rendah | Log collection |
| 8 | Physical Configuration | Sangat rendah | Fotografi |
| 9 | Archival Media | Sangat rendah | Standard copy |

### 4.2 Akuisisi RAM (Memory Dump)

Memory dump adalah proses mengambil snapshot dari isi RAM pada suatu titik waktu. Konten RAM yang dapat diambil mencakup:

- **Running processes** dan threads
- **Network connections** yang aktif
- **Encryption keys** yang sedang digunakan
- **Malware** yang hanya ada di memori (fileless malware)
- **User credentials** dan password
- **Registry hives** yang di-load
- **Chat messages** dan clipboard content

#### 4.2.1 Tools Akuisisi RAM

| Tool | Platform | Metode | Keterangan |
|------|----------|--------|------------|
| **Belkasoft RAM Capturer** | Windows | User-mode | Gratis, minimal footprint |
| **WinPMEM** | Windows | Kernel driver | Open source, bagian dari Rekall |
| **FTK Imager** | Windows | User-mode | Fungsi memory capture terintegrasi |
| **DumpIt** | Windows | User-mode | Portable, satu klik |
| **LiME** | Linux | Kernel module | Open source, untuk Linux |

> **Konteks Militer:** Dalam investigasi insiden di jaringan militer, akuisisi RAM harus menjadi langkah pertama sebelum tindakan lain. RAM mungkin berisi encryption key untuk volume terenkripsi, kredensial akses ke sistem terkait, atau malware yang beroperasi sepenuhnya di memori (fileless attack) yang semakin sering digunakan oleh APT yang menarget infrastruktur pertahanan.

#### Solved Problem 8 ⭐⭐

**Soal:** Seorang analis forensik militer menemukan workstation yang diduga terinfeksi malware fileless. Workstation masih menyala. Mengapa akuisisi RAM harus dilakukan sebelum shutdown?

**Penyelesaian:**

*Step 1:* Pahami karakteristik fileless malware

Fileless malware beroperasi sepenuhnya di RAM tanpa menyimpan file ke disk. Contoh teknik:
- PowerShell injection
- Process hollowing
- DLL injection
- Reflective loading

*Step 2:* Analisis dampak shutdown

| Data | Sebelum Shutdown | Setelah Shutdown |
|------|-----------------|-----------------|
| Malware code | ✅ Ada di RAM | ❌ Hilang permanen |
| Network C2 connections | ✅ Terlihat | ❌ Hilang |
| Encryption keys | ✅ Di RAM | ❌ Hilang |
| Injected processes | ✅ Terdeteksi | ❌ Tidak ada bukti |
| Disk artifacts | ✅ Ada | ✅ Ada |

**Jawaban:** Akuisisi RAM harus dilakukan sebelum shutdown karena fileless malware hanya ada di memori dan akan hilang permanen saat sistem dimatikan. Dengan melakukan memory dump, analis dapat memperoleh: (1) Kode malware untuk analisis, (2) Koneksi C2 aktif untuk tracing, (3) Encryption keys, dan (4) Bukti process injection. Setelah shutdown, satu-satunya yang tersisa hanyalah artefak disk yang mungkin minim atau tidak ada sama sekali untuk fileless malware.

---

#### Solved Problem 9 ⭐

**Soal:** Sebutkan empat jenis informasi forensik penting yang hanya dapat diperoleh dari RAM!

**Penyelesaian:**

*Step 1:* Identifikasi data yang eksklusif di RAM

**Empat jenis informasi forensik dari RAM:**

1. **Encryption keys**: Kunci dekripsi BitLocker, VeraCrypt, atau TrueCrypt yang hanya tersimpan di memori saat volume terbuka
2. **Fileless malware**: Kode malware yang beroperasi sepenuhnya di memori tanpa menyentuh disk
3. **Active network connections**: Detail koneksi jaringan real-time termasuk remote IP, port, dan PID
4. **Decrypted data**: Isi file yang sedang dibuka/edit dalam bentuk plaintext meskipun disimpan terenkripsi di disk

**Jawaban:** Empat informasi forensik yang hanya dapat diperoleh dari RAM adalah: (1) Encryption keys untuk volume terenkripsi, (2) Fileless malware yang beroperasi di memori, (3) Active network connections secara real-time, dan (4) Decrypted data dari file yang sedang dibuka dalam bentuk plaintext.

---

## 5. Verifikasi Integritas Data

### 5.1 Fungsi Hash Kriptografi

> **Fungsi Hash Kriptografi** adalah fungsi matematika yang mengubah input data dengan ukuran sembarang menjadi output fixed-length (digest/hash value) yang unik dan tidak dapat dibalik.

Properti penting fungsi hash untuk forensik:
1. **Deterministic**: Input yang sama selalu menghasilkan output yang sama
2. **One-way**: Tidak mungkin merekonstruksi input dari hash output
3. **Collision resistant**: Sangat sulit menemukan dua input berbeda dengan hash sama
4. **Avalanche effect**: Perubahan satu bit pada input menghasilkan perubahan signifikan pada output

#### 5.1.1 Algoritma Hash yang Umum Digunakan

| Algoritma | Panjang Output | Status | Penggunaan Forensik |
|-----------|---------------|--------|-------------------|
| **MD5** | 128 bit (32 hex) | Deprecated (collision found) | Legacy, masih sering digunakan |
| **SHA-1** | 160 bit (40 hex) | Deprecated (collision found 2017) | Transisi, masih diterima |
| **SHA-256** | 256 bit (64 hex) | Aman | Standar saat ini |
| **SHA-512** | 512 bit (128 hex) | Aman | High-security applications |

> **Rekomendasi:** Gunakan minimal SHA-256 sebagai standar. Untuk backward compatibility, sertakan juga MD5 agar kompatibel dengan tools legacy. Banyak tools forensik menghitung kedua hash secara bersamaan.

![Proses Verifikasi Hash](images/p04-04-hash-verification.png)

*Gambar 4.4: Proses verifikasi integritas menggunakan hash*

### 5.2 Proses Verifikasi dalam Akuisisi

Verifikasi integritas dilakukan dalam tiga tahap:

1. **Pre-acquisition hash**: Hitung hash dari media sumber sebelum akuisisi
2. **Post-acquisition hash**: Hitung hash dari forensic image setelah akuisisi
3. **Comparison**: Bandingkan kedua hash — harus identik

Contoh output verifikasi FTK Imager:

```
[Acquisition Details]
Source: \\.\PhysicalDrive1
Destination: D:\Cases\Case001\evidence.E01

[Computed Hashes]
MD5 checksum:    d41d8cd98f00b204e9800998ecf8427e
SHA1 checksum:   da39a3ee5e6b4b0d3255bfef95601890afd80709

[Verification Results]
MD5 Match:  VERIFIED
SHA1 Match: VERIFIED
```

#### Solved Problem 10 ⭐

**Soal:** Mengapa dalam forensik digital sebaiknya menggunakan lebih dari satu algoritma hash untuk verifikasi?

**Penyelesaian:**

*Step 1:* Pahami risiko penggunaan single hash

*Step 2:* Analisis manfaat multi-hash

**Alasan menggunakan lebih dari satu hash:**

1. **Collision resistance**: MD5 dan SHA-1 telah terbukti vulnerable terhadap collision attacks. Menggunakan dua algoritma berbeda membuat kemungkinan collision pada keduanya secara bersamaan sangat kecil
2. **Backward compatibility**: MD5 masih banyak digunakan di database forensik dan tools legacy
3. **Defense in depth**: Jika satu algoritma ditemukan lemah di masa depan, algoritma lain masih memberikan jaminan
4. **Legal acceptance**: Beberapa pengadilan masih menerima MD5 sementara yang lain sudah beralih ke SHA-256

**Jawaban:** Menggunakan lebih dari satu algoritma hash memberikan: (1) Ketahanan terhadap collision attacks pada algoritma yang sudah deprecated, (2) Backward compatibility dengan tools legacy, (3) Defense in depth terhadap kelemahan algoritma masa depan, dan (4) Penerimaan yang lebih luas di berbagai forum hukum dan tribunal militer.

---

#### Solved Problem 11 ⭐⭐

**Soal:** Setelah melakukan akuisisi USB drive 32GB, hash MD5 dari image adalah `a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6`. Seminggu kemudian, hash dihitung ulang dan hasilnya `a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d7` (berbeda di digit terakhir). Apa implikasi forensiknya?

**Penyelesaian:**

*Step 1:* Analisis perbedaan hash

Hash sebelumnya: `...c5d6`
Hash sekarang:   `...c5d7`

Perbedaan meskipun hanya satu karakter menunjukkan **image file telah berubah**. Karena avalanche effect, perubahan sekecil apapun pada data menghasilkan hash yang sangat berbeda — namun dalam kasus ini perbedaan hanya di satu digit terakhir, yang tetap berarti data berbeda.

*Step 2:* Evaluasi implikasi

1. **Integritas gagal**: Image tidak lagi identik dengan bukti asli
2. **Chain of custody terganggu**: Ada modifikasi pada bukti
3. **Kemungkinan penyebab**: Korupsi disk, bad sector, modifikasi tidak sengaja, atau tampering
4. **Admissibility terancam**: Bukti mungkin ditolak di pengadilan

**Jawaban:** Perbedaan hash, meskipun hanya satu digit, berarti image forensik telah berubah dan **integritas bukti gagal**. Implikasinya: (1) Image tidak lagi dapat diandalkan sebagai salinan autentik, (2) Chain of custody dipertanyakan, (3) Bukti mungkin ditolak di pengadilan atau tribunal militer. Investigator harus mencari penyebab perubahan dan jika mungkin melakukan akuisisi ulang dari media asli.

---

## 6. Remote Acquisition dan Network-Based Imaging

### 6.1 Konsep Remote Acquisition

> **Remote Acquisition** adalah proses akuisisi data forensik dari sistem yang berada di lokasi berbeda melalui jaringan, tanpa perlu akses fisik langsung ke media penyimpanan.

Remote acquisition menjadi semakin penting dalam konteks militer modern di mana:
- Aset digital tersebar di berbagai Kodam, Lantamal, dan Lanud
- Insiden siber dapat terjadi di lokasi terpencil
- Tim forensik terbatas dan tidak selalu dapat hadir secara fisik
- Respon cepat diperlukan sebelum bukti hilang

#### 6.1.1 Metode Remote Acquisition

| Metode | Deskripsi | Tools |
|--------|-----------|-------|
| **Agent-based** | Software agent terinstal di target | EnCase Enterprise, F-Response |
| **Agentless** | Menggunakan protokol remote yang ada | WMI, PowerShell Remoting, SSH |
| **Network boot** | Boot target dari forensic image melalui jaringan | PXE boot + forensic tools |
| **Cloud API** | Akuisisi dari cloud service melalui API | AWS CLI, Azure Tools |

#### Solved Problem 12 ⭐⭐

**Soal:** Sebuah Lantamal di Indonesia Timur melaporkan dugaan intrusi pada servernya. Tim forensik berada di Jakarta. Jelaskan langkah remote acquisition yang dapat dilakukan!

**Penyelesaian:**

*Step 1:* Evaluasi infrastruktur dan konektivitas
- Jarak jauh → harus remote
- Perlu minimal personel lokal yang terlatih
- Bandwidth jaringan militer harus diperhitungkan

*Step 2:* Rancang prosedur remote acquisition

1. **Persiapan Remote**:
   - Verifikasi konektivitas VPN militer Jakarta-Lantamal
   - Pastikan bandwidth memadai untuk transfer data
   - Siapkan personel lokal untuk menjalankan instruksi

2. **Akuisisi RAM** (via remote/personel lokal):
   - Kirim Belkasoft RAM Capturer via secure channel
   - Instruksikan personel lokal untuk menjalankan memory dump
   - Transfer file dump melalui encrypted channel

3. **Logical Acquisition** (via remote):
   - Gunakan PowerShell Remoting atau F-Response
   - Kumpulkan log, registry hives, dan file kritis
   - Hash setiap file yang ditransfer

4. **Physical Acquisition** (jika bandwidth memungkinkan):
   - Gunakan FTK Imager Lite yang dijalankan personel lokal
   - Simpan image ke storage lokal, kirimkan fisik jika bandwidth tidak memadai

**Jawaban:** Langkah remote acquisition: (1) Verifikasi konektivitas VPN dan bandwidth, (2) Akuisisi RAM via personel lokal dengan tools portable, (3) Logical acquisition via PowerShell Remoting atau F-Response untuk log dan file kritis, (4) Physical acquisition ke storage lokal jika bandwidth tidak memadai untuk transfer online. Semua transfer harus melalui encrypted channel dan setiap file harus di-hash untuk verifikasi.

---

## 7. Tantangan Akuisisi pada Media Modern

### 7.1 Tantangan SSD (Solid State Drive)

SSD menghadirkan tantangan unik bagi forensik digital karena arsitektur dan mekanisme operasinya yang berbeda dari HDD:

| Tantangan | Deskripsi | Dampak Forensik |
|-----------|-----------|----------------|
| **TRIM** | Perintah OS ke SSD untuk menghapus block yang tidak digunakan | Data terhapus tidak dapat di-recover |
| **Garbage Collection** | SSD secara internal menghapus block kosong | Data hilang tanpa perintah OS |
| **Wear Leveling** | Data dipindahkan antar cell untuk meratakan keausan | Lokasi data tidak statis |
| **Over-provisioning** | Area tersembunyi untuk manajemen SSD | Area tidak dapat diakses dengan tools standar |
| **Encryption** | SSD modern sering memiliki hardware encryption | Self-encrypting drive (SED) mengunci data |

> **Implikasi:** Untuk SSD, waktu antara penghapusan data dan akuisisi sangat kritis. Semakin cepat akuisisi dilakukan setelah insiden, semakin besar kemungkinan data yang dihapus masih dapat di-recover (sebelum TRIM dan garbage collection menghapusnya secara permanen).

#### Solved Problem 13 ⭐⭐

**Soal:** Sebuah laptop militer dengan SSD NVMe dan BitLocker aktif disita dalam keadaan sleep mode. Jelaskan strategi akuisisi terbaik!

**Penyelesaian:**

*Step 1:* Analisis situasi
- SSD NVMe → tantangan TRIM dan garbage collection
- BitLocker aktif → disk terenkripsi
- Sleep mode → RAM masih berisi data, termasuk kemungkinan BitLocker key

*Step 2:* Tentukan strategi

**KRITIS: Jangan matikan laptop!**

1. **Akuisisi RAM** (prioritas tertinggi):
   - Bangunkan laptop dari sleep mode (tanpa login)
   - Jalankan RAM Capturer dari USB
   - BitLocker key kemungkinan masih di RAM

2. **Extract BitLocker Key dari RAM**:
   - Analisis memory dump menggunakan Volatility
   - Plugin `bitlocker` untuk ekstraksi Full Volume Encryption Key (FVEK)

3. **Physical Acquisition** (setelah key diperoleh):
   - Gunakan key untuk mendekripsi volume
   - Buat physical image dari volume yang sudah didekripsi
   - Atau image terenkripsi + key terpisah

4. **Dokumentasi**:
   - Catat semua langkah
   - Hash semua file yang dihasilkan
   - Dokumentasikan state laptop (sleep, battery level, dll)

**Jawaban:** Strategi terbaik adalah **jangan matikan laptop** dan segera: (1) Akuisisi RAM untuk mengamankan BitLocker key yang masih tersimpan di memori, (2) Ekstraksi BitLocker FVEK dari memory dump menggunakan Volatility, (3) Physical acquisition setelah key diperoleh, (4) Dokumentasi lengkap. Mematikan laptop akan menghilangkan key enkripsi dan membuat data di SSD tidak dapat diakses.

---

### 7.2 Tantangan Media Terenkripsi

Enkripsi merupakan tantangan signifikan dalam forensik digital:

| Jenis Enkripsi | Contoh | Tantangan |
|---------------|--------|-----------|
| **Full Disk Encryption** | BitLocker, VeraCrypt, LUKS | Seluruh disk terenkripsi |
| **File-level Encryption** | EFS, AxCrypt | File individual terenkripsi |
| **Container Encryption** | VeraCrypt container | Volume virtual terenkripsi |
| **Hardware Encryption** | Self-Encrypting Drive (SED) | Transparan, selalu aktif |

Pendekatan mengatasi enkripsi:
1. **Live acquisition** saat key masih di RAM
2. **Key recovery** dari memory dump
3. **Password recovery** menggunakan brute-force/dictionary attack
4. **Compulsion** melalui perintah pengadilan (legal approach)
5. **Exploit known vulnerabilities** pada implementasi enkripsi

#### Solved Problem 14 ⭐⭐⭐

**Soal:** Tim forensik TNI menemukan server di pangkalan udara yang menggunakan VeraCrypt dengan hidden volume. Server sudah dimatikan. Jelaskan tantangan forensik dan pendekatan yang mungkin dilakukan!

**Penyelesaian:**

*Step 1:* Pahami tantangan VeraCrypt hidden volume

VeraCrypt hidden volume memiliki fitur **plausible deniability**:
- Volume tersembunyi berada di dalam volume biasa
- Tidak ada cara untuk membuktikan keberadaan hidden volume
- Password berbeda untuk outer volume dan hidden volume
- Tanpa password yang benar, hidden volume tidak terdeteksi

*Step 2:* Analisis situasi
- Server sudah mati → encryption key hilang dari RAM
- Hidden volume → keberadaannya tidak dapat dibuktikan secara teknis
- VeraCrypt → algoritma enkripsi kuat (AES-256, Twofish, Serpent)

*Step 3:* Rancang pendekatan

1. **Physical acquisition** dari seluruh disk (terenkripsi)
2. **Password recovery attempts**:
   - Dictionary attack dengan wordlist militer
   - Brute-force jika password pendek
   - Analisis artefak lain untuk petunjuk password
3. **Cari backup key atau password**:
   - Periksa dokumen fisik di sekitar server
   - Analisis perangkat lain milik operator (mungkin menyimpan password)
   - Email atau chat yang mungkin berisi informasi password
4. **Legal approach**:
   - Perintah pengadilan militer untuk tersangka membuka volume
   - Namun tersangka dapat memberikan password outer volume saja (plausible deniability)
5. **Side-channel analysis**:
   - Analisis file system timestamps dan access patterns
   - Korelasi dengan log jaringan dan aktivitas pengguna

**Jawaban:** Tantangan utama adalah server sudah mati (key hilang dari RAM) dan VeraCrypt hidden volume memiliki plausible deniability. Pendekatan yang mungkin: (1) Physical acquisition disk terenkripsi, (2) Password recovery melalui dictionary/brute-force attack, (3) Pencarian backup key dari sumber lain, (4) Pendekatan legal melalui perintah pengadilan militer, dan (5) Side-channel analysis melalui artefak dan korelasi log. Tantangan terbesar adalah membuktikan keberadaan hidden volume tanpa password yang benar.

---

### 7.3 Tantangan RAID Array

> **RAID (Redundant Array of Independent Disks)** menggabungkan beberapa disk menjadi satu logical volume, yang menghadirkan tantangan khusus dalam akuisisi forensik.

| Level RAID | Konfigurasi | Tantangan Forensik |
|-----------|-------------|-------------------|
| **RAID 0** | Striping | Data tersebar antar disk, semua disk diperlukan |
| **RAID 1** | Mirroring | Perlu mengidentifikasi disk yang paling up-to-date |
| **RAID 5** | Striping + distributed parity | Perlu mengetahui strip size, disk order, dan parity rotation |
| **RAID 10** | Mirror + stripe | Kompleksitas tinggi, perlu semua disk |

Pendekatan akuisisi RAID:
1. **Hardware RAID controller**: Image seluruh logical volume melalui controller
2. **Individual disk imaging**: Image setiap disk terpisah, rekonstruksi menggunakan tools
3. **Virtual reconstruction**: Gunakan tools seperti Autopsy RAID plugin atau R-Studio

#### Solved Problem 15 ⭐⭐⭐

**Soal:** Server data militer menggunakan RAID 5 dengan 4 disk. Satu disk telah gagal dan sudah diganti minggu lalu. Jelaskan implikasi forensik dan strategi akuisisi!

**Penyelesaian:**

*Step 1:* Pahami implikasi RAID 5 dengan disk replacement

- RAID 5 menggunakan striping dengan distributed parity
- Saat disk gagal, data di-rebuild dari parity
- Disk pengganti berisi data yang di-rebuild, bukan data historis asli
- Data dari disk lama yang gagal mungkin masih berisi bukti

*Step 2:* Rancang strategi akuisisi

1. **Image semua disk yang ada** (termasuk disk pengganti):
   - 3 disk asli yang masih berfungsi
   - 1 disk pengganti yang sudah di-rebuild

2. **Coba image disk yang gagal** (jika masih tersedia):
   - Disk lama mungkin masih memiliki data parsial
   - Gunakan recovery tools untuk membaca sektor yang masih bisa diakses

3. **Image logical volume** melalui RAID controller:
   - Image seluruh logical volume sebagai satu kesatuan
   - Ini memberikan view data yang lengkap (setelah rebuild)

4. **Dokumentasi konfigurasi RAID**:
   - Strip size, disk order, parity rotation
   - Catat bahwa disk telah diganti dan data di-rebuild

**Jawaban:** Implikasi forensik: (1) Disk pengganti hanya berisi data yang di-rebuild, bukan data historis, (2) Bukti yang mungkin ada di deleted/unallocated space disk asli yang gagal hilang dari array yang sudah di-rebuild. Strategi: (1) Image semua disk termasuk pengganti, (2) Coba recovery data dari disk lama yang gagal, (3) Image logical volume melalui controller, dan (4) Dokumentasikan konfigurasi RAID secara detail. Penting untuk mencatat bahwa bukti mungkin tidak lengkap karena disk replacement.

---

## 8. Tools dan Prosedur Akuisisi

### 8.1 FTK Imager

> **FTK Imager** adalah tool akuisisi forensik gratis dari Exterro (sebelumnya AccessData) yang mampu melakukan physical dan logical acquisition serta membuat image dalam berbagai format.

Kemampuan FTK Imager:
- Physical dan logical acquisition
- Memory capture (RAM dump)
- Format output: Raw (dd), SMART, E01, AFF
- Hashing otomatis (MD5, SHA-1, SHA-256)
- Preview evidence tanpa mounting
- Export individual files dari image

#### 8.1.1 Prosedur Akuisisi dengan FTK Imager

```
Langkah Akuisisi Physical dengan FTK Imager:
═══════════════════════════════════════════════
1. Koneksikan media sumber melalui write blocker
2. Buka FTK Imager → File → Create Disk Image
3. Pilih source type: "Physical Drive"
4. Pilih drive sumber (pastikan benar!)
5. Add destination → pilih format (E01 direkomendasikan)
6. Isi metadata kasus:
   - Case Number
   - Evidence Number
   - Unique Description
   - Examiner Name
   - Notes
7. Tentukan folder dan nama file output
8. Set fragment size (jika perlu split)
9. Centang "Verify images after they are created"
10. Klik Start
11. Tunggu proses selesai
12. Verifikasi hash match
13. Dokumentasikan hasil
```

### 8.2 Akuisisi dengan dd (WSL/Linux)

> **dd** adalah utilitas command-line Unix/Linux yang dapat melakukan bit-by-bit copy dari satu sumber ke tujuan. Pada Windows, dd dapat digunakan melalui Windows Subsystem for Linux (WSL).

Sintaks dasar dd:
```bash
dd if=/dev/sdb of=/path/to/image.dd bs=4096 conv=noerror,sync status=progress
```

| Parameter | Deskripsi |
|-----------|-----------|
| `if` | Input file (sumber) |
| `of` | Output file (tujuan) |
| `bs` | Block size (ukuran blok per operasi) |
| `conv=noerror` | Lanjutkan meskipun ada error |
| `conv=sync` | Padding zero untuk blok error |
| `status=progress` | Tampilkan progress |

Variasi dd yang lebih canggih:
- **dc3dd**: Versi forensic dari dd dengan hashing built-in
- **dcfldd**: Versi DoD Computer Forensics Lab dari dd dengan fitur tambahan

#### Solved Problem 16 ⭐⭐

**Soal:** Tulis perintah dd lengkap untuk mengakuisisi USB drive (`/dev/sdb`) ke file image, kemudian verifikasi hasilnya!

**Penyelesaian:**

*Step 1:* Tulis perintah akuisisi

```bash
# Akuisisi dengan dd
sudo dd if=/dev/sdb of=/mnt/cases/evidence_usb.dd bs=4096 \
  conv=noerror,sync status=progress

# Atau menggunakan dcfldd dengan hashing terintegrasi
sudo dcfldd if=/dev/sdb of=/mnt/cases/evidence_usb.dd bs=4096 \
  hash=md5,sha256 hashlog=/mnt/cases/evidence_usb.hashlog
```

*Step 2:* Verifikasi hash

```bash
# Hitung hash dari media sumber
sudo md5sum /dev/sdb > /mnt/cases/source_md5.txt
sudo sha256sum /dev/sdb > /mnt/cases/source_sha256.txt

# Hitung hash dari image
md5sum /mnt/cases/evidence_usb.dd > /mnt/cases/image_md5.txt
sha256sum /mnt/cases/evidence_usb.dd > /mnt/cases/image_sha256.txt

# Bandingkan
diff /mnt/cases/source_md5.txt /mnt/cases/image_md5.txt
diff /mnt/cases/source_sha256.txt /mnt/cases/image_sha256.txt
```

**Jawaban:** Perintah `dd if=/dev/sdb of=evidence_usb.dd bs=4096 conv=noerror,sync status=progress` untuk akuisisi, diikuti verifikasi dengan `md5sum` dan `sha256sum` pada sumber dan image. Hash harus identik untuk membuktikan integritas. Alternatif yang lebih baik adalah `dcfldd` yang melakukan hashing secara bersamaan dengan akuisisi.

---

### 8.3 Arsenal Image Mounter

> **Arsenal Image Mounter** adalah tool gratis yang memungkinkan mounting forensic image sebagai volume fisik atau virtual di Windows, mendukung format raw, E01, dan AFF4.

Kegunaan mounting forensic image:
- Menelusuri file system menggunakan Windows Explorer
- Menjalankan tools analisis yang memerlukan drive letter
- Melakukan triage cepat tanpa memulai full analysis di Autopsy
- Read-only mounting menjaga integritas image

#### Solved Problem 17 ⭐

**Soal:** Jelaskan perbedaan antara mounting forensic image sebagai "read-only" dan sebagai "writable"! Kapan masing-masing digunakan?

**Penyelesaian:**

*Step 1:* Bandingkan kedua mode

| Aspek | Read-Only Mount | Writable Mount |
|-------|----------------|----------------|
| **Modifikasi** | Tidak dapat menulis ke image | Dapat menulis (differencing disk) |
| **Integritas** | Terjaga 100% | Image asli terjaga (perubahan di diff file) |
| **Penggunaan** | Analisis dan review | Menjalankan OS dari image, testing |
| **Hash** | Tetap sama | Image asli tetap, diff file terpisah |

**Jawaban:** Read-only mount digunakan untuk analisis forensik karena menjaga integritas image sepenuhnya. Writable mount (melalui differencing disk) digunakan untuk menjalankan OS dari image atau menguji hipotesis, dimana perubahan disimpan di file terpisah sehingga image asli tetap utuh.

---

## 9. Akuisisi dari Sumber Khusus

### 9.1 Akuisisi Cloud Storage

Akuisisi dari cloud memiliki tantangan unik:
- Data tersimpan di server pihak ketiga
- Jurisdiksi hukum yang berbeda
- Ketergantungan pada API dan kooperasi provider
- Data dapat berubah secara real-time

Pendekatan akuisisi cloud:
1. **API-based**: Menggunakan API resmi provider (Google Takeout, Microsoft Graph)
2. **Client-based**: Akuisisi dari client sync folder di perangkat lokal
3. **Legal request**: Melalui permintaan resmi ke provider

#### Solved Problem 18 ⭐⭐

**Soal:** Seorang prajurit TNI diduga mengunggah dokumen rahasia ke layanan cloud pribadi. Data sudah tersinkronisasi ke laptopnya. Jelaskan dua pendekatan akuisisi yang dapat dilakukan!

**Penyelesaian:**

*Step 1:* Identifikasi sumber data

- Cloud server (remote) — memerlukan kerjasama provider
- Laptop (lokal) — folder sinkronisasi berisi copy data

*Step 2:* Rancang pendekatan

**Pendekatan 1: Akuisisi dari Laptop (Lokal)**
- Physical image dari laptop → includes sync folder
- Analisis folder sinkronisasi (e.g., Google Drive, OneDrive, Dropbox folder)
- Database sinkronisasi berisi metadata dan log aktivitas
- Browser artifacts: login credentials, access history

**Pendekatan 2: Akuisisi dari Cloud (Remote)**
- Permintaan resmi ke provider melalui jalur hukum
- Gunakan API provider jika memiliki kredensial sah (dengan izin pengadilan)
- Export menggunakan layanan takeout (Google Takeout, Microsoft Data Export)
- Catat timestamp dan metadata dari cloud

**Jawaban:** Dua pendekatan: (1) Akuisisi dari laptop dengan physical imaging yang mencakup folder sinkronisasi, database sync, dan browser artifacts, (2) Akuisisi dari cloud melalui permintaan resmi ke provider atau penggunaan API dengan izin pengadilan. Pendekatan laptop lebih cepat dan dalam kontrol investigator, sementara cloud acquisition memberikan data yang mungkin sudah dihapus dari laptop.

---

### 9.2 Akuisisi Storage Virtualized

Dalam lingkungan militer modern, banyak sistem menggunakan virtualisasi:

| Platform | Format Image | Metode Akuisisi |
|----------|-------------|-----------------|
| **VMware** | VMDK | Copy file VMDK + snapshot files |
| **Hyper-V** | VHDX | Copy file VHDX + AVHDX (diff) |
| **VirtualBox** | VDI | Copy file VDI |
| **KVM/QEMU** | QCOW2 | Copy file QCOW2 |

#### Solved Problem 19 ⭐⭐

**Soal:** Server fisik di Pusdiksus TNI menjalankan 5 virtual machine menggunakan VMware ESXi. Bagaimana cara melakukan akuisisi forensik terhadap salah satu VM yang dicurigai?

**Penyelesaian:**

*Step 1:* Identifikasi komponen VM yang perlu diakuisisi

Komponen VMware VM:
- `.vmdk` — virtual disk file (data utama)
- `.vmx` — konfigurasi VM
- `.vmsd` — snapshot metadata
- `.vmsn` — snapshot state
- `.log` — VM log files
- `.nvram` — BIOS/UEFI settings

*Step 2:* Rancang prosedur akuisisi

1. **Jika VM bisa di-suspend**: Suspend VM → copy semua file VM → hash semua file
2. **Jika VM harus tetap berjalan**: Buat snapshot → copy snapshot + base disk → hash
3. **Live acquisition dalam VM**: Jalankan tools forensik di dalam VM untuk RAM dan disk
4. **ESXi host level**: Akses datastore melalui SSH/SFTP dan copy file

**Jawaban:** Akuisisi VM VMware: (1) Jika memungkinkan, suspend VM kemudian copy semua file VM (.vmdk, .vmx, .vmsd, .log), (2) Jika VM harus tetap aktif, buat snapshot terlebih dahulu, (3) Pertimbangkan juga live acquisition dari dalam VM untuk volatile data, dan (4) Hash semua file untuk verifikasi. Seluruh file VM harus dicopy, bukan hanya .vmdk, karena metadata dan log mengandung informasi forensik penting.

---

## 10. Dokumentasi dan Pelaporan Akuisisi

### 10.1 Acquisition Log

Setiap akuisisi harus didokumentasikan dalam acquisition log yang mencakup:

```
╔══════════════════════════════════════════════════════╗
║           FORENSIC ACQUISITION LOG                    ║
╠══════════════════════════════════════════════════════╣
║ Case Number    : [Nomor Kasus]                       ║
║ Evidence Item  : [Nomor Bukti]                       ║
║ Date/Time      : [Tanggal dan Waktu]                 ║
║ Examiner       : [Nama dan NIP]                      ║
║                                                      ║
║ SOURCE INFORMATION                                   ║
║ Device Type    : [HDD/SSD/USB/RAM/dll]              ║
║ Make/Model     : [Merk dan Model]                    ║
║ Serial Number  : [Nomor Seri]                        ║
║ Capacity       : [Kapasitas]                         ║
║                                                      ║
║ ACQUISITION DETAILS                                  ║
║ Method         : [Physical/Logical/Memory]           ║
║ Tool           : [Nama Tool dan Versi]               ║
║ Write Blocker  : [Tipe dan Serial]                   ║
║ Format         : [Raw/E01/AFF4]                      ║
║ Start Time     : [HH:MM:SS]                         ║
║ End Time       : [HH:MM:SS]                         ║
║                                                      ║
║ VERIFICATION                                         ║
║ Source MD5     : [hash]                              ║
║ Image MD5      : [hash]                              ║
║ MD5 Match      : [YES/NO]                            ║
║ Source SHA-256 : [hash]                              ║
║ Image SHA-256  : [hash]                              ║
║ SHA-256 Match  : [YES/NO]                            ║
║                                                      ║
║ NOTES                                                ║
║ [Catatan tambahan]                                   ║
║                                                      ║
║ Examiner Signature: ________________                 ║
║ Witness Signature : ________________                 ║
╚══════════════════════════════════════════════════════╝
```

#### Solved Problem 20 ⭐⭐

**Soal:** Mengapa acquisition log harus menyertakan informasi serial number perangkat, versi tool yang digunakan, dan tanda tangan saksi?

**Penyelesaian:**

*Step 1:* Analisis pentingnya setiap elemen

| Elemen | Alasan |
|--------|--------|
| **Serial number** | Mengidentifikasi secara unik perangkat bukti, mencegah penukaran |
| **Versi tool** | Memungkinkan reproduksi proses yang identik, penting jika ada dispute |
| **Tanda tangan saksi** | Memvalidasi bahwa proses benar-benar terjadi sesuai dokumentasi |

*Step 2:* Hubungkan dengan prinsip forensik

- **Chain of custody**: Serial number membuktikan perangkat yang sama
- **Repeatability**: Versi tool memungkinkan replikasi
- **Accountability**: Tanda tangan saksi mencegah penyangkalan

**Jawaban:** Serial number perangkat diperlukan untuk mengidentifikasi bukti secara unik dan mencegah penukaran. Versi tool dicatat untuk memungkinkan reproduksi proses yang identik jika dipertanyakan. Tanda tangan saksi memvalidasi bahwa akuisisi benar-benar dilakukan sesuai prosedur. Ketiganya mendukung chain of custody, repeatability, dan accountability yang merupakan pilar dasar forensik digital.

---

#### Solved Problem 21 ⭐⭐⭐

**Soal:** Dalam sebuah operasi intelijen, tim forensik TNI mengamankan 10 perangkat digital dari sebuah lokasi. Waktu yang tersedia hanya 2 jam sebelum lokasi harus dikosongkan. Buatkan prioritas dan strategi akuisisi!

**Penyelesaian:**

*Step 1:* Inventaris dan klasifikasi perangkat

Contoh 10 perangkat yang mungkin ditemukan:
1. Laptop (menyala) — prioritas SANGAT TINGGI
2. Laptop (mati) — prioritas TINGGI
3. Desktop PC — prioritas TINGGI
4. Smartphone #1 — prioritas TINGGI
5. Smartphone #2 — prioritas TINGGI
6. USB drive #1 — prioritas SEDANG
7. USB drive #2 — prioritas SEDANG
8. External HDD — prioritas SEDANG
9. Tablet — prioritas TINGGI
10. SD Card — prioritas SEDANG

*Step 2:* Alokasi waktu (120 menit total)

| Waktu | Aktivitas | Perangkat |
|-------|-----------|-----------|
| 0-10 min | Dokumentasi fotografis semua perangkat | Semua |
| 10-25 min | RAM dump laptop menyala | Laptop #1 |
| 10-25 min | (paralel) Airplane mode smartphones | Smartphone #1, #2 |
| 25-55 min | Image USB drives dan SD Card (kecil, cepat) | USB #1, #2, SD Card |
| 25-55 min | (paralel) Logical acquisition smartphones | Smartphone #1, #2 |
| 55-90 min | Physical image external HDD | External HDD |
| 90-120 min | Label dan packaging semua perangkat | Semua |

*Step 3:* Strategi jika waktu habis

- Perangkat yang belum di-image → seize secara fisik untuk image di lab
- Prioritaskan volatile data → pastikan RAM dump dan smartphone selesai
- Perangkat media kecil (USB, SD) → seize dan image di lab

**Jawaban:** Strategi 2 jam: (1) 10 menit pertama untuk dokumentasi foto semua perangkat, (2) RAM dump laptop yang menyala sebagai prioritas utama (volatile), (3) Secara paralel, aktifkan airplane mode pada smartphone, (4) Image media kecil (USB, SD Card) karena cepat selesai, (5) Logical acquisition smartphone, (6) Physical image external HDD, (7) Label dan packaging. Perangkat yang tidak sempat di-image harus di-seize secara fisik untuk di-image di laboratorium forensik. Seluruh proses didokumentasikan dalam acquisition log.

---

#### Solved Problem 22 ⭐⭐⭐

**Soal:** Seorang jaksa penuntut militer mempertanyakan validitas bukti digital karena examiner tidak menggunakan write blocker saat akuisisi dan hanya menggunakan MD5 untuk verifikasi. Buatkan argumen teknis yang menjelaskan potensi masalah ini!

**Penyelesaian:**

*Step 1:* Analisis masalah pertama — Tidak menggunakan write blocker

**Risiko tanpa write blocker:**
1. OS otomatis memodifikasi media saat dikoneksikan
2. Timestamp file berubah (last accessed time)
3. Journal filesystem ter-update (NTFS $LogFile, $MFT)
4. Autoplay/autorun dapat mengeksekusi dan memodifikasi data
5. Antivirus dapat menambah/mengubah file

**Dampak:** Hash media setelah terkoneksi tanpa write blocker akan berbeda dari hash sebelumnya, sehingga tidak ada baseline yang clean untuk perbandingan.

*Step 2:* Analisis masalah kedua — Hanya menggunakan MD5

**Kelemahan MD5:**
1. Collision attack ditemukan oleh Wang et al. (2004)
2. Practical collision attack didemonstrasikan (2012)
3. Identical-prefix collision memungkinkan dua file berbeda dengan hash MD5 sama
4. Tidak lagi direkomendasikan oleh NIST untuk aplikasi keamanan

**Catatan:** Meskipun collision attack pada MD5 memerlukan upaya khusus, ketiadaan algoritma hash yang lebih kuat memberikan celah untuk mempertanyakan integritas bukti.

*Step 3:* Rumuskan argumen teknis

**Argumen jaksa yang valid:**
1. Tanpa write blocker, tidak ada jaminan bukti tidak termodifikasi → **violation of evidence integrity principle**
2. MD5 telah deprecated oleh NIST → **standar verifikasi di bawah best practice**
3. Gabungan keduanya menunjukkan prosedur yang tidak memenuhi standar forensik

**Counter-argument potensial dari examiner:**
1. Jika ada hash yang dihitung sebelum koneksi (misalnya dari chain of custody sebelumnya), perubahan dapat diidentifikasi dan dijelaskan
2. MD5 collision masih sulit dilakukan secara targeted pada forensic image
3. Namun, best practice seharusnya diikuti

**Jawaban:** Argumen teknis: (1) Tanpa write blocker, OS dapat memodifikasi bukti secara otomatis (timestamp, journal, autoplay), sehingga integritas bukti tidak dapat dibuktikan. (2) MD5 telah terbukti vulnerable terhadap collision attacks sejak 2004 dan tidak lagi direkomendasikan NIST, sehingga verifikasi integritas tidak memenuhi standar forensik terkini. (3) Kombinasi kedua kekurangan ini secara signifikan melemahkan chain of custody dan dapat menjadi dasar untuk menolak bukti di pengadilan militer. Examiner seharusnya menggunakan hardware write blocker dan minimal SHA-256 sebagai standar hash.

---

## Supplementary Problems

### Problem S1 ⭐
**Soal:** Sebutkan tiga perbedaan antara hardware write blocker dan software write blocker!

**Jawaban:** (1) Hardware write blocker independen dari OS sedangkan software bergantung pada OS, (2) Hardware write blocker lebih mahal tapi lebih terpercaya, (3) Hardware write blocker mendukung berbagai interface fisik sementara software write blocker terbatas pada yang didukung OS. Hardware write blocker lebih direkomendasikan dalam investigasi formal.

---

### Problem S2 ⭐
**Soal:** Apa yang dimaksud dengan "forensic image" dan mengapa penting untuk membuat working copy?

**Jawaban:** Forensic image adalah salinan bit-by-bit dari seluruh media penyimpanan. Working copy (salinan kedua dari forensic image) penting karena: (1) Analisis dilakukan pada working copy sehingga image master tetap pristine, (2) Jika working copy rusak, dapat dibuat ulang dari master, (3) Menjaga integritas bukti asli sesuai prinsip forensik.

---

### Problem S3 ⭐⭐
**Soal:** Jelaskan perbedaan antara HPA (Host Protected Area) dan DCO (Device Configuration Overlay) pada hard disk!

**Jawaban:** HPA adalah area tersembunyi di akhir hard disk yang dikonfigurasi oleh BIOS/firmware dan tidak terlihat oleh OS, sedangkan DCO adalah area yang dikonfigurasi oleh manufacturer untuk mengurangi kapasitas yang terlihat. Keduanya dapat digunakan untuk menyembunyikan data. HPA dapat dideteksi dengan membandingkan native max address dan user max address menggunakan tools seperti hdparm, sedangkan DCO memerlukan tools khusus untuk deteksi. Keduanya harus diperiksa dalam investigasi forensik menyeluruh.

---

### Problem S4 ⭐⭐
**Soal:** Mengapa akuisisi SSD harus dilakukan sesegera mungkin setelah insiden?

**Jawaban:** Karena SSD memiliki mekanisme TRIM dan garbage collection yang secara otomatis menghapus data dari block yang tidak digunakan. Setelah file dihapus, TRIM memberi tahu SSD controller bahwa block tersebut tidak diperlukan, dan garbage collection akan membersihkannya secara permanen dalam waktu yang tidak dapat diprediksi. Semakin cepat akuisisi dilakukan, semakin besar kemungkinan data yang dihapus masih ada sebelum garbage collection membersihkannya.

---

### Problem S5 ⭐⭐⭐
**Soal:** Dalam konteks militer, bandingkan strategi akuisisi untuk insiden siber di: (a) Markas besar dengan infrastruktur lengkap, dan (b) Pos lapangan dengan infrastruktur terbatas!

**Jawaban:**
**(a) Markas besar:**
- Full forensic lab tersedia → physical acquisition lengkap
- Write blocker hardware tersedia → integritas terjamin
- Bandwidth jaringan besar → remote acquisition ke server forensik
- Tim forensik berpengalaman hadir → prosedur standar penuh
- Storage besar → image dalam format E01 dengan kompresi

**(b) Pos lapangan:**
- Infrastruktur terbatas → prioritas triage dan logical acquisition
- Mungkin tidak ada write blocker → gunakan software write blocker atau boot dari forensic USB
- Bandwidth terbatas → image lokal ke portable storage, kirim fisik
- Personel terbatas → gunakan tools otomatis dan portable (DumpIt, FTK Imager Lite)
- Storage terbatas → prioritaskan volatile data dan data kritis, image ke format terkompresi

Konteks operasional menentukan metode dan skala akuisisi, namun prinsip dasar (integritas, verifikasi, dokumentasi) tetap harus dipertahankan.

---

## Ringkasan

| Konsep | Deskripsi Singkat |
|--------|-------------------|
| **Akuisisi Forensik** | Pembuatan salinan bitwise yang akurat dari media penyimpanan digital |
| **Write Blocker** | Perangkat yang mencegah penulisan ke media bukti saat akuisisi |
| **Physical Acquisition** | Salinan bit-by-bit seluruh media termasuk unallocated space |
| **Logical Acquisition** | Salinan file dan folder yang terlihat pada file system aktif |
| **Sparse Acquisition** | Salinan area tertentu berdasarkan kriteria spesifik |
| **Live vs Dead Acquisition** | Live pada sistem aktif (volatile data), dead pada sistem mati (lebih stabil) |
| **Format Raw (dd)** | Salinan exact tanpa kompresi atau metadata, kompatibilitas tertinggi |
| **Format E01** | Format EnCase dengan kompresi, metadata kasus, CRC per block |
| **Format AFF4** | Format open source dengan kompresi, signing, dan enkripsi |
| **Memory Forensics** | Akuisisi dan analisis RAM untuk bukti volatile |
| **Hash Verification** | MD5, SHA-1, SHA-256 untuk membuktikan integritas image |
| **Remote Acquisition** | Akuisisi melalui jaringan untuk sistem di lokasi berbeda |
| **Tantangan SSD** | TRIM, garbage collection, wear leveling menghambat recovery |
| **Tantangan Enkripsi** | Full disk encryption memerlukan key yang mungkin hanya di RAM |
| **Tantangan RAID** | Konfigurasi multi-disk memerlukan pemahaman arsitektur array |
| **Acquisition Log** | Dokumentasi lengkap proses akuisisi termasuk hash dan metadata |
| **Tools Utama** | FTK Imager, dd/dcfldd, Belkasoft RAM Capturer, Arsenal Image Mounter |

---

## Referensi

1. Casey, E. (2022). *Digital Evidence and Computer Crime: Forensic Science, Computers, and the Internet* (4th Ed.). Academic Press. Chapter 5-6.
2. Phillips, A., Nelson, B., & Steuart, C. (2022). *Guide to Computer Forensics and Investigations* (6th Ed.). Cengage Learning. Chapter 3.
3. Nikkel, B. (2021). *Practical Forensic Imaging: Securing Digital Evidence with Linux Tools* (2nd Ed.). No Starch Press. Chapter 1-3, 7-8.
4. Carrier, B. (2005). *File System Forensic Analysis*. Addison-Wesley. Chapter 3.
5. NIST SP 800-86: Guide to Integrating Forensic Techniques into Incident Response.
6. ISO/IEC 27037:2012 — Guidelines for identification, collection, acquisition and preservation of digital evidence.
7. RFC 3227: Guidelines for Evidence Collection and Archiving.
8. UU No. 1 Tahun 2024 tentang Informasi dan Transaksi Elektronik.

---

## License / Lisensi

This repository is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

Commercial use is permitted, provided attribution is given to the author.

© 2026 Anindito
