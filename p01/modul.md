# Modul 01: Pengantar Forensik Digital dalam Konteks Militer

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 01  
**Topik:** Pengantar Forensik Digital dalam Konteks Militer  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Menjelaskan definisi dan ruang lingkup forensik digital secara komprehensif
2. Menguraikan evolusi kejahatan siber dan ancaman terhadap sistem pertahanan nasional
3. Membedakan jenis-jenis kejahatan siber berdasarkan sumber ancaman (internal vs eksternal)
4. Mengidentifikasi karakteristik bukti digital dan sumber-sumber potensialnya
5. Menjelaskan konsep forensic readiness dalam konteks organisasi militer
6. Memahami hubungan forensik digital dengan Cyber Intelligence dan Information Operations
7. Menerapkan prinsip etika profesional dan memahami regulasi terkait forensik digital di Indonesia

---

## 1. Pendahuluan Forensik Digital

### 1.1 Definisi Forensik Digital

> **Forensik Digital (Digital Forensics)** adalah cabang ilmu forensik yang berfokus pada identifikasi, pengumpulan, preservasi, analisis, dan penyajian bukti digital dalam format yang dapat diterima secara hukum untuk keperluan investigasi dan proses peradilan.

Forensik digital merupakan perpaduan antara ilmu komputer, hukum, dan teknik investigasi. Dalam konteks militer, forensik digital menjadi komponen kritis dalam operasi intelijen siber dan pertahanan jaringan.

| Aspek | Forensik Tradisional | Forensik Digital |
|-------|---------------------|------------------|
| **Objek** | Bukti fisik (sidik jari, DNA) | Bukti elektronik (file, log, metadata) |
| **Lokasi** | TKP fisik | Sistem komputer, jaringan, cloud |
| **Preservasi** | Penyegelan fisik | Imaging dan hashing |
| **Analisis** | Laboratorium forensik | Workstation forensik digital |
| **Volatilitas** | Relatif stabil | Sangat mudah berubah/hilang |

#### 1.1.1 Ruang Lingkup Forensik Digital

Forensik digital mencakup beberapa subdisiplin:

1. **Computer Forensics**: Analisis komputer dan media penyimpanan
2. **Network Forensics**: Investigasi lalu lintas jaringan (dibahas [Pertemuan 10](../p10/modul.md))
3. **Mobile Forensics**: Analisis perangkat mobile (dibahas [Pertemuan 15](../p15/modul.md))
4. **Memory Forensics**: Analisis volatile data dan RAM (dibahas [Pertemuan 05](../p05/modul.md))
5. **Malware Forensics**: Analisis perangkat lunak berbahaya (dibahas [Pertemuan 11](../p11/modul.md))
6. **Cloud Forensics**: Investigasi lingkungan cloud (dibahas [Pertemuan 14](../p14/modul.md))

![Ruang Lingkup Forensik Digital](images/p01-01-digital-forensics-scope.png)

*Gambar 1.1: Ruang lingkup dan subdisiplin forensik digital*

#### Solved Problem 1 ⭐

**Soal:** Sebutkan tiga perbedaan utama antara forensik tradisional dan forensik digital!

**Penyelesaian:**

*Step 1:* Identifikasi aspek perbandingan yang relevan

*Step 2:* Analisis karakteristik masing-masing

| Aspek | Forensik Tradisional | Forensik Digital |
|-------|---------------------|------------------|
| **1. Volatilitas** | Bukti fisik relatif stabil dan tidak mudah berubah | Bukti digital sangat volatile, dapat berubah atau hilang dalam hitungan detik |
| **2. Replikasi** | Sulit membuat salinan sempurna | Dapat membuat salinan identik (bit-by-bit copy) |
| **3. Lokasi** | Terbatas pada lokasi fisik TKP | Dapat tersebar di berbagai lokasi (lokal, cloud, jaringan) |

**Jawaban:** Tiga perbedaan utama adalah: (1) Tingkat volatilitas bukti digital yang jauh lebih tinggi, (2) Kemampuan untuk membuat salinan identik pada bukti digital, dan (3) Lokasi bukti digital yang dapat tersebar di berbagai sistem dan lokasi geografis.

---

### 1.2 Sejarah Perkembangan Forensik Digital

Forensik digital berkembang seiring dengan evolusi teknologi informasi dan kejahatan siber:

| Era | Periode | Perkembangan |
|-----|---------|--------------|
| **Awal** | 1980-an | Munculnya virus komputer pertama, investigasi ad-hoc |
| **Formalisasi** | 1990-an | Pembentukan unit forensik komputer kepolisian |
| **Standarisasi** | 2000-an | Pengembangan tools dan metodologi standar |
| **Modern** | 2010-sekarang | Cloud forensics, IoT forensics, AI-assisted analysis |

> **Milestone Penting:**
> - 1984: FBI membentuk Computer Analysis and Response Team (CART)
> - 1998: Scientific Working Group on Digital Evidence (SWGDE) didirikan
> - 2004: ISO/IEC 27037 dikembangkan sebagai standar internasional
> - 2016: NIST merilis SP 800-86 untuk panduan forensik digital

#### 1.2.1 Forensik Digital di Indonesia

Di Indonesia, forensik digital berkembang pesat setelah:
- Pemberlakuan UU No. 11 Tahun 2008 tentang ITE
- Pembaruan UU No. 1 Tahun 2024 tentang ITE
- Pembentukan Pusat Forensik Digital di POLRI dan lembaga terkait

#### Solved Problem 2 ⭐

**Soal:** Jelaskan mengapa standardisasi dalam forensik digital sangat penting!

**Penyelesaian:**

*Step 1:* Identifikasi tujuan standardisasi

*Step 2:* Analisis dampak standardisasi

**Alasan pentingnya standardisasi:**

1. **Admissibility Hukum**: Bukti yang dikumpulkan dengan prosedur standar lebih mudah diterima di pengadilan
2. **Reproducibility**: Hasil analisis dapat diverifikasi dan direproduksi oleh investigator lain
3. **Interoperabilitas**: Tools dan prosedur dapat digunakan lintas organisasi dan jurisdiksi
4. **Quality Assurance**: Menjamin kualitas dan integritas investigasi

**Jawaban:** Standardisasi penting karena menjamin bukti digital dapat diterima di pengadilan, memungkinkan verifikasi hasil oleh pihak ketiga, memfasilitasi kerjasama lintas organisasi, dan menjamin kualitas proses investigasi.

---

### 1.3 Prinsip Dasar Forensik Digital

> **Lima Prinsip Fundamental Forensik Digital:**
> 1. **Tidak Mengubah Bukti**: Setiap tindakan tidak boleh mengubah data asli
> 2. **Chain of Custody**: Dokumentasi penuh atas penanganan bukti
> 3. **Kompetensi**: Investigator harus kompeten dan terlatih
> 4. **Dokumentasi**: Semua aktivitas harus didokumentasikan
> 5. **Hukum dan Etika**: Kepatuhan terhadap hukum yang berlaku

#### 1.3.1 Prinsip Locard dalam Konteks Digital

