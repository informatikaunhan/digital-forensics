# Latihan Pertemuan 04: Akuisisi dan Duplikasi Data Forensik

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 04  
**Topik:** Akuisisi dan Duplikasi Data Forensik  
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
Akuisisi forensik berbeda dari copy file biasa terutama karena:

A. Akuisisi forensik menggunakan kabel USB yang lebih cepat  
B. Akuisisi forensik membuat salinan bitwise dari seluruh media termasuk deleted data dan slack space  
C. Akuisisi forensik hanya menyalin file yang berukuran besar  
D. Akuisisi forensik memerlukan koneksi internet  
E. Akuisisi forensik hanya bisa dilakukan di laboratorium

---

### Soal 2
Fungsi utama write blocker dalam proses akuisisi forensik adalah:

A. Mempercepat proses penyalinan data  
B. Mengkompresi data agar lebih kecil  
C. Mencegah penulisan data ke media bukti sehingga integritas terjaga  
D. Mengenkripsi data selama proses transfer  
E. Memformat media penyimpanan sebelum akuisisi

---

### Soal 3
Metode akuisisi yang menyalin seluruh isi media penyimpanan secara bit-by-bit termasuk unallocated space, HPA, dan DCO disebut:

A. Logical acquisition  
B. Sparse acquisition  
C. Physical acquisition  
D. Remote acquisition  
E. Selective acquisition

---

### Soal 4
Manakah yang BUKAN merupakan keunggulan format E01 (EnCase) dibandingkan format raw (dd)?

A. Mendukung kompresi data  
B. Menyimpan metadata kasus (examiner, case number)  
C. Memiliki CRC per data block  
D. Bersifat open source dan bebas lisensi  
E. Ukuran file image lebih kecil karena kompresi

---

### Soal 5
Berdasarkan Order of Volatility (RFC 3227), urutan pengumpulan data yang benar dari paling volatile ke paling stabil adalah:

A. Hard Disk → RAM → CPU Register → Network State  
B. CPU Register → RAM → Network State → Hard Disk  
C. RAM → Hard Disk → CPU Register → Archival Media  
D. Network State → CPU Register → RAM → Hard Disk  
E. Archival Media → Hard Disk → RAM → CPU Register

---

### Soal 6
Live acquisition HARUS dilakukan ketika:

A. Media penyimpanan menggunakan file system FAT32  
B. Komputer sudah dalam keadaan mati  
C. Full disk encryption aktif dan key hanya tersedia di RAM  
D. Kapasitas hard disk sangat besar  
E. Bukti disimpan dalam USB flash drive

---

### Soal 7
Format image forensik yang mendukung kompresi, metadata, DAN enkripsi secara bersamaan adalah:

A. Raw (dd)  
B. E01 (EnCase)  
C. SMART  
D. AFF4 (Advanced Forensics Format 4)  
E. VMDK

---

### Soal 8
Algoritma hash yang direkomendasikan sebagai standar utama untuk forensik digital saat ini adalah:

A. MD5  
B. SHA-1  
C. CRC32  
D. SHA-256  
E. MD4

---

### Soal 9
Laptop ditemukan dalam sleep mode dengan BitLocker aktif. Langkah pertama yang PALING TEPAT dilakukan adalah:

A. Cabut baterai dan hard disk untuk dead acquisition  
B. Matikan laptop dengan menekan tombol power  
C. Akuisisi RAM terlebih dahulu untuk mendapatkan encryption key  
D. Jalankan chkdsk untuk memeriksa integritas disk  
E. Login ke Windows menggunakan password administrator

---

### Soal 10
Mengapa SSD lebih sulit untuk data recovery forensik dibandingkan HDD?

A. SSD memiliki kapasitas yang lebih besar  
B. SSD tidak mendukung format image E01  
C. TRIM dan garbage collection menghapus data secara otomatis di background  
D. SSD menggunakan file system yang berbeda dari HDD  
E. SSD tidak kompatibel dengan write blocker

---

### Soal 11
Tool yang BUKAN digunakan untuk akuisisi RAM (memory dump) adalah:

A. Belkasoft RAM Capturer  
B. WinPMEM  
C. FTK Imager (Memory Capture)  
D. Wireshark  
E. DumpIt

---

### Soal 12
Dalam proses verifikasi hash setelah akuisisi, jika hash media sumber TIDAK cocok dengan hash forensic image, maka:

A. Image tetap valid karena perbedaan hash adalah hal normal  
B. Integritas image diragukan dan bukti mungkin ditolak pengadilan  
C. Lakukan akuisisi ulang dengan block size yang berbeda  
D. Hash tidak relevan dalam forensik modern  
E. Gunakan MD5 sebagai pengganti SHA-256

---

### Soal 13
Perintah dcfldd yang benar untuk melakukan akuisisi forensik dengan hashing otomatis adalah:

A. `dcfldd if=/dev/sdb of=evidence.dd hash=md5,sha256`  
B. `dcfldd copy /dev/sdb evidence.dd --hash`  
C. `dcfldd clone /dev/sdb --output=evidence.dd`  
D. `dcfldd image /dev/sdb -o evidence.dd -verify`  
E. `dcfldd /dev/sdb > evidence.dd | sha256sum`

---

### Soal 14
Dalam konteks akuisisi RAID 5, tantangan utama yang dihadapi investigator forensik adalah:

A. RAID 5 tidak mendukung physical acquisition  
B. Perlu mengetahui strip size, disk order, dan pola parity rotation  
C. RAID 5 menggunakan enkripsi hardware yang tidak bisa didekripsi  
D. Hanya satu disk yang menyimpan data, sisanya cadangan  
E. RAID 5 tidak kompatibel dengan FTK Imager

---

### Soal 15
Logical acquisition cocok digunakan dalam situasi:

A. Investigasi lengkap yang memerlukan deleted file recovery  
B. Analisis slack space dan unallocated space  
C. Triage cepat untuk mengidentifikasi bukti relevan pada live system  
D. Akuisisi HPA (Host Protected Area)  
E. Investigasi malware yang bersembunyi di boot sector

---

### Soal 16
Data yang TIDAK dapat diperoleh melalui memory dump (RAM acquisition) adalah:

A. Encryption key yang sedang aktif  
B. Koneksi jaringan yang sedang berlangsung  
C. Data pada archival tape yang offline  
D. Running processes dan DLL yang dimuat  
E. Plaintext dari file terenkripsi yang sedang dibuka

---

### Soal 17
Keunggulan hardware write blocker dibandingkan software write blocker adalah:

A. Harganya lebih murah  
B. Bekerja independen dari sistem operasi sehingga lebih reliable  
C. Mendukung lebih banyak format file  
D. Kecepatan transfer data lebih lambat  
E. Tidak memerlukan kalibrasi

---

### Soal 18
Sparse acquisition paling tepat digunakan ketika:

A. Investigator memerlukan salinan 100% identik dari media  
B. Disk berkapasitas sangat besar dan fokus investigasi hanya pada area tertentu  
C. Media penyimpanan dalam kondisi rusak total  
D. Investigator tidak memiliki write blocker  
E. Sistem operasi sudah terhapus

---

### Soal 19
Dalam Acquisition Log, informasi yang WAJIB dicantumkan meliputi:

A. Nomor kasus, serial number media, hash value, dan nama examiner  
B. Hanya nama examiner dan tanggal akuisisi  
C. Hanya hash value saja  
D. Nomor IP address server dan MAC address router  
E. Password administrator dan login credentials

---

### Soal 20
Remote acquisition penting dalam konteks militer terutama karena:

A. Seluruh aset digital militer disimpan di satu lokasi terpusat  
B. Aset digital tersebar di berbagai Kodam, Lantamal, dan Lanud dengan jarak berjauhan  
C. Investigator forensik militer tidak dilatih untuk field operation  
D. Remote acquisition tidak memerlukan izin komandan  
E. Koneksi internet di unit militer selalu stabil dan cepat

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Jelaskan mengapa copy-paste file biasa tidak dapat menggantikan akuisisi forensik! Sebutkan minimal tiga perbedaan utama!

---

### Soal 2 ⭐
Sebutkan dan jelaskan lima prinsip dasar akuisisi forensik!

---

### Soal 3 ⭐
Jelaskan perbedaan antara hardware write blocker dan software write blocker! Dalam situasi apa masing-masing lebih tepat digunakan?

---

### Soal 4 ⭐
Jelaskan tiga metode akuisisi forensik (physical, logical, sparse) dan kapan masing-masing metode digunakan!

---

### Soal 5 ⭐⭐
Bandingkan dead acquisition dan live acquisition! Jelaskan keuntungan, kerugian, dan situasi yang tepat untuk masing-masing!

---

### Soal 6 ⭐⭐
Jelaskan Order of Volatility berdasarkan RFC 3227! Mengapa urutan ini penting dalam live acquisition? Berikan contoh data pada setiap tingkat volatilitas!

---

### Soal 7 ⭐⭐
Bandingkan tiga format image forensik utama (Raw/dd, E01, AFF4) dari aspek kompresi, metadata, hash terintegrasi, open source, dan enkripsi! Rekomendasikan format mana yang paling tepat untuk skenario militer dan jelaskan alasannya!

---

### Soal 8 ⭐⭐
Jelaskan mengapa memory forensics (RAM acquisition) sangat penting dalam investigasi modern! Sebutkan empat jenis data yang hanya tersedia di RAM dan tool yang dapat digunakan untuk akuisisi RAM!

---

### Soal 9 ⭐⭐
Jelaskan properti fungsi hash kriptografis yang membuatnya cocok untuk verifikasi integritas dalam forensik! Mengapa SHA-256 direkomendasikan menggantikan MD5?

---

### Soal 10 ⭐⭐
Jelaskan prosedur lengkap akuisisi forensik menggunakan FTK Imager dari awal hingga verifikasi! Sebutkan setiap langkah dan parameter penting yang perlu diperhatikan!

---

### Soal 11 ⭐⭐
Jelaskan lima tantangan akuisisi pada media SSD (TRIM, garbage collection, wear leveling, over-provisioning, hardware encryption) dan dampaknya terhadap data recovery forensik!

---

### Soal 12 ⭐⭐⭐
Seorang investigator menemukan laptop militer dengan SSD NVMe, BitLocker aktif, dan kondisi sleep mode. Jelaskan strategi akuisisi lengkap langkah demi langkah, termasuk urutan prioritas, tools yang digunakan, dan potensi risiko di setiap langkah!

---

### Soal 13 ⭐⭐⭐
Jelaskan tantangan dan prosedur akuisisi forensik pada sistem RAID 5 server militer! Bandingkan pendekatan akuisisi melalui RAID controller vs akuisisi individual disk + rekonstruksi!

---

### Soal 14 ⭐⭐⭐
Desain prosedur operasi standar (SOP) akuisisi forensik untuk unit respons siber militer yang mencakup persiapan toolkit, prosedur di lapangan, verifikasi, dan dokumentasi! Pertimbangkan kendala operasional di medan!

---

### Soal 15 ⭐⭐⭐
Seorang pengacara pembela menantang validitas bukti digital dengan argumen: (1) tidak ada hardware write blocker yang digunakan, (2) hanya MD5 yang digunakan untuk hashing, dan (3) acquisition log tidak lengkap. Analisis setiap argumen dan jelaskan bagaimana investigator seharusnya mempersiapkan diri untuk menghadapi tantangan semacam ini!

---

## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Intrusi Server Kodam XIV/Hasanuddin

**Latar Belakang:**

Tim CSIRT Kodam XIV/Hasanuddin mendeteksi aktivitas mencurigakan pada server utama data personel militer pada tanggal 20 Januari 2026 pukul 02:30 WIT. Sistem IDS mencatat anomali berupa koneksi outbound ke IP asing yang tidak dikenal. Server tersebut masih berjalan (live) dan menyimpan data sensitif personel termasuk data penugasan dan riwayat dinas.

**Informasi Teknis:**
- Server: Dell PowerEdge R750 dengan RAID 5 (4 × 2TB SAS HDD)
- OS: Windows Server 2022
- Enkripsi: BitLocker aktif pada volume data
- RAM: 128 GB
- Sistem masih berjalan dan melayani operasional Kodam
- Koneksi jaringan masih aktif
- Backup terakhir dilakukan 3 hari sebelumnya

**Tugas:**

1. **Prioritasi (15 poin):** Tentukan urutan akuisisi berdasarkan Order of Volatility! Jelaskan alasan urutan yang Anda pilih dan data apa yang akan diperoleh dari setiap tahap!

2. **Strategi Akuisisi (20 poin):** Jelaskan strategi akuisisi lengkap mencakup:
   - Keputusan live vs dead acquisition dan justifikasinya
   - Tool yang digunakan untuk setiap tahap
   - Format image yang dipilih dan alasannya
   - Penanganan RAID 5 dan BitLocker
   - Langkah verifikasi integritas

3. **Risiko dan Mitigasi (15 poin):** Identifikasi minimal lima risiko yang mungkin terjadi selama proses akuisisi dan jelaskan langkah mitigasi untuk masing-masing!

4. **Dokumentasi (15 poin):** Buatlah template Acquisition Log lengkap untuk kasus ini, termasuk semua field yang diperlukan untuk memenuhi standar admissibility bukti!

---

### Studi Kasus 2: Operasi Triage di Lokasi Penggerebekan

**Latar Belakang:**

Tim forensik militer diterjunkan ke sebuah lokasi yang digunakan sebagai pusat komunikasi ilegal yang diduga mendukung aktivitas terorisme. Operasi penggerebekan berhasil mengamankan lokasi pada tanggal 25 Januari 2026 pukul 06:00 WIB. Komandan lapangan memberikan waktu maksimal 3 jam sebelum lokasi harus dikosongkan karena pertimbangan keamanan.

**Perangkat yang Ditemukan:**
1. 2 × Laptop (1 menyala dalam sleep mode, 1 mati) — keduanya SSD NVMe
2. 1 × Desktop PC (menyala, layar menampilkan aplikasi chat terenkripsi) — HDD 2TB
3. 3 × Smartphone (2 Android, 1 iPhone) — semua dalam keadaan menyala
4. 5 × USB flash drive (berbagai kapasitas: 8GB-128GB)
5. 2 × External HDD (1TB dan 2TB)
6. 1 × MicroSD card 256GB
7. 1 × Router Wi-Fi dengan log masih aktif

**Peralatan Forensik yang Tersedia:**
- 1 × Forensic laptop dengan FTK Imager dan RAM capture tools
- 1 × Hardware write blocker (USB 3.0 dan SATA)
- 3 × External HDD kosong (4TB, 4TB, 2TB)
- Evidence bags, label, dan kamera dokumentasi
- 1 × USB hub dengan 4 port

**Tugas:**

1. **Perencanaan Triage (20 poin):** Buat timeline akuisisi menit-per-menit untuk 3 jam yang tersedia! Tentukan urutan prioritas perangkat dan justifikasinya berdasarkan:
   - Volatilitas data
   - Kemungkinan mengandung bukti kritis
   - Waktu yang diperlukan untuk akuisisi
   - Keterbatasan peralatan

