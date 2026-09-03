# tuesday.betelgeusehou.com

The Tuesday burger deal share page. One static file, no build step.

Linked from the Klaviyo campaign "Tuesday Burger Deal," and reusable on the Google
Business Profile, in Toast SMS, and on table cards.

---

## Deploy on Railway

1. New project → deploy from this folder (GitHub repo or `railway up` from here).
2. Railway detects Node and runs `npm start`, which serves `public/` on `$PORT`.
   No other configuration needed.
3. Settings → Networking → **Custom Domain** → `tuesday.betelgeusehou.com`.
   Railway shows a CNAME target.

## DNS

Add ONE record at whoever holds `betelgeusehou.com` DNS:

| Type | Name | Value |
|---|---|---|
| CNAME | `tuesday` | *(the target Railway gives you)* |

**The apex and www are untouched.** `betelgeusehou.com` keeps pointing at Toast.
Only the `tuesday` subdomain goes to Railway. Nothing about the existing site changes,
and there is no Toast setting to configure.

HTTPS is automatic once the CNAME resolves — usually a few minutes.

---

## What the page does

Two buttons. Each one tries three things in order, so it works regardless of what the
browser supports:

1. **`navigator.share()`** — the native share sheet. Needs HTTPS and a real tap, both
   satisfied here. Opens Messages, WhatsApp, whatever they use.
2. **`sms:`** — only for older mobile browsers with no share API.
3. **On-screen message with a Copy button** — desktop, or anything that refused the
   first two.

A user who opens the share sheet and backs out gets `AbortError`. That's a cancel, not
a failure, so the page does nothing — no fallback panel appears behind them. Same for
`NotAllowedError`.

## Why not embed this on the Toast site

Toast Websites has a Code Block for embedded HTML, but embedded code lands in a
sandboxed iframe. `navigator.share()` needs the top-level document or an iframe granted
`allow="web-share"`, which a site builder won't give you. The share sheet would fail
there for the same reason it failed inside Gmail and Google Drive's preview.

## Editing copy

Everything lives in `public/index.html` — no templating, no framework. The two share
messages are the `data-msg` attributes on the buttons. Change the price or the beer
brands in both the visible copy and the `data-msg` strings, or they'll disagree.

## Verified facts on this page

From `081826 Washington Menu.pdf`:

- A Burger in a Pizza Oven, $9 — ketchup, mustard, pickles, white onions, American cheese
- Happy hour 4–6 daily · kitchen till 11 PM Sun–Thu · 21+ after 9 PM
- Deal: 2 burgers + 2 tall boys, $20, **dine in only**, Washington Ave only
- Tall boys: PBR, Lone Star, Montucky Cold Snacks
