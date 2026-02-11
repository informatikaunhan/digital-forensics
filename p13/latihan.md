# Latihan Pertemuan 13: Forensik Dark Web dan Investigasi Email

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 13  
**Topik:** Forensik Dark Web dan Investigasi Email  
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
Berapa persentase perkiraan konten internet yang termasuk dalam surface web?

A. ~50-60%  
B. ~20-30%  
C. ~10-15%  
D. ~4-5%  
E. ~1-2%

---

### Soal 2
Manakah yang BUKAN merupakan bagian dari deep web?

A. Akun email pengguna  
B. Database perbankan online  
C. Website Wikipedia yang terindeks Google  
D. Intranet perusahaan militer  
E. Sistem rekam medis rumah sakit

---

### Soal 3
Jaringan Tor awalnya dikembangkan oleh:

A. MIT Computer Science and Artificial Intelligence Laboratory  
B. U.S. Naval Research Laboratory  
C. DARPA (Defense Advanced Research Projects Agency)  
D. NSA (National Security Agency)  
E. Electronic Frontier Foundation

---

### Soal 4
Dalam arsitektur Tor, relay yang mengetahui IP asli pengguna adalah:

A. Exit Node  
B. Middle Node  
C. Guard Node  
D. Bridge Node  
E. Rendezvous Point

---

### Soal 5
Apa yang membedakan hidden services (.onion) dari website biasa yang diakses melalui Tor?

A. Hidden services menggunakan enkripsi yang lebih kuat  
B. Trafik tidak pernah meninggalkan jaringan Tor melalui exit node  
C. Hidden services hanya bisa diakses dari negara tertentu  
D. Hidden services tidak menggunakan relay sama sekali  
E. Trafik hidden services tidak terenkripsi

---

### Soal 6
Artefak forensik mana yang paling menunjukkan bahwa Tor Browser pernah dijalankan di sistem Windows?

A. File cookies di browser Chrome  
B. File prefetch TOR.EXE-*.pf di C:\Windows\Prefetch  
C. History browser Internet Explorer  
D. File temporary di folder %TEMP%  
E. Registry key di HKLM\SOFTWARE

---

### Soal 7
Bitcoin bersifat pseudonymous, yang berarti:

A. Semua transaksi benar-benar anonim  
B. Transaksi tercatat publik di blockchain tetapi alamat tidak langsung terkait identitas  
C. Hanya pemerintah yang bisa melacak transaksi  
D. Setiap transaksi memerlukan identitas KYC  
E. Bitcoin tidak dapat dilacak sama sekali

---

### Soal 8
Teknik forensik blockchain yang mengelompokkan beberapa alamat Bitcoin milik satu entitas disebut:

A. Transaction Graph Analysis  
B. Exchange Identification  
C. Cluster Analysis  
D. Mixing Detection  
E. Heuristic Validation

---

### Soal 9
Cryptocurrency yang menggunakan ring signatures dan stealth addresses untuk menyembunyikan pengirim dan penerima adalah:

A. Bitcoin  
B. Ethereum  
C. Litecoin  
D. Monero  
E. Dogecoin

---

### Soal 10
Protokol email yang berfungsi untuk pengiriman email dari klien ke server dan antar server adalah:

A. POP3  
B. IMAP  
C. SMTP  
D. HTTP  
E. FTP

---

### Soal 11
Perbedaan utama antara POP3 dan IMAP dari perspektif forensik adalah:

A. POP3 menggunakan enkripsi tetapi IMAP tidak  
B. POP3 mengunduh email ke lokal sedangkan IMAP menyimpan di server  
C. IMAP lebih cepat dari POP3  
D. POP3 hanya mendukung text sedangkan IMAP mendukung attachment  
E. Tidak ada perbedaan yang signifikan

---

### Soal 12
Cara yang benar untuk membaca header "Received" dalam email forensics adalah:

A. Dari atas ke bawah  
B. Dari bawah ke atas  
C. Hanya header pertama yang relevan  
D. Hanya header terakhir yang relevan  
E. Urutan tidak penting

---

### Soal 13
Field email header manakah yang paling mudah dipalsukan (spoofable)?

A. Received  
B. X-Originating-IP  
C. From  
D. Authentication-Results  
E. Message-ID yang dibuat server

---

