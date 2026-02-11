# Latihan Pertemuan 06: Forensik Windows II — Registry dan Artefak Pengguna

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 06  
**Topik:** Forensik Windows II — Registry dan Artefak Pengguna  
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
Windows Registry adalah:

A. Sistem file yang menyimpan dokumen pengguna  
B. Database hierarkis terpusat yang menyimpan konfigurasi sistem, hardware, software, dan preferensi pengguna  
C. Folder khusus di dalam direktori System32  
D. Program utilitas untuk mengelola driver hardware  
E. Log file yang mencatat semua aktivitas sistem

---

### Soal 2
Root key manakah yang menyimpan konfigurasi sistem dan hardware?

A. HKEY_CURRENT_USER  
B. HKEY_CLASSES_ROOT  
C. HKEY_LOCAL_MACHINE  
D. HKEY_CURRENT_CONFIG  
E. HKEY_USERS

---

### Soal 3
File registry hive yang menyimpan artefak per-pengguna seperti UserAssist dan MRU Lists adalah:

A. SAM  
B. SYSTEM  
C. SOFTWARE  
D. NTUSER.DAT  
E. SECURITY

---

### Soal 4
Lokasi file fisik dari hive SAM, SYSTEM, dan SOFTWARE di Windows adalah:

A. C:\Users\Public\Registry\  
B. C:\Windows\System32\config\  
C. C:\Windows\Registry\  
D. C:\ProgramData\Microsoft\Registry\  
E. C:\Windows\Temp\Registry\

---

### Soal 5
Dalam konteks registry forensik, "Last Write Time" merujuk pada:

A. Waktu terakhir value di dalam key dimodifikasi  
B. Waktu terakhir key itu sendiri dimodifikasi  
C. Waktu terakhir registry hive dibuka  
D. Waktu terakhir Windows melakukan boot  
E. Waktu terakhir backup registry dibuat

---

### Soal 6
Informasi riwayat perangkat USB yang terhubung ke komputer dapat ditemukan di registry path:

A. HKLM\SOFTWARE\Enum\USBSTOR  
B. HKLM\SYSTEM\ControlSet001\Enum\USBSTOR  
C. HKCU\Software\Microsoft\USB  
D. HKLM\HARDWARE\USB\Devices  
E. HKU\.DEFAULT\USB\History

---

### Soal 7
Artefak UserAssist merekam informasi tentang:

A. Semua executable yang dijalankan melalui command line  
B. Semua file yang disimpan ke disk  
C. Program GUI yang dijalankan melalui Windows Explorer  
D. Semua koneksi jaringan yang dibuat  
E. Semua proses yang berjalan di background

---

### Soal 8
Data UserAssist di-encode menggunakan cipher:

A. AES-128  
B. Base64  
C. ROT13  
D. XOR  
E. Caesar cipher dengan shift 5

---

### Soal 9
Artefak manakah yang menyediakan SHA-1 hash dari executable yang pernah dijalankan, memungkinkan identifikasi malware melalui VirusTotal?

A. UserAssist  
B. ShimCache (AppCompatCache)  
C. BAM/DAM  
D. AmCache  
E. Prefetch

---

### Soal 10
ShellBags merekam informasi tentang:

A. Password yang pernah disimpan di browser  
B. Perintah command line yang pernah dijalankan  
C. Pengaturan tampilan folder yang pernah dibuka pengguna di Windows Explorer  
D. File yang pernah dicetak  
E. Alamat email yang pernah dikirim

---

### Soal 11
Mengapa ShellBags sangat berharga dalam investigasi USB?

A. Merekam serial number dari setiap USB yang terhubung  
B. Merekam file yang disalin ke dan dari USB  
C. Merekam folder yang pernah dibrowse pengguna pada USB, bahkan setelah USB dicabut  
D. Merekam password untuk USB yang dienkripsi  
E. Merekam waktu koneksi dan pemutusan USB

---

### Soal 12
Artefak MRU (Most Recently Used) manakah yang merekam file yang dibuka atau disimpan melalui dialog box Open/Save Windows?

A. RecentDocs  
B. RunMRU  
C. TypedPaths  
D. OpenSaveMRU (OpenSavePidlMRU)  
E. TypedURLs

---

### Soal 13
Perbedaan utama antara ShimCache dan AmCache adalah:

A. ShimCache hanya tersedia di Windows 7, AmCache di Windows 10  
B. ShimCache menyimpan hash file, AmCache tidak  
C. AmCache menyimpan SHA-1 hash executable, ShimCache tidak menyimpan hash  
D. ShimCache berada di NTUSER.DAT, AmCache di SYSTEM  
E. Keduanya identik tanpa perbedaan signifikan

---

### Soal 14
BAM (Background Activity Moderator) memiliki keunggulan forensik dibandingkan ShimCache karena:

A. BAM menyimpan hash SHA-256 dari setiap executable  
B. BAM tersedia di semua versi Windows  
C. BAM langsung mengasosiasikan eksekusi dengan SID pengguna spesifik  
D. BAM merekam seluruh parameter command line  
E. BAM merekam durasi eksekusi setiap program

---

### Soal 15
Profil jaringan wireless yang pernah terhubung ke komputer Windows tersimpan di:

A. HKCU\Software\Microsoft\WiFi  
B. HKLM\SYSTEM\Network\Profiles  
C. HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles  
D. HKLM\HARDWARE\Network\WiFi  
E. HKU\.DEFAULT\Network\History

---

### Soal 16
Transaction log files (.LOG1, .LOG2) pada registry hive penting secara forensik karena:

A. Menyimpan backup lengkap dari registry  
B. Merekam semua perubahan sejak registry pertama kali dibuat  
C. Dapat mengandung data yang belum di-flush ke hive utama, terutama setelah crash  
D. Menyimpan password pengguna dalam plaintext  
E. Merekam waktu akses setiap key oleh setiap pengguna

---

### Soal 17
Tool forensik yang BUKAN bagian dari Eric Zimmerman suite adalah:

A. Registry Explorer  
B. ShellBags Explorer  
C. AmcacheParser  
D. USBDeview  
E. RECmd

---

### Soal 18
Keuntungan utama Registry Explorer dibandingkan Regedit bawaan Windows untuk analisis forensik adalah:

A. Registry Explorer gratis sedangkan Regedit berbayar  
B. Registry Explorer dapat membuka hive secara offline dan melakukan replay transaction logs  
C. Registry Explorer memiliki antarmuka yang lebih menarik  
D. Registry Explorer dapat menghapus malware dari registry  
E. Registry Explorer dapat mengenkripsi registry untuk perlindungan

---

### Soal 19
Saat melakukan korelasi USB forensik, untuk menentukan drive letter yang di-assign ke USB tertentu, investigator harus memeriksa:

A. USBSTOR  
B. MountedDevices  
C. ShellBags  
D. MountPoints2  
E. DeviceClasses

---

### Soal 20
Semua timestamp di Windows Registry disimpan dalam format:

A. Unix Epoch (detik sejak 1 Januari 1970)  
B. Local time berdasarkan timezone pengguna  
C. UTC (Coordinated Universal Time)  
D. GMT+7 (Waktu Indonesia Barat)  
E. Format tanggal lokal sesuai regional settings

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Sebutkan lima root key utama dalam Windows Registry beserta fungsi masing-masing!

---

### Soal 2 ⭐
Jelaskan perbedaan antara registry key, value, dan data type! Berikan contoh untuk masing-masing!

---

### Soal 3 ⭐
Apa yang dimaksud dengan registry hive? Sebutkan enam hive utama beserta lokasi file fisiknya di Windows!

---

### Soal 4 ⭐
Jelaskan empat metode yang dapat digunakan untuk mengekstraksi registry hive files dari sistem Windows!

---

### Soal 5 ⭐⭐
Jelaskan informasi apa saja yang dapat diperoleh dari analisis SAM hive dan bagaimana informasi tersebut berguna dalam investigasi forensik militer!

---

### Soal 6 ⭐⭐
Uraikan proses korelasi empat sumber registry untuk membangun profil lengkap penggunaan perangkat USB (USBSTOR, MountedDevices, MountPoints2, ShellBags)! Jelaskan informasi unik yang disediakan oleh masing-masing sumber!

---

### Soal 7 ⭐⭐
Jelaskan mekanisme encoding ROT13 pada UserAssist! Mengapa Windows menggunakan encoding ini? Berikan dua contoh decoding nama program dari ROT13!

---

### Soal 8 ⭐⭐
Bandingkan empat artefak eksekusi program (UserAssist, ShimCache, AmCache, BAM/DAM) dari aspek: lokasi hive, cakupan (per-user vs system-wide), ketersediaan hash, dan best use case!

---

### Soal 9 ⭐⭐
Jelaskan lima jenis MRU lists yang terdapat di NTUSER.DAT beserta nilai forensik masing-masing! Bagaimana informasi dari berbagai MRU lists dapat dikorelasikan?

---

### Soal 10 ⭐⭐
Apa yang dimaksud dengan wireless network profiles di registry? Informasi apa saja yang dapat diperoleh dan bagaimana MAC address access point dapat digunakan untuk geolokasi?

---

### Soal 11 ⭐⭐
Anda menemukan entry ShimCache untuk file `C:\Temp\payload.exe` pada workstation militer. Jelaskan langkah-langkah investigasi yang harus dilakukan untuk menentukan apakah file tersebut benar-benar dieksekusi, oleh siapa, dan kapan! Sebutkan artefak-artefak lain yang perlu diperiksa!

---

### Soal 12 ⭐⭐⭐
Jelaskan bagaimana seorang investigator dapat merekonstruksi timeline aktivitas mencurigakan menggunakan data dari minimal lima sumber registry yang berbeda! Berikan contoh skenario dan timeline yang dihasilkan!

---

### Soal 13 ⭐⭐⭐
Diskusikan empat teknik anti-forensik yang dapat diterapkan terhadap artefak registry dan empat teknik mitigasi yang dapat digunakan investigator untuk mengatasi masing-masing teknik tersebut!

---

### Soal 14 ⭐⭐⭐
Rancang Standard Operating Procedure (SOP) untuk analisis registry dalam investigasi forensik militer yang mencakup: persiapan, akuisisi, analisis, dan pelaporan! SOP harus mencakup minimal 10 langkah!

---

### Soal 15 ⭐⭐⭐
Dalam konteks pertahanan, jelaskan bagaimana analisis registry dapat diintegrasikan dengan artefak non-registry (Event Logs, Prefetch, $MFT, browser artifacts) untuk membangun kasus investigasi yang komprehensif! Berikan contoh skenario insider threat!

---

## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Kebocoran Data di Pangkalan Militer

**Latar Belakang:**

Seorang analis intelijen di Kodam XIV Hasanuddin, Sersan Satu Dian, dicurigai melakukan pencurian data rahasia dari workstation di ruang operasi. Aktivitas mencurigakan terdeteksi oleh sistem monitoring pada pukul 23:00 WIB, jauh di luar jam kerja normal. Sersan Satu Dian memiliki izin akses ke workstation tersebut untuk tugas rutin, namun akses di malam hari tidak sesuai prosedur.

Tim forensik telah mengakuisisi forensic image dari workstation tersebut dan mengekstraksi semua registry hive files beserta transaction logs.

**Temuan Awal dari Registry:**

Dari analisis SAM hive:
- Akun "dian_s1" (SID: S-1-5-21-3847291065-1234567890-9876543210-1005)
- Last Login: 2026-01-15 23:02:15 UTC
- Login Count: 247
- Account Status: Active
- Ditemukan juga akun "svc_backup" (RID: 1009) yang tidak terdaftar di dokumentasi IT

Dari analisis SYSTEM hive (USBSTOR):
- Disk&Ven_Kingston&Prod_DataTraveler&Rev_3.0
- Serial: 60A44C413B3EE431A135&0
- Properties timestamps menunjukkan: First Install: 2026-01-15 23:05:22 UTC, Last Connected: 2026-01-15 23:45:18 UTC, Last Removal: 2026-01-15 23:48:33 UTC

Dari analisis SOFTWARE hive (NetworkList):
- Profile "AndroidAP_Dian": DateCreated: 2026-01-15 23:50:01 UTC, DateLastConnected: 2026-01-15 23:50:01 UTC
- DefaultGatewayMac: A4:77:33:2B:8C:F1

