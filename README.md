# Handoff: Tagesmutter Elen – Website-Designvarianten

## Overview
Six alternative homepage designs for the childminder service "Kindertagespflege Elen" (German Tagesmutter; ages 1–3, max. 5 children). They are **awaiting customer review**. Keep all six as separate options. Do not merge, redesign or restyle them.

Note: the request said "five variants", but six currently exist (01–06). All six are included here.

| # | Name | Source file | Static build |
|---|------|-------------|--------------|
| 01 | Kleines Nest (warm, handmade) | `source/Kleines Nest.dc.html` | `static/kleines-nest/index.html` |
| 02 | Bauklötzchen (bold, playful) | `source/Bauklötzchen.dc.html` | `static/bauklotzchen/index.html` |
| 03 | Tagespflege Elen (calm, editorial) | `source/Tagespflege Elen.dc.html` | `static/tagespflege-elen/index.html` |
| 04 | Sonnenkäfer (scrapbook) | `source/Sonnenkäfer.dc.html` | `static/sonnenkaefer/index.html` |
| 05 | Pusteblume (sticker book, sibling of 04) | `source/Pusteblume.dc.html` | `static/pusteblume/index.html` |
| 06 | Business (professional) | `source/Elen Business.dc.html` | `static/elen-business/index.html` |

## About the Design Files
These files are **design references built in HTML**. They show the intended look and behaviour. They are not production code. The task is to recreate them in the target stack using its normal patterns. If no stack exists yet, pick one that fits (e.g. Astro, Eleventy or plain HTML/CSS).

Recreate the designs **1:1**. The customer is reviewing exactly what these files show.

## Fidelity
**High-fidelity.** Final colours, typography, spacing, responsive behaviour and interactions. Photos are **placeholders**: striped boxes with monospace labels like `foto: kinder im garten`. Real photos are still missing.

## How to run
**Static builds (recommended for review):** `static/<variant>/index.html` is a single self-contained file. Code, CSS and fonts are inlined, and there is no build step. Serve the folder with any static server:
```
cd static && npx serve .      # or: python3 -m http.server
```
Then open `/kleines-nest/`, `/bauklotzchen/` and so on. Opening a file directly via `file://` also works.

**Source files:** `source/*.dc.html` plus `source/support.js`. Serve the `source/` folder over HTTP (e.g. `npx serve source`) and open a `.dc.html` file. Do not edit the files in `static/` by hand. They are compiled output.

## Source file format (important)
Each `.dc.html` is a "Design Component". It combines a template (HTML with **inline styles only**), `{{ path }}` placeholders, `<sc-for>` loops, `<sc-if>` conditions and a small logic class (`class Component extends DCLogic { renderVals() {...} }`).

`support.js` is the runtime that renders this format. It is **specific to the design tool** and should not be used as a production dependency. Treat the templates as markup and style references, and the logic classes as data (lists for daily schedule, weekly plan, FAQ, etc.).

## Environment dependencies (Claude preview)
- **`support.js` runtime**: tool-specific. It loads React 18.3.1 and ReactDOM from `unpkg.com`, and Babel on demand. The static builds already contain these inline.
- **Tweak props**: only editable inside the design tool. Outside it, these defaults apply:
  - `freePlaces` (int, default 2) in all variants except 06. It drives the "x Plätze frei ab Januar 2027" text, or "ausgebucht" when 0.
  - `showGallery` (bool, default true) in 01, 02, 03, 04 and 05.
  - `showFaq` (bool, default true) in 06.
- **Google Fonts**: the source files load fonts from `fonts.googleapis.com`. The static builds contain them inline. For production, self-host them (German privacy law, GDPR).
- **Short-lived preview URLs** from earlier in the project (`claudeusercontent.com`) have expired. Do not use them.
- There is no other dependency on the preview environment: no images, no API calls, no tracking.

## Variant selector (top navigation "Designvorschau")
There is a sticky dark bar at the very top of every variant. It exists only for customer review and is **not part of the website design**. Remove it before going live.
- Style: background `#111110`, text `#b8b8b0`, IBM Plex Mono 12px/500, uppercase, letter-spacing .06em, padding 8px, pill links 8px 12px, radius 999px.
  - Active link: bg `#f4f4f0`, text `#111110`, `aria-current="page"`.
  - Hover: text `#fff`, bg `#262624`.
  - Horizontally scrollable on narrow screens (`overflow-x:auto`).
- Dot colours: 01 `#c8643b` · 02 `#e8c35a` · 03 `#8fa9d9` · 04 `#e2483d` · 05 `#9b82e8` · 06 `#5fb3a8`
- Link logic (`renderVals().nav`): if the URL ends in `.dc.html`, link to the source filenames. Otherwise link to `../<folder>/index.html`.
- `static/design-nav-snippet.html` is a standalone version with a small script that highlights the current page. You can paste it after `<body>`.

## Legal links
In the footer of all variants, "Impressum" and "Datenschutz" point to the existing pages. They open in a new tab (`target="_blank" rel="noopener noreferrer"`):
- https://tagesmutter-elen.netlify.app/impressum
- https://tagesmutter-elen.netlify.app/datenschutz

Variant 06 also has a "Datenschutz" link next to the form button.

These links are **not proof of legal compliance**. The content of those pages has not been checked.

