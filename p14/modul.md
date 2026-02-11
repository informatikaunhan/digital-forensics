# Modul 14: Forensik Cloud Computing

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 14  
**Topik:** Forensik Cloud Computing  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Menjelaskan model layanan cloud computing (IaaS, PaaS, SaaS) dan implikasi forensiknya
2. Mengidentifikasi tantangan unik dalam investigasi forensik di lingkungan cloud
3. Menganalisis artefak forensik dari layanan cloud storage (Dropbox, Google Drive, OneDrive)
4. Melakukan forensik pada virtual machine dan disk image (VMDK, VHD)
5. Memahami forensik container (Docker dan Kubernetes)
6. Menganalisis cloud logging dan monitoring services
7. Menjelaskan isu jurisdiksi hukum dan multi-tenancy dalam cloud forensics

---

## 1. Pengantar Cloud Computing dan Forensik

### 1.1 Definisi Cloud Computing

> **Cloud Computing** adalah model penyediaan layanan komputasi (server, penyimpanan, database, jaringan, perangkat lunak) melalui internet ("the cloud") yang memungkinkan akses on-demand dengan skalabilitas fleksibel.

Cloud computing telah mengubah paradigma penyimpanan dan pemrosesan data secara fundamental. Dalam konteks militer, adopsi cloud membawa tantangan baru bagi investigator forensik karena bukti digital tidak lagi tersimpan di lokasi fisik yang dapat dikontrol secara langsung.

| Aspek | Komputasi Tradisional | Cloud Computing |
|-------|----------------------|-----------------|
| **Lokasi Data** | Server lokal, on-premise | Data center tersebar global |
| **Kontrol Fisik** | Penuh oleh organisasi | Bergantung pada provider |
| **Akses Data** | Melalui jaringan internal | Melalui internet |
| **Skalabilitas** | Terbatas kapasitas fisik | Elastis dan on-demand |
| **Forensik** | Akses langsung ke hardware | Memerlukan kerjasama provider |

### 1.2 Model Layanan Cloud Computing

Cloud computing memiliki tiga model layanan utama yang masing-masing memiliki implikasi forensik berbeda:

#### 1.2.1 Infrastructure as a Service (IaaS)

IaaS menyediakan infrastruktur komputasi virtual seperti server, storage, dan jaringan. Contoh: Amazon EC2, Microsoft Azure VM, Google Compute Engine.

Pada model IaaS, pengguna memiliki kontrol penuh atas sistem operasi dan aplikasi, sehingga artefak forensik mirip dengan sistem on-premise. Investigator dapat mengakses disk image virtual machine, memory dump, dan log sistem operasi.

#### 1.2.2 Platform as a Service (PaaS)

PaaS menyediakan platform pengembangan aplikasi lengkap. Contoh: Google App Engine, Microsoft Azure App Service, Heroku.

Akses forensik pada PaaS lebih terbatas karena pengguna hanya mengontrol aplikasi dan data, bukan sistem operasi atau infrastruktur di bawahnya. Artefak forensik utama berasal dari application logs dan database.

#### 1.2.3 Software as a Service (SaaS)

SaaS menyediakan aplikasi siap pakai melalui internet. Contoh: Google Workspace, Microsoft 365, Salesforce.

SaaS memiliki tantangan forensik terbesar karena pengguna hanya mengakses antarmuka aplikasi. Bukti digital bergantung pada fitur logging dan export yang disediakan provider.

| Model | Kontrol Pengguna | Artefak Forensik | Tantangan |
|-------|-----------------|-----------------|-----------|
| **IaaS** | OS, aplikasi, data | VM image, memory, OS logs | Akses ke hypervisor |
| **PaaS** | Aplikasi, data | App logs, database | Akses terbatas ke OS |
| **SaaS** | Data saja | Activity logs, export data | Bergantung pada provider |

#### Solved Problem 1 ⭐

**Soal:** Sebuah markas komando TNI menggunakan layanan IaaS untuk menjalankan server komunikasi virtual. Jelaskan artefak forensik apa saja yang tersedia bagi investigator dan bagaimana perbedaannya dengan forensik server fisik tradisional!

**Penyelesaian:**

*Step 1:* Identifikasi artefak pada IaaS

Pada model IaaS, investigator memiliki akses ke virtual machine disk image (VMDK/VHD), memory snapshot, network flow logs, dan hypervisor logs. Pengguna memiliki kontrol penuh atas guest OS sehingga artefak sistem operasi tersedia lengkap.

*Step 2:* Bandingkan dengan server fisik

| Artefak | Server Fisik | IaaS Cloud |
|---------|-------------|------------|
| Disk | Akses fisik langsung ke HDD/SSD | Virtual disk (VMDK/VHD) melalui snapshot |
| Memory | RAM dump fisik | Memory snapshot melalui hypervisor |
| Network | Tap fisik pada switch/router | Virtual network flow logs |
| BIOS/Firmware | Akses langsung | Tidak tersedia (dikelola provider) |
| Timestamp | Hardware clock | Sinkronisasi dengan NTP provider |

**Jawaban:** Pada IaaS, artefak forensik meliputi virtual disk image, memory snapshot, dan network flow logs. Perbedaan utama dengan server fisik: (1) tidak ada akses ke firmware/BIOS, (2) disk dalam format virtual yang memerlukan tools khusus, (3) memory capture melalui hypervisor bukan tools fisik, dan (4) bukti tambahan berupa API call logs dari cloud provider.

---

### 1.3 Model Deployment Cloud

Selain model layanan, cloud juga memiliki model deployment yang mempengaruhi strategi forensik:

1. **Public Cloud**: Infrastruktur dimiliki provider, digunakan bersama oleh banyak organisasi. Tantangan: multi-tenancy dan shared resources.
2. **Private Cloud**: Infrastruktur eksklusif untuk satu organisasi. Memberikan kontrol forensik lebih baik.
3. **Hybrid Cloud**: Kombinasi public dan private cloud. Data dapat berpindah antar environment.
4. **Community Cloud**: Infrastruktur dibagi beberapa organisasi dengan kepentingan serupa (contoh: cloud khusus militer).

Dalam konteks pertahanan Indonesia, penggunaan private cloud atau community cloud (seperti Government Cloud / GovCloud) menjadi pilihan strategis untuk menjaga kedaulatan data sambil memanfaatkan fleksibilitas cloud.

#### Solved Problem 2 ⭐

**Soal:** Mengapa model hybrid cloud menimbulkan tantangan lebih kompleks bagi investigator forensik dibandingkan private cloud?

**Penyelesaian:**

*Step 1:* Analisis arsitektur hybrid cloud

Hybrid cloud menggabungkan infrastruktur on-premise (private) dengan layanan public cloud. Data dan workload dapat berpindah antar environment berdasarkan kebijakan atau kebutuhan.

*Step 2:* Identifikasi tantangan forensik

Tantangan utama: (1) data tersebar di dua lokasi dengan yurisdiksi berbeda, (2) perlu koordinasi dengan cloud provider untuk akses bukti di public cloud, (3) network traffic antara on-premise dan cloud memerlukan monitoring terpisah, (4) timestamp synchronization antara dua environment, (5) chain of custody lebih kompleks karena melibatkan pihak ketiga.

