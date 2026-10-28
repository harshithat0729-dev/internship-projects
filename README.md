# Internship Projects

Four self-contained front-end projects built during my internship, each in a single HTML
file with its CSS and JavaScript inline. No build step, no dependencies, no install — open
any file in a browser and it runs.

**Author:** Harshitha Thummala · [harshithat0729@gmail.com](mailto:harshithat0729@gmail.com)

---

## Contents

| # | File | Project | What it is |
|---|------|---------|------------|
| 01 | `image-gallery.html` | Image Gallery | Filterable masonry photo gallery with a lightbox viewer |
| 02 | `calcio.html` | Calcio | Keyboard-first calculator with live preview and history |
| 03 | `portfolio.html` | Portfolio | This portfolio site — hero, skills, projects, contact |
| 04 | `sonify.html` | Sonify | Music player with live-synthesised tracks and a spectrum display |

Open `portfolio.html` first — it links to the other three.

---

## 1. Image Gallery — `image-gallery.html`

**What the code does**
- Photo data lives in one `PHOTOS` array (seed, title, category). The gallery is rendered
  from that array, so adding a photo means adding one object — no HTML editing.
- A masonry layout built with CSS `columns`, so images keep their natural aspect ratio and
  reflow from four columns on desktop down to two on a phone.
- Lightbox loads a larger version of the same photo, with a title, category and an
  `n / total` counter.

**Features**
- Category filter chips generated automatically from the data (All, Nature, Architecture,
  People, Abstract) with an active state and `aria-pressed`.
- Full-screen lightbox with blurred backdrop.
- Previous / next buttons that wrap around at both ends.
- Keyboard control: `←` `→` to browse, `Esc` to close.
- Click the dark backdrop to close; body scroll is locked while the viewer is open.
- Staggered fade-up animation on load, hover zoom and a caption that slides in.
- Lazy loading on every image (`loading="lazy"`).

**Functionality**
`setFilter()` re-renders the grid from the filtered array and resets the lightbox index.
`openLightbox()` / `step()` keep a `current` pointer into the visible list, so browsing only
ever moves through photos in the selected category.

---

## 2. Calcio — `calcio.html`

**What the code does**
- The expression is kept as a plain string. Numbers, operators and the decimal point are
  appended to it; the result is only computed for display, never stored.
- `sanitize()` strips every character except digits, `.`, `+ - * /` before anything is
  evaluated, so the string handed to the evaluator can never contain executable code.
- Results are rounded to 12 decimal places to avoid floating-point noise like
  `0.30000000000000004`.

**Features**
- Live preview: the running result appears faintly while you type, and turns solid on `=`.
- Calculation history panel (last 30 entries) — click any entry to reuse its result.
- "Clear" button for the history, "AC" for the current expression.
- Sign toggle (±), percent (÷100 of the last number), backspace.
- Division-by-zero and malformed expressions show `Invalid expression` instead of crashing.
- Full keyboard support: `0-9`, `.`, `+ - * /`, `Enter`/`=` to evaluate, `Backspace`,
  `Esc` to clear, `%`.
- Operator replacement — pressing `+` then `×` swaps the operator instead of stacking it.

**Functionality**
`evaluate()` returns `null` for anything unusable, which the UI treats as "no preview yet".
`equals()` pushes `{expression, result}` onto the history array, re-renders the list and
leaves the answer on screen so the next keypress starts a fresh calculation.

---

## 3. Portfolio — `portfolio.html`

**What the code does**
- A single-page portfolio: sticky nav, hero with an intro card, about, skills, project
  cards and a contact form.
- Theming is done with CSS custom properties — `body.dark` swaps one set of variables, and
  the choice is remembered in `localStorage`.
- All motion is driven by an `IntersectionObserver`, so nothing animates off-screen.

**Features**
- Light / dark theme toggle, remembered between visits.
- Scroll-reveal on every section, and skill bars that animate to their value the first time
  they come into view.
- Responsive layout with a hamburger menu under 860px; links close the menu on tap.
- Project cards that open the other three projects in this repository.
- Contact form with client-side validation (name, message, email pattern) and a toast
  confirmation.
- Direct email and GitHub links; smooth scrolling with `scroll-margin` so anchors don't
  hide under the sticky nav.

**Functionality**
The form is front-end only — it validates and confirms, but does not send anywhere, which
is why the toast says so. Wiring it to a real endpoint (Formspree, a serverless function,
or `mailto:`) is a one-line change in the submit handler.

---

## 4. Sonify — `sonify.html`

**What the code does**
- There are no audio files. Each track is a list of `[MIDI note, length in beats]` pairs,
  and the player schedules oscillators with the **Web Audio API** to play them.
- `buildTimeline()` converts a melody into absolute note start times and a total duration,
  which drives the seek bar and the time readout.
- `scheduleFrom(offset)` schedules only the notes still ahead of the playhead, so seeking
  mid-track restarts the schedule at exactly that point rather than replaying from zero.
- An `AnalyserNode` sits between the master gain and the speakers and feeds a canvas that
  draws the frequency spectrum.

**Features**
- Play / pause, previous, next, shuffle and repeat.
- Seek bar that works both while paused and mid-playback.
- Volume slider wired to a single master gain node.
- Live spectrum visualiser with a per-track colour gradient.
- Playlist panel with per-track mood and duration; click any row to jump to it.
- Per-track artwork colour and oscillator waveform (sawtooth, triangle, sine, square) give
  each of the four tracks its own character.
- Keyboard shortcuts: `Space` play/pause, `←` `→` previous/next.
- Auto-advance to the next track when one finishes; respects repeat and shuffle.

**Functionality**
Pause calls `AudioContext.suspend()`, which freezes the audio clock, so elapsed time is
preserved exactly and resume continues where it stopped. Playback position is always
`offset + (ctx.currentTime - startedAt)`, which is what the seek bar reads each frame.

---

## Tech used across all four

Plain **HTML5, CSS3 and JavaScript (ES6+)** — no frameworks, no bundlers, no external
libraries. Browser APIs used: Canvas 2D, Web Audio API, IntersectionObserver, localStorage,
CSS custom properties, CSS Grid and multi-column layout.

## Running locally

```bash
git clone https://github.com/harshithat0729-dev/internship-projects.git
cd internship-projects
open portfolio.html      # or: xdg-open portfolio.html
```

Or simply double-click any `.html` file. A local server is optional, but handy:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
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

- The gallery uses seeded placeholder photos (`picsum.photos`) so the repository ships no
  binaries. Replace the `seed` values with your own image URLs when you add real shots.
- The calculator evaluates a sanitised arithmetic string; it is not a general-purpose
  expression parser, so it deliberately accepts nothing beyond `+ - * /`.
- The contact form is a UI demo with validation — no backend is attached.
