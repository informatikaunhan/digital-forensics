# Latihan Pertemuan 11: Forensik Malware

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 11  
**Topik:** Forensik Malware  
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
Perbedaan utama antara forensik malware dan analisis malware murni adalah:

A. Forensik malware menggunakan tools yang lebih canggih  
B. Forensik malware menekankan rantai penjagaan bukti dan dokumentasi untuk keperluan hukum  
C. Analisis malware tidak memerlukan sandbox  
D. Analisis malware selalu lebih cepat daripada forensik malware  
E. Forensik malware hanya dilakukan oleh militer

---

### Soal 2
Jenis malware yang mampu menyebar secara otomatis melalui jaringan TANPA memerlukan file induk dan interaksi pengguna adalah:

A. Virus  
B. Trojan  
C. Worm  
D. Adware  
E. Ransomware

---

### Soal 3
Dalam konteks ancaman militer, serangan siber yang melibatkan aktor negara, menggunakan malware khusus, dan bertahan dalam jangka panjang dikategorikan sebagai:

A. Malware komoditas  
B. Serangan script kiddie  
C. Ransomware-as-a-Service  
D. Advanced Persistent Threat (APT)  
E. Drive-by download

---

### Soal 4
Teknik analisis statis dilakukan dengan cara:

A. Mengeksekusi malware dalam mesin virtual  
B. Memeriksa properti file, string, dan impor TANPA mengeksekusi malware  
C. Menjalankan malware sambil memonitor Process Monitor  
D. Menggunakan debugger untuk single-stepping  
E. Mengirim malware ke sandbox daring

---

### Soal 5
Ketika PEStudio menunjukkan entropy 7,8 pada section .text dari sebuah file executable, interpretasi yang paling tepat adalah:

A. File executable tersebut normal dan aman  
B. File berisi banyak gambar dan resources  
C. File kemungkinan besar telah di-pack atau dienkripsi  
D. File menggunakan kompresi ZIP standar  
E. File hanya berisi teks biasa

---

### Soal 6
Tool dari Mandiant yang mampu mengekstrak string yang telah di-obfuskasi dari binary malware adalah:

A. PEStudio  
B. strings (Linux)  
C. FLOSS  
D. Wireshark  
E. FTK Imager

---

### Soal 7
Dalam struktur file PE (Portable Executable), bagian yang berisi kode program utama yang akan dieksekusi adalah:

A. .data  
B. .rsrc  
C. .rdata  
D. .text  
E. .reloc

---

### Soal 8
Fungsi API Windows yang paling mengindikasikan kemampuan injeksi proses adalah:

A. CreateFile dan ReadFile  
B. CreateRemoteThread dan VirtualAllocEx  
C. InternetOpen dan HttpSendRequest  
D. RegSetValue dan RegCreateKey  
E. CryptEncrypt dan CryptGenKey

---

### Soal 9
Langkah PERTAMA yang harus dilakukan sebelum mengeksekusi malware dalam lingkungan sandbox adalah:

A. Menjalankan pemindaian antivirus  
B. Mengambil snapshot mesin virtual  
C. Menginstal Process Monitor  
D. Mencatat hash file malware  
E. Menghubungkan VM ke internet

---

### Soal 10
Tool yang mensimulasikan layanan jaringan (DNS, HTTP, SMTP) sehingga malware mengira sudah terhubung ke internet adalah:

A. Wireshark  
B. Process Monitor  
C. FakeNet-NG  
D. Regshot  
E. Volatility

---

### Soal 11
Filter Process Monitor yang paling efektif untuk mendeteksi mekanisme persistensi malware adalah:

A. Operation is ReadFile  
B. Process Name is explorer.exe  
C. Path contains \Run  
D. Operation is TCP/IP Connect  
E. Path contains System32

---

### Soal 12
Mekanisme persistensi yang menggunakan WMI untuk trigger berbasis event disebut:

A. Registry Run Key  
B. Scheduled Task  
C. DLL Hijacking  
D. WMI Event Subscription  
E. Startup Folder

