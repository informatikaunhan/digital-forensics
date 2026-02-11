# Latihan Pertemuan 05: Forensik Windows I — Volatile Data dan Artefak Sistem

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 05  
**Topik:** Forensik Windows I — Analisis Volatile Data dan Artefak Sistem  
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
Berdasarkan order of volatility (RFC 3227), data manakah yang harus dikumpulkan PERTAMA dalam proses investigasi forensik?

A. File di hard disk  
B. RAM / Memory dump  
C. Backup tape  
D. Event logs  
E. Browser history

---

### Soal 2
Pada pendekatan live forensics, investigator mengumpulkan bukti dari sistem yang:

A. Sudah dimatikan dan dicabut dari jaringan  
B. Masih dalam kondisi menyala dan berjalan  
C. Sudah di-image ke media forensik  
D. Sedang dalam proses booting ulang  
E. Sudah di-format ulang

---

### Soal 3
Manakah yang BUKAN merupakan data yang tersimpan di RAM dan bernilai forensik?

A. Daftar proses aktif  
B. Encryption keys (BitLocker)  
C. MBR (Master Boot Record)  
D. Koneksi jaringan aktif  
E. Clipboard data

---

### Soal 4
Tools manakah yang paling tepat digunakan untuk melakukan memory acquisition pada sistem Windows yang masih berjalan?

A. Autopsy  
B. Wireshark  
C. DumpIt  
D. PhotoRec  
E. HxD Hex Editor

---

### Soal 5
Volatility 3 memiliki keunggulan dibanding Volatility 2 karena:

A. Berbasis Python 2 yang lebih stabil  
B. Memerlukan profile selection secara manual  
C. Mendukung auto-detection profil OS melalui symbol tables  
D. Hanya mendukung Windows XP dan Windows 7  
E. Tidak memerlukan memory dump sebagai input

---

### Soal 6
Plugin Volatility 3 yang digunakan untuk mendeteksi injected code pada proses di memori adalah:

A. `windows.pslist`  
B. `windows.netscan`  
C. `windows.malfind`  
D. `windows.hashdump`  
E. `windows.envars`

---

### Soal 7
Dalam hierarki proses normal Windows, parent process yang sah untuk `svchost.exe` adalah:

A. `explorer.exe`  
B. `csrss.exe`  
C. `winlogon.exe`  
D. `services.exe`  
E. `lsass.exe`

---

### Soal 8
Teknik rootkit yang memanipulasi linked list kernel untuk menyembunyikan proses dari Task Manager disebut:

A. DLL Injection  
B. Process Hollowing  
C. DKOM (Direct Kernel Object Manipulation)  
D. APC Injection  
E. Token Impersonation

---

### Soal 9
Event ID pada Windows Security Log yang menunjukkan bahwa audit log telah dihapus (anti-forensik) adalah:

A. 4624  
B. 4625  
C. 4672  
D. 1102  
E. 7045

---

### Soal 10
Event ID 4625 pada Security Log menunjukkan:

A. Successful logon  
B. Failed logon attempt  
C. New service installed  
D. User account created  
E. Audit log cleared

---

### Soal 11
Lokasi penyimpanan Windows Event Logs dalam format EVTX adalah:

