# Holoflex — Hologram Labels landing page: upload manifest

Deploys to **https://www.holoflex.com/hologram_labels/**
Server folder: `/home/holoflex/public_html/hologram_labels/`

Content swap of the approved Self-Adhesive Labels build. Same design, layout,
CSS, JS, PHP endpoint and Apps Script integration. Built 2026-09-10; product
photography added and the security-block crop fixed later the same day. Not deployed.

## 1. Upload these (and nothing else) to `/hologram_labels/`

| Path | Purpose |
|---|---|
| `index.html` | Landing page |
| `thank-you-lp.html` | Post-submit confirmation page (fires the conversion once) |
| `submit-enquiry.php` | Enquiry endpoint: CSV → Apps Script webhook → mail() |
| `css/hologram-labels-lp.css` | Standalone stylesheet (renamed from `self-adhesive-labels-lp.css`) |
| `js/hologram-labels-lp.js` | Page script (renamed from `self-adhesive-labels-lp.js`) |
| `fonts/lp-poppins-400.woff2` | Self-hosted font |
| `fonts/lp-poppins-500.woff2` | Self-hosted font |
| `fonts/lp-poppins-600.woff2` | Self-hosted font |
| `fonts/lp-poppins-700.woff2` | Self-hosted font |
| `fonts/lp-lora-700.woff2` | Self-hosted font |
| `images/lp-holoflex-logo-64.png` / `.webp` | Header logo 1x |
| `images/lp-holoflex-logo-128.png` / `.webp` | Header logo 2x |
| `images/lp-holoflex-logo-footer-80.jpg` / `.webp` | Footer logo 1x |
| `images/lp-holoflex-logo-footer-160.jpg` / `.webp` | Footer logo 2x |
| `images/img1.jpeg` | Core block figure (1600×800, single file — see section 4) |
| `images/img2.jpeg` | Security block photo (1600×800, single file — see section 4) |
| `images/lp-app-pharma-400.jpg` / `.webp` | Gallery photo 1x (400×267) |
| `images/lp-app-pharma-800.jpg` / `.webp` | Gallery photo 2x (800×534) |
| `images/lp-app-automotive-spares-400.jpg` / `.webp` | Gallery photo 1x (400×267) |
| `images/lp-app-automotive-spares-800.jpg` / `.webp` | Gallery photo 2x (800×534) |
| `images/lp-app-agrochemicals-400.jpg` / `.webp` | Gallery photo 1x (400×267) |
| `images/lp-app-agrochemicals-800.jpg` / `.webp` | Gallery photo 2x (800×534) |
| `images/lp-app-electricals-appliances-400.jpg` / `.webp` | Gallery photo 1x (400×267) |
| `images/lp-app-electricals-appliances-800.jpg` / `.webp` | Gallery photo 2x (800×534) |
| `images/lp-app-liquor-beverage-400.jpg` / `.webp` | Gallery photo 1x (400×267) |
| `images/lp-app-liquor-beverage-800.jpg` / `.webp` | Gallery photo 2x (800×534) |
| `images/lp-app-certificates-documents-400.jpg` / `.webp` | Gallery photo 1x (400×267) |
| `images/lp-app-certificates-documents-800.jpg` / `.webp` | Gallery photo 2x (800×534) |

Total: 44 files, 1,555 KB on disk. Keep the folder structure exactly as above —
every path in the HTML, CSS and JS is relative to `/hologram_labels/`.

Page weight for a WebP-capable browser, third-party GTM script excluded:
726 KB at 1x (HTML, CSS, JS, fonts and logos 172 KB; core and security photos
464 KB; six gallery tiles 90 KB) and 923 KB at 2x (gallery tiles 277 KB).

**Do NOT upload:** `_source/` (Vercel preview copy and photo originals) or this manifest.

## 2. What changed in `submit-enquiry.php` vs Self-Adhesive Labels

| Constant | Value |
|---|---|
| `LP_PAGE_ID` | `hologram-labels` |
| `LP_CSV_FILE` | `hologram-labels-enquiries.csv` |
| `LP_PAGE_URL` | `https://www.holoflex.com/hologram_labels/` |
| `LP_SUBJECT` | `New hologram label enquiry` |
| `$allowedInterests` | Five hologram options matching the two `<select>`s in `index.html` |

Unchanged, exactly as the earlier builds: `LP_DATA_DIR` (`/home/holoflex/lp-data`),
`LP_WEBHOOK_URL`, `LP_WEBHOOK_SECRET`, `LP_RATE_MAX` / `LP_RATE_WINDOW`,
`LP_MIN_ELAPSED_MS`, `LP_TIMEZONE`, recipients and From address. The campaign
attribution fields (gclid + five UTMs), the optional City field and the
structured plain-text email body are all carried over unchanged.

Same Apps Script deployment, same Google Sheet. The `Page ID` column (B)
separates these leads from the other two pages. The CSV lands as a **new file**
in `/home/holoflex/lp-data/` on the first lead; the shared `rate-limit.json`
and `lp-errors.log` are reused.

**One thing to check in Apps Script.** `Code.gs` currently lists the third page
in `PAGE_NAMES` as `'holograms': 'Holograms'`, but this build sends
`hologram-labels` (as specified). The script accepts unlisted ids without
losing the lead — it just shows the slug verbatim in the Sheet and email
instead of a friendly name. To get "Hologram Labels" as the display name,
change that line to `'hologram-labels': 'Hologram Labels'` and deploy a new
version. Nothing else in the script needs to change.

## 2a. Why Holoflex — the only wording that differs from Self-Adhesive Labels