---

### Soal 13
Pola komunikasi periodik malware ke server C2 untuk memeriksa perintah baru disebut:

A. Handshaking  
B. Beaconing  
C. Tunneling  
D. Pivoting  
E. Lateral movement

---

### Soal 14
Teknik malware yang menghasilkan nama domain secara algoritmik untuk komunikasi C2 disebut:

A. DNS Poisoning  
B. DNS Tunneling  
C. Domain Generation Algorithm (DGA)  
D. DNS Spoofing  
E. Fast-flux DNS

---

### Soal 15
Indikator utama DNS tunneling yang dapat dideteksi dari log DNS adalah:

A. Query ke domain populer seperti google.com  
B. Subdomain sangat panjang dengan karakter acak  
C. Waktu respons DNS yang cepat  
D. Query hanya ke A record  
E. Volume DNS rendah dari satu host

---

### Soal 16
Plugin Volatility 3 yang secara khusus dirancang untuk mendeteksi kode injeksi dan process hollowing adalah:

A. windows.pslist  
B. windows.netscan  
C. windows.malfind  
D. windows.cmdline  
E. windows.svcscan

---

### Soal 17
Dalam analisis pohon proses, jika `svchost.exe` memiliki proses induk `explorer.exe` (bukan `services.exe`), maka ini mengindikasikan:

A. Proses svchost sedang memperbarui Windows  
B. Proses svchost yang sah  
C. Kemungkinan malware menyamar sebagai svchost  
D. Proses svchost sedang dimulai ulang  
E. Explorer.exe sedang mengalami crash

---

### Soal 18
Teknik anti-analisis di mana malware memeriksa keberadaan VMware tools atau registry key virtualisasi disebut:

A. Anti-debugging  
B. Packing  
C. Anti-VM (Anti-Virtual Machine)  
D. Obfuskasi kode  
E. Polimorfisme

---

### Soal 19
Dalam Pyramid of Pain, jenis indikator kompromi yang paling sulit diubah oleh penyerang dan paling berharga untuk pembela adalah:

A. Hash file (MD5/SHA256)  
B. Alamat IP  
C. Nama domain  
D. TTP (Taktik, Teknik, dan Prosedur)  
E. Artefak jaringan

---

### Soal 20
Teknik MITRE ATT&CK T1547.001 (Registry Run Keys) termasuk dalam taktik:

A. Initial Access (TA0001)  
B. Execution (TA0002)  
C. Persistence (TA0003)  
D. Defense Evasion (TA0005)  
E. Command and Control (TA0011)

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Jelaskan perbedaan antara forensik malware dan analisis malware! Mengapa perbedaan ini penting dalam konteks investigasi militer?

---

### Soal 2 ⭐
Sebutkan dan jelaskan lima jenis utama malware beserta karakteristik forensiknya masing-masing!

---

### Soal 3 ⭐
Jelaskan lima metode penyebaran malware dan artefak forensik yang ditinggalkan oleh masing-masing metode!

---

### Soal 4 ⭐
Apa yang dimaksud dengan analisis statis? Sebutkan kelebihan dan keterbatasannya!

---

### Soal 5 ⭐⭐
Jelaskan langkah-langkah melakukan analisis statis terhadap file PE mencurigakan beserta tools yang digunakan!

---

### Soal 6 ⭐⭐
Apa yang dimaksud dengan entropy dalam konteks analisis malware? Bagaimana entropy digunakan untuk mendeteksi packing?

---

### Soal 7 ⭐⭐
Jelaskan prosedur penyiapan sandbox yang aman untuk analisis dinamis! Mengapa isolasi jaringan sangat penting?

---

### Soal 8 ⭐⭐
Sebutkan dan jelaskan minimal empat mekanisme persistensi malware pada sistem Windows beserta cara deteksinya!

---

### Soal 9 ⭐⭐
Jelaskan perbedaan antara beaconing biasa dan beaconing dengan jitter! Mengapa jitter menyulitkan deteksi?

---

