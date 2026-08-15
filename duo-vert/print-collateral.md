---
name: project-duovert-print-collateral
description: "Duo Vert door-to-door print kit (flyer 5x7, door hanger, business card) — Vistaprint glossy cardstock, current design state and open feedback"
metadata: 
  node_type: memory
  type: project
  originSessionId: 1f0b5c08-2c27-432c-90ca-34b7b04ef3a6
  modified: 2026-08-13
---

Building a 3-piece print kit for door-to-door sales: flyer, door hanger, business card — glossy cardstock via Vistaprint. Session started 2026-08-11.

**Why:** Emile & Beckett go door-to-door; existing flyer (photo-heavy Canva-style) was outdated and needed real reviews + a cleaner design. Scope expanded mid-session to the full print kit once the flyer direction stabilized.

**How to apply:** Resume from the flyer (most iterated) before touching door hanger / business card — they were built once as a first draft and haven't been revised since the flyer's design language kept changing. Re-sync door hanger + business card to match the flyer's final look once flyer is signed off.

## Current state (end of session, still in progress — not signed off)

- **Flyer**: 5×7in, latest published artifact is "v11" (file `duovert-flyer-v11.html` in that session's scratchpad — scratchpad paths are session-local and won't persist, rebuild from this memory if needed). Last change: arrow-connector between avant/après photos instead of a boxy white card (user rejected the card look). Not yet confirmed by Emile — resume by asking for reaction to the arrow-connector version.
- **Door hanger** (`duovert-doorhanger.html`, 3.5×8.5in) and **business card** (`duovert-businesscard.html`, 3.5×2in) — built once, early in the process, using an earlier/rougher visual system (before the flyer's font, QR-crop, and mime-bug fixes landed). They need a revisit pass once the flyer is final: same fonts, same clean QR asset, same gold-accent discipline.

## Design system settled on (apply to all 3 pieces)

- Fonts: Fraunces (900, headlines) + Archivo (400-800, body/UI) — both self-hosted via base64 `@font-face` (Google Fonts blocks direct linking in Artifacts' CSP), fetched fresh each session since `/tmp` assets don't persist.
- Palette: warm off-white/stone background (`#f7f3ea`), dark forest green (`#14251a`), single gold accent (`#f2c14e`) used sparingly (borders, one CTA line, icons) — not gradients. A muddy multi-stop gold gradient was explicitly rejected as "looks like dirty 1980s gold."
- Real Google "G" logo (multicolor, official paths) in a small white chip — a hand-drawn stroke-icon version was rejected as "not the real logo."
- One consolidated CTA anchored at the QR block ("Réservez votre soumission gratuite" + "Scannez le code ci-dessus") — a separate boxed "button" CTA was rejected as looking like a clickable web element on a non-clickable print piece.
- No diagonal-stripe/repeating-gradient "texture" — read as dated ("années 80"), removed everywhere.
- Photos: real site photos from `~/Documents/duovert-site/public/` (hero-restauration, avant/apres-pave-uni-gatineau-1 and -2, emile-beckett-final.jpg cropped tight to head+chest+logo — see crop notes below).

## Hard-won technical lessons (avoid repeating)

- **PNG mislabeled as JPEG breaks the QR code silently.** `sips -Z <size> file.png --out file.jpg` does NOT convert format — it just renames a PNG to a `.jpg`-suffixed file and silently keeps PNG bytes ("Output file suffix should be png" warning, easy to miss). Embedding that as `data:image/jpeg;base64,...` produces a mime/content mismatch that some browsers refuse to render — this caused a "the QR code doesn't show" bug that took real debugging to find. **Always verify**: decode the base64 and check magic bytes match the declared mime (`\xff\xd8` for jpeg, `\x89PNG` for png) before publishing. For a real PNG→JPEG conversion use PIL (`Image.open(...).convert('RGB').save(..., 'JPEG')`), not sips.
- **QR source file**: the user's QR image (`~/Downloads/qr code duovert.ca.jpg`, encodes duovert.ca) has a green rounded border + "SCAN ME" text baked into the graphic. Crop tightly inside the white area — original full-res crop box `(115,115)-(1930,1930)` on the 2048×2389 source — to get a clean black/white QR with no green border and no text band, then convert properly to real JPEG.
- **Team photo (Émile & Beckett) crop**: source `emile-beckett-final.jpg` is 878×1560 portrait, full-body, tree canopy fills the top ~30%. For a SMALL circular avatar, a full-body or even chest-to-knee crop looks "stretched" (object-fit:cover on a landscape-ish crop in a small circle still shows nearly the whole body since the box aspect forces height-limited scaling). The crop that actually reads as a clean headshot: `img.crop((x0, 460, x0+680, 830))` where `x0=(878-680)//2` — i.e. a 680×370 landscape-oriented pre-crop showing head+shoulders+polo logo only, nothing lower. Pre-crop tight in Python/PIL rather than relying on CSS object-fit/object-position to do the work — the math doesn't cooperate for a 2-person landscape source photo squeezed into a small circle.
- **Google Fonts fetch**: `curl "https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,900&family=Archivo:wght@500;700;800&display=swap"` — a browser user-agent gets woff2, no UA gets ttf (either works for embedding, just larger file for ttf). Only the "latin" subset block's URL is needed for this project (French + English, no special characters requiring other subsets).
- Vistaprint door hanger size used: 3.5×8.5in (industry-standard assumption, not yet confirmed against Emile's actual Vistaprint checkout template — flag this before he orders).

## Content facts locked in for this kit

- Reviews used: Kilyan Dido (5★, "Excellent service... professionnels et ponctuels...") and Akio (5★, "Incredible work by two respectful young entrepreneurs..." — 2 typos fixed from the original screenshot: "entrepeneurs"→"entrepreneurs", "amd"→"and"). Superseded the earlier Jessy/Jeff review pair used on the website — website testimonials in [[duo-vert/website-build-overview]] are unrelated/unchanged, this is print-only.
- "17+ avis Google" is the trust stat used throughout (badge in hero, review count reference) — not tied to the specific reviewCount used in the website's schema.org markup, don't assume they need to match.
- Active promo: **-15% de rabais de fin de saison, sur les projets qui incluent cour avant ET arrière** (not a standalone per-service discount). Currently expressed via a full-width ribbon under the hero (flat dark-green bg, gold bold "-15%", not the earlier rejected medallion-stamp-on-photo or muddy-gold-gradient versions).
- No phone number available for the business card back — only duovert.ca + duo.vert.gatineau@gmail.com. Ask Emile if he wants a phone number added.
