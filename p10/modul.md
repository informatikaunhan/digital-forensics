# Modul 10: Forensik Jaringan

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 10  
**Topik:** Forensik Jaringan  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Menjelaskan prinsip dasar forensik jaringan dan metodologi traffic analysis
2. Mengidentifikasi dan mengklasifikasikan jenis-jenis bukti jaringan (logs, packet captures, flow data)
3. Melakukan analisis protokol TCP/IP, HTTP/HTTPS, DNS, dan SMTP menggunakan Wireshark
4. Menganalisis artefak IDS/IPS dan firewall logs untuk rekonstruksi serangan
5. Menerapkan teknik wireless network forensics dalam investigasi insiden
6. Melakukan rekonstruksi serangan berbasis jaringan dan mendeteksi covert channels
7. Memahami tantangan analisis encrypted traffic dan pertimbangan hukum dalam network monitoring

---

## 1. Prinsip Dasar Forensik Jaringan

### 1.1 Definisi Forensik Jaringan

> **Forensik Jaringan (Network Forensics)** adalah cabang forensik digital yang berfokus pada pemantauan, penangkapan, pencatatan, dan analisis lalu lintas jaringan untuk tujuan pengumpulan bukti, deteksi intrusi, dan investigasi insiden keamanan.

Forensik jaringan sering disebut sebagai "network wiretapping" dalam konteks digital. Berbeda dengan forensik komputer yang menganalisis data statis pada media penyimpanan (seperti yang telah dibahas pada [Pertemuan 03](../p03/modul.md)–[07](../p07/modul.md)), forensik jaringan berfokus pada data yang bergerak melalui jaringan — data yang bersifat sangat volatile dan memerlukan penanganan khusus.

Marcus Ranum, salah satu pionir dalam bidang ini, mendefinisikan dua pendekatan utama dalam forensik jaringan:

| Pendekatan | Deskripsi | Analogi |
|------------|-----------|---------|
| **"Catch-it-as-you-can"** | Semua paket yang melewati titik lalu lintas tertentu ditangkap dan disimpan untuk analisis selanjutnya | Memasang kamera CCTV yang merekam semua aktivitas |
| **"Stop, look, and listen"** | Setiap paket dianalisis secara real-time dalam memori, hanya informasi tertentu yang disimpan | Penjaga keamanan yang mengawasi dan mencatat aktivitas mencurigakan |

Dalam konteks militer Indonesia, forensik jaringan menjadi komponen kritis dalam:

1. **Deteksi intrusi** terhadap jaringan pertahanan (seperti jaringan Kodam atau Mabes TNI)
2. **Investigasi kebocoran data** melalui analisis lalu lintas keluar (egress traffic)
3. **Atribusi serangan siber** yang menargetkan infrastruktur militer kritis
4. **Operasi intelijen siber** untuk mengidentifikasi ancaman dan aktor yang terlibat

![Konsep Forensik Jaringan](images/p10-01-network-forensics-concept.png)

*Gambar 10.1: Konsep dan ruang lingkup forensik jaringan*

### 1.2 Perbedaan dengan Forensik Komputer

Memahami perbedaan antara forensik jaringan dan forensik komputer (yang telah dipelajari pada [Pertemuan 03](../p03/modul.md)–[07](../p07/modul.md)) sangat penting untuk menentukan pendekatan investigasi yang tepat:

| Aspek | Forensik Komputer | Forensik Jaringan |
|-------|-------------------|-------------------|
| **Sumber Data** | Hard disk, memori, registry | Packet captures, logs, flow data |
| **Sifat Data** | Relatif statis (data at rest) | Sangat dinamis (data in motion) |
| **Preservasi** | Imaging media penyimpanan | Real-time capture atau log retention |
| **Volume** | Terbatas pada kapasitas media | Potensial sangat besar (terabyte/hari) |
| **Volatilitas** | Rendah-sedang | Sangat tinggi |
| **Cakupan** | Satu sistem/perangkat | Seluruh segmen jaringan |
| **Waktu** | Post-mortem analysis | Real-time atau near-real-time |
| **Konteks** | Aktivitas pada satu host | Komunikasi antar host |

#### Solved Problem 1 ⭐

**Soal:** Seorang investigator forensik di Kodam IX/Udayana menerima laporan tentang kemungkinan kebocoran data rahasia melalui jaringan. Jelaskan mengapa forensik jaringan lebih tepat digunakan sebagai pendekatan utama dibandingkan forensik komputer pada kasus ini!

**Penyelesaian:**

*Step 1:* Identifikasi karakteristik kasus — kebocoran data melalui jaringan berarti data telah berpindah dari satu titik ke titik lain.

*Step 2:* Analisis keunggulan forensik jaringan untuk kasus ini:
- **Deteksi jalur exfiltration**: Forensik jaringan dapat mengidentifikasi ke mana data dikirim (IP tujuan, domain, protokol yang digunakan)
- **Volume transfer**: Analisis traffic dapat menunjukkan volume data yang ditransfer dan anomali dalam pola lalu lintas
- **Waktu kejadian**: Timestamp pada packet capture memberikan kronologi yang presisi tentang kapan kebocoran terjadi
- **Identifikasi metode**: Dapat mengungkap apakah data dikirim melalui HTTP, DNS tunneling, encrypted channel, atau metode lainnya

*Step 3:* Limitasi forensik komputer dalam kasus ini:
- Forensik komputer hanya menunjukkan data diakses, tetapi tidak secara langsung membuktikan data tersebut telah dikirim keluar
- Jika pelaku menggunakan teknik anti-forensik pada host (seperti yang akan dibahas pada [Pertemuan 12](../p12/modul.md)), bukti di komputer mungkin sudah dihapus

**Jawaban:** Forensik jaringan lebih tepat karena kasus ini melibatkan perpindahan data melalui jaringan. Forensik jaringan dapat mengidentifikasi tujuan pengiriman data, volume transfer, waktu kejadian secara presisi, dan metode exfiltration yang digunakan — informasi yang sulit diperoleh hanya dari analisis komputer sumber.

### 1.3 Arsitektur Pengumpulan Bukti Jaringan

Untuk melakukan forensik jaringan secara efektif, diperlukan arsitektur pengumpulan bukti yang tepat. Berikut adalah komponen-komponen kunci:

#### 1.3.1 Network TAP (Test Access Point)

> **Network TAP** adalah perangkat keras yang dipasang secara inline pada segmen jaringan untuk menyalin semua lalu lintas yang melewatinya tanpa memengaruhi performa jaringan.

Karakteristik Network TAP:
- **Passive**: Tidak mengubah atau mengganggu lalu lintas
- **Full-duplex capture**: Menangkap lalu lintas dua arah
- **Fail-safe**: Jika TAP gagal, lalu lintas jaringan tetap mengalir
- **Tidak terdeteksi**: Tidak memiliki alamat IP atau MAC pada jaringan

#### 1.3.2 SPAN/Mirror Port

> **SPAN (Switched Port Analyzer)** adalah fitur pada switch yang mengkopi lalu lintas dari satu atau beberapa port ke port monitoring yang ditentukan.

```
Konfigurasi SPAN pada Cisco Switch:
Switch(config)# monitor session 1 source interface Gi0/1 - Gi0/24
Switch(config)# monitor session 1 destination interface Gi0/48
```

| Aspek | Network TAP | SPAN Port |
|-------|-------------|-----------|
| **Biaya** | Tinggi (perangkat tambahan) | Rendah (fitur bawaan switch) |
| **Performa** | Tidak memengaruhi jaringan | Dapat memengaruhi switch CPU |
| **Paket Hilang** | Tidak ada | Mungkin terjadi saat beban tinggi |
| **Konfigurasi** | Instalasi fisik | Konfigurasi software |
| **Full-duplex** | Ya, selalu | Tergantung konfigurasi |

#### 1.3.3 Posisi Strategis Sensor

Dalam konteks jaringan militer, penempatan sensor forensik harus mempertimbangkan:

```
[Internet] ──── [Firewall] ──── [DMZ] ──── [IDS/IPS] ──── [Core Switch] ──── [Endpoints]
                    │                          │                  │
                  TAP #1                     TAP #2            SPAN Port
              (Perimeter)              (Pre-Firewall)      (Internal Traffic)
```

- **TAP #1 (Perimeter)**: Menangkap semua lalu lintas masuk/keluar dari jaringan
- **TAP #2 (Pre-Firewall)**: Menangkap lalu lintas sebelum difilter oleh IDS/IPS
- **SPAN Port (Internal)**: Memonitor lalu lintas antar host internal

#### Solved Problem 2 ⭐⭐

**Soal:** Tim Satsiber (Satuan Siber) TNI merencanakan pemasangan infrastruktur forensik jaringan di sebuah pangkalan militer. Jelaskan di mana saja sensor forensik sebaiknya dipasang dan alasannya!

**Penyelesaian:**

*Step 1:* Identifikasi topologi jaringan tipikal pangkalan militer:
- Koneksi internet (gateway)
- DMZ (server publik)
- Jaringan operasional (classified)
- Jaringan administratif (unclassified)
- Jaringan nirkabel

*Step 2:* Tentukan posisi sensor untuk cakupan optimal:

| Posisi | Jenis Sensor | Alasan |
|--------|-------------|--------|
| **Gateway Internet** | Network TAP | Menangkap semua komunikasi eksternal; kritis untuk deteksi exfiltration dan C2 communication |
| **Antara DMZ dan LAN** | Network TAP | Mendeteksi lateral movement dari DMZ ke jaringan internal |
| **Core Switch Internal** | SPAN Port | Memonitor lalu lintas antar segmen jaringan; mendeteksi insider threat |
| **Segmen Classified** | Network TAP + Full Packet Capture | Segmen paling sensitif memerlukan recording lengkap untuk audit |
| **Wireless Access Points** | Wireless IDS/Sensor | Mendeteksi rogue access point dan unauthorized wireless access |

*Step 3:* Pertimbangan khusus militer:
- Sensor pada segmen classified harus memiliki sertifikasi keamanan
- Semua data capture harus disimpan dengan enkripsi
- Retensi data minimal 90 hari untuk kebutuhan investigasi

**Jawaban:** Sensor forensik sebaiknya dipasang di lima titik strategis: gateway internet (TAP untuk semua komunikasi eksternal), antara DMZ dan LAN (TAP untuk deteksi lateral movement), core switch (SPAN untuk lalu lintas internal), segmen classified (TAP + full capture untuk audit), dan wireless access points (wireless IDS untuk deteksi akses tidak sah).

---

## 2. Bukti Digital dari Jaringan (Network Evidence)

### 2.1 Jenis-Jenis Bukti Jaringan

Bukti digital dari jaringan dapat dikategorikan dalam tiga tingkatan berdasarkan kedetailan informasi yang direkam:

#### 2.1.1 Full Packet Capture (PCAP)

> **Full Packet Capture** adalah metode perekaman yang menangkap keseluruhan paket data yang melewati titik monitoring, termasuk header dan payload.

Full packet capture menyimpan informasi paling lengkap dan merupakan standar emas dalam forensik jaringan. Format file standar adalah **PCAP** (Packet Capture) atau **PCAPNG** (PCAP Next Generation).

Contoh informasi yang dapat diekstrak dari PCAP:
- Seluruh konten komunikasi HTTP yang tidak terenkripsi
- File yang ditransfer melalui jaringan
- Kredensial yang dikirim dalam plain text
- Query DNS dan respons
- Metadata komunikasi terenkripsi (TLS handshake, certificate info)

Kelebihan dan kekurangan:

| Aspek | Keterangan |
|-------|------------|
| **Kelebihan** | Informasi paling lengkap; dapat merekonstruksi sesi lengkap |
| **Kekurangan** | Memerlukan storage sangat besar; pada jaringan 1 Gbps, ~10 TB/hari |
| **Cocok untuk** | Investigasi mendalam; segmen jaringan sensitif |

#### 2.1.2 Flow Data (NetFlow/sFlow/IPFIX)

