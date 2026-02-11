# Modul 15: Forensik Mobile dan Internet of Things (IoT)

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 15  
**Topik:** Forensik Mobile dan Internet of Things (IoT)  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Menjelaskan arsitektur sistem operasi Android dan iOS dari perspektif forensik
2. Menerapkan metode akuisisi data forensik pada perangkat mobile
3. Menganalisis artefak mobile: kontak, pesan, call logs, data aplikasi, dan lokasi GPS
4. Melakukan ekstraksi dan analisis backup perangkat mobile (Android dan iOS)
5. Memahami prinsip dasar forensik Internet of Things (IoT) dan protokol komunikasinya
6. Menginvestigasi perangkat IoT termasuk smart home, wearable, drone, dan sistem SCADA
7. Menyusun laporan forensik komprehensif untuk investigasi mobile dan IoT dalam konteks militer

---

## 1. Pendahuluan Forensik Mobile

### 1.1 Mengapa Forensik Mobile Penting?

Perangkat mobile telah menjadi bagian integral dari kehidupan modern, termasuk dalam operasi militer. Smartphone menyimpan volume data yang sangat besar: komunikasi, lokasi, foto, catatan, dan data aplikasi yang dapat menjadi bukti digital kritis dalam investigasi.

Dalam konteks militer Indonesia, forensik mobile relevan untuk:

| Konteks | Penjelasan |
|---------|------------|
| **Investigasi Insider Threat** | Analisis perangkat personel TNI yang dicurigai membocorkan informasi rahasia melalui aplikasi pesan |
| **Operasi Intelijen** | Ekstraksi data dari perangkat yang disita dalam operasi penggerebekan |
| **Insiden Keamanan** | Investigasi penggunaan perangkat mobile untuk mengakses jaringan militer secara tidak sah |
| **Counter-Intelligence** | Deteksi aplikasi spyware atau surveillance pada perangkat personel militer |
| **Operasi Lapangan** | Analisis perangkat yang ditemukan di zona operasi militer |

> **Fakta Penting:** Rata-rata pengguna smartphone menghasilkan lebih dari 2 GB data per hari, mencakup metadata lokasi, riwayat komunikasi, aktivitas aplikasi, dan jejak browsing yang semuanya berpotensi menjadi bukti digital.

### 1.2 Arsitektur Sistem Operasi Mobile

#### 1.2.1 Arsitektur Android

Android adalah sistem operasi open-source berbasis kernel Linux yang dikembangkan oleh Google. Pemahaman arsitektur Android sangat penting untuk forensik karena menentukan di mana data tersimpan dan bagaimana mengaksesnya.

| Layer | Komponen | Relevansi Forensik |
|-------|----------|-------------------|
| **Applications** | User apps, system apps | Data aplikasi, cache, preferences |
| **Application Framework** | Activity Manager, Content Provider | Database, shared data |
| **Libraries & Runtime** | SQLite, SSL, ART/Dalvik | Database format, enkripsi |
| **Hardware Abstraction Layer** | Camera, GPS, Sensors | Metadata sensor, lokasi |
| **Linux Kernel** | File system, drivers, security | Partisi, file system ext4/f2fs |

**Struktur Partisi Android:**

```
/boot        → Kernel dan ramdisk
/system      → Sistem operasi Android
/data        → Data pengguna dan aplikasi ← FOKUS FORENSIK UTAMA
/cache       → Cache sistem
/recovery    → Recovery mode
/sdcard      → Penyimpanan eksternal/emulasi
```

**Lokasi Data Penting di Android:**

| Data | Lokasi |
|------|--------|
| Database kontak | `/data/data/com.android.providers.contacts/databases/contacts2.db` |
| SMS/MMS | `/data/data/com.android.providers.telephony/databases/mmssms.db` |
| Call logs | `/data/data/com.android.providers.contacts/databases/calllog.db` |
| WiFi networks | `/data/misc/wifi/wpa_supplicant.conf` |
| Browser history | `/data/data/com.android.browser/databases/` |
| WhatsApp | `/data/data/com.whatsapp/databases/msgstore.db` |
| Lokasi GPS | `/data/data/com.google.android.gms/databases/` |

#### Solved Problem 1 ⭐

**Soal:** Seorang investigator forensik TNI AD perlu mengakses riwayat pesan WhatsApp dari perangkat Android milik personel yang dicurigai membocorkan informasi operasi ke pihak luar. Jelaskan di mana file database WhatsApp berada dan format penyimpanannya!

**Penyelesaian:**

*Step 1:* Identifikasi lokasi penyimpanan WhatsApp pada Android

Database utama WhatsApp tersimpan di:
- Internal: `/data/data/com.whatsapp/databases/msgstore.db`
- Backup: `/sdcard/WhatsApp/Databases/msgstore.db.crypt14`

*Step 2:* Identifikasi format penyimpanan

| File | Format | Keterangan |
|------|--------|------------|
| `msgstore.db` | SQLite 3 | Database utama, tidak terenkripsi (memerlukan root access) |
| `wa.db` | SQLite 3 | Database kontak WhatsApp |
| `msgstore.db.crypt14` | Encrypted SQLite | Backup terenkripsi di SD card |
| `axolotl.db` | SQLite 3 | Database enkripsi Signal Protocol |

*Step 3:* Cara akses forensik

Untuk mengakses `msgstore.db` secara langsung diperlukan root access atau metode akuisisi fisik. Backup yang terenkripsi (`.crypt14`) memerlukan key file yang tersimpan di `/data/data/com.whatsapp/files/key`.

**Jawaban:** Database WhatsApp berada di `/data/data/com.whatsapp/databases/msgstore.db` dalam format SQLite 3. File ini hanya dapat diakses dengan root access atau akuisisi fisik. Backup terenkripsi (`.crypt14`) tersedia di SD card namun memerlukan key file untuk dekripsi.

---

#### 1.2.2 Arsitektur iOS

iOS adalah sistem operasi proprietary dari Apple yang berjalan pada iPhone, iPad, dan iPod Touch. iOS memiliki arsitektur keamanan yang lebih ketat dibandingkan Android, sehingga forensik iOS memiliki tantangan tersendiri.

| Layer | Komponen | Relevansi Forensik |
|-------|----------|-------------------|
| **Cocoa Touch** | UIKit, MapKit, MessageUI | Data UI, lokasi, pesan |
| **Media** | Core Audio, Core Image | Media files, metadata |
| **Core Services** | Core Data, Core Location, SQLite | Database, lokasi, penyimpanan |
| **Core OS** | Kernel, Security, Keychain | Enkripsi, secure boot |

**Fitur Keamanan iOS yang Mempengaruhi Forensik:**

| Fitur | Deskripsi | Dampak Forensik |
|-------|-----------|----------------|
| **Secure Enclave** | Prosesor keamanan terpisah | Hardware encryption key tidak dapat diekstrak |
| **Data Protection API** | Enkripsi file berbasis class | File terenkripsi saat perangkat terkunci |
| **Keychain** | Penyimpanan kredensial aman | Password tersimpan terenkripsi |
| **Full Disk Encryption** | Enkripsi seluruh storage | Memerlukan passcode untuk akuisisi |
| **Secure Boot Chain** | Verifikasi boot sequence | Mencegah modifikasi OS |

**Lokasi Data Penting di iOS:**

| Data | Lokasi (relatif terhadap backup/image) |
|------|---------------------------------------|
| SMS/iMessage | `HomeDomain/Library/SMS/sms.db` |
| Call History | `HomeDomain/Library/CallHistoryDB/CallHistory.storedata` |
| Contacts | `HomeDomain/Library/AddressBook/AddressBook.sqlitedb` |
| Safari History | `HomeDomain/Library/Safari/History.db` |
| Photos | `CameraRollDomain/Media/DCIM/` |
| Location Data | `RootDomain/Library/Caches/locationd/consolidated.db` |
| WiFi Connections | `SystemPreferencesDomain/SystemConfiguration/com.apple.wifi.plist` |

#### Solved Problem 2 ⭐

**Soal:** Jelaskan mengapa akuisisi forensik pada perangkat iOS umumnya lebih sulit dibandingkan Android, dan sebutkan tiga fitur keamanan iOS yang menjadi penghalang utama!

**Penyelesaian:**

*Step 1:* Identifikasi perbedaan mendasar

Android bersifat open-source dengan opsi rooting yang relatif mudah, sedangkan iOS bersifat closed-source dengan arsitektur keamanan yang sangat ketat.

*Step 2:* Analisis tiga fitur keamanan utama