The eight card **titles** are live Google Ads callouts and are verbatim on
every landing page. The same three card **descriptions** were reworded, one
noun each, following the pattern set on the Self-Adhesive Labels build.

| Card | Self-Adhesive Labels description | Hologram Labels description |
|---|---|---|
| Direct From Manufacturer | Labels, substrates and security features made in-house — no reseller margin. | Holograms, films and security features made in-house — no reseller margin. |
| Trusted by Top Brands | Pharma, FMCG and consumer brands across India. | Pharma, automotive and consumer brands across India. |
| Free Samples & Quote | Handle the label before you commit. Pricing within 24 hours. | Handle the hologram before you commit. Pricing within 24 hours. |

The other five descriptions (35+ Years' Experience, Custom Designs, Bulk
Orders Welcome, Made in India, Fast Turnaround) are unchanged.

## 2b. Copy notes

Only the supplied content was used, with three exceptions that had no text in
the brief and were written to fit the existing slots:

- **Problem block heading and lede** ("When a Pack Has Nothing the Buyer Can
  Check." plus a two-sentence lede). The four tiles are verbatim.
- **CTA band A** (the dark band after the core block). The brief supplied one
  mid-page band, which went to band B; band A carries the product summary
  ("Overt • Covert • Forensic — …") and the standard "Free samples and a
  customised quotation within 24 hours." line.
- **Problem tile 04** originally read "look like warning stickers". The brief
  also bans the word "sticker" anywhere in the copy, so it now reads "look like
  warning labels". Everything else in that tile is verbatim.

Also changed to fit the page: nav item 1 is "How It Works" (anchor
`#authentication`, the core block); nav item 2 is "Brand Protection" (anchor
`#security`, unchanged id). The five product-interest options in both forms
are new for this page and mirror `$allowedInterests` in the PHP.

The banned terms from the brief ("prevent counterfeiting", "duplication nearly
impossible", "highly counterfeit-resistant", "ensures", "unparalleled",
"leading", "top", "highest standards", "defeat duplicators", "hologram
sticker") do not appear anywhere in the copy. The only occurrence of "Top" is
the callout title "Trusted by Top Brands", which is a live Google Ads asset
and verbatim on all three pages. British spelling throughout.

## 3. Post-upload checks

1. Open `https://www.holoflex.com/hologram_labels/` — fonts, logos and all
   eight photographs render; the header nav anchors (`#authentication`,
   `#security`, `#applications`, `#why-holoflex`, `#faq`, `#enquiry-footer`)
   all scroll.
2. Submit a test enquiry from the hero form and one from the footer form.
   - Redirects to `thank-you-lp.html`; the `enquiry_form_submit` dataLayer
     event carries `page_id: "hologram-labels"`.
   - A row appears in the Sheet with Page ID `hologram-labels`.
   - The notification email arrives with subject
     `New hologram label enquiry — <company> (hero form)`.
   - `/home/holoflex/lp-data/hologram-labels-enquiries.csv` exists.
3. Append `?gclid=test&utm_source=google&utm_campaign=hol-test` to the URL,
   submit, and confirm the campaign columns populate.

## 4. Photography

**Gallery (added 2026-09-10).** All six application tiles are in place. Each
original (1536×1024, in `_source/assets/`) was centre-cropped to 3:2 and saved
at 400×267 (≤25 KB) and 800×534 (≤60 KB), JPG + WebP, `lp-` prefixed, lazy-loaded.

| Tile | Original | Output stem |
|---|---|---|
| Pharmaceutical | `lp-app-pharmaceutical.jpg` | `lp-app-pharma` |
| Automotive Spares | `lp-app-automotive.jpg` | `lp-app-automotive-spares` |
| Agrochemicals | `lp-app-agrochemicals.jpg` | `lp-app-agrochemicals` |
| Electricals & Appliances | `lp-app-electricals.jpg` | `lp-app-electricals-appliances` |
| Liquor & Beverage | `lp-app-liquor.jpg` | `lp-app-liquor-beverage` |
| Certificates & Documents | `lp-app-certificates.jpg` | `lp-app-certificates-documents` |

**Core block figure and security block (added 2026-09-10).** These two slots
carry `images/img1.jpeg` and `images/img2.jpeg` (1600×800, ~235 KB each) as
single un-optimised files, not the `lp-` WebP + JPEG pairs the commented-in
`<picture>` markup expects. They render correctly; converting them to
`lp-hologram-features-460/-920` and `lp-security-600/-1200` would cut roughly
350 KB from the page. The commented `<picture>` blocks are still in the HTML
for that swap.

**Security block crop fix (2026-09-10).** `.lp-security__media` no longer has a
fixed height at any breakpoint; the box takes the photo's own aspect ratio, so
callouts near the photo's edges are never cropped. The same change was applied
to the Garment Tags and Self-Adhesive Labels builds.

**Still open.**

| Slot | Files | Size | Subject |
|---|---|---|---|
| Hero background (optional) | `lp-hero-holograms-1600.jpg` via `.lp-hero__bg` in the CSS | ≤200 KB | Macro of a security hologram on a pack or reel |

## 5. Vercel preview copy

`_source/vercel-preview/` is the review build, same pattern as the two earlier
pages: root-absolute paths, Google Tag Manager removed, `[PREVIEW]` in the
titles, `noindex, nofollow` plus an `X-Robots-Tag` header from `vercel.json`,
and the forms validate but do not submit (they show a notice instead). It
contains no PHP. Regenerated from production on 2026-09-10, so it carries the
current copy, all eight photographs and the security-block crop fix; the only
differences from production are the transformations listed above. Push that folder to its own GitHub repo and import it into
Vercel as a static project, as was done for the Garment Tags and Self-Adhesive
Labels previews. Not deployed yet.