### Soal 14
Mekanisme autentikasi email yang memverifikasi server pengirim melalui DNS TXT record adalah:

A. DKIM  
B. DMARC  
C. SPF  
D. TLS  
E. PGP

---

### Soal 15
DKIM menggunakan teknologi apa untuk memverifikasi integritas email?

A. Sertifikat SSL  
B. Hash MD5  
C. Tanda tangan digital (public key cryptography)  
D. Token berbasis waktu  
E. Enkripsi simetris AES

---

### Soal 16
Jika hasil pemeriksaan email menunjukkan "spf=fail; dkim=none; dmarc=fail", apa kesimpulan yang paling tepat?

A. Email tersebut aman dan terverifikasi  
B. Server penerima mengalami gangguan teknis  
C. Ada indikasi kuat bahwa email tersebut di-spoofed  
D. Email dikirim dari mobile device  
E. Pengirim menggunakan VPN

---

### Soal 17
Jenis serangan phishing yang secara spesifik menargetkan pimpinan/eksekutif tingkat tinggi disebut:

A. Spear phishing  
B. Whaling  
C. Clone phishing  
D. Vishing  
E. Smishing

---

### Soal 18
URL "kemenhan.go.id.malicious.xyz" menggunakan teknik phishing yang disebut:

A. Typosquatting  
B. Homograph attack  
C. Subdomain abuse  
D. URL shortening  
E. Path manipulation

---

### Soal 19
Format file email yang merupakan Personal Storage Table milik Microsoft Outlook adalah:

A. .eml  
B. .mbox  
C. .pst  
D. .dbx  
E. .msg

---

### Soal 20
Langkah pertama yang harus dilakukan saat menerima laporan email phishing dalam investigasi forensik adalah:

A. Mengklik tautan untuk melihat tujuannya  
B. Memforward email ke atasan  
C. Menyimpan/preservasi email asli (.eml/.msg)  
D. Menghapus email segera  
E. Membalas email pengirim untuk konfirmasi

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Jelaskan perbedaan antara surface web, deep web, dan dark web! Berikan masing-masing dua contoh!

---

### Soal 2 ⭐
Jelaskan fungsi tiga jenis relay dalam jaringan Tor: Guard Node, Middle Node, dan Exit Node!

---

### Soal 3 ⭐
Apa yang dimaksud dengan onion routing? Mengapa disebut "onion" (bawang)?

---

### Soal 4 ⭐
Sebutkan lima artefak forensik yang menunjukkan penggunaan Tor Browser pada sistem Windows!

---

### Soal 5 ⭐⭐
Jelaskan mengapa Bitcoin disebut pseudonymous dan bukan anonymous! Bagaimana investigator dapat melacak transaksi Bitcoin?

---

### Soal 6 ⭐⭐
Bandingkan kemampuan forensik pada Bitcoin dan Monero! Mengapa Monero lebih sulit dilacak?

---

### Soal 7 ⭐⭐
Jelaskan lima langkah metodologi OSINT (DEFINE, DISCOVER, ANALYZE, VALIDATE, REPORT) dalam konteks investigasi dark web!

---

### Soal 8 ⭐⭐
Jelaskan alur lengkap pengiriman email dari pengirim ke penerima, termasuk komponen MUA, MSA, MTA, dan MDA!

---

### Soal 9 ⭐⭐
Jelaskan perbedaan SPF, DKIM, dan DMARC! Apa yang diverifikasi oleh masing-masing mekanisme?

---

### Soal 10 ⭐⭐
Bagaimana cara membaca header "Received" dalam email untuk menentukan IP pengirim asli? Berikan contoh!

---

### Soal 11 ⭐⭐
Sebutkan dan jelaskan lima jenis serangan phishing beserta target masing-masing!

---

### Soal 12 ⭐⭐⭐
Jelaskan langkah-langkah investigasi phishing email secara lengkap, dari preservasi hingga dokumentasi!

---

### Soal 13 ⭐⭐⭐
Bagaimana cara mendeteksi bahwa sebuah email telah di-spoofed? Sebutkan minimal lima indikator yang perlu diperiksa!

---

### Soal 14 ⭐⭐⭐
Jelaskan lokasi artefak email pada berbagai klien (Outlook, Thunderbird, Windows Mail) dan format file masing-masing! Bagaimana tools forensik seperti Autopsy dapat membantu analisis?

---

