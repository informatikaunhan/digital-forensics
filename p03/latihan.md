# Latihan Pertemuan 03: Hard Disk dan Sistem File

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 03  
**Topik:** Hard Disk dan Sistem File  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Petunjuk Pengerjaan

1. Kerjakan semua soal secara mandiri
2. Waktu pengerjaan: 120 menit
3. Untuk soal pilihan ganda, pilih satu jawaban yang paling tepat
4. Untuk soal uraian, jawab dengan lengkap dan sistematis
5. Studi kasus memerlukan analisis mendalam dan penerapan konsep

---

## Bagian A: Soal Pilihan Ganda (20 Soal)

### Soal 1
Komponen HDD yang berfungsi sebagai media penyimpanan data secara magnetik adalah:

A. Controller board  
B. Spindle motor  
C. Platter  
D. Actuator arm  
E. Read/Write head

---

### Soal 2
Metode pengalamatan disk modern yang menggantikan CHS dan menggunakan pengalamatan linier adalah:

A. FAT  
B. LBA  
C. MBR  
D. GPT  
E. NTFS

---

### Soal 3
Berapakah batas kapasitas geometri CHS yang menjadi alasan beralih ke LBA?

A. 2 GB  
B. 4 GB  
C. 8,4 GB  
D. 2 TB  
E. 128 TB

---

### Soal 4
Tipe sel NAND flash yang paling cocok untuk keperluan militer karena daya tahan dan keandalan tertinggi adalah:

A. QLC  
B. TLC  
C. MLC  
D. SLC  
E. PLC

---

### Soal 5
Berapa ukuran total struktur Master Boot Record (MBR)?

A. 446 bytes  
B. 512 bytes  
C. 1024 bytes  
D. 2048 bytes  
E. 4096 bytes

---

### Soal 6
Berapa jumlah maksimal partisi primer yang didukung oleh skema MBR?

A. 2  
B. 4  
C. 8  
D. 16  
E. 128

---

### Soal 7
Boot signature yang menandakan MBR valid pada offset 0x1FE adalah:

A. 0xAA55  
B. 0x55AA  
C. 0xEF01  
D. 0xFF00  
E. 0x0000

---

### Soal 8
Keunggulan utama GPT dibandingkan MBR dari perspektif forensik adalah:

A. Ukuran partisi lebih kecil  
B. Tidak memerlukan UEFI  
C. Backup header dan entries memberikan redundansi  
D. Kompatibel dengan BIOS lama  
E. Menggunakan CHS addressing

---

### Soal 9
Batas ukuran file maksimal pada FAT32 yang menjadi kendala penyimpanan image forensik besar adalah:

A. 1 GB  
B. 2 GB  
C. 4 GB  
D. 8 GB  
E. 16 GB

---

### Soal 10
Sistem file Windows yang menyediakan fitur journaling, Alternate Data Streams, dan Master File Table adalah:

A. FAT12  
B. FAT16  
C. FAT32  
D. exFAT  
E. NTFS

---

### Soal 11
Dalam NTFS, database yang menyimpan metadata semua file dan direktori disebut:

A. File Allocation Table  
B. Master File Table  
C. Inode Table  
D. Catalog File  
E. Super Block

---

### Soal 12
Ukuran standar satu entri MFT pada NTFS biasanya adalah:

A. 128 bytes  
B. 256 bytes  
C. 512 bytes  
D. 1024 bytes  
E. 4096 bytes

---

### Soal 13
Atribut NTFS yang menyimpan timestamps MACE dan TIDAK DAPAT dimanipulasi oleh program user-mode adalah:

A. $STANDARD_INFORMATION  
B. $FILE_NAME  
C. $DATA  
D. $SECURITY_DESCRIPTOR  
E. $INDEX_ROOT

---

### Soal 14
Teknik anti-forensik yang memanipulasi timestamp file untuk mengecoh investigator disebut:

A. Steganografi  
B. Data carving  
C. Timestomping  
D. Slack hiding  
E. Log tampering

---

### Soal 15
Fitur NTFS yang memungkinkan data tersembunyi dilampirkan pada file tanpa mengubah ukuran yang terlihat di Explorer adalah:

A. File compression  
B. EFS encryption  
C. Alternate Data Streams  
D. Sparse files  
E. Hard links

---

### Soal 16
Area antara akhir data file dan akhir cluster yang dialokasikan, yang berpotensi menyimpan data residual, disebut:

A. Unallocated space  
B. Free space  
C. Slack space  
D. Bad sector  
E. Partition gap

---

### Soal 17
Fitur unik pada sistem file ext4 yang sangat berguna untuk forensik dan TIDAK dimiliki oleh NTFS adalah:

A. Journaling  
B. Timestamps  
C. Deletion time (dtime)  
D. File permissions  
E. Directory indexing

---

### Soal 18
Perintah TRIM pada SSD menyebabkan tantangan forensik karena:

A. Mengenkripsi data yang dihapus  
B. Memindahkan data ke partisi lain  
C. Menghapus data secara fisik dari flash  
D. Mengkompresi data yang tidak terpakai  
E. Menduplikasi data di lokasi berbeda

---

### Soal 19
Mekanisme internal SSD yang mendistribusikan penulisan secara merata ke seluruh sel NAND disebut:

A. TRIM  
B. Garbage collection  
C. Wear leveling  
D. Over-provisioning  
E. Bad block management

---

### Soal 20
Pada proses penghapusan file di NTFS, apa yang terjadi saat file dikosongkan dari Recycle Bin?

A. Data dihapus secara fisik dari disk  
B. Cluster langsung di-overwrite dengan nol  
C. MFT entry ditandai "not in use" tetapi data tetap ada  
D. File dipindahkan ke area terproteksi  
E. Sector disk ditandai sebagai bad sector

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Jelaskan perbedaan arsitektur antara HDD dan SSD! Sertakan minimal tiga aspek perbandingan yang relevan untuk forensik digital.

---

