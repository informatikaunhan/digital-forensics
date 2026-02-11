# Modul 05: Forensik Windows I - Volatile Data dan Artefak Sistem

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 05  
**Topik:** Forensik Windows I - Analisis Volatile Data dan Artefak Sistem  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Menjelaskan konsep order of volatility dan menerapkan prioritas pengumpulan bukti forensik
2. Melakukan analisis memory dump menggunakan Volatility 3 untuk mengidentifikasi proses, koneksi jaringan, dan loaded modules
3. Mengekstrak dan menganalisis Windows Event Logs (Security, System, Application) untuk rekonstruksi kejadian
4. Menganalisis Prefetch files dan SuperFetch untuk menentukan riwayat eksekusi program
5. Mengidentifikasi dan menginterpretasi Jump Lists, Recent Documents, dan LNK files
6. Mengekstrak dan menganalisis browser artifacts dari Chrome, Firefox, dan Edge
7. Melakukan analisis Recycle Bin, Thumbnail Cache, dan Windows Timeline dalam konteks investigasi militer

---

## 1. Order of Volatility dan Prioritas Pengumpulan Bukti

### 1.1 Konsep Order of Volatility

> **Order of Volatility** adalah urutan prioritas pengumpulan bukti digital berdasarkan tingkat kemudahan data tersebut hilang atau berubah. Prinsip ini mewajibkan investigator untuk mengumpulkan data yang paling volatile terlebih dahulu.

Konsep ini pertama kali diformalisasikan dalam RFC 3227 (*Guidelines for Evidence Collection and Archiving*) yang menjadi acuan standar dalam forensik digital. Dalam konteks militer, pengumpulan volatile data menjadi sangat kritis karena sistem pertahanan sering kali harus segera dioperasikan kembali setelah insiden.

| Urutan | Sumber Data | Volatilitas | Waktu Persistensi | Contoh Bukti |
|--------|-------------|-------------|-------------------|--------------|
| 1 | CPU Registers & Cache | Sangat Tinggi | Nanoseconds–Microseconds | Instruction pointer, stack data |
| 2 | RAM (Memory) | Tinggi | Hilang saat power off | Proses aktif, encryption keys, passwords |
| 3 | Network State | Tinggi | Real-time, tidak disimpan permanen | Koneksi aktif, routing tables, ARP cache |
| 4 | Running Processes | Tinggi | Hilang saat power off | Malware di memori, injected code |
| 5 | Disk (Temporary Data) | Sedang | Dapat di-overwrite | Swap file, page file, temp files |
| 6 | Disk (Persistent Data) | Rendah | Persisten hingga dihapus | File sistem, logs, dokumen |
| 7 | Remote Logging | Sangat Rendah | Persisten di server log | Syslog, SIEM data, cloud logs |
| 8 | Backup/Archive | Sangat Rendah | Persisten jangka panjang | Tape backup, offsite backup |

![Order of Volatility Pyramid](images/p05-01-order-of-volatility.png)

*Gambar 5.1: Piramida Order of Volatility — data di puncak paling cepat hilang*

### 1.2 Live Forensics vs Dead Forensics

Keputusan apakah melakukan live forensics atau dead forensics sangat menentukan bukti apa saja yang dapat dikumpulkan:

| Aspek | Live Forensics | Dead Forensics |
|-------|---------------|----------------|
| **Kondisi Sistem** | Masih berjalan (powered on) | Sudah dimatikan (powered off) |
| **Volatile Data** | Dapat diakses | Hilang secara permanen |
| **Risiko** | Mengubah state sistem | Kehilangan volatile data |
| **Data yang Tersedia** | RAM, proses, koneksi jaringan, encryption keys | Disk content, persistent artifacts |
| **Kompleksitas** | Tinggi — harus cepat dan terdokumentasi | Sedang — dapat dianalisis tanpa tekanan waktu |
| **Konteks Militer** | Sistem C2 aktif, server operasi | Workstation sitaan, media penyimpanan |

> **Penting untuk konteks militer:** Dalam operasi pertahanan, sistem Command and Control (C2) sering kali tidak dapat dimatikan. Investigator militer harus mampu melakukan live forensics tanpa mengganggu operasi yang sedang berjalan.

#### Solved Problem 1 ⭐

**Soal:** Seorang investigator forensik TNI menemukan workstation yang dicurigai terinfeksi malware di jaringan Kodam. Workstation tersebut masih menyala. Sebutkan urutan tiga langkah pertama yang harus dilakukan berdasarkan order of volatility!

**Penyelesaian:**

*Step 1:* Identifikasi data paling volatile yang tersedia pada sistem yang masih berjalan

Berdasarkan piramida order of volatility, data yang paling volatile pada workstation yang masih menyala adalah: RAM/memory, network state, dan running processes.

*Step 2:* Tentukan urutan pengumpulan

| Prioritas | Langkah | Alasan |
|-----------|---------|--------|
| 1 | Capture RAM (memory dump) | Data paling volatile — hilang saat power off |
| 2 | Dokumentasikan koneksi jaringan aktif | Network state berubah setiap saat |
| 3 | Rekam daftar proses yang berjalan | Proses malware bisa terdeteksi di sini |

**Jawaban:** Tiga langkah pertama: (1) Melakukan memory dump menggunakan tools seperti Belkasoft RAM Capturer, (2) Mendokumentasikan koneksi jaringan aktif dengan `netstat -anob`, (3) Merekam daftar proses yang berjalan dengan Process Explorer atau `tasklist`. Semua langkah mengikuti prinsip order of volatility dari data paling volatile.

---

#### Solved Problem 2 ⭐

**Soal:** Apa perbedaan antara live forensics dan dead forensics? Kapan masing-masing pendekatan sebaiknya digunakan?

**Penyelesaian:**

*Step 1:* Definisikan kedua pendekatan

- **Live forensics**: Investigasi dilakukan pada sistem yang masih berjalan
- **Dead forensics**: Investigasi dilakukan pada sistem yang sudah dimatikan

*Step 2:* Identifikasi kapan masing-masing digunakan

| Situasi | Pendekatan | Alasan |
|---------|-----------|--------|
| Malware di memori yang perlu diidentifikasi | Live | Data di RAM hilang saat mati |
| Analisis hard disk dari barang sitaan | Dead | Tidak ada volatile data, fokus persistensi |
| Sistem C2 militer yang tidak boleh dimatikan | Live | Kontinuitas operasi harus dijaga |
| Investigasi perangkat yang sudah dimatikan tersangka | Dead | Satu-satunya pilihan karena volatile data sudah hilang |

**Jawaban:** Live forensics dilakukan pada sistem yang masih aktif untuk menangkap volatile data (RAM, proses, koneksi jaringan) dan digunakan ketika data volatile penting atau sistem tidak boleh dimatikan. Dead forensics dilakukan pada sistem yang sudah mati, fokus pada persistent storage, dan lebih aman dari risiko kontaminasi. Dalam konteks militer, live forensics sering dipilih karena sistem pertahanan tidak boleh terganggu.

---

## 2. Analisis Memory (Memory Forensics)

### 2.1 Konsep Memory Forensics

> **Memory Forensics** adalah teknik investigasi yang menganalisis konten RAM (Random Access Memory) komputer untuk mengekstrak bukti digital yang hanya tersedia saat sistem berjalan.

Memory forensics menjadi sangat penting dalam investigasi modern karena banyak malware canggih (fileless malware) yang hanya beroperasi di memori tanpa meninggalkan jejak di disk. Dalam konteks militer, serangan APT (Advanced Persistent Threat) sering menggunakan teknik ini.

#### 2.1.1 Apa yang Tersimpan di RAM?

| Kategori | Contoh Data | Nilai Forensik |
|----------|-------------|----------------|
| **Proses** | Daftar proses aktif, PID, parent process | Identifikasi malware, proses mencurigakan |
| **Koneksi Jaringan** | IP address tujuan, port, status koneksi | Deteksi C2 communication, data exfiltration |
| **Loaded Modules** | DLL yang di-load, kernel modules | Deteksi rootkit, injected code |
| **Encryption Keys** | BitLocker keys, TrueCrypt keys, password | Membuka data terenkripsi |
| **Command History** | Perintah CMD/PowerShell | Rekonstruksi aktivitas penyerang |
| **Clipboard Data** | Data yang di-copy | Data sensitif, password yang di-copy |
| **Open Files** | Handle ke file yang terbuka | Dokumen yang sedang diakses |

#### 2.1.2 Tools Memory Acquisition

