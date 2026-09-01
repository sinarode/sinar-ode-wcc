# Website Sinar Ode Content Creator — Panduan Edit Manual

File utama: **`index.html`** — satu file ini berisi semuanya (desain, isi,
dan logika interaktif). Buka pakai text editor apa saja (Notepad, VS Code,
Sublime, dll), lalu buka file `.html`-nya di browser untuk lihat hasilnya.

> 💡 Tips: setiap kali habis edit, simpan file-nya lalu tekan refresh (F5)
> di browser buat lihat perubahan.

---

## 1. Struktur Folder yang Disarankan

Taruh semua file di **satu folder yang sama**, seperti ini:

```
folder-website/
├── index.html                  ← file utama
├── foto-graduation-1.jpg       ← foto-foto kamu
├── foto-graduation-2.jpg
├── foto-graduation-3.jpg
├── video-about-us.mp4          ← video-video kamu
├── wcc 1.mp4
├── wcc 2.mp4
└── logo (sudah ditanam di dalam kode, tidak perlu file terpisah)
```

Kalau strukturnya begini, `src="wcc 1.mp4"` di dalam kode otomatis nemu
file videonya. Kalau foto/videonya kamu taruh di dalam subfolder (misal
folder `assets/`), tulis path-nya jadi `assets/wcc 1.mp4`.

---

## 2. Menambahkan / Mengganti Foto

**Lokasi di kode:** cari komentar `TAMBAHKAN FOTO DI SINI` (bagian
"Our Moment" → galeri Graduation).

Setiap foto adalah sebuah `<div class="ph ...">` dengan gambar diisi lewat
`style="background-image:url('...')"`. Contoh:

```html
<div class="ph big" style="background-image:url('foto-graduation-1.jpg');"></div>
```

Untuk ganti foto → ganti saja nama file di dalam `url('...')`.
Untuk pakai foto dari internet → tempel link fotonya, contoh:

```html
<div class="ph big" style="background-image:url('https://contoh.com/foto.jpg');"></div>
```

**Menambah foto baru?** Tinggal copy satu blok `<div class="ph ...">...</div>`
lalu tempel lagi, ganti nama filenya.

---

## 3. Menambahkan / Mengganti Video

**Lokasi di kode:** cari komentar `TAMBAHKAN VIDEO DI SINI` (bagian
"About Us" dan "Our Moment → Wedding", tampilan bingkai HP).

```html
<video class="phone-video" muted loop playsinline src="wcc 1.mp4"></video>
```

Untuk ganti video → ganti nama file di dalam `src="..."`.
Format yang paling aman dipakai di semua browser: **`.mp4`**.

⚠️ **Jangan hapus atribut `muted`** dari tag `<video>` — itu wajib supaya
browser mengizinkan video main otomatis. (Detail soal ini di bagian 5.)

---

## 4. Mengatur Ukuran Foto & Video

Ukuran diatur secara **terpusat** lewat variabel di bagian paling atas
kode (`<style>` → `:root { ... }`), jadi ubah satu angka = berubah semua.

```css
:root{
  --phone-w: 220px;        /* lebar bingkai HP (video) */
  --phone-h: 452px;        /* tinggi bingkai HP (video) */
  --gallery-big-h: 300px;  /* tinggi foto besar galeri Graduation */
  --gallery-small-h: 140px;/* tinggi 2 foto kecil di sampingnya */
}
```

Cara kerja crop foto/video: defaultnya pakai `object-fit: cover` (untuk
video) / `background-size: cover` (untuk foto) — artinya gambar akan
dipotong rapi supaya penuh mengisi bingkai, tanpa gepeng. Kalau kamu mau
foto/videonya tampil **utuh tanpa terpotong** (mungkin ada bagian kosong
di pinggir), cari baris CSS ini dan ganti `cover` jadi `contain`:

```css
.phone-frame video{ ... object-fit:cover; ... }   /* untuk video HP */
.gallery .ph{ ... background-size:cover; ... }     /* untuk foto galeri */
```

---

## 5. Perilaku Video (Auto Play, Auto Mute, Tap to Pause)

Video di bingkai HP itu **interaktif**, ada 2 fitur otomatis:

1. **Scroll ke video → video main + suaranya nyala.**
   Scroll lewat / keluar dari video → video pause + suaranya mati lagi.
   - Titik "mulai main"-nya diatur oleh `threshold:0.6` (artinya 60% dari
     bingkai HP harus kelihatan dulu). Cari kata `videoIO` di bagian
     `<script>` paling bawah kalau mau mengubah angka ini (0–1).

2. **Ketuk / klik videonya langsung → pause atau main lagi secara manual.**
   Ada ikon ▶ / ⏸ yang muncul sekilas sebagai tanda. Kalau video di-pause
   manual lewat ketukan, dia tidak akan otomatis main lagi walau masih
   kelihatan di layar — dia baru "reset" ke mode otomatis setelah di-scroll
   keluar layar lalu masuk lagi.

