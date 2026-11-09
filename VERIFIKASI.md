# Verifikasi modul LaTeX dan kode pertemuan 9

## Status terbaru, 6 November 2026

PASS: ekstensi terpasang dengan verifikasi aktif, build IDE dari ZIP baru berhasil, lisensi aktif, dan simulator VS Code berjalan. Tombol browser, tiga masukan DHT22, serta sepuluh pemeriksaan HTTP port 8180 lulus. Firmware tidak diubah dan tidak memakai task loopback uji. Lihat bukti/vscode_workflow_result.json, http_forward_result.json, serta screenshot dan log serial. Simulator dihentikan sesudah uji.

## Status sebelumnya, 2 November 2026

Pengujian port masuk VS Code masih BLOCKED: pemasangan ekstensi ditolak karena verifikasi tanda tangan Untrusted. Pemeriksaan tanda tangan tetap aktif. Lihat bukti/vscode_port_result.json.

Bagian berikut menyimpan riwayat pengujian sebelumnya.

Tanggal: 29 Oktober 2026.

## Dokumen

- PASS: PDF 16 halaman berasal dari pdfLaTeX, dua putaran kompilasi; daftar isi dan bookmark tersedia.
- PASS: tidak ada error, referensi tak terdefinisi, peringatan LaTeX/paket, atau overfull box pada log akhir.
- PASS: font tertanam bukan Type 3; teks berada di dalam halaman dan tidak berisi karakter pengganti.
- PASS: seluruh baris empat program mahasiswa dimuat langsung melalui lstinputlisting.
- PASS: CPL08, CPMK082, Sub-CPMK01-03, dan materi pertemuan 9 sesuai sumber JSON RPS.

## Kompilasi kode

Lingkungan: PlatformIO Core 6.2.0, espressif32 6.12.0, Arduino ESP32 2.0.17, DHTesp 1.19.0.
- 01_wifi: PASS, BIN dan ELF terbentuk dari sumber yang diperiksa.
- 02_sensor: PASS, BIN dan ELF terbentuk dari sumber yang diperiksa.
- 03_cloud: PASS, BIN dan ELF terbentuk dari sumber yang diperiksa.
- 04_server: PASS, BIN dan ELF terbentuk dari sumber yang diperiksa.

## Eksekusi pada ESP32 virtual Wokwi

Firmware lokal hasil PlatformIO dimuat ke Wokwi web. Uji server memakai HTTP nyata melalui TCP loopback pada ESP32 virtual. Harness mengimpor sumber mahasiswa tanpa mengubah handler server; task terpisah mengirim permintaan agar loop server tetap berjalan.
- PASS: Wi-Fi normal memperoleh IP 10.10.0.2; SSID tidak tersedia berakhir dengan pesan kegagalan setelah batas waktu.
- PASS: DHT22 mengikuti tiga masukan: 28.0 C/65.0%, 28.1 C/65.5%, dan 29.1 C/70.5%.
- PASS: GET / mengembalikan form POST; GET /sensor memuat suhu dan kelembapan DHT22.
- PASS: POST state=on/off mengembalikan 303 dan mengubah pin LED sesuai perintah.
- PASS: POST state=salah mengembalikan 400 dan mempertahankan status LED; JSON mencerminkan LED menyala.
- PASS: GET alamat tidak tersedia mengembalikan 404. Total tujuh pemeriksaan HTTP; failures=0.
- PASS: NTP dan Wi-Fi siap; HTTPS ke ThingSpeak memakai CA paket dan menerima HTTP 400/body 0 untuk key uji sengaja salah. Program mencatat bahwa data belum tersimpan.

## Batas bukti

Sesuai arahan pengguna, pengujian dilanjutkan tanpa akun ThingSpeak. Penulisan sukses ke channel dan grafik cloud belum diuji karena Write API Key tidak tersedia. Respons 400 membuktikan transport HTTPS dan jalur penolakan dari ESP32 virtual, bukan keberhasilan penyimpanan.
Aktivasi ekstensi Wokwi VS Code dan penerusan localhost:8180 belum diuji di komputer kelas. Konfigurasinya mengikuti dokumentasi Wokwi. Klik form browser ke ESP32 melalui gateway masuk belum dijalankan; handler, form, dan perubahan LED diuji melalui HTTP loopback.
Tidak dilakukan uji pada perangkat fisik. Firmware tidak menggunakan setInsecure().

## Berkas bukti dan pengulangan

Dalam ZIP sumber LaTeX, log serial dan manifest hash berada di tests/evidence/ serta bukti/. Empat harness pengujian ada di tests/firmware_*/; semuanya mengimpor sumber asli di praktik/. Masuk ke folder proyek yang dipilih, jalankan pio run, lalu muat firmware.bin di Wokwi melalui F1 > Upload Firmware and Start Simulation atau gunakan ekstensi VS Code yang telah aktif.
Jalankan python modul_mk38/tools/verify_week09.py dari root repositori untuk memeriksa kembali artefak dan bukti yang tersimpan. Pemeriksaan rekaman tidak menjalankan ulang simulator.

## Pengujian versi bertahap

Empat versi kecil lulus PlatformIO. GET HTTPS 200 dan perubahan payload DHT22 28/65 ke 32/70 diamati pada Wokwi. Dua versi server kecil lulus enam pemeriksaan HTTP loopback pada ESP32 virtual; penerusan port masuk VS Code pada komputer kelas tetap memerlukan pemeriksaan setempat. Uji layanan HTTP versi lengkap terdahulu tetap tersedia. Penulisan ThingSpeak belum diuji tanpa akun.

Lihat bukti/progressive_*.json untuk hasil baru dan hash kode; EVALUASI_KEJELASAN.md memetakan urutan belajar. Versi lengkap terdahulu tetap disertakan, dan pengujian fisik tidak diasumsikan dari hasil kompilasi.

