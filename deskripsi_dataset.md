# Deskripsi Dataset: `dataset.csv`

## 1. Ringkasan Umum

Dataset ini merupakan hasil **social listening / media monitoring** atas percakapan warganet **lintas berbagai platform media sosial**, dengan topik utama seputar **Presiden Prabowo Subianto** dan isu-isu politik/kebijakan terkait pemerintahannya (mis. kurban Idul Adha dari dana APBN, pelemahan rupiah, IHSG anjlok, program MBG, ekspor SDA, Danantara, ojol, dsb).

Dataset **berasal dari enam platform**: Threads, TikTok, Facebook, YouTube, Instagram, dan Twitter/X. Dataset disusun **berurutan per blok platform** (bukan tercampur acak per baris) — artinya satu rentang indeks baris cenderung berisi data dari satu platform yang sama secara berurutan sebelum berpindah ke platform berikutnya.

## 2. Struktur Kolom

Dataset memiliki **11 kolom**, dengan struktur yang konsisten di semua platform yang teramati:

| No | Nama Kolom | Tipe Data | Deskripsi |
|---|---|---|---|
| 1 | `No` | Integer | Nomor urut baris |
| 2 | `Type` | Kategori | Jenis konten; istilahnya berbeda-beda tergantung platform asal (lihat §3.1) |
| 3 | `Mentions` | Teks | Isi komentar/caption/postingan (konten utama) |
| 4 | `Majas` | Kategori | Label gaya bahasa/figur retoris hasil anotasi |
| 5 | `Date` | Datetime | Waktu publikasi, format `YYYY-MM-DD HH:MM:SS` |
| 6 | `Link` | URL | Tautan ke postingan asli; formatnya khas tiap platform (lihat §3.2) |
| 7 | `Media` | Kategori | Nama platform sumber: Threads, TikTok, Facebook, Youtube, Instagram, Twitter |
| 8 | `Sentiment` | Kategori | Label sentimen hasil anotasi |
| 9 | `Category Group` | Kategori | Kelompok isu kebijakan (mis. "Asta Cita") |
| 10 | `Category` | Kategori | Sub-kategori isu spesifik (mis. "Asta Cita 1" s.d. "Asta Cita 8") |
| 11 | *(tanpa nama header)* | Teks | Identitas pembuat konten — pengisiannya bergantung platform (lihat §3.4) |

## 3. Karakteristik Umum per Platform

### 3.1 Nilai `Type`

Nilai pada kolom `Type` bersifat spesifik-platform:

| Platform | Contoh nilai Type |
|---|---|
| Threads | `th-comment`, `text`, `photo`, `video` |
| TikTok | `video`, `photo` |
| Facebook | `fb-comment`, `fb-post` |
| YouTube | `-` (tidak memiliki sub-tipe; konsisten bernilai strip) |
| Instagram | `ig-comment`, `video`, `image` |
| Twitter/X | `rt` (retweet), `reply`, `mention` |

### 3.2 Format `Link`

| Platform | Pola URL |
|---|---|
| Threads | `https://www.threads.com/@username/post/xxxx` |
| TikTok | `https://www.tiktok.com/@username/video/xxxx` atau `.../photo/xxxx` |
| Facebook | `https://www.facebook.com/{id}/posts/{id}` (terkadang disertai parameter `?comment_id=`) |
| YouTube | `https://www.youtube.com/watch?v=xxxx` |
| Instagram | `https://www.instagram.com/p/xxxx` |
| Twitter/X | `https://twitter.com/web/statuses/xxxx` |

### 3.3 Kategori Majas

Skema anotasi Majas **sama/dipakai konsisten di semua platform**, namun kemunculannya bervariasi tergantung sumber. Secara umum, kategori majas meliputi:

- **SINISME** — komentar bernada menyindir dengan makna berkebalikan dari yang diucapkan
- **SARKASME** — sindiran tajam/kasar yang eksplisit
- **IRONI** — pernyataan yang bertolak belakang dengan kenyataan/harapan
- **LITERAL** — pernyataan/caption faktual apa adanya, umumnya dari akun berita atau caption video informatif
- **RETORIS** — kalimat tanya yang tidak butuh jawaban, dipakai untuk menegaskan sikap
- **SINDIRAN** — kritik tidak langsung
- **HIPERBOLA** — pernyataan berlebihan untuk menekankan emosi
- **SATIRE** — kritik yang dibingkai dengan humor/ejekan
- **METAFORA** & **SIMILE** — perumpamaan/analogi
- **PENEGASAN** — kalimat yang menegaskan suatu fakta/klaim
- **REPETISI** — pengulangan kata/frasa untuk penekanan
- **KRITIK** — kritik langsung tanpa figur retoris tertentu

