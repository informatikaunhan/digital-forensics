# Modul 13: Forensik Dark Web dan Investigasi Email

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 13  
**Topik:** Forensik Dark Web dan Investigasi Email  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Menjelaskan konsep surface web, deep web, dan dark web serta perbedaannya
2. Memahami arsitektur jaringan Tor dan mekanisme anonimisasi
3. Menerapkan teknik investigasi dark web dan OSINT untuk keperluan forensik
4. Memahami dasar forensik cryptocurrency dan blockchain analysis
5. Menganalisis header email untuk pelacakan sumber komunikasi
6. Menjelaskan mekanisme autentikasi email: SPF, DKIM, dan DMARC
7. Melakukan investigasi phishing dan social engineering forensics
8. Mengekstrak artefak email dari berbagai email client

---

## 1. Lapisan Web: Surface, Deep, dan Dark Web

### 1.1 Konsep Tiga Lapisan Web

> **Surface Web** adalah bagian dari World Wide Web yang terindeks oleh mesin pencari konvensional seperti Google, Bing, atau Yahoo. Surface web hanya mencakup sekitar 4-5% dari keseluruhan konten internet.

> **Deep Web** adalah bagian internet yang tidak terindeks oleh mesin pencari konvensional, mencakup konten yang dilindungi password, database privat, konten dinamis, dan halaman yang memerlukan autentikasi. Deep web mencakup sekitar 90-95% dari keseluruhan internet.

> **Dark Web** adalah subset kecil dari deep web yang sengaja disembunyikan dan hanya dapat diakses melalui perangkat lunak khusus seperti Tor Browser. Dark web menggunakan overlay network yang mengenkripsi trafik dan menyembunyikan identitas pengguna.

| Aspek | Surface Web | Deep Web | Dark Web |
|-------|-------------|----------|----------|
| **Akses** | Browser biasa | Login/autentikasi | Software khusus (Tor) |
| **Indeksasi** | Terindeks mesin pencari | Tidak terindeks | Tidak terindeks |
| **Proporsi** | ~4-5% | ~90-95% | <1% |
| **Contoh** | Google, Wikipedia, berita | Email, perbankan online, database akademik | Hidden services (.onion) |
| **Anonimitas** | Rendah | Sedang | Tinggi |
| **Legalitas** | Legal | Legal | Campuran (legal & ilegal) |

![Diagram Lapisan Web](images/p13-01-web-layers.png)

*Gambar 13.1: Ilustrasi tiga lapisan web — surface, deep, dan dark web*

#### 1.1.1 Konten Deep Web

Deep web mencakup berbagai konten yang sah dan legal, antara lain:

1. **Database Akademik**: Jurnal ilmiah, repositori penelitian (JSTOR, IEEE Xplore)
2. **Sistem Perbankan**: Internet banking, sistem pembayaran
3. **Rekam Medis**: Sistem informasi rumah sakit, BPJS
4. **Email**: Gmail, Outlook, Yahoo Mail
5. **Sistem Pemerintahan**: Database kependudukan, pajak, militer
6. **Cloud Storage**: Google Drive, Dropbox, OneDrive yang memerlukan login
7. **Intranet Militer**: Jaringan internal TNI, Kemenhan

#### 1.1.2 Konten Dark Web

Dark web memiliki konten yang beragam, baik legal maupun ilegal:

**Konten Legal:**
- Komunikasi anonim untuk jurnalis dan aktivis
- Layanan whistleblowing (SecureDrop)
- Forum diskusi privasi dan kebebasan berekspresi
- Mirror situs berita untuk negara dengan sensor ketat

**Konten Ilegal:**
- Marketplace perdagangan narkotika dan senjata
- Forum peretasan dan jual beli data curian
- Layanan ransomware-as-a-service
- Perdagangan identitas dan dokumen palsu
- Konten eksploitasi anak (CSAM)

#### Solved Problem 1 ⭐

**Soal:** Seorang investigator forensik di Kodam menemukan bahwa seorang personel mengakses situs dengan domain `.onion`. Jelaskan apa yang dimaksud domain .onion dan mengapa hal ini menjadi perhatian keamanan!

**Penyelesaian:**

*Step 1:* Identifikasi karakteristik domain .onion

Domain `.onion` adalah top-level domain khusus yang hanya dapat diakses melalui jaringan Tor. Alamat .onion terdiri dari string alfanumerik acak (16 karakter untuk v2, 56 karakter untuk v3) yang dihasilkan dari kunci kriptografi.

*Step 2:* Analisis implikasi keamanan

| Aspek | Implikasi |
|-------|-----------|
| Anonimitas | Pengguna menyembunyikan identitas dan lokasi |
| Enkripsi | Trafik dienkripsi berlapis sehingga sulit dimonitor |
| Untraceable | Server .onion tidak dapat dilacak lokasi fisiknya |
| Bypass filtering | Melewati filtering jaringan militer |

*Step 3:* Kesimpulan

**Jawaban:** Domain .onion adalah alamat hidden service di jaringan Tor yang hanya bisa diakses melalui Tor Browser. Hal ini menjadi perhatian keamanan militer karena: (1) mengindikasikan personel berupaya mengakses internet secara anonim, kemungkinan untuk menghindari monitoring jaringan Kodam, (2) potensi kebocoran informasi sensitif melalui kanal yang tidak termonitor, dan (3) kemungkinan akses ke konten ilegal atau komunikasi dengan pihak yang tidak berwenang.

---

### 1.2 Arsitektur Jaringan Tor

> **Tor (The Onion Router)** adalah jaringan overlay yang menganonimkan trafik internet dengan merutekan koneksi melalui serangkaian relay terenkripsi. Tor dikembangkan awalnya oleh U.S. Naval Research Laboratory dan dikelola oleh Tor Project, Inc.

#### 1.2.1 Komponen Utama Tor

1. **Tor Client/Browser**: Aplikasi yang menginisiasi koneksi ke jaringan Tor
2. **Directory Authorities**: Server yang menyimpan daftar relay aktif dan konsensus jaringan
3. **Entry/Guard Node**: Relay pertama yang mengetahui IP asli pengguna
4. **Middle/Relay Node**: Relay tengah yang hanya mengetahui node sebelum dan sesudahnya
5. **Exit Node**: Relay terakhir yang menghubungkan ke internet terbuka (untuk non-.onion)
6. **Bridge Relay**: Relay tersembunyi untuk menghindari pemblokiran Tor
7. **Hidden Service (Onion Service)**: Server yang beroperasi di dalam jaringan Tor

#### 1.2.2 Proses Onion Routing

Mekanisme onion routing menggunakan enkripsi berlapis:

```
[Data Asli]
    ↓ Enkripsi dengan kunci Exit Node
[Layer 3: Encrypted]
    ↓ Enkripsi dengan kunci Middle Node
[Layer 2: Encrypted]
    ↓ Enkripsi dengan kunci Guard Node
[Layer 1: Encrypted]
    ↓ Kirim ke Guard Node
```

Setiap relay hanya dapat membuka satu lapisan enkripsi, sehingga:
- **Guard Node**: Mengetahui IP pengguna, tetapi tidak mengetahui tujuan atau konten
- **Middle Node**: Tidak mengetahui IP pengguna maupun tujuan akhir
- **Exit Node**: Mengetahui tujuan dan konten (jika tidak HTTPS), tetapi tidak mengetahui IP pengguna

#### 1.2.3 Hidden Services (.onion)

Untuk mengakses hidden service, proses lebih kompleks:

1. Hidden service membuat **introduction points** pada beberapa relay
2. Hidden service mempublikasikan descriptor ke **distributed hash table (DHT)**
3. Klien mendapatkan descriptor dan memilih **rendezvous point**
4. Klien mengirim pesan melalui introduction point ke hidden service
5. Hidden service terhubung ke rendezvous point
6. Komunikasi berlangsung melalui rendezvous point

#### Solved Problem 2 ⭐⭐

**Soal:** Jelaskan mengapa middle relay pada jaringan Tor tidak dapat mengetahui identitas pengguna maupun tujuan akhir komunikasi!

**Penyelesaian:**

*Step 1:* Pahami posisi middle relay dalam circuit

Middle relay berada di posisi tengah circuit Tor, antara guard node dan exit node.

*Step 2:* Analisis informasi yang dimiliki middle relay

| Informasi | Dimiliki? | Penjelasan |
|-----------|-----------|------------|
| IP pengguna asli | ❌ | Hanya guard node yang mengetahui |
| IP guard node | ✅ | Menerima data dari guard node |
| IP exit node | ✅ | Meneruskan data ke exit node |
| Tujuan akhir | ❌ | Terenkripsi dalam lapisan yang hanya bisa dibuka exit node |
| Konten data | ❌ | Terenkripsi berlapis |