### Soal 2 ⭐
Gambarkan struktur 512 bytes MBR dan jelaskan fungsi masing-masing bagiannya!

---

### Soal 3 ⭐
Sebutkan minimal empat keunggulan GPT dibandingkan MBR! Mengapa GPT lebih reliable untuk forensik?

---

### Soal 4 ⭐
Jelaskan mekanisme kerja FAT sebagai linked list! Mengapa FAT32 memiliki batas file 4 GB?

---

### Soal 5 ⭐⭐
Jelaskan struktur MFT entry pada NTFS! Sertakan penjelasan tentang signature, flags, dan minimal tiga atribut utama beserta type code-nya.

---

### Soal 6 ⭐⭐
Apa yang dimaksud dengan timestamps MACE pada NTFS? Jelaskan perbedaan antara timestamps di $STANDARD_INFORMATION dan $FILE_NAME, serta mengapa perbedaan ini penting untuk forensik!

---

### Soal 7 ⭐⭐
Bagaimana cara mendeteksi timestomping pada file NTFS? Jelaskan langkah-langkah deteksi dan berikan contoh anomali yang menunjukkan timestomping!

---

### Soal 8 ⭐⭐
Jelaskan perbedaan antara $LogFile dan $UsnJrnl pada NTFS! Bagaimana keduanya saling melengkapi dalam investigasi forensik?

---

### Soal 9 ⭐⭐
Apa itu Alternate Data Streams (ADS)? Jelaskan penggunaan legitimate dan malicious dari ADS, serta bagaimana cara mendeteksinya!

---

### Soal 10 ⭐⭐
Hitung slack space yang terjadi jika file berukuran 10.500 bytes disimpan pada volume NTFS dengan ukuran cluster 4.096 bytes! Jelaskan komponen RAM slack dan drive slack.

---

### Soal 11 ⭐⭐
Bandingkan MFT entry (NTFS) dengan inode (ext4) dari aspek ukuran, timestamps, deletion time, dan kemampuan mendeteksi timestomping!

---

### Soal 12 ⭐⭐⭐
Seorang investigator menemukan laptop dengan dual-boot Windows 10 (NTFS) dan Ubuntu (ext4). File mencurigakan bernama "operation_plan.pdf" ditemukan di partisi NTFS dengan timestamps berikut:

- $STANDARD_INFORMATION Created: 2024-03-15 08:00:00
- $FILE_NAME Created: 2025-12-01 14:30:00
- $STANDARD_INFORMATION Modified: 2024-06-20 10:15:00

Pada partisi ext4, ditemukan file "ops_backup.pdf" yang identik (hash sama) dengan:
- mtime: 2025-12-01 14:25:00
- ctime: 2025-12-01 14:25:00
- dtime: 0 (tidak dihapus)

Analisis temuan ini! Apa kesimpulan Anda tentang timeline sebenarnya dan teknik anti-forensik yang digunakan?

---

### Soal 13 ⭐⭐⭐
Jelaskan mengapa recovery data pada SSD jauh lebih sulit dibandingkan HDD! Bahas tiga mekanisme SSD (TRIM, wear leveling, garbage collection) dan dampaknya terhadap forensik. Apa strategi alternatif yang dapat digunakan investigator?

---

### Soal 14 ⭐⭐⭐
Anda ditugaskan menganalisis disk image dari sebuah server militer yang menggunakan GPT. Saat pemeriksaan, ditemukan bahwa GPT header utama (LBA 1) rusak/corrupt.

a. Jelaskan langkah-langkah recovery menggunakan backup GPT header!  
b. Di mana lokasi backup GPT header dan entries?  
c. Informasi apa saja yang dapat diperoleh dari GPT header?  
d. Bagaimana CRC32 checksum membantu verifikasi integritas?

---

### Soal 15 ⭐⭐⭐
Dalam kasus investigasi kebocoran data di markas Kodam, ditemukan file NTFS dengan data runs sebagai berikut pada atribut $DATA:

```
Data Run 1: Offset 100, Length 5 clusters
Data Run 2: Offset +200, Length 3 clusters  
Data Run 3: Offset -50, Length 2 clusters
```

Cluster size = 4.096 bytes.

a. Hitung lokasi fisik (cluster number) setiap data run!  
b. Berapa total ukuran file yang dialokasikan?  
c. Apa yang ditunjukkan oleh adanya tiga data run (bukan satu)?  
d. Bagaimana cara merekonstruksi file lengkap dari data runs ini?

---


## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Investigasi Kebocoran Dokumen di Lanud (Pangkalan Udara)

**Latar Belakang:**

Tim SOC Lanud X mendeteksi anomali pada server file sharing internal. Dokumen bertanda "RAHASIA" mengenai rute penerbangan VIP ditemukan bocor ke pihak luar melalui email pribadi seorang teknisi. Server menggunakan Windows Server 2019 dengan NTFS pada disk 1 TB (GPT).

Saat investigasi awal, ditemukan:
- Laptop teknisi menggunakan SSD NVMe 512 GB, TRIM aktif
- USB drive 64 GB (FAT32) disita dari teknisi
- Terdapat file "jadwal_VIP.docx" di server dengan ukuran Explorer 45 KB, namun FTK Imager menunjukkan total 12,5 MB
- Pada USB drive, file dengan nama "foto_liburan.zip" memiliki hash identik dengan dokumen rahasia
- Log $UsnJrnl server menunjukkan file "jadwal_VIP.docx" di-rename dari "RAHASIA_rute.docx" pada tanggal 20 November 2025
- Timestamps $SI pada "jadwal_VIP.docx" menunjukkan Created: 2024-01-10
- Timestamps $FN pada "jadwal_VIP.docx" menunjukkan Created: 2025-11-20

**Pertanyaan:**

**1a.** Analisis perbedaan ukuran file (45 KB vs 12,5 MB). Apa yang menyebabkan perbedaan ini? Mekanisme apa yang digunakan untuk menyembunyikan data? (15 poin)

