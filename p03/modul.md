# Modul 03: Hard Disk dan Sistem File

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 03  
**Topik:** Hard Disk dan Sistem File  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Menjelaskan arsitektur dan geometri hard disk (HDD dan SSD) secara komprehensif
2. Membedakan skema partisi MBR dan GPT beserta implikasi forensiknya
3. Menganalisis struktur berbagai sistem file Windows (FAT12, FAT16, FAT32, exFAT, NTFS)
4. Mengidentifikasi dan menginterpretasi struktur internal NTFS (MFT, $LogFile, $UsnJrnl)
5. Memahami sistem file Linux (ext2, ext3, ext4) dan pengenalan sistem file Mac (HFS+, APFS)
6. Menjelaskan konsep cluster, slack space, dan unallocated space dalam konteks forensik
7. Menganalisis tantangan forensik pada SSD termasuk TRIM, wear leveling, dan garbage collection

---

## 1. Arsitektur dan Geometri Hard Disk

### 1.1 Hard Disk Drive (HDD)

> **Hard Disk Drive (HDD)** adalah perangkat penyimpanan non-volatile yang menggunakan piringan magnetik berputar (platter) untuk membaca dan menulis data secara elektromagnetik.

HDD telah menjadi media penyimpanan utama dalam sistem komputer selama beberapa dekade. Dalam konteks forensik militer, pemahaman mendalam tentang arsitektur HDD sangat penting karena mayoritas sistem legacy TNI dan infrastruktur pertahanan masih menggunakan media penyimpanan ini.

| Komponen | Fungsi | Relevansi Forensik |
|----------|--------|-------------------|
| **Platter** | Media magnetik penyimpanan data | Data dapat dipulihkan meskipun sudah dihapus |
| **Read/Write Head** | Membaca dan menulis data ke platter | Kerusakan head dapat menyebabkan data loss |
| **Spindle Motor** | Memutar platter pada kecepatan konstan | Kecepatan rotasi memengaruhi performa akuisisi |
| **Actuator Arm** | Memposisikan head di atas track yang tepat | Mekanisme presisi untuk akses data |
| **Controller Board** | Mengontrol operasi disk dan interface | Firmware dapat menyimpan informasi forensik |

#### 1.1.1 Geometri CHS dan LBA

Sistem pengalamatan data pada HDD telah berevolusi dari CHS ke LBA:

**CHS (Cylinder-Head-Sector):**
- **Cylinder**: Kumpulan track vertikal pada semua platter
- **Head**: Mengidentifikasi sisi platter (head 0 = sisi atas)
- **Sector**: Subdivisi terkecil pada track (umumnya 512 bytes)

**LBA (Logical Block Addressing):**
- Sistem pengalamatan linier yang lebih sederhana
- Setiap sektor diberi nomor urut mulai dari 0
- Menggantikan CHS pada disk modern
- Mendukung kapasitas disk yang lebih besar

![Geometri Hard Disk](images/p03-01-hdd-geometry.png)

*Gambar 3.1: Arsitektur dan geometri hard disk drive*

#### Solved Problem 1 ⭐

**Soal:** Sebuah hard disk memiliki konfigurasi CHS berikut: 16.383 cylinder, 16 head, dan 63 sector per track. Jika ukuran setiap sektor adalah 512 bytes, berapa kapasitas total hard disk tersebut?

**Penyelesaian:**

*Step 1:* Hitung jumlah total sektor

Total sektor = Cylinder × Head × Sector per track
Total sektor = 16.383 × 16 × 63 = 16.514.064 sektor

*Step 2:* Hitung kapasitas total

Kapasitas = Total sektor × Ukuran sektor
Kapasitas = 16.514.064 × 512 bytes = 8.455.200.768 bytes

*Step 3:* Konversi ke satuan yang lebih besar

Kapasitas = 8.455.200.768 / 1.073.741.824 ≈ **7,87 GB**

**Jawaban:** Kapasitas total hard disk adalah sekitar **7,87 GB** (8,46 miliar bytes). Perlu dicatat bahwa batas CHS tradisional adalah 8,4 GB, yang menjadi alasan utama migrasi ke sistem pengalamatan LBA.

---

### 1.2 Solid State Drive (SSD)

> **Solid State Drive (SSD)** adalah perangkat penyimpanan non-volatile yang menggunakan chip memori flash NAND untuk menyimpan data secara elektronik tanpa komponen mekanik bergerak.

SSD semakin banyak digunakan dalam sistem informasi militer karena ketahanannya terhadap guncangan dan getaran di lapangan. Namun, SSD menghadirkan tantangan forensik yang signifikan.

| Aspek | HDD | SSD |
|-------|-----|-----|
| **Media** | Piringan magnetik | Chip flash NAND |
| **Komponen bergerak** | Ya (platter, head) | Tidak ada |
| **Kecepatan** | Lebih lambat (5400-15000 RPM) | Lebih cepat (IOPS tinggi) |
| **Ketahanan fisik** | Rentan guncangan | Tahan guncangan |
| **Recovery data** | Relatif mudah | Sangat sulit |
| **TRIM support** | Tidak | Ya |
| **Wear leveling** | Tidak | Ya |

#### 1.2.1 Arsitektur Internal SSD

Komponen utama SSD:

1. **NAND Flash Chips**: Menyimpan data dalam sel memori (SLC, MLC, TLC, QLC)
2. **Controller**: Mengelola operasi baca/tulis, wear leveling, dan garbage collection
3. **DRAM Cache**: Buffer untuk operasi baca/tulis (tidak ada pada semua SSD)
4. **Interface**: SATA, NVMe (PCIe), atau M.2

#### 1.2.2 Tipe Sel NAND Flash

| Tipe | Bit per Sel | Daya Tahan | Kecepatan | Penggunaan |
|------|------------|------------|-----------|------------|
| **SLC** | 1 | Sangat tinggi (~100K P/E) | Tercepat | Enterprise, militer |
| **MLC** | 2 | Tinggi (~10K P/E) | Cepat | Enterprise |
| **TLC** | 3 | Sedang (~3K P/E) | Sedang | Consumer |
| **QLC** | 4 | Rendah (~1K P/E) | Paling lambat | Storage massal |

#### Solved Problem 2 ⭐

**Soal:** Jelaskan mengapa SSD tipe SLC lebih banyak digunakan dalam sistem informasi militer dibandingkan TLC atau QLC!

**Penyelesaian:**

*Step 1:* Identifikasi kebutuhan lingkungan militer

Sistem militer membutuhkan: keandalan tinggi, daya tahan, kecepatan, dan umur pakai panjang.

*Step 2:* Bandingkan karakteristik SLC dengan TLC/QLC

| Kriteria Militer | SLC | TLC/QLC |
|-----------------|-----|---------|
| **Keandalan** | Sangat tinggi (1 bit per sel, error rate rendah) | Lebih rendah (multi-bit, error rate tinggi) |
| **Daya tahan** | ~100.000 siklus P/E | ~1.000-3.000 siklus P/E |
| **Konsistensi performa** | Stabil | Menurun seiring penggunaan |
| **Rentang suhu operasi** | Lebih luas (-40°C hingga 85°C) | Lebih terbatas |

**Jawaban:** SSD SLC lebih cocok untuk militer karena: (1) Keandalan sangat tinggi dengan error rate rendah, (2) Daya tahan 30-100x lipat dibanding TLC/QLC, (3) Performa konsisten pada kondisi ekstrem, (4) Rentang suhu operasi lebih luas yang sesuai dengan lingkungan lapangan militer.

---

## 2. Skema Partisi: MBR dan GPT

### 2.1 Master Boot Record (MBR)

> **Master Boot Record (MBR)** adalah struktur data berukuran 512 bytes yang terletak di sektor pertama (LBA 0) dari media penyimpanan, berisi boot code dan tabel partisi.

MBR dikembangkan oleh IBM pada tahun 1983 dan masih banyak ditemukan pada sistem legacy, termasuk banyak sistem informasi pertahanan yang lebih tua.

#### 2.1.1 Struktur MBR (512 bytes)

| Offset (hex) | Ukuran | Deskripsi |
|-------------|--------|-----------|
| 0x000–0x1BD | 446 bytes | Bootstrap code (boot loader) |
| 0x1BE–0x1CD | 16 bytes | Partition entry 1 |
| 0x1CE–0x1DD | 16 bytes | Partition entry 2 |
| 0x1DE–0x1ED | 16 bytes | Partition entry 3 |
| 0x1EE–0x1FD | 16 bytes | Partition entry 4 |
| 0x1FE–0x1FF | 2 bytes | Boot signature (0x55AA) |

#### 2.1.2 Struktur Partition Entry (16 bytes)

