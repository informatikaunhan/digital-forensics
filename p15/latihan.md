# Latihan Pertemuan 15: Forensik Mobile dan Internet of Things (IoT)

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 15  
**Topik:** Forensik Mobile dan Internet of Things (IoT)  
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
Partisi Android yang menjadi fokus utama dalam investigasi forensik karena menyimpan data pengguna dan aplikasi adalah:

A. /boot  
B. /system  
C. /data  
D. /cache  
E. /recovery

---

### Soal 2
Database SMS pada perangkat Android tersimpan dalam file:

A. contacts2.db  
B. calllog.db  
C. mmssms.db  
D. msgstore.db  
E. telephony.db

---

### Soal 3
Fitur keamanan iOS yang menyimpan encryption key pada prosesor terpisah sehingga tidak dapat diekstrak melalui software adalah:

A. Full Disk Encryption  
B. USB Restricted Mode  
C. Secure Boot Chain  
D. Secure Enclave  
E. Data Protection API

---

### Soal 4
Metode akuisisi mobile yang menghasilkan salinan bit-by-bit dari seluruh storage perangkat termasuk unallocated space disebut:

A. Manual acquisition  
B. Logical acquisition  
C. File system acquisition  
D. Physical acquisition  
E. Cloud acquisition

---

### Soal 5
Perintah ADB yang digunakan untuk membuat backup lengkap perangkat Android adalah:

A. `adb pull /data`  
B. `adb backup -all -shared`  
C. `adb dump -full`  
D. `adb copy -all`  
E. `adb extract -complete`

---

### Soal 6
Kolom `type` pada database SMS Android yang bernilai 2 menunjukkan bahwa pesan tersebut:

A. Diterima (received)  
B. Dikirim (sent)  
C. Draft  
D. Gagal kirim (failed)  
E. Diteruskan (forwarded)

---

### Soal 7
Fitur USB Restricted Mode pada iOS akan menonaktifkan port USB setelah perangkat terkunci selama:

A. 15 menit  
B. 30 menit  
C. 1 jam  
D. 6 jam  
E. 24 jam

---

### Soal 8
Database WhatsApp pada perangkat Android tersimpan di lokasi:

A. `/data/data/com.whatsapp/files/msgstore.db`  
B. `/data/data/com.whatsapp/databases/msgstore.db`  
C. `/sdcard/WhatsApp/msgstore.db`  
D. `/system/app/whatsapp/databases/msgstore.db`  
E. `/data/app/com.whatsapp/msgstore.db`

---

### Soal 9
Protokol IoT yang menggunakan model publish-subscribe melalui broker adalah:

A. CoAP  
B. HTTP  
C. MQTT  
D. Modbus  
E. BLE

---

### Soal 10
Port standar yang digunakan oleh protokol MQTT adalah:

A. 80  
B. 443  
C. 502  
D. 1883  
E. 5683

---

### Soal 11
Protokol industri yang berjalan pada port 502 dan digunakan dalam sistem SCADA adalah:

A. DNP3  
B. Modbus TCP  
C. OPC UA  
D. BACnet  
E. MQTT

---

### Soal 12
Tool open-source yang digunakan untuk mem-parsing flight log drone DJI adalah:

A. Autopsy  
B. Wireshark  
C. DatCon  
D. FTK Imager  
E. Volatility

---

### Soal 13
Langkah pertama yang harus dilakukan saat mengamankan drone tidak dikenal di zona militer adalah:

A. Menyalakan drone untuk mengecek flight log  
B. Menghubungkan ke komputer forensik  
C. Melepas baterai dan menyimpan di Faraday bag  
D. Melakukan factory reset  
E. Menerbangkan kembali untuk tes

---

### Soal 14
Sumber data lokasi pada perangkat mobile yang memiliki akurasi tertinggi (3-5 meter) adalah:

A. Cell Tower triangulation  
B. WiFi positioning  
C. GPS  
D. Bluetooth proximity  
E. IP geolocation

---

### Soal 15
ALEAPP adalah tool open-source forensik yang digunakan untuk menganalisis artefak dari platform:

