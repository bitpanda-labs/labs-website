# CLAUDE.md — Bitpanda Labs Website

## Project overview

Jekyll static site hosted on GitHub Pages at `labs.bitpanda.com`. Showcases internal Bitpanda R&D experiments. No build pipeline beyond Jekyll — pure HTML, CSS, vanilla JS.

## Build & dev commands

```bash
# Serve locally
bundle exec jekyll serve --port 4000

# Build only
bundle exec jekyll build
```

Ruby 3.3 is required (`.ruby-version` specifies `3.3`). If `bundle` is not found, install it with `gem install bundler` and re-run.

## Commit conventions

This project uses [Conventional Commits](https://www.conventionalcommits.org/).

- No period at end of subject line
- No commit body — subject line only
- No Anthropic/Claude attribution in commits
- Ticket references go in the body or as a footer, not the subject
- Branch names use `type/short-description` — e.g. `fix/clickable-experiment-cards`, `feature/ui-modernisation`

## Architecture

| Path | Purpose |
|------|---------|
| `_layouts/default.html` | Base layout — nav, footer, cookie consent, analytics, all global JS |
| `_layouts/experiment.html` | Individual experiment article layout |
| `assets/css/style.css` | Single stylesheet (~1800 lines) — all styling |
| `index.html` | Homepage — hero, carousel, about section |
| `experiments.html` | All experiments grid listing |
| `about.html` | About page |
| `_experiments/` | Experiment content (one folder per experiment) |
| `_config.yml` | Jekyll config — URL, collections, future-post control |

## Adding a new experiment

1. Copy `_experiments/_template.md` into a new folder: `_experiments/YYYY_MM_DD_name/index.md`
2. Add `image.jpg` (hero image, landscape, ideally 16:9) to the same folder
3. Front matter fields:
   ```yaml
   layout: experiment
   title: "Title"
   date: YYYY-MM-DD        # Future date = scheduled, won't publish until that date
   categories: ["AI & Machine Learning", "Developer Experience"]
   excerpt: "One or two sentences shown on cards and in meta tags."
   action: "Learn more"   # or "Try out"
   ```
4. Write content in Markdown below the `---`

Available categories are documented in `_experiments/README.md`.

## Design system

### Brand colors
```css
--panda-green: #103e36       /* Primary — dominant background and text */
--panda-black: #1a1a1a       /* Body text */
--panda-white: #ffffff
--panda-grey: #f8f9fa        /* Light section backgrounds */
--panda-grey-dark: #6c757d   /* Secondary text, metadata */
--panda-link: #16764d        /* In-article links */
/* Mint #2cec9a — accent only, use very sparingly */
```

### Fonts
- **Headlines/display**: `'Bitpanda Compressed'` (local TTF, uppercase, tight line-height)
- **Body/UI**: `'Inter'` (Google Fonts, weights 400/700/800)
- **Nav brand**: `'Euclid Square'` (local TTF, "Labs" wordmark only)

### Animation system
All animations use CSS custom properties defined in `:root`:
```css
--ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1)   /* Apple-signature curve */
--ease-out-quart: cubic-bezier(0.25, 1, 0.5, 1)
--duration-fast/normal/slow/reveal
--stagger-offset: 0.12s
```

Scroll-triggered reveals use the `.reveal` / `.reveal-children` CSS + Intersection Observer pattern (JS is in `_layouts/default.html`). Elements start hidden only when `body.js-ready` is present — graceful degradation when JS is off.

### Key CSS patterns
- Cards: `border-radius: 24px`, `background: #f3f3f2`, `box-shadow: var(--shadow-light)`
- Buttons: `border-radius: 100px` (pill shape)
- Sections use `scroll-margin-top: 80px` for fixed-header offset
- Carousel shadow clearance: `padding: 1.5rem 1.5rem 2rem 1.5rem; margin: -1.5rem -1.5rem 0 -1.5rem` on `.carousel-track` — do not remove

## Important CSS rules — do not break

- `!important` on carousel dot `display: none` in mobile breakpoint is intentional — it overrides inline JS styles
- `.post-card` has no fixed height — cards size to content, grid equalizes heights in each row
- `.carousel-item` is `display: flex` with child `.card` set to `flex: 1` — required for equal card heights in carousel
- Header uses `body.has-hero > header` (transparent over hero) / `body.has-hero.header-scrolled > header` (glass) pattern — only homepage gets `has-hero`
- Cookie banner uses `visibility: hidden/visible` + `pointer-events` (not `display: none/block`) so the slide-up transition actually plays

## Analytics

RudderStack (consent-gated). Write key: `38CkLWnKmZjzNGHOIsGgd4NPjK8`. Only loads after cookie acceptance. The `loadRudderStack()` function is defined in `_layouts/default.html` and calls `rudderanalytics.page()`.

## Deployment

Push to `main` → GitHub Actions builds Jekyll → deploys to GitHub Pages automatically. The workflow is in `.github/workflows/deploy.yml`.