| Offset | Ukuran | Deskripsi |
|--------|--------|-----------|
| 0x00 | 1 byte | Status (0x80 = bootable, 0x00 = non-bootable) |
| 0x01–0x03 | 3 bytes | CHS address of first sector |
| 0x04 | 1 byte | Partition type (0x07 = NTFS, 0x0B = FAT32) |
| 0x05–0x07 | 3 bytes | CHS address of last sector |
| 0x08–0x0B | 4 bytes | LBA of first sector |
| 0x0C–0x0F | 4 bytes | Number of sectors |

**Keterbatasan MBR:**
- Maksimal 4 partisi primer (atau 3 primer + 1 extended)
- Kapasitas maksimal 2 TB (2^32 × 512 bytes)
- Satu titik kegagalan (single point of failure)

![Struktur MBR](images/p03-02-mbr-structure.png)

*Gambar 3.2: Struktur Master Boot Record dan tabel partisi*

#### Solved Problem 3 ⭐

**Soal:** Seorang investigator forensik militer menemukan hard disk dengan MBR yang menunjukkan partition type 0x07 pada partition entry pertama. Sistem file apa yang digunakan dan apa implikasinya untuk investigasi?

**Penyelesaian:**

*Step 1:* Identifikasi partition type code

Partition type 0x07 menunjukkan sistem file **NTFS** (New Technology File System).

*Step 2:* Tentukan implikasi forensik

| Aspek | Implikasi |
|-------|-----------|
| **Sistem file** | NTFS - sistem file utama Windows |
| **Artefak tersedia** | MFT, $LogFile, $UsnJrnl, ADS |
| **Recovery** | Mendukung recovery file terhapus melalui MFT |
| **Metadata** | Kaya metadata termasuk timestamps MACE |
| **Journaling** | Memiliki journal yang menyimpan perubahan |

**Jawaban:** Partition type 0x07 menunjukkan sistem file NTFS. Implikasi forensik: investigator dapat mengekstrak metadata kaya dari MFT, memanfaatkan journal ($LogFile, $UsnJrnl) untuk timeline analysis, dan melakukan recovery file terhapus. NTFS juga mendukung Alternate Data Streams yang sering digunakan untuk menyembunyikan data.

---

### 2.2 GUID Partition Table (GPT)

> **GUID Partition Table (GPT)** adalah standar partisi modern yang merupakan bagian dari spesifikasi UEFI, menggunakan Globally Unique Identifiers untuk mengidentifikasi partisi.

GPT merupakan pengganti MBR yang mengatasi berbagai keterbatasan teknis. Sistem pertahanan modern semakin banyak yang bermigrasi ke GPT.

#### 2.2.1 Struktur GPT

| Komponen | Lokasi | Ukuran | Deskripsi |
|----------|--------|--------|-----------|
| **Protective MBR** | LBA 0 | 1 sektor | Kompatibilitas mundur dengan MBR |
| **GPT Header** | LBA 1 | 1 sektor | Metadata tabel partisi |
| **Partition Entries** | LBA 2-33 | 32 sektor | Hingga 128 entri partisi |
| **Data Partitions** | LBA 34+ | Variabel | Area data partisi |
| **Backup Entries** | LBA -33 s/d -2 | 32 sektor | Salinan cadangan entri partisi |
| **Backup GPT Header** | LBA terakhir | 1 sektor | Salinan cadangan header |

#### 2.2.2 Perbandingan MBR vs GPT

| Aspek | MBR | GPT |
|-------|-----|-----|
| **Kapasitas maks** | 2 TB | 9,4 ZB (teoritis) |
| **Partisi maks** | 4 primer | 128 (default) |
| **Redundansi** | Tidak ada | Header dan entries cadangan |
| **Identifikasi** | Byte partition type | GUID unik |
| **Boot** | BIOS | UEFI |
| **CRC check** | Tidak | Ya (CRC32) |

#### Solved Problem 4 ⭐

**Soal:** Mengapa GPT lebih handal dibandingkan MBR dari perspektif forensik?

**Penyelesaian:**

*Step 1:* Identifikasi fitur kehandalan GPT

*Step 2:* Analisis implikasi forensik

| Fitur GPT | Kehandalan | Implikasi Forensik |
|-----------|------------|-------------------|
| **Redundansi** | Backup header dan entries | Data partisi dapat dipulihkan dari backup |
| **CRC32** | Deteksi korupsi data | Verifikasi integritas struktur partisi |
| **GUID** | Identifikasi unik | Pelacakan partisi lebih presisi |
| **Protective MBR** | Kompatibilitas mundur | Tools MBR tidak akan merusak GPT |

**Jawaban:** GPT lebih handal karena: (1) Menyimpan salinan cadangan di akhir disk sehingga tabel partisi yang rusak dapat dipulihkan, (2) Menggunakan CRC32 untuk deteksi korupsi, (3) GUID unik memungkinkan pelacakan partisi yang lebih akurat, (4) Protective MBR mencegah tools lama merusak struktur GPT secara tidak sengaja.

---

## 3. Sistem File Windows

### 3.1 Keluarga FAT (File Allocation Table)

> **File Allocation Table (FAT)** adalah keluarga sistem file yang dikembangkan oleh Microsoft, menggunakan tabel alokasi untuk melacak lokasi file pada media penyimpanan.

FAT merupakan sistem file yang paling sederhana dan masih banyak digunakan pada media removable seperti USB flash drive yang sering menjadi barang bukti dalam investigasi militer.

#### 3.1.1 Evolusi Sistem File FAT

| Sistem File | Tahun | Ukuran Entri FAT | Kapasitas Maks | Ukuran File Maks |
|-------------|-------|-----------------|----------------|-----------------|
| **FAT12** | 1977 | 12 bit | 32 MB | 32 MB |
| **FAT16** | 1987 | 16 bit | 2 GB | 2 GB |
| **FAT32** | 1996 | 32 bit | 2 TB | 4 GB |
| **exFAT** | 2006 | 32 bit | 128 PB | 16 EB |

#### 3.1.2 Struktur Volume FAT

Setiap volume FAT terdiri dari tiga area utama:

1. **Reserved Area**: Boot sector, BIOS Parameter Block (BPB), dan informasi sistem file
2. **FAT Area**: Satu atau dua salinan File Allocation Table
3. **Data Area**: Area penyimpanan file dan direktori

#### 3.1.3 Cara Kerja FAT

FAT bekerja seperti daftar berantai (*linked list*):

- Setiap file memiliki entri direktori yang menunjuk ke cluster pertama
- Setiap entri FAT menunjuk ke cluster berikutnya dalam rantai
- Nilai khusus menandai akhir rantai (EOF), cluster kosong, atau cluster rusak

| Nilai FAT32 | Arti |
|-------------|------|
| 0x00000000 | Cluster kosong (free) |
| 0x00000002–0x0FFFFFEF | Cluster data berikutnya |
| 0x0FFFFFF7 | Bad cluster |
| 0x0FFFFFF8–0x0FFFFFFF | End of cluster chain (EOF) |

![Sistem File FAT](images/p03-03-fat-structure.png)

*Gambar 3.3: Struktur dan mekanisme File Allocation Table*

#### Solved Problem 5 ⭐

**Soal:** Sebuah USB flash drive yang disita dari tersangka kebocoran data di Kodam menggunakan FAT32. File rahasia berukuran 5 GB ditemukan dalam daftar file yang pernah ada. Mengapa file tersebut tidak dapat disimpan utuh pada media ini?

**Penyelesaian:**

*Step 1:* Identifikasi batasan FAT32

FAT32 memiliki batas ukuran file maksimal **4 GB** (4.294.967.295 bytes atau 4 GB minus 1 byte).

*Step 2:* Bandingkan dengan ukuran file

File rahasia: 5 GB > 4 GB (batas FAT32)

*Step 3:* Analisis implikasi forensik

- File 5 GB **tidak mungkin disimpan utuh** pada FAT32
- Kemungkinan: file dipecah menjadi beberapa bagian, dikompresi, atau tersangka menggunakan media lain
- Investigator perlu memeriksa apakah ada file split (*.001, *.002, dst) atau file arsip terkompresi

**Jawaban:** FAT32 memiliki batas ukuran file maksimal 4 GB. File 5 GB tidak dapat disimpan utuh. Kemungkinan tersangka memecah file menjadi beberapa bagian, menggunakan kompresi, atau mentransfer melalui media lain dengan sistem file NTFS atau exFAT yang mendukung file lebih besar.

---

#### Solved Problem 6 ⭐

**Soal:** Apa perbedaan utama antara exFAT dan FAT32, dan kapan exFAT lebih relevan dalam investigasi forensik?

**Penyelesaian:**

*Step 1:* Bandingkan kedua sistem file

| Aspek | FAT32 | exFAT |
|-------|-------|-------|
| **Ukuran file maks** | 4 GB | 16 EB (hampir tak terbatas) |
| **Ukuran volume maks** | 2 TB | 128 PB |
| **Journaling** | Tidak | Tidak (tetapi memiliki bitmap) |
| **Kompatibilitas** | Universal | Luas (Windows, macOS, Linux) |
| **Penggunaan umum** | USB, SD card lama | SD card besar (SDXC), media modern |