Pola: Konten dari akun **media/berita** (di YouTube dan TikTok) cenderung didominasi label **LITERAL** (caption berita apa adanya), sedangkan **komentar personal warganet** (di Threads dan Twitter) cenderung didominasi label **SINISME, SARKASME,** dan **IRONI**.

### 3.4 Pengisian kolom ke-11 (identitas pembuat konten) — bervariasi per platform

| Platform | Pola pengisian |
|---|---|
| Threads | Kosong |
| YouTube | Kosong |
| TikTok | Kosong |
| Facebook | Terisi — berupa **nama tampilan** akun (mis. "Romi Zain", "tribunnews"); beberapa ID numerik panjang berubah jadi notasi ilmiah seperti `"6,63E+14"` |
| Instagram | Terisi — berupa **username/handle** akun (mis. "shob_shadows") |
| Twitter/X | Terisi — berupa **username/handle** akun (mis. "totoromengaji") |

Kolom ini pada dasarnya adalah kolom identitas pembuat konten, tetapi pengisiannya hanya konsisten untuk separuh dari platform yang ada dalam dataset.

### 3.5 Sentimen dan Kategorisasi Isu

Label `Sentiment` **selalu bernilai Negative** — mengindikasikan bahwa dataset yang tersedia sudah difilter/disortir sedemikian rupa (misalnya per-platform per-sentimen), bukan sampel acak murni dari seluruh populasi data yang mencakup ragam sentimen lain.

Kolom `Category Group` dan `Category` (skema "Asta Cita 1" s.d. "Asta Cita 8") konsisten di seluruh platform, namun mayoritas baris pada tiap platform tetap tidak terkategorikan (bernilai "-"). Ini menunjukkan proses kategorisasi isu kebijakan baru diterapkan pada sebagian kecil data secara menyeluruh, bukan spesifik ke satu platform saja.

## 4. Kualitas & Masalah Data (berlaku umum di semua platform)

- **Mojibake / encoding rusak** konsisten di seluruh platform — emoji dan karakter kutip pintar (`“…”`) ter-decode salah menjadi urutan karakter seperti `Ã°ÂŸÂ¤Â£` atau `Ã¢Â€ÂœÃ¢Â€Â`. Ini indikasi kuat file sumber mengalami **double-encoding UTF-8** sebelum diekspor ke `.csv`.
- **Duplikasi konten** cukup umum — retweet berantai di Twitter, cross-post caption yang sama di beberapa video YouTube/TikTok, serta komentar berulang di Facebook/Instagram (bisa dari akun yang sama memposting ulang, atau caption template yang dipakai banyak kreator).
- **Teks terpotong** di tengah kalimat pada banyak baris dengan konten panjang, karena batas panjang karakter saat ekspor per-sel.
- **Data numerik ter-korupsi jadi notasi ilmiah** — khususnya pada kolom identitas Facebook, beberapa ID numerik panjang berubah format menjadi seperti `"6,63E+14"`, klasik artefak akibat file sempat dibuka/diproses di aplikasi spreadsheet (Excel) yang salah mendeteksi tipe data ID sebagai angka.
- **Sel berisi teks multi-baris** (pada Facebook dan YouTube) — teks `Mentions` panjang sering mengandung newline literal di dalam tanda kutip CSV, sehingga tampak seperti beberapa baris terpisah jika dibuka dengan teks editor biasa (bukan parser CSV yang benar).
- **Skema `Type` tidak seragam antar-platform** — nilainya spesifik-platform, sehingga analisis lintas platform berdasarkan `Type` memerlukan pemetaan/normalisasi terlebih dahulu (mis. menyamakan `fb-comment`, `th-comment`, `ig-comment`, `reply` sebagai satu kategori umum "komentar").
- **Kolom ke-11 tidak konsisten terisi** antar-platform (lihat §3.4) — perlu penanganan *missing value* yang sesuai sebelum dipakai sebagai fitur identitas pembuat konten.