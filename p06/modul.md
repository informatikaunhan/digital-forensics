# Modul 06: Forensik Windows II - Registry dan Artefak Pengguna

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 06  
**Topik:** Forensik Windows II - Registry dan Artefak Pengguna  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Menjelaskan struktur dan lokasi Windows Registry hives secara komprehensif
2. Menganalisis registry hives (NTUSER.DAT, SAM, SYSTEM, SOFTWARE, SECURITY) untuk investigasi forensik
3. Melakukan user account analysis dan profiling dari artefak registry
4. Mengekstraksi riwayat perangkat USB dan mounted devices dari registry
5. Menganalisis application execution artifacts (UserAssist, BAM/DAM, ShimCache, AmCache)
6. Menginterpretasikan Shell Bags dan MRU lists untuk merekonstruksi aktivitas pengguna
7. Melakukan timeline reconstruction menggunakan data registry dalam konteks investigasi militer

---

## 1. Pengenalan Windows Registry

### 1.1 Definisi dan Fungsi Registry

> **Windows Registry** adalah database hierarkis terpusat yang menyimpan informasi konfigurasi sistem operasi, perangkat keras, perangkat lunak, dan preferensi pengguna di sistem Windows. Dalam forensik digital, registry merupakan salah satu sumber bukti paling kaya karena merekam hampir semua aktivitas yang terjadi di sistem.

Dari perspektif forensik militer, Windows Registry menjadi sumber kritis untuk:
- Merekonstruksi aktivitas pengguna yang dicurigai melakukan kebocoran data
- Mengidentifikasi perangkat USB yang pernah terhubung ke sistem klasifikasi
- Mendeteksi instalasi perangkat lunak tidak sah di workstation militer
- Melacak koneksi jaringan yang tidak terotorisasi dari dalam jaringan pertahanan

| Aspek | Deskripsi |
|-------|-----------|
| **Jenis Data** | Konfigurasi OS, hardware, software, user preferences |
| **Struktur** | Hierarkis (tree-based), terdiri dari keys, subkeys, dan values |
| **Penyimpanan** | Tersimpan dalam file hive di disk |
| **Volatilitas** | Sebagian volatile (HKLM\HARDWARE), sebagian persistent |
| **Nilai Forensik** | Sangat tinggi — merekam aktivitas sistem dan pengguna |

### 1.2 Struktur Hierarki Registry

Windows Registry tersusun dalam struktur hierarki yang terdiri dari lima root keys utama:

| Root Key | Singkatan | Fungsi |
|----------|-----------|--------|
| **HKEY_LOCAL_MACHINE** | HKLM | Konfigurasi sistem dan hardware |
| **HKEY_CURRENT_USER** | HKCU | Preferensi pengguna yang sedang login |
| **HKEY_USERS** | HKU | Profil semua pengguna yang terdaftar |
| **HKEY_CLASSES_ROOT** | HKCR | Asosiasi file dan COM objects |
| **HKEY_CURRENT_CONFIG** | HKCC | Profil hardware aktif |

Setiap key dalam registry dapat memiliki:
- **Subkeys**: Cabang di bawah key utama
- **Values**: Data yang tersimpan dalam key, terdiri dari nama, tipe, dan data
- **Timestamps**: Last Write Time yang mencatat kapan key terakhir dimodifikasi

> **Penting untuk Forensik:** Setiap registry key memiliki timestamp "Last Write Time" yang merekam kapan key terakhir dimodifikasi. Namun, timestamp ini berlaku untuk **key**, bukan individual **value**. Jika satu value dalam key berubah, timestamp key akan diperbarui untuk semua values di dalamnya.

![Struktur Hierarki Windows Registry](images/p06-01-registry-hierarchy.png)

*Gambar 6.1: Struktur hierarki Windows Registry dan hubungan antar root keys*

#### Solved Problem 1 ⭐

**Soal:** Sebutkan lima root key utama Windows Registry dan jelaskan fungsi masing-masing!

**Penyelesaian:**

*Step 1:* Identifikasi kelima root key

*Step 2:* Jelaskan fungsi forensik masing-masing

| Root Key | Fungsi | Nilai Forensik |
|----------|--------|----------------|
| **HKLM** | Konfigurasi sistem, hardware, software yang terinstal | Informasi sistem, software, konfigurasi keamanan |
| **HKCU** | Preferensi pengguna yang sedang aktif login | Aktivitas pengguna saat ini |
| **HKU** | Profil semua pengguna terdaftar (termasuk Default) | Profil semua pengguna, perbandingan aktivitas |
| **HKCR** | Asosiasi tipe file dan objek COM | File type associations, default applications |
| **HKCC** | Profil hardware yang sedang aktif | Konfigurasi hardware saat ini |

**Jawaban:** Lima root key Windows Registry adalah HKLM (konfigurasi sistem/hardware), HKCU (preferensi pengguna aktif), HKU (profil semua pengguna), HKCR (asosiasi file), dan HKCC (profil hardware aktif). Dari perspektif forensik, HKLM dan HKU adalah sumber bukti paling kaya karena menyimpan informasi persisten tentang sistem dan semua pengguna.

---

### 1.3 Tipe Data Registry

Registry values dapat memiliki berbagai tipe data:

| Tipe | Nama Registry | Deskripsi | Contoh |
|------|--------------|-----------|--------|
| **REG_SZ** | String | Teks biasa | `"C:\Windows\notepad.exe"` |
| **REG_EXPAND_SZ** | Expandable String | Teks dengan variabel environment | `"%SystemRoot%\notepad.exe"` |
| **REG_BINARY** | Binary | Data biner mentah | `48 65 6C 6C 6F` |
| **REG_DWORD** | 32-bit Integer | Bilangan bulat 32-bit | `0x00000001` |
| **REG_QWORD** | 64-bit Integer | Bilangan bulat 64-bit | `0x0000000000000001` |
| **REG_MULTI_SZ** | Multi-String | Array string | `["value1", "value2"]` |
| **REG_NONE** | None | Tanpa tipe data tertentu | — |

> **Catatan Forensik:** Banyak artefak forensik penting (seperti UserAssist dan ShimCache) disimpan dalam format REG_BINARY yang memerlukan parsing khusus. Tools seperti Registry Explorer dari Eric Zimmerman dapat melakukan decoding otomatis terhadap data biner ini.

#### Solved Problem 2 ⭐

**Soal:** Apa perbedaan antara REG_SZ dan REG_EXPAND_SZ? Mengapa perbedaan ini penting dalam forensik?

**Penyelesaian:**

*Step 1:* Bandingkan kedua tipe data

| Aspek | REG_SZ | REG_EXPAND_SZ |
|-------|--------|---------------|
| **Konten** | String literal/statis | String dengan variabel environment |
| **Contoh** | `C:\Windows\notepad.exe` | `%SystemRoot%\notepad.exe` |
| **Ekspansi** | Tidak ada | `%SystemRoot%` → `C:\Windows` |

*Step 2:* Jelaskan implikasi forensik

**Jawaban:** REG_SZ menyimpan teks literal tanpa perubahan, sedangkan REG_EXPAND_SZ mengandung variabel environment (seperti `%SystemRoot%`, `%USERPROFILE%`) yang di-expand saat dibaca. Dalam forensik, ini penting karena: (1) malware sering menggunakan REG_EXPAND_SZ untuk persistence agar path tetap valid di berbagai sistem, dan (2) investigator harus memahami environment variables target untuk menerjemahkan path yang sebenarnya.

---

## 2. Registry Hive Files

### 2.1 Lokasi Hive Files di Disk

> **Registry Hive** adalah file fisik yang menyimpan bagian-bagian registry di disk. Setiap hive merepresentasikan subtree dari registry.

| Hive | Lokasi File | Registry Path |
|------|------------|---------------|
| **SAM** | `C:\Windows\System32\config\SAM` | HKLM\SAM |
| **SECURITY** | `C:\Windows\System32\config\SECURITY` | HKLM\SECURITY |
| **SOFTWARE** | `C:\Windows\System32\config\SOFTWARE` | HKLM\SOFTWARE |
| **SYSTEM** | `C:\Windows\System32\config\SYSTEM` | HKLM\SYSTEM |
| **DEFAULT** | `C:\Windows\System32\config\DEFAULT` | HKU\.DEFAULT |
| **NTUSER.DAT** | `C:\Users\<username>\NTUSER.DAT` | HKCU / HKU\<SID> |
| **UsrClass.dat** | `C:\Users\<username>\AppData\Local\Microsoft\Windows\UsrClass.dat` | HKCU\Software\Classes |

> **Penting:** File hive bersifat **locked** saat Windows berjalan. Untuk forensik, hive harus diekstraksi dari forensic image atau menggunakan tools khusus seperti FTK Imager untuk live extraction.

### 2.2 Transaction Logs

Setiap hive memiliki file transaction log yang mencatat perubahan sebelum di-commit ke hive utama:

- `SAM.LOG1`, `SAM.LOG2`
- `SYSTEM.LOG1`, `SYSTEM.LOG2`
- `SOFTWARE.LOG1`, `SOFTWARE.LOG2`
- dan seterusnya

> **Signifikansi Forensik:** Transaction logs dapat mengandung data yang belum di-flush ke hive utama, terutama jika sistem mengalami crash atau shutdown tidak normal. Registry Explorer Eric Zimmerman dapat menggabungkan (replay) transaction logs dengan hive untuk mendapatkan data terlengkap.

