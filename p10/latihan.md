# Latihan Pertemuan 10: Forensik Jaringan

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 10  
**Topik:** Forensik Jaringan  
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
Forensik jaringan didefinisikan sebagai cabang forensik digital yang berfokus pada:

A. Analisis file system dan registry pada komputer  
B. Pemantauan, penangkapan, pencatatan, dan analisis lalu lintas jaringan untuk pengumpulan bukti  
C. Pemrograman sistem keamanan firewall  
D. Desain arsitektur jaringan yang aman  
E. Instalasi dan konfigurasi antivirus pada endpoint

---

### Soal 2
Dalam dua pendekatan forensik jaringan menurut Marcus Ranum, metode "catch-it-as-you-can" berarti:

A. Menganalisis setiap paket secara real-time dan menyimpan yang relevan  
B. Menangkap semua paket pada titik monitoring untuk dianalisis kemudian  
C. Hanya menangkap paket yang sudah ditandai oleh IDS  
D. Menyimpan hanya metadata tanpa payload  
E. Menjalankan capture hanya saat serangan terdeteksi

---

### Soal 3
Perbedaan utama antara forensik komputer dan forensik jaringan terletak pada:

A. Forensik komputer menggunakan tools yang lebih mahal  
B. Forensik jaringan hanya bisa dilakukan secara post-mortem  
C. Forensik komputer menangani data at rest, forensik jaringan menangani data in motion  
D. Forensik jaringan tidak memerlukan chain of custody  
E. Forensik komputer tidak bisa mendeteksi malware

---

### Soal 4
Perangkat yang dipasang secara inline pada kabel jaringan untuk menyalin semua traffic tanpa mempengaruhi aliran data disebut:

A. SPAN Port  
B. Network TAP  
C. Proxy Server  
D. Load Balancer  
E. Packet Sniffer

---

### Soal 5
Rasio perbandingan storage antara Full Packet Capture (PCAP) dan Flow Data untuk volume data yang sama adalah sekitar:

A. 5:1  
B. 50:1  
C. 100:1  
D. 500:1  
E. 1000:1

---

### Soal 6
Informasi yang terdapat dalam Flow Data (NetFlow/sFlow/IPFIX) meliputi semua berikut ini, KECUALI:

A. IP address sumber dan tujuan  
B. Port number sumber dan tujuan  
C. Isi payload komunikasi  
D. Jumlah bytes yang ditransfer  
E. Timestamp mulai dan berakhir koneksi

---

### Soal 7
Dalam TCP three-way handshake, urutan flag yang benar untuk membuat koneksi adalah:

A. ACK → SYN → SYN-ACK  
B. SYN → ACK → SYN-ACK  
C. SYN → SYN-ACK → ACK  
D. FIN → SYN-ACK → ACK  
E. RST → SYN → ACK

---

### Soal 8
Banyaknya paket SYN tanpa diikuti ACK dari satu IP sumber ke banyak port pada satu target merupakan indikasi:

A. DDoS attack  
B. DNS tunneling  
C. TCP SYN scan  
D. ARP poisoning  
E. Man-in-the-middle attack

---

### Soal 9
Dalam forensik DNS, banyaknya respons NXDOMAIN (domain tidak ditemukan) dari satu host internal merupakan indikator:

A. DNS server mengalami kegagalan  
B. Konfigurasi DNS yang salah  
C. Malware dengan Domain Generation Algorithm (DGA)  
D. DNS amplification attack  
E. Penggunaan VPN oleh user

---

### Soal 10
DNS tunneling dapat dideteksi melalui indikator berikut, KECUALI:

A. Subdomain sangat panjang (>30 karakter)  
B. Volume query tinggi ke satu domain tertentu  
C. Tipe query TXT atau NULL yang tidak lazim  
D. Query ke domain-domain terkenal seperti google.com  
E. Entropy tinggi pada subdomain

---

### Soal 11
Seorang analis menemukan HTTP POST request menggunakan User-Agent `curl/7.68.0` yang mengirim file bernama `laporan_operasi_q4.xlsx` ke domain eksternal. Temuan ini paling mengindikasikan:

A. Akses web browsing normal  
B. Software update otomatis  
C. Data exfiltration melalui HTTP  
D. Backup file ke cloud storage  
E. Pengunduhan malware

---

### Soal 12
Informasi yang masih tersedia pada traffic HTTPS (TLS) tanpa kunci dekripsi meliputi semua berikut ini, KECUALI:

A. SNI (Server Name Indication)  
B. Certificate details  
C. URL path lengkap  
D. Ukuran data yang ditransfer  
E. JA3 fingerprint hash

---

### Soal 13
JA3 fingerprinting dalam konteks forensik jaringan digunakan untuk:

A. Mendekripsi traffic HTTPS  
B. Mengidentifikasi implementasi TLS berdasarkan Client Hello parameters  
C. Memecahkan password yang terenkripsi  
D. Mendeteksi perubahan pada certificate server  
E. Mengaudit konfigurasi cipher suite pada firewall

---

### Soal 14
IDS yang mendeteksi serangan berdasarkan pencocokan pola yang sudah diketahui disebut:

A. Anomaly-based IDS  
B. Hybrid IDS  
C. Signature-based IDS  
D. Heuristic IDS  
E. Behavioral IDS

---

### Soal 15
Alert Snort/Suricata berikut menunjukkan apa?  
`[**] [1:2024217:3] ET MALWARE Trickbot CnC Beacon [**] [Priority: 1]`

A. Legitimate traffic ke server update  
B. Komunikasi Command and Control malware Trickbot  
C. Scanning port oleh attacker  
D. Brute force SSH login  
E. DNS amplification attack

---

### Soal 16
Dalam investigasi wireless network forensics, Probe Request dari perangkat mobile mengungkap:

A. Password Wi-Fi yang tersimpan  
B. Riwayat SSID jaringan yang pernah terhubung  
C. Isi data yang dikirim melalui Wi-Fi  
D. Konfigurasi IP address perangkat  
E. Versi operating system perangkat

---

### Soal 17
Indikator utama keberadaan Rogue Access Point di lingkungan jaringan militer adalah:

A. Kecepatan Wi-Fi yang lebih lambat dari biasanya  
B. SSID sama dengan jaringan resmi tetapi BSSID (MAC address AP) berbeda  
C. Peningkatan jumlah perangkat terkoneksi  
D. Penggunaan bandwidth yang meningkat  
E. Adanya jaringan 5 GHz selain 2.4 GHz

---

### Soal 18
Pada covert channel menggunakan ICMP tunneling, indikator anomali yang paling jelas adalah:

A. Frekuensi ping yang sangat rendah  
B. Payload ICMP yang jauh lebih besar dari normal (>64 bytes)  
C. Penggunaan ICMP type 3 (Destination Unreachable)  
D. Ping ke gateway default  
E. Round-trip time yang stabil

---