*Step 2:* Identifikasi relevansi forensik exFAT

exFAT lebih relevan saat menginvestigasi: media penyimpanan berkapasitas besar, kartu SD kamera/drone militer (SDXC), dan perangkat modern yang memerlukan file >4 GB.

**Jawaban:** exFAT mengatasi keterbatasan ukuran file FAT32 (4 GB) dan lebih relevan dalam investigasi forensik yang melibatkan media penyimpanan modern seperti kartu SD drone militer, kamera pengawasan, dan USB drive berkapasitas besar yang menyimpan file video atau image berukuran besar.

---

### 3.2 NTFS (New Technology File System)

> **NTFS (New Technology File System)** adalah sistem file jurnal proprietari Microsoft yang menjadi sistem file utama pada Windows sejak Windows NT 3.1. NTFS menyediakan fitur keamanan, kompresi, enkripsi, dan metadata yang kaya untuk keperluan forensik.

NTFS merupakan sistem file yang paling sering ditemui dalam investigasi forensik karena dominasi Windows pada workstation dan server, termasuk dalam lingkungan pertahanan.

#### 3.2.1 Fitur Utama NTFS

| Fitur | Deskripsi | Relevansi Forensik |
|-------|-----------|-------------------|
| **Journaling** | Mencatat perubahan sebelum diterapkan | Recovery dan timeline analysis |
| **MFT** | Database metadata semua file | Sumber utama informasi file |
| **ADS** | Alternate Data Streams | Penyembunyian data |
| **Encryption** | EFS (Encrypting File System) | Tantangan dekripsi |
| **Compression** | Kompresi file transparan | Perhatikan saat akuisisi |
| **Quotas** | Pembatasan penggunaan disk | Informasi penggunaan per user |
| **Hard/Soft Links** | Tautan antar file | Hubungan file tersembunyi |
| **MACE Timestamps** | Modified, Accessed, Created, Entry Modified | Timeline analysis detail |

#### 3.2.2 Struktur Volume NTFS

| Area | Deskripsi | Forensik |
|------|-----------|----------|
| **Boot Sector** | VBR, BPB, bootstrap code | Informasi volume dan cluster size |
| **MFT** | Master File Table | Database utama metadata |
| **MFT Mirror** | Salinan parsial MFT | Backup recovery |
| **System Files** | $Bitmap, $LogFile, $Volume | Informasi sistem file |
| **Data Area** | File dan direktori pengguna | Konten file |

#### 3.2.3 Master File Table (MFT)

MFT adalah komponen paling penting NTFS dari perspektif forensik. Setiap file dan direktori pada volume NTFS memiliki setidaknya satu entri dalam MFT.

**Struktur MFT Entry (biasanya 1024 bytes):**

| Offset | Ukuran | Field | Deskripsi |
|--------|--------|-------|-----------|
| 0x00 | 4 bytes | Signature | "FILE" (0x454C4946) |
| 0x04 | 2 bytes | Offset to update sequence | Lokasi fixup array |
| 0x06 | 2 bytes | Size of update sequence | Ukuran fixup |
| 0x08 | 8 bytes | $LogFile Sequence Number | Nomor urut transaksi |
| 0x10 | 2 bytes | Sequence number | Nomor reuse entri |
| 0x12 | 2 bytes | Hard link count | Jumlah hard link |
| 0x14 | 2 bytes | Offset to first attribute | Offset atribut pertama |
| 0x16 | 2 bytes | Flags | 0x01=In use, 0x02=Directory |
| 0x18 | 4 bytes | Used size of MFT entry | Ukuran terpakai |
| 0x1C | 4 bytes | Allocated size of MFT entry | Ukuran teralokasi |

**MFT System Files (Metafiles):**

| MFT Record | Nama | Fungsi |
|------------|------|--------|
| 0 | **$MFT** | Master File Table itu sendiri |
| 1 | **$MFTMirr** | Mirror dari 4 record pertama MFT |
| 2 | **$LogFile** | Journal transaksi |
| 3 | **$Volume** | Informasi volume |
| 4 | **$AttrDef** | Definisi atribut |
| 5 | **.** (root) | Direktori root |
| 6 | **$Bitmap** | Peta alokasi cluster |
| 7 | **$Boot** | Boot sector |
| 8 | **$BadClus** | Daftar cluster rusak |
| 9 | **$Secure** | Deskriptor keamanan |
| 10 | **$UpCase** | Tabel uppercase |
| 11 | **$Extend** | Direktori ekstensi |

![Struktur MFT NTFS](images/p03-04-ntfs-mft-structure.png)

*Gambar 3.4: Struktur Master File Table dan atribut NTFS*

#### 3.2.4 Atribut NTFS

Setiap entri MFT terdiri dari atribut-atribut yang menyimpan metadata dan konten file:

| Atribut | Type ID | Deskripsi | Nilai Forensik |
|---------|---------|-----------|---------------|
| **$STANDARD_INFORMATION** | 0x10 | Timestamps MACE, flags, owner | Timeline analysis utama |
| **$ATTRIBUTE_LIST** | 0x20 | Daftar atribut jika MFT entry penuh | File besar/kompleks |
| **$FILE_NAME** | 0x30 | Nama file, timestamps MACE | Nama asli, timestamps alternatif |
| **$OBJECT_ID** | 0x40 | Identifier unik objek | Tracking file |
| **$SECURITY_DESCRIPTOR** | 0x50 | ACL dan permission | Analisis akses |
| **$VOLUME_NAME** | 0x60 | Nama volume | Identifikasi volume |
| **$DATA** | 0x80 | Konten file | Data aktual file |
| **$INDEX_ROOT** | 0x90 | Root B-tree index | Struktur direktori |
| **$INDEX_ALLOCATION** | 0xA0 | Non-resident index | Direktori besar |
| **$BITMAP** | 0xB0 | Bitmap alokasi | Status alokasi |
| **$LOGGED_UTILITY_STREAM** | 0x100 | EFS encryption data | Informasi enkripsi |

#### 3.2.5 NTFS Timestamps (MACE)

NTFS menyimpan empat timestamps untuk setiap file, yang sangat penting dalam forensik:

| Timestamp | $STANDARD_INFORMATION | $FILE_NAME |
|-----------|----------------------|------------|
| **M** - Modified | Terakhir konten diubah | Diset saat file dibuat |
| **A** - Accessed | Terakhir diakses | Diset saat file dibuat |
| **C** - Created (Birth) | Waktu pembuatan | Waktu pembuatan |
| **E** - Entry Modified | Terakhir MFT entry diubah | Diset saat file dibuat |

> **Penting untuk Forensik:** Timestamps di $STANDARD_INFORMATION dapat dimanipulasi oleh user/aplikasi (timestomping), sedangkan timestamps di $FILE_NAME hanya diubah oleh kernel Windows, sehingga lebih reliable sebagai bukti.

#### Solved Problem 7 ⭐⭐

**Soal:** Seorang analis forensik di BSSN menemukan file yang dicurigai telah dilakukan timestomping. Timestamp $STANDARD_INFORMATION menunjukkan Created: 2020-01-15, namun timestamp $FILE_NAME menunjukkan Created: 2025-11-20. Apa yang dapat disimpulkan?

**Penyelesaian:**

*Step 1:* Pahami perbedaan kedua atribut timestamp

- `$STANDARD_INFORMATION`: Dapat dimodifikasi oleh aplikasi user-mode (rentan timestomping)
- `$FILE_NAME`: Hanya dapat dimodifikasi oleh kernel Windows (lebih terpercaya)

*Step 2:* Analisis anomali

| Atribut | Created Date | Dapat Dimanipulasi? |
|---------|-------------|---------------------|
| $STANDARD_INFORMATION | 2020-01-15 | Ya (oleh user/aplikasi) |
| $FILE_NAME | 2025-11-20 | Tidak (hanya kernel) |

*Step 3:* Kesimpulan

Anomali: $STANDARD_INFORMATION Created **lebih lama** dari $FILE_NAME Created. Ini secara logis tidak mungkin terjadi secara natural karena keduanya diset bersamaan saat file pertama kali dibuat.

**Jawaban:** Kesimpulan: telah terjadi **timestomping**. Seseorang memodifikasi timestamp $STANDARD_INFORMATION agar terlihat lebih lama (2020) padahal file sebenarnya dibuat pada 2025-11-20 (berdasarkan $FILE_NAME yang tidak dapat dimanipulasi). Ini mengindikasikan upaya anti-forensik untuk menyamarkan waktu pembuatan file yang sebenarnya.

---

#### Solved Problem 8 ⭐⭐

**Soal:** Jelaskan peran $LogFile dan $UsnJrnl dalam investigasi forensik dan perbedaan di antara keduanya!

**Penyelesaian:**

*Step 1:* Identifikasi fungsi masing-masing

*Step 2:* Bandingkan kedua journal