### Soal 10 ⭐⭐
Apa yang dimaksud dengan Domain Generation Algorithm (DGA)? Jelaskan cara kerja dan metode deteksinya!

---

### Soal 11 ⭐⭐
Jelaskan mengapa forensik memori sangat kritis dalam investigasi malware! Sebutkan minimal lima jenis informasi yang hanya diperoleh dari analisis memori!

---

### Soal 12 ⭐⭐⭐
Jelaskan perbedaan antara injeksi proses dan process hollowing! Sertakan API calls yang digunakan dan cara deteksi menggunakan Volatility!

---

### Soal 13 ⭐⭐⭐
Jelaskan cara kerja DNS tunneling sebagai mekanisme C2 dan bagaimana cara mendeteksinya!

---

### Soal 14 ⭐⭐⭐
Buat contoh dokumentasi IOC lengkap untuk malware hipotetis yang ditemukan di jaringan Kodam! Petakan ke kerangka kerja MITRE ATT&CK!

---

### Soal 15 ⭐⭐⭐
Seorang analis menemukan malware yang menggunakan teknik anti-VM, anti-debugging, dan packing sekaligus. Jelaskan strategi komprehensif untuk menganalisis malware tersebut!

---

## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Ransomware pada Sistem Logistik Kodam

**Latar Belakang:**

Sistem logistik Kodam IX/Udayana mengalami insiden ransomware pada Senin pagi, 12 Januari 2026. Personel menemukan sejumlah besar file pada server logistik terenkripsi dan terdapat file `DECRYPT_FILES.html` di setiap folder.

**Informasi Awal:**
- File terenkripsi memiliki ekstensi `.milcrypt` ditambahkan setelah ekstensi asli
- Ransom note berisi timer mundur 72 jam dan tuntutan pembayaran cryptocurrency
- Server logistik menjalankan Windows Server 2019
- 3 workstation Windows 10 juga terinfeksi
- Backup terakhir dilakukan 5 hari sebelumnya
- Seorang personel mengaku membuka email dengan lampiran dari "vendor logistik" pada Jumat sore
- Log event pada server menunjukkan modifikasi file massal dimulai pukul 02:15 WIB Senin
- Firewall mencatat koneksi keluar ke IP 185.xx.xx.xx pada port 443 sejak Jumat malam

**Tim Investigasi:**
- 2 investigator forensik digital dari Pusdiksus Siber
- 1 analis malware dari CSIRT TNI
- 1 penasihat hukum militer

**Pertanyaan:**

**1a.** Rekonstruksi timeline serangan dari informasi yang tersedia! Identifikasi tahap-tahap kill chain yang terdeteksi! (15 poin)

**1b.** Tentukan urutan akuisisi bukti digital berdasarkan urutan volatilitas! Mengapa urutan ini penting khususnya untuk kasus ransomware? (15 poin)

**1c.** Jelaskan langkah analisis statis yang akan dilakukan terhadap sampel ransomware! (15 poin)

**1d.** Jelaskan bagaimana analisis dinamis dalam sandbox dapat mengungkap perilaku ransomware ini! (15 poin)

**1e.** Buat rekomendasi mitigasi agar insiden serupa tidak terulang! (10 poin)

---

### Studi Kasus 2: Malware APT pada Sistem Komunikasi Lantamal

**Latar Belakang:**

Tim CSIRT TNI AL mendeteksi anomali pada jaringan Lantamal V Surabaya. Analisis awal menunjukkan indikasi APT yang sangat canggih.

**Temuan Awal:**
1. Analisis Volatility pada RAM dump workstation operator komunikasi:
   - `svchost.exe` (PID 3456) dengan proses induk `explorer.exe` (bukan `services.exe`)
   - `malfind` mendeteksi halaman memori RWX di proses `lsass.exe`
   - `netscan` menunjukkan koneksi dari PID 3456 ke IP 103.xx.xx.xx:8443
2. Analisis lalu lintas jaringan:
   - Query DNS ke domain ber-entropy tinggi setiap 5 menit ± 30 detik
   - 500+ respons NXDOMAIN dalam 24 jam terakhir
   - Beberapa query berhasil di-resolve ke IP 103.xx.xx.xx