### Soal 15 ⭐⭐⭐
Jelaskan pertimbangan legal dan etika dalam melakukan investigasi dark web untuk keperluan militer Indonesia! Sebutkan regulasi yang relevan!

---

## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Penjualan Data Personel Militer di Dark Web

**Latar Belakang:**

Tim Cyber Threat Intelligence TNI mendeteksi posting di sebuah dark web marketplace yang mengiklankan penjualan database personel militer Indonesia. Data yang ditawarkan meliputi nama, NRP, pangkat, unit penugasan, dan alamat rumah dari sekitar 5.000 personel aktif. Penjual menggunakan username "gh0st_id" dan meminta pembayaran dalam Monero (XMR).

**Informasi Awal:**
- Posting ditemukan di marketplace ".onion" pada 20 Februari 2026
- Sampel data yang ditampilkan penjual cocok dengan format database internal TNI
- Penjual aktif di beberapa forum dark web dengan username serupa
- Harga yang diminta: 5 XMR (sekitar 150 juta Rupiah)
- Penjual mengklaim data berasal dari "sumber internal"

**Pertanyaan:**

1. Jelaskan langkah-langkah investigasi OSINT yang harus dilakukan untuk mengidentifikasi penjual "gh0st_id"! (20 poin)
2. Mengapa penggunaan Monero oleh penjual mempersulit investigasi dibandingkan jika menggunakan Bitcoin? Jelaskan teknik forensik yang masih mungkin dilakukan! (15 poin)
3. Bagaimana cara memverifikasi keaslian data yang ditawarkan tanpa membeli dataset tersebut? (15 poin)
4. Jelaskan aspek legal dan etika yang harus diperhatikan tim investigasi dalam operasi ini! (10 poin)
5. Rekomendasikan langkah mitigasi jika data terbukti berasal dari kebocoran internal! (10 poin)

---

### Studi Kasus 2: Kampanye Spear Phishing terhadap Perwira TNI

**Latar Belakang:**

Beberapa perwira menengah di Kodam Jaya menerima email yang mengaku dari Sekretariat Jenderal Kementerian Pertahanan. Email tersebut menginformasikan tentang "undangan rapat koordinasi pertahanan nasional" dan melampirkan file "Undangan_Rakor_Kemenhan_2026.pdf.exe". Dua dari lima penerima mengklik lampiran tersebut.

**Header Email (Sampel):**
```
From: sekjen@kemenhan.go.id
Return-Path: <admin@kemenhan-portal.xyz>
Received: from smtp.kemenhan-portal.xyz (198.51.100.75)
  by mail.kodam-jaya.mil.id; Mon, 17 Feb 2026 09:15:22 +0700
Message-ID: <abc123@kemenhan-portal.xyz>
X-Originating-IP: [198.51.100.75]
Authentication-Results: mail.kodam-jaya.mil.id;
  spf=fail (sender IP is 198.51.100.75);
  dkim=none;
  dmarc=fail (p=REJECT)
Subject: [URGENT] Undangan Rakor Pertahanan Nasional 2026
X-Mailer: PHPMailer 6.8.0
```

**Pertanyaan:**

1. Analisis header email di atas! Identifikasi minimal lima indikator bahwa email ini adalah spoofed/phishing! (20 poin)
2. Jelaskan mengapa ekstensi file "Undangan_Rakor_Kemenhan_2026.pdf.exe" berbahaya dan teknik social engineering apa yang digunakan! (10 poin)
3. Apa langkah-langkah incident response yang harus dilakukan setelah diketahui dua perwira mengklik lampiran? (20 poin)
4. Bagaimana cara melakukan analisis forensik pada workstation yang terinfeksi tanpa menghilangkan bukti? (15 poin)
5. Rekomendasikan langkah pencegahan agar insiden serupa tidak terulang! (5 poin)

---

## Kunci Jawaban

### Bagian A: Pilihan Ganda