### Soal 19
Untuk analisis PCAP berukuran sangat besar (>50 GB), langkah pertama yang PALING tepat adalah:

A. Langsung membuka file di Wireshark  
B. Menjalankan `capinfos` untuk mendapatkan statistik dasar  
C. Menghapus paket yang dianggap tidak penting  
D. Mengkonversi ke format text  
E. Membagi file menjadi ukuran 100 MB

---

### Soal 20
Regulasi di Indonesia yang mengatur bahwa intersepsi komunikasi elektronik memerlukan perintah pengadilan adalah:

A. UU No. 3 Tahun 2002 tentang Pertahanan Negara  
B. UU No. 1 Tahun 2024 tentang Informasi dan Transaksi Elektronik  
C. PP No. 71 Tahun 2019 tentang Penyelenggaraan Sistem Elektronik  
D. Perpres No. 82 Tahun 2022 tentang BSSN  
E. UU No. 14 Tahun 2008 tentang Keterbukaan Informasi Publik

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Jelaskan perbedaan antara pendekatan "catch-it-as-you-can" dan "stop, look, and listen" dalam forensik jaringan! Kapan masing-masing pendekatan lebih tepat digunakan?

---

### Soal 2 ⭐
Sebutkan dan jelaskan tiga jenis bukti jaringan (network evidence) beserta kelebihan dan kekurangan masing-masing!

---

### Soal 3 ⭐
Apa yang dimaksud dengan Network TAP dan SPAN Port? Jelaskan perbedaan keduanya dalam konteks pengumpulan bukti jaringan!

---

### Soal 4 ⭐
Sebutkan lima sumber log jaringan yang penting untuk forensik dan informasi kunci yang dapat diperoleh dari masing-masing!

---

### Soal 5 ⭐⭐
Jelaskan bagaimana TCP flags (SYN, ACK, FIN, RST, PSH) dapat digunakan untuk mendeteksi aktivitas mencurigakan seperti port scanning dan brute force! Sertakan contoh filter Wireshark yang relevan!

---

### Soal 6 ⭐⭐
Jelaskan mekanisme DNS tunneling dan mengapa teknik ini efektif untuk data exfiltration! Sebutkan minimal lima indikator yang dapat digunakan untuk mendeteksinya!

---

### Soal 7 ⭐⭐
Bandingkan informasi yang masih tersedia dan yang terenkripsi pada traffic HTTPS! Jelaskan bagaimana analis forensik tetap dapat memperoleh informasi berharga dari traffic terenkripsi!

---

### Soal 8 ⭐⭐
Jelaskan konsep JA3 fingerprinting dan bagaimana teknik ini digunakan untuk mengidentifikasi malware yang berkomunikasi melalui HTTPS!

---

### Soal 9 ⭐⭐
Jelaskan perbedaan antara signature-based IDS dan anomaly-based IDS! Dalam skenario militer, mengapa pendekatan hybrid lebih direkomendasikan?

---

### Soal 10 ⭐⭐
Sebutkan dan jelaskan tujuh langkah metodologi rekonstruksi serangan berbasis jaringan! Berikan contoh aktivitas pada setiap langkah!

---

### Soal 11 ⭐⭐
Jelaskan tantangan khusus dalam wireless network forensics! Bagaimana Probe Request dan Rogue AP detection dapat membantu investigasi?

---

### Soal 12 ⭐⭐⭐
Seorang analis menemukan bahwa host 192.168.1.50 mengirim 200 ICMP echo request per menit ke 203.0.113.100 dengan payload rata-rata 512 bytes selama 24 jam. Hitung estimasi volume data yang dieksfiltrasikan dan jelaskan mengapa pola ini mencurigakan dibandingkan dengan ping normal!

---

### Soal 13 ⭐⭐⭐
Jelaskan strategi bertahap untuk menganalisis file PCAP berukuran 50 GB! Sebutkan tools yang digunakan pada setiap tahap dan alasannya!

---

### Soal 14 ⭐⭐⭐
Jelaskan pertimbangan hukum dan etika dalam melakukan full packet capture pada jaringan militer Indonesia! Referensikan regulasi yang relevan dan prinsip-prinsip yang harus diterapkan!

---

### Soal 15 ⭐⭐⭐
Berdasarkan log korelasi berikut, lakukan rekonstruksi serangan dan petakan ke dalam Cyber Kill Chain!

```
08:20:00 [DNS]     192.168.1.100 → exploit-kit.malware.net
08:20:01 [FW]      ALLOW → 203.0.113.50:80 (HTTP)
08:20:02 [IDS]     ET EXPLOIT Kit Landing Page detected
08:20:05 [Proxy]   GET /payload.exe → 200 OK
08:20:10 [FW]      ALLOW → 198.51.100.25:443 (HTTPS)
08:20:11 [IDS]     ET MALWARE CnC Beacon Activity
08:21:00 [FW]      ALLOW → 198.51.100.25:443 (5.2 MB)
08:21:30 [IDS]     ET POLICY Large Outbound Data Transfer
```

---

## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Deteksi C2 di Jaringan Kodam XII/Tanjungpura

**Latar Belakang:**

Tim Satsiber TNI menerima alert dari IDS di jaringan Kodam XII/Tanjungpura. Selama satu minggu terakhir, alert berkala terdeteksi dari satu workstation unit Intelijen. Alert muncul secara konsisten pada interval ~5 menit, dimulai sekitar pukul 02:00 dini hari dan berakhir sekitar pukul 04:30.

**Data yang Tersedia:**

IDS Alert Log:
```
[IDS] 2025-01-15 02:00:03 ET MALWARE CnC Activity 192.168.10.45 → 185.100.87.XXX:443
[IDS] 2025-01-15 02:05:02 ET MALWARE CnC Activity 192.168.10.45 → 185.100.87.XXX:443
[IDS] 2025-01-15 02:10:04 ET MALWARE CnC Activity 192.168.10.45 → 185.100.87.XXX:443
... (berlanjut hingga 04:30)
[IDS] 2025-01-15 02:30:15 ET POLICY Large Outbound Data Transfer 192.168.10.45 → 185.100.87.XXX:443 (45 MB)
```

PCAP Analysis (partial):
```
TLS SNI: cloud-sync.net
Certificate: Self-signed, CN=localhost, Valid: 2025-01-01 to 2026-01-01
JA3 Hash: 72a589da586844d7f0818ce684948eea (match: Cobalt Strike)
```

DNS Log:
```
2025-01-15 01:59:58 192.168.10.45 A cloud-sync.net → 185.100.87.XXX
2025-01-15 02:00:01 192.168.10.45 TXT dGhpcyBpcyBhIHRlc3Q.data.cloud-sync.net
```

Informasi Host:
- Hostname: WS-INTEL-007
- OS: Windows 10 Enterprise
- User: Kapten. B (unit Intelijen Kodam)
- Lokasi: Ruang Kerja Intelijen Lt. 2