Kenapa videonya harus punya atribut `muted` di HTML? Karena hampir semua
browser (Chrome, Safari, dll) **melarang** video autoplay dengan suara
tanpa interaksi pengguna dulu. Trik yang dipakai di sini: video mulai main
dalam keadaan `muted` (diizinkan), lalu suaranya baru dinyalakan lewat
kode setelah videonya berjalan. Kalau atribut `muted` dihapus dari HTML,
videonya berisiko gagal autoplay sama sekali.

---

## 6. Tombol WhatsApp / Instagram / TikTok

**Lokasi di kode:** cari komentar `GANTI LINK SOSMED DI SINI` (di bagian
"Term & Condition", dekat bagian bawah file).

### WhatsApp
```html
<a class="social-btn whatsapp" href="https://wa.me/6283159371090" target="_blank" rel="noopener">
  ...
  +62 831-5937-1090
</a>
```
- Ganti angka di `https://wa.me/NOMOR` — formatnya: **62** (kode negara
  Indonesia, tanpa tanda +) lalu nomornya **tanpa angka 0 di depan**,
  dan **tanpa spasi / strip**.
  Contoh: nomor `0831-5937-1090` → jadi `https://wa.me/6283159371090`
- Jangan lupa ganti juga teks nomor yang tampil di tombolnya.

### Instagram
```html
<a class="social-btn chip" href="https://instagram.com/sinarode_mc" target="_blank" rel="noopener">
  ...
  @sinarode_mc
</a>
```
- Ganti `sinarode_mc` di `href` dengan username Instagram tujuan (tanpa
  tanda @ di URL-nya).
- Ganti juga teks `@sinarode_mc` yang tampil di tombolnya.

### TikTok
```html
<a class="social-btn chip" href="https://www.tiktok.com/@sinarode_mc" target="_blank" rel="noopener">
  ...
  @sinarode_mc
</a>
```
- Sama seperti Instagram, tapi URL TikTok **pakai tanda @** di depan
  username-nya: `https://www.tiktok.com/@username`.

### Menambah / Mengurangi Akun
Setiap tombol adalah satu blok utuh `<a class="social-btn chip">...</a>`.
Mau tambah akun baru → copy satu blok, tempel, lalu ganti link & teksnya.
Mau hapus akun → hapus saja satu bloknya.

---

## 7. Mengganti Teks (Harga, Deskripsi, dll)

Semua teks di halaman ini (harga paket, deskripsi, syarat & ketentuan,
dll) adalah teks polos di dalam tag HTML — cari teksnya langsung pakai
**Ctrl+F / Cmd+F** di text editor kamu, lalu edit di tempat.

Contoh mengganti harga paket:
```html
<div class="pkg-price">Rp 500.000</div>
```
Tinggal ganti angkanya jadi harga baru.

---

## 8. Warna & Font (Opsional, untuk yang mau ubah tema)

Semua warna diatur lewat variabel di `:root` (bagian paling atas
`<style>`), sama seperti ukuran foto/video:

```css
--cream: #FAF6ED;       /* warna latar utama */
--gold: #A9812E;        /* warna emas aksen */
--gold-dark: #7C5A1E;   /* warna emas gelap (judul, tombol) */
--ink: #3B2E14;         /* warna teks utama */
```

Font yang dipakai: **Playfair Display** (judul besar), **Cormorant
Garamond** (tulisan miring/tagline), **Poppins** (teks biasa) — semuanya
dari Google Fonts, sudah otomatis ter-load selama koneksi internet ada.

---

## 9. Cara "Publish" / Membagikan Website Ini

- **Kirim langsung ke klien:** cukup zip folder `folder-website/` (yang
  isinya `index.html` + semua foto/video), lalu kirim. Klien tinggal
  extract dan buka `index.html` di browser.
- **Upload ke hosting/domain:** upload semua isi folder (index.html +
  foto + video) ke hosting kamu (Netlify, Vercel, cPanel, dll). Karena
  namanya sudah `index.html`, otomatis jadi halaman utama.

---

## Ringkasan Cepat (Cheat Sheet)

| Mau ganti apa?              | Cari komentar / kata kunci ini di kode |
|------------------------------|------------------------------------------|
| Foto galeri Graduation       | `TAMBAHKAN FOTO DI SINI`                |
| Video di bingkai HP          | `TAMBAHKAN VIDEO DI SINI`               |
| Ukuran HP & foto             | `--phone-w`, `--phone-h`, `--gallery-big-h` |
| object-fit cover/contain     | `object-fit:cover` / `background-size:cover` |
| Kapan video mulai main       | `threshold:0.6` (cari `videoIO`)        |
| Link WhatsApp / IG / TikTok  | `GANTI LINK SOSMED DI SINI`             |
| Warna & font                 | `:root{` paling atas `<style>`          |

Kalau ragu, semua bagian penting di dalam `index.html` juga sudah dikasih
komentar langsung di kodenya (kalimat yang diawali `<!-- -->` untuk HTML
atau `/* */` untuk CSS/JS) — jadi bisa dibaca sambil edit.