| No | Jawaban | Penjelasan |
|----|---------|------------|
| 1 | D | Surface web hanya ~4-5% dari total konten internet |
| 2 | C | Wikipedia terindeks oleh mesin pencari sehingga termasuk surface web |
| 3 | B | Tor dikembangkan oleh U.S. Naval Research Laboratory pada pertengahan 1990-an |
| 4 | C | Guard node adalah relay pertama yang menerima koneksi langsung dari pengguna |
| 5 | B | Pada hidden services, komunikasi berlangsung sepenuhnya dalam jaringan Tor tanpa melalui exit node |
| 6 | B | File prefetch merupakan artefak Windows yang mencatat eksekusi program |
| 7 | B | Pseudonymous berarti transaksi publik tetapi alamat tidak langsung terkait identitas real |
| 8 | C | Cluster analysis mengelompokkan alamat Bitcoin yang kemungkinan milik satu entitas |
| 9 | D | Monero menggunakan ring signatures, stealth addresses, dan RingCT |
| 10 | C | SMTP (Simple Mail Transfer Protocol) digunakan untuk pengiriman email |
| 11 | B | POP3 mengunduh ke lokal (bukti di device), IMAP menyimpan di server (bukti di server + cache) |
| 12 | B | Header Received dibaca dari bawah ke atas untuk menelusuri jalur dari asal ke tujuan |
| 13 | C | Field "From" dapat diatur bebas oleh pengirim sehingga paling mudah dipalsukan |
| 14 | C | SPF (Sender Policy Framework) memverifikasi server pengirim melalui DNS TXT record |
| 15 | C | DKIM menggunakan public key cryptography untuk tanda tangan digital pada email |
| 16 | C | Ketiga mekanisme gagal menunjukkan email tidak berasal dari domain yang diklaim |
| 17 | B | Whaling secara spesifik menargetkan pimpinan tingkat tinggi (C-suite/eksekutif) |
| 18 | C | Subdomain abuse menempatkan domain asli sebagai subdomain dari domain penyerang |
| 19 | C | .pst (Personal Storage Table) adalah format database email Outlook |
| 20 | C | Preservasi email asli adalah langkah pertama untuk menjaga integritas bukti |

---

### Bagian B: Uraian

#### Jawaban Soal 1 ⭐

**Perbedaan tiga lapisan web:**

| Aspek | Surface Web | Deep Web | Dark Web |
|-------|------------|----------|----------|
| **Proporsi** | ~4-5% | ~90-95% | <1% |
| **Akses** | Browser biasa | Login/autentikasi | Software khusus (Tor) |
| **Indeksasi** | Terindeks mesin pencari | Tidak terindeks | Tidak terindeks |
| **Contoh 1** | Google, Wikipedia | Email (Gmail) | Marketplace .onion |
| **Contoh 2** | Situs berita publik | Internet banking | SecureDrop |

---

#### Jawaban Soal 2 ⭐

**Tiga jenis relay Tor:**

1. **Guard Node** — Relay pertama yang menerima koneksi dari pengguna. Mengetahui IP asli pengguna tetapi tidak mengetahui tujuan akhir maupun konten.

2. **Middle Node** — Relay tengah yang meneruskan trafik. Tidak mengetahui IP pengguna, tujuan akhir, maupun konten. Hanya mengetahui guard node sebelumnya dan exit node berikutnya.

3. **Exit Node** — Relay terakhir yang menghubungkan ke internet terbuka. Mengetahui tujuan akhir dan konten (jika non-HTTPS), tetapi tidak mengetahui IP asli pengguna.

---

#### Jawaban Soal 3 ⭐

**Onion routing** adalah teknik komunikasi anonim di mana data dienkripsi dalam beberapa lapisan sebelum dikirim melalui serangkaian relay. Disebut "onion" karena setiap relay hanya membuka (mendekripsi) satu lapisan enkripsi, seperti mengupas kulit bawang lapis demi lapis. Guard node membuka lapisan pertama, middle node membuka lapisan kedua, dan exit node membuka lapisan terakhir untuk mengakses data asli.

---

#### Jawaban Soal 4 ⭐

**Lima artefak forensik Tor pada Windows:**