**1b.** Analisis timestamps file dan $UsnJrnl. Apakah terjadi timestomping? Jelaskan reasoning Anda dan rekonstruksi timeline sebenarnya! (15 poin)

**1c.** Mengingat laptop teknisi menggunakan SSD dengan TRIM aktif, strategi apa yang Anda gunakan untuk recovery bukti dari laptop? Jelaskan minimal 4 strategi alternatif! (15 poin)

**1d.** USB drive menggunakan FAT32. Jelaskan bagaimana Anda menganalisis USB drive untuk menemukan bukti tambahan, termasuk slack space, deleted files, dan FAT entries! (10 poin)

**1e.** Buatkan ringkasan temuan forensik yang menghubungkan semua bukti (server NTFS, laptop SSD, USB FAT32) dalam format tabel timeline! (10 poin)

---

### Studi Kasus 2: Sabotase Sistem C4ISR oleh Advanced Persistent Threat

**Latar Belakang:**

Sistem C4ISR (Command, Control, Communications, Computers, Intelligence, Surveillance, and Reconnaissance) di Mabesad mengalami anomali. Investigasi forensik diminta setelah terdeteksi modifikasi tidak sah pada database target radar.

Temuan awal:
- Server utama menggunakan dual disk: HDD 2 TB (MBR, NTFS) dan SSD 1 TB (GPT, NTFS)
- Disk HDD menyimpan database operasional (partisi D:)
- Disk SSD menyimpan OS Windows Server 2022 dan aplikasi (partisi C:)
- Pada HDD, ditemukan unallocated space 200 GB yang mencurigakan antara partisi D: dan akhir disk
- Analisis MFT pada HDD menunjukkan 47 file dengan flag "not in use" yang memiliki nama terkait operasi militer
- $LogFile pada SSD menunjukkan operasi MFT yang mencurigakan pada pukul 03:00-04:00 selama 5 hari berturut-turut
- Tool analisis menemukan executable tersembunyi di ADS file "readme.txt" pada SSD

**Pertanyaan:**

**2a.** Server menggunakan dua disk dengan skema partisi berbeda (MBR pada HDD, GPT pada SSD). Jelaskan implikasi forensik dari masing-masing skema partisi dan bagaimana Anda akan menganalisis keduanya! (15 poin)

**2b.** Ditemukan 200 GB unallocated space pada HDD. Jelaskan jenis-jenis unallocated space yang mungkin dan bagaimana melakukan file carving pada area tersebut! (15 poin)

**2c.** Analisis 47 file "not in use" pada MFT. Jelaskan proses recovery file yang dihapus dari NTFS, termasuk kondisi apa saja yang menentukan apakah file masih bisa dipulihkan! (15 poin)

**2d.** Executable ditemukan dalam ADS file "readme.txt". Jelaskan bagaimana malware menggunakan ADS untuk persistence dan bagaimana Anda mengekstrak serta menganalisis executable tersebut! (10 poin)

**2e.** Susun matriks keputusan untuk menentukan pendekatan imaging yang tepat (physical vs logical) untuk setiap disk, dengan mempertimbangkan jenis disk (HDD/SSD), skema partisi, dan tujuan forensik! (10 poin)

---
## Kunci Jawaban

### Bagian A: Pilihan Ganda

| No | Jawaban | No | Jawaban |
|----|---------|-----|---------|
| 1  | C       | 11  | B       |
| 2  | B       | 12  | D       |
| 3  | C       | 13  | B       |
| 4  | D       | 14  | C       |
| 5  | B       | 15  | C       |
| 6  | B       | 16  | C       |
| 7  | B       | 17  | C       |
| 8  | C       | 18  | C       |
| 9  | C       | 19  | C       |
| 10 | E       | 20  | C       |

---

### Bagian B: Uraian

#### Jawaban Soal 1 ⭐

**Perbandingan HDD dan SSD:**

| Aspek | HDD | SSD |
|-------|-----|-----|
| **Media penyimpanan** | Piringan magnetik berputar | Chip flash NAND (elektronik) |
| **Komponen bergerak** | Ya (platter, actuator, spindle) | Tidak ada |
| **Recovery data** | Relatif mudah — data tetap di platter meski dihapus | Sangat sulit — TRIM menghapus data secara fisik |
| **TRIM** | Tidak ada | Ada — menghancurkan bukti digital |
| **Wear leveling** | Tidak ada | Ada — data tersebar di lokasi berbeda |
| **Kecepatan** | Lebih lambat (mekanik) | Lebih cepat (elektronik) |
| **Ketahanan fisik** | Rentan guncangan/jatuh | Lebih tahan guncangan |

**Implikasi forensik:** HDD lebih "forensic-friendly" karena data yang dihapus masih ada di platter sampai ditimpa. SSD secara aktif menghapus data melalui TRIM dan garbage collection, membuat recovery jauh lebih sulit.

---

#### Jawaban Soal 2 ⭐

**Struktur MBR (512 bytes):**

| Offset | Ukuran | Komponen | Fungsi |
|--------|--------|----------|--------|
| 0x000 | 446 bytes | **Bootstrap Code** | Kode program yang dieksekusi BIOS saat booting |
| 0x1BE | 64 bytes | **Partition Table** (4 × 16 bytes) | Empat entri partisi, masing-masing 16 bytes mendefinisikan lokasi, tipe, dan ukuran partisi |
| 0x1FE | 2 bytes | **Boot Signature** (0x55AA) | Penanda bahwa sektor ini adalah MBR yang valid |

**Detail partition entry (16 bytes):**
- Byte 0: Status flag (0x80 = bootable, 0x00 = non-bootable)
- Byte 1-3: CHS address of first sector
- Byte 4: Partition type (0x07 = NTFS, 0x0B = FAT32, 0x83 = Linux)
- Byte 5-7: CHS address of last sector
- Byte 8-11: LBA of first sector
- Byte 12-15: Number of sectors

---

#### Jawaban Soal 3 ⭐

**Keunggulan GPT dibandingkan MBR:**