**Jawaban:** Hybrid cloud lebih kompleks karena bukti digital tersebar di dua environment dengan kontrol akses berbeda. Investigator harus berkoordinasi dengan provider untuk bagian public cloud, menangani isu yurisdiksi lintas batas, dan menjaga konsistensi chain of custody di kedua environment.

---

## 2. Tantangan Forensik di Lingkungan Cloud

### 2.1 Tantangan Teknis

#### 2.1.1 Multi-Tenancy dan Evidence Isolation

Dalam lingkungan cloud, beberapa pelanggan (tenants) berbagi infrastruktur fisik yang sama. Tantangan utama adalah mengisolasi bukti digital milik target investigasi tanpa mengakses data tenant lain.

Multi-tenancy menciptakan risiko: (1) data cross-contamination antar tenant, (2) kesulitan membuktikan bahwa bukti berasal dari VM target, (3) shared storage yang membuat data recovery lebih kompleks, dan (4) network traffic yang tercampur dalam infrastruktur virtual.

#### 2.1.2 Volatilitas dan Elastisitas

Cloud resources bersifat ephemeral — virtual machine dapat dibuat dan dihancurkan dalam hitungan menit. Auto-scaling dapat menambah atau menghapus instance secara otomatis. Bukti digital dapat hilang ketika instance di-terminate.

Strategi mitigasi meliputi: continuous logging ke external storage, snapshot berkala untuk VM kritis, dan implementasi forensic readiness framework yang mengantisipasi kebutuhan investigasi.

#### 2.1.3 Distributed Architecture

Data dalam cloud tersebar di beberapa data center yang mungkin berlokasi di negara berbeda. Replikasi data untuk redundancy dan availability membuat penentuan lokasi "asli" data menjadi sulit.

#### Solved Problem 3 ⭐⭐

**Soal:** Seorang investigator forensik militer perlu mengakuisisi bukti dari server cloud yang digunakan kelompok teroris siber. Server tersebut berjalan di AWS dengan auto-scaling group yang menambah/menghapus instance berdasarkan load. Jelaskan strategi akuisisi yang tepat!

**Penyelesaian:**

*Step 1:* Identifikasi tantangan spesifik

Auto-scaling group secara otomatis membuat dan menghancurkan EC2 instance. Jika instance di-terminate, data ephemeral pada instance storage akan hilang. Hanya data pada EBS volume yang persisten.

*Step 2:* Rancang strategi akuisisi

Strategi multi-layer: (1) segera minta AWS untuk membekukan atau mempreservasi instance melalui proses hukum, (2) buat snapshot dari semua EBS volume yang terkait, (3) capture memory dari running instance menggunakan tools seperti LiME atau melalui AWS SSM, (4) ambil VPC Flow Logs dan CloudTrail logs sebelum melewati retention period, (5) disable auto-scaling sementara untuk mencegah penghapusan instance aktif.

*Step 3:* Preservasi bukti

Simpan semua snapshot dan logs ke S3 bucket terpisah dengan write-once policy. Hitung hash untuk setiap artefak dan dokumentasikan chain of custody.

**Jawaban:** Strategi akuisisi meliputi: pembekuan instance melalui jalur hukum, snapshot EBS volume, memory capture dari instance aktif, pengumpulan CloudTrail dan VPC Flow Logs, serta penonaktifan auto-scaling. Semua bukti dipreservasi dengan hashing dan write-once policy.

---

### 2.2 Tantangan Hukum dan Jurisdiksi

#### 2.2.1 Cross-Border Data

Cloud provider global menyimpan data di data center yang tersebar di berbagai negara. Investigasi forensik cloud sering melibatkan isu yurisdiksi lintas batas karena data mungkin tersimpan di negara yang berbeda dari lokasi investigator.

Tantangan hukum meliputi: (1) perbedaan regulasi privasi data antar negara (GDPR di Eropa, UU PDP di Indonesia), (2) proses Mutual Legal Assistance Treaty (MLAT) yang memakan waktu, (3) potensi konflik hukum antara negara tempat data disimpan dan negara yang meminta akses, dan (4) cloud provider yang mungkin menolak memberikan akses tanpa perintah pengadilan dari yurisdiksi yang tepat.

#### 2.2.2 Service Level Agreement (SLA) dan Terms of Service

SLA dan ToS cloud provider menentukan hak dan kewajiban terkait akses data, termasuk: kemampuan provider untuk memberikan data kepada penegak hukum, retention period untuk log dan data, prosedur untuk merespons legal request, dan batasan tanggung jawab provider dalam preservasi bukti.

#### 2.2.3 Regulasi Indonesia

Di Indonesia, beberapa regulasi relevan untuk forensik cloud meliputi:

1. **PP No. 71 Tahun 2019** tentang Penyelenggaraan Sistem dan Transaksi Elektronik — mengatur kewajiban penyelenggara sistem elektronik untuk menyediakan akses data bagi penegak hukum
2. **UU No. 27 Tahun 2022** tentang Pelindungan Data Pribadi — mengatur transfer data lintas batas
3. **Permenhan terkait keamanan informasi** — mengatur pengelolaan data pertahanan

#### Solved Problem 4 ⭐⭐

**Soal:** Badan Siber dan Sandi Negara (BSSN) menduga adanya eksfiltrasi data rahasia pertahanan melalui layanan cloud storage yang server-nya berada di Singapura. Jelaskan langkah hukum dan teknis yang diperlukan untuk mengakses bukti!

**Penyelesaian:**

*Step 1:* Jalur hukum

Langkah hukum: (1) ajukan permintaan resmi ke penyedia layanan cloud melalui jalur hukum Indonesia (PP 71/2019), (2) jika provider tidak merespons, gunakan jalur MLAT antara Indonesia dan Singapura, (3) koordinasi dengan Computer Emergency Response Team (CERT) Singapura melalui jalur diplomatik, (4) jika data terkait ancaman pertahanan nasional, dapat menggunakan jalur kerja sama intelijen.

*Step 2:* Langkah teknis paralel

Secara paralel: (1) akuisisi artefak cloud client pada endpoint Indonesia (cache, sync folders, database lokal), (2) analisis network logs untuk mengidentifikasi komunikasi dengan cloud server, (3) capture metadata dan activity logs yang masih tersedia, (4) preservasi bukti dari email dan notifikasi terkait aktivitas cloud.

**Jawaban:** Diperlukan kombinasi jalur hukum (PP 71/2019, MLAT Indonesia-Singapura, koordinasi diplomatik) dan teknis (akuisisi artefak cloud client lokal, analisis network traffic, preservasi metadata). Investigator harus bekerja secara paralel pada kedua jalur karena proses hukum lintas batas membutuhkan waktu.

---

## 3. Cloud Storage Forensics

### 3.1 Artefak Dropbox pada Windows

Dropbox menyimpan berbagai artefak forensik pada sistem lokal pengguna:

#### 3.1.1 Lokasi Database dan Cache