> **Flow Data** adalah ringkasan metadata komunikasi jaringan yang mencatat informasi tentang siapa berkomunikasi dengan siapa, kapan, menggunakan protokol apa, dan berapa banyak data yang ditransfer — tanpa merekam isi komunikasi.

Elemen standar dalam satu flow record:

```
Source IP       : 192.168.1.100
Destination IP  : 203.0.113.50
Source Port     : 49152
Destination Port: 443
Protocol        : TCP (6)
Bytes           : 1,245,678
Packets         : 847
Start Time      : 2025-01-15 08:23:45
End Time        : 2025-01-15 08:25:12
TCP Flags       : SYN, ACK, PSH, FIN
```

| Format | Vendor | Versi | Keunggulan |
|--------|--------|-------|------------|
| **NetFlow** | Cisco | v5, v9 | Standar industri; didukung luas |
| **sFlow** | InMon | v5 | Sampling-based; overhead rendah |
| **IPFIX** | IETF | RFC 7011 | Standar terbuka; fleksibel |
| **jFlow** | Juniper | - | Kompatibel NetFlow v5/v9 |

#### 2.1.3 Log Files

Log files dari berbagai perangkat jaringan menyediakan catatan aktivitas yang penting untuk forensik:

| Sumber Log | Informasi Kunci | Format Umum |
|------------|-----------------|-------------|
| **Firewall** | Koneksi yang diizinkan/ditolak, rule yang terpicu | Syslog, CSV, proprietary |
| **IDS/IPS** | Alert keamanan, signature match, anomali | Syslog, JSON, PCAP (trigger) |
| **Proxy Server** | URL yang diakses, user agent, response code | Common Log Format (CLF) |
| **DNS Server** | Query dan response, NXDOMAIN | Syslog, query log |
| **DHCP Server** | IP assignment, MAC address, lease time | Syslog |
| **VPN Gateway** | Koneksi VPN, user authentication, durasi | Syslog, proprietary |
| **Web Server** | Access log, error log, request details | CLF, Combined Log Format |

Contoh format Combined Log Format dari web server:

```
203.0.113.50 - admin [15/Jan/2025:08:23:45 +0700] "GET /admin/config.php HTTP/1.1" 200 4523 "http://192.168.1.10/login" "Mozilla/5.0 (Windows NT 10.0)"
```

Interpretasi:
- `203.0.113.50` — IP sumber
- `admin` — user terautentikasi
- `[15/Jan/2025:08:23:45 +0700]` — timestamp
- `"GET /admin/config.php HTTP/1.1"` — HTTP request
- `200` — status code (OK)
- `4523` — ukuran response dalam bytes
- URL referrer dan user agent string

#### Solved Problem 3 ⭐

**Soal:** Sebuah pangkalan Lantamal (Pangkalan Utama TNI AL) memiliki anggaran terbatas untuk implementasi forensik jaringan. Mereka hanya dapat memilih satu dari tiga jenis bukti jaringan (full packet capture, flow data, atau log files). Jenis bukti mana yang sebaiknya diprioritaskan dan mengapa?

**Penyelesaian:**

*Step 1:* Evaluasi setiap opsi:

| Kriteria | Full PCAP | Flow Data | Log Files |
|----------|-----------|-----------|-----------|
| **Biaya Storage** | Sangat tinggi | Rendah | Sedang |
| **Detail** | Sangat lengkap | Metadata saja | Bervariasi |
| **Ease of Use** | Butuh expertise | Mudah dianalisis | Familiar |
| **Existing Infra** | Butuh sensor baru | Fitur bawaan router | Sudah ada |

*Step 2:* Dengan anggaran terbatas, **flow data** menjadi pilihan paling cost-effective:
- Hampir semua router Cisco/Juniper sudah mendukung NetFlow/jFlow tanpa biaya tambahan
- Storage requirement sangat kecil dibanding full PCAP (rasio ~500:1)
- Cukup untuk mendeteksi anomali pola komunikasi, volume transfer abnormal, dan koneksi ke IP mencurigakan

*Step 3:* Strategi pelengkap: aktifkan juga log dari firewall dan IDS yang sudah ada (tanpa biaya tambahan), sebagai lapisan informasi tambahan di atas flow data.

**Jawaban:** Dengan anggaran terbatas, flow data (NetFlow/sFlow) sebaiknya diprioritaskan karena biaya implementasi rendah (sudah menjadi fitur bawaan sebagian besar router), kebutuhan storage minimal, dan tetap memberikan visibilitas yang memadai terhadap pola komunikasi jaringan untuk deteksi anomali dan investigasi awal.

### 2.2 Preservasi Bukti Jaringan

Preservasi bukti jaringan memerlukan pendekatan khusus mengingat sifatnya yang volatile:

#### 2.2.1 Chain of Custody untuk Bukti Jaringan

Prinsip chain of custody (seperti yang telah dipelajari pada [Pertemuan 02](../p02/modul.md)) tetap berlaku dengan adaptasi khusus:

```
Dokumentasi Chain of Custody - Bukti Jaringan
=============================================
Nomor Bukti      : NF-2025-001
Jenis             : Full Packet Capture (PCAP)
Sumber            : TAP pada gateway utama Kodam IX/Udayana
Periode Capture   : 2025-01-15 00:00:00 - 2025-01-15 23:59:59
Ukuran File       : 2.3 TB
Hash MD5          : a1b2c3d4e5f6g7h8i9j0...
Hash SHA-256      : 1234abcd5678efgh...
Tool Capture      : tcpdump 4.9.3 / libpcap 1.10.1
Filter Applied    : none (full capture)
Capture Interface : eth0 (connected to TAP)
Link Speed        : 1 Gbps
Operator          : Kapten Inf. [nama]
```

Langkah-langkah preservasi:

1. **Hitung hash** file PCAP segera setelah capture selesai
2. **Buat salinan forensik** (minimal 2 copy pada media terpisah)
3. **Dokumentasikan** semua metadata: tool, versi, filter, interface
4. **Simpan dengan aman** di media write-protected
5. **Catat setiap akses** terhadap file bukti

#### Solved Problem 4 ⭐⭐

**Soal:** Seorang investigator forensik menangkap paket selama 24 jam pada jaringan 100 Mbps di sebuah markas militer. Estimasikan ukuran file PCAP yang dihasilkan dan jelaskan strategi penyimpanan yang tepat!

**Penyelesaian:**

*Step 1:* Hitung estimasi ukuran file PCAP:
- Bandwidth: 100 Mbps
- Asumsi utilisasi rata-rata: ~30% (jaringan perkantoran militer)
- Data per detik: 100 Mbps × 30% = 30 Mbps = 3.75 MB/s
- Data per jam: 3.75 MB/s × 3,600 s = 13,500 MB ≈ 13.5 GB/jam
- **Data per 24 jam: 13.5 GB × 24 = ~324 GB**

*Step 2:* Pertimbangan overhead PCAP:
- Header PCAP per paket: ~16 bytes
- Dengan rata-rata ~1000 paket/detik pada 30% utilisasi
- Overhead header: ~16 KB/s × 86,400 s ≈ 1.3 GB (relatif kecil)
- **Total estimasi: ~325 GB**

*Step 3:* Strategi penyimpanan:
- Gunakan **storage NAS/SAN** dengan kapasitas minimal 1 TB (untuk buffer beberapa hari)
- Terapkan **retention policy**: simpan full PCAP selama 7-14 hari, konversi ke flow data untuk penyimpanan jangka panjang (90+ hari)
- Gunakan **kompresi gzip** pada file PCAP yang sudah selesai (rasio kompresi ~50-60%)
- Setelah kompresi: ~325 GB × 50% = ~163 GB

**Jawaban:** File PCAP dari capture 24 jam pada jaringan 100 Mbps dengan utilisasi 30% diestimasi berukuran ~325 GB. Strategi penyimpanan yang tepat meliputi NAS/SAN dengan kapasitas 1+ TB, retention policy 7-14 hari untuk full PCAP dengan konversi ke flow data untuk jangka panjang, dan kompresi gzip untuk mengurangi ukuran ~50%.

---

## 3. Analisis Protokol Jaringan

### 3.1 Model TCP/IP dan Relevansi Forensik

Pemahaman mendalam tentang model TCP/IP sangat penting untuk analisis forensik jaringan. Setiap layer memberikan informasi yang berbeda untuk investigasi:

| Layer | Protokol | Informasi Forensik |
|-------|----------|---------------------|
| **Application (Layer 7)** | HTTP, HTTPS, DNS, SMTP, FTP | Konten komunikasi, URL, email, file transfer |
| **Transport (Layer 4)** | TCP, UDP | Port number, session tracking, flags |
| **Internet (Layer 3)** | IP, ICMP | Alamat IP sumber/tujuan, TTL, routing |
| **Network Access (Layer 2)** | Ethernet, Wi-Fi | MAC address, VLAN tags |

### 3.2 Analisis TCP

#### 3.2.1 TCP Three-Way Handshake

Setiap koneksi TCP dimulai dengan three-way handshake. Memahami proses ini penting untuk mengidentifikasi koneksi yang berhasil versus percobaan scanning:

```
Client (192.168.1.100)          Server (10.0.0.1:80)
         |                              |
         |---- SYN (seq=1000) -------->|
         |                              |
         |<--- SYN-ACK (seq=2000, ------|
         |     ack=1001)                |
         |                              |
         |---- ACK (seq=1001, -------->|
         |     ack=2001)                |
         |                              |
         |    [Koneksi Established]      |
```

#### 3.2.2 TCP Flags dan Signifikansi Forensik

| Flag | Hex | Signifikansi Forensik |
|------|-----|----------------------|
| **SYN** | 0x02 | Inisiasi koneksi; banyak SYN tanpa ACK = SYN scan/flood |
| **ACK** | 0x10 | Koneksi established; ACK scan untuk bypass firewall |
| **FIN** | 0x01 | Terminasi normal; FIN scan untuk stealth port scanning |
| **RST** | 0x04 | Koneksi direset; port tertutup atau filtered |
| **PSH** | 0x08 | Push data ke aplikasi; mengindikasikan transfer data aktif |
| **URG** | 0x20 | Data urgent; jarang digunakan, bisa jadi anomali |
| **SYN+ACK** | 0x12 | Respons SYN; port terbuka |
| **RST+ACK** | 0x14 | Port tertutup (respons normal) |

Contoh filter Wireshark untuk analisis TCP flags:

```
# Mendeteksi SYN scan (SYN tanpa completion)
tcp.flags.syn == 1 && tcp.flags.ack == 0

# Mendeteksi RST flood  
tcp.flags.reset == 1

# Koneksi yang berhasil established
tcp.flags == 0x12

# Christmas Tree scan (semua flags aktif)
tcp.flags.fin == 1 && tcp.flags.syn == 1 && tcp.flags.rst == 1 && tcp.flags.psh == 1 && tcp.flags.ack == 1 && tcp.flags.urg == 1
```

#### Solved Problem 5 ⭐⭐

**Soal:** Dalam sebuah PCAP file dari jaringan Kodam, ditemukan pola berikut dari IP 203.0.113.50 menuju berbagai port pada server 10.0.0.5:

```
Paket 1: 203.0.113.50:45231 → 10.0.0.5:22   [SYN] → [RST, ACK]
Paket 2: 203.0.113.50:45232 → 10.0.0.5:80   [SYN] → [SYN, ACK] → [RST]
Paket 3: 203.0.113.50:45233 → 10.0.0.5:443  [SYN] → [SYN, ACK] → [RST]
Paket 4: 203.0.113.50:45234 → 10.0.0.5:3389 [SYN] → [RST, ACK]
Paket 5: 203.0.113.50:45235 → 10.0.0.5:8080 [SYN] → [RST, ACK]
Paket 6: 203.0.113.50:45236 → 10.0.0.5:3306 [SYN] → [SYN, ACK] → [RST]
```

Identifikasi jenis aktivitas ini dan tentukan port mana yang terbuka!