3. Analisis statis file `svc_update.exe`:
   - Entropy keseluruhan: 7,6
   - Impor: hanya `LoadLibraryA`, `GetProcAddress`, `VirtualAlloc`
   - Waktu kompilasi: tanggal masa depan (2030)
   - String: tidak ada yang terbaca (semua terobfuskasi)
4. Analisis registry:
   - Entri baru di `HKLM\...\CurrentVersion\Run`: `SvcUpdate`
   - Scheduled task: `SystemHealthCheck` berjalan setiap 6 jam
   - Service baru: `WindowsSecurityHealth` mengarah ke `svc_update.exe`

**Pertanyaan:**

**2a.** Analisis anomali pohon proses berdasarkan temuan Volatility! Jelaskan teknik apa yang kemungkinan digunakan malware! (15 poin)

**2b.** Identifikasi pola komunikasi C2! Apakah ini beaconing dengan jitter? Apakah ada indikasi DGA? (15 poin)

**2c.** Jelaskan mengapa file `svc_update.exe` sangat mencurigakan dan langkah analisis selanjutnya! (15 poin)

**2d.** Petakan seluruh temuan ke kerangka kerja MITRE ATT&CK! (15 poin)

**2e.** Buat rencana remediasi! Jelaskan mengapa semua mekanisme persistensi harus dihapus secara SIMULTAN! (10 poin)

---

## Kunci Jawaban

### Bagian A: Pilihan Ganda

| No | Jawaban | Penjelasan |
|----|---------|------------|
| 1 | **B** | Forensik malware menekankan rantai penjagaan bukti dan dokumentasi formal untuk keperluan hukum |
| 2 | **C** | Worm bersifat mandiri dan menyebar otomatis melalui jaringan tanpa file induk |
| 3 | **D** | APT melibatkan aktor negara, malware khusus, dan bertahan jangka panjang |
| 4 | **B** | Analisis statis memeriksa properti file tanpa mengeksekusi malware |
| 5 | **C** | Entropy 7,8 sangat tinggi (skala 0-8), mengindikasikan packing atau enkripsi |
| 6 | **C** | FLOSS mampu mengekstrak string biasa maupun yang terobfuskasi |
| 7 | **D** | Section .text berisi kode program (machine code) |
| 8 | **B** | CreateRemoteThread dan VirtualAllocEx digunakan untuk menyisipkan kode ke proses lain |
| 9 | **B** | Snapshot VM harus diambil sebelum eksekusi agar bisa dikembalikan ke kondisi bersih |
| 10 | **C** | FakeNet-NG mensimulasikan layanan jaringan untuk analisis perilaku malware |
| 11 | **C** | Path contains \Run mendeteksi akses ke registry Run keys untuk persistensi |
| 12 | **D** | WMI Event Subscription menggunakan repositori WMI untuk persistensi berbasis event |
| 13 | **B** | Beaconing adalah pola komunikasi periodik malware ke server C2 |
| 14 | **C** | DGA menghasilkan nama domain secara algoritmik untuk komunikasi C2 |
| 15 | **B** | DNS tunneling ditandai subdomain panjang berisi data ter-encode (base64/hex) |
| 16 | **C** | windows.malfind dirancang khusus mendeteksi kode injeksi dan halaman memori RWX |
| 17 | **C** | svchost.exe yang sah selalu berada di bawah services.exe; induk explorer.exe = mencurigakan |
| 18 | **C** | Anti-VM mendeteksi artefak virtualisasi untuk menghindari analisis |
| 19 | **D** | TTP berada di puncak Pyramid of Pain — paling sulit diubah penyerang |
| 20 | **C** | Registry Run Keys (T1547.001) termasuk taktik Persistence (TA0003) |

---

### Bagian B: Uraian

#### Jawaban Soal 1 ⭐

