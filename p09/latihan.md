# Latihan Pertemuan 09: Teknik Recovery Data dan File Carving

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 09  
**Topik:** Teknik Recovery Data dan File Carving  
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
Ketika sebuah file "dihapus" secara normal (bukan penghapusan aman), apa yang sebenarnya terjadi pada data di hard disk?

A. Data dihapus sepenuhnya dari disk  
B. Data dienkripsi oleh sistem operasi  
C. Penunjuk/entri di file system dihapus, data tetap ada  
D. Data dipindahkan ke Recycle Bin secara fisik  
E. Data ditimpa dengan nilai nol

### Soal 2
Slack space pada sistem file terdiri dari dua komponen. Apa saja kedua komponen tersebut?

A. Active slack dan Passive slack  
B. RAM slack dan File slack  
C. Primary slack dan Secondary slack  
D. System slack dan User slack  
E. Allocated slack dan Free slack

### Soal 3
Sebuah file berukuran 6.500 bytes disimpan pada sistem dengan ukuran cluster 8 KB (8.192 bytes). Berapa besar slack space yang terbentuk?

A. 1.500 bytes  
B. 1.692 bytes  
C. 6.500 bytes  
D. 8.192 bytes  
E. 14.692 bytes

### Soal 4
Tanda tangan file (magic bytes) untuk file JPEG adalah:

A. `89 50 4E 47`  
B. `FF D8 FF`  
C. `25 50 44 46`  
D. `50 4B 03 04`  
E. `4D 5A`

### Soal 5
Manakah dari berikut ini yang merupakan tanda tangan untuk file PDF?

A. `FF D8 FF E0`  
B. `89 50 4E 47 0D 0A 1A 0A`  
C. `25 50 44 46` (%PDF)  
D. `50 4B 03 04` (PK..)  
E. `47 49 46 38` (GIF8)

### Soal 6
File carving adalah teknik untuk:

A. Membuat partisi baru pada hard disk  
B. Mengekstrak file tanpa menggunakan metadata file system  
C. Mengkompresi file untuk menghemat ruang  
D. Mengenkripsi file untuk keamanan  
E. Menghapus file secara permanen

### Soal 7
Teknik carving yang paling umum digunakan untuk file dengan footer yang jelas adalah:

A. Statistical carving  
B. Header/Size carving  
C. Header/Footer carving  
D. Block-based carving  
E. Entropy-based carving

### Soal 8
Tool pemulihan data yang memiliki antarmuka grafis yang ramah pengguna dan kemampuan pratinjau adalah:

A. PhotoRec  
B. Foremost  
C. Scalpel  
D. Recuva  
E. ddrescue

### Soal 9
Mengapa hasil pemulihan PhotoRec tidak mempertahankan nama file dan struktur folder asli?

A. Tool tidak cukup canggih  
B. PhotoRec menggunakan carving berbasis signature tanpa metadata  
C. File system rusak  
D. Pengguna salah konfigurasi  
E. Keterbatasan ruang penyimpanan

### Soal 10
Dalam perbandingan Foremost vs Scalpel untuk carving image 500 GB, tool mana yang lebih direkomendasikan?

A. Foremost, karena lebih akurat  
B. Scalpel, karena multi-threaded dan 2-3x lebih cepat  
C. Sama saja, tidak ada perbedaan  
D. Foremost, karena penggunaan memori lebih rendah  
E. Keduanya tidak cocok untuk image besar

### Soal 11
Basis data SQLite dapat diidentifikasi dengan signature:

A. `21 42 44 4E` ("!BDN")  
B. `53 51 4C 69 74 65 20 66 6F 72 6D 61 74 20 33` ("SQLite format 3")  
C. `4D 53 43 46` ("MSCF")  
D. `1F 8B 08` (GZIP)  
E. `52 61 72 21` ("Rar!")

### Soal 12
File PST (Personal Storage Table) Microsoft Outlook memiliki signature:

A. `50 4B 03 04`  
B. `21 42 44 4E`  
C. `4D 5A`  
D. `D0 CF 11 E0`  
E. `FF FE`