2. **Strategi per Perangkat (20 poin):** Untuk setiap kategori perangkat, jelaskan:
   - Metode akuisisi yang dipilih (physical/logical/memory)
   - Tool dan prosedur spesifik
   - Format image yang digunakan
   - Estimasi waktu akuisisi

3. **Penanganan Perangkat Tidak Sempat (10 poin):** Untuk perangkat yang mungkin tidak sempat diakuisisi dalam 3 jam, jelaskan prosedur seize fisik yang benar termasuk packaging, labeling, dan transportasi!

4. **Dokumentasi Lapangan (15 poin):** Buatlah checklist dokumentasi lapangan yang harus diisi oleh tim forensik selama operasi triage! Sertakan field untuk foto, sketsa, identifikasi perangkat, dan chain of custody awal!

---

## Kunci Jawaban

### Bagian A: Pilihan Ganda

| No | Jawaban | Penjelasan |
|----|---------|------------|
| 1 | **B** | Akuisisi forensik membuat salinan bitwise dari seluruh media, termasuk deleted data, slack space, dan area tersembunyi — copy biasa hanya menyalin file aktif |
| 2 | **C** | Write blocker mencegah penulisan data ke media bukti, menjaga integritas data asli agar tidak berubah selama proses akuisisi |
| 3 | **C** | Physical acquisition menyalin seluruh isi media secara bit-by-bit termasuk semua area: partisi, unallocated space, HPA, dan DCO |
| 4 | **D** | E01 adalah format proprietary dari Guidance Software (OpenText), bukan open source. AFF4 yang bersifat open source |
| 5 | **B** | Urutan RFC 3227: CPU Register (paling volatile) → RAM → Network State → Running Processes → Disk → Archival Media (paling stabil) |
| 6 | **C** | Live acquisition wajib saat FDE aktif karena encryption key hanya ada di RAM — shutdown menyebabkan key hilang dan data terkunci permanen |
| 7 | **D** | AFF4 adalah satu-satunya format yang mendukung kompresi, metadata, DAN enkripsi sekaligus. E01 tidak mendukung enkripsi, raw tidak mendukung ketiganya |
| 8 | **D** | SHA-256 adalah standar yang direkomendasikan saat ini. MD5 dan SHA-1 sudah deprecated karena collision vulnerability |
| 9 | **C** | Akuisisi RAM harus dilakukan pertama untuk mendapatkan BitLocker FVEK sebelum sistem dimatikan — mematikan = key hilang permanen |
| 10 | **C** | TRIM memberi tahu SSD controller untuk menghapus blok tidak terpakai, dan garbage collection menghapus data secara background tanpa perintah OS |
| 11 | **D** | Wireshark adalah tool analisis network traffic (packet capture), bukan tool akuisisi RAM. Yang lain semua adalah RAM capture tools |
| 12 | **B** | Hash mismatch berarti integritas image diragukan — data mungkin termodifikasi dan bukti kemungkinan besar ditolak pengadilan |
| 13 | **A** | Sintaks dcfldd yang benar: `dcfldd if=[sumber] of=[tujuan] hash=[algoritma]` untuk akuisisi dengan hashing otomatis |
| 14 | **B** | RAID 5 menggunakan data striping + distributed parity, sehingga investigator perlu mengetahui strip size, disk order, dan parity rotation untuk rekonstruksi |
| 15 | **C** | Logical acquisition cocok untuk triage cepat karena hanya menyalin file aktif — cepat dan memberikan gambaran awal bukti yang ada |
| 16 | **C** | Data pada archival tape offline tidak bisa diperoleh melalui RAM dump — hanya data yang sedang aktif di memori yang dapat di-capture |
| 17 | **B** | Hardware write blocker bekerja pada level hardware/firmware, independen dari OS — tidak terpengaruh bug OS atau driver vulnerability |
| 18 | **B** | Sparse acquisition ideal untuk disk berkapasitas besar dengan fokus investigasi pada area atau tipe file tertentu saja |
| 19 | **A** | Acquisition log wajib mencantumkan nomor kasus, identifikasi media (S/N), hash value (pre dan post), nama examiner, tanggal/waktu, tool, dan metode |
| 20 | **B** | Aset digital militer tersebar di berbagai satuan (Kodam, Lantamal, Lanud) dengan jarak berjauhan, sehingga remote acquisition menjadi kritis |

---

### Bagian B: Uraian

#### Jawaban Soal 1 ⭐

**Perbedaan Copy Biasa dan Akuisisi Forensik:**

| Aspek | Copy Biasa | Akuisisi Forensik |
|-------|-----------|-------------------|
| **Cakupan** | Hanya file dan folder yang terlihat oleh OS | Seluruh sektor disk termasuk deleted files, slack space, HPA, DCO |
| **Metode** | Copy-paste melalui file system | Bit-by-bit duplication pada level raw device |
| **Verifikasi** | Tidak ada mekanisme verifikasi integritas | Hash cryptographic (SHA-256) untuk membuktikan salinan identik |
| **Metadata** | Timestamp berubah (created, modified, accessed) | Metadata asli terjaga karena menggunakan write blocker |
| **Admissibility** | Tidak diterima sebagai bukti di pengadilan | Diterima sebagai bukti karena proses terdokumentasi dan terverifikasi |

Copy biasa mengubah metadata file (seperti access time) dan tidak mencakup area yang tidak terlihat oleh OS, sehingga bukti penting seperti deleted files, file fragments di slack space, dan data di unallocated space hilang.

---

#### Jawaban Soal 2 ⭐

**Lima Prinsip Dasar Akuisisi Forensik:**

1. **Keutuhan Bukti (Evidence Integrity):** Data asli tidak boleh berubah selama proses akuisisi. Menggunakan write blocker (hardware atau software) untuk mencegah modifikasi. Setiap tindakan harus meminimalkan dampak terhadap bukti.

2. **Verifikasi (Verification):** Menggunakan hash cryptographic (SHA-256) untuk membuktikan bahwa forensic image identik dengan media sumber. Hash dihitung sebelum dan sesudah akuisisi, keduanya harus cocok.

3. **Dokumentasi (Documentation):** Seluruh proses akuisisi harus tercatat dalam acquisition log termasuk waktu, tool, metode, serial number media, dan hash value. Dokumentasi menjadi bukti bahwa proses dilakukan dengan benar.

4. **Repeatability:** Proses harus dapat diulangi oleh investigator lain dengan menghasilkan output yang sama (untuk dead acquisition). Ini memenuhi persyaratan scientific method dan peer review.

5. **Chain of Custody:** Dokumentasi kronologis tentang siapa yang menangani bukti, kapan, dan untuk tujuan apa, dari pengambilan hingga presentasi di pengadilan.

---

#### Jawaban Soal 3 ⭐

**Hardware Write Blocker:**
- Perangkat fisik yang dipasang antara media bukti dan komputer forensik
- Bekerja pada level hardware/firmware, independen dari OS
- Keunggulan: lebih reliable, tidak terpengaruh bug OS atau malware
- Kelemahan: mahal ($200-500+), memerlukan model berbeda untuk setiap interface (SATA, USB, NVMe)
- Tepat digunakan: investigasi formal di lab forensik, kasus hukum yang memerlukan standar tertinggi