**Pertanyaan:**

**1a.** Analisis semua bukti jaringan yang tersedia dan identifikasi minimal 5 indikator compromise (IOC) yang menunjukkan aktivitas malicious! Jelaskan signifikansi masing-masing IOC! (15 poin)

**1b.** Berdasarkan JA3 hash dan pola komunikasi, identifikasi jenis malware yang digunakan! Jelaskan bagaimana JA3 fingerprinting membantu identifikasi meskipun traffic terenkripsi! (10 poin)

**1c.** Jelaskan mengapa penyerang memilih interval 5 menit dan waktu 02:00-04:30! Apa implikasi pola beaconing ini terhadap strategi deteksi? (10 poin)

**1d.** Buat rencana investigasi forensik jaringan lengkap! Tentukan tools yang akan digunakan, data tambahan yang perlu dikumpulkan, dan langkah-langkah analisis! (15 poin)

**1e.** Analisis implikasi keamanan temuan ini terhadap operasional Kodam XII! Rekomendasikan minimal 5 langkah mitigasi dan perbaikan yang harus dilakukan! (15 poin)

---

### Studi Kasus 2: Data Exfiltration melalui DNS Tunneling di Lantamal V

**Latar Belakang:**

Administrator jaringan Lantamal V (Pangkalan Utama TNI AL) Surabaya mendeteksi anomali pada traffic DNS. Volume query DNS dari satu subnet meningkat 500% dalam 3 hari terakhir. Sebagian besar query menuju domain yang sama: `secure-update.tech`.

**Data yang Tersedia:**

DNS Query Log (sample):
```
2025-02-10 14:23:01 10.20.30.45 TXT dGFrdGlzIGRlZmVuc2UgcGxhbg.data.secure-update.tech
2025-02-10 14:23:02 10.20.30.45 TXT b3BlcmF0aW9uIHJlcG9ydCBxNA.data.secure-update.tech  
2025-02-10 14:23:03 10.20.30.45 TXT c2hpcCBsb2NhdGlvbiBkYXRh.data.secure-update.tech
2025-02-10 14:23:04 10.20.30.45 TXT Y3JldyBtYW5pZmVzdCBkZXRhaWw.data.secure-update.tech
... (ribuan query serupa)
```

Flow Data Summary:
```
Source: 10.20.30.45 → DNS Server 10.20.30.1 → Upstream DNS
Protocol: UDP/53
Avg Query Rate: 150 queries/menit
Avg Subdomain Length: 35 karakter
Total DNS Traffic (3 hari): ~2.8 GB
Normal DNS Traffic (baseline): ~50 MB/hari
```

Wireshark Statistics:
```
Protocol Hierarchy: DNS 78% of all traffic from 10.20.30.45
DNS Query Types: TXT (92%), A (6%), AAAA (2%)
Unique subdomains queried: 147,852
Response NXDOMAIN: 0% (semua resolve)
```

Host Information:
- IP: 10.20.30.45
- Hostname: WS-LOGISTICS-003
- OS: Windows 10
- User: Letda. S (Staf Logistik Lantamal)
- Akses: Sistem inventaris kapal dan logistik perbekalan

**Pertanyaan:**

**2a.** Decode sampel DNS query yang diberikan dan tentukan jenis informasi apa yang sedang dieksfiltrasikan! Jelaskan implikasi keamanan terhadap operasional TNI AL! (15 poin)

**2b.** Berdasarkan flow data, hitung estimasi total volume data yang berhasil dieksfiltrasikan melalui DNS tunneling dalam 3 hari! Tunjukkan perhitungan Anda! (10 poin)

**2c.** Jelaskan mengapa DNS tunneling efektif sebagai metode exfiltration di lingkungan militer! Sebutkan minimal 4 alasan teknis dan operasional! (10 poin)

**2d.** Buat prosedur forensik lengkap untuk investigasi kasus ini, termasuk: bukti yang harus diamankan, tools yang digunakan, analisis yang dilakukan, dan format laporan forensik! (15 poin)

**2e.** Rekomendasikan arsitektur keamanan DNS yang dapat mencegah serangan serupa di masa depan! Sertakan konfigurasi teknis dan kebijakan operasional! (15 poin)

---

## Kunci Jawaban

### Bagian A: Pilihan Ganda

| No | Jawaban | Penjelasan |
|----|---------|------------|
| 1 | **B** | Forensik jaringan mencakup pemantauan, penangkapan, pencatatan, dan analisis lalu lintas jaringan untuk pengumpulan bukti investigasi |
| 2 | **B** | "Catch-it-as-you-can" menangkap semua paket pada titik monitoring, menyimpan untuk analisis kemudian (full packet capture) |
| 3 | **C** | Perbedaan fundamental: forensik komputer menangani data at rest (statis), forensik jaringan menangani data in motion (dinamis) |
| 4 | **B** | Network TAP (Test Access Point) adalah perangkat inline yang menyalin traffic secara passive tanpa mempengaruhi aliran data |
| 5 | **D** | Rasio PCAP:Flow ≈ 500:1 karena flow hanya menyimpan metadata (IP, port, timestamp, byte count) tanpa payload |
| 6 | **C** | Flow Data hanya berisi metadata (IP, port, protocol, bytes, timestamp). Payload/isi komunikasi tidak termasuk |
| 7 | **C** | Urutan benar: Client kirim SYN → Server respons SYN-ACK → Client kirim ACK → Connection established |
| 8 | **C** | Satu IP mengirim banyak SYN ke banyak port pada satu target tanpa menyelesaikan handshake = TCP SYN scan (half-open scan) |
| 9 | **C** | Banyak NXDOMAIN = host mencoba resolve domain acak yang tidak ada, indikasi malware DGA yang generate domain C2 secara algoritma |
| 10 | **D** | Query ke domain terkenal (google.com) adalah normal. DNS tunneling terindikasi dari subdomain panjang, volume tinggi, TXT record, dan entropy tinggi |
| 11 | **C** | curl (bukan browser) + file militer + POST ke domain mencurigakan = data exfiltration yang disengaja melalui HTTP |
| 12 | **C** | URL path terenkripsi pada HTTPS. SNI, certificate, JA3, dan ukuran data masih dapat diamati |
| 13 | **B** | JA3 menghasilkan hash MD5 dari Client Hello TLS parameters, unik per implementasi TLS (browser, malware, tool berbeda) |
| 14 | **C** | Signature-based IDS mencocokkan traffic dengan database pola serangan yang sudah diketahui |
| 15 | **B** | "ET MALWARE Trickbot CnC Beacon" = deteksi komunikasi Command and Control malware Trickbot dengan Priority 1 (kritis) |
| 16 | **B** | Probe Request mengandung SSID jaringan yang pernah terhubung, mengungkap riwayat koneksi perangkat |
| 17 | **B** | Rogue AP/Evil Twin: SSID sama dengan resmi tapi BSSID berbeda, sering dengan enkripsi yang lebih lemah |
| 18 | **B** | Ping normal memiliki payload 32-64 bytes. Payload >64 bytes pada frekuensi tinggi mengindikasikan ICMP tunneling |
| 19 | **B** | capinfos memberikan statistik tanpa memuat seluruh file ke memori; langkah pertama sebelum analisis mendalam |
| 20 | **B** | UU No. 1 Tahun 2024 (revisi UU ITE) mengatur bahwa intersepsi komunikasi memerlukan perintah pengadilan/penegak hukum |