### Soal 13
Perintah TRIM pada SSD berfungsi untuk:

A. Meningkatkan kecepatan baca disk  
B. Memberitahu SSD bahwa blok data dapat dihapus  
C. Mengenkripsi data yang dihapus  
D. Membuat cadangan otomatis  
E. Defragmentasi disk

### Soal 14
Tingkat keberhasilan pemulihan pada HDD tradisional dibandingkan SSD dengan TRIM aktif adalah:

A. HDD: 90%, SSD: 10%  
B. HDD: 50%, SSD: 50%  
C. HDD: 10%, SSD: 90%  
D. Sama-sama 90%  
E. Sama-sama 10%

### Soal 15
Pada laptop dengan SSD TRIM aktif, file dihapus 3 jam lalu. Apa tindakan PERTAMA yang harus dilakukan?

A. Jalankan PhotoRec segera  
B. Matikan laptop segera  
C. Reboot ke Linux  
D. Periksa Recycle Bin  
E. Instal software pemulihan

### Soal 16
Tool yang paling cocok untuk pemulihan dari media rusak dengan bad sector adalah:

A. Recuva  
B. PhotoRec  
C. Autopsy  
D. ddrescue  
E. FTK Imager

### Soal 17
Sistem file yang memberikan potensi pemulihan TERTINGGI adalah:

A. NTFS  
B. FAT32  
C. ext4  
D. APFS  
E. exFAT

### Soal 18
Dalam hex editor, Anda menemukan pola `FF D8 FF E0 00 10 4A 46 49 46`. Jenis file apa ini?

A. Gambar PNG  
B. Gambar JPEG  
C. Dokumen PDF  
D. Arsip ZIP  
E. Binary EXE

### Soal 19
Mengapa TIDAK boleh menyimpan hasil pemulihan ke drive yang sama dengan sumber data?

A. Akan memperlambat proses  
B. Akan menimpa data yang sedang dipulihkan  
C. Melanggar aturan hak cipta  
D. Tool tidak akan berfungsi  
E. File akan rusak

### Soal 20
Untuk investigasi forensik militer dengan kebutuhan rantai bukti, manajemen kasus, dan pelaporan profesional, tool yang paling direkomendasikan adalah:

A. Recuva  
B. PhotoRec  
C. Scalpel  
D. Autopsy  
E. HxD Hex Editor

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Jelaskan perbedaan antara "penghapusan file" dan "penghapusan data". Mengapa file yang "dihapus" masih dapat dipulihkan?

### Soal 2 ⭐
Sebutkan dan jelaskan 3 lokasi di media penyimpanan di mana sisa data dapat ditemukan!

### Soal 3 ⭐
File berukuran 3.750 bytes disimpan pada cluster 4 KB. Hitung berapa besar RAM slack dan File slack (asumsi: 1 cluster = 8 sektor, 1 sektor = 512 bytes)!

### Soal 4 ⭐
Apa yang dimaksud dengan "tanda tangan file" atau "magic bytes"? Mengapa lebih dapat diandalkan daripada ekstensi file untuk identifikasi jenis file?

### Soal 5 ⭐⭐
Sebutkan 4 tool pemulihan data yang berbeda dan jelaskan masing-masing kelebihan utamanya!

### Soal 6 ⭐⭐
Jelaskan secara detail proses "Header/Footer Carving" untuk mengekstrak file JPEG dari unallocated space!

### Soal 7 ⭐⭐
Diberikan hex dump berikut:
```
00000000: 25 50 44 46 2D 31 2E 37 0D 0A 25 E2 E3 CF D3 0A
00000010: 31 20 30 20 6F 62 6A 0A 3C 3C 2F 54 79 70 65 2F
```
Identifikasi jenis file, versi, dan jelaskan analisis Anda!

### Soal 8 ⭐⭐
Bandingkan PhotoRec dan Scalpel dari aspek: threading, kecepatan, penggunaan memori, dan use case. Kapan sebaiknya menggunakan masing-masing tool?

