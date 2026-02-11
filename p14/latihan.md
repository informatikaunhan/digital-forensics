# Latihan Pertemuan 14: Forensik Cloud Computing

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 14  
**Topik:** Forensik Cloud Computing  
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
Model layanan cloud yang memberikan akses forensik PALING lengkap karena pengguna mengontrol sistem operasi adalah:

A. SaaS (Software as a Service)  
B. PaaS (Platform as a Service)  
C. IaaS (Infrastructure as a Service)  
D. FaaS (Function as a Service)  
E. DaaS (Desktop as a Service)

---

### Soal 2
Tantangan utama forensik di lingkungan cloud yang disebabkan oleh beberapa pelanggan menggunakan infrastruktur fisik yang sama disebut:

A. Data volatility  
B. Multi-tenancy  
C. Load balancing  
D. Auto-scaling  
E. Data replication

---

### Soal 3
Lokasi database metadata Google Drive pada sistem Windows adalah:

A. `%APPDATA%\Google\DriveFS\metadata.db`  
B. `%LOCALAPPDATA%\Google\DriveFS\<hash>\metadata_sqlite_db`  
C. `%USERPROFILE%\Google Drive\metadata.db`  
D. `%PROGRAMFILES%\Google\DriveFS\config.db`  
E. `%TEMP%\Google\DriveFS\metadata_sqlite_db`

---

### Soal 4
Format disk virtual machine yang digunakan oleh VMware adalah:

A. VHD  
B. QCOW2  
C. VDI  
D. VMDK  
E. VHDX

---

### Soal 5
Fitur OneDrive yang menggunakan NTFS reparse points dan menyebabkan file di File Explorer belum tentu memiliki konten lokal disebut:

A. Smart Sync  
B. Selective Sync  
C. Files On-Demand  
D. Stream Mode  
E. Cloud Placeholder

---

### Soal 6
Perintah Docker yang digunakan untuk mengekspor seluruh filesystem container menjadi file tar adalah:

A. `docker save`  
B. `docker backup`  
C. `docker export`  
D. `docker commit`  
E. `docker archive`

---

### Soal 7
Layanan AWS yang mencatat semua API calls dalam akun dan menjadi sumber utama bukti forensik cloud adalah:

A. AWS GuardDuty  
B. AWS CloudTrail  
C. AWS CloudWatch  
D. AWS Inspector  
E. AWS Config

---

### Soal 8
File berekstensi `.vmem` pada VMware berisi informasi tentang:

A. Konfigurasi hardware virtual machine  
B. Perubahan disk sejak snapshot  
C. Isi RAM virtual machine pada saat snapshot  
D. Metadata snapshot seperti timestamp  
E. Log aktivitas hypervisor

---

### Soal 9
Metode akuisisi cloud yang memberikan preservasi bukti PALING lengkap termasuk deleted files dan unallocated space adalah:

A. API-based acquisition  
B. Client-based acquisition  
C. Provider-assisted acquisition  
D. Snapshot-based acquisition  
E. Log-based acquisition

---

### Soal 10
Regulasi Indonesia yang mengatur kewajiban penyelenggara sistem elektronik untuk menyediakan akses data bagi penegak hukum adalah:

A. UU No. 1 Tahun 2024 tentang ITE  
B. PP No. 71 Tahun 2019  
C. UU No. 27 Tahun 2022 tentang PDP  
D. Perpres No. 95 Tahun 2018  
E. PP No. 82 Tahun 2012

---

### Soal 11
Mode sinkronisasi Google Drive yang menyimpan semua file secara penuh di lokal dan cloud, sehingga memberikan bukti forensik lebih lengkap adalah:

A. Stream mode  
B. Mirror mode  
C. Cache mode  
D. Sync mode  
E. Backup mode

---

### Soal 12
Perbedaan utama antara perintah `docker save` dan `docker export` adalah:

A. `save` untuk container, `export` untuk image  
B. `save` menyimpan image dengan layers, `export` menyimpan filesystem container  
C. `save` membuat backup encrypted, `export` membuat backup plain  
D. `save` untuk container running, `export` untuk container stopped  
E. Tidak ada perbedaan, keduanya identik