1. **Kapasitas:** GPT mendukung hingga 9,4 ZB vs MBR hanya 2 TB
2. **Jumlah partisi:** GPT mendukung hingga 128 partisi vs MBR hanya 4 primer
3. **Redundansi:** GPT menyimpan backup header dan entries di akhir disk — jika header utama rusak, dapat dipulihkan dari backup
4. **Integritas:** GPT menggunakan CRC32 checksum untuk mendeteksi kerusakan data
5. **Identifikasi unik:** Setiap partisi memiliki GUID unik yang memudahkan identifikasi forensik
6. **Protective MBR:** Menyediakan kompatibilitas mundur dengan sistem lama

**Mengapa lebih reliable untuk forensik:** Redundansi backup GPT memungkinkan recovery tabel partisi jika terjadi kerusakan atau sabotase pada header utama, dan CRC32 membantu memverifikasi integritas data partisi.

---

#### Jawaban Soal 4 ⭐

**Mekanisme FAT sebagai Linked List:**

1. **Directory entry** menyimpan nama file dan nomor cluster pertama
2. **Entri FAT** pada cluster tersebut menunjuk ke cluster berikutnya
3. Proses berulang hingga ditemukan **EOF marker** (0x0FFFFFF8–0x0FFFFFFF pada FAT32)
4. Seluruh rantai cluster membentuk file utuh

**Batas 4 GB pada FAT32:**

FAT32 menyimpan ukuran file dalam directory entry menggunakan field 4 bytes (32 bit). Nilai maksimal 32-bit unsigned integer = 2³² - 1 = 4.294.967.295 bytes ≈ 4 GB. Ini menjadi kendala karena image forensik dan file video sering melebihi 4 GB, sehingga exFAT atau NTFS lebih cocok untuk keperluan forensik.

---

#### Jawaban Soal 5 ⭐⭐

**Struktur MFT Entry:**

**Header MFT Entry:**
- **Signature** (offset 0x00): "FILE" (0x454C4946) — menandakan MFT entry yang valid
- **$LogFile Sequence Number** (offset 0x08): Nomor urut transaksi untuk recovery
- **Flags** (offset 0x16): Status file
  - 0x00 = Deleted/not in use
  - 0x01 = File in use
  - 0x02 = Directory
  - 0x03 = Directory in use

**Atribut Utama:**

| Atribut | Type Code | Fungsi |
|---------|-----------|--------|
| **$STANDARD_INFORMATION** | 0x10 | Timestamps MACE, file flags, security ID |
| **$FILE_NAME** | 0x30 | Nama file (Win32 dan DOS 8.3), timestamps alternatif, parent directory reference |
| **$DATA** | 0x80 | Konten aktual file — bisa resident (dalam MFT entry) atau non-resident (data runs ke cluster) |
| **$SECURITY_DESCRIPTOR** | 0x50 | Access Control List (ACL) dan permission |
| **$INDEX_ROOT** | 0x90 | Struktur B-tree untuk direktori |

Setiap atribut terdiri dari header (type, length, resident/non-resident flag) dan konten.

---

#### Jawaban Soal 6 ⭐⭐

**MACE Timestamps pada NTFS:**

MACE = Modified, Accessed, Created, Entry Modified — empat timestamps yang dicatat untuk setiap file.

**Perbedaan $STANDARD_INFORMATION vs $FILE_NAME:**

| Aspek | $STANDARD_INFORMATION (0x10) | $FILE_NAME (0x30) |
|-------|------------------------------|-------------------|
| Siapa yang dapat mengubah | Program user-mode (API SetFileTime) | Hanya kernel Windows |
| Update otomatis | Ya, setiap operasi file | Hanya saat file dibuat/dipindah/direname |
| Dapat dimanipulasi | **Ya** — rentan timestomping | **Tidak** oleh user biasa |
| Jumlah timestamps | 4 (M, A, C, E) | 4 (M, A, C, E) |

**Pentingnya untuk forensik:**
- Perbedaan antara $SI dan $FN timestamps mengungkap **timestomping**
- Jika $SI Created lebih lama dari $FN Created → secara logis tidak mungkin natural, karena $FN diset saat file benar-benar dibuat
- $FN timestamps adalah "ground truth" yang lebih reliable sebagai bukti

---

#### Jawaban Soal 7 ⭐⭐

**Deteksi Timestomping:**

**Langkah-langkah:**

1. **Ekstrak MFT** menggunakan tool seperti MFTExplorer atau analyzeMFT
2. **Bandingkan** timestamps $STANDARD_INFORMATION dan $FILE_NAME untuk setiap file
3. **Identifikasi anomali** berdasarkan aturan logis:
   - $FN Created seharusnya ≤ $SI Created (atau sama)
   - $SI Entry Modified seharusnya ≥ $SI Created
4. **Periksa $UsnJrnl** untuk melihat kapan file benar-benar dibuat/diubah
5. **Cross-reference** dengan $LogFile untuk operasi MFT

**Contoh anomali timestomping:**

| Atribut | Created | Modified |
|---------|---------|----------|
| $STANDARD_INFO | **2020-01-15** | 2020-02-10 |
| $FILE_NAME | **2025-12-01** | 2025-12-01 |

**Anomali:** $SI Created (2020) lebih lama dari $FN Created (2025). Ini mustahil secara natural karena $FN Created diset oleh kernel saat file dibuat. Artinya tersangka menggunakan tool seperti Timestomp untuk memundurkan $SI Created agar file terlihat lebih lama dari yang sebenarnya.

---

#### Jawaban Soal 8 ⭐⭐

**$LogFile vs $UsnJrnl:**

| Aspek | $LogFile | $UsnJrnl |
|-------|----------|----------|
| **Tujuan** | Recovery konsistensi setelah crash | Tracking perubahan file untuk aplikasi |
| **Level** | File system level rendah | File level tinggi |
| **Data** | Operasi metadata MFT (before/after values) | Nama file, reason code, timestamp |
| **Ukuran** | Relatif kecil (~64 MB), sering di-recycle | Besar, menyimpan histori lebih lama |
| **Forensik** | Rekonstruksi operasi teknis yang terjadi | Timeline aktivitas user (create, delete, rename) |