*Step 3:* Kesimpulan

**Jawaban:** Middle relay tidak dapat mengetahui identitas pengguna karena lapisan enkripsi pertama (dari guard node ke middle) sudah menyembunyikan IP asli pengguna. Middle relay hanya melihat IP guard node sebagai sumber dan IP exit node sebagai tujuan. Konten dan tujuan akhir juga tidak diketahui karena masih terenkripsi dalam lapisan yang hanya dapat didekripsi oleh exit node. Desain ini memastikan bahwa tidak ada satu pun relay yang memiliki informasi lengkap tentang koneksi.

---

### 1.3 Teknik Investigasi Dark Web

#### 1.3.1 Pendekatan Investigasi

Investigasi dark web dalam konteks militer memerlukan pendekatan khusus:

1. **Passive Monitoring**: Mengumpulkan data dari sumber terbuka tanpa interaksi aktif
2. **Active Investigation**: Mengakses dark web secara langsung menggunakan identitas tersamarkan
3. **Technical Analysis**: Menganalisis infrastruktur teknis hidden services
4. **OSINT (Open Source Intelligence)**: Menggunakan informasi terbuka untuk mengkorelasikan identitas

#### 1.3.2 Tools Investigasi Dark Web

| Tool | Fungsi | Jenis |
|------|--------|-------|
| **Tor Browser** | Mengakses .onion sites | Browser |
| **Ahmia.fi** | Mesin pencari dark web | Search Engine |
| **Onion.live** | Monitoring status .onion sites | Monitoring |
| **Maltego** | OSINT dan link analysis | Intelligence |
| **Hunchly** | Web capture dan dokumentasi | Documentation |
| **ExoneraTor** | Memeriksa apakah IP adalah relay Tor | Verification |
| **OnionScan** | Scanning keamanan hidden services | Scanner |

#### 1.3.3 Teknik De-anonimisasi

Meskipun Tor dirancang untuk anonimitas, terdapat teknik yang dapat mengungkap identitas:

1. **Traffic Correlation Attack**: Menghubungkan trafik masuk dan keluar jaringan Tor berdasarkan timing dan volume
2. **Browser Exploitation**: Mengeksploitasi kerentanan browser untuk mengungkap IP asli
3. **Operational Security (OPSEC) Failure**: Kesalahan pengguna seperti login dengan akun yang sama
4. **Cryptocurrency Tracing**: Melacak transaksi Bitcoin yang tidak dilakukan dengan benar
5. **Server Misconfiguration**: Konfigurasi hidden service yang membocorkan IP asli

#### Solved Problem 3 ⭐⭐

**Soal:** Tim siber Lantamal menemukan indikasi penggunaan Tor di jaringan pangkalan. Sebutkan tiga artefak forensik yang dapat membuktikan penggunaan Tor pada workstation Windows!

**Penyelesaian:**

*Step 1:* Identifikasi artefak Tor pada Windows

*Step 2:* Analisis lokasi dan jenis artefak

| No | Artefak | Lokasi | Keterangan |
|----|---------|--------|------------|
| 1 | Tor Browser folder | `%USERPROFILE%\Desktop\Tor Browser\` atau `Downloads` | Bundle Tor Browser yang diunduh |
| 2 | Prefetch files | `C:\Windows\Prefetch\` | File `TOR.EXE-*.pf` atau `FIREFOX.EXE-*.pf` dari Tor Browser |
| 3 | Registry entries | `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist` | Bukti eksekusi Tor Browser |
| 4 | DNS cache | Memory dump atau `ipconfig /displaydns` | Query ke `torproject.org` |
| 5 | Network connections | Netstat, event logs | Koneksi ke known Tor entry nodes (port 9001, 9030) |

**Jawaban:** Tiga artefak forensik utama: (1) File prefetch `TOR.EXE-*.pf` di folder `C:\Windows\Prefetch\` yang membuktikan program pernah dieksekusi beserta timestamp-nya, (2) Entry di registry UserAssist yang mencatat eksekusi Tor Browser dengan ROT13 encoding, dan (3) Artefak jaringan berupa log koneksi ke IP node Tor yang dikenal pada port 9001/9030.

---

## 2. Forensik Cryptocurrency

### 2.1 Dasar Cryptocurrency dan Blockchain

> **Cryptocurrency** adalah mata uang digital yang menggunakan kriptografi untuk keamanan transaksi dan beroperasi secara desentralisasi tanpa otoritas pusat. Bitcoin, yang diciptakan oleh Satoshi Nakamoto pada 2009, adalah cryptocurrency pertama dan paling terkenal.

> **Blockchain** adalah teknologi distributed ledger yang mencatat semua transaksi dalam blok-blok yang saling terhubung secara kriptografi. Setiap blok mengandung hash dari blok sebelumnya, timestamp, dan data transaksi.

#### 2.1.1 Komponen Utama Bitcoin

1. **Wallet Address**: Alamat publik untuk menerima Bitcoin (hash dari public key)
2. **Private Key**: Kunci rahasia untuk menandatangani transaksi
3. **Transaction**: Transfer Bitcoin dari satu alamat ke alamat lain
4. **Block**: Kumpulan transaksi yang divalidasi oleh miner
5. **Mining**: Proses validasi transaksi dan penambahan blok baru

#### 2.1.2 Pseudonymity vs Anonymity

Bitcoin bersifat **pseudonymous**, bukan anonymous:
- Semua transaksi tercatat secara publik di blockchain
- Alamat Bitcoin tidak langsung terkait identitas, tetapi dapat dilacak
- Analisis pola transaksi dapat mengungkap hubungan antar alamat
- Titik konversi (exchange) sering memerlukan KYC (Know Your Customer)

### 2.2 Teknik Forensik Cryptocurrency

| Teknik | Deskripsi | Tools |
|--------|-----------|-------|
| **Cluster Analysis** | Mengelompokkan alamat yang dimiliki entitas sama | Chainalysis, Crystal |
| **Transaction Graph** | Memvisualisasi aliran Bitcoin antar alamat | Blockchain.com, OXT |
| **Heuristic Analysis** | Menggunakan pola seperti common-input-ownership | Wallet Explorer |
| **Exchange Identification** | Mengidentifikasi alamat milik exchange | Chainalysis |
| **Mixing Detection** | Mendeteksi penggunaan mixing/tumbling services | Reactor |

#### 2.2.1 Blockchain Explorer

Blockchain explorer adalah tools yang memungkinkan investigator menelusuri transaksi:

- **Blockchain.com Explorer**: Explorer Bitcoin paling populer
- **Etherscan.io**: Explorer untuk Ethereum
- **Blockchair.com**: Multi-cryptocurrency explorer
- **OXT.me**: Explorer Bitcoin dengan fitur analisis lanjutan

#### Solved Problem 4 ⭐⭐

**Soal:** Investigator menemukan alamat Bitcoin `1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa` pada perangkat tersangka. Jelaskan langkah-langkah forensik untuk melacak transaksi!

**Penyelesaian:**

*Step 1:* Akses blockchain explorer

Buka Blockchain.com Explorer dan masukkan alamat Bitcoin tersebut.

*Step 2:* Analisis informasi yang tersedia

| Data | Informasi yang Diperoleh |
|------|--------------------------|
| Balance | Saldo saat ini di alamat tersebut |
| Total Received | Total Bitcoin yang pernah diterima |
| Total Sent | Total Bitcoin yang pernah dikirim |
| Transactions | Daftar semua transaksi masuk dan keluar |
| First Seen | Tanggal transaksi pertama |

*Step 3:* Telusuri aliran dana

- Identifikasi alamat pengirim dan penerima untuk setiap transaksi
- Cari alamat yang dikenal (exchange, marketplace)
- Buat grafik aliran dana untuk visualisasi

*Step 4:* Korelasi dengan data lain

**Jawaban:** Langkah forensik: (1) Masukkan alamat ke blockchain explorer untuk melihat riwayat transaksi lengkap, (2) Identifikasi semua alamat pengirim dan penerima yang terhubung, (3) Gunakan cluster analysis untuk mengelompokkan alamat yang mungkin dimiliki entitas yang sama, (4) Identifikasi alamat milik exchange yang dikenal karena exchange umumnya memiliki data KYC, dan (5) Buat timeline transaksi dan korelasikan dengan aktivitas tersangka lainnya.

---

## 3. OSINT untuk Investigasi Dark Web

### 3.1 Teknik OSINT

> **OSINT (Open Source Intelligence)** adalah pengumpulan dan analisis informasi dari sumber-sumber yang tersedia secara publik untuk mendukung pengambilan keputusan intelijen.

#### 3.1.1 Sumber OSINT untuk Dark Web

1. **Mesin Pencari Dark Web**: Ahmia.fi, DarkSearch.io
2. **Dark Web Monitoring Services**: Recorded Future, DarkOwl
3. **Paste Sites**: Pastebin, Ghostbin (sering digunakan untuk membagikan data curian)
4. **Forum Monitoring**: Pemantauan forum underground
5. **Social Media OSINT**: Korelasi akun dark web dengan media sosial
6. **Domain WHOIS**: Informasi registrasi domain terkait
7. **Cached Content**: Wayback Machine, Google Cache

#### 3.1.2 Metodologi OSINT Investigation

```
1. DEFINE    → Tentukan target investigasi
2. DISCOVER  → Kumpulkan data dari berbagai sumber
3. ANALYZE   → Analisis dan korelasi temuan
4. VALIDATE  → Verifikasi keakuratan informasi
5. REPORT    → Dokumentasi dan pelaporan
```

#### Solved Problem 5 ⭐⭐⭐

**Soal:** Tim intelijen siber TNI mendapati bahwa data personel militer dijual di sebuah marketplace dark web. Jelaskan metodologi OSINT yang akan Anda gunakan untuk mengidentifikasi pelaku!

**Penyelesaian:**

*Step 1:* Define — Tentukan target

Target investigasi adalah penjual data personel militer di marketplace dark web. Informasi yang dicari: identitas penjual, metode mendapatkan data, dan cakupan data yang bocor.

*Step 2:* Discover — Pengumpulan data

| Sumber | Data yang Dikumpulkan |
|--------|----------------------|
| Marketplace listing | Username penjual, deskripsi produk, harga, metode pembayaran |
| Profil penjual | Riwayat transaksi, reputasi, waktu registrasi |
| Cryptocurrency | Alamat Bitcoin/Monero dari penjual |
| Forum underground | Posting dengan username yang sama |
| Paste sites | Data dump yang mungkin diunggah sebagai sampel |
| Surface web | Pencarian username yang sama di platform publik |

*Step 3:* Analyze — Korelasi data

- Cross-reference username di berbagai platform
- Analisis waktu posting untuk menentukan timezone
- Analisis bahasa dan gaya penulisan (stylometry)
- Traceback cryptocurrency ke exchange (jika ada KYC)

*Step 4:* Validate — Verifikasi

**Jawaban:** Metodologi OSINT: (1) Kumpulkan semua informasi dari listing marketplace termasuk username, deskripsi, sampel data, dan alamat cryptocurrency, (2) Lakukan cross-platform search untuk menemukan username yang sama di forum, social media, dan paste sites, (3) Analisis pola waktu aktivitas untuk mengidentifikasi timezone pelaku, (4) Gunakan stylometry untuk menganalisis keunikan gaya bahasa, (5) Lacak transaksi cryptocurrency ke exchange yang memerlukan verifikasi identitas, dan (6) Korelasikan semua temuan untuk mempersempit identitas pelaku.

---

## 4. Sistem dan Protokol Email

### 4.1 Arsitektur Sistem Email

> **Email (Electronic Mail)** adalah sistem pengiriman pesan elektronik melalui jaringan komputer. Sistem email modern menggunakan arsitektur client-server dengan beberapa protokol untuk pengiriman dan penerimaan pesan.

#### 4.1.1 Komponen Sistem Email

1. **MUA (Mail User Agent)**: Aplikasi klien email (Outlook, Thunderbird, Gmail web)
2. **MSA (Mail Submission Agent)**: Menerima email dari MUA untuk dikirim
3. **MTA (Mail Transfer Agent)**: Merutekan dan meneruskan email antar server
4. **MDA (Mail Delivery Agent)**: Mengirimkan email ke mailbox penerima
5. **Mailbox**: Penyimpanan email penerima

#### 4.1.2 Protokol Email

| Protokol | Fungsi | Port Default | Port TLS/SSL |
|----------|--------|-------------|--------------|
| **SMTP** | Pengiriman email | 25 | 465/587 |
| **POP3** | Mengunduh email ke lokal | 110 | 995 |
| **IMAP** | Akses email di server | 143 | 993 |
| **MAPI** | Protokol Microsoft Exchange | - | 443 (HTTPS) |

#### 4.1.3 Alur Pengiriman Email

```
Pengirim (MUA) → MSA → MTA Pengirim → [DNS MX Lookup] → MTA Penerima → MDA → Mailbox → Penerima (MUA)
```

Contoh alur detail:

```
alice@kodam.mil.id mengirim email ke bob@lantamal.mil.id