**Software Write Blocker:**
- Utilitas software yang mengintercepti perintah write pada level OS
- Bergantung pada sistem operasi dan driver yang berjalan
- Keunggulan: gratis atau murah, fleksibel, mudah diupdate
- Kelemahan: bergantung OS (jika OS crash, proteksi bisa hilang), kurang dipercaya di pengadilan
- Tepat digunakan: triage lapangan, situasi darurat saat hardware write blocker tidak tersedia, latihan

---

#### Jawaban Soal 4 ⭐

**Tiga Metode Akuisisi Forensik:**

1. **Physical Acquisition:** Menyalin seluruh media penyimpanan secara bit-by-bit termasuk semua partisi, unallocated space, slack space, HPA, dan DCO. Menghasilkan salinan identik dari media asli. Digunakan untuk investigasi lengkap yang memerlukan recovery deleted files dan analisis mendalam.

2. **Logical Acquisition:** Menyalin file dan folder yang terlihat oleh sistem operasi saja, tanpa deleted data atau unallocated space. Lebih cepat dan menghasilkan image berukuran lebih kecil. Digunakan untuk triage cepat, live system yang tidak boleh dimatikan, dan ketika fokus investigasi sudah jelas.

3. **Sparse Acquisition:** Menyalin area tertentu dari media berdasarkan kriteria spesifik (rentang sektor, tipe file, atau lokasi tertentu). Merupakan kompromi antara physical dan logical. Digunakan untuk disk berkapasitas sangat besar dengan fokus investigasi pada area tertentu saja.

---

#### Jawaban Soal 5 ⭐⭐

**Perbandingan Dead Acquisition dan Live Acquisition:**

| Aspek | Dead Acquisition | Live Acquisition |
|-------|-----------------|------------------|
| **Kondisi** | Sistem mati, disk dilepas | Sistem masih berjalan |
| **Keuntungan** | Konsisten, repeatable, integritas tinggi, risiko kontaminasi rendah | Volatile data terjaga, encryption key di RAM tersedia, aktifitas real-time tercapture |
| **Kerugian** | Volatile data hilang, encrypted volume terkunci | Integritas lebih rendah, tidak fully repeatable, proses OS tetap berjalan mengubah state |
| **Repeatability** | Tinggi — hash selalu konsisten | Rendah — state sistem berubah tiap detik |

**Situasi untuk Dead Acquisition:**
- Media tidak terenkripsi
- Volatile data tidak diperlukan
- Investigasi memerlukan standar integritas tertinggi
- Investigator memiliki waktu cukup

**Situasi untuk Live Acquisition:**
- Full Disk Encryption aktif (BitLocker, VeraCrypt) — key di RAM
- Sistem kritis yang tidak boleh dimatikan (server C2 militer)
- Diperlukan bukti volatile: fileless malware, koneksi C2 aktif, proses mencurigakan
- Sistem menggunakan cloud storage yang terkunci jika offline

---

#### Jawaban Soal 6 ⭐⭐

**Order of Volatility (RFC 3227):**

| Urutan | Sumber Data | Volatilitas | Contoh Data |
|--------|-------------|-------------|-------------|
| 1 | CPU Register/Cache | Sangat tinggi (nanoseconds) | Instruction pointer, temporary values |
| 2 | RAM (Memory) | Tinggi (hilang saat shutdown) | Encryption keys, running processes, network connections, malware injected |
| 3 | Network State | Tinggi (berubah real-time) | ARP cache, routing table, active connections (netstat) |
| 4 | Running Processes | Tinggi (berubah terus) | Process list, open files, loaded DLLs |
| 5 | Disk / Storage | Rendah (persisten) | Files, logs, registry, databases |
| 6 | Archival Media | Sangat rendah (stabil) | Backup tapes, optical media, offline storage |

**Pentingnya urutan ini dalam live acquisition:**
- Data paling volatile harus diakuisisi pertama karena dapat hilang sewaktu-waktu
- RAM berisi data kritis yang tidak tersedia di disk (encryption keys, fileless malware)
- Jika investigator mulai dari disk, volatile data di RAM mungkin sudah berubah/hilang
- Kesalahan urutan dapat menyebabkan hilangnya bukti kritis yang tidak dapat dipulihkan

---

#### Jawaban Soal 7 ⭐⭐

**Perbandingan Format Image Forensik:**

| Fitur | Raw (dd) | E01 (EnCase) | AFF4 |
|-------|---------|-------------|------|
| Kompresi | Tidak | Ya (zlib) | Ya (multi: snappy, zlib, lz4) |
| Metadata | Tidak | Ya (case info, examiner) | Ya (extensible, RDF) |
| Hash terintegrasi | Tidak | MD5/SHA-1 per block | SHA-256+ |
| Open source | Ya | Tidak (proprietary) | Ya |
| Enkripsi | Tidak | Tidak | Ya |
| Kompatibilitas | Sangat tinggi (universal) | Tinggi (semua tool forensik) | Sedang (growing) |
| Split file | Ya (.001, .002) | Ya (auto-split) | Ya (multiple streams) |

**Rekomendasi untuk Skenario Militer:**
- **E01** sebagai format utama: kompresi mengurangi kebutuhan storage (penting di lapangan), metadata kasus terintegrasi, kompatibilitas tinggi dengan tools forensik standar
- **AFF4** untuk kasus yang memerlukan enkripsi image (data classified), digital signatures untuk autentikasi, dan interoperabilitas open source
- Simpan **raw** sebagai backup jika diperlukan mounting langsung atau tools khusus

---

#### Jawaban Soal 8 ⭐⭐

**Pentingnya Memory Forensics:**

Memory forensics kritis karena banyak bukti penting hanya ada di RAM dan hilang permanen saat sistem dimatikan. Dalam investigasi modern, penyerang semakin banyak menggunakan teknik "living off the land" dan fileless attack yang tidak meninggalkan jejak di disk.

**Empat Jenis Data yang Hanya Tersedia di RAM:**

1. **Encryption Keys:** BitLocker Full Volume Encryption Key (FVEK), VeraCrypt master key, Wi-Fi passwords, TLS session keys. Tanpa key ini, data terenkripsi tidak dapat diakses.

2. **Fileless Malware:** Kode malware yang hanya berjalan di memori (PowerShell in-memory execution, DLL injection, process hollowing) tanpa pernah ditulis ke disk. Restart = malware hilang tanpa jejak.

3. **Network Connections:** Koneksi C2 (Command & Control) aktif, remote IP dan port, session data. Informasi ini kritis untuk attribution dan menentukan scope serangan.

4. **Decrypted Data:** File terenkripsi yang sedang dibuka tersimpan dalam bentuk plaintext di RAM. Menutup file = plaintext hilang dari memori.

**Tools Akuisisi RAM:**
- **Belkasoft RAM Capturer** (Windows, gratis, user-mode, minimal footprint)
- **WinPMEM** (Windows, open source, kernel driver via Rekall project)
- **FTK Imager** (Windows, memory capture terintegrasi dalam tool imaging)
- **DumpIt** (Windows, portable, single-click execution)
- **LiME** (Linux, kernel module, open source)

---

#### Jawaban Soal 9 ⭐⭐

**Properti Hash Kriptografis untuk Forensik:**

1. **Deterministic:** Input yang sama selalu menghasilkan output yang sama. Ini memungkinkan verifikasi berulang — siapa pun yang menghitung hash dari image yang sama akan mendapatkan hasil identik.

2. **One-way (Pre-image Resistance):** Tidak mungkin membalikkan hash digest ke input asli. Ini menjamin bahwa hash tidak mengungkapkan konten data.

3. **Collision Resistance:** Sangat sulit menemukan dua input berbeda yang menghasilkan hash sama. Ini menjamin keunikan — jika hash cocok, data identik.