| Aspek | $LogFile | $UsnJrnl |
|-------|---------|----------|
| **Nama lengkap** | Transaction Log File | Update Sequence Number Journal |
| **Tujuan utama** | Recovery setelah crash | Tracking perubahan file |
| **Level** | Operasi file system level rendah | Perubahan file level tinggi |
| **Ukuran** | Biasanya 64 MB | Biasanya 32-64 MB |
| **Data yang dicatat** | Operasi metadata (MFT changes) | Nama file, alasan perubahan, timestamp |
| **Overwrite** | Circular log (cepat tertimpa) | Circular, tapi lebih lama tersimpan |
| **Nilai forensik** | Rekonstruksi operasi file system | Timeline aktivitas file user |

*Step 3:* Contoh penggunaan forensik

- **$LogFile**: Mendeteksi perubahan MFT entry setelah file dihapus, memulihkan metadata
- **$UsnJrnl**: Menelusuri sejarah file — kapan dibuat, diubah, dihapus, diganti nama

**Jawaban:** $LogFile mencatat operasi file system level rendah untuk recovery setelah crash, sementara $UsnJrnl mencatat perubahan file level tinggi yang lebih berguna untuk timeline analysis. Keduanya saling melengkapi: $LogFile membantu merekonstruksi operasi teknis, sedangkan $UsnJrnl memberikan gambaran aktivitas pengguna yang lebih mudah diinterpretasi.

---

#### 3.2.6 Alternate Data Streams (ADS)

> **Alternate Data Streams (ADS)** adalah fitur NTFS yang memungkinkan file memiliki beberapa stream data selain stream utama ($DATA default). ADS sering digunakan untuk menyembunyikan data karena tidak terlihat melalui Windows Explorer atau perintah `dir` standar.

**Cara membuat ADS:**
```cmd
echo "data rahasia" > file.txt:hidden_stream
```

**Cara mendeteksi ADS:**
```cmd
dir /R
streams.exe file.txt    (Sysinternals)
```

| Penggunaan Legitimate | Penggunaan Malicious |
|----------------------|---------------------|
| Zone.Identifier (download source) | Menyembunyikan malware |
| Thumbnail cache | Menyembunyikan data curian |
| Summary information | Exfiltration channel |
| macOS resource fork compatibility | Menyimpan konfigurasi C2 |

#### Solved Problem 9 ⭐⭐

**Soal:** Dalam investigasi kebocoran dokumen rahasia di Mabesad, investigator menemukan file `laporan.docx` berukuran 15 KB menurut Windows Explorer, namun FTK Imager menunjukkan total ukuran termasuk streams adalah 2,3 MB. Apa yang mungkin terjadi?

**Penyelesaian:**

*Step 1:* Identifikasi penyebab perbedaan ukuran

Perbedaan ukuran (15 KB vs 2,3 MB) menunjukkan adanya **Alternate Data Streams (ADS)** yang menyimpan data tambahan sebesar ~2,285 MB.

*Step 2:* Langkah investigasi

1. Periksa ADS menggunakan `dir /R laporan.docx` atau Sysinternals Streams
2. Ekstrak konten ADS: `more < laporan.docx:nama_stream`
3. Analisis konten ADS — kemungkinan data rahasia disembunyikan di stream alternatif

*Step 3:* Evaluasi temuan

| Temuan | Analisis |
|--------|---------|
| File utama 15 KB | Dokumen Word biasa/kosong |
| ADS ~2,285 MB | Kemungkinan dokumen rahasia tersembunyi |
| Rasio 1:153 | Sangat anomali, indikasi penyembunyian data |

**Jawaban:** Perbedaan ukuran menunjukkan file `laporan.docx` memiliki Alternate Data Streams berukuran ~2,285 MB yang tidak terlihat melalui Windows Explorer biasa. Kemungkinan tersangka menyembunyikan dokumen rahasia dalam ADS file yang tampak tidak mencurigakan. Investigator harus mengekstrak dan menganalisis semua ADS pada file tersebut.

---

## 4. Konsep Cluster, Slack Space, dan Unallocated Space

### 4.1 Cluster dan Alokasi Penyimpanan

> **Cluster** (atau allocation unit) adalah unit terkecil yang dialokasikan oleh sistem file untuk menyimpan data. Satu cluster terdiri dari satu atau lebih sektor yang berurutan.

| Ukuran Volume | Cluster Size Default (NTFS) | Cluster Size Default (FAT32) |
|---------------|----------------------------|------------------------------|
| ≤ 512 MB | 512 bytes | 512 bytes |
| 512 MB – 1 GB | 1 KB | 4 KB |
| 1 – 2 GB | 2 KB | 4 KB |
| 2 – 16 GB | 4 KB | 4 KB |
| 16 – 32 GB | 4 KB | 8 KB |
| 32 GB – 2 TB | 4 KB | 16 KB |

### 4.2 Slack Space

> **Slack Space** adalah area yang tidak terpakai di dalam cluster yang sudah dialokasikan. Slack space dapat menyimpan sisa data dari file sebelumnya yang sangat berharga secara forensik.

Slack space terdiri dari dua komponen:

1. **RAM Slack (File Slack)**: Area antara akhir data file dan akhir sektor terakhir. Pada Windows modern, biasanya diisi dengan nol.
2. **Drive Slack (Disk Slack)**: Area antara akhir sektor terakhir file dan akhir cluster. Dapat berisi data dari file sebelumnya.

![Slack Space](images/p03-05-slack-space.png)

*Gambar 3.5: Konsep slack space dalam alokasi cluster*

#### Solved Problem 10 ⭐

**Soal:** Sebuah file berukuran 5.000 bytes disimpan pada volume NTFS dengan cluster size 4.096 bytes (4 KB). Berapa jumlah cluster yang dialokasikan dan berapa besar slack space yang dihasilkan?

**Penyelesaian:**

*Step 1:* Hitung jumlah cluster yang diperlukan

File size: 5.000 bytes
Cluster size: 4.096 bytes
Cluster yang diperlukan: ⌈5.000 / 4.096⌉ = ⌈1,22⌉ = **2 cluster**

*Step 2:* Hitung total ruang yang dialokasikan

Ruang teralokasi: 2 × 4.096 = 8.192 bytes

*Step 3:* Hitung slack space

Slack space: 8.192 - 5.000 = **3.192 bytes**

| Komponen | Ukuran | Penjelasan |
|----------|--------|------------|
| Data file | 5.000 bytes | Data aktual |
| RAM Slack | Sisa di sektor terakhir file | Diisi nol pada Windows modern |
| Drive Slack | Sektor setelah sektor terakhir file hingga akhir cluster | Mungkin berisi data lama |
| **Total Slack** | **3.192 bytes** | Area berpotensi forensik |

**Jawaban:** Dialokasikan 2 cluster (8.192 bytes) untuk file 5.000 bytes, menghasilkan **3.192 bytes slack space**. Slack space ini berpotensi menyimpan fragmen data dari file sebelumnya yang pernah menggunakan cluster yang sama.

---

### 4.3 Unallocated Space

> **Unallocated Space** adalah area pada disk yang tidak ditempati oleh partisi atau file aktif. Area ini dapat menyimpan data dari file yang telah dihapus dan merupakan target utama file carving.

Jenis unallocated space:

| Jenis | Lokasi | Konten Potensial |
|-------|--------|-----------------|
| **Volume slack** | Antara akhir file system dan akhir partisi | Data lama |
| **Partition gap** | Antara partisi | Data tersembunyi |
| **Unpartitioned space** | Setelah partisi terakhir | Partisi tersembunyi, data tersembunyi |
| **Deleted file space** | Cluster yang dirilis setelah file dihapus | File terhapus yang dapat di-recover |

#### Solved Problem 11 ⭐⭐

**Soal:** Jelaskan mengapa file yang sudah dihapus dari Recycle Bin masih dapat dipulihkan dan kapan file benar-benar tidak dapat dipulihkan!

**Penyelesaian:**

*Step 1:* Pahami proses penghapusan file

| Tahap | Aksi | Data di Disk |
|-------|------|-------------|
| File aktif | Entry di MFT/FAT aktif | Data utuh di cluster |
| Hapus ke Recycle Bin | File dipindah ke $Recycle.Bin | Data utuh, metadata berubah |
| Kosongkan Recycle Bin | Entry MFT ditandai "not in use" | **Data masih utuh di cluster** |
| Cluster ditimpa | File baru menggunakan cluster yang sama | **Data hilang permanen** |

*Step 2:* Jelaskan mengapa masih bisa dipulihkan

Saat file dihapus, sistem file hanya menandai cluster sebagai "free" tanpa menghapus data aktual. Data tetap ada sampai ditimpa oleh file baru.

*Step 3:* Identifikasi kondisi data tidak dapat dipulihkan

1. Cluster telah ditimpa oleh file baru
2. SSD dengan TRIM yang telah menghapus data secara fisik
3. Secure wipe/overwrite (multiple passes)
4. Kerusakan fisik media yang parah