A. iOS  
B. Android  
C. Windows Phone  
D. BlackBerry  
E. Semua platform mobile

---

### Soal 16
Encrypted iTunes backup mengandung data tambahan yang tidak tersedia pada unencrypted backup, KECUALI:

A. Keychain (passwords)  
B. Health Data  
C. WiFi Passwords  
D. Foto dan video  
E. Saved browser passwords

---

### Soal 17
Metode akuisisi mobile level tertinggi yang memerlukan proses desolder chip memori dari perangkat disebut:

A. JTAG  
B. Physical acquisition  
C. Chip-off  
D. Root extraction  
E. Logical dump

---

### Soal 18
Data forensik dari smartwatch yang dapat membantu menentukan kondisi fisik pengguna pada waktu tertentu adalah:

A. GPS tracks  
B. Step count  
C. Heart rate dan SpO2  
D. Notifikasi  
E. Paired devices

---

### Soal 19
Fungsi Faraday bag dalam forensik mobile adalah:

A. Menjaga suhu perangkat  
B. Mencegah kerusakan fisik  
C. Memblokir sinyal wireless untuk mencegah remote wipe  
D. Mempercepat proses akuisisi  
E. Mengenkripsi data perangkat

---

### Soal 20
Standar NIST yang menjadi panduan utama untuk forensik perangkat mobile adalah:

A. NIST SP 800-86  
B. NIST SP 800-88  
C. NIST SP 800-101  
D. NIST SP 800-144  
E. NIST SP 800-61

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Jelaskan perbedaan arsitektur keamanan antara Android dan iOS yang mempengaruhi proses forensik!

**Jawaban:**

Android bersifat open-source dengan opsi rooting yang relatif mudah, memungkinkan akses file system secara langsung. iOS bersifat closed-source dengan arsitektur keamanan berlapis: Secure Enclave menyimpan encryption key di hardware terpisah, Full Disk Encryption mengenkripsi seluruh data berbasis passcode, dan USB Restricted Mode menonaktifkan USB setelah 1 jam terkunci. Akibatnya, akuisisi fisik pada iOS jauh lebih sulit dibanding Android.

---

### Soal 2 ⭐
Sebutkan dan jelaskan lima level akuisisi data pada forensik mobile!

**Jawaban:**

1. **Manual (L1):** Interaksi langsung dengan UI perangkat, menghasilkan screenshot dan konten layar
2. **Logical (L2):** Backup standar OS menggunakan ADB/iTunes, menghasilkan file user dan database
3. **File System (L3):** Akses file system lengkap via root/jailbreak, termasuk deleted files
4. **Physical (L4):** Bit-by-bit image seluruh storage, termasuk unallocated space
5. **Chip-off (L5):** Desolder chip memori dari perangkat, menghasilkan raw NAND/eMMC data

---

### Soal 3 ⭐
Jelaskan langkah-langkah akuisisi logical perangkat Android menggunakan ADB!

**Jawaban:**

1. Aktifkan mode pesawat untuk mencegah remote wipe
2. Pastikan baterai minimal 50% dan dokumentasi kondisi awal
3. Aktifkan USB Debugging via Developer Options
4. Hubungkan via USB, verifikasi dengan `adb devices`
5. Jalankan `adb backup -all -shared -apk` untuk full backup
6. Pull data spesifik (WhatsApp, DCIM, Download)
7. Hitung hash SHA-256 untuk verifikasi integritas
8. Dokumentasi chain of custody

---

### Soal 4 ⭐
Tuliskan query SQL untuk menampilkan semua pesan SMS yang diterima (type = 1) beserta waktu dalam format tanggal yang mudah dibaca!

**Jawaban:**

```sql
SELECT address AS pengirim,
    body AS isi_pesan,
    datetime(date/1000, 'unixepoch', 'localtime') AS waktu,
    read AS status_baca
FROM sms
WHERE type = 1
ORDER BY date DESC;
```

---

### Soal 5 ⭐⭐
Jelaskan perbedaan data yang tersedia antara encrypted dan unencrypted iTunes backup!

**Jawaban:**