| Fitur | Mekanisme | Dampak pada Forensik |
|-------|-----------|---------------------|
| **Secure Enclave** | Prosesor terpisah yang menyimpan encryption key | Key tidak pernah terekspos ke prosesor utama, tidak bisa diekstrak via software |
| **Full Disk Encryption** | Seluruh data terenkripsi dengan key berbasis passcode | Tanpa passcode, data tidak dapat diakses meski storage diekstrak fisik |
| **USB Restricted Mode** | Port USB dinonaktifkan setelah 1 jam terkunci | Koneksi forensic tool terputus jika perangkat sudah terkunci lebih dari 1 jam |

**Jawaban:** Akuisisi iOS lebih sulit karena arsitektur keamanan berlapis: (1) Secure Enclave menyimpan encryption key di hardware terpisah yang tidak dapat diekstrak via software, (2) Full Disk Encryption mengenkripsi seluruh data berbasis passcode, dan (3) USB Restricted Mode memutus koneksi USB setelah 1 jam perangkat terkunci.

---

### 1.3 Metode Akuisisi Mobile Forensics

Akuisisi data adalah proses pengumpulan data dari perangkat mobile untuk analisis forensik. Terdapat beberapa level akuisisi dengan trade-off antara kelengkapan data dan tingkat kesulitan.

| Level | Metode | Data yang Diperoleh | Tools |
|-------|--------|-------------------|-------|
| **Level 1: Manual** | Interaksi langsung dengan UI | Konten layar, screenshot | Kamera, screen recorder |
| **Level 2: Logical** | Backup standar OS | File user, database, settings | iTunes, ADB, Autopsy |
| **Level 3: File System** | Akses file system lengkap | Semua file termasuk deleted | ADB root, SSH |
| **Level 4: Physical** | Bit-by-bit image | Raw data termasuk unallocated space | Cellebrite, JTAG |
| **Level 5: Chip-off** | Ekstraksi chip memori | Raw NAND/eMMC data | Desolder, adapter |

#### 1.3.1 Akuisisi Android

**Metode ADB (Android Debug Bridge):**

```bash
# Cek koneksi perangkat
adb devices

# Logical acquisition - backup
adb backup -all -shared -apk -system backup.ab

# File system acquisition (root required)
adb shell su -c "dd if=/dev/block/mmcblk0 of=/sdcard/image.raw bs=4096"

# Pull specific databases
adb pull /data/data/com.whatsapp/databases/msgstore.db

# Screenshot
adb exec-out screencap -p > screen.png

# Daftar semua packages yang terinstall
adb shell pm list packages
```

**Android Backup Extractor:**

```bash
# Konversi .ab ke .tar
java -jar abe.jar unpack backup.ab backup.tar

# Ekstrak
tar xf backup.tar
```

#### 1.3.2 Akuisisi iOS

**Metode iTunes Backup:**

| Tipe Backup | Enkripsi | Data yang Termasuk |
|-------------|----------|-------------------|
| **Unencrypted** | Tidak | File dasar, foto, kontak, pesan |
| **Encrypted** | Ya (password user) | Semua data termasuk keychain, health data, WiFi passwords |

**Lokasi Backup iTunes di Windows:**
```
C:\Users\<username>\Apple\MobileSync\Backup\
```

**Lokasi Backup iTunes di macOS:**
```
~/Library/Application Support/MobileSync/Backup/
```

Setiap backup berisi file `Manifest.db` (SQLite) yang memetakan nama file hash ke path aslinya.

#### Solved Problem 3 ⭐⭐

**Soal:** Tim Siber TNI AL perlu melakukan akuisisi data dari smartphone Android milik personel yang dicurigai melakukan komunikasi dengan agen asing. Perangkat dalam kondisi menyala dan tidak di-lock. Jelaskan langkah-langkah akuisisi logical menggunakan ADB!

**Penyelesaian:**

*Step 1:* Persiapan sebelum akuisisi

1. Aktifkan mode pesawat untuk mencegah remote wipe
2. Pastikan baterai mencukupi (minimal 50%)
3. Dokumentasi kondisi awal perangkat (foto/video)
4. Catat informasi perangkat (model, IMEI, OS version)

*Step 2:* Aktifkan USB Debugging

1. Masuk ke Settings → About Phone
2. Tap "Build Number" 7 kali untuk mengaktifkan Developer Options
3. Settings → Developer Options → Enable USB Debugging
4. Sambungkan perangkat via USB dan authorize koneksi

*Step 3:* Lakukan akuisisi logical

```bash
# 1. Verifikasi koneksi
adb devices

# 2. Catat informasi perangkat
adb shell getprop ro.product.model
adb shell getprop ro.build.version.release
adb shell settings get secure android_id

# 3. Full backup
adb backup -all -shared -apk -system -f evidence_backup.ab

# 4. Pull data spesifik yang penting
adb pull /sdcard/WhatsApp/
adb pull /sdcard/DCIM/
adb pull /sdcard/Download/

# 5. Daftar semua aplikasi terinstall
adb shell pm list packages -f > installed_apps.txt

# 6. Hitung hash untuk verifikasi
certutil -hashfile evidence_backup.ab SHA256
```

*Step 4:* Dokumentasi chain of custody

Catat waktu mulai dan selesai akuisisi, hash file hasil, dan nama investigator.

**Jawaban:** Langkah akuisisi: (1) aktifkan mode pesawat, (2) dokumentasi kondisi perangkat, (3) aktifkan USB Debugging, (4) verifikasi koneksi ADB, (5) lakukan full backup dengan `adb backup -all`, (6) pull data spesifik (WhatsApp, DCIM, Download), (7) hitung hash SHA-256 untuk integritas, dan (8) dokumentasi chain of custody.

---

## 2. Analisis Artefak Mobile

### 2.1 Database SQLite pada Mobile

Mayoritas data pada perangkat mobile disimpan dalam format database SQLite. Pemahaman tentang SQLite sangat penting untuk forensik mobile.

**Tools untuk Analisis SQLite:**

| Tool | Deskripsi | Platform |
|------|-----------|----------|
| **DB Browser for SQLite** | GUI database browser | Windows, macOS, Linux |
| **sqlite3** | Command-line tool | Cross-platform |
| **Autopsy** | Forensic suite dengan SQLite viewer | Windows |

**Query Dasar untuk Forensik Mobile:**

```sql
-- Membaca semua tabel dalam database
SELECT name FROM sqlite_master WHERE type='table';

-- Membaca pesan SMS
SELECT address, body, date, type FROM sms 
ORDER BY date DESC;

-- Membaca call logs
SELECT number, duration, date, type FROM calls 
ORDER BY date DESC;

-- Membaca kontak
SELECT display_name, data1 AS phone 
FROM raw_contacts 
JOIN data ON raw_contacts._id = data.raw_contact_id;

-- WhatsApp messages
SELECT key_remote_jid, data, timestamp, media_url 
FROM messages 
ORDER BY timestamp DESC;
```

### 2.2 Analisis Komunikasi

#### 2.2.1 SMS dan Call Log

Data SMS pada Android tersimpan di `mmssms.db` dengan struktur:

| Kolom | Keterangan |
|-------|------------|
| `_id` | ID unik pesan |
| `address` | Nomor telepon pengirim/penerima |
| `body` | Isi pesan |
| `date` | Timestamp Unix (milliseconds) |
| `type` | 1 = Received, 2 = Sent, 3 = Draft |
| `read` | 0 = Unread, 1 = Read |
| `thread_id` | ID percakapan |

#### 2.2.2 Aplikasi Pesan Instan

| Aplikasi | Database | Lokasi |
|----------|----------|--------|
| **WhatsApp** | `msgstore.db` | `/data/data/com.whatsapp/databases/` |
| **Telegram** | `cache4.db` | `/data/data/org.telegram.messenger/files/` |
| **Signal** | `signal.db` | `/data/data/org.thoughtcrime.securesms/databases/` |
| **LINE** | `naver_line` | `/data/data/jp.naver.line.android/databases/` |

#### Solved Problem 4 ⭐⭐

**Soal:** Investigator menemukan file `mmssms.db` dari akuisisi perangkat Android. Tuliskan query SQL untuk menampilkan semua pesan SMS yang dikirim (type = 2) selama bulan Januari 2026, diurutkan berdasarkan waktu pengiriman!

**Penyelesaian:**

*Step 1:* Pahami format timestamp Android

Android menyimpan timestamp dalam Unix epoch format dengan satuan milliseconds. Untuk Januari 2026:
- 1 Januari 2026 00:00:00 UTC = 1767225600000 ms
- 31 Januari 2026 23:59:59 UTC = 1769904000000 ms

*Step 2:* Tulis query SQL

```sql
SELECT 
    address AS nomor_tujuan,
    body AS isi_pesan,
    datetime(date/1000, 'unixepoch', 'localtime') AS waktu_kirim,
    read AS status_baca
FROM sms
WHERE type = 2 
    AND date >= 1767225600000 
    AND date < 1769904000000
ORDER BY date ASC;
```