A. `C:\Windows\Logs\`  
B. `C:\Windows\System32\winevt\Logs\`  
C. `C:\Users\[username]\AppData\Logs\`  
D. `C:\Windows\Temp\Logs\`  
E. `C:\ProgramData\Logs\`

---

### Soal 12
Pada Windows 10, file Prefetch dapat menyimpan hingga berapa timestamp eksekusi terakhir?

A. 1  
B. 4  
C. 8  
D. 16  
E. 32

---

### Soal 13
Informasi manakah yang TIDAK dapat diperoleh dari analisis file Prefetch?

A. Nama executable yang dijalankan  
B. Jumlah eksekusi (run count)  
C. Isi/konten file yang dijalankan  
D. Timestamp eksekusi terakhir  
E. File dan directory yang direferensikan saat eksekusi

---

### Soal 14
Dalam Recycle Bin Windows 10, file `$I` berfungsi untuk menyimpan:

A. Konten asli file yang dihapus  
B. Metadata file (path asli, ukuran, timestamp penghapusan)  
C. Thumbnail file yang dihapus  
D. Hash file yang dihapus  
E. Permission file yang dihapus

---

### Soal 15
Jump Lists tipe Automatic pada Windows dibuat secara otomatis ketika:

A. User melakukan pin pada taskbar  
B. User membuka file melalui aplikasi  
C. Sistem melakukan Windows Update  
D. User membuat shortcut desktop  
E. Aplikasi pertama kali diinstal

---

### Soal 16
Data browser artifacts pada Google Chrome disimpan dalam format database:

A. Microsoft Access (.mdb)  
B. MySQL  
C. SQLite  
D. PostgreSQL  
E. XML

---

### Soal 17
Tool yang dikembangkan oleh Eric Zimmerman untuk menganalisis Prefetch files adalah:

A. JLECmd  
B. LECmd  
C. PECmd  
D. EvtxECmd  
E. WxTCmd

---

### Soal 18
Artefak Windows manakah yang tetap menyimpan thumbnail gambar meskipun file asli sudah dihapus dan Recycle Bin dikosongkan?

A. Jump Lists  
B. LNK Files  
C. Prefetch  
D. Thumbnail Cache (thumbcache)  
E. Event Logs

---

### Soal 19
Dalam konteks forensik militer, alasan utama mengapa live forensics lebih sering dipilih dibanding dead forensics pada sistem C2 (Command and Control) adalah:

A. Live forensics lebih mudah dilakukan  
B. Dead forensics tidak menghasilkan bukti yang valid  
C. Sistem C2 tidak boleh dimatikan untuk menjaga kontinuitas operasi  
D. Live forensics menghasilkan forensic image yang lebih besar  
E. Dead forensics memerlukan izin komandan tertinggi

---

### Soal 20
Tool yang digunakan untuk membangun Super Timeline dengan menggabungkan timestamps dari berbagai sumber artefak forensik adalah:

A. FTK Imager  
B. Autopsy  
C. log2timeline / plaso  
D. Wireshark  
E. Volatility 3

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Jelaskan konsep order of volatility dan sebutkan urutan pengumpulan bukti dari data paling volatile hingga paling stabil!

---

### Soal 2 ⭐
Apa perbedaan antara live forensics dan dead forensics? Berikan contoh situasi di mana masing-masing pendekatan lebih tepat digunakan!

---

### Soal 3 ⭐
Sebutkan empat jenis data yang dapat diekstrak dari RAM dump dan jelaskan nilai forensiknya masing-masing!

---

### Soal 4 ⭐
Jelaskan perbedaan antara plugin `windows.pslist` dan `windows.psscan` pada Volatility 3. Mengapa membandingkan output keduanya penting dalam investigasi malware?

---

### Soal 5 ⭐⭐
Sebutkan dan jelaskan lima Event ID pada Windows Security Log yang paling penting untuk investigasi insiden keamanan!

---

### Soal 6 ⭐⭐
Jelaskan bagaimana Event ID 1102 dan Event ID 4720 saling terkait dalam konteks serangan siber. Berikan skenario investigasinya!

---

### Soal 7 ⭐⭐
Apa informasi forensik yang dapat diperoleh dari analisis Prefetch file? Bagaimana informasi ini membantu investigasi penggunaan tools hacking pada sistem militer?

---

### Soal 8 ⭐⭐
Jelaskan perbedaan antara Automatic Jump Lists dan Custom Jump Lists. Bagaimana keduanya saling melengkapi dalam investigasi forensik?

---

### Soal 9 ⭐⭐
Sebutkan lokasi penyimpanan data browser artifacts untuk Chrome, Firefox, dan Edge. Mengapa browser artifacts merupakan sumber bukti forensik yang sangat berharga?

---

### Soal 10 ⭐⭐
Jelaskan fungsi file `$I` dan `$R` dalam Recycle Bin Windows 10. Berikan contoh konkret dengan nama file dan isi metadata-nya!

---

### Soal 11 ⭐⭐
Jelaskan bagaimana LNK files dapat membantu mengidentifikasi penggunaan USB drive dalam kasus pencurian data. Sebutkan data forensik spesifik yang terkandung dalam LNK file!

---

### Soal 12 ⭐⭐⭐
Investigator menemukan proses `csrss.exe` berjalan dari `C:\Users\Public\csrss.exe` dengan koneksi ke IP 185.x.x.x port 443. Analisis temuan ini secara komprehensif menggunakan pengetahuan tentang hierarki proses Windows normal dan framework MITRE ATT&CK!

---

### Soal 13 ⭐⭐⭐
Jelaskan langkah-langkah membangun Super Timeline menggunakan KAPE, log2timeline/plaso, dan Timeline Explorer. Sebutkan minimal 8 sumber artefak yang diintegrasikan!

---

### Soal 14 ⭐⭐⭐
Bandingkan Volatility 3 dengan Volatility 2 dari aspek bahasa pemrograman, deteksi profil OS, arsitektur, dukungan OS modern, dan kecepatan. Jelaskan mengapa Volatility 3 lebih direkomendasikan untuk investigasi modern!

---

### Soal 15 ⭐⭐⭐
Rancang prosedur pengumpulan volatile data pada sistem Command and Control (C2) militer yang masih aktif beroperasi. Prosedur harus mencakup tools yang digunakan, urutan langkah, dan cara menjaga minimal footprint pada sistem target!

---

## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Eksfiltrasi Data Rahasia Militer via USB

#### Latar Belakang

Seorang personel di Kodam XIV/Hasanuddin dilaporkan mengakses file rahasia di luar jam kerja. Hasil monitoring awal menunjukkan aktivitas mencurigakan pada workstation miliknya (Windows 10, hostname: WS-KODAM14-023). Unit Siber TNI ditugaskan melakukan investigasi forensik.

#### Data yang Ditemukan

Dari hasil analisis awal terhadap forensic image workstation tersebut, ditemukan data berikut:

**Event Logs:**
- Event ID 4624 (Successful Logon): User `TNI_Kapten_A` login pada 2026-01-15 pukul 02:15 WIB (di luar jam kerja)
- Event ID 7045 (New Service Installed): USB Mass Storage service tercatat aktif pada 02:18 WIB
- Event ID 1102 (Audit Log Cleared): Security log dihapus pada 03:05 WIB

**Prefetch:**
- `EXPLORER.EXE-[hash].pf`: Last execution 02:19 WIB
- `XCOPY.EXE-[hash].pf`: Run count 3, last execution 02:45 WIB
- `CCLEANER.EXE-[hash].pf`: Run count 1, last execution 03:00 WIB

**Jump Lists:**
- Microsoft Word: `D:\Classified\OPSEC-Garuda-2026.docx` (accessed 02:22 WIB)
- Microsoft Excel: `D:\Classified\Personnel-Database.xlsx` (accessed 02:30 WIB)
- File Explorer: `E:\Backup\` (accessed 02:35 WIB)

**LNK Files:**
- Target: `E:\Backup\OPSEC-Garuda-2026.docx`
- Volume Serial: F8A2-B3C1
- Volume Label: KINGSTON_USB
- Machine ID: WS-KODAM14-023

**Recycle Bin:**
- `$IXYZ123.docx`: Original path `D:\Classified\temp_opsec.docx`, deleted 02:50 WIB
- `$RXYZ123.docx`: File berisi salinan sebagian dokumen OPSEC

**Browser History (Chrome):**
- 03:10 WIB: Akses ke `https://mega.nz/upload` (cloud storage)

#### Pertanyaan

1. Susun timeline kronologis lengkap aktivitas tersangka berdasarkan semua artefak yang ditemukan! (25 poin)

2. Identifikasi minimal 5 teknik anti-forensik yang dilakukan oleh tersangka dan jelaskan bagaimana masing-masing artefak berhasil merekam aktivitasnya meskipun ada upaya penghapusan jejak! (25 poin)

3. Berdasarkan temuan artefak, buatlah ringkasan eksekutif investigasi forensik yang mencakup: fakta kunci, bukti pendukung, kesimpulan, dan rekomendasi tindak lanjut! (25 poin)

4. Jika investigator hanya memiliki akses ke 3 sumber artefak saja (dari semua yang tersedia), artefak mana yang paling kritis untuk membuktikan eksfiltrasi data? Jelaskan alasannya! (25 poin)

---

### Studi Kasus 2: Serangan APT pada Server Pertahanan Udara

#### Latar Belakang