Unencrypted backup mengandung data dasar: foto, kontak, SMS, dan call history. Encrypted backup mengandung semua data tersebut ditambah: Keychain (semua password tersimpan), Health Data, WiFi passwords, saved browser passwords, dan browser history lengkap. Encrypted backup memerlukan password yang di-set pengguna untuk dekripsi.

---

### Soal 6 ⭐⭐
Sebutkan empat sumber data lokasi pada perangkat mobile beserta tingkat akurasinya!

**Jawaban:**

1. **GPS:** 3-5 meter, dari sinyal satelit
2. **Cell Tower:** 100-300 meter, dari triangulasi menara seluler
3. **WiFi:** 10-30 meter, dari posisi WiFi access point
4. **Bluetooth:** 1-3 meter, dari proximity BLE beacon

---

### Soal 7 ⭐⭐
Jelaskan perbedaan antara protokol MQTT dan CoAP dalam konteks forensik IoT!

**Jawaban:**

MQTT menggunakan TCP (port 1883) dengan model publish-subscribe melalui broker, sehingga broker menyimpan log yang menjadi sumber bukti utama. CoAP menggunakan UDP (port 5683) dengan model request-response langsung antar perangkat tanpa broker, sehingga memerlukan network capture di titik yang tepat untuk mendapatkan bukti forensik.

---

### Soal 8 ⭐⭐
Sebutkan lima tantangan utama dalam forensik IoT!

**Jawaban:**

1. **Heterogenitas:** Beragam perangkat, OS, dan protokol yang berbeda
2. **Resource terbatas:** CPU, RAM, dan storage minimal pada perangkat IoT
3. **Volatilitas data:** Data sering di-overwrite karena storage terbatas
4. **Akses fisik sulit:** Perangkat tertanam atau sulit dijangkau
5. **Cloud dependency:** Data tersimpan di cloud vendor, memerlukan koordinasi akses

---

### Soal 9 ⭐⭐
Jelaskan prosedur forensik untuk mengamankan dan menginvestigasi drone tidak dikenal yang ditemukan di zona militer!

**Jawaban:**

1. **Preservasi:** Jangan nyalakan drone, lepas baterai untuk mencegah remote wipe, dokumentasi foto semua sisi, catat serial number, simpan di Faraday bag
2. **Akuisisi:** Image SD card menggunakan FTK Imager, ekstrak flight log dari internal memory, download foto/video dari SD card
3. **Analisis:** Parsing flight log menggunakan DatCon, analisis EXIF metadata foto untuk koordinat dan waktu, visualisasi rute di Google Earth (KML)
4. **Identifikasi:** Cek DJI account registration, analisis paired devices, korelasi dengan smartphone operator jika tersedia

---

### Soal 10 ⭐⭐
Jelaskan komponen utama sistem ICS/SCADA dan data forensik yang tersedia dari masing-masing komponen!

**Jawaban:**

1. **HMI (Human-Machine Interface):** Operator logs dan alarm history
2. **PLC (Programmable Logic Controller):** Program logic dan configuration changes
3. **RTU (Remote Terminal Unit):** Sensor data dan communication logs
4. **Historian:** Time-series process data untuk analisis anomali

---

### Soal 11 ⭐⭐
Jelaskan bagaimana data wearable device (smartwatch) dapat membantu rekonstruksi timeline kejadian dalam investigasi militer!

**Jawaban:**

Data wearable membantu merekonstruksi kejadian melalui: heart rate pattern untuk menentukan kapan kondisi fisik mulai berubah, GPS track untuk lokasi tepat kejadian, step count dan activity pattern untuk rekonstruksi pergerakan, fall detection untuk menentukan waktu collapse, dan SpO2 untuk mendeteksi perubahan kondisi kesehatan. Korelasi data-data ini menghasilkan timeline kronologis yang akurat.

---

### Soal 12 ⭐⭐⭐
Apa perbedaan antara tool ALEAPP dan iLEAPP? Jelaskan fungsi masing-masing!

**Jawaban:**