| Artefak | Lokasi |
|---------|--------|
| Database konfigurasi | `%APPDATA%\Dropbox\instance1\config.dbx` |
| File index database | `%APPDATA%\Dropbox\instance_db\` |
| Cache files | `%LOCALAPPDATA%\Dropbox\` |
| Sync history | `%APPDATA%\Dropbox\instance1\sync_history.db` |
| Deleted file cache | `%USERPROFILE%\Dropbox\.dropbox.cache\` |
| Filecache database | `%APPDATA%\Dropbox\instance1\filecache.db` |

#### 3.1.2 Informasi Forensik dari Dropbox

Database Dropbox (format SQLite yang dienkripsi dengan DPAPI) mengandung informasi berharga:

1. **Account Information**: Email, display name, user ID yang terdaftar
2. **File Metadata**: Nama file, ukuran, hash, timestamp modifikasi
3. **Sync History**: Riwayat sinkronisasi file termasuk yang sudah dihapus
4. **Shared Links**: Daftar file yang dibagikan dan penerima
5. **Device Information**: Nama perangkat, OS version, last connect time

#### 3.1.3 Dropbox Cache Forensics

Folder `.dropbox.cache` menyimpan file yang baru dihapus dari cloud selama beberapa hari sebelum purge otomatis. Ini menjadi sumber bukti penting untuk file yang sudah dihapus dari akun Dropbox.

#### Solved Problem 5 ⭐⭐

**Soal:** Seorang perwira diduga membocorkan dokumen rahasia melalui Dropbox. Investigator menemukan laptop tersangka tetapi akun Dropbox sudah di-unlink. Artefak apa yang masih dapat diekstrak?

**Penyelesaian:**

*Step 1:* Periksa residual artifacts

Meskipun akun sudah di-unlink, beberapa artefak mungkin masih ada: (1) folder sinkronisasi lokal (`%USERPROFILE%\Dropbox\`) mungkin masih berisi file jika tidak dihapus manual, (2) database cache di `%APPDATA%\Dropbox\` mungkin masih ada, (3) `.dropbox.cache` mungkin berisi file yang baru dihapus, (4) Windows Registry masih menyimpan informasi instalasi dan konfigurasi.

*Step 2:* Analisis tambahan

Artefak tambahan: (1) Prefetch files untuk Dropbox.exe menunjukkan kapan terakhir dijalankan, (2) NTFS Journal dan USN Journal mungkin mencatat operasi file di folder Dropbox, (3) Thumbnail cache (thumbcache_*.db) mungkin berisi preview dokumen, (4) Windows Search database mungkin telah mengindeks konten file Dropbox, (5) Web browser history jika dropbox.com diakses via browser.

**Jawaban:** Artefak yang masih tersedia: residual sync folder, database cache, deleted file cache, Registry entries, Prefetch files, NTFS Journal, thumbnail cache, Windows Search index, dan browser history. Investigator harus memeriksa semua lokasi ini secara sistematis karena unlink hanya memutus koneksi akun tanpa selalu menghapus semua artefak lokal.

---

### 3.2 Artefak Google Drive pada Windows

#### 3.2.1 Lokasi Artefak Google Drive

| Artefak | Lokasi |
|---------|--------|
| Database utama | `%LOCALAPPDATA%\Google\DriveFS\` |
| Metadata database | `%LOCALAPPDATA%\Google\DriveFS\<account_hash>\metadata_sqlite_db` |
| Mirror folder | `%USERPROFILE%\My Drive\` atau lokasi custom |
| Content cache | `%LOCALAPPDATA%\Google\DriveFS\<account_hash>\content_cache\` |
| Log files | `%LOCALAPPDATA%\Google\DriveFS\Logs\` |

#### 3.2.2 Metadata Database Google Drive

Database `metadata_sqlite_db` mengandung informasi kritis:

1. **items table**: Daftar semua file dan folder (termasuk yang dihapus)
2. **properties**: Metadata file termasuk owner, shared status
3. **local_entry**: Mapping antara file cloud dan file lokal
4. **cloud_entry**: Informasi versi cloud dan hash content

#### 3.2.3 Google Drive Stream vs Mirror

Google Drive mendukung dua mode sinkronisasi:

- **Stream**: File disimpan di cloud, hanya didownload saat diakses. Artefak lokal terbatas pada cache.
- **Mirror**: File disimpan secara penuh di lokal dan cloud. Artefak lokal lebih lengkap.

Perbedaan ini penting bagi investigator karena mode stream menyimpan lebih sedikit data lokal.

#### Solved Problem 6 ⭐⭐

**Soal:** Bagaimana cara membedakan file yang di-stream versus yang di-mirror pada Google Drive, dan apa implikasinya terhadap kelengkapan bukti forensik?

**Penyelesaian:**

*Step 1:* Identifikasi mode sinkronisasi

Mode dapat diidentifikasi dari: (1) konfigurasi di Google Drive preferences, (2) database metadata yang mencatat status sinkronisasi setiap file, (3) ukuran folder lokal — mode mirror menggunakan ruang disk sesuai ukuran file, stream hanya menyimpan metadata dan cache.

*Step 2:* Implikasi forensik

Mode Stream: hanya file yang pernah dibuka yang memiliki cache lokal. File yang tidak pernah diakses tidak memiliki konten lokal. Investigator perlu mengakses akun Google untuk mendapatkan file lengkap.

Mode Mirror: semua file tersedia secara lokal. Investigator mendapatkan salinan lengkap semua konten tanpa perlu akses ke akun cloud.

**Jawaban:** Mode stream hanya menyimpan cache file yang pernah diakses sehingga bukti lokal tidak lengkap. Mode mirror menyimpan semua file secara lokal, memberikan bukti yang lebih lengkap. Investigator perlu memeriksa konfigurasi Drive dan database metadata untuk menentukan mode yang digunakan.

---

### 3.3 Artefak OneDrive pada Windows

#### 3.3.1 Lokasi Artefak OneDrive

| Artefak | Lokasi |
|---------|--------|
| Database utama | `%LOCALAPPDATA%\Microsoft\OneDrive\settings\` |
| Sync database | `%LOCALAPPDATA%\Microsoft\OneDrive\logs\` |
| File metadata | `%LOCALAPPDATA%\Microsoft\OneDrive\settings\<cid>.dat` |
| Sync folder | `%USERPROFILE%\OneDrive\` |
| Files On-Demand placeholder | NTFS reparse points |
| OneDrive log | `%LOCALAPPDATA%\Microsoft\OneDrive\logs\Business1\SyncDiagnostics.log` |

#### 3.3.2 OneDrive Files On-Demand

Fitur "Files On-Demand" pada OneDrive menggunakan NTFS reparse points untuk menampilkan file cloud di File Explorer tanpa menyimpan konten secara lokal. Status file ditandai dengan ikon:

- **Cloud icon** (☁️): File hanya online, konten tidak tersimpan lokal
- **Green checkmark** (✅): File tersinkronisasi penuh ke lokal
- **Green circle with checkmark**: File "Always keep on this device"

Dari perspektif forensik, file "online-only" tidak memiliki konten lokal, tetapi metadata-nya tetap tercatat dalam database OneDrive dan NTFS.

#### Solved Problem 7 ⭐

**Soal:** Jelaskan bagaimana fitur Files On-Demand pada OneDrive mempengaruhi proses akuisisi forensik!

**Penyelesaian:**

*Step 1:* Analisis dampak

Files On-Demand menyebabkan file yang ditampilkan di File Explorer belum tentu memiliki konten lokal. Saat melakukan forensic imaging, file "online-only" hanya menghasilkan placeholder (reparse point) tanpa konten aktual.

*Step 2:* Strategi akuisisi

Strategi: (1) identifikasi status setiap file (online-only vs locally available) dari NTFS attributes, (2) untuk file online-only, akses diperlukan ke akun OneDrive, (3) metadata dari database OneDrive lokal tetap mengandung informasi berharga (nama file, ukuran, timestamps, sharing status), (4) file yang pernah dibuka mungkin masih ada di cache.

**Jawaban:** Files On-Demand membuat forensic imaging standar tidak mengambil konten file online-only. Investigator perlu memeriksa NTFS reparse points untuk mengidentifikasi file yang hanya ada di cloud, menganalisis database metadata lokal untuk informasi file, dan mengakses akun OneDrive untuk mendapatkan konten lengkap.

---

## 4. Virtual Machine Forensics

### 4.1 Format Disk Virtual Machine

Virtual machine menyimpan data dalam file disk virtual yang dapat dianalisis secara forensik:

| Format | Platform | Ekstensi | Keterangan |
|--------|----------|----------|------------|
| VMDK | VMware | .vmdk | Virtual Machine Disk |
| VHD | Microsoft Hyper-V | .vhd | Virtual Hard Disk (legacy) |
| VHDX | Microsoft Hyper-V | .vhdx | Virtual Hard Disk v2 (modern) |
| QCOW2 | KVM/QEMU | .qcow2 | QEMU Copy-on-Write |
| VDI | VirtualBox | .vdi | Virtual Disk Image |
| OVA/OVF | Multi-platform | .ova, .ovf | Open Virtualization Format |

### 4.2 Analisis VMDK dengan FTK Imager

FTK Imager dapat langsung membuka file VMDK sebagai evidence item:

1. Buka FTK Imager → File → Add Evidence Item
2. Pilih "Image File" sebagai source type
3. Browse ke lokasi file VMDK
4. FTK Imager akan me-mount VMDK dan menampilkan file system di dalamnya

Investigator kemudian dapat melakukan analisis standar: browsing file system, keyword search, file recovery, dan export evidence.

### 4.3 Snapshot dan Memory Analysis

Virtual machine snapshots menyimpan state lengkap VM pada titik waktu tertentu, termasuk:

1. **Disk state**: Perubahan disk sejak snapshot dibuat (stored as delta/differencing disk)
2. **Memory state**: Isi RAM VM pada saat snapshot (tersimpan sebagai .vmem atau .vsv file)
3. **Configuration state**: Pengaturan hardware virtual

File memory snapshot (.vmem untuk VMware, .bin untuk Hyper-V) dapat dianalisis menggunakan Volatility framework untuk mengekstrak informasi dari RAM:

```
# Contoh analisis memory snapshot VMware menggunakan Volatility 3
python3 vol.py -f snapshot.vmem windows.pslist
python3 vol.py -f snapshot.vmem windows.netscan
python3 vol.py -f snapshot.vmem windows.filescan
```

#### Solved Problem 8 ⭐⭐

**Soal:** Investigator menemukan folder VMware dengan file berikut: `server.vmx`, `server.vmdk`, `server-s001.vmdk`, `server-Snapshot1.vmsn`, `server-Snapshot1.vmem`. Jelaskan apa fungsi masing-masing file dan informasi forensik yang dapat diekstrak!

**Penyelesaian:**

*Step 1:* Identifikasi setiap file

| File | Fungsi | Informasi Forensik |
|------|--------|-------------------|
| `server.vmx` | Konfigurasi VM | Hardware settings, network config, nama VM |
| `server.vmdk` | Base disk image | Seluruh file system guest OS |
| `server-s001.vmdk` | Delta disk | Perubahan sejak snapshot |
| `server-Snapshot1.vmsn` | Snapshot state | Metadata snapshot (timestamp, description) |
| `server-Snapshot1.vmem` | Memory snapshot | Isi RAM saat snapshot — proses, network, credentials |

*Step 2:* Strategi analisis

Analisis disk: buka `server.vmdk` di FTK Imager untuk browsing file system. Untuk state terkini, gabungkan base disk dengan delta disk.

Analisis memory: buka `server-Snapshot1.vmem` di Volatility untuk mengekstrak running processes, network connections, open files, dan potential credentials.

**Jawaban:** Setiap file mewakili komponen VM: vmx (konfigurasi), vmdk (disk), vmsn (snapshot metadata), vmem (memory capture). Investigator harus menganalisis disk image untuk artefak file system dan memory snapshot untuk data volatile menggunakan Volatility.

---

### 4.4 Hypervisor Forensics

Hypervisor (VMware ESXi, Microsoft Hyper-V, KVM) menyediakan informasi forensik tambahan:

1. **Hypervisor logs**: Event creation, deletion, modification VM
2. **Performance metrics**: Resource usage yang dapat menunjukkan aktivitas anomali
3. **Network virtual switch logs**: Traffic antar VM
4. **Storage controller logs**: I/O operations pada virtual disk

Pada VMware ESXi, log utama berada di `/var/log/vmkernel.log` dan `/var/log/hostd.log`. Pada Hyper-V, informasi tercatat di Windows Event Log dengan source "Hyper-V-*".

#### Solved Problem 9 ⭐⭐

**Soal:** Seorang administrator data center militer diduga membuat VM tidak sah untuk menjalankan cryptocurrency mining. Bagaimana hypervisor logs dapat digunakan untuk membuktikan hal ini?

**Penyelesaian:**

*Step 1:* Identifikasi artefak hypervisor

Log hypervisor mencatat lifecycle setiap VM: creation time, configuration changes, resource allocation, dan power state events. Administrator yang membuat VM tidak sah akan meninggalkan jejak di log ini.

*Step 2:* Analisis spesifik

Bukti yang dicari: (1) VM creation event dengan timestamp dan user yang membuatnya, (2) resource allocation yang tidak wajar (CPU dan GPU tinggi untuk mining), (3) network traffic ke mining pool, (4) performance metrics yang menunjukkan penggunaan resource berlebihan, (5) pola power on/off yang tidak lazim (misalnya hanya aktif di luar jam kerja).

**Jawaban:** Hypervisor logs mencatat VM creation event (termasuk siapa yang membuat), resource allocation anomali, dan aktivitas jaringan. Mining VM teridentifikasi dari high CPU usage, network connections ke mining pool, dan VM yang dibuat tanpa otorisasi resmi.

---

## 5. Container Forensics

### 5.1 Docker Forensics

Docker container merupakan teknologi virtualisasi ringan yang semakin banyak digunakan. Dari perspektif forensik, Docker menyimpan artefak pada beberapa lokasi:

#### 5.1.1 Lokasi Artefak Docker

| Artefak | Lokasi (Linux) | Keterangan |
|---------|----------------|------------|
| Container data | `/var/lib/docker/containers/` | Metadata, logs per container |
| Image layers | `/var/lib/docker/overlay2/` | File system layers |
| Volumes | `/var/lib/docker/volumes/` | Persistent data |
| Network config | `/var/lib/docker/network/` | Network namespace config |
| Container logs | `/var/lib/docker/containers/<id>/<id>-json.log` | stdout/stderr logs |

#### 5.1.2 Docker Forensic Commands

```bash
# List semua container (termasuk stopped)
docker ps -a