**Saling melengkapi:**
- **$LogFile** menjawab "bagaimana" — operasi metadata MFT apa yang terjadi secara teknis
- **$UsnJrnl** menjawab "siapa, kapan, apa" — file apa yang dibuat/dihapus/direname dan kapan
- Kombinasi keduanya memberikan gambaran lengkap tentang aktivitas file system

**Contoh:** Jika $UsnJrnl menunjukkan file "rahasia.pdf" dihapus pada 14:30, $LogFile dapat menunjukkan operasi MFT spesifik yang mengubah flag dari 0x01 ke 0x00.

---

#### Jawaban Soal 9 ⭐⭐

**Alternate Data Streams (ADS):**

ADS adalah fitur NTFS yang memungkinkan file memiliki beberapa stream data. Stream default (:$DATA) menyimpan konten file, tetapi stream tambahan (file.txt:hidden) bisa menyimpan data yang tidak terlihat melalui Windows Explorer atau perintah `dir` standar.

**Penggunaan Legitimate:**

| Stream | Fungsi |
|--------|--------|
| Zone.Identifier | Menyimpan informasi sumber download (internet, intranet) |
| Thumbnail cache | Menyimpan preview gambar |
| Summary information | Metadata dokumen (author, title) |

**Penggunaan Malicious:**
- **Menyembunyikan malware:** Payload diletakkan dalam ADS file biasa
- **Exfiltration:** Data rahasia disimpan dalam ADS file dokumen normal
- **C2 configuration:** Konfigurasi command & control dalam ADS

**Cara deteksi:**
1. `dir /R` — menampilkan ADS pada Windows
2. `streams.exe` (Sysinternals) — mendeteksi dan menampilkan ADS
3. FTK Imager / Autopsy — menampilkan semua stream dalam disk image
4. PowerShell: `Get-Item -Path file.txt -Stream *`

---

#### Jawaban Soal 10 ⭐⭐

**Perhitungan Slack Space:**

**File:** 10.500 bytes | **Cluster size:** 4.096 bytes | **Sector size:** 512 bytes

**Langkah 1: Hitung cluster yang dialokasikan**
- Jumlah cluster = ⌈10.500 / 4.096⌉ = ⌈2,56⌉ = **3 cluster**
- Ruang teralokasi = 3 × 4.096 = **12.288 bytes**

**Langkah 2: Hitung total slack space**
- Total slack = 12.288 − 10.500 = **1.788 bytes**

**Langkah 3: Rincian RAM slack dan drive slack**
- File menggunakan 10.500 bytes
- Sektor terakhir yang digunakan: 10.500 / 512 = 20,51 → sektor ke-21
- Data pada sektor terakhir: 10.500 − (20 × 512) = 10.500 − 10.240 = **260 bytes**
- **RAM slack** = 512 − 260 = **252 bytes** (sisa sektor terakhir, biasanya diisi nol pada Windows modern)
- **Drive slack** = 1.788 − 252 = **1.536 bytes** (3 sektor penuh antara akhir sektor terakhir dan akhir cluster ke-3)

**Signifikansi forensik:** Drive slack (1.536 bytes) dapat berisi data residual dari file sebelumnya yang menempati cluster tersebut.

---

#### Jawaban Soal 11 ⭐⭐

**Perbandingan MFT Entry vs Inode:**

| Aspek | MFT Entry (NTFS) | Inode (ext4) |
|-------|-------------------|--------------|
| **Ukuran** | 1024 bytes (biasanya) | 256 bytes |
| **Timestamps** | 8 total: 4 di $SI + 4 di $FN | 4: atime, mtime, ctime, dtime |
| **Presisi waktu** | 100 nanosecond intervals | Nanosecond (ext4), detik (ext2/3) |
| **Deletion time** | **Tidak ada** — hanya flag change | **Ada (dtime)** — waktu penghapusan |
| **ADS** | Didukung (multiple $DATA streams) | Tidak didukung |
| **Timestomping detection** | **Mudah:** bandingkan $SI vs $FN | **Sulit:** tidak ada dual timestamps |
| **Resident data** | File kecil disimpan dalam MFT entry | Tidak ada (selalu di data blocks) |
| **Block pointers** | Data runs (offset + length pairs) | Direct, indirect, double indirect, extent tree |

**Kesimpulan:** NTFS lebih kaya metadata (8 timestamps, ADS) sehingga lebih baik untuk deteksi anti-forensik. ext4 memiliki keunggulan unik berupa dtime yang secara eksplisit mencatat waktu penghapusan.

---

#### Jawaban Soal 12 ⭐⭐⭐

**Analisis Timeline Cross-Platform:**

**Langkah 1: Identifikasi anomali timestamps NTFS**
- $SI Created: 2024-03-15 → **lebih lama** dari $FN Created: 2025-12-01
- Ini **mustahil secara natural** — $FN Created diset kernel saat file dibuat
- **Kesimpulan:** Timestomping terdeteksi pada $STANDARD_INFORMATION

**Langkah 2: Analisis file ext4**
- mtime dan ctime: 2025-12-01 14:25:00
- Hash identik dengan file NTFS → **file yang sama**
- dtime = 0 → file masih aktif

**Langkah 3: Rekonstruksi timeline sebenarnya**

| Waktu | Event | Bukti |
|-------|-------|-------|
| 2025-12-01 ~14:25 | File dibuat/disalin di partisi ext4 | mtime/ctime ext4 |
| 2025-12-01 ~14:30 | File disalin ke partisi NTFS | $FN Created |
| Setelah itu | Timestomping dilakukan pada $SI | $SI Created dimundurkan ke 2024-03-15 |
| Setelah itu | $SI Modified diset ke 2024-06-20 | Upaya membuat file terlihat "lama" |