![Lokasi Registry Hive Files](images/p06-02-hive-locations.png)

*Gambar 6.2: Lokasi fisik registry hive files di sistem file Windows*

#### Solved Problem 3 ⭐

**Soal:** Sebutkan lokasi file NTUSER.DAT dan jelaskan mengapa file ini sangat penting dalam forensik!

**Penyelesaian:**

*Step 1:* Identifikasi lokasi file

NTUSER.DAT terletak di `C:\Users\<username>\NTUSER.DAT` dan merupakan file hidden dan system.

*Step 2:* Jelaskan signifikansi forensik

| Aspek | Deskripsi |
|-------|-----------|
| **Konten** | Semua pengaturan dan aktivitas spesifik pengguna |
| **Artefak** | UserAssist, MRU lists, typed URLs, recent docs |
| **Per-User** | Setiap akun memiliki NTUSER.DAT terpisah |
| **Ukuran** | Biasanya 5-50 MB, tergantung aktivitas |

**Jawaban:** NTUSER.DAT terletak di `C:\Users\<username>\NTUSER.DAT`. File ini sangat penting karena menyimpan semua preferensi dan artefak aktivitas spesifik pengguna, termasuk program yang dijalankan (UserAssist), file yang baru diakses (MRU lists), URL yang diketik di Explorer, dan pengaturan aplikasi. Setiap user account memiliki file NTUSER.DAT terpisah, memungkinkan investigator melakukan profiling per pengguna.

---

### 2.3 Ekstraksi Hive Files

Untuk analisis forensik, hive files harus diekstraksi dari forensic image atau sistem live menggunakan metode yang menjaga integritas bukti:

| Metode | Tool | Keterangan |
|--------|------|------------|
| **Dari Forensic Image** | FTK Imager, Autopsy | Navigasi ke lokasi hive, extract |
| **Live Extraction** | FTK Imager (live) | Mount physical drive, export files |
| **Registry Backup** | `reg save` command | `reg save HKLM\SAM C:\output\SAM` |
| **Volume Shadow Copy** | vssadmin, Arsenal Image Mounter | Akses snapshot sebelumnya |

#### Solved Problem 4 ⭐

**Soal:** Mengapa file registry hive tidak dapat disalin langsung menggunakan Windows Explorer saat sistem berjalan?

**Penyelesaian:**

*Step 1:* Pahami mekanisme locking

Windows mengunci (lock) file hive registry saat sistem beroperasi karena file tersebut terus-menerus dibaca dan ditulis oleh sistem operasi. Exclusive lock mencegah proses lain mengakses file.

*Step 2:* Jelaskan solusi forensik

| Metode Alternatif | Cara Kerja |
|-------------------|-----------|
| **FTK Imager (Live)** | Mengakses raw disk sector, bypass file system lock |
| **Volume Shadow Copy** | Mengakses salinan snapshot sebelumnya |
| **`reg save`** | Windows API untuk export hive yang sedang loaded |
| **Forensic Image** | Analisis offline dari image, tidak ada lock |

**Jawaban:** File registry hive di-lock secara eksklusif oleh Windows saat berjalan karena sistem terus membaca dan menulis ke file tersebut. Untuk mengekstraksi, investigator dapat menggunakan FTK Imager (mengakses raw disk), Volume Shadow Copy (snapshot sebelumnya), perintah `reg save` (export via Windows API), atau menganalisis secara offline dari forensic image.

---

## 3. SAM Hive — User Account Analysis

### 3.1 Struktur SAM Hive

> **SAM (Security Account Manager)** menyimpan informasi akun pengguna lokal, termasuk nama pengguna, Security Identifier (SID), password hashes, dan metadata akun.

Lokasi penting dalam SAM hive:

```
SAM\Domains\Account\Users\
├── Names\
│   ├── Administrator
│   ├── Guest
│   └── <username>
├── 000001F4 (Administrator - RID 500)
├── 000001F5 (Guest - RID 501)
└── 000003E8 (First user - RID 1000)
```

### 3.2 Informasi Akun yang Dapat Diekstraksi

| Artefak | Lokasi Registry | Nilai Forensik |
|---------|----------------|----------------|
| **Nama Pengguna** | SAM\Domains\Account\Users\Names | Identifikasi akun |
| **SID** | Dihitung dari RID | Korelasi dengan artefak lain |
| **Last Login Time** | Value F pada user key | Kapan terakhir login |
| **Last Password Change** | Value F pada user key | Kapan password diubah |
| **Account Creation** | Value F pada user key | Kapan akun dibuat |
| **Login Count** | Value F pada user key | Frekuensi penggunaan |
| **Account Flags** | Value F pada user key | Status aktif/disabled/locked |
| **Password Hints** | Value V pada user key | Petunjuk password (jika ada) |
| **Password Hashes** | Value V pada user key | NT/LM hash (untuk analisis) |

> **Konteks Militer:** Dalam investigasi insider threat di lingkungan TNI, analisis SAM hive dapat mengungkap kapan akun mencurigakan dibuat, frekuensi login, dan apakah ada akun tidak sah yang dibuat pada workstation pertahanan.

### 3.3 Security Identifier (SID)

SID adalah identifier unik untuk setiap akun dan grup di Windows:

```
Format: S-1-5-21-<Domain/Machine ID>-<RID>
Contoh: S-1-5-21-3623811015-3361044348-30300820-1013
```

| RID | Akun |
|-----|------|
| 500 | Administrator (built-in) |
| 501 | Guest |
| 1000+ | Akun pengguna yang dibuat |

#### Solved Problem 5 ⭐

**Soal:** Dari analisis SAM hive, ditemukan akun dengan RID 1001, last login time 2 hari sebelum insiden keamanan, dan login count sebanyak 47 kali. Apa yang dapat disimpulkan dari temuan ini?

**Penyelesaian:**

*Step 1:* Interpretasi RID

RID 1001 menunjukkan ini adalah akun pengguna kedua yang dibuat setelah akun default (akun pertama yang dibuat memiliki RID 1000).

*Step 2:* Analisis artefak

| Artefak | Nilai | Interpretasi |
|---------|-------|-------------|
| **RID** | 1001 | Akun pengguna ke-2 yang dibuat |
| **Last Login** | 2 hari sebelum insiden | Akun aktif digunakan menjelang insiden |
| **Login Count** | 47 | Pengguna reguler/aktif |

*Step 3:* Kesimpulan

**Jawaban:** Temuan menunjukkan bahwa akun dengan RID 1001 adalah akun pengguna reguler (kedua yang dibuat di sistem) yang aktif digunakan dengan 47 kali login. Last login 2 hari sebelum insiden menjadikan pengguna ini sebagai person of interest yang perlu diinvestigasi lebih lanjut. Investigator harus mengkorelasikan dengan artefak lain (NTUSER.DAT, event logs) untuk memahami aktivitas spesifik pengguna menjelang insiden.

---

## 4. SYSTEM Hive — Konfigurasi Sistem

### 4.1 Artefak Forensik dalam SYSTEM Hive

SYSTEM hive menyimpan konfigurasi hardware dan sistem yang kritis untuk forensik:

| Artefak | Lokasi Registry | Nilai Forensik |
|---------|----------------|----------------|
| **Computer Name** | `SYSTEM\ControlSet001\Control\ComputerName\ComputerName` | Identifikasi sistem |
| **Time Zone** | `SYSTEM\ControlSet001\Control\TimeZoneInformation` | Koreksi timestamp |
| **Network Interfaces** | `SYSTEM\ControlSet001\Services\Tcpip\Parameters\Interfaces\<GUID>` | Konfigurasi IP |
| **Mounted Devices** | `SYSTEM\MountedDevices` | Drive letters dan device associations |
| **USB History** | `SYSTEM\ControlSet001\Enum\USBSTOR` | Riwayat perangkat USB |
| **Services** | `SYSTEM\ControlSet001\Services` | Layanan yang terinstal |
| **ShimCache** | `SYSTEM\ControlSet001\Control\Session Manager\AppCompatCache` | Program execution history |
| **Last Shutdown** | `SYSTEM\ControlSet001\Control\Windows\ShutdownTime` | Waktu shutdown terakhir |

### 4.2 ControlSet dan CurrentControlSet

SYSTEM hive memiliki beberapa ControlSet yang merepresentasikan konfigurasi berbeda:

| Key | Fungsi |
|-----|--------|
| **ControlSet001** | Biasanya konfigurasi yang terakhir berhasil boot |
| **ControlSet002** | Konfigurasi alternatif (last known good) |
| **CurrentControlSet** | Alias ke ControlSet aktif (hanya ada saat live) |
| **Select** | Menentukan ControlSet mana yang aktif |

> **Tips Forensik:** Untuk menentukan ControlSet mana yang aktif, periksa `SYSTEM\Select\Current`. Value-nya menunjukkan nomor ControlSet yang digunakan saat boot terakhir.

### 4.3 USB Device History dari SYSTEM Hive

Salah satu artefak paling berharga untuk investigasi militer adalah riwayat perangkat USB:

**Lokasi:** `SYSTEM\ControlSet001\Enum\USBSTOR`