4. **Avalanche Effect:** Perubahan 1 bit pada input menghasilkan hash yang sangat berbeda (~50% bit berubah). Ini memastikan bahkan modifikasi terkecil terdeteksi.

**Mengapa SHA-256 menggantikan MD5:**
- **MD5 collision ditemukan (2004-2008):** Penelitian oleh Wang et al. menunjukkan collision dapat ditemukan dalam waktu singkat, memungkinkan pemalsuan integritas
- **SHA-1 juga compromised (2017):** Google Project SHAttered membuktikan practical collision
- **SHA-256 masih aman:** Belum ada practical collision attack yang ditemukan, output 256-bit memberikan ruang kemungkinan yang sangat besar (2^256)
- **Praktik terbaik:** Gunakan SHA-256 sebagai standar utama + MD5 untuk backward compatibility dengan database hash legacy

---

#### Jawaban Soal 10 ⭐⭐

**Prosedur Akuisisi dengan FTK Imager:**

1. **Persiapan:**
   - Koneksikan media bukti ke forensic workstation melalui hardware write blocker
   - Verifikasi write blocker aktif (indicator LED)
   - Siapkan media tujuan dengan kapasitas cukup (minimal sama dengan media sumber + 10%)

2. **Identifikasi Media:**
   - Buka FTK Imager → File → Add Evidence Item
   - Pilih "Physical Drive" untuk physical acquisition
   - Verifikasi drive yang dipilih benar (periksa model, serial number, kapasitas)

3. **Konfigurasi Image:**
   - File → Create Disk Image → Physical Drive
   - Pilih format output (E01 direkomendasikan)
   - Isi metadata kasus: Case Number, Evidence Number, Unique Description, Examiner Name, Notes
   - Tentukan lokasi penyimpanan dan split size (jika diperlukan)

4. **Opsi Penting:**
   - Centang "Verify images after they are created" (wajib!)
   - Pilih compression level (biasanya default/normal)
   - Centang "Precalculate Progress Statistics"

5. **Eksekusi dan Monitoring:**
   - Klik Start → Monitoring progress bar dan estimasi waktu
   - Jangan mengganggu proses — jangan cabut kabel atau matikan komputer
   - Catat waktu mulai dan selesai

6. **Verifikasi:**
   - Setelah selesai, FTK Imager otomatis menghitung hash (MD5 + SHA-1)
   - Bandingkan hash sumber dengan hash image — harus IDENTIK
   - Catat hash value dalam acquisition log

7. **Dokumentasi:**
   - Screenshot hasil verifikasi
   - Isi acquisition log lengkap
   - Simpan hash file (.txt) bersama image

---

#### Jawaban Soal 11 ⭐⭐

**Lima Tantangan Akuisisi SSD:**

1. **TRIM:** Perintah dari OS yang memberitahu SSD controller bahwa blok data tidak lagi digunakan. Controller kemudian menghapus blok tersebut saat idle, menghilangkan kemungkinan recovery. Dampak: deleted files pada SSD dengan TRIM aktif hampir mustahil dipulihkan, berbeda dengan HDD di mana deleted files masih ada di unallocated space.

2. **Garbage Collection:** Proses internal SSD controller yang menghapus dan mengkonsolidasi blok data di background tanpa perintah dari OS. Ini berjalan saat SSD idle. Dampak: data dapat terhapus bahkan tanpa aksi pengguna atau OS — semakin lama SSD dibiarkan menyala, semakin banyak data yang hilang.

3. **Wear Leveling:** Controller SSD memindahkan data antar blok secara otomatis untuk meratakan penggunaan sel NAND, memperpanjang umur SSD. Dampak: lokasi fisik data berubah tanpa sepengetahuan OS, sehingga analisis level sektor menjadi tidak konsisten dan physical imaging berulang mungkin menghasilkan hash berbeda.

4. **Over-provisioning:** Area cadangan yang dialokasikan oleh manufacturer (7-28% dari total kapasitas) untuk wear leveling dan penggantian bad blocks. Area ini tidak visible oleh OS atau tools standar. Dampak: data sensitif yang dipindahkan ke area ini tidak bisa diakses dengan tools forensik konvensional.

5. **Hardware Encryption (Self-Encrypting Drive/SED):** Beberapa SSD memiliki enkripsi hardware transparan yang aktif secara default. Dampak: jika ATA password terlupakan atau firmware dimodifikasi, data tidak bisa didekripsi meskipun berhasil di-image secara physical.

---

#### Jawaban Soal 12 ⭐⭐⭐

**Strategi Akuisisi Laptop Militer (SSD NVMe + BitLocker + Sleep Mode):**

**Langkah 1 — Dokumentasi Awal (5 menit):**
- Foto kondisi laptop: layar, indikator LED, kabel terhubung
- Catat model laptop, serial number, posisi saat ditemukan
- JANGAN tutup lid, JANGAN tekan tombol apapun yang bisa trigger shutdown

**Langkah 2 — Akuisisi RAM (15-20 menit):**
- Tool: Belkasoft RAM Capturer atau DumpIt dari USB external
- Colokkan USB berisi tool ke laptop (USB yang sudah prepared)
- Jalankan RAM Capturer → pilih output ke USB terpisah
- Output: file .mem sebesar RAM (misalnya 16GB)
- Risiko: tool mungkin crash jika RAM terlalu besar, atau antivirus memblokir kernel access
- Mitigasi: gunakan tool alternatif (WinPMEM) jika yang pertama gagal

**Langkah 3 — Ekstraksi BitLocker Key dari RAM (di lab):**
- Tool: Volatility 3 dengan plugin `windows.bitlocker.Bitlocker`
- Ekstraksi Full Volume Encryption Key (FVEK) dari memory dump
- Simpan FVEK secara aman

**Langkah 4 — Evaluasi Keputusan Shutdown:**
- Jika FVEK berhasil diekstraksi → shutdown laptop aman dilakukan
- Jika FVEK gagal diekstraksi → pertimbangkan logical acquisition dalam keadaan live
- Risiko shutdown: key hilang, tapi jika FVEK sudah disimpan, ini bukan masalah

**Langkah 5 — Physical Acquisition SSD (30-60 menit):**
- Lepaskan SSD NVMe dari laptop
- Koneksikan ke forensic workstation via NVMe-to-USB adapter + write blocker
- Tool: FTK Imager → Create Disk Image → Physical Drive → format E01
- Image akan terenkripsi BitLocker
- Verifikasi hash image

**Langkah 6 — Dekripsi dan Analisis:**
- Mount image menggunakan Arsenal Image Mounter
- Decrypt dengan FVEK yang diekstraksi dari RAM
- Lakukan analisis pada volume terdekripsi

**Langkah 7 — Dokumentasi Lengkap:**
- Acquisition log untuk RAM dump + disk image
- Chain of custody untuk laptop, SSD, dan USB containing tools
- Screenshot setiap tahap proses

---

#### Jawaban Soal 13 ⭐⭐⭐

**Akuisisi Forensik RAID 5 Server Militer:**

**Tantangan RAID 5:**
- Data di-stripe across multiple disks dengan distributed parity
- Perlu mengetahui: strip size, disk order, parity rotation pattern
- Melepas disk dari controller dapat menyebabkan RAID rebuild yang merusak data
- Server militer mungkin tidak boleh dimatikan

**Pendekatan 1: Akuisisi melalui RAID Controller (Recommended):**

Keuntungan:
- RAID controller menyajikan volume logical yang sudah terekonstruksi
- Investigator melihat data seperti yang dilihat OS — lebih mudah dianalisis
- Tidak perlu mengetahui RAID parameters