**Kesimpulan:**
1. File sebenarnya dibuat pada **1 Desember 2025**, bukan Maret 2024
2. Tersangka melakukan **timestomping** pada partisi NTFS untuk memalsukan tanggal pembuatan
3. Tersangka **tidak mengetahui** bahwa $FILE_NAME timestamps tidak terpengaruh
4. File ext4 menjadi **bukti koraborasi** yang mengkonfirmasi timeline sebenarnya
5. Tool yang kemungkinan digunakan: Timestomp (Metasploit) atau NirCmd

**Rekomendasi:** Periksa $UsnJrnl dan $LogFile untuk konfirmasi tambahan, serta analisis MFT entry pada partisi NTFS untuk melihat sequence number.

---

#### Jawaban Soal 13 ⭐⭐⭐

**Recovery Data SSD vs HDD:**

**1. TRIM**
- **Mekanisme:** OS memberitahu SSD bahwa blok tidak digunakan → SSD menghapus data secara **fisik** dari flash NAND
- **Dampak forensik:** Data yang di-TRIM tidak dapat dipulihkan karena sudah dihapus pada level hardware
- **Perbedaan dengan HDD:** Pada HDD, cluster yang dirilis hanya ditandai "free" di file system — data masih ada di platter

**2. Wear Leveling**
- **Mekanisme:** Controller SSD mendistribusikan penulisan merata ke semua sel NAND untuk memperpanjang umur
- **Dampak forensik:** Data fisik tidak berada di lokasi yang diharapkan file system; satu file bisa tersebar di lokasi berbeda tanpa pola yang jelas; mapping LBA-to-PBA berubah dinamis
- **Perbedaan dengan HDD:** HDD menyimpan data di lokasi yang ditunjuk file system

**3. Garbage Collection**
- **Mekanisme:** Proses internal SSD yang menggabungkan halaman valid dan menghapus halaman invalid secara otomatis
- **Dampak forensik:** Berjalan **tanpa perintah user**, bahkan saat komputer idle; data dapat dihapus sebelum investigator sempat mengakuisisi
- **Perbedaan dengan HDD:** HDD tidak memiliki proses otomatis yang menghapus data

**Strategi Alternatif:**

| Strategi | Deskripsi |
|----------|-----------|
| **RAM acquisition** | Capture RAM sebelum shutdown — file yang sedang dibuka ada di memory |
| **MFT metadata** | $UsnJrnl dan MFT entry tetap mencatat eksistensi file meski data sudah di-TRIM |
| **Cloud/backup** | Data mungkin tersinkronisasi ke cloud atau backup |
| **Over-provisioned area** | ~7-28% kapasitas SSD tidak dapat diakses OS; mungkin berisi data residual |
| **Chip-off forensics** | Membaca langsung chip NAND (mahal, destruktif) |
| **Non-TRIM scenarios** | Jika TRIM dinonaktifkan, recovery seperti HDD |

---

#### Jawaban Soal 14 ⭐⭐⭐

**Recovery GPT dari Backup:**

**a. Langkah recovery:**

1. **Verifikasi kerusakan** — baca LBA 1 dan periksa signature "EFI PART" serta CRC32
2. **Lokasi backup** — baca LBA terakhir disk (backup GPT header)
3. **Validasi backup** — periksa CRC32 backup header dan entries
4. **Baca partition entries** dari backup (32 LBA sebelum LBA terakhir)
5. **Rekonstruksi** tabel partisi dari informasi backup
6. **Opsional:** Restore header utama dari backup jika diperlukan

**b. Lokasi backup GPT:**
- **Backup GPT Header:** LBA terakhir disk (last LBA)
- **Backup Partition Entries:** 32 LBA sebelum backup header (misal LBA -34 sampai LBA -2)
- Header utama di LBA 1, entries utama di LBA 2-33

**c. Informasi dari GPT Header:**

| Field | Deskripsi |
|-------|-----------|
| Signature | "EFI PART" (identifikasi) |
| Revision | Versi GPT |
| Header size | Ukuran header |
| CRC32 of header | Checksum integritas header |
| My LBA | Lokasi header ini |
| Alternate LBA | Lokasi backup header |
| First usable LBA | LBA pertama untuk data |
| Last usable LBA | LBA terakhir untuk data |
| Disk GUID | Identifier unik disk |
| Partition entry start LBA | Lokasi awal partition entries |
| Number of partition entries | Jumlah entri (biasanya 128) |
| CRC32 of partition entries | Checksum integritas entries |

**d. CRC32 membantu verifikasi:**
- GPT Header menyimpan dua CRC32: satu untuk header sendiri, satu untuk partition entries
- Jika CRC32 tidak cocok → data corrupt/dimanipulasi
- Investigator dapat membandingkan CRC32 primary vs backup untuk mendeteksi tampering
- Jika keduanya berbeda, yang mana yang benar perlu diverifikasi dengan bukti lain

---

#### Jawaban Soal 15 ⭐⭐⭐

**Rekonstruksi NTFS Data Runs:**

**a. Lokasi fisik setiap data run:**

Data runs menggunakan offset relatif dari data run sebelumnya:

| Data Run | Offset (relatif) | Cluster Start | Length |
|----------|-------------------|---------------|--------|
| 1 | 100 (absolut) | **100** | 5 cluster |
| 2 | +200 (dari 100) | 100 + 200 = **300** | 3 cluster |
| 3 | -50 (dari 300) | 300 − 50 = **250** | 2 cluster |

**b. Total ukuran file yang dialokasikan:**
- Total cluster = 5 + 3 + 2 = **10 cluster**
- Total bytes = 10 × 4.096 = **40.960 bytes** (40 KB)

**c. Arti tiga data runs:**
- File **terfragmentasi** menjadi tiga bagian terpisah pada disk
- Cluster 100-104, 300-302, dan 250-251 menyimpan bagian berbeda dari file
- Fragmentasi menunjukkan file ditulis saat disk sudah terisi parsial, atau file mengalami modifikasi ukuran
- Fragmentasi **tidak mempengaruhi integritas** file — NTFS merekonstruksi file dari data runs