| Tool | Platform | Format Output | Keterangan |
|------|----------|---------------|------------|
| **Belkasoft RAM Capturer** | Windows | .mem | Gratis, GUI sederhana |
| **DumpIt** | Windows | .raw | Gratis, CLI, sangat ringan |
| **WinPmem** | Windows | .raw, .aff4 | Open source, bagian dari Rekall |
| **FTK Imager** | Windows | .mem | Fitur memory capture terintegrasi |
| **LiME** | Linux | .lime | Linux Memory Extractor |

### 2.2 Analisis Memory dengan Volatility 3

> **Volatility 3** adalah framework open-source untuk analisis memory forensics. Merupakan versi terbaru dari Volatility Framework yang ditulis ulang dalam Python 3 dengan arsitektur yang lebih modular.

#### 2.2.1 Plugin Volatility 3 yang Penting

| Plugin | Fungsi | Contoh Penggunaan |
|--------|--------|-------------------|
| `windows.pslist` | Daftar proses yang berjalan | Identifikasi proses mencurigakan |
| `windows.pstree` | Hierarki proses (parent-child) | Deteksi proses dengan parent anomali |
| `windows.netscan` | Koneksi jaringan aktif dan histori | Deteksi koneksi ke C2 server |
| `windows.cmdline` | Command line arguments setiap proses | Rekonstruksi perintah yang dijalankan |
| `windows.dlllist` | DLL yang di-load oleh setiap proses | Deteksi DLL injection |
| `windows.malfind` | Deteksi injected code di memori | Identifikasi malware fileless |
| `windows.handles` | Open handles (file, registry, mutex) | File dan resource yang diakses |
| `windows.filescan` | Scan file objects di memori | Recovery file yang terbuka |
| `windows.hashdump` | Dump password hashes | Ekstraksi kredensial |
| `windows.envars` | Environment variables | Informasi konfigurasi sistem |

![Alur Kerja Analisis Memory](images/p05-02-memory-analysis-workflow.png)

*Gambar 5.2: Alur kerja analisis memory forensics menggunakan Volatility 3*

#### 2.2.2 Contoh Perintah Volatility 3

```bash
# Daftar proses
python vol.py -f memory.dmp windows.pslist

# Hierarki proses
python vol.py -f memory.dmp windows.pstree

# Koneksi jaringan
python vol.py -f memory.dmp windows.netscan

# Deteksi malware injection
python vol.py -f memory.dmp windows.malfind

# Command line arguments
python vol.py -f memory.dmp windows.cmdline

# Dump password hashes
python vol.py -f memory.dmp windows.hashdump
```

#### Solved Problem 3 ⭐

**Soal:** Sebutkan empat jenis data yang dapat diekstrak dari RAM dump dan jelaskan nilai forensiknya!

**Penyelesaian:**

*Step 1:* Identifikasi empat jenis data yang tersimpan di RAM

*Step 2:* Jelaskan nilai forensik masing-masing

| Data di RAM | Nilai Forensik |
|-------------|----------------|
| **Daftar proses aktif** | Mengidentifikasi malware atau program tidak sah yang berjalan |
| **Koneksi jaringan** | Mendeteksi komunikasi ke Command & Control server musuh |
| **Encryption keys** | Membuka data terenkripsi seperti BitLocker tanpa password |
| **Command history** | Merekonstruksi perintah yang dijalankan oleh penyerang |

**Jawaban:** Empat jenis data dari RAM dump: (1) Daftar proses aktif — untuk identifikasi malware, (2) Koneksi jaringan — untuk deteksi komunikasi C2, (3) Encryption keys — untuk membuka data terenkripsi, (4) Command history — untuk rekonstruksi aktivitas penyerang.

---

#### Solved Problem 4 ⭐⭐

**Soal:** Seorang analis forensik militer menggunakan Volatility 3 untuk menganalisis memory dump dari server Kodam. Output plugin `windows.pstree` menunjukkan proses `svchost.exe` dengan parent process `explorer.exe`. Apakah ini normal? Jelaskan!

**Penyelesaian:**

*Step 1:* Pahami hierarki proses normal Windows

Dalam sistem Windows yang normal, proses `svchost.exe` selalu memiliki parent process `services.exe`, yang merupakan anak dari `wininit.exe`. Hierarki normalnya:

```
System → smss.exe → wininit.exe → services.exe → svchost.exe
```

*Step 2:* Evaluasi anomali

| Aspek | Normal | Temuan |
|-------|--------|--------|
| Parent `svchost.exe` | `services.exe` | `explorer.exe` |
| Implikasi | Sistem service manager yang sah | Proses mencurigakan menyamar sebagai service |

*Step 3:* Tentukan kesimpulan

Ini adalah **anomali signifikan**. Malware sering menyamar sebagai `svchost.exe` tetapi parent process-nya mengungkapkan bahwa proses tersebut bukan service Windows yang sah.

**Jawaban:** Ini **TIDAK normal** dan sangat mencurigakan. `svchost.exe` yang sah selalu memiliki parent `services.exe`, bukan `explorer.exe`. Temuan ini mengindikasikan kemungkinan malware yang menyamar sebagai svchost.exe. Investigator harus melakukan analisis lebih lanjut menggunakan `windows.malfind` dan `windows.dlllist` untuk mengidentifikasi injected code atau modul berbahaya.

---

#### Solved Problem 5 ⭐⭐

**Soal:** Jelaskan perbedaan antara plugin `windows.pslist` dan `windows.psscan` pada Volatility 3. Mengapa keduanya penting dalam investigasi malware?

**Penyelesaian:**

*Step 1:* Pahami mekanisme kerja masing-masing plugin

| Plugin | Mekanisme | Cakupan |
|--------|-----------|---------|
| `windows.pslist` | Menelusuri doubly-linked list `EPROCESS` dari kernel | Hanya proses yang terdaftar di linked list aktif |
| `windows.psscan` | Scan seluruh memori mencari struktur `EPROCESS` | Semua proses termasuk yang sudah terminated atau tersembunyi |

*Step 2:* Analisis pentingnya untuk investigasi malware

Rootkit dan malware canggih dapat memanipulasi linked list `EPROCESS` untuk menyembunyikan proses dari `pslist` (teknik yang disebut DKOM — Direct Kernel Object Manipulation). Namun, `psscan` tetap dapat menemukan proses tersebut karena melakukan scan pola di seluruh memori.

**Jawaban:** `pslist` menelusuri linked list proses aktif kernel, sementara `psscan` melakukan brute-force scan seluruh memori untuk menemukan struktur proses. Membandingkan output keduanya penting karena proses yang muncul di `psscan` tetapi tidak di `pslist` kemungkinan disembunyikan oleh rootkit menggunakan teknik DKOM (Direct Kernel Object Manipulation).

---

## 3. Windows Event Logs

### 3.1 Konsep Event Logs

> **Windows Event Logs** adalah mekanisme pencatatan aktivitas sistem, keamanan, dan aplikasi yang dilakukan oleh Windows secara otomatis. Event logs merupakan salah satu sumber bukti forensik paling berharga pada sistem Windows.

Event logs disimpan dalam format EVTX (mulai dari Windows Vista/Server 2008) di lokasi:

```
C:\Windows\System32\winevt\Logs\
```

#### 3.1.1 Jenis Event Log Utama

| Log | File | Fungsi | Contoh Event |
|-----|------|--------|-------------|
| **Security** | Security.evtx | Audit keamanan: login, akses file, policy changes | Event ID 4624 (logon), 4625 (failed logon) |
| **System** | System.evtx | Aktivitas sistem: service, driver, hardware | Event ID 7045 (service installed), 6005 (startup) |
| **Application** | Application.evtx | Aktivitas aplikasi: error, warning, info | Application crash, update events |
| **PowerShell** | Windows PowerShell.evtx | Eksekusi script PowerShell | Event ID 4104 (script block logging) |
| **Sysmon** | Microsoft-Windows-Sysmon%4Operational.evtx | Monitoring proses, jaringan, file | Event ID 1 (process create), 3 (network) |

#### 3.1.2 Event ID Penting untuk Investigasi