ALEAPP (Android Logs Events And Protobuf Parser) menganalisis artefak Android dari backup, tar, atau folder, menghasilkan laporan HTML dari artifacts seperti WhatsApp, Chrome, WiFi, calls, dan SMS. iLEAPP (iOS Logs Events And Property Parser) menganalisis artefak iOS dari iTunes backup, tar, zip, atau folder, menghasilkan laporan HTML dari artifacts seperti iMessage, Safari, Health, dan Locations. Keduanya open-source dan ditulis dalam Python.

---

### Soal 13 ⭐⭐⭐
Jelaskan mengapa Faraday bag penting dalam forensik mobile dan kapan harus digunakan!

**Jawaban:**

Faraday bag memblokir semua sinyal RF (seluler, WiFi, Bluetooth) sehingga mencegah: remote wipe yang dapat menghapus seluruh data perangkat, komunikasi masuk yang mengubah state perangkat, dan sinkronisasi cloud yang memodifikasi data. Faraday bag harus digunakan segera setelah perangkat diamankan, sebelum proses akuisisi dimulai, terutama jika perangkat masih menyala.

---

### Soal 14 ⭐⭐⭐
Jelaskan kemungkinan recovery data setelah factory reset pada perangkat Android dengan berbagai kondisi enkripsi!

**Jawaban:**

- **Android < 6.0 tanpa FDE:** Kemungkinan tinggi, file carving pada unallocated space efektif
- **Android 6.0+ dengan FDE:** Kemungkinan rendah, data terenkripsi dan key dihapus saat reset
- **Android 10+ dengan FBE:** Kemungkinan sangat rendah karena file-based encryption
- **SD Card terpisah:** Kemungkinan tinggi jika SD card tidak di-wipe secara terpisah
- **Alternatif:** Cloud recovery dari Google Account, SIM card analysis, JTAG/chip-off

---

### Soal 15 ⭐⭐⭐
Jelaskan framework NIST SP 800-101 untuk forensik perangkat mobile beserta aktivitas pada setiap tahapannya!

**Jawaban:**

1. **Preservation:** Isolasi perangkat dengan Faraday bag, aktifkan mode pesawat, dokumentasi kondisi awal, pertahankan daya perangkat
2. **Acquisition:** Pilih metode akuisisi yang tepat (L1-L5) sesuai kebutuhan dan kondisi perangkat, lakukan imaging, hitung hash
3. **Examination:** Ekstraksi data menggunakan tools forensik (Autopsy, ALEAPP/iLEAPP), identifikasi artefak relevan
4. **Analysis:** Interpretasi data, korelasi antar artefak, bangun timeline kejadian, identifikasi bukti yang mendukung hipotesis
5. **Reporting:** Dokumentasi temuan, kesimpulan, dan rekomendasi dalam format laporan forensik standar

---


## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Investigasi Kebocoran Informasi via Mobile

#### Latar Belakang

Tim Cyber Intelligence TNI AD mendeteksi adanya kebocoran informasi terkait rencana latihan gabungan militer. Analisis awal menunjukkan bahwa informasi tersebut disebarkan melalui aplikasi pesan pada smartphone. Seorang perwira yang memiliki akses ke informasi rahasia menjadi tersangka utama.

Perangkat yang diamankan:
- Smartphone Android Samsung Galaxy S23 (Android 14, menyala, tidak di-lock)
- Smartwatch Samsung Galaxy Watch 5 (terhubung ke smartphone)
- Laptop yang tersinkronisasi dengan smartphone

#### Pertanyaan

**1a. Prosedur Pengamanan dan Preservasi (10 poin)**

Jelaskan langkah-langkah pengamanan dan preservasi yang harus dilakukan terhadap ketiga perangkat tersebut!

**1b. Strategi Akuisisi Data (10 poin)**

Jelaskan strategi akuisisi data untuk smartphone Android yang sudah dalam kondisi tidak di-lock!

**1c. Analisis Artefak Komunikasi (15 poin)**

Investigator berhasil mengekstrak database WhatsApp (msgstore.db) dan Telegram (cache4.db). Jelaskan langkah analisis untuk mengidentifikasi komunikasi yang berisi kebocoran informasi!