1. **Folder Tor Browser** — `%USERPROFILE%\Desktop\Tor Browser\`
2. **Prefetch files** — `C:\Windows\Prefetch\TOR.EXE-*.pf` dan `FIREFOX.EXE-*.pf`
3. **Registry UserAssist** — Entri di `NTUSER.DAT\...\UserAssist` (encoded ROT13)
4. **DNS Cache** — Query ke `torproject.org` atau domain terkait
5. **Network connections** — Koneksi ke IP relay Tor pada port 9001/9030

---

#### Jawaban Soal 5 ⭐⭐

**Bitcoin pseudonymous:**
Bitcoin disebut pseudonymous karena semua transaksi tercatat secara publik dan permanen di blockchain, tetapi alamat wallet tidak langsung terkait dengan identitas dunia nyata. Berbeda dengan anonymous (seperti Monero), di mana detail transaksi tersembunyi.

**Teknik pelacakan:**

1. **Cluster analysis** — Mengelompokkan alamat yang kemungkinan milik satu entitas berdasarkan pola common-input-ownership
2. **Transaction graph** — Memvisualisasi aliran dana untuk mengidentifikasi pola
3. **Exchange identification** — Mengidentifikasi alamat milik exchange yang memiliki data KYC
4. **Blockchain explorer** — Menggunakan tools seperti Blockchain.com untuk menelusuri riwayat transaksi

---

#### Jawaban Soal 6 ⭐⭐

**Perbandingan forensik Bitcoin vs Monero:**

| Aspek | Bitcoin | Monero |
|-------|---------|--------|
| **Ledger** | Transparan — semua transaksi publik | Opaque — detail transaksi tersembunyi |
| **Pengirim** | Alamat terlihat | Tersembunyi (stealth addresses) |
| **Jumlah** | Terlihat | Tersembunyi (RingCT) |
| **Tracing** | Dapat dilacak dengan cluster analysis | Sangat sulit karena ring signatures |

**Monero lebih sulit dilacak** karena menggunakan: (1) ring signatures yang mencampur transaksi pengguna dengan decoy, (2) stealth addresses yang menghasilkan alamat sekali pakai, dan (3) RingCT yang menyembunyikan jumlah transaksi.

---

#### Jawaban Soal 7 ⭐⭐

**Metodologi OSINT untuk investigasi dark web:**

1. **DEFINE** — Tentukan tujuan investigasi, target, dan ruang lingkup. Contoh: mengidentifikasi penjual data curian di marketplace.
2. **DISCOVER** — Kumpulkan data dari berbagai sumber: mesin pencari dark web (Ahmia.fi), paste sites, forum, social media, cached content.
3. **ANALYZE** — Analisis dan korelasi temuan: hubungkan username, bahasa, waktu posting, cryptocurrency addresses lintas platform.
4. **VALIDATE** — Verifikasi keakuratan informasi melalui sumber independen dan cross-referencing.
5. **REPORT** — Dokumentasi lengkap dengan timeline, bukti, dan rekomendasi tindak lanjut.

---

#### Jawaban Soal 8 ⭐⭐

**Alur pengiriman email:**

1. Pengirim menulis email di **MUA** (Mail User Agent), misalnya Outlook
2. MUA mengirim ke **MSA** (Mail Submission Agent) via SMTP port 587
3. MSA meneruskan ke **MTA** (Mail Transfer Agent) pengirim
4. MTA pengirim melakukan **DNS MX lookup** untuk domain penerima
5. MTA pengirim mengirim ke **MTA penerima** via SMTP port 25
6. MTA penerima meneruskan ke **MDA** (Mail Delivery Agent)
7. MDA menyimpan email ke **mailbox** penerima
8. Penerima mengakses email melalui **MUA** via IMAP/POP3

Setiap MTA yang memproses email menambahkan header "Received" ke email.

---

#### Jawaban Soal 9 ⭐⭐

**Perbedaan SPF, DKIM, dan DMARC:**

| Mekanisme | Fungsi | Yang Diverifikasi |
|-----------|--------|-------------------|
| **SPF** | Memverifikasi server pengirim melalui DNS TXT record | Apakah IP server pengirim terotorisasi oleh domain |
| **DKIM** | Memverifikasi integritas email dengan tanda tangan digital | Apakah konten email tidak dimodifikasi dan berasal dari domain yang diklaim |
| **DMARC** | Kebijakan gabungan SPF+DKIM | Instruksi ke server penerima jika email gagal autentikasi (none/quarantine/reject) |

Ketiganya bekerja bersama sebagai defense-in-depth untuk melawan email spoofing.

---

#### Jawaban Soal 10 ⭐⭐

**Membaca header Received:**
Header Received dibaca dari **bawah ke atas**. Header paling bawah ditambahkan oleh server pertama yang menerima email (terdekat ke pengirim), sedangkan header paling atas ditambahkan oleh server terakhir (terdekat ke penerima).

**Contoh:**
```
Received: from mail.kodam.mil.id (10.20.30.1)
  by inbox.lantamal.mil.id; 14:22:30 +0700     ← [3] Terakhir

