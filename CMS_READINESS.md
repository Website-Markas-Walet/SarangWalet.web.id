---
site: sarangwalet.web.id
deploy: { host: cloudflare_pages, cf_account: null, branch: main, deploy_hook: unknown }
url_patterns: { artikel: "/blog, /berita (index); post: /<slug> folder/index.html; taxonomy: /category/<slug>, /author/<slug>", kota: "5 services x kota; folder: /cuci-sarang-walet-<kota>, /distributor-sarang-walet-<kota>, /konsultan-walet-<kota>; root .html: /bangun-gedung-walet-<kota>.html", page: "/<slug> folder/index.html, no trailing slash", desain: "/desain-gedung-walet-<kota>.html (root .html)" }
chrome: { header_lines: "222-362", footer_lines: "606-666", byte_identical: "variants:2" }
analytics: { gtm: "GTM-MLS8Q69", ga4_property_id: "", tiktok: "", fb_verify: "", adsense: "" }
brand: { primary: "#5089c6", font: "Open Sans + DM Serif Display", logo: "/wp-content/uploads/2021/08/LOGO-SARANGWALET-HITAM.png", favicon: "/wp-content/uploads/2021/08/FAVICON-SARANGWALET.png" }
ecosystem_menu: present
contact: { wa: "6285235350662", email: "sales@markaswalet.com" }
media: { local_mb: 61, hotlink: "blogspot:74 (unique urls) across 675 files" }
content: { articles: 33, kota_unique: 102, kota_admin_level: kota }
cleanup:
  - "og:url masih menunjuk domain sandbox http://fierce-starling.w5.wpsandbox.pro (canonical sudah bersih /) -> normalisasi ke domain produksi saat regen head"
  - "74 gambar hotlink blogspot/blogger dipakai di 675 file (termasuk og:image) -> rehome ke R2"
  - "0 webp (773 jpg + 56 png + 14 svg + 7 jpeg) -> peluang optimasi format"
  - "solo & surakarta = kota sama, 2 slug terpisah -> putuskan gabung/pertahankan"
  - "tidak ada robots.txt & sitemap.xml di root -> sediakan"
  - "fitur dinamis mati: WPForms kontak (form id 6), search box, komentar -> tanpa backend"
blockers:
  - "Konfirmasi Cloudflare: Pages vs CDN-only, akun #1 atau #2, deploy hook ada/tidak"
  - "Konfirmasi branch produksi = main"
  - "Pilih mekanisme akses tulis CMS (rekomendasi: GitHub App scope repo ini, contents:write)"
---

# Catatan kesiapan integrasi CMS — sarangwalet.web.id

Situs ini adalah **HTML statis hasil export WordPress 6.0** (tema Astra + Elementor +
WPForms). 910 file `.html`, ± 61 MB media. **Tanpa API, tanpa database, tanpa build system.**
Sumber kebenaran = repo git ini. Scope integrasi **aditif**: CMS generate/edit konten lalu
commit ke repo; navbar/menu/footer & URL tidak diubah.

## Deploy (BLOCKER)
Dari repo hanya bisa dipastikan: sumber git, ada branch `main` (dugaan produksi) + branch kerja
`claude/markaswalet-centralized-cms-egkdst`, dan ada folder `cdn-cgi/` (jejak Cloudflare).
**Tidak ada** config deploy tersimpan di repo (`.github/workflows`, `wrangler.toml`,
`_redirects`, `_headers`, `CNAME`). Wiring repo->live ada di dashboard, perlu konfirmasi pemilik
(lihat `blockers`).

## Chrome (2 varian — jangan diperlakukan sebagai satu blob)
Header ± 18 KB (baris 222-362) + footer ± 11 KB (baris 606-666) **terduplikasi di tiap file**,
**tidak byte-identik**:
- **Homepage**: logo putih transparan (`LOGO-SARANGWALET-PUTIH`), judul `<h1>`.
- **Halaman dalam**: logo hitam (`cropped-LOGO-SARANGWALET-HITAM` + srcset), judul `<span>`.
- Plus active-state menu (`current-menu-item`/`aria-current`) berpindah per halaman.
Template chrome kanonik (varian halaman dalam) + slot `<!-- CONTENT -->` sudah diekstrak
terpisah (`_layout.html`, dikirim ke koordinator, tidak di-commit ke repo situs).
Menu memuat tautan ekosistem eksternal: GedungWalet.com, ParfumWalet.com, BudidayaWalet.net,
CuciWalet.com.

## Konten & kota
- **service_type wajib jadi dimensi**: 5 layanan x 102 kota. Perhatikan 2 gaya URL dalam satu
  dimensi (cuci/distributor/konsultan = `folder/`; bangun/desain = `*.html` root).
- **Dataset kota master 102 kota** (slug + nama + provinsi) sudah disiapkan
  (`cities-master.json`, diverifikasi 0-selisih vs folder repo) — kandidat dataset bersama
  lintas situs.
- `articles: 33` = halaman editorial/landing topik (`walet-untuk-*` 20, `rumah-walet-*` material 6,
  `hama-*` 4, `sarang-*` 3). `/blog` & `/berita` masih index; ekstraksi per-post terstruktur
  (title/body/meta/tanggal/kategori/author/cover) menyusul.

## Media
Lokal ± 61 MB di `wp-content/uploads/` (per tahun 2020-2022), path absolut + srcset multi-ukuran.
Hotlink: **74 URL gambar unik** dari blogspot/blogger dipakai di **675 file** (± 2.900 referensi,
termasuk `og:image`) -> rehome ke R2 (prioritas). Nol webp.

## URL/SEO
URL root-absolute tanpa domain, mayoritas tanpa trailing slash. **Jangan ubah slug** (nilai SEO);
sediakan 301 bila perlu. Analytics via `GTM-MLS8Q69`.