**Penyelesaian:**

*Step 1:* Analisis pola — satu IP sumber mengirim SYN ke banyak port berbeda pada satu target. Ini adalah karakteristik **TCP SYN scan (port scanning)**.

*Step 2:* Identifikasi port status berdasarkan respons:
- **SYN → RST, ACK**: Port **tertutup** (server langsung menolak)
- **SYN → SYN, ACK → RST**: Port **terbuka** (server merespons, scanner mengirim RST untuk menutup tanpa melengkapi handshake)

*Step 3:* Klasifikasi:

| Port | Service | Respons | Status |
|------|---------|---------|--------|
| 22 | SSH | RST, ACK | **Tertutup** |
| 80 | HTTP | SYN, ACK → RST | **Terbuka** |
| 443 | HTTPS | SYN, ACK → RST | **Terbuka** |
| 3389 | RDP | RST, ACK | **Tertutup** |
| 8080 | HTTP Alt | RST, ACK | **Tertutup** |
| 3306 | MySQL | SYN, ACK → RST | **Terbuka** |

*Step 4:* Implikasi keamanan — port MySQL (3306) terbuka dan dapat diakses dari luar jaringan merupakan risiko keamanan tinggi, terutama karena scanner eksternal dapat mendeteksinya.

**Jawaban:** Aktivitas ini adalah TCP SYN scan (port scanning) dari IP eksternal 203.0.113.50. Port yang terbuka adalah: 80 (HTTP), 443 (HTTPS), dan 3306 (MySQL). Port 3306 yang terbuka untuk akses eksternal merupakan temuan keamanan kritis yang memerlukan tindakan segera.

### 3.3 Analisis DNS

#### 3.3.1 Forensik DNS

> **DNS (Domain Name System)** adalah protokol yang menerjemahkan nama domain menjadi alamat IP. Dalam forensik jaringan, DNS merupakan salah satu sumber bukti paling berharga karena hampir semua aktivitas internet dimulai dengan query DNS.

Informasi forensik dari DNS:

| Elemen DNS | Nilai Forensik |
|------------|----------------|
| **Query name** | Domain yang diakses oleh host |
| **Query type** | A (IPv4), AAAA (IPv6), MX (mail), TXT (text record) |
| **Response IP** | Server yang direspon; dapat berubah jika DNS hijacking |
| **TTL** | Indikator caching; TTL rendah sering digunakan C2 |
| **NXDOMAIN** | Domain tidak ada; banyak NXDOMAIN = DGA malware |
| **TXT records** | Sering digunakan untuk DNS tunneling dan data exfiltration |

Filter Wireshark untuk analisis DNS:

```
# Semua query DNS
dns.qry.name

# DNS query ke domain tertentu
dns.qry.name contains "suspicious-domain.com"

# DNS response NXDOMAIN (domain tidak ditemukan)
dns.flags.rcode == 3

# DNS TXT records (potential tunneling)
dns.qry.type == 16

# DNS query dengan panjang nama tidak wajar (>50 karakter)
dns.qry.name.len > 50
```

#### 3.3.2 DNS Tunneling

> **DNS Tunneling** adalah teknik yang menyalahgunakan protokol DNS untuk mengirimkan data melalui query dan response DNS, sering digunakan untuk data exfiltration atau komunikasi Command and Control (C2) karena DNS jarang diblokir oleh firewall.

Indikator DNS tunneling:

```
Contoh query DNS normal:
www.example.com          → Panjang: 15 karakter

Contoh query DNS tunneling:
aGVsbG8gd29ybGQ.data.evil.com    → Panjang: 34 karakter
(Base64 encoded "hello world" sebagai subdomain)

bXlfc2VjcmV0X2RhdGFfMTIzNDU2.c2.evil.com  → Panjang: 48 karakter
(Encoded secret data sebagai subdomain)
```

Karakteristik DNS tunneling yang dapat dideteksi:

1. **Query name panjang tidak wajar** (>30 karakter untuk subdomain)
2. **Volume query tinggi** ke satu domain tertentu
3. **Tipe query TXT atau NULL** yang tidak lazim
4. **Entropy tinggi** pada subdomain (data encoded)
5. **Pola request periodik** (beaconing)

#### Solved Problem 6 ⭐⭐⭐

**Soal:** Berikut adalah sampel DNS query yang ditemukan dalam PCAP dari jaringan markas TNI:

```
Query 1: www.google.com                          (A record)
Query 2: mail.kemhan.go.id                       (MX record)
Query 3: dGhpcyBpcyBhIHRlc3Q.data.xyz123.net    (TXT record)
Query 4: c2VjcmV0IGZpbGUgY29udGVudA.data.xyz123.net  (TXT record)
Query 5: www.bing.com                            (A record)
Query 6: ZW5jcnlwdGVkIGRhdGEgaGVyZQ.data.xyz123.net  (TXT record)
Query 7: update.microsoft.com                    (A record)
```

Identifikasi query mana yang mencurigakan dan jelaskan alasannya!

**Penyelesaian:**

*Step 1:* Analisis setiap query:
- Query 1, 5, 7: Domain terkenal dan lazim — **normal**
- Query 2: Domain pemerintah Indonesia — **normal**
- Query 3, 4, 6: Subdomain panjang dengan karakter Base64, semua menuju domain yang sama (xyz123.net), tipe TXT — **sangat mencurigakan**

*Step 2:* Decode Base64 pada query mencurigakan:
- `dGhpcyBpcyBhIHRlc3Q` → "this is a test"
- `c2VjcmV0IGZpbGUgY29udGVudA` → "secret file content"
- `ZW5jcnlwdGVkIGRhdGEgaGVyZQ` → "encrypted data here"

*Step 3:* Indikator DNS tunneling terpenuhi:
- ✅ Subdomain sangat panjang dengan encoded data
- ✅ Tipe query TXT (tidak lazim untuk browsing normal)
- ✅ Semua query menuju satu domain yang sama (xyz123.net)
- ✅ Data yang di-encode mengandung konten bermakna
- ✅ Domain tujuan tidak dikenal/tidak lazim

**Jawaban:** Query 3, 4, dan 6 sangat mencurigakan dan merupakan indikasi kuat **DNS tunneling** untuk data exfiltration. Subdomain berisi data Base64-encoded yang ketika di-decode mengungkap teks bermakna ("this is a test", "secret file content", "encrypted data here"). Semua query mengarah ke domain yang sama (xyz123.net) dengan tipe TXT record. Ini menunjukkan adanya upaya pengiriman data rahasia melalui saluran DNS.

### 3.4 Analisis HTTP/HTTPS

#### 3.4.1 Forensik HTTP

HTTP adalah protokol clear-text yang memberikan informasi sangat kaya untuk forensik:

```
GET /documents/classified/report-2025.pdf HTTP/1.1
Host: internal-server.kodam.mil.id
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36
Accept: application/pdf
Cookie: session_id=a1b2c3d4e5; user=admin
Referer: http://internal-server.kodam.mil.id/documents/
Authorization: Basic YWRtaW46cGFzc3dvcmQxMjM=
```

Informasi forensik yang dapat diekstrak:

| Elemen | Informasi | Nilai Forensik |
|--------|-----------|----------------|
| **Method** | GET, POST, PUT, DELETE | Jenis aksi yang dilakukan |
| **URI** | /documents/classified/report-2025.pdf | File yang diakses |
| **Host** | internal-server.kodam.mil.id | Server target |
| **User-Agent** | Mozilla/5.0... | Browser/tool yang digunakan |
| **Cookie** | session_id=...; user=admin | Identitas session dan user |
| **Authorization** | Basic YWRtaW46cGFzc3dvcmQxMjM= | Kredensial (Base64: admin:password123) |
| **Referer** | URL sebelumnya | Navigasi pengguna |

#### 3.4.2 Forensik HTTPS dan Tantangannya

HTTPS menggunakan TLS (Transport Layer Security) untuk mengenkripsi komunikasi, sehingga payload tidak dapat dibaca langsung dari packet capture. Namun, beberapa informasi tetap tersedia:

| Informasi yang **Tersedia** | Informasi yang **Terenkripsi** |
|-----------------------------|-------------------------------|
| IP sumber dan tujuan | URL lengkap (path) |
| SNI (Server Name Indication) | Konten request/response |
| Certificate details | Header HTTP |
| Ukuran data yang ditransfer | Cookie dan kredensial |
| Pola timing koneksi | Body payload |
| TLS version dan cipher suite | Query string |

```
# Filter Wireshark untuk analisis TLS/HTTPS
# SNI (Server Name Indication) - domain yang diakses
tls.handshake.extensions_server_name

# TLS certificate issuer
tls.handshake.certificate

# TLS version
tls.handshake.version

# Cipher suite yang digunakan
tls.handshake.ciphersuite
```

#### Solved Problem 7 ⭐⭐

**Soal:** Dalam analisis PCAP dari jaringan operasional TNI, ditemukan HTTP request berikut:

```
POST /upload.php HTTP/1.1
Host: file-share.suspicious-site.net
Content-Type: multipart/form-data; boundary=--abc123
Content-Length: 15728640
User-Agent: curl/7.68.0

----abc123
Content-Disposition: form-data; name="file"; filename="laporan_operasi_q4.xlsx"
Content-Type: application/vnd.openxmlformats-officedocument.spreadsheetml.sheet
[binary data...]
```

Analisis temuan ini dari perspektif forensik!

**Penyelesaian:**

*Step 1:* Identifikasi elemen mencurigakan:
- **Method POST ke /upload.php**: Upload file ke server eksternal
- **Host: file-share.suspicious-site.net**: Domain mencurigakan, bukan layanan file-sharing terkenal
- **Filename: laporan_operasi_q4.xlsx**: Nama file mengindikasikan dokumen operasional militer
- **Content-Length: 15,728,640 bytes (~15 MB)**: File berukuran besar
- **User-Agent: curl/7.68.0**: Digunakan command-line tool, bukan browser — mengindikasikan transfer otomatis atau disengaja

*Step 2:* Rekonstruksi skenario:
Seseorang menggunakan tool command-line `curl` untuk mengupload file "laporan_operasi_q4.xlsx" (kemungkinan laporan operasi kuartal 4) ke website eksternal yang mencurigakan. Ini merupakan indikasi kuat **data exfiltration**.

*Step 3:* Langkah investigasi lanjutan:
1. Identifikasi host internal mana yang mengirim request ini (dari IP sumber)
2. Extract file dari PCAP (Wireshark → File → Export Objects → HTTP)
3. Analisis file yang diextract untuk menentukan tingkat klasifikasi
4. Korelasikan dengan log autentikasi untuk mengidentifikasi user
5. Periksa history DNS dan koneksi lain ke domain suspicious-site.net

**Jawaban:** Temuan ini merupakan indikasi kuat data exfiltration. Seseorang menggunakan curl (command-line) untuk mengupload dokumen operasional militer berukuran ~15 MB ke server eksternal mencurigakan. Elemen kunci mencurigakan meliputi penggunaan command-line tool (bukan browser), domain tujuan yang tidak dikenal, nama file yang mengindikasikan dokumen operasional, dan ukuran file yang besar.

### 3.5 Analisis SMTP/Email

#### 3.5.1 Forensik Protokol Email

> **SMTP (Simple Mail Transfer Protocol)** adalah protokol standar untuk pengiriman email. Dalam forensik jaringan, analisis SMTP pada level paket dapat mengungkap komunikasi email yang tidak terdeteksi oleh log server email.

Anatomi komunikasi SMTP dalam packet capture:

```
C: EHLO client.example.com
S: 250-mail.server.com Hello
C: MAIL FROM:<sender@example.com>
S: 250 OK
C: RCPT TO:<recipient@external.com>
S: 250 OK
C: DATA
S: 354 Start mail input
C: From: sender@example.com
C: To: recipient@external.com
C: Subject: Project Update
C: Date: Wed, 15 Jan 2025 08:30:00 +0700
C: Content-Type: multipart/mixed; boundary="boundary123"
C: 
C: --boundary123
C: Content-Type: text/plain
C: 
C: Please find the attached document.
C: 
C: --boundary123
C: Content-Type: application/pdf
C: Content-Disposition: attachment; filename="report.pdf"
C: Content-Transfer-Encoding: base64
C: 
C: [Base64 encoded file data]
C: --boundary123--
C: .
S: 250 OK Message queued
```