**Jawaban:** File yang dihapus masih dapat dipulihkan karena penghapusan hanya menandai cluster sebagai "free" tanpa menghapus data aktual. File benar-benar tidak dapat dipulihkan jika: (1) cluster telah ditimpa file baru, (2) TRIM pada SSD telah membersihkan data, (3) dilakukan secure wipe, atau (4) media mengalami kerusakan fisik yang parah.

---

## 5. Sistem File Linux: ext2, ext3, ext4

### 5.1 Evolusi Sistem File ext

> **Extended File System (ext)** adalah keluarga sistem file yang dikembangkan untuk sistem operasi Linux. ext4 adalah versi terkini yang paling banyak digunakan.

| Fitur | ext2 | ext3 | ext4 |
|-------|------|------|------|
| **Tahun** | 1993 | 2001 | 2008 |
| **Journaling** | Tidak | Ya | Ya |
| **Ukuran file maks** | 2 TB | 2 TB | 16 TB |
| **Ukuran volume maks** | 32 TB | 32 TB | 1 EB |
| **Extents** | Tidak | Tidak | Ya |
| **Timestamps** | Detik | Detik | Nanosecond |
| **Delayed allocation** | Tidak | Tidak | Ya |

### 5.2 Struktur Sistem File ext4

Setiap volume ext4 terdiri dari:

1. **Superblock**: Metadata utama file system (lokasi: block 0 atau 1)
2. **Block Group Descriptors**: Deskripsi setiap block group
3. **Block Groups**: Pembagian volume menjadi grup-grup block
4. **Inode Table**: Metadata file (setara MFT pada NTFS)
5. **Data Blocks**: Penyimpanan data file

#### 5.2.1 Inode (Index Node)

Inode adalah struktur data yang menyimpan metadata file pada sistem file ext. Setiap file memiliki tepat satu inode.

| Field | Ukuran | Deskripsi |
|-------|--------|-----------|
| Mode | 2 bytes | Tipe file dan permission |
| UID | 2 bytes | User ID pemilik |
| Size | 4 bytes | Ukuran file |
| Timestamps | 4×4 bytes | atime, ctime, mtime, dtime |
| GID | 2 bytes | Group ID |
| Link count | 2 bytes | Jumlah hard link |
| Block pointers | 60 bytes | Direct, indirect, double indirect, triple indirect blocks |

> **Penting untuk Forensik:** ext4 menyimpan **deletion time (dtime)** di inode, yang tidak ada pada NTFS. Ini sangat berguna untuk mengetahui kapan file dihapus.

#### Solved Problem 12 ⭐⭐

**Soal:** Bandingkan mekanisme penyimpanan metadata file antara NTFS (MFT) dan ext4 (Inode)! Mana yang lebih informatif untuk forensik?

**Penyelesaian:**

*Step 1:* Bandingkan kedua mekanisme

| Aspek | NTFS (MFT) | ext4 (Inode) |
|-------|-----------|-------------|
| **Struktur** | MFT Entry (1024 bytes) | Inode (256 bytes default) |
| **Nama file** | Disimpan dalam MFT entry ($FILE_NAME) | Disimpan di directory entry (terpisah dari inode) |
| **Timestamps** | 4 (MACE) × 2 atribut = 8 timestamps | 4 (atime, mtime, ctime, dtime) |
| **Presisi waktu** | 100 nanosecond | ext4: nanosecond, ext2/3: second |
| **Deletion time** | Tidak ada (tapi MFT entry flag berubah) | Ada (dtime) |
| **ADS** | Didukung (multiple $DATA streams) | Tidak didukung |
| **Extended attributes** | Melalui atribut NTFS | xattr |
| **Journaling info** | $LogFile, $UsnJrnl | Journal (ext3/ext4) |

*Step 2:* Evaluasi untuk forensik

NTFS lebih informatif karena: memiliki dual timestamps ($SI dan $FN) yang memungkinkan deteksi timestomping, ADS yang perlu diperiksa, dan $UsnJrnl yang mencatat perubahan file.

ext4 memiliki keunggulan unik: deletion time (dtime) langsung tercatat di inode.

**Jawaban:** NTFS lebih informatif secara keseluruhan untuk forensik karena menyimpan 8 timestamps (4 di $STANDARD_INFORMATION + 4 di $FILE_NAME), mendukung ADS, dan memiliki journal yang lebih detail ($UsnJrnl). Namun ext4 memiliki keunggulan unik yaitu menyimpan deletion time (dtime) di inode yang langsung menunjukkan kapan file dihapus.

---

## 6. Pengenalan Sistem File Mac: HFS+ dan APFS

### 6.1 HFS+ (Mac OS Extended)

> **HFS+ (Hierarchical File System Plus)** adalah sistem file yang digunakan oleh macOS sebelum APFS. Masih ditemukan pada perangkat Mac lama dan beberapa iPod.

| Fitur | HFS+ |
|-------|------|
| **Tahun** | 1998 |
| **Journaling** | Ya (opsional) |
| **Case sensitivity** | Opsional (default: case-insensitive) |
| **Catalog file** | B-tree untuk metadata (setara MFT) |
| **Timestamp epoch** | 1 Januari 1904 |
| **Hard link** | Didukung |
| **Compression** | Didukung (transparent) |

### 6.2 APFS (Apple File System)

> **APFS (Apple File System)** adalah sistem file modern Apple yang menggantikan HFS+ sejak macOS High Sierra (2017). Dirancang untuk SSD dan flash storage.

| Fitur | APFS |
|-------|------|
| **Tahun** | 2017 |
| **Optimisasi** | SSD dan flash storage |
| **Snapshots** | Ya (backup Time Machine) |
| **Cloning** | Ya (instant file/directory copy) |
| **Encryption** | Native (per-volume dan per-file) |
| **Space sharing** | Container dengan volume yang berbagi ruang |
| **Timestamps** | Nanosecond precision |

#### 6.2.1 Tantangan Forensik APFS

| Tantangan | Deskripsi |
|-----------|-----------|
| **Enkripsi native** | Sulit diakses tanpa kunci/password |
| **Cloning** | File clone berbagi data blocks, menyulitkan analisis |
| **Snapshots** | Data mungkin tersimpan dalam snapshot tersembunyi |
| **Space sharing** | Batas volume tidak jelas |
| **Tool support** | Dukungan tool forensik masih berkembang |

#### Solved Problem 13 ⭐⭐

**Soal:** Mengapa seorang investigator forensik militer perlu memahami APFS meskipun TNI umumnya menggunakan Windows? Berikan tiga skenario relevan!

**Penyelesaian:**

*Step 1:* Identifikasi skenario di mana perangkat Apple relevan dalam konteks militer

*Step 2:* Uraikan tiga skenario

| Skenario | Konteks | Relevansi APFS |
|----------|---------|---------------|
| **1. Perangkat pribadi personel** | Perwira menggunakan MacBook/iPhone pribadi untuk komunikasi | Data sensitif mungkin tersimpan di perangkat Apple pribadi |
| **2. Investigasi pihak ketiga** | Vendor/kontraktor pertahanan menggunakan Mac | Investigasi rantai pasok pertahanan |
| **3. Operasi intelijen** | Target menggunakan perangkat Apple | Forensik perangkat musuh/target operasi |

**Jawaban:** Investigator militer perlu memahami APFS karena: (1) Personel TNI mungkin menyimpan data sensitif di perangkat Apple pribadi (BYOD), (2) Kontraktor dan vendor pertahanan sering menggunakan Mac dalam operasi mereka, (3) Dalam operasi intelijen, target mungkin menggunakan perangkat Apple yang perlu dianalisis secara forensik.

---

## 7. File System Journaling

### 7.1 Konsep Journaling

> **Journaling** adalah mekanisme yang mencatat perubahan yang akan dilakukan pada file system sebelum perubahan tersebut benar-benar diterapkan. Ini menjamin konsistensi file system setelah crash atau power failure.

#### 7.1.1 Mode Journaling

| Mode | Yang Dicatat | Keamanan | Performa | Contoh |
|------|-------------|----------|----------|--------|
| **Journal** (full) | Metadata + data | Tertinggi | Terendah | ext3/4 journal mode |
| **Ordered** (default) | Metadata saja (data ditulis duluan) | Tinggi | Sedang | ext3/4 default |
| **Writeback** | Metadata saja (urutan tidak dijamin) | Rendah | Tertinggi | ext3/4 writeback |

#### 7.1.2 Implikasi Forensik Journaling

| Sistem File | Journal | Informasi Forensik |
|-------------|---------|-------------------|
| **NTFS** | $LogFile | Operasi file system, metadata changes |
| **NTFS** | $UsnJrnl | Change journal: file creation, deletion, rename |
| **ext3/ext4** | jbd/jbd2 | Transaksi file system |
| **HFS+** | .journal | Metadata transactions |
| **APFS** | Built-in | Transaction groups |