### Soal 9 ⭐⭐
Jelaskan mengapa FAT32 memberikan potensi pemulihan lebih tinggi (90%) dibanding APFS (45%)! Sebutkan minimal 3 faktor teknis yang mempengaruhi!

### Soal 10 ⭐⭐
Dalam konteks forensik basis data, jelaskan bagaimana cara mengidentifikasi dan mengekstrak basis data SQLite dari forensic image! Sebutkan tool yang digunakan dan langkah verifikasinya!

### Soal 11 ⭐⭐
File PST (Personal Storage Table) ditemukan dalam unallocated space. Jelaskan nilai forensik dari file ini dalam konteks investigasi kebocoran dokumen militer! Sebutkan minimal 4 jenis informasi yang dapat diekstrak!

### Soal 12 ⭐⭐⭐
Jelaskan mekanisme perintah TRIM pada SSD dan dampaknya terhadap forensik digital! Mengapa jendela pemulihan pada SSD jauh lebih pendek dibanding HDD? Apa strategi alternatif jika file tidak dapat dipulihkan dari SSD?

### Soal 13 ⭐⭐⭐
Anda memiliki hard disk rusak 1 TB dengan 3.245 bad sector. Jelaskan prosedur langkah-demi-langkah menggunakan ddrescue untuk memaksimalkan pemulihan data! Sebutkan flag/option yang digunakan dan alasannya!

### Soal 14 ⭐⭐⭐
Dalam hex editor (HxD), Anda menemukan pola hex dari offset 0x00000000 hingga 0x0001A4F2. Header menunjukkan `FF D8 FF E0` dan footer di offset 0x0001A4F2 adalah `FF D9`. Jelaskan prosedur manual carving untuk mengekstrak file JPEG ini! Hitung ukuran file dan jelaskan langkah verifikasi!

### Soal 15 ⭐⭐⭐
Bandingkan secara komprehensif potensi pemulihan antara FAT32, NTFS, ext4, dan APFS! Buat tabel perbandingan yang mencakup: tingkat pemulihan, faktor kunci, penanganan metadata, dan rekomendasi penggunaan!

---

## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Kebocoran Dokumen Rahasia di Kodam Jaya

#### Latar Belakang
Pada tanggal 15 Januari 2026, Kodam Jaya melaporkan kebocoran dokumen rahasia tentang rencana operasi militer. Investigasi awal menunjukkan bahwa Mayor Budi (nama samaran), seorang perwira staf operasi, diduga terlibat dalam kebocoran tersebut. Tim investigasi forensik digital TNI AD melakukan penyitaan laptop kerja Mayor Budi.

#### Informasi Teknis Perangkat
- **Laptop:** Dell Latitude 5420
- **Prosesor:** Intel Core i7-1185G7
- **RAM:** 16 GB DDR4
- **Penyimpanan:** Samsung 980 PRO 512 GB NVMe SSD
- **Sistem Operasi:** Windows 11 Pro (build 22H2)
- **Status TRIM:** Aktif (dikonfirmasi via `fsutil behavior query DisableDeleteNotify`)
- **Shutdown Terakhir:** 14 Januari 2026, 23:45 WIB

#### Temuan Awal
1. **Recycle Bin:** Kosong (dikosongkan 14 Januari 2026, 23:30 WIB)
2. **Riwayat Browser:** Dibersihkan (log CCleaner: 14 Januari 2026, 23:35 WIB)
3. **File Terkini:** Tidak ada dokumen rahasia di `%APPDATA%\Microsoft\Windows\Recent`
4. **Email:** Tidak ada email keluar dengan lampiran mencurigakan di 7 hari terakhir
5. **Aktivitas USB:** Log menunjukkan USB SanDisk 64GB terkoneksi pada 14 Januari 2026, 22:15-22:30 WIB

#### Pencitraan Forensik
Tim berhasil membuat forensic image dengan FTK Imager:
- **Format Image:** E01 (EnCase Evidence File)
- **Ukuran Image:** 387 GB (terkompresi)
- **Hash (SHA-256):** `a3f8d9e2c4b7a6f1e9d8c7b6a5f4e3d2c1b0a9f8e7d6c5b4a3f2e1d0c9b8a7f6`
- **Waktu Akuisisi:** 15 Januari 2026, 09:15 WIB

