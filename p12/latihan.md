# Latihan Pertemuan 12: Investigasi Serangan Web dan Teknik Anti-Forensik

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 12  
**Topik:** Investigasi Serangan Web dan Teknik Anti-Forensik  
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
Teknik serangan yang menyisipkan perintah SQL melalui input aplikasi web disebut:

A. Cross-Site Scripting  
B. SQL Injection  
C. Cross-Site Request Forgery  
D. Remote File Inclusion  
E. Directory Traversal

---

### Soal 2
Lokasi default log pada web server Apache di sistem Linux adalah:

A. `/var/log/nginx/`  
B. `/var/log/httpd/` atau `/var/log/apache2/`  
C. `C:\inetpub\logs\`  
D. `/etc/apache2/logs/`  
E. `/home/www/logs/`

---

### Soal 3
HTTP status code yang menunjukkan bahwa server berhasil memproses request penyerang adalah:

A. 301  
B. 403  
C. 404  
D. 200  
E. 500

---

### Soal 4
Pada entri log Apache, field yang menunjukkan aplikasi atau browser yang digunakan penyerang adalah:

A. Referer  
B. Status Code  
C. User-Agent  
D. Request URI  
E. Remote Host

---

### Soal 5
Jenis SQL injection yang mengandalkan banyak request berulang dengan variasi parameter untuk mendapatkan data satu bit per satu bit disebut:

A. In-Band SQLi  
B. UNION-based SQLi  
C. Error-based SQLi  
D. Blind SQLi  
E. Out-of-Band SQLi

---

### Soal 6
Jenis XSS yang payload-nya tersimpan di database server dan menyerang semua pengunjung halaman adalah:

A. Reflected XSS  
B. DOM-based XSS  
C. Stored XSS  
D. Self XSS  
E. Mutated XSS

---

### Soal 7
Indikator utama serangan CSRF pada log web server adalah:

A. User-Agent berisi nama scanner  
B. Payload SQL pada parameter URL  
C. Referer dari domain eksternal pada request POST ke endpoint sensitif  
D. Banyak request 404 berturut-turut  
E. Status code 500 yang berulang

---

### Soal 8
Skrip berbahaya yang diunggah ke web server untuk memberikan akses remote kepada penyerang disebut:

A. Rootkit  
B. Keylogger  
C. Web shell  
D. Ransomware  
E. Botnet client

---

### Soal 9
Perintah Linux yang tepat untuk mencari file PHP berisi fungsi `eval` atau `system` di direktori web adalah:

A. `find /var/www/ -name "*.php" -mtime -7`  
B. `grep -rl "eval\|system" /var/www/html/ --include="*.php"`  
C. `ls -la /var/www/html/*.php`  
D. `cat /var/www/html/*.php | grep eval`  
E. `locate *.php | xargs grep system`

---

### Soal 10
Empat kategori utama teknik anti-forensik menurut klasifikasi Dr. Marcus Rogers adalah:

A. Enkripsi, dekripsi, kompresi, dekompresi  
B. Data hiding, artifact wiping, trail obfuscation, attacks against tools  
C. Steganografi, kriptografi, hashing, encoding  
D. Prevention, detection, response, recovery  
E. Deletion, modification, creation, obfuscation

---

### Soal 11
Teknik menyembunyikan informasi rahasia di dalam gambar sehingga keberadaan pesan tidak terdeteksi disebut:

A. Kriptografi  
B. Hashing  
C. Steganografi  
D. Encoding  
E. Obfuscation

---

### Soal 12
Tool yang digunakan untuk mendeteksi steganografi pada file PNG dan BMP adalah:

A. Wireshark  
B. Volatility  
C. zsteg  
D. FTK Imager  
E. Autopsy

---

### Soal 13
Fitur NTFS yang memungkinkan penyimpanan data tersembunyi di dalam file normal disebut:

A. Extended Attributes  
B. Alternate Data Streams (ADS)  
C. Sparse Files  
D. Junction Points  
E. Hard Links

---

### Soal 14
Perintah Windows untuk mendeteksi ADS pada sebuah file adalah:

A. `attrib +h file.txt`  
B. `dir /R file.txt`  
C. `type file.txt`  
D. `copy file.txt`  
E. `del file.txt:stream`

---

### Soal 15
Standar secure deletion dari Departemen Pertahanan AS yang menggunakan 3 passes overwrite adalah:

A. Gutmann Method  
B. NIST 800-88  
C. DoD 5220.22-M  
D. Random Data  
E. Zero Fill

---

### Soal 16
Artefak Windows yang dapat membuktikan bahwa tool wiping (seperti BleachBit) pernah dijalankan adalah:

A. Pagefile.sys  
B. Hiberfil.sys  
C. Prefetch files  
D. SAM database  
E. Boot Configuration Data

---

### Soal 17
Teknik mengubah metadata waktu file untuk menyesatkan analisis timeline forensik disebut:

A. Log tampering  
B. Data wiping  
C. Timestomping  
D. ADS hiding  
E. Steganografi

---

### Soal 18
Pada NTFS, atribut MFT yang TIDAK dapat dimanipulasi oleh tools user-level adalah:

A. $STANDARD_INFORMATION  
B. $FILE_NAME  
C. $DATA  
D. $ATTRIBUTE_LIST  
E. $SECURITY_DESCRIPTOR

---

### Soal 19
Forensic countermeasure yang paling efektif untuk mencegah log tampering adalah:

A. Full disk encryption  
B. Centralized logging ke server terpisah  
C. Anti-virus pada web server  
D. Firewall rules  
E. Backup harian

---

### Soal 20
Windows Security Event ID yang mencatat penghapusan audit log adalah:

A. Event ID 4624  
B. Event ID 4672  
C. Event ID 1102  
D. Event ID 4688  
E. Event ID 7045

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Jelaskan apa yang dimaksud dengan SQL injection dan mengapa serangan ini berbahaya bagi sistem informasi militer!

---

### Soal 2 ⭐
Sebutkan tiga jenis SQL injection beserta cara deteksi masing-masing pada log web server!

---

### Soal 3 ⭐
Jelaskan perbedaan antara Reflected XSS dan Stored XSS dari perspektif forensik!

---

### Soal 4 ⭐
Dari entri log berikut, identifikasi jenis serangan dan informasi forensik yang relevan:
```
203.0.113.50 - - [20/Jan/2025:14:05:33 +0700] "GET /admin/users.php?id=1' OR '1'='1 HTTP/1.1" 200 8721 "-" "sqlmap/1.5"
```

---

### Soal 5 ⭐⭐
Tuliskan perintah grep untuk mendeteksi upaya SQL injection pada file access log Apache!

---

### Soal 6 ⭐⭐
Jelaskan bagaimana cara mendeteksi keberadaan web shell pada web server yang telah dikompromikan!

---

### Soal 7 ⭐⭐
Sebutkan empat kategori teknik anti-forensik dan berikan satu contoh untuk masing-masing!

---

### Soal 8 ⭐⭐
Jelaskan perbedaan antara steganografi dan kriptografi, serta mengapa steganografi lebih sulit dideteksi!

---

### Soal 9 ⭐⭐
Bagaimana cara mendeteksi dan mengekstrak Alternate Data Streams pada sistem Windows?

---

### Soal 10 ⭐⭐
Sebuah investigator menemukan bahwa BleachBit digunakan pada laptop tersangka. Sebutkan artefak yang masih mungkin ditemukan!

---

### Soal 11 ⭐⭐
Jelaskan bagaimana timestamp manipulation pada NTFS dapat dideteksi dengan membandingkan atribut $STANDARD_INFORMATION dan $FILE_NAME!

---

### Soal 12 ⭐⭐⭐
Investigator menemukan gap 3 jam pada access log Apache di hari kerja. Jelaskan langkah-langkah untuk memverifikasi apakah log telah dimanipulasi!

---

### Soal 13 ⭐⭐⭐
Rancang strategi forensic countermeasures untuk web server sistem komando di Kodam (minimal 4 langkah)!

---

### Soal 14 ⭐⭐⭐
Jelaskan mengapa "Logging Failures" (OWASP A09) sangat kritis dari perspektif forensik digital militer!

---

### Soal 15 ⭐⭐⭐
Dari analisis MFT berikut, tentukan apakah timestamp dimanipulasi dan jelaskan alasan Anda:

| Atribut | Created | Modified |
|---------|---------|----------|
| $STANDARD_INFORMATION | 2019-06-15 08:00:00 | 2019-06-15 08:00:00 |
| $FILE_NAME | 2025-02-20 14:30:12 | 2025-02-20 14:30:12 |

---

## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Serangan pada Sistem Informasi Lantamal

Sistem informasi personalia Lantamal IX melaporkan anomali pada 25 Februari 2025. Tim CERT menemukan entri log berikut:

```
10.0.0.55 - - [25/Feb/2025:01:15:22 +0700] "GET /personnel.php?emp_id=1' UNION SELECT username,password,NULL FROM admin_users-- HTTP/1.1" 200 12340
10.0.0.55 - - [25/Feb/2025:01:16:45 +0700] "POST /upload.php HTTP/1.1" 200 156
10.0.0.55 - - [25/Feb/2025:01:18:03 +0700] "GET /uploads/img/shell.php?cmd=whoami HTTP/1.1" 200 15
10.0.0.55 - - [25/Feb/2025:01:18:30 +0700] "GET /uploads/img/shell.php?cmd=cat+/etc/shadow HTTP/1.1" 200 2341
```

Selain itu, ditemukan file `laporan_personalia.pdf` di desktop tersangka dengan timestamp `$STANDARD_INFORMATION` Created: 2018-03-10 dan `$FILE_NAME` Created: 2025-02-25.

**Pertanyaan:**
1. Rekonstruksi urutan serangan berdasarkan log (10 poin)
2. Identifikasi semua jenis serangan yang terjadi (10 poin)
3. Analisis teknik anti-forensik yang digunakan (10 poin)
4. Tentukan tingkat keparahan insiden (10 poin)
5. Jelaskan langkah investigasi forensik yang harus dilakukan (10 poin)

---

### Studi Kasus 2: Analisis Anti-Forensik Insider Threat

Seorang anggota staf IT di Kodam IV/Diponegoro diduga membocorkan data operasi ke pihak asing. Dari analisis forensik pada laptopnya, ditemukan:

1. BleachBit tercatat di Prefetch files (terakhir dijalankan 22 Feb 2025 23:45)
2. Beberapa gambar JPEG di folder "Foto Liburan" memiliki ukuran file yang tidak proporsional dengan resolusinya
3. File `notulensi_rapat.docx` memiliki ADS berisi teks terenkripsi
4. Access log pada web server internal menunjukkan gap dari 22:00-23:30 pada 22 Feb 2025
5. Volume Shadow Copies telah dihapus

**Pertanyaan:**
1. Identifikasi semua teknik anti-forensik yang digunakan tersangka (10 poin)
2. Jelaskan analisis steganografi yang harus dilakukan pada gambar JPEG (10 poin)
3. Bagaimana cara mengekstrak dan menganalisis ADS pada file Word (10 poin)
4. Jelaskan artefak apa yang masih mungkin ditemukan meskipun BleachBit digunakan (10 poin)
5. Buat rekomendasi forensic countermeasures untuk mencegah kejadian serupa (10 poin)

---

## Kunci Jawaban

### Bagian A: Pilihan Ganda

| No | Jawaban | No | Jawaban |
|----|---------|----|---------|
| 1 | B | 11 | C |
| 2 | B | 12 | C |
| 3 | D | 13 | B |
| 4 | C | 14 | B |
| 5 | D | 15 | C |
| 6 | C | 16 | C |
| 7 | C | 17 | C |
| 8 | C | 18 | B |
| 9 | B | 19 | B |
| 10 | B | 20 | C |

---

### Bagian B: Uraian

**Soal 1:**
SQL injection adalah teknik menyisipkan perintah SQL berbahaya melalui input aplikasi web. Berbahaya bagi militer karena dapat menyebabkan kebocoran data sensitif (personel, operasi), manipulasi data logistik, dan pengambilalihan server sistem komando.

**Soal 2:**
(1) In-Band SQLi: payload terlihat langsung di URL/parameter log. (2) Blind SQLi: banyak request berulang dengan variasi parameter kecil. (3) Out-of-Band SQLi: sulit dideteksi dari log web saja, perlu korelasi dengan DNS/network log.

**Soal 3:**
Reflected XSS: payload hanya di URL request, bukti ada di access log dan browser history, tidak persisten. Stored XSS: payload tersimpan di database, bukti persisten, semua pengunjung halaman terdampak, investigator bisa analisis kapan saja dari database dump.

**Soal 4:**
Jenis serangan: SQL Injection. Informasi forensik: IP penyerang `203.0.113.50`, waktu 20 Jan 2025 14:05 WIB, payload `' OR '1'='1` (classic SQLi), tool `sqlmap/1.5` (automated scanner), response `200` menandakan request diproses.

**Soal 5:**
```bash
grep -iE "(union.*select|or.*1.*=.*1|select.*from|drop.*table|insert.*into|waitfor.*delay)" /var/log/apache2/access.log
```

**Soal 6:**
(1) Cari file PHP dengan fungsi berbahaya: `grep -rl "eval\|system\|exec" /var/www/html/`. (2) Cari file baru dimodifikasi: `find /var/www/html/ -name "*.php" -mtime -7`. (3) Cari file PHP di direktori upload. (4) Cari file berukuran sangat kecil (< 5KB).

**Soal 7:**
(1) Data Hiding: steganografi menyembunyikan pesan dalam gambar. (2) Artifact Wiping: BleachBit menghapus jejak aktivitas. (3) Trail Obfuscation: timestomping mengubah metadata waktu file. (4) Attacks Against Tools: file crafted yang membuat tool forensik crash.

**Soal 8:**
Steganografi menyembunyikan keberadaan pesan, kriptografi melindungi isi pesan. Steganografi lebih sulit dideteksi karena media pembawa (gambar/audio) tampak normal dan tidak menimbulkan kecurigaan, sedangkan data terenkripsi jelas terlihat ada meskipun isinya tidak terbaca.

**Soal 9:**
Deteksi: `dir /R namafile` atau `streams.exe -s C:\Users\suspect\` atau `Get-Item namafile -Stream *` di PowerShell. Ekstraksi: `more < namafile:namastream` atau `Get-Content namafile -Stream namastream`. Autopsy juga mendeteksi ADS secara otomatis pada image NTFS.

**Soal 10:**
Artefak yang masih mungkin: (1) Prefetch files `BLEACHBIT.EXE-*.pf`. (2) Registry entri instalasi. (3) Event ID 1102 jika audit log dihapus. (4) USN Journal. (5) Volume Shadow Copies (jika belum dihapus). (6) Sisa artefak di RAM jika sistem belum dimatikan.

**Soal 11:**
$STANDARD_INFORMATION dapat dimanipulasi oleh tools user-level (timestomp), sedangkan $FILE_NAME hanya di-update oleh kernel Windows. Jika timestamp $SI lebih lama dari $FN, atau keduanya sangat berbeda, maka terjadi timestomping. Timestamp asli adalah yang ada di $FILE_NAME.

**Soal 12:**
(1) Bandingkan dengan log firewall/IDS pada rentang waktu sama. (2) Periksa metadata file log dengan `stat` — lihat apakah file dimodifikasi setelah entri terakhir. (3) Bandingkan dengan remote log server jika ada. (4) Analisis filesystem timeline untuk akses/modifikasi file log. (5) Periksa bash history untuk perintah editing log (sed, vi, nano).

**Soal 13:**
(1) Centralized logging: kirim semua log ke server terpisah via rsyslog. (2) File integrity monitoring: AIDE atau Tripwire untuk memantau perubahan file kritis. (3) Database audit logging: catat semua query dari koneksi web application. (4) Automated memory acquisition: siapkan skrip memory dump saat alert IDS. (5) Network traffic capture: full packet capture pada segmen web server.

**Soal 14:**
Tanpa logging yang memadai: (1) tidak ada data untuk merekonstruksi serangan, (2) deteksi serangan terlambat atau tidak terdeteksi sama sekali, (3) pada sistem pertahanan berdampak pada keamanan nasional, (4) tidak memenuhi standar keamanan militer yang mewajibkan audit trail lengkap.

**Soal 15:**
Ya, timestamp dimanipulasi. $SI menunjukkan 2019, $FN menunjukkan 2025. Karena $FN dikelola kernel dan tidak bisa diubah user, timestamp asli file adalah 20 Feb 2025 (dari $FN). Penyerang menggunakan tool timestomping untuk mengubah $SI ke 2019. Selain itu, Created dan Modified $SI identik, indikasi perubahan serentak oleh tool.

---

### Bagian C: Studi Kasus

**Studi Kasus 1:**

1. **Urutan serangan**: 01:15 — SQL injection pada parameter `emp_id` mengekstrak kredensial admin. 01:16 — Upload file (web shell). 01:18:03 — Eksekusi web shell dengan `whoami`. 01:18:30 — Baca file `/etc/shadow` untuk mendapat password hash.

2. **Jenis serangan**: SQL injection (UNION-based), file upload vulnerability, remote code execution via web shell, privilege escalation attempt (baca /etc/shadow).

3. **Anti-forensik**: Timestamp manipulation pada `laporan_personalia.pdf` — $SI Created 2018 vs $FN Created 2025, menunjukkan timestomping.

4. **Keparahan**: KRITIS. Penyerang sudah mendapat RCE, membaca /etc/shadow (eskalasi privileged), dan data personalia terkompromi. Perlu incident response segera.

5. **Langkah investigasi**: Isolasi server, forensic imaging, analisis web shell, periksa database query log untuk cakupan kebocoran, verifikasi timestamp via MFT, periksa log firewall/IDS untuk aktivitas lanjutan.

**Studi Kasus 2:**

1. **Teknik anti-forensik**: (a) Artifact wiping: BleachBit. (b) Steganografi: gambar JPEG dengan ukuran tidak proporsional. (c) Data hiding: ADS pada file Word. (d) Log tampering: gap pada access log. (e) Artifact destruction: penghapusan Volume Shadow Copies.

2. **Analisis steganografi**: Preservasi gambar dan hitung hash. Periksa metadata dengan exiftool. Coba ekstraksi dengan steghide (password kosong dulu). Brute-force dengan stegseek jika ada password. Analisis visual dengan StegSolve. Cek binwalk untuk embedded files.

3. **Ekstraksi ADS**: `Get-Item notulensi_rapat.docx -Stream *` untuk list semua stream. `Get-Content notulensi_rapat.docx -Stream [namastream]` untuk baca isi. Jika terenkripsi, simpan untuk analisis kriptografi lebih lanjut.

4. **Artefak sisa BleachBit**: Prefetch files (sudah ditemukan), registry entries, USN Journal, event log 1102, file di Recycle Bin jika terlewat, dan RAM artifacts jika sistem masih hidup.

5. **Rekomendasi countermeasures**: Centralized logging ke server terpisah, file integrity monitoring (AIDE), DLP (Data Loss Prevention) untuk memantau transfer file sensitif, pembatasan instalasi software (tidak boleh install wiping tools), audit berkala perangkat staf yang menangani data rahasia.

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