Prosedur:
1. Live acquisition: gunakan FTK Imager pada volume logical yang disajikan controller
2. Format E01 dengan kompresi
3. Verifikasi hash

Kelemahan:
- Tidak mendapatkan raw data per disk
- Jika controller rusak, image tidak berguna
- Tidak bisa mengakses data di luar volume (deleted partitions)

**Pendekatan 2: Image Individual Disk + Rekonstruksi:**

Keuntungan:
- Mendapatkan raw data dari setiap disk
- Bisa mengakses data di luar RAID volume
- Tidak bergantung pada RAID controller

Prosedur:
1. Catat konfigurasi RAID: strip size, disk order, parity pattern
2. Shutdown server (jika diizinkan) → lepas setiap disk
3. Image setiap disk secara individual (physical acquisition)
4. Rekonstruksi RAID di lab menggunakan tools: R-Studio, X-Ways, atau mdadm

Kelemahan:
- Memerlukan pengetahuan RAID parameters yang tepat
- Proses rekonstruksi bisa gagal jika parameter salah
- Lebih lama dan kompleks

**Rekomendasi untuk Militer:** Lakukan KEDUA pendekatan jika waktu memungkinkan — akuisisi logical melalui controller sebagai primary, lalu image individual disk sebagai backup.

---

#### Jawaban Soal 14 ⭐⭐⭐

**SOP Akuisisi Forensik Unit Respons Siber Militer:**

**Fase 1 — Persiapan Toolkit (Sebelum Deployment):**
- Forensic laptop tercharge penuh + charger
- Hardware write blocker (SATA, USB, NVMe)
- External HDD terformat dan terverifikasi kosong (minimal 3 unit berbeda kapasitas)
- USB forensic toolkit: FTK Imager portable, RAM capture tools (Belkasoft, WinPMEM, DumpIt), Sysinternals Suite
- Evidence bags (berbagai ukuran), label anti-tamper, spidol permanen
- Kamera dokumentasi dengan memori kosong
- Form chain of custody dan acquisition log (cetak)
- Sarung tangan anti-statik
- Checklist prosedur (laminated)
- Toolkit obeng untuk membuka casing komputer/laptop

**Fase 2 — Prosedur di Lapangan:**
1. **Amankan lokasi:** koordinasi dengan tim operasi, pastikan keamanan fisik
2. **Dokumentasi visual:** foto seluruh area, setiap perangkat, kondisi layar, kabel, posisi
3. **Identifikasi perangkat:** catat model, serial number, kondisi (menyala/mati/sleep)
4. **Klasifikasi prioritas:** berdasarkan volatilitas dan kemungkinan bukti
5. **Akuisisi volatile data:** RAM dump dari semua sistem yang menyala
6. **Akuisisi removable media:** USB, SD card via write blocker
7. **Akuisisi disk:** physical atau logical sesuai kondisi
8. **Seize perangkat yang belum selesai:** packaging dengan evidence bag

**Fase 3 — Verifikasi:**
- Hitung hash (SHA-256 + MD5) untuk setiap image
- Bandingkan hash pre dan post acquisition
- Catat semua hash di acquisition log
- Backup image ke media terpisah jika memungkinkan

**Fase 4 — Dokumentasi:**
- Isi acquisition log untuk setiap bukti
- Isi chain of custody form
- Transfer evidence ke lab dengan dokumentasi serah terima
- Briefing singkat ke komandan tentang bukti yang diamankan

**Kendala Operasional di Medan:**
- Keterbatasan listrik → siapkan power bank/generator portable
- Koneksi network buruk → akuisisi lokal, tidak bergantung internet
- Waktu terbatas → prioritaskan volatile data dan media kecil
- Kondisi lingkungan ekstrem → proteksi peralatan dari debu, air, panas

---

#### Jawaban Soal 15 ⭐⭐⭐

**Analisis Tantangan Hukum terhadap Bukti Digital:**

**Argumen 1: Tidak ada hardware write blocker:**

Dampak: tanpa write blocker, OS mungkin telah memodifikasi media bukti (timestamp update, journal write, antivirus scan). Ini meragukan integritas bukti asli.

Analisis: argumen ini KUAT dan merupakan kelemahan serius. Meskipun software write blocker mungkin digunakan, hardware write blocker memberikan perlindungan yang lebih dipercaya secara hukum karena independen dari OS.

Respons investigator:
- Dokumentasikan jika software write blocker digunakan sebagai alternatif dan jelaskan alasannya
- Tunjukkan bahwa hash verification menunjukkan konsistensi
- Jika memang tidak ada write blocker sama sekali, akui keterbatasan dan jelaskan circumstance (misalnya live acquisition di medan operasi)

**Argumen 2: Hanya MD5 yang digunakan:**

Dampak: MD5 sudah deprecated karena collision vulnerability (Wang et al., 2004). Pengacara bisa berargumen bahwa data mungkin telah dimanipulasi dengan collision attack.

Analisis: argumen ini CUKUP KUAT secara teoritis, meskipun practical preimage attack (memodifikasi evidence sehingga menghasilkan hash MD5 yang sama) masih sangat sulit. Namun, standar profesional sudah mensyaratkan SHA-256.

Respons investigator:
- Idealnya selalu gunakan SHA-256 + MD5 (dual hashing)
- Jelaskan bahwa collision attack pada evidence forensik sangat impractical
- Untuk kasus mendatang, pastikan SHA-256 selalu digunakan sebagai standar

**Argumen 3: Acquisition log tidak lengkap:**

Dampak: acquisition log yang tidak lengkap melemahkan chain of custody dan mempertanyakan profesionalisme investigator. Tanpa dokumentasi lengkap, sulit membuktikan bahwa proses dilakukan dengan benar.

Analisis: argumen ini SANGAT KUAT karena documentation trail adalah fondasi admissibility bukti digital. ISO 27037 dan NIST SP 800-86 mensyaratkan dokumentasi komprehensif.

Respons investigator:
- Selalu gunakan template acquisition log terstandarisasi
- Isi SEMUA field tanpa pengecualian: nomor kasus, tanggal/waktu, examiner, tool + versi, serial number media, metode, format, hash source, hash image, status match
- Review acquisition log sebelum menyelesaikan proses
- Simpan foto dan screenshot sebagai bukti tambahan

**Persiapan Menghadapi Tantangan Serupa:**
1. SOP terstandarisasi untuk setiap akuisisi
2. Training reguler untuk investigator
3. Peer review terhadap proses dan dokumentasi
4. Hardware write blocker wajib dalam toolkit
5. Dual hashing (SHA-256 + MD5) sebagai standar minimum
6. Template acquisition log yang lengkap dan teraudit

---

### Bagian C: Studi Kasus

#### Studi Kasus 1: Intrusi Server Kodam XIV/Hasanuddin

**1. Prioritasi Berdasarkan Order of Volatility (15 poin):**

| Urutan | Target | Alasan | Data yang Diperoleh |
|--------|--------|--------|---------------------|
| 1 | RAM (128 GB) | Paling volatile, hilang saat shutdown, berisi key BitLocker | Encryption keys, active processes, network connections, fileless malware, injected DLLs |
| 2 | Network State | Koneksi ke IP asing masih aktif | Active connections (netstat), routing table, ARP cache, DNS cache — kritis untuk identifikasi C2 server |
| 3 | Running Processes | Proses mencurigakan masih berjalan | Process list, open files, loaded modules, command history |
| 4 | Disk/Storage (RAID 5) | Persisten tapi perlu BitLocker key dari RAM | Log files, malware artifacts, exfiltrated data indicators, event logs |
| 5 | Backup (3 hari lalu) | Sebagai baseline untuk perbandingan | Clean state sebelum intrusi untuk comparison analysis |