**1d. Pemanfaatan Data Smartwatch (15 poin)**

Bagaimana data dari Samsung Galaxy Watch 5 dapat mendukung investigasi? Jelaskan data apa saja yang relevan dan bagaimana menganalisisnya!


---

### Studi Kasus 2: Serangan pada Infrastruktur IoT Militer

#### Latar Belakang

Pangkalan udara militer TNI AU mengalami serangkaian insiden keamanan dalam satu minggu:
- Hari 1: Smart camera perimeter berhenti merekam selama 2 jam pada malam hari
- Hari 3: Sensor cuaca IoT mengirim data anomali ke IP address tidak dikenal
- Hari 5: Smart lock gudang peralatan mencatat akses tidak sah pada dini hari
- Hari 6: Drone tidak dikenal terdeteksi terbang di atas area runway

Tim BSSN diminta melakukan investigasi forensik terintegrasi.

#### Pertanyaan

**2a. Pemetaan Bukti Digital IoT (10 poin)**

Buatlah pemetaan sumber bukti digital dari setiap perangkat IoT yang terlibat dalam insiden!

**2b. Analisis Insiden Smart Camera (10 poin)**

Jelaskan langkah forensik untuk menentukan apakah smart camera sengaja dimatikan atau mengalami gangguan teknis!

**2c. Investigasi Sensor Cuaca (10 poin)**

Sensor cuaca mengirim data tambahan ke IP tidak dikenal. Jelaskan analisis forensik untuk menentukan apakah sensor telah dikompromikan!

**2d. Korelasi Antar-Insiden (10 poin)**

Jelaskan bagaimana keempat insiden tersebut dapat saling terkait dan bagaimana membangun korelasi temporal antar-insiden!

**2e. Rekomendasi IoT Forensic Readiness (10 poin)**

Berdasarkan hasil investigasi, buatlah rekomendasi IoT Forensic Readiness untuk pangkalan udara tersebut!


## Kunci Jawaban

### Bagian A: Pilihan Ganda


| No | Jawaban | Penjelasan Singkat |
|----|---------|-------------------|
| 1 | **C** | Partisi `/data` menyimpan semua data pengguna dan aplikasi |
| 2 | **C** | SMS tersimpan di `mmssms.db` pada provider telephony |
| 3 | **D** | Secure Enclave adalah prosesor terpisah untuk menyimpan encryption key |
| 4 | **D** | Physical acquisition menghasilkan bit-by-bit image termasuk unallocated space |
| 5 | **B** | `adb backup -all -shared` membuat backup lengkap termasuk shared storage |
| 6 | **B** | Type 2 = Sent, Type 1 = Received, Type 3 = Draft |
| 7 | **C** | USB Restricted Mode aktif setelah 1 jam perangkat terkunci |
| 8 | **B** | WhatsApp menyimpan database di `/data/data/com.whatsapp/databases/` |
| 9 | **C** | MQTT menggunakan model publish-subscribe melalui broker |
| 10 | **D** | MQTT menggunakan port 1883 (unencrypted) dan 8883 (TLS) |
| 11 | **B** | Modbus TCP berjalan pada port 502 untuk industrial automation |
| 12 | **C** | DatCon digunakan untuk parsing flight log DJI (.DAT) |
| 13 | **C** | Lepas baterai dan Faraday bag untuk mencegah remote wipe dan modifikasi |
| 14 | **C** | GPS memberikan akurasi tertinggi yaitu 3-5 meter |
| 15 | **B** | ALEAPP (Android Logs Events And Protobuf Parser) khusus Android |
| 16 | **D** | Foto dan video tersedia di kedua tipe backup |
| 17 | **C** | Chip-off memerlukan desolder chip memori dari circuit board |
| 18 | **C** | Heart rate dan SpO2 menunjukkan kondisi fisik pada waktu tertentu |
| 19 | **C** | Faraday bag memblokir sinyal RF untuk mencegah remote wipe |
| 20 | **C** | NIST SP 800-101 Rev. 1: Guidelines on Mobile Device Forensics |

---


### Bagian B: Uraian