---

### Bagian B: Uraian

#### Jawaban Soal 1 ⭐

**Perbedaan dua pendekatan forensik jaringan:**

| Aspek | Catch-it-as-you-can | Stop, look, and listen |
|-------|---------------------|----------------------|
| **Metode** | Tangkap semua paket, analisis nanti | Analisis real-time, simpan yang relevan |
| **Storage** | Sangat besar (~10 TB/hari untuk 1 Gbps) | Relatif kecil |
| **Kelengkapan** | Bukti lengkap (termasuk payload) | Mungkin kehilangan detail |
| **Latensi analisis** | Tinggi (post-capture) | Rendah (near real-time) |

**Kapan digunakan:**
- **Catch-it-as-you-can**: investigasi insiden spesifik, jaringan bandwidth rendah, kebutuhan preservasi bukti lengkap (lingkungan militer classified)
- **Stop, look, and listen**: monitoring jaringan bandwidth tinggi, deteksi real-time, resources storage terbatas

**Skor: 5 poin** (2 poin definisi, 2 poin perbandingan, 1 poin konteks penggunaan)

---

#### Jawaban Soal 2 ⭐

**Tiga jenis bukti jaringan:**

1. **Full Packet Capture (PCAP)**
   - *Kelebihan*: Informasi paling lengkap (header + payload), dapat merekonstruksi sesi komunikasi, extract file
   - *Kekurangan*: Storage sangat besar (~10 TB/hari pada 1 Gbps), perlu processing power tinggi

2. **Flow Data (NetFlow/sFlow/IPFIX)**
   - *Kelebihan*: Ringan (~20 GB/hari), metadata cukup untuk triage, fitur bawaan router
   - *Kekurangan*: Tanpa payload/konten komunikasi, tidak bisa extract file

3. **Log Files (Firewall, IDS, DNS, Proxy, dll.)**
   - *Kelebihan*: Sudah tersedia di perangkat, konteks keamanan spesifik, mudah dikorelasikan
   - *Kekurangan*: Detail bervariasi per sumber, mungkin tidak lengkap, format tidak standar

**Skor: 5 poin** (1 poin per jenis + kelebihan/kekurangan, 2 poin perbandingan)

---

#### Jawaban Soal 3 ⭐

**Network TAP (Test Access Point):**
- Perangkat hardware yang dipasang inline pada kabel jaringan
- Menyalin semua traffic secara passive (tidak mempengaruhi aliran data)
- Kelebihan: fail-safe (jika TAP mati, traffic tetap lewat), full-duplex capture, tidak terdeteksi
- Kekurangan: memerlukan perangkat fisik tambahan, perlu akses fisik untuk instalasi

**SPAN/Mirror Port:**
- Fitur pada managed switch yang menyalin traffic dari satu/beberapa port ke port monitoring
- Konfigurasi melalui CLI/GUI switch
- Kelebihan: tanpa biaya tambahan, mudah dikonfigurasi, fleksibel
- Kekurangan: dapat menyebabkan packet loss pada traffic tinggi, mempengaruhi performa switch

**Perbedaan utama:** TAP adalah hardware dedicated (lebih reliable), SPAN adalah fitur software switch (lebih praktis).

**Skor: 5 poin** (2 poin TAP, 2 poin SPAN, 1 poin perbandingan)

---

#### Jawaban Soal 4 ⭐

**Lima sumber log jaringan dan informasi kunci:**

1. **Firewall Log**: Koneksi yang diizinkan/ditolak, rule yang terpicu, source/destination IP dan port
2. **IDS/IPS Log**: Alert keamanan, signature match, severity level, paket yang memicu alert
3. **DNS Server Log**: Query dan response (domain yang diakses), NXDOMAIN, tipe query
4. **Proxy Server Log**: URL yang diakses, user agent, HTTP response code, username
5. **DHCP Server Log**: IP assignment, MAC address perangkat, lease time, hostname

**Skor: 5 poin** (1 poin per sumber log dengan informasi kunci)

---

#### Jawaban Soal 5 ⭐⭐

**TCP Flags untuk deteksi aktivitas mencurigakan:**

1. **SYN Scan Detection**: Banyak SYN tanpa ACK dari satu IP ke banyak port
   - Filter: `tcp.flags.syn == 1 && tcp.flags.ack == 0`
   - Pola: Satu source IP → banyak destination port pada satu target

2. **FIN Scan Detection**: Paket FIN dikirim ke port tertutup (respons RST) = stealth scan
   - Filter: `tcp.flags.fin == 1 && tcp.flags.ack == 0`

3. **SSH Brute Force**: Banyak SYN ke port 22 dari satu IP
   - Filter: `tcp.dstport == 22 && tcp.flags.syn == 1`
   - Indikator: Banyak koneksi gagal (RST) diikuti satu koneksi berhasil

4. **RST Analysis**: Banyak RST menunjukkan port tertutup atau koneksi yang ditolak
   - Filter: `tcp.flags.reset == 1`

5. **PSH Flag**: Push data aktif; volume besar PSH ke satu tujuan = data exfiltration potensial

**Skor: 8 poin** (2 poin per jenis deteksi × 3 minimal + 2 poin filter Wireshark)

---

#### Jawaban Soal 6 ⭐⭐

**Mekanisme DNS Tunneling:**
DNS tunneling memanfaatkan protokol DNS (port 53) untuk mengirim data non-DNS. Data dienkode (biasanya Base64) dan ditempatkan sebagai subdomain dalam DNS query. Server C2 penyerang yang mengontrol domain authoritative menerima dan mendekode data tersebut.

Contoh: `dGhpcyBpcyBhIHRlc3Q.data.evil.com` → Base64 decode → "this is a test"

**Mengapa efektif:**
- DNS hampir selalu diizinkan melalui firewall
- Sulit dibedakan dari traffic DNS normal tanpa analisis mendalam
- Port 53 jarang dimonitor secara detail

**Lima indikator deteksi:**
1. **Subdomain sangat panjang** (>30 karakter, sering mengandung karakter Base64)
2. **Volume query tinggi** ke satu domain tertentu yang tidak dikenal
3. **Tipe query TXT atau NULL** yang tidak lazim untuk operasi normal
4. **Entropy tinggi** pada subdomain (data terenkode memiliki distribusi karakter merata)
5. **Pola periodik** (beaconing) menunjukkan komunikasi C2 terjadwal