Tim CERT TNI mendeteksi anomali pada server yang mengelola data radar pertahanan udara di Komando Pertahanan Udara Nasional (Kohanudnas). Sistem monitoring mencatat lonjakan traffic keluar ke IP yang berlokasi di luar negeri. Server berbasis Windows Server 2019 dengan hostname SRV-RADAR-01.

#### Data yang Ditemukan

**Memory Dump (dianalisis dengan Volatility 3):**

Output `windows.pstree`:
```
PID    PPID   Name
4      0      System
...
672    560    services.exe
812    672    svchost.exe
844    672    svchost.exe
...
1204   672    svchost.exe
3456   2180   explorer.exe
4521   3456   csrss.exe          ← ANOMALI
5678   3456   svchost.exe        ← ANOMALI
```

Output `windows.netscan`:
```
Offset     Proto  Local           Foreign          State    PID
0x1234...  TCPv4  10.10.1.50:443  185.234.xx.xx:80 ESTAB    4521
0x5678...  TCPv4  10.10.1.50:445  185.234.xx.xx:443 ESTAB   5678
0x9abc...  TCPv4  10.10.1.50:135  192.168.1.20:445 ESTAB    5678
```

Output `windows.malfind` (ringkasan):
```
PID 4521 (csrss.exe): Suspicious memory region at 0x00400000
  - PAGE_EXECUTE_READWRITE protection
  - Contains MZ header (PE file in memory)
PID 5678 (svchost.exe): Suspicious memory region at 0x10000000
  - PAGE_EXECUTE_READWRITE protection
  - Obfuscated shellcode detected
```

Output `windows.cmdline`:
```
PID 4521: C:\Users\Public\csrss.exe -connect 185.234.xx.xx -port 80 -enc
PID 5678: C:\ProgramData\Microsoft\svchost.exe -lateral 192.168.1.20
```

**Event Logs:**
- Event ID 4624 (Logon Type 10 — Remote): User `SRV-RADAR-01\admin_backup` dari IP 192.168.1.15 pada 2026-01-20 01:30 WIB
- Event ID 4672: Special privileges assigned to `admin_backup`
- Event ID 7045: Service "Windows Performance Monitor" (path: `C:\ProgramData\Microsoft\svchost.exe`) installed pada 01:45 WIB
- Event ID 4720: User account `admin$` created pada 02:00 WIB
- Event ID 4104 (PowerShell): Script block berisi `Invoke-Mimikatz -DumpCreds` terdeteksi pada 02:15 WIB

