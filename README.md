# Kotori Love Exp Patch Tools

Kumpulan alat dan arsip kerja untuk eksperimen modifikasi serta pembuatan patch **Kotori Love Exp**. Repository ini menyimpan paket **BijikTools v0.6-beta2**, arsip data asli dan hasil repack, arsip sistem, serta panduan penggunaan berbahasa Indonesia.

Alur kerja BijikTools dimulai dari unpack arsip, extract teks ke JSON, menerjemahkan teks, insert kembali ke script game, lalu repack menjadi `data01000.arc`. Gunakan **Engine Target: BGI / Ethornell** sejak awal dan **Pack ARC V1** pada tahap repack.

**Status:** bahan kerja/eksperimen. Kompatibilitas hasil repack dan arsip sistem dengan setiap versi game belum diverifikasi; repository ini tidak menyatakan bahwa patch terjemahan sudah selesai.

## Isi repository

| File | Keterangan |
| --- | --- |
| `BijikTools-v0.6-beta2.rar` | Paket aplikasi Windows beserta dependensi dan alat pendukung. Ekstrak untuk mengakses `BijikTools.exe`. |
| `data01000_original.arc` | Salinan arsip data asli untuk referensi, pembanding, dan sumber workflow. Header arsip: `PackFile`. |
| `data01000.entah` | Salinan dengan ekstensi alternatif; isinya identik dengan `data01000_original.arc` berdasarkan SHA-256. |
| `data01000.repacked.arc` | Hasil eksperimen repack yang sudah tersedia, dengan header `BURIKO ARC20`. Workflow di bawah menghasilkan arsip baru menggunakan **Pack ARC V1**. |
| `system.arc` | Arsip berheader `PackFile` yang memuat `ipl._bp`, `launcher._bp`, dan `VeraMoBd._ft`. Disimpan sebagai bahan kerja terpisah dari workflow script `data01000`. |
| `tutorial.png` | Panduan visual persiapan file, pencadangan, dan penerapan perubahan. |
| `SHA256SUMS.txt` | Checksum enam file distribusi untuk memeriksa keutuhan salinan. |

Susunan file yang tersedia di GitHub:

```text
Kotori-Love-Exp-Patch-Tools/
├── .gitignore
├── BijikTools-v0.6-beta2.rar
├── data01000_original.arc
├── data01000.entah
├── data01000.repacked.arc
├── system.arc
├── tutorial.png
├── SHA256SUMS.txt
└── README.md
```

## Persiapan