**Jawaban:** Query di atas memfilter pesan dengan `type = 2` (sent) dalam rentang timestamp Januari 2026, mengkonversi timestamp ke format tanggal yang mudah dibaca, dan mengurutkan secara kronologis.

---

### 2.3 Analisis Data Lokasi

Data lokasi GPS adalah salah satu artefak forensik paling bernilai dari perangkat mobile karena dapat merekonstruksi pergerakan pengguna.

**Sumber Data Lokasi pada Perangkat Mobile:**

| Sumber | Deskripsi | Akurasi |
|--------|-----------|---------|
| **GPS** | Sinyal satelit | 3-5 meter |
| **Cell Tower** | Triangulasi menara seluler | 100-300 meter |
| **WiFi** | Posisi berdasarkan WiFi AP | 10-30 meter |
| **Bluetooth** | Proximity dengan BLE beacon | 1-3 meter |
| **Photo EXIF** | Metadata GPS dalam foto | Bervariasi |

**Analisis EXIF Metadata pada Foto:**

```bash
# Menggunakan ExifTool
exiftool -gps* photo.jpg

# Output contoh:
# GPS Latitude  : 6 deg 10' 30.00" S
# GPS Longitude : 106 deg 49' 35.00" E
# GPS Altitude  : 15 m
# GPS Time Stamp: 14:30:00
```

Koordinat di atas menunjukkan lokasi di sekitar Jakarta, yang bisa menjadi bukti keberadaan pemilik perangkat di lokasi tersebut pada waktu tertentu.

#### Solved Problem 5 ⭐⭐

**Soal:** Dari analisis perangkat Android personel TNI AD, ditemukan foto dengan metadata EXIF berikut:
- GPS Latitude: 7° 17' 45.6" S
- GPS Longitude: 112° 44' 30.8" E
- Date/Time Original: 2026:01:15 09:30:00

Tentukan lokasi dan signifikansi temuan ini dalam konteks investigasi keamanan militer!

**Penyelesaian:**

*Step 1:* Konversi koordinat ke format desimal

```
Latitude:  7 + 17/60 + 45.6/3600 = 7.296° S = -7.296
Longitude: 112 + 44/60 + 30.8/3600 = 112.742° E = 112.742
```

*Step 2:* Identifikasi lokasi

Koordinat (-7.296, 112.742) menunjukkan lokasi di Surabaya, Jawa Timur, yang merupakan area Pangkalan Utama TNI AL (Lantamal V) dan Kodam V/Brawijaya.

*Step 3:* Evaluasi signifikansi

Foto yang diambil di sekitar instalasi militer memerlukan penyelidikan lebih lanjut: apakah personel memiliki otorisasi untuk memotret di area tersebut, dan apakah konten foto mengandung informasi sensitif.

**Jawaban:** Lokasi berada di Surabaya, dekat instalasi militer Lantamal V dan Kodam V/Brawijaya. Pengambilan foto di sekitar fasilitas militer perlu investigasi lebih lanjut terkait otorisasi dan konten foto yang mungkin mengandung informasi rahasia.

---

### 2.4 Analisis Data Aplikasi

#### 2.4.1 Browser History dan Bookmarks

```sql
-- Chrome History (Android)
-- Database: /data/data/com.android.chrome/app_chrome/Default/History

SELECT url, title, 
    datetime(last_visit_time/1000000-11644473600, 'unixepoch', 'localtime') AS visit_time,
    visit_count
FROM urls
ORDER BY last_visit_time DESC
LIMIT 50;
```

#### 2.4.2 Social Media Artifacts

| Platform | Database | Data Kunci |
|----------|----------|-----------|
| **Facebook** | `fb.db`, `threads_db2` | Posts, messages, friends |
| **Instagram** | `direct.db` | DM, stories viewed |
| **Twitter/X** | `twitter.db` | Tweets, DMs, timeline |
| **TikTok** | `db_im_xx` | Messages, viewed videos |

#### Solved Problem 6 ⭐⭐⭐

**Soal:** Tim Cyber TNI menemukan bahwa seorang personel menggunakan Telegram untuk berkomunikasi dengan akun anonim. Database `cache4.db` berhasil diekstrak. Jelaskan bagaimana menganalisis database Telegram untuk mengidentifikasi pola komunikasi!

**Penyelesaian:**

*Step 1:* Buka database menggunakan DB Browser for SQLite

Database Telegram Android (`cache4.db`) menyimpan data dalam beberapa tabel utama:

| Tabel | Konten |
|-------|--------|
| `messages` | Isi pesan, timestamp, media |
| `users` | Informasi pengguna |
| `contacts` | Daftar kontak |
| `dialogs` | Daftar percakapan |
| `media_v4` | File media yang dikirim/diterima |

*Step 2:* Query untuk analisis pola komunikasi

```sql
-- Identifikasi semua percakapan dan frekuensinya
SELECT uid, COUNT(*) AS jumlah_pesan, 
    MIN(date) AS pesan_pertama,
    MAX(date) AS pesan_terakhir
FROM messages
GROUP BY uid
ORDER BY jumlah_pesan DESC;

-- Detail pesan dengan user tertentu
SELECT m.uid, m.data AS isi_pesan,
    datetime(m.date, 'unixepoch', 'localtime') AS waktu,
    u.name AS nama_user
FROM messages m
LEFT JOIN users u ON m.uid = u.uid
WHERE m.uid = [target_uid]
ORDER BY m.date;
```

*Step 3:* Analisis pola

Dari query di atas, investigator dapat mengidentifikasi: frekuensi komunikasi, jam-jam aktif, pola pengiriman pesan (apakah ada jadwal rutin), dan konten yang mencurigakan.

**Jawaban:** Analisis dilakukan dengan membuka `cache4.db` menggunakan SQLite browser, memeriksa tabel `messages` dan `users` untuk mengidentifikasi lawan bicara dan frekuensi komunikasi, kemudian menganalisis pola waktu dan konten pesan untuk mendeteksi aktivitas mencurigakan.

---

## 3. Forensik Internet of Things (IoT)

### 3.1 Pengantar IoT Forensics

Internet of Things (IoT) adalah jaringan perangkat fisik yang terhubung ke internet dan saling bertukar data. Dalam konteks militer, perangkat IoT meliputi sensor, kamera pengawas, drone, sistem kontrol industri, dan perangkat wearable yang digunakan personel.

**Tantangan Unik Forensik IoT:**

| Tantangan | Penjelasan |
|-----------|------------|
| **Heterogenitas** | Beragam perangkat, OS, dan protokol |
| **Keterbatasan Resource** | CPU, RAM, dan storage minimal |
| **Volatilitas Data** | Data sering di-overwrite karena storage terbatas |
| **Akses Fisik** | Perangkat bisa tertanam atau sulit dijangkau |
| **Protokol Non-Standar** | MQTT, CoAP, Zigbee, BLE memerlukan tools khusus |
| **Cloud Dependency** | Data sering tersimpan di cloud vendor |
| **Kurangnya Logging** | Banyak perangkat IoT tidak memiliki mekanisme log yang memadai |

### 3.2 Arsitektur dan Protokol IoT

#### 3.2.1 Arsitektur IoT 3-Layer

```
┌─────────────────────────────┐
│      APPLICATION LAYER       │  ← Dashboard, analitik, API
├─────────────────────────────┤
│       NETWORK LAYER          │  ← Komunikasi, routing, gateway
├─────────────────────────────┤
│      PERCEPTION LAYER        │  ← Sensor, aktuator, perangkat fisik
└─────────────────────────────┘
```

#### 3.2.2 Protokol Komunikasi IoT

| Protokol | Transport | Penggunaan | Forensik |
|----------|-----------|------------|----------|
| **MQTT** | TCP | Smart home, sensor monitoring | Intercept via broker, topic analysis |
| **CoAP** | UDP | Constrained devices | Packet capture, resource discovery |
| **Zigbee** | IEEE 802.15.4 | Home automation, sensor network | RF sniffing, key extraction |
| **BLE** | Bluetooth Low Energy | Wearables, proximity | GATT analysis, advertising data |
| **LoRaWAN** | LoRa | Long-range sensor network | Gateway log analysis |
| **Z-Wave** | Z-Wave protocol | Smart home | Controller log analysis |
| **Modbus** | TCP/Serial | Industrial control (SCADA) | Traffic capture, register analysis |

#### Solved Problem 7 ⭐

**Soal:** Jelaskan perbedaan antara protokol MQTT dan CoAP dalam konteks forensik IoT!

**Penyelesaian:**