| Aspek | Analisis Malware | Forensik Malware |
|-------|-----------------|------------------|
| **Tujuan** | Memahami fungsi malware | Mendukung investigasi hukum |
| **Fokus** | Reverse engineering | Rantai bukti & dokumentasi formal |
| **Keluaran** | Laporan teknis internal | Laporan forensik admissible |
| **Penanganan bukti** | Tidak ketat | Wajib mengikuti prosedur preservasi |

Dalam konteks militer, hasil forensik malware dapat digunakan sebagai bukti di tribunal militer dan mendukung atribusi serangan untuk keperluan intelijen.

---

#### Jawaban Soal 2 ⭐

1. **Virus** — Memerlukan file induk, menyebar melalui file terinfeksi. Artefak: header PE termodifikasi, timestamp file berubah.
2. **Worm** — Mandiri, menyebar otomatis via jaringan. Artefak: file exe di folder temp, lalu lintas scanning.
3. **Trojan** — Menyamar sebagai software sah (RAT, keylogger, dropper). Artefak: koneksi jaringan persisten, nama file menyesatkan.
4. **Ransomware** — Mengenkripsi file dan meminta tebusan. Artefak: ransom note, file terenkripsi, log modifikasi massal.
5. **APT** — Kampanye multi-tahap oleh aktor negara. Artefak: malware khusus, persistensi berlapis, infrastruktur C2 canggih.

---

#### Jawaban Soal 3 ⭐

| Metode | Artefak Forensik |
|--------|------------------|
| **Email phishing** | Header email, metadata lampiran, log SMTP |
| **Drive-by download** | Cache browser, riwayat unduhan |
| **USB/media lepas** | Autorun.inf, log SetupAPI, riwayat registry USB |
| **Watering hole** | Riwayat browser, log DNS |
| **Rantai pasokan** | Log instalasi, ketidakcocokan hash, anomali tanda tangan digital |

---

#### Jawaban Soal 4 ⭐

**Analisis statis** = teknik memeriksa malware TANPA mengeksekusinya.

**Kelebihan:** Aman (malware tidak dijalankan), gambaran menyeluruh kemampuan, mengidentifikasi URL/IP/string, mengungkap API calls.

**Keterbatasan:** Gagal terhadap obfuskasi/packing, tidak melihat perilaku runtime, string terenkripsi tersembunyi, malware polimorfik mengubah signature.

---

#### Jawaban Soal 5 ⭐⭐

1. **Hashing** (PEStudio/HashMyFiles) → identifikasi unik, pencarian VirusTotal
2. **Verifikasi tipe file** (file/TrID) → pastikan tipe sebenarnya vs ekstensi
3. **Analisis timestamp** (PEStudio) → periksa waktu kompilasi, anomali
4. **Tanda tangan digital** (sigcheck) → verifikasi sertifikat
5. **Analisis entropy** (PEStudio) → deteksi packing (>7 = mencurigakan)
6. **Ekstraksi string** (FLOSS) → URL, IP, registry key, perintah
7. **Analisis impor** (PEStudio) → identifikasi API mencurigakan (injeksi, kripto, jaringan)
8. **Analisis resource** (Resource Hacker) → file atau skrip yang disematkan

---

#### Jawaban Soal 6 ⭐⭐

**Entropy** mengukur keacakan data pada skala 0-8.

| Rentang | Interpretasi |
|---------|-------------|
| 0–4 | Teks/data normal |
| 4–6 | Executable normal |
| 6–7 | Terkompresi |
| **7–8** | **Terenkripsi/packed — MENCURIGAKAN** |

File packed juga biasanya hanya memiliki sedikit impor (LoadLibraryA, GetProcAddress, VirtualAlloc) karena impor asli tersembunyi di dalam data packed.

---

#### Jawaban Soal 7 ⭐⭐

**Prosedur sandbox:**
1. Instal hypervisor (VMware/VirtualBox)
2. Buat VM Windows dengan tools analisis (ProcMon, FakeNet-NG, Wireshark, Regshot)
3. Atur jaringan ke **Host-only** (bukan Bridged/NAT)
4. **Ambil snapshot** sebelum eksekusi
5. Jalankan FakeNet-NG untuk simulasi layanan jaringan
6. Eksekusi malware dan monitor perilaku
7. Kembalikan ke snapshot setelah selesai

