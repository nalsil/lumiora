# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static website for **Lumiora** (루미오라), a Korean software company. Hosted on GitHub Pages at `www.lumiora.co.kr`. No build step required — files are served as-is.

## Development

No package manager or build tools. To preview locally:

```bash
python -m http.server 8000
# or
npx http-server
```

## Architecture

### Routing

The root `index.html` is **not** a content page — it is a language dispatcher. It runs inline JS to read `navigator.language` and `window.location.replace()`s to `en/index.html` (browser language starting with `en`) or `ko/index.html` (everything else), with a `<noscript>` refresh and manual language buttons as fallback.

Localized content lives in two parallel directories — `ko/` (Korean) and `en/` (English) — each holding the same three pages:

| Page | Purpose |
|------|---------|
| `index.html` | Company homepage |
| `myungkyung.html` | Product page for the 명경 / MyungKyung app |
| `notivault.html` | Product page for the NotiVault app |

When editing content, keep each page in sync across `ko/` ↔ `en/`.

### NotiVault privacy policy

`notivault/privacy/` is a self-contained, 24-language privacy policy:

- `index.html` is a language dispatcher (same pattern as root): it resolves `?lang=`/`#hash`/`navigator.languages` against a `SUPPORTED` list with an `ALIAS` map (e.g. `iw→he`, `zh→zh-CN`, `pt→pt-BR`) and redirects to `privacy-<code>.html`. English (`privacy-en.html`) is the default.
- `privacy-<code>.html` are the per-language pages; all share `privacy.css`.
- `privacy-policy-ko.md` is the Korean source text the HTML pages are derived from.

When the policy text changes, update the markdown source and all `privacy-*.html` files; if you add/remove a language, also update the `SUPPORTED`/`ALIAS` arrays in `index.html`.

### Static assets

Images live in `images/`, with per-product subfolders (`images/myungkyung/`, `images/notivault/`) holding app screenshots referenced by the product pages.

### Design System

Tailwind CSS is loaded via CDN (`cdn.tailwindcss.com`, some pages with `?plugins=forms,container-queries`). Each content page contains an inline `tailwind.config` block defining shared tokens:

- **Primary color:** `#1754cf` (blue)
- **Backgrounds:** `#f6f6f8` (light), `#111621` (dark)
- **Fonts:** Space Grotesk + Noto Sans KR (display), Noto Sans KR (body)
- **Dark mode:** `class`-based

The NotiVault privacy pages are an exception — they use plain CSS (`privacy.css`) with `prefers-color-scheme` dark mode, not Tailwind.

Content pages follow the same section layout: sticky header (logo + nav + language switcher) → hero → content sections → footer.

## Key Details

- **Company:** 루미오라 / Lumiora — 대표 박성진, 사업자등록번호 376-09-03284. Full company details (Korean) in `doc/01.홈페이지정보.txt`.
- **Contact email:** contact@lumiora.co.kr
- **Domain config:** `CNAME` routes `www.lumiora.co.kr` to GitHub Pages
- **AdMob config:** `app-ads.txt` contains the publisher ID for Google ad network verification
- **Edit requests:** ad-hoc change requests from the owner are logged in `doc/prompts/` (Korean)