*Step 1:* Bandingkan karakteristik kedua protokol

| Aspek | MQTT | CoAP |
|-------|------|------|
| **Transport** | TCP (port 1883/8883) | UDP (port 5683) |
| **Model** | Publish-Subscribe | Request-Response (mirip HTTP) |
| **Arsitektur** | Memerlukan broker | Langsung antar perangkat |
| **QoS** | 3 level (0, 1, 2) | Confirmable/Non-confirmable |
| **Overhead** | Minimum 2 bytes header | 4 bytes header |

*Step 2:* Implikasi forensik

- **MQTT**: Broker menyimpan log subscribe/publish yang menjadi sumber bukti penting. Forensik dapat dilakukan dengan menganalisis broker log dan men-capture traffic ke/dari broker.
- **CoAP**: Komunikasi langsung peer-to-peer, sehingga memerlukan network capture di titik yang tepat. Resource discovery (`.well-known/core`) dapat mengungkap kapabilitas perangkat.

**Jawaban:** MQTT menggunakan TCP dengan model publish-subscribe melalui broker, memudahkan forensik melalui broker log. CoAP menggunakan UDP dengan model request-response langsung antar perangkat, memerlukan packet capture untuk analisis forensik.

---

### 3.3 Forensik Smart Home Devices

Smart home devices menyimpan berbagai data yang bernilai forensik, termasuk pola aktivitas penghuni, rekaman audio/video, dan histori perintah.

#### 3.3.1 Amazon Echo / Google Home / Smart Speaker

| Artefak | Sumber | Data |
|---------|--------|------|
| **Voice Commands** | Cloud account | Histori perintah suara |
| **Activity Log** | Cloud/App | Timestamp aktivasi, queries |
| **Connected Devices** | App settings | Daftar perangkat terintegrasi |
| **Routines** | App configuration | Jadwal otomatis |
| **Network Traffic** | Router/capture | Pola komunikasi, endpoint |

**Cara Forensik Smart Speaker:**

1. **Cloud Acquisition**: Akses akun pengguna (Amazon/Google) untuk histori perintah
2. **App Data**: Analisis aplikasi companion di smartphone
3. **Network Analysis**: Capture traffic antara speaker dan cloud
4. **Physical**: Dump firmware (tingkat lanjut)

#### 3.3.2 Smart Camera / CCTV IP

Smart camera menyimpan rekaman video, motion events, dan metadata yang sangat bernilai untuk forensik.

| Data | Lokasi | Format |
|------|--------|--------|
| **Video Recordings** | SD Card / Cloud | MP4, H.264/H.265 |
| **Motion Events** | App / Cloud | JSON with timestamps |
| **Configuration** | Device firmware | Config files |
| **Network Logs** | Device / Router | Syslog |

#### Solved Problem 8 ⭐⭐

**Soal:** Sebuah insiden keamanan terjadi di pos penjagaan militer yang dilengkapi smart camera. Camera berhenti merekam selama 30 menit pada malam kejadian. Jelaskan langkah forensik untuk menginvestigasi apakah camera sengaja dimatikan atau mengalami gangguan!

**Penyelesaian:**

*Step 1:* Preservasi bukti

- Jangan restart camera, dokumentasi kondisi saat ini
- Capture network traffic aktif
- Backup SD card jika ada
- Catat timestamp saat ini pada perangkat

*Step 2:* Analisis multi-sumber

| Sumber | Yang Diperiksa |
|--------|---------------|
| **SD Card** | Gap dalam rekaman video, file timeline |
| **Camera Logs** | System log, shutdown/reboot events |
| **Router Logs** | Koneksi camera ke jaringan, disconnect events |
| **Cloud Platform** | Activity log di platform cloud camera |
| **DHCP Logs** | Lease renew/release patterns |
| **NVR/DVR** | Recording timeline dari network recorder |

*Step 3:* Tentukan penyebab

Jika camera log menunjukkan shutdown command → kemungkinan sengaja dimatikan. Jika router log menunjukkan koneksi aktif namun tidak ada rekaman → kemungkinan ada manipulasi pada camera. Jika seluruh koneksi terputus bersamaan → kemungkinan gangguan power atau jaringan.

**Jawaban:** Langkah forensik meliputi: preservasi kondisi perangkat, analisis SD card untuk gap rekaman, pemeriksaan camera log dan router log untuk menentukan apakah terjadi shutdown command (sengaja), manipulasi software, atau gangguan power/jaringan. Korelasi timestamp dari multiple sources membantu menentukan penyebab sebenarnya.

---

### 3.4 Forensik Wearable Devices

Perangkat wearable (smartwatch, fitness tracker) menyimpan data kesehatan, lokasi, dan aktivitas yang dapat menjadi bukti forensik.

**Data Forensik dari Wearable:**

| Kategori | Data | Nilai Forensik |
|----------|------|---------------|
| **Kesehatan** | Heart rate, SpO2, sleep pattern | Menentukan kondisi fisik pada waktu tertentu |
| **Aktivitas** | Steps, distance, calories | Rekonstruksi pergerakan |
| **Lokasi** | GPS tracks, geofencing | Pelacakan posisi |
| **Notifikasi** | Mirror dari smartphone | Pesan, panggilan, email |
| **Biometrik** | Fingerprint, ECG | Identifikasi pengguna |

#### Solved Problem 9 ⭐⭐

**Soal:** Seorang perwira TNI ditemukan tidak sadarkan diri di area latihan militer. Smartwatch yang dikenakannya merekam data kesehatan. Bagaimana data wearable dapat membantu rekonstruksi kejadian?

**Penyelesaian:**

*Step 1:* Identifikasi data yang tersedia

Data dari smartwatch yang relevan:
- Heart rate timeline (menit per menit)
- Step count dan activity pattern
- GPS location history
- SpO2 (saturasi oksigen)
- Fall detection events

*Step 2:* Analisis timeline

| Waktu (Contoh) | Heart Rate | Steps | Event |
|-----------------|-----------|-------|-------|
| 14:00 | 72 bpm | Normal | Aktivitas biasa |
| 14:30 | 145 bpm | Meningkat | Latihan fisik intensif |
| 15:00 | 180 bpm | Menurun | Kemungkinan stress/panik |
| 15:15 | 42 bpm | 0 | Penurunan drastis |
| 15:16 | - | 0 | Fall detection triggered |

*Step 3:* Korelasi dengan GPS

Track GPS menunjukkan pergerakan korban dan titik terakhir sebelum collapse, membantu menentukan lokasi kejadian dan apakah ada pergerakan setelahnya (kemungkinan dipindahkan).

**Jawaban:** Data wearable membantu merekonstruksi timeline kejadian melalui: (1) heart rate pattern untuk menentukan kapan kondisi mulai memburuk, (2) GPS track untuk lokasi tepat kejadian, (3) fall detection untuk waktu collapse, dan (4) korelasi data kesehatan dengan aktivitas untuk menentukan penyebab.

---

### 3.5 Forensik Drone dan Unmanned Systems

Drone (UAV) semakin banyak digunakan dalam operasi militer untuk pengintaian, pemetaan, dan misi khusus. Forensik drone menjadi sangat relevan dalam konteks pertahanan.

**Komponen Data Forensik Drone:**

| Komponen | Data | Format |
|----------|------|--------|
| **Flight Controller** | Flight logs, waypoints, GPS tracks | DAT, TXT, CSV |
| **Gimbal/Camera** | Foto, video, metadata EXIF | JPG, MP4, MOV |
| **Remote Controller** | Paired devices, flight history | Vendor-specific |
| **Mobile App** | Flight records, account info, cached maps | SQLite, cache files |
| **SD Card** | Media, logs | FAT32/exFAT |
| **Internal Storage** | Firmware, configuration | Vendor-specific |

**Tools untuk Drone Forensics:**

| Tool | Fungsi |
|------|--------|
| **DatCon** | Parser untuk DJI flight log (.DAT) |
| **CsvView** | Visualisasi flight path dari CSV |
| **Autopsy** | Analisis SD card dan mobile app |
| **Google Earth** | Visualisasi flight path KML/KMZ |
| **ExifTool** | Ekstraksi metadata foto/video |

#### Solved Problem 10 ⭐⭐⭐

**Soal:** Sebuah drone tidak dikenal tertangkap terbang di atas instalasi militer Kodam Jaya. Drone berhasil diamankan dalam kondisi utuh. Jelaskan prosedur forensik lengkap untuk menginvestigasi drone tersebut!

**Penyelesaian:**

*Step 1:* Preservasi dan dokumentasi awal

