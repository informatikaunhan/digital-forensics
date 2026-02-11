# Modul 11: Forensik Malware

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 11  
**Topik:** Forensik Malware  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Mengklasifikasikan berbagai jenis malware dan memahami karakteristik masing-masing
2. Menjelaskan malware delivery mechanisms dan infection vectors dalam konteks ancaman militer
3. Melakukan static analysis terhadap file mencurigakan menggunakan tools forensik
4. Memahami prinsip dynamic analysis dan penggunaan sandbox environments
5. Mengidentifikasi persistence mechanisms yang digunakan malware
6. Menganalisis malware communication patterns termasuk C2 dan beaconing
7. Menerapkan memory forensics untuk deteksi dan analisis malware

---

## 1. Pengantar Forensik Malware

### 1.1 Definisi dan Ruang Lingkup

> **Forensik Malware (Malware Forensics)** adalah cabang forensik digital yang berfokus pada identifikasi, analisis, dan dokumentasi perangkat lunak berbahaya (malicious software) untuk memahami fungsi, asal-usul, dampak, dan atribusi malware dalam konteks investigasi insiden keamanan.

Forensik malware berbeda dari analisis malware murni karena memiliki dimensi hukum dan evidentiary. Dalam konteks militer, forensik malware menjadi komponen kritis dalam:

1. **Cyber Defense Operations**: Memahami ancaman yang menyerang infrastruktur pertahanan
2. **Incident Response**: Merespons insiden malware pada sistem jaringan Kodam, Lantamal, dan instalasi TNI
3. **Attribution**: Mengidentifikasi aktor di balik serangan siber terhadap aset pertahanan
4. **Intelligence Gathering**: Mengekstrak IOC (Indicators of Compromise) untuk threat intelligence

| Aspek | Analisis Malware | Forensik Malware |
|-------|-----------------|------------------|
| **Tujuan** | Memahami fungsi malware | Mendukung investigasi dan proses hukum |
| **Fokus** | Reverse engineering | Chain of custody dan dokumentasi |
| **Output** | Technical report | Laporan forensik yang dapat diterima hukum |
| **Konteks** | Security research | Investigasi insiden keamanan |
| **Standar** | Best practices industri | Standar forensik dan prosedur legal |

### 1.2 Ancaman Malware dalam Konteks Pertahanan

Dalam lingkungan militer Indonesia, ancaman malware dapat dikategorikan berdasarkan tingkat sofistikasi dan aktor:

| Kategori | Aktor | Target Militer | Contoh |
|----------|-------|----------------|--------|
| **Commodity Malware** | Kriminal umum | Komputer individu personel | Ransomware, banking trojan |
| **Targeted Attack** | Kelompok terorganisir | Jaringan unit tertentu | Spear phishing dengan RAT |
| **APT (Advanced Persistent Threat)** | State-sponsored | Infrastruktur kritis pertahanan | Custom implant, zero-day |
| **Cyber Weapon** | Negara | Sistem kontrol industri militer | Stuxnet-like malware |

![Tingkat Sofistikasi Ancaman Malware](images/p11-01-malware-threat-spectrum.png)

*Gambar 11.1: Spektrum ancaman malware berdasarkan sofistikasi dan aktor*

#### Solved Problem 1 ⭐

**Soal:** Jelaskan perbedaan antara analisis malware dan forensik malware, serta mengapa forensik malware lebih relevan dalam konteks investigasi insiden keamanan militer!

**Penyelesaian:**

*Step 1:* Identifikasi aspek perbandingan utama

Analisis malware dan forensik malware memiliki tujuan, metodologi, dan output yang berbeda meskipun menggunakan teknik yang serupa.

*Step 2:* Jelaskan perbedaan kritis

| Aspek | Analisis Malware | Forensik Malware |
|-------|-----------------|------------------|
| **Tujuan Utama** | Memahami cara kerja malware | Mendukung investigasi hukum/militer |
| **Chain of Custody** | Tidak diperlukan | Wajib dijaga ketat |
| **Dokumentasi** | Technical notes | Laporan forensik formal |
| **Validitas Hukum** | Tidak menjadi pertimbangan | Harus admissible di pengadilan/tribunal militer |

*Step 3:* Kontekstualisasi untuk militer

**Jawaban:** Dalam konteks militer, forensik malware lebih relevan karena: (1) Hasil analisis harus dapat digunakan dalam proses hukum militer maupun sipil, (2) Chain of custody harus dijaga untuk memastikan integritas bukti digital, (3) Dokumentasi harus mengikuti standar forensik agar dapat mendukung atribusi serangan terhadap aset pertahanan, dan (4) Temuan harus dapat dikomunikasikan kepada pimpinan operasional untuk pengambilan keputusan strategis.

---

## 2. Klasifikasi Malware

### 2.1 Taksonomi Malware

> **Malware (Malicious Software)** adalah perangkat lunak yang dirancang dengan sengaja untuk menyebabkan kerusakan, mengganggu operasi, mencuri data, atau memperoleh akses tidak sah ke sistem komputer atau jaringan.

Klasifikasi malware berdasarkan karakteristik dan perilakunya:

#### 2.1.1 Virus

Virus adalah malware yang memerlukan host file untuk menyebar. Virus menempelkan diri pada file executable atau dokumen dan aktif ketika host file dijalankan.

**Karakteristik:**
- Memerlukan interaksi pengguna untuk penyebaran awal
- Memodifikasi file host (parasitic behavior)
- Dapat mereplikasi diri ke file lain dalam sistem
- Jenis: file infector, boot sector virus, macro virus, polymorphic virus

**Contoh Forensik:** Ketika menginvestigasi workstation di kantor Kodam yang terinfeksi virus, investigator akan memeriksa perubahan pada file executable, khususnya modifikasi pada PE header dan penambahan section baru.

#### 2.1.2 Worm

Worm adalah malware yang dapat menyebar secara mandiri melalui jaringan tanpa memerlukan host file atau interaksi pengguna.

**Karakteristik:**
- Self-propagating melalui jaringan
- Tidak memerlukan host file
- Mengeksploitasi vulnerability jaringan untuk penyebaran
- Dapat menyebabkan denial of service karena traffic berlebihan

**Relevansi Militer:** Worm seperti Conficker pernah menginfeksi jaringan militer beberapa negara. Dalam konteks jaringan TNI, worm dapat menyebar antar unit melalui jaringan internal yang saling terhubung.

#### 2.1.3 Trojan

Trojan adalah malware yang menyamar sebagai software legitimate untuk mengelabui pengguna agar menginstalnya.

**Jenis-jenis Trojan:**

| Jenis | Fungsi | Indikator Forensik |
|-------|--------|-------------------|
| **RAT (Remote Access Trojan)** | Memberikan remote control penuh | Koneksi outbound persisten, registry persistence |
| **Banking Trojan** | Mencuri kredensial finansial | Hook pada browser, form grabbing artifacts |
| **Downloader** | Mengunduh payload tambahan | HTTP/HTTPS requests ke domain mencurigakan |
| **Dropper** | Menyimpan dan mengeksekusi payload | File creation di temp directories |
| **Keylogger** | Merekam keystroke | Log files tersembunyi, hook pada keyboard API |

#### 2.1.4 Ransomware

> **Ransomware** adalah malware yang mengenkripsi file korban dan meminta tebusan (ransom) untuk mendekripsi data tersebut. Ransomware merupakan salah satu ancaman paling signifikan terhadap infrastruktur digital pertahanan.

**Tipe Ransomware:**

1. **Crypto Ransomware**: Mengenkripsi file pengguna dengan algoritma kriptografi kuat (AES-256, RSA-2048)
2. **Locker Ransomware**: Mengunci akses ke sistem operasi tanpa mengenkripsi file
3. **Double Extortion**: Mengenkripsi dan mengeksfiltras data, mengancam publikasi
4. **Ransomware-as-a-Service (RaaS)**: Model bisnis di mana pembuat ransomware menyewakan infrastruktur

**Artefak Forensik Ransomware:**
- Ransom note files (README.txt, DECRYPT_FILES.html)
- File terenkripsi dengan ekstensi baru (.encrypted, .locked, .crypted)
- Registry entries untuk persistence
- Network traffic ke C2 server dan payment infrastructure
- Event log entries menunjukkan mass file modification

#### 2.1.5 Advanced Persistent Threat (APT)

APT bukan jenis malware tunggal, melainkan kampanye serangan terkoordinasi yang menggunakan berbagai malware dan teknik.

**Karakteristik APT:**
- Disponsori oleh negara atau organisasi dengan sumber daya besar
- Bertujuan spionase jangka panjang
- Menggunakan custom malware dan zero-day exploits
- Multi-stage attack dengan lateral movement
- Low-and-slow approach untuk menghindari deteksi

**Kill Chain APT:**

```
Reconnaissance → Weaponization → Delivery → Exploitation → 
Installation → Command & Control → Actions on Objectives
```

#### Solved Problem 2 ⭐

**Soal:** Sebuah sistem di Lantamal X menunjukkan gejala berikut: file-file dokumen berubah ekstensi menjadi ".mil_encrypted", muncul file "BACA_INI.txt" di setiap folder, dan ditemukan koneksi outbound ke IP di luar negeri. Identifikasi jenis malware dan artefak forensik yang harus dikumpulkan!

**Penyelesaian:**