Received: from webmail.kodam.mil.id (10.20.30.5)
  by mail.kodam.mil.id; 14:22:25 +0700         ← [2] Tengah

Received: from [172.16.50.100]
  by webmail.kodam.mil.id; 14:22:20 +0700       ← [1] ASAL
```

**IP pengirim asli: 172.16.50.100** (dari header Received paling bawah).

---

#### Jawaban Soal 11 ⭐⭐

**Lima jenis serangan phishing:**

1. **Phishing** — Email massal meniru entitas terpercaya (bank, layanan). Target: massal/umum.
2. **Spear Phishing** — Email yang dipersonalisasi untuk individu spesifik berdasarkan riset OSINT. Target: individu tertentu.
3. **Whaling** — Phishing yang menargetkan pimpinan/eksekutif tingkat tinggi. Target: C-suite, jenderal, direktur.
4. **BEC (Business Email Compromise)** — Memalsukan identitas pimpinan untuk memerintahkan transfer dana. Target: staf keuangan/administrasi.
5. **Clone Phishing** — Menduplikasi email legitimate yang pernah diterima korban dengan mengganti attachment/link. Target: penerima email asli.

---

#### Jawaban Soal 12 ⭐⭐⭐

**Langkah investigasi phishing email:**

1. **Preservasi** — Simpan email asli dalam format .eml/.msg. Jangan forward karena bisa mengubah header. Buat hash file untuk integritas.
2. **Header Analysis** — Periksa field Received, Return-Path, X-Originating-IP, Message-ID. Verifikasi hasil SPF, DKIM, DMARC.
3. **URL Analysis** — Analisis semua URL tanpa mengklik. Gunakan tools seperti VirusTotal, URLScan.io, atau sandbox untuk analisis.
4. **Attachment Analysis** — Submit lampiran ke sandbox (Any.run, VirusTotal). Periksa hash, tipe file sebenarnya, macro, dan behavior.
5. **IOC Extraction** — Kumpulkan semua Indicators of Compromise: IP address, domain, URL, email address, file hash.
6. **Threat Intelligence** — Cek IOC terhadap platform TI (MISP, OTX) untuk menentukan campaign atau aktor yang dikenal.
7. **Scope Assessment** — Tentukan berapa banyak penerima yang terkena, siapa yang mengklik, dan potensi dampak.
8. **Documentation** — Buat laporan forensik lengkap dengan timeline, bukti, IOC, dan rekomendasi.

---

#### Jawaban Soal 13 ⭐⭐⭐

**Cara mendeteksi email spoofed (5+ indikator):**

1. **Return-Path berbeda dari From** — Return-Path menunjuk ke domain berbeda dari alamat pengirim yang ditampilkan.
2. **Header Received menunjuk domain asing** — Server pengirim aktual bukan milik domain yang diklaim.
3. **SPF=fail** — IP server pengirim tidak terotorisasi oleh DNS record domain pengirim.
4. **DKIM=none/fail** — Tidak ada tanda tangan digital atau tanda tangan tidak valid.
5. **DMARC=fail** — Email gagal verifikasi DMARC yang mengkombinasikan SPF dan DKIM.
6. **X-Originating-IP tidak sesuai** — IP berasal dari lokasi/network yang tidak berhubungan dengan organisasi pengirim.
7. **Message-ID domain tidak cocok** — Domain dalam Message-ID berbeda dari domain pengirim yang diklaim.
8. **X-Mailer mencurigakan** — Penggunaan mass mailer tools seperti PHPMailer.

---

#### Jawaban Soal 14 ⭐⭐⭐

**Lokasi artefak email pada berbagai klien:**

| Client | Format | Lokasi |
|--------|--------|--------|
| **Outlook (POP)** | .pst | `%USERPROFILE%\Documents\Outlook Files\` |
| **Outlook (Exchange)** | .ost | `%LOCALAPPDATA%\Microsoft\Outlook\` |
| **Thunderbird** | Mbox | `%APPDATA%\Thunderbird\Profiles\` |
| **Windows Mail** | .eml | `%LOCALAPPDATA%\Comms\Unistore\data\` |

**Peran Autopsy:**
Autopsy dapat menganalisis file .pst dan .mbox langsung dari forensic image tanpa perlu menginstal email client. Autopsy memiliki Email Parser module yang mengekstrak email, attachment, metadata, dan memungkinkan pencarian keyword serta timeline analysis pada seluruh koleksi email.

---

#### Jawaban Soal 15 ⭐⭐⭐

**Pertimbangan legal dan etika investigasi dark web militer:**

**Regulasi yang relevan:**
- **UU ITE** (UU No. 1 Tahun 2024) — Mengatur penyadapan dan akses sistem elektronik
- **UU Pertahanan Negara** (UU No. 3 Tahun 2002) — Dasar hukum operasi pertahanan
- **UU Intelijen Negara** (UU No. 17 Tahun 2011) — Kewenangan pengumpulan intelijen
- **Peraturan BSSN** — Standar keamanan siber nasional

**Pertimbangan etika:**
1. **Proporsionalitas** — Tindakan investigasi harus sebanding dengan ancaman
2. **Legalitas** — Operasi harus memiliki dasar hukum yang jelas dan otorisasi komando
3. **Minimalisasi** — Hanya mengumpulkan data yang diperlukan untuk investigasi
4. **Dokumentasi** — Setiap langkah harus terdokumentasi untuk akuntabilitas
5. **Proteksi privasi** — Menghormati hak privasi kecuali ada dasar hukum yang kuat

---

### Bagian C: Studi Kasus

#### Studi Kasus 1: Penjualan Data Personel Militer di Dark Web

**Jawaban 1 — Langkah OSINT (20 poin):**

1. **Username Analysis** — Cari "gh0st_id" di berbagai platform dark web, surface web, paste sites, dan social media. Gunakan tools seperti Maltego dan Sherlock.
2. **Linguistic Analysis** — Analisis bahasa posting (Bahasa Indonesia/Inggris, gaya penulisan, slang) untuk mempersempit profil.
3. **Temporal Analysis** — Catat waktu posting dan aktivitas online untuk menentukan timezone dan pola aktivitas.
4. **Transaction History** — Meskipun Monero sulit dilacak, analisis posting sebelumnya apakah pernah menggunakan Bitcoin.
5. **Cross-Platform Correlation** — Cari username serupa di forum lain, GitHub, media sosial.
6. **Stylometry** — Analisis gaya penulisan untuk korelasi dengan akun/postingan lain.
7. **Technical Fingerprinting** — Analisis metadata screenshot/sampel data yang dipublikasikan penjual.
8. **Forum Intelligence** — Monitor interaksi penjual dengan buyer lain untuk mendapatkan informasi tambahan.

**Jawaban 2 — Monero vs Bitcoin (15 poin):**

Monero mempersulit investigasi karena: (1) **ring signatures** mencampur transaksi dengan decoy sehingga pengirim asli tidak bisa diidentifikasi, (2) **stealth addresses** menghasilkan alamat sekali pakai untuk setiap transaksi, (3) **RingCT** menyembunyikan jumlah transaksi.

**Teknik forensik yang masih mungkin:**
- Analisis endpoint: forensik perangkat tersangka jika teridentifikasi
- Exchange analysis: jika tersangka pernah menukar Monero melalui exchange dengan KYC
- Timing analysis: korelasi waktu transaksi dengan aktivitas online
- Network analysis: monitoring node Monero untuk metadata koneksi

**Jawaban 3 — Verifikasi tanpa membeli (15 poin):**

1. Bandingkan sampel data dengan format database internal yang diketahui
2. Verifikasi NRP/nama dalam sampel dengan data personel yang sah
3. Periksa keakuratan informasi seperti pangkat, unit, dan penugasan
4. Cek apakah data sesuai dengan periode waktu tertentu
5. Analisis konsistensi format data (encoding, field structure)

**Jawaban 4 — Aspek legal dan etika (10 poin):**

- Pastikan otorisasi dari komando yang berwenang
- Operasi berdasarkan UU Intelijen dan UU ITE
- Dokumentasi lengkap setiap langkah investigasi
- Jangan melakukan pembelian data tanpa izin otoritas hukum
- Koordinasi dengan BSSN dan Bareskrim Polri jika diperlukan

**Jawaban 5 — Langkah mitigasi (10 poin):**

1. Identifikasi sumber kebocoran melalui audit akses internal
2. Reset kredensial seluruh personel yang datanya terekspos
3. Notifikasi personel yang terkena dampak
4. Perkuat access control dan monitoring pada database sensitif
5. Lakukan security awareness training tentang insider threat

---

#### Studi Kasus 2: Kampanye Spear Phishing terhadap Perwira TNI

**Jawaban 1 — Analisis header (20 poin):**

Lima indikator email spoofed/phishing:

1. **Return-Path berbeda dari From** — From menunjukkan `sekjen@kemenhan.go.id`, tetapi Return-Path menunjukkan `admin@kemenhan-portal.xyz` (domain berbeda).
2. **Domain palsu** — Domain `kemenhan-portal.xyz` bukan domain resmi Kemenhan (`kemenhan.go.id`). Ini adalah domain pihak ketiga yang dibuat menyerupai domain resmi.
3. **SPF=fail** — IP 198.51.100.75 tidak terotorisasi mengirim email atas nama `kemenhan.go.id`.
4. **DKIM=none** — Tidak ada tanda tangan DKIM, email asli dari Kemenhan seharusnya memiliki DKIM.
5. **DMARC=fail (p=REJECT)** — Domain asli memiliki kebijakan DMARC reject, artinya email ini seharusnya ditolak tetapi tetap terkirim karena konfigurasi server penerima.
6. **X-Mailer: PHPMailer** — Tools mass mailing, bukan klien email standar pemerintah.
7. **Message-ID domain** — `@kemenhan-portal.xyz` bukan domain resmi Kemenhan.

**Jawaban 2 — Double extension (10 poin):**

File "Undangan_Rakor_Kemenhan_2026.pdf.exe" menggunakan teknik **double extension**. Windows secara default menyembunyikan ekstensi file, sehingga pengguna hanya melihat ".pdf" dan mengira file adalah dokumen PDF, padahal file sebenarnya adalah executable (.exe) yang kemungkinan berisi malware. Teknik social engineering yang digunakan: (1) menggunakan topik relevan (undangan rapat), (2) mengklaim berasal dari institusi berwenang (Kemenhan), dan (3) menambahkan kata "URGENT" untuk menciptakan urgensi.

**Jawaban 3 — Incident response (20 poin):**

1. **Containment** — Isolasi workstation yang terinfeksi dari jaringan. Block domain `kemenhan-portal.xyz` dan IP `198.51.100.75` di firewall.
2. **Assessment** — Identifikasi semua 5 penerima email. Tentukan siapa yang mengklik (2 perwira) dan siapa yang hanya membuka email.
3. **Evidence Collection** — Ambil memory dump dan forensic image dari workstation terinfeksi sebelum cleanup.
4. **Eradication** — Scan malware pada workstation terinfeksi. Reset kredensial kedua perwira yang mengklik. Periksa persistence mechanisms.
5. **Recovery** — Restore workstation dari backup bersih jika diperlukan. Verifikasi integritas sistem.
6. **Notification** — Laporkan insiden ke BSSN dan Tim CSIRT internal.
7. **Lessons Learned** — Dokumentasi insiden, analisis gap, dan update prosedur.

**Jawaban 4 — Forensik workstation (15 poin):**

1. **Jangan matikan komputer** — Lakukan live acquisition terlebih dahulu
2. **Capture volatile data** — Memory dump menggunakan tools seperti WinPMEM
3. **Network capture** — Rekam trafik jaringan untuk identifikasi C2 communication
4. **Forensic imaging** — Buat full disk image menggunakan FTK Imager
5. **Log collection** — Kumpulkan event logs, prefetch files, registry hives
6. **Malware analysis** — Analisis file .exe dalam sandbox environment
7. **Timeline analysis** — Buat timeline aktivitas sejak email diterima

**Jawaban 5 — Pencegahan (5 poin):**

1. Implementasi email filtering yang memvalidasi SPF/DKIM/DMARC dan menolak email yang gagal
2. Security awareness training tentang phishing dengan simulasi berkala
3. Kebijakan untuk menampilkan ekstensi file dan memblokir eksekusi .exe dari email
4. Implementasi sandboxing untuk lampiran email
5. Pelaporan phishing yang mudah dan cepat (tombol "Report Phishing" di email client)

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