1. Alice menulis email di Outlook (MUA)
2. Outlook mengirim ke smtp.kodam.mil.id (MSA/MTA) via SMTP port 587
3. MTA kodam melakukan DNS MX lookup untuk lantamal.mil.id
4. MTA kodam mengirim ke mx.lantamal.mil.id (MTA penerima) via SMTP port 25
5. MTA lantamal menyerahkan ke MDA
6. MDA menyimpan di mailbox Bob
7. Bob mengakses email via Outlook/IMAP
```

#### Solved Problem 6 ⭐

**Soal:** Jelaskan perbedaan antara protokol POP3 dan IMAP serta implikasinya terhadap forensik email!

**Penyelesaian:**

*Step 1:* Bandingkan karakteristik kedua protokol

| Aspek | POP3 | IMAP |
|-------|------|------|
| Model | Download & delete | Sinkronisasi |
| Penyimpanan | Lokal (klien) | Server |
| Multi-device | Tidak mendukung | Mendukung |
| Offline access | Ya (setelah download) | Terbatas |
| Bandwidth | Rendah (sekali download) | Lebih tinggi |

*Step 2:* Analisis implikasi forensik

| Implikasi | POP3 | IMAP |
|-----------|------|------|
| Lokasi bukti | Di perangkat lokal | Di server dan lokal (cache) |
| Kelengkapan data | Lengkap di lokal | Perlu akses server |
| Recovery | Lebih mudah dari disk | Perlu koordinasi dengan provider |
| Metadata | Tersimpan di file lokal | Tersimpan di server |

**Jawaban:** POP3 mengunduh email ke perangkat lokal dan biasanya menghapus dari server, sehingga bukti email terlokalisasi di perangkat dan recovery dilakukan dari analisis disk. IMAP menyinkronkan email antara server dan klien, sehingga bukti tersebar di beberapa lokasi. Dari perspektif forensik, POP3 memudahkan karena semua bukti ada di perangkat lokal, sedangkan IMAP memerlukan koordinasi dengan administrator server atau provider email untuk mendapatkan bukti yang lengkap.

---

### 4.2 Struktur Email Header

> **Email header** adalah bagian dari pesan email yang berisi informasi metadata tentang pengiriman, routing, dan autentikasi pesan. Header email adalah sumber utama bukti forensik dalam investigasi email.

#### 4.2.1 Field Header Utama

| Field | Deskripsi | Contoh |
|-------|-----------|--------|
| **From** | Alamat pengirim (dapat dipalsukan) | `From: alice@example.com` |
| **To** | Alamat penerima | `To: bob@target.com` |
| **Subject** | Subjek pesan | `Subject: Laporan Bulanan` |
| **Date** | Tanggal dan waktu pengiriman | `Date: Mon, 15 Jan 2025 08:30:00 +0700` |
| **Message-ID** | Identifikasi unik pesan | `Message-ID: <abc123@mail.example.com>` |
| **Received** | Jejak routing (dibaca dari bawah ke atas) | `Received: from mx1.example.com...` |
| **Return-Path** | Alamat untuk bounce/error | `Return-Path: <alice@example.com>` |
| **X-Originating-IP** | IP pengirim asli (tidak selalu ada) | `X-Originating-IP: [192.168.1.100]` |
| **X-Mailer** | Software email yang digunakan | `X-Mailer: Microsoft Outlook 16.0` |

#### 4.2.2 Membaca Header "Received"

Header `Received` adalah field terpenting dalam forensik email. Setiap server yang memproses email menambahkan header Received di bagian atas, sehingga **urutan pembacaan dari bawah ke atas** untuk menelusuri jalur pengiriman.

```
Received: from mx2.target.com (10.0.0.2) by mailbox.target.com
  with ESMTP; Mon, 15 Jan 2025 08:30:15 +0700        ← [3] Terakhir
Received: from relay.isp.com (203.0.113.50) by mx2.target.com
  with SMTP; Mon, 15 Jan 2025 08:30:10 +0700          ← [2] Tengah
Received: from [192.168.1.100] by relay.isp.com
  with ESMTP; Mon, 15 Jan 2025 08:30:05 +0700         ← [1] PERTAMA (sumber asli)