*Step 1:* Identifikasi jenis malware berdasarkan gejala

Gejala menunjukkan **Crypto Ransomware** dengan kemungkinan double extortion:
- Perubahan ekstensi file → enkripsi file
- Ransom note (BACA_INI.txt) → permintaan tebusan
- Koneksi outbound → komunikasi dengan C2 atau exfiltration

*Step 2:* Daftar artefak forensik yang harus dikumpulkan

| Kategori | Artefak | Lokasi/Sumber |
|----------|---------|---------------|
| **File System** | Ransom note, encrypted files, malware executable | Disk image |
| **Registry** | Run/RunOnce keys, scheduled tasks | NTUSER.DAT, SOFTWARE hive |
| **Memory** | Running processes, encryption keys | RAM dump |
| **Network** | C2 connections, DNS queries | PCAP, firewall logs |
| **Event Logs** | Process creation, file modifications | Windows Event Logs |

**Jawaban:** Malware tersebut adalah crypto ransomware. Artefak yang harus dikumpulkan meliputi: (1) Salinan ransom note untuk analisis variant, (2) Sample file terenkripsi dan file asli untuk analisis enkripsi, (3) Memory dump untuk kemungkinan ekstraksi encryption key, (4) Network logs untuk identifikasi C2 infrastructure, dan (5) Registry dan event logs untuk rekonstruksi timeline infeksi.

---

### 2.2 Malware Delivery Mechanisms

Pemahaman tentang mekanisme penyebaran malware penting untuk analisis forensik karena menentukan titik awal investigasi.

| Delivery Method | Deskripsi | Artefak Forensik |
|----------------|-----------|------------------|
| **Phishing Email** | Email dengan attachment/link malicious | Email headers, attachment metadata |
| **Drive-by Download** | Eksploitasi browser saat mengunjungi website | Browser cache, download history |
| **USB/Removable Media** | Malware menyebar via USB drive | Autorun.inf, USB history di registry |
| **Watering Hole** | Kompromi website yang sering dikunjungi target | Browser history, DNS logs |
| **Supply Chain** | Kompromi software update atau vendor | Software installation logs, hash mismatch |
| **Exploit Kit** | Framework otomatis untuk eksploitasi vulnerability | Landing page artifacts, redirect chains |

#### Solved Problem 3 ⭐⭐

**Soal:** Seorang perwira di Mabes TNI menerima email dengan subjek "Undangan Rapat Koordinasi Lintas Matra" yang berisi attachment file Word. Setelah dibuka, komputer mulai menunjukkan perilaku anomali. Jelaskan langkah-langkah analisis forensik untuk menentukan delivery mechanism dan initial infection vector!

**Penyelesaian:**

*Step 1:* Analisis email sebagai delivery vector

```
Periksa:
1. Email header → trace routing, sender verification (SPF/DKIM/DMARC)
2. Attachment metadata → file hash, creation date, author
3. Macro content → VBA code analysis
```

*Step 2:* Timeline reconstruction

```
Timeline yang harus direkonstruksi:
1. Waktu email diterima (email server logs)
2. Waktu attachment dibuka (Prefetch, file access times)
3. Proses yang spawn setelah pembukaan dokumen (Sysmon/Event Log 4688)
4. Aktivitas jaringan setelah infeksi (firewall/proxy logs)
```

*Step 3:* Identifikasi infection chain

```
Email diterima → Attachment dibuka → Macro dieksekusi → 
PowerShell/cmd spawned → Payload downloaded → Persistence established
```

**Jawaban:** Delivery mechanism adalah spear phishing dengan weaponized document. Langkah analisis: (1) Ekstrak dan analisis email header untuk verifikasi asal-usul, (2) Analisis macro pada dokumen Word menggunakan olevba/oletools, (3) Periksa Prefetch files dan Event Log 4688 untuk timeline eksekusi, (4) Analisis network logs untuk identifikasi payload download dan C2, (5) Periksa registry untuk persistence mechanism yang dibuat malware.

---

## 3. Static Analysis

### 3.1 Konsep Static Analysis

> **Static Analysis** adalah teknik analisis malware yang dilakukan tanpa mengeksekusi (menjalankan) file malware. Analisis ini berfokus pada pemeriksaan properti file, strings, imports, dan struktur internal binary.

Static analysis merupakan langkah pertama dalam analisis malware karena relatif aman (tidak menjalankan malware) dan dapat menghasilkan informasi awal yang berharga.

**Keuntungan Static Analysis:**
- Aman karena malware tidak dieksekusi
- Memberikan gambaran menyeluruh tentang kemampuan malware
- Dapat mengidentifikasi strings, URLs, IP addresses yang di-embed
- Mengidentifikasi library dan API calls yang digunakan

**Keterbatasan Static Analysis:**
- Dapat dikalahkan oleh obfuscation dan packing
- Tidak dapat mengobservasi perilaku runtime
- Encrypted strings tidak terlihat tanpa dekripsi
- Polymorphic malware dapat mengubah signature

### 3.2 File Properties Analysis

Langkah pertama static analysis adalah memeriksa properti dasar file:

**Informasi yang Diperiksa:**

1. **File Hash (MD5, SHA-1, SHA-256)**: Identifikasi unik file
2. **File Type**: Verifikasi tipe file sebenarnya vs ekstensi
3. **File Size**: Perbandingan dengan ukuran normal
4. **Timestamps**: Creation, modification, access time
5. **Digital Signature**: Verifikasi sertifikat digital

**Tool: PEStudio**

PEStudio adalah tool gratis untuk analisis file PE (Portable Executable) Windows yang menyediakan informasi komprehensif.

```
Informasi yang ditampilkan PEStudio:
- File indicators (hash values, entropy)
- Imported libraries dan functions
- Strings yang terdeteksi
- Sections analysis (entropy per section)
- Resources (embedded files, icons)
- Certificate verification
- VirusTotal lookup
```

### 3.3 Strings Analysis

> **Strings Analysis** adalah proses mengekstrak dan menganalisis string yang readable (teks) dari dalam binary file. String dapat mengungkap URLs, IP addresses, registry keys, pesan error, dan informasi penting lainnya.

**Tipe Strings:**
- **ASCII Strings**: Teks standar dalam encoding ASCII
- **Unicode Strings**: Teks dalam encoding Unicode (UTF-16)
- **Obfuscated Strings**: String yang di-encode atau dienkripsi

**Tool: FLOSS (FireEye Labs Obfuscated String Solver)**

FLOSS adalah tool dari Mandiant yang dapat mengekstrak string biasa maupun string yang di-obfuscate.

```bash
# Ekstraksi string dasar
floss malware_sample.exe

# Hanya string yang di-decode
floss --only decoded malware_sample.exe

# Output ke file
floss malware_sample.exe > strings_output.txt

# Dengan minimum length
floss -n 8 malware_sample.exe
```

**String yang Dicari dalam Analisis Forensik:**

| Kategori | Contoh String | Signifikansi |
|----------|--------------|--------------|
| **URLs/IPs** | `http://evil.com/gate.php`, `192.168.x.x` | C2 infrastructure |
| **Registry Keys** | `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` | Persistence |
| **File Paths** | `C:\Windows\Temp\payload.exe` | Drop locations |
| **API Names** | `CreateRemoteThread`, `VirtualAllocEx` | Code injection capability |
| **Commands** | `cmd.exe /c`, `powershell -enc` | Execution methods |
| **Credentials** | `admin`, `password`, Base64 strings | Hardcoded credentials |
| **Mutex Names** | `Global\MalwareMutex123` | Instance control |

#### Solved Problem 4 ⭐⭐

**Soal:** Dari hasil strings analysis sebuah sample malware, ditemukan strings berikut. Analisis signifikansi masing-masing string:

```
http://103.45.67.89/update/config.php
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
CreateRemoteThread
VirtualAllocEx
WriteProcessMemory
cmd.exe /c whoami
Global\MutexCheckRunning
Mozilla/5.0 (Windows NT 10.0; Win64; x64)
```

**Penyelesaian:**

*Step 1:* Kategorisasi strings

| String | Kategori | Analisis |
|--------|----------|----------|
| `http://103.45.67.89/update/config.php` | **C2 Server** | URL menuju server C2 untuk konfigurasi/perintah |
| `HKLM\...\Run` | **Persistence** | Registry key untuk auto-start saat boot |
| `CreateRemoteThread` | **Code Injection** | API untuk injeksi kode ke proses lain |
| `VirtualAllocEx` | **Code Injection** | API untuk alokasi memori di proses remote |
| `WriteProcessMemory` | **Code Injection** | API untuk menulis ke memori proses remote |
| `cmd.exe /c whoami` | **Reconnaissance** | Perintah untuk identifikasi user yang sedang login |
| `Global\MutexCheckRunning` | **Mutex** | Mencegah multiple instance malware berjalan |
| `Mozilla/5.0...` | **User-Agent** | Menyamarkan traffic sebagai browser normal |

*Step 2:* Rekonstruksi perilaku

**Jawaban:** Strings menunjukkan malware dengan kemampuan: (1) Berkomunikasi dengan C2 server di IP 103.45.67.89, (2) Menetapkan persistence melalui registry Run key, (3) Melakukan process injection menggunakan trifecta API (VirtualAllocEx → WriteProcessMemory → CreateRemoteThread), (4) Melakukan reconnaissance pada sistem target, (5) Menggunakan mutex untuk single-instance execution, dan (6) Menyamarkan traffic dengan User-Agent browser legitimate.