**Isolasi penting** karena: mencegah penyebaran ke jaringan produksi, mencegah pengiriman data curian, mencegah pengunduhan payload tambahan.

---

#### Jawaban Soal 8 ⭐⭐

1. **Registry Run Keys** — `HKLM\...\Run` → eksekusi saat login. Deteksi: Autoruns, Registry Explorer.
2. **Scheduled Task** — Task Scheduler → trigger fleksibel. Deteksi: schtasks /query, Event Log ID 4698.
3. **Windows Service** — `HKLM\SYSTEM\...\Services` → eksekusi saat boot. Deteksi: sc query, Volatility svcscan.
4. **WMI Event Subscription** — Repositori WMI → trigger berbasis event. Deteksi: Get-WMIObject, Autoruns tab WMI.
5. **DLL Hijacking** — Penyalahgunaan urutan pencarian DLL. Deteksi: analisis DLL non-standar di direktori aplikasi.
6. **Startup Folder** — `%AppData%\...\Startup`. Deteksi: pemeriksaan langsung folder.

---

#### Jawaban Soal 9 ⭐⭐

**Beaconing biasa:** interval tetap dan konsisten (300s, 300s, 300s). Mudah dideteksi karena pola sangat reguler.

**Beaconing dengan jitter:** interval bervariasi (285s, 312s, 298s). Jitter menambahkan deviasi acak dari interval dasar.

**Jitter menyulitkan deteksi** karena: pola tidak lagi periodik sempurna, menyerupai lalu lintas pengguna normal, IDS berbasis pola gagal mendeteksi, memerlukan analisis statistik jangka panjang.

---

#### Jawaban Soal 10 ⭐⭐

**DGA** = malware menghasilkan ratusan nama domain secara algoritmik (hash tanggal + seed) sebagai saluran C2.

**Cara kerja:** Malware membuat ratusan domain per hari → query DNS ke semua → sebagian besar NXDOMAIN → operator hanya mendaftarkan 1-2 domain aktif.

**Deteksi:** (1) Entropy domain tinggi (karakter acak), (2) Rasio NXDOMAIN tinggi dari satu host, (3) Volume query DNS anomali, (4) Domain tidak mengandung kata bermakna.

---

#### Jawaban Soal 11 ⭐⭐

**Forensik memori kritis** karena informasi berikut HANYA ada di RAM:

1. **Kode ter-unpack** — malware packed terbuka di memori
2. **Malware tanpa file** — hanya berjalan di memori
3. **Kunci enkripsi** — kunci ransomware mungkin masih ada
4. **Kode terinjeksi** — injeksi ke proses sah hanya terlihat di memori
5. **Koneksi jaringan aktif** — koneksi C2 lengkap dengan PID
6. **Kredensial** — kata sandi dan token sesi
7. **Komunikasi terdekripsi** — data C2 yang sudah didekripsi

---

#### Jawaban Soal 12 ⭐⭐⭐

| Aspek | Injeksi Proses | Process Hollowing |
|-------|---------------|-------------------|
| **Mekanisme** | Sisipkan kode ke proses berjalan | Ganti kode proses yang di-suspend |
| **Kode asli** | Tetap ada | Di-unmap dan diganti |
| **API** | OpenProcess → VirtualAllocEx → WriteProcessMemory → CreateRemoteThread | CreateProcess(SUSPENDED) → NtUnmapViewOfSection → WriteProcessMemory → ResumeThread |

**Deteksi Volatility:** `windows.malfind` mendeteksi halaman memori RWX. Process hollowing juga teridentifikasi dari ketidakcocokan antara image di disk dan kode di memori.

---

#### Jawaban Soal 13 ⭐⭐⭐