> *Kunci jawaban uraian menggunakan rubrik penilaian berdasarkan kelengkapan dan kedalaman jawaban.*

### Bagian C: Studi Kasus

#### Studi Kasus 1: Investigasi Kebocoran Informasi via Mobile



1. **Smartphone:** Aktifkan mode pesawat segera, masukkan ke Faraday bag, dokumentasi kondisi layar (screenshot/foto), catat model, IMEI, dan versi OS, pastikan baterai mencukupi atau hubungkan charger
2. **Smartwatch:** Aktifkan mode pesawat atau matikan Bluetooth/WiFi, simpan di Faraday bag terpisah, dokumentasi kondisi dan informasi perangkat
3. **Laptop:** Jangan matikan jika menyala (preservasi RAM), foto kondisi layar, dokumentasi aplikasi yang berjalan, catat koneksi jaringan aktif
4. **Umum:** Dokumentasi chain of custody untuk semua perangkat, foto semua perangkat dari berbagai sudut, catat waktu dan lokasi pengamanan

---



1. **Logical acquisition via ADB:**
   - Aktifkan USB Debugging (Settings → Developer Options)
   - `adb devices` untuk verifikasi koneksi
   - `adb backup -all -shared -apk -system` untuk full backup
   - Pull data kritis: `adb pull /sdcard/WhatsApp/`, `adb pull /sdcard/DCIM/`
2. **Cloud data:**
   - Cek Google Account yang login, screenshot daftar akun
   - Akses Google Takeout untuk location history jika diotorisasi
3. **App-specific:**
   - List semua packages: `adb shell pm list packages`
   - Screenshot aplikasi pesan yang terbuka
4. **Hash verification:** Hitung SHA-256 untuk setiap file hasil akuisisi

---



1. **Analisis WhatsApp (msgstore.db):**
   - Buka dengan DB Browser for SQLite
   - Query pesan keluar dengan kata kunci terkait operasi militer:
     ```sql
     SELECT key_remote_jid, data,
       datetime(timestamp/1000, 'unixepoch', 'localtime') AS waktu
     FROM messages
     WHERE data LIKE '%latihan%' OR data LIKE '%operasi%' OR data LIKE '%rahasia%'
     ORDER BY timestamp;
     ```
   - Identifikasi media yang dikirim (foto dokumen rahasia):
     ```sql
     SELECT key_remote_jid, media_mime_type, media_url, timestamp
     FROM messages WHERE media_url IS NOT NULL;
     ```
   - Analisis frekuensi komunikasi per kontak untuk mendeteksi pola anomali

2. **Analisis Telegram (cache4.db):**
   - Periksa tabel `messages` dan `users` untuk identifikasi lawan bicara
   - Cari pesan dengan konten sensitif menggunakan keyword search
   - Identifikasi secret chats dan self-destructing messages (jika ada sisa artifacts)

3. **Korelasi antar platform:** Bandingkan timeline komunikasi WhatsApp dan Telegram untuk mendeteksi pola pengiriman informasi terkoordinasi

---



1. **Data lokasi GPS:** Rekonstruksi pergerakan tersangka, identifikasi kunjungan ke lokasi mencurigakan di luar tugas resmi (misalnya bertemu pihak ketiga)
2. **Notifikasi mirror:** Smartwatch menyimpan salinan notifikasi dari smartphone termasuk preview pesan WhatsApp/Telegram yang masuk, memberikan bukti tambahan komunikasi
3. **Timeline aktivitas:** Heart rate dan step count untuk memverifikasi alibi tersangka, apakah sesuai dengan klaim aktivitas pada waktu tertentu
4. **Koneksi Bluetooth:** Log paired devices menunjukkan kapan smartwatch terhubung/terputus dari smartphone, mengkonfirmasi keberadaan smartphone di dekat tersangka
5. **Cara analisis:**
   - Akuisisi melalui Samsung Health app di smartphone
   - Backup data watch via Samsung Smart Switch
   - Analisis database SQLite dari backup untuk location, health, dan notification data
   - Korelasi temporal dengan artefak smartphone

---


---