> **Nilai Forensik:** Journal menyimpan "sejarah" perubahan file system yang sangat berguna untuk merekonstruksi timeline aktivitas, bahkan setelah file dihapus atau dimodifikasi.

#### Solved Problem 14 ⭐⭐

**Soal:** Bagaimana journal file system dapat membantu investigator membuktikan bahwa seorang personel militer menghapus file rahasia pada waktu tertentu?

**Penyelesaian:**

*Step 1:* Identifikasi sumber bukti dari journal

*Step 2:* Rekonstruksi timeline menggunakan journal

Pada NTFS, investigator dapat menggunakan:

1. **$UsnJrnl**: Mencatat event penghapusan file (USN_REASON_FILE_DELETE) dengan timestamp presisi tinggi
2. **$LogFile**: Mencatat operasi MFT yang menandai entry sebagai "not in use"
3. **MFT entry**: Flag berubah dari 0x01 (in use) menjadi 0x00

| Sumber | Informasi | Bukti |
|--------|-----------|-------|
| $UsnJrnl | Timestamp penghapusan, nama file, alasan | Kapan dan file apa yang dihapus |
| $LogFile | Operasi teknis pada MFT | Konfirmasi teknis penghapusan |
| MFT | Entry flag, sequence number | Status file sebelum dan sesudah |

**Jawaban:** Journal membantu membuktikan penghapusan karena: (1) $UsnJrnl mencatat event FILE_DELETE dengan timestamp presisi nanosecond dan nama file asli, (2) $LogFile merekam perubahan teknis pada MFT entry, (3) Kombinasi ketiganya memberikan bukti kuat yang menunjukkan siapa (melalui SID), kapan, dan file apa yang dihapus. Bukti ini sulit untuk dihapus karena journal dikelola oleh kernel OS.

---

## 8. Forensik SSD: Tantangan dan Solusi

### 8.1 TRIM Command

> **TRIM** adalah perintah ATA yang memberitahu SSD bahwa blok data tertentu tidak lagi digunakan dan dapat dihapus secara internal. TRIM mengoptimalkan performa SSD tetapi menghancurkan data yang mungkin dapat dipulihkan.

#### 8.1.1 Cara Kerja TRIM

| Tahap | HDD | SSD dengan TRIM |
|-------|-----|----------------|
| File aktif | Data di cluster | Data di page |
| File dihapus (OS) | Cluster ditandai free, data tetap | Cluster ditandai free |
| TRIM dieksekusi | - (tidak ada TRIM di HDD) | Controller menghapus page secara fisik |
| Recovery | Mungkin (data masih ada) | **Tidak mungkin** (data sudah dihapus fisik) |

### 8.2 Wear Leveling

> **Wear Leveling** adalah teknik yang mendistribusikan penulisan secara merata ke seluruh sel NAND flash untuk memperpanjang umur SSD. Akibatnya, data tidak selalu ditulis ke lokasi yang sama.

| Tipe | Mekanisme | Implikasi Forensik |
|------|-----------|-------------------|
| **Dynamic** | Hanya mendistribusikan blok yang sering ditulis | Data lama mungkin tersisa di lokasi asli |
| **Static** | Mendistribusikan semua blok termasuk yang jarang ditulis | Data tersebar lebih luas, recovery lebih sulit |

### 8.3 Garbage Collection

> **Garbage Collection** adalah proses internal SSD yang menggabungkan halaman valid, menghapus halaman invalid, dan menyiapkan blok kosong untuk penulisan masa depan. Proses ini berjalan otomatis dan dapat menghancurkan data tanpa perintah pengguna.

#### Solved Problem 15 ⭐⭐

**Soal:** Seorang tersangka pencurian data rahasia militer menggunakan laptop dengan SSD NVMe. TRIM diaktifkan. File rahasia telah dihapus 2 jam sebelum laptop disita. Evaluasi kemungkinan recovery data!

**Penyelesaian:**

*Step 1:* Analisis faktor-faktor yang memengaruhi recovery

| Faktor | Status | Dampak pada Recovery |
|--------|--------|---------------------|
| **Media** | SSD NVMe | Recovery sangat sulit |
| **TRIM** | Aktif | Data kemungkinan sudah dihapus fisik |
| **Waktu** | 2 jam | TRIM biasanya dieksekusi dalam menit |
| **Aktivitas** | Laptop masih hidup | Garbage collection mungkin sudah berjalan |

*Step 2:* Evaluasi kemungkinan recovery

- TRIM pada SSD modern biasanya dieksekusi **hampir segera** (queued TRIM)
- Setelah 2 jam, kemungkinan besar data sudah dihapus secara fisik
- Kemungkinan recovery: **sangat rendah** (<5%)

*Step 3:* Strategi alternatif

1. Periksa RAM dump (jika laptop masih hidup saat disita)
2. Periksa $UsnJrnl dan $LogFile untuk bukti eksistensi file
3. Periksa metadata MFT (entry mungkin masih ada meskipun data sudah hilang)
4. Periksa cloud backup atau destinasi sinkronisasi
5. Periksa over-provisioned area SSD (area yang tidak dijangkau TRIM)

**Jawaban:** Kemungkinan recovery data aktual sangat rendah (<5%) karena TRIM pada NVMe biasanya dieksekusi hampir segera. Namun investigator masih dapat: (1) Memperoleh bukti eksistensi file dari $UsnJrnl dan MFT metadata, (2) Memeriksa RAM jika laptop masih hidup, (3) Memeriksa over-provisioned area, (4) Mencari salinan di cloud atau perangkat lain yang terhubung.

---

#### Solved Problem 16 ⭐⭐⭐

**Soal:** Sebuah pangkalan militer AU (Lanud) kehilangan data peta pertahanan udara. Hard disk server menggunakan NTFS dengan cluster size 4 KB. Investigator menemukan MFT entry file yang dihapus dengan $DATA attribute menunjukkan data runs: (cluster 1000, 50 cluster), (cluster 2500, 30 cluster), (cluster 5000, 20 cluster). Jelaskan bagaimana merekonstruksi file dan hitung total ukuran data yang dapat dipulihkan!

**Penyelesaian:**

*Step 1:* Pahami data runs NTFS

Data runs menunjukkan lokasi fisik data file pada disk:

| Run | Starting Cluster | Length | Range |
|-----|-----------------|--------|-------|
| 1 | 1000 | 50 cluster | Cluster 1000-1049 |
| 2 | 2500 | 30 cluster | Cluster 2500-2529 |
| 3 | 5000 | 20 cluster | Cluster 5000-5019 |

*Step 2:* Hitung total ukuran

Total cluster = 50 + 30 + 20 = 100 cluster
Ukuran per cluster = 4 KB = 4.096 bytes
Total ukuran = 100 × 4.096 = **409.600 bytes (400 KB)**

*Step 3:* Prosedur rekonstruksi

1. Verifikasi cluster belum ditimpa (periksa $Bitmap)
2. Ekstrak data dari setiap run secara berurutan
3. Gabungkan data: Run 1 + Run 2 + Run 3
4. Verifikasi file signature dan integritas
5. Hitung hash untuk chain of custody

*Step 4:* Pertimbangan

| Risiko | Mitigasi |
|--------|---------|
| Cluster mungkin sudah ditimpa sebagian | Periksa $Bitmap untuk status alokasi |
| File mungkin terenkripsi (EFS) | Periksa $LOGGED_UTILITY_STREAM attribute |
| Fragment mungkin tidak lengkap | Lakukan file carving pada unallocated space |

**Jawaban:** File tersebar di 3 lokasi (fragmented) dengan total 100 cluster = **400 KB** data. Rekonstruksi dilakukan dengan mengekstrak data dari setiap run secara berurutan (cluster 1000-1049, 2500-2529, 5000-5019) dan menggabungkannya. Investigator harus memverifikasi melalui $Bitmap bahwa cluster belum ditimpa, kemudian memvalidasi file yang direkonstruksi dengan file signature analysis.

---

#### Solved Problem 17 ⭐⭐⭐

**Soal:** Dalam investigasi kebocoran data di sistem Command, Control, Communications, Computers, Intelligence, Surveillance, and Reconnaissance (C4ISR) TNI, investigator menemukan partisi yang menggunakan GPT. Analisis hex dump menunjukkan GPT header yang rusak (CRC32 tidak valid) tetapi backup GPT header di akhir disk masih utuh. Jelaskan langkah recovery tabel partisi dan implikasinya!

**Penyelesaian:**

*Step 1:* Pahami mekanisme redundansi GPT

GPT menyimpan dua salinan:
- **Primary GPT**: Header di LBA 1, entries di LBA 2-33
- **Backup GPT**: Header di LBA terakhir, entries di LBA -33 s/d -2

*Step 2:* Prosedur recovery