| Event ID | Log | Deskripsi | Relevansi Forensik |
|----------|-----|-----------|-------------------|
| 4624 | Security | Successful logon | Identifikasi akses sah dan lateral movement |
| 4625 | Security | Failed logon | Deteksi brute force, credential stuffing |
| 4648 | Security | Logon with explicit credentials | Penggunaan RunAs, pass-the-hash |
| 4672 | Security | Special privileges assigned | Eskalasi hak akses |
| 4688 | Security | New process created | Tracking eksekusi program |
| 4720 | Security | User account created | Persistence: pembuatan akun backdoor |
| 7045 | System | New service installed | Persistence: service backdoor |
| 1102 | Security | Audit log cleared | Anti-forensik: penghapusan log |
| 4104 | PowerShell | Script block logging | Deteksi PowerShell attacks |

![Arsitektur Windows Event Log](images/p05-03-event-log-architecture.png)

*Gambar 5.3: Arsitektur Windows Event Log dan lokasi penyimpanan*

### 3.2 Tools Analisis Event Log

| Tool | Pengembang | Kelebihan |
|------|-----------|-----------|
| **FullEventLogView** | NirSoft | Gratis, GUI intuitif, filter powerful |
| **EvtxECmd** | Eric Zimmerman | CLI, output CSV untuk analisis massal |
| **Event Log Explorer** | FSPro Labs | GUI profesional, timeline view |
| **Log Parser** | Microsoft | Query SQL pada event logs |
| **Windows Event Viewer** | Microsoft (built-in) | Tersedia default di semua Windows |

#### Solved Problem 6 ⭐

**Soal:** Sebutkan tiga Event ID pada Security Log yang paling penting untuk mendeteksi aktivitas login mencurigakan, beserta deskripsinya!

**Penyelesaian:**

*Step 1:* Identifikasi Event ID terkait login

| Event ID | Deskripsi | Deteksi |
|----------|-----------|---------|
| **4624** | Successful Logon | Login berhasil — identifikasi siapa yang mengakses |
| **4625** | Failed Logon | Login gagal — indikasi brute force attack |
| **4648** | Logon using Explicit Credentials | RunAs atau pass-the-hash — lateral movement |

*Step 2:* Jelaskan relevansi forensik

Event 4625 yang berulang dari IP yang sama dalam waktu singkat mengindikasikan brute force. Event 4624 dengan Logon Type 10 (RemoteInteractive) dari internal IP yang tidak biasa mengindikasikan lateral movement.

**Jawaban:** Tiga Event ID terpenting: (1) **4624** — Successful Logon, untuk melacak siapa yang berhasil mengakses sistem, (2) **4625** — Failed Logon, untuk mendeteksi upaya brute force, (3) **4648** — Explicit Credential Logon, untuk mendeteksi penggunaan kredensial curian atau lateral movement.

---

#### Solved Problem 7 ⭐⭐

**Soal:** Investigator menemukan Event ID 1102 (Security log cleared) pada workstation di pangkalan militer, diikuti oleh Event ID 4720 (user account created) dengan username "admin$". Analisis temuan ini!

**Penyelesaian:**

*Step 1:* Analisis Event ID 1102

Event ID 1102 menunjukkan bahwa seseorang sengaja menghapus Security audit log. Ini adalah teknik anti-forensik yang sangat mencurigakan karena menghapus jejak aktivitas sebelumnya.

*Step 2:* Analisis Event ID 4720

Pembuatan akun "admin$" dengan tanda dollar ($) di akhir adalah teknik persistence yang dikenal. Pada Windows, akun dengan suffix "$" adalah hidden account yang tidak muncul di daftar pengguna standar.

*Step 3:* Korelasikan temuan

| Event | Interpretasi | Indikasi |
|-------|-------------|----------|
| 1102 (Log Cleared) | Penghapusan jejak | Anti-forensik — penyerang menghapus bukti |
| 4720 (User Created: admin$) | Pembuatan backdoor | Persistence — akun tersembunyi untuk akses berkelanjutan |
| Urutan kronologis | Clear log → buat akun backdoor | Penyerang membersihkan jejak lalu membangun persistence |

**Jawaban:** Temuan ini mengindikasikan **intrusi aktif dengan teknik anti-forensik dan persistence**. Event 1102 menunjukkan penyerang menghapus log untuk menghilangkan jejak. Event 4720 dengan username "admin$" menunjukkan pembuatan hidden account sebagai backdoor — karakter "$" menyembunyikan akun dari daftar user standar. Kombinasi ini menunjukkan serangan yang terencana dan memerlukan incident response segera.

---

## 4. Prefetch Files dan SuperFetch

### 4.1 Konsep Prefetch

> **Prefetch** adalah mekanisme Windows yang menyimpan informasi tentang program yang dijalankan untuk mempercepat loading di eksekusi berikutnya. File prefetch merupakan artefak forensik berharga karena merekam riwayat eksekusi program.

Lokasi Prefetch files:
```
C:\Windows\Prefetch\
```

Format nama file: `[NAMA_PROGRAM]-[HASH].pf`

Contoh: `CMD.EXE-4A81B364.pf`

#### 4.1.1 Informasi yang Tersimpan dalam Prefetch

| Data | Deskripsi | Nilai Forensik |
|------|-----------|----------------|
| **Nama executable** | Program yang dijalankan | Identifikasi program yang pernah dieksekusi |
| **Run count** | Jumlah eksekusi | Frekuensi penggunaan program |
| **Timestamp terakhir** | Waktu eksekusi terakhir (hingga 8 timestamp di Win10) | Timeline aktivitas |
| **File/Directory referenced** | File dan folder yang diakses saat program berjalan | Konteks penggunaan program |
| **Volume information** | Volume yang terpasang saat eksekusi | Identifikasi media eksternal |

#### 4.1.2 Batasan Prefetch

| Versi Windows | Max Prefetch Files | Timestamps Tersimpan |
|---------------|-------------------|---------------------|
| Windows XP/Vista/7 | 128 | 1 (terakhir) |
| Windows 8/8.1 | 1024 | 8 (delapan terbaru) |
| Windows 10/11 | 1024 | 8 (delapan terbaru) |

> **Catatan:** Prefetch dinonaktifkan secara default pada SSD di beberapa konfigurasi Windows 10/11. Investigator harus memeriksa konfigurasi `EnablePrefetcher` di Registry.

### 4.2 SuperFetch (SysMain)

> **SuperFetch** (dikenal sebagai SysMain mulai Windows 10) adalah layanan yang memprediksi aplikasi mana yang akan digunakan pengguna dan melakukan preloading ke memori. Data SuperFetch tersimpan dalam file database di:

```
C:\Windows\Prefetch\AgAppLaunch.db
C:\Windows\Prefetch\AgGlUAD_S-1-5-21-*.db
```

File database SuperFetch mengandung informasi tambahan tentang pola penggunaan aplikasi yang dapat membantu membangun profil aktivitas pengguna.

#### Solved Problem 8 ⭐

**Soal:** Apa informasi yang dapat diperoleh dari analisis Prefetch file `MIMIKATZ.EXE-A234B567.pf`?

**Penyelesaian:**

*Step 1:* Identifikasi program — Mimikatz adalah tool credential dumping yang sering digunakan oleh penyerang

*Step 2:* Identifikasi informasi yang tersimpan

| Informasi dari Prefetch | Contoh | Nilai Forensik |
|------------------------|--------|----------------|
| Nama program | MIMIKATZ.EXE | Konfirmasi tool hacking dijalankan |
| Run count | Misal: 3 kali | Penyerang menjalankan 3 kali |
| Last execution time(s) | 8 timestamp terbaru | Kapan saja tool dijalankan |
| Referenced files | DLL yang di-load | Versi dan konteks eksekusi |
| Volume path | D:\Tools\ | Dari mana program dijalankan |

**Jawaban:** Dari Prefetch file tersebut dapat diperoleh: (1) Konfirmasi bahwa Mimikatz — tool credential dumping — pernah dijalankan, (2) Jumlah eksekusi (run count), (3) Hingga 8 timestamp kapan saja dieksekusi, (4) File dan directory yang diakses saat eksekusi, (5) Volume/drive asal program. Ini merupakan bukti kuat bahwa terjadi upaya pencurian kredensial pada sistem.

---

## 5. Jump Lists dan Recent Documents

### 5.1 Jump Lists

> **Jump Lists** adalah fitur Windows (sejak Windows 7) yang menyimpan daftar file, folder, dan task yang baru digunakan untuk setiap aplikasi. Jump Lists muncul ketika user klik kanan pada ikon di taskbar.

Lokasi Jump Lists:
```
C:\Users\[username]\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\
C:\Users\[username]\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations\
```

| Tipe | Ekstensi | Deskripsi |
|------|----------|-----------|
| **Automatic** | .automaticDestinations-ms | Dibuat otomatis oleh Windows saat file dibuka |
| **Custom** | .customDestinations-ms | Dibuat oleh aplikasi (pinned items, frequent items) |