**DNS Tunneling:** data di-encode (base64/hex) lalu disisipkan sebagai subdomain dalam query DNS. Contoh: `SGVsbG8=.data.evil-c2.com`. Server DNS penyerang mendecode subdomain dan mengirim perintah balik via respons DNS (TXT record).

**Keuntungan:** DNS biasanya diizinkan firewall dan sulit diblokir.

**Deteksi:** (1) Subdomain sangat panjang, (2) Entropy subdomain tinggi, (3) Volume query anomali, (4) TXT record berlebihan, (5) Payload respons DNS besar.

---

#### Jawaban Soal 14 ⭐⭐⭐

**Contoh IOC malware "MilRat" pada jaringan Kodam:**

| Tipe IOC | Nilai | MITRE ATT&CK |
|----------|-------|---------------|
| Hash SHA256 | `a1b2c3...` (binary) | — |
| Nama file | `svc_update.exe` | T1036.005 (Penyamaran) |
| IP C2 | `103.45.67.89` | T1071.001 (Protokol Web) |
| Domain C2 | `update-svc[.]net` | T1071.001 |
| Registry | `HKLM\...\Run\SvcUpdate` | T1547.001 (Registry Run Keys) |
| Scheduled Task | `SystemHealthCheck` | T1053.005 (Tugas Terjadwal) |
| Proses terinjeksi | `lsass.exe` (RWX) | T1055 (Injeksi Proses) |
| Pola DGA | `[a-f0-9]{12}\.net` | T1568.002 (DGA) |

---

#### Jawaban Soal 15 ⭐⭐⭐

**Strategi bertahap:**

1. **Analisis statis awal** — Hashing, identifikasi packer (Detect It Easy), coba unpack otomatis (UPX), ekstrak string (FLOSS)
2. **Lawan anti-VM** — Buat VM realistis (hapus artefak VMware, RAM >4GB, banyak file), atau gunakan mesin bare-metal terisolasi
3. **Lawan anti-debug** — Patch IsDebuggerPresent, gunakan plugin ScyllaHide, gunakan debug kernel-mode
4. **Analisis dinamis** — Jalankan dengan ProcMon + FakeNet-NG, capture perilaku runtime
5. **Forensik memori** — Dump memori saat malware berjalan → Volatility malfind → ekstrak kode ter-unpack untuk analisis lanjutan

---

### Bagian C: Studi Kasus

#### Jawaban Studi Kasus 1: Ransomware Kodam

**1a. Timeline (15 poin):**

| Waktu | Kejadian | Tahap Kill Chain |
|-------|----------|-----------------|
| Jumat sore | Personel membuka email phishing | Delivery |
| Jumat sore | Lampiran mengeksekusi dropper | Exploitation |
| Jumat malam | Koneksi keluar ke 185.xx.xx.xx:443 | C2 |
| Jumat–Minggu | Reconnaissance internal, lateral movement | Actions (persiapan) |
| Senin 02:15 | Enkripsi file massal di luar jam kerja | Actions (eksekusi) |
| Senin pagi | Personel menemukan ransom note | Deteksi |

**1b. Urutan volatilitas (15 poin):**

1. **RAM dump** server + 3 workstation — KRITIS: kunci enkripsi mungkin masih ada!
2. **Status jaringan** — koneksi C2 aktif
3. **Proses berjalan** — proses ransomware mungkin masih aktif
4. **File temporer** — ransom note, staging files
5. **Disk imaging** — forensic image server dan workstation
6. **Log jaringan** — firewall, proxy, DNS

Khusus ransomware: jika sistem di-reboot, kunci enkripsi hilang dari memori dan file tidak dapat didekripsi.

**1c. Analisis statis (15 poin):**

1. Hashing → pencarian VirusTotal untuk identifikasi keluarga ransomware
2. PEStudio → entropy, impor (cari API kripto: CryptEncrypt, CryptGenKey)
3. FLOSS → ekstrak string (template ransom note, URL C2, alamat cryptocurrency, daftar ekstensi target)
4. Analisis struktur PE → waktu kompilasi, tanda tangan digital
5. Analisis resource → ransom note tersematkan, kunci publik

