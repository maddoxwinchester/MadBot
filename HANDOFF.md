# MadBot Extensions — Homepage Handoff

## How to start a Claude Code session with this context

1. Point this session at this file first: *"Read HANDOFF.md before touching
   anything."* Claude Code doesn't auto-read this — you have to say so.
2. This is a brand-new project folder — there's no code here yet, just this
   doc. The actual homepage (repo, HTML, etc.) hasn't been built.

## What this is

A portfolio/index page listing all of Maddox's Chrome extensions, hosted free
on GitHub Pages, linking each one out to its own Gumroad product page for
actual purchase. **GitHub Pages is static hosting only — it cannot process
payments.** Every "Buy" link on this page must point out to that product's
Gumroad page, not attempt its own checkout.

This is deliberately a *separate* project from any individual extension —
don't nest its repo inside an extension's repo, and don't let an extension's
`CLAUDE.md` bleed into decisions here.

## Branding

- Business name: **MadBot Extensions** (ties to the Gumroad seller handle
  `maddoxbot` and the PayPal business account of the same name — confirm the
  PayPal name is spelled correctly before treating it as canonical; it was
  entered as "MadBot Extentions" in one place, which may be a typo).
- Visual reference: the existing Gumroad custom landing page for Watch
  Progress Tracker — live at https://maddoxbot.gumroad.com/l/progresstracker,
  source at `C:\Users\maddo\OneDrive\Desktop\!Projects\Watch Progress Tracker\landing.html`.
  Dark theme, Tailwind CDN, orange/ember accent, inline SVG icons (no external
  image dependencies — that page runs inside Gumroad's sandbox, which blocks
  external image hosts entirely; the homepage has no such restriction once
  it's on GitHub Pages, so real screenshots/images are fair game there).
  Reusing this look would keep the umbrella site and the product page
  visually consistent, but that's a judgment call, not a requirement.

## Known extensions to potentially list

Found under `C:\Users\maddo\OneDrive\Desktop\!Projects\` on 2026-08-10.
**Only Watch Progress Tracker's status below is verified — the rest are
folder names only. Check each one's actual state (manifest.json, README,
live Chrome Web Store listing, Gumroad product) before writing copy that
claims it's available or before linking to it.**

- **Watch Progress Tracker** ("Universal Watch Progress Tracker") — DONE,
  live on Gumroad: https://maddoxbot.gumroad.com/l/progresstracker. Chrome
  MV3. Free tier (unlimited local tracking) + Pro subscription ($4/mo,
  $29/yr, or $49/every 2 years) unlocking cross-device sync and watch-time
  stats.
- **Focus Blocker** — folder exists, status unverified.
- **Image extension** — folder exists, status unverified (also check its
  actual product name from inside the folder — "Image extension" is just
  the directory name).
- **Screenshot Annotator** — folder exists, status unverified.
- **Web Highlighter** — folder exists, status unverified.
- **trade-finder** — Roblox trading assistant, Chrome MV3, uses
  `chrome.debugger` for trusted-click automation. Has its own detailed
  `HANDOFF.md` at `!Projects/HANDOFF.md` (sitting at the parent level, not
  inside `trade-finder/` itself — that's just where it happens to be, not a
  convention to copy). Not known to be on Gumroad or monetized — verify
  before listing it next to paid products, and consider whether a
  Roblox-trading tool belongs on the same storefront as media/productivity
  extensions at all (different audience).

## Technical setup still needed

- `gh` CLI is **not installed** in this environment (checked 2026-08-10 —
  `gh: command not found`). Install it if you want to create/push the repo
  via CLI instead of the GitHub web UI.
- GitHub Pages needs a **public** repo for free hosting. Maddox's existing
  extension repos are private by habit — don't default to private for this
  one, or Pages won't work without a paid GitHub plan.
- For a root personal/business site with no custom domain, use the special
  repo name `<username>.github.io` (e.g. `maddoxwinchester.github.io`) — it
  auto-serves at that URL with no extra config. For a project-scoped page
  instead, any public repo works once Pages is enabled under
  Settings → Pages.
- No custom domain purchased yet, as far as known. Not required — the
  default `.github.io` URL works fine. A domain can be added later via a
  `CNAME` file in the repo if Maddox buys one (there's a standing "set up a
  custom domain, later" note in the Watch Progress Tracker project memory
  too — worth doing both at once if it happens).

## Poster art API (TheTVDB) — reusable here, with a caveat

Watch Progress Tracker already has a working TheTVDB v4 API integration
(`lib/posters.js` + `lib/constants.js` in that project) for fetching real
show/movie poster art — auth token flow, search, caching, the works. It's
plausible to reuse the same API for this homepage too (e.g. real poster
thumbnails in a Watch Progress Tracker demo section, instead of the generic
SVG glyph icons used on the Gumroad landing page).

**Unlike the Gumroad landing page, this homepage is NOT sandboxed** — it can
make live `fetch()` calls to external APIs like TheTVDB directly, no
restriction there. But this repo is **public**, and GitHub Pages is 100%
client-side — there is no server to hide a key behind. Any TVDB API key
placed in this repo's JS is visible to anyone via view-source, forever (or
until rotated). That's a real decision to make deliberately, not by
accident:

- Check TheTVDB's terms for whether client-exposed keys are acceptable at
  hobby-project rate limits before treating this as a non-issue.
- If not comfortable with exposure, either don't fetch real posters on this
  page (use static/original art instead, same call already made for the
  Gumroad page), or proxy the call through a small serverless function
  (Cloudflare Worker, Vercel edge function, etc.) that holds the real key —
  which does add actual backend infrastructure, contradicting the
  no-custom-backend simplicity this whole setup otherwise has.
- Don't reuse the exact same key from Watch Progress Tracker's
  `lib/constants.js` here without thinking about it either way — if it gets
  scraped/abused from a public repo, it could affect both projects' quota.

## Don't

- Don't embed checkout/payment logic on this page — link out to Gumroad
  instead (see "What this is" above).
- Don't nest this repo inside an existing one, or inside the parent
  `!Projects` folder's own git history if one exists there — Maddox
  deliberately keeps every project as its own isolated repo. (One outer repo
  at a parent directory was previously found to be accidentally tracking
  plaintext credential files — steer clear of broad git operations outside
  this project's own folder.)
- Don't commit real API keys or secrets into this repo, especially since
  it'll be public. If any product page here ever needs a live API call,
  that call belongs server-side or proxied, never a raw key in client-side
  JS on a public repo. (See "Poster art API" above — this applies directly
  to the TVDB key.)

## Open questions for Maddox

- Repo name/location: `maddoxwinchester.github.io` (root personal site) vs.
  a dedicated repo name?
- Which of Focus Blocker / Image extension / Screenshot Annotator / Web
  Highlighter / trade-finder are actually finished and ready to list?
- Any existing brand assets (logo, specific color palette) beyond "MadBot
  Extensions" + the Watch Progress Tracker page's dark/ember theme?
- Confirm "MadBot Extensions" vs. "MadBot Extentions" spelling before it's
  baked into page copy, since a typo here would be more visible than on a
  payment processor's back-office field.