---

### 3.4 PE File Structure Analysis

File executable Windows menggunakan format PE (Portable Executable). Memahami struktur PE penting untuk analisis malware.

**Komponen Utama PE File:**

```
┌─────────────────────────────┐
│     DOS Header (MZ)         │  ← "MZ" signature (0x4D5A)
├─────────────────────────────┤
│     DOS Stub                │  ← "This program cannot be run in DOS mode"
├─────────────────────────────┤
│     PE Signature            │  ← "PE\0\0" (0x50450000)
├─────────────────────────────┤
│     COFF File Header        │  ← Machine type, number of sections
├─────────────────────────────┤
│     Optional Header         │  ← Entry point, image base, subsystem
├─────────────────────────────┤
│     Section Headers         │  ← .text, .data, .rdata, .rsrc
├─────────────────────────────┤
│     Section Data            │  ← Actual code and data
│     .text (code)            │
│     .data (initialized data)│
│     .rdata (read-only data) │
│     .rsrc (resources)       │
└─────────────────────────────┘
```

**Sections yang Relevan untuk Forensik:**

| Section | Fungsi Normal | Anomali Mencurigakan |
|---------|--------------|---------------------|
| **.text** | Kode executable | Entropy tinggi (packed/encrypted) |
| **.data** | Data terinitialisasi | Ukuran sangat besar (embedded payload) |
| **.rdata** | Data read-only, import table | Import table yang minimal (packed) |
| **.rsrc** | Resource (icons, strings) | File executable di dalam resource |
| **UPX0/UPX1** | Tidak ada | Indikasi packing dengan UPX |
| **Nama aneh** | Tidak ada | Section dengan nama random menunjukkan custom packer |

### 3.5 Import Analysis

> **Import Analysis** adalah pemeriksaan terhadap fungsi-fungsi API Windows yang diimpor oleh sebuah executable. Import table mengungkap kemampuan potensial malware.

**API Calls yang Mencurigakan:**

| Kategori | API Functions | Indikasi |
|----------|--------------|----------|
| **Process Injection** | `CreateRemoteThread`, `VirtualAllocEx`, `WriteProcessMemory` | Injeksi kode ke proses lain |
| **Keylogging** | `SetWindowsHookEx`, `GetAsyncKeyState`, `GetKeyState` | Pencatatan keystroke |
| **File Operations** | `CreateFile`, `WriteFile`, `DeleteFile` | Manipulasi file |
| **Registry** | `RegSetValueEx`, `RegCreateKeyEx` | Persistence, konfigurasi |
| **Network** | `InternetOpen`, `HttpSendRequest`, `WSAStartup` | Komunikasi jaringan |
| **Crypto** | `CryptEncrypt`, `CryptGenKey`, `CryptAcquireContext` | Enkripsi (ransomware) |
| **Anti-Debug** | `IsDebuggerPresent`, `CheckRemoteDebuggerPresent` | Deteksi analisis |
| **Screen Capture** | `BitBlt`, `GetDC`, `GetDesktopWindow` | Screenshot/screen recording |

#### Solved Problem 5 ⭐⭐

**Soal:** Sebuah file PE yang ditemukan di workstation personel Korem memiliki karakteristik berikut: ukuran file 45 KB, hanya memiliki 2 sections (.text dan .rsrc), entropy section .text = 7.8, dan hanya mengimpor 3 fungsi dari kernel32.dll (LoadLibraryA, GetProcAddress, VirtualAlloc). Apa yang dapat disimpulkan dari analisis ini?

**Penyelesaian:**

*Step 1:* Analisis entropy

Entropy section .text = 7.8 (mendekati maksimum 8.0) mengindikasikan data yang sangat acak, yang merupakan ciri khas:
- File yang di-pack (compressed)
- File yang di-encrypt
- Data acak/noise

*Step 2:* Analisis imports

Hanya 3 import: `LoadLibraryA`, `GetProcAddress`, `VirtualAlloc`
- `LoadLibraryA`: Memuat DLL secara dinamis
- `GetProcAddress`: Mendapatkan alamat fungsi dari DLL
- `VirtualAlloc`: Mengalokasikan memori

Ini adalah pattern klasik **packed executable** yang akan:
1. Mengalokasikan memori baru (VirtualAlloc)
2. Mendekripsi/decompress kode asli ke memori tersebut
3. Memuat library yang diperlukan secara dinamis (LoadLibraryA)
4. Menyelesaikan alamat fungsi saat runtime (GetProcAddress)

*Step 3:* Kesimpulan

**Jawaban:** File tersebut hampir pasti merupakan **packed executable**. Indikatornya: (1) Entropy tinggi (7.8) menunjukkan konten terenkripsi/terkompresi, (2) Import table minimal dengan hanya 3 fungsi yang merupakan pattern khas unpacking stub, (3) Ukuran kecil (45 KB) konsisten dengan packed binary. Langkah selanjutnya adalah mencoba unpacking menggunakan tools seperti UPX atau manual unpacking melalui dynamic analysis untuk mendapatkan binary asli.

---

## 4. Dynamic Analysis

### 4.1 Konsep Dynamic Analysis

> **Dynamic Analysis** adalah teknik analisis malware yang dilakukan dengan mengeksekusi (menjalankan) malware dalam lingkungan yang terkontrol dan terisolasi (sandbox) sambil memantau perilakunya secara real-time.

Dynamic analysis melengkapi static analysis dengan memberikan gambaran tentang perilaku aktual malware saat dijalankan.

**Perbandingan Static vs Dynamic Analysis:**

| Aspek | Static Analysis | Dynamic Analysis |
|-------|----------------|-----------------|
| **Eksekusi Malware** | Tidak | Ya, dalam sandbox |
| **Risiko** | Minimal | Perlu isolasi ketat |
| **Obfuscation** | Sulit menembus | Bypass packing/encryption |
| **Waktu** | Relatif cepat | Memerlukan waktu observasi |
| **Coverage** | Semua kode (termasuk dead code) | Hanya kode yang tereksekusi |
| **Tools** | PEStudio, FLOSS, strings | Process Monitor, Wireshark, sandbox |

### 4.2 Sandbox Environment Setup

> **Sandbox** adalah lingkungan terisolasi yang digunakan untuk mengeksekusi malware secara aman tanpa risiko menginfeksi sistem produksi.

**Prinsip Setup Sandbox:**

1. **Isolasi Jaringan**: Sandbox harus terisolasi dari jaringan produksi
2. **Snapshot**: Ambil snapshot VM sebelum eksekusi untuk easy rollback
3. **Monitoring Tools**: Instal tools monitoring sebelum eksekusi malware
4. **Simulasi Jaringan**: Gunakan FakeNet-NG untuk simulasi layanan jaringan
5. **Logging**: Aktifkan verbose logging pada semua monitoring tools

**Arsitektur Sandbox:**

```
┌────────────────────────────────────────────────┐
│                HOST MACHINE                     │
│  ┌──────────────────────────────────────────┐  │
│  │          ANALYSIS VM (Sandbox)            │  │
│  │                                           │  │
│  │  ┌──────────┐  ┌──────────┐  ┌────────┐  │  │
│  │  │ ProcMon  │  │ ProcExp  │  │Wireshark│  │  │
│  │  └──────────┘  └──────────┘  └────────┘  │  │
│  │  ┌──────────┐  ┌──────────┐  ┌────────┐  │  │
│  │  │ FakeNet  │  │ Regshot  │  │  Sysmon │  │  │
│  │  └──────────┘  └──────────┘  └────────┘  │  │
│  │                                           │  │
│  │  [Malware Sample]  ← Eksekusi di sini    │  │
│  └──────────────────────────────────────────┘  │
│                                                 │
│  Network: Host-only / Isolated                  │
│  Snapshot: Taken before execution               │
└────────────────────────────────────────────────┘
```

### 4.3 Behavioral Analysis Tools

#### 4.3.1 Process Monitor (ProcMon)

Process Monitor dari Sysinternals memantau aktivitas file system, registry, dan process/thread secara real-time.

**Filter yang Berguna untuk Analisis Malware:**

| Filter | Konfigurasi | Tujuan |
|--------|-------------|--------|
| **Process Name** | `is <malware.exe>` | Filter aktivitas malware spesifik |
| **Operation** | `is WriteFile` | Monitor file creation/modification |
| **Operation** | `is RegSetValue` | Monitor registry modification |
| **Path** | `contains \Run` | Deteksi persistence via Run key |
| **Path** | `contains \Temp` | File drop di temp directory |

```
Contoh output ProcMon yang menunjukkan persistence:
Time        Process         Operation      Path                                    Detail
10:45:01    malware.exe     RegSetValue    HKLM\...\Run\svchost                   Data: C:\Users\...\svchost.exe
10:45:02    malware.exe     CreateFile     C:\Users\Admin\AppData\Local\Temp\...   SUCCESS
10:45:03    malware.exe     WriteFile      C:\Users\Admin\AppData\Local\Temp\...   Length: 145920
```

#### 4.3.2 Process Explorer (ProcExp)

Process Explorer menampilkan informasi detail tentang proses yang berjalan, termasuk parent-child relationships.

**Informasi yang Diperoleh:**
- Process tree (parent-child relationships)
- Loaded DLLs per process
- Open handles (files, registry keys, network connections)
- Thread details dan stack traces
- VirusTotal hash lookup