Urutan ini memastikan data paling volatile diamankan terlebih dahulu, terutama RAM yang berisi BitLocker key dan bukti koneksi C2 yang sedang berlangsung.

**2. Strategi Akuisisi (20 poin):**

**Keputusan: Live Acquisition WAJIB** karena:
- BitLocker aktif → key hanya di RAM → shutdown = data terkunci
- Server masih melayani operasional Kodam → downtime tidak diinginkan
- Koneksi C2 masih aktif → bukti network volatile

**Tahap-tahap:**

a) **RAM Acquisition (128 GB, ~15-20 menit):**
- Tool: WinPMEM (kernel mode, lebih reliable untuk server) atau Belkasoft RAM Capturer
- Output ke external HDD terpisah (format raw .mem)
- Verifikasi: hash SHA-256 dari file dump

b) **Network State Capture (5 menit):**
- Tool: netstat -anob, arp -a, ipconfig /all, route print
- Simpan output ke file teks di external media
- Capture network traffic: Wireshark/tcpdump pada interface server (jika memungkinkan)

c) **Process/System State Capture (10 menit):**
- Tool: Sysinternals (ProcMon, ProcExp, Autoruns)
- Command: tasklist /V, wmic process list full
- Event Log export: wevtutil epl System, Security, Application

d) **Disk Acquisition melalui RAID Controller (4-8 jam):**
- Tool: FTK Imager → Physical Drive (RAID logical volume)
- Format: E01 dengan kompresi
- Karena server harus tetap hidup, gunakan live imaging pada logical volume
- Alternatif: VSS (Volume Shadow Copy) snapshot jika tersedia

e) **BitLocker Handling:**
- Ekstraksi FVEK dari RAM dump menggunakan Volatility 3 (plugin bitlocker)
- Simpan key secara aman (encrypted storage)
- Gunakan key untuk dekripsi image di lab

f) **Verifikasi:**
- Hash SHA-256 + MD5 untuk setiap image
- Bandingkan hash untuk konsistensi
- Dokumentasikan semua hash value

**3. Risiko dan Mitigasi (15 poin):**

| No | Risiko | Mitigasi |
|----|--------|----------|
| 1 | RAM dump gagal karena anti-cheat/security software memblokir kernel access | Siapkan multiple tools (WinPMEM, Belkasoft, DumpIt). Disable endpoint protection jika diizinkan |
| 2 | Server crash selama live acquisition menyebabkan semua volatile data hilang | Prioritaskan RAM dump first. Jika crash, segera lakukan dead acquisition pada RAID |
| 3 | Penyerang mendeteksi aktivitas forensik dan melakukan anti-forensics (wipe) | Isolasi server dari jaringan (jika diizinkan) setelah network state capture. Monitor aktivitas real-time |
| 4 | Kapasitas media tujuan tidak cukup untuk 8TB RAID + 128GB RAM | Siapkan minimal 2× kapasitas total. Gunakan E01 kompresi. Bawa cadangan media |
| 5 | BitLocker key tidak ditemukan di RAM dump (key sudah di-flush) | Cek BitLocker Recovery Key di Active Directory. Hubungi admin IT Kodam untuk recovery key backup |
| 6 | Live imaging pada RAID menyebabkan penurunan performa server | Lakukan di luar jam operasional peak. Informasikan ke admin IT untuk monitoring performa |

**4. Template Acquisition Log (15 poin):**

```
╔══════════════════════════════════════════════════╗
║         FORENSIC ACQUISITION LOG                  ║
╠══════════════════════════════════════════════════╣
║ Case Number    : KODAM-XIV/CSIRT/2026-001         ║
║ Evidence Item  : EV-001 (RAM Dump)                ║
║ Date/Time Start: 2026-01-20 03:15 WIT             ║
║ Date/Time End  : 2026-01-20 03:35 WIT             ║
║ Examiner       : [Nama Investigator / NIP]        ║
║ Witness        : [Nama Saksi / Jabatan]           ║
║                                                    ║
║ SOURCE INFORMATION:                                ║
║ Device Type    : Server Dell PowerEdge R750        ║
║ Serial Number  : [S/N dari label server]           ║
║ OS             : Windows Server 2022               ║
║ RAM Capacity   : 128 GB                            ║
║ Status         : Running (Live)                    ║
║                                                    ║
║ ACQUISITION DETAILS:                               ║
║ Method         : Live RAM Acquisition              ║
║ Tool           : WinPMEM v4.0.rc1                  ║
║ Format         : Raw memory dump (.mem)            ║
║ Output File    : KODAM14_RAM_20260120.mem          ║
║ Output Size    : 128,849,018,880 bytes             ║
║ Destination    : Seagate External 4TB (S/N: xxx)   ║
║                                                    ║
║ HASH VERIFICATION:                                 ║
║ SHA-256 (image): [64 character hex string]         ║
║ MD5 (image)    : [32 character hex string]         ║
║ Note           : Source hash N/A (live RAM dump)   ║
║                                                    ║
║ NOTES:                                             ║
║ Server tetap berjalan selama akuisisi.             ║
║ BitLocker aktif pada volume D:                     ║
║ Koneksi outbound ke IP asing tercatat aktif.       ║
║                                                    ║
║ Examiner Signature: _______________                ║
║ Witness Signature : _______________                ║
╚══════════════════════════════════════════════════╝
```

*(Log terpisah dibuat untuk setiap evidence item: RAM, Network State, Disk Image)*

---

#### Studi Kasus 2: Operasi Triage di Lokasi Penggerebekan

**1. Perencanaan Triage — Timeline 180 Menit (20 poin):**

| Waktu | Aktivitas | Target | Justifikasi |
|-------|----------|--------|-------------|
| 0:00-0:15 | Dokumentasi foto seluruh lokasi dan semua perangkat | Semua (16 item) | Dokumentasi visual wajib sebelum menyentuh apapun — dibutuhkan untuk chain of custody dan konteks |
| 0:15-0:30 | RAM dump laptop sleep mode (SSD #1) | Volatile data kritis | Laptop sleep = RAM masih ada. SSD + kemungkinan FDE = RAM priority. Paling volatile |
| 0:30-0:50 | RAM + logical acquisition Desktop PC (chat terenkripsi visible) | Active session data | Layar menampilkan chat terenkripsi = bukti kritis sedang terbuka. Capture RAM (chat keys) + screenshot layar + logical acquisition folder chat |
| 0:50-1:05 | Image 5× USB flash drive (8-128GB) + MicroSD 256GB | Removable media | Kapasitas kecil = akuisisi cepat via write blocker. Sering menyimpan dokumen penting. Total ~700GB, imaging ~15 menit |
| 1:05-1:20 | Akuisisi data smartphone (3 unit) | Mobile data | Smartphone menyala = volatile. Logical acquisition menggunakan FTK Imager/ADB. Aktifkan airplane mode dahulu |
| 1:20-1:50 | Image External HDD 1TB (via write blocker) | Bulk storage | Physical acquisition E01 dengan kompresi. ~30 menit untuk 1TB |
| 1:50-2:30 | Image External HDD 2TB (via write blocker) | Bulk storage | Physical acquisition. ~40 menit untuk 2TB |
| 2:30-2:40 | Capture router config dan log | Network evidence | Export running config, DHCP leases, connected device history, system logs |
| 2:40-2:55 | Label, packaging, dan sealing semua perangkat | Chain of custody | Evidence bag, label anti-tamper, numbering |
| 2:55-3:00 | Final check dan serah terima ke komandan | Dokumentasi | Verifikasi checklist lengkap, sign-off |

**Catatan Prioritas:**
- Laptop mati (SSD #2) → seize fisik (tidak perlu on-site imaging, SSD tanpa FDE indikator)
- Jika waktu tersisa, baru image laptop mati

**2. Strategi per Perangkat (20 poin):**

**a) Laptop Sleep Mode (SSD NVMe #1):**
- Metode: RAM acquisition → Logical acquisition (jika waktu cukup)
- Tool: DumpIt (satu klik, cepat) dari USB
- RAM dump ke external HDD kosong
- Format: raw (.mem) untuk RAM
- Estimasi: 10-15 menit (tergantung ukuran RAM)

**b) Desktop PC (menyala, chat terenkripsi):**
- Metode: Screenshot layar → RAM acquisition → Logical acquisition (folder chat + user profile)
- Tool: Kamera (screenshot layar), Belkasoft RAM Capturer, FTK Imager logical
- Format: Raw (.mem) untuk RAM, E01 untuk logical
- Estimasi: 15-20 menit

**c) Laptop Mati (SSD NVMe #2):**
- Metode: Seize fisik → Physical acquisition di lab
- Prosedur: Matikan (sudah mati), label, evidence bag
- Akuisisi di lab dengan NVMe write blocker
- Estimasi on-site: 3 menit (label + bag)

**d) 3× Smartphone:**
- Metode: Logical acquisition (terbatas pada PC Windows)
- Tool: FTK Imager atau Android ADB backup (Android), iTunes backup (iPhone)
- Aktifkan airplane mode SEBELUM akuisisi
- Format: logical image / backup file
- Estimasi: 5 menit per device