#### 5.1.1 Informasi dalam Jump Lists

Jump Lists mengandung data forensik kaya termasuk path file lengkap, timestamp akses, volume serial number, dan bahkan path jaringan (UNC path). Ini sangat berharga untuk menunjukkan file apa saja yang diakses pengguna dan dari lokasi mana.

### 5.2 Recent Documents (LNK Files)

> **LNK Files** (shortcut files) adalah file Windows yang berisi referensi ke file atau folder lain. Windows secara otomatis membuat LNK file di folder Recent ketika pengguna membuka dokumen.

Lokasi:
```
C:\Users\[username]\AppData\Roaming\Microsoft\Windows\Recent\
```

| Data dalam LNK File | Contoh | Nilai Forensik |
|---------------------|--------|----------------|
| **Target path** | D:\Secret\classified.docx | File yang diakses |
| **Working directory** | D:\Secret\ | Lokasi kerja |
| **Timestamps** | Created, Modified, Accessed | Timeline penggunaan |
| **Volume serial number** | A1B2-C3D4 | Identifikasi media/drive |
| **Volume label** | USB_BACKUP | Nama drive |
| **Machine ID** | NetBIOS name | Komputer asal file |
| **MAC address** | 00:1A:2B:3C:4D:5E | Identifikasi perangkat jaringan |

![Artefak Jump Lists dan LNK Files](images/p05-04-jumplist-lnk-artifacts.png)

*Gambar 5.4: Hubungan antara Jump Lists, LNK files, dan Recent Documents*

### 5.3 Tools Analisis

| Tool | Pengembang | Fungsi |
|------|-----------|--------|
| **JLECmd** | Eric Zimmerman | Parsing Jump Lists |
| **LECmd** | Eric Zimmerman | Parsing LNK files |
| **JumpList Explorer** | Eric Zimmerman | GUI Jump List viewer |
| **LNK Parser** | TZWorks | Analisis LNK files |

#### Solved Problem 9 ⭐

**Soal:** Bagaimana Jump Lists dapat membantu investigasi kasus pencurian data rahasia militer melalui USB?

**Penyelesaian:**

*Step 1:* Pahami skenario — tersangka diduga menyalin file rahasia ke USB

*Step 2:* Identifikasi informasi relevan dari Jump Lists

| Data Jump Lists | Informasi | Bukti |
|----------------|-----------|-------|
| Path file | E:\Classified\operasi_garuda.docx | Dokumen rahasia yang dibuka dari USB |
| Volume serial | F8A2-B3C1 | Identifikasi USB drive spesifik |
| Timestamp | 2026-01-15 02:30 | Akses di luar jam kerja |
| Application | WINWORD.EXE | Dibuka dengan Microsoft Word |

**Jawaban:** Jump Lists dapat menunjukkan: (1) File spesifik yang dibuka dari USB drive (path E:\), (2) Volume serial number untuk mengidentifikasi USB drive tertentu, (3) Timestamp akses yang menunjukkan kapan file dibuka, (4) Aplikasi yang digunakan untuk membuka file. Kombinasi data ini membuktikan bahwa tersangka mengakses file rahasia dari media penyimpanan eksternal.

---

#### Solved Problem 10 ⭐⭐

**Soal:** Jelaskan perbedaan antara Automatic dan Custom Jump Lists serta bagaimana keduanya berkontribusi pada investigasi forensik!

**Penyelesaian:**

*Step 1:* Bandingkan kedua tipe Jump Lists

| Aspek | Automatic Jump Lists | Custom Jump Lists |
|-------|---------------------|-------------------|
| **Dibuat oleh** | Windows secara otomatis | Aplikasi tertentu |
| **Konten** | File terbaru yang dibuka per aplikasi | Pinned items, frequent items, task |
| **Kapan dibuat** | Setiap kali file dibuka | Saat user melakukan pin atau aplikasi menyimpan |
| **Lokasi** | AutomaticDestinations\ | CustomDestinations\ |

*Step 2:* Kontribusi pada investigasi

- **Automatic**: Memberikan histori komprehensif file yang dibuka, termasuk file yang sudah dihapus dari disk
- **Custom**: Menunjukkan file yang sering digunakan atau di-pin oleh pengguna, mengindikasikan file yang dianggap penting

**Jawaban:** **Automatic Jump Lists** dibuat otomatis oleh Windows setiap kali file dibuka dan memberikan histori lengkap akses file — bahkan untuk file yang sudah dihapus. **Custom Jump Lists** dibuat oleh aplikasi untuk menyimpan pinned dan frequent items, menunjukkan file yang dianggap penting oleh pengguna. Keduanya saling melengkapi: Automatic memberikan bukti aktivitas, Custom memberikan konteks prioritas pengguna.

---

## 6. Browser Artifacts

### 6.1 Konsep Browser Forensics

> **Browser Forensics** adalah analisis artefak yang ditinggalkan oleh web browser untuk merekonstruksi aktivitas browsing pengguna. Browser adalah salah satu sumber bukti forensik paling kaya karena merekam hampir semua aktivitas online.

#### 6.1.1 Lokasi Data Browser