```

#### Solved Problem 7 ⭐⭐

**Soal:** Perhatikan header email berikut. Tentukan IP asli pengirim dan jalur pengiriman email!

```
Received: from mail.kodam.mil.id (10.20.30.1) by inbox.lantamal.mil.id
  with ESMTPS; Tue, 20 Feb 2025 14:22:30 +0700
Received: from webmail.kodam.mil.id (10.20.30.5) by mail.kodam.mil.id
  with ESMTP; Tue, 20 Feb 2025 14:22:25 +0700
Received: from [172.16.50.100] (unknown [172.16.50.100])
  by webmail.kodam.mil.id with ESMTPA; Tue, 20 Feb 2025 14:22:20 +0700
From: mayor.ahmad@kodam.mil.id
To: kapten.budi@lantamal.mil.id
```

**Penyelesaian:**

*Step 1:* Baca header Received dari bawah ke atas

| Urutan | From | By | Waktu |
|--------|------|----|-------|
| 1 (terbawah) | 172.16.50.100 | webmail.kodam.mil.id | 14:22:20 |
| 2 | webmail.kodam.mil.id (10.20.30.5) | mail.kodam.mil.id | 14:22:25 |
| 3 (teratas) | mail.kodam.mil.id (10.20.30.1) | inbox.lantamal.mil.id | 14:22:30 |

*Step 2:* Identifikasi IP asli

IP asli pengirim adalah `172.16.50.100` (pada header Received paling bawah).

*Step 3:* Rekonstruksi jalur

```
[172.16.50.100] → webmail.kodam.mil.id → mail.kodam.mil.id → inbox.lantamal.mil.id
  (Pengirim)       (Webmail server)      (MTA Kodam)         (MTA Lantamal)
```

**Jawaban:** IP asli pengirim adalah `172.16.50.100` yang merupakan alamat IP internal jaringan Kodam. Jalur pengiriman: pengirim di IP 172.16.50.100 → webmail.kodam.mil.id (14:22:20) → mail.kodam.mil.id (14:22:25) → inbox.lantamal.mil.id (14:22:30), dengan total waktu transit 10 detik.

---

## 5. Autentikasi Email: SPF, DKIM, DMARC

### 5.1 SPF (Sender Policy Framework)

> **SPF (Sender Policy Framework)** adalah mekanisme autentikasi email yang memungkinkan pemilik domain menentukan server mana yang berhak mengirim email atas nama domain tersebut melalui DNS TXT record.

Contoh SPF record:
```
v=spf1 ip4:203.0.113.0/24 include:_spf.google.com -all
```

| Mekanisme | Arti |
|-----------|------|
| `v=spf1` | Versi SPF |
| `ip4:203.0.113.0/24` | IP range yang diizinkan |
| `include:_spf.google.com` | Sertakan SPF Google |
| `-all` | Tolak semua yang tidak cocok |

Qualifier SPF:
- `+` (pass): Diizinkan (default)
- `-` (fail): Ditolak
- `~` (softfail): Diterima tetapi ditandai
- `?` (neutral): Tidak ada pernyataan

### 5.2 DKIM (DomainKeys Identified Mail)

> **DKIM** adalah metode autentikasi email yang menggunakan tanda tangan digital untuk memverifikasi bahwa email benar-benar dikirim dari domain yang diklaim dan konten tidak dimodifikasi selama transit.

Proses DKIM:
1. Server pengirim menandatangani header dan body email dengan private key
2. Tanda tangan disertakan dalam header `DKIM-Signature`
3. Server penerima mengambil public key dari DNS TXT record domain pengirim
4. Server penerima memverifikasi tanda tangan

### 5.3 DMARC (Domain-based Message Authentication, Reporting & Conformance)

> **DMARC** adalah kebijakan yang menggabungkan SPF dan DKIM untuk memberikan instruksi kepada server penerima tentang apa yang harus dilakukan jika email gagal autentikasi.

Contoh DMARC record:
```
v=DMARC1; p=reject; rua=mailto:dmarc@kodam.mil.id; ruf=mailto:forensics@kodam.mil.id
```

| Parameter | Arti |
|-----------|------|
| `v=DMARC1` | Versi DMARC |
| `p=reject` | Kebijakan: tolak email yang gagal |
| `rua=` | Alamat untuk laporan agregat |
| `ruf=` | Alamat untuk laporan forensik |

Kebijakan DMARC:
- `p=none`: Monitor saja, tidak ada tindakan
- `p=quarantine`: Masukkan ke spam/karantina
- `p=reject`: Tolak email sepenuhnya

#### Solved Problem 8 ⭐⭐

**Soal:** Email yang mengaku dari `panglima@tni.mil.id` diterima oleh sistem email Kemenhan. Hasil pemeriksaan menunjukkan SPF: fail, DKIM: fail, DMARC: fail. Apa kesimpulan forensik dari hasil ini?

**Penyelesaian:**

*Step 1:* Analisis setiap hasil autentikasi

| Autentikasi | Hasil | Arti |
|-------------|-------|------|
| SPF: fail | Email dikirim dari server yang tidak terdaftar di SPF record tni.mil.id |
| DKIM: fail | Tanda tangan digital tidak valid atau tidak ada |
| DMARC: fail | Gabungan SPF dan DKIM gagal, melanggar kebijakan domain |

*Step 2:* Kesimpulan forensik

**Jawaban:** Kegagalan ketiga mekanisme autentikasi (SPF, DKIM, DMARC) merupakan indikasi kuat bahwa email tersebut dipalsukan (spoofed). Email kemungkinan besar bukan berasal dari server resmi tni.mil.id. Ini merupakan upaya phishing atau social engineering yang menargetkan personel Kemenhan dengan memalsukan identitas pejabat tinggi TNI. Langkah selanjutnya: analisis header untuk menentukan IP asli pengirim, dan koordinasi dengan tim keamanan siber untuk respons insiden.

---

## 6. Investigasi Phishing dan Social Engineering

### 6.1 Jenis Serangan Phishing

| Jenis | Deskripsi | Target |
|-------|-----------|--------|
| **Phishing** | Email massal yang meniru entitas terpercaya | Umum/massal |
| **Spear Phishing** | Email yang ditargetkan ke individu/organisasi spesifik | Individu tertentu |
| **Whaling** | Phishing yang menargetkan eksekutif tinggi | Pimpinan/C-suite |
| **BEC (Business Email Compromise)** | Memalsukan email pimpinan untuk transfer dana | Staf keuangan |
| **Clone Phishing** | Menduplikasi email legitimate dengan modifikasi | Penerima email asli |
| **Vishing** | Phishing melalui telepon/voice | Umum |
| **Smishing** | Phishing melalui SMS | Pengguna mobile |

### 6.2 Indikator Phishing Email

Elemen yang perlu diperiksa dalam investigasi phishing:

1. **Header Analysis**: SPF/DKIM/DMARC status, IP pengirim
2. **Sender Verification**: Domain typo, display name mismatch
3. **URL Inspection**: Hover link, URL shortener, typosquatting
4. **Attachment Analysis**: Jenis file, macro, executable tersembunyi
5. **Content Analysis**: Urgensi palsu, ancaman, reward yang tidak realistis
6. **Language Analysis**: Kesalahan tata bahasa, gaya bahasa tidak konsisten

### 6.3 Analisis URL Phishing

Teknik deteksi URL phishing:

| Teknik | Contoh |
|--------|--------|
| **Typosquatting** | `go0gle.com` vs `google.com` |
| **Subdomain abuse** | `google.com.malicious.site` |
| **URL shortening** | `bit.ly/xxx` menyembunyikan URL asli |
| **Homograph attack** | `gооgle.com` (huruf Cyrillic) |
| **Path manipulation** | `malicious.com/google.com/login` |

#### Solved Problem 9 ⭐⭐

**Soal:** Seorang perwira TNI menerima email yang mengaku dari Kemenhan dengan tautan `https://kemenhan-ri.go.id.secure-login.xyz/portal`. Identifikasi teknik phishing yang digunakan!

**Penyelesaian:**

*Step 1:* Analisis struktur URL

```
https://kemenhan-ri.go.id.secure-login.xyz/portal
        ^^^^^^^^^^^^^^^^^ ^^^^^^^^^^^^^^^^
        subdomain          domain sebenarnya
```

*Step 2:* Identifikasi teknik

| Komponen | Analisis |
|----------|----------|
| Domain asli | `secure-login.xyz` (bukan kemenhan.go.id) |
| Subdomain | `kemenhan-ri.go.id` digunakan sebagai subdomain untuk menipu |
| TLD | `.xyz` bukan domain pemerintah Indonesia |
| Path | `/portal` memberi kesan akses portal resmi |