**Skor: 8 poin** (3 poin mekanisme, 2 poin alasan efektif, 3 poin indikator)

---

#### Jawaban Soal 7 ⭐⭐

**Perbandingan informasi pada HTTPS:**

| Masih Tersedia | Terenkripsi |
|---------------|-------------|
| IP sumber & tujuan | URL path lengkap |
| SNI (Server Name Indication) | Konten request/response |
| Certificate details (CN, issuer, validity) | Header HTTP |
| Ukuran data yang ditransfer | Cookie & kredensial |
| Pola timing koneksi | Body payload |
| TLS version & cipher suite | Query string |
| JA3/JA3S fingerprint hash | POST data |

**Cara memperoleh informasi berharga:**
1. **SNI analysis**: Identifikasi domain yang diakses melalui Client Hello
2. **Certificate inspection**: Self-signed certificate = red flag, periksa CN, issuer, validity
3. **JA3 fingerprinting**: Identifikasi malware/tool berdasarkan TLS implementation unik
4. **Traffic pattern analysis**: Deteksi beaconing C2 dari pola timing reguler
5. **Volume analysis**: Upload besar ke domain tertentu = potential exfiltration
6. **Certificate transparency logs**: Verifikasi legitimasi certificate

**Skor: 8 poin** (3 poin perbandingan tabel, 5 poin teknik analisis)

---

#### Jawaban Soal 8 ⭐⭐

**Konsep JA3 Fingerprinting:**
JA3 adalah metode fingerprinting TLS yang dikembangkan oleh Salesforce. Teknik ini menghasilkan hash MD5 dari parameter Client Hello dalam TLS handshake, termasuk: TLS version, cipher suites, extensions, elliptic curves, dan elliptic curve point formats.

Setiap implementasi TLS (browser, malware, tool) memiliki kombinasi parameter yang unik, menghasilkan JA3 hash yang berbeda.

**Cara identifikasi malware:**
1. Hash JA3 dihitung dari Client Hello tanpa perlu dekripsi
2. Hash dibandingkan dengan database JA3 yang diketahui (misalnya: Cobalt Strike = `72a589da586844d7f0818ce684948eea`)
3. Malware yang menyamar sebagai browser terdeteksi karena JA3 hash tidak cocok dengan browser asli
4. Perubahan versi malware dapat terdeteksi dari perubahan JA3 hash
5. Tool otomatis (curl, wget, custom tool) memiliki JA3 berbeda dari browser

**Skor: 8 poin** (3 poin konsep, 3 poin cara identifikasi, 2 poin contoh)

---

#### Jawaban Soal 9 ⭐⭐

**Perbandingan:**

| Aspek | Signature-based | Anomaly-based |
|-------|----------------|---------------|
| **Metode** | Cocokkan pola serangan yang diketahui | Deteksi penyimpangan dari baseline normal |
| **Kelebihan** | Akurat, false positive rendah | Deteksi serangan baru/zero-day |
| **Kelemahan** | Tidak deteksi zero-day | False positive tinggi |
| **Database** | Perlu update signature berkala | Perlu training baseline |
| **Contoh** | Snort, Suricata (rule-based) | Machine learning-based anomaly detection |

**Alasan hybrid direkomendasikan untuk militer:**
1. **Ancaman beragam**: Militer menghadapi APT (zero-day) dan serangan konvensional
2. **Nilai target tinggi**: Infrastruktur pertahanan terlalu kritis untuk bergantung satu metode
3. **Compliance**: Regulasi pertahanan mengharuskan deteksi komprehensif
4. **Cost-benefit**: False positive dari anomaly-based ditangani oleh konfirmasi signature-based
5. **Evolving threats**: APT nation-state sering menggunakan teknik baru yang tidak ada di database signature

**Skor: 8 poin** (3 poin perbandingan, 5 poin alasan hybrid untuk militer)

---

#### Jawaban Soal 10 ⭐⭐

**Tujuh langkah rekonstruksi serangan:**

1. **Collection**: Kumpulkan semua PCAP, flow data, dan log dari berbagai sumber (firewall, IDS, DNS, proxy)
   - *Contoh*: Copy PCAP dari sensor, export firewall log, download IDS alert

2. **Filtering**: Filter noise dan fokuskan pada IP, port, dan timeframe yang relevan
   - *Contoh*: Filter PCAP berdasarkan IP tersangka dan rentang waktu insiden

3. **Correlation**: Korelasikan event dari berbagai sumber untuk membangun gambaran lengkap
   - *Contoh*: Gabungkan DNS query + firewall allow + IDS alert untuk satu koneksi

4. **Timeline**: Susun timeline kronologis dari semua event yang terkorelasi
   - *Contoh*: Buat spreadsheet dengan timestamp, sumber, event, dan signifikansi

5. **Analysis**: Identifikasi teknik, taktik, dan prosedur (TTP) yang digunakan penyerang
   - *Contoh*: Mapping ke MITRE ATT&CK framework

6. **Attribution**: Kaitkan temuan dengan aktor ancaman yang dikenal berdasarkan IOC
   - *Contoh*: JA3 hash cocok dengan Cobalt Strike, IP C2 terdaftar di threat intelligence

7. **Reporting**: Dokumentasi temuan, bukti pendukung, timeline, dan rekomendasi
   - *Contoh*: Laporan forensik formal dengan chain of custody

**Skor: 8 poin** (1 poin per langkah + contoh, 1 poin struktur keseluruhan)

---

#### Jawaban Soal 11 ⭐⭐

**Tantangan wireless network forensics:**

1. **Broadcast medium**: Sinyal radio dapat ditangkap siapa saja dalam jangkauan tanpa koneksi fisik
2. **Encryption**: WPA2/WPA3 mengenkripsi payload; tanpa kunci, konten tidak bisa dibaca
3. **Attribution sulit**: MAC address dapat di-spoof, membuat identifikasi perangkat asli sulit
4. **Signal interference**: Gangguan sinyal menyebabkan packet loss dan capture tidak lengkap
5. **Legal issues**: Penangkapan komunikasi wireless memerlukan otorisasi khusus
6. **Ephemeral nature**: Koneksi wireless bersifat sementara, evidence cepat hilang

**Probe Request untuk investigasi:**
- Mengungkap SSID jaringan yang pernah terhubung (riwayat pergerakan)
- Mengidentifikasi MAC address perangkat dalam area monitoring
- Melacak kehadiran perangkat pada lokasi dan waktu tertentu

**Rogue AP Detection:**
- Membandingkan BSSID dengan inventaris AP resmi
- Mengidentifikasi Evil Twin (SSID sama, BSSID berbeda)
- Mendeteksi AP tanpa enkripsi di area keamanan (honeypot)

**Skor: 8 poin** (4 poin tantangan, 2 poin probe request, 2 poin rogue AP)

---

#### Jawaban Soal 12 ⭐⭐⭐