#### 4.3.3 FakeNet-NG

> **FakeNet-NG** adalah tool dari Mandiant yang mensimulasikan layanan jaringan (DNS, HTTP, HTTPS, SMTP, dll.) sehingga malware "berpikir" telah terhubung ke internet, memungkinkan analisis network behavior tanpa koneksi nyata.

```
Contoh output FakeNet-NG:
[DNS] malware.exe query: evil-c2-server.com → 192.168.1.100 (faked)
[HTTP] malware.exe GET http://evil-c2-server.com/gate.php?id=VICTIM001
[HTTP] malware.exe POST http://evil-c2-server.com/exfil.php (Data: 4096 bytes)
```

#### Solved Problem 6 ⭐⭐

**Soal:** Selama dynamic analysis di sandbox, malware menunjukkan perilaku berikut dalam 5 menit pertama eksekusi:

1. Membuat file `C:\Windows\System32\svcupdate.exe`
2. Menambah registry key `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run\WindowsUpdate`
3. Membuat scheduled task "SystemHealthCheck" yang berjalan setiap 30 menit
4. Menghubungi `update.microsoft-service[.]com` via HTTPS
5. Mengeksekusi `whoami`, `ipconfig /all`, `net user`, `systeminfo`

Kategorikan setiap perilaku dan jelaskan signifikansinya!

**Penyelesaian:**

| # | Perilaku | Kategori | Signifikansi |
|---|----------|----------|-------------|
| 1 | Copy ke System32 | **Persistence + Evasion** | Menempatkan diri di lokasi trusted system |
| 2 | Registry Run key | **Persistence** | Auto-start saat boot, primary persistence |
| 3 | Scheduled Task | **Persistence (Backup)** | Secondary persistence mechanism |
| 4 | HTTPS ke domain palsu | **C2 Communication** | Domain typosquatting microsoft-service[.]com |
| 5 | System commands | **Discovery/Reconnaissance** | Mengumpulkan informasi sistem target |

**Jawaban:** Malware ini menunjukkan perilaku RAT (Remote Access Trojan) yang sophisticated. Fase pertama adalah establishing persistence melalui dua mekanisme redundan (registry Run key dan scheduled task). Malware kemudian melakukan discovery commands untuk profiling sistem target sebelum berkomunikasi dengan C2 server menggunakan domain yang menyerupai layanan Microsoft legitimate (typosquatting). Penggunaan HTTPS menunjukkan upaya menghindari network detection.

---

## 5. Persistence Mechanisms

### 5.1 Pengertian Persistence

> **Persistence** adalah teknik yang digunakan malware untuk memastikan keberadaannya tetap bertahan di sistem meskipun setelah reboot, log off, atau restart. Pemahaman tentang persistence mechanisms kritis untuk analisis forensik karena menjadi artefak yang paling sering ditemukan.

### 5.2 Kategori Persistence Mechanisms

#### 5.2.1 Registry-Based Persistence

Registry adalah lokasi persistence yang paling umum:

| Registry Key | Scope | Timing |
|-------------|-------|--------|
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` | All users | Login |
| `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` | Current user | Login |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce` | All users | Login (sekali) |
| `HKLM\SYSTEM\CurrentControlSet\Services` | System | Boot |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell` | All users | Login |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Userinit` | All users | Login |

#### 5.2.2 Scheduled Tasks

```xml
<!-- Contoh malicious scheduled task -->
<Task>
  <Triggers>
    <TimeTrigger>
      <Repetition>
        <Interval>PT30M</Interval>  <!-- Setiap 30 menit -->
      </Repetition>
    </TimeTrigger>
  </Triggers>
  <Actions>
    <Exec>
      <Command>C:\Windows\Temp\backdoor.exe</Command>
    </Exec>
  </Actions>
</Task>
```

#### 5.2.3 DLL Hijacking dan Side-Loading

DLL hijacking memanfaatkan urutan pencarian DLL Windows untuk memuat DLL malicious:

```
Search Order:
1. Directory dari executable
2. System directory (C:\Windows\System32)
3. 16-bit system directory
4. Windows directory
5. Current directory
6. PATH environment variable directories
```

Malware menempatkan DLL dengan nama yang sama di directory dengan prioritas lebih tinggi.

#### 5.2.4 Persistence Lainnya

| Mekanisme | Deskripsi | Artefak Forensik |
|-----------|-----------|-----------------|
| **WMI Event Subscription** | Event consumer yang menjalankan malware | WMI repository |
| **Startup Folder** | Shortcut di folder startup | `%AppData%\...\Start Menu\Programs\Startup` |
| **COM Hijacking** | Mengganti CLSID legitimate | Registry CLSID entries |
| **Boot Record** | Modifikasi MBR/VBR | Boot sector analysis |
| **Image File Execution Options** | Debugger redirect | IFEO registry key |

#### Solved Problem 7 ⭐⭐⭐

**Soal:** Investigator menemukan malware yang menggunakan tiga mekanisme persistence secara simultan pada workstation di Kodam VI/Mulawarman. Jelaskan mengapa malware menggunakan multiple persistence dan bagaimana investigator harus menangani remediation!

**Penyelesaian:**

*Step 1:* Alasan multiple persistence

Malware menggunakan beberapa persistence mechanisms untuk:
1. **Redundancy**: Jika satu mekanisme terdeteksi dan dihapus, yang lain masih aktif
2. **Defense Evasion**: Setiap mekanisme mungkin menggunakan binary yang berbeda
3. **Escalation**: Beberapa mekanisme mungkin berjalan dengan privilege level berbeda
4. **Recovery**: Malware dapat merestorasi persistence yang dihapus menggunakan mekanisme lain

*Step 2:* Prosedur remediation

```
1. Identifikasi SEMUA persistence mechanisms sebelum memulai removal
2. Dokumentasikan setiap persistence (screenshot, registry export)
3. Identifikasi file executable yang dirujuk setiap persistence
4. Verifikasi tidak ada mechanism tambahan yang terlewat
5. Lakukan removal secara SIMULTAN untuk mencegah re-infection
6. Reboot dan verifikasi malware tidak kembali
7. Monitor sistem selama beberapa hari post-remediation
```

**Jawaban:** Malware menggunakan multiple persistence untuk redundancy dan resilience. Investigator harus: (1) Melakukan comprehensive scan untuk mengidentifikasi SEMUA persistence sebelum remediation, (2) Mendokumentasikan setiap mekanisme untuk keperluan forensik, (3) Melakukan removal secara simultan agar malware tidak dapat re-establish persistence, (4) Memverifikasi keberhasilan removal pasca-reboot, dan (5) Mempertimbangkan reimaging sistem jika tingkat kompromisasi terlalu dalam.

---

## 6. Malware Communication: C2, Beaconing, dan DGA

### 6.1 Command and Control (C2)

> **Command and Control (C2)** adalah infrastruktur komunikasi yang digunakan malware untuk menerima perintah dari operator dan mengirimkan data yang dicuri. C2 merupakan komponen kritis dalam arsitektur malware karena memungkinkan kontrol jarak jauh terhadap sistem yang terinfeksi.

**Tipe C2 Communication:**

| Tipe | Deskripsi | Deteksi |
|------|-----------|---------|
| **HTTP/HTTPS** | Komunikasi melalui web protocols | Proxy logs, unusual HTTP patterns |
| **DNS Tunneling** | Data dikirim melalui DNS queries | Anomali DNS (panjang query, frekuensi) |
| **Social Media** | Perintah disisipkan di platform sosial | Traffic ke social media APIs |
| **Custom Protocol** | Protokol komunikasi khusus | Unusual port usage, protocol anomaly |
| **P2P (Peer-to-Peer)** | Komunikasi antar bot tanpa server sentral | Unusual inter-host communication |

### 6.2 Beaconing

> **Beaconing** adalah pola komunikasi di mana malware secara periodik menghubungi C2 server untuk memeriksa perintah baru. Beaconing memiliki interval yang dapat diidentifikasi melalui analisis traffic.

**Karakteristik Beaconing:**

```
Contoh Beacon Pattern (HTTP):
[10:00:00] GET /check?id=abc123 → 200 OK (no commands)
[10:05:00] GET /check?id=abc123 → 200 OK (no commands)
[10:10:00] GET /check?id=abc123 → 200 OK {"cmd": "upload", "file": "*.docx"}
[10:15:00] POST /upload?id=abc123 [encrypted data]
[10:20:00] GET /check?id=abc123 → 200 OK (no commands)
```

**Jitter**: Malware sophisticated menambahkan variasi waktu (jitter) pada interval beaconing untuk menghindari deteksi pattern matching.

```
Tanpa jitter: 300s, 300s, 300s, 300s  ← Mudah terdeteksi
Dengan jitter (20%): 285s, 312s, 298s, 327s  ← Lebih sulit terdeteksi
```

### 6.3 Domain Generation Algorithm (DGA)

> **DGA (Domain Generation Algorithm)** adalah teknik di mana malware secara algoritmik menghasilkan nama domain untuk C2 communication. Hanya operator yang mengetahui domain mana yang akan aktif (registered) pada waktu tertentu.

**Cara Kerja DGA:**

