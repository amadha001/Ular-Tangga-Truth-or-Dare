# Teman Tangga — Ular Tangga Online

Game web ular tangga multiplayer dengan kartu pertanyaan Truth or Dare acak, tampilan cute pastel, kode room, 2–6 pemain, papan 100 kotak, ular/tangga, dan sinkronisasi real-time melalui Firebase Firestore.

## File di dalam project

- `index.html` — frontend game (HTML, CSS, JavaScript).
- `firestore.rules` — aturan akses awal untuk Firestore.
- `README.md` — panduan setup dan deploy.

## Penting: konfigurasi Firebase diperlukan

Source code ini sudah berisi alur game dan sinkronisasi real-time, tetapi belum dapat membuat room online sampai dihubungkan ke project Firebase milikmu. Firebase menyediakan backend real-time. Tidak perlu membuat server sendiri untuk versi awal ini.

### 1. Buat project Firebase

1. Buka https://console.firebase.google.com/ dan pilih **Create a project**.
2. Buat project, misalnya `teman-tangga-online`.
3. Di Project Overview, pilih ikon web `</>` untuk mendaftarkan aplikasi web.
4. Beri nama aplikasi lalu salin object `firebaseConfig` yang diberikan Firebase.

### 2. Tempel konfigurasi

Buka `index.html` dengan editor kode. Cari `const firebaseConfig = { ... }` lalu ganti nilai `GANTI_DENGAN_...` dengan konfigurasi aplikasi Firebase milikmu. Jangan mengubah nama properti.

Konfigurasi web Firebase (termasuk apiKey) memang dipakai di frontend. Keamanan data harus ditegakkan lewat Firestore Security Rules, bukan dengan menyembunyikan config.

### 3. Aktifkan Anonymous Authentication

1. Firebase Console → **Build → Authentication**.
2. Klik **Get started**.
3. Di tab **Sign-in method**, aktifkan **Anonymous**.
4. Simpan.

### 4. Buat database Firestore

1. Firebase Console → **Build → Firestore Database** → **Create database**.
2. Pilih lokasi database yang dekat dengan pemain (lokasi database tidak bisa diubah setelah dibuat).
3. Setelah database aktif, buka tab **Rules**.
4. Ganti rules awal dengan isi file `firestore.rules`, lalu klik **Publish**.

**Catatan keamanan:** rules ini ditujukan untuk starter game dan membatasi baca ke anggota room. Untuk game publik berskala besar, rules ini belum mencegah anggota yang nakal memalsukan posisi/giliran karena validasi aksi utama masih berjalan di browser. Pindahkan lempar dadu, validasi giliran, pembuatan kartu, dan update posisi ke Firebase Cloud Functions dengan transaksi tepercaya sebelum dipakai untuk kompetisi atau data sensitif.

### 5. Tes secara lokal

Jangan membuka `index.html` langsung sebagai `file://`. Jalankan server lokal agar module JavaScript dapat dimuat:

**Pilihan A — VS Code**
1. Buka folder project di VS Code.
2. Pasang ekstensi Live Server jika belum ada.
3. Klik kanan `index.html` → **Open with Live Server**.

**Pilihan B — Python**
Di terminal dari folder project jalankan:

```bash
python -m http.server 5500
```

Lalu buka `http://localhost:5500`.

Untuk tes multiplayer, buka URL yang sama di dua browser/perangkat dan buat room dari salah satunya. Agar teman dari internet dapat bergabung, deploy ke hosting publik.

### 6. Deploy gratis ke Netlify

1. Buka https://app.netlify.com/ dan login.
2. Pilih **Add new site → Deploy manually**.
3. Ekstrak ZIP, lalu seret folder project (folder yang berisi `index.html`) ke area upload Netlify.
4. Tunggu deploy selesai, lalu buka URL HTTPS yang diberikan.
5. Bagikan URL tersebut kepada teman. Satu orang membuat room, yang lain memasukkan kode room.

Alternatif: unggah folder project ke GitHub, lalu hubungkan repository ke Netlify agar pembaruan dapat di-deploy ulang dengan mudah.

## Cara bermain

1. Masukkan nama dan buat room.
2. Bagikan kode 6 karakter kepada 1–5 teman.
3. Setelah minimal 2 pemain bergabung, host menekan **Mulai permainan**.
4. Pemain yang mendapat giliran menekan **Lempar dadu**.
5. Pion bergerak sesuai hasil dadu. Tangga membawa pemain naik dan ular membawa pemain turun.
6. Setelah gerakan selesai, kartu Truth atau Dare muncul. Pemain boleh menjawab atau melewati pertanyaan.
7. Setelah kartu ditutup, giliran berlanjut. Pemenang adalah pemain yang mencapai kotak 100 dengan angka dadu yang tepat.

## Catatan fitur saat ini

- Kartu pertanyaan berisi konten bawaan berbahasa Indonesia; jenis Truth/Dare dipilih acak.
- Kartu dibagikan ke seluruh pemain di room agar mereka dapat melihat prompt yang sama.
- Room lobby dibatasi 6 pemain.
- Nama pemain dan posisi disimpan di dokumen room Firestore.
- Tombol "Keluar ke beranda" hanya kembali ke lobby aplikasi; tidak menghapus pemain/room agar tidak mengganggu permainan yang sedang berlangsung.
- Tidak ada akun atau password; pemain menggunakan Firebase Anonymous Auth.
- Ini starter project yang bisa dijalankan setelah setup Firebase. Belum ada dashboard admin untuk mengedit bank pertanyaan.