**Perhitungan estimasi data exfiltration:**

- Payload per paket: 512 bytes
- Frekuensi: 200 request/menit
- Durasi: 24 jam

Perhitungan:
```
Volume = 512 bytes × 200/menit × 60 menit × 24 jam
       = 512 × 200 × 60 × 24
       = 512 × 288,000
       = 147,456,000 bytes
       = ~140.6 MB dalam 24 jam
```

**Mengapa pola ini mencurigakan (perbandingan dengan ping normal):**

| Parameter | Normal Ping | Temuan Ini | Rasio Anomali |
|-----------|------------|------------|---------------|
| Frekuensi | 1-4/siklus | 200/menit | ~50-200× |
| Payload | 32-64 bytes | 512 bytes | ~8-16× |
| Durasi | Beberapa detik | 24 jam terus | Continuous |
| Destination | Bervariasi | Satu IP tetap | Persistent |
| Pola | Sporadis | Sangat reguler | Machine-like |

**Signifikansi:**
- 140 MB cukup untuk mengeksfiltrasikan ribuan dokumen atau database
- Pola reguler menunjukkan automated tool, bukan aktivitas manusia
- Satu destination IP menunjukkan komunikasi C2 terstruktur
- ICMP tunneling dipilih karena ping biasanya diizinkan oleh firewall

**Skor: 12 poin** (4 poin perhitungan benar, 4 poin perbandingan, 4 poin analisis signifikansi)

---

#### Jawaban Soal 13 ⭐⭐⭐

**Strategi bertahap analisis PCAP 50 GB:**

**Tahap 1: Overview** — Tool: `capinfos`
- Jalankan `capinfos file.pcap` untuk statistik dasar
- Informasi: jumlah paket, durasi capture, data rate, encapsulation
- Alasan: cepat, tidak memuat seluruh file ke memori

**Tahap 2: Automated Parsing** — Tool: `Zeek`
- Jalankan `zeek -r file.pcap`
- Generate log terstruktur: conn.log, dns.log, http.log, ssl.log, files.log
- Alasan: menghasilkan ringkasan terstruktur dari jutaan paket

**Tahap 3: Flow Summary** — Tool: `TShark`
- Jalankan `tshark -r file.pcap -q -z conv,tcp` untuk ringkasan konversi
- Identifikasi top talkers dan koneksi mencurigakan
- Alasan: scriptable, dapat diproses dengan awk/grep

**Tahap 4: Targeted Filtering** — Tool: `tcpdump`
- Jalankan `tcpdump -r file.pcap -w subset.pcap "host 192.168.1.100"` 
- Ekstrak subset berdasarkan IP, port, atau protokol yang mencurigakan
- Alasan: cepat untuk filtering berdasarkan BPF expression

**Tahap 5: Split** — Tool: `editcap`
- Jalankan `editcap -c 100000 subset.pcap split_`
- Pecah file besar menjadi bagian-bagian (100K paket)
- Alasan: file kecil dapat dibuka di Wireshark tanpa crash

**Tahap 6: Deep Analysis** — Tool: `Wireshark`
- Buka file subset yang sudah dipecah
- Gunakan Follow Stream, Export Objects, Expert Info
- Alasan: GUI interaktif untuk analisis per-paket mendalam

**Peringatan:** JANGAN langsung buka file 50 GB di Wireshark — akan menyebabkan crash atau hang.

**Skor: 12 poin** (2 poin per tahap × 6 tahap)

---

#### Jawaban Soal 14 ⭐⭐⭐

**Pertimbangan hukum network monitoring militer Indonesia:**

**Regulasi yang relevan:**
1. **UU No. 1/2024 (ITE)**: Intersepsi komunikasi elektronik memerlukan perintah pengadilan atau penegak hukum. Namun, monitoring pada jaringan organisasi sendiri dengan notifikasi dapat dikecualikan.
2. **UU No. 3/2002 (Pertahanan Negara)**: Memberikan wewenang kepada institusi pertahanan untuk pengamanan informasi dalam konteks pertahanan.
3. **PP No. 71/2019**: Mewajibkan penyelenggara sistem elektronik untuk melakukan logging dan monitoring.
4. **Perpres No. 82/2022 (BSSN)**: Koordinasi keamanan siber nasional.

**Prinsip yang harus diterapkan:**
1. **Otorisasi Komandan**: Monitoring harus diotorisasi secara tertulis oleh komandan yang berwenang
2. **Proporsionalitas**: Cakupan monitoring harus proporsional dengan ancaman yang dihadapi
3. **Notifikasi**: Banner login yang menginformasikan bahwa jaringan dimonitor
4. **Penyimpanan sesuai klasifikasi**: Data capture disimpan sesuai tingkat klasifikasi (rahasia, sangat rahasia)
5. **Akses terbatas**: Hanya personel yang berwenang dan memiliki clearance yang boleh mengakses data capture

**Dilema etika:**
- Keseimbangan antara keamanan nasional dan privasi personel
- Transparansi: personel harus tahu bahwa traffic dimonitor
- Scope creep: monitoring keamanan tidak boleh digunakan untuk tujuan lain
- Perlindungan data pribadi personel yang termasuk dalam capture

**Contoh banner login:**
```
PERINGATAN: Sistem ini milik TNI. Semua aktivitas dimonitor dan dicatat.
Penggunaan tidak sah akan diproses sesuai hukum yang berlaku.
```

**Skor: 12 poin** (4 poin regulasi, 4 poin prinsip, 4 poin etika)

---

#### Jawaban Soal 15 ⭐⭐⭐

**Rekonstruksi serangan dan mapping Cyber Kill Chain:**

| Waktu | Sumber | Event | Kill Chain |
|-------|--------|-------|-----------|
| 08:20:00 | DNS | Resolve exploit-kit.malware.net | **Delivery** |
| 08:20:01 | Firewall | ALLOW HTTP ke 203.0.113.50 | **Delivery** |
| 08:20:02 | IDS | Exploit Kit Landing Page | **Exploitation** |
| 08:20:05 | Proxy | Download payload.exe | **Installation** |
| 08:20:10 | Firewall | ALLOW HTTPS ke 198.51.100.25 | **Command & Control** |
| 08:20:11 | IDS | CnC Beacon Activity | **Command & Control** |
| 08:21:00 | Firewall | 5.2 MB outbound ke C2 | **Actions on Objectives** |
| 08:21:30 | IDS | Large Outbound Data Transfer | **Actions on Objectives** |

**Analisis detail:**
1. **Delivery**: Korban mengakses exploit kit melalui browser (DNS resolve menunjukkan initial access vector)
2. **Exploitation**: Exploit kit berhasil mengeksploitasi vulnerability pada browser/plugin
3. **Installation**: Payload.exe berhasil diunduh dan kemungkinan dieksekusi (karena C2 aktif 5 detik kemudian)
4. **C2**: Koneksi HTTPS ke server berbeda (198.51.100.25) menunjukkan C2 channel terpisah dari delivery
5. **Exfiltration**: 5.2 MB data dikirim ke C2 server hanya ~50 detik setelah instalasi