## Pengujian ulang setelah audit halaman

30 Oktober 2026. Empat proyek utama dan empat versi awal dikompilasi. Dua versi server awal dijalankan pada Wokwi: teks/rute tidak dikenal serta JSON sensor/respons 503 lulus enam pemeriksaan HTTP loopback. Uji ini tidak memeriksa koneksi masuk gateway VS Code atau penulisan channel ThingSpeak sendiri.

Rincian gabungan: PENGUJIAN_ULANG_09_15.json pada folder seri. Catatan setiap halaman: EVALUASI_HALAMAN.md. Bukti runtime mentah berada di bukti/.

## Pengujian channel demonstrasi 2 November 2026

Catatan lama di atas mencerminkan pengujian sebelum akun tersedia. Channel demonstrasi 3560582 kemudian dibuat dan penulisannya berhasil diuji dari ESP32 virtual. Lihat bukti/thingspeak_live_result.json: status PASS, hash program cocok, serta entry ID dibaca kembali dari ThingSpeak. Konfigurasi secrets.demo.h memakai Write API Key yang diminta pengguna untuk dicantumkan; Read API Key tetap lokal. Logika program, urutan versi, dan kode RPS tidak berubah. Build PDF tiga lintasan dan tinjauan render memakai edisi ini. Pi fisik dan cluster MQTT privat belum diuji.

## Screenshot, 6 November 2026

Ditambahkan 6 screenshot di dekat langkah praktik. Folder tahap, build, simulator, masukan DHT22, dan hasil HTTP dihubungkan dengan langkahnya. Masukan pada gambar 32/70 dibedakan dari rekaman JSON 28/65. PDF terbaru 23 halaman; ukuran huruf utama tetap 11 pt. Gambar asli tetap disertakan dalam ZIP sumber. Kode praktik, konfigurasi contoh, urutan versi, dan bobot RPS tidak diubah. Bukti runtime sebelumnya tetap berlaku pada kode yang sama; edisi gambar ini bukan pengujian ulang perangkat fisik.

## Sampul, 6 November 2026

Sampul diseragamkan dan dirender ulang. Seluruh 22 halaman setelah sampul mempunyai teks dan raster yang identik dengan edisi screenshot. Kode praktik tidak berubah; simulator tidak dijalankan ulang pada revisi sampul ini. Lihat ../AUDIT_SAMPUL_09_15.md serta bukti/editorial_page_review.json untuk audit dan hash PDF terbaru.

## Penguatan brief 6 November 2026

Kontrak field, satuan, waktu, pembulatan, serta penelusuran POST--GET--MySQL; backend disiapkan di luar jadwal inti.

Program utama lama tetap mempunyai hash yang sama; bukti runtime sebelumnya tidak diberi tanggal uji ulang baru. PDF dan paket terbaru diperiksa terpisah dari uji fisik. Lihat ../PENGEMBANGAN_BRIEF_09_15.md.

## Edisi API dan GitHub 7 November 2026

Tutorial utama Laravel/MySQL di dalam PDF serta panduan VS Code--PlatformIO--Wokwi: lokasi C++, struktur proyek, pemilihan satu versi, fungsi folder bernomor, health sampai dashboard/kendali.

Program utama lama tetap mempunyai hash yang sama; bukti runtime sebelumnya tidak diberi tanggal uji ulang baru. API baru menjadi jalur utama PDF. Pemasangan dari clone GitHub, empat tahap route HTTP, tiga halaman HTML, serta 12 uji/105 assertion lulus pada MySQL uji terpisah. Lihat ../PEMASANGAN_API_BERSIH.json. PDF dan paket terbaru diperiksa terpisah dari uji fisik. Lihat ../PENGEMBANGAN_BRIEF_09_15.md.

## Pemeriksaan instruksi 7 November 2026

Enam tantangan sensor tetap tersedia. Pengayaan aktuator memakai tiga versi GPIO/LED REST/LED dan servo REST, 16 langkah, tiga tabel MySQL, konfirmasi, expiry, restart, serta dua screenshot sesi Wokwi baru. Input sintetis pada sepuluh screenshot lama tetap diberi batas tersendiri. Lihat bukti/audit_instruksi_20261107.json untuk cakupan bagian dan perubahan. Tujuh kotak HTTP PowerShell diganti dengan skrip Python yang diuji terpisah; kotak lain dipertahankan. Pengayaan aktuator pada pertemuan 9 terdiri atas 24 kotak. Lihat HTTP_PYTHON_09_11.json untuk hash dan cakupan uji klien Python. Bukti runtime lama tetap memakai tanggal uji aslinya. Bukti aktuator baru dicatat terpisah pada api_sensor/bukti/aktuator_verifikasi.json; tidak ada uji perangkat fisik atau keterpahaman mahasiswa baru.

## Tantangan dan screenshot 8 November 2026

Enam tantangan sensor tetap tersedia. Pengayaan aktuator memakai tiga versi GPIO/LED REST/LED dan servo REST, 16 langkah, tiga tabel MySQL, konfirmasi, expiry, restart, serta dua screenshot sesi Wokwi baru. Input sintetis pada sepuluh screenshot lama tetap diberi batas tersendiri. Sepuluh screenshot tantangan sensor berasal dari client HTTP sintetis dan tetap dicatat pada bukti/tantangan_screenshot_20261108.json. Dua screenshot aktuator tambahan berasal dari ESP32 Wokwi VS Code ke Laravel/MySQL; laporan servo membuktikan nilai PWM firmware tanpa sensor posisi mekanis. Panduan BUKA_PRAKTIKUM.md menjelaskan sumber Git dan dependency komputer baru.