```python
# Contoh sederhana DGA (untuk edukasi)
import datetime
import hashlib

def generate_domain(date, seed="malware_seed"):
    """Generate domain name based on date and seed"""
    date_str = date.strftime("%Y%m%d")
    hash_input = f"{date_str}{seed}"
    hash_result = hashlib.md5(hash_input.encode()).hexdigest()
    domain = hash_result[:12] + ".com"
    return domain

# Output: berbeda setiap hari
# 2025-01-15 → "a1b2c3d4e5f6.com"
# 2025-01-16 → "f6e5d4c3b2a1.com"
```

**Deteksi DGA:**
- Domain dengan entropy tinggi (banyak karakter acak)
- Banyak DNS query yang gagal (NXDOMAIN) karena domain belum diregistrasi
- Pattern yang tidak konsisten dengan penamaan domain normal
- Volume DNS queries yang tidak wajar

#### Solved Problem 8 ⭐⭐⭐

**Soal:** Analisis log DNS dari jaringan Kodam XII/Tanjungpura menunjukkan satu workstation membuat 500+ DNS queries dalam 1 jam ke domain-domain berikut:

```
a3f29bc1d4e7.net → NXDOMAIN
b7e1c3a2f9d0.net → NXDOMAIN
c9d4e7f2a1b3.net → NXDOMAIN
d2f7a9c1e4b3.net → 103.45.67.89
```

Jelaskan apa yang terjadi dan bagaimana menanganinya!

**Penyelesaian:**

*Step 1:* Identifikasi pola

- 500+ queries dalam 1 jam → Volume anomali
- Domain names dengan karakter random → High entropy
- Mayoritas NXDOMAIN → Domain belum diregistrasi
- Satu domain resolve ke IP → Ini kemungkinan domain C2 yang aktif

*Step 2:* Diagnosis

Ini adalah pattern klasik **DGA-based malware**:
1. Malware mengenerate ratusan domain secara algoritmik
2. Mayoritas domain tidak diregistrasi (NXDOMAIN)
3. Operator hanya mendaftarkan beberapa domain
4. Ketika malware menemukan domain yang resolve, komunikasi C2 dimulai

*Step 3:* Penanganan

**Jawaban:** Workstation tersebut terinfeksi malware yang menggunakan DGA. Langkah penanganan: (1) Isolasi workstation dari jaringan segera, (2) Capture memory dump sebelum shutdown, (3) Block IP 103.45.67.89 di firewall, (4) Analisis malware untuk mengidentifikasi DGA algorithm dan memprediksi domain masa depan, (5) Implement DNS sinkholing untuk domain-domain DGA yang diprediksi, (6) Scan semua workstation di jaringan Kodam untuk malware yang sama.

---

## 7. Memory Forensics untuk Malware Detection

### 7.1 Mengapa Memory Forensics Penting untuk Malware

> **Memory Forensics** dalam konteks malware adalah analisis RAM (Random Access Memory) dump untuk mendeteksi dan menganalisis malware yang sedang berjalan, termasuk malware yang menggunakan teknik fileless atau in-memory execution.

Memory forensics kritis karena:
1. **Unpacked Code**: Malware yang di-pack akan ter-unpack di memori
2. **Fileless Malware**: Malware yang hanya berjalan di memori tanpa file di disk
3. **Encryption Keys**: Kunci enkripsi ransomware mungkin masih di memori
4. **Injected Code**: Kode yang di-inject ke proses legitimate hanya terlihat di memori
5. **Network Connections**: Koneksi aktif ke C2 server

### 7.2 Volatility 3 untuk Malware Detection

Tool utama untuk memory forensics adalah Volatility 3 (seperti yang telah dibahas pada [Pertemuan 05](../p05/modul.md)).

**Plugin Volatility untuk Malware Detection:**

| Plugin | Fungsi | Deteksi Malware |
|--------|--------|----------------|
| `windows.pslist` | Daftar proses | Proses dengan nama mencurigakan |
| `windows.pstree` | Process tree | Parent-child relationship anomali |
| `windows.malfind` | Deteksi injected code | Code injection dan hollowing |
| `windows.cmdline` | Command line arguments | Command line mencurigakan |
| `windows.netscan` | Koneksi jaringan | C2 connections |
| `windows.dlllist` | Loaded DLLs | DLL injection |
| `windows.handles` | Open handles | File dan registry yang diakses |
| `windows.svcscan` | Windows services | Malicious services |

### 7.3 Teknik Deteksi Malware via Memory

#### 7.3.1 Process Analysis

```bash
# Daftar proses
python3 vol.py -f memory.dmp windows.pslist

# Process tree untuk melihat parent-child relationships
python3 vol.py -f memory.dmp windows.pstree

# Anomali yang dicari:
# - svchost.exe yang bukan child dari services.exe
# - cmd.exe/powershell.exe yang child dari WINWORD.EXE
# - Proses dengan nama mirip sistem tapi typo (scvhost.exe)
# - Proses dengan PID anomali atau timestamps tidak konsisten
```

#### 7.3.2 Code Injection Detection (Malfind)

```bash
# Deteksi injected/hollowed code
python3 vol.py -f memory.dmp windows.malfind

# Output menunjukkan:
# - Proses dengan memory regions PAGE_EXECUTE_READWRITE
# - Disassembly dari kode yang di-inject
# - Memory sections tanpa backing file di disk
```

**Indikator Code Injection:**
- Memory page dengan protection `PAGE_EXECUTE_READWRITE` (RWX)
- Region memori yang berisi kode executable tanpa backing file
- Header MZ/PE di dalam memory region proses yang bukan executable
- Alamat entry point yang menunjuk ke luar image base

#### 7.3.3 Network Connection Analysis

```bash
# Analisis koneksi jaringan
python3 vol.py -f memory.dmp windows.netscan

# Hal yang dicari:
# - Koneksi ke IP external yang tidak dikenal
# - Listening ports yang tidak standar
# - Proses sistem yang membuat koneksi internet
# - Multiple connections ke IP yang sama (beaconing)
```

#### Solved Problem 9 ⭐⭐⭐

**Soal:** Hasil `windows.pstree` dari memory dump workstation di Dislitbangad menunjukkan:

```
PID    PPID   Name              
4      0      System            
88     4      Registry          
512    4      smss.exe          
640    528    csrss.exe         
720    528    wininit.exe       
748    720    services.exe      
800    748    svchost.exe       
852    748    svchost.exe       
1024   748    svchost.exe       
1200   2840   svchost.exe     ← Anomali!
2840   1024   explorer.exe      
3100   2840   chrome.exe        
3200   3100   chrome.exe        
4500   2840   WINWORD.EXE       
4600   4500   cmd.exe         ← Anomali!
4700   4600   powershell.exe  ← Anomali!
```

Identifikasi anomali dan jelaskan analisisnya!

**Penyelesaian:**

*Step 1:* Identifikasi anomali pada process tree

**Anomali 1: svchost.exe (PID 1200, PPID 2840)**
- `svchost.exe` seharusnya adalah child dari `services.exe` (PID 748)
- PPID 2840 adalah `explorer.exe` → ini bukan parent yang legitimate
- Kemungkinan: malware yang menyamar sebagai svchost.exe

**Anomali 2: cmd.exe (PID 4600, PPID 4500 = WINWORD.EXE)**
- `cmd.exe` yang di-spawn oleh Microsoft Word sangat mencurigakan
- Ini adalah indikasi kuat **macro malware** atau exploit

**Anomali 3: powershell.exe (PID 4700, PPID 4600 = cmd.exe)**
- PowerShell yang dipanggil dari cmd yang di-spawn Word
- Classic attack chain: Word → Macro → cmd → PowerShell → Payload

*Step 2:* Rekonstruksi attack chain

```
WINWORD.EXE (PID 4500)
  → cmd.exe (PID 4600)        ← Macro execution
    → powershell.exe (PID 4700) ← Payload download/execution
      → [payload activity]

explorer.exe (PID 2840)
  → svchost.exe (PID 1200)    ← Fake svchost (persistence)
```

**Jawaban:** Terdapat tiga anomali kritis: (1) svchost.exe palsu (PID 1200) dengan parent explorer.exe menunjukkan malware yang menyamar sebagai proses sistem, (2) Chain WINWORD→cmd→powershell menunjukkan infeksi melalui macro malware, kemungkinan dari attachment email. Investigator harus: (a) Dump proses 1200 dan 4700 untuk analisis lebih lanjut, (b) Periksa command line arguments powershell.exe, (c) Analisis dokumen Word yang dibuka, (d) Periksa network connections dari proses mencurigakan.

---

## 8. Anti-Analysis Techniques

### 8.1 Packing dan Obfuscation

> **Packing** adalah teknik kompresi atau enkripsi executable sehingga konten asli tidak dapat dibaca tanpa proses unpacking. **Obfuscation** adalah teknik mengubah kode agar sulit dipahami tanpa mengubah fungsionalitasnya.

**Packer Umum:**

| Packer | Tipe | Deteksi |
|--------|------|---------|
| **UPX** | Open source compression | Section names UPX0, UPX1 |
| **Themida** | Commercial protector | Complex anti-debug |
| **VMProtect** | Virtualization-based | Custom VM bytecode |
| **Custom** | Dibuat sendiri | Unusual PE structure |

**Teknik Unpacking:**
1. **Automatic**: Gunakan tools seperti UPX (`upx -d packed.exe`)
2. **Manual**: Jalankan di debugger, temukan OEP (Original Entry Point), dump
3. **Memory Dump**: Jalankan malware dan dump dari memory

