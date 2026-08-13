# Modification of the Ising Model for Empirical Public Opinion Data

Repositori ini berisi *source code* dan *dataset* yang digunakan dalam Laporan Take-Home Assignment — **Great Institute**.

Proyek ini memodifikasi Model Ising klasik (fisika statistik) untuk memodelkan dinamika opini publik di media sosial, menggunakan data *social listening* nyata alih-alih data sintetis.

## Ringkasan

Setiap unit opini (komentar/postingan warganet) diperlakukan sebagai sebuah *spin* $s_i \in [-1, +1]$, dengan nilai ditentukan dari gaya bahasa (majas) yang digunakan. Interaksi antar-spin ($J_{ij}$) dibangun dari kedekatan struktural (thread/unggahan yang sama), kesamaan platform, kesamaan isu, dan peluruhan temporal. Simulasi Monte Carlo (Metropolis–Hastings) kemudian dijalankan untuk mempelajari dinamika opini yang muncul dari interaksi tersebut.

Hamiltonian termodifikasi yang digunakan:

$$H = -\sum_{i<j} J_0\, A_{ij}\, \delta(\text{Media}_i,\text{Media}_j)\, \delta(\text{Category}_i,\text{Category}_j)\, e^{-|\Delta t_{ij}|/\tau}\; s_i s_j \;-\; \sum_i \big[h_{\text{media}}(\text{Media}_i) + h_{\text{event}}(t_i)\big]\, s_i$$

dengan $s_i = w(\text{Majas}_i)$.

## Studi Kasus

Dataset merupakan hasil *social listening* lintas enam platform (Threads, TikTok, Facebook, YouTube, Instagram, Twitter/X) atas percakapan warganet mengenai **Presiden Prabowo Subianto** dan isu kebijakan terkait pemerintahannya (mis. kurban Idul Adha dari dana APBN, pelemahan rupiah, IHSG anjlok, program MBG, Danantara, ojol, dsb). Seluruh baris berlabel sentimen *Negative*; variasi intensitas opini dimodelkan lewat gaya bahasa (majas: SINISME, SARKASME, IRONI, LITERAL, dsb.), bukan polaritas sentimen.

## Struktur Repositori

| File/Folder | Deskripsi |
|---|---|
| `simulasi.ipynb` | Notebook utama: implementasi lengkap simulasi, dari pembersihan data hingga visualisasi hasil |
| `dataset.csv` | Dataset mentah hasil social listening (11 kolom, ±3.000 baris) |
| `deskripsi_dataset.md` | Dokumentasi rinci struktur, karakteristik per platform, dan isu kualitas data |
| `requirements.txt` | Daftar dependensi Python |
| `outputs/` | Contoh hasil visualisasi (magnetisasi, distribusi spin, dsb.) dari notebook |
| `LICENSE` | Lisensi MIT |

## Alur Kerja Notebook (`simulasi.ipynb`)

1. **Setup & konfigurasi** — parameter simulasi ($J_0$, $\tau$, $\beta$, jumlah sweep, dll.)
2. **Pemuatan & pembersihan data** — perbaikan mojibake, parsing tanggal, deduplikasi, normalisasi kategori
3. **Pemetaan spin** $s_i = w(\text{Majas}_i)$ — dari label majas ke nilai kontinu $[-1, +1]$
4. **Konstruksi graf interaksi** $A_{ij}$ — berdasarkan struktur thread/unggahan dari kolom `Type` dan `Link`
5. **Konstruksi** $J_{ij}$ — homofili platform, homofili isu, peluruhan temporal
6. **Konstruksi medan eksternal** $h_i(t)$ — bias platform + proxy guncangan peristiwa (KDE volume postingan harian)
7. **Simulasi Metropolis-Hastings** — varian *soft-spin* karena $s_i$ kontinu, bukan biner
8. **Estimasi parameter** — *inverse Ising* / *simulated moment matching* pada subset kalibrasi
9. **Visualisasi & interpretasi** — jejak magnetisasi, distribusi spin, peta $J_{ij}$ per platform, kurva $h_{\text{event}}(t)$

## Instalasi & Menjalankan

```bash
pip install -r requirements.txt
jupyter notebook simulasi.ipynb
```

Dependensi utama: `numpy`, `pandas`, `networkx`, `matplotlib`, `scipy`.

## Catatan Kualitas Data

Dataset memiliki beberapa isu yang sudah ditangani secara minimal di tahap pembersihan (lihat `deskripsi_dataset.md` untuk detail lengkap):
- Mojibake akibat double-encoding UTF-8 pada emoji/karakter kutip pintar
- Duplikasi konten (retweet berantai, cross-post)
- Teks terpotong pada baris dengan konten panjang
- ID numerik yang terkorupsi menjadi notasi ilmiah (khusus kolom identitas Facebook)
- Skema `Type` yang tidak seragam antar-platform

## Lisensi

Proyek ini dilisensikan di bawah [MIT License](LICENSE).

## Disclaimer

Bobot pemetaan majas → spin (`MAJAS_WEIGHTS`) dan parameter model bersifat indikatif untuk keperluan Take-Home Assignment, dan dapat dikalibrasi ulang melalui expert scoring atau validasi antar-anotator untuk penggunaan di luar konteks akademis ini.
