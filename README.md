# Internship Projects

Four front-end projects from my internship, each built as a **single self-contained HTML
file** — markup, CSS and JavaScript all in one place. No framework, no build step, no
`npm install`. Open a file in a browser and it runs.

**Harshitha Thummala** · BSc. Artificial Intelligence student
[harshithat0729@gmail.com](mailto:harshithat0729@gmail.com) ·
[LinkedIn](https://www.linkedin.com/in/harshitha-thummala-312bb239b)

---

## Contents

| # | File in this repo | Original file | Project |
|---|-------------------|---------------|---------|
| 01 | `image-gallery.html` | `Final_Image_Gallery.html` | Image Gallery — live photo gallery of Indian landmarks, nature, festivals and more |
| 02 | `calcio.html` | `calcio.html` | Calcio — scientific calculator with graph, matrix, calculus tools and an AI tutor |
| 03 | `portfolio.html` | `Harshitha_Portfolio_Single_6.html` | Portfolio — personal site with projects, skills, certifications and contact |
| 04 | `sonify.html` | `sonify.html` | Sonify — music player that plays your own local files, with themes and a persisted library |

Start with `portfolio.html` — it is the front door to the other three.

---

## 1. Image Gallery — `image-gallery.html`

A gallery of 32 Indian photographs organised into eight collections: **Landmarks, Nature,
Festivals, Food, Architecture, Wildlife, Beaches and Culture**, four entries each.

**Features**

- **Live photos, no binaries in the repo.** Every photo is fetched at runtime from the
  Wikimedia Commons API (`action=query`, `generator=search`) using a per-item search query,
  then cached in memory so the same image is never fetched twice.
- **Category filters** generated from the data itself, plus an **All** view.
- **Search box** that filters on title and category as you type.
- **Favourites** — heart any photo; the list is saved in `localStorage` (`gallery_favs`) and
  a "Favs" toggle shows only your saved photos.
- **Shuffle** button for a randomised view.
- **Light/dark theme** toggle, remembered in `localStorage` (`gallery_theme`).
- **Detail modal** per photo: large image, title, category, a short description, a link to
  the image's source on Wikimedia Commons, and a **Download image** button.
- Graceful loading states — "Loading photo…", "Photo unavailable", "Unable to load photo" —
  instead of broken image icons.

**How it works**

All content lives in one `collections` array of `{category, items}`, where each item is
`[title, searchQuery, description]`. `createFilters()` builds the filter row from that array,
`renderGallery()` applies the category, search term and favourites filters together,
`loadImage()` handles the Commons request and caching, and `openModal()` fills the modal and
sets the source and download links.

**Note:** because photos come from Wikimedia Commons, this project needs an internet
connection. The layout, filters, favourites and theming all work offline.

---

## 2. Calcio — `calcio.html`

A scientific calculator that goes well past arithmetic: a proper expression engine, a set of
maths tools, and an AI tutor chat panel beside it.

**Features**

- **Scientific calculator** — full expression input with brackets, powers, `log`, `ln`, trig,
  factorials, `nCr` / `nPr`, `π` and `e`, a **DEG / RAD** switch and an **S⇔D** button that
  flips the answer between decimal and fraction.
- **Calculation history** panel, with light and dark modes.
- **Graph tab** — plot up to three functions of `x`, drag to pan, zoom in and out.
- **Matrix tab** — set the sizes of A and B, fill in values, then multiply, take the
  determinant or invert (with a guard for singular matrices).
- **Equations tab** — solve quadratics from `a`, `b`, `c`, reporting real or complex roots
  from the discriminant.
- **More tools** — definite integrals (`∫ a→b`), derivatives at a point (`d/dx at a`),
  summations (`Σ a→b`) and a function table.
- **Vector tools** — dot product, cross product and angle between vectors.
- **Statistics** (mean, standard deviation), unit conversion, base conversion.
- **AI tutor chat** — ask a question and get a step-by-step explanation.
  - With no API key it answers from a built-in FAQ knowledge base covering standard
    deviation, matrices, DEG vs RAD, quadratics, fractions, graphs, calculus, permutations
    and logarithms — fully offline.
  - With a key it calls the Anthropic Messages API (`api.anthropic.com/v1/messages`) for a
    full answer. The key is typed into the input field at runtime and is **never stored or
    bundled in the file** — reload the page and it is gone.
- The tutor is context-aware: it is told the current expression and the last answer.
- An **AI Tutor in Claude** button at the top of the tutor panel opens the full tutor
  (the Claude artifact) in a new tab, for the complete step-by-step answers.

**How it works**

The expression is tokenised and prepared by `prep()` and evaluated by `ev()`, with `calc()`
driving the display. Each tool panel is a function of its own (`quad`, `sim`, `integ`,
`deriv`, `sigma`, `table`, `roots`, `cx`, `vec`, `unit`, `stats`, `base`), and `ask()` routes
a chat message to `faq()` or `apiAsk()` depending on whether a key was supplied.

---

## 3. Portfolio — `portfolio.html`

My personal site — a single page covering who I am, what I work with, what I have built and
how to reach me.

**Features**

- **Hero** with name, degree (BSc. Artificial Intelligence) and an embedded avatar image.
- **About** section, written in plain language.
- **Skills** grouped into four panels: Programming, Web Development, AI & Data, Tools &
  Platforms.
- **Projects** — the Lab Tracker Website (lab attendance with check-in/check-out timings),
  Calcio (the calculator above), a Python AI Chatbot and a Scam Message Detector, plus a note
  on 30+ smaller Python projects (Smart File Organizer, Data Cleaner, Text Analyzer, Weather
  Prediction System, Password Strength Analyzer).
- **Certifications & awards** — Kaggle Python course, Microsoft Fundamentals & Libraries of
  Python, Web Blitz participation certificate, plus awards in essay writing, throwball and
  kabaddi, and academics.
- **Languages** — Telugu, English, Hindi.
- **Contact cards** — email, phone and LinkedIn.
- **Dark / light theme** that follows the system preference by default and remembers an
  explicit choice in `localStorage`.
- **Mobile menu** and **scroll-reveal** animations on every section.

**How it works**

The whole site is one HTML file with an inline stylesheet built on CSS custom properties, so
theming is a single `data-theme` attribute on the root element. A small inline script handles
the theme toggle, the mobile navigation, and the reveal-on-scroll behaviour (via
`IntersectionObserver`). The only external request is the Inter webfont from Google Fonts.

---

## 4. Sonify — `sonify.html`

A full music player in the browser that plays **your own audio files**, not just a fixed
playlist.

**Features**

- **Local library.** Add songs three ways: the file picker, drag-and-drop anywhere on the
  window, or the "Add from Downloads" flow. Supported formats: `mp3, m4a, aac, wav, ogg,
  oga, flac, opus, wma`.
- **Real metadata.** `parseID3()` and `parseAPIC()` read ID3v2 tags — title (`TIT2`), artist
  (`TPE1`), album (`TALB`), year (`TYER` / `TDRC`) — and extract the embedded cover art. If a
  file has no tags, `guessFromFilename()` falls back to a sensible title/artist split.
- **Library survives a reload.** Tracks and covers are stored in **IndexedDB**
  (database `sonify-player`, store `local-tracks`) and restored on load via
  `restoreLocalTracks()`. Removing a track deletes it from the database too.
- **Playlists and favourites** persisted separately (`sonify-playlists-v1`,
  `sonify-favourites-v1`), plus player settings in `sonify-settings`.
- **Transport** — play/pause, next, previous, shuffle, repeat, seek bar, volume with icon
  feedback, and a large central play button.
- **Themes.** Six built-in palettes — plain dark, aurora, sunset, ocean, candy, mono — plus
  an **auto** mode that samples the dominant hue of the current cover art
  (`extractHue()` / `rgbToHsl()`) and repaints the whole UI, including a full-bleed ambient
  backdrop, in that colour.
- **Per-track styling** — each song gets a generated cover when art is missing
  (`placeholderCover()`, and the `lakeBg` / `concertBg` / `neonBg` / `sunsetBg` / `vinylBg`
  generators) and its own visual style.
- **Demo track included** — "Morning Drift" ships embedded as base64 audio so the player has
  something to play the moment it opens.
- **Keyboard shortcuts** — `Space` play/pause, `←` `→` seek ±5 s, `↑` `↓` volume, `n` next,
  `p` previous, `m` mute, `s` shuffle, `r` repeat, `t` timer. All are listed in the in-app
  Shortcuts panel.
- Responsive glass UI, built for both desktop and phone widths.

**How it works**

A single `<audio>` element is the engine. `addLocalFiles()` / `addSingleFile()` read a file
into a blob URL, pull the ID3 tags and cover, probe the duration (`probeDuration()`) and push
the track into `state.tracks`; `dbPut()` writes it to IndexedDB. Playback, queue order
(`refreshOrder()`), repeat/shuffle modes and the theme are all derived from that one state
object, and `saveSettings()` mirrors the parts worth keeping into `localStorage`.

**Note:** because it is a 2 MB file, most of the size is the embedded demo audio and cover
data, not code. Everything runs client-side — no server, no uploads.

---

## Tech used

Plain **HTML5, CSS3 and modern JavaScript (ES6+)** throughout. Browser APIs in play:
`fetch` (Wikimedia Commons API), `localStorage`, **IndexedDB**, the **Canvas 2D** API,
`IntersectionObserver`, `File` / `Blob` URLs, the `<audio>` element, and CSS custom properties
with Grid and Flexbox. One optional external service: the Anthropic Messages API, used only
when you paste your own key into Calcio's tutor.

## Running locally

```bash
git clone https://github.com/harshithat0729-dev/internship-projects.git
cd internship-projects
```

Then open any file directly, or serve the folder (recommended — IndexedDB, file pickers and
API calls behave more consistently over `http://`):

```bash
python3 -m http.server 8000
# visit http://localhost:8000/portfolio.html
```

## Repository structure

```
internship-projects/
├── README.md
├── image-gallery.html
├── calcio.html
├── portfolio.html
└── sonify.html
```

## Notes

- The gallery loads its photos live from Wikimedia Commons, so it needs a connection; all
  images remain the property of their original Commons uploaders.
- Calcio's AI tutor works offline from its FAQ knowledge base. The full tutor opens through
  the **AI Tutor in Claude** button (the Claude artifact link), and a live in-page answer
  needs your own Anthropic API key, entered at runtime and never saved in the file.
- Sonify keeps your music in the browser's IndexedDB — it never leaves your device, and
  clearing site data clears the library.