### 8.2 Anti-Debugging Techniques

Malware menggunakan berbagai teknik untuk mendeteksi apakah sedang di-debug:

| Teknik | API/Method | Counter-Measure |
|--------|-----------|-----------------|
| **IsDebuggerPresent** | Cek PEB.BeingDebugged flag | Patch return value |
| **CheckRemoteDebuggerPresent** | Cek external debugger | Hook API |
| **NtQueryInformationProcess** | ProcessDebugPort | Return false info |
| **Timing Check** | `RDTSC`, `GetTickCount` | Adjust time values |
| **Hardware Breakpoint** | Check Debug Registers | Clear DR registers |
| **Exception-based** | Trigger exception, check handler | Set exception handler |

### 8.3 Anti-VM Techniques

Malware mendeteksi virtual machine untuk menghindari sandbox analysis:

| Teknik | Yang Dicek | Contoh |
|--------|-----------|--------|
| **Registry** | VM-specific keys | `HKLM\SOFTWARE\VMware, Inc.` |
| **Processes** | VM tools processes | `vmtoolsd.exe`, `VBoxService.exe` |
| **Hardware** | MAC address prefix | `00:0C:29:xx` (VMware), `08:00:27:xx` (VBox) |
| **BIOS** | BIOS string | "VBOX", "VMware" dalam SMBIOS |
| **Files** | VM-specific files | `C:\Windows\System32\drivers\vmmouse.sys` |
| **CPUID** | Hypervisor brand | Bit 31 ECX pada CPUID leaf 1 |

#### Solved Problem 10 ⭐⭐

**Soal:** Saat melakukan dynamic analysis, malware mendeteksi lingkungan sandbox dan menghentikan eksekusinya. Jelaskan tiga teknik yang mungkin digunakan malware dan bagaimana mengatasinya!

**Penyelesaian:**

*Step 1:* Identifikasi teknik anti-analysis yang umum

| # | Teknik | Deteksi | Counter-Measure |
|---|--------|---------|-----------------|
| 1 | **VM Detection** | Cek registry VMware/VBox | Hapus registry keys VM, modifikasi BIOS strings |
| 2 | **Anti-Debugging** | IsDebuggerPresent | Patch API return value, gunakan kernel debugger |
| 3 | **Environment Check** | Cek jumlah file, RAM, user interaction | Tambah file dummy, alokasi RAM memadai, simulasi mouse/keyboard |

*Step 2:* Solusi komprehensif

**Jawaban:** Tiga teknik dan penanganannya: (1) **VM Detection** - malware memeriksa registry keys, processes, atau hardware identifier yang spesifik VM; counter: gunakan bare-metal sandbox atau modifikasi VM agar tidak terdeteksi (menghapus VM tools, mengubah MAC address, memodifikasi BIOS), (2) **Anti-Debugging** - malware menggunakan API seperti IsDebuggerPresent; counter: patch API calls atau gunakan stealthy debugging tools, (3) **Environment Check** - malware memeriksa tanda-tanda sandbox (few files, no user activity, recent install); counter: buat VM yang menyerupai sistem nyata dengan file, history, dan simulasi aktivitas user.

---

## 9. Indicators of Compromise (IOC) dan Threat Intelligence

### 9.1 Definisi dan Tipe IOC

> **Indicators of Compromise (IOC)** adalah artefak forensik yang mengindikasikan bahwa sebuah sistem telah terkompromi oleh malware atau serangan siber. IOC digunakan untuk deteksi, hunting, dan sharing informasi ancaman.

**Pyramid of Pain (David Bianco):**

```
        ┌─────────────┐
        │   TTPs      │  ← Paling sulit diubah oleh attacker
        ├─────────────┤
        │   Tools     │
        ├─────────────┤
        │ Network/    │
        │ Host        │
        │ Artifacts   │
        ├─────────────┤
        │ Domain      │
        │ Names       │
        ├─────────────┤
        │ IP          │
        │ Addresses   │
        ├─────────────┤
        │ Hash Values │  ← Paling mudah diubah oleh attacker
        └─────────────┘
```

**Tipe-tipe IOC:**

| Tipe IOC | Contoh | Durasi Validitas |
|----------|--------|-----------------|
| **File Hash** | MD5/SHA256 malware binary | Pendek (berubah setiap compile) |
| **IP Address** | C2 server IP | Menengah (dapat berubah) |
| **Domain** | C2 domain name | Menengah |
| **URL** | Phishing URL, payload URL | Pendek |
| **Registry Key** | Persistence registry entries | Panjang |
| **Mutex** | Nama mutex unik | Panjang |
| **File Path** | Drop location malware | Menengah |
| **YARA Rule** | Pattern matching rule | Panjang |
| **TTPs** | MITRE ATT&CK technique | Sangat panjang |

### 9.2 MITRE ATT&CK Framework

> **MITRE ATT&CK** adalah knowledge base yang berisi taktik dan teknik yang digunakan oleh adversaries dalam serangan siber. Framework ini menjadi standar de facto untuk mendeskripsikan perilaku ancaman.

**Taktik ATT&CK yang Relevan untuk Forensik Malware:**

| Tactic ID | Tactic | Contoh Technique |
|-----------|--------|-----------------|
| TA0001 | Initial Access | Phishing (T1566), Drive-by Compromise (T1189) |
| TA0002 | Execution | PowerShell (T1059.001), Windows Command Shell (T1059.003) |
| TA0003 | Persistence | Registry Run Keys (T1547.001), Scheduled Task (T1053.005) |
| TA0004 | Privilege Escalation | Process Injection (T1055) |
| TA0005 | Defense Evasion | Obfuscated Files (T1027), Process Injection (T1055) |
| TA0007 | Discovery | System Information (T1082), Network Configuration (T1016) |
| TA0011 | Command and Control | Web Protocols (T1071.001), DNS (T1071.004) |
| TA0010 | Exfiltration | Exfiltration Over C2 Channel (T1041) |

### 9.3 Dokumentasi IOC dan Reporting

**Format IOC Documentation:**

```yaml
# IOC Report Template
incident_id: INC-2025-KODAM-0042
date: 2025-03-15
analyst: [Nama Investigator]
classification: RAHASIA

malware_info:
  family: TrojanRAT
  variant: MilTarget
  first_seen: 2025-03-14

indicators:
  hashes:
    - md5: a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6
    - sha256: abcdef1234567890...
  
  network:
    - ip: 103.45.67.89
      port: 443
      protocol: HTTPS
    - domain: update.microsoft-service[.]com
  
  host:
    - registry: HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run\WindowsUpdate
    - filepath: C:\Windows\System32\svcupdate.exe
    - mutex: Global\MutexCheckRunning
    - scheduled_task: SystemHealthCheck

  mitre_attack:
    - T1566.001 (Phishing: Spearphishing Attachment)
    - T1059.001 (Command and Scripting: PowerShell)
    - T1547.001 (Boot or Logon Autostart: Registry Run Keys)
    - T1053.005 (Scheduled Task)
    - T1071.001 (Application Layer Protocol: Web)
```

#### Solved Problem 11 ⭐⭐⭐

**Soal:** Setelah menyelesaikan analisis malware yang ditemukan di jaringan Denhubdam VII/Diponegoro, investigator telah mengidentifikasi IOC berikut. Petakan temuan ini ke MITRE ATT&CK framework dan buat rekomendasi mitigasi!

IOC:
- Hash: SHA256 abc123...
- C2: update-service[.]mil[.]id (spoofed military domain)
- Persistence: Service "WindowsDefenderUpdate"
- Delivery: Spear phishing email dengan PDF exploit
- Lateral Movement: PsExec ke 5 workstation lain

**Penyelesaian:**

| IOC | MITRE ATT&CK | Tactic | Mitigasi |
|-----|-------------|--------|----------|
| Spear phishing PDF | T1566.001 | Initial Access | Email filtering, user awareness training |
| PDF exploit | T1203 | Execution | Patch management, application whitelisting |
| Service persistence | T1543.003 | Persistence | Monitor service creation, baseline services |
| C2 communication | T1071.001 | C2 | DNS monitoring, block suspicious domains |
| PsExec lateral movement | T1570 | Lateral Movement | Network segmentation, restrict admin tools |

**Jawaban:** Mapping ke MITRE ATT&CK menunjukkan serangan multi-stage yang terkoordinasi. Rekomendasi mitigasi: (1) Implementasi email gateway dengan sandboxing untuk deteksi PDF exploit, (2) Deploy EDR untuk deteksi persistence dan lateral movement, (3) Network segmentation antara unit-unit Kodam untuk mencegah lateral movement, (4) DNS sinkholing untuk domain C2, (5) Restrict penggunaan tools administrasi seperti PsExec, dan (6) Conduct threat hunting berkala menggunakan IOC yang teridentifikasi.

---

## 10. Studi Kasus Forensik Malware dalam Konteks Militer

### 10.1 Studi Kasus: APT Attack pada Sistem Komunikasi Militer

**Skenario:** Sistem komunikasi terenkripsi di salah satu satuan TNI mengalami anomali berupa peningkatan traffic outbound yang tidak wajar. Tim CSIRT TNI melakukan investigasi forensik.

**Timeline Investigasi:**