**Jawaban:** Teknik phishing yang digunakan adalah **subdomain abuse** — pelaku menggunakan `kemenhan-ri.go.id` sebagai subdomain dari domain `secure-login.xyz` untuk menipu korban agar mengira URL tersebut milik Kemenhan. Domain sebenarnya adalah `secure-login.xyz` yang merupakan domain asing. Perwira harus memeriksa domain utama (sebelum TLD) untuk verifikasi keaslian URL.

---

### 6.4 Investigasi Phishing: Langkah-langkah

1. **Preservasi**: Simpan email asli dalam format .eml atau .msg, jangan forward
2. **Header Analysis**: Periksa Received, SPF, DKIM, DMARC
3. **URL Analysis**: Analisis semua URL tanpa mengklik, gunakan sandboxing
4. **Attachment Analysis**: Analisis lampiran di sandbox (Any.run, VirusTotal)
5. **IOC Extraction**: Kumpulkan IP, domain, hash, email address
6. **Threat Intelligence**: Cek IOC di platform threat intelligence
7. **Scope Assessment**: Tentukan berapa banyak penerima yang terkena
8. **Documentation**: Buat laporan forensik lengkap

#### Solved Problem 10 ⭐⭐⭐

**Soal:** Dalam investigasi serangan phishing terhadap Mabes TNI, ditemukan email dengan lampiran `Surat_Perintah_2025.pdf.exe`. Jelaskan teknik penyamaran yang digunakan dan langkah analisis forensik yang diperlukan!

**Penyelesaian:**

*Step 1:* Analisis teknik penyamaran

| Aspek | Analisis |
|-------|----------|
| Nama file | Menggunakan double extension (`.pdf.exe`) |
| Ekstensi asli | `.exe` — file executable Windows |
| Ekstensi palsu | `.pdf` — membuat korban berpikir ini PDF |
| Nama sosial | "Surat_Perintah" memanfaatkan konteks militer |
| Tahun | "2025" memberikan kesan dokumen terkini |

*Step 2:* Langkah analisis forensik

1. **Jangan eksekusi file** — analisis di lingkungan sandbox
2. **Hash calculation**: Hitung MD5/SHA256 file
3. **VirusTotal check**: Submit hash ke VirusTotal
4. **Static analysis**: Periksa properties file, strings, imports
5. **Dynamic analysis**: Jalankan di sandbox (Any.run) untuk observasi perilaku
6. **IOC extraction**: Kumpulkan C2 domain, IP, mutex, registry changes

**Jawaban:** Teknik penyamaran: (1) Double extension `.pdf.exe` yang memanfaatkan setting Windows default yang menyembunyikan ekstensi, membuat file tampak sebagai PDF, dan (2) Social engineering dengan nama "Surat_Perintah_2025" yang relevan dengan konteks militer. Langkah forensik: hitung hash file, cek di VirusTotal, lakukan static analysis untuk memeriksa strings dan imports, jalankan di sandbox untuk dynamic analysis, dan ekstrak semua IOC untuk threat intelligence sharing.

---

## 7. Lokasi Artefak Email pada Berbagai Klien

### 7.1 Artefak Email pada Windows

