# Alistron website

Marketing site for Alistron (Alistron Bionics and Alistron STEM), live at
[alistron.in](https://alistron.in). Built with Next.js as a fully static export and
hosted on Cloudflare Pages.

## Development

```bash
npm install
npm run dev        # http://localhost:3000
npm run lint
```

Content lives in `src/data/` (events, projects, team, journey). The contact form posts
to `/api/contact`, which only exists on Cloudflare (`public/_worker.js`), so it shows
an error in local dev. That's expected.

## Deploy to Cloudflare Pages

1. Build the upload bundle:

   ```bash
   rm -f cloudflare-pages-deploy.zip
   npm run build:cloudflare
   ```

2. In Cloudflare: **Workers & Pages → your Pages project → Create deployment**, then
   drag in `cloudflare-pages-deploy.zip` (or the `out/` folder).

3. **Required for the contact form**: under **Settings → Environment variables**
   (Production), set:

   | Variable | Value |
   | --- | --- |
   | `RESEND_API_KEY` | API key from [resend.com](https://resend.com) |
   | `RESEND_FROM_EMAIL` | A sender on a domain verified in Resend, e.g. `website@alistron.in` |
   | `RESEND_TO_EMAIL` | Inbox that receives enquiries (defaults to `contact@alistron.in`) |

   Without a valid key the form doesn't pretend to succeed: it asks visitors to email
   `contact@alistron.in` directly, so no enquiry is silently lost. Redeploy after
   changing variables.

## Brand assets

The logo, favicon, apple icon, social share image and hero emblem are generated from
`assets/brand/alistron-logo-board.png`:

```bash
node scripts/generate-brand-assets.mjs
```

Photos not used by any page are kept in `assets/unused-photos/` so they don't ship
with the site.
