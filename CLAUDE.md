# CLAUDE.md — AI Assistant Guide for applimits-pages

## Project Overview

This is the **static website** for [Applimits](https://applimits.com), an iOS/macOS screen time management app by ButterFalcon. The site is hosted on **GitHub Pages** with the custom domain `applimits.com`.

The site provides:
- FAQ documentation (interactive accordion-style)
- Privacy Policy
- Terms of Use
- Multi-language support (6 languages)

## Tech Stack

- **Pure HTML/CSS/JavaScript** — no build tools, no package manager, no framework
- **GitHub Pages** for hosting (custom domain via `CNAME` file)
- No `package.json`, no npm dependencies, no build step

## Directory Structure

```
/
├── CNAME                    # GitHub Pages custom domain (applimits.com)
├── styles.css               # Shared CSS for privacy policy & terms pages
├── artwork.jpg              # App logo (512x512)
├── index.html               # Root index (empty)
├── faq.html                 # FAQ — Japanese (default language)
├── privacypolicy.html       # Privacy Policy — Japanese
├── termsofuse.html          # Terms of Use — Japanese
├── en/                      # English
│   ├── faq.html
│   ├── privacypolicy.html
│   ├── termsofuse.html
│   └── index.html
├── es/                      # Spanish
│   ├── faq.html
│   ├── privacypolicy.html
│   └── termsofuse.html
├── ko/                      # Korean
│   ├── faq.html
│   ├── privacypolicy.html
│   └── termsofuse.html
├── zh-Hans/                 # Simplified Chinese
│   ├── faq.html
│   ├── privacypolicy.html
│   └── termsofuse.html
├── zh-Hant/                 # Traditional Chinese
│   ├── faq.html
│   ├── privacypolicy.html
│   └── termsofuse.html
└── sample/                  # Static HTML snapshot (reference only)
    ├── FAQ.html
    └── FAQ_files/
```

## Localization

- **Japanese** is the default language, served from the root directory (`/faq.html`, etc.)
- Other languages live in subdirectories: `en/`, `es/`, `ko/`, `zh-Hans/`, `zh-Hant/`
- Each language directory contains the same set of pages: `faq.html`, `privacypolicy.html`, `termsofuse.html`
- The `<html lang="...">` attribute must match the language (e.g., `lang="en"`, `lang="ja"`, `lang="es"`, `lang="ko"`, `lang="zh-Hans"`, `lang="zh-Hant"`)

**When adding or modifying content, always update all 6 language versions.**

## Styling Conventions

### FAQ Pages
- **Inline `<style>` tags** — each FAQ page contains its own embedded CSS
- Uses CSS custom properties (variables) defined in `:root`
- Light mode defaults with `@media (prefers-color-scheme: dark)` overrides
- iOS design language: `--primary-color: #007AFF`, rounded corners (`border-radius: 10px`), card-based layout

### Privacy Policy & Terms of Use Pages
- Use the **shared `styles.css`** file (referenced via `../styles.css` from subdirectories)
- Simpler styling with the same responsive approach

### Design System
- Font stack: `-apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI", Roboto, Helvetica, Arial, sans-serif`
- Max container width: `800px`, centered
- Mobile-first responsive design with breakpoints at `600px`
- iOS web app meta tags: `apple-mobile-web-app-capable`, `apple-mobile-web-app-status-bar-style`

## JavaScript

Minimal JS is used — only for the FAQ accordion toggle:

```javascript
document.addEventListener('DOMContentLoaded', function () {
    const questions = document.querySelectorAll('.faq-question');
    questions.forEach(question => {
        question.addEventListener('click', function () {
            const answer = this.nextElementSibling;
            this.classList.toggle('active');
            if (this.classList.contains('active')) {
                answer.style.maxHeight = answer.scrollHeight + 'px';
            } else {
                answer.style.maxHeight = null;
            }
        });
    });
});
```

This script is **duplicated inline** in every FAQ page. When modifying accordion behavior, update it in all FAQ files.

## Development Workflow

1. **No build step** — edit HTML/CSS/JS files directly
2. **Preview locally** — open HTML files in a browser (or use a local HTTP server for correct relative paths)
3. **Deploy** — push to `main` branch; GitHub Pages serves files automatically
4. **Custom domain** — configured via the `CNAME` file pointing to `applimits.com`

## Key Conventions

- **No build tools or preprocessors** — keep it simple, pure HTML/CSS/JS
- **Self-contained pages** — FAQ pages embed their own styles and scripts inline
- **Consistent structure across languages** — every language directory mirrors the same file set
- **iOS-native look and feel** — follow Apple's Human Interface Guidelines for colors, typography, and component styling
- **Dark mode support** — all pages must work in both light and dark modes via `prefers-color-scheme`
- **Semantic HTML** — use `<header>`, `<main>`, `<section>`, `<footer>` elements
- **Responsive** — all pages must work on mobile viewports (test at 375px width minimum)

## Common Tasks

### Adding a new FAQ entry
1. Add the question/answer HTML to `faq.html` in the root (Japanese)
2. Replicate in all language directories (`en/`, `es/`, `ko/`, `zh-Hans/`, `zh-Hant/`) with translated content
3. Follow the existing `.faq-item` / `.faq-question` / `.faq-answer` HTML pattern

### Adding a new language
1. Create a new directory (e.g., `fr/` for French)
2. Copy all page files from an existing language directory
3. Translate content and update the `lang` attribute on `<html>`
4. Update relative paths (e.g., `../styles.css` for shared CSS)

### Modifying shared styles
- Edit `styles.css` for privacy policy and terms of use pages
- For FAQ pages, update the inline `<style>` block in **each** FAQ file across all languages

## Files to Ignore

- `sample/` — static HTML snapshot for reference, not part of the live site
- `index.html` files — currently empty placeholders