| Email Client | Format File | Lokasi Default |
|-------------|-------------|----------------|
| **Outlook (POP)** | .pst | `%USERPROFILE%\Documents\Outlook Files\` |
| **Outlook (IMAP/Exchange)** | .ost | `%LOCALAPPDATA%\Microsoft\Outlook\` |
| **Thunderbird** | Mbox + .msf | `%APPDATA%\Thunderbird\Profiles\<profile>\Mail\` |
| **Windows Mail** | .eml | `%LOCALAPPDATA%\Comms\Unistore\data\` |
| **Gmail (Chrome)** | SQLite cache | `%LOCALAPPDATA%\Google\Chrome\User Data\Default\` |

### 7.2 Format File Email

| Format | Deskripsi | Dapat Dibuka Dengan |
|--------|-----------|---------------------|
| **.eml** | Standar RFC 822, single email | Outlook, Thunderbird, teks editor |
| **.msg** | Format proprietary Microsoft | Outlook, kernel MSG viewer |
| **.pst** | Personal Storage Table (Outlook) | Outlook, pst viewer, Autopsy |
| **.ost** | Offline Storage Table (Outlook) | Outlook, OST to PST converter |
| **.mbox** | Format Unix mailbox | Thunderbird, mbox viewer |
| **.dbx** | Format Outlook Express (lama) | DBX viewer |

### 7.3 Tools Forensik Email

| Tool | Fungsi | Lisensi |
|------|--------|---------|
| **Autopsy** | Analisis PST/MBOX dari forensic image | Open Source |
| **Kernel Email Forensics** | Recovery dan analisis berbagai format | Commercial |
| **MXToolbox** | Analisis header email online | Free |
| **MailHeader.org** | Parsing header email | Free |
| **PhishTool** | Analisis phishing email | Free tier |
| **Email Header Analyzer (Google)** | Visualisasi routing email | Free |

#### Solved Problem 11 ⭐⭐

**Soal:** Investigator perlu mengekstrak email dari laptop seorang tersangka yang menggunakan Microsoft Outlook dengan akun Exchange. Di mana file artefak email tersimpan dan tools apa yang digunakan?

**Penyelesaian:**

*Step 1:* Identifikasi tipe file berdasarkan konfigurasi

Outlook dengan Exchange menggunakan file `.ost` (Offline Storage Table) yang tersimpan di `%LOCALAPPDATA%\Microsoft\Outlook\`.

*Step 2:* Path lengkap

```
C:\Users\<username>\AppData\Local\Microsoft\Outlook\<email>@exchange.ost
```

*Step 3:* Tools yang digunakan

| Tool | Fungsi |
|------|--------|
| FTK Imager | Akuisisi forensic image dan ekstraksi file .ost |
| Kernel OST Viewer | Membuka dan membaca file .ost tanpa Outlook |
| Autopsy | Analisis forensic image, modul email parser |
| SysTools OST Viewer | Alternative viewer untuk .ost |

**Jawaban:** File artefak email tersimpan sebagai `.ost` di `C:\Users\<username>\AppData\Local\Microsoft\Outlook\`. Untuk ekstraksi, gunakan FTK Imager untuk akuisisi forensic image, lalu gunakan Kernel OST Viewer atau Autopsy untuk membaca isi file .ost tanpa memerlukan instalasi Outlook.

---

## 8. Forensik Komunikasi Digital

### 8.1 Instant Messaging Forensics

Selain email, komunikasi digital mencakup platform instant messaging yang perlu diinvestigasi:

| Platform | Lokasi Artefak (Windows) | Format Database |
|----------|--------------------------|-----------------|
| **WhatsApp Desktop** | `%APPDATA%\WhatsApp\` | SQLite |
| **Telegram Desktop** | `%APPDATA%\Telegram Desktop\tdata\` | Custom binary |
| **Signal Desktop** | `%APPDATA%\Signal\` | SQLite (encrypted) |
| **Discord** | `%APPDATA%\discord\` | LevelDB |
| **Slack** | `%APPDATA%\Slack\` | SQLite/IndexedDB |
| **Microsoft Teams** | `%APPDATA%\Microsoft\Teams\` | SQLite/LevelDB |

### 8.2 Artefak Chat yang Dapat Diekstrak

1. **Pesan teks**: Konten percakapan dengan timestamp
2. **Media files**: Foto, video, audio, dokumen yang dikirim/diterima
3. **Contacts**: Daftar kontak dan informasi profil
4. **Group information**: Nama grup, anggota, dan role
5. **Call logs**: Riwayat panggilan voice/video
6. **Status/Stories**: Konten yang dibagikan sebagai status
7. **Deleted messages**: Pesan yang dihapus (bergantung platform)
8. **Metadata**: Waktu baca, waktu kirim, status delivery

#### Solved Problem 12 ⭐⭐

**Soal:** Investigasi menunjukkan tersangka menggunakan WhatsApp Desktop untuk berkomunikasi dengan pihak asing. Jelaskan artefak forensik WhatsApp Desktop pada Windows dan cara mengekstraknya!

**Penyelesaian:**

*Step 1:* Identifikasi lokasi artefak

```
%APPDATA%\WhatsApp\
├── databases\
│   └── msgstore.db        ← Database pesan (SQLite)
├── Media\
│   ├── WhatsApp Images\   ← Foto yang dikirim/diterima
│   ├── WhatsApp Video\    ← Video
│   └── WhatsApp Documents\← Dokumen
├── IndexedDB\             ← Data tambahan
└── Cache\                 ← File cache termasuk thumbnail
```

*Step 2:* Proses ekstraksi

| Langkah | Tool | Aksi |
|---------|------|------|
| 1 | FTK Imager | Akuisisi image disk |
| 2 | Autopsy | Navigasi ke lokasi artefak |
| 3 | DB Browser for SQLite | Buka msgstore.db |
| 4 | File Explorer/Autopsy | Ekstrak media files |

**Jawaban:** Artefak WhatsApp Desktop di Windows tersimpan di `%APPDATA%\WhatsApp\`. Database pesan utama (`msgstore.db`) berformat SQLite dapat dibuka dengan DB Browser for SQLite. Media files (foto, video, dokumen) tersimpan di subfolder Media. Ekstraksi dilakukan dengan akuisisi disk image menggunakan FTK Imager, lalu analisis menggunakan Autopsy atau navigasi manual ke lokasi artefak.

---

## 9. Aspek Legal dan Etika Investigasi Dark Web

### 9.1 Kerangka Hukum Indonesia

| Regulasi | Relevansi |
|----------|-----------|
| **UU No. 1/2024 (ITE)** | Pencurian data, akses ilegal, distribusi konten ilegal |
| **UU No. 3/2002 (Pertahanan Negara)** | Ancaman terhadap pertahanan nasional melalui siber |
| **UU No. 17/2011 (Intelijen Negara)** | Kewenangan penyadapan dan pengumpulan intelijen siber |
| **PP No. 71/2019 (Penyelenggaraan Sistem Elektronik)** | Kewajiban penyelenggara sistem elektronik |
| **Perpres No. 82/2022 (BSSN)** | Koordinasi keamanan siber nasional |

### 9.2 Pertimbangan Etika

1. **Proporsionalitas**: Tindakan investigasi harus proporsional dengan ancaman
2. **Legalitas**: Semua aksi harus memiliki dasar hukum yang jelas
3. **Minimalisasi**: Hanya mengumpulkan data yang relevan dengan investigasi
4. **Dokumentasi**: Semua langkah harus terdokumentasi untuk audit trail
5. **Proteksi Privasi**: Melindungi data pihak tidak terkait

#### Solved Problem 13 ⭐⭐⭐

**Soal:** Tim siber TNI berencana melakukan investigasi di dark web marketplace yang menjual data intelijen militer. Jelaskan pertimbangan legal dan etika yang harus dipenuhi!

**Penyelesaian:**

*Step 1:* Identifikasi pertimbangan legal

| Aspek Legal | Pertimbangan |
|-------------|-------------|
| Kewenangan | Harus ada surat perintah atau dasar hukum yang jelas (UU Intelijen, UU ITE) |
| Jurisdiksi | Dark web bersifat lintas yurisdiksi, perlu koordinasi internasional |
| Entrapment | Investigator tidak boleh memancing tersangka melakukan kejahatan |
| Chain of custody | Semua bukti harus terdokumentasi dengan chain of custody yang valid |
| Akses server asing | Memerlukan mutual legal assistance treaty (MLAT) |

*Step 2:* Identifikasi pertimbangan etika

| Aspek Etika | Pertimbangan |
|-------------|-------------|
| Proporsionalitas | Investigasi harus sepadan dengan ancaman |
| Data minimization | Hanya kumpulkan data yang relevan |
| Perlindungan informan | Identitas sumber harus dilindungi |
| Konten ilegal | Jangan mengunduh/menyimpan konten ilegal yang ditemukan |
| Transparansi internal | Laporan ke komandan yang berwenang |

**Jawaban:** Pertimbangan legal: (1) pastikan ada dasar hukum yang jelas seperti surat perintah intelijen berdasarkan UU No. 17/2011, (2) perhatikan jurisdiksi lintas negara dan kebutuhan MLAT, (3) hindari entrapment, dan (4) jaga chain of custody. Pertimbangan etika: (1) tindakan harus proporsional, (2) minimalisasi data, (3) lindungi identitas informan, (4) jangan simpan konten ilegal, dan (5) laporkan secara transparan ke pimpinan.

---

## 10. Enron Email Dataset sebagai Studi Kasus

### 10.1 Tentang Enron Dataset

> **Enron Email Dataset** adalah kumpulan sekitar 500.000 email dari 150 karyawan Enron Corporation yang dirilis oleh Federal Energy Regulatory Commission (FERC) selama investigasi kebangkrutan Enron pada tahun 2001-2002. Dataset ini telah menjadi standar de facto untuk penelitian forensik email.

Karakteristik dataset:
- **Volume**: ~500.000 email, ~1.5 GB
- **Periode**: 1998-2002
- **Pengguna**: 150 karyawan senior Enron
- **Format**: Mbox/EML files
- **Ketersediaan**: Publik (Carnegie Mellon University)

### 10.2 Nilai Forensik Enron Dataset

| Aspek | Nilai Forensik |
|-------|---------------|
| **Realistic data** | Email asli dari korporasi nyata |
| **Diverse content** | Komunikasi bisnis, personal, internal memo |
| **Timeline analysis** | Kronologi menuju kebangkrutan dapat direkonstruksi |
| **Network analysis** | Pola komunikasi dan hierarki organisasi |
| **Keyword search** | Latihan pencarian berdasarkan kata kunci investigasi |

#### Solved Problem 14 ⭐⭐

**Soal:** Jelaskan bagaimana Enron Email Dataset dapat digunakan untuk melatih teknik forensik email dalam konteks investigasi militer!

**Penyelesaian:**

*Step 1:* Identifikasi keterampilan yang dapat dilatih

| Keterampilan | Aplikasi pada Enron Dataset |
|-------------|----------------------------|
| Email parsing | Parsing ribuan email dari format mbox |
| Header analysis | Analisis header email dari berbagai MTA |
| Keyword search | Pencarian kata kunci terkait fraud |
| Timeline reconstruction | Rekonstruksi kronologi skandal |
| Network analysis | Pemetaan jaringan komunikasi |
| Pattern recognition | Identifikasi pola komunikasi mencurigakan |

*Step 2:* Relevansi dengan konteks militer

**Jawaban:** Enron Dataset dapat digunakan untuk melatih: (1) kemampuan parsing dan analisis ribuan email secara efisien, relevan dengan investigasi kebocoran data militer, (2) identifikasi pola komunikasi mencurigakan menggunakan network analysis, mirip dengan deteksi insider threat, (3) rekonstruksi timeline dari metadata email, keterampilan esensial dalam investigasi insiden, dan (4) pencarian bukti berdasarkan keyword, teknik dasar dalam setiap investigasi forensik digital.

---

## 11. Privacy-Enhancing Technologies

### 11.1 Email Terenkripsi

| Teknologi | Deskripsi | Contoh |
|-----------|-----------|--------|
| **PGP/GPG** | Enkripsi end-to-end menggunakan public key cryptography | GnuPG, Kleopatra |
| **S/MIME** | Enkripsi berbasis sertifikat digital | Outlook, Thunderbird |
| **ProtonMail** | Layanan email terenkripsi | protonmail.com |
| **Tutanota** | Layanan email terenkripsi Jerman | tutanota.com |

### 11.2 Tantangan Forensik pada Komunikasi Terenkripsi

1. **Content Encryption**: Konten email tidak dapat dibaca tanpa kunci dekripsi
2. **Metadata Retention**: Meskipun konten terenkripsi, metadata (siapa, kapan, ke mana) sering masih tersedia
3. **Key Management**: Kunci enkripsi mungkin tersimpan di perangkat atau keyserver
4. **Legal Authority**: Perlu dasar hukum untuk meminta kunci dekripsi
5. **Volatile Keys**: Ephemeral keys dalam Signal Protocol hilang setelah digunakan

### 11.3 VPN dan Proxy dalam Konteks Forensik

| Teknologi | Fungsi Anonimisasi | Artefak Forensik |
|-----------|-------------------|------------------|
| **VPN** | Menyembunyikan IP, mengenkripsi trafik | Config files, connection logs, registry entries |
| **Proxy** | Meneruskan trafik melalui server perantara | Browser proxy settings, PAC files |
| **Tor** | Multi-layer encryption, onion routing | Tor Browser bundle, relay list |
| **I2P** | Garlic routing, hidden services | I2P router files, tunnel configuration |

#### Solved Problem 15 ⭐⭐

**Soal:** Investigator menemukan bahwa tersangka menggunakan ProtonMail untuk berkomunikasi. Jelaskan tantangan forensik dan pendekatan yang dapat dilakukan!

**Penyelesaian:**

*Step 1:* Identifikasi tantangan

| Tantangan | Penjelasan |
|-----------|------------|
| End-to-end encryption | ProtonMail mengenkripsi email sehingga server tidak bisa membaca konten |
| Yurisdiksi Swiss | ProtonMail berbasis di Swiss dengan hukum privasi ketat |
| Minimal metadata | ProtonMail menyimpan metadata minimal |
| No IP logging (by default) | Secara default tidak menyimpan log IP |

*Step 2:* Pendekatan forensik yang mungkin

| Pendekatan | Keterangan |
|-----------|------------|
| Akuisisi perangkat | Jika terautentikasi, email tersimpan di cache browser |
| Browser forensics | History, cookies, cache, localStorage terkait ProtonMail |
| RAM analysis | Memory dump saat browser terbuka mungkin berisi email plaintext |
| Legal request | Swiss merespons permintaan legal tertentu |
| Endpoint analysis | Fokus pada perangkat pengirim/penerima bukan server |

**Jawaban:** Tantangan utama: enkripsi end-to-end membuat konten tidak bisa diakses di level server, dan yurisdiksi Swiss mempersulit permintaan legal. Pendekatan: (1) fokus pada endpoint forensics dengan mengakuisisi perangkat tersangka dan mengekstrak cache browser, (2) memory forensics untuk menangkap email plaintext dari RAM saat sesi aktif, (3) browser artifacts untuk membuktikan penggunaan ProtonMail, dan (4) ajukan legal request melalui jalur MLAT jika diperlukan.

---

## 12. Cryptocurrency Tingkat Lanjut

### 12.1 Privacy Coins

| Cryptocurrency | Teknik Privasi | Tingkat Anonimitas |
|---------------|----------------|-------------------|
| **Bitcoin** | Pseudonymous | Rendah-Sedang |
| **Monero (XMR)** | Ring signatures, stealth addresses, RingCT | Tinggi |
| **Zcash (ZEC)** | zk-SNARKs (zero-knowledge proofs) | Tinggi (shielded tx) |
| **Dash** | CoinJoin (PrivateSend) | Sedang |

### 12.2 Bitcoin Mixing/Tumbling

Bitcoin mixing/tumbling adalah layanan yang mengobfuskasi jejak transaksi:

1. Pengguna mengirim Bitcoin ke alamat mixing service
2. Service mencampurkan Bitcoin dari berbagai pengguna
3. Bitcoin dikembalikan ke alamat baru pengguna (dikurangi fee)
4. Koneksi antara alamat asal dan tujuan menjadi sulit dilacak

#### Solved Problem 16 ⭐⭐⭐

**Soal:** Dalam investigasi ransomware yang menargetkan sistem informasi Kodam, pelaku meminta tebusan dalam Monero (XMR). Jelaskan mengapa Monero lebih sulit dilacak dibanding Bitcoin dan teknik forensik yang masih mungkin dilakukan!

**Penyelesaian:**

*Step 1:* Perbandingan traceability

| Aspek | Bitcoin | Monero |
|-------|---------|--------|
| Ledger | Transparan | Opaque |
| Alamat pengirim | Terlihat | Tersembunyi (stealth address) |
| Alamat penerima | Terlihat | Tersembunyi (stealth address) |
| Jumlah transaksi | Terlihat | Tersembunyi (RingCT) |
| Riwayat transaksi | Dapat dilacak | Diobfuskasi (ring signatures) |

*Step 2:* Teknik forensik yang masih mungkin

| Teknik | Keterangan |
|--------|------------|
| Exchange analysis | Monero tetap perlu dikonversi ke fiat melalui exchange |
| Timing analysis | Korelasi waktu transaksi dengan aktivitas tersangka |
| Output analysis | Analisis statistik terhadap ring signature |
| Metadata leakage | IP address saat broadcast transaksi (jika tidak pakai Tor) |

**Jawaban:** Monero lebih sulit dilacak karena menggunakan: (1) stealth addresses yang membuat setiap transaksi menggunakan alamat one-time sehingga tidak bisa dihubungkan, (2) ring signatures yang mencampur transaksi dengan decoy, dan (3) RingCT yang menyembunyikan jumlah transaksi. Teknik forensik yang masih mungkin: analisis exchange tempat Monero dikonversi, timing analysis untuk korelasi transaksi, dan network-level analysis untuk menangkap IP saat transaksi di-broadcast.

---

## 13. Studi Kasus Forensik: Serangan Phishing terhadap Pangkalan Militer

### Skenario

Pangkalan Udara TNI AU menerima laporan bahwa beberapa perwira menerima email yang mengaku dari Kemenhan meminta mereka mengunduh "update kebijakan keamanan" dari tautan yang disertakan. Tiga perwira mengklik tautan tersebut dan memasukkan kredensial mereka.

#### Solved Problem 17 ⭐⭐⭐

**Soal:** Sebagai investigator forensik, lakukan analisis terhadap skenario di atas. Tentukan langkah investigasi, artefak yang perlu dikumpulkan, dan rekomendasi mitigasi!

**Penyelesaian:**

*Step 1:* Langkah investigasi awal

| Langkah | Aksi |
|---------|------|
| 1. Preservation | Simpan email asli (.eml), jangan forward/reply |
| 2. Scope assessment | Identifikasi semua penerima email phishing |
| 3. Credential reset | Segera reset password ketiga perwira |
| 4. Network monitoring | Monitor anomali dari akun terkompromi |

*Step 2:* Artefak yang dikumpulkan

| Artefak | Sumber | Informasi |
|---------|--------|-----------|
| Email header | Email server | IP pengirim, routing, autentikasi |
| Phishing URL | Email content | Domain, hosting, SSL certificate |
| Web server logs | Proxy/firewall | Akses ke phishing domain |
| Browser history | Workstation perwira | Bukti akses URL phishing |
| Credentials submitted | Phishing page (jika diakses) | Data yang dicuri |

*Step 3:* Rekomendasi mitigasi

**Jawaban:** Langkah investigasi: (1) preservasi email asli dan isolasi workstation, (2) analisis header untuk identifikasi IP dan server pengirim, (3) analisis URL phishing termasuk WHOIS, SSL cert, dan hosting, (4) periksa web proxy logs untuk mengetahui siapa saja yang mengakses, (5) reset kredensial dan monitor akun terkompromi. Rekomendasi mitigasi: implementasi 2FA, pelatihan security awareness, email filtering yang lebih ketat, dan simulasi phishing berkala.

---

## 14. Forensik Email Lanjutan

### 14.1 Email Spoofing Detection

Teknik mendeteksi email yang di-spoof:

1. **Periksa header Received**: Apakah jalur routing konsisten?
2. **Verifikasi SPF**: Apakah IP pengirim sesuai SPF record?
3. **Validasi DKIM**: Apakah tanda tangan digital valid?
4. **Cek DMARC**: Apa kebijakan DMARC domain pengirim?
5. **Analisis X-Originating-IP**: Apakah IP masuk akal?
6. **Periksa Message-ID**: Apakah domain Message-ID sesuai pengirim?
7. **Timezone consistency**: Apakah timezone header konsisten dengan lokasi pengirim?

#### Solved Problem 18 ⭐⭐

**Soal:** Perhatikan header email berikut dan tentukan apakah email ini legitimate atau spoofed!

```
From: admin@kodam-jaya.mil.id
Return-Path: <hacker@malicious-domain.ru>
Received: from mail.malicious-domain.ru (198.51.100.50)
  by mx.kodam-jaya.mil.id; Wed, 12 Mar 2025 10:15:00 +0700