1. Jangan menyalakan drone, dokumentasi kondisi fisik (foto semua sisi)
2. Catat serial number, model, dan identifikasi fisik
3. Lepaskan baterai untuk mencegah remote wipe
4. Simpan dalam Faraday bag untuk mencegah komunikasi wireless
5. Dokumentasi chain of custody

*Step 2:* Akuisisi data

| Sumber | Metode Akuisisi |
|--------|----------------|
| SD Card | Remove dan image menggunakan FTK Imager |
| Internal Storage | Koneksi via USB (jika memungkinkan) |
| Flight Controller | Ekstraksi log via vendor software (DJI Assistant) |
| Remote Controller | Analisis paired controller jika tersedia |
| Mobile App | Akuisisi dari smartphone operator (jika teridentifikasi) |

*Step 3:* Analisis

| Analisis | Tujuan |
|----------|--------|
| Flight Log | Rekonstruksi rute terbang, asal lepas landas, dan pola penerbangan |
| Media Files | Identifikasi target pengamatan drone |
| EXIF Data | Koordinat dan waktu pengambilan foto/video |
| Registration Info | Identifikasi pemilik/operator |
| WiFi/BLE History | Identifikasi controller yang pernah terkoneksi |

*Step 4:* Rekonstruksi

Gunakan DatCon untuk parsing flight log DJI, ekspor ke format KML, dan visualisasikan di Google Earth untuk melihat rute penerbangan secara detail.

**Jawaban:** Prosedur forensik drone: (1) preservasi tanpa menyalakan, lepas baterai, simpan di Faraday bag; (2) akuisisi SD card, internal storage, dan flight controller log; (3) analisis flight log untuk rute terbang, media files untuk target pengamatan, dan EXIF data untuk lokasi/waktu; (4) rekonstruksi rute di Google Earth dan identifikasi operator melalui registration info dan paired devices.

---

### 3.6 Forensik Sistem Kontrol Industri (ICS/SCADA)

Industrial Control Systems (ICS) dan Supervisory Control and Data Acquisition (SCADA) digunakan untuk mengendalikan infrastruktur kritis, termasuk fasilitas militer seperti sistem pembangkit listrik, distribusi air, dan sistem pertahanan.

**Komponen ICS/SCADA:**

| Komponen | Fungsi | Relevansi Forensik |
|----------|--------|-------------------|
| **HMI** | Human-Machine Interface | Operator logs, alarm history |
| **PLC** | Programmable Logic Controller | Program logic, configuration changes |
| **RTU** | Remote Terminal Unit | Sensor data, communication logs |
| **Historian** | Data logging server | Time-series process data |
| **SCADA Server** | Supervisory control | Control commands, event logs |

**Protokol ICS/SCADA:**

| Protokol | Penggunaan | Port | Forensik |
|----------|------------|------|----------|
| **Modbus TCP** | Industrial automation | 502 | Register read/write analysis |
| **DNP3** | Power grid, utilities | 20000 | Point data, event logs |
| **OPC UA** | Industrial interoperability | 4840 | Session logs, data access |
| **BACnet** | Building automation | 47808 | Object access logs |
| **EtherNet/IP** | Manufacturing | 44818 | CIP message analysis |

#### Solved Problem 11 ⭐⭐⭐

**Soal:** Sistem SCADA di pembangkit listrik pangkalan militer mengalami anomali: parameter suhu reaktor menunjukkan perubahan mendadak yang tidak sesuai dengan kondisi operasional. Jelaskan langkah forensik untuk menentukan apakah ini serangan siber atau gangguan teknis!

**Penyelesaian:**

*Step 1:* Isolasi dan preservasi

1. Jangan matikan sistem SCADA (preservasi data volatile)
2. Capture network traffic segera menggunakan port mirroring
3. Screenshot HMI dan kondisi alarm saat ini
4. Backup Historian database

*Step 2:* Analisis multi-layer

| Layer | Yang Diperiksa | Indikator Serangan |
|-------|---------------|-------------------|
| **Network** | PCAP untuk traffic Modbus/DNP3 | Command injection, unauthorized register writes |
| **HMI** | Operator log, login history | Login anomali, di luar jam kerja |
| **PLC** | Program logic, configuration | Perubahan logic yang tidak terotorisasi |
| **Historian** | Time-series data | Manipulasi data sensor |
| **Firewall** | Connection logs | Koneksi dari IP tidak dikenal |

*Step 3:* Analisis Modbus traffic

```
# Filter Wireshark untuk Modbus
modbus.func_code == 6    # Write Single Register
modbus.func_code == 16   # Write Multiple Registers

# Cari write command yang mencurigakan
# Jika ada write ke register suhu dari IP yang bukan HMI → indikasi serangan
```

*Step 4:* Korelasi temporal

Bandingkan timestamp perubahan parameter suhu dengan: network event, login activity, dan perubahan PLC configuration untuk menentukan apakah terjadi serangan terkoordinasi atau anomali teknis.

**Jawaban:** Langkah forensik: (1) isolasi tanpa mematikan sistem, capture traffic; (2) analisis network traffic untuk command injection pada Modbus; (3) periksa HMI login log dan PLC configuration changes; (4) korelasi temporal antara anomali suhu, network events, dan login activity. Indikator serangan meliputi: write command dari IP unauthorized, login anomali, dan perubahan PLC logic.

---

## 4. Forensik Vehicle dan Telematics

### 4.1 Digital Evidence pada Kendaraan Modern

Kendaraan militer modern dilengkapi dengan berbagai sistem elektronik yang menyimpan data forensik.

**Sumber Data Forensik Kendaraan:**

| Sistem | Data | Lokasi |
|--------|------|--------|
| **ECU (Engine Control Unit)** | Parameter mesin, error codes | OBD-II port |
| **Infotainment** | Media, kontak, call log, navigasi | Head unit storage |
| **Event Data Recorder** | Kecepatan, brake, airbag events | Crash data module |
| **GPS/Telematics** | Rute, lokasi, geofence violations | Telematics unit |
| **Dashcam** | Video recording, G-sensor data | SD card |
| **Key Fob** | Lock/unlock history | Vehicle controller |
| **Bluetooth** | Paired devices, call history | Infotainment module |

#### Solved Problem 12 ⭐⭐

**Soal:** Kendaraan dinas militer terlibat dalam insiden di luar jam operasional. Infotainment system kendaraan tersimpan data Bluetooth paired devices. Bagaimana data ini dapat membantu investigasi?

**Penyelesaian:**

*Step 1:* Identifikasi data Bluetooth

Data paired devices menyimpan:
- MAC address perangkat
- Device name (sering berisi nama pemilik)
- Waktu pertama pairing
- Riwayat koneksi

*Step 2:* Analisis forensik

| Data | Informasi |
|------|-----------|
| MAC Address | Identifikasi unik perangkat yang pernah terkoneksi |
| Device Name | Contoh: "iPhone milik Sertu Budi" |
| Connection Time | Waktu koneksi menunjukkan siapa yang menggunakan kendaraan |
| Call Log via BT | Riwayat panggilan saat terhubung ke kendaraan |

*Step 3:* Korelasi

Bandingkan daftar paired devices dengan personel yang memiliki akses ke kendaraan, dan cocokkan connection timestamp dengan waktu insiden.

**Jawaban:** Data Bluetooth paired devices membantu mengidentifikasi siapa yang menggunakan kendaraan melalui MAC address dan device name, menentukan waktu penggunaan via connection timestamp, dan mengakses riwayat panggilan yang dilakukan saat terhubung ke kendaraan. Ini membantu menentukan pengguna kendaraan saat insiden terjadi.

---

## 5. Mobile Backup Forensics

### 5.1 Android Backup Analysis

#### 5.1.1 Format Backup Android

| Format | Ekstensi | Tools |
|--------|----------|-------|
| **ADB Backup** | `.ab` | Android Backup Extractor (ABE) |
| **Google Backup** | Cloud | Google Takeout |
| **Samsung Backup** | `.sbu` | Samsung Smart Switch |
| **Custom Recovery** | `.img`, `.tar` | TWRP, Odin |

**Langkah Analisis ADB Backup:**

```bash
# 1. Konversi backup ke tar
java -jar abe.jar unpack backup.ab backup.tar [password]

# 2. Ekstrak tar
tar xf backup.tar

# 3. Struktur yang dihasilkan
apps/
├── com.whatsapp/
│   ├── db/
│   │   └── msgstore.db
│   └── f/
├── com.android.chrome/
│   └── db/
│       └── History
└── com.android.providers.contacts/
    └── db/
        └── contacts2.db
```

### 5.2 iOS Backup Analysis

#### 5.2.1 Struktur iTunes Backup

iTunes backup menyimpan file dengan nama SHA-1 hash dari domain dan relative path-nya.

**File Penting dalam iOS Backup:**