#### Analisis Awal Autopsy
- **Unallocated Space:** 87 GB
- **File Terhapus Terdeteksi:** 2.847 file
- **Jenis File (terhapus):**
  - Dokumen (PDF, DOCX): 124 file
  - Gambar (JPEG, PNG): 1.983 file
  - Video (MP4): 87 file
  - Arsip (ZIP, RAR): 23 file
  - Basis data (SQLite): 15 file
  - Email (PST): 1 file
  - Lainnya: 614 file

#### Hasil File Carving (PhotoRec)
PhotoRec berhasil memulihkan:
- 3.421 file total dari unallocated space
- File penting yang ditemukan:
  - `f0002847.pdf` (ukuran: 2,4 MB, tanggal: 10 Januari 2026)
  - `f0002848.docx` (ukuran: 187 KB, tanggal: 12 Januari 2026)
  - `f0002849.pst` (ukuran: 145 MB, berbagai tanggal)
  - `f0003201.db` (SQLite, ukuran: 8,9 MB, tanggal: 14 Januari 2026)

#### Temuan Analisis Hex
Analisis hex manual menemukan:
- Offset `0x1A4F0000`: Header `25 50 44 46 2D 31 2E 37` (PDF 1.7)
- Offset `0x1A71C3F2`: Footer `25 25 45 4F 46 0D 0A` (%%EOF)
- Ukuran terhitung: 2.278.386 bytes (2,17 MB)
- File potensial: Dokumen dengan kata kunci "OPERASI RAHASIA"

#### Analisis PST (readpst)
PST yang diekstrak berisi:
- 2.847 email (2023-2026)
- 423 dengan lampiran
- **Email menarik:**
  - Kepada: `contact@anonymmail.org` (eksternal, layanan email terenkripsi)
  - Tanggal: 14 Januari 2026, 21:45 WIB
  - Subjek: "Material Requested"
  - Lampiran: [TERHAPUS, nama file: `OPPLAN-2026-RAHASIA.pdf`]
  - Metadata menunjukkan: Ukuran asli 2,4 MB

#### Analisis Basis Data SQLite
Struktur basis data `f0003201.db`:
```sql
CREATE TABLE transfer_file (
  id INTEGER PRIMARY KEY,
  nama_file TEXT,
  ukuran INTEGER,
  tujuan TEXT,
  waktu INTEGER,
  hash TEXT
);
```

Contoh record:
```
1, OPPLAN-2026-RAHASIA.pdf, 2478932, USB_SANDISK_64GB, 1705245300, a3f8d9...
2, RINGKASAN-INTEL-Q4.docx, 191283, contact@anonymmail.org, 1705244700, e7c4b2...
```

#### Rekonstruksi Timeline
- **22:15 WIB:** USB terhubung
- **22:17 WIB:** File `OPPLAN-2026-RAHASIA.pdf` disalin ke USB (berdasarkan log SQLite)
- **22:30 WIB:** USB terputus
- **22:45 WIB:** File dihapus dari laptop
- **23:30 WIB:** Recycle Bin dikosongkan
- **23:35 WIB:** CCleaner dijalankan (riwayat browser, file temp)
- **23:45 WIB:** Sistem dimatikan

#### Pertanyaan Analisis:

**1a. Strategi Pemulihan Data (15 poin)**
Berdasarkan informasi bahwa SSD dengan TRIM aktif dan file dihapus ~9 jam sebelum penyitaan, evaluasi kemungkinan pemulihan file `OPPLAN-2026-RAHASIA.pdf`. Jelaskan: probabilitas pemulihan (dengan alasan), mengapa TRIM menjadi faktor kritis, dan sumber alternatif untuk memulihkan konten file.

**1b. Analisis File Carving (15 poin)**
PhotoRec memulihkan file `f0002847.pdf` (2,4 MB) yang ukurannya cocok dengan file yang disalin ke USB. Jelaskan prosedur untuk: memverifikasi bahwa ini adalah file yang dicari, mengekstrak dan memvalidasi integritas file, membandingkan hash dengan log di basis data SQLite, dan membuka serta memeriksa konten (dengan tool apa?).