Message-ID: <random123@malicious-domain.ru>
X-Originating-IP: [198.51.100.50]
Authentication-Results: spf=fail; dkim=none; dmarc=fail
```

**Penyelesaian:**

*Step 1:* Analisis indikator

| Indikator | Nilai | Status |
|-----------|-------|--------|
| From | admin@kodam-jaya.mil.id | Mengklaim dari Kodam |
| Return-Path | hacker@malicious-domain.ru | ⚠️ Tidak sesuai From |
| Received from | mail.malicious-domain.ru | ⚠️ Server Rusia |
| Message-ID domain | malicious-domain.ru | ⚠️ Tidak sesuai From |
| X-Originating-IP | 198.51.100.50 | ⚠️ IP bukan milik Kodam |
| SPF | fail | ⚠️ Gagal autentikasi |
| DKIM | none | ⚠️ Tidak ada tanda tangan |
| DMARC | fail | ⚠️ Gagal policy check |

**Jawaban:** Email ini **spoofed**. Bukti: (1) Return-Path menunjuk ke domain Rusia yang berbeda dari From, (2) Received header menunjukkan email berasal dari server Rusia bukan server Kodam, (3) Message-ID menggunakan domain malicious-domain.ru, (4) semua autentikasi (SPF, DKIM, DMARC) gagal, dan (5) X-Originating-IP bukan milik jaringan Kodam.

---

### 14.2 Email Recovery dan Deleted Email

Teknik recovery email yang dihapus:

| Sumber | Metode Recovery |
|--------|----------------|
| **PST/OST file** | Scan database untuk deleted items yang belum di-compact |
| **Deleted Items folder** | Email mungkin masih di folder sampah |
| **Server backup** | Minta backup dari administrator email |
| **Unallocated space** | File carving pada disk untuk fragmen email |
| **Memory dump** | Email yang baru dibaca mungkin masih di RAM |
| **Journaling** | Exchange journaling menyimpan salinan semua email |

#### Solved Problem 19 ⭐⭐

**Soal:** Tersangka insider threat menghapus semua email dari Outlook sebelum laptopnya disita. Jelaskan tiga metode recovery yang dapat dilakukan!

**Penyelesaian:**

*Step 1:* Identifikasi metode recovery

| Metode | Teknik | Probabilitas Keberhasilan |
|--------|--------|--------------------------|
| 1. PST recovery | Scan file PST/OST untuk deleted records yang belum overwritten | Tinggi (jika file belum di-compact) |
| 2. File carving | Carving pada unallocated space disk untuk fragmen email | Sedang (bergantung fragmentasi) |
| 3. Server-side | Minta backup atau journaling records dari Exchange admin | Tinggi (jika journaling aktif) |

**Jawaban:** Tiga metode: (1) Scan file PST/OST menggunakan tools seperti Kernel PST Recovery karena email yang "dihapus" sering hanya ditandai sebagai deleted dalam database, (2) File carving pada unallocated space menggunakan Foremost atau Scalpel untuk menemukan fragmen email, dan (3) Recovery dari server Exchange yang mungkin masih menyimpan salinan melalui journaling, retention policy, atau backup.

---

## 15. Chat Platform Forensics Tingkat Lanjut

### 15.1 Analisis Database SQLite Chat

Banyak platform messaging menyimpan data dalam format SQLite. Langkah analisis:

1. **Lokasi database**: Temukan file .db/.sqlite di lokasi artefak platform
2. **Buka dengan DB Browser**: Gunakan DB Browser for SQLite
3. **Eksplorasi tabel**: Identifikasi tabel pesan, kontak, media
4. **Query SQL**: Tulis query untuk ekstraksi data spesifik
5. **Timeline**: Buat timeline dari timestamp pesan

Contoh SQL query untuk WhatsApp:
```sql
SELECT messages.key_remote_jid AS 'Kontak',
       messages.data AS 'Pesan',
       datetime(messages.timestamp/1000, 'unixepoch', 'localtime') AS 'Waktu',
       messages.media_mime_type AS 'Tipe Media'