| Domain | File | SHA-1 Name | Konten |
|--------|------|-----------|--------|
| HomeDomain | Library/SMS/sms.db | `3d0d7e5fb2ce288813306e4d4636395e047a3d28` | SMS/iMessage |
| HomeDomain | Library/CallHistoryDB/ | `2b2b0084a1bc3a5ac8c27afdf14afb42c61a19ca` | Call History |
| HomeDomain | Library/AddressBook/ | `31bb7ba8914766d4ba40d6dfb6113c8b614be442` | Contacts |
| WirelessDomain | Library/Databases/CellularUsage.db | Varies | Cell usage |

**Tools untuk iOS Backup Analysis:**

| Tool | Fungsi |
|------|--------|
| **iBackupBot** | Browse dan export iOS backup |
| **libimobiledevice** | Open-source iOS backup tools |
| **Autopsy** | Forensic analysis suite |
| **iPhone Backup Extractor** | GUI-based extraction |

#### Solved Problem 13 ⭐⭐

**Soal:** Investigator memiliki encrypted iTunes backup dari iPhone personel TNI. Jelaskan perbedaan data yang tersedia antara encrypted dan unencrypted backup, serta cara menganalisisnya!

**Penyelesaian:**

*Step 1:* Bandingkan konten backup

| Data | Unencrypted | Encrypted |
|------|------------|-----------|
| Foto dan Video | ✅ | ✅ |
| Kontak | ✅ | ✅ |
| SMS/iMessage | ✅ | ✅ |
| Call History | ✅ | ✅ |
| **Keychain (passwords)** | ❌ | ✅ |
| **Health Data** | ❌ | ✅ |
| **WiFi Passwords** | ❌ | ✅ |
| **Saved Passwords** | ❌ | ✅ |
| **Browser History** | Partial | ✅ Full |

*Step 2:* Cara analisis encrypted backup

Encrypted backup memerlukan password yang di-set oleh pengguna. Tanpa password, backup tidak dapat di-dekripsi. Langkah analisis:

1. Gunakan password jika diketahui
2. Jika password tidak diketahui, gunakan brute-force tools (seperti hashcat)
3. Setelah terdekripsi, gunakan iBackupBot atau Autopsy untuk browse konten

**Jawaban:** Encrypted backup mengandung data lebih lengkap termasuk keychain (passwords), health data, dan WiFi passwords yang tidak tersedia di unencrypted backup. Analisis memerlukan password dekripsi, setelah itu dapat dilakukan menggunakan iBackupBot atau Autopsy.

---

## 6. Location Data Forensics Lanjutan

### 6.1 Google Location History

Google menyimpan histori lokasi pengguna Android secara detail di server Google.

**Cara Akuisisi Google Location Data:**

1. **Google Takeout**: Download data dari takeout.google.com
2. **File Output**: `Location History.json` atau `Records.json`
3. **Format**: JSON dengan latitude, longitude, timestamp, accuracy, dan source

**Contoh Entry Location History:**

```json
{
  "timestampMs": "1706000000000",
  "latitudeE7": -72960000,
  "longitudeE7": 1127420000,
  "accuracy": 20,
  "source": "GPS",
  "deviceTag": 123456789,
  "activity": [{
    "type": "WALKING",
    "confidence": 85
  }]
}
```

### 6.2 Cell Tower Analysis

Analisis menara seluler membantu menentukan area keberadaan perangkat ketika GPS tidak aktif.

| Data | Sumber | Informasi |
|------|--------|-----------|
| **Cell ID** | Device/Provider | Identifikasi menara |
| **LAC** | Device/Provider | Location Area Code |
| **Signal Strength** | Device log | Perkiraan jarak ke menara |
| **Timing Advance** | Network log | Jarak lebih presisi |

#### Solved Problem 14 ⭐⭐⭐

**Soal:** Dalam investigasi pencurian data rahasia di Markas Besar TNI, Google Location History dari perangkat tersangka menunjukkan pergerakan yang mencurigakan. Data menunjukkan kunjungan berulang ke lokasi yang bukan bagian dari tugas resminya. Bagaimana menganalisis data ini untuk membangun timeline investigasi?

**Penyelesaian:**

*Step 1:* Akuisisi dan parsing data lokasi

```python
import json
from datetime import datetime

with open('Records.json', 'r') as f:
    data = json.load(f)

for location in data['locations']:
    lat = location['latitudeE7'] / 1e7
    lng = location['longitudeE7'] / 1e7
    ts = datetime.fromtimestamp(int(location['timestampMs']) / 1000)
    print(f"{ts} | {lat}, {lng}")
```

*Step 2:* Filter lokasi mencurigakan

Identifikasi kunjungan ke lokasi yang bukan tugas resmi dengan membandingkan titik lokasi dengan daftar lokasi tugas yang diotorisasi.

*Step 3:* Visualisasi timeline

Ekspor ke format KML untuk visualisasi di Google Earth, dengan color-coding berdasarkan waktu (jam kerja vs di luar jam kerja).

*Step 4:* Korelasi dengan bukti lain

Bandingkan waktu kunjungan ke lokasi mencurigakan dengan: log akses sistem informasi militer, catatan badge masuk gedung, dan aktivitas komunikasi (SMS, WhatsApp, email).

**Jawaban:** Analisis dilakukan dengan: (1) parsing JSON location history, (2) filtering kunjungan ke lokasi non-tugas, (3) visualisasi timeline di Google Earth, dan (4) korelasi temporal dengan log akses sistem, badge gedung, dan aktivitas komunikasi untuk membangun bukti circumstantial yang kuat.

---

## 7. Anti-Forensik pada Mobile dan IoT

### 7.1 Teknik Anti-Forensik Mobile

| Teknik | Deskripsi | Counter-Measure |
|--------|-----------|----------------|
| **Factory Reset** | Menghapus semua data pengguna | File carving pada unallocated space |
| **Secure Messaging** | Signal, disappearing messages | Server-side data, metadata analysis |
| **App Hiding** | Menyembunyikan aplikasi | Package list analysis, storage anomaly |
| **Encryption** | Mengenkripsi file/partisi | Brute-force, memory analysis |
| **Remote Wipe** | Menghapus data dari jarak jauh | Faraday bag, airplane mode |
| **GPS Spoofing** | Memalsukan lokasi GPS | Consistency check dengan cell tower |
| **VPN/Tor** | Menyembunyikan traffic | VPN app artifacts, tor cache |

#### Solved Problem 15 ⭐⭐

**Soal:** Seorang personel militer melakukan factory reset pada smartphone Android-nya sebelum penyerahan ke tim forensik. Apakah data masih bisa di-recover? Jelaskan teknik yang dapat digunakan!

**Penyelesaian:**

*Step 1:* Pahami mekanisme factory reset

Factory reset pada Android melakukan:
- Wipe `/data` partition
- Wipe `/cache` partition
- Namun TIDAK selalu melakukan secure erase (tergantung versi Android dan enkripsi)

*Step 2:* Kemungkinan recovery

| Kondisi | Kemungkinan Recovery |
|---------|---------------------|
| Android < 6.0, tanpa FDE | Tinggi (file carving pada unallocated space) |
| Android 6.0+, FDE aktif | Rendah (data terenkripsi, key dihapus) |
| Android 10+, FBE aktif | Sangat rendah (file-based encryption) |
| SD Card terpisah | Tinggi (jika SD card tidak di-wipe) |

*Step 3:* Teknik recovery

1. **File Carving**: Gunakan Autopsy/Scalpel untuk carving pada physical image
2. **JTAG/Chip-off**: Akses langsung ke memory chip
3. **Cloud Recovery**: Data yang tersinkronisasi ke Google Account
4. **SD Card Analysis**: File yang tersimpan di external storage
5. **SIM Card**: Kontak dan SMS yang tersimpan di SIM

**Jawaban:** Kemungkinan recovery tergantung pada versi Android dan status enkripsi. Pada Android lama tanpa enkripsi, file carving dapat memulihkan data. Pada Android modern dengan enkripsi, data internal sangat sulit di-recover, namun alternatif termasuk: analisis SD card, cloud recovery dari Google Account, dan SIM card analysis.

---

## 8. Tools dan Framework untuk Mobile/IoT Forensics

### 8.1 Tools Mobile Forensics

| Tool | Tipe | Platform | Fitur Utama |
|------|------|----------|-------------|
| **Autopsy** | Open Source | Windows | Analisis image, SQLite viewer, timeline |
| **ADB** | Open Source | Cross-platform | Akuisisi logical Android |
| **Cellebrite UFED** | Komersial | Dedicated device | Physical acquisition, decoding |
| **Magnet AXIOM** | Komersial | Windows | Multi-platform analysis |
| **MOBILedit** | Komersial | Windows | Phone data extraction |
| **Andriller** | Open Source | Python | Android forensic tools collection |
| **ALEAPP** | Open Source | Python | Android Logs Events and Protobuf Parser |
| **iLEAPP** | Open Source | Python | iOS Logs Events and Property Parser |