**Observasi kritis:**
- Seluruh kill chain selesai dalam **~90 detik** — menunjukkan serangan otomatis yang sangat efisien
- Dua IP berbeda digunakan: satu untuk delivery (203.0.113.50), satu untuk C2 (198.51.100.25)
- C2 menggunakan HTTPS (port 443) untuk menghindari deteksi

**Skor: 12 poin** (4 poin mapping benar, 4 poin analisis detail, 4 poin observasi kritis)

---

### Bagian C: Studi Kasus

#### Jawaban Studi Kasus 1

**1a. Identifikasi IOC (15 poin):**

Lima+ IOC yang teridentifikasi:

1. **Beaconing pattern 5 menit**: IDS alert pada interval sangat reguler (~5 menit) menunjukkan automated C2 communication, bukan aktivitas manusia
2. **Waktu aktivitas 02:00-04:30**: Aktivitas di luar jam kerja normal menunjukkan upaya menghindari deteksi
3. **JA3 Hash match Cobalt Strike** (`72a589da586844d7f0818ce684948eea`): Konfirmasi penggunaan framework post-exploitation Cobalt Strike
4. **Self-signed certificate (CN=localhost)**: Legitimate HTTPS server menggunakan certificate dari CA terpercaya, bukan self-signed
5. **DNS TXT query dengan Base64 encoding**: Query DNS yang mengandung data encoded (`dGhpcyBpcyBhIHRlc3Q`) menunjukkan DNS tunneling sebagai secondary C2 channel
6. **Large outbound transfer (45 MB)**: Volume data yang signifikan dikirim ke IP luar pada dini hari
7. **Komunikasi ke IP asing** (185.100.87.XXX) melalui HTTPS dari host di unit Intelijen

**1b. Identifikasi malware (10 poin):**

Malware yang digunakan: **Cobalt Strike Beacon**

JA3 fingerprinting membantu karena:
- Meskipun payload terenkripsi HTTPS, Client Hello TLS memiliki parameter unik
- JA3 hash `72a589da586844d7f0818ce684948eea` cocok dengan database threat intelligence untuk Cobalt Strike
- Cobalt Strike memiliki implementasi TLS sendiri yang berbeda dari browser standard
- Identifikasi ini tidak memerlukan dekripsi traffic sama sekali

Cobalt Strike adalah framework red team/post-exploitation yang sering digunakan oleh APT groups untuk operasi C2. Kemampuannya meliputi: beaconing, keystroke logging, screenshot, file transfer, lateral movement, dan privilege escalation.

**1c. Analisis pola beaconing (10 poin):**

**Alasan interval 5 menit:**
- Cukup sering untuk menerima perintah baru dalam waktu wajar
- Cukup jarang untuk tidak menimbulkan excessive traffic yang mudah terdeteksi
- Interval standar Cobalt Strike (configurable, default ~60 detik, biasanya dinaikkan untuk stealth)

**Alasan waktu 02:00-04:30:**
- Di luar jam kerja = aktivitas jaringan normal minimal (anomali lebih mudah bersembunyi di volume rendah paradoxically, tetapi juga lebih mudah terdeteksi jika ada monitoring)
- SOC biasanya memiliki manning lebih sedikit pada shift malam
- Exfiltration 45 MB pada dini hari mengurangi risiko deteksi visual oleh administrator

**Implikasi terhadap strategi deteksi:**
- Monitoring 24/7 wajib, terutama di luar jam kerja
- Deteksi beaconing berdasarkan interval reguler (statistical analysis)
- Alerting pada koneksi keluar yang terjadi pada jam tidak wajar
- Volume-based alerting untuk outbound transfer di atas threshold

**1d. Rencana investigasi forensik jaringan (15 poin):**

**Tools yang digunakan:**
1. **Wireshark**: Analisis mendalam PCAP, Follow Stream, Export Objects
2. **Zeek**: Generate log terstruktur dari full PCAP
3. **TShark**: Automated extraction dan filtering
4. **NetworkMiner**: Auto-extract file dan credential dari PCAP
5. **Volatility 3**: Memory forensics pada WS-INTEL-007
6. **FTK Imager**: Imaging disk WS-INTEL-007

**Data tambahan yang perlu dikumpulkan:**
1. Full PCAP dari TAP/SPAN selama periode insiden (minimal 2 minggu)
2. Memory dump dari WS-INTEL-007 (volatile data prioritas tinggi)
3. Disk image dari WS-INTEL-007
4. Firewall log lengkap (allow dan deny)
5. DNS query log dari DNS server internal
6. Authentication log (Active Directory)
7. Endpoint security log dari semua host di subnet

**Langkah-langkah analisis:**
1. Preservasi: Hash semua evidence file, dokumentasi chain of custody
2. PCAP analysis: Identifikasi semua koneksi dari 192.168.10.45 ke IP eksternal
3. DNS analysis: Analisis semua DNS query dari host, decode Base64 subdomain
4. Certificate analysis: Extract dan analisis certificate pada TLS connection
5. Timeline construction: Korelasikan log dari semua sumber
6. Lateral movement check: Apakah host lain berkomunikasi ke C2 yang sama?
7. Data loss assessment: Tentukan apa yang dieksfiltrasikan berdasarkan volume dan timing
8. Memory forensics: Identifikasi proses malicious, extract konfigurasi Cobalt Strike

**1e. Implikasi dan rekomendasi (15 poin):**

**Implikasi keamanan:**
- Workstation di unit Intelijen Kodam telah compromised — informasi sensitif mungkin bocor
- Cobalt Strike memungkinkan lateral movement — infeksi mungkin lebih luas
- 45 MB data dieksfiltrasikan — jenis informasi harus diidentifikasi segera
- Kepercayaan terhadap keamanan infrastruktur Kodam terpengaruh

**Rekomendasi mitigasi (minimal 5):**
1. **Immediate containment**: Isolasi WS-INTEL-007 dari jaringan, blokir IP C2 di firewall
2. **Network segmentation**: Pisahkan segmen intelijen dengan firewall internal yang ketat
3. **DNS monitoring**: Implementasi DNS sinkholing dan monitoring untuk DNS tunneling
4. **TLS inspection**: Deploy SSL/TLS inspection pada proxy untuk traffic keluar
5. **Behavioral monitoring**: Implementasi deteksi beaconing berbasis machine learning
6. **Endpoint protection**: Deploy EDR pada semua workstation critical
7. **After-hours monitoring**: Tingkatkan kapasitas SOC untuk monitoring 24/7
8. **Threat hunting**: Lakukan proactive hunt menggunakan IOC yang ditemukan pada seluruh jaringan

---

#### Jawaban Studi Kasus 2