Filter Wireshark untuk analisis SMTP:

```
# Semua traffic SMTP
smtp

# Email dengan attachment
smtp.req.parameter contains "filename"

# Email ke domain tertentu
smtp.req.parameter contains "@external-domain.com"

# SMTP authentication
smtp.req.command == "AUTH"
```

#### Solved Problem 8 ⭐⭐

**Soal:** Jelaskan mengapa analisis email pada level jaringan (SMTP traffic) dapat memberikan informasi yang tidak tersedia dari analisis email server log biasa!

**Penyelesaian:**

*Step 1:* Informasi dari email server log biasanya terbatas pada metadata (sender, recipient, timestamp, subject, size) tanpa konten email.

*Step 2:* Analisis SMTP traffic pada PCAP memberikan informasi tambahan:

| Informasi | Server Log | SMTP Traffic (PCAP) |
|-----------|-----------|---------------------|
| Sender/Recipient | ✅ | ✅ |
| Timestamp | ✅ | ✅ |
| Subject | ✅ | ✅ |
| **Body email** | ❌ | ✅ |
| **Attachment content** | ❌ | ✅ (dapat di-extract) |
| **SMTP commands** | ❌ | ✅ |
| **Authentication details** | Partial | ✅ (lengkap) |
| **Encryption negotiation** | ❌ | ✅ (STARTTLS) |
| **Client IP asli** | Via header | ✅ (dari paket) |

*Step 3:* Kasus khusus di mana SMTP traffic analysis kritis:
- Email dikirim melalui relay server yang tidak di-log
- Pelaku menggunakan SMTP server eksternal dari dalam jaringan
- Attachment mengandung data sensitif yang perlu di-extract sebagai bukti
- Diperlukan verifikasi apakah email benar-benar terkirim vs hanya tercatat di outbox

**Jawaban:** Analisis SMTP traffic pada PCAP memberikan akses ke konten lengkap email (body text dan attachment), detail autentikasi SMTP, negosiasi enkripsi, dan IP asli pengirim — informasi yang tidak tersedia dari server log biasa yang biasanya hanya mencatat metadata.

---

## 4. Analisis IDS/IPS dan Firewall Logs

### 4.1 Intrusion Detection System (IDS) Artifacts

> **IDS (Intrusion Detection System)** adalah sistem yang memonitor lalu lintas jaringan atau aktivitas sistem untuk mendeteksi aktivitas berbahaya atau pelanggaran kebijakan keamanan.

Jenis IDS berdasarkan pendekatan:

| Jenis | Metode Deteksi | Kelebihan | Kelemahan |
|-------|---------------|-----------|-----------|
| **Signature-based** | Mencocokkan pola yang diketahui | Akurat untuk serangan yang dikenal; rendah false positive | Tidak mendeteksi serangan baru (zero-day) |
| **Anomaly-based** | Mendeteksi penyimpangan dari baseline normal | Dapat mendeteksi serangan baru | Tinggi false positive; butuh training |
| **Hybrid** | Kombinasi signature + anomaly | Cakupan luas | Kompleksitas tinggi |

#### 4.1.1 Snort/Suricata Alert Analysis

Snort dan Suricata adalah IDS open-source yang banyak digunakan. Format alert standar Snort:

```
[**] [1:2024217:3] ET MALWARE Trickbot CnC Beacon [**]
[Classification: A Network Trojan was Detected] [Priority: 1]
01/15/2025-08:23:45.123456 192.168.1.100:49152 -> 203.0.113.50:443
TCP TTL:128 TOS:0x0 ID:12345 IpLen:20 DgmLen:287 DF
***AP*** Seq: 0xABCDEF12  Ack: 0x12345678  Win: 0xFAF0  TcpLen: 20
```

Interpretasi:
- **[1:2024217:3]**: Generator ID: Sid: Revision
- **ET MALWARE Trickbot CnC Beacon**: Deskripsi — deteksi komunikasi C2 malware Trickbot
- **[Priority: 1]**: Prioritas tertinggi (kritis)
- **192.168.1.100:49152 → 203.0.113.50:443**: Host internal berkomunikasi dengan C2 server

### 4.2 Firewall Log Analysis

Firewall log memberikan catatan tentang koneksi yang diizinkan dan ditolak. Contoh format log dari berbagai vendor:

**Contoh log iptables (Linux):**
```
Jan 15 08:23:45 firewall kernel: [DROP] IN=eth0 OUT= MAC=aa:bb:cc:dd:ee:ff SRC=203.0.113.50 DST=10.0.0.5 LEN=60 TOS=0x00 PREC=0x00 TTL=48 ID=12345 DF PROTO=TCP SPT=45231 DPT=22 WINDOW=65535 RES=0x00 SYN URGP=0
```

**Contoh log Palo Alto Networks:**
```
2025/01/15 08:23:45,TRAFFIC,drop,192.168.1.100,203.0.113.50,0.0.0.0,0.0.0.0,Deny-All,admin,,web-browsing,vsys1,trust,untrust,eth1/1,eth1/2,Log-Forwarding,2025/01/15 08:23:45,12345,1,49152,443,0,0,0x0,tcp,deny,256,256,0,1,2025/01/15 08:23:45,0,any,0,4567890,0x0,192.168.0.0-192.168.255.255,US
```

#### 4.2.1 Korelasi Log untuk Rekonstruksi Serangan

Teknik korelasi log dari berbagai sumber untuk mendapatkan gambaran lengkap serangan:

```
Timeline Rekonstruksi:
===================================
08:20:00 [DNS Log]    → 192.168.1.100 query: exploit-kit.malware.net → 203.0.113.50
08:20:01 [Firewall]   → ALLOW 192.168.1.100:49152 → 203.0.113.50:80 (HTTP)
08:20:02 [IDS Alert]  → ET EXPLOIT Kit Landing Page detected
08:20:05 [Proxy Log]  → 192.168.1.100 GET http://exploit-kit.malware.net/payload.exe 200 OK
08:20:10 [Firewall]   → ALLOW 192.168.1.100:49200 → 198.51.100.25:443 (HTTPS)
08:20:11 [IDS Alert]  → ET MALWARE CnC Beacon Activity Detected
08:20:15 [DNS Log]    → 192.168.1.100 query: c2.attacker.net → 198.51.100.25
08:21:00 [Firewall]   → ALLOW 192.168.1.100:49300 → 198.51.100.25:443 (data: 5.2MB)
08:21:30 [IDS Alert]  → ET POLICY Large Outbound Data Transfer
```

#### Solved Problem 9 ⭐⭐⭐

**Soal:** Berdasarkan timeline log di atas, rekonstruksi kronologi serangan dan identifikasi tahapan serangan berdasarkan Cyber Kill Chain!

**Penyelesaian:**

*Step 1:* Korelasikan event dalam timeline:

| Waktu | Event | Tahap Kill Chain |
|-------|-------|-----------------|
| 08:20:00 | DNS resolve exploit-kit.malware.net | **Delivery** — user mengakses website berbahaya |
| 08:20:01 | Koneksi HTTP ke server exploit kit | **Delivery** — koneksi ke exploit kit |
| 08:20:02 | IDS mendeteksi exploit kit landing page | **Exploitation** — exploit kit aktif |
| 08:20:05 | Download payload.exe berhasil | **Installation** — malware di-download |
| 08:20:10 | Koneksi HTTPS ke IP berbeda | **Command & Control** — malware berkomunikasi dengan C2 |
| 08:20:15 | DNS resolve c2.attacker.net | **C2** — resolve domain C2 server |
| 08:21:00 | Transfer data 5.2 MB ke C2 server | **Actions on Objectives** — data exfiltration |
| 08:21:30 | IDS alert data transfer besar | **Detection** — deteksi aktivitas |

*Step 2:* Narasi rekonstruksi:
1. Host 192.168.1.100 mengakses website yang mengarahkan ke exploit kit (kemungkinan melalui phishing link atau malvertising)
2. Exploit kit berhasil mengeksploitasi kerentanan pada browser/plugin
3. Malware (payload.exe) berhasil di-download dan dieksekusi
4. Dalam waktu 5 detik, malware membuka koneksi C2 ke server berbeda (198.51.100.25)
5. Dalam waktu 50 detik setelah instalasi, malware sudah melakukan exfiltration data 5.2 MB

**Jawaban:** Serangan mengikuti Cyber Kill Chain: Delivery (akses exploit kit via web), Exploitation (exploit kit aktif), Installation (download payload.exe), C2 (koneksi ke 198.51.100.25 via HTTPS), dan Actions on Objectives (exfiltration 5.2 MB data). Seluruh proses dari akses awal hingga exfiltration terjadi dalam waktu ~90 detik, menunjukkan serangan yang sangat terautomasi.

---

## 5. Wireless Network Forensics

### 5.1 Tantangan Forensik Jaringan Nirkabel

> **Wireless Network Forensics** adalah proses pengumpulan dan analisis bukti digital dari komunikasi jaringan nirkabel, termasuk Wi-Fi (IEEE 802.11), Bluetooth, dan protokol wireless lainnya.

Tantangan khusus dalam forensik wireless:

| Tantangan | Penjelasan |
|-----------|------------|
| **Broadcast medium** | Sinyal dapat ditangkap oleh siapa saja dalam jangkauan |
| **Ephemeral** | Data tidak tersimpan di media penyimpanan tetap |
| **Encryption** | WPA2/WPA3 mengenkripsi payload; tanpa kunci tidak dapat di-decrypt |
| **Interference** | Gangguan sinyal dapat menyebabkan packet loss |
| **Attribution** | MAC address dapat di-spoof; sulit mengidentifikasi perangkat asli |
| **Legal issues** | Intersepsi komunikasi wireless memerlukan otorisasi hukum |

### 5.2 Artefak Wi-Fi

Informasi yang dapat diekstrak dari capture wireless:

#### 5.2.1 Beacon Frames

```
IEEE 802.11 Beacon Frame:
  SSID: "KODAM-WIFI-SECURE"
  BSSID: AA:BB:CC:DD:EE:FF
  Channel: 6
  Supported Rates: 6, 9, 12, 18, 24, 36, 48, 54 Mbps
  RSN Information: WPA2-Enterprise (AES-CCMP)
  Country: ID
  Timestamp: 08:23:45.123456
```

#### 5.2.2 Probe Requests

> **Probe Request** adalah frame yang dikirim oleh perangkat wireless untuk mencari jaringan yang pernah terhubung sebelumnya. Frame ini mengandung SSID jaringan yang dicari dan MAC address perangkat.

Implikasi forensik probe request:
- Mengungkap **riwayat koneksi** perangkat (jaringan apa saja yang pernah diakses)
- Dapat digunakan untuk **tracking** perangkat/individu
- **MAC address** mengidentifikasi perangkat (meskipun dapat di-spoof)

```
Contoh Probe Request yang ditemukan:
Device MAC: 11:22:33:44:55:66
Probe for SSID: "HOTEL-WIFI-FREE"
Probe for SSID: "AIRPORT-GUEST"  
Probe for SSID: "KODAM-INTERNAL"
Probe for SSID: "HOME-NETWORK-Admin"
```

Dari contoh di atas, dapat disimpulkan bahwa perangkat ini pernah terhubung ke Wi-Fi hotel, bandara, jaringan Kodam, dan jaringan rumah — memberikan informasi tentang pergerakan dan kebiasaan pemiliknya.

#### 5.2.3 Rogue Access Point Detection

Dalam konteks militer, deteksi rogue access point sangat kritis:

```
Hasil scanning Wi-Fi di area markas:
==============================================
SSID                  BSSID              CH  Signal  Encryption
KODAM-WIFI-SECURE     AA:BB:CC:DD:EE:FF   6  -45dBm  WPA2-Ent
KODAM-WIFI-SECURE     11:22:33:44:55:66   1  -60dBm  WPA2-PSK    ← ROGUE!
GUEST-NETWORK         AA:BB:CC:DD:EE:01   11 -50dBm  WPA2-PSK
FREE-WIFI-KODAM       99:88:77:66:55:44   6  -70dBm  Open        ← ROGUE!
```

Indikator rogue AP:
- SSID sama dengan jaringan resmi tetapi BSSID berbeda
- Tipe enkripsi berbeda (WPA2-PSK vs WPA2-Enterprise)
- AP dengan nama yang mirip tetapi tidak terregistrasi
- AP tanpa enkripsi (Open) di area keamanan

#### Solved Problem 10 ⭐⭐

**Soal:** Tim keamanan Lantamal V menemukan access point dengan SSID "LANTAMAL-WIFI-SECURE" yang tidak terdaftar dalam inventaris jaringan. Jelaskan langkah-langkah forensik untuk menginvestigasi rogue access point ini!

**Penyelesaian:**

*Step 1:* **Identifikasi dan Dokumentasi**
- Catat BSSID (MAC address AP), channel, signal strength, dan tipe enkripsi
- Gunakan tool seperti Airodump-ng atau inSSIDer untuk profiling AP
- Bandingkan BSSID dengan database perangkat jaringan resmi Lantamal

*Step 2:* **Lokalisasi Fisik**
- Gunakan signal strength dari beberapa titik untuk triangulasi lokasi AP
- Signal strength berbanding terbalik dengan jarak (semakin kuat = semakin dekat)
- Identifikasi lokasi fisik AP dalam area Lantamal

*Step 3:* **Capture Traffic**
- Lakukan packet capture pada channel yang digunakan rogue AP
- Identifikasi klien yang terhubung ke rogue AP (dari association frames)
- Analisis apakah ada data yang ditransmisikan melalui rogue AP

*Step 4:* **Analisis Rogue AP**
- Jika AP berhasil ditemukan, dokumentasikan perangkat keras (merek, model, serial number)
- Periksa konfigurasi: apakah men-forward traffic ke server tertentu (man-in-the-middle)
- Analisis apakah AP dikonfigurasi untuk menangkap kredensial (evil twin attack)

*Step 5:* **Forensik Klien**
- Identifikasi semua perangkat yang pernah terhubung ke rogue AP
- Periksa apakah kredensial atau data sensitif telah dikompromikan
- Lakukan forensik pada perangkat yang terhubung jika diperlukan

**Jawaban:** Investigasi rogue AP mencakup lima langkah: identifikasi dan dokumentasi profil AP, lokalisasi fisik melalui triangulasi sinyal, capture traffic pada channel rogue AP, analisis konfigurasi AP untuk mendeteksi man-in-the-middle atau evil twin, dan forensik pada semua klien yang pernah terhubung untuk menilai dampak keamanan.

---

## 6. Rekonstruksi Serangan Berbasis Jaringan

### 6.1 Network-Based Attack Reconstruction

Rekonstruksi serangan berbasis jaringan melibatkan penyusunan kronologi lengkap serangan dari bukti jaringan yang dikumpulkan. Proses ini membutuhkan korelasi data dari berbagai sumber.

#### 6.1.1 Metodologi Rekonstruksi

```
Metodologi Rekonstruksi Serangan Jaringan:
==========================================
1. Collection   → Kumpulkan semua PCAP, logs, flow data
2. Filtering    → Filter noise; fokus pada IP/port/timeframe relevan
3. Correlation  → Korelasikan event dari berbagai sumber
4. Timeline     → Susun timeline kronologis
5. Analysis     → Identifikasi teknik dan taktik penyerang
6. Attribution  → Kaitkan dengan aktor ancaman yang dikenal
7. Reporting    → Dokumentasi temuan dan rekomendasi
```

#### 6.1.2 Wireshark untuk Rekonstruksi

Teknik-teknik penting dalam Wireshark untuk rekonstruksi:

| Teknik | Menu/Filter | Kegunaan |
|--------|-------------|----------|
| **Follow TCP Stream** | Klik kanan → Follow → TCP Stream | Melihat keseluruhan komunikasi satu sesi |
| **Export Objects** | File → Export Objects → HTTP/SMB/FTP | Mengekstrak file yang ditransfer |
| **Conversations** | Statistics → Conversations | Melihat ringkasan semua komunikasi |
| **IO Graph** | Statistics → I/O Graph | Visualisasi volume lalu lintas dari waktu ke waktu |
| **Protocol Hierarchy** | Statistics → Protocol Hierarchy | Distribusi protokol dalam capture |
| **Expert Information** | Analyze → Expert Information | Warning dan error yang terdeteksi |

### 6.2 Mendeteksi Serangan Umum dalam PCAP

#### 6.2.1 Port Scanning

Filter untuk mendeteksi port scanning:

```
# SYN Scan — banyak SYN dari satu IP ke banyak port
tcp.flags.syn == 1 && tcp.flags.ack == 0

# Statistik endpoint: Statistics → Endpoints → TCP
# Cari source IP dengan jumlah destination port sangat tinggi
```

#### 6.2.2 Brute Force Attack

```
# SSH Brute Force — banyak koneksi ke port 22
tcp.dstport == 22 && tcp.flags.syn == 1

# HTTP Login Brute Force — banyak POST ke halaman login
http.request.method == "POST" && http.request.uri contains "login"

# Failed authentication patterns
# Banyak koneksi dengan durasi sangat singkat ke port yang sama
```

#### 6.2.3 Data Exfiltration

```
# Transfer data besar ke luar jaringan
# Gunakan Statistics → Conversations → TCP
# Cari koneksi dengan bytes transferred sangat tinggi ke IP eksternal

# DNS Exfiltration
dns.qry.name.len > 50

# ICMP Tunneling
icmp.type == 8 && data.len > 64
```

#### 6.2.4 Command and Control (C2) Communication

```
# Beaconing pattern — koneksi periodik ke IP yang sama
# Analisis interval koneksi menggunakan:
# Statistics → Flow Graph

# HTTP C2 dengan User-Agent tidak lazim
http.user_agent contains "bot" || http.user_agent contains "curl" || http.user_agent == ""

# DNS C2 — query berulang ke satu domain dengan interval reguler
dns.qry.name contains "c2domain.net"
```

#### Solved Problem 11 ⭐⭐⭐

**Soal:** Anda diberikan file PCAP dari jaringan Mabes TNI dan diminta mengidentifikasi apakah terdapat aktivitas Command and Control (C2). Jelaskan langkah-langkah analisis yang akan Anda lakukan menggunakan Wireshark!

**Penyelesaian:**

*Step 1:* **Identifikasi Koneksi Anomali**
- Buka PCAP di Wireshark
- Buka Statistics → Conversations → TCP
- Urutkan berdasarkan jumlah paket (Packets) secara descending
- Identifikasi koneksi dengan pola: volume rendah tetapi jumlah sesi sangat banyak ke IP yang sama

*Step 2:* **Analisis Beaconing Pattern**
- Buka Statistics → Flow Graph
- Cari pola koneksi periodik (misalnya setiap 60 detik, 300 detik, atau 3600 detik)
- C2 biasanya berkomunikasi dengan interval yang relatif konsisten (±5% variasi)
- Filter: `ip.dst == [suspicious_IP]` lalu analisis timestamp antar paket

*Step 3:* **Analisis DNS untuk C2 Domain**
- Filter: `dns` lalu analisis domain yang di-query
- Cari domain dengan pola mencurigakan: DGA (domain generation algorithm), domain baru, TLD tidak lazim
- Periksa frekuensi query: query berulang ke satu domain dengan interval reguler = indikasi C2

*Step 4:* **Analisis HTTP/HTTPS C2**
- Filter: `http.request` untuk melihat semua HTTP request
- Cari User-Agent yang tidak lazim atau kosong
- Cari URL pattern yang konsisten (misalnya `/check-in`, `/beacon`, `/gate.php`)
- Untuk HTTPS, analisis TLS SNI: `tls.handshake.extensions_server_name`

*Step 5:* **Analisis Payload**
- Follow TCP Stream pada koneksi mencurigakan
- C2 sering menggunakan encoding (Base64, XOR) — cari pola data yang tidak natural
- Periksa ukuran response: C2 beacon biasanya memiliki response kecil dan konsisten

*Step 6:* **Korelasi dan Validasi**
- Cross-reference IP/domain mencurigakan dengan threat intelligence (VirusTotal, AbuseIPDB)
- Validasi temuan dengan log IDS/IPS dan firewall
- Buat timeline lengkap aktivitas C2

**Jawaban:** Analisis C2 dalam PCAP dilakukan melalui enam langkah: identifikasi koneksi anomali (banyak sesi ke satu IP), analisis beaconing pattern (interval koneksi periodik), analisis DNS untuk C2 domain (query berulang ke domain mencurigakan), analisis HTTP/HTTPS (User-Agent dan URL pattern), analisis payload (encoding mencurigakan), dan korelasi dengan threat intelligence.

---

## 7. Covert Channels dan Data Exfiltration Detection

### 7.1 Covert Channels

> **Covert Channel** adalah metode komunikasi yang menggunakan saluran atau protokol yang tidak dimaksudkan untuk transfer data, dengan tujuan menyembunyikan komunikasi dari deteksi keamanan.

Jenis-jenis covert channel yang umum:

| Metode | Protokol | Teknik | Deteksi |
|--------|----------|--------|---------|
| **DNS Tunneling** | DNS | Data encoded dalam subdomain query | Query panjang tidak wajar; volume tinggi ke satu domain |
| **ICMP Tunneling** | ICMP | Data dalam payload ping | ICMP payload besar (>64 bytes); volume tinggi |
| **HTTP Tunneling** | HTTP | Data dalam header kustom atau URL parameter | Header tidak standar; URL panjang tidak wajar |
| **Steganography** | Varies | Data tersembunyi dalam gambar/file | Deteksi statistik; file size anomaly |
| **Timing Channel** | Any | Informasi encoded dalam interval antar paket | Analisis pola timing |

### 7.2 Teknik Deteksi Data Exfiltration

#### 7.2.1 Volume-Based Detection

```
# Wireshark: Identifikasi transfer data besar ke luar
# Statistics → Conversations → TCP
# Sort by Bytes (B→A untuk outbound)
# Threshold alert: > 10 MB ke satu IP eksternal dalam satu sesi

# Filter koneksi dengan data besar
tcp.len > 1400 && ip.dst != 10.0.0.0/8 && ip.dst != 172.16.0.0/12 && ip.dst != 192.168.0.0/16
```

#### 7.2.2 Protocol-Based Detection

```
# ICMP dengan payload besar (potential tunneling)
icmp && data.len > 64

# DNS dengan query panjang (potential tunneling)
dns.qry.name.len > 50

# HTTP POST dengan body besar ke domain external
http.request.method == "POST" && http.content_length > 1000000
```

#### Solved Problem 12 ⭐⭐⭐

**Soal:** Dalam investigasi jaringan Kodam, Anda menemukan bahwa host 192.168.1.50 mengirim rata-rata 200 ICMP echo request per menit ke IP 203.0.113.100, dengan payload rata-rata 512 bytes per paket. Analisis apakah ini merupakan ICMP tunneling dan estimasi volume data yang mungkin telah di-exfiltrate selama 24 jam!

**Penyelesaian:**

*Step 1:* Analisis apakah ini normal:
- ICMP echo request normal (ping): biasanya 1-4 paket per siklus, payload 32-64 bytes
- Temuan: **200 paket/menit** dengan **512 bytes payload** — sangat abnormal

| Parameter | Normal Ping | Temuan Ini |
|-----------|-------------|------------|
| Frekuensi | 1-4/siklus | 200/menit |
| Payload | 32-64 bytes | 512 bytes |
| Durasi | Beberapa detik | 24 jam terus menerus |
| Destination | Bervariasi | Satu IP tetap |