**e) 5× USB + MicroSD (total ~700GB):**
- Metode: Physical acquisition via write blocker
- Tool: FTK Imager → Create Disk Image → Physical Drive
- Format: E01 dengan kompresi
- Proses satu per satu (1 write blocker + USB hub)
- Estimasi: 2-3 menit per USB kecil (8-32GB), 5-8 menit per USB besar (128GB), 5 menit MicroSD

**f) 2× External HDD (1TB + 2TB):**
- Metode: Physical acquisition via SATA write blocker
- Tool: FTK Imager → Physical Drive → E01
- Format: E01 dengan kompresi
- Estimasi: ~30 menit (1TB), ~40 menit (2TB) pada USB 3.0

**g) Router Wi-Fi:**
- Metode: Logical extraction
- Prosedur: Login ke admin panel (jika accessible), export config, DHCP log, syslog
- Jika tidak accessible: seize fisik
- Estimasi: 5-10 menit

**3. Penanganan Perangkat Tidak Sempat (10 poin):**

**Prosedur Seize Fisik:**

1. **Dokumentasi:** foto perangkat in-situ sebelum dipindahkan, catat kondisi (menyala/mati, LED, kabel)

2. **Shutdown yang benar:**
   - Sistem menyala: untuk laptop/PC → jangan shutdown normal (bisa trigger wipe), cabut power cord langsung (untuk desktop) atau tahan power button 5 detik (laptop)
   - Pengecualian: jika FDE aktif dan RAM belum di-dump, pertimbangkan risiko

3. **Packaging:**
   - Sarung tangan anti-statik saat handling
   - Bungkus dengan anti-static bag (untuk hard drive dan media elektronik)
   - Masukkan ke evidence bag berlabel
   - Seal dengan tamper-evident tape

4. **Labeling:**
   - Evidence number (unique identifier)
   - Deskripsi: tipe, model, serial number, warna
   - Tanggal dan waktu seize
   - Nama petugas yang mengamankan
   - Lokasi ditemukan

5. **Transportasi:**
   - Jauhkan dari medan magnet, air, dan suhu ekstrem
   - Jangan tumpuk media penyimpanan
   - Simpan dalam kendaraan yang terkondisi
   - Dokumentasikan siapa yang membawa dan rute transportasi

**4. Checklist Dokumentasi Lapangan (15 poin):**

```
╔══════════════════════════════════════════════════════╗
║     CHECKLIST DOKUMENTASI FORENSIK LAPANGAN          ║
╠══════════════════════════════════════════════════════╣
║                                                      ║
║ INFORMASI OPERASI                                    ║
║ □ Nomor Operasi    : ________________________        ║
║ □ Tanggal/Waktu    : ____/____/____ | ____:____ WIB  ║
║ □ Lokasi           : ________________________        ║
║ □ Koordinat GPS    : ________________________        ║
║ □ Komandan Operasi : ________________________        ║
║ □ Tim Forensik     : ________________________        ║
║                                                      ║
║ DOKUMENTASI VISUAL                                   ║
║ □ Foto panorama lokasi (4 arah)                      ║
║ □ Foto close-up setiap perangkat in-situ             ║
║ □ Foto layar perangkat yang menyala                  ║
║ □ Foto serial number / label setiap device           ║
║ □ Foto kabel dan koneksi antar perangkat             ║
║ □ Sketsa layout ruangan dengan posisi perangkat      ║
║                                                      ║
║ IDENTIFIKASI PERANGKAT (isi per item)                ║
║ □ Evidence No : EV-____                              ║
║ □ Tipe        : □ Laptop □ Desktop □ Server          ║
║                 □ Smartphone □ USB □ HDD □ Lainnya   ║
║ □ Merk/Model  : ________________________             ║
║ □ Serial No   : ________________________             ║
║ □ Kondisi     : □ Menyala □ Sleep □ Mati             ║
║ □ Enkripsi    : □ Ya □ Tidak □ Tidak diketahui       ║
║ □ Foto diambil: □ Ya (No: ____)                      ║
║ □ Akuisisi    : □ RAM □ Physical □ Logical □ Seize   ║
║ □ Hash (jika diakuisisi): ________________________   ║
║                                                      ║
║ CHAIN OF CUSTODY AWAL                                ║
║ □ Diamankan oleh  : ______________ | ____:____ WIB   ║
║ □ Diserahkan ke   : ______________ | ____:____ WIB   ║
║ □ Disimpan di     : ________________________         ║
║ □ Tanda tangan penyerah  : ______________            ║
║ □ Tanda tangan penerima  : ______________            ║
║ □ Tanda tangan saksi     : ______________            ║
║                                                      ║
║ CATATAN KHUSUS                                       ║
║ ________________________________________________     ║
║ ________________________________________________     ║
║ ________________________________________________     ║
║                                                      ║
║ VERIFIKASI AKHIR                                     ║
║ □ Semua perangkat terdokumentasi                     ║
║ □ Semua image terverifikasi hash                     ║
║ □ Semua evidence bag tersegel                        ║
║ □ Checklist lengkap ditandatangani                   ║
║                                                      ║
║ Penanggung Jawab: ______________ | ____:____ WIB     ║
╚══════════════════════════════════════════════════════╝
```

---

## Rubrik Penilaian

### Pilihan Ganda
- Setiap soal benar: 2 poin
- Total: 40 poin

### Uraian
- Soal ⭐: maksimal 5 poin
- Soal ⭐⭐: maksimal 8 poin  
- Soal ⭐⭐⭐: maksimal 12 poin
- Total: 116 poin

### Studi Kasus
- Studi Kasus 1: 65 poin
- Studi Kasus 2: 65 poin
- Total: 130 poin

### Total Keseluruhan: 286 poin

---

## License

This repository is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

Commercial use is permitted, provided attribution is given to the author.

© 2026 Anindito