**d. Cara merekonstruksi file lengkap:**

1. **Parse MFT entry** — ekstrak atribut $DATA dan decode data runs
2. **Baca cluster secara berurutan:**
   - Baca cluster 100, 101, 102, 103, 104 (data run 1: 5 × 4.096 = 20.480 bytes)
   - Baca cluster 300, 301, 302 (data run 2: 3 × 4.096 = 12.288 bytes)
   - Baca cluster 250, 251 (data run 3: 2 × 4.096 = 8.192 bytes)
3. **Gabungkan** semua cluster dalam urutan data run → file utuh
4. **Verifikasi** ukuran aktual dari $FILE_NAME atau $STANDARD_INFORMATION (file mungkin tidak menggunakan seluruh cluster terakhir)
5. **Hash** file hasil rekonstruksi untuk verifikasi

**Tool yang digunakan:** FTK Imager dapat melakukan ini secara otomatis; untuk analisis manual, gunakan HxD atau The Sleuth Kit (icat, istat).


### Bagian C: Studi Kasus


#### Studi Kasus 1: Investigasi Kebocoran Dokumen di Lanud

**1a. Analisis Perbedaan Ukuran (15 poin)**

**Penyebab:** File menggunakan **Alternate Data Streams (ADS)**

- Ukuran Explorer (45 KB) menunjukkan hanya **default $DATA stream**
- Ukuran FTK Imager (12,5 MB) menunjukkan **total semua streams** termasuk ADS
- Selisih: 12,5 MB − 45 KB ≈ **12,45 MB data tersembunyi** dalam ADS

**Mekanisme:**
1. Teknisi membuat file "jadwal_VIP.docx" yang terlihat normal (45 KB)
2. Dokumen rahasia (12,45 MB) disembunyikan dalam ADS: `jadwal_VIP.docx:hidden_data`
3. ADS tidak terlihat di Explorer, hanya FTK Imager dan tool forensik yang menampilkannya

**Verifikasi:**
- `dir /R jadwal_VIP.docx` — akan menampilkan streams tambahan
- `streams.exe jadwal_VIP.docx` (Sysinternals) — daftar semua ADS
- Hash ADS stream dan bandingkan dengan dokumen rahasia

---

**1b. Analisis Timestamps dan Timeline (15 poin)**

**Ya, terjadi timestomping:**

| Atribut | Created |
|---------|---------|
| $STANDARD_INFORMATION | 2024-01-10 |
| $FILE_NAME | 2025-11-20 |

**Anomali:** $SI Created (Jan 2024) lebih lama dari $FN Created (Nov 2025) — mustahil secara natural.

**Timeline rekonstruksi:**

| Waktu | Event | Sumber Bukti |
|-------|-------|-------------|
| 20 Nov 2025 | File "RAHASIA_rute.docx" dibuat/disalin ke server | $FN Created |
| 20 Nov 2025 | File di-rename menjadi "jadwal_VIP.docx" | $UsnJrnl |
| 20 Nov 2025 | Dokumen rahasia disembunyikan dalam ADS | ADS analysis |
| Setelahnya | Timestomping: $SI dimundurkan ke 2024-01-10 | $SI vs $FN anomaly |

---

**1c. Strategi Recovery Bukti SSD (15 poin)**

| No | Strategi | Penjelasan |
|----|----------|-----------|
| 1 | **RAM acquisition** | Jika laptop masih menyala, capture RAM — file yang pernah dibuka ada di memory |
| 2 | **MFT analysis** | MFT entry file yang dihapus masih ada (flag 0x00) meski data sudah di-TRIM |
| 3 | **$UsnJrnl** | Journal masih mencatat nama file, timestamp, dan operasi meski file sudah dihapus |
| 4 | **Browser artifacts** | Email pribadi → browser cache, cookies, history mungkin ada di RAM atau MFT |
| 5 | **Over-provisioned area** | 7-28% SSD tidak bisa diakses OS — perlu chip-off forensics |
| 6 | **Cloud/sync** | Cek sinkronisasi OneDrive, Google Drive, atau email draft |
| 7 | **Hibernation file** | hiberfil.sys mungkin berisi snapshot memory dengan data file |

---

**1d. Analisis USB Drive FAT32 (10 poin)**

1. **Physical imaging** USB dengan FTK Imager (E01 format + hash SHA-256)
2. **FAT table analysis:** Periksa entri FAT untuk cluster chains aktif dan file "foto_liburan.zip"
3. **Deleted files recovery:** Entri direktori yang dihapus ditandai 0xE5 di byte pertama nama — data masih ada jika cluster belum ditimpa
4. **Slack space:** Setiap file menghasilkan slack space; pada FAT32 dengan cluster besar, slack bisa signifikan
5. **Unallocated space:** File carving pada cluster yang ditandai 0x00000000 di FAT
6. **File hash:** Verifikasi hash "foto_liburan.zip" = hash dokumen rahasia → konfirmasi file disamarkan

---

**1e. Timeline Terpadu (10 poin)**

| Waktu | Event | Lokasi | Bukti |
|-------|-------|--------|-------|
| ~20 Nov 2025 | File rahasia dibuat/disalin | Server NTFS | $FN Created |
| 20 Nov 2025 | File direname dari RAHASIA_rute ke jadwal_VIP | Server NTFS | $UsnJrnl |
| ~20 Nov 2025 | Dokumen disisipkan ke ADS | Server NTFS | ADS (12,45 MB) |
| ~20 Nov 2025 | Timestomping dilakukan | Server NTFS | $SI vs $FN anomaly |
| ~20 Nov 2025 | File disalin ke USB sebagai "foto_liburan.zip" | USB FAT32 | Hash match |
| ~20 Nov 2025 | File dikirim via email pribadi | Laptop SSD | $UsnJrnl, browser artifacts |