> **Prinsip Locard (Locard's Exchange Principle)** menyatakan bahwa setiap kontak meninggalkan jejak. Dalam forensik digital, prinsip ini berarti bahwa setiap interaksi dengan sistem komputer akan meninggalkan artefak digital.

Contoh penerapan prinsip Locard dalam forensik digital:
- Mengakses website meninggalkan cache, cookies, dan history
- Menghapus file meninggalkan entry di file system journal
- Koneksi jaringan meninggalkan log di firewall dan router

#### Solved Problem 3 ⭐

**Soal:** Berikan contoh penerapan prinsip "Tidak Mengubah Bukti" dalam praktik forensik digital!

**Penyelesaian:**

*Step 1:* Pahami prinsip "Tidak Mengubah Bukti"

*Step 2:* Identifikasi teknik implementasi

**Contoh Penerapan:**

| Teknik | Implementasi |
|--------|-------------|
| **Write Blocker** | Menggunakan hardware/software write blocker saat mengakses media penyimpanan |
| **Forensic Imaging** | Membuat salinan bit-by-bit (forensic image) sebelum melakukan analisis |
| **Hash Verification** | Menghitung dan memverifikasi hash (MD5/SHA) untuk membuktikan integritas |
| **Working Copy** | Melakukan analisis pada salinan, bukan bukti asli |

**Jawaban:** Contoh penerapan prinsip ini adalah menggunakan write blocker hardware saat mengakses hard disk tersangka untuk mencegah modifikasi tidak sengaja, kemudian membuat forensic image dan memverifikasi integritasnya dengan hash sebelum melakukan analisis pada salinan tersebut.

---

## 2. Kejahatan Siber dan Ancaman Terhadap Sistem Pertahanan

### 2.1 Definisi Kejahatan Siber

> **Kejahatan Siber (Cybercrime)** adalah aktivitas kriminal yang melibatkan komputer atau jaringan komputer sebagai target, alat, atau tempat terjadinya kejahatan.

#### 2.1.1 Klasifikasi Kejahatan Siber

![Klasifikasi Kejahatan Siber](images/p01-02-cybercrime-classification.png)

*Gambar 1.2: Klasifikasi kejahatan siber berdasarkan target dan metode*

**Kategori berdasarkan peran komputer:**

| Kategori | Deskripsi | Contoh |
|----------|-----------|--------|
| **Computer as Target** | Komputer adalah sasaran serangan | Hacking, malware, DDoS |
| **Computer as Tool** | Komputer digunakan untuk melakukan kejahatan | Penipuan online, phishing |
| **Computer as Incidental** | Komputer menyimpan bukti kejahatan lain | Catatan transaksi narkoba |

### 2.2 Jenis Kejahatan Siber: Internal vs Eksternal

> **Insider Threat (Ancaman Internal)** adalah ancaman yang berasal dari individu dalam organisasi yang memiliki akses sah ke sistem dan informasi.

> **External Threat (Ancaman Eksternal)** adalah ancaman yang berasal dari luar organisasi, termasuk hackers, kelompok kriminal, dan aktor negara.

| Karakteristik | Internal Threat | External Threat |
|---------------|-----------------|-----------------|
| **Akses** | Memiliki akses sah | Harus mendapatkan akses |
| **Pengetahuan** | Memahami sistem internal | Perlu reconnaissance |
| **Deteksi** | Sulit dideteksi | Relatif lebih mudah |
| **Motivasi** | Finansial, balas dendam | Finansial, ideologis, spionase |

#### 2.2.1 Ancaman Internal (Insider Threat)

Jenis-jenis insider threat:

1. **Malicious Insider**: Secara sengaja melakukan tindakan berbahaya
2. **Negligent Insider**: Menyebabkan kerusakan karena kelalaian
3. **Compromised Insider**: Akun atau kredensial dicuri pihak luar

#### Solved Problem 4 ⭐⭐

**Soal:** Seorang administrator sistem di pangkalan militer diketahui menyalin data sensitif ke USB drive pribadi. Analisis jenis ancaman ini dan langkah forensik awal yang harus dilakukan!

**Penyelesaian:**

*Step 1:* Klasifikasi jenis ancaman

- Sumber: Internal (administrator sistem)
- Tipe: Malicious Insider atau Negligent Insider (perlu investigasi lebih lanjut)
- Akses: Memiliki akses sah ke sistem

*Step 2:* Identifikasi langkah forensik awal

1. **Preserve Evidence**:
   - Amankan USB drive sebagai bukti
   - Buat forensic image dari workstation administrator
   - Capture volatile data (RAM) jika sistem masih menyala

2. **Document Chain of Custody**:
   - Catat siapa, kapan, dan bagaimana bukti dikumpulkan
   - Foto kondisi workspace dan perangkat

3. **Collect Relevant Logs**:
   - USB insertion/removal logs
   - File access logs
   - Network activity logs
   - Authentication logs

**Jawaban:** Ini adalah kasus **Insider Threat** dengan potensi **Malicious Insider**. Langkah forensik awal: (1) Mengamankan USB drive dan membuat forensic image workstation, (2) Mendokumentasikan chain of custody, (3) Mengumpulkan log USB, file access, dan authentication untuk analisis lebih lanjut.

---

### 2.3 Ancaman Siber Terhadap Pertahanan Nasional

Sistem pertahanan menghadapi berbagai kategori ancaman siber:

#### 2.3.1 Advanced Persistent Threat (APT)

> **Advanced Persistent Threat (APT)** adalah serangan siber yang dilakukan oleh aktor dengan kemampuan tinggi (biasanya aktor negara) dengan tujuan spesifik dan bersifat persisten dalam jangka panjang.

Karakteristik APT:
- **Advanced**: Menggunakan teknik canggih dan zero-day exploits
- **Persistent**: Bertahan dalam sistem untuk waktu lama
- **Threat**: Memiliki niat dan kemampuan untuk menyebabkan kerusakan

#### 2.3.2 Kategori Aktor Ancaman

| Aktor | Motivasi | Kemampuan | Target |
|-------|----------|-----------|--------|
| **Script Kiddies** | Kesenangan | Rendah | Oportunistik |
| **Hacktivists** | Ideologis | Sedang | Organisasi tertentu |
| **Cybercriminals** | Finansial | Tinggi | Data berharga |
| **Nation-State Actors** | Spionase, sabotase | Sangat tinggi | Infrastruktur kritis |

#### Solved Problem 5 ⭐⭐

**Soal:** Jelaskan mengapa APT sangat berbahaya bagi sistem pertahanan nasional dan sebutkan indikator yang dapat menunjukkan adanya APT!

**Penyelesaian:**

*Step 1:* Analisis mengapa APT berbahaya

1. **Stealth**: APT dirancang untuk tidak terdeteksi dalam waktu lama
2. **Persistence**: Dapat bertahan meski sebagian ditemukan
3. **Data Exfiltration**: Mencuri data sensitif secara kontinu
4. **Strategic Impact**: Dapat mempengaruhi keputusan strategis
5. **Sabotage Potential**: Kemampuan merusak sistem kritis

*Step 2:* Identifikasi indikator APT (Indicators of Compromise/IOC)

| Kategori | Indikator |
|----------|-----------|
| **Network** | Unusual outbound traffic, beaconing patterns |
| **Host** | Unexpected processes, suspicious scheduled tasks |
| **User** | Login anomalies, privilege escalation attempts |
| **File** | Unknown executables, modified system files |

**Jawaban:** APT berbahaya karena sifatnya yang tersembunyi, persisten, dan dapat mencuri data sensitif dalam jangka panjang. Indikator APT meliputi: traffic jaringan abnormal keluar, pola komunikasi berkala (beaconing), proses tidak dikenal, login anomali, dan file sistem yang dimodifikasi.

---

### 2.4 Cyber Kill Chain

> **Cyber Kill Chain** adalah model yang menggambarkan tahapan serangan siber, dikembangkan oleh Lockheed Martin untuk membantu memahami dan menghentikan serangan.

![Cyber Kill Chain](images/p01-03-cyber-kill-chain.png)

*Gambar 1.3: Tahapan Cyber Kill Chain*

| Tahap | Deskripsi | Bukti Forensik |
|-------|-----------|----------------|
| **1. Reconnaissance** | Pengumpulan informasi target | Log DNS, social media traces |
| **2. Weaponization** | Pembuatan payload berbahaya | - (terjadi di sisi penyerang) |
| **3. Delivery** | Pengiriman payload ke target | Email logs, download history |
| **4. Exploitation** | Eksploitasi kerentanan | Event logs, crash dumps |
| **5. Installation** | Instalasi backdoor/malware | File system artifacts, registry |
| **6. C2** | Komunikasi dengan command server | Network traffic, beaconing |
| **7. Actions** | Pencapaian tujuan akhir | Data exfiltration logs, file access |

#### Solved Problem 6 ⭐⭐

**Soal:** Pada tahap mana dalam Cyber Kill Chain forensik digital paling efektif untuk mengumpulkan bukti? Jelaskan alasannya!

**Penyelesaian:**

*Step 1:* Analisis setiap tahap dari perspektif forensik

| Tahap | Ketersediaan Bukti | Alasan |
|-------|-------------------|--------|
| Reconnaissance | Rendah | Terjadi di luar infrastruktur target |
| Weaponization | Tidak ada | Terjadi di sistem penyerang |
| Delivery | Sedang | Email logs, download records |
| Exploitation | Tinggi | Event logs, memory artifacts |
| Installation | Sangat Tinggi | File system, registry, persistence |
| C2 | Tinggi | Network logs, memory |
| Actions | Tinggi | Access logs, data movement |

*Step 2:* Tentukan tahap optimal

**Jawaban:** Forensik digital paling efektif pada tahap **Installation** dan **Command & Control** karena:
1. **Installation**: Meninggalkan artefak permanen di file system, registry, dan scheduled tasks
2. **C2**: Menghasilkan network traffic yang dapat dianalisis
3. Kedua tahap ini memiliki rasio signal-to-noise yang baik dan bukti yang lebih persisten

---

## 3. Bukti Digital

### 3.1 Definisi dan Karakteristik Bukti Digital

> **Bukti Digital (Digital Evidence)** adalah informasi yang disimpan atau ditransmisikan dalam format digital yang dapat digunakan dalam proses investigasi atau peradilan.

Karakteristik bukti digital:

| Karakteristik | Deskripsi | Implikasi Forensik |
|---------------|-----------|-------------------|
| **Latent** | Tidak terlihat tanpa alat khusus | Memerlukan tools forensik |
| **Fragile** | Mudah dimodifikasi atau dihancurkan | Preservasi segera diperlukan |
| **Crossing Jurisdictions** | Dapat melewati batas geografis | Kerumitan hukum internasional |
| **Duplicable** | Dapat disalin dengan sempurna | Memungkinkan analisis tanpa merusak asli |
| **Time-Sensitive** | Dapat berubah seiring waktu | Akuisisi cepat diperlukan |

### 3.2 Sumber Bukti Digital

![Sumber Bukti Digital](images/p01-04-digital-evidence-sources.png)

*Gambar 1.4: Berbagai sumber bukti digital*

#### 3.2.1 Kategori Sumber Bukti Digital

1. **Media Penyimpanan**
   - Hard Disk Drive (HDD)
   - Solid State Drive (SSD) - dibahas [Pertemuan 03](../p03/modul.md)
   - USB Flash Drive
   - Memory Cards
   - Optical Media (CD/DVD)

2. **Volatile Data** (dibahas lebih detail [Pertemuan 05](../p05/modul.md))
   - RAM contents
   - Running processes
   - Network connections
   - Clipboard data

3. **Jaringan** (dibahas [Pertemuan 10](../p10/modul.md))
   - Packet captures
   - Firewall logs
   - IDS/IPS logs
   - Router logs

4. **Perangkat Mobile** (dibahas [Pertemuan 15](../p15/modul.md))
   - Smartphones
   - Tablets
   - Wearable devices

5. **Cloud dan Remote** (dibahas [Pertemuan 14](../p14/modul.md))
   - Cloud storage
   - Email servers
   - Social media

#### Solved Problem 7 ⭐

**Soal:** Urutkan sumber bukti digital berikut berdasarkan tingkat volatilitas dari paling volatile ke paling stabil: Hard disk, RAM, CPU Register, Network traffic!

**Penyelesaian:**

*Step 1:* Pahami konsep Order of Volatility

Order of Volatility adalah urutan pengumpulan bukti berdasarkan seberapa cepat data dapat hilang.

*Step 2:* Urutkan berdasarkan volatilitas

| Urutan | Sumber | Volatilitas | Waktu Persistensi |
|--------|--------|-------------|-------------------|
| 1 | CPU Register | Paling tinggi | Nanoseconds |
| 2 | RAM | Tinggi | Hilang saat power off |
| 3 | Network Traffic | Sedang-Tinggi | Real-time, tidak disimpan |
| 4 | Hard Disk | Rendah | Persisten (hingga dihapus) |

**Jawaban:** Urutan dari paling volatile: **CPU Register → RAM → Network Traffic → Hard Disk**

---

### 3.3 Artefak Digital

> **Artefak Digital (Digital Artifacts)** adalah jejak atau residu data yang tertinggal pada sistem komputer sebagai hasil dari aktivitas pengguna atau sistem.

#### 3.3.1 Jenis-Jenis Artefak Digital

| Kategori | Contoh Artefak | Lokasi Umum |
|----------|---------------|-------------|
| **File System** | Deleted files, timestamps | MFT, file system journal |
| **Registry** | User activities, installed software | Windows Registry hives |
| **Browser** | History, cache, cookies | Browser data folders |
| **Application** | Recent documents, logs | App-specific folders |
| **System** | Event logs, prefetch | System folders |

#### Solved Problem 8 ⭐

**Soal:** Apa perbedaan antara metadata dan konten file dalam konteks bukti digital?

**Penyelesaian:**

*Step 1:* Definisikan metadata dan konten

*Step 2:* Bandingkan karakteristiknya

| Aspek | Metadata | Konten |
|-------|----------|--------|
| **Definisi** | Data tentang data | Isi aktual file |
| **Contoh** | Tanggal dibuat, nama author | Teks dokumen, gambar |
| **Modifikasi** | Dapat berubah tanpa mengubah konten | Perubahan konten bisa tidak mengubah metadata |
| **Nilai Forensik** | Timeline, attribution | Substansi kasus |

**Jawaban:** **Metadata** adalah informasi tentang file (seperti tanggal pembuatan, author, ukuran) sedangkan **konten** adalah isi aktual file. Dalam forensik, metadata penting untuk timeline analysis dan attribution, sementara konten penting untuk substansi kasus.

---

### 3.4 Integritas Bukti Digital

> **Integritas Bukti** adalah jaminan bahwa bukti digital tidak dimodifikasi sejak saat pengumpulan hingga presentasi di pengadilan.

Metode menjaga integritas:

1. **Hashing**: Menggunakan algoritma hash (MD5, SHA-1, SHA-256)
2. **Write Blocking**: Mencegah penulisan ke media bukti
3. **Chain of Custody**: Dokumentasi penanganan bukti
4. **Digital Signatures**: Tanda tangan digital untuk verifikasi

#### 3.4.1 Algoritma Hash dalam Forensik

```
Contoh Hash Values:

File: evidence.dd

MD5:    d41d8cd98f00b204e9800998ecf8427e
SHA-1:  da39a3ee5e6b4b0d3255bfef95601890afd80709
SHA-256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
```

| Algoritma | Panjang | Status | Rekomendasi |
|-----------|---------|--------|-------------|
| MD5 | 128-bit | Compromised | Tidak direkomendasikan |
| SHA-1 | 160-bit | Compromised | Tidak direkomendasikan |
| SHA-256 | 256-bit | Secure | Direkomendasikan |
| SHA-512 | 512-bit | Secure | Untuk keamanan tinggi |

#### Solved Problem 9 ⭐⭐

**Soal:** Anda membuat forensic image dari hard disk tersangka. Hash MD5 sebelum dan sesudah imaging berbeda. Apa kemungkinan penyebabnya dan bagaimana mengatasinya?

**Penyelesaian:**

*Step 1:* Identifikasi kemungkinan penyebab

1. **Bad sectors**: Sektor rusak yang dibaca berbeda setiap kali
2. **Write pada bukti**: Akses tanpa write blocker
3. **SSD TRIM**: SSD melakukan garbage collection
4. **Kesalahan hardware**: Kabel atau adapter bermasalah
5. **Proses imaging tidak lengkap**: Error saat imaging

*Step 2:* Tentukan langkah penanganan

| Penyebab | Solusi |
|----------|--------|
| Bad sectors | Gunakan tool yang handle bad sectors (dd dengan conv=noerror) |
| Write pada bukti | Selalu gunakan write blocker |
| SSD TRIM | Akuisisi secepat mungkin, pertimbangkan chip-off |
| Hardware error | Ganti kabel, verifikasi koneksi |
| Imaging error | Ulangi dengan memverifikasi setiap step |

*Step 3:* Dokumentasi

**Jawaban:** Kemungkinan penyebab: bad sectors, akses tanpa write blocker, atau SSD TRIM. Solusi: (1) Selalu gunakan write blocker, (2) Gunakan imaging tool yang handle bad sectors, (3) Dokumentasikan perbedaan hash dan penyebabnya, (4) Buat multiple copies dan verifikasi.

---

## 4. Forensic Readiness dalam Konteks Militer

### 4.1 Definisi Forensic Readiness

> **Forensic Readiness** adalah kemampuan organisasi untuk memaksimalkan penggunaan bukti digital sambil meminimalkan biaya investigasi.

![Forensic Readiness Framework](images/p01-05-forensic-readiness.png)

*Gambar 1.5: Komponen Forensic Readiness*

### 4.2 Komponen Forensic Readiness

| Komponen | Deskripsi | Implementasi Militer |
|----------|-----------|---------------------|
| **Policy** | Kebijakan dan prosedur | SOP forensik, klasifikasi data |
| **Technology** | Infrastruktur teknis | Log management, forensic tools |
| **People** | SDM terlatih | Tim forensik internal |
| **Process** | Prosedur operasional | Incident response plan |

#### 4.2.1 Manfaat Forensic Readiness

1. **Proaktif vs Reaktif**: Persiapan sebelum insiden terjadi
2. **Pengurangan Biaya**: Investigasi lebih efisien
3. **Kualitas Bukti**: Bukti lebih lengkap dan admissible
4. **Waktu Respons**: Investigasi lebih cepat
5. **Compliance**: Memenuhi regulasi dan standar

#### Solved Problem 10 ⭐⭐

**Soal:** Sebuah pangkalan TNI ingin menerapkan forensic readiness. Identifikasi lima langkah awal yang harus dilakukan!

**Penyelesaian:**

*Step 1:* Analisis kebutuhan organisasi militer

*Step 2:* Tentukan langkah-langkah prioritas

**Lima Langkah Awal Forensic Readiness:**

| No | Langkah | Aktivitas |
|----|---------|-----------|
| 1 | **Assessment** | Identifikasi aset kritis dan potensi ancaman |
| 2 | **Policy Development** | Buat SOP forensik sesuai regulasi militer |
| 3 | **Log Infrastructure** | Implementasi centralized logging |
| 4 | **Training** | Latih tim IT dan keamanan dasar forensik |
| 5 | **Tool Preparation** | Siapkan forensic toolkit dan media penyimpanan |

**Jawaban:** Lima langkah awal: (1) Assessment risiko dan aset kritis, (2) Pengembangan kebijakan dan SOP forensik, (3) Implementasi infrastruktur logging terpusat, (4) Pelatihan personel IT dan keamanan, (5) Persiapan toolkit forensik dan media penyimpanan steril.

---

### 4.3 Logging dan Monitoring

> **Logging** adalah proses pencatatan aktivitas sistem dan pengguna untuk keperluan audit dan forensik.

Jenis-jenis log penting:

| Jenis Log | Informasi | Lokasi (Windows) |
|-----------|-----------|------------------|
| **Security** | Authentication, access | Event Viewer |
| **Application** | Software events | Event Viewer |
| **System** | OS events | Event Viewer |
| **Firewall** | Network traffic | Firewall logs |
| **Web Server** | HTTP requests | IIS/Apache logs |

#### Solved Problem 11 ⭐

**Soal:** Sebutkan tiga jenis event Windows yang penting untuk investigasi unauthorized access!

**Penyelesaian:**

*Step 1:* Identifikasi event terkait akses tidak sah

**Event ID penting untuk unauthorized access:**

| Event ID | Deskripsi | Kepentingan |
|----------|-----------|-------------|
| **4624** | Successful logon | Siapa yang login berhasil |
| **4625** | Failed logon | Percobaan akses gagal |
| **4672** | Special privileges assigned | Eskalasi privilege |

**Jawaban:** Tiga event Windows penting: (1) Event ID 4624 (successful logon) untuk tracking siapa yang masuk, (2) Event ID 4625 (failed logon) untuk mendeteksi brute force, (3) Event ID 4672 (special privileges) untuk mendeteksi eskalasi privilege.

---

## 5. Hubungan Forensik Digital dengan Operasi Militer

### 5.1 Cyber Intelligence

> **Cyber Intelligence** adalah pengumpulan, analisis, dan pemanfaatan informasi tentang ancaman siber untuk mendukung pengambilan keputusan.

Hubungan dengan forensik digital:

| Aspek | Cyber Intelligence | Forensik Digital |
|-------|-------------------|------------------|
| **Fokus** | Threat prediction | Incident investigation |
| **Timing** | Proaktif | Reaktif |
| **Output** | Threat reports | Evidence reports |
| **Integration** | IOC dari forensik memperkaya intel | Intel membantu fokus investigasi |

#### 5.1.1 Intelligence Cycle dalam Konteks Forensik

![Intelligence Cycle](images/p01-06-intelligence-cycle.png)

*Gambar 1.6: Siklus Intelligence dan peran forensik digital*

### 5.2 Information Operations (Info Ops)

> **Information Operations** adalah penggunaan terintegrasi kemampuan terkait informasi untuk mempengaruhi, mengganggu, atau memanfaatkan pengambilan keputusan adversary.

Komponen Info Ops yang terkait forensik digital:

1. **Computer Network Operations (CNO)**
   - Computer Network Attack (CNA)
   - Computer Network Defense (CND)
   - Computer Network Exploitation (CNE)

2. **Electronic Warfare (EW)**
3. **Psychological Operations (PSYOP)**
4. **Operations Security (OPSEC)**

#### Solved Problem 12 ⭐⭐

**Soal:** Jelaskan bagaimana forensik digital mendukung Computer Network Defense (CND)!

**Penyelesaian:**

*Step 1:* Pahami CND

Computer Network Defense adalah tindakan untuk melindungi, memonitor, menganalisis, mendeteksi, dan merespons aktivitas tidak sah dalam sistem informasi.

*Step 2:* Identifikasi kontribusi forensik digital

| Fase CND | Kontribusi Forensik Digital |
|----------|----------------------------|
| **Protect** | Identifikasi vulnerability dari insiden sebelumnya |
| **Monitor** | Analisis log dan traffic untuk baseline |
| **Analyze** | Analisis mendalam terhadap anomali |
| **Detect** | Mengembangkan IOC dari investigasi |
| **Respond** | Investigasi dan containment insiden |

**Jawaban:** Forensik digital mendukung CND dengan: (1) Menyediakan lesson learned untuk memperkuat pertahanan, (2) Membantu memahami baseline normal sistem, (3) Melakukan analisis mendalam saat anomali terdeteksi, (4) Menghasilkan IOC untuk deteksi ancaman serupa, (5) Investigasi insiden untuk respons efektif.

---

### 5.3 Computer Network Operations (CNO)

![CNO Triad](images/p01-07-cno-triad.png)

*Gambar 1.7: Tiga komponen Computer Network Operations*

| Komponen | Tujuan | Peran Forensik |
|----------|--------|----------------|
| **CNA** | Mengganggu/merusak sistem adversary | Post-operation analysis |
| **CND** | Melindungi sistem sendiri | Incident response, threat hunting |
| **CNE** | Mengumpulkan intelijen | Analisis data yang diakuisisi |

#### Solved Problem 13 ⭐⭐⭐

**Soal:** Dalam konteks operasi militer gabungan, bagaimana forensik digital dapat mendukung attribution serangan siber yang menarget infrastruktur kritis nasional?

**Penyelesaian:**

*Step 1:* Pahami konsep attribution

Attribution adalah proses mengidentifikasi pelaku serangan siber dengan tingkat kepercayaan tertentu.

*Step 2:* Identifikasi metode attribution berbasis forensik

| Level | Teknik Forensik | Output |
|-------|----------------|--------|
| **Technical** | Malware analysis, network forensics | IOC, TTPs |
| **Operational** | Infrastructure analysis | C2 servers, domains |
| **Strategic** | Pattern analysis, correlation | Actor profiles |

*Step 3:* Jelaskan proses lengkap

1. **Evidence Collection**: Mengumpulkan bukti dari sistem terdampak
2. **Malware Analysis**: Reverse engineering untuk signature dan capability
3. **Infrastructure Mapping**: Melacak C2 dan domain
4. **TTP Analysis**: Membandingkan dengan known threat actors
5. **Correlation**: Menghubungkan dengan intelligence lain

**Jawaban:** Forensik digital mendukung attribution melalui: (1) Pengumpulan dan analisis bukti teknis (malware, logs, network traffic), (2) Identifikasi Tactics, Techniques, and Procedures (TTPs) pelaku, (3) Mapping infrastruktur serangan (C2, domains), (4) Korelasi dengan threat intelligence untuk mengidentifikasi aktor yang mungkin, (5) Dokumentasi temuan dengan chain of custody untuk mendukung tindakan diplomatik atau militer.

---

## 6. Peran dan Tanggung Jawab Investigator Forensik Militer

### 6.1 Struktur Tim Forensik

> **Digital Forensic Examiner** adalah profesional yang bertanggung jawab mengumpulkan, menganalisis, dan melaporkan bukti digital.

![Tim Forensik Digital](images/p01-08-forensic-team-structure.png)

*Gambar 1.8: Struktur tim forensik digital militer*

### 6.2 Kompetensi yang Diperlukan

| Kategori | Kompetensi | Sertifikasi Relevan |
|----------|-----------|---------------------|
| **Teknis** | OS internals, file systems, networking | CHFI, EnCE, GCFE |
| **Hukum** | Chain of custody, rules of evidence | - |
| **Analitis** | Critical thinking, pattern recognition | - |
| **Komunikasi** | Report writing, expert testimony | - |

#### 6.2.1 Sertifikasi Forensik Digital

| Sertifikasi | Organisasi | Fokus |
|-------------|------------|-------|
| CHFI | EC-Council | Computer Hacking Forensic Investigation |
| EnCE | OpenText | EnCase tool certification |
| GCFE | GIAC | GIAC Certified Forensic Examiner |
| GCFA | GIAC | GIAC Certified Forensic Analyst |
| CCE | ISFCE | Certified Computer Examiner |

#### Solved Problem 14 ⭐

**Soal:** Mengapa kemampuan komunikasi penting bagi investigator forensik digital?

**Penyelesaian:**

*Step 1:* Identifikasi situasi yang memerlukan komunikasi

*Step 2:* Analisis dampak komunikasi efektif

| Situasi | Kebutuhan Komunikasi |
|---------|---------------------|
| **Report Writing** | Menjelaskan temuan teknis dalam bahasa yang dipahami |
| **Court Testimony** | Menyajikan bukti secara meyakinkan |
| **Briefing Command** | Memberikan update situasi untuk pengambilan keputusan |
| **Team Coordination** | Kolaborasi efektif dalam investigasi |

**Jawaban:** Komunikasi penting karena investigator forensik harus: (1) Menulis laporan yang dipahami oleh non-teknis (hakim, komandan), (2) Memberikan kesaksian ahli di pengadilan, (3) Menyampaikan briefing untuk pengambilan keputusan operasional, (4) Berkoordinasi dengan tim multidisiplin.

---

### 6.3 Tanggung Jawab Etis dan Legal

> **Prinsip Etika Forensik Digital:**
> 1. Integritas: Menjaga keakuratan dan kelengkapan bukti
> 2. Objektivitas: Tidak bias dalam analisis
> 3. Kerahasiaan: Melindungi informasi sensitif
> 4. Kompetensi: Bekerja dalam batas kemampuan

#### 6.3.1 Dilema Etis dalam Forensik Militer

| Dilema | Pertimbangan |
|--------|-------------|
| **Scope creep** | Menemukan bukti kejahatan lain di luar investigasi |
| **Privacy** | Mengakses data pribadi anggota militer |
| **Chain of command** | Tekanan untuk hasil tertentu |
| **Classified info** | Menangani bukti dengan klasifikasi tinggi |

#### Solved Problem 15 ⭐⭐

**Soal:** Seorang investigator forensik militer menemukan bukti penggelapan dana di komputer tersangka saat menginvestigasi kebocoran data. Bagaimana seharusnya investigator menangani situasi ini?

**Penyelesaian:**

*Step 1:* Identifikasi dilema etis

- **Masalah**: Menemukan bukti kejahatan di luar scope investigasi
- **Pertimbangan**: Legalitas akuisisi, scope warrant/perintah

*Step 2:* Tentukan langkah yang tepat

1. **Dokumentasi**: Catat temuan dan bagaimana ditemukan
2. **Stop Investigation**: Jangan investigasi lebih lanjut temuan baru
3. **Report**: Laporkan ke atasan dan penasihat hukum
4. **Seek Guidance**: Tunggu instruksi untuk melanjutkan
5. **Preserve**: Jaga integritas bukti yang sudah ditemukan

**Jawaban:** Investigator harus: (1) Mendokumentasikan temuan tanpa investigasi lebih lanjut, (2) Melaporkan kepada atasan dan penasihat hukum, (3) Menjaga integritas bukti, (4) Menunggu perluasan scope investigasi atau perintah baru. Investigasi tidak boleh dilanjutkan tanpa otorisasi yang tepat.

---

## 7. Regulasi Forensik Digital di Indonesia

### 7.1 Kerangka Hukum

#### 7.1.1 UU No. 1 Tahun 2024 tentang ITE

> **UU ITE** mengatur tentang informasi dan transaksi elektronik, termasuk pengakuan bukti elektronik dan kejahatan siber.

Pasal-pasal penting untuk forensik digital:

| Pasal | Substansi |
|-------|-----------|
| **Pasal 5** | Pengakuan bukti elektronik sebagai alat bukti yang sah |
| **Pasal 6** | Pengakuan dokumen elektronik setara dokumen tertulis |
| **Pasal 27-37** | Definisi perbuatan yang dilarang (kejahatan siber) |
| **Pasal 43** | Kewenangan penyidik dalam mengakses sistem elektronik |

#### 7.1.2 UU No. 3 Tahun 2002 tentang Pertahanan Negara

Relevansi dengan forensik digital:
- Pertahanan siber sebagai bagian pertahanan nasional
- Kewenangan TNI dalam operasi siber pertahanan
- Penggunaan teknologi informasi untuk pertahanan

### 7.2 Admissibility Bukti Digital

> **Admissibility** adalah kemampuan bukti untuk diterima di pengadilan berdasarkan kriteria tertentu.

Kriteria admissibility bukti digital:

| Kriteria | Deskripsi | Cara Memenuhi |
|----------|-----------|---------------|
| **Authenticity** | Bukti asli, bukan fabrikasi | Hash verification, chain of custody |
| **Reliability** | Proses yang dapat diandalkan | Tool validation, standard procedures |
| **Relevance** | Terkait dengan kasus | Scope investigation |
| **Completeness** | Tidak mengubah konteks | Full imaging, metadata preservation |

#### Solved Problem 16 ⭐⭐⭐

**Soal:** Bagaimana memastikan bukti digital dari komputer terduga pelaku kebocoran informasi militer dapat diterima di pengadilan militer?

**Penyelesaian:**

*Step 1:* Identifikasi persyaratan admissibility

*Step 2:* Buat checklist kepatuhan

**Langkah-langkah memastikan admissibility:**

| Tahap | Aktivitas | Dokumentasi |
|-------|-----------|-------------|
| **Pre-Acquisition** | Perintah/surat tugas investigasi | Copy perintah |
| **Acquisition** | Write blocker, hash before/after | Acquisition log, hash values |
| **Chain of Custody** | Dokumentasi setiap transfer | CoC form signed |
| **Analysis** | Gunakan tool validated | Tool validation records |
| **Reporting** | Laporan lengkap dan objektif | Final report |
| **Storage** | Bukti diamankan dengan tepat | Evidence room log |

*Step 3:* Pertimbangan khusus pengadilan militer

- Kepatuhan terhadap prosedur militer
- Klasifikasi keamanan bukti
- Kompetensi investigator (sertifikasi)

**Jawaban:** Memastikan admissibility dengan: (1) Otorisasi investigasi yang sah, (2) Menggunakan write blocker dan verifikasi hash, (3) Dokumentasi chain of custody lengkap, (4) Analisis dengan tool tervalidasi, (5) Laporan objektif dan komprehensif, (6) Penyimpanan bukti yang aman. Untuk pengadilan militer, tambahan pertimbangan klasifikasi keamanan dan prosedur militer.

---

### 7.3 Standar dan Best Practices

#### 7.3.1 Standar Internasional

| Standar | Fokus | Penerapan |
|---------|-------|-----------|
| **ISO/IEC 27037** | Identification, collection, acquisition, preservation | Proses investigasi |
| **ISO/IEC 27041** | Assurance for digital evidence | Quality management |
| **ISO/IEC 27042** | Analysis and interpretation | Metodologi analisis |
| **ISO/IEC 27043** | Incident investigation | Framework investigasi |

#### 7.3.2 NIST Guidelines

> **NIST SP 800-86** adalah panduan untuk mengintegrasikan teknik forensik ke dalam incident response.

Fase-fase menurut NIST:

1. **Collection**: Mengidentifikasi dan mengumpulkan data
2. **Examination**: Memproses data untuk membuatnya dapat dianalisis
3. **Analysis**: Menganalisis data untuk menjawab pertanyaan investigasi
4. **Reporting**: Menyajikan hasil dalam format yang tepat

#### Solved Problem 17 ⭐

**Soal:** Sebutkan empat fase forensik digital menurut NIST SP 800-86!

**Penyelesaian:**

**Empat Fase NIST:**

| Fase | Aktivitas | Output |
|------|-----------|--------|
| **1. Collection** | Identifikasi, akuisisi, preservasi | Forensic images, data copies |
| **2. Examination** | Processing, reduction, filtering | Extracted data, parsed artifacts |
| **3. Analysis** | Correlation, timeline, interpretation | Findings, conclusions |
| **4. Reporting** | Documentation, presentation | Forensic report |

**Jawaban:** Empat fase menurut NIST SP 800-86: (1) Collection - pengumpulan dan preservasi data, (2) Examination - pemrosesan data menjadi bentuk yang dapat dianalisis, (3) Analysis - analisis untuk menjawab pertanyaan investigasi, (4) Reporting - dokumentasi dan penyajian hasil.

---

## 8. Tools Forensik Digital - Pengenalan

### 8.1 Kategori Tools Forensik

| Kategori | Fungsi | Contoh Tools |
|----------|--------|--------------|
| **Imaging** | Membuat forensic image | FTK Imager, dd |
| **Analysis Suite** | Analisis komprehensif | Autopsy, EnCase |
| **Memory Analysis** | Analisis RAM | Volatility |
| **Network Analysis** | Analisis traffic | Wireshark |
| **Recovery** | Pemulihan data | PhotoRec, Recuva |

### 8.2 FTK Imager

> **FTK Imager** adalah tool gratis untuk akuisisi data dan previewing bukti digital.

Kemampuan FTK Imager:
- Membuat forensic image (dd, E01, AFF)
- Mount forensic image
- Export files dan folders
- Compute dan verify hash

### 8.3 Autopsy

> **Autopsy** adalah platform forensik digital open-source untuk analisis hard disk, smartphone, dan media lainnya.

Modul Autopsy:
- File Analysis
- Timeline Analysis
- Hash Lookup
- Keyword Search
- Web Artifacts
- Email Analysis

#### Solved Problem 18 ⭐

**Soal:** Apa perbedaan antara FTK Imager dan Autopsy?

**Penyelesaian:**

| Aspek | FTK Imager | Autopsy |
|-------|------------|---------|
| **Fungsi Utama** | Akuisisi/imaging | Analisis |
| **Output** | Forensic images | Reports, findings |
| **Fase Forensik** | Collection | Examination, Analysis |
| **Kompleksitas** | Sederhana | Komprehensif |
| **Lisensi** | Free | Free (Open Source) |

**Jawaban:** FTK Imager fokus pada **akuisisi** (membuat forensic image), sedangkan Autopsy fokus pada **analisis** (mengekstrak dan menganalisis bukti dari image). Dalam workflow forensik, FTK Imager digunakan di fase Collection, sedangkan Autopsy di fase Examination dan Analysis.

---

### 8.4 Windows Sysinternals Suite

> **Sysinternals Suite** adalah kumpulan tools system dan troubleshooting Windows yang berguna untuk forensik.

Tools penting untuk forensik:

| Tool | Fungsi |
|------|--------|
| **Process Explorer** | Monitoring proses detail |
| **Process Monitor** | Logging aktivitas real-time |
| **Autoruns** | Startup items analysis |
| **TCPView** | Network connections |
| **Strings** | Ekstraksi text strings |

#### Solved Problem 19 ⭐⭐

**Soal:** Bagaimana Process Monitor dapat digunakan dalam investigasi forensik live?

**Penyelesaian:**

*Step 1:* Pahami kemampuan Process Monitor

Process Monitor (ProcMon) mencatat:
- File system activity
- Registry activity
- Process/thread activity
- Network activity

*Step 2:* Identifikasi use cases forensik

| Use Case | Aktivitas ProcMon |
|----------|------------------|
| **Malware Analysis** | Track malware behavior (file creation, registry changes) |
| **Unauthorized Access** | Monitor file access patterns |
| **Data Exfiltration** | Track file reads dan network activity |
| **Persistence** | Detect registry autostart entries |

*Step 3:* Best practices

- Mulai capture sebelum aktivitas mencurigakan
- Filter untuk mengurangi noise
- Export log untuk analisis offline

**Jawaban:** Process Monitor dapat digunakan untuk: (1) Menganalisis behavior malware secara real-time (file creation, registry modification), (2) Mendeteksi akses file tidak sah, (3) Melacak aktivitas yang mengarah ke data exfiltration, (4) Mengidentifikasi mekanisme persistence. Log ProcMon dapat di-export untuk analisis lebih lanjut.

---

## 9. Dokumentasi Investigasi Forensik

### 9.1 Pentingnya Dokumentasi

> **Dokumentasi** dalam forensik digital adalah pencatatan sistematis setiap aktivitas investigasi untuk memastikan reproducibility dan admissibility bukti.

Prinsip dokumentasi:
1. **Contemporaneous**: Dicatat saat aktivitas berlangsung
2. **Complete**: Mencakup semua detail relevan
3. **Accurate**: Faktual dan tidak bias
4. **Chronological**: Urut berdasarkan waktu

### 9.2 Jenis Dokumentasi

| Dokumen | Fungsi | Timing |
|---------|--------|--------|
| **Case Notes** | Catatan aktivitas investigator | Selama investigasi |
| **Chain of Custody** | Tracking penanganan bukti | Setiap transfer bukti |
| **Acquisition Log** | Detail proses akuisisi | Saat akuisisi |
| **Analysis Report** | Temuan dan kesimpulan | Akhir investigasi |

### 9.3 Chain of Custody

> **Chain of Custody (CoC)** adalah dokumentasi kronologis yang menunjukkan siapa yang menangani bukti, kapan, dan untuk tujuan apa.

Elemen Chain of Custody:

| Elemen | Informasi |
|--------|-----------|
| **Item Description** | Deskripsi bukti (model, serial number) |
| **Date/Time** | Kapan transfer terjadi |
| **From/To** | Siapa menyerahkan dan menerima |
| **Purpose** | Tujuan transfer |
| **Condition** | Kondisi bukti saat transfer |
| **Signatures** | Tanda tangan kedua pihak |

#### Solved Problem 20 ⭐⭐

**Soal:** Buat template sederhana Chain of Custody form!

**Penyelesaian:**

```
CHAIN OF CUSTODY FORM
=====================

Case Number: ________________
Case Name: _________________

EVIDENCE DESCRIPTION
--------------------
Item #: ________
Description: __________________________________
Make/Model: __________________________________
Serial Number: _______________________________
Initial Hash (SHA-256): ______________________

CUSTODY LOG
-----------
| Date/Time | Released By | Received By | Purpose | Condition | Signature |
|-----------|-------------|-------------|---------|-----------|-----------|
|           |             |             |         |           |           |
|           |             |             |         |           |           |

NOTES
-----
__________________________________________________
__________________________________________________

Prepared by: _____________ Date: ________________
```

**Jawaban:** Template di atas mencakup elemen esensial CoC: identifikasi case, deskripsi bukti dengan hash awal, log transfer dengan semua informasi yang diperlukan, dan ruang untuk catatan tambahan.

---

### 9.4 Laporan Forensik

Struktur laporan forensik:

1. **Executive Summary**: Ringkasan untuk non-teknis
2. **Case Information**: Detail kasus dan scope
3. **Evidence Description**: Deskripsi bukti yang dianalisis
4. **Tools and Methods**: Tools dan metodologi yang digunakan
5. **Findings**: Temuan detail dengan supporting evidence
6. **Analysis**: Interpretasi temuan
7. **Conclusions**: Kesimpulan berdasarkan temuan
8. **Appendices**: Hash values, timeline, technical details

#### Solved Problem 21 ⭐⭐⭐

**Soal:** Investigator menemukan malware di workstation anggota militer. Buat executive summary untuk laporan forensik!

**Penyelesaian:**

*Step 1:* Identifikasi informasi yang perlu disertakan dalam executive summary

*Step 2:* Tulis dalam bahasa non-teknis

```
EXECUTIVE SUMMARY
=================

Tanggal Laporan: [Tanggal]
Nomor Kasus: [Nomor]
Investigator: [Nama, Sertifikasi]

RINGKASAN INVESTIGASI
---------------------
Investigasi forensik digital dilakukan terhadap workstation 
(Evidence #001) milik [Nama] setelah adanya laporan aktivitas 
mencurigakan pada sistem jaringan pangkalan.

TEMUAN UTAMA
------------
1. Terdeteksi keberadaan malware jenis Remote Access Trojan 
   (RAT) pada sistem sejak [tanggal perkiraan instalasi]
   
2. Malware memiliki kemampuan untuk:
   - Mengakses file dari jarak jauh
   - Mencatat ketikan keyboard
   - Mengambil tangkapan layar

3. Terdapat indikasi komunikasi keluar ke server yang 
   berlokasi di [negara/IP]

4. Sejumlah file dengan klasifikasi [level] potensial 
   telah diakses selama periode infeksi

KESIMPULAN
----------
Dengan tingkat kepercayaan TINGGI, workstation tersebut 
telah terkompromi oleh malware yang memungkinkan akses 
tidak sah ke sistem dan data. Investigasi lanjutan 
diperlukan untuk menentukan:
- Extent of data exfiltration
- Attribution pelaku
- Kerentanan yang dieksploitasi

REKOMENDASI
-----------
1. Isolasi sistem terdampak dari jaringan
2. Reset kredensial pengguna dan sistem terkait
3. Audit sistem lain untuk malware serupa
4. Review dan perkuat kebijakan keamanan
```

**Jawaban:** Executive summary harus mencakup: (1) Informasi kasus dasar, (2) Ringkasan investigasi, (3) Temuan utama dalam bahasa non-teknis, (4) Kesimpulan dengan tingkat kepercayaan, (5) Rekomendasi tindak lanjut.

---

## 10. Etika Profesional Investigator Forensik

### 10.1 Kode Etik

> **Kode Etik Investigator Forensik** adalah seperangkat prinsip yang mengatur perilaku profesional dalam melakukan investigasi.

Prinsip utama:
1. **Integritas**: Jujur dan tidak memalsukan bukti
2. **Objektivitas**: Tidak bias, melaporkan semua temuan
3. **Kompetensi**: Bekerja dalam batas kemampuan
4. **Kerahasiaan**: Melindungi informasi sensitif
5. **Profesionalisme**: Menjaga standar tertinggi

### 10.2 Konflik Kepentingan

| Situasi | Penanganan |
|---------|-----------|
| **Mengenal tersangka** | Disclose dan recuse jika perlu |
| **Tekanan hasil tertentu** | Dokumentasikan, konsultasi legal |
| **Compensation based on outcome** | Tolak atau disclose |
| **Dual roles** | Jelaskan batasan |

#### Solved Problem 22 ⭐⭐⭐

**Soal:** Seorang investigator forensik militer diminta oleh atasannya untuk "memastikan" bukti menunjukkan bahwa tersangka bersalah. Bagaimana investigator harus merespons?

**Penyelesaian:**

*Step 1:* Identifikasi masalah etis

- Tekanan untuk bias hasil investigasi
- Potensi pelanggaran integritas bukti
- Konflik antara kepatuhan perintah dan etika profesional

*Step 2:* Evaluasi opsi respons

| Opsi | Konsekuensi | Etis? |
|------|-------------|-------|
| Mematuhi perintah | Bukti dimanipulasi | ❌ |
| Menolak langsung | Potensi sanksi karir | Etis tapi berisiko |
| Eskalasi | Melaporkan ke atasan lebih tinggi | ✅ Direkomendasikan |
| Dokumentasi | Mencatat tekanan sebagai bukti | ✅ Prudent |

*Step 3:* Tentukan tindakan yang tepat

1. **Dokumentasikan** permintaan tersebut secara tertulis
2. **Jelaskan** kepada atasan tentang kewajiban etis dan legal investigator
3. **Eskalasi** ke penasihat hukum atau atasan lebih tinggi jika tekanan berlanjut
4. **Tolak** untuk memanipulasi bukti bagaimanapun caranya
5. **Lakukan** investigasi secara objektif sesuai metodologi

**Jawaban:** Investigator harus: (1) Mendokumentasikan permintaan tersebut, (2) Menjelaskan kewajiban etis dan legal untuk objektif, (3) Eskalasi ke penasihat hukum jika tekanan berlanjut, (4) Menolak tegas untuk memanipulasi bukti. Investigator harus tetap profesional dan melakukan investigasi secara objektif, melaporkan semua temuan baik yang memberatkan maupun yang meringankan tersangka.

---

## Supplementary Problems

### Problem S1 ⭐
**Soal:** Sebutkan lima sumber bukti digital yang paling volatile!

**Jawaban:** (1) CPU registers, (2) CPU cache, (3) RAM, (4) Network state, (5) Running processes. Semua ini hilang ketika sistem dimatikan.

---

### Problem S2 ⭐⭐
**Soal:** Apa perbedaan antara live forensics dan dead forensics?

**Jawaban:** 
- **Live forensics**: Analisis sistem yang masih berjalan, dapat mengakses volatile data (RAM, processes, network connections)
- **Dead forensics**: Analisis sistem yang sudah dimatikan, fokus pada persistent storage (hard disk)
- Live forensics penting untuk menangkap bukti volatile, tapi berisiko mengubah state sistem

---

### Problem S3 ⭐⭐
**Soal:** Mengapa SSD memerlukan pertimbangan khusus dalam forensik digital?

**Jawaban:** SSD memiliki tantangan khusus karena: (1) TRIM command secara otomatis menghapus data yang tidak digunakan, (2) Wear leveling menyebabkan data tersebar di lokasi berbeda, (3) Garbage collection dapat menghapus data tanpa perintah pengguna. Ini membuat recovery data lebih sulit dibanding HDD. (Akan dibahas detail di [Pertemuan 03](../p03/modul.md))

---

### Problem S4 ⭐⭐⭐
**Soal:** Bagaimana prinsip Locard's Exchange diterapkan dalam investigasi intrusi jaringan?

**Jawaban:** Prinsip Locard menyatakan setiap kontak meninggalkan jejak. Dalam intrusi jaringan:
- **Attacker meninggalkan**: Log entries, malware, registry modifications, network artifacts, timestamp anomalies
- **Attacker mengambil**: System information, credentials, data files
- Investigator mencari "jejak" ini untuk merekonstruksi serangan dan mengidentifikasi pelaku

---

### Problem S5 ⭐⭐⭐
**Soal:** Jelaskan konsep anti-forensik dan tiga teknik yang umum digunakan!

**Jawaban:** 
**Anti-forensik** adalah teknik untuk menghindari, menghalangi, atau menyesatkan investigasi forensik.

Tiga teknik umum:
1. **Data hiding**: Steganography, alternate data streams, encrypted containers
2. **Artifact wiping**: Secure deletion, log cleaning, timestamp modification
3. **Trail obfuscation**: Onion routing (Tor), proxy chains, timestomping

(Akan dibahas detail di [Pertemuan 12](../p12/modul.md))

---

## Ringkasan

| Konsep | Deskripsi Singkat |
|--------|-------------------|
| **Forensik Digital** | Identifikasi, pengumpulan, preservasi, analisis, dan penyajian bukti digital |
| **Prinsip Dasar** | Tidak mengubah bukti, chain of custody, kompetensi, dokumentasi, hukum & etika |
| **Kejahatan Siber** | Internal threat vs External threat, APT, Cyber Kill Chain |
| **Bukti Digital** | Latent, fragile, crossing jurisdictions, duplicable, time-sensitive |
| **Forensic Readiness** | Policy, Technology, People, Process |
| **Cyber Intelligence** | Integrasi forensik dengan operasi intelijen siber |
| **CNO** | CNA, CND, CNE - peran forensik dalam operasi jaringan komputer |
| **Regulasi Indonesia** | UU ITE No. 1/2024, UU Pertahanan No. 3/2002 |
| **Standar** | ISO 27037, NIST SP 800-86 |
| **Tools** | FTK Imager (akuisisi), Autopsy (analisis), Sysinternals (live analysis) |
| **Dokumentasi** | Case notes, chain of custody, acquisition log, forensic report |
| **Etika** | Integritas, objektivitas, kompetensi, kerahasiaan, profesionalisme |

---

## Referensi

1. Casey, E. (2022). *Digital Evidence and Computer Crime: Forensic Science, Computers, and the Internet* (4th Ed.). Academic Press. Chapter 1-2.
2. Phillips, A., Nelson, B., & Steuart, C. (2022). *Guide to Computer Forensics and Investigations* (6th Ed.). Cengage Learning. Chapter 1.
3. Sammons, J. (2020). *The Basics of Digital Forensics* (2nd Ed.). Syngress. Chapter 1.
4. Johansen, G. (2020). *Digital Forensics and Incident Response* (2nd Ed.). Packt. Chapter 1.
5. NIST SP 800-86: Guide to Integrating Forensic Techniques into Incident Response.
6. ISO/IEC 27037:2012 - Guidelines for identification, collection, acquisition and preservation of digital evidence.
7. UU No. 1 Tahun 2024 tentang Informasi dan Transaksi Elektronik.
8. UU No. 3 Tahun 2002 tentang Pertahanan Negara.

---

## License / Lisensi

This repository is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

Commercial use is permitted, provided attribution is given to the author.

© 2026 Anindito