# Inspect detail container
docker inspect <container_id>

# Export container filesystem
docker export <container_id> > container_backup.tar

# Melihat perubahan filesystem
docker diff <container_id>

# Melihat history image
docker history <image_name>

# Melihat logs container
docker logs <container_id>
```

### 5.2 Kubernetes Forensics

Kubernetes (K8s) orchestration platform memiliki artefak forensik yang lebih kompleks:

1. **etcd database**: Menyimpan seluruh state cluster
2. **Pod logs**: Log dari setiap container dalam pod
3. **Audit logs**: Mencatat semua API request ke cluster
4. **Events**: Kejadian penting dalam cluster
5. **ConfigMaps dan Secrets**: Konfigurasi dan credentials (mungkin terenkripsi)

#### Solved Problem 10 ⭐⭐⭐

**Soal:** Sebuah container Docker yang digunakan sebagai C2 (Command and Control) server oleh penyerang telah dihentikan (stopped) tetapi belum dihapus. Jelaskan langkah forensik untuk mengekstrak bukti!

**Penyelesaian:**

*Step 1:* Preservasi container

Pertama, pastikan container tidak dihapus: `docker ps -a` untuk memverifikasi keberadaan. Buat backup segera: `docker export <container_id> > c2_evidence.tar` untuk mengekstrak filesystem.

*Step 2:* Ekstraksi artefak

| Langkah | Command | Informasi |
|---------|---------|-----------|
| 1. Inspect | `docker inspect <id>` | Config, network, mounts, timestamps |
| 2. Logs | `docker logs <id>` | Command history, output |
| 3. Diff | `docker diff <id>` | Modified files dari base image |
| 4. Export | `docker export <id>` | Full filesystem |
| 5. Copy | `docker cp <id>:/path /local` | File spesifik |

*Step 3:* Analisis

Dari filesystem yang diekstrak: (1) analisis tools yang diinstal oleh penyerang, (2) periksa log files dan command history, (3) identifikasi konfigurasi C2 (IP address, ports, certificates), (4) periksa network connections dan DNS resolv.conf, (5) analisis crontab dan scripts untuk persistence.

**Jawaban:** Langkah forensik: export filesystem container, extract logs, inspect metadata, analisis diff untuk perubahan dari base image. Fokus pada tools penyerang, konfigurasi C2 (IP, port, certificates), dan artefak komunikasi. Container stopped masih menyimpan seluruh filesystem dan logs.

---

## 6. Cloud Logging dan Monitoring

### 6.1 AWS CloudTrail

AWS CloudTrail mencatat semua API calls dalam akun AWS. Setiap event log mencakup:

```json
{
    "eventVersion": "1.08",
    "eventTime": "2025-01-15T08:30:00Z",
    "eventSource": "ec2.amazonaws.com",
    "eventName": "RunInstances",
    "awsRegion": "ap-southeast-1",
    "sourceIPAddress": "203.0.113.50",
    "userIdentity": {
        "type": "IAMUser",
        "userName": "admin-user"
    },
    "requestParameters": {
        "instanceType": "t2.micro",
        "imageId": "ami-0abcdef1234"
    }
}
```

Informasi forensik dari CloudTrail: siapa (userIdentity), apa (eventName), kapan (eventTime), dari mana (sourceIPAddress), dan detail operasi (requestParameters).

### 6.2 Azure Activity Log

Microsoft Azure mencatat aktivitas melalui beberapa layer:

1. **Activity Log**: Operasi pada level resource (create, modify, delete)
2. **Sign-in Logs**: Autentikasi pengguna dan status
3. **Audit Logs**: Perubahan pada Azure AD
4. **Resource Logs**: Aktivitas detail per-resource

### 6.3 Google Cloud Audit Logs

Google Cloud Platform menyediakan:

1. **Admin Activity Logs**: Perubahan konfigurasi resource (selalu aktif)
2. **Data Access Logs**: Operasi baca/tulis data (perlu diaktifkan)
3. **System Event Logs**: Aksi otomatis oleh Google
4. **Policy Denied Logs**: Request yang ditolak oleh kebijakan

#### Solved Problem 11 ⭐⭐

**Soal:** Dari sample CloudTrail log berikut, identifikasi aktivitas mencurigakan dan jelaskan signifikansi forensiknya!

```json
{
    "eventTime": "2025-01-15T02:30:00Z",
    "eventName": "CreateAccessKey",
    "sourceIPAddress": "185.220.101.45",
    "userIdentity": {"userName": "svc-backup"},
    "responseElements": {"accessKey": {"accessKeyId": "AKIA..."}}
}
```

**Penyelesaian:**

*Step 1:* Analisis event

Event menunjukkan pembuatan access key baru untuk akun `svc-backup` pada pukul 02:30 UTC (dini hari). IP address `185.220.101.45` perlu diperiksa — IP range `185.220.101.x` dikenal sebagai Tor exit node.

*Step 2:* Identifikasi anomali

Anomali: (1) Service account (`svc-backup`) seharusnya tidak membuat access key baru — ini menunjukkan credential compromise, (2) waktu operasi di luar jam kerja, (3) source IP dari Tor network menunjukkan upaya anonimisasi, (4) pembuatan access key baru bisa menjadi langkah persistence oleh penyerang.

**Jawaban:** Event menunjukkan kemungkinan credential compromise: service account membuat access key baru dari Tor exit node pada dini hari. Ini merupakan teknik persistence — penyerang membuat credential baru untuk mempertahankan akses meskipun password di-reset.

---

### 6.4 Cloud Log Analysis Tools

Beberapa tools yang berguna untuk analisis cloud logs:

| Tool | Fungsi | Ketersediaan |
|------|--------|-------------|
| **jq** | Command-line JSON processor | Free, open source |
| **AWS CLI** | Akses dan filter CloudTrail | Free |
| **Azure CLI** | Akses Azure logs | Free |
| **gcloud CLI** | Akses GCP logs | Free |
| **Elastic Stack** | Centralized log analysis | Free (basic) |
| **Splunk** | SIEM dan log analysis | Commercial (free trial) |

Contoh penggunaan jq untuk analisis CloudTrail:

```bash
# Filter event berdasarkan nama
cat cloudtrail.json | jq '.Records[] | select(.eventName == "CreateAccessKey")'