*Step 2:* Indikator ICMP tunneling:
- ✅ Volume sangat tinggi (200/menit)
- ✅ Payload jauh di atas normal (512 vs 32-64 bytes)
- ✅ Destination IP tunggal dan konsisten
- ✅ Berjalan terus-menerus selama 24 jam

*Step 3:* Estimasi volume data exfiltration:
- Data per paket: 512 bytes
- Paket per menit: 200
- **Data per menit**: 512 × 200 = 102,400 bytes = 100 KB
- **Data per jam**: 100 KB × 60 = 6,000 KB ≈ 5.86 MB
- **Data per 24 jam**: 5.86 MB × 24 ≈ **140.6 MB**

*Step 4:* Kesimpulan — ini hampir pasti ICMP tunneling yang digunakan untuk data exfiltration. Volume 140 MB dalam 24 jam cukup untuk mengekstrak sejumlah besar dokumen, database, atau file konfigurasi.

**Jawaban:** Ini sangat kuat merupakan ICMP tunneling. Frekuensi 200 paket/menit dengan payload 512 bytes sangat jauh dari pola ICMP normal. Estimasi data yang di-exfiltrate selama 24 jam adalah ~140.6 MB (512 bytes × 200 paket/menit × 60 menit × 24 jam). Volume ini cukup signifikan untuk mentransfer dokumen rahasia dalam jumlah besar.

---

## 8. Encrypted Traffic Analysis

### 8.1 Tantangan Analisis Traffic Terenkripsi

Dengan meningkatnya penggunaan enkripsi (HTTPS, TLS, SSH), investigator forensik menghadapi tantangan signifikan dalam menganalisis konten komunikasi. Namun, metadata yang tidak terenkripsi tetap memberikan informasi berharga.

### 8.2 Informasi yang Tersedia dari Encrypted Traffic

#### 8.2.1 TLS Handshake Analysis

```
Client Hello:
  TLS Version: 1.3
  SNI: mail.protonmail.com
  Cipher Suites: TLS_AES_256_GCM_SHA384, TLS_CHACHA20_POLY1305_SHA256
  Extensions: supported_versions, key_share, psk_key_exchange_modes
  
Server Hello:
  TLS Version: 1.3
  Selected Cipher: TLS_AES_256_GCM_SHA384
  
Certificate:
  Subject: CN=mail.protonmail.com
  Issuer: Let's Encrypt Authority X3
  Valid From: 2025-01-01
  Valid To: 2025-04-01
  Serial: 03:AB:CD:EF:12:34:56:78
```

Informasi forensik dari TLS handshake:

| Elemen | Informasi |
|--------|-----------|
| **SNI** | Domain yang diakses (kecuali pada TLS 1.3 dengan ECH) |
| **Certificate** | Identitas server; certificate self-signed = mencurigakan |
| **Cipher suite** | Level keamanan enkripsi yang digunakan |
| **JA3/JA3S hash** | Fingerprint klien/server TLS — dapat mengidentifikasi malware |

#### 8.2.2 JA3 Fingerprinting

> **JA3** adalah metode fingerprinting koneksi TLS berdasarkan parameter Client Hello (TLS version, cipher suites, extensions, elliptic curves, dan elliptic curve point formats), menghasilkan hash MD5 yang unik untuk setiap kombinasi.

```
JA3 Hash Examples:
Malware Trickbot:  e7d705a3286e19ea42f587b344ee6865
Chrome Browser:    b32309a26951912be7dba376398abc3b
Firefox Browser:   839bbe3ed07fed922ded5aaf714d6842
curl tool:         456523fc94726331a4d5a2e1d40b2cd7
```

JA3 hash dapat digunakan untuk:
- Mengidentifikasi malware yang menggunakan library TLS tertentu
- Mendeteksi tool otomatis yang menyamar sebagai browser
- Membedakan tipe klien berdasarkan implementasi TLS-nya

#### Solved Problem 13 ⭐⭐

**Soal:** Dalam analisis PCAP, semua koneksi ke server C2 menggunakan HTTPS sehingga payload terenkripsi. Jelaskan informasi apa saja yang masih dapat Anda kumpulkan tanpa mendekripsi traffic!

**Penyelesaian:**

*Step 1:* Informasi dari metadata jaringan (tanpa dekripsi):

| Sumber | Informasi | Kegunaan |
|--------|-----------|----------|
| **IP Header** | IP sumber/tujuan | Identifikasi host terlibat dan server C2 |
| **TCP Header** | Port, timestamps, flags | Pola komunikasi, waktu, durasi |
| **TLS SNI** | Domain tujuan | Identifikasi domain C2 |
| **TLS Certificate** | CN, issuer, validity | Validasi identitas server; self-signed = C2 |
| **JA3 Hash** | TLS fingerprint klien | Identifikasi malware family |
| **Packet Size** | Bytes per paket | Pola transfer data |
| **Timing** | Inter-arrival time | Deteksi beaconing |
| **Volume** | Total bytes transferred | Estimasi data exfiltration |
| **DNS** | Domain resolution | Domain C2 dan infrastructure |

*Step 2:* Teknik analisis tambahan:
- **Traffic Analysis**: Pola timing dan volume dapat mengungkap beaconing meskipun payload terenkripsi
- **Statistical Analysis**: Distribusi ukuran paket yang tidak natural mengindikasikan data exfiltration
- **Certificate Analysis**: C2 server sering menggunakan self-signed certificate atau certificate dari free CA
- **Correlation**: Korelasi timing koneksi HTTPS dengan event di host (dari log endpoint)

**Jawaban:** Tanpa dekripsi, informasi yang masih tersedia meliputi: IP sumber/tujuan, port, TLS SNI (domain), certificate details, JA3 fingerprint, ukuran paket, pola timing (beaconing), volume transfer, dan DNS queries. Informasi ini cukup untuk mengidentifikasi C2 server, mendeteksi beaconing, estimasi volume exfiltration, dan bahkan mengidentifikasi malware family melalui JA3 hash.

---

## 9. Pertimbangan Hukum dalam Network Monitoring

### 9.1 Kerangka Hukum Indonesia

Monitoring jaringan dan intersepsi komunikasi di Indonesia diatur oleh beberapa regulasi:

| Regulasi | Ketentuan Relevan |
|----------|-------------------|
| **UU No. 1/2024 (ITE)** | Intersepsi harus berdasarkan perintah pengadilan atau penegak hukum |
| **PP No. 71/2019** | Penyelenggaraan sistem elektronik; kewajiban logging |
| **UU No. 3/2002 (Pertahanan Negara)** | Wewenang pertahanan dalam pengamanan informasi |
| **Perpres No. 82/2022 (BSSN)** | Koordinasi keamanan siber nasional |

### 9.2 Prinsip Legal Network Monitoring dalam Konteks Militer

Dalam konteks militer Indonesia, monitoring jaringan diperbolehkan dengan memperhatikan:

1. **Otorisasi Komandan**: Monitoring pada jaringan militer harus diotorisasi oleh komandan yang berwenang
2. **Proporsionalitas**: Cakupan monitoring harus proporsional dengan ancaman
3. **Notifikasi**: Pengguna jaringan militer harus diinformasikan bahwa jaringan dimonitor (biasanya melalui banner login)
4. **Penyimpanan**: Data capture harus disimpan sesuai ketentuan klasifikasi
5. **Akses**: Akses terhadap data capture hanya untuk personel yang berwenang

```
Contoh Banner Login pada Jaringan Militer:
===========================================
╔══════════════════════════════════════════════════════╗
║ PERHATIAN: Ini adalah sistem informasi milik         ║
║ Tentara Nasional Indonesia (TNI).                    ║
║                                                      ║
║ Dengan mengakses sistem ini, Anda menyetujui bahwa:  ║
║ • Aktivitas Anda dapat dipantau dan direkam          ║
║ • Tidak ada harapan privasi pada sistem ini          ║
║ • Penggunaan tidak sah akan ditindak sesuai hukum    ║
║                                                      ║
║ Akses tidak sah merupakan pelanggaran hukum.         ║
╚══════════════════════════════════════════════════════╝
```

#### Solved Problem 14 ⭐⭐

**Soal:** Seorang investigator forensik diminta melakukan full packet capture pada jaringan Kodam selama satu minggu. Jelaskan pertimbangan hukum dan etika yang harus diperhatikan!

**Penyelesaian:**

*Step 1:* Pertimbangan hukum:
- **Otorisasi**: Dapatkan otorisasi tertulis dari Komandan Kodam atau Asisten Intelijen
- **Dasar hukum**: Pastikan monitoring didasarkan pada regulasi yang berlaku (UU Pertahanan, SOP internal TNI)
- **Scope**: Definisikan dengan jelas segmen jaringan yang dicapture (operasional vs administratif)
- **Durasi**: Dokumentasikan periode capture yang diotorisasi

*Step 2:* Pertimbangan etika:
- **Proporsionalitas**: Hanya capture segmen yang relevan dengan investigasi; hindari blanket surveillance
- **Minimisasi data**: Jika memungkinkan, gunakan filter untuk mengurangi data yang ditangkap
- **Privasi**: Meskipun jaringan militer memiliki ekspektasi privasi yang lebih rendah, komunikasi personal (jika ada) tetap harus diperlakukan dengan sensitif
- **Transparansi**: Pastikan banner login sudah terpasang yang menginformasikan monitoring

*Step 3:* Prosedural:
- Dokumentasikan chain of custody dari awal capture hingga penyimpanan
- Enkripsi semua data capture sesuai tingkat klasifikasi
- Batasi akses hanya untuk investigator yang ditunjuk
- Tentukan retention policy: berapa lama data disimpan dan kapan dihapus

**Jawaban:** Full packet capture pada jaringan Kodam memerlukan otorisasi tertulis dari Komandan, dasar hukum yang jelas, scope dan durasi yang terdefinisi, banner notification pada jaringan, prinsip proporsionalitas dan minimisasi data, enkripsi data sesuai klasifikasi, chain of custody yang ketat, dan retention policy yang jelas.

---

## 10. Tools Forensik Jaringan

### 10.1 Wireshark

> **Wireshark** adalah tool analisis protokol jaringan open-source paling populer yang memungkinkan capture dan analisis interaktif terhadap lalu lintas jaringan secara mendalam.

Fitur utama Wireshark untuk forensik:

| Fitur | Kegunaan Forensik |
|-------|-------------------|
| **Display Filters** | Menyaring paket berdasarkan berbagai kriteria |
| **Follow Stream** | Merekonstruksi sesi komunikasi lengkap |
| **Export Objects** | Mengekstrak file yang ditransfer (HTTP, SMB, FTP, dll.) |
| **Statistics** | Ringkasan percakapan, protokol, dan endpoint |
| **Expert Info** | Mendeteksi anomali dan error dalam traffic |
| **Coloring Rules** | Visualisasi tipe traffic berdasarkan warna |
| **Profile** | Konfigurasi custom untuk berbagai jenis analisis |

### 10.2 tcpdump dan TShark

```bash
# tcpdump - capture paket dari command line
# Capture semua traffic pada interface eth0, simpan ke file
tcpdump -i eth0 -w capture.pcap

# Capture hanya traffic HTTP
tcpdump -i eth0 port 80 -w http_traffic.pcap

# Capture traffic dari/ke IP tertentu
tcpdump -i eth0 host 192.168.1.100 -w specific_host.pcap

# TShark - command line version Wireshark
# Membaca PCAP dan menampilkan summary
tshark -r capture.pcap

# Extract semua HTTP requests
tshark -r capture.pcap -Y "http.request" -T fields -e ip.src -e http.host -e http.request.uri

# Extract DNS queries
tshark -r capture.pcap -Y "dns.qry.name" -T fields -e ip.src -e dns.qry.name
```

### 10.3 NetworkMiner