---

#### Studi Kasus 2: Sabotase Sistem C4ISR

**2a. Implikasi Forensik MBR vs GPT (15 poin)**

| Aspek | HDD (MBR) | SSD (GPT) |
|-------|-----------|-----------|
| **Partisi maks** | 4 primer | 128 |
| **Redundansi** | Tidak ada — MBR corrupt = partisi hilang | Backup GPT header/entries |
| **Recovery** | Harus rebuild dari scratch jika corrupt | Restore dari backup header |
| **Hidden partitions** | Periksa extended partitions | Periksa semua 128 entries |
| **Imaging** | Physical imaging standar | Physical imaging, perhatikan TRIM |

**Analisis:**
- **HDD MBR:** Parse MBR di LBA 0, identifikasi 4 partition entries, periksa tipe partisi, cari extended partitions, scan unallocated space
- **SSD GPT:** Baca GPT header (LBA 1), parse 128 partition entries, verifikasi CRC32, bandingkan primary dan backup headers, periksa apakah ada entries yang dimanipulasi

---

**2b. Analisis Unallocated Space 200 GB (15 poin)**

**Jenis yang mungkin:**
1. **Partition gap:** Ruang antara akhir partisi D: dan akhir disk yang tidak dipartisi
2. **Deleted partition:** Partisi yang dihapus oleh penyerang untuk menghilangkan bukti
3. **Hidden partition:** Area sengaja tidak dipartisi untuk menyimpan data tersembunyi

**File carving:**
1. **Scan signature:** Cari file headers (JPEG: FF D8 FF, PDF: %PDF, ZIP: PK) di unallocated space
2. **Tool:** Autopsy (built-in carver), Foremost, Scalpel, PhotoRec
3. **Proses:** Baca setiap sektor, cocokkan header signatures, ekstrak file sampai footer ditemukan
4. **Verifikasi:** Hash setiap file yang di-carve dan bandingkan dengan known files
5. **Perhatikan:** MBR hanya memiliki 4 entry — mungkin partisi kelima (extended) telah dihapus

---

**2c. Recovery 47 File Deleted (15 poin)**

**Proses recovery:**
1. **Parse MFT:** Identifikasi entry dengan flag 0x00 (not in use)
2. **Baca atribut:** $FILE_NAME (nama), $STANDARD_INFORMATION (timestamps), $DATA (data runs)
3. **Periksa data runs:** Tentukan cluster mana yang digunakan file
4. **Verifikasi cluster:** Periksa $Bitmap — apakah cluster sudah dialokasikan ulang

**Kondisi yang menentukan keberhasilan recovery:**

| Kondisi | Recovery | Alasan |
|---------|----------|--------|
| MFT entry intact, cluster belum dialokasikan ulang | **Penuh** | Metadata + data tersedia |
| MFT entry intact, cluster sebagian ditimpa | **Parsial** | Metadata ada, data fragmentary |
| MFT entry intact, semua cluster ditimpa | **Metadata saja** | Nama, ukuran, timestamps tersedia |
| MFT entry di-overwrite | **Tidak mungkin** | Perlu file carving tanpa metadata |

**Untuk 47 file militer:** Karena ini HDD (bukan SSD), kemungkinan recovery tinggi jika cluster belum ditimpa.

---

**2d. Analisis ADS Malware (10 poin)**

**Cara malware menggunakan ADS:**
1. **Persistence:** Malware disimpan dalam ADS file sistem yang jarang diperiksa (readme.txt)
2. **Execution:** Scheduled task atau registry run key menjalankan `readme.txt:malware.exe`
3. **Evasion:** ADS tidak terlihat di Explorer dan tidak terdeteksi banyak antivirus

**Langkah ekstraksi dan analisis:**
1. `dir /R readme.txt` — identifikasi nama ADS
2. `more < readme.txt:suspicious.exe` — verifikasi konten
3. Ekspor: `expand readme.txt:suspicious.exe extracted_malware.exe`
4. Hash extracted file → cek VirusTotal
5. Analisis statis: strings, PE header, imports
6. Analisis dinamis: jalankan di sandbox terisolasi
7. Korelasi: periksa scheduled tasks dan registry untuk references ke ADS

---

**2e. Matriks Keputusan Imaging (10 poin)**

| Disk | Tipe | Skema | Pendekatan | Alasan |
|------|------|-------|------------|--------|
| **HDD 2 TB** | HDD | MBR | **Physical imaging** | Tangkap seluruh disk termasuk unallocated 200 GB, slack space, deleted files — HDD tidak memiliki TRIM |
| **SSD 1 TB** | SSD | GPT | **Physical imaging** (prioritas tinggi, segera!) | TRIM dan GC dapat menghapus bukti kapan saja; imaging harus dilakukan ASAP; pertimbangkan juga logical imaging untuk metadata |

**Pertimbangan tambahan:**

| Faktor | HDD | SSD |
|--------|-----|-----|
| Urgensi waktu | Sedang (data stabil) | **Tinggi** (TRIM dapat berjalan) |
| Write blocker | Hardware write blocker | Wajib hardware write blocker (cegah TRIM) |
| Format image | E01 (kompresi + hash) | E01 atau raw (dd) |
| Verifikasi | SHA-256 | SHA-256 |
| Catatan | Analisis unallocated space krusial | Segera cabut power jika memungkinkan untuk mencegah GC |

---

## Rubrik Penilaian

### Pilihan Ganda
- Setiap soal benar: 2 poin
- Total: 40 poin

### Uraian
- Soal ⭐: maksimal 5 poin
- Soal ⭐⭐: maksimal 8 poin  
- Soal ⭐⭐⭐: maksimal 12 poin
- Total: 115 poin

### Studi Kasus
- Studi Kasus 1: 65 poin
- Studi Kasus 2: 65 poin
- Total: 130 poin

### Total Keseluruhan: 285 poin

---

## License

This repository is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

Commercial use is permitted, provided attribution is given to the author.

© 2026 Anindito