**Prefetch:**
- `CSRSS.EXE-A1B2C3D4.pf`: Path `C:\Users\Public\`, run count 47, first execution 2026-01-10
- `SVCHOST.EXE-E5F6G7H8.pf`: Path `C:\ProgramData\Microsoft\`, run count 12
- `POWERSHELL.EXE-[hash].pf`: Run count 8, last execution 02:15 WIB

#### Pertanyaan

1. Identifikasi semua anomali pada output `windows.pstree`. Jelaskan mengapa setiap temuan dianggap anomali berdasarkan hierarki proses Windows normal! (20 poin)

2. Analisis output `windows.netscan` dan `windows.cmdline`. Identifikasi jenis komunikasi (C2, lateral movement, dll.) dan jelaskan tujuan masing-masing koneksi! (20 poin)

3. Petakan semua temuan ke framework MITRE ATT&CK. Identifikasi minimal 6 teknik yang digunakan oleh penyerang beserta Tactic dan Technique ID-nya! (20 poin)

4. Berdasarkan Prefetch file `CSRSS.EXE-A1B2C3D4.pf` yang menunjukkan first execution pada 10 Januari dan run count 47, apa yang dapat disimpulkan tentang durasi dan persistensi serangan? (20 poin)

5. Susun rencana incident response dan containment berdasarkan semua temuan di atas. Prioritaskan langkah-langkah berdasarkan urgensi dan dampak terhadap operasi pertahanan udara! (20 poin)

---

## Kunci Jawaban

### Bagian A: Pilihan Ganda

| No | Jawaban | Penjelasan |
|----|---------|------------|
| 1 | **B** | Berdasarkan order of volatility (RFC 3227), RAM/memory adalah data paling volatile yang harus dikumpulkan pertama karena hilang saat power off |
| 2 | **B** | Live forensics dilakukan pada sistem yang masih menyala untuk menangkap volatile data (RAM, proses, koneksi jaringan) |
| 3 | **C** | MBR (Master Boot Record) tersimpan di disk (sektor pertama hard drive), bukan di RAM. Data lainnya (proses, encryption keys, koneksi, clipboard) tersedia di memori |
| 4 | **C** | DumpIt adalah tool ringan untuk memory acquisition pada Windows. Autopsy untuk analisis disk, Wireshark untuk network, PhotoRec untuk file carving, HxD untuk hex editing |
| 5 | **C** | Volatility 3 mendukung auto-detection profil OS melalui symbol tables (ISF), tidak perlu memilih profil secara manual seperti Volatility 2 |
| 6 | **C** | `windows.malfind` mendeteksi memory regions yang mengandung injected code dengan proteksi PAGE_EXECUTE_READWRITE |
| 7 | **D** | `svchost.exe` yang sah selalu memiliki parent `services.exe`. Parent lain mengindikasikan malware yang menyamar |
| 8 | **C** | DKOM memanipulasi doubly-linked list EPROCESS di kernel untuk menghapus entry proses, menyembunyikannya dari pslist dan Task Manager |
| 9 | **D** | Event ID 1102 menunjukkan Security audit log telah dihapus — teknik anti-forensik untuk menghilangkan jejak |
| 10 | **B** | Event ID 4625 merekam setiap upaya login yang gagal, berguna untuk mendeteksi brute force attack |
| 11 | **B** | Event Logs format EVTX disimpan di `C:\Windows\System32\winevt\Logs\` sejak Windows Vista/Server 2008 |
| 12 | **C** | Windows 10/11 menyimpan hingga 8 timestamp eksekusi terakhir dalam file Prefetch (peningkatan dari 1 pada Windows XP/7) |
| 13 | **C** | Prefetch menyimpan metadata eksekusi (nama, run count, timestamps, referenced files) tetapi TIDAK menyimpan isi/konten file executable |
| 14 | **B** | File `$I` menyimpan metadata: path asli file, ukuran file, dan timestamp penghapusan. File `$R` menyimpan konten asli |
| 15 | **B** | Automatic Jump Lists dibuat otomatis oleh Windows setiap kali user membuka file melalui aplikasi (MRU — Most Recently Used) |
| 16 | **C** | Chrome, Firefox, dan Edge menyimpan data artifacts (history, cookies, login data) dalam format database SQLite |
| 17 | **C** | PECmd (Prefetch Explorer Command Line) adalah tool Eric Zimmerman untuk parsing Prefetch files. JLECmd untuk Jump Lists, LECmd untuk LNK files |
| 18 | **D** | Thumbnail Cache (thumbcache_*.db) menyimpan thumbnail gambar yang bertahan meskipun file asli dihapus dan Recycle Bin dikosongkan |
| 19 | **C** | Sistem C2 militer tidak boleh dimatikan karena mengganggu operasi pertahanan yang sedang berjalan. Live forensics memungkinkan pengumpulan bukti tanpa mematikan sistem |
| 20 | **C** | log2timeline/plaso mengekstrak dan menggabungkan timestamps dari berbagai sumber artefak forensik ke dalam satu Super Timeline terpadu |

---

### Bagian B: Uraian

#### Jawaban Soal 1 ⭐

**Order of Volatility** adalah urutan prioritas pengumpulan bukti digital berdasarkan tingkat kemudahan data hilang atau berubah, didefinisikan dalam RFC 3227.

Urutan dari paling volatile hingga paling stabil:

| Urutan | Sumber Data | Volatilitas |
|--------|-------------|-------------|
| 1 | CPU Registers & Cache | Sangat tinggi — nanoseconds |
| 2 | RAM / Memory | Tinggi — hilang saat power off |
| 3 | Network State | Tinggi — real-time, tidak disimpan |
| 4 | Running Processes | Tinggi — hilang saat power off |
| 5 | Disk (Temporary Data) | Sedang — dapat di-overwrite |
| 6 | Disk (Persistent Data) | Rendah — persisten |
| 7 | Remote Logging / SIEM | Sangat rendah — persisten di server |
| 8 | Backup / Archive | Sangat rendah — jangka panjang |

Prinsip utama: kumpulkan data paling volatile terlebih dahulu karena data tersebut paling cepat hilang.

---

#### Jawaban Soal 2 ⭐

| Aspek | Live Forensics | Dead Forensics |
|-------|---------------|----------------|
| **Kondisi** | Sistem masih menyala | Sistem sudah dimatikan |
| **Volatile Data** | Dapat diakses (RAM, proses, network) | Hilang secara permanen |
| **Risiko** | Mengubah state sistem | Kehilangan volatile data |
| **Kompleksitas** | Tinggi — harus cepat | Sedang — tanpa tekanan waktu |

**Situasi live forensics lebih tepat:**
- Sistem C2 militer yang tidak boleh dimatikan
- Mendeteksi fileless malware yang hanya ada di RAM
- Mengidentifikasi koneksi jaringan aktif ke C2 server

**Situasi dead forensics lebih tepat:**
- Analisis hard disk dari barang sitaan
- Workstation yang sudah dimatikan oleh tersangka
- Analisis media penyimpanan yang diserahkan sebagai bukti

---

#### Jawaban Soal 3 ⭐

| Data di RAM | Nilai Forensik |
|-------------|----------------|
| **Daftar proses aktif** | Mengidentifikasi malware atau program tidak sah yang berjalan, termasuk fileless malware |
| **Koneksi jaringan** | Mendeteksi komunikasi ke Command & Control server musuh dan data exfiltration |
| **Encryption keys** | Membuka data terenkripsi (BitLocker, TrueCrypt) tanpa memerlukan password |
| **Command history** | Merekonstruksi perintah CMD/PowerShell yang dijalankan oleh penyerang |

---

#### Jawaban Soal 4 ⭐

| Plugin | Mekanisme | Cakupan |
|--------|-----------|---------|
| `windows.pslist` | Menelusuri doubly-linked list EPROCESS dari kernel | Hanya proses yang terdaftar di linked list aktif |
| `windows.psscan` | Brute-force scan seluruh memori mencari struktur EPROCESS | Semua proses termasuk yang terminated atau tersembunyi |

**Pentingnya membandingkan:** Rootkit menggunakan teknik DKOM untuk memanipulasi linked list EPROCESS sehingga proses tersembunyi dari `pslist`. Proses yang muncul di `psscan` tetapi tidak di `pslist` kemungkinan besar disembunyikan oleh rootkit — ini merupakan indikator kuat adanya malware canggih.

---

#### Jawaban Soal 5 ⭐⭐

| Event ID | Deskripsi | Relevansi Forensik |
|----------|-----------|-------------------|
| **4624** | Successful Logon | Melacak siapa yang berhasil mengakses sistem, logon type menunjukkan metode (interactive, remote, network) |
| **4625** | Failed Logon | Mendeteksi upaya brute force — banyak gagal login dari IP yang sama dalam waktu singkat |
| **4720** | User Account Created | Mendeteksi pembuatan akun backdoor — terutama hidden accounts dengan suffix "$" |
| **7045** | New Service Installed | Mendeteksi persistence melalui instalasi service berbahaya |
| **1102** | Audit Log Cleared | Mendeteksi anti-forensik — penghapusan log menunjukkan upaya menghilangkan jejak |

---

#### Jawaban Soal 6 ⭐⭐

**Event ID 1102** (Audit Log Cleared) menunjukkan penyerang sengaja menghapus Security log untuk menghilangkan jejak aktivitas sebelumnya. Ini adalah teknik anti-forensik.

**Event ID 4720** (User Account Created) dengan username seperti "admin$" menunjukkan pembuatan hidden account sebagai backdoor. Suffix "$" menyembunyikan akun dari daftar user standar Windows.

**Skenario investigasi:** Penyerang pertama kali menghapus log (Event 1102) untuk menghilangkan jejak aktivitas awal, kemudian membuat akun backdoor (Event 4720) untuk memastikan akses berkelanjutan ke sistem. Urutan kronologis: clear log → buat backdoor account menunjukkan serangan terencana yang memerlukan incident response segera.

Meskipun penyerang menghapus log, Event 1102 sendiri tetap terekam karena merupakan event yang dicatat setelah penghapusan, sehingga investigator mengetahui bahwa upaya anti-forensik dilakukan.

---

#### Jawaban Soal 7 ⭐⭐

Informasi dari Prefetch file:

| Data | Deskripsi | Nilai Forensik |
|------|-----------|----------------|
| Nama executable | Program yang dijalankan | Konfirmasi program dieksekusi |
| Run count | Jumlah total eksekusi | Frekuensi penggunaan |
| Timestamps | Hingga 8 timestamp terakhir (Win10) | Timeline aktivitas |
| Referenced files | File/DLL yang di-load | Konteks penggunaan |
| Volume info | Drive saat eksekusi | Identifikasi media eksternal |

**Contoh investigasi tools hacking:** Jika ditemukan `MIMIKATZ.EXE-[hash].pf` di folder Prefetch, ini membuktikan bahwa Mimikatz (tool credential dumping) pernah dijalankan pada sistem militer. Run count menunjukkan berapa kali dieksekusi, timestamps menunjukkan kapan, dan volume info dapat mengidentifikasi apakah tool dijalankan dari USB drive eksternal. Bahkan jika penyerang sudah menghapus file Mimikatz dari disk, file Prefetch tetap menjadi bukti eksekusi.

---

#### Jawaban Soal 8 ⭐⭐

| Aspek | Automatic Jump Lists | Custom Jump Lists |
|-------|---------------------|-------------------|
| **Dibuat oleh** | Windows secara otomatis | Aplikasi tertentu |
| **Konten** | File terbaru yang dibuka per aplikasi | Pinned items, frequent items, custom tasks |
| **Pemicu** | Setiap kali file dibuka | Saat user melakukan pin atau aplikasi menyimpan |
| **Lokasi** | AutomaticDestinations\ | CustomDestinations\ |
| **Format** | .automaticDestinations-ms | .customDestinations-ms |

**Saling melengkapi:** Automatic memberikan histori komprehensif semua file yang pernah dibuka (termasuk file yang sudah dihapus), sedangkan Custom menunjukkan file yang dianggap penting oleh pengguna (di-pin). Automatic menjawab "apa yang pernah dibuka?", Custom menjawab "apa yang dianggap penting?". Kombinasi keduanya membangun profil aktivitas dan prioritas pengguna secara lengkap.

---

#### Jawaban Soal 9 ⭐⭐

| Browser | Lokasi Profil |
|---------|--------------|
| **Chrome** | `%LocalAppData%\Google\Chrome\User Data\Default\` |
| **Firefox** | `%AppData%\Mozilla\Firefox\Profiles\[profile]\` |
| **Edge** | `%LocalAppData%\Microsoft\Edge\User Data\Default\` |

Browser artifacts sangat berharga karena merekam hampir semua aktivitas online pengguna: situs yang dikunjungi (history), data sesi (cookies), salinan halaman web (cache), file yang diunduh (downloads), kata kunci pencarian, kredensial tersimpan (login data), dan data formulir (autofill). Database dalam format SQLite memudahkan analisis menggunakan tools standar. Bahkan setelah user menghapus history di browser, data masih dapat ditemukan melalui analisis database SQLite atau file carving.

---

#### Jawaban Soal 10 ⭐⭐

| File | Fungsi |
|------|--------|
| **$I[ID]** | Menyimpan metadata: path asli file, ukuran file, timestamp penghapusan |
| **$R[ID]** | Menyimpan konten asli file yang dihapus |

**Contoh konkret:** Jika file `D:\Operasi\rencana_serbu.pdf` (ukuran 2.5 MB) dihapus pada 15 Januari 2026 pukul 14:30:

- `$I9X2K3L.pdf` berisi:
  - Header/Version: 2 (Windows 10)
  - File Size: 2,621,440 bytes
  - Deletion Time: 2026-01-15 14:30:00 (FILETIME format)
  - Original Path: `D:\Operasi\rencana_serbu.pdf` (Unicode string)

- `$R9X2K3L.pdf` berisi: file PDF asli lengkap dengan semua kontennya

Kedua file berlokasi di `C:\$Recycle.Bin\[SID]\` dengan ID yang sama, memungkinkan korelasi metadata dengan konten asli.

---

#### Jawaban Soal 11 ⭐⭐

Data forensik dalam LNK file yang membantu identifikasi USB:

| Data LNK | Contoh | Bukti USB |
|----------|--------|-----------|
| **Target path** | `E:\Classified\secret.docx` | File diakses dari drive eksternal (E:\) |
| **Volume serial number** | F8A2-B3C1 | Identifikasi unik USB drive spesifik |
| **Volume label** | KINGSTON_USB | Nama/merk USB drive |
| **Machine ID** | WS-KODAM-023 | Komputer di mana USB dicolokkan |
| **Timestamps** | Created, Modified, Accessed | Kapan file di USB diakses |
| **MAC address** | 00:1A:2B:3C:4D:5E | Identifikasi perangkat jaringan asal |

LNK file tetap ada di folder Recent meskipun USB sudah dicabut dan file asli sudah tidak tersedia, sehingga menjadi bukti kuat bahwa file tertentu pernah diakses dari media penyimpanan eksternal tertentu pada waktu tertentu.

---

#### Jawaban Soal 12 ⭐⭐⭐

**Analisis anomali proses:**

| Aspek | Normal | Temuan | Status |
|-------|--------|--------|--------|
| Lokasi csrss.exe | `C:\Windows\System32\` | `C:\Users\Public\` | **ANOMALI KRITIS** |
| Parent csrss.exe | smss.exe | explorer.exe | **ANOMALI KRITIS** |
| Koneksi csrss.exe | Tidak ada koneksi keluar | 185.x.x.x:443 (ESTABLISHED) | **ANOMALI KRITIS** |

**Mapping MITRE ATT&CK:**

| Tactic | Technique | Temuan |
|--------|-----------|--------|
| **Defense Evasion** | Masquerading (T1036) | Nama file meniru proses sistem Windows |
| **Persistence** | Public folder location | Folder writable semua user untuk persistensi |
| **Command & Control** | Encrypted Channel (T1573) | Port 443 = HTTPS terenkripsi ke IP eksternal |
| **Execution** | Command-line arguments | Parameter `-connect -port -enc` menunjukkan C2 client |

**Kesimpulan:** Ini adalah malware APT yang menyamar sebagai proses sistem kritis Windows. `csrss.exe` asli hanya ada di `System32`, tidak melakukan koneksi jaringan keluar, dan memiliki parent `smss.exe`. Ketiga anomali ini mengkonfirmasi bahwa file di `C:\Users\Public\` adalah malware C2 agent, bukan proses Windows yang sah.

**Langkah lanjutan:** Gunakan `windows.malfind` untuk analisis injected code, `windows.dlllist` untuk loaded modules, periksa Prefetch untuk first execution timestamp, dan lakukan threat intelligence terhadap IP 185.x.x.x.

---

#### Jawaban Soal 13 ⭐⭐⭐

**Langkah membangun Super Timeline:**

| Langkah | Aksi | Tool | Output |
|---------|------|------|--------|
| 1 | Kumpulkan forensic image | FTK Imager | Raw/E01 image |
| 2 | Ekstraksi artefak otomatis | KAPE | Parsed artifacts (CSV) |
| 3 | Jalankan log2timeline pada image | plaso | Plaso storage file |
| 4 | Filter berdasarkan timeframe | psort | CSV timeline |
| 5 | Buka dan analisis | Timeline Explorer | Visualisasi interaktif |
| 6 | Filter, korelasi, dan dokumentasi | Timeline Explorer | Focused report |

**Minimal 8 sumber artefak yang diintegrasikan:**

1. **$MFT timestamps** — Created, Modified, Accessed, Entry Modified dari setiap file
2. **Event Logs (EVTX)** — Security, System, Application, PowerShell
3. **Prefetch files** — Timestamp eksekusi program
4. **Jump Lists** — Timestamp akses file per aplikasi
5. **LNK files** — Timestamp target file dan metadata
6. **Browser artifacts** — History, downloads, cookies timestamps
7. **Recycle Bin ($I files)** — Timestamp penghapusan file
8. **Windows Registry** — Last written timestamps pada registry keys
9. **ActivitiesCache.db** — Windows Timeline activity timestamps
10. **Thumbnail Cache** — Kapan thumbnail pertama kali dibuat

Super Timeline menggabungkan semua timestamps ini ke satu format kronologis sehingga investigator dapat melihat seluruh aktivitas pada sistem dalam satu tampilan terintegrasi.

---

#### Jawaban Soal 14 ⭐⭐⭐

| Aspek | Volatility 2 | Volatility 3 |
|-------|-------------|--------------|
| **Bahasa** | Python 2 (end-of-life) | Python 3 (aktif dikembangkan) |
| **Profil OS** | Manual profile selection — rawan kesalahan | Auto-detection via symbol tables (ISF) |
| **Arsitektur** | Monolitik | Modular dengan layer system |
| **Support Windows** | Hingga Windows 10 awal | Windows 10/11 terbaru + Server 2019/2022 |
| **Kecepatan** | Lebih lambat | Lebih cepat (optimized parsing) |
| **Plugin System** | Plugin terpisah per OS | Unified plugin framework |
| **Maintenance** | Deprecated, tidak aktif | Aktif dikembangkan oleh Volatility Foundation |

**Mengapa Volatility 3 lebih direkomendasikan:**

1. **Future-proof**: Python 2 sudah end-of-life sejak 2020, tidak mendapat security updates
2. **Akurasi**: Auto-detection menghilangkan risiko kesalahan pemilihan profil
3. **Kompatibilitas**: Mendukung OS modern yang digunakan dalam infrastruktur pertahanan
4. **Performance**: Arsitektur yang dioptimasi memungkinkan analisis memory dump besar secara efisien
5. **Ekstensibilitas**: Symbol tables dalam format ISF lebih mudah di-update untuk OS baru

---

#### Jawaban Soal 15 ⭐⭐⭐

**Prosedur pengumpulan volatile data pada sistem C2 aktif:**

**Persiapan (sebelum menyentuh sistem target):**
- Siapkan USB forensik read-only berisi tools: DumpIt/Belkasoft RAM Capturer, Sysinternals Suite, batch scripts
- Siapkan USB terpisah (write) untuk output — jangan simpan output di sistem target
- Verifikasi hash semua tools forensik sebelum deployment
- Dokumentasikan kondisi awal: foto layar, catat waktu mulai

**Urutan langkah (dari paling volatile):**

| Prioritas | Langkah | Tool | Output | Footprint |
|-----------|---------|------|--------|-----------|
| 1 | Memory dump | DumpIt / Belkasoft | memory.raw | Minimal — berjalan di background |
| 2 | Network state | `netstat -anob` | netstat.txt | Sangat minimal |
| 3 | Process list | `tasklist /v` + Process Explorer | processes.txt | Minimal |
| 4 | DNS cache | `ipconfig /displaydns` | dns_cache.txt | Sangat minimal |
| 5 | ARP cache | `arp -a` | arp_cache.txt | Sangat minimal |
| 6 | Logged-on users | `query user` | users.txt | Sangat minimal |
| 7 | Salin Event Logs | `xcopy winevt\Logs\` | EVTX files | Minimal |
| 8 | Salin Prefetch | `xcopy Prefetch\` | PF files | Minimal |

**Cara menjaga minimal footprint:**
- Semua tools dijalankan dari USB read-only (bukan dari disk target)
- Output ditulis ke USB terpisah (bukan ke disk target)
- Tidak menginstal software apapun pada sistem target
- Dokumentasikan setiap perubahan state yang terjadi
- Hash semua file yang dikumpulkan untuk chain of custody
- Seluruh proses tidak memerlukan restart atau shutdown sistem C2

---

### Bagian C: Studi Kasus

#### Studi Kasus 1: Eksfiltrasi Data Rahasia Militer via USB

**Jawaban 1 — Timeline Kronologis (25 poin):**

| Waktu (WIB) | Sumber Artefak | Aktivitas | Signifikansi |
|-------------|----------------|-----------|--------------|
| 02:15 | Event Log 4624 | Login user TNI_Kapten_A | Akses di luar jam kerja — anomali |
| 02:18 | Event Log 7045 | USB Mass Storage service installed | USB drive dicolokkan |
| 02:19 | Prefetch | EXPLORER.EXE dieksekusi | Navigasi file system |
| 02:22 | Jump Lists (Word) | `D:\Classified\OPSEC-Garuda-2026.docx` dibuka | Akses dokumen rahasia OPSEC |
| 02:30 | Jump Lists (Excel) | `D:\Classified\Personnel-Database.xlsx` dibuka | Akses database personel |
| 02:35 | Jump Lists (Explorer) | `E:\Backup\` diakses | Navigasi ke USB drive |
| 02:35–02:45 | LNK Files | `E:\Backup\OPSEC-Garuda-2026.docx` (Volume: KINGSTON_USB) | Dokumen OPSEC disalin ke USB |
| 02:45 | Prefetch | XCOPY.EXE (run count 3) | Penyalinan file massal ke USB |
| 02:50 | Recycle Bin | `temp_opsec.docx` dihapus dari D:\Classified\ | Penghapusan salinan sementara |
| 03:00 | Prefetch | CCLEANER.EXE dieksekusi | **Anti-forensik**: upaya hapus jejak |
| 03:05 | Event Log 1102 | Security audit log dihapus | **Anti-forensik**: hapus log keamanan |
| 03:10 | Browser (Chrome) | Akses mega.nz/upload | **Double exfiltration**: upload ke cloud storage |

**Jawaban 2 — Teknik Anti-Forensik (25 poin):**

| No | Teknik Anti-Forensik | Artefak yang Tetap Merekam | Penjelasan |
|----|---------------------|--------------------------|------------|
| 1 | Menjalankan CCleaner untuk hapus jejak | **Prefetch**: CCLEANER.EXE-[hash].pf | Prefetch membuktikan CCleaner dijalankan, justru menjadi bukti upaya anti-forensik |
| 2 | Menghapus Security audit log (Event 1102) | **Event Log**: Event 1102 sendiri tetap terekam | Windows mencatat event penghapusan log setelah log dihapus |
| 3 | Menghapus file sementara ke Recycle Bin | **Recycle Bin**: $I dan $R files | Metadata dan konten file masih tersedia di $Recycle.Bin |
| 4 | Mengakses di luar jam kerja (dini hari) | **Event Log 4624**: timestamp login | Event log mencatat waktu login yang tepat |
| 5 | Menggunakan XCOPY untuk penyalinan | **Prefetch**: XCOPY.EXE dengan run count | Prefetch merekam penggunaan tool penyalinan |

**Jawaban 3 — Ringkasan Eksekutif (25 poin):**

**RINGKASAN EKSEKUTIF INVESTIGASI FORENSIK**

**Kasus:** Dugaan Eksfiltrasi Data Rahasia — WS-KODAM14-023

**Fakta Kunci:**
- Personel TNI_Kapten_A login pada pukul 02:15 WIB (di luar jam kerja)
- USB drive KINGSTON_USB (serial: F8A2-B3C1) dicolokkan ke workstation
- Dua dokumen rahasia diakses: OPSEC-Garuda-2026.docx dan Personnel-Database.xlsx
- Dokumen OPSEC dikonfirmasi disalin ke USB drive melalui LNK file dan XCOPY
- Upaya upload tambahan ke mega.nz terdeteksi dari browser history
- Tiga teknik anti-forensik digunakan: CCleaner, penghapusan log, penghapusan file sementara

**Kesimpulan:** Dengan tingkat kepercayaan TINGGI, personel TNI_Kapten_A melakukan eksfiltrasi dokumen rahasia melalui USB drive dan berpotensi melalui cloud storage. Bukti multi-artefak saling mengkonfirmasi dan upaya anti-forensik justru memperkuat indikasi kesengajaan.

**Rekomendasi:**
1. Sita USB drive KINGSTON_USB (serial F8A2-B3C1) untuk analisis lebih lanjut
2. Blokir akun TNI_Kapten_A dan lakukan audit akses terhadap semua sistem yang pernah diakses
3. Monitor akun mega.nz untuk menentukan apakah data sudah berhasil di-upload
4. Review kebijakan akses waktu kerja dan implementasikan pembatasan login di luar jam dinas
5. Lakukan analisis damage assessment terhadap dokumen yang berpotensi bocor

**Jawaban 4 — Tiga Artefak Paling Kritis (25 poin):**

| Prioritas | Artefak | Alasan |
|-----------|---------|--------|
| 1 | **LNK Files** | Memberikan bukti langsung: path file di USB (E:\), volume serial untuk identifikasi USB spesifik, machine ID, dan timestamps. Satu artefak ini membuktikan file rahasia ada di USB drive tertentu |
| 2 | **Event Logs** | Membuktikan siapa (TNI_Kapten_A), kapan (02:15 WIB — di luar jam kerja), dan bahwa USB dicolokkan (Event 7045). Event 1102 menunjukkan upaya anti-forensik yang menguatkan kesengajaan |
| 3 | **Jump Lists** | Menunjukkan dokumen spesifik yang diakses (OPSEC-Garuda-2026.docx, Personnel-Database.xlsx) dan path USB (E:\Backup\), mengkonfirmasi akses ke file rahasia dan ke media eksternal |

Ketiga artefak ini memberikan bukti who (Event Logs), what (Jump Lists — dokumen apa), where (LNK — ke USB mana), dan when (timestamps dari ketiganya), membentuk narasi investigasi yang komprehensif.

---

#### Studi Kasus 2: Serangan APT pada Server Pertahanan Udara

**Jawaban 1 — Anomali pada pstree (20 poin):**

| Proses | PID | PPID | Anomali | Penjelasan |
|--------|-----|------|---------|------------|
| `csrss.exe` | 4521 | 3456 (explorer.exe) | **Parent salah** | csrss.exe yang sah memiliki parent smss.exe, BUKAN explorer.exe. Ini malware yang menyamar |
| `csrss.exe` | 4521 | — | **Lokasi salah** | Dari cmdline: path `C:\Users\Public\` — csrss.exe asli hanya di `C:\Windows\System32\` |
| `svchost.exe` | 5678 | 3456 (explorer.exe) | **Parent salah** | svchost.exe yang sah memiliki parent services.exe (PID 672), BUKAN explorer.exe |
| `svchost.exe` | 5678 | — | **Lokasi salah** | Dari cmdline: path `C:\ProgramData\Microsoft\` — svchost.exe asli hanya di `System32` |
| Kedua proses | 4521, 5678 | 3456 | **Diluncurkan dari explorer** | Proses sistem tidak pernah diluncurkan dari explorer.exe — ini mengindikasikan user-level execution |

**Jawaban 2 — Analisis Netscan dan Cmdline (20 poin):**

| PID | Koneksi | Cmdline | Jenis Komunikasi | Tujuan |
|-----|---------|---------|-------------------|--------|
| 4521 | 10.10.1.50:443 → 185.234.xx.xx:80 | `-connect 185.234.xx.xx -port 80 -enc` | **C2 Communication** | Komunikasi terenkripsi ke Command & Control server di luar negeri |
| 5678 | 10.10.1.50:445 → 185.234.xx.xx:443 | — | **C2 Secondary Channel** | Kanal C2 cadangan menggunakan port 443 (HTTPS) untuk redundansi |
| 5678 | 10.10.1.50:135 → 192.168.1.20:445 | `-lateral 192.168.1.20` | **Lateral Movement** | Menyebar ke sistem internal lain (192.168.1.20) menggunakan SMB (port 445) |

Server 10.10.1.50 (SRV-RADAR-01) berkomunikasi dengan C2 eksternal DAN melakukan penyebaran ke jaringan internal — menunjukkan serangan APT yang sudah dalam tahap lanjut.

**Jawaban 3 — Mapping MITRE ATT&CK (20 poin):**

| No | Tactic | Technique ID | Technique | Temuan |
|----|--------|-------------|-----------|--------|
| 1 | **Initial Access** | T1078 | Valid Accounts | Login menggunakan akun admin_backup via Remote Desktop (Logon Type 10) |
| 2 | **Execution** | T1059.001 | PowerShell | Script block `Invoke-Mimikatz -DumpCreds` terdeteksi di Event 4104 |
| 3 | **Persistence** | T1543.003 | Windows Service | Service "Windows Performance Monitor" palsu diinstal (Event 7045) |
| 4 | **Persistence** | T1136.001 | Local Account | Hidden account `admin$` dibuat (Event 4720) |
| 5 | **Defense Evasion** | T1036.005 | Masquerading: Match Legitimate Name | Malware menyamar sebagai csrss.exe dan svchost.exe |
| 6 | **Credential Access** | T1003 | OS Credential Dumping | Mimikatz digunakan untuk dump kredensial |
| 7 | **Lateral Movement** | T1021.002 | SMB/Windows Admin Shares | Koneksi ke 192.168.1.20:445 dengan parameter `-lateral` |
| 8 | **Command & Control** | T1573 | Encrypted Channel | Komunikasi C2 terenkripsi (parameter `-enc`) ke IP eksternal |

**Jawaban 4 — Analisis Durasi dan Persistensi (20 poin):**

Dari Prefetch `CSRSS.EXE-A1B2C3D4.pf`:
- **First execution:** 10 Januari 2026
- **Run count:** 47 kali
- **Investigasi saat ini:** 20 Januari 2026

**Kesimpulan:**
- **Durasi serangan minimal 10 hari** (10 Jan – 20 Jan) — menunjukkan APT berjangka panjang
- **Rata-rata eksekusi:** 47 kali dalam 10 hari ≈ 4-5 kali per hari — mengindikasikan malware yang berjalan secara periodik (scheduled task atau service-based persistence)
- **Persistensi:** Dikonfirmasi oleh instalasi service palsu (Event 7045) yang memastikan malware berjalan otomatis setelah reboot
- **Severity:** Sangat tinggi — selama 10 hari penyerang memiliki akses ke data radar pertahanan udara
- **Damage assessment:** Dengan 47 kali eksekusi dan koneksi C2 aktif, volume data yang berpotensi dieksfiltrasi sangat signifikan

**Jawaban 5 — Rencana Incident Response (20 poin):**

**PRIORITAS TINGGI (segera, dalam 1 jam):**

| No | Langkah | Alasan |
|----|---------|--------|
| 1 | Isolasi SRV-RADAR-01 dari jaringan | Hentikan komunikasi C2 dan lateral movement |
| 2 | Block IP 185.234.xx.xx di semua firewall | Putuskan koneksi ke C2 server |
| 3 | Isolasi host 192.168.1.20 | Sistem sudah terinfeksi via lateral movement |

**PRIORITAS SEDANG (dalam 4 jam):**

| No | Langkah | Alasan |
|----|---------|--------|
| 4 | Disable akun admin_backup dan admin$ | Hapus akses penyerang ke semua sistem |
| 5 | Reset kredensial semua akun privileged | Mimikatz telah dump kredensial — semua password compromised |
| 6 | Memory dump dan forensic image SRV-RADAR-01 dan 192.168.1.20 | Preservasi bukti lengkap |
| 7 | Scan seluruh jaringan untuk IOC | Identifikasi sistem lain yang mungkin terinfeksi |

**PRIORITAS RENDAH (dalam 24 jam):**

| No | Langkah | Alasan |
|----|---------|--------|
| 8 | Analisis forensik mendalam (Super Timeline) | Rekonstruksi lengkap aktivitas penyerang |
| 9 | Threat intelligence terhadap IP 185.234.xx.xx | Attribution dan identifikasi threat actor |
| 10 | Review dan perkuat kebijakan keamanan | Cegah serangan serupa di masa depan |
| 11 | Laporan insiden ke pimpinan dan badan terkait | Kepatuhan prosedur pelaporan |

**Catatan penting:** Fungsi radar pertahanan udara harus segera dialihkan ke sistem backup selama proses containment dan recovery untuk memastikan tidak ada celah dalam pertahanan udara nasional.

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

## Referensi

1. Carvey, H. (2022). *Windows Forensic Analysis* (5th Ed.). Academic Press. Chapter 3-5.
2. Casey, E. (2022). *Digital Evidence and Computer Crime* (4th Ed.). Academic Press. Chapter 9.
3. Ligh, M.H., et al. (2014). *The Art of Memory Forensics*. Wiley. Chapter 1-5.
4. Volatility Foundation. (2024). *Volatility 3 Documentation*. https://volatility3.readthedocs.io/
5. Zimmerman, E. (2024). *Eric Zimmerman's Tools*. https://ericzimmerman.github.io/
6. SANS Institute. (2023). *Windows Forensic Analysis Poster*.
7. RFC 3227: Guidelines for Evidence Collection and Archiving.
8. MITRE ATT&CK Framework. https://attack.mitre.org/

---

## License

This repository is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

Commercial use is permitted, provided attribution is given to the author.

© 2026 Anindito