---

### Soal 13
Pada lingkungan Kubernetes, database yang menyimpan seluruh state cluster adalah:

A. PostgreSQL  
B. MongoDB  
C. etcd  
D. Redis  
E. SQLite

---

### Soal 14
Folder pada sistem Windows yang menyimpan file yang baru dihapus dari akun Dropbox sebelum purge otomatis adalah:

A. `%APPDATA%\Dropbox\trash\`  
B. `%TEMP%\Dropbox\deleted\`  
C. `%USERPROFILE%\Dropbox\.dropbox.cache\`  
D. `%LOCALAPPDATA%\Dropbox\recycle\`  
E. `%APPDATA%\Dropbox\instance1\deleted\`

---

### Soal 15
Proses hukum internasional untuk meminta akses bukti digital yang tersimpan di server cloud negara lain disebut:

A. Interpol Red Notice  
B. Mutual Legal Assistance Treaty (MLAT)  
C. International Court Order  
D. Bilateral Data Sharing Agreement  
E. Cross-Border Evidence Request

---

### Soal 16
Tool command-line yang digunakan untuk memfilter dan menganalisis CloudTrail logs dalam format JSON adalah:

A. grep  
B. awk  
C. jq  
D. sed  
E. cut

---

### Soal 17
Pada CloudTrail log, field yang menunjukkan identitas pengguna yang melakukan operasi API adalah:

A. sourceIPAddress  
B. eventName  
C. requestParameters  
D. userIdentity  
E. responseElements

---

### Soal 18
Model deployment cloud yang paling sesuai untuk menyimpan data rahasia pertahanan Indonesia karena memberikan kontrol penuh kepada organisasi adalah:

A. Public cloud  
B. Hybrid cloud  
C. Community cloud  
D. Private cloud  
E. Multi-cloud

---

### Soal 19
Langkah pertama yang harus dilakukan investigator saat menemukan VM yang diduga digunakan penyerang di cloud environment adalah:

A. Menghapus VM untuk mencegah serangan berlanjut  
B. Membuat snapshot VM untuk preservasi bukti  
C. Mematikan VM secara langsung  
D. Mengubah password akun cloud  
E. Memindahkan VM ke region lain

---

### Soal 20
Perintah Docker yang menampilkan perubahan filesystem container dibandingkan dengan base image adalah:

A. `docker inspect`  
B. `docker diff`  
C. `docker history`  
D. `docker logs`  
E. `docker stats`

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Jelaskan tiga model layanan cloud computing dan bandingkan tingkat akses forensik pada masing-masing model!

---

### Soal 2 ⭐
Sebutkan empat tantangan teknis utama dalam forensik cloud dan berikan penjelasan singkat untuk masing-masing!

---

### Soal 3 ⭐
Jelaskan perbedaan mode Stream dan Mirror pada Google Drive serta implikasi forensiknya!

---

### Soal 4 ⭐
Sebutkan lima jenis informasi forensik yang dapat diekstrak dari database Dropbox pada sistem lokal!

---

### Soal 5 ⭐⭐
Jelaskan bagaimana fitur Files On-Demand pada OneDrive mempengaruhi proses forensic imaging!

---

### Soal 6 ⭐⭐
Sebutkan lima format disk virtual machine beserta platform masing-masing, dan jelaskan langkah menganalisis VMDK menggunakan FTK Imager!

---

### Soal 7 ⭐⭐
Jelaskan perbedaan artefak forensik yang tersedia pada Docker container yang berstatus running, stopped, dan deleted!

---

### Soal 8 ⭐⭐
Jelaskan empat informasi forensik yang terkandung dalam sebuah CloudTrail log entry dan bagaimana masing-masing berguna dalam investigasi!

---

### Soal 9 ⭐⭐
Jelaskan empat metode akuisisi data dari cloud environment beserta kelebihan dan kekurangan masing-masing!

---

### Soal 10 ⭐⭐
Sebutkan lima komponen file VMware dan informasi forensik yang terkandung di masing-masing!

---

### Soal 11 ⭐⭐
Jelaskan tiga regulasi Indonesia yang relevan dengan forensik cloud computing dan cakupan masing-masing!

---

### Soal 12 ⭐⭐⭐
Jelaskan mengapa model hybrid cloud menimbulkan tantangan forensik lebih kompleks dibandingkan private cloud!

---

### Soal 13 ⭐⭐⭐
Jelaskan lima elemen tambahan yang harus ada dalam dokumentasi chain of custody untuk bukti dari cloud environment!

---

### Soal 14 ⭐⭐⭐
Seorang perwira diduga membocorkan dokumen rahasia melalui Dropbox, tetapi akun sudah di-unlink dari laptopnya. Jelaskan artefak apa saja yang masih dapat diekstrak dan bagaimana memvalidasi kepemilikan file!

---

### Soal 15 ⭐⭐⭐
Desain strategi akuisisi forensik untuk VM cloud dengan auto-scaling group di AWS yang digunakan oleh penyerang! Jelaskan langkah teknis dan preservasi bukti!

---


## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Eksfiltrasi Data via Cloud Storage di Kodam Jaya

**Skenario:**

Tim keamanan siber Kodam Jaya mendeteksi anomali bandwidth pada jaringan markas. Monitoring menunjukkan lonjakan upload traffic ke server Google Drive pada jam 22:00-02:00 selama tiga hari berturut-turut. Sumber traffic berasal dari workstation milik Kapten Arief, staf bagian operasi.

Setelah dilaporkan, Kapten Arief mengklaim hanya menggunakan Google Drive untuk menyimpan tugas kuliah. Namun, investigasi awal menemukan bahwa beberapa dokumen perencanaan operasi militer yang bersifat TERBATAS telah hilang dari server internal.

Tim forensik ditugaskan untuk menginvestigasi workstation Kapten Arief. Laptop masih menyala dan Google Drive client tampak aktif di system tray. Akun Google terhubung melalui Google Drive for Desktop dalam mode Stream.

**Pertanyaan:**

1. Jelaskan langkah preservasi bukti yang harus dilakukan pada laptop yang masih menyala! (15 poin)
2. Identifikasi artefak Google Drive yang perlu dianalisis dan lokasinya! (20 poin)
3. Bagaimana mode Stream mempengaruhi kelengkapan bukti lokal? Jelaskan strategi untuk mendapatkan bukti yang tidak tersedia secara lokal! (15 poin)
4. Jelaskan bagaimana mengkorelasikan bukti lokal dengan bukti dari cloud untuk membangun timeline! (20 poin)
5. Bagaimana cara membuktikan bahwa dokumen militer yang hilang memang diunggah ke Google Drive oleh Kapten Arief? (15 poin)
6. Sebutkan regulasi yang relevan dan prosedur hukum yang diperlukan! (15 poin)

**Total: 100 poin**

---

### Studi Kasus 2: Investigasi Serangan APT pada Cloud Infrastruktur Kemhan

**Skenario:**

Pusat Data dan Informasi (Pusdatin) Kementerian Pertahanan menggunakan layanan Azure untuk hosting beberapa aplikasi non-klasifikasi. Azure Security Center mendeteksi aktivitas mencurigakan berikut dalam 48 jam terakhir:

**Log Azure Activity:**
```
Hari 1 - 14:00: Sign-in dari IP 103.x.x.x (Jakarta) - user: admin.pusdatin
Hari 1 - 14:30: CreateRoleAssignment - user "ext-consultant" diberi role Contributor
Hari 1 - 22:00: Sign-in dari IP 45.x.x.x (VPN asing) - user: ext-consultant
Hari 1 - 22:15: Create VM (Standard_D4s_v3) di resource group "temp-rg"
Hari 1 - 22:30: Create Storage Account "tempbackup2025"
Hari 1 - 23:00-04:00: Multiple ListBlobs dan GetBlob pada storage "kemhan-docs"
Hari 2 - 04:15: CopyBlob dari "kemhan-docs" ke "tempbackup2025"
Hari 2 - 04:30: Delete VM dan resource group "temp-rg"
Hari 2 - 04:35: Delete RoleAssignment untuk "ext-consultant"
```

Tim forensik diminta menganalisis insiden ini dan mempersiapkan laporan untuk pimpinan Kemhan.

**Pertanyaan:**

1. Petakan setiap event pada timeline ke fase cyber kill chain dan identifikasi taktik penyerang! (20 poin)
2. Identifikasi Indicators of Compromise (IoC) dari log yang tersedia! (15 poin)
3. Jelaskan langkah pengumpulan bukti forensik dari Azure environment! (20 poin)
4. Analisis mengapa penyerang membuat VM baru dan storage account terpisah! (15 poin)
5. Jelaskan strategi untuk mengidentifikasi data apa saja yang berhasil dieksfiltrasi! (15 poin)
6. Susun rekomendasi keamanan untuk mencegah insiden serupa! (15 poin)

**Total: 100 poin**

---


## Kunci Jawaban

### Bagian A: Pilihan Ganda


| No | Jawaban | Penjelasan Singkat |
|----|---------|-------------------|
| 1 | **C** | IaaS memberikan kontrol penuh atas OS sehingga artefak forensik paling lengkap |
| 2 | **B** | Multi-tenancy: beberapa tenant berbagi infrastruktur fisik, menyulitkan isolasi bukti |
| 3 | **B** | Database metadata Google Drive ada di `%LOCALAPPDATA%\Google\DriveFS\<hash>\metadata_sqlite_db` |
| 4 | **D** | VMDK (Virtual Machine Disk) adalah format disk VMware |
| 5 | **C** | Files On-Demand menggunakan NTFS reparse points sebagai placeholder file cloud |
| 6 | **C** | `docker export` mengekspor filesystem container menjadi tar archive |
| 7 | **B** | CloudTrail mencatat semua API calls — sumber utama bukti forensik AWS |
| 8 | **C** | File .vmem berisi memory snapshot (isi RAM) VM saat snapshot dibuat |
| 9 | **D** | Snapshot-based menangkap state lengkap disk termasuk deleted files dan unallocated space |
| 10 | **B** | PP No. 71/2019 mengatur kewajiban penyediaan akses data bagi penegak hukum |
| 11 | **B** | Mirror mode menyimpan semua file secara penuh di lokal dan cloud |
| 12 | **B** | `docker save` menyimpan image + layers; `docker export` menyimpan filesystem container flat |
| 13 | **C** | etcd adalah key-value store yang menyimpan seluruh state cluster Kubernetes |
| 14 | **C** | `.dropbox.cache` menyimpan file yang baru dihapus sebelum purge otomatis |
| 15 | **B** | MLAT adalah mekanisme hukum internasional untuk permintaan bukti lintas batas |
| 16 | **C** | jq adalah command-line JSON processor untuk memfilter dan menganalisis data JSON |
| 17 | **D** | Field userIdentity berisi informasi siapa yang melakukan API call |
| 18 | **D** | Private cloud memberikan kontrol penuh dan eksklusivitas untuk organisasi |
| 19 | **B** | Snapshot VM menjadi prioritas pertama untuk preservasi state sebelum perubahan lain |
| 20 | **B** | `docker diff` menampilkan file yang ditambahkan (A), diubah (C), atau dihapus (D) dari base image |

---


### Bagian B: Uraian


#### Jawaban Soal 1 ⭐

| Model | Kontrol User | Akses Forensik |
|-------|-------------|---------------|
| **IaaS** | OS, aplikasi, data | Paling lengkap: VM image, memory, OS logs |
| **PaaS** | Aplikasi, data | Terbatas: app logs dan database |
| **SaaS** | Data saja | Paling terbatas: activity logs dan export data |

Semakin tinggi abstraksi layanan, semakin terbatas kemampuan forensik tradisional.

---

#### Jawaban Soal 2 ⭐

| Tantangan | Penjelasan |
|-----------|-----------|
| **Multi-tenancy** | Beberapa tenant berbagi infrastruktur fisik — sulit mengisolasi bukti |
| **Volatilitas & Elastisitas** | VM dibuat/dihancurkan otomatis oleh auto-scaling, bukti bisa hilang |
| **Distributed Architecture** | Data tersebar di beberapa data center lintas negara |
| **Encryption** | Data terenkripsi at-rest dan in-transit, perlu akses ke key management |

---

#### Jawaban Soal 3 ⭐

| Aspek | Stream | Mirror |
|-------|--------|--------|
| **Penyimpanan** | File di cloud, download on-demand | File penuh di lokal + cloud |
| **Forensik** | Hanya file yang pernah dibuka ada di cache lokal | Semua file tersedia di lokal |

Implikasi: Mode stream menyebabkan forensic imaging tidak mendapatkan konten file yang belum pernah diakses. Investigator perlu akses ke akun Google untuk bukti lengkap.

---

#### Jawaban Soal 4 ⭐

Lima informasi forensik dari database Dropbox lokal:
1. **Account Info** — email, display name, user ID
2. **File Metadata** — nama, ukuran, hash, timestamps
3. **Sync History** — riwayat sinkronisasi termasuk file terhapus
4. **Shared Links** — file yang dibagikan dan penerima
5. **Device Info** — nama perangkat, OS, waktu koneksi terakhir

---

#### Jawaban Soal 5 ⭐⭐

Files On-Demand menyebabkan file yang tampil di File Explorer belum tentu memiliki konten lokal. File "online-only" hanya berupa placeholder (NTFS reparse point). Saat forensic imaging, file tersebut tidak mengandung konten aktual — hanya metadata. Investigator harus memeriksa NTFS attributes untuk mengidentifikasi status file dan mengakses akun OneDrive untuk mendapatkan konten lengkap.

---

#### Jawaban Soal 6 ⭐⭐

**Lima format disk VM:**

| Format | Platform |
|--------|----------|
| VMDK | VMware |
| VHD/VHDX | Microsoft Hyper-V |
| QCOW2 | KVM/QEMU |
| VDI | VirtualBox |
| OVA/OVF | Multi-platform |

**Langkah analisis VMDK dengan FTK Imager:**
1. File → Add Evidence Item
2. Pilih "Image File"
3. Browse ke file .vmdk
4. Klik Finish — FTK mount VMDK
5. Analisis: browse filesystem, keyword search, file recovery

---

#### Jawaban Soal 7 ⭐⭐

| Status | Artefak Tersedia |
|--------|-----------------|
| **Running** | Filesystem, logs, memory, network connections, proses aktif — paling lengkap |
| **Stopped** | Filesystem, logs, metadata — tanpa data volatile (memory, network) |
| **Deleted** | Hanya residual di overlay2 dan volumes yang belum dihapus — minimal |

Prioritas: akuisisi segera sebelum container dihapus, karena data volatile hilang saat container dihentikan.

---

#### Jawaban Soal 8 ⭐⭐

| Field | Informasi | Kegunaan Forensik |
|-------|-----------|------------------|
| **userIdentity** | Siapa yang melakukan operasi | Identifikasi pelaku |
| **eventName** | Jenis operasi API | Mengetahui aksi yang dilakukan |
| **eventTime** | Waktu operasi | Membangun timeline serangan |
| **sourceIPAddress** | IP asal request | Melacak lokasi dan anomali (misal Tor exit node) |

---

#### Jawaban Soal 9 ⭐⭐

| Metode | Kelebihan | Kekurangan |
|--------|-----------|------------|
| **Snapshot-based** | Preservasi state lengkap termasuk deleted files | Perlu akses ke akun cloud |
| **API-based** | Dapat diautomasi, scripting | Terbatas pada data yang di-expose API |
| **Client-based** | Tidak perlu akses cloud provider | Data mungkin tidak lengkap |
| **Provider-assisted** | Akses ke server-side logs | Proses hukum panjang |

---

#### Jawaban Soal 10 ⭐⭐

| File | Fungsi | Info Forensik |
|------|--------|--------------|
| `.vmx` | Konfigurasi VM | Hardware settings, network, nama VM |
| `.vmdk` | Base disk image | Seluruh filesystem guest OS |
| `-s00x.vmdk` | Delta disk | Perubahan sejak snapshot |
| `.vmsn` | Snapshot metadata | Timestamp, deskripsi snapshot |
| `.vmem` | Memory snapshot | RAM: proses, koneksi jaringan, credentials |

---

#### Jawaban Soal 11 ⭐⭐

| Regulasi | Cakupan |
|----------|---------|
| **PP No. 71/2019** | Kewajiban penyelenggara sistem elektronik menyediakan akses data bagi penegak hukum |
| **UU No. 27/2022 (UU PDP)** | Mengatur transfer data pribadi lintas batas |
| **Permenhan Keamanan Informasi** | Pengelolaan data pertahanan dan kedaulatan data nasional |

---

#### Jawaban Soal 12 ⭐⭐⭐

Hybrid cloud lebih kompleks karena:
1. **Data tersebar** di dua environment (on-premise dan public cloud) dengan kontrol akses berbeda
2. **Koordinasi ganda** — perlu akses internal dan kerjasama provider untuk public cloud
3. **Yurisdiksi berbeda** — data di public cloud mungkin berada di negara lain
4. **Timestamp sync** — perlu sinkronisasi waktu antara dua environment
5. **Chain of custody lebih rumit** karena melibatkan pihak ketiga (provider)

---

#### Jawaban Soal 13 ⭐⭐⭐

Lima elemen tambahan chain of custody untuk cloud evidence:
1. **Cloud Service Info** — provider, region, account ID
2. **Acquisition Method** — metode yang digunakan (API, snapshot, client-side)
3. **Timestamp Synchronization** — verifikasi sinkronisasi waktu antar evidence source
4. **Hash Verification** — hash setiap file/snapshot yang diakuisisi
5. **Access Logs** — dokumentasi siapa yang mengakses cloud account dan kapan

---

#### Jawaban Soal 14 ⭐⭐⭐

**Artefak yang masih dapat diekstrak:**
1. Residual sync folder di `%USERPROFILE%\Dropbox\`
2. Database cache di `%APPDATA%\Dropbox\`
3. `.dropbox.cache` berisi file terhapus
4. Registry entries menunjukkan informasi instalasi dan akun
5. Prefetch files untuk Dropbox.exe
6. NTFS Journal dan thumbnail cache

**Validasi kepemilikan:**
- Korelasikan metadata cache dengan database Dropbox lokal (config.dbx berisi email dan user ID)
- Verifikasi NTFS ownership pada file
- Registry entries yang menunjukkan akun email tersangka
- Cross-reference timestamps dengan Dropbox activity log (jika tersedia dari provider)

---

#### Jawaban Soal 15 ⭐⭐⭐

**Strategi akuisisi:**
1. **Containment** — disable auto-scaling untuk mencegah penghapusan instance aktif
2. **Snapshot EBS** — buat snapshot semua EBS volume yang terkait
3. **Memory capture** — capture memory dari running instance via AWS SSM atau LiME
4. **Collect logs** — ambil CloudTrail logs dan VPC Flow Logs sebelum melewati retention period
5. **Legal hold** — ajukan permintaan ke AWS untuk membekukan instance melalui proses hukum

**Preservasi bukti:**
- Simpan semua snapshot dan logs ke S3 bucket terpisah dengan write-once policy
- Hitung hash untuk setiap artefak yang diakuisisi
- Dokumentasikan chain of custody dan timeline akuisisi

---


### Bagian C: Studi Kasus

#### Studi Kasus 1: Eksfiltrasi Data via Cloud Storage di Kodam Jaya

**1. Langkah Preservasi (15 poin)**

| Langkah | Tindakan |
|---------|----------|
| Dokumentasi visual | Foto layar, catat running programs, system tray status |
| Memory capture | RAM dump menggunakan FTK Imager atau WinPMEM — prioritas utama |
| Network state | Catat koneksi aktif (`netstat -an`), proses jaringan |
| Forensic imaging | Buat forensic image disk menggunakan FTK Imager |
| Preserve cloud state | Screenshot Google Drive settings dan sync status |

Urutan: memory → network → screenshot → disk imaging. Jangan matikan laptop sebelum memory capture selesai.

**2. Artefak Google Drive (20 poin)**

| Artefak | Lokasi | Informasi |
|---------|--------|-----------|
| Metadata DB | `%LOCALAPPDATA%\Google\DriveFS\<hash>\metadata_sqlite_db` | Daftar semua file termasuk yang dihapus |
| Content cache | `%LOCALAPPDATA%\Google\DriveFS\<hash>\content_cache\` | File yang pernah dibuka |
| Log files | `%LOCALAPPDATA%\Google\DriveFS\Logs\` | Aktivitas sync |
| Browser history | Profile browser Chrome/Edge | Aktivitas Google Drive web |
| RAM dump | Dari memory capture | Koneksi aktif, credentials, data in-transit |

**3. Dampak Mode Stream (15 poin)**

Mode Stream hanya menyimpan cache file yang pernah diakses. Dokumen yang diupload tetapi tidak dibuka lokal tidak memiliki konten di cache.

Strategi untuk bukti yang tidak tersedia lokal:
- Ajukan legal request ke Google melalui PP 71/2019 untuk mendapatkan file list, sharing settings, dan activity log
- Analisis metadata_sqlite_db yang tetap mencatat metadata semua file (nama, ukuran, timestamps) meskipun konten tidak tersedia lokal
- Periksa browser cache jika file pernah dibuka via browser

**4. Korelasi Bukti (20 poin)**

| Sumber Bukti | Data | Korelasi |
|-------------|------|----------|
| Network logs Kodam | Upload traffic ke Google IP (22:00-02:00) | Konfirmasi waktu upload |
| Google Drive metadata_sqlite_db | File timestamps, nama dokumen | Identifikasi file yang diunggah |
| Server internal logs | File access logs pada dokumen TERBATAS | Waktu akses dokumen sebelum upload |
| Google activity log (via legal) | Login history, file upload history | Konfirmasi akun dan aktivitas cloud |

Timeline dibangun dengan menghubungkan: akses dokumen di server → transfer ke laptop → upload ke Google Drive.

**5. Pembuktian Upload Dokumen (15 poin)**

Bukti yang diperlukan:
- Metadata database Google Drive yang menunjukkan file dengan nama/ukuran sama dengan dokumen yang hilang
- Hash comparison antara dokumen asli di server dan file di Google Drive (jika tersedia via legal request)
- Network traffic logs yang menunjukkan upload ke IP Google Drive pada waktu yang sesuai
- Google Drive activity log yang mencatat upload event dari IP workstation Kapten Arief

**6. Regulasi dan Prosedur Hukum (15 poin)**

| Regulasi | Penerapan |
|----------|-----------|
| PP No. 71/2019 | Dasar hukum permintaan data ke Google Indonesia |
| UU No. 27/2022 (PDP) | Mengatur akses data pribadi untuk penegakan hukum |
| KUHP Militer | Dasar penuntutan pelanggaran keamanan data militer |
| Permenhan keamanan informasi | Pelanggaran prosedur pengelolaan data TERBATAS |

Prosedur: koordinasi dengan Dinas Hukum TNI, penyusunan surat permintaan resmi, dan dokumentasi chain of custody.

---

#### Studi Kasus 2: Investigasi Serangan APT pada Cloud Infrastruktur Kemhan

**1. Pemetaan Kill Chain (20 poin)**

| Waktu | Event | Fase | Taktik |
|-------|-------|------|--------|
| Hari 1, 14:00 | Sign-in admin.pusdatin | Initial Access | Credential compromise (akun legitimate) |
| Hari 1, 14:30 | CreateRoleAssignment ext-consultant | Persistence + Privilege Escalation | Buat akun backdoor dengan Contributor role |
| Hari 1, 22:00 | Sign-in ext-consultant (VPN asing) | Lateral Movement | Gunakan credential baru dari lokasi berbeda |
| Hari 1, 22:15-22:30 | Create VM + Storage Account | Resource Development | Siapkan infrastruktur untuk exfiltration |
| Hari 1, 23:00-04:00 | ListBlobs + GetBlob | Discovery + Collection | Enumerasi dan kumpulkan data target |
| Hari 2, 04:15 | CopyBlob | Exfiltration | Transfer data ke storage penyerang |
| Hari 2, 04:30-04:35 | Delete VM, RG, RoleAssignment | Defense Evasion | Hapus jejak serangan |

**2. Indicators of Compromise (15 poin)**

| Tipe IoC | Nilai | Keterangan |
|----------|-------|------------|
| IP Address | 45.x.x.x | VPN asing — sumber login ext-consultant |
| User Account | ext-consultant | Akun backdoor yang dibuat penyerang |
| Resource | temp-rg, tempbackup2025 | Infrastruktur serangan |
| Pola Waktu | 22:00-04:00 | Aktivitas di luar jam kerja |
| Behavior | CopyBlob lintas storage account | Pola exfiltration data |

**3. Pengumpulan Bukti (20 poin)**

| Langkah | Sumber Bukti | Tindakan |
|---------|-------------|----------|
| 1 | Azure Activity Log | Export seluruh log untuk periode 48+ jam |
| 2 | Azure AD Sign-in Logs | Export login history termasuk IP, location, device |
| 3 | Azure AD Audit Logs | Export perubahan role dan permission |
| 4 | Storage Access Logs | Export blob access logs dari "kemhan-docs" |
| 5 | Network Watcher | Capture flow logs dan network traffic |
| 6 | Security Center Alerts | Export semua alert dan recommendations |

Semua log harus di-hash dan didokumentasikan dalam chain of custody.

**4. Analisis Taktik Penyerang (15 poin)**

Penyerang membuat VM baru dan storage account terpisah karena:
- **VM baru** digunakan sebagai staging server di dalam Azure environment — traffic antar Azure service lebih cepat dan tidak melewati firewall perimeter
- **Storage account terpisah** sebagai tujuan CopyBlob — data bisa diakses dari luar setelah exfiltration selesai tanpa mengakses storage asli lagi
- **Penghapusan setelahnya** menghilangkan jejak infrastruktur serangan
- Pola ini menunjukkan penyerang memiliki pengetahuan mendalam tentang Azure — kemungkinan APT atau insider

**5. Identifikasi Data yang Dieksfiltrasi (15 poin)**

Strategi:
- Analisis Storage Access Logs dari "kemhan-docs" — identifikasi blob apa saja yang di-ListBlobs dan GetBlob pada rentang 23:00-04:00
- Periksa CopyBlob logs untuk mengetahui file spesifik yang disalin ke tempbackup2025
- Meskipun storage account tempbackup2025 sudah dihapus, Azure soft-delete mungkin masih menyimpan data selama retention period
- Hubungi Microsoft melalui jalur resmi untuk recovery data dari deleted storage account
- Bandingkan daftar file yang diakses dengan klasifikasi data untuk menilai dampak

**6. Rekomendasi Keamanan (15 poin)**

| Area | Rekomendasi |
|------|------------|
| **Access Control** | Implementasi MFA untuk semua akun admin, Privileged Identity Management (PIM) |
| **Monitoring** | Alert real-time untuk CreateRoleAssignment, login dari IP asing, pembuatan resource di luar jam kerja |
| **Network** | Batasi sign-in hanya dari IP range Indonesia, conditional access policies |
| **Data Protection** | Aktifkan soft-delete dan versioning pada semua storage account, DLP policies |
| **Incident Response** | Perpanjang log retention period, siapkan playbook forensik cloud |

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