### 8.2 Framework Investigasi Mobile/IoT

**Framework NIST SP 800-101 Rev. 1 untuk Mobile Forensics:**

```
1. Preservation   → Isolasi perangkat, Faraday bag, dokumentasi
2. Acquisition    → Pilih metode akuisisi yang tepat
3. Examination    → Ekstraksi data menggunakan tools
4. Analysis       → Interpretasi dan korelasi data
5. Reporting      → Dokumentasi temuan dan kesimpulan
```

#### Solved Problem 16 ⭐

**Soal:** Jelaskan perbedaan antara tools open-source ALEAPP dan iLEAPP dalam forensik mobile!

**Penyelesaian:**

| Aspek | ALEAPP | iLEAPP |
|-------|--------|--------|
| **Platform Target** | Android | iOS |
| **Input** | Android backup, tar, folder | iTunes backup, tar, zip, folder |
| **Output** | HTML report | HTML report |
| **Parser** | Android-specific artifacts | iOS-specific artifacts |
| **Artifacts** | WhatsApp, Chrome, WiFi, calls, SMS | iMessage, Safari, Health, Locations |
| **Language** | Python | Python |

**Jawaban:** ALEAPP (Android Logs Events And Protobuf Parser) menganalisis artefak Android, sedangkan iLEAPP (iOS Logs Events And Property Parser) menganalisis artefak iOS. Keduanya open-source, ditulis dalam Python, dan menghasilkan laporan HTML.

---

## 9. Studi Kasus Terintegrasi

### 9.1 Studi Kasus: Investigasi Drone Ilegal di Zona Militer

#### Solved Problem 17 ⭐⭐⭐

**Soal:** Tim keamanan Kodam IV/Diponegoro mendeteksi drone tidak dikenal terbang di atas area penyimpanan amunisi. Drone berhasil ditembak jatuh dan diamankan bersama sebuah smartphone yang diduga milik operator di lokasi terdekat. Jelaskan prosedur forensik terintegrasi untuk investigasi kasus ini!

**Penyelesaian:**

*Step 1:* Pengamanan dan preservasi bukti

| Bukti | Tindakan |
|-------|----------|
| Drone | Foto semua sisi, lepas baterai, Faraday bag untuk komponen wireless |
| Smartphone | Aktifkan airplane mode, jangan matikan, Faraday bag |
| SD Cards | Remove dari drone dan smartphone, simpan dalam anti-static bag |
| Area | Dokumentasi lokasi penemuan, foto area |

*Step 2:* Akuisisi data drone

1. Image SD card menggunakan FTK Imager
2. Ekstrak flight log dari internal memory
3. Download foto/video dari SD card
4. Parsing flight log menggunakan DatCon

*Step 3:* Akuisisi data smartphone

1. Logical acquisition menggunakan ADB (jika Android)
2. Pull data DJI app (atau aplikasi drone lain)
3. Ekstraksi histori lokasi
4. Analisis komunikasi (SMS, WhatsApp, Telegram)

*Step 4:* Analisis korelasi

| Data Drone | Data Smartphone | Korelasi |
|-----------|----------------|----------|
| Flight path GPS | Location history | Apakah smartphone ada di lokasi take-off? |
| Paired controller MAC | Bluetooth history | Apakah smartphone pernah paired? |
| Flight timestamps | App usage | Apakah DJI app aktif saat flight? |
| Serial number | DJI account | Apakah drone registered ke akun di smartphone? |

*Step 5:* Laporan forensik

Buat laporan komprehensif yang menghubungkan drone dengan operator, termasuk flight path visualization, timeline korelasi, dan bukti digital pendukung.

**Jawaban:** Prosedur terintegrasi: (1) preservasi drone dan smartphone, (2) akuisisi SD card, flight log, dan data smartphone, (3) analisis korelasi antara flight path dengan location history, paired device dengan Bluetooth history, dan flight time dengan app usage, (4) identifikasi operator melalui DJI account registration dan bukti fisik/digital lainnya.

---

#### Solved Problem 18 ⭐⭐⭐

**Soal:** Analisis forensik pada smartphone operator drone (soal sebelumnya) menemukan database WhatsApp yang berisi percakapan dengan nomor asing. Beberapa pesan berisi koordinat GPS dan foto aerial instalasi militer. Tuliskan prosedur analisis untuk membangun bukti kasus spionase!

**Penyelesaian:**

*Step 1:* Analisis WhatsApp database

```sql
-- Identifikasi pesan dengan koordinat GPS
SELECT key_remote_jid, data, timestamp,
    datetime(timestamp/1000, 'unixepoch', 'localtime') AS waktu
FROM messages
WHERE data LIKE '%°%' OR data LIKE '%.%,%' OR data LIKE '%coordinate%'
ORDER BY timestamp;

-- Identifikasi media yang dikirim
SELECT key_remote_jid, media_url, media_mime_type,
    datetime(timestamp/1000, 'unixepoch', 'localtime') AS waktu
FROM messages
WHERE media_url IS NOT NULL AND media_mime_type LIKE 'image%'
ORDER BY timestamp;
```

*Step 2:* Analisis foto aerial

1. Ekstrak EXIF metadata dari foto yang dikirim
2. Cocokkan koordinat EXIF dengan lokasi instalasi militer
3. Verifikasi apakah foto diambil dari drone (altitude metadata)
4. Identifikasi informasi sensitif yang terlihat dalam foto

*Step 3:* Identifikasi nomor asing

1. Analisis `wa.db` untuk informasi kontak
2. Reverse lookup nomor telepon
3. Periksa pattern komunikasi (waktu, frekuensi)
4. Identifikasi apakah nomor menggunakan VPN atau layanan anonimisasi

*Step 4:* Timeline bukti spionase

Bangun timeline kronologis yang menghubungkan: penerbangan drone (flight log) → pengambilan foto aerial (EXIF) → pengiriman foto ke nomor asing (WhatsApp) → koordinat instalasi militer yang dibagikan.

**Jawaban:** Prosedur analisis: (1) query WhatsApp database untuk pesan berisi koordinat dan media foto, (2) analisis EXIF metadata foto aerial untuk memastikan diambil dari drone di atas instalasi militer, (3) identifikasi dan trace nomor asing penerima, (4) bangun timeline kronologis dari flight → capture → transmission sebagai bukti spionase.

---

#### Solved Problem 19 ⭐⭐

**Soal:** Sebuah smart lock di gudang senjata satuan marinir mencatat akses yang tidak biasa pada dini hari. Sistem CCTV smart camera di area tersebut juga mengalami downtime bersamaan. Jelaskan analisis forensik IoT untuk kasus ini!

**Penyelesaian:**

*Step 1:* Identifikasi sumber bukti IoT

| Perangkat | Data Forensik |
|-----------|--------------|
| Smart Lock | Access log, user credentials, timestamps |
| Smart Camera | Recording gap, system log, network log |
| WiFi Router | DHCP log, connection history |
| IoT Hub/Gateway | Device communication log |

*Step 2:* Analisis smart lock

- Periksa access log: siapa, kapan, metode (PIN/card/app/fingerprint)
- Identifikasi credential yang digunakan saat akses mencurigakan
- Periksa apakah ada credential baru yang ditambahkan sebelum insiden

*Step 3:* Analisis korelasi CCTV-Lock

| Waktu | Smart Lock | CCTV | Analisis |
|-------|-----------|------|----------|
| 02:45 | - | Offline | Camera dimatikan atau network disrupted |
| 02:50 | Access PIN #4 | Offline | Akses saat CCTV mati → premeditated |
| 03:15 | Exit detected | Offline | Keluar masih tanpa CCTV |
| 03:20 | - | Online | CCTV kembali aktif setelah aksi selesai |

*Step 4:* Identifikasi pelaku

- Cek siapa pemilik PIN #4 di smart lock
- Analisis network: apakah CCTV dimatikan via remote command
- Periksa log WiFi router untuk koneksi yang tidak biasa saat insiden

**Jawaban:** Analisis forensik: (1) periksa access log smart lock untuk identifikasi credential dan waktu akses, (2) analisis CCTV downtime untuk menentukan apakah camera sengaja dimatikan, (3) korelasi temporal antara akses lock dan CCTV downtime yang menunjukkan tindakan terencana, (4) identifikasi pelaku melalui credential ownership dan network log.

---

#### Solved Problem 20 ⭐⭐⭐

