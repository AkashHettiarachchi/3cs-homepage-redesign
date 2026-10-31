# 3CS: "a state of mind" · Concept Homepage

A single-page concept redesign of the 3CS homepage (web design, web development, AI agents, website security and digital marketing). It is one static `index.html` with no build step and no framework, plus a few media files.

## Project structure

```
.
├── index.html            # the whole site: markup, CSS and JS
├── favicon.ico           # browser tab icon (16/32/48 px)
├── icons/
│   ├── favicon-16.png
│   ├── favicon-32.png
│   ├── favicon-48.png
│   ├── apple-touch-icon.png   # 180 px, iOS home screen
│   ├── icon-192.png
│   └── icon-512.png
└── videos/
    ├── project-01.mp4 / .jpg  # portfolio card backgrounds (+ poster frames)
    ├── project-02.mp4 / .jpg
    └── project-03.mp4 / .jpg
```

Keep this folder structure intact. `index.html` loads the icons and videos by relative path.

## Running it

1. Keep `index.html`, `favicon.ico`, `icons/` and `videos/` together.
2. Open `index.html` in a modern browser, or upload the folder to any static host (Netlify, Vercel, GitHub Pages, cPanel, S3 and so on).
3. An internet connection is needed for the Google Fonts and the Three.js library (see Dependencies).

## Page sections

| Section | Anchor | What it does |
|---|---|---|
| Header | n/a | Glass nav, email button, theme picker (accent colour and light/dark/system), mobile menu |
| Hero | `#top` | Wide display headline (outlined and gradient words), CTAs, 3D scene behind |
| Marquee | n/a | Two counter-moving rows; scroll speeds them up and skews the band; hover slows a row |
| About | `#about` | Intro, rotating ring, count-up stat strip |
| Services | `#services` | Bento grid of 5 cards, each with its own live animation |
| Portfolio | `#work` | 3 cards with muted 4-second looping video backgrounds |
| Why 3CS | `#why` | 4 expanding panels (Awards, Clients, Security, Investors) |
| Journey | `#journey` | Scroll-driven timeline: a glowing dot travels the line and lights each step |
| Contact | `#contact` | Call-to-action band |

The tiles `#awards`, `#clients` and `#investors` inside Why 3CS are also nav targets.

## Features

- **3D background:** a fixed Three.js scene (orb, orbit rings, floating shapes, particles) that reacts to pointer and scroll.
- **Theming:** 5 accent palettes (3CS Teal, Ocean, Deep Navy, Aqua Mint, Slate Teal) and Light / Dark / System modes, saved in `localStorage`. Colours are CSS variables (`--p`, `--p2`, `--q`, `--acc`, `--grad`, and so on).
- **Typography:** headings use Michroma (wide, extended, uppercase); body uses Inter and Outfit; the About headline and the marquee's top row use Fraunces.
- **Performance:** videos and card animations only run while on screen.
- **Accessibility:** decorative visuals are `aria-hidden`; `prefers-reduced-motion` is respected for the newer animations (the page then shows a still final state).
- **Responsive:** breakpoints around 1100, 960, 700 and 560 px. The hero heading and section headings scale with the viewport width.

## Dependencies (loaded from CDNs)

| Library | Used for | Source |
|---|---|---|
| Three.js r160 | 3D background | jsDelivr (via an import map) |
| Outfit, Inter, Fraunces, Michroma | Typography | Google Fonts |

There are no npm packages and no build tooling.

## Customising

All CSS lives in the single `<style>` block and all scripts sit near the end of `index.html`. Later CSS blocks intentionally override earlier ones, so make edits in the newest block for a given component.

- **Hero letter spacing:** change the number in `h1{letter-spacing:.04em}` (`.02em` is tighter, `.06em` is looser).
- **Heading font:** edit the `h1,h2,h2.serif{font-family:'Michroma',...}` rule and the matching Google Fonts `<link>`.
- **Portfolio videos:** replace `videos/project-0X.mp4` and its `.jpg` poster, keeping the names. To make a silent 4-second clip:
  ```bash
  ffmpeg -t 4 -i input.mp4 -an -vf "scale=1280:720,fps=24" \
    -c:v libx264 -crf 27 -pix_fmt yuv420p -movflags +faststart videos/project-01.mp4
  ffmpeg -i input.mp4 -vframes 1 -vf "scale=960:-1" -q:v 4 videos/project-01.jpg
  ```
- **Favicon:** regenerate `favicon.ico` and the files in `icons/` from your logo, keeping the same names.
- **Stats and copy:** the numbers in the About stat strip, the Why 3CS panels (50+, 100+) and the "+248%" in the Digital Marketing card are placeholders. Replace them with confirmed figures.
- **Contact details:** `info@3cs.lk` and `+94 759 100 662` appear in the header button and the contact band.

### CSS variable names to avoid

`--a` and `--b` hold the two colours of each swatch in the theme picker, and `--ang` drives the spinning card borders. Do not reuse these names for something else, especially not in an `@property` rule, or the swatches or borders will break.

## Known issues and to-do

- **CDN dependency:** the theme picker, scroll-reveal animations, counters and 3D scene share one `<script type="module">`. If the Three.js CDN fails to load, those features stop and the `.reveal` content stays invisible. Moving the 3D code into a guarded dynamic `import()` would fix this.
- **Nav highlight:** the active-link logic only watches direct children of `<main>`, so Clients, Awards and Investors never highlight, and the nav order differs from the page order.
- **Theme flash:** the HTML defaults to dark before JavaScript applies the saved or system theme, so light-mode visitors may see a brief dark flash.
- **Placeholder content:** the Investors, Clients and Awards panels are one-line placeholders with no linked pages.
- **SEO and social:** no Open Graph or Twitter tags, canonical URL or structured data yet.
- **Unverified claims:** confirm the awards, audits, agents, projects and years-of-experience figures before publishing.

## Browser support

Current Chrome, Edge, Safari and Firefox. The page uses `:has()`, `@property`, `color-mix()`, CSS masks and import maps; older browsers will lose some visual effects.

## Status

Concept redesign. The portfolio video backgrounds stand in for approved project imagery.