**1c. Forensik Email PST (15 poin)**
Email dengan lampiran terhapus ditemukan di PST. Jelaskan: bagaimana cara memulihkan lampiran yang terhapus dari PST, tool yang digunakan (`readpst`, PST Walker, atau lainnya), metadata apa saja yang dapat diekstrak dari header email, dan nilai forensik dari email ke layanan terenkripsi `anonymmail.org`.

**1d. Bukti Basis Data SQLite (10 poin)**
Basis data `f0003201.db` berisi log transfer file. Jelaskan: bagaimana basis data ini ter-create (aplikasi apa kemungkinannya?), nilai forensik dari tabel `transfer_file`, bagaimana memverifikasi hash yang tercantum, dan apakah ini bukti yang cukup untuk membuktikan eksfiltrasi data?

**1e. Legal dan Investigasi Lanjutan (10 poin)**
Berdasarkan bukti yang terkumpul, jelaskan: apakah bukti cukup kuat untuk membuktikan kebocoran?, apa saja yang harus dicari pada USB SanDisk 64GB (belum ditemukan)?, langkah investigasi lanjutan (log jaringan, CCTV, wawancara), pertimbangan legal terkait UU ITE dan UU Pertahanan, serta dokumentasi yang diperlukan untuk rantai bukti.

---

### Studi Kasus 2: Sabotase Sistem Pertahanan Udara Melalui Insider Threat

#### Latar Belakang
Pada tanggal 20 Januari 2026, sistem radar pertahanan udara di Lanud Sultan Hasanuddin, Makassar, mengalami malfungsi mendadak yang menyebabkan blind spot selama 4 jam. Investigasi Security Operations Center (SOC) menemukan indikasi sabotase oleh insider. Suspek utama adalah Sersan Andi (nama samaran), teknisi sistem radar yang memiliki hak akses tinggi.

#### Informasi Teknis Perangkat
- **Workstation:** HP Z2 Tower G9
- **Prosesor:** Intel Xeon W-1390P
- **RAM:** 32 GB ECC DDR4
- **Penyimpanan 1 (OS):** WD Black 1 TB NVMe SSD (TRIM aktif)
- **Penyimpanan 2 (Data):** Seagate Exos 4 TB HDD (RAID 1)
- **Sistem Operasi:** Windows Server 2022
- **Akses Terakhir:** 19 Januari 2026, 23:55 WIB

#### Timeline Insiden (dari log SIEM)
- **19 Jan, 23:30:** Sersan Andi login ke workstation (kredensial terverifikasi)
- **19 Jan, 23:35:** Hak akses ditingkatkan (disetujui via akun admin)
- **19 Jan, 23:40-23:55:** Akses file mencurigakan ke `C:\RadarControl\Config\`
- **20 Jan, 00:05:** Alert malfungsi sistem radar
- **20 Jan, 00:07:** Auto-failover gagal
- **20 Jan, 04:15:** Sistem dipulihkan oleh tim backup

#### Akuisisi Forensik
**SSD (OS Drive):**
- Image dibuat dengan write-blocker
- Format: dd (raw image)
- Ukuran: 931 GB
- Hash: `f7e6d5c4b3a2f1e0d9c8b7a6f5e4d3c2b1a0f9e8d7c6b5a4f3e2d1c0b9a8f7e6`

**HDD (Data Drive):**
- RAID 1 mirror (kedua drive di-image)
- Format: E01
- Ukuran: 3,8 TB (masing-masing)
- Kondisi: Baik, tidak ada bad sector

#### Analisis File Terhapus
**Dari SSD (Autopsy + PhotoRec):**
- `RadarControl_Backup.zip` (dihapus 19 Jan 23:42, ukuran: 4,7 MB)
- `system_config_original.xml` (dihapus 19 Jan 23:44, ukuran: 187 KB)
- `malware_payload.exe` (dihapus 19 Jan 23:55, ukuran: 2,1 MB)
- `cleanup_script.ps1` (dihapus 19 Jan 23:57, ukuran: 4,3 KB)

**Status Pemulihan:**
- `RadarControl_Backup.zip`: ✗ Tidak dipulihkan (TRIM aktif dalam 2 menit)
- `system_config_original.xml`: ✓ Sebagian dipulihkan (73% lengkap, beberapa sektor tertimpa)
- `malware_payload.exe`: ✗ Tidak dipulihkan (TRIM + tertimpa)
- `cleanup_script.ps1`: ✓ Sepenuhnya dipulihkan

#### Konten Script yang Dipulihkan (`cleanup_script.ps1`)
```powershell
# Script pembersihan - Hapus bukti
$files = @(
    "C:\RadarControl\Config\backup\RadarControl_Backup.zip",
    "C:\RadarControl\Config\system_config_original.xml",
    "C:\Temp\payload.exe"
)