| Langkah | Aksi | Tool |
|---------|------|------|
| 1 | Buat image forensik terlebih dahulu | FTK Imager, dd |
| 2 | Baca backup GPT header dari LBA terakhir | Hex editor (HxD) |
| 3 | Validasi CRC32 backup header | Python script, gdisk |
| 4 | Bandingkan backup entries dengan primary entries | Hex comparison |
| 5 | Gunakan backup untuk merekonstruksi primary | gdisk, TestDisk |
| 6 | Verifikasi partisi yang terdeteksi | Autopsy, FTK Imager |

*Step 3:* Implikasi forensik

- Kerusakan GPT header bisa **disengaja** (anti-forensik) atau **tidak disengaja** (korupsi)
- Perbandingan primary vs backup dapat mengungkap modifikasi yang dilakukan
- Jika hanya primary yang rusak dan backup utuh → kemungkinan anti-forensik rendah
- Jika keduanya berbeda signifikan → kemungkinan manipulasi yang tidak sempurna

**Jawaban:** Recovery dilakukan menggunakan backup GPT header yang utuh: (1) Buat forensic image, (2) Validasi backup header CRC32, (3) Rekonstruksi primary dari backup menggunakan gdisk/TestDisk, (4) Verifikasi partisi yang terdeteksi. Kerusakan primary GPT header saja menunjukkan kemungkinan korupsi alami, tetapi investigator harus memeriksa apakah ada tanda-tanda manipulasi yang disengaja sebagai teknik anti-forensik.

---

#### Solved Problem 18 ⭐⭐⭐

**Soal:** Tim forensik Kodam menemukan hard disk dari tersangka yang berisi partisi NTFS dan ext4 pada disk yang sama. $UsnJrnl NTFS menunjukkan sebuah file bernama "rencana_operasi.pdf" dihapus pada timestamp T1. Inode ext4 pada partisi Linux menunjukkan file "backup.pdf" dengan creation time T1-5 menit dan deletion time T1+3 menit. Analisis hubungan kedua temuan!

**Penyelesaian:**

*Step 1:* Buat timeline

| Waktu | Event | Partisi | Sumber Bukti |
|-------|-------|---------|-------------|
| T1 - 5 menit | backup.pdf dibuat | ext4 | Inode ctime |
| T1 | rencana_operasi.pdf dihapus | NTFS | $UsnJrnl |
| T1 + 3 menit | backup.pdf dihapus | ext4 | Inode dtime |

*Step 2:* Analisis korelasi

Pola yang terlihat:
1. Backup dibuat di partisi ext4 **sebelum** file asli dihapus dari NTFS
2. Backup dihapus **segera setelah** file asli dihapus
3. Jendela waktu sangat singkat (total 8 menit) menunjukkan aksi terencana

*Step 3:* Hipotesis

Tersangka kemungkinan:
1. Membuat salinan cadangan "rencana_operasi.pdf" ke partisi ext4 sebagai "backup.pdf"
2. Menghapus file asli dari partisi NTFS
3. Kemudian menghapus salinan cadangan dari partisi ext4
4. Mungkin telah mentransfer file ke media eksternal sebelum menghapus kedua salinan

*Step 4:* Implikasi forensik

- ext4 menyimpan deletion time (dtime) yang membuktikan kapan backup dihapus
- NTFS $UsnJrnl membuktikan kapan file asli dihapus
- Korelasi kedua bukti menunjukkan pola **data exfiltration** yang terencana

**Jawaban:** Timeline menunjukkan pola data exfiltration terencana: (1) Backup dibuat di ext4 5 menit sebelum file asli dihapus dari NTFS, (2) Backup dihapus 3 menit setelah penghapusan asli. Keunggulan ext4 menyimpan deletion time membuktikan timing penghapusan. Investigator harus memeriksa: (a) apakah ada media eksternal yang terhubung pada rentang waktu tersebut (USB log), (b) koneksi jaringan untuk kemungkinan transfer online, (c) attempt recovery kedua file dari masing-masing partisi.

---

#### Solved Problem 19 ⭐

**Soal:** Apa yang dimaksud dengan file system fragmentation dan bagaimana pengaruhnya terhadap investigasi forensik?

**Penyelesaian:**

*Step 1:* Definisikan fragmentasi

**Fragmentasi** terjadi ketika data file tersebar di cluster yang tidak berurutan pada disk. Ini terjadi karena file system mengalokasikan cluster yang tersedia, yang mungkin tidak bersebelahan.

*Step 2:* Pengaruh pada forensik

| Aspek | Pengaruh |
|-------|----------|
| **File carving** | Lebih sulit karena data tidak berurutan |
| **Recovery** | File terfragmentasi mungkin tidak dapat dipulihkan sempurna |
| **Analisis timeline** | Fragmentasi memberikan informasi tentang pola penggunaan disk |
| **Performa akuisisi** | Akuisisi tetap sama (bit-by-bit) tapi analisis lebih kompleks |

**Jawaban:** Fragmentasi menyebabkan data file tersebar di cluster yang tidak berurutan. Dampak forensik: (1) File carving lebih sulit karena harus melacak rantai cluster, (2) Recovery file terfragmentasi mungkin tidak lengkap tanpa metadata file system, (3) Namun fragmentasi juga memberikan informasi forensik tentang pola penggunaan disk dan umur file.

---

#### Solved Problem 20 ⭐

**Soal:** Sebuah USB flash drive berkapasitas 8 GB menggunakan FAT32 dengan cluster size 4 KB. Jika drive berisi 2.000 file yang masing-masing berukuran 1 KB, berapa total ruang yang terbuang akibat slack space?

**Penyelesaian:**

*Step 1:* Hitung alokasi per file

Setiap file 1 KB memerlukan minimal 1 cluster (4 KB) karena cluster adalah unit alokasi terkecil.

*Step 2:* Hitung slack space per file

Slack per file = Cluster size - File size = 4 KB - 1 KB = **3 KB**

*Step 3:* Hitung total slack space

Total slack = 2.000 file × 3 KB = **6.000 KB = ~5,86 MB**

*Step 4:* Hitung persentase pemborosan

Total terpakai sebenarnya = 2.000 × 1 KB = 2.000 KB = ~1,95 MB
Total teralokasi = 2.000 × 4 KB = 8.000 KB = ~7,81 MB
Persentase terbuang = 6.000 / 8.000 × 100% = **75%**

**Jawaban:** Total slack space = **6.000 KB (~5,86 MB)**, yang berarti 75% ruang teralokasi terbuang. Ini menunjukkan bahwa cluster size yang besar relatif terhadap ukuran file menyebabkan pemborosan signifikan. Dari perspektif forensik, slack space ini berpotensi menyimpan data dari file sebelumnya.

---

#### Solved Problem 21 ⭐⭐⭐

**Soal:** Rancang prosedur forensik lengkap untuk menganalisis hard disk server Sistem Informasi Pertahanan yang menggunakan dual boot Windows (NTFS) dan Linux (ext4) pada disk GPT. Server disita setelah terdeteksi koneksi mencurigakan ke server asing.

**Penyelesaian:**

*Step 1:* Persiapan dan preservasi

| Tahap | Aksi | Alat |
|-------|------|------|
| 1 | Dokumentasi fisik server (foto, serial number) | Kamera, formulir COC |
| 2 | Pasang write blocker hardware | Tableau forensic bridge |
| 3 | Buat forensic image (E01 format) | FTK Imager |
| 4 | Verifikasi hash (MD5 + SHA-256) | FTK Imager, HashMyFiles |
| 5 | Simpan image di lokasi aman | Encrypted storage |

*Step 2:* Analisis struktur disk

| Langkah | Tujuan | Tool |
|---------|--------|------|
| Periksa GPT header dan partition entries | Identifikasi semua partisi | FTK Imager, gdisk |
| Identifikasi tipe partisi | NTFS vs ext4 vs swap vs EFI | Autopsy |
| Periksa unpartitioned space | Partisi tersembunyi | Hex editor (HxD) |

*Step 3:* Analisis partisi NTFS (Windows)

1. Parse MFT menggunakan MFTExplorer
2. Analisis $UsnJrnl untuk timeline perubahan file
3. Analisis $LogFile untuk operasi file system
4. Periksa ADS pada file mencurigakan
5. Ekstrak event logs, prefetch, registry hives

*Step 4:* Analisis partisi ext4 (Linux)

1. Mount image secara read-only
2. Analisis /var/log untuk koneksi jaringan (auth.log, syslog)
3. Periksa bash_history untuk command yang dieksekusi
4. Analisis cron jobs untuk scheduled tasks
5. Periksa inode deleted files (dtime)

*Step 5:* Korelasi temuan cross-platform

1. Sinkronisasi timeline kedua OS
2. Identifikasi aktivitas yang berkaitan (file sharing antara partisi)
3. Korelasikan network logs dari kedua OS
4. Buat unified timeline