| Waktu | Kejadian | Artefak |
|-------|---------|---------|
| T+0 | Anomali traffic terdeteksi | IDS alert, netflow data |
| T+1h | Isolasi sistem terdampak | Network isolation log |
| T+2h | Memory acquisition | RAM dump (16 GB) |
| T+3h | Disk imaging | Forensic image (500 GB) |
| T+4h | Memory analysis (Volatility) | Injected code di svchost.exe |
| T+6h | Static analysis malware | Custom RAT with DGA |
| T+8h | Network traffic analysis | C2 beaconing ke 3 domains |
| T+12h | Full analysis complete | Comprehensive IOC list |

**Temuan Utama:**

1. **Initial Access**: Spear phishing email yang menyamar sebagai komunikasi internal
2. **Malware Type**: Custom RAT dengan kemampuan keylogging, screen capture, dan file exfiltration
3. **Persistence**: WMI Event Subscription (lebih sulit terdeteksi dari registry)
4. **C2**: DGA-based domain generation dengan HTTPS communication
5. **Lateral Movement**: Pass-the-hash untuk mengakses server lain
6. **Data Exfiltration**: Dokumen klasifikasi diekstrak melalui encrypted C2 channel

#### Solved Problem 12 ⭐⭐⭐

**Soal:** Berdasarkan studi kasus di atas, jelaskan mengapa memory forensics menjadi kunci identifikasi malware dan apa yang akan terjadi jika investigator langsung melakukan disk imaging tanpa memory acquisition terlebih dahulu!

**Penyelesaian:**

*Step 1:* Mengapa memory forensics kritis

Memory forensics menjadi kunci karena:
1. Custom RAT kemungkinan di-pack → kode asli hanya tersedia di memori setelah unpacking
2. WMI persistence tidak meninggalkan binary standalone di disk
3. Injected code di svchost.exe hanya visible di memory
4. Encryption keys untuk C2 communication ada di memori
5. Koneksi aktif ke C2 server terlihat di network connections memory

*Step 2:* Konsekuensi tanpa memory acquisition

Jika langsung disk imaging:
- Power off akan menghilangkan seluruh konten RAM
- Injected code hilang → sulit mengidentifikasi proses yang compromised
- Active network connections hilang → C2 infrastructure tidak teridentifikasi
- Encryption keys hilang → tidak bisa mendekripsi C2 traffic
- Running processes dan command lines hilang

**Jawaban:** Memory forensics menjadi kunci karena custom RAT menggunakan teknik in-memory execution dan code injection yang tidak meninggalkan artefak di disk. Tanpa memory acquisition: (1) Kode malware yang ter-unpack di memori akan hilang, (2) Evidence code injection ke svchost.exe tidak ditemukan, (3) C2 connections aktif tidak teridentifikasi, (4) WMI persistence mungkin terlewat karena bukan binary di disk. Ini menunjukkan pentingnya prinsip **order of volatility** (seperti yang telah dibahas pada [Pertemuan 05](../p05/modul.md)) - volatile data harus diacquire terlebih dahulu sebelum non-volatile data.

---

## Supplementary Problems

### Solved Problem 13 ⭐

**Soal:** Apa perbedaan antara virus dan worm dalam konteks forensik digital?

**Penyelesaian:**

| Aspek | Virus | Worm |
|-------|-------|------|
| **Host File** | Memerlukan host | Standalone, tidak perlu host |
| **Penyebaran** | Melalui file terinfeksi | Self-propagating via jaringan |
| **Interaksi User** | Perlu user menjalankan file | Otomatis tanpa interaksi |
| **Artefak Disk** | Modified PE files, parasitic code | Standalone executable di temp/system dirs |
| **Artefak Network** | Minimal (via file sharing) | Scanning activity, exploit traffic |

**Jawaban:** Perbedaan utama: virus bersifat parasitic (menempelkan diri pada file host) dan memerlukan interaksi pengguna, sedangkan worm bersifat standalone dan menyebar secara otomatis melalui jaringan. Dalam forensik, virus meninggalkan artefak berupa file yang termodifikasi, sementara worm meninggalkan network-based artefak seperti scanning activity.

---

### Solved Problem 14 ⭐

**Soal:** Jelaskan apa itu entropy dalam konteks static analysis dan mengapa entropy tinggi mencurigakan!

**Penyelesaian:**

> **Entropy** adalah ukuran keacakan (randomness) data, dengan skala 0-8 untuk data byte. Entropy 0 = semua byte identik, entropy 8 = data sepenuhnya acak.

| Entropy Range | Interpretasi | Contoh |
|--------------|-------------|--------|
| 0-1 | Sangat teratur | File dengan repeating data |
| 1-4 | Normal text | Source code, dokumen teks |
| 4-6 | Mixed content | Normal executable |
| 6-7 | Compressed | ZIP, legitimate packed files |
| 7-8 | Encrypted/packed | Malware packed, encrypted data |

**Jawaban:** Entropy tinggi (>7.0) pada section executable mencurigakan karena menunjukkan data yang terkompresi atau terenkripsi. Normal executable code memiliki entropy 4-6. Section .text dengan entropy 7.8 kemungkinan besar packed atau encrypted, yang merupakan teknik umum untuk menyembunyikan malware dari static analysis dan antivirus.

---

### Solved Problem 15 ⭐⭐

**Soal:** Sebutkan dan jelaskan lima API call Windows yang paling sering digunakan oleh malware untuk process injection, beserta fungsi masing-masing!

**Penyelesaian:**

| # | API Call | Fungsi | Peran dalam Injection |
|---|---------|--------|----------------------|
| 1 | `OpenProcess` | Membuka handle ke proses target | Mendapatkan akses ke proses yang akan di-inject |
| 2 | `VirtualAllocEx` | Mengalokasikan memori di proses remote | Menyiapkan ruang untuk kode malicious |
| 3 | `WriteProcessMemory` | Menulis data ke memori proses remote | Menyalin shellcode/DLL ke memori yang dialokasikan |
| 4 | `CreateRemoteThread` | Membuat thread di proses remote | Mengeksekusi kode yang telah di-inject |
| 5 | `NtUnmapViewOfSection` | Menghapus mapping section | Process hollowing - mengosongkan proses legitimate |

**Jawaban:** Classic injection pattern: OpenProcess → VirtualAllocEx → WriteProcessMemory → CreateRemoteThread. Untuk process hollowing, ditambahkan NtUnmapViewOfSection untuk mengosongkan code section proses legitimate sebelum menulis kode malicious.

---

### Solved Problem 16 ⭐⭐

**Soal:** Apa perbedaan antara process injection dan process hollowing?

**Penyelesaian:**

| Aspek | Process Injection | Process Hollowing |
|-------|------------------|-------------------|
| **Teknik** | Menyisipkan kode ke proses yang sedang berjalan | Mengganti kode proses legitimate dengan kode malicious |
| **Target Process** | Proses yang sudah running | Proses yang baru dibuat (suspended) |
| **Original Code** | Tetap ada bersamaan | Diganti sepenuhnya |
| **State** | Target process running | Target dibuat suspended, lalu di-resume |
| **Deteksi** | Malfind (RWX pages), DLL yang tidak sesuai | Image mismatch antara disk dan memory |

**Jawaban:** Process injection menyisipkan kode tambahan ke proses yang sudah berjalan, sedangkan process hollowing membuat proses legitimate dalam keadaan suspended, mengganti seluruh kodenya dengan malware, kemudian melanjutkan eksekusi. Hollowing lebih sulit terdeteksi karena proses terlihat legitimate di task manager namun menjalankan kode malicious.

---

### Solved Problem 17 ⭐⭐

**Soal:** Jelaskan bagaimana DNS tunneling bekerja sebagai mekanisme C2 communication!

**Penyelesaian:**

**Cara Kerja DNS Tunneling:**

```
Malware (Victim) → DNS Query → DNS Server → C2 Server
                                    ↓
C2 Server → DNS Response → DNS Server → Malware (Victim)

Contoh encoding data dalam DNS query:
Normal:  www.google.com
Tunnel:  SGVsbG8gV29ybGQ=.data.evil-c2.com
         ^^^^^^^^^^^^^^^^^^^
         Base64 encoded stolen data
```

1. Malware mengenkode data yang akan dikirim dalam subdomain DNS query
2. DNS query diteruskan ke authoritative DNS server yang dikontrol attacker
3. C2 server menerima data, memproses, dan mengirim perintah balik via DNS response
4. Data dapat dienkode dalam TXT, CNAME, MX, atau record DNS lainnya

**Deteksi DNS Tunneling:**
- DNS query dengan subdomain sangat panjang
- Volume DNS query yang tidak normal dari satu host
- Query ke domain yang baru/tidak dikenal
- Tipe record yang tidak biasa (TXT records berlebihan)
- Payload DNS response yang besar

**Jawaban:** DNS tunneling memanfaatkan protokol DNS untuk mengirim data yang dienkode sebagai subdomain dalam DNS queries. Efektif karena DNS traffic jarang diblokir atau diinspeksi secara mendalam. Deteksi memerlukan analisis anomali pada volume, panjang query, dan entropy subdomain.

---

### Solved Problem 18 ⭐⭐⭐

**Soal:** Sebuah ransomware menginfeksi 15 komputer di markas Korem. Memory dump berhasil diambil dari salah satu komputer yang masih menyala. Jelaskan langkah-langkah untuk mengekstrak encryption key dari memory dump!