#### Studi Kasus 2: Serangan pada Infrastruktur IoT Militer



| Perangkat | Sumber Bukti | Data Forensik |
|-----------|-------------|---------------|
| **Smart Camera** | SD card, cloud platform, system log | Gap rekaman, shutdown events, login history |
| **Sensor Cuaca** | Firmware, network traffic, MQTT broker | Traffic anomali, destination IP, payload data |
| **Smart Lock** | Device log, app data, gateway log | Access log, credential used, timestamps |
| **Drone** | SD card, flight controller, remote app | Flight log, foto/video, GPS track, paired devices |
| **Router/Switch** | DHCP log, firewall log, NetFlow | Connection history, anomalous traffic patterns |
| **MQTT Broker** | Subscribe/publish log | Device komunikasi, topic patterns |

---



1. **Preservasi:** Jangan restart camera, capture network traffic aktif, backup SD card
2. **Analisis SD card:** Periksa gap dalam timeline rekaman video, identifikasi file terakhir sebelum dan pertama setelah gap
3. **System log camera:** Cari shutdown/reboot events pada log perangkat, identifikasi apakah ada shutdown command atau crash
4. **Router log:** Periksa koneksi camera ke jaringan selama periode gap, apakah device disconnect atau tetap connected
5. **Kesimpulan:**
   - Jika ada shutdown command di log → sengaja dimatikan
   - Jika koneksi aktif tapi tidak merekam → manipulasi software
   - Jika seluruh koneksi terputus → gangguan power/jaringan

---



1. **Network capture:** Filter traffic dari IP sensor ke destination selain weather server resmi menggunakan Wireshark
2. **Bandingkan traffic normal vs anomali:**
   - Normal: interval 5 menit, ~200 bytes payload, ke server resmi
   - Anomali: interval tidak teratur, >1KB payload, ke IP asing
3. **Analisis payload:** Periksa apakah data yang dikirim ke IP asing adalah data sensor asli atau data lain (kemungkinan exfiltration)
4. **Firmware analysis:** Dump firmware sensor jika memungkinkan, cari backdoor atau modifikasi tidak terotorisasi
5. **Geolokasi IP:** Identifikasi lokasi dan pemilik IP destination yang mencurigakan

---



1. **Pola serangan terkoordinasi:**
   - Hari 1: Camera dimatikan → menghilangkan surveillance (reconnaissance/preparation)
   - Hari 3: Sensor dikompromikan → exfiltration data atau command channel
   - Hari 5: Akses smart lock → penetrasi fisik saat surveillance down
   - Hari 6: Drone → pengintaian lanjutan setelah penetrasi berhasil

2. **Korelasi temporal:**
   - Bandingkan timestamp semua insiden untuk menemukan overlap atau sequential pattern
   - Periksa apakah IP destination sensor cuaca terkait dengan controller drone
   - Cek apakah credential smart lock yang digunakan diregistrasi sebelum insiden camera
   - Analisis apakah ada common access point (misalnya WiFi network yang sama digunakan untuk mengakses camera dan lock)

3. **Indikasi APT:** Pola eskalasi dari reconnaissance (camera off) → infrastructure compromise (sensor) → physical access (lock) → surveillance (drone) menunjukkan serangan terencana yang kemungkinan dilakukan oleh aktor yang sama.

---



1. **Inventarisasi dan segmentasi:** Katalog semua perangkat IoT, segmentasi jaringan IoT dari jaringan operasional utama dengan VLAN terpisah
2. **Logging terpusat:** Implementasi SIEM untuk mengumpulkan log dari semua perangkat IoT, retensi minimal 180 hari
3. **Monitoring real-time:** Deteksi anomali traffic IoT secara otomatis, alert untuk komunikasi ke IP tidak dikenal
4. **Hardening perangkat:** Update firmware berkala, ganti credential default, nonaktifkan fitur yang tidak digunakan
5. **SOP incident response:** Prosedur jelas untuk preservasi bukti IoT, tim response terlatih, simulasi insiden berkala
6. **Redundansi surveillance:** Backup recording terpisah dari kamera utama, multiple camera coverage untuk area kritis

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
