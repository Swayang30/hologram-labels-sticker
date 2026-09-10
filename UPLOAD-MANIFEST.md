# Holoflex — Hologram Labels landing page: upload manifest

Deploys to **https://www.holoflex.com/hologram_labels/**
Server folder: `/home/holoflex/public_html/hologram_labels/`

Content swap of the approved Self-Adhesive Labels build. Same design, layout,
CSS, JS, PHP endpoint and Apps Script integration. Built 2026-09-10. Not deployed.

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

Total: 18 files. Keep the folder structure exactly as above — every path in
the HTML, CSS and JS is relative to `/hologram_labels/`.

No product photography ships with this build (see section 4); the page renders
neutral placeholder tiles until the photos arrive.

**Do NOT upload:** `_source/` (Vercel preview copy) or this manifest.

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

1. Open `https://www.holoflex.com/hologram_labels/` — fonts, logos and the
   placeholder tiles render; the header nav anchors (`#authentication`,
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

## 4. Photography still needed (placeholder slots)

Every product image slot renders a neutral `.lp-ph` tile with the final
`<picture>` markup commented in beside it. To drop a photo in: add the files
below to `images/`, delete the `.lp-ph` div and uncomment the `<picture>`.
The 1x file must be ≤ the size shown; the 2x file may be roughly double.
Supply JPG + WebP for each.

| Slot | Files (JPG + WebP each) | Size (1x / 2x) | Subject |
|---|---|---|---|
| Core block figure | `lp-hologram-features-460`, `-920` | 460×287 / 920×574, ≤60 KB | Security hologram label on a carton: overt colour-shift beside a covert element under a decoding film |
| Security block | `lp-security-600`, `-1200` | 600×450 / 1200×900, ≤80 KB | Tamper-evident hologram label partly lifted to show the VOID pattern, with a serial number and QR code |
| Gallery: Pharmaceutical | `lp-app-pharma-400`, `-800` | 400×267 / 800×534, ≤25 KB | Hologram label on a pharma carton beside a blister strip |
| Gallery: Automotive Spares | `lp-app-automotive-spares-400`, `-800` | 400×267 / 800×534, ≤25 KB | Hologram label on a spare-parts box with a warranty seal |
| Gallery: Agrochemicals | `lp-app-agrochemicals-400`, `-800` | 400×267 / 800×534, ≤25 KB | Hologram label on an agrochemical drum beside a can and a sachet |
| Gallery: Electricals & Appliances | `lp-app-electricals-appliances-400`, `-800` | 400×267 / 800×534, ≤25 KB | Hologram warranty seal on an appliance carton |
| Gallery: Liquor & Beverage | `lp-app-liquor-beverage-400`, `-800` | 400×267 / 800×534, ≤25 KB | Hologram label across the cap and neck of a bottle |
| Gallery: Certificates & Documents | `lp-app-certificates-documents-400`, `-800` | 400×267 / 800×534, ≤25 KB | Security hologram on a certificate beside an ID pass and a licence |
| Hero background (optional) | `lp-hero-holograms-1600.jpg` via `.lp-hero__bg` in the CSS | ≤200 KB | Macro of a security hologram on a pack or reel |

The security-block CSS box cover-crops, so a native 3:2 photo (600×400 /
1200×800) also works there, as it did on the Self-Adhesive Labels build. If a
3:2 file is used, change the `<img>` `height` attribute to match.

## 5. Vercel preview copy

`_source/vercel-preview/` is the review build, same pattern as the two earlier
pages: root-absolute paths, Google Tag Manager removed, `[PREVIEW]` in the
titles, `noindex, nofollow` plus an `X-Robots-Tag` header from `vercel.json`,
and the forms validate but do not submit (they show a notice instead). It
contains no PHP. Push that folder to its own GitHub repo and import it into
Vercel as a static project, as was done for the Garment Tags and Self-Adhesive
Labels previews. Not deployed yet.
