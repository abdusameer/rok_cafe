# rōk coffee and tea

A one-page, scroll-driven site for rōk coffee and tea (Los Angeles). Plain HTML, CSS and JavaScript in a single
`index.html`, with media in `assets/`. No framework, no build step.

## What's on the page

- **Hero:** a scroll-scrubbed video of matcha pouring into a rōk cup until it overflows. Scrolling down plays it
  forward and scrolling up plays it back. Phones get a square centre cut of the same clip (`hero-scrub-portrait.mp4`),
  which decodes about twice as fast. With reduced motion turned on, a still hero shows instead and no video is downloaded.
- **Locations:** a pinned scroll story through the four cafés (Olympic Blvd, Studio City, Silver Lake, Wilshire Blvd),
  with a route map drawn from their addresses. It falls back to a plain list with reduced motion or on very short screens.
- **Our story and rōk x Fellow.**
- **Fellow exploded view:** a pinned, scroll-scrubbed 3D render of the Carter 3-in-1 kit. It lifts out of its box,
  separates into every part, and the Move Lid is put back together. Color swatches switch between Matte White,
  Sienna, Smoke Green and Stone Blue without losing your place. Only the chosen color and screen format are downloaded.
- **Press, Girls Inc., Uji matcha, FAQs, footer.**

On touch screens, one swipe moves one step through the hero, the locations, the Fellow view and the press cards: when a
swipe comes to rest part way through a step, the page glides on to the next one in that direction. Ordinary sections
scroll freely, and nothing happens while a finger is on the screen.

Tested at 21 window sizes, from a 360 px phone to a 2560 px monitor, including short and narrow browser windows.

## Keeping it smooth

- The scrubbed videos have a keyframe every 4 frames and no B-frames, so any scroll position is only a few decoded
  frames away. Re-encode new clips the same way (`-g 4 -keyint_min 1 -bf 0 -sc_threshold 0 -movflags +faststart`).
- Everything that moves while you scroll (the location wipes, the press deck and its dimming) moves with transforms and
  opacity on layers that are already painted, so nothing is repainted frame by frame.
- Photos are sized for the largest screen that shows them (drinks 1200 px wide, Uji field 1600 px) and saved as
  baseline JPEGs, which decode faster than WebP.

## Preview locally

The hero video is loaded with `fetch`, which browsers block on `file://` pages. Serve the folder instead:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000. Opening `index.html` directly still works, but shows the still hero.

## Imagery

| File | Source |
|---|---|
| `assets/loc-*.jpg` | rōk's own storefront photos |
| `assets/fellow-carter.webp` | Fellow's product photo of the Carter 3-in-1 gift box |
| `assets/hero-scrub.mp4`, `hero-scrub-portrait.mp4`, `hero-poster.jpg`, `hero-ending.jpg` | AI-generated with Higgsfield |
| `assets/drink-*.jpg`, `assets/uji-field.jpg` | AI-generated with Higgsfield, stand-ins for real photos |
| `assets/fellow/*` | 3D reconstruction of the Fellow Carter 3-in-1 kit, modeled and rendered in Blender from product photos. Some dimensions are estimated, and the "FELLOW" and "HELLO" prints use a stand-in font |

To swap in a real photo, replace the file with one of the same name.

## Hosting on GitHub Pages

The site is ready to serve from the repo root. In the repo on GitHub: **Settings → Pages → Build and deployment →
Source: Deploy from a branch → Branch: `main`, folder `/ (root)` → Save.** It goes live at
https://abdusameer.github.io/rok_cafe/ a minute or two later.

`.nojekyll` tells GitHub Pages to serve the files exactly as they are. The `og:url` and `og:image` tags
(marked `DEPLOY STEP` in `index.html`) already point at that address; update them if the site moves to its own domain.

The Fellow product photos in `assets/Web_PDP_*.webp` and `assets/uc_*.webp` are included for later use and are
not shown on the page yet.

## Still to do

- The **Shop** button links to the home page; point it at the real shop link.
