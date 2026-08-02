# Mariana Pickersgill — Portfolio v2

Static site. No build step, no backend, no login. Three files matter:

- `index.html` — the whole site
- `press.json` — the Press section list
- `content.json` — the Content section grid

## Updating press mentions

Open `press.json` and add an entry to the array:

```json
{
  "outlet": "Outlet name",
  "headline": "Exact headline or a short description",
  "date": "2026-01-15",
  "url": "https://example.com/the-article",
  "image": "images/press/outlet-slug.jpg"
}
```

Order doesn't matter — the site sorts by date automatically, newest first. The "Featured in N national outlets" number also updates itself from the number of distinct outlets in the list.

`image` is optional — omit it and the card just runs without a thumbnail (no broken-image icon). To add one: save a copy of the article's cover photo into `images/press/` (don't hotlink the outlet's own image URL — it can break or get blocked later) and point `image` at that local path. Keep it landscape-ish; the card crops to a fixed box.

## Updating content / channels

Open `content.json` and add an entry:

```json
{
  "platform": "Instagram",
  "label": "Short title for the card",
  "description": "One sentence on what this proves.",
  "url": "https://instagram.com/...",
  "tag": "Growth",
  "image": "images/content/spun-post.jpg"
}
```

`tag` is the small mono label in the corner of the card (e.g. "Growth", "Community", "Ongoing") — keep it to one or two words.

`image` is optional, same as press. **Instagram blocks unauthenticated scraping**, so there's no automatic way to pull a post's photo or a profile picture — if you want images on these cards, screenshot or save the image yourself and drop it in `images/content/`, then reference it here. Without an `image` field the card just shows text, no placeholder.

Both sections currently link out to the channel or article rather than embedding it. If you later want live embeds (e.g. Instagram's own embed widget) for specific posts, that's a v3 change to `index.html`, not to the JSON files.

## Testing changes locally

Because the page loads `press.json` and `content.json` with `fetch()`, opening `index.html` directly by double-clicking it (`file://…`) will fail to load them in Chrome/most browsers. Run a tiny local server from this folder instead:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000` in a browser. This restriction goes away once the site is actually hosted (Netlify, GitHub Pages, etc.) — it only affects local file-double-click testing.

## Deploying

Drag this whole folder into [Netlify Drop](https://app.netlify.com/drop), or push it to a GitHub repo and enable GitHub Pages. No build command, no environment variables.

## What's not built yet (on purpose)

- **Live auto-pulling feeds** (site automatically shows your latest Instagram/LinkedIn posts) — this needs developer app review and auth tokens with Meta/LinkedIn, which is more infrastructure than this site needs right now. Worth revisiting as a v3 if you want it.
- **Rich embeds** (showing the actual post inline instead of a link-out card) — possible per-platform later, but skipped for now to keep the site fast, dependency-free, and reliable on mobile.