| Browser | Lokasi Profil | Database Utama |
|---------|--------------|----------------|
| **Chrome** | `%LocalAppData%\Google\Chrome\User Data\Default\` | History, Cookies, Login Data (SQLite) |
| **Firefox** | `%AppData%\Mozilla\Firefox\Profiles\[profile]\` | places.sqlite, cookies.sqlite |
| **Edge** | `%LocalAppData%\Microsoft\Edge\User Data\Default\` | History, Cookies (format Chrome) |

#### 6.1.2 Jenis Artefak Browser

| Artefak | Deskripsi | Lokasi (Chrome) | Nilai Forensik |
|---------|-----------|-----------------|----------------|
| **History** | URL yang dikunjungi dengan timestamp | History | Aktivitas browsing |
| **Cookies** | Data sesi dan preferensi website | Cookies | Identifikasi akun |
| **Cache** | Salinan halaman web dan media | Cache/ | Konten yang diakses (meski sudah dihapus dari server) |
| **Downloads** | Daftar file yang diunduh | History (tabel downloads) | File yang diperoleh dari internet |
| **Bookmarks** | Halaman yang ditandai | Bookmarks (JSON) | Minat dan situs yang sering dikunjungi |
| **Autofill** | Data formulir tersimpan | Web Data | Informasi pribadi yang dimasukkan |
| **Login Data** | Kredensial tersimpan | Login Data | Username dan password |
| **Session/Tab** | Tab dan sesi yang aktif | Sessions, Tabs | Aktivitas browsing terakhir |

### 6.2 Tools Analisis Browser

| Tool | Platform | Kelebihan |
|------|----------|-----------|
| **Hindsight** | Chrome/Edge | Open source, Python, output CSV/XLSX |
| **DB Browser for SQLite** | Semua browser | Langsung membuka database SQLite |
| **Browser History Examiner** | Semua browser | GUI lengkap, timeline view |
| **ChromeCacheView** | Chrome | Analisis cache file |
| **MZCacheView** | Firefox | Analisis cache Firefox |

#### Solved Problem 11 ⭐

**Soal:** Sebutkan lokasi penyimpanan data history browsing pada Google Chrome dan jelaskan format database-nya!

**Penyelesaian:**

*Step 1:* Identifikasi lokasi file

```
C:\Users\[username]\AppData\Local\Google\Chrome\User Data\Default\History
```

*Step 2:* Jelaskan format database

File `History` pada Chrome adalah database SQLite3 yang berisi beberapa tabel penting:

| Tabel | Isi | Forensik |
|-------|-----|----------|
| `urls` | URL, jumlah kunjungan, last visit time | Situs yang dikunjungi |
| `visits` | Detail setiap kunjungan, timestamp, transition type | Timeline kunjungan |
| `downloads` | URL sumber, path tujuan, timestamp | File yang diunduh |
| `keyword_search_terms` | Kata kunci pencarian | Apa yang dicari pengguna |

**Jawaban:** Data history Chrome disimpan dalam file `History` di path `%LocalAppData%\Google\Chrome\User Data\Default\`. File ini berformat database SQLite3 dengan tabel utama: `urls` (daftar situs), `visits` (detail kunjungan), `downloads` (file unduhan), dan `keyword_search_terms` (pencarian). Database ini dapat dibuka menggunakan DB Browser for SQLite atau tool forensik seperti Hindsight.

---

## 7. Windows Timeline dan Activity History

### 7.1 Windows Timeline

> **Windows Timeline** (diperkenalkan di Windows 10 ver. 1803) adalah fitur yang merekam aktivitas pengguna secara kronologis, termasuk aplikasi yang dibuka, dokumen yang diedit, dan website yang dikunjungi.

Database Timeline disimpan di:
```
C:\Users\[username]\AppData\Local\ConnectedDevicesPlatform\L.[username]\ActivitiesCache.db
```

File `ActivitiesCache.db` adalah database SQLite yang berisi:

| Tabel | Data | Nilai Forensik |
|-------|------|----------------|
| `Activity` | Aplikasi yang digunakan, timestamp | Rekonstruksi aktivitas harian |
| `ActivityOperation` | Operasi yang dilakukan | Detail aksi pengguna |
| `Activity_PackageId` | Package ID aplikasi | Identifikasi aplikasi |

### 7.2 Activity History

Meskipun Microsoft telah mengurangi fitur Timeline di Windows 11, database ActivitiesCache.db masih dapat berisi data historis yang sangat berharga. Data ini mencakup aktivitas lintas perangkat jika pengguna menggunakan akun Microsoft yang tersinkronisasi.

#### Solved Problem 12 ⭐⭐

**Soal:** Seorang anggota TNI dicurigai mengakses dokumen rahasia di luar jam kerja. Bagaimana Windows Timeline dapat membantu membuktikan atau membantah dugaan ini?

**Penyelesaian:**

*Step 1:* Identifikasi data relevan di ActivitiesCache.db

Database ActivitiesCache.db menyimpan rekaman detail aktivitas pengguna termasuk:
- Aplikasi yang dibuka (AppId)
- Timestamp mulai dan selesai (StartTime, EndTime)
- Durasi aktivitas (ActiveDuration)
- Konten yang diakses (ContentUri)

*Step 2:* Analisis untuk membuktikan/membantah dugaan

| Data Timeline | Bukti yang Dihasilkan |
|---------------|----------------------|
| AppId = Microsoft.Office.WINWORD | Membuka Microsoft Word |
| ContentUri = D:\Classified\OPSEC-Report.docx | Dokumen rahasia spesifik |
| StartTime = 2026-01-20 23:45:00 | Di luar jam kerja (malam hari) |
| EndTime = 2026-01-21 00:30:00 | Durasi 45 menit |

**Jawaban:** Windows Timeline melalui database ActivitiesCache.db dapat menunjukkan: (1) Aplikasi apa yang dibuka (Word, Excel), (2) Dokumen spesifik yang diakses (file path lengkap), (3) Timestamp mulai dan selesai yang tepat, (4) Durasi penggunaan. Data ini dapat membuktikan apakah tersangka membuka dokumen rahasia di luar jam kerja dengan bukti timestamp yang terdokumentasi otomatis oleh sistem.

---

## 8. Recycle Bin Analysis

### 8.1 Konsep Recycle Bin Forensics

> **Recycle Bin** pada Windows menyimpan file yang dihapus pengguna beserta metadata originalnya. Analisis Recycle Bin sangat penting karena pengguna sering menghapus bukti ke Recycle Bin tanpa menyadari bahwa data masih dapat dipulihkan.

Lokasi Recycle Bin:
```
C:\$Recycle.Bin\[SID]\
```

#### 8.1.1 Struktur File Recycle Bin (Windows 10/11)

Untuk setiap file yang dihapus, Windows membuat dua file:

| File | Deskripsi | Isi |
|------|-----------|-----|
| **$I[ID]** | Information/metadata file | Path asli, ukuran file, timestamp penghapusan |
| **$R[ID]** | Renamed/actual file | Konten asli file yang dihapus |

Contoh:
- `$IABCDEF.docx` → Metadata (path asli: D:\Secret\classified.docx, deleted: 2026-01-15 14:30)
- `$RABCDEF.docx` → File asli yang dihapus

#### 8.1.2 Informasi dalam $I File

| Field | Deskripsi | Offset |
|-------|-----------|--------|
| Header | Version (2 = Win10) | Byte 0-7 |
| File Size | Ukuran file asli | Byte 8-15 |
| Deletion Time | Timestamp penghapusan (FILETIME) | Byte 16-23 |
| File Path | Path lengkap file asli (Unicode) | Byte 24+ |

### 8.2 Analisis Recycle Bin dalam Konteks Militer

Dalam investigasi insiden keamanan militer, Recycle Bin sering mengandung:
- Dokumen rahasia yang coba dihapus oleh insider threat
- Tools hacking yang dihapus setelah digunakan
- Log atau file konfigurasi yang menunjukkan aktivitas mencurigakan

#### Solved Problem 13 ⭐

**Soal:** Apa fungsi file $I dan $R dalam Recycle Bin Windows 10? Berikan contoh!

**Penyelesaian:**

*Step 1:* Jelaskan fungsi masing-masing file

| File | Fungsi | Analogi |
|------|--------|---------|
| **$I** | Menyimpan metadata (path asli, ukuran, waktu hapus) | "Label" pada paket yang disimpan |
| **$R** | Menyimpan konten file asli | Paket asli yang disimpan |

*Step 2:* Berikan contoh

Jika file `D:\Operasi\rencana_operasi.pdf` dihapus:
- `$I9X2K3L.pdf` → berisi: path asli, ukuran 2.5MB, dihapus 2026-01-15 14:30
- `$R9X2K3L.pdf` → file PDF asli dengan isi lengkap

**Jawaban:** File **$I** menyimpan metadata file yang dihapus (path asli, ukuran, timestamp penghapusan), sedangkan file **$R** menyimpan konten asli file yang dihapus. Contoh: jika `rencana_operasi.pdf` dihapus dari `D:\Operasi\`, maka `$I[ID].pdf` berisi metadata dan `$R[ID].pdf` berisi dokumen PDF asli. Forensik Recycle Bin memungkinkan pemulihan file dan konteks penghapusan.

---

#### Solved Problem 14 ⭐⭐

**Soal:** Investigator menemukan bahwa Recycle Bin pada workstation tersangka sudah dikosongkan (emptied). Apakah file masih bisa di-recovery? Jelaskan!

**Penyelesaian:**

*Step 1:* Pahami apa yang terjadi saat Recycle Bin dikosongkan

Ketika Recycle Bin dikosongkan:
- File $I dan $R dihapus dari $Recycle.Bin
- Entry di file system (MFT untuk NTFS) ditandai sebagai "available"
- **Namun**, konten data pada disk belum tentu di-overwrite

*Step 2:* Evaluasi kemungkinan recovery

| Kondisi | Kemungkinan Recovery | Metode |
|---------|---------------------|--------|
| HDD, baru dikosongkan | Tinggi | File carving di unallocated space |
| HDD, sudah lama dikosongkan | Sedang | Tergantung aktivitas disk |
| SSD dengan TRIM aktif | Rendah | TRIM mungkin sudah menghapus data fisik |
| Data di MFT resident | Tinggi | Data kecil (<700 byte) masih di MFT entry |

**Jawaban:** Ya, file **masih berpotensi di-recovery** meskipun Recycle Bin sudah dikosongkan, tergantung kondisi. Pada HDD, data fisik belum di-overwrite dan dapat di-carve menggunakan teknik file carving di unallocated space. Pada SSD, recovery lebih sulit karena TRIM mungkin sudah menghapus data secara fisik. File kecil yang MFT-resident juga masih bisa ditemukan. Tools seperti PhotoRec dan Autopsy dapat digunakan untuk file carving. (Teknik recovery detail dibahas di [Pertemuan 09](../p09/modul.md))

---

## 9. LNK Files dan Shortcut Analysis

### 9.1 Analisis Mendalam LNK Files

LNK files merupakan salah satu artefak paling informatif di Windows karena menyimpan banyak metadata tentang target file, termasuk informasi yang mungkin sudah tidak tersedia di tempat lain.

#### 9.1.1 Skenario Penggunaan LNK dalam Forensik Militer

| Skenario | Data LNK yang Relevan | Bukti |
|----------|----------------------|-------|
| **USB data theft** | Volume serial, drive label | Identifikasi USB yang digunakan |
| **Akses file rahasia** | Target path, timestamps | Kapan dan file apa yang diakses |
| **Lateral movement** | Network path (UNC), MAC address | Akses ke share jaringan |
| **Timeline** | Created, modified, accessed | Kronologi aktivitas |

#### Solved Problem 15 ⭐⭐

**Soal:** Analisis LNK file menunjukkan data berikut. Apa kesimpulan forensiknya?

```
Target Path: \\192.168.10.50\CLASSIFIED\intel_brief_q4.xlsx
Created: 2026-01-10 08:15:00
Modified: 2026-01-10 09:30:00
Accessed: 2026-01-12 23:45:00
Volume Serial: A1B2-C3D4
Machine ID: SRV-KODAM-05
```

**Penyelesaian:**

*Step 1:* Analisis setiap elemen data

| Data | Interpretasi |
|------|-------------|
| Target Path | File Excel di network share \\192.168.10.50\CLASSIFIED — folder classified |
| Created | Dibuat 10 Jan 2026 pagi |
| Modified | Terakhir diubah 10 Jan pagi |
| Accessed | Diakses 12 Jan 2026 pukul 23:45 — **di luar jam kerja** |
| Volume Serial | Identifikasi volume jaringan |
| Machine ID | File diakses dari/berada di server SRV-KODAM-05 |

*Step 2:* Korelasikan temuan

Akses pada pukul 23:45 ke folder `CLASSIFIED` pada server Kodam melalui network share mengindikasikan aktivitas mencurigakan di luar jam kerja.

**Jawaban:** Temuan menunjukkan: (1) File intelijen (`intel_brief_q4.xlsx`) di folder CLASSIFIED diakses melalui network share, (2) Akses terakhir pada pukul 23:45 — di luar jam kerja normal — menunjukkan aktivitas anomali, (3) Machine ID `SRV-KODAM-05` mengidentifikasi server sumber di Kodam, (4) Investigator harus mengkorelasikan dengan event logs server untuk mengidentifikasi user yang melakukan akses.

---

## 10. Thumbnail Cache dan IconCache

### 10.1 Thumbnail Cache (Thumbcache)

> **Thumbnail Cache** adalah database yang menyimpan versi kecil (thumbnail) dari gambar, video, dan dokumen yang pernah ditampilkan di Windows Explorer. Artefak ini sangat berharga karena thumbnail tetap tersimpan bahkan setelah file asli dihapus.

Lokasi:
```
C:\Users\[username]\AppData\Local\Microsoft\Windows\Explorer\
```

File-file thumbnail cache:
- `thumbcache_16.db` — thumbnail 16×16 piksel
- `thumbcache_32.db` — thumbnail 32×32 piksel
- `thumbcache_96.db` — thumbnail 96×96 piksel
- `thumbcache_256.db` — thumbnail 256×256 piksel
- `thumbcache_1024.db` — thumbnail 1024×1024 piksel
- `thumbcache_idx.db` — index database

### 10.2 IconCache

> **IconCache** (`iconcache.db`) menyimpan ikon dari program yang pernah diinstal atau dijalankan, termasuk program yang sudah di-uninstall.

Lokasi:
```
C:\Users\[username]\AppData\Local\Microsoft\Windows\Explorer\iconcache_*.db
```

### 10.3 Nilai Forensik

| Artefak | Bukti yang Tersimpan | Nilai Investigasi |
|---------|---------------------|-------------------|
| **Thumbcache** | Thumbnail gambar/video/dokumen yang pernah dilihat | Membuktikan keberadaan file gambar/video meskipun sudah dihapus |
| **IconCache** | Ikon program yang pernah ada | Membuktikan program pernah diinstal meskipun sudah di-uninstall |

#### Solved Problem 16 ⭐⭐

**Soal:** Investigator menganalisis thumbnail cache dari laptop seorang prajurit dan menemukan thumbnail gambar peta operasi militer, namun file asli sudah dihapus dan Recycle Bin sudah dikosongkan. Apa nilai forensik temuan ini?

**Penyelesaian:**

*Step 1:* Pahami karakteristik thumbnail cache

Thumbnail cache menyimpan versi kecil dari gambar yang pernah ditampilkan di Windows Explorer. Thumbnail **tidak** ikut terhapus ketika file asli dihapus atau Recycle Bin dikosongkan.

*Step 2:* Evaluasi nilai forensik

| Aspek | Nilai |
|-------|-------|
| **Bukti keberadaan** | Membuktikan file gambar peta operasi pernah ada di komputer |
| **Konten visual** | Thumbnail dapat menunjukkan konten gambar meskipun resolusi rendah |
| **Timeline** | Metadata thumbnail menunjukkan kapan file pertama kali dilihat |
| **Korelasi** | Dapat dikorelasikan dengan Recycle Bin $I files untuk konfirmasi |

**Jawaban:** Temuan thumbnail memiliki nilai forensik tinggi: (1) Membuktikan secara visual bahwa gambar peta operasi pernah ada di komputer meskipun file asli sudah dihapus, (2) Thumbnail tidak ikut terhapus saat file atau Recycle Bin dikosongkan, (3) Menyediakan bukti visual konten file (meski resolusi rendah), (4) Dapat dikorelasikan dengan artefak lain untuk membangun timeline. Ini merupakan bukti penting dalam kasus kebocoran informasi operasi militer.

---

## 11. Integrasi Artefak dan Timeline Analysis

### 11.1 Korelasi Multi-Artefak

Kekuatan forensik Windows terletak pada kemampuan mengkorelasikan berbagai sumber artefak untuk membangun gambaran aktivitas yang komprehensif:

| Artefak | Menjawab Pertanyaan |
|---------|---------------------|
| **Event Logs** | Siapa yang login? Kapan? Dari mana? |
| **Prefetch** | Program apa yang dijalankan? Berapa kali? |
| **Jump Lists** | File apa yang dibuka? Dari drive mana? |
| **LNK Files** | File apa yang diakses? Di komputer mana? |
| **Browser Artifacts** | Website apa yang dikunjungi? Apa yang diunduh? |
| **Recycle Bin** | File apa yang dihapus? Kapan? |
| **Timeline** | Apa aktivitas kronologis pengguna? |
| **Thumbnail Cache** | Gambar/video apa yang pernah dilihat? |
| **Memory** | Proses apa yang berjalan? Koneksi ke mana? |

![Korelasi Multi-Artefak Windows](images/p05-05-multi-artifact-correlation.png)

*Gambar 5.5: Korelasi multi-artefak Windows untuk rekonstruksi kejadian*

#### Solved Problem 17 ⭐⭐

**Soal:** Jelaskan bagaimana Prefetch, Event Logs, dan Jump Lists saling melengkapi dalam investigasi eksekusi program mencurigakan!

**Penyelesaian:**

*Step 1:* Identifikasi kontribusi masing-masing artefak

| Artefak | Kontribusi | Pertanyaan yang Dijawab |
|---------|------------|------------------------|
| **Prefetch** | Riwayat eksekusi program | Apa yang dijalankan? Berapa kali? Kapan? |
| **Event Logs** | Konteks keamanan dan sistem | Siapa yang menjalankan? Hak akses apa? |
| **Jump Lists** | File yang diakses program | File apa yang dibuka oleh program? |

*Step 2:* Contoh integrasi

Investigasi eksekusi `powershell.exe` yang mencurigakan:

1. **Prefetch** (`POWERSHELL.EXE-[hash].pf`): Menunjukkan PowerShell dijalankan 5 kali, terakhir pada 2026-01-15 02:00
2. **Event Log** (Event ID 4688): Menunjukkan PowerShell dijalankan oleh user `admin_backup` dengan privilege escalation (Event ID 4672)
3. **Jump Lists** (PowerShell): Menunjukkan script `C:\Temp\exfil.ps1` diakses

**Jawaban:** Ketiga artefak saling melengkapi: **Prefetch** membuktikan program dijalankan dan frekuensinya, **Event Logs** mengidentifikasi siapa yang menjalankan dan dengan hak akses apa, **Jump Lists** menunjukkan file yang diakses oleh program. Kombinasi ketiganya memberikan gambaran lengkap: apa yang dijalankan, oleh siapa, kapan, dan untuk mengakses apa.

---

#### Solved Problem 18 ⭐⭐⭐

**Soal:** Seorang personel di Kodam XIV/Hasanuddin diduga melakukan eksfiltrasi data melalui USB drive. Susun rencana analisis artefak Windows yang komprehensif untuk membuktikan atau membantah dugaan ini!

**Penyelesaian:**

*Step 1:* Identifikasi hipotesis investigasi

Hipotesis: Personel menyalin data rahasia dari workstation ke USB drive dan menghapus jejak.

*Step 2:* Rancang analisis multi-artefak

| Tahap | Artefak | Tujuan Analisis | Tool |
|-------|---------|-----------------|------|
| 1 | **Event Logs** (Security) | Identifikasi siapa yang login, kapan | FullEventLogView |
| 2 | **Event Logs** (System 7045) | Apakah ada service USB baru dipasang | EvtxECmd |
| 3 | **Prefetch** | Program apa yang dijalankan (explorer, xcopy, robocopy) | PECmd |
| 4 | **Jump Lists** | File apa yang dibuka — cari path E:\ atau F:\ (USB) | JLECmd |
| 5 | **LNK Files** | Konfirmasi akses file + volume serial USB | LECmd |
| 6 | **Recycle Bin** | File yang dihapus setelah penyalinan | $I parser |
| 7 | **Browser History** | Apakah ada upload ke cloud setelah penyalinan | Hindsight |
| 8 | **Thumbnail Cache** | Visualisasi dokumen yang pernah diakses | Thumbcache Viewer |
| 9 | **Timeline** | Rekonstruksi kronologi lengkap | WxTCmd |

*Step 3:* Bangun timeline terintegrasi

```
[08:00] Login Event 4624 - user: TNI_Officer_X
[08:15] Prefetch: EXPLORER.EXE - navigasi ke folder classified
[08:20] Event 7045: USB Mass Storage Device service installed
[08:22] LNK: E:\Backup\ terdeteksi (Volume Serial: XXXX-YYYY)
[08:25] Jump Lists: D:\Classified\operasi.docx dibuka
[08:30] Jump Lists: E:\Backup\operasi.docx terdeteksi  ← FILE DISALIN
[08:35] Recycle Bin: $I file menunjukkan D:\Classified\temp_copy.docx dihapus
[08:40] Event Logs: USB device disconnected
[09:00] Prefetch: CCLEANER.EXE ← UPAYA MENGHAPUS JEJAK
```

**Jawaban:** Rencana analisis mencakup 9 tahap pemeriksaan artefak yang saling mengkonfirmasi: Event Logs untuk identifikasi login dan pemasangan USB, Prefetch untuk riwayat eksekusi program, Jump Lists dan LNK untuk bukti akses file ke/dari USB, Recycle Bin untuk file yang dihapus, Browser History untuk kemungkinan upload tambahan, Thumbnail Cache untuk visualisasi dokumen, dan Timeline untuk kronologi lengkap. Semua temuan diintegrasikan dalam timeline untuk membangun bukti komprehensif.

---

#### Solved Problem 19 ⭐⭐⭐

**Soal:** Dalam investigasi serangan APT terhadap jaringan pertahanan, memory dump menunjukkan proses `csrss.exe` berjalan dari `C:\Users\Public\csrss.exe` dengan koneksi keluar ke IP 185.x.x.x port 443. Lakukan analisis komprehensif terhadap temuan ini!

**Penyelesaian:**

*Step 1:* Analisis anomali proses

| Aspek | Normal | Temuan |
|-------|--------|--------|
| Lokasi csrss.exe | `C:\Windows\System32\` | `C:\Users\Public\` ← **ANOMALI** |
| Parent process | smss.exe | Perlu diperiksa |
| Jumlah instance | 1-2 (Session 0 dan 1) | Perlu diperiksa |
| Koneksi jaringan | Tidak ada | Koneksi ke 185.x.x.x:443 ← **ANOMALI** |

*Step 2:* Identifikasi taktik penyerang

| Taktik MITRE ATT&CK | Teknik | Temuan |
|---------------------|--------|--------|
| **Defense Evasion** | Masquerading (T1036) | Nama file meniru proses sistem |
| **Persistence** | Location di Public folder | Folder yang writable oleh semua user |
| **C2 Communication** | HTTPS (port 443) | Komunikasi terenkripsi ke IP eksternal |

*Step 3:* Langkah investigasi lanjutan

1. Gunakan `windows.malfind` untuk mendeteksi injected code
2. Gunakan `windows.netscan` untuk mapping semua koneksi
3. Periksa Prefetch untuk timestamp eksekusi pertama
4. Periksa Event Logs untuk identifikasi vektor infeksi awal
5. Analisis IP 185.x.x.x untuk threat intelligence

**Jawaban:** Temuan ini mengindikasikan **malware APT yang menyamar sebagai proses sistem**. Tiga anomali kritis: (1) `csrss.exe` asli hanya berjalan dari `System32`, bukan `C:\Users\Public`, (2) `csrss.exe` asli tidak melakukan koneksi jaringan keluar, (3) Koneksi ke IP eksternal port 443 menunjukkan komunikasi C2 terenkripsi. Ini konsisten dengan teknik masquerading (MITRE ATT&CK T1036). Langkah lanjutan: analisis malfind untuk injected code, mapping jaringan lengkap, dan threat intelligence terhadap IP tujuan.

---

#### Solved Problem 20 ⭐⭐⭐

**Soal:** Jelaskan bagaimana investigator dapat membangun "Super Timeline" yang mengintegrasikan semua artefak Windows yang telah dipelajari! Sebutkan tools yang diperlukan dan langkah-langkahnya!

**Penyelesaian:**

*Step 1:* Konsep Super Timeline

> **Super Timeline** adalah timeline terintegrasi yang menggabungkan timestamps dari semua sumber artefak forensik ke dalam satu format kronologis terpadu.

*Step 2:* Tools yang diperlukan

| Tool | Fungsi |
|------|--------|
| **log2timeline/plaso** | Mengekstrak timestamp dari berbagai sumber artefak dan menggabungkannya |
| **Timeline Explorer** (Eric Zimmerman) | GUI viewer untuk super timeline |
| **KAPE** (Kroll Artifact Parser and Extractor) | Koleksi dan parsing artefak otomatis |

*Step 3:* Langkah pembuatan

| Langkah | Aksi | Output |
|---------|------|--------|
| 1 | Kumpulkan forensic image | Raw/E01 image |
| 2 | Jalankan KAPE untuk ekstraksi artefak | Parsed artifacts (CSV) |
| 3 | Jalankan log2timeline pada image | Plaso database |
| 4 | Filter dan export menggunakan psort | CSV timeline |
| 5 | Buka di Timeline Explorer | Visualisasi interaktif |
| 6 | Filter berdasarkan timeframe insiden | Focused timeline |

*Step 4:* Sumber data yang diintegrasikan

Prefetch, Event Logs, Jump Lists, LNK files, $MFT timestamps, Recycle Bin, Browser artifacts, Registry, Windows Timeline, Thumbnail Cache, dan lainnya — semua diintegrasikan ke satu timeline.

**Jawaban:** Super Timeline dibangun dengan: (1) Mengumpulkan forensic image, (2) Mengekstrak artefak menggunakan KAPE, (3) Menggunakan log2timeline/plaso untuk menggabungkan semua timestamps dari berbagai sumber (Prefetch, Event Logs, Jump Lists, LNK, MFT, Recycle Bin, Browser, Registry) ke satu format terpadu, (4) Memfilter menggunakan psort berdasarkan timeframe insiden, (5) Menganalisis dengan Timeline Explorer untuk visualisasi interaktif. Super Timeline memberikan gambaran kronologis lengkap dari semua aktivitas pada sistem.

---

#### Solved Problem 21 ⭐⭐⭐

**Soal:** Bandingkan Volatility 3 dengan Volatility 2 dalam konteks analisis memory forensics. Mengapa Volatility 3 lebih direkomendasikan untuk investigasi modern?

**Penyelesaian:**

*Step 1:* Perbandingan teknis

| Aspek | Volatility 2 | Volatility 3 |
|-------|-------------|--------------|
| **Bahasa** | Python 2 | Python 3 |
| **Profil OS** | Memerlukan profile selection manual | Auto-detection melalui symbol tables |
| **Arsitektur** | Monolitik | Modular dengan layer system |
| **Support Windows** | Hingga Windows 10 awal | Windows 10/11 terbaru |
| **Kecepatan** | Lebih lambat | Lebih cepat (optimized) |
| **Maintenance** | Deprecated (Python 2 end-of-life) | Aktif dikembangkan |
| **Plugin System** | Plugin terpisah per OS | Unified plugin framework |

*Step 2:* Keunggulan Volatility 3

1. **Auto-detection**: Tidak perlu menebak profil OS — mengurangi kesalahan
2. **Future-proof**: Python 3 aktif, Python 2 sudah end-of-life
3. **Modern OS support**: Mendukung Windows 10/11 dengan update terbaru
4. **Performance**: Lebih cepat berkat arsitektur yang dioptimasi
5. **Symbol tables**: Menggunakan ISF (Intermediate Symbol Format) yang lebih fleksibel

**Jawaban:** Volatility 3 direkomendasikan karena: (1) Berbasis Python 3 yang masih aktif dikembangkan (Volatility 2 berbasis Python 2 yang sudah end-of-life), (2) Auto-detection profil OS tanpa perlu seleksi manual, (3) Mendukung Windows 10/11 versi terbaru, (4) Arsitektur modular yang lebih cepat dan fleksibel, (5) Menggunakan symbol tables (ISF) yang lebih mudah di-update. Untuk investigasi militer modern yang memerlukan analisis sistem terkini, Volatility 3 adalah pilihan yang tepat.

---

#### Solved Problem 22 ⭐

**Soal:** Sebutkan lima artefak Windows yang tetap tersimpan meskipun file asli sudah dihapus!

**Penyelesaian:**

*Step 1:* Identifikasi artefak persisten

| No | Artefak | Lokasi | Data yang Tersimpan |
|----|---------|--------|---------------------|
| 1 | **Thumbnail Cache** | thumbcache_*.db | Thumbnail gambar/video/dokumen |
| 2 | **Jump Lists** | AutomaticDestinations/ | Path dan metadata file yang dibuka |
| 3 | **LNK Files** | Recent/ | Path, timestamps, volume info |
| 4 | **Prefetch** | Prefetch/ | Riwayat eksekusi program |
| 5 | **Event Logs** | winevt/Logs/ | Aktivitas sistem dan keamanan |

**Jawaban:** Lima artefak yang bertahan setelah file dihapus: (1) **Thumbnail Cache** — gambar kecil file yang pernah ditampilkan, (2) **Jump Lists** — riwayat file yang dibuka per aplikasi, (3) **LNK Files** — shortcut dengan metadata lengkap, (4) **Prefetch** — riwayat eksekusi program, (5) **Event Logs** — catatan aktivitas sistem. Artefak-artefak ini menjadi bukti keberadaan file meskipun file asli sudah dihapus dan Recycle Bin dikosongkan.

---

## Supplementary Problems

### Problem S1 ⭐
**Soal:** Apa yang dimaksud dengan "fileless malware" dan mengapa memory forensics penting untuk mendeteksinya?

**Jawaban:** **Fileless malware** adalah malware yang beroperasi sepenuhnya di memori (RAM) tanpa menulis file ke disk. Karena tidak ada file di disk, antivirus berbasis signature sulit mendeteksinya. Memory forensics menjadi satu-satunya cara untuk mengidentifikasi dan menganalisis fileless malware — melalui analisis proses, injected code, dan koneksi jaringan di RAM.

---

### Problem S2 ⭐
**Soal:** Mengapa Event ID 1102 sangat penting dalam investigasi forensik?

**Jawaban:** Event ID 1102 menunjukkan bahwa Security audit log telah dihapus. Ini penting karena: (1) Penghapusan log adalah teknik anti-forensik untuk menghilangkan jejak, (2) Event ini sendiri tetap terekam sehingga investigator tahu log pernah dihapus, (3) Informasi siapa yang menghapus log tersimpan dalam event, (4) Timestamp penghapusan memberikan indikasi kapan upaya anti-forensik dilakukan.

---

### Problem S3 ⭐⭐
**Soal:** Bagaimana cara Prefetch pada Windows 10 dapat menyimpan hingga 8 timestamp eksekusi? Apa keuntungan forensiknya?

**Jawaban:** Windows 10 memperluas header Prefetch dari 1 timestamp (Windows XP/7) menjadi 8 timestamp terbaru. Setiap kali program dijalankan, timestamp baru ditambahkan ke array (dengan FIFO — oldest digantikan). Keuntungan forensik: investigator dapat melihat pola eksekusi program (misalnya, tool hacking dijalankan 8 kali dalam seminggu) yang memberikan konteks tentang frekuensi dan pola penggunaan.

---

### Problem S4 ⭐⭐
**Soal:** Jelaskan teknik DKOM (Direct Kernel Object Manipulation) dan bagaimana memory forensics dapat mengatasinya!

**Jawaban:** **DKOM** adalah teknik rootkit yang memanipulasi linked list kernel (EPROCESS) untuk menyembunyikan proses dari daftar proses normal. Rootkit menghapus entry proses dari doubly-linked list sehingga tidak muncul di Task Manager atau `pslist`. Memory forensics mengatasinya dengan `psscan` yang melakukan scan pola di seluruh memori tanpa bergantung pada linked list — sehingga proses yang disembunyikan tetap terdeteksi.

---

### Problem S5 ⭐⭐⭐
**Soal:** Dalam konteks investigasi insiden di pangkalan militer TNI, rancang prosedur pengumpulan volatile data yang tidak mengganggu operasi sistem C2 (Command and Control) yang sedang aktif!

**Jawaban:** Prosedur pengumpulan volatile data pada sistem C2 aktif:

1. **Persiapan**: Siapkan tools pada USB forensik yang sudah diverifikasi (hash-checked) — DumpIt, WinPmem, Sysinternals
2. **Dokumentasi awal**: Foto layar, catat waktu, dokumen status operasi
3. **Memory capture**: Jalankan DumpIt atau Belkasoft RAM Capturer — proses ini berjalan di background tanpa mengganggu operasi C2
4. **Network state**: Jalankan `netstat -anob > netstate.txt` dan `ipconfig /all > ipconfig.txt`
5. **Process list**: `tasklist /v > processes.txt` dan ambil output Process Explorer
6. **Event logs**: Salin `C:\Windows\System32\winevt\Logs\` ke media eksternal
7. **Prefetch**: Salin `C:\Windows\Prefetch\` ke media eksternal
8. **Verifikasi**: Hash semua file yang dikumpulkan

Kunci: semua tools dijalankan dari USB read-only, output ke USB terpisah, minimal footprint pada sistem target.

---

## Ringkasan

| Konsep | Deskripsi Singkat |
|--------|-------------------|
| **Order of Volatility** | Urutan prioritas pengumpulan bukti: CPU registers → RAM → Network → Disk → Backup |
| **Memory Forensics** | Analisis RAM untuk proses, koneksi, encryption keys, malware fileless |
| **Volatility 3** | Framework Python 3 untuk analisis memory dump dengan auto-detection |
| **Windows Event Logs** | Security (4624, 4625, 1102), System (7045), PowerShell (4104) |
| **Prefetch** | Riwayat eksekusi program: nama, run count, 8 timestamps, referenced files |
| **Jump Lists** | Riwayat file yang dibuka per aplikasi: Automatic dan Custom |
| **LNK Files** | Shortcut dengan metadata: target path, timestamps, volume serial, machine ID |
| **Browser Artifacts** | History, cookies, cache, downloads, login data — format SQLite |
| **Windows Timeline** | ActivitiesCache.db — rekaman kronologis aktivitas pengguna |
| **Recycle Bin** | $I file (metadata) + $R file (konten asli) — file yang dihapus |
| **Thumbnail Cache** | Gambar kecil file yang pernah ditampilkan — bertahan setelah file dihapus |
| **Multi-Artefak** | Korelasi semua artefak untuk membangun Super Timeline |
| **Tools Utama** | Volatility 3, PECmd, JLECmd, LECmd, FullEventLogView, Hindsight, KAPE |

---

## Referensi

1. Carvey, H. (2022). *Windows Forensic Analysis* (5th Ed.). Academic Press. Chapter 3-5.
2. Casey, E. (2022). *Digital Evidence and Computer Crime: Forensic Science, Computers, and the Internet* (4th Ed.). Academic Press. Chapter 9.
3. Phillips, A., Nelson, B., & Steuart, C. (2022). *Guide to Computer Forensics and Investigations* (6th Ed.). Cengage Learning. Chapter 6.
4. Ligh, M.H., Case, A., Levy, J., & Walters, A. (2014). *The Art of Memory Forensics*. Wiley. Chapter 1-5.
5. Volatility Foundation. (2024). *Volatility 3 Documentation*. https://volatility3.readthedocs.io/
6. Zimmerman, E. (2024). *Eric Zimmerman's Tools Documentation*. https://ericzimmerman.github.io/
7. SANS Institute. (2023). *Windows Forensic Analysis Poster*. https://www.sans.org/posters/
8. RFC 3227: Guidelines for Evidence Collection and Archiving.

---

## License / Lisensi

This repository is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

Commercial use is permitted, provided attribution is given to the author.

© 2026 Anindito