- Windows untuk menjalankan aplikasi yang disertakan.
- [7-Zip](https://www.7-zip.org/) atau aplikasi lain yang dapat mengekstrak RAR.
- [.NET Desktop Runtime 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0), sesuai konfigurasi runtime BijikTools yang disertakan.
- Instalasi Kotori Love Exp sebagai tempat pengujian dan salinan cadangan arsip yang akan diubah.

## Mulai menggunakan

1. Unduh repository melalui **Code → Download ZIP**, atau clone:

   ```powershell
   git clone https://github.com/SakuraSymphonyReTranslation/Kotori-Love-Exp-Patch-Tools.git
   ```

2. Ekstrak `BijikTools-v0.6-beta2.rar` di dalam folder repository. Pertahankan susunan folder `lib`, `external`, dan `app` agar file pendukung tetap tersedia.
3. Jalankan `BijikTools.exe` dari folder hasil ekstraksi. Jika Windows meminta runtime, pasang .NET Desktop Runtime 8 yang sesuai.
4. Pilih **Engine Target: BGI / Ethornell** sebelum memproses arsip atau script.
5. Buat folder `work` di dalam repository untuk menyimpan bahan dan hasil proses. Salin `data01000_original.arc` dari root repository ke `work`, lalu beri nama salinannya `data01000.arc`.

Langkah penyiapan ini menyesuaikan nama file di GitHub dengan panduan BijikTools: `data01000_original.arc` menjadi sumber arsip asli, sementara `work/data01000.arc` menjadi input awal. Seluruh path pada workflow berikut berada di dalam `work`; pilih path lengkapnya saat mengisi kolom aplikasi.

## Workflow BijikTools

### 1. Unpack arsip

Di **Archive Tool**, pastikan **Engine Target: BGI / Ethornell**, pilih `work/data01000.arc` sebagai input arsip, lalu unpack ke folder `work/data01000`.

Folder `data01000` berisi script dalam format asli game yang akan digunakan pada tahap extract dan insert.

### 2. Simpan cadangan asli

Setelah unpack selesai, rename `work/data01000.arc` menjadi `work/data01000_original.arc`. Dengan begitu, nama `data01000.arc` siap dipakai untuk hasil repack pada langkah terakhir. Salinan asli di root repository tetap tersedia sebagai referensi.

### 3. Extract script ke JSON

Buka **Extract Script** dengan pengaturan berikut:

| Pengaturan | Path |
| --- | --- |
| Folder input | `work/data01000` |
| Folder output JSON | `work/data01000_json` |

Gunakan format **JSON**, lalu klik **Gas Extract!**. Teks yang akan diterjemahkan disimpan sebagai file `.json` di `data01000_json`.

### 4. Terjemahkan teks

Buka file `.json` di `work/data01000_json`, lalu edit teks untuk diterjemahkan. Pertahankan struktur JSON, nama file, dan informasi pendukung agar hasilnya dapat dimasukkan kembali ke script yang sesuai.

### 5. Insert terjemahan ke script game

Buka **Insert Script** dan isi:

| Kolom | Path |
| --- | --- |
| Folder Script (Awal) | `work/data01000` |
| Folder JSON | `work/data01000_json` |
| Folder Output (Script Baru) | `work/data01000res` |

Klik **Gaskan!** untuk memasukkan teks terjemahan ke script asli. Hasil di `data01000res` sudah kembali ke format script game dan siap dipack.

### 6. Repack menjadi data01000.arc

Kembali ke **Archive Tool**, lalu isi:

| Pengaturan | Nilai |
| --- | --- |
| Engine Target | **BGI / Ethornell** |
| Folder input | `work/data01000res` |
| Output Path | `work/data01000.arc` |

Klik **Pack ARC V1**. Sumber repack adalah folder `data01000res`, yaitu hasil **Insert Script** pada langkah 5.

File akhir workflow adalah `work/data01000.arc`. File `data01000.repacked.arc` di root repository merupakan hasil eksperimen ARC20 yang sudah tersedia; nama tersebut dipertahankan untuk referensi format arsip.

### 7. Uji hasil pada game

Salin `work/data01000.arc` ke salinan instalasi game, mengikuti lokasi arsip yang dibaca oleh game. Cadangkan file yang akan diganti terlebih dahulu. Jalankan game dan periksa bagian yang diterjemahkan; jika hasil gagal dimuat, pulihkan arsip cadangan.

## Susunan folder kerja

Setelah langkah extract, insert, dan repack selesai, susunan lokal yang diharapkan adalah:

```text
Kotori-Love-Exp-Patch-Tools/
├── ...                         # File distribusi dari GitHub
├── BijikTools-v0.6-beta2/       # Hasil ekstraksi paket alat
│   ├── BijikTools.exe
│   ├── lib/
│   ├── external/
│   └── app/
└── work/
    ├── data01000_original.arc  # Cadangan input setelah unpack
    ├── data01000/              # Script asli hasil unpack
    ├── data01000_json/         # JSON untuk diterjemahkan
    ├── data01000res/           # Script game hasil insert
    └── data01000.arc           # Arsip akhir dari Pack ARC V1
```

Folder hasil ekstraksi alat dan `work` dibuat saat menjalankan panduan. Keduanya diabaikan oleh Git agar bahan kerja lokal terpisah dari file distribusi repository.

## Tutorial visual

![Tutorial persiapan, pencadangan, dan penerapan perubahan](tutorial.png)

Gambar ini disertakan sebagai referensi persiapan dan pencadangan. Untuk pemrosesan script `data01000`, ikuti path dan urutan pada **Workflow BijikTools** di atas.

## Memeriksa file

Di PowerShell, checksum sebuah file dapat diperiksa dengan:

```powershell
Get-FileHash -Algorithm SHA256 -LiteralPath '.\BijikTools-v0.6-beta2.rar'
```

Bandingkan hasilnya dengan entri terkait di `SHA256SUMS.txt`.

## Paket alat dan kredit

Arsip BijikTools menyertakan aplikasi utama, CopasTool, serta komponen pendukung seperti EntisDecryptTool, rosetta, tjs2-decompiler, tjs2Compiler, dan pustaka FreeMote. Daftar ini menjelaskan isi paket; tidak semua komponen tersebut diperlukan untuk Kotori Love Exp.

Repository ini mengarsipkan paket alat yang sudah tersedia, bukan menyediakan seluruh source code BijikTools. Kredit aplikasi, pustaka, dan aset tetap milik pembuat atau pemegang hak masing-masing. Tidak ada lisensi baru yang diterapkan atas komponen pihak ketiga melalui repository ini.

Dikelola oleh [Sakura Symphony Re; Translation](https://github.com/SakuraSymphonyReTranslation).

## English overview

A working collection for Kotori Love Exp patch experiments, containing BijikTools v0.6-beta2, original and repacked data archives, a system archive, and an Indonesian guide. Start with a working copy of `data01000_original.arc`, select **BGI / Ethornell**, unpack the archive, extract and translate JSON, insert the translation into game scripts, then use **Pack ARC V1** to create `data01000.arc`. The existing `data01000.repacked.arc` is an ARC20 experiment. Game compatibility and translation completion have not been verified. Keep backups and test changes on a separate game installation.