**1d. Analisis dinamis (15 poin):**

Jalankan dalam sandbox terisolasi dengan ProcMon + FakeNet-NG:
- **Sistem file:** pola enkripsi (file mana duluan, ukuran batch)
- **Registry:** mekanisme persistensi yang dibuat
- **Jaringan:** komunikasi C2, pertukaran kunci
- **Proses:** proses anak, eksekusi PowerShell
- **Penghapusan backup:** perintah `vssadmin delete shadows`

**1e. Rekomendasi mitigasi (10 poin):**

1. Backup 3-2-1 (3 salinan, 2 media, 1 offline/air-gapped)
2. Filter email dengan sandboxing lampiran
3. Segmentasi jaringan logistik dari jaringan umum
4. Pelatihan anti-phishing reguler untuk seluruh personel
5. Endpoint protection dengan kemampuan deteksi ransomware
6. SOP penanganan insiden ransomware (termasuk JANGAN bayar tebusan)

---

#### Jawaban Studi Kasus 2: APT Lantamal

**2a. Anomali pohon proses (15 poin):**

- **svchost.exe dengan induk explorer.exe** → svchost sah SELALU di bawah services.exe. Ini penyamaran malware (T1036.005).
- **Halaman RWX di lsass.exe** → kode terinjeksi di proses keamanan kritis. Kemungkinan injeksi proses (T1055) untuk pencurian kredensial.
- **Koneksi PID 3456 ke IP 103.xx.xx.xx:8443** → komunikasi C2 dari proses palsu via HTTPS alternatif.

**2b. Pola C2 (15 poin):**

- **Beaconing dengan jitter: YA** — interval 5 menit ± 30 detik = jitter ~10%. Tidak periodik sempurna.
- **DGA: YA** — 500+ NXDOMAIN, domain ber-entropy tinggi, hanya beberapa yang berhasil resolve. Pola klasik DGA: generate banyak → query semua → hanya 1-2 aktif.

**2c. Analisis svc_update.exe (15 poin):**

Sangat mencurigakan karena: (1) entropy 7,6 = packed, (2) hanya 3 impor = impor asli tersembunyi, (3) waktu kompilasi masa depan = timestomping, (4) tidak ada string terbaca = terobfuskasi total, (5) 15 indikator mencurigakan PEStudio.

**Langkah selanjutnya:** Identifikasi packer → coba unpack → jika gagal, analisis dinamis di sandbox anti-VM → dump memori saat runtime → analisis kode ter-unpack.

**2d. Pemetaan MITRE ATT&CK (15 poin):**

| Temuan | Taktik | Teknik |
|--------|--------|--------|
| Penyamaran sebagai svchost | Defense Evasion | T1036.005 Penyamaran |
| Binary packed (entropy 7,6) | Defense Evasion | T1027.002 Software Packing |
| Timestamp masa depan | Defense Evasion | T1070.006 Timestomping |
| Registry Run key | Persistence | T1547.001 Registry Run Keys |
| Scheduled Task | Persistence | T1053.005 Tugas Terjadwal |
| Windows Service | Persistence | T1543.003 Layanan Windows |
| Injeksi ke lsass.exe | Credential Access | T1003.001 Memori LSASS |
| C2 HTTPS port 8443 | C2 | T1071.001 Protokol Web |
| Domain DGA | C2 | T1568.002 DGA |

**2e. Rencana remediasi (10 poin):**

**Mengapa harus simultan:** jika hanya satu persistensi dihapus, malware akan memulihkan diri dari mekanisme lain yang masih aktif (misal: hapus registry → scheduled task masih jalan → malware buat ulang registry).

**Langkah simultan:**
1. Blokir IP dan domain C2 di firewall
2. Hapus serentak: registry entry + scheduled task + service
3. Terminasi proses malicious
4. Hapus binary malware
5. Reset semua kredensial (karena lsass diinjeksi)
6. Verifikasi tidak ada persistensi tersisa
7. Monitoring lalu lintas untuk beaconing residual

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
