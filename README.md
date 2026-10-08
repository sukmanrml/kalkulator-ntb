# Kalkulator NTB

Alat bantu untuk memeriksa kewajaran **Nilai Tambah Bruto (NTB)**, upah, dan komponen biaya sebuah usaha berdasarkan kategori dan KBLI. Alat ini adalah versi web dari berkas Excel *Kalkulator NTB_SharedV1.1*.

Semua perhitungan berjalan di browser. Angka yang diisi tidak dikirim dan tidak disimpan di mana pun.

## Cara memakai

1. Pilih **Kategori KBLI** dan **KBLI 5 digit**. Kedua daftar bisa dicari. Memilih KBLI mengisi kategorinya otomatis.
2. Isi banyaknya tenaga kerja, pengeluaran (rincian 26a sampai 26e) dan pendapatan (27a dan 27b), dalam rupiah setahun.
3. Hasil muncul di sebelah kanan saat Anda mengetik. Tombol **Salin hasil** menyalin ringkasan sebagai teks.

Tombol **Isi contoh** mengisi contoh dari berkas Excel (kategori A, KBLI 01111, 3 tenaga kerja).

## Aturan pemeriksaan

| Perhitungan | Rumus |
|---|---|
| Upah per tenaga kerja setahun | 26a ÷ tenaga kerja. Wajar bila 12 juta sampai 144 juta. |
| Total pengeluaran | 26a + 26b + 26c + 26d + 26e. Bila lebih besar dari total pendapatan, komponen terbesar ditandai. |
| Output | 27a − 26c |
| Nilai tambah | Output − 26b − 26d |
| Rasio NTB | Nilai tambah ÷ output. Dibandingkan dengan rentang kategori. |
| Rasio upah | 26a ÷ nilai tambah. Wajar bila 40% sampai 60%. |
| Saran upah | Nilai tambah × persentase upah kategori (minimum sampai maksimum). |
| Saran biaya | Bila rasio NTB di luar rentang: di atas 85% biaya dinaikkan ke rentang saran, selain itu diturunkan. |
| Klasifikasi usaha | Mikro 1 sampai 4, Kecil 5 sampai 19, Menengah/Besar 20 atau lebih tenaga kerja. |

## Perbedaan dengan berkas Excel

- Usaha dengan 20 tenaga kerja atau lebih diklasifikasikan **Menengah/Besar**. Di Excel, sel batas maksimumnya kosong sehingga hasilnya "Tidak Valid".
- Saran saat rasio NTB terlalu rendah untuk biaya pembelian berbunyi "Turunkan Biaya Pembelian Barang dan Jasa". Di Excel tertulis "Turunkan Biaya Operasional" (salah salin).
- Saran upah tidak ditampilkan bila nilai tambah nol atau negatif. Sebagai gantinya muncul peringatan nilai tambah negatif.
- Kategori P tidak punya rentang rasio NTB, sehingga selalu ditandai di luar rentang (sama seperti Excel). Kategori V tidak punya batas sama sekali dan diberi catatan.

## Pengembangan

Dibangun dengan Vue 3, TypeScript, Vite, Tailwind CSS 4, dan [shadcn-vue](https://www.shadcn-vue.com) (gaya `new-york`, dasar `stone`, warna utama teal).

```bash
pnpm install
pnpm dev          # server pengembangan
pnpm test         # tes perhitungan (vitest)
pnpm build        # hasil di dist/
```

Logika perhitungan ada di `src/lib/ntb.ts` dan tidak bergantung pada Vue. Data kategori, KBLI, dan batas wajar ada di `src/data/ntb-data.json`. Berkas itu dibuat dari berkas Excel dengan:

```bash
python3 tools/extract-data.py "/jalur/ke/Kalkulator NTB_SharedV1.1.xlsx"
```

Skrip hanya membaca master KBLI dan batas wajar (Sheet2), bukan data responden.

## Publikasi ke GitHub Pages

Situs diterbitkan dari cabang `gh-pages`, tanpa GitHub Actions:

```bash
pnpm deploy
```

Skrip membangun situs, lalu mendorong isi `dist/` ke cabang `gh-pages` pada remote `origin`. Di pengaturan repositori, sumber Pages adalah cabang `gh-pages`, folder root.

## Catatan

Batas kewajaran berasal dari berkas sumber dan hanya membantu pemeriksaan awal. Keputusan akhir tetap pada petugas.


Lisensi: MIT.