## Interactions & Behavior
- All content navigation links (Über mich, Konzept, …) are `href="#"` placeholders. There are no subpages yet.
- **06 FAQ**: accordion. Only one item is open at a time, and the first is open by default. State is `open` (index, -1 = none). Clicking an open item closes it. The icon shows "+" or "−".
- **06 contact form**: **prototype only**. There is no submit handler and no validation, and it shows no success message. It never claims an enquiry was sent. Fields: Name, E-Mail, Alter des Kindes, Gewünschter Beginn, Nachricht. A real backend and privacy notice are needed before launch.
- Hover: links get `opacity:.75` (01–04) or `.8` (05, 06).
- No animations.
- **Responsive**: fluid.
  - Content is centred with a max width: 1440px for 01–03, 1360px for 04–05, 1280px for 06.
  - Side padding uses `clamp(20px, ~4–5vw, 48–72px)`.
  - Grids use `repeat(auto-fit, minmax(min(100%, Npx), 1fr))`, and flex rows use `flex-wrap`.
  - Headlines use `clamp()`.
  - There are no media queries. Recreating this with media queries is fine as long as the layout matches visually.

## Design Tokens per variant
**01 Kleines Nest**
- Colours: bg `#faf5ee`, section `#f3e8db`, ink `#3b2a20`, muted `#6b5445` / `#8a6f5e`, accent `#c8643b`, dark CTA block `#3b2a20`.
- Fonts: Young Serif (headlines) and Nunito Sans 400/600/700.
- Radii: 24px cards, 32px CTA block, pill buttons. The hero image has an arch shape (radius 300px at the top, 32px at the bottom).

**02 Bauklötzchen**
- Colours: bg `#fbfaf6`, ink `#1d2b24`, green `oklch(0.58 0.12 150)`, yellow `oklch(0.86 0.1 85)`. Tiles use `oklch(0.9 0.06 {150|30|240|85})`. Dividers are 2px solid ink.
- Fonts: Bricolage Grotesque 400–800. The h1 goes up to 148px with letter-spacing -.045em.
- Radii: 14px buttons, 24–28px tiles.

**03 Tagespflege Elen**
- Colours: bg `#f4f3ef`, section `#e6e9e4`, ink `#23303f`, muted `#4d5a69` / `#6a7686`, time accent `oklch(0.52 0.09 240)`, lines `#c4cac5`.
- Fonts: Newsreader (headlines, italic), Karla (body), IBM Plex Mono (labels).
- Radius: 4px on images.

**04 Sonnenkäfer**
- Colours: bg `#efe7d6` with a 28px blue grid, ink `#1e2233`, blue `#2f4fb3`, red `#e2483d`, yellow `#f2c94c`, pink `#f4b8ad`, mint `#bfe0c6`.
- Fonts: Figtree 400–800 and Caveat 500/700 (handwritten accents).
- Borders are 2–3px ink with hard shadows (3–8px offset, no blur). Photos are rotated polaroids with tape strips.

**05 Pusteblume**
- Colours: bg `#fdf6ec` with a dot grid, ink `#2a2140`, purple `#6b4fc8`, mint `#9ee0c4`, pink `#ff9fb2`, yellow `#ffd36b`, lilac `#c6b6f5`.
- Fonts: Fredoka 500–700, Nunito 400–800, Caveat 700.
- Borders are 3px ink with `0 5–8px 0` ink shadows. Photos are round stickers.

**06 Business**
- Colours: white, `#f3f5f8` / `#f8f9fb` surfaces, navy `#14284b`, footer `#0e1d38`, teal `#2a9d8f` / `#1f6f66`, tint `#e5f3f1`, lines `#e1e6ee`, input border `#c7d0dd`, muted `#4b5b75`.
- Fonts: IBM Plex Serif 400/500 (headlines), IBM Plex Sans 400–700, IBM Plex Mono.
- Radii: 6px buttons, 10px cards.

Exact values for every element are in the inline styles of the source files, which are the single source of truth.

## Assets
- There are no image, icon or SVG files. All visuals are CSS: circles, squares, stripe placeholders.
- The only SVG is the `__bundler_thumbnail` template, a splash for the bundler. It can be ignored.
- Real photos are still missing. The placeholder labels say what belongs in each slot.
- Fonts come from Google Fonts: Young Serif, Nunito Sans, Nunito, Bricolage Grotesque, Newsreader, Karla, IBM Plex Mono/Sans/Serif, Figtree, Caveat, Fredoka.

## Placeholder content (confirm with customer)
The following are **made up** and need to be confirmed:
- names ("Elen Muster")
- phone number, email addresses, address
- "seit 2016", DJI/300 UE, § 43 SGB VIII
- Januar 2027
- the care packages in 06
- all FAQ answers

## Files
```
design_handoff_tagesmutter/
├─ README.md
├─ source/            # editable design sources + runtime
│  ├─ Kleines Nest.dc.html, Bauklötzchen.dc.html, Tagespflege Elen.dc.html,
│  │  Sonnenkäfer.dc.html, Pusteblume.dc.html, Elen Business.dc.html
│  └─ support.js      # design-tool runtime (not for production)
└─ static/            # self-contained builds, ready for static hosting
   ├─ <variant>/index.html  (×6)
   └─ design-nav-snippet.html
```
The filenames contain umlauts (ö, ä) and spaces, and the source nav links refer to them. Keep the names, or update the `nav` map in each logic class if you rename them.