> **NetworkMiner** adalah Network Forensic Analysis Tool (NFAT) open-source yang secara otomatis mengekstrak file, gambar, credential, dan informasi host dari file PCAP.

Keunggulan NetworkMiner:
- Ekstraksi file otomatis dari PCAP
- Identifikasi OS fingerprinting
- Credential harvesting dari traffic clear-text
- Visualisasi host dan komunikasi
- Tidak memerlukan konfigurasi kompleks

### 10.4 Zeek (formerly Bro)

```bash
# Zeek - network analysis framework
# Analisis PCAP file
zeek -r capture.pcap

# Menghasilkan beberapa log file:
# conn.log     - semua koneksi
# dns.log      - query DNS
# http.log     - request HTTP
# ssl.log      - koneksi SSL/TLS
# files.log    - file yang terdeteksi
# notice.log   - alert dan anomali

# Contoh conn.log
# ts         uid          orig_h        orig_p  resp_h        resp_p  proto
1705299825  CYhJZW4      192.168.1.100 49152   203.0.113.50  443     tcp
```

### 10.5 Perbandingan Tools

| Tool | Tipe | Kelebihan | Keterbatasan |
|------|------|-----------|-------------|
| **Wireshark** | GUI Analyzer | Visual, interaktif, fitur lengkap | Lambat untuk file besar (>1 GB) |
| **tcpdump** | CLI Capture | Ringan, cepat, tersedia di semua Linux | Analisis terbatas |
| **TShark** | CLI Analyzer | Automatable, scriptable | Butuh command line expertise |
| **NetworkMiner** | NFAT | Ekstraksi otomatis, mudah digunakan | Fitur analisis kurang mendalam |
| **Zeek** | NSM Framework | Analisis mendalam, log terstruktur | Learning curve tinggi |
| **Nmap** | Scanner | Port scanning, service detection | Bukan tool analisis forensik |

#### Solved Problem 15 ⭐

**Soal:** Sebutkan kombinasi tools yang paling efektif untuk investigasi forensik jaringan pada kasus data exfiltration di pangkalan militer!

**Penyelesaian:**

*Step 1:* Identifikasi kebutuhan berdasarkan tahapan investigasi:

| Tahap | Tool | Alasan |
|-------|------|--------|
| **Capture** | tcpdump | Ringan, cepat, tidak membebani sistem |
| **Quick Triage** | NetworkMiner | Ekstraksi otomatis file dan kredensial dari PCAP |
| **Deep Analysis** | Wireshark | Analisis mendalam per paket; Follow Stream; Expert Info |
| **Automated Processing** | Zeek | Log terstruktur untuk korelasi; conn.log dan dns.log |
| **Scripted Analysis** | TShark | Otomasi ekstraksi field tertentu untuk batch processing |
| **File Analysis** | Wireshark Export + hashing tools | Verifikasi file yang diekstrak dari network capture |

*Step 2:* Workflow yang direkomendasikan:

```
tcpdump (capture) → NetworkMiner (quick triage) → Zeek (automated logs)
                                                         ↓
Wireshark (deep analysis) ← TShark (scripted extraction) ←
```

**Jawaban:** Kombinasi optimal adalah: tcpdump untuk capture (ringan dan cepat), NetworkMiner untuk quick triage (ekstraksi otomatis), Zeek untuk automated processing (log terstruktur), Wireshark untuk deep analysis (analisis mendalam), dan TShark untuk scripted extraction. Workflow dimulai dari capture → quick triage → automated processing → deep analysis.

---

## 11. Studi Kasus: Investigasi Forensik Jaringan pada Insiden Militer

### 11.1 Skenario: Deteksi Komunikasi C2 di Jaringan Kodam

**Latar Belakang:** Tim Satsiber TNI mendeteksi anomali pada jaringan Kodam XII/Tanjungpura. IDS mencatat alert berkala dari satu workstation, dan monitoring bandwidth menunjukkan peningkatan outbound traffic di luar jam kerja.

**Kronologi Investigasi:**

*Fase 1: Alert Review*
```
[IDS Alert] 2025-01-15 02:00:03 ET MALWARE Possible CnC Activity
Source: 192.168.10.45  Destination: 185.100.87.XXX:443
[IDS Alert] 2025-01-15 02:05:02 ET MALWARE Possible CnC Activity  
Source: 192.168.10.45  Destination: 185.100.87.XXX:443
[IDS Alert] 2025-01-15 02:10:04 ET MALWARE Possible CnC Activity
Source: 192.168.10.45  Destination: 185.100.87.XXX:443
```

Pola: Alert setiap ~5 menit pada jam 02:00 dini hari → **beaconing pattern**.

*Fase 2: PCAP Analysis*

Investigator mengumpulkan PCAP dari TAP pada gateway dan menganalisis:

```
Wireshark Filter: ip.src == 192.168.10.45 && ip.dst == 185.100.87.XXX

Temuan:
- 288 koneksi HTTPS dalam 24 jam (interval ~5 menit)
- TLS SNI: update-service.cloud-sync[.]net
- JA3 Hash: e7d705a3286e19ea42f587b344ee6865 (match Cobalt Strike)
- Certificate: Self-signed, CN=localhost
- Data transfer: avg 2 KB per request, dengan spike 45 MB pada 02:30
```

*Fase 3: DNS Analysis*

```
DNS Query Log:
192.168.10.45 → update-service.cloud-sync.net (A record) → 185.100.87.XXX
192.168.10.45 → check.cloud-sync.net (TXT record) → [encoded response]
```

*Fase 4: Korelasi*

Korelasi dengan log DHCP dan Active Directory:
- 192.168.10.45 → MAC: AA:BB:CC:11:22:33 → Hostname: WS-INTEL-007
- User terakhir login: kapten.xyz (dari AD log)
- Workstation berada di unit Intelijen Kodam

*Fase 5: Temuan dan Rekomendasi*

| Temuan | Detail |
|--------|--------|
| **Malware** | Cobalt Strike beacon (berdasarkan JA3 hash) |
| **C2 Server** | 185.100.87.XXX (cloud-sync[.]net) |
| **Beaconing** | Interval 5 menit via HTTPS |
| **Exfiltration** | ~45 MB data pada jam 02:30 |
| **Host Terdampak** | WS-INTEL-007 (unit Intelijen) |
| **Durasi Kompromis** | Minimal 24 jam (kemungkinan lebih lama) |

#### Solved Problem 16 ⭐⭐⭐

**Soal:** Berdasarkan studi kasus di atas, mengapa self-signed certificate pada C2 server merupakan indikator penting, dan bagaimana cara mendeteksinya secara otomatis dalam volume PCAP yang besar?

**Penyelesaian:**