# Cari aktivitas dari IP tertentu
cat cloudtrail.json | jq '.Records[] | select(.sourceIPAddress == "185.220.101.45")'

# List semua user dan event unik
cat cloudtrail.json | jq '[.Records[] | {user: .userIdentity.userName, event: .eventName}] | unique'
```

#### Solved Problem 12 ⭐⭐

**Soal:** Tuliskan perintah jq untuk menemukan semua event "DeleteBucket" pada CloudTrail log yang terjadi di luar jam kerja (sebelum 07:00 atau setelah 18:00 UTC)!

**Penyelesaian:**

*Step 1:* Identifikasi filter yang diperlukan

Perlu memfilter berdasarkan: (1) eventName == "DeleteBucket", dan (2) jam dari eventTime < 07 atau > 18.

*Step 2:* Konstruksi perintah jq

```bash
cat cloudtrail.json | jq '.Records[] | select(.eventName == "DeleteBucket") | select((.eventTime | split("T")[1] | split(":")[0] | tonumber) < 7 or (.eventTime | split("T")[1] | split(":")[0] | tonumber) > 18)'
```

**Jawaban:** Perintah di atas memfilter event DeleteBucket yang terjadi di luar jam kerja dengan mem-parsing hour dari timestamp eventTime dan membandingkannya dengan threshold 07:00 dan 18:00 UTC.

---

## 7. Data Acquisition dari Cloud Environments

### 7.1 Metode Akuisisi Cloud

Akuisisi data dari cloud memerlukan pendekatan yang berbeda dari forensik tradisional:

| Metode | Deskripsi | Kelebihan | Kekurangan |
|--------|-----------|-----------|------------|
| **Snapshot-based** | Buat snapshot VM/disk | Preservasi state lengkap | Memerlukan akses ke akun cloud |
| **API-based** | Ekstrak data melalui API | Dapat diautomasi, scripting | Terbatas pada data yang di-expose API |
| **Client-based** | Analisis artefak pada client lokal | Tidak perlu akses cloud | Data mungkin tidak lengkap |
| **Provider-assisted** | Request data dari provider | Akses ke server-side logs | Proses hukum yang panjang |

### 7.2 Akuisisi Artefak Cloud Client

Langkah akuisisi artefak cloud storage client pada Windows:

1. **Identifikasi cloud services**: Periksa installed programs, startup items, dan system tray
2. **Locate database files**: Navigasi ke lokasi artefak masing-masing layanan
3. **Copy database files**: Gunakan FTK Imager atau robocopy untuk salinan forensik
4. **Parse databases**: Gunakan DB Browser for SQLite untuk analisis database
5. **Extract deleted artifacts**: Periksa cache folders dan unallocated space

### 7.3 Chain of Custody untuk Cloud Evidence

Dokumentasi chain of custody untuk bukti cloud memiliki elemen tambahan:

1. **Cloud Service Information**: Provider, region, account identifier
2. **Acquisition Method**: API call, snapshot, atau client-side
3. **Timestamp Synchronization**: Verifikasi sinkronisasi waktu antar evidence source
4. **Hash Verification**: Hash setiap file/snapshot yang diakuisisi
5. **Access Logs**: Dokumentasi siapa yang mengakses cloud account dan kapan

#### Solved Problem 13 ⭐

**Soal:** Jelaskan mengapa akuisisi forensik berbasis snapshot lebih diutamakan dibandingkan API-based untuk kasus investigasi yang memerlukan preservasi bukti lengkap!

**Penyelesaian:**

*Step 1:* Analisis metode snapshot

Snapshot menangkap state lengkap disk (atau VM) pada titik waktu tertentu, termasuk: file aktif, deleted files pada unallocated space, file system metadata, dan potential artifacts yang tidak terekspos melalui API.

*Step 2:* Bandingkan dengan API-based

API-based hanya dapat mengekstrak data yang disediakan oleh API provider. Ini berarti: tidak ada akses ke deleted data, tidak ada file system metadata lengkap, dan terbatas pada apa yang provider tentukan sebagai exportable data.

**Jawaban:** Snapshot preservasi state disk lengkap termasuk deleted files dan unallocated space yang tidak tersedia via API. Untuk investigasi yang memerlukan bukti komprehensif, snapshot memberikan forensic image yang setara dengan akuisisi disk tradisional.

---

## 8. Multi-Tenancy Challenges dan Evidence Isolation

### 8.1 Risiko Kontaminasi Bukti

Dalam lingkungan multi-tenant, risiko kontaminasi bukti meliputi:

1. **Shared storage**: Data dari tenant berbeda mungkin berada pada disk fisik yang sama
2. **Memory residue**: RAM yang sebelumnya digunakan tenant lain mungkin mengandung data residual
3. **Network co-mingling**: Traffic dari berbagai tenant melewati infrastructure yang sama
4. **Cached data**: Cache dari aktivitas tenant sebelumnya pada resource yang sama

### 8.2 Strategi Evidence Isolation

Untuk memastikan validitas bukti dari lingkungan multi-tenant:

1. **Logical isolation verification**: Pastikan data yang dikumpulkan hanya milik target investigasi
2. **Timestamp correlation**: Verifikasi bahwa artefak cocok dengan timeline aktivitas target
3. **Metadata analysis**: Periksa owner, permissions, dan access control pada setiap artefak
4. **Network segmentation review**: Validasi bahwa traffic yang dianalisis berasal dari resource target
5. **Provider documentation**: Dapatkan surat keterangan dari provider tentang isolasi tenant

#### Solved Problem 14 ⭐⭐⭐

**Soal:** Dalam investigasi terhadap tenant yang diduga melakukan data exfiltration dari shared cloud environment, bagaimana investigator memastikan bahwa bukti yang dikumpulkan valid dan tidak terkontaminasi oleh data tenant lain?

**Penyelesaian:**

*Step 1:* Strategi isolasi teknis

Langkah teknis: (1) minta provider untuk memberikan data khusus tenant target melalui proses formal, (2) verifikasi resource isolation melalui tenant ID dan VPC/VNET configuration, (3) cross-reference semua artefak dengan account identifiers tenant target.

*Step 2:* Validasi forensik

Validasi: (1) setiap artefak harus memiliki identifiable link ke tenant target (user ID, VM ID, IP address), (2) timestamp harus konsisten dengan timeline operasional tenant, (3) dokumentasikan metode isolasi yang digunakan untuk keperluan persidangan, (4) dapatkan surat resmi dari provider yang menerangkan arsitektur isolasi tenant mereka.

**Jawaban:** Investigator memastikan validitas bukti melalui: request formal ke provider untuk data tenant-specific, verifikasi tenant ID pada setiap artefak, cross-reference timestamps dengan timeline operasional, dan dokumentasi resmi dari provider tentang isolasi multi-tenant. Setiap bukti harus dapat di-trace ke identifier unik tenant target.

---

## 9. Forensik Cloud dalam Konteks Militer

### 9.1 Cloud Computing di Lingkungan Pertahanan Indonesia

Penggunaan cloud computing di lingkungan militer Indonesia memiliki pertimbangan khusus:

1. **Kedaulatan Data**: Data pertahanan harus disimpan di data center dalam wilayah Indonesia
2. **Government Cloud**: Penggunaan layanan cloud khusus pemerintah yang memenuhi standar keamanan
3. **Klasifikasi Data**: Tidak semua data pertahanan boleh ditempatkan di cloud — data rahasia dan sangat rahasia memerlukan infrastruktur terpisah
4. **Audit dan Monitoring**: Kewajiban logging yang lebih ketat dibanding cloud komersial

### 9.2 Skenario Forensik Cloud Militer

Beberapa skenario yang mungkin dihadapi investigator forensik militer:

1. **Insider threat**: Personel militer menggunakan cloud storage komersial untuk mengeksfiltrasi data rahasia
2. **Compromised cloud infrastructure**: Serangan APT terhadap cloud militer
3. **Shadow IT**: Penggunaan layanan cloud tidak sah oleh unit-unit militer
4. **Supply chain attack**: Kompromi terhadap cloud provider yang digunakan kontraktor pertahanan

#### Solved Problem 15 ⭐⭐⭐

**Soal:** Sebuah insiden keamanan di Kodam Jaya mendeteksi bahwa seorang prajurit menggunakan akun Google Drive pribadi untuk menyimpan dokumen perencanaan operasi militer. Investigator menemukan laptop prajurit tersebut. Jelaskan langkah investigasi forensik cloud yang lengkap!

**Penyelesaian:**

*Step 1:* Preservasi evidence

Langkah awal: (1) ambil forensic image dari laptop menggunakan FTK Imager, (2) dokumentasikan status layar dan running processes, (3) capture memory jika laptop masih menyala, (4) catat semua cloud sync indicators di system tray.

*Step 2:* Analisis artefak Google Drive lokal

Analisis pada image: (1) periksa `%LOCALAPPDATA%\Google\DriveFS\` untuk database metadata, (2) identifikasi dokumen yang disinkronkan melalui metadata_sqlite_db, (3) periksa folder sinkronisasi lokal untuk salinan dokumen, (4) analisis browser history untuk aktivitas Google Drive web.

*Step 3:* Akuisisi cloud-side evidence

Melalui jalur hukum dan komando: (1) ajukan permintaan ke Google melalui jalur resmi (legal process), (2) periksa sharing settings — apakah dokumen dibagikan ke pihak lain, (3) periksa activity log akun untuk mengetahui siapa saja yang mengakses dokumen, (4) identifikasi device lain yang terhubung ke akun Google Drive.

*Step 4:* Korelasi evidence

Hubungkan temuan lokal dengan cloud: timeline akses file, IP address login, device list, dan sharing history.

**Jawaban:** Investigasi meliputi: forensic imaging laptop, analisis database Google Drive lokal (metadata, sync history, cache), akuisisi cloud-side evidence melalui legal process ke Google (sharing settings, activity log, connected devices), dan korelasi timeline antara bukti lokal dan cloud. Fokus utama: identifikasi dokumen apa yang dieksfiltrasi, kepada siapa dibagikan, dan dari device mana diakses.

---

## 10. Studi Kasus Komprehensif

### Solved Problem 16 ⭐⭐⭐

**Soal:** Jelaskan perbedaan pendekatan forensik untuk investigasi data breach pada masing-masing model cloud: IaaS, PaaS, dan SaaS!

**Penyelesaian:**

| Aspek | IaaS (contoh: AWS EC2) | PaaS (contoh: Heroku) | SaaS (contoh: O365) |
|-------|----------------------|---------------------|-------------------|
| **Akses investigator** | Full OS access, disk image | Application logs, database | Activity logs, export API |
| **Memory forensics** | Dapat dilakukan via snapshot | Tidak tersedia | Tidak tersedia |
| **Disk forensics** | Full disk image (EBS snapshot) | Terbatas pada app storage | Tidak tersedia |
| **Network forensics** | VPC Flow Logs, packet capture | Limited network logs | Login IP, access logs |
| **Log sources** | OS + cloud provider logs | Platform + app logs | Provider activity logs |
| **Timeline** | Jam-menit (sesuai investigator) | Bergantung retention policy | Bergantung provider |

**Jawaban:** IaaS memberikan akses forensik paling lengkap (mirip on-premise), PaaS membatasi akses ke layer aplikasi, dan SaaS paling terbatas dengan hanya activity logs dan export data. Semakin tinggi abstraksi layanan cloud, semakin terbatas kemampuan forensik tradisional.

---

### Solved Problem 17 ⭐⭐

**Soal:** Bagaimana cara menggunakan FTK Imager untuk membuka dan menganalisis file VMDK?

**Penyelesaian:**

*Step 1:* Langkah membuka VMDK di FTK Imager

1. Jalankan FTK Imager
2. File → Add Evidence Item
3. Pilih "Image File"
4. Browse ke lokasi file .vmdk
5. Klik "Finish"

*Step 2:* Analisis

Setelah VMDK di-mount, investigator dapat: (1) browse file system layaknya forensic image biasa, (2) extract file yang relevan sebagai evidence, (3) melakukan keyword search, (4) melihat deleted files pada unallocated space, (5) export individual files atau folder.

**Jawaban:** FTK Imager membuka VMDK melalui Add Evidence Item → Image File. Setelah dimount, VMDK dapat dianalisis sama dengan forensic image tradisional: browse filesystem, extract files, keyword search, dan recovery deleted files.

---

### Solved Problem 18 ⭐⭐

**Soal:** Jelaskan bagaimana DB Browser for SQLite digunakan untuk menganalisis database metadata Google Drive!

**Penyelesaian:**

*Step 1:* Persiapan

1. Copy file `metadata_sqlite_db` dari `%LOCALAPPDATA%\Google\DriveFS\<account_hash>\`
2. Buka DB Browser for SQLite
3. File → Open Database → pilih file metadata_sqlite_db

*Step 2:* Analisis tabel

Tabel yang perlu diperiksa:
- **items**: Berisi daftar file/folder dengan nama, tipe, ukuran, parent ID
- **cloud_entry**: Informasi versi cloud, checksum
- **local_entry**: Mapping file lokal

Query berguna:
```sql
-- Cari semua file (bukan folder)
SELECT * FROM items WHERE is_folder = 0;

