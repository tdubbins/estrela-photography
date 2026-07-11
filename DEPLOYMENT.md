# Deployment & Launch Guide — Estrela Photography

How this site is hosted, how the temporary "coming soon" page works, and the exact
steps to take it live and later launch the full site.

**Repo:** `tdubbins/estrela-photography`
**Host:** GitHub Pages — source **`main` / root**, HTTPS enforced
**Live URL (current):** https://tdubbins.github.io/estrela-photography/
**Target domain:** `estrelaphotography.com` (not yet registered — see [Domain setup](#domain-setup))

---

## Branch model

| Branch | Role | `index.html` is… |
|---|---|---|
| **`main`** | **Published** — this is what GitHub Pages serves | the **coming-soon** page (temporary) |
| **`full-site`** | **Development** — do all site work here | the **full website** (work in progress) |

We use a "branch swap": the finished site lives on `full-site` while `main` shows a
coming-soon teaser. At launch, `full-site` is merged into `main` and the real site goes live.

> Do day-to-day editing on **`full-site`**. Only touch `main` to publish/unpublish the teaser.

### How this was set up (for reference)
- `full-site` branched from `main` with the complete WIP site preserved.
- On `main`, `index.html` was replaced with the contents of the coming-soon page
  (commit `Serve coming-soon page as site index (temporary until launch)`).
- The coming-soon source also exists on `full-site` as `coming-soon.html`.

---

## The coming-soon page

- Source file: **`coming-soon.html`** (on `full-site`). On `main` its contents ARE `index.html`.
- Self-contained, reuses the real brand assets: `logo.svg`, the site fonts
  (Italiana / Cormorant Garamond / Special Elite / Allura), and the moss/cream/gold palette.
- Copy: "Website coming soon" → finishing touches → connect on Instagram
  (`@estrela.photography`) → "See you there! ✨", signed *Esther*.
- `noindex, nofollow` so search engines don't index the temporary page.

If you edit the coming-soon page, edit **`coming-soon.html`** on `full-site`, then re-copy
it into `main`'s `index.html` (see below).

---

## Task 1 — Go live with the coming-soon page

Do this **when the domain is being set up / just before the QR goes out.** Until then,
holding off keeps the shareable `tdubbins.github.io` preview showing the WIP site.

```bash
git push origin main          # publishes the coming-soon page to the live site
git push origin full-site     # backs up the dev branch (safe, no live effect)
```

To re-copy an edited coming-soon page from `full-site` into `main`:

```bash
git checkout full-site && cp coming-soon.html /tmp/cs.html
git checkout main && cp /tmp/cs.html index.html
git commit -am "Update coming-soon page" && git push origin main
```

### Optional: hide the other WIP pages while the teaser is up
`gallery.html`, `journal.html`, `voucher.html` still exist on `main` and are reachable by
direct URL (nothing links to them). To remove them from the published teaser:

```bash
git checkout main
git rm gallery.html journal.html voucher.html
git commit -m "Remove WIP inner pages from coming-soon branch"
git push origin main
```
They remain safe on `full-site` and come back at launch.

---

## Task 2 — Launch the full site

When Esther approves the finished site:

```bash
git checkout main
git checkout full-site -- .          # take the full site's files
git checkout full-site -- index.html # ensure the real homepage overwrites the teaser
git commit -am "Launch: replace coming-soon with full site"
git push origin main
```

Then in `index.html`, remove the temporary line:
`<meta name="robots" content="noindex, nofollow">` so search engines can index the real site.

The QR code and domain do **not** change — the same URL now serves the full site.

---

## Domain setup

`estrelaphotography.com` is **available** (unregistered as of this writing).

**Recommended registrar: Porkbun** (~$11/yr flat, free WHOIS privacy, Cloudflare-backed DNS,
one-click GitHub Pages config). Spaceship is ~$1/yr cheaper but has reports of arbitrary
account lockdowns — not worth the risk for a brand domain a printed QR depends on.
Avoid checkdomain (~€26/yr renewal + paid privacy).

**Register in Esther's name** (she should own her brand domain). Decline all upsells
(no email, no premium DNS, no SSL — GitHub Pages provides free HTTPS).

### DNS records (point the domain at GitHub Pages)
In Porkbun → Domain Management → Details → DNS → **Quick DNS Config → GitHub button**, which
creates these apex `A` records automatically:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```
Plus IPv6 `AAAA` (optional): `2606:50c0:8000::153`, `…8001::153`, `…8002::153`, `…8003::153`
And a `www` record: `CNAME  www  →  tdubbins.github.io`

### GitHub side
Repo → **Settings → Pages** → Custom domain: `estrelaphotography.com` → Save
(creates a `CNAME` file). Wait for the DNS check to pass, then tick **Enforce HTTPS**.

### Verify before trusting the QR
```bash
dig estrelaphotography.com +short     # should return the four 185.199.x.153 IPs
```
Then load https://estrelaphotography.com with a valid padlock. **Only then** finalise the QR.

Sources: [GitHub Pages custom-domain docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site),
[Porkbun GitHub Pages KB](https://kb.porkbun.com/article/64-how-to-connect-your-domain-to-github-pages).

---

## QR code (for Tomorrow Magazine — August edition, deadline July 13)

- **Encodes:** `https://estrelaphotography.com` (permanent — shows coming-soon now, full site
  at launch, same QR forever). Fallback if domain can't be readied in time: the Instagram URL.
- **Readability:** the code must sit on a **light** area (cream/white) with **dark** modules —
  black or dark moss `#3d4a35`. Do NOT put black modules directly on the dark green background
  (dark-on-dark won't scan). The green is the surround; the code lives on a light card with a
  subtle black frame, on the green logo background.
- **Min printed size:** ~2 × 2 cm. Confirm format with the magazine (vector/PDF or 300 DPI PNG;
  RGB vs CMYK).
- Generate the final QR + logo composition **only after** the domain resolves over HTTPS.

---

## Quick reference

| I want to… | Do this |
|---|---|
| Edit the real site | Work on `full-site` |
| Preview the real site | `git checkout full-site` → open `index.html` |
| Publish coming-soon | `git push origin main` |
| Update coming-soon text | edit `coming-soon.html` on `full-site`, copy into `main`'s `index.html` |
| Launch the full site | merge `full-site` → `main` (see Task 2), remove `noindex` |
