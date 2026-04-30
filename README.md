# Estrela Photography

The website for Esther — a photographer based in Germany. Family, pregnancy, portrait and personal-branding photography, with a per-client online gallery.

Live at: **estrelaphotography.com** *(once domain is configured)*

## What's in this repo

- `index.html` — the public site (single page: home · about · offers · work · testimonials · contact)
- `gallery.html` — the client gallery (per-client photo selection + download)
- `README.md` — this file

That's everything. No build step, no dependencies. Just open `index.html` in a browser to see it.

## Stack

- Vanilla HTML, CSS, JavaScript — no framework, no build pipeline
- Fonts via [Bunny Fonts](https://fonts.bunny.net) (GDPR-friendly)
- Hosted on **GitHub Pages**
- Client-gallery photos served from **Cloudinary**

## Local development

```bash
# clone the repo, then just open the file
open index.html
```

For the gallery to fetch real photos, set the Cloudinary cloud name in `gallery.html` (search for `CLOUDINARY_CLOUD_NAME`).

## Deploying

Pushing to the `main` branch publishes the site automatically via GitHub Pages.

## Credits

Designed and built with Esther.
