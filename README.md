# Kalkulator NTB

Kalkulator NTB memeriksa apakah Nilai Tambah Bruto, upah, dan komponen biaya sebuah usaha masuk akal untuk kategori KBLI-nya. Aplikasi ini memindahkan berkas Excel *Kalkulator NTB_SharedV1.1* ke situs web, sehingga petugas cukup membuka tautan tanpa menyalin berkas.

Alamat situs: https://sukmanrml.github.io/kalkulator-ntb/

![Tampilan kalkulator dengan contoh dari berkas Excel](docs/tampilan.png)

Perhitungan berjalan di peramban Anda. Angka yang diisi tidak dikirim ke server mana pun dan tidak disimpan setelah Anda menutup halaman.

## Cara memakai

1. Pilih kategori KBLI dan KBLI 5 digit. Kedua daftar bisa dicari dengan kode atau kata, misalnya `47111` atau `jagung`. Memilih KBLI mengisi kategorinya.
2. Isi banyaknya tenaga kerja, pengeluaran (rincian 26a sampai 26e), dan pendapatan (27a dan 27b). Semua angka dalam rupiah setahun.
3. Baca hasilnya di panel kanan. Hasil berubah setiap kali Anda mengetik. Tombol "Salin hasil" menyalin ringkasannya sebagai teks, misalnya untuk ditempel ke catatan pemeriksaan.

Tombol "Isi contoh" mengisi contoh dari berkas Excel, dan "Kosongkan" menghapus semua isian.

## Contoh hitungan

Contoh bawaan: kategori A (pertanian), KBLI 01111 pertanian jagung, 3 tenaga kerja, upah Rp9.000.000, biaya produksi Rp10.000.000, biaya operasional Rp500.000, dan pendapatan Rp36.000.000.

| Hasil | Nilai | Keterangan |
|---|---|---|
| Upah per tenaga kerja | Rp3.000.000 setahun | Di bawah batas Rp12.000.000. Muncul peringatan agar upah dan jumlah tenaga kerja diperiksa kembali, karena setara Rp250.000 per bulan. |
| Nilai tambah | Rp25.500.000 | Rp36.000.000 dikurangi biaya produksi dan operasional. |
| Rasio NTB | 70,83% | Masih dalam rentang kategori A, yaitu 51% sampai 94%. |
| Rasio upah | 35,29% | Di luar rentang 40% sampai 60%. Saran upah: Rp3.422.100 sampai Rp10.266.300. |

## Aturan pemeriksaan

| Perhitungan | Rumus dan batas |
|---|---|
| Upah per tenaga kerja setahun | 26a dibagi tenaga kerja. Wajar antara Rp12.000.000 dan Rp144.000.000. |
| Total pengeluaran | 26a + 26b + 26c + 26d + 26e. Jika melebihi total pendapatan, komponen terbesar ditandai. |
| Output | 27a - 26c |
| Nilai tambah | Output - 26b - 26d |
| Rasio NTB | Nilai tambah dibagi output, dibandingkan dengan rentang kategori. |
| Rasio upah | 26a dibagi nilai tambah. Wajar antara 40% dan 60%. |
| Saran upah | Nilai tambah dikali persentase upah kategori, dari batas bawah sampai batas atas. |
| Saran biaya | Muncul jika rasio NTB di luar rentang. Rasio di atas 85% berarti biaya terlalu kecil, dan aplikasi menyarankan rentang biaya sebagai bagian dari output. Pada rasio 85% atau lebih rendah, biaya disarankan turun. |
| Klasifikasi usaha | Mikro 1 sampai 4 tenaga kerja, kecil 5 sampai 19, menengah/besar 20 atau lebih. |

Dari 22 kategori KBLI (A sampai V), 20 punya rentang rasio NTB. Kategori P tidak punya rentang dan selalu ditandai di luar rentang, sama seperti di Excel. Kategori V tidak punya batas sama sekali, jadi aplikasi hanya menampilkan catatan.

## Perbedaan dengan berkas Excel

Tiga perilaku berbeda dari Excel:

- Usaha dengan 20 tenaga kerja atau lebih diklasifikasikan menengah/besar. Di Excel hasilnya "Tidak Valid" karena sel batas maksimumnya kosong.
- Saran untuk biaya pembelian saat rasio NTB di luar rentang dan tidak melebihi 85% berbunyi "Turunkan Biaya Pembelian Barang dan Jasa". Di Excel tertulis "Turunkan Biaya Operasional", salah salin dari baris di bawahnya.
- Saran upah tidak muncul jika nilai tambah nol atau negatif. Sebagai gantinya muncul peringatan nilai tambah negatif.

## Data

`src/data/ntb-data.json` berisi 22 kategori KBLI, 1.559 kode KBLI lima digit, rentang rasio NTB, persentase upah per kategori, dan batas umum. Berkas ini dibuat dari berkas Excel dengan skrip yang hanya membaca master KBLI dan lembar batas wajar:

```bash
python3 tools/extract-data.py "/jalur/ke/Kalkulator NTB_SharedV1.1.xlsx"
```

Skrip tidak membaca isian responden.

## Pengembangan

Aplikasi memakai Vue 3, TypeScript, Vite, Tailwind CSS 4, dan [shadcn-vue](https://www.shadcn-vue.com) dengan gaya `new-york`, warna dasar `stone`, dan warna utama teal. Tema terang atau gelap mengikuti pengaturan sistem dan bisa diganti lewat tombol di pojok kanan atas.

```bash
pnpm install
pnpm dev        # server pengembangan
pnpm test       # 11 tes perhitungan (vitest)
pnpm build      # hasil di dist/
```

Rumus ada di `src/lib/ntb.ts` dan tidak bergantung pada Vue, sehingga bisa diuji tanpa antarmuka. Penyusunan teks peringatan ada di `src/lib/summary.ts`, dan `src/composables/useNtbCalculator.ts` menghubungkan keduanya ke komponen.

## Menerbitkan ke GitHub Pages

Situs terbit dari cabang `gh-pages`. Cabang itu tidak memerlukan GitHub Actions.

```bash
pnpm deploy
```

Skrip membangun situs, lalu mendorong isi `dist/` ke cabang `gh-pages` di remote `origin`. Di pengaturan repositori, sumber Pages adalah cabang `gh-pages` dengan folder root.

## Batasan

Batas kewajaran berasal dari berkas sumber dan dipakai untuk pemeriksaan awal. Petugas tetap memutuskan apakah angka sebuah usaha perlu dikonfirmasi ulang ke responden.

## Lisensi

MIT. Lihat berkas `LICENSE`.