**Penyelesaian:**

*Step 1:* Identifikasi proses ransomware

```bash
# Cari proses ransomware
python3 vol.py -f memdump.raw windows.pslist | grep -i "ransom\|encrypt\|lock"
python3 vol.py -f memdump.raw windows.cmdline
```

*Step 2:* Analisis memori proses ransomware

```bash
# Dump proses ransomware
python3 vol.py -f memdump.raw windows.memmap --pid <PID> --dump

# Atau dump seluruh virtual address space
python3 vol.py -f memdump.raw windows.dumpfiles --pid <PID>
```

*Step 3:* Pencarian encryption keys

```bash
# Cari crypto-related patterns
# AES key schedule pattern
# RSA key markers
# Strings analysis pada dump proses
strings -a process_dump.dmp | grep -iE "key|crypt|aes|rsa|BEGIN"

# Gunakan tools khusus seperti findaes atau rsakeyfind
findaes memdump.raw
```

*Step 4:* Validasi key

Coba gunakan key yang ditemukan untuk mendekripsi satu file terenkripsi. Jika berhasil, key valid untuk seluruh file.

**Jawaban:** Langkah ekstraksi encryption key: (1) Identifikasi PID proses ransomware dari memory dump, (2) Dump memory space proses tersebut, (3) Cari pattern kunci kriptografi (AES key schedule, RSA key structures), (4) Gunakan tool khusus seperti `findaes` atau `rsakeyfind`, (5) Validasi key dengan mencoba dekripsi file. Key mungkin masih ada di memori jika ransomware belum membersihkannya atau jika komputer belum di-reboot.

---

### Solved Problem 19 ⭐

**Soal:** Apa yang dimaksud dengan Ransomware-as-a-Service (RaaS) dan mengapa ini menjadi ancaman signifikan bagi infrastruktur pertahanan?

**Penyelesaian:**

> **Ransomware-as-a-Service (RaaS)** adalah model bisnis di mana pembuat ransomware menyewakan infrastruktur dan tools mereka kepada "affiliates" yang melakukan serangan, dengan pembagian keuntungan dari tebusan yang dibayar.

**Alasan RaaS menjadi ancaman signifikan:**
1. **Lowered Barrier**: Pelaku tidak perlu keahlian teknis tinggi
2. **Scalability**: Banyak affiliates = banyak serangan simultan
3. **Sophistication**: Tools buatan developer ahli
4. **Rapid Evolution**: Cepat beradaptasi terhadap defense

**Jawaban:** RaaS memungkinkan siapa saja dengan motivasi (termasuk aktor yang menargetkan infrastruktur militer) untuk melakukan serangan ransomware canggih tanpa keahlian teknis tinggi. Ini meningkatkan volume dan diversitas ancaman terhadap sistem pertahanan secara signifikan.

---

### Solved Problem 20 ⭐⭐

**Soal:** Jelaskan perbedaan antara fileless malware dan traditional malware dalam konteks deteksi forensik!

**Penyelesaian:**

| Aspek | Traditional Malware | Fileless Malware |
|-------|-------------------|-----------------|
| **Persistence** | File executable di disk | WMI, registry, PowerShell scripts |
| **Execution** | Binary file dijalankan | In-memory execution, LOLBins |
| **Disk Artifact** | PE file, dropped files | Minimal atau tidak ada |
| **Memory Artifact** | Proses dengan binary file | Injected code, PowerShell in memory |
| **Detection** | AV signature, file hash | Behavioral analysis, memory forensics |
| **Analysis** | Static + Dynamic | Primarily dynamic + memory |

**Jawaban:** Fileless malware tidak meninggalkan file executable di disk, beroperasi sepenuhnya di memori menggunakan tools legitimate (LOLBins) seperti PowerShell, WMI, dan .NET. Deteksi memerlukan memory forensics, behavioral monitoring, dan analisis event logs karena signature-based detection tidak efektif.

---

### Solved Problem 21 ⭐⭐⭐

**Soal:** Tim forensik menemukan YARA rule berikut untuk mendeteksi malware tertentu. Jelaskan apa yang dicari oleh rule ini!

```yara
rule SuspiciousMilTarget
{
    meta:
        author = "CSIRT TNI"
        description = "Detects MilTarget RAT"
        
    strings:
        $mz = { 4D 5A }
        $str1 = "Mozilla/5.0" ascii
        $str2 = "cmd.exe /c" ascii
        $api1 = "VirtualAllocEx" ascii
        $api2 = "CreateRemoteThread" ascii
        $c2 = /update\-service\[?\.\]?mil\[?\.\]?id/ ascii
        $mutex = "Global\\MilTargetMutex" ascii
        
    condition:
        $mz at 0 and
        ($str1 and $str2) and
        ($api1 and $api2) and
        ($c2 or $mutex)
}
```

**Penyelesaian:**

*Step 1:* Analisis setiap komponen rule

| Komponen | Yang Dicari | Tujuan |
|----------|-----------|--------|
| `$mz at 0` | MZ header di offset 0 | Memastikan file adalah PE executable |
| `$str1` | User-Agent browser | Traffic masquerading |
| `$str2` | Command shell execution | Command execution capability |
| `$api1 + $api2` | Process injection APIs | Code injection detection |
| `$c2` | C2 domain pattern | Specific C2 infrastructure |
| `$mutex` | Specific mutex name | Unique malware identifier |

*Step 2:* Analisis kondisi

Rule akan match jika file: (1) adalah PE file, DAN (2) mengandung kedua strings, DAN (3) mengandung kedua API calls injection, DAN (4) mengandung domain C2 ATAU mutex name.

**Jawaban:** YARA rule ini mendeteksi RAT bernama "MilTarget" yang menargetkan infrastruktur militer. Rule mencari PE executable yang memiliki kemampuan: traffic masquerading (User-Agent fake), command execution, process injection (VirtualAllocEx + CreateRemoteThread), dan identifikasi spesifik melalui C2 domain atau mutex name. Ini menunjukkan approach defense-in-depth dalam deteksi - menggabungkan multiple indicators untuk mengurangi false positives.

---

### Solved Problem 22 ⭐

**Soal:** Mengapa MITRE ATT&CK framework penting dalam dokumentasi forensik malware?

**Penyelesaian:**

MITRE ATT&CK penting karena:

1. **Standarisasi Bahasa**: Memberikan terminologi standar untuk mendeskripsikan teknik serangan
2. **Completeness Check**: Memastikan semua taktik telah dianalisis
3. **Intelligence Sharing**: Format yang dipahami secara universal
4. **Gap Analysis**: Mengidentifikasi area deteksi yang kurang
5. **Attribution**: Membantu menghubungkan teknik dengan threat actors yang dikenal

**Jawaban:** MITRE ATT&CK framework menyediakan bahasa standar untuk dokumentasi forensik malware, memungkinkan komunikasi efektif antar tim keamanan, sharing IOC yang terstruktur, dan gap analysis terhadap kemampuan deteksi organisasi. Dalam konteks militer, framework ini memfasilitasi koordinasi cyber defense lintas unit dan matra TNI.

---

## Ringkasan

| Konsep | Deskripsi Kunci |
|--------|----------------|
| **Forensik Malware** | Analisis malware dengan standar forensik untuk mendukung investigasi |
| **Klasifikasi Malware** | Virus, worm, trojan, ransomware, APT - masing-masing dengan karakteristik forensik berbeda |
| **Static Analysis** | Analisis tanpa eksekusi: file properties, strings, PE structure, imports |
| **Dynamic Analysis** | Analisis dalam sandbox: behavioral monitoring, network capture, system changes |
| **Persistence** | Registry Run keys, scheduled tasks, services, WMI, DLL hijacking |
| **C2 Communication** | HTTP/HTTPS, DNS tunneling, DGA, beaconing patterns |
| **Memory Forensics** | Deteksi code injection, fileless malware, extraction of keys/C2 info |
| **Anti-Analysis** | Packing, obfuscation, anti-debugging, anti-VM, environment checking |
| **IOC** | Hash, IP, domain, registry key, YARA rules, MITRE ATT&CK mapping |

---

## Referensi

1. Sikorski, M. & Honig, A. (2012). *Practical Malware Analysis: The Hands-On Guide to Dissecting Malicious Software*. No Starch Press. Chapter 1-6.
2. Ligh, M.H., Case, A., Levy, J., & Walters, A. (2014). *The Art of Memory Forensics: Detecting Malware and Threats in Windows, Linux, and Mac Memory*. Wiley. Chapter 10-15.
3. Casey, E. (2022). *Digital Evidence and Computer Crime: Forensic Science, Computers and the Internet*. Academic Press. Chapter 12.
4. Johansen, G. (2020). *Digital Forensics and Incident Response*. Packt Publishing. Chapter 8-9.
5. MITRE ATT&CK Framework. https://attack.mitre.org/
6. MalwareBazaar. https://bazaar.abuse.ch/
7. PEStudio. https://www.winitor.com/
8. FLOSS (FireEye Labs Obfuscated String Solver). https://github.com/mandiant/flare-floss
9. FakeNet-NG. https://github.com/mandiant/flare-fakenet-ng
10. MemLabs. https://github.com/stuxnet999/MemLabs

---

## License / Lisensi

Materi ini dilisensikan di bawah [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/).

© 2025 Program Studi Teknologi Informasi Pertahanan, Universitas Pertahanan RI