-- Cari file dengan ekstensi tertentu
SELECT * FROM items WHERE stable_id LIKE '%.docx' OR name LIKE '%.pdf';

-- Cari file yang di-share
SELECT * FROM items WHERE shared = 1;
```

**Jawaban:** DB Browser for SQLite membuka database metadata Google Drive untuk menganalisis tabel items (daftar file), cloud_entry (versi cloud), dan local_entry (file lokal). SQL queries digunakan untuk memfilter file berdasarkan tipe, nama, atau status sharing.

---

### Solved Problem 19 ⭐⭐⭐

**Soal:** Tim forensik Lantamal V Surabaya menginvestigasi dugaan serangan terhadap infrastruktur cloud Angkatan Laut. CloudTrail log menunjukkan serangkaian event berikut dalam 24 jam:

1. `ConsoleLogin` dari IP 103.x.x.x (Indonesia) — jam 08:00
2. `CreateUser` nama "temp-admin" — jam 08:15
3. `AttachUserPolicy` policy "AdministratorAccess" ke temp-admin — jam 08:16
4. `ConsoleLogin` dari IP 185.x.x.x (Tor exit node) menggunakan temp-admin — jam 23:45
5. `CreateAccessKey` untuk temp-admin — jam 23:50
6. `GetObject` dari S3 bucket "classified-docs" — jam 23:55 sampai 02:30 (berulang kali)
7. `DeleteUser` temp-admin — jam 02:35

Analisis timeline ini dan identifikasi tahapan serangan!

**Penyelesaian:**

*Step 1:* Pemetaan timeline ke kill chain

| Waktu | Event | Fase Serangan |
|-------|-------|--------------|
| 08:00 | Login dari ID | Initial Access — akun legitimate compromised |
| 08:15-08:16 | Create temp-admin + AdminAccess | Privilege Escalation + Persistence |
| 23:45 | Login temp-admin dari Tor | Lateral Movement via new credential |
| 23:50 | Create AccessKey | Additional Persistence |
| 23:55-02:30 | GetObject berulang | Data Exfiltration |
| 02:35 | Delete temp-admin | Anti-Forensics / Covering Tracks |

*Step 2:* Analisis

Pola serangan: penyerang menggunakan akun legitimate (mungkin melalui credential theft) untuk membuat backdoor account (temp-admin) dengan hak admin penuh. Kemudian pada malam hari, menggunakan Tor untuk anonimisasi, penyerang mengakses dan mengunduh dokumen rahasia dari S3 bucket. Terakhir, penyerang menghapus akun temp-admin untuk menghilangkan jejak.

**Jawaban:** Serangan mengikuti pola: credential compromise → privilege escalation (temp-admin) → persistence (access key) → data exfiltration (S3 downloads via Tor) → covering tracks (delete user). Ini merupakan serangan terencana dengan teknik anti-forensics.

---

### Solved Problem 20 ⭐⭐

**Soal:** Jelaskan perbedaan antara forensik Docker container yang masih running, stopped, dan yang sudah dihapus!

**Penyelesaian:**

| Status | Artefak Tersedia | Tools |
|--------|-----------------|-------|
| **Running** | Filesystem, logs, memory, network, processes | docker inspect, docker logs, docker top, docker export |
| **Stopped** | Filesystem, logs, metadata (tanpa memory/network aktif) | docker inspect, docker logs, docker export |
| **Deleted** | Kemungkinan residual di overlay2, volume data | Manual analysis di /var/lib/docker, volume inspection |

**Jawaban:** Running container memiliki artefak paling lengkap (termasuk memory dan network). Stopped container kehilangan data volatile tetapi filesystem dan logs masih ada. Deleted container memerlukan analisis manual pada Docker storage layer dan volume yang mungkin masih tersisa.

---

### Solved Problem 21 ⭐⭐

**Soal:** Seorang investigator menemukan file `.dropbox.cache` berisi 15 file yang dihapus dari akun Dropbox tersangka. Bagaimana cara memvalidasi bahwa file-file tersebut benar dari akun tersangka dan bukan cache dari pengguna lain?

**Penyelesaian:**

*Step 1:* Validasi kepemilikan

Metode validasi: (1) korelasikan metadata file di cache dengan informasi di database Dropbox (`config.dbx`, `filecache.db`), (2) periksa file path yang menunjukkan berada di bawah folder Dropbox user profile tersangka, (3) periksa NTFS ownership dan security descriptors, (4) korelasikan timestamps file dengan activity log akun Dropbox (jika tersedia).

*Step 2:* Cross-validation

Bukti pendukung: (1) Windows Registry entries untuk Dropbox yang menunjukkan akun email tersangka, (2) Dropbox `config.dbx` berisi user ID dan email, (3) $MFT entries yang menunjukkan file path lengkap di bawah user profile tersangka.

**Jawaban:** Validasi melalui: korelasi metadata cache dengan database Dropbox lokal, verifikasi NTFS ownership, Registry entries yang menunjukkan akun tersangka, dan cross-reference timestamps dengan Dropbox activity log. Semua bukti harus menunjuk pada satu akun user yang sama.

---

### Solved Problem 22 ⭐⭐⭐

**Soal:** Desain prosedur forensik cloud yang komprehensif untuk skenario berikut: Pusat Data dan Informasi Kementerian Pertahanan mendeteksi anomali pada layanan Azure yang mereka gunakan. Azure Security Center menunjukkan adanya akses tidak sah ke Azure Blob Storage yang berisi dokumen strategis pertahanan.

**Penyelesaian:**

*Step 1:* Incident Response dan Containment

1. Aktifkan incident response team dan notifikasi BSSN
2. Isolasi resource yang terkompromi tanpa menghancurkan bukti (revoke access keys, bukan delete)
3. Enable diagnostic logging pada semua resource terkait jika belum aktif
4. Dokumentasikan state awal Security Center alerts

*Step 2:* Evidence Collection

1. Export Azure Activity Log untuk periode yang relevan
2. Export Sign-in Logs dan Audit Logs dari Azure AD
3. Ambil Azure Blob Storage access logs
4. Capture network traffic melalui Azure Network Watcher
5. Snapshot VM yang terkait (jika ada)
6. Export Azure Security Center alerts dan recommendations

*Step 3:* Analysis

1. Timeline reconstruction dari semua log sources
2. Identifikasi initial access vector (stolen credential, misconfiguration, vulnerability)
3. Trace lateral movement melalui subscription dan resource group
4. Identifikasi data apa yang diakses atau dieksfiltrasi
5. Cari persistence mechanisms (new users, modified roles, backdoor access)

*Step 4:* Reporting dan Remediation

1. Dokumentasikan chain of custody untuk semua bukti
2. Buat timeline komprehensif dari seluruh insiden
3. Identifikasi dampak dan klasifikasi data yang terdampak
4. Rekomendasikan perbaikan keamanan

**Jawaban:** Prosedur meliputi empat fase: (1) containment tanpa menghancurkan bukti, (2) pengumpulan bukti dari Azure logs (Activity, Sign-in, Audit, Blob access), (3) analisis timeline dan attack path, (4) dokumentasi dan rekomendasi. Kunci: preservasi bukti harus dilakukan segera karena cloud logs memiliki retention period terbatas.

---

## Supplementary Problems

*Tidak ada supplementary problems tambahan untuk modul ini.*

---

## Ringkasan

| Topik | Poin Kunci |
|-------|-----------|
| **Cloud Models** | IaaS → akses forensik terluas; PaaS → akses terbatas; SaaS → paling terbatas |
| **Tantangan Utama** | Multi-tenancy, volatilitas, distribusi data, yurisdiksi |
| **Cloud Storage** | Dropbox, Google Drive, OneDrive menyimpan artefak lokal yang bernilai forensik |
| **VM Forensics** | VMDK/VHD dapat dianalisis dengan FTK Imager; memory snapshot dengan Volatility |
| **Container** | Docker export untuk filesystem; inspect untuk metadata; logs untuk aktivitas |
| **Cloud Logs** | CloudTrail (AWS), Activity Log (Azure), Audit Logs (GCP) — sumber utama bukti |
| **Akuisisi** | Snapshot-based paling komprehensif; client-based sebagai fallback |
| **Hukum** | PP 71/2019, UU PDP, MLAT untuk akses lintas batas |

---

## Referensi

1. Casey, E. (2022). *Digital Evidence and Computer Crime*. Chapter 15
2. Johansen, G. (2020). *Digital Forensics and Incident Response*. Chapter 11
3. NIST SP 800-144: Guidelines on Security and Privacy in Public Cloud Computing
4. Cloud Security Alliance. (2020). *Cloud Forensics Guidance*
5. Dykstra, J. & Sherman, A. (2012). "Acquiring forensic evidence from infrastructure-as-a-service cloud computing". *Digital Investigation*
6. Zawoad, S. & Hasan, R. (2015). "Digital Forensics in the Age of Big Data: Challenges, Approaches, and Opportunities". *IEEE BigData Congress*

---

## License / Lisensi

**Lisensi:** Materi ini dilindungi di bawah lisensi Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0).

**© 2025 Universitas Pertahanan RI — Program Studi Teknologi Informasi Pertahanan**