foreach ($file in $files) {
    if (Test-Path $file) {
        Remove-Item $file -Force
        Write-Host "Dihapus: $file"
    }
}

# Bersihkan log
Clear-EventLog -LogName Application
Clear-EventLog -LogName System

# Penghapusan aman
sdelete -p 7 C:\Temp\*.exe
```

#### Analisis HDD (RAID Data Drive)
**File System:** NTFS
**Unallocated Space:** 247 GB
**File Terhapus:** 127 file dipulihkan via Foremost

**Pemulihan Kunci:**
- `external_connection_log.csv` (dihapus 19 Jan 23:50)
- `suspicious_network_traffic.pcap` (dihapus 19 Jan 23:52, ukuran: 89 MB)
- `encrypted_communication.zip` (dihapus 19 Jan 23:53, dilindungi password)

#### Analisis Lalu Lintas Jaringan (`suspicious_network_traffic.pcap`)
- **IP Tujuan:** 203.xxx.xxx.xxx (Lokasi: Beijing, China)
- **Protokol:** HTTPS (port 443)
- **Transfer Data:** 47 MB diunggah
- **Waktu:** 19 Jan 23:41-23:47
- **Sertifikat:** Self-signed (mencurigakan)

#### Log Koneksi Eksternal (`external_connection_log.csv`)
```csv
timestamp,source_ip,dest_ip,dest_port,bytes_sent,bytes_received,protocol
2026-01-19 23:41:32,10.20.30.105,203.xxx.xxx.xxx,443,47234891,2847,HTTPS
2026-01-19 23:47:15,10.20.30.105,203.xxx.xxx.xxx,443,0,0,HTTPS
```

#### Analisis Malware (Pemulihan Sebagian)
Dari fragmen yang di-carve `malware_payload.exe`:
- **Header:** `4D 5A` (PE executable)
- **String parsial:**
  - "RadarControlInterface"
  - "DisableFailover"
  - "ExfiltrateConfig"
  - "C2Server=203.xxx.xxx.xxx"
- **Ukuran perkiraan:** 2,1 MB
- **Timestamp kompilasi:** 15 Jan 2026, 14:22 UTC

#### Analisis Arsip Terenkripsi
`encrypted_communication.zip`:
- **Enkripsi:** AES-256
- **Dilindungi password:** Ya
- **Berisi:** 3 file (berdasarkan header arsip)
  - `radar_config.xml` (187 KB)
  - `access_credentials.txt` (2 KB)
  - `exfiltration_instructions.pdf` (421 KB)

**Upaya Pemulihan Password:**
- Dictionary attack: Gagal (24 jam, 100M password)
- Brute force: Tidak feasible (AES-256)
- Alternatif: Cari password di memory dump, browser saved password, dll.

#### Analisis Memory Dump
RAM dump (32 GB) dari workstation:
- Tool: Belkasoft RAM Capturer
- Analisis: Volatility 3

**Temuan Kunci:**
- Process: `malicious_service.exe` (PID: 4892, parent: services.exe)
- Koneksi jaringan: Outbound ke 203.xxx.xxx.xxx
- String di memori:
  - Kandidat password: "P@ssw0rd_R4dar2026!"
  - Perintah C2: "disable_failover", "upload_config", "cleanup"

#### Analisis Registry (SSD)
**HKEY_CURRENT_USER\Software\RadarControl:**
- Terakhir dimodifikasi: 19 Jan 2026, 23:43
- Value: `BackupLocation` terhapus (VSS menunjukkan aslinya: "C:\RadarControl\Config\backup\")

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services:**
- Service baru: `RadarMonitoringService` (terhapus)
- Path binary: `C:\Temp\payload.exe` (sekarang terhapus)
- Jenis start: Automatic

#### VSS (Volume Shadow Copies)
Windows Server memiliki VSS aktif:
- **Shadow Copy 1:** 18 Jan 2026, 02:00 (pra-insiden)
- **Shadow Copy 2:** 19 Jan 2026, 02:00 (pra-insiden)

**Dari Shadow Copy 2:**
- `system_config_original.xml` asli dipulihkan (100% lengkap)
- Snapshot registry menunjukkan tidak ada service berbahaya
- Tidak ada file mencurigakan di `C:\Temp\`

#### Pertanyaan Analisis:

**2a. Strategi Pemulihan Multi-Penyimpanan (15 poin)**
Workstation memiliki SSD (OS) dengan TRIM dan HDD (Data) tanpa TRIM. Jelaskan: mengapa tingkat pemulihan berbeda drastis antara kedua penyimpanan, strategi untuk memaksimalkan pemulihan dari masing-masing drive, mengapa `cleanup_script.ps1` dapat dipulihkan tapi `malware_payload.exe` tidak, dan peran Volume Shadow Copies dalam pemulihan.

**2b. Analisis Fragmen Malware (15 poin)**
Malware hanya sebagian dipulihkan (fragmen). Jelaskan: apa yang dapat dianalisis dari PE executable parsial, bagaimana mengekstrak string dari fragmen yang di-carve, nilai forensik dari string "DisableFailover" dan "C2Server", dan apakah fragmen cukup untuk dijadikan bukti?

**2c. Pemulihan Password Arsip Terenkripsi (15 poin)**
Arsip `encrypted_communication.zip` dipulihkan tapi dilindungi password. Jelaskan: strategi untuk memulihkan password (tanpa brute-force AES-256), lokasi-lokasi di mana password mungkin tersimpan (memory, registry, browser, dll.), bagaimana memverifikasi kandidat password dari memory dump, dan jika password tidak dapat di-crack, apa pendekatan alternatif?

**2d. Korelasi Forensik Jaringan (10 poin)**
Dari PCAP dan log CSV, terlihat eksfiltrasi data 47 MB ke China. Jelaskan: apa yang kemungkinan diunggah (berdasarkan ukuran file dan bukti), bagaimana mengorelasikan timeline antara penghapusan file, transfer jaringan, dan eksekusi malware, nilai forensik dari sertifikat self-signed, dan investigasi lanjutan (hubungi ISP, kerjasama internasional, dll.).

**2e. Integrasi Bukti dan Pelaporan (10 poin)**
Integrasikan semua bukti menjadi laporan investigasi komprehensif: rekonstruksi timeline lengkap (dari login hingga pembersihan), rantai kejadian (eksekusi malware → modifikasi konfigurasi → eksfiltrasi → pembersihan), kekuatan bukti (apa yang dapat dibuktikan secara definitif vs. circumstantial), tindakan yang direkomendasikan (disipliner, legal, penguatan sistem), dan pelajaran yang dipetik serta langkah preventif.

---

## Kunci Jawaban

### Bagian A: Pilihan Ganda

> *Kunci jawaban pilihan ganda tersedia pada dokumen terpisah.*

### Bagian B: Uraian

> *Kunci jawaban uraian menggunakan rubrik penilaian berdasarkan kelengkapan dan kedalaman jawaban.*

### Bagian C: Studi Kasus

> *Panduan jawaban studi kasus menggunakan rubrik analitis berdasarkan kelengkapan investigasi dan kualitas rekomendasi.*

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