Struktur:
```
USBSTOR\
├── Disk&Ven_SanDisk&Prod_Cruzer&Rev_8.01\
│   └── 4C530000270816105372&0\
│       ├── FriendlyName = "SanDisk Cruzer USB Device"
│       ├── DeviceDesc = "Disk drive"
│       └── Properties\ (timestamps)
├── Disk&Ven_Kingston&Prod_DataTraveler&Rev_PMAP\
│   └── ...
```

| Informasi | Sumber | Contoh |
|-----------|--------|--------|
| **Vendor** | Key name | SanDisk |
| **Product** | Key name | Cruzer |
| **Revision** | Key name | 8.01 |
| **Serial Number** | Subkey name | 4C530000270816105372 |
| **First Connect** | Key timestamp / setupapi.dev.log | 2025-01-15 08:30 |
| **Last Connect** | Properties\{83da6326...}\0064 | 2025-03-20 14:22 |
| **Last Removal** | Properties\{83da6326...}\0067 | 2025-03-20 16:45 |

![Alur Analisis USB Device History](images/p06-03-usb-analysis-flow.png)

*Gambar 6.3: Alur analisis riwayat perangkat USB dari berbagai sumber registry*

#### Solved Problem 6 ⭐⭐

**Soal:** Dalam investigasi kebocoran data rahasia di Kodam, ditemukan entry USBSTOR berikut: `Disk&Ven_SanDisk&Prod_Ultra&Rev_1.00\4C530000311015109374&0`. First connect time menunjukkan 3 jam sebelum file rahasia terakhir diakses. Jelaskan informasi apa saja yang dapat diekstraksi dan langkah investigasi selanjutnya!

**Penyelesaian:**

*Step 1:* Parse informasi dari key name

| Komponen | Nilai | Interpretasi |
|----------|-------|-------------|
| **Vendor** | SanDisk | Merek perangkat USB |
| **Product** | Ultra | Model perangkat |
| **Revision** | 1.00 | Versi firmware |
| **Serial Number** | 4C530000311015109374 | Identifikasi unik perangkat |

*Step 2:* Identifikasi langkah investigasi lanjutan

1. **Korelasi MountedDevices** — Cek `SYSTEM\MountedDevices` untuk drive letter yang di-assign
2. **Cek setupapi.dev.log** — Konfirmasi first install time di `C:\Windows\INF\setupapi.dev.log`
3. **NTUSER.DAT** — Periksa `MountPoints2` di NTUSER.DAT pengguna terkait
4. **Event Logs** — Security Event ID 4663 (object access) untuk file yang dikopi ke drive USB
5. **Cari perangkat fisik** — Gunakan serial number untuk mengidentifikasi USB flash drive spesifik

*Step 3:* Analisis timeline

**Jawaban:** Dari entry USBSTOR, perangkat SanDisk Ultra dengan serial number 4C530000311015109374 terhubung 3 jam sebelum file rahasia terakhir diakses. Langkah selanjutnya: (1) Tentukan drive letter dari MountedDevices, (2) Korelasikan dengan ShellBags untuk melihat folder yang diakses di drive tersebut, (3) Periksa event logs untuk aktivitas file copy, (4) Lacak perangkat fisik menggunakan serial number, (5) Jika ditemukan, lakukan imaging forensik terhadap USB drive tersebut.

---

## 5. SOFTWARE Hive — Artefak Perangkat Lunak

### 5.1 Informasi Sistem Operasi

| Artefak | Lokasi Registry |
|---------|----------------|
| **Versi Windows** | `SOFTWARE\Microsoft\Windows NT\CurrentVersion` |
| **Product Name** | `...\CurrentVersion\ProductName` |
| **Build Number** | `...\CurrentVersion\CurrentBuild` |
| **Install Date** | `...\CurrentVersion\InstallDate` (Unix timestamp) |
| **Registered Owner** | `...\CurrentVersion\RegisteredOwner` |

### 5.2 Program Terinstal

**Lokasi:** `SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\<Application>`

Setiap subkey menyimpan informasi aplikasi:

| Value | Deskripsi |
|-------|-----------|
| **DisplayName** | Nama aplikasi |
| **DisplayVersion** | Versi aplikasi |
| **InstallDate** | Tanggal instalasi |
| **InstallLocation** | Lokasi instalasi |
| **Publisher** | Penerbit/vendor |
| **UninstallString** | Perintah uninstall |

> **Konteks Militer:** Memeriksa daftar program terinstal penting untuk mendeteksi software tidak sah (misalnya, tools hacking, remote access trojans, atau software enkripsi tidak terotorisasi) di workstation militer.

### 5.3 Network Profiles

**Lokasi:** `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles\<GUID>`

| Value | Deskripsi | Nilai Forensik |
|-------|-----------|----------------|
| **ProfileName** | Nama jaringan (SSID) | Jaringan yang pernah terhubung |
| **DateCreated** | Pertama kali terhubung | Timeline koneksi |
| **DateLastConnected** | Terakhir terhubung | Aktivitas terkini |
| **Category** | 0=Public, 1=Private, 2=Domain | Tipe jaringan |
| **NameType** | 6=Wired, 71=Wireless | Jenis koneksi |

#### Solved Problem 7 ⭐

**Soal:** Dari SOFTWARE hive, ditemukan network profile dengan nama "KEDAI_KOPI_FREE_WIFI" dan DateLastConnected 1 hari sebelum insiden. Workstation ini seharusnya hanya terhubung ke jaringan militer. Apa signifikansinya?

**Penyelesaian:**

*Step 1:* Identifikasi anomali

Workstation militer terhubung ke jaringan WiFi publik (kedai kopi) merupakan pelanggaran kebijakan keamanan dan potensial vektor serangan.

*Step 2:* Analisis risiko

| Risiko | Deskripsi |
|--------|-----------|
| **Man-in-the-Middle** | Data dapat diintersep di jaringan publik |
| **Policy Violation** | Melanggar kebijakan penggunaan jaringan militer |
| **Data Exfiltration** | Potensi pemindahan data melalui jaringan tidak aman |
| **Malware Infection** | Jaringan publik sebagai vektor distribusi malware |

**Jawaban:** Koneksi ke "KEDAI_KOPI_FREE_WIFI" pada workstation militer menunjukkan pelanggaran kebijakan keamanan serius. Workstation mungkin telah dibawa keluar dari lingkungan aman dan terhubung ke jaringan publik tidak terenkripsi, menciptakan risiko intersepsi data, infeksi malware, atau eksfiltrasi data. Investigasi harus memeriksa aktivitas apa yang dilakukan saat terhubung ke jaringan tersebut.

---

## 6. NTUSER.DAT — Artefak Aktivitas Pengguna

### 6.1 UserAssist

> **UserAssist** merekam program-program yang dijalankan pengguna melalui Windows Explorer (GUI), termasuk jumlah eksekusi dan waktu terakhir dijalankan.

**Lokasi:** `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count`

| GUID | Deskripsi |
|------|-----------|
| `{CEBFF5CD-ACE2-4F4F-9178-9926F41749EA}` | Executable file execution |
| `{F4E57C4B-2036-45F0-A9AB-443BCFE33D9F}` | Shortcut file execution |

> **Encoding:** Data UserAssist di-encode menggunakan ROT13 cipher. Misalnya, `Pnyphyngbe` = `Calculator` setelah ROT13 decode.

Setiap entry UserAssist mengandung:
- **Run Count**: Berapa kali program dijalankan
- **Focus Count**: Berapa kali window mendapat focus
- **Focus Time**: Total waktu focus (milliseconds)
- **Last Execution Time**: Timestamp terakhir dijalankan (Windows FILETIME)

#### Solved Problem 8 ⭐⭐

**Soal:** Dari analisis UserAssist, ditemukan entry ROT13 `P:\Hfref\Nqzva\Qrfxgbc\JvaFPC.rkr` dengan run count 5 dan last execution time 30 menit sebelum kebocoran data terdeteksi. Decode path tersebut dan analisis implikasinya!

**Penyelesaian:**

*Step 1:* Decode ROT13

| ROT13 Encoded | Decoded |
|---------------|---------|
| P | C |
| Hfref | Users |
| Nqzva | Admin |
| Qrfxgbc | Desktop |
| JvaFPC | WinSCP |
| rkr | exe |

**Decoded path:** `C:\Users\Admin\Desktop\WinSCP.exe`

*Step 2:* Analisis signifikansi

WinSCP adalah client SCP/SFTP/FTP untuk transfer file. Dijalankan 5 kali dari Desktop (bukan dari lokasi instalasi standar) menunjukkan ini mungkin portable version yang sengaja dibawa/didownload.

*Step 3:* Implikasi keamanan

**Jawaban:** Path yang di-decode adalah `C:\Users\Admin\Desktop\WinSCP.exe`. WinSCP adalah aplikasi transfer file (SCP/SFTP/FTP). Dijalankan 5 kali dari Desktop (bukan Program Files) menunjukkan kemungkinan portable version — indikasi kuat bahwa pengguna sengaja membawa tools untuk mentransfer file. Eksekusi terakhir 30 menit sebelum deteksi kebocoran menjadikan ini bukti kritis yang menghubungkan pengguna dengan aktivitas eksfiltrasi data.

---

### 6.2 MRU (Most Recently Used) Lists

MRU lists merekam item-item yang baru-baru ini diakses oleh pengguna:

| Artefak MRU | Lokasi di NTUSER.DAT | Informasi |
|-------------|----------------------|-----------|
| **RecentDocs** | `Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs` | File yang baru dibuka |
| **RunMRU** | `Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU` | Perintah dari Run dialog |
| **TypedPaths** | `Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths` | Path yang diketik di Explorer |
| **TypedURLs** | `Software\Microsoft\Internet Explorer\TypedURLs` | URL yang diketik |
| **OpenSaveMRU** | `Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePidlMRU` | File yang dibuka/disimpan via dialog |
| **LastVisitedMRU** | `Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU` | Aplikasi dan folder terakhir |

#### Solved Problem 9 ⭐⭐

**Soal:** Dari RecentDocs registry key, ditemukan entry berikut: `.docx\0` → `Laporan_Rahasia_Pertahanan_2025.docx`. Dari OpenSaveMRU ditemukan file yang sama dengan path `E:\Exfil\Laporan_Rahasia_Pertahanan_2025.docx`. Apa analisis Anda?

**Penyelesaian:**

*Step 1:* Korelasikan temuan

| Sumber | Temuan | Interpretasi |
|--------|--------|-------------|
| **RecentDocs** | File `.docx` dibuka | Pengguna mengakses dokumen rahasia |
| **OpenSaveMRU** | Path `E:\Exfil\` | File disimpan ke drive E: dalam folder "Exfil" |

*Step 2:* Analisis

- Drive `E:` kemungkinan besar adalah removable media (USB drive) — perlu dikorelasikan dengan MountedDevices
- Nama folder "Exfil" sangat mencurigakan, menunjukkan intent eksfiltrasi
- File bernama "Laporan_Rahasia_Pertahanan_2025" mengindikasikan dokumen dengan klasifikasi keamanan

**Jawaban:** Korelasi antara RecentDocs dan OpenSaveMRU menunjukkan bahwa pengguna tidak hanya membuka dokumen rahasia pertahanan, tetapi juga menyimpannya ke drive E: (kemungkinan removable media) dalam folder bernama "Exfil" — istilah yang umum digunakan untuk eksfiltrasi data. Ini merupakan indikasi kuat adanya upaya sengaja untuk mengekstraksi dokumen rahasia. Langkah selanjutnya: korelasikan drive E: dengan USBSTOR dan MountedDevices, periksa ShellBags untuk folder "Exfil", dan identifikasi USB device yang digunakan.

---

### 6.3 Shell Bags

> **Shell Bags** merekam pengaturan tampilan folder Windows Explorer untuk setiap folder yang pernah dibuka pengguna, termasuk folder di removable media dan network shares yang sudah tidak terhubung.

**Lokasi:**
- `NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU` — Bag container
- `NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags` — Bag settings
- `UsrClass.dat\Local Settings\Software\Microsoft\Windows\Shell\BagMRU` — Additional bags
- `UsrClass.dat\Local Settings\Software\Microsoft\Windows\Shell\Bags` — Additional settings

**Informasi yang dapat diekstraksi:**

| Data | Deskripsi |
|------|-----------|
| **Folder Path** | Setiap folder yang pernah dibuka |
| **First Accessed** | Kapan folder pertama kali diakses |
| **Last Accessed** | Kapan folder terakhir diakses |
| **View Settings** | Tampilan yang digunakan (icons, details, dll.) |
| **Network Paths** | Share paths yang pernah diakses |
| **Removable Media** | Folder di USB/external drive yang pernah dibrowse |

> **Mengapa Shell Bags Sangat Berharga:** Shell Bags merekam akses folder **bahkan setelah folder atau drive tersebut dihapus atau dilepas**. Jika seorang tersangka memasukkan USB drive, membrowse folder-foldernya, lalu mencabut USB drive, Shell Bags masih akan menyimpan catatan folder apa saja yang diakses.

![Arsitektur ShellBags dalam Forensik](images/p06-04-shellbags-architecture.png)

*Gambar 6.4: Arsitektur Shell Bags dan cara kerjanya merekam aktivitas folder*

#### Solved Problem 10 ⭐⭐

**Soal:** Dari analisis ShellBags, ditemukan entry folder berikut: `E:\ProjectX\Classified\Intel_Reports\`. Namun, saat ini tidak ada drive E: yang terhubung ke sistem. Jelaskan apa yang dapat disimpulkan dan bagaimana Shell Bags membantu investigasi!

**Penyelesaian:**

*Step 1:* Interpretasi temuan Shell Bags

Shell Bags merekam bahwa pengguna pernah membrowse folder `E:\ProjectX\Classified\Intel_Reports\` menggunakan Windows Explorer. Entry ini tetap ada meskipun drive E: sudah tidak terhubung.

*Step 2:* Informasi yang dapat diekstraksi

| Data | Nilai |
|------|-------|
| **Drive** | E: (removable media berdasarkan korelasi MountedDevices) |
| **Struktur Folder** | ProjectX → Classified → Intel_Reports |
| **First Access** | Timestamp dari ShellBag entry |
| **Last Access** | Timestamp terakhir dari ShellBag entry |

*Step 3:* Langkah investigasi lanjutan

**Jawaban:** Shell Bags membuktikan bahwa pengguna pernah secara aktif membrowse folder `E:\ProjectX\Classified\Intel_Reports\` pada removable media, meskipun drive tersebut sudah dicabut. Struktur folder "Classified/Intel_Reports" sangat mencurigakan dalam konteks militer. Investigator harus: (1) Korelasikan drive E: dengan USBSTOR untuk identifikasi perangkat USB, (2) Periksa timestamps untuk menentukan kapan akses terjadi, (3) Cari perangkat fisik berdasarkan serial number, (4) Jika ditemukan, lakukan imaging dan analisis konten perangkat.

---

### 6.4 MountPoints2

> **MountPoints2** merekam semua volume yang pernah di-mount oleh pengguna, termasuk USB drives, network shares, dan mapped drives.

**Lokasi:** `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2`

Setiap subkey merepresentasikan volume GUID:
```
MountPoints2\
├── {a1b2c3d4-e5f6-7890-abcd-ef1234567890}  ← USB drive
├── {11223344-5566-7788-99aa-bbccddeeff00}  ← Another volume
└── ##server##share                            ← Network share
```

#### Solved Problem 11 ⭐⭐

**Soal:** Jelaskan bagaimana menghubungkan informasi dari USBSTOR, MountedDevices, dan MountPoints2 untuk membuat profil lengkap perangkat USB!

**Penyelesaian:**

*Step 1:* Identifikasi sumber data dan kontribusinya

| Sumber | Lokasi | Informasi |
|--------|--------|-----------|
| **USBSTOR** | SYSTEM hive | Vendor, Product, Serial Number, Timestamps |
| **MountedDevices** | SYSTEM hive | Drive letter assignment |
| **MountPoints2** | NTUSER.DAT | User yang mengakses volume |
| **setupapi.dev.log** | File system | First install timestamp |
| **Event Logs** | Event Log files | Connection/disconnection events |

*Step 2:* Proses korelasi

1. Dari **USBSTOR**: Identifikasi perangkat (SanDisk Ultra, SN: 4C530000...)
2. Dari **MountedDevices**: Volume GUID dan drive letter (E:)
3. Dari **MountPoints2** di NTUSER.DAT tiap user: User mana yang mengakses drive E:
4. Dari **ShellBags**: Folder apa yang di-browse di drive E:

**Jawaban:** Profil USB lengkap dibangun dengan mengkorelasikan: USBSTOR (identitas perangkat), MountedDevices (drive letter), MountPoints2 (pengguna yang mengakses), setupapi.dev.log (first connect), dan event logs (connect/disconnect events). Korelasi ini memungkinkan investigator menjawab: perangkat apa, milik siapa, siapa yang mengaksesnya, kapan dihubungkan, dan apa yang dilakukan dengan perangkat tersebut.

---

## 7. Application Execution Artifacts

### 7.1 ShimCache (Application Compatibility Cache)

> **ShimCache** (juga dikenal sebagai AppCompatCache) merekam metadata tentang executable yang dijalankan atau diakses oleh sistem untuk keperluan application compatibility.

**Lokasi:** `SYSTEM\ControlSet001\Control\Session Manager\AppCompatCache`

| Data | Deskripsi |
|------|-----------|
| **File Path** | Full path executable |
| **Last Modified Time** | Timestamp modifikasi file |
| **Execution Flag** | Apakah file dieksekusi (Windows 7) atau hanya diakses (Windows 8+) |
| **File Size** | Ukuran file (pada beberapa versi) |

> **Catatan Penting:** Pada Windows 8 dan yang lebih baru, adanya entry di ShimCache **tidak** selalu berarti file dieksekusi. File yang diakses (misalnya, di-browse di Explorer) juga dapat muncul di ShimCache. Gunakan artefak lain (Prefetch, AmCache) untuk konfirmasi eksekusi.

### 7.2 AmCache

> **AmCache** merekam informasi tentang aplikasi dan driver yang dieksekusi, termasuk hash SHA-1 file.

**Lokasi:** `C:\Windows\AppCompat\Programs\Amcache.hve` (hive terpisah)

| Data | Deskripsi | Nilai Forensik |
|------|-----------|----------------|
| **File Path** | Lokasi lengkap executable | Identifikasi program |
| **SHA-1 Hash** | Hash file saat pertama dieksekusi | Identifikasi malware, verifikasi |
| **File Size** | Ukuran file | Korelasi |
| **Link Date** | Compile time dari PE header | Deteksi timestomping |
| **Publisher** | Digital signature publisher | Verifikasi legitimasi |
| **First Execution** | Timestamp key creation | Timeline eksekusi |

> **Keunggulan AmCache:** SHA-1 hash yang terekam memungkinkan investigator mengidentifikasi malware bahkan setelah file dihapus — hash dapat di-query ke database malware seperti VirusTotal.

### 7.3 BAM/DAM (Background Activity Moderator)

> **BAM (Background Activity Moderator)** dan **DAM (Desktop Activity Moderator)** merekam path executable dan timestamp eksekusi terakhir untuk setiap pengguna.

**Lokasi:** `SYSTEM\ControlSet001\Services\bam\State\UserSettings\<SID>`

| Data | Deskripsi |
|------|-----------|
| **Value Name** | Full path executable |
| **Value Data** | Windows FILETIME timestamp (last execution) |

> **Keunggulan BAM/DAM:** Artefak ini langsung mengasosiasikan eksekusi program dengan SID pengguna spesifik dan menyediakan timestamp eksekusi yang akurat.

#### Solved Problem 12 ⭐⭐

**Soal:** Bandingkan empat artefak eksekusi program (UserAssist, ShimCache, AmCache, BAM) dan jelaskan kapan masing-masing paling berguna dalam investigasi!

**Penyelesaian:**

*Step 1:* Buat tabel perbandingan

| Artefak | Hive | Per-User? | Hash? | Run Count? | Timestamp? | Catatan |
|---------|------|-----------|-------|------------|------------|---------|
| **UserAssist** | NTUSER.DAT | Ya | Tidak | Ya | Ya | Hanya GUI execution |
| **ShimCache** | SYSTEM | Tidak | Tidak | Tidak | Ya (modified) | Bisa false positive di Win8+ |
| **AmCache** | Amcache.hve | Tidak | SHA-1 | Tidak | Ya (first run) | Hash untuk identifikasi malware |
| **BAM/DAM** | SYSTEM | Ya (via SID) | Tidak | Tidak | Ya (last run) | Windows 10+ only |

*Step 2:* Tentukan kapan masing-masing paling berguna

| Kebutuhan Investigasi | Artefak Terbaik | Alasan |
|----------------------|-----------------|--------|
| Siapa yang menjalankan program? | UserAssist, BAM | Per-user association |
| Berapa kali program dijalankan? | UserAssist | Satu-satunya yang punya run count |
| Apakah file tersebut malware? | AmCache | Menyediakan SHA-1 hash |
| Kapan program pertama dijalankan? | AmCache | First execution timestamp |
| Program apa saja yang pernah ada di sistem? | ShimCache | Mencakup file yang diakses tapi mungkin sudah dihapus |

**Jawaban:** Empat artefak saling melengkapi: UserAssist untuk GUI execution per-user dengan run count; ShimCache untuk broad coverage termasuk file yang mungkin sudah dihapus; AmCache untuk identifikasi malware via SHA-1 hash; BAM/DAM untuk per-user execution dengan timestamp akurat di Windows 10+. Investigasi yang menyeluruh harus mengkorelasikan keempat artefak untuk mendapatkan gambaran lengkap.

---

### 7.4 Korelasi Application Execution Artifacts

Untuk memaksimalkan hasil investigasi, artefak eksekusi perlu dikorelasikan:

![Korelasi Artefak Eksekusi Program](images/p06-05-execution-artifacts-correlation.png)

*Gambar 6.5: Korelasi antar artefak eksekusi program untuk verifikasi temuan*

#### Solved Problem 13 ⭐⭐⭐

**Soal:** Dalam investigasi insiden di Markas Besar TNI, ditemukan artefak berikut:
- **ShimCache:** `C:\Temp\mimikatz.exe` (Modified: 2025-03-15 02:30)
- **AmCache:** `C:\Temp\mimikatz.exe` (SHA-1: a1b2c3d4..., First Run: 2025-03-15 02:35)
- **BAM:** SID S-1-5-21-...-1001 → `C:\Temp\mimikatz.exe` (Last: 2025-03-15 02:45)
- **UserAssist:** Tidak ada entry untuk mimikatz.exe

Lakukan analisis korelasi dan tentukan kesimpulan investigasi!

**Penyelesaian:**

*Step 1:* Analisis setiap artefak

| Artefak | Temuan | Interpretasi |
|---------|--------|-------------|
| **ShimCache** | File ada, modified 02:30 | File diakses/ditempatkan di sistem |
| **AmCache** | SHA-1 tersedia, first run 02:35 | File dieksekusi, hash bisa di-query ke malware DB |
| **BAM** | SID 1001, last run 02:45 | User dengan RID 1001 menjalankan file, mungkin multiple execution |
| **UserAssist** | Tidak ada entry | File TIDAK dijalankan melalui GUI (Explorer) |

*Step 2:* Korelasi timeline

```
02:30 - File mimikatz.exe ditempatkan/dimodifikasi di C:\Temp\
02:35 - File pertama kali dieksekusi (AmCache)
02:45 - File terakhir dieksekusi oleh user SID-1001 (BAM)
```

*Step 3:* Analisis mendalam

- **Mimikatz** adalah tool credential dumping yang sangat umum digunakan dalam serangan
- File dijalankan dari `C:\Temp\` bukan lokasi instalasi normal → deliberate placement
- Tidak ada di UserAssist → dijalankan via command line (CMD/PowerShell), bukan GUI
- Waktu eksekusi dini hari (02:30-02:45) → anomali jam kerja

*Step 4:* Kesimpulan

**Jawaban:** Korelasi empat artefak mengkonfirmasi bahwa mimikatz.exe (tool pencurian kredensial) ditempatkan di `C:\Temp\` dan dieksekusi via command line (bukan GUI, karena tidak ada di UserAssist) oleh user RID 1001 pada dini hari. SHA-1 dari AmCache harus di-query ke VirusTotal untuk konfirmasi. Timeline 02:30-02:45 menunjukkan aktivitas di luar jam kerja normal. Ini merupakan indikasi kuat adanya serangan credential dumping — kemungkinan bagian dari lateral movement atau privilege escalation dalam jaringan pertahanan.

---

## 8. Wireless Network Profiles

### 8.1 Profil Jaringan Wireless

Windows menyimpan profil jaringan wireless di beberapa lokasi:

| Lokasi | Informasi |
|--------|-----------|
| `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles` | Nama jaringan, timestamps |
| `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged` | MAC address AP, SSID |
| `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Managed` | Domain-managed networks |
| `SYSTEM\ControlSet001\Services\Tcpip\Parameters\Interfaces\<GUID>` | IP configuration per interface |

### 8.2 Network Signatures

Dari `NetworkList\Signatures\Unmanaged\<GUID>`:

| Value | Informasi |
|-------|-----------|
| **FirstNetwork** | SSID jaringan |
| **DefaultGatewayMac** | MAC address gateway/access point |
| **ProfileGuid** | GUID yang menghubungkan ke Profiles |

> **Nilai Forensik Militer:** MAC address access point dapat digunakan untuk menentukan lokasi fisik koneksi. Jika access point terdaftar di database seperti WiGLE.net, lokasi geografis dapat diperkirakan — berguna untuk memverifikasi apakah personel militer mengakses jaringan dari lokasi yang seharusnya.

#### Solved Problem 14 ⭐⭐

**Soal:** Jelaskan bagaimana forensik wireless network profiles dapat digunakan untuk membuktikan bahwa seorang anggota militer membawa laptop dinas ke lokasi yang tidak diotorisasi!

**Penyelesaian:**

*Step 1:* Identifikasi artefak yang relevan

| Artefak | Informasi | Penggunaan |
|---------|-----------|-----------|
| **Network Profiles** | SSID + timestamps | Nama jaringan dan kapan terhubung |
| **Network Signatures** | MAC address AP | Identifikasi access point fisik |
| **IP Configuration** | IP addresses assigned | Subnet jaringan yang digunakan |

*Step 2:* Proses investigasi

1. Ekstrak semua network profiles dari registry
2. Filter jaringan yang bukan milik organisasi militer
3. Gunakan MAC address AP untuk query WiGLE.net atau database internal
4. Korelasikan timestamps dengan jadwal tugas personel

*Step 3:* Membangun bukti

**Jawaban:** Profil jaringan di registry merekam setiap SSID dan MAC address access point yang pernah terhubung beserta timestamp. Investigator dapat: (1) Mengidentifikasi SSID non-militer (kafe, hotel, residence), (2) Menggunakan MAC address AP untuk geolokasi via WiGLE.net, (3) Membangun timeline keberadaan laptop berdasarkan kapan terhubung ke setiap jaringan. Ini membuktikan laptop dinas dibawa ke lokasi tidak terotorisasi, melanggar kebijakan keamanan operasional.

---

## 9. Timeline Reconstruction dari Registry

### 9.1 Sumber Timestamp Registry

| Artefak | Tipe Timestamp | Lokasi |
|---------|---------------|--------|
| **Key Last Write Time** | Setiap registry key | Semua hive |
| **UserAssist** | Last execution time | NTUSER.DAT |
| **USBSTOR** | Connect/disconnect | SYSTEM |
| **Network Profiles** | DateCreated, DateLastConnected | SOFTWARE |
| **BAM/DAM** | Last execution | SYSTEM |
| **AmCache** | First execution | Amcache.hve |
| **SAM** | Last login, creation | SAM |
| **Shutdown Time** | System shutdown | SYSTEM |

### 9.2 Metodologi Timeline Reconstruction

Langkah-langkah membangun timeline dari data registry:

1. **Ekstraksi**: Extract semua hive files dari forensic image
2. **Parsing**: Gunakan tools (Registry Explorer, RegRipper) untuk parse artefak
3. **Normalisasi**: Konversi semua timestamp ke timezone yang konsisten
4. **Korelasi**: Gabungkan data dari berbagai hive ke satu timeline
5. **Analisis**: Identifikasi pola, anomali, dan sequence of events
6. **Dokumentasi**: Buat visual timeline untuk presentasi

> **Perhatian Timezone:** Periksa timezone konfigurasi sistem dari `SYSTEM\ControlSet001\Control\TimeZoneInformation` sebelum menginterpretasikan timestamp. Semua registry timestamps disimpan dalam UTC — tool seperti Registry Explorer akan menampilkan dalam UTC kecuali dikonfigurasi sebaliknya.

#### Solved Problem 15 ⭐⭐⭐

**Soal:** Dari analisis registry lengkap workstation seorang personel di pangkalan militer, ditemukan timeline berikut:

| Waktu (UTC+7) | Artefak | Kejadian |
|---------------|---------|----------|
| 22:00 | SAM | Last login user "operator1" |
| 22:05 | UserAssist | Explorer.exe launched |
| 22:10 | USBSTOR | USB SanDisk Ultra connected |
| 22:12 | ShellBags | Folder `E:\DataBackup\` browsed |
| 22:15 | OpenSaveMRU | `secret_ops.xlsx` opened |
| 22:20 | BAM | `xcopy.exe` executed by operator1 |
| 22:25 | ShellBags | `E:\DataBackup\Copied\` browsed |
| 22:30 | USBSTOR | USB disconnected |
| 22:32 | Network Profiles | Connected to "AndroidAP" hotspot |
| 22:35 | BAM | `curl.exe` executed by operator1 |
| 22:40 | Network Profiles | Disconnected from "AndroidAP" |
| 22:42 | SAM | Account locked |

Rekonstruksi skenario yang terjadi dan berikan rekomendasi investigasi!

**Penyelesaian:**

*Step 1:* Rekonstruksi skenario berdasarkan timeline

**22:00-22:05:** Personel "operator1" login ke workstation di luar jam kerja normal (malam hari) dan membuka Windows Explorer.

**22:10-22:25:** USB drive SanDisk Ultra dihubungkan. Pengguna membrowse folder `E:\DataBackup\`, membuka file `secret_ops.xlsx`, lalu menjalankan `xcopy.exe` (command line copy utility) dan membrowse folder `E:\DataBackup\Copied\` — mengindikasikan penyalinan file dari workstation ke USB.

**22:30-22:40:** USB dicabut, kemudian workstation terhubung ke hotspot "AndroidAP" (tethering handphone) dan `curl.exe` dijalankan — mengindikasikan transfer data via jaringan internet, bypass dari jaringan militer.

**22:42:** Akun terkunci — kemungkinan oleh sistem deteksi atau administrator.

*Step 2:* Analisis risiko

| Fase | Aktivitas | Risiko |
|------|-----------|--------|
| **Fase 1** | Login malam hari | Anomali waktu akses |
| **Fase 2** | Copy data ke USB | Eksfiltrasi via removable media |
| **Fase 3** | Upload via hotspot | Eksfiltrasi via internet, bypass monitoring |
| **Fase 4** | Account locked | Deteksi atau policy enforcement |

*Step 3:* Rekomendasi

**Jawaban:** Timeline menunjukkan skenario eksfiltrasi data terencana dua tahap: (1) Copy file rahasia ke USB drive, (2) Upload melalui hotspot personal (bypass monitoring jaringan militer). Rekomendasi: (1) Preserve semua artefak dan buat forensic image lengkap, (2) Identifikasi dan sita USB SanDisk Ultra berdasarkan serial number, (3) Periksa handphone yang digunakan sebagai hotspot, (4) Analisis konten `secret_ops.xlsx` untuk menentukan klasifikasi data, (5) Review log curl.exe untuk mengetahui tujuan upload, (6) Koordinasi dengan intelijen untuk menentukan apakah ini insider threat atau espionage.

---

## 10. Tools Forensik Registry

### 10.1 Registry Explorer (Eric Zimmerman)

> **Registry Explorer** adalah tool GUI gratis dari Eric Zimmerman untuk browsing dan analisis registry hive dengan kemampuan parsing otomatis untuk artefak forensik.

Fitur utama:
- Browsing hive files secara offline
- Replay transaction logs (.LOG1, .LOG2)
- Bookmarks untuk artefak forensik penting
- Decode otomatis (ROT13, FILETIME, binary structures)
- Export ke berbagai format

### 10.2 ShellBags Explorer (Eric Zimmerman)

Tool khusus untuk parsing dan visualisasi Shell Bags dari NTUSER.DAT dan UsrClass.dat.

### 10.3 AmcacheParser (Eric Zimmerman)

Command-line tool untuk parsing Amcache.hve dan menghasilkan output CSV/Timeline.

### 10.4 RegRipper

> **RegRipper** adalah tool open-source yang menggunakan plugins untuk mengekstrak dan menginterpretasikan artefak spesifik dari registry hive files.

| Tool | Fungsi | Output |
|------|--------|--------|
| **Registry Explorer** | GUI browsing + analysis | Interactive |
| **ShellBags Explorer** | Shell Bags analysis | GUI + CSV |
| **AmcacheParser** | AmCache parsing | CSV/Timeline |
| **RegRipper** | Plugin-based extraction | Text reports |
| **USBDeview** | USB device history | GUI + CSV |

#### Solved Problem 16 ⭐

**Soal:** Sebutkan tiga keunggulan Registry Explorer dibandingkan Windows Registry Editor (regedit) bawaan untuk analisis forensik!

**Penyelesaian:**

*Step 1:* Bandingkan kedua tool

| Fitur | Registry Editor (regedit) | Registry Explorer |
|-------|--------------------------|-------------------|
| **Offline Analysis** | Hanya live registry | Bisa load hive offline |
| **Transaction Logs** | Tidak support | Replay LOG1/LOG2 |
| **Auto Decode** | Tidak ada | ROT13, FILETIME, binary |
| **Forensic Bookmarks** | Tidak ada | Bookmark artefak penting |
| **Timeline** | Tidak ada | Last Write Time visibility |
| **Integritas Bukti** | Mengubah registry live | Read-only analysis |

**Jawaban:** Tiga keunggulan: (1) Registry Explorer dapat membuka hive files secara offline tanpa mengubah bukti, sedangkan regedit hanya bekerja pada live registry; (2) Registry Explorer melakukan auto-decode data biner seperti ROT13 (UserAssist) dan FILETIME timestamps; (3) Registry Explorer dapat memutar ulang (replay) transaction logs untuk mendapatkan data terlengkap yang mungkin belum di-commit ke hive utama.

---

#### Solved Problem 17 ⭐

**Soal:** Apa fungsi dari transaction logs (`.LOG1`, `.LOG2`) dalam konteks forensik registry?

**Penyelesaian:**

*Step 1:* Pahami mekanisme transaction log

Windows menggunakan transaction log sebagai mekanisme journaling — perubahan registry ditulis ke log terlebih dahulu sebelum di-commit ke hive utama. Ini mencegah korupsi jika sistem crash.

*Step 2:* Signifikansi forensik

| Skenario | Dampak Forensik |
|----------|----------------|
| **Shutdown normal** | Log biasanya sudah di-flush ke hive |
| **Crash/force shutdown** | Log mungkin mengandung data yang belum di-commit |
| **Anti-forensik** | Perubahan terakhir mungkin hanya ada di log |

**Jawaban:** Transaction logs (`.LOG1`, `.LOG2`) berfungsi sebagai jurnal perubahan registry. Dalam forensik, logs ini penting karena dapat mengandung perubahan yang belum di-commit ke hive utama (terutama jika sistem mengalami crash atau shutdown tidak normal). Registry Explorer dapat melakukan replay transaction logs untuk merekonstruksi state registry yang paling lengkap dan terkini.

---

#### Solved Problem 18 ⭐⭐

**Soal:** Jelaskan langkah-langkah untuk melakukan forensik registry secara offline dari sebuah forensic image menggunakan tools Eric Zimmerman!

**Penyelesaian:**

*Step 1:* Persiapan

1. Mount forensic image menggunakan Arsenal Image Mounter atau FTK Imager
2. Navigasi ke lokasi hive files:
   - `C:\Windows\System32\config\` untuk SAM, SECURITY, SOFTWARE, SYSTEM
   - `C:\Users\<username>\NTUSER.DAT` untuk setiap user
   - `C:\Users\<username>\AppData\Local\Microsoft\Windows\UsrClass.dat`
   - `C:\Windows\AppCompat\Programs\Amcache.hve`
3. Export semua hive files beserta transaction logs ke folder kerja

*Step 2:* Analisis

| Langkah | Tool | Target |
|---------|------|--------|
| **1. System Info** | Registry Explorer → SYSTEM | ComputerName, timezone, last shutdown |
| **2. User Accounts** | Registry Explorer → SAM | Akun pengguna, last login, login count |
| **3. USB Devices** | Registry Explorer → SYSTEM | USBSTOR entries |
| **4. User Activity** | Registry Explorer → NTUSER.DAT | UserAssist, MRU, TypedURLs |
| **5. Shell Bags** | ShellBags Explorer → NTUSER.DAT + UsrClass.dat | Folder access history |
| **6. App Execution** | AmcacheParser → Amcache.hve | Program execution + SHA-1 hashes |
| **7. Timeline** | Gabungkan semua output | Chronological reconstruction |

**Jawaban:** Langkah forensik registry offline: (1) Mount forensic image dan extract semua hive files + transaction logs, (2) Buka SYSTEM hive dengan Registry Explorer untuk info sistem dan USB history, (3) Buka SAM untuk analisis akun, (4) Buka setiap NTUSER.DAT untuk UserAssist, MRU, dan artefak pengguna, (5) Gunakan ShellBags Explorer untuk analisis folder access, (6) Jalankan AmcacheParser untuk execution history dan hash, (7) Gabungkan semua temuan ke dalam timeline terpadu.

---

#### Solved Problem 19 ⭐⭐

**Soal:** Seorang investigator menemukan bahwa timestamp Last Write Time pada key registry `RunMRU` menunjukkan tanggal 5 menit sebelum insiden, dan entry terbaru adalah `cmd /c net user hacker P@ssw0rd /add`. Apa analisis dan tindakan yang perlu dilakukan?

**Penyelesaian:**

*Step 1:* Interpretasi RunMRU entry

| Komponen | Analisis |
|----------|---------|
| **RunMRU** | Merekam perintah yang dijalankan via Win+R (Run dialog) |
| **Perintah** | `cmd /c net user hacker P@ssw0rd /add` |
| **Aksi** | Membuat akun pengguna baru bernama "hacker" dengan password "P@ssw0rd" |
| **Timestamp** | 5 menit sebelum insiden |

*Step 2:* Analisis signifikansi

Perintah ini menunjukkan pembuatan akun backdoor — teknik umum dalam persistence attacker.

*Step 3:* Tindakan investigasi

1. **Verifikasi** akun "hacker" di SAM hive
2. **Cek** apakah akun tersebut memiliki hak administrator (member of Administrators group)
3. **Periksa** login history akun "hacker"
4. **Korelasikan** dengan event logs (Event ID 4720 = account creation)
5. **Cari** artefak eksekusi lain pada waktu yang sama
6. **Identifikasi** siapa yang login saat perintah dijalankan

**Jawaban:** RunMRU menunjukkan perintah pembuatan akun backdoor "hacker" dijalankan 5 menit sebelum insiden — ini merupakan bukti kuat aktivitas malicious. Tindakan: (1) Verifikasi akun di SAM, (2) Periksa privilege level, (3) Korelasikan dengan Event ID 4720 di Security logs, (4) Identifikasi user yang login saat perintah dijalankan (dari SAM last login dan event logs), (5) Periksa apakah akun "hacker" kemudian digunakan untuk aktivitas lanjutan.

---

#### Solved Problem 20 ⭐⭐⭐

**Soal:** Dalam investigasi di Lantamal (Pangkalan Utama TNI AL), sebuah workstation yang menangani komunikasi enkripsi menunjukkan anomali. Dari analisis registry lengkap, ditemukan:

1. **SAM:** Akun baru "sysadmin2" dibuat 3 hari sebelum insiden (RID 1003)
2. **SOFTWARE (NetworkList):** Koneksi ke SSID "SHIP_GUEST_WIFI" dan "HOTEL_MARINA"
3. **NTUSER.DAT sysadmin2:** UserAssist menunjukkan `putty.exe` (23 runs), `winscp.exe` (15 runs)
4. **ShellBags:** Folder `\\192.168.100.50\share\upload\` dan `D:\Crypto_Keys\`
5. **AmCache:** `keylogger.exe` (SHA-1: abc123...) first run 2 hari sebelum insiden
6. **SYSTEM (USBSTOR):** Kingston DataTraveler, SN: 50E549C20E8FF441, first connect 3 hari sebelum insiden

Lakukan analisis komprehensif dan buat rekomendasi!

**Penyelesaian:**

*Step 1:* Bangun timeline

| Hari | Artefak | Kejadian |
|------|---------|----------|
| **H-3** | SAM + USBSTOR | Akun "sysadmin2" dibuat + USB Kingston pertama kali terhubung |
| **H-2** | AmCache | keylogger.exe pertama kali dieksekusi |
| **H-1 s/d H-0** | UserAssist, ShellBags, NetworkList | PuTTY/WinSCP digunakan berulang, folder crypto keys diakses, network share diakses |

*Step 2:* Analisis artefak

| Temuan | Analisis |
|--------|---------|
| **Akun sysadmin2** | Akun backdoor — kemungkinan dibuat oleh attacker atau insider |
| **Network SHIP_GUEST_WIFI** | Workstation terhubung ke WiFi kapal tamu — di luar jaringan aman |
| **HOTEL_MARINA** | Workstation dibawa ke hotel — pelanggaran kebijakan serius |
| **PuTTY (23 runs)** | SSH client — kemungkinan remote access ke sistem lain |
| **WinSCP (15 runs)** | File transfer — eksfiltrasi data |
| **Network share** | `\\192.168.100.50\share\upload\` — file di-upload ke server internal/external |
| **D:\Crypto_Keys\** | Akses ke folder kunci kriptografi — data sangat sensitif |
| **keylogger.exe** | Keystroke logging — menangkap kredensial dan data sensitif |
| **USB Kingston** | Removable media — vektor untuk import tools dan export data |

*Step 3:* Rekonstruksi skenario

Skenario ini konsisten dengan **insider threat dengan bantuan tools canggih:**
1. Pelaku membuat akun backdoor dan memasukkan tools via USB
2. Keylogger dipasang untuk mencuri kredensial
3. Data kriptografi diakses dan ditransfer via SCP/SSH
4. Workstation dibawa ke lokasi tidak aman (hotel, kapal tamu)

*Step 4:* Rekomendasi

**Jawaban:**

**Analisis:** Ini merupakan kasus insider threat terkoordinasi di fasilitas komunikasi enkripsi TNI AL. Pelaku membuat akun backdoor, memasang keylogger via USB, mengakses kunci kriptografi, dan menggunakan PuTTY/WinSCP untuk mentransfer data ke server external. Workstation juga dibawa ke lokasi tidak aman (hotel, guest WiFi kapal).

**Rekomendasi:**
1. **Isolasi segera** workstation dari jaringan dan preserve bukti
2. **Sita USB Kingston** (SN: 50E549C20E8FF441) dan lakukan forensik
3. **Query SHA-1** keylogger.exe ke VirusTotal dan malware databases
4. **Identifikasi server** 192.168.100.50 dan periksa data yang di-upload
5. **Audit semua kunci kriptografi** yang mungkin terkompromi — rotasi segera
6. **Identifikasi kreator** akun sysadmin2 dari event logs
7. **Periksa sistem lain** yang mungkin diakses via PuTTY
8. **Koordinasi dengan Dinas Intel** untuk assessment dampak keamanan nasional
9. **Review kebijakan** penggunaan removable media dan akses fisik workstation

---

#### Solved Problem 21 ⭐⭐⭐

**Soal:** Bagaimana teknik anti-forensik dapat mempengaruhi analisis registry, dan apa langkah mitigasinya?

**Penyelesaian:**

*Step 1:* Identifikasi teknik anti-forensik terhadap registry

| Teknik Anti-Forensik | Deskripsi | Dampak |
|----------------------|-----------|--------|
| **Registry wiping** | Menghapus keys/values spesifik | Menghilangkan artefak |
| **Timestamp manipulation** | Mengubah Last Write Time | Merusak timeline |
| **UserAssist clearing** | Menghapus entry UserAssist | Menghilangkan execution history |
| **ShellBags clearing** | Menghapus Shell Bags | Menghilangkan folder access history |
| **Privacy cleaners** | CCleaner, BleachBit | Menghapus multiple artefak sekaligus |
| **Registry key renaming** | Mengganti nama key | Menyamarkan artefak |

*Step 2:* Langkah mitigasi dan deteksi

| Mitigasi | Implementasi |
|----------|-------------|
| **Transaction logs** | Replay LOG1/LOG2 — mungkin berisi data sebelum wipe |
| **Volume Shadow Copies** | Snapshot sebelumnya mungkin berisi registry asli |
| **Event Logs** | Event ID 1102 (log clear), application events dari cleaner tools |
| **Registry backup** | RegBack folder (`C:\Windows\System32\config\RegBack\`) |
| **Cross-reference** | Gunakan artefak non-registry (Prefetch, event logs) untuk verifikasi |
| **Anti-forensik detection** | Kehadiran privacy cleaner tools sendiri merupakan artefak |

**Jawaban:** Teknik anti-forensik registry meliputi wiping, timestamp manipulation, dan penggunaan privacy cleaners. Mitigasi: (1) Transaction logs mungkin menyimpan data sebelum wipe, (2) Volume Shadow Copies menyediakan snapshot historis, (3) RegBack folder berisi backup periodik, (4) Event logs mencatat aktivitas pembersihan, (5) Cross-reference dengan artefak non-registry (Prefetch, event logs, $MFT), (6) Deteksi keberadaan cleaner tools — penggunaan CCleaner atau BleachBit sendiri merupakan artefak yang harus dilaporkan.

---

#### Solved Problem 22 ⭐⭐⭐

**Soal:** Rancang prosedur standar operasional (SOP) analisis forensik registry untuk Tim Forensik Digital Kodam yang mencakup tahap persiapan hingga pelaporan!

**Penyelesaian:**

*Step 1:* Definisikan tahapan SOP

**SOP Analisis Forensik Registry — Tim Forensik Digital Kodam**

| Tahap | Aktivitas | Durasi Est. |
|-------|-----------|-------------|
| **1. Persiapan** | Siapkan workstation forensik, tools, dan template | 30 menit |
| **2. Akuisisi** | Extract hive files + transaction logs dari forensic image | 45 menit |
| **3. Verifikasi** | Hash verification semua hive files | 15 menit |
| **4. Analisis Sistem** | SYSTEM hive: ComputerName, timezone, shutdown, USB, ShimCache | 60 menit |
| **5. Analisis Akun** | SAM hive: User accounts, login history, anomalies | 30 menit |
| **6. Analisis Software** | SOFTWARE hive: Installed programs, network profiles | 45 menit |
| **7. Analisis Pengguna** | NTUSER.DAT per user: UserAssist, MRU, ShellBags, MountPoints2 | 90 menit/user |
| **8. Analisis Eksekusi** | AmCache + BAM: Program execution, hashes | 45 menit |
| **9. Timeline** | Gabungkan semua artefak ke timeline terpadu | 60 menit |
| **10. Pelaporan** | Dokumentasi temuan, kesimpulan, rekomendasi | 120 menit |

*Step 2:* Detail setiap tahap

**Tahap 4 - Analisis Sistem (Detail):**
1. Catat Computer Name dan timezone
2. Catat last shutdown time
3. Identifikasi ControlSet aktif
4. Extract daftar USB devices dari USBSTOR
5. Parse ShimCache untuk execution history
6. Catat daftar services yang terinstal

**Tahap 7 - Analisis Pengguna (Detail):**
1. Parse UserAssist (decode ROT13) — daftar program GUI
2. Extract RunMRU — perintah Run dialog
3. Extract TypedURLs/TypedPaths
4. Parse RecentDocs dan OpenSaveMRU
5. Analisis ShellBags menggunakan ShellBags Explorer
6. Extract MountPoints2 — volumes yang diakses

**Jawaban:** SOP mencakup 10 tahap dari persiapan hingga pelaporan, dengan estimasi total 8-10 jam untuk satu workstation. Tahap kritis adalah analisis pengguna (NTUSER.DAT) yang harus dilakukan per user account, dan timeline reconstruction yang mengkorelasikan semua temuan. SOP harus diaudit dan diperbarui setiap 6 bulan untuk mengakomodasi artefak baru dan perubahan sistem operasi.

---

## Supplementary Problems

### Problem S1 ⭐
**Soal:** Apa perbedaan antara HKCU dan HKU dalam Windows Registry?

**Jawaban:** HKCU (HKEY_CURRENT_USER) adalah alias yang menunjuk ke profil pengguna yang sedang login di HKU (HKEY_USERS). HKU menyimpan profil semua pengguna terdaftar yang masing-masing diidentifikasi oleh SID mereka, sedangkan HKCU hanya menunjuk ke SID pengguna aktif.

---

### Problem S2 ⭐⭐
**Soal:** Mengapa ShellBags dianggap salah satu artefak paling berharga dalam forensik USB?

**Jawaban:** ShellBags sangat berharga karena: (1) Merekam semua folder yang pernah di-browse pengguna termasuk folder di USB/removable media, (2) Data tetap tersimpan meskipun USB sudah dicabut atau folder sudah dihapus, (3) Menyimpan timestamps akses, dan (4) Merekam struktur folder lengkap yang pernah diakses. Ini memungkinkan investigator mengetahui folder apa saja yang dibrowse di USB drive bahkan tanpa memiliki USB drive fisiknya.

---

### Problem S3 ⭐⭐
**Soal:** Bagaimana cara menentukan timezone yang dikonfigurasi pada sistem target dari registry?

**Jawaban:** Timezone dapat ditemukan di `SYSTEM\ControlSet001\Control\TimeZoneInformation`. Values penting: `TimeZoneKeyName` (nama timezone, misal "SE Asia Standard Time"), `ActiveTimeBias` (offset dalam menit dari UTC), dan `Bias` (standard time bias). Ini kritis karena semua registry timestamps disimpan dalam UTC dan perlu dikonversi ke timezone lokal untuk interpretasi yang benar.

---

### Problem S4 ⭐⭐⭐
**Soal:** Jelaskan bagaimana Volume Shadow Copies dapat membantu ketika artefak registry telah dihapus melalui anti-forensik!

**Jawaban:** Volume Shadow Copies (VSS) adalah snapshot otomatis yang dibuat Windows secara berkala. Ketika artefak registry dihapus via anti-forensik, VSS mungkin menyimpan salinan registry hive dari sebelum penghapusan. Investigator dapat: (1) Menggunakan `vssadmin list shadows` atau Arsenal Image Mounter untuk mengakses shadow copies, (2) Extract registry hives dari snapshot lama, (3) Membandingkan hive sebelum dan sesudah anti-forensik untuk mengidentifikasi apa yang dihapus, (4) Merekonstruksi artefak dari multiple shadow copies untuk membangun timeline lebih lengkap.

---

### Problem S5 ⭐⭐⭐
**Soal:** Rancang checklist analisis registry untuk kasus investigasi insider threat di lingkungan militer!

**Jawaban:**

**Checklist Forensik Registry — Insider Threat:**
1. **Akun Anomali:** Periksa SAM untuk akun baru, akun tidak aktif yang tiba-tiba aktif, login di luar jam kerja
2. **USB History:** Analisis USBSTOR, MountedDevices, MountPoints2 untuk perangkat tidak terotorisasi
3. **Program Mencurigakan:** UserAssist, AmCache untuk tools transfer (WinSCP, PuTTY), tools hacking (mimikatz), atau privacy cleaners
4. **Network Anomali:** Network profiles untuk koneksi ke jaringan non-militer
5. **File Access:** RecentDocs, OpenSaveMRU, ShellBags untuk akses ke file/folder sensitif atau classified
6. **Timeline:** Korelasi semua artefak untuk merekonstruksi sequence kejadian
7. **Anti-Forensik:** Cek keberadaan cleaner tools, registry gaps, timestamp anomalies

---

## Ringkasan

| Konsep | Deskripsi Singkat |
|--------|-------------------|
| **Windows Registry** | Database hierarkis untuk konfigurasi sistem dan aktivitas pengguna |
| **Registry Hive** | File fisik penyimpan registry (SAM, SYSTEM, SOFTWARE, NTUSER.DAT, dll.) |
| **SAM Hive** | Informasi akun pengguna, SID, login history, password hashes |
| **SYSTEM Hive** | Konfigurasi sistem, USB history (USBSTOR), ShimCache, shutdown time |
| **SOFTWARE Hive** | Program terinstal, versi OS, network profiles (SSID, MAC AP) |
| **NTUSER.DAT** | Artefak per-pengguna: UserAssist, MRU, ShellBags, MountPoints2 |
| **UserAssist** | Program GUI yang dijalankan, run count, last execution (ROT13 encoded) |
| **ShellBags** | Folder yang pernah dibrowse, termasuk removable media yang sudah dicabut |
| **ShimCache** | Application compatibility cache, executable yang diakses/dijalankan |
| **AmCache** | Execution history dengan SHA-1 hash untuk identifikasi malware |
| **BAM/DAM** | Per-user execution timestamps (Windows 10+) |
| **MRU Lists** | Recent documents, Run commands, typed paths, typed URLs |
| **USB Forensik** | USBSTOR + MountedDevices + MountPoints2 + ShellBags = profil USB lengkap |
| **Timeline Reconstruction** | Korelasi semua artefak registry untuk membangun kronologi kejadian |
| **Tools** | Registry Explorer, ShellBags Explorer, AmcacheParser, RegRipper, USBDeview |

---

## Referensi

1. Carvey, H. (2022). *Windows Forensic Analysis* (5th Ed.). Elsevier. Chapter 6-8.
2. Casey, E. (2022). *Digital Evidence and Computer Crime: Forensic Science, Computers, and the Internet* (4th Ed.). Academic Press. Chapter 9.
3. Phillips, A., Nelson, B., & Steuart, C. (2022). *Guide to Computer Forensics and Investigations* (6th Ed.). Cengage Learning. Chapter 6-7.
4. Sammons, J. (2020). *The Basics of Digital Forensics* (2nd Ed.). Syngress. Chapter 5.
5. Johansen, G. (2020). *Digital Forensics and Incident Response* (2nd Ed.). Packt. Chapter 7.
6. SANS Institute. (2023). *Windows Forensic Analysis Poster*. SANS DFIR.
7. Eric Zimmerman Tools Documentation. https://ericzimmerman.github.io/
8. NIST SP 800-86: Guide to Integrating Forensic Techniques into Incident Response.

---

## License / Lisensi

This repository is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

Commercial use is permitted, provided attribution is given to the author.

© 2026 Anindito