**Jawaban:** Prosedur mencakup 5 tahap: (1) Preservasi dengan write blocker dan forensic imaging, (2) Analisis struktur GPT untuk identifikasi semua partisi, (3) Analisis NTFS fokus pada MFT, journal, dan ADS, (4) Analisis ext4 fokus pada log files, bash history, dan inode dengan dtime, (5) Korelasi cross-platform untuk unified timeline. Pendekatan dual-OS memerlukan kombinasi tools Windows dan Linux forensics.

---

#### Solved Problem 22 ⭐⭐

**Soal:** Jelaskan perbedaan antara physical imaging dan logical imaging, dan kapan masing-masing lebih tepat digunakan dalam konteks forensik militer!

**Penyelesaian:**

*Step 1:* Definisikan kedua metode

| Aspek | Physical Imaging | Logical Imaging |
|-------|-----------------|-----------------|
| **Cakupan** | Seluruh disk (bit-by-bit) | File dan folder yang terlihat saja |
| **Termasuk** | Semua data: aktif, dihapus, slack, unallocated | Hanya file/folder yang ada di file system |
| **Ukuran output** | Sama dengan kapasitas disk | Lebih kecil (hanya data aktif) |
| **Waktu** | Lebih lama | Lebih cepat |
| **Recovery potential** | Maksimal | Terbatas |
| **Unallocated space** | Ya | Tidak |
| **Slack space** | Ya | Tidak |

*Step 2:* Kapan menggunakan masing-masing

| Skenario Militer | Metode | Alasan |
|-----------------|--------|--------|
| Investigasi kebocoran data rahasia | Physical | Memerlukan recovery file terhapus dan analisis slack space |
| Audit kepatuhan rutin | Logical | Cukup memeriksa file yang ada |
| Insiden keamanan siber aktif | Physical | Perlu analisis menyeluruh termasuk malware tersembunyi |
| Forensik perangkat di medan perang | Logical | Keterbatasan waktu dan sumber daya |
| Analisis quick triage | Logical | Perlu hasil cepat untuk keputusan taktis |

**Jawaban:** Physical imaging mencakup seluruh disk termasuk data terhapus dan slack space — digunakan untuk investigasi serius seperti kebocoran data rahasia atau insiden keamanan. Logical imaging hanya mencakup file yang terlihat — digunakan saat waktu terbatas seperti forensik lapangan atau audit rutin. Dalam konteks militer, physical imaging adalah standar untuk kasus hukum, sementara logical imaging cocok untuk triage cepat di lapangan.

---

## Supplementary Problems

### Problem S1 ⭐
**Soal:** Sebutkan lima perbedaan utama antara HDD dan SSD yang memengaruhi forensik digital!

**Jawaban:** (1) HDD menyimpan data secara magnetik sehingga data terhapus masih dapat dipulihkan, sedangkan SSD dengan TRIM menghapus data secara fisik. (2) HDD tidak memiliki wear leveling sehingga data ditulis di lokasi yang dapat diprediksi. (3) HDD tidak memiliki garbage collection. (4) SSD memiliki over-provisioned area yang mungkin menyimpan data sisa. (5) HDD mendukung recovery data dari bad sectors lebih baik karena data magnetik masih dapat dibaca dengan teknik khusus.

---

### Problem S2 ⭐⭐
**Soal:** Mengapa investigator harus memeriksa Zone.Identifier ADS dan apa informasi yang dapat diperoleh?

**Jawaban:** Zone.Identifier adalah ADS yang otomatis ditambahkan Windows pada file yang didownload dari internet. Informasi yang tersimpan meliputi: (1) ZoneId (0=Local, 1=Intranet, 2=Trusted, 3=Internet, 4=Untrusted), (2) URL sumber download, (3) Referrer URL. Ini membuktikan bahwa file didownload dari internet dan dapat mengidentifikasi sumber asli file.

---

### Problem S3 ⭐⭐
**Soal:** Apa yang dimaksud dengan resident dan non-resident attribute pada NTFS?

**Jawaban:**
- **Resident attribute**: Data atribut disimpan langsung dalam MFT entry. Untuk file kecil (biasanya <700 bytes pada MFT 1 KB entry), seluruh konten file disimpan di MFT tanpa menggunakan cluster terpisah.
- **Non-resident attribute**: Data terlalu besar untuk MFT entry sehingga disimpan di cluster terpisah, dengan MFT entry menyimpan data runs (pointer ke cluster).
- Implikasi forensik: File kecil yang resident di MFT tetap dapat dipulihkan meskipun cluster data sudah ditimpa.

---

### Problem S4 ⭐⭐⭐
**Soal:** Jelaskan bagaimana SSD over-provisioning dapat menjadi sumber bukti forensik!

**Jawaban:** Over-provisioned area adalah ruang ekstra pada SSD (biasanya 7-28% kapasitas) yang tidak dapat diakses oleh file system. Area ini digunakan controller untuk wear leveling dan garbage collection. Nilai forensik: (1) Data yang sudah di-TRIM dari user-accessible area mungkin masih ada di over-provisioned area, (2) Data tidak terpengaruh oleh TRIM karena berada di luar jangkauan OS, (3) Akses memerlukan teknik chip-off atau vendor-specific tools, (4) Pada SSD militer/enterprise, area ini bisa lebih besar dan menyimpan lebih banyak data sisa.

---

### Problem S5 ⭐⭐⭐
**Soal:** Bagaimana perbedaan penanganan forensik pada disk berformat MBR vs GPT saat menggunakan FTK Imager?

**Jawaban:** 
- **MBR**: FTK Imager membaca partition table dari sektor pertama (LBA 0), menampilkan maksimal 4 partisi primer. Investigator harus memeriksa extended partitions secara terpisah dan memeriksa unpartitioned space setelah partisi terakhir.
- **GPT**: FTK Imager membaca GPT header di LBA 1 dan partition entries di LBA 2-33. Dapat menampilkan hingga 128 partisi. Investigator harus memverifikasi konsistensi antara primary dan backup GPT header, memeriksa protective MBR, dan memeriksa gap antara EFI System Partition dan partisi data.
- Keduanya: selalu lakukan physical imaging untuk memastikan tidak ada data tersembunyi di area yang tidak terpartisi.

---

## Ringkasan

| Konsep | Deskripsi Singkat |
|--------|-------------------|
| **HDD** | Media magnetik dengan platter berputar, data terhapus masih recoverable |
| **SSD** | Media flash NAND tanpa komponen mekanik, tantangan forensik signifikan |
| **MBR** | Skema partisi legacy, maks 4 partisi, 2 TB |
| **GPT** | Skema partisi modern, maks 128 partisi, redundansi backup |
| **FAT** | Sistem file sederhana berbasis linked list, umum pada media removable |
| **NTFS** | Sistem file utama Windows, MFT sebagai database metadata, ADS, journaling |
| **MFT** | Master File Table — pusat metadata NTFS, target utama analisis forensik |
| **MACE Timestamps** | Modified, Accessed, Created, Entry Modified — kritis untuk timeline |
| **ADS** | Alternate Data Streams — mekanisme penyembunyian data pada NTFS |
| **ext4** | Sistem file Linux modern dengan journaling, menyimpan deletion time |
| **APFS** | Sistem file Apple modern, enkripsi native, snapshot, cloning |
| **Slack Space** | Area tidak terpakai dalam cluster, menyimpan data residual |
| **Unallocated Space** | Area tidak terpartisi, target file carving |
| **Journaling** | Mekanisme logging perubahan file system, kritis untuk timeline |
| **TRIM** | Perintah SSD yang menghapus data secara fisik, menghancurkan bukti |
| **Wear Leveling** | Distribusi penulisan SSD, menyebabkan data tersebar |
| **Tools** | FTK Imager (imaging), MFTExplorer (MFT), Autopsy (analisis), HxD (hex) |

---

## Referensi

1. Carrier, B. (2005). *File System Forensic Analysis*. Addison-Wesley. Chapter 3-11.
2. Casey, E. (2022). *Digital Evidence and Computer Crime: Forensic Science, Computers, and the Internet* (4th Ed.). Academic Press. Chapter 8.
3. Phillips, A., Nelson, B., & Steuart, C. (2022). *Guide to Computer Forensics and Investigations* (6th Ed.). Cengage Learning. Chapter 4-5.
4. Nikkel, B. (2021). *Practical Forensic Imaging: Securing Digital Evidence with Linux Tools* (2nd Ed.). No Starch Press. Chapter 4-6.
5. Bell, G. & Boddington, R. (2023). *Solid State Drive Forensics: Where Do We Stand?* Digital Investigation, Vol. 44.
6. Quick, D. & Choo, K.K.R. (2022). *Digital Forensics: Threats, Frameworks, and Best Practices*. Elsevier. Chapter 3.
7. NIST SP 800-86: Guide to Integrating Forensic Techniques into Incident Response.
8. Microsoft (2024). *NTFS Technical Reference*. Microsoft Docs.

---

## License / Lisensi

This repository is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

Commercial use is permitted, provided attribution is given to the author.

© 2026 Anindito