**2a. Decode DNS query dan implikasi (15 poin):**

Decode Base64 dari subdomain:
```
dGFrdGlzIGRlZmVuc2UgcGxhbg → "taktis defense plan"
b3BlcmF0aW9uIHJlcG9ydCBxNA → "operation report q4"
c2hpcCBsb2NhdGlvbiBkYXRh   → "ship location data"
Y3JldyBtYW5pZmVzdCBkZXRhaWw → "crew manifest detail"
```

**Informasi yang dieksfiltrasikan:**
1. Rencana pertahanan taktis (taktis defense plan)
2. Laporan operasi kuartal 4 (operation report q4)
3. Data lokasi kapal (ship location data)
4. Detail manifest kru (crew manifest detail)

**Implikasi keamanan terhadap TNI AL:**
- **Lokasi kapal**: Informasi posisi KRI sangat rahasia; kebocoran membahayakan keselamatan kapal dan kru
- **Rencana pertahanan**: Adversary dapat mengeksploitasi kelemahan yang teridentifikasi dalam rencana
- **Manifest kru**: Data personel dapat digunakan untuk social engineering atau targeting
- **Laporan operasi**: Mengungkap kapabilitas dan pola operasi TNI AL

Ini merupakan ancaman serius terhadap **keamanan operasional (OPSEC)** TNI AL.

**2b. Estimasi volume exfiltration (10 poin):**

Perhitungan berdasarkan flow data:
```
Total DNS traffic 3 hari: 2.8 GB
Normal baseline: 50 MB/hari × 3 hari = 150 MB
Anomalous traffic: 2.8 GB - 150 MB = ~2.65 GB total DNS traffic anomali
```

Estimasi data efektif (mengingat overhead DNS):
```
Setiap DNS query: ~35 karakter subdomain × 0.75 (Base64 efficiency) ≈ 26 bytes data per query
Query rate: 150 queries/menit
Data per menit: 150 × 26 = 3,900 bytes/menit
Data per jam: 3,900 × 60 = 234,000 bytes/jam ≈ 228 KB/jam
Data per hari: 228 KB × 24 = ~5.5 MB/hari
Data 3 hari: ~16.5 MB efektif
```

Alternatif (berdasarkan total DNS anomali):
```
DNS overhead (header, response) ≈ 50-70% dari total
Data efektif ≈ 30-50% dari 2.65 GB = ~800 MB - 1.3 GB
```

Catatan: Pendekatan berbeda menghasilkan angka berbeda karena overhead DNS bervariasi. Yang penting adalah demonstrasi metodologi perhitungan.

**2c. Mengapa DNS tunneling efektif di lingkungan militer (10 poin):**

Minimal 4 alasan teknis dan operasional:

1. **DNS selalu diizinkan**: Firewall militer memblokir banyak protokol, tetapi DNS (port 53) hampir selalu diizinkan karena diperlukan untuk operasi normal
2. **Volume tersembunyi**: DNS query individual kecil; dalam volume normal, tunnel traffic bersembunyi di antara ribuan query legitimate
3. **Monitoring gap**: Kebanyakan monitoring fokus pada HTTP/HTTPS; DNS jarang diinspeksi secara mendalam
4. **Sulit diblokir tanpa dampak**: Memblokir DNS berarti menghentikan resolusi domain = menghentikan sebagian besar layanan jaringan
5. **Bypass proxy**: DNS biasanya tidak melewati proxy, menghindari inspeksi konten
6. **Enkripsi opsional**: Data bisa dienkripsi sebelum di-encode Base64, menambah lapisan keamanan bagi penyerang
7. **Resilience**: Jika satu DNS server diblokir, penyerang bisa menggunakan DNS resolver alternatif

**2d. Prosedur forensik lengkap (15 poin):**

**Bukti yang harus diamankan:**
1. DNS query log dari DNS server internal (minimal 30 hari)
2. Full PCAP dari sensor yang mencakup subnet 10.20.30.0/24
3. Flow data dari router core
4. Memory dump dari WS-LOGISTICS-003
5. Disk image dari WS-LOGISTICS-003
6. Firewall log
7. Endpoint security log

**Tools yang digunakan:**
1. **tcpdump**: Capture traffic DNS dari host tersangka
2. **Wireshark**: Analisis mendalam DNS queries, decode Base64
3. **Zeek**: Parse PCAP, generate dns.log terstruktur
4. **TShark**: Bulk extraction DNS queries: `tshark -r file.pcap -Y "dns.qry.type==16" -T fields -e dns.qry.name`
5. **Python script**: Automated Base64 decoding semua subdomain
6. **FTK Imager**: Forensic imaging disk
7. **Volatility 3**: Memory analysis

**Analisis yang dilakukan:**
1. Decode seluruh 147,852 subdomain unik untuk menentukan data yang bocor
2. Timeline analysis: kapan tunneling dimulai, apakah ada eskalasi
3. Identifikasi malware/tool yang digunakan untuk tunneling (iodine, dnscat2, dll.)
4. Korelasi dengan aktivitas user (apakah saat user login atau automated)
5. Periksa apakah ada host lain yang berkomunikasi ke domain yang sama
6. Filesystem analysis: identifikasi tool tunneling dan file yang diakses

**Format laporan forensik:**
1. Executive Summary
2. Deskripsi Insiden
3. Metodologi Investigasi
4. Timeline Kronologis
5. Temuan Teknis (IOC, decoded data, malware analysis)
6. Dampak Assessment
7. Rekomendasi
8. Lampiran (chain of custody, hash values, raw evidence)

**2e. Arsitektur keamanan DNS (15 poin):**

**Konfigurasi teknis:**
1. **DNS Sinkholing**: Redirect query ke domain suspicious ke internal sinkhole server
2. **DNS Firewall/RPZ (Response Policy Zone)**: Blokir domain yang terdaftar di threat intelligence
3. **DNS query monitoring**: Deploy tool seperti Zeek atau dnstap untuk log semua DNS query
4. **DNS query length limitation**: Blokir query dengan subdomain >50 karakter
5. **TXT record restriction**: Blokir atau alert pada DNS TXT query ke domain non-whitelist
6. **Encrypted DNS control**: Blokir DoH (DNS over HTTPS) dan DoT (DNS over TLS) ke resolver eksternal
7. **Internal recursive resolver only**: Memaksa semua DNS query melalui DNS server internal

**Kebijakan operasional:**
1. **Whitelist DNS destinations**: Hanya izinkan DNS query ke resolver internal dan upstream terverifikasi
2. **Baseline monitoring**: Establish baseline volume DNS per host, alert pada deviasi >200%
3. **Regular auditing**: Review DNS log mingguan untuk anomali
4. **Incident response plan**: Prosedur spesifik untuk DNS-based exfiltration
5. **User awareness**: Training personel tentang ancaman DNS tunneling
6. **Segmentation**: DNS server terpisah untuk jaringan classified vs administratif

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
