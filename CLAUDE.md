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

Four HTML pages with parallel Korean/English versions:

| File | Language | Purpose |
|------|----------|---------|
| `index.html` | Korean | Company homepage |
| `myungkyung.html` | Korean | Product page for 명경 app |
| `en/index.html` | English | Company homepage |
| `en/myungkyung.html` | English | Product page for MyungKyung app |

Static assets (images) live in `images/`.

### Design System

Tailwind CSS is loaded via CDN. Each HTML file contains an inline `tailwind.config` block defining shared tokens:

- **Primary color:** `#1754cf` (blue)
- **Backgrounds:** `#f6f6f8` (light), `#111621` (dark)
- **Fonts:** Space Grotesk + Noto Sans KR (display), Noto Sans KR (body)
- **Dark mode:** `class`-based

### Page Structure

All pages follow the same section layout: sticky header (logo + nav + language switcher) → hero → content sections → footer.

When editing content, keep both `index.html` ↔ `en/index.html` and `myungkyung.html` ↔ `en/myungkyung.html` in sync.

## Key Details

- **Contact email:** contact@lumiora.co.kr
- **Domain config:** `CNAME` file routes `www.lumiora.co.kr` to GitHub Pages
- **AdMob config:** `app-ads.txt` contains publisher ID for Google ad network verification
- **Company info:** See `doc/01.홈페이지정보.txt` for full company details (Korean)