*Step 1:* Mengapa self-signed certificate penting:
- Server legitimate biasanya menggunakan certificate dari CA terpercaya (DigiCert, Let's Encrypt, dll.)
- C2 server sering menggunakan self-signed certificate karena penyerang tidak memiliki domain yang terverifikasi oleh CA
- Self-signed certificate memiliki issuer yang sama dengan subject (CN=localhost atau CN mirip)
- Ini bukan bukti konklusif (beberapa server internal juga menggunakan self-signed), tetapi merupakan **anomali yang signifikan** untuk server eksternal

*Step 2:* Deteksi otomatis menggunakan TShark:
```bash
# Ekstrak certificate yang issuer == subject (self-signed)
tshark -r capture.pcap -Y "tls.handshake.certificate" \
  -T fields -e ip.dst -e tls.handshake.certificate \
  | grep -i "self-signed"

# Menggunakan Zeek untuk analisis certificate:
# File ssl.log otomatis mencatat validation_status
cat ssl.log | awk '$NF ~ /self signed/' | cut -f3,5,10
```

*Step 3:* Pendekatan enterprise:
- Implementasi TLS inspection pada firewall (untuk internal traffic)
- Whitelist certificate yang diizinkan pada jaringan militer
- Alert otomatis untuk koneksi ke server dengan self-signed certificate di luar jaringan internal

**Jawaban:** Self-signed certificate pada C2 server penting karena server legitimate umumnya menggunakan CA terpercaya, sedangkan C2 server sering menggunakan self-signed (issuer = subject). Deteksi otomatis dapat dilakukan menggunakan TShark untuk mengekstrak certificate info dari PCAP, Zeek ssl.log yang otomatis mencatat validation status, atau whitelist-based alerting pada firewall untuk koneksi ke server dengan self-signed certificate eksternal.

---

## Supplementary Problems


#### Solved Problem 17 ⭐

**Soal:** Apa perbedaan antara display filter dan capture filter di Wireshark? Berikan contoh masing-masing!

**Penyelesaian:**

| Aspek | Capture Filter (BPF) | Display Filter (Wireshark) |
|-------|----------------------|---------------------------|
| **Kapan** | Saat capture berlangsung | Setelah capture, untuk viewing |
| **Syntax** | Berkeley Packet Filter | Wireshark proprietary |
| **Performance** | Mengurangi data yang di-capture | Menyembunyikan paket di display |
| **Contoh HTTP** | `port 80` | `http` |
| **Contoh IP** | `host 192.168.1.100` | `ip.addr == 192.168.1.100` |
| **Contoh DNS** | `port 53` | `dns` |
| **Reversible** | ❌ Data yang tidak dicapture hilang | ✅ Dapat diubah kapan saja |

**Jawaban:** Capture filter (BPF syntax) diterapkan saat proses capture untuk membatasi paket yang direkam (irreversible), sedangkan display filter (Wireshark syntax) diterapkan setelah capture untuk menyaring tampilan (reversible). Untuk forensik, disarankan capture tanpa filter (untuk kelengkapan), lalu gunakan display filter untuk analisis.

#### Solved Problem 18 ⭐⭐

**Soal:** Jelaskan bagaimana Wireshark Export Objects dapat digunakan untuk mengekstrak file dari packet capture dan berikan langkah-langkahnya!

**Penyelesaian:**

*Step 1:* Buka PCAP file di Wireshark

*Step 2:* Navigasi ke File → Export Objects → pilih protokol:
- **HTTP**: Untuk file yang ditransfer via web (HTML, gambar, PDF, executable)
- **SMB**: Untuk file yang ditransfer via Windows file sharing
- **FTP-DATA**: Untuk file yang ditransfer via FTP
- **TFTP**: Untuk file yang ditransfer via TFTP
- **IMF**: Untuk email dan attachment (Internet Message Format)

*Step 3:* Pada dialog Export Objects:
- **Hostname**: Domain server sumber
- **Content Type**: MIME type file
- **Size**: Ukuran file
- **Filename**: Nama file asli

*Step 4:* Pilih file yang ingin diekstrak → Save atau Save All

*Step 5:* Verifikasi integritas file yang diekstrak:
```bash
# Hitung hash file yang diekstrak
md5sum extracted_file.pdf
sha256sum extracted_file.pdf
```

**Jawaban:** Export Objects di Wireshark (File → Export Objects → HTTP/SMB/FTP) memungkinkan ekstraksi otomatis file yang ditransfer melalui jaringan. Dialog menampilkan hostname, content type, size, dan filename. File yang diekstrak harus diverifikasi integritasnya menggunakan hash.

#### Solved Problem 19 ⭐⭐

**Soal:** Dalam konteks jaringan militer, mengapa flow data (NetFlow) sering lebih praktis daripada full packet capture untuk monitoring jangka panjang?

**Penyelesaian:**

| Aspek | Full PCAP | NetFlow |
|-------|-----------|---------|
| **Storage** | ~10 TB/hari (1 Gbps) | ~20 GB/hari (1 Gbps) → rasio 500:1 |
| **Privacy** | Merekam konten lengkap | Hanya metadata (tanpa payload) |
| **Legal** | Mungkin memerlukan izin khusus | Biasanya tercakup dalam monitoring standar |
| **Processing** | Membutuhkan analisis intensif | Mudah di-query dan di-aggregate |
| **Retention** | Terbatas (hari-minggu) | Dapat disimpan berbulan-bulan |
| **Real-time** | Sulit pada kecepatan tinggi | Mudah dianalisis secara real-time |

Untuk jaringan militer:
- Flow data cukup untuk **baseline monitoring** dan **anomaly detection**
- Full PCAP hanya diaktifkan **on-demand** ketika anomali terdeteksi dari flow data
- Pendekatan hybrid ini meminimalkan kebutuhan storage sambil mempertahankan kapabilitas investigasi

**Jawaban:** NetFlow lebih praktis untuk monitoring jangka panjang karena rasio storage 500:1 dibanding PCAP, tidak merekam konten (mengurangi masalah privasi), mudah dianalisis secara real-time, dan dapat disimpan selama berbulan-bulan. Pendekatan optimal adalah monitoring flow data secara kontinu dengan full PCAP yang diaktifkan on-demand saat anomali terdeteksi.

#### Solved Problem 20 ⭐⭐⭐

**Soal:** Bagaimana cara membedakan DDoS attack dari legitimate traffic spike dalam PCAP? Berikan filter Wireshark yang relevan!

**Penyelesaian:**

*Step 1:* Indikator DDoS vs legitimate spike:

| Indikator | DDoS Attack | Legitimate Spike |
|-----------|-------------|-----------------|
| **Source IPs** | Banyak IP berbeda (distributed) | Bervariasi, dari IP yang dikenal |
| **Request pattern** | Identik/similar requests | Request bervariasi dan natural |
| **Geographic distribution** | Dari banyak negara secara bersamaan | Sesuai demografi user |
| **Protocol** | Sering single protocol | Mix protocols |
| **TCP behavior** | Banyak incomplete handshake | Normal handshake |
| **Timing** | Mendadak, tanpa gradual increase | Biasanya gradual |

*Step 2:* Filter Wireshark untuk identifikasi DDoS:

```
# SYN Flood - banyak SYN tanpa ACK
tcp.flags.syn == 1 && tcp.flags.ack == 0

# Statistik unique source IP
# Statistics → Endpoints → IPv4 → Sort by Packets

# UDP Flood
udp && !dns && !dhcp

# HTTP Flood
http.request && (http.request.method == "GET" || http.request.method == "POST")
# Kemudian: Statistics → HTTP → Requests → Sort by Count

# ICMP Flood
icmp.type == 8

# DNS Amplification
dns.flags.response == 1 && dns.resp.len > 512
```

*Step 3:* Teknik konfirmasi:
```
# IO Graph untuk melihat traffic spike
# Statistics → IO Graph → Filter: tcp.flags.syn == 1

# Conversation analysis
# Statistics → Conversations → IPv4
# Cari: banyak source IP dengan sedikit paket masing-masing = DDoS
#        sedikit source IP dengan banyak paket = legitimate atau DoS
```

**Jawaban:** DDoS dapat dibedakan dari legitimate spike melalui: analisis distribusi source IP (banyak IP dengan sedikit paket = DDoS), pola request yang identik, banyak incomplete TCP handshake, dan peningkatan mendadak tanpa gradual. Filter Wireshark meliputi SYN flood detection (`tcp.flags.syn == 1 && tcp.flags.ack == 0`), endpoint statistics, dan IO Graph untuk visualisasi spike.

#### Solved Problem 21 ⭐⭐

**Soal:** Seorang investigator menemukan file PCAP berukuran 50 GB dari jaringan Lantamal. Jelaskan strategi yang efisien untuk menganalisis file sebesar ini!

**Penyelesaian:**

*Step 1:* Jangan buka langsung di Wireshark — file 50 GB akan sangat lambat dan memerlukan RAM besar.

*Step 2:* Gunakan pendekatan berlapis:

| Langkah | Tool | Tujuan |
|---------|------|--------|
| 1. **Overview** | `capinfos capture.pcap` | Statistik dasar: jumlah paket, durasi, data rate |
| 2. **Automated Parsing** | Zeek: `zeek -r capture.pcap` | Generate log files terstruktur (conn, dns, http, ssl) |
| 3. **Flow Summary** | `tshark -r capture.pcap -qz conv,tcp` | Ringkasan semua konversi TCP |
| 4. **Filtering** | `tcpdump -r capture.pcap -w filtered.pcap [filter]` | Ekstrak subset berdasarkan kriteria |
| 5. **Split** | `editcap -c 100000 capture.pcap split_` | Pecah menjadi file-file kecil (100K paket) |
| 6. **Targeted Analysis** | Wireshark pada file filtered/split | Analisis mendalam pada subset relevan |

*Step 3:* Contoh workflow untuk investigasi data exfiltration:
```bash
# Step 1: Overview
capinfos capture.pcap

# Step 2: Generate Zeek logs
zeek -r capture.pcap

# Step 3: Identifikasi top talkers
cat conn.log | zeek-cut id.orig_h id.resp_h orig_bytes | sort -t$'\t' -k3 -rn | head -20

# Step 4: Filter koneksi besar ke IP eksternal
tcpdump -r capture.pcap 'dst net not (10.0.0.0/8 or 172.16.0.0/12 or 192.168.0.0/16) and greater 1000' -w exfil_candidates.pcap

# Step 5: Analisis di Wireshark
wireshark exfil_candidates.pcap
```

**Jawaban:** Untuk PCAP 50 GB, gunakan pendekatan berlapis: capinfos untuk overview, Zeek untuk automated parsing ke log terstruktur, tshark/tcpdump untuk filtering dan ekstraksi subset relevan, editcap untuk split file, dan baru kemudian Wireshark untuk analisis mendalam pada subset yang sudah difilter. Jangan pernah membuka file 50 GB langsung di Wireshark.

#### Solved Problem 22 ⭐⭐⭐

**Soal:** Tim Satsiber TNI mendeteksi bahwa sebuah workstation di kantor intelijen mengirim data ke server di luar negeri setiap malam pukul 02:00. Investigasi awal menunjukkan traffic terenkripsi (HTTPS). Rancang rencana investigasi forensik jaringan yang komprehensif!

**Penyelesaian:**

*Step 1:* **Fase Persiapan**
- Dapatkan otorisasi tertulis dari komandan yang berwenang
- Identifikasi dan verifikasi posisi sensor (TAP/SPAN) pada segmen yang relevan
- Siapkan storage untuk PCAP capture (estimasi: >100 GB untuk monitoring beberapa hari)
- Dokumentasikan baseline traffic normal pada jam kerja dan di luar jam kerja

*Step 2:* **Fase Collection (3-5 hari)**
```bash
# Full packet capture pada segmen yang relevan
tcpdump -i eth0 host [workstation_IP] -w investigation_%Y%m%d%H%M%S.pcap -G 3600 -Z root

# Paralel: Kumpulkan DNS logs, firewall logs, IDS alerts, dan proxy logs
# untuk periode yang sama
```

*Step 3:* **Fase Analysis**

a) **Traffic Pattern Analysis:**
- Identifikasi pola beaconing (interval koneksi) menggunakan Wireshark IO Graph
- Dokumentasikan destination IP, port, TLS SNI, dan JA3 hash
- Analisis volume data transfer per sesi

b) **TLS/Certificate Analysis:**
```bash
tshark -r capture.pcap -Y "tls.handshake.type == 2" \
  -T fields -e ip.dst -e tls.handshake.extensions_server_name \
  -e tls.handshake.certificate
```

c) **DNS Correlation:**
- Identifikasi domain yang di-resolve sebelum koneksi HTTPS
- Periksa domain registration (WHOIS), usia domain, hosting provider

d) **Threat Intelligence:**
- Cross-reference IP/domain dengan VirusTotal, AbuseIPDB, AlienVault OTX
- Cek JA3 hash terhadap database malware yang diketahui
- Identifikasi apakah certificate cocok dengan known C2 infrastructure

*Step 4:* **Fase Korelasi**
- Korelasi timeline: DNS query → TLS handshake → data transfer → connection close
- Korelasi dengan event log Windows pada workstation (jika tersedia)
- Identifikasi proses yang membuat koneksi (dari memory forensik jika dilakukan)

*Step 5:* **Fase Response dan Reporting**
- Buat timeline lengkap dengan semua evidence
- Dokumentasikan IOC (Indicators of Compromise): IP, domain, JA3, certificate hash
- Berikan rekomendasi: isolasi host, block IP/domain di firewall, forensik host

**Jawaban:** Rencana investigasi mencakup lima fase: Persiapan (otorisasi, sensor, storage), Collection (full PCAP + multi-source logs selama 3-5 hari), Analysis (traffic patterns, TLS/certificate analysis, DNS correlation, threat intelligence cross-reference), Korelasi (timeline construction dari semua sumber), dan Response (documentation, IOC extraction, remediation recommendations). Pendekatan ini memastikan investigasi menyeluruh meskipun traffic terenkripsi.

---

## Ringkasan


| Topik | Poin Kunci |
|-------|------------|
| **Prinsip Forensik Jaringan** | Catch-it-as-you-can vs stop-look-listen; data in motion vs data at rest |
| **Network Evidence** | Full PCAP, flow data, log files; masing-masing dengan kelebihan/kekurangan |
| **Protocol Analysis** | TCP flags, DNS forensics, HTTP analysis, SMTP analysis |
| **IDS/Firewall** | Alert analysis, log korelasi, timeline reconstruction |
| **Wireless Forensics** | Beacon frames, probe requests, rogue AP detection |
| **Attack Reconstruction** | Metodologi korelasi multi-sumber |
| **Covert Channels** | DNS tunneling, ICMP tunneling, detection techniques |
| **Encrypted Traffic** | TLS metadata, JA3 fingerprinting, certificate analysis |
| **Legal Considerations** | UU ITE, otorisasi monitoring, chain of custody |
| **Tools** | Wireshark, tcpdump, TShark, NetworkMiner, Zeek |

## Referensi

1. Davidoff, S. & Ham, J. (2022). *Network Forensics: Tracking Hackers through Cyberspace*. 2nd Edition. Prentice Hall. Chapter 1-8.
2. Sanders, C. (2023). *Practical Packet Analysis*. 4th Edition. No Starch Press. Chapter 1-8.
3. Casey, E. (2022). *Digital Evidence and Computer Crime*. 4th Edition. Academic Press. Chapter 11.
4. NIST SP 800-86: *Guide to Integrating Forensic Techniques into Incident Response*. Network Forensics Section.
5. Johansen, G. (2020). *Digital Forensics and Incident Response*. 2nd Edition. Packt. Chapter 8.
6. Bejtlich, R. (2013). *The Practice of Network Security Monitoring*. No Starch Press.
7. RFC 3227: *Guidelines for Evidence Collection and Archiving*.

---


## Glosarium

| Istilah | Definisi |
|---------|----------|
| **Beaconing** | Pola komunikasi periodik dari malware ke C2 server |
| **C2 (Command and Control)** | Server yang digunakan penyerang untuk mengendalikan malware |
| **Covert Channel** | Saluran komunikasi tersembunyi menggunakan protokol tidak semestinya |
| **DGA (Domain Generation Algorithm)** | Algoritma yang menghasilkan nama domain secara otomatis untuk C2 |
| **DNS Tunneling** | Teknik pengiriman data melalui protokol DNS |
| **Flow Data** | Ringkasan metadata komunikasi jaringan |
| **Full Packet Capture (PCAP)** | Perekaman lengkap semua paket jaringan |
| **JA3** | Metode fingerprinting koneksi TLS berdasarkan Client Hello |
| **Network TAP** | Perangkat keras untuk menyalin lalu lintas jaringan |
| **Rogue Access Point** | Access point tidak sah yang dipasang di jaringan |
| **SNI (Server Name Indication)** | Extension TLS yang mengindikasikan domain yang diakses |
| **SPAN Port** | Fitur switch untuk mengkopi traffic ke port monitoring |

---

## License / Lisensi

*Modul ini disusun sebagai bahan ajar Mata Kuliah Digital Forensic for Military Purposes, Program Studi Teknologi Informasi Pertahanan, Universitas Pertahanan RI.*

*Lisensi: Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)*