FROM messages
WHERE messages.data IS NOT NULL
ORDER BY messages.timestamp DESC
LIMIT 100;
```

#### Solved Problem 20 ⭐⭐

**Soal:** Investigator perlu mengekstrak semua pesan WhatsApp dari database `msgstore.db` yang berisi kata "rahasia" atau "classified". Tulis query SQL yang sesuai!

**Penyelesaian:**

```sql
SELECT 
    key_remote_jid AS 'Nomor_Kontak',
    data AS 'Isi_Pesan',
    datetime(timestamp/1000, 'unixepoch', 'localtime') AS 'Waktu_Kirim',
    CASE key_from_me 
        WHEN 0 THEN 'Diterima'
        WHEN 1 THEN 'Dikirim'
    END AS 'Arah'
FROM messages
WHERE data LIKE '%rahasia%' 
   OR data LIKE '%classified%'
   OR data LIKE '%RAHASIA%'
ORDER BY timestamp ASC;
```

**Jawaban:** Query di atas mencari semua pesan yang mengandung kata "rahasia" atau "classified" (case-insensitive), menampilkan nomor kontak, isi pesan, waktu kirim, dan arah pesan (dikirim/diterima), diurutkan berdasarkan waktu.

---

## Supplementary Problems


#### Solved Problem 21 ⭐

**Soal:** Apa perbedaan antara deep web dan dark web?

**Penyelesaian:**

| Aspek | Deep Web | Dark Web |
|-------|----------|----------|
| Definisi | Konten tidak terindeks mesin pencari | Subset deep web yang sengaja disembunyikan |
| Akses | Login/autentikasi biasa | Software khusus (Tor, I2P) |
| Proporsi | ~90-95% internet | <1% internet |
| Konten | Legal (email, banking, database) | Campuran legal dan ilegal |
| Anonimitas | Tidak dirancang untuk anonimitas | Dirancang untuk anonimitas tinggi |

**Jawaban:** Deep web adalah seluruh konten internet yang tidak terindeks mesin pencari, sebagian besar berisi konten legal seperti email dan perbankan. Dark web adalah subset kecil dari deep web yang sengaja disembunyikan dan hanya bisa diakses dengan software khusus seperti Tor, dirancang untuk anonimitas tinggi.

#### Solved Problem 22 ⭐⭐⭐

**Soal:** Satuan Siber TNI mendeteksi email phishing canggih yang menargetkan 50 perwira di berbagai Angkatan. Email lolos filter SPF karena menggunakan domain mirip (`tni-go.id` bukan `tni.go.id`). Jelaskan teknik serangan ini dan langkah respons komprehensif!

**Penyelesaian:**

*Step 1:* Identifikasi teknik serangan

| Teknik | Detail |
|--------|--------|
| **Typosquatting/Lookalike domain** | `tni-go.id` sangat mirip `tni.go.id` |
| **SPF bypass** | Pelaku membuat SPF record valid untuk domain palsu |
| **Targeted spear phishing** | Menargetkan 50 perwira spesifik |
| **Multi-Angkatan** | Koordinasi serangan lintas organisasi |

*Step 2:* Langkah respons komprehensif

| Fase | Aksi |
|------|------|
| **Containment** | Block domain tni-go.id di email gateway dan firewall |
| **Assessment** | Identifikasi 50 penerima, tentukan siapa yang mengklik |
| **Eradication** | Reset kredensial, scan malware di workstation terdampak |
| **Recovery** | Restore dari backup jika ada kompromi |
| **Lessons Learned** | Update email filtering, awareness training |

*Step 3:* Tindakan jangka panjang

**Jawaban:** Teknik: typosquatting domain yang lolos SPF karena pelaku membuat DNS records valid untuk domain palsu. Respons: (1) Immediate — block domain palsu di semua gateway, identifikasi dan isolasi workstation yang terkompromi, (2) Short-term — reset kredensial 50 perwira, analisis forensik pada workstation yang mengklik, ekstrak IOC dari email, (3) Long-term — implementasi DMARC yang ketat, domain monitoring untuk lookalike domains, security awareness training berkala, dan simulasi phishing rutin.

---

## Ringkasan


| Topik | Poin Kunci |
|-------|-----------|
| **Lapisan Web** | Surface (4-5%), Deep (90-95%), Dark (<1%) |
| **Tor** | Onion routing, 3 relay, enkripsi berlapis |
| **Cryptocurrency** | Pseudonymous (Bitcoin), private (Monero) |
| **Email Header** | Received dibaca dari bawah ke atas |
| **Autentikasi** | SPF + DKIM + DMARC = defense-in-depth |
| **Phishing** | Analisis header, URL, attachment, IOC |
| **Artefak Email** | PST/OST (Outlook), Mbox (Thunderbird) |
| **Chat Forensics** | SQLite database, media files, metadata |

## Referensi

1. Casey, E. (2022). *Digital Evidence and Computer Crime: Forensic Science, Computers, and the Internet*. 4th Edition. Academic Press. Chapter 14.
2. Davidoff, S. & Ham, J. (2022). *Network Forensics: Tracking Hackers through Cyberspace*. 2nd Edition. Pearson. Chapter 9-10.
3. Johansen, G. (2020). *Digital Forensics and Incident Response*. 2nd Edition. Packt Publishing. Chapter 10.
4. Phillips, A., et al. (2022). *Guide to Computer Forensics and Investigations*. 6th Edition. Cengage. Chapter 12.
5. Europol. (2023). *Internet Organised Crime Threat Assessment (IOCTA) 2023*.
6. NIST. (2021). *Guidelines on Email Forensics*. NIST Special Publication.
7. Tor Project. (2024). *Tor: Overview*. https://www.torproject.org/about/overview.html.en
8. Chainalysis. (2024). *The 2024 Crypto Crime Report*.

---

## License / Lisensi

Modul ini dilisensikan di bawah [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-nc-sa/4.0/).

**© 2025 Program Studi Informatika, Fakultas Sains dan Teknologi Pertahanan, Universitas Pertahanan RI**