**Soal:** Tim BSSN mendeteksi anomali traffic pada jaringan IoT sensor cuaca di pangkalan udara militer. Sensor cuaca yang seharusnya mengirim data setiap 5 menit, terdeteksi mengirim paket data tambahan ke IP address yang tidak dikenal. Jelaskan analisis forensik untuk menentukan apakah sensor IoT telah dikompromikan!

**Penyelesaian:**

*Step 1:* Capture dan analisis traffic

```
# Filter Wireshark untuk traffic sensor
ip.src == [sensor_IP] && !ip.dst == [legitimate_server]

# Filter MQTT traffic
mqtt && ip.src == [sensor_IP]

# Analisis DNS query mencurigakan
dns.qry.name && ip.src == [sensor_IP]
```

*Step 2:* Bandingkan traffic normal vs anomali

| Parameter | Traffic Normal | Traffic Anomali |
|-----------|---------------|----------------|
| Interval | Setiap 5 menit | Tambahan di antara interval |
| Destination | Weather server IP | IP tidak dikenal |
| Payload size | ~200 bytes (data cuaca) | >1KB (data berlebih) |
| Protocol | MQTT ke broker | HTTP/HTTPS ke IP asing |

*Step 3:* Analisis firmware sensor

1. Dump firmware jika memungkinkan
2. Cari backdoor atau modifikasi pada firmware
3. Periksa apakah ada update firmware yang tidak terotorisasi
4. Analisis binary untuk hidden functionality

*Step 4:* Network forensics

- Traceroute ke IP destination yang mencurigakan
- Geolokasi IP address
- Periksa payload data yang dikirim (apakah data sensor asli atau data lain)
- Analisis timing pattern pengiriman

**Jawaban:** Analisis: (1) capture traffic dari sensor dan filter komunikasi ke IP tidak dikenal, (2) bandingkan pattern traffic normal vs anomali (interval, destination, payload size), (3) analisis firmware untuk backdoor atau modifikasi, (4) forensik jaringan untuk identifikasi destination dan konten data yang diekfiltrasi. Indikator kompromi meliputi: komunikasi ke IP asing, payload berlebih, dan interval pengiriman abnormal.

---

#### Solved Problem 21 ⭐⭐

**Soal:** Seorang prajurit TNI AU kehilangan smartphone di area publik. Perangkat dilindungi dengan screen lock dan enkripsi. Jelaskan risiko keamanan dan langkah mitigasi dari perspektif forensik!

**Penyelesaian:**

*Step 1:* Assessment risiko

| Risiko | Level | Keterangan |
|--------|-------|------------|
| Data rahasia bocor | Tinggi | Jika enkripsi berhasil dibobol |
| Akses email militer | Tinggi | Email tersinkronisasi di smartphone |
| Lokasi historis | Sedang | Riwayat pergerakan personel militer |
| Kontak militer | Sedang | Daftar kontak personel dan satuan |
| Remote wipe gagal | Tinggi | Jika perangkat dalam airplane mode/offline |

*Step 2:* Langkah mitigasi segera

1. **Remote Wipe**: Kirim perintah wipe via Find My Device (Android) atau Find My (iOS)
2. **Change Credentials**: Ganti password semua akun yang login di perangkat
3. **Revoke Tokens**: Revoke session/token untuk aplikasi militer
4. **Notify Chain of Command**: Laporkan ke rantai komando
5. **Monitor**: Pantau aktivitas anomali pada akun yang terhubung

*Step 3:* Forensik preventif untuk masa depan

Implementasikan kebijakan MDM (Mobile Device Management) yang memungkinkan remote wipe otomatis setelah beberapa kali gagal unlock, dan mandatory encryption pada semua perangkat personel militer.

**Jawaban:** Risiko utama: kebocoran data rahasia, akses email militer, dan histori lokasi. Langkah mitigasi: (1) remote wipe segera, (2) ganti semua password, (3) revoke token aplikasi militer, (4) lapor ke rantai komando, (5) monitor aktivitas akun. Langkah preventif: implementasi MDM dengan remote wipe otomatis dan mandatory encryption.

---

#### Solved Problem 22 ⭐⭐⭐

**Soal:** Tim Cyber Defense TNI sedang mengembangkan SOP forensik untuk perangkat IoT di instalasi militer. Buatlah framework komprehensif untuk IoT Forensic Readiness di lingkungan militer!

**Penyelesaian:**

*Step 1:* Inventarisasi IoT Assets

| Kategori | Perangkat | Lokasi | Criticality |
|----------|-----------|--------|------------|
| Surveillance | CCTV, motion sensor | Perimeter | Tinggi |
| Access Control | Smart lock, card reader | Entry point | Tinggi |
| Environmental | Sensor suhu, kelembaban | Server room | Sedang |
| Communication | Radio IoT, repeater | Area operasi | Tinggi |
| Vehicle | GPS tracker, telematics | Kendaraan dinas | Sedang |

*Step 2:* Logging Requirements

| Layer | Yang Di-log | Retention |
|-------|-------------|-----------|
| Device | Boot events, config changes | 180 hari |
| Network | All traffic, connection logs | 90 hari |
| Application | User actions, data access | 180 hari |
| Cloud | API calls, authentication | 365 hari |

*Step 3:* Response Procedure

```
1. Detection    → Identifikasi anomali via SIEM/monitoring
2. Isolation    → Isolasi perangkat tanpa mematikan
3. Preservation → Capture traffic, backup logs
4. Acquisition  → Image storage, dump firmware
5. Analysis     → Korelasi multi-device
6. Reporting    → Laporan forensik standar militer
```

*Step 4:* Training dan Awareness

- Pelatihan forensik IoT untuk tim Cyber Defense
- Simulasi insiden berkala
- Update SOP mengikuti perkembangan teknologi

**Jawaban:** Framework IoT Forensic Readiness meliputi: (1) inventarisasi komprehensif semua perangkat IoT dan klasifikasi criticality, (2) implementasi logging yang memadai di setiap layer (device, network, application, cloud) dengan retention policy, (3) SOP response yang jelas dari detection hingga reporting, dan (4) program training berkala untuk tim Cyber Defense.

---

## Supplementary Problems

*Tidak ada supplementary problems tambahan untuk modul ini.*

---

## Ringkasan

### Poin-Poin Kunci

1. **Forensik Mobile** mencakup akuisisi dan analisis data dari perangkat Android dan iOS, dengan berbagai level akuisisi dari manual hingga chip-off

2. **Artefak Mobile** yang penting meliputi: database SQLite (kontak, SMS, call log), data aplikasi (WhatsApp, Telegram), lokasi GPS dan EXIF metadata, browser history, dan backup files

3. **Forensik IoT** menghadapi tantangan unik karena heterogenitas perangkat, keterbatasan resource, dan beragamnya protokol komunikasi (MQTT, CoAP, Zigbee, BLE)

4. **Smart Home Forensics** mencakup analisis smart speaker, smart camera, smart lock, dan perangkat terhubung lainnya yang menyimpan pola aktivitas penghuni

5. **Drone Forensics** sangat relevan untuk militer, mencakup analisis flight log, media files, dan identifikasi operator

6. **ICS/SCADA Forensics** penting untuk keamanan infrastruktur kritis militer, melibatkan analisis protokol industri seperti Modbus dan DNP3

7. **Anti-Forensik Mobile** meliputi factory reset, secure messaging, dan encryption, yang masing-masing memiliki counter-measures

8. **IoT Forensic Readiness** memerlukan inventarisasi asset, implementasi logging, SOP response, dan training berkala

---

## Referensi

1. Easttom, C. (2021). *An In-Depth Guide to Mobile Device Forensics*. CRC Press.
2. Casey, E. (2022). *Digital Evidence and Computer Crime* (4th ed.). Academic Press.
3. Johansen, G. (2020). *Digital Forensics and Incident Response* (2nd ed.). Packt Publishing.
4. NIST SP 800-101 Rev. 1: *Guidelines on Mobile Device Forensics*.
5. Harbawi, M. & Varol, A. (2017). "An Improved Digital Evidence Acquisition Model for the Internet of Things Forensic." *IEEE Conference*.
6. Lillis, D., et al. (2016). "Current Challenges and Future Research Areas for Digital Forensic Investigation." *ADFSL*.
7. Oriwoh, E., et al. (2013). "Internet of Things Forensics: Challenges and Approaches." *IEEE Conference*.
8. Quick, D. & Choo, K.K.R. (2018). "IoT Device Forensics and Data Reduction." *IEEE Access*.

---

## License / Lisensi

*Modul ini dilisensikan di bawah [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-nc-sa/4.0/).*

**© 2025 Universitas Pertahanan RI — Program Studi Informatika, Fakultas Sains dan Teknologi Pertahanan**
