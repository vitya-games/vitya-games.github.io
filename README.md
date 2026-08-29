# vitya-games.github.io

Vitya Games landing page.

## Featured games

- Hotdog Runner
- Hotdog Race
- Gull Flyer
- Grill Sitter
- Vitya's Life
- Usage Limit Reached

The four iOS game pages live in this repository. Legacy support and privacy
routes remain on `vitya-labs.github.io` until the corresponding App Store
Connect metadata has been migrated and verified.

## Hotdog Race is the publishing set, not just a landing page

The other three games collect a high score on the device and need two documents:
a support page and a privacy policy. Hotdog Race has an account, a dedicated
server and a shop behind it, so it needs the set a reviewer actually asks for,
and `hotdog-race/` carries all of it:

| Page | What it is for |
| --- | --- |
| `index.html` | The marketing page, and the application homepage Google's OAuth consent screen points at |
| `privacy.html` | App Store Connect's required privacy policy URL, and the OAuth consent screen's privacy link |
| `terms.html` | The EULA, including Apple's minimum terms for a custom one, and the OAuth consent screen's terms link |
| `support.html` | App Store Connect's required support URL |
| `account-deletion.html` | The public account deletion route App Review expects from an app that creates accounts |
| `press.html` | Fact sheet, two lengths of description, feature list and screenshots |

All six carry the same three languages as every other page here. The exact URLs
are recorded in the game repository's `docs/publishing-urls.md`, which is where
the store and console forms are filled in from.

**The shelf does not link to them, and that is deliberate.** Hotdog Race is
unannounced, so its card on the front page is a `project-card--teaser` like Grill
Sitter's: blurred artwork, no name, "Coming soon to iOS". The
six pages are reachable by their URLs — which is all a store form or an OAuth
consent screen needs — and nothing navigates to them until the game is announced.
Turning the teaser back into a full card is a launch-day step, not a missing
link.

Screenshots under `hotdog-race/assets/` are captured from the running game at
1280×720; the icon is the approved App Store artwork, resized. Regenerating them
is described in the game repository's `docs/publishing-urls.md`.

Static HTML, CSS and JavaScript, ready for GitHub Pages.

## Languages

Every page is trilingual: English, Hungarian and — for fun — Pirate English.
Copy lives in `data-en` / `data-hu` / `data-pr` attributes (plus `data-title-*`,
`data-description-*` on `<body>` and `data-aria-*` for `aria-label`s), and the
switch in the header is driven by `site.js`. English is the fallback for any
missing translation, and `site.js` is kept byte-identical in all four site
repositories.

The four sites are four separate origins, so `localStorage` cannot carry the
chosen language between them. Links to a sibling site are rewritten to carry a
`?lang=` parameter, which the target page adopts on load and then removes from
the address bar.

Pirate English also swaps the artwork: an `<img>` carrying `data-logo-pr` shows
its pirate mascot in that language and returns to the original logo in English
and Hungarian.

Translations have different lengths, so `site.js` measures every translated
element in all three languages once the page has loaded and reserves the
largest box it needs — width for anything sitting in a horizontal row,
height for everything else. Switching languages therefore never moves the
layout. The measurement is repeated after the webfonts settle and after a
viewport resize.