Dari analisis NTUSER.DAT (akun dian_s1):
- UserAssist: `{1AC14E77}\JvaFPC.rkr` — Run Count: 3, Last Run: 2026-01-15 23:12:44 UTC
- UserAssist: `{1AC14E77}\Pzq.rkr` — Run Count: 5, Last Run: 2026-01-15 23:40:22 UTC
- RecentDocs: `Laporan_SIGINT_Q4_2025.xlsx`, `Daftar_Aset_Radar_Kodam.pdf`, `Intel_Brief_Januari.docx`
- OpenSaveMRU (.xlsx): `F:\Transfer\Laporan_SIGINT_Q4_2025.xlsx`
- OpenSaveMRU (.pdf): `F:\Transfer\Daftar_Aset_Radar_Kodam.pdf`
- TypedPaths: `\\SERVER-OPS\Classified\Intel_Reports`
- ShellBags: `F:\Transfer\`, `F:\Transfer\Intel\`, `F:\Transfer\Backup\`
- MountPoints2: Volume GUID `{ab12cd34-...}` terhubung ke drive `F:`

Dari analisis SYSTEM hive (BAM untuk SID-1005):
- `\Device\HarddiskVolume3\Windows\System32\xcopy.exe` — 2026-01-15 23:15:30 UTC
- `\Device\HarddiskVolume3\Windows\System32\cmd.exe` — 2026-01-15 23:40:22 UTC
- `\Device\HarddiskVolume3\Users\dian_s1\Desktop\WinSCP.exe` — 2026-01-15 23:52:10 UTC

Dari analisis AmCache:
- `WinSCP.exe` — SHA-1: 7A3B2C1D4E5F6789... — First Run: 2026-01-15 23:12:00 UTC — Publisher: Martin Prikryl

**Pertanyaan:**

**1a.** Decode semua entry UserAssist yang menggunakan ROT13 dan identifikasi program apa saja yang dijalankan oleh Sersan Satu Dian! (10 poin)

**1b.** Lakukan korelasi USB forensik lengkap menggunakan semua sumber registry yang tersedia (USBSTOR, MountedDevices/MountPoints2, ShellBags, OpenSaveMRU). Bangun profil lengkap perangkat USB! (15 poin)

**1c.** Rekonstruksi timeline kronologis seluruh aktivitas berdasarkan semua artefak registry yang ditemukan! Timeline harus mencakup minimal 10 event! (20 poin)

**1d.** Analisis akun "svc_backup" (RID: 1009) yang tidak terdokumentasi. Apa signifikansi forensiknya? Artefak registry tambahan apa yang harus diperiksa? (10 poin)

**1e.** Berdasarkan seluruh temuan, buat kesimpulan investigasi dan rekomendasi tindak lanjut! (10 poin)

---

### Studi Kasus 2: Deteksi Malware di Jaringan Pertahanan Udara

**Latar Belakang:**

Tim Cyber Defense Komando Pertahanan Udara Nasional (Kohanudnas) mendeteksi anomali pada tiga workstation di ruang kontrol radar. Workstation-workstation ini menunjukkan koneksi keluar ke IP address yang tidak dikenal pada jam-jam tidak biasa (02:00-04:00 WIB). Setelah melakukan forensic imaging, tim menganalisis registry dari ketiga workstation.

**Temuan Registry dari Workstation WS-RADAR-01:**

SAM Analysis:
- Akun normal: admin_radar (RID: 1001), operator1 (RID: 1002), operator2 (RID: 1003)
- Akun mencurigakan: "sysmon_svc" (RID: 1010), Account Creation: 2026-01-03 02:15:00 UTC, Login Count: 0, Account Flags: Hidden + Active

SYSTEM Hive — Services:
- Service Name: "WindowsSystemMonitor"
- ImagePath: `C:\Windows\Temp\svchost32.exe`
- Start Type: Automatic
- Description: "Windows System Performance Monitor"

SYSTEM Hive — ShimCache:
- `C:\Windows\Temp\svchost32.exe` — Modified: 2026-01-03 02:10:15 UTC
- `C:\Windows\Temp\mimikatz.exe` — Modified: 2026-01-03 02:20:30 UTC
- `C:\Windows\Temp\procdump.exe` — Modified: 2026-01-03 02:25:00 UTC
- `C:\Windows\System32\schtasks.exe` — Modified: 2026-01-03 02:30:00 UTC

AmCache Analysis:
- `svchost32.exe` — SHA-1: 1A2B3C4D5E6F... — Publisher: (none) — Link Date: 2025-12-28
- `mimikatz.exe` — SHA-1: 9F8E7D6C5B4A... — Publisher: Benjamin Delpy — Link Date: 2025-12-15

SOFTWARE Hive — NetworkList:
- Profile "RADAR-INTERNAL" (wired): DateCreated: 2024-06-01, DateLastConnected: 2026-01-15
- Profile "HOTEL-IBIS-WIFI": DateCreated: 2026-01-02, DateLastConnected: 2026-01-02
- DefaultGatewayMac (Hotel): B8:27:EB:3C:4D:5E

NTUSER.DAT (admin_radar):
- RunMRU: `cmd /c net user sysmon_svc P@ssw0rd123! /add /active:yes`
- TypedPaths: `C:\Windows\Temp`, `\\192.168.50.100\c$`
- ShellBags: `C:\Windows\Temp\`, `C:\Windows\Temp\tools\`, `C:\Users\admin_radar\Downloads\update_patch.zip`
- UserAssist menunjukkan `7-Zip` dijalankan 2x pada 2026-01-03 02:05 UTC

BAM (SID admin_radar):
- `C:\Windows\Temp\mimikatz.exe` — 2026-01-03 02:22:00 UTC
- `C:\Windows\Temp\procdump.exe` — 2026-01-03 02:26:00 UTC
- `C:\Windows\System32\schtasks.exe` — 2026-01-03 02:32:00 UTC

SYSTEM Hive — USBSTOR: Tidak ditemukan entry USB sejak 6 bulan terakhir.

**Temuan Serupa pada WS-RADAR-02 dan WS-RADAR-03:**

- Ditemukan file `svchost32.exe` di `C:\Windows\Temp\` dengan SHA-1 hash yang identik
- ShimCache dan AmCache menunjukkan eksekusi `svchost32.exe` namun TIDAK ada mimikatz atau procdump
- BAM menunjukkan eksekusi `svchost32.exe` oleh akun SYSTEM (bukan user account)
- Tidak ada entry di RunMRU atau UserAssist

**Pertanyaan:**

**2a.** Analisis akun "sysmon_svc" dan service "WindowsSystemMonitor". Jelaskan minimal 5 indikator yang menunjukkan ini adalah aktivitas malicious, bukan legitimate! (15 poin)

**2b.** Berdasarkan SHA-1 hash dari AmCache, jelaskan langkah investigasi yang harus dilakukan untuk mengidentifikasi svchost32.exe! Mengapa link date (compile time) penting dalam analisis ini? (10 poin)

**2c.** Rekonstruksi timeline serangan pada WS-RADAR-01 berdasarkan semua artefak registry! Identifikasi tahapan serangan dan tools yang digunakan! (20 poin)

**2d.** Bandingkan temuan antara WS-RADAR-01 dengan WS-RADAR-02/03. Apa yang dapat disimpulkan tentang metode penyebaran malware dan peran masing-masing workstation? (10 poin)

**2e.** Analisis temuan network profile "HOTEL-IBIS-WIFI" pada workstation radar militer. Apa signifikansi forensik dan implikasi keamanannya? Bagaimana MAC address AP dapat membantu investigasi? (10 poin)

---

## Kunci Jawaban

### Bagian A: Pilihan Ganda

| No | Jawaban | No | Jawaban |
|----|---------|-----|---------|
| 1 | B | 11 | C |
| 2 | C | 12 | D |
| 3 | D | 13 | C |
| 4 | B | 14 | C |
| 5 | B | 15 | C |
| 6 | B | 16 | C |
| 7 | C | 17 | D |
| 8 | C | 18 | B |
| 9 | D | 19 | B |
| 10 | C | 20 | C |

---

### Bagian B: Uraian

#### Jawaban Soal 1 ⭐

Lima root key utama Windows Registry:

| Root Key | Singkatan | Fungsi |
|----------|-----------|--------|
| **HKEY_LOCAL_MACHINE** | HKLM | Menyimpan konfigurasi sistem-wide: hardware, software terinstal, layanan, driver, dan konfigurasi keamanan |
| **HKEY_CURRENT_USER** | HKCU | Menyimpan preferensi dan konfigurasi user yang sedang login aktif |
| **HKEY_USERS** | HKU | Menyimpan profil semua pengguna yang pernah login, termasuk default profile |
| **HKEY_CLASSES_ROOT** | HKCR | Menyimpan asosiasi file (ekstensi → aplikasi), COM object registrations |
| **HKEY_CURRENT_CONFIG** | HKCC | Menyimpan profil hardware aktif saat ini (subset dari HKLM\SYSTEM) |

---

#### Jawaban Soal 2 ⭐

**Registry Key:** Container/folder dalam hierarki registry yang dapat memiliki subkeys dan values. Contoh: `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion`

**Registry Value:** Data individual yang tersimpan di dalam key, terdiri dari nama, tipe data, dan data itu sendiri. Contoh: Value name "ProgramFilesDir" dengan data "C:\Program Files"

**Data Types utama:**

| Tipe | Deskripsi | Contoh |
|------|-----------|--------|
| REG_SZ | String teks | `C:\Windows\notepad.exe` |
| REG_DWORD | Integer 32-bit | `0x00000001` |
| REG_BINARY | Data biner | `48 65 6C 6C 6F` |
| REG_EXPAND_SZ | String dengan variabel | `%SystemRoot%\system32` |
| REG_MULTI_SZ | Array string | `["Service1", "Service2"]` |

---

#### Jawaban Soal 3 ⭐

**Registry Hive** adalah file fisik di disk yang menyimpan sebagian dari database registry. Saat Windows boot, hive files dimuat ke memori dan dipetakan ke root keys yang sesuai.

| Hive | Lokasi File | Mapped To |
|------|-------------|-----------|
| **SAM** | `C:\Windows\System32\config\SAM` | HKLM\SAM |
| **SYSTEM** | `C:\Windows\System32\config\SYSTEM` | HKLM\SYSTEM |
| **SOFTWARE** | `C:\Windows\System32\config\SOFTWARE` | HKLM\SOFTWARE |
| **SECURITY** | `C:\Windows\System32\config\SECURITY` | HKLM\SECURITY |
| **NTUSER.DAT** | `C:\Users\<username>\NTUSER.DAT` | HKCU / HKU\<SID> |
| **UsrClass.dat** | `C:\Users\<username>\AppData\Local\Microsoft\Windows\UsrClass.dat` | HKCU\Software\Classes |

---

#### Jawaban Soal 4 ⭐

Empat metode ekstraksi registry hive:

1. **Dari Forensic Image:** Menggunakan FTK Imager atau Autopsy untuk menavigasi ke lokasi hive files dalam forensic image (E01/DD) dan mengekstraknya. Metode paling aman karena bekerja pada salinan.

2. **Live Extraction menggunakan FTK Imager:** Menjalankan FTK Imager pada sistem hidup untuk mengekstrak hive files yang sedang di-lock oleh OS. FTK Imager menggunakan Windows API khusus untuk bypass file lock.

3. **Registry Backup menggunakan `reg save`:** Menjalankan perintah `reg save HKLM\SYSTEM system.hiv` dari command prompt dengan hak Administrator. Mengeksport hive melalui Windows API.

4. **Volume Shadow Copy:** Menggunakan `vssadmin list shadows` untuk mengakses snapshot sistem sebelumnya yang berisi versi historis dari hive files. Berguna untuk melihat state registry pada waktu tertentu di masa lalu.

---

#### Jawaban Soal 5 ⭐⭐

**Informasi dari SAM Hive:**

| Artefak | Informasi | Nilai Forensik Militer |
|---------|-----------|----------------------|
| Username + SID | Identitas akun dan identifier unik | Menghubungkan aktivitas ke personel spesifik |
| RID | Relative Identifier (500=Admin, 501=Guest, 1000+=user) | Deteksi akun backdoor (RID di bawah 1000 yang tidak standar) |
| Last Login Time | Timestamp login terakhir | Verifikasi alibi, deteksi akses di luar jam kerja |
| Login Count | Jumlah total login | Pola penggunaan, anomali frekuensi |
| Account Creation Time | Kapan akun dibuat | Deteksi akun yang baru dibuat secara tidak sah |
| Password Last Changed | Kapan password terakhir diubah | Deteksi perubahan password mencurigakan |
| Account Flags | Status: active, disabled, locked, hidden | Deteksi akun hidden yang sengaja disembunyikan |
| Password Hashes (NT/LM) | Hash password | Analisis kekuatan password, credential analysis |

**Konteks Militer:** Analisis SAM dapat mengungkap akun backdoor yang dibuat oleh insider threat, akun yang di-reaktivasi setelah personel dipindahkan, dan anomali login yang menunjukkan akses tidak sah ke workstation pertahanan.

---

#### Jawaban Soal 6 ⭐⭐

**Proses Korelasi USB Forensik:**

| Sumber | Lokasi | Informasi Unik |
|--------|--------|----------------|
| **USBSTOR** | SYSTEM\ControlSet001\Enum\USBSTOR | Vendor, Product, Revision, Serial Number, timestamps koneksi (First Install, Last Connected, Last Removal) |
| **MountedDevices** | SYSTEM\MountedDevices | Mapping antara device identifier dan drive letter (mis. \DosDevices\E:) → menjawab "drive letter apa?" |
| **MountPoints2** | NTUSER.DAT\...\Explorer\MountPoints2 | Mengidentifikasi USER spesifik yang mengakses volume → menjawab "siapa yang mengakses?" |
| **ShellBags** | NTUSER.DAT dan UsrClass.dat | Folder yang pernah dibrowse pada USB → menjawab "folder apa yang dibuka?" Persists bahkan setelah USB dicabut |

**Proses Korelasi:**
1. Identifikasi device di USBSTOR → vendor, serial number
2. Match device identifier di MountedDevices → drive letter (misal F:)
3. Cek MountPoints2 untuk SID user → siapa yang mount volume
4. Analisis ShellBags untuk path dengan drive letter tersebut → folder apa yang dibrowse
5. Cek setupapi.dev.log untuk konfirmasi first install timestamp
6. Gabungkan semua informasi → profil USB lengkap: apa, siapa, kapan, apa yang dilakukan

---

#### Jawaban Soal 7 ⭐⭐

**ROT13 (Rotate by 13 places):**
- Substitution cipher sederhana yang mengganti setiap huruf dengan huruf ke-13 setelahnya dalam alfabet
- A↔N, B↔O, C↔P, dst.
- Angka, simbol, dan path separator tidak berubah
- ROT13 bersifat self-inverse: ROT13(ROT13(x)) = x

**Mengapa Windows menggunakan ROT13:**
- Bukan untuk keamanan (trivially reversible)
- Kemungkinan untuk mencegah user casual memodifikasi data secara langsung melalui Regedit
- Obfuscation ringan, bukan enkripsi

**Contoh Decoding:**

| Encoded (ROT13) | Decoded |
|-----------------|---------|
| `Pnyphyngbe` | `Calculator` |
| `JvaFPC.rkr` | `WinSCP.exe` |
| `Pzq.rkr` | `Cmd.exe` |
| `P:\Hfref\Nqzva\Qrfxgbc\` | `C:\Users\Admin\Desktop\` |

---

#### Jawaban Soal 8 ⭐⭐

**Perbandingan Empat Artefak Eksekusi:**

| Aspek | UserAssist | ShimCache | AmCache | BAM/DAM |
|-------|-----------|-----------|---------|---------|
| **Hive** | NTUSER.DAT | SYSTEM | Amcache.hve | SYSTEM |
| **Cakupan** | Per-user | System-wide | System-wide | Per-user (via SID) |
| **Hash** | Tidak ada | Tidak ada | SHA-1 | Tidak ada |
| **Run Count** | Ya | Tidak | Tidak | Tidak |
| **Timestamp** | Last execution | Last modified (file) | First execution | Last execution |
| **GUI vs CLI** | GUI only (Explorer) | Semua executable | Semua executable | Semua executable |
| **Windows Version** | Semua | Semua | Win 8+ | Win 10+ |
| **Best Use** | Frekuensi penggunaan GUI | Broad coverage, deleted files | Identifikasi malware via hash | Per-user execution timing |

**Strategi Korelasi:** Gunakan AmCache untuk identifikasi malware (SHA-1 → VirusTotal), BAM untuk menentukan siapa yang menjalankan (SID), UserAssist untuk frekuensi, dan ShimCache untuk cakupan terluas termasuk file yang sudah dihapus.

---

#### Jawaban Soal 9 ⭐⭐

**Lima Jenis MRU Lists di NTUSER.DAT:**

| MRU | Lokasi | Merekam | Nilai Forensik |
|-----|--------|---------|----------------|
| **RecentDocs** | `...\Explorer\RecentDocs` | File yang baru dibuka (per ekstensi) | Daftar file yang diakses user, dikelompokkan per tipe |
| **RunMRU** | `...\Explorer\RunMRU` | Perintah yang diketik di Run dialog (Win+R) | Perintah yang sengaja dijalankan — indikator intent |
| **TypedPaths** | `...\Explorer\TypedPaths` | Path yang diketik langsung di Explorer address bar | Network shares atau lokasi spesifik yang diakses secara manual |
| **TypedURLs** | `...\Internet Explorer\TypedURLs` | URL yang diketik di browser | Situs yang dikunjungi secara sengaja |
| **OpenSaveMRU** | `...\ComDlg32\OpenSavePidlMRU` | File yang dibuka/disimpan via dialog box Open/Save | Lokasi lengkap (termasuk drive letter) — kritis untuk korelasi USB |

**Korelasi MRU:** RecentDocs menunjukkan file apa yang dibuka, OpenSaveMRU menunjukkan lokasi lengkap termasuk drive letter (misal `F:\Transfer\file.xlsx`), yang kemudian dikorelasikan dengan USBSTOR (apa USB-nya), MountedDevices (drive letter), dan ShellBags (folder yang dibrowse).

---

#### Jawaban Soal 10 ⭐⭐

**Wireless Network Profiles** disimpan di `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles` dan `NetworkList\Signatures`.

**Informasi yang tersedia:**

| Value | Informasi | Nilai Forensik |
|-------|-----------|----------------|
| ProfileName | SSID jaringan | Identifikasi jaringan (mis. "Hotel-WiFi", "AndroidAP") |
| DateCreated | Timestamp pertama kali terhubung | Timeline koneksi awal |
| DateLastConnected | Timestamp koneksi terakhir | Aktivitas jaringan terkini |
| Category | Domain/Private/Public | Tipe jaringan |
| DefaultGatewayMac | MAC address access point | Identifikasi fisik AP |

**Geolokasi via MAC Address:**
1. Ambil DefaultGatewayMac dari registry
2. Query ke database WiGLE.net (Wireless Geographic Logging Engine)
3. WiGLE memetakan MAC address AP ke koordinat geografis berdasarkan wardriving data
4. Hasil: lokasi fisik di mana workstation pernah terhubung ke jaringan tersebut

**Konteks Militer:** Dapat membuktikan bahwa workstation militer pernah dibawa ke lokasi tidak sah (hotel, kafe, rumah), mendeteksi koneksi ke jaringan publik yang melanggar kebijakan keamanan, atau mendeteksi penggunaan personal hotspot untuk bypass monitoring jaringan militer.

---

#### Jawaban Soal 11 ⭐⭐

**Langkah Investigasi ShimCache `payload.exe`:**

1. **Periksa ShimCache detail:** Catat Last Modified Time dari entry — perhatikan bahwa di Windows 8+, entry di ShimCache TIDAK selalu berarti file dieksekusi

2. **Cross-check AmCache:** Cari `payload.exe` di Amcache.hve — jika ada entry, file PASTI pernah dijalankan. Catat SHA-1 hash, First Run timestamp, Publisher, dan Link Date

3. **Query SHA-1 ke VirusTotal:** Upload hash ke VirusTotal untuk identifikasi — apakah dikenali sebagai malware?

4. **Periksa BAM/DAM:** Cari entry di `SYSTEM\...\bam\State\UserSettings\<SID>` — jika ditemukan, identifikasi SID pengguna yang menjalankan dan timestamp eksekusi

5. **Periksa UserAssist:** Jika ada entry → dijalankan via GUI/Explorer. Jika tidak ada → kemungkinan dijalankan via command line atau script

6. **Periksa Prefetch:** File `PAYLOAD.EXE-{hash}.pf` di `C:\Windows\Prefetch` — konfirmasi eksekusi dan last run times

7. **Periksa Event Logs:** Security log (Event ID 4688) untuk process creation — konfirmasi eksekusi dengan parameter command line

8. **Periksa $MFT:** Timeline file creation, modification, access — deteksi timestomping jika ada diskrepansi dengan ShimCache

9. **Kesimpulan:** Gabungkan semua artefak untuk menentukan: (a) apakah benar dieksekusi, (b) oleh user mana (SID), (c) kapan (timeline), (d) bagaimana (GUI vs CLI), (e) apa nature-nya (malware/legitimate)

---

#### Jawaban Soal 12 ⭐⭐⭐

**Skenario: Insider Threat — Data Exfiltration**

**Sumber Registry untuk Timeline:**

1. **SAM:** Login timestamp (kapan login)
2. **USBSTOR + Properties:** USB connect/disconnect (kapan USB digunakan)
3. **UserAssist:** Program execution (apa yang dijalankan)
4. **BAM:** Execution timestamps per user (kapan program dijalankan, oleh siapa)
5. **OpenSaveMRU:** File access dengan lokasi lengkap (file apa, di mana)
6. **NetworkList:** Network connection timestamps (kapan terhubung ke jaringan)
7. **ShellBags:** Folder browsing (folder apa yang dibuka)

**Contoh Timeline Rekonstruksi:**

| No | Waktu (UTC) | Sumber | Kejadian |
|----|-------------|--------|----------|
| 1 | 22:00 | SAM | Login "suspect_user" (di luar jam kerja) |
| 2 | 22:05 | UserAssist | File Explorer dibuka |
| 3 | 22:08 | TypedPaths | Navigasi ke `\\SERVER\Classified` |
| 4 | 22:10 | RecentDocs | `Operation_Plan.docx` dibuka |
| 5 | 22:12 | USBSTOR | USB Kingston DataTraveler terhubung |
| 6 | 22:15 | OpenSaveMRU | `E:\Backup\Operation_Plan.docx` disimpan |
| 7 | 22:18 | BAM | `xcopy.exe` dijalankan oleh SID suspect |
| 8 | 22:25 | ShellBags | Folder `E:\Backup\Intel\` dibrowse |
| 9 | 22:30 | USBSTOR | USB disconnected |
| 10 | 22:32 | NetworkList | Terhubung ke "Personal_Hotspot" |
| 11 | 22:35 | BAM | `curl.exe` dijalankan (upload?) |
| 12 | 22:40 | SAM | Session berakhir |

**Analisis:** Timeline menunjukkan pola klasik eksfiltrasi dua tahap — salin ke removable media, lalu upload melalui jaringan pribadi yang bypass monitoring.

---

#### Jawaban Soal 13 ⭐⭐⭐

| Teknik Anti-Forensik | Deskripsi | Mitigasi |
|---------------------|-----------|----------|
| **Registry Wiping** | Menghapus key/value forensik penting menggunakan tools seperti CCleaner atau script custom | Replay transaction logs (.LOG1/.LOG2) yang mungkin masih mengandung data sebelum penghapusan; periksa Volume Shadow Copies untuk versi historis |
| **Timestamp Manipulation** | Memodifikasi Last Write Time pada key registry menggunakan tools khusus | Cross-reference dengan artefak non-registry (Event Logs, $MFT timestamps, Prefetch) yang lebih sulit dimanipulasi secara bersamaan |
| **UserAssist Clearing** | Menghapus entry UserAssist atau memodifikasi run count | Periksa BAM/DAM, AmCache, dan Prefetch sebagai sumber alternatif execution history; fakta bahwa UserAssist kosong padahal artefak lain menunjukkan aktivitas itu sendiri merupakan anomali |
| **Privacy Cleaner Usage** | Menggunakan CCleaner, BleachBit untuk menghapus berbagai artefak sekaligus | Deteksi keberadaan cleaner tool itu sendiri di ShimCache/AmCache/Prefetch; periksa RegBack folder (`C:\Windows\System32\config\RegBack`) untuk backup periodik; cari remnants di unallocated space |

**Prinsip Penting:** Kehadiran privacy cleaner tools sendiri merupakan artefak yang harus dilaporkan. Pola penghapusan selektif (hanya forensic artifacts, bukan data normal) menunjukkan kesadaran anti-forensik yang memperkuat kecurigaan.

---

#### Jawaban Soal 14 ⭐⭐⭐

**SOP Analisis Registry dalam Investigasi Forensik Militer:**

**Fase 1: Persiapan**
1. **Verifikasi otorisasi** — pastikan perintah investigasi dan surat tugas resmi
2. **Siapkan workstation forensik** — install Eric Zimmerman tools, RegRipper, USBDeview; verifikasi integritas tools

**Fase 2: Akuisisi**
3. **Verifikasi integritas forensic image** — validasi hash (SHA-256) dari forensic image
4. **Ekstrak registry hive files** — extract SAM, SYSTEM, SOFTWARE, SECURITY, NTUSER.DAT, UsrClass.dat, beserta transaction logs (.LOG1, .LOG2)
5. **Hitung hash individual hive** — dokumentasikan SHA-256 setiap hive file yang diekstrak

**Fase 3: Analisis**
6. **Load hive di Registry Explorer** — aktifkan "Replay Transaction Logs" untuk mendapatkan data terlengkap
7. **Analisis SAM** — identifikasi semua akun, SID, login timestamps, akun mencurigakan (hidden, backdoor)
8. **Analisis SYSTEM** — USBSTOR (USB history), ShimCache (execution), TimeZoneInformation (koreksi timestamp), Services (persistence)
9. **Analisis SOFTWARE** — installed programs, network profiles, wireless connections
10. **Analisis NTUSER.DAT per user** — UserAssist (decode ROT13), MRU lists, ShellBags, MountPoints2

**Fase 4: Korelasi dan Pelaporan**
11. **Korelasi multi-artefak** — bangun timeline terpadu dari semua sumber registry, cross-reference dengan artefak non-registry
12. **Dokumentasikan temuan** — buat laporan dengan screenshot, timestamp, path registry, dan interpretasi; sertakan chain of custody semua file

---

#### Jawaban Soal 15 ⭐⭐⭐

**Integrasi Registry dengan Artefak Non-Registry:**

| Artefak Registry | Artefak Non-Registry Pelengkap | Tujuan Korelasi |
|-----------------|-------------------------------|-----------------|
| UserAssist (execution) | Prefetch files | Konfirmasi eksekusi, dapatkan run count dan timestamps tambahan |
| ShimCache (execution) | Event Log 4688 (Process Creation) | Konfirmasi eksekusi + dapatkan parameter command line |
| USBSTOR (USB history) | setupapi.dev.log | Konfirmasi first install USB dengan presisi |
| RecentDocs (file access) | $MFT + $UsnJrnl | Timeline file creation/modification/deletion di filesystem |
| AmCache (SHA-1 hash) | VirusTotal / threat intel | Identifikasi malware, attribution |
| NetworkList (WiFi profiles) | WLAN event logs (11000-11006) | Detail koneksi/diskoneksi WiFi |

**Skenario Insider Threat:**

Seorang teknisi di markas TNI diduga meng-copy dokumen rahasia:

1. **Registry:** SAM menunjukkan login malam hari → USBSTOR menunjukkan USB baru → OpenSaveMRU menunjukkan file rahasia dibuka dari USB drive → BAM menunjukkan xcopy.exe dijalankan
2. **Event Logs:** Security 4624 (logon) mengonfirmasi login → 4688 menunjukkan `xcopy /s D:\Classified\* F:\` → 4663 menunjukkan akses ke file rahasia
3. **Filesystem:** $MFT menunjukkan file classified di-access → Prefetch `XCOPY.EXE-{hash}.pf` mengonfirmasi eksekusi
4. **Browser Artifacts:** History menunjukkan akses ke cloud storage setelah copy
5. **Korelasi:** Timeline terpadu dari 5+ sumber membangun narasi komprehensif yang sulit dibantah di pengadilan militer

---

### Bagian C: Studi Kasus

#### Studi Kasus 1: Kebocoran Data di Pangkalan Militer

**1a. Decode UserAssist ROT13 (10 poin)**

| Entry ROT13 | Decoded | Program |
|-------------|---------|---------|
| `JvaFPC.rkr` | `WinSCP.exe` | WinSCP — file transfer client (SCP/SFTP) |
| `Pzq.rkr` | `Cmd.exe` | Command Prompt |

**Program yang dijalankan Sersan Satu Dian:**
1. **WinSCP.exe** — Run Count: 3, Last Run: 23:12:44 UTC → tool transfer file via SCP/SFTP, dijalankan 3 kali
2. **Cmd.exe** — Run Count: 5, Last Run: 23:40:22 UTC → Command Prompt, dijalankan 5 kali

**Signifikansi:** WinSCP adalah tool yang umum digunakan untuk transfer file ke server remote via SSH/SCP/SFTP. Keberadaan dan penggunaannya pada workstation militer di malam hari sangat mencurigakan — mengindikasikan upaya eksfiltrasi data ke server eksternal.

---

**1b. Korelasi USB Forensik (15 poin)**

**Profil USB Lengkap:**

| Komponen | Sumber | Temuan |
|----------|--------|--------|
| **Device** | USBSTOR | Kingston DataTraveler, Rev 3.0 |
| **Serial Number** | USBSTOR | 60A44C413B3EE431A135&0 |
| **First Connect** | USBSTOR Properties | 2026-01-15 23:05:22 UTC |
| **Last Connect** | USBSTOR Properties | 2026-01-15 23:45:18 UTC |
| **Last Removal** | USBSTOR Properties | 2026-01-15 23:48:33 UTC |
| **Drive Letter** | MountPoints2 + Volume GUID | F: |
| **User** | MountPoints2 (SID-1005) | dian_s1 |
| **Folders Browsed** | ShellBags | `F:\Transfer\`, `F:\Transfer\Intel\`, `F:\Transfer\Backup\` |
| **Files Saved** | OpenSaveMRU | `F:\Transfer\Laporan_SIGINT_Q4_2025.xlsx`, `F:\Transfer\Daftar_Aset_Radar_Kodam.pdf` |

**Kesimpulan USB:** USB Kingston DataTraveler milik pribadi (bukan inventaris resmi) dihubungkan selama ~43 menit. User dian_s1 membuat dan mengakses folder-folder terstruktur (`Transfer`, `Intel`, `Backup`) di USB, dan menyimpan dokumen rahasia ke USB tersebut.

---

**1c. Timeline Kronologis (20 poin)**

| No | Waktu (UTC) | Sumber | Kejadian |
|----|-------------|--------|----------|
| 1 | 23:02:15 | SAM | Login akun "dian_s1" (di luar jam kerja) |
| 2 | 23:05:22 | USBSTOR | USB Kingston DataTraveler pertama kali terhubung (drive F:) |
| 3 | 23:12:00 | AmCache | WinSCP.exe pertama kali dijalankan (first run) |
| 4 | 23:12:44 | UserAssist | WinSCP.exe dijalankan via GUI |
| 5 | ~23:13-14 | TypedPaths | Navigasi manual ke `\\SERVER-OPS\Classified\Intel_Reports` |
| 6 | ~23:14-15 | RecentDocs | File-file intel dibuka (SIGINT, Radar, Intel Brief) |
| 7 | 23:15:30 | BAM | xcopy.exe dijalankan oleh SID-1005 → salin file ke USB |
| 8 | ~23:16-30 | OpenSaveMRU | File disimpan ke `F:\Transfer\` |
| 9 | ~23:16-30 | ShellBags | Folder `F:\Transfer\`, `F:\Transfer\Intel\`, `F:\Transfer\Backup\` dibrowse |
| 10 | 23:40:22 | BAM + UserAssist | cmd.exe dijalankan |
| 11 | 23:45:18 | USBSTOR | USB last connected (mungkin re-check) |
| 12 | 23:48:33 | USBSTOR | USB dicabut |
| 13 | 23:50:01 | NetworkList | Terhubung ke hotspot "AndroidAP_Dian" (personal hotspot) |
| 14 | 23:52:10 | BAM | WinSCP.exe dijalankan → kemungkinan upload file ke server remote via SFTP |

**Pola Aktivitas:** Eksfiltrasi dua tahap — pertama copy ke USB menggunakan xcopy, kemudian upload ke server remote menggunakan WinSCP melalui personal hotspot untuk bypass monitoring jaringan militer.

---

**1d. Analisis Akun svc_backup (10 poin)**

**Signifikansi Forensik:**

Akun "svc_backup" (RID: 1009) yang tidak terdokumentasi di sistem IT merupakan **red flag** serius:

- **Tidak ada di dokumentasi IT** → kemungkinan dibuat secara tidak sah
- **Nama menyerupai service account** → upaya menyamarkan akun sebagai legitimate
- **Perlu diperiksa:** apakah dibuat oleh dian_s1 atau pihak lain
- **Kemungkinan:** akun backdoor untuk akses di kemudian hari

**Artefak tambahan yang harus diperiksa:**
1. **SAM detail svc_backup:** Account Creation Time, Last Login, Login Count, Password Last Changed
2. **Security Event Log 4720:** Event pembuatan akun → siapa yang membuatnya
3. **RunMRU/BAM dari semua user:** Perintah `net user svc_backup ... /add`
4. **Group Membership:** Apakah ditambahkan ke grup Administrators?
5. **NTUSER.DAT svc_backup:** Jika ada, apakah pernah digunakan?
6. **Scheduled Tasks:** Apakah ada task yang berjalan dengan kredensial svc_backup?

---

**1e. Kesimpulan dan Rekomendasi (10 poin)**

**Kesimpulan:**

Berdasarkan analisis komprehensif terhadap semua artefak registry, terbukti kuat bahwa Sersan Satu Dian melakukan:
1. Akses tidak sah ke workstation di luar jam kerja (23:02 UTC)
2. Pencurian dokumen rahasia (SIGINT, aset radar, intel brief) menggunakan USB pribadi tidak terdaftar
3. Eksfiltrasi data melalui xcopy ke USB dan WinSCP melalui personal hotspot (bypass monitoring)
4. Kemungkinan pembuatan akun backdoor (svc_backup) — perlu investigasi lanjutan

**Rekomendasi:**
1. **Sita USB Kingston** (SN: 60A44C413B3EE431A135&0) dan handphone (sumber hotspot AndroidAP_Dian)
2. **Analisis WinSCP logs/configuration** untuk menentukan server tujuan upload
3. **Investigasi akun svc_backup** — audit creation source
4. **Periksa handphone** untuk bukti koordinasi dan penerima data
5. **Query MAC AP** (A4:77:33:2B:8C:F1) ke WiGLE untuk konfirmasi lokasi hotspot
6. **Revoke semua akses** Sersan Satu Dian dan nonaktifkan akun svc_backup
7. **Eskalasi** ke penegak hukum militer dan intelijen

---

#### Studi Kasus 2: Deteksi Malware di Jaringan Pertahanan Udara

**2a. Analisis Akun dan Service Malicious (15 poin)**

**Lima Indikator Malicious:**

| No | Indikator | Analisis |
|----|-----------|----------|
| 1 | **Akun hidden + active** | Account Flags menunjukkan akun sengaja disembunyikan — legitimate service accounts tidak perlu di-hide |
| 2 | **Login Count = 0** | Akun dibuat tapi tidak pernah login interaktif → dibuat untuk service/backdoor, bukan penggunaan normal |
| 3 | **ImagePath di Temp folder** | `C:\Windows\Temp\svchost32.exe` — service legitimate tidak pernah dijalankan dari Temp folder; svchost32.exe menyerupai svchost.exe (typosquatting) |
| 4 | **Nama menyesatkan** | "WindowsSystemMonitor" dan "sysmon_svc" menyerupai tools legitimate (Sysmon/Sysinternals) — social engineering untuk menghindari deteksi manual |
| 5 | **Waktu pembuatan dini hari** | Account Creation 02:15 UTC — konsisten dengan pola serangan (jam 02:00-04:00 WIB) untuk menghindari deteksi personel |
| Bonus | **RunMRU menunjukkan command** | `net user sysmon_svc P@ssw0rd123! /add /active:yes` — pembuatan akun via command line manual, bukan deployment resmi IT |

---

**2b. Investigasi SHA-1 Hash (10 poin)**

**Langkah Investigasi:**
1. **Query SHA-1 ke VirusTotal:** Upload hash `1A2B3C4D5E6F...` — cek apakah dikenali sebagai malware
2. **Query ke threat intelligence feeds:** MISP, AlienVault OTX, IBM X-Force
3. **Analisis Publisher:** svchost32.exe tidak memiliki Publisher → tidak digitally signed → highly suspicious
4. **Bandingkan dengan known malware:** Match dengan IOC database nasional dan internasional
5. **Reverse engineering** jika sample tersedia: analisis behavior, C2 communication, capabilities

**Pentingnya Link Date (Compile Time):**
- Link Date `2025-12-28` → dikompilasi 6 hari sebelum deployment (2026-01-03) → malware relatif baru atau custom-built
- Jika Link Date jauh berbeda dari timestamp lain → kemungkinan timestomping
- Link Date dapat digunakan untuk menghubungkan ke kampanye malware lain yang dikompilasi pada waktu sama
- Link Date yang sangat dekat dengan deploy date mengindikasikan targeted attack, bukan commodity malware

---

**2c. Timeline Serangan WS-RADAR-01 (20 poin)**

| No | Waktu (UTC) | Sumber | Kejadian | Tahap |
|----|-------------|--------|----------|-------|
| 1 | 2026-01-02 | NetworkList | Workstation terhubung ke "HOTEL-IBIS-WIFI" | Initial Access? |
| 2 | 2026-01-03 02:05 | UserAssist | 7-Zip dijalankan 2x → extract update_patch.zip | Delivery |
| 3 | 2026-01-03 02:10 | ShimCache | svchost32.exe dropped di `C:\Windows\Temp\` | Installation |
| 4 | 2026-01-03 02:15 | SAM | Akun "sysmon_svc" dibuat (hidden + active) | Persistence |
| 5 | 2026-01-03 02:15 | RunMRU | `net user sysmon_svc P@ssw0rd123! /add /active:yes` | Persistence |
| 6 | 2026-01-03 02:20 | ShimCache | mimikatz.exe diakses | Credential Access |
| 7 | 2026-01-03 02:22 | BAM | mimikatz.exe dieksekusi oleh admin_radar | Credential Access |
| 8 | 2026-01-03 02:25 | ShimCache | procdump.exe diakses | Credential Access |
| 9 | 2026-01-03 02:26 | BAM | procdump.exe dieksekusi (dump LSASS memory) | Credential Access |
| 10 | 2026-01-03 02:30 | ShimCache | schtasks.exe digunakan | Persistence |
| 11 | 2026-01-03 02:32 | BAM | schtasks.exe → buat scheduled task untuk svchost32.exe | Persistence |
| 12 | 2026-01-03 02:32+ | Services (SYSTEM) | Service "WindowsSystemMonitor" terdaftar (auto-start) | Persistence |

**Tahapan Serangan (MITRE ATT&CK):**
- Initial Access: Kemungkinan via koneksi WiFi hotel atau malicious update
- Execution: Extract ZIP → jalankan tools
- Persistence: Service creation + scheduled task + hidden user account
- Credential Access: mimikatz (credential dumping) + procdump (LSASS dump)

---

**2d. Perbandingan Workstation (10 poin)**

| Aspek | WS-RADAR-01 | WS-RADAR-02/03 |
|-------|-------------|----------------|
| **svchost32.exe** | Ada, SHA-1 identik | Ada, SHA-1 identik |
| **mimikatz/procdump** | Ada di ShimCache + BAM | Tidak ada |
| **User execution** | admin_radar (via BAM) | SYSTEM account |
| **RunMRU/UserAssist** | Ada (manual activity) | Tidak ada |
| **USB activity** | Tidak ada | Tidak ada |

**Kesimpulan Penyebaran:**
1. **WS-RADAR-01 adalah "patient zero"** — serangan manual oleh attacker (admin_radar compromised), evidenced by RunMRU, UserAssist, dan manual tool usage
2. **WS-RADAR-02/03 terinfeksi melalui lateral movement** — svchost32.exe disebarkan secara automated (tanpa interaksi GUI), dijalankan sebagai SYSTEM bukan user account
3. **Metode lateral:** Kemungkinan via credentials yang didump mimikatz → PsExec/remote service creation → auto-deploy svchost32.exe
4. **No credential tools di WS-02/03:** Attacker hanya perlu persistence, credential sudah didapat dari WS-01

---

**2e. Analisis Network Profile Hotel (10 poin)**

**Signifikansi Forensik:**

Keberadaan profil WiFi "HOTEL-IBIS-WIFI" pada workstation radar militer merupakan **anomali serius:**

1. **Pelanggaran Kebijakan:** Workstation radar militer seharusnya hanya terhubung ke jaringan internal (RADAR-INTERNAL). Koneksi ke jaringan hotel menunjukkan workstation pernah dibawa ke luar lingkungan militer atau jaringan hotel dijangkau dari lokasi militer.

2. **Vektor Serangan Potensial:** Koneksi ke WiFi publik hotel (2026-01-02) terjadi satu hari sebelum serangan (2026-01-03). Kemungkinan:
   - Man-in-the-Middle attack di jaringan hotel
   - Malicious update/download dari jaringan tidak aman
   - Drive-by download saat browsing dari jaringan hotel

3. **Analisis MAC Address:**
   - DefaultGatewayMac: `B8:27:EB:3C:4D:5E`
   - Prefix `B8:27:EB` = Raspberry Pi Foundation → AP ini menggunakan Raspberry Pi, BUKAN router hotel komersial
   - Implikasi: Kemungkinan **rogue access point** (evil twin) yang menyamar sebagai WiFi hotel menggunakan Raspberry Pi!

4. **Tindak Lanjut:**
   - Query MAC ke WiGLE.net untuk lokasi fisik AP
   - Konfirmasi apakah Hotel Ibis di sekitar pangkalan menggunakan router Raspberry Pi (hampir pasti tidak)
   - Investigasi siapa yang membawa/memindahkan workstation ke area jangkauan WiFi tersebut
   - Periksa physical access logs ke ruang radar

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
