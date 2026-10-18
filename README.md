# sean.cc

Personal website and writing home base: articles, resume, photography, and fitness.
Built with [Astro](https://docs.astro.build) (blog starter) and deployed to Cloudflare Workers as a static site.

## Run it locally

Requires Node.js 22.12 or newer.

```sh
npm install          # first time, or after package changes
npm run dev          # http://localhost:4321
```

To test the production build the way Cloudflare serves it:

```sh
npx astro build && npx wrangler dev
```

## Deploying

- A push to `main` builds and deploys to https://sean.cc automatically (Cloudflare Workers Builds).
- Pushing any other branch creates a preview build. Use a branch for anything you want to look at before it goes live.
- A failed build does not replace the live site. Build logs: Cloudflare dashboard, Worker `sean-cc`, Deployments.
- `www.sean.cc` redirects to `sean.cc` with a Cloudflare Redirect Rule (not in this repo).

## Working on two Macs

`git pull` before starting, `git push` when done. The repo lives at `~/Code/sean-cc` on each machine, outside iCloud.

## Where things live

| Path | What it is |
| :--- | :--- |
| `src/content/blog/` | Posts (Markdown or MDX) |
| `src/pages/` | Pages; each file becomes a URL |
| `src/components/` | Header, footer, and other shared pieces |
| `src/styles/global.css` | Site-wide styles and the color palette |
| `src/consts.ts` | Site title and description |
| `public/` | Files served as-is (favicon, etc.); images here are not optimized |
| `astro.config.mjs` | Astro settings, including `site: 'https://sean.cc'` |
| `wrangler.jsonc` | Cloudflare Worker settings; `name` must stay `sean-cc` |

## Writing workflow

1. Draft in the Obsidian vault (iCloud, not in this repo).
2. Move finished posts to the vault's `Published` folder.
3. Copy them into `src/content/blog/`, then commit and push. *(A script to do the copying and convert Obsidian-only syntax is still on the to-do list.)*

Edit published posts in the vault, not in the repo copy. Use lowercase-hyphenated filenames (`my-first-post.md`) with title and date in the front matter.

## Theme

Monochrome dark theme with the system font (San Francisco on Macs). Colors are CSS variables at the top of `src/styles/global.css`, with a matching light version that follows the visitor's device setting. Code blocks are shown in grayscale by one CSS rule in the same file.
The starter's variable names were kept, so some names no longer describe the color: `--black` is the lightest text (headings), `--gray-dark` is body text, and `--gray-light` is a dark border tone.

## Email (iCloud+ custom domain)

`sean@sean.cc` sends and receives through iCloud+ Custom Email Domain (managed in iCloud settings). DNS is hosted on Cloudflare, where these records make it work. Keep them all **DNS only** (gray cloud); proxying the DKIM CNAME would break signing.

| Record | Value | Purpose |
| :--- | :--- | :--- |
| MX `sean.cc` | `mx01.mail.icloud.com`, `mx02.mail.icloud.com` (priority 10) | Delivers incoming mail to iCloud |
| TXT `sean.cc` | `v=spf1 include:icloud.com ~all` | Authorizes iCloud to send as sean.cc |
| CNAME `sig1._domainkey` | `sig1.dkim.sean.cc.at.icloudmailadmin.com` | DKIM signing key, hosted by Apple |
| TXT `sean.cc` | `apple-domain=...` | Apple's domain-ownership check |
| TXT `_dmarc` | `v=DMARC1; p=reject; sp=reject; adkim=s; aspf=s` | Tells receivers to reject mail that fails the checks |
| TXT `*._domainkey` | `v=DKIM1; p=` | Left from the old "no email" setup. It only covers selectors that don't exist (the `sig1` record takes precedence), so it blocks forged DKIM keys. |

DMARC is strict (`adkim=s`, `aspf=s`), so anything that sends as sean.cc must sign with DKIM for exactly `sean.cc`. Mail from iCloud does.

**If you add a sender** (a newsletter service, for example): add its SPF `include:` to the existing SPF record (one SPF record only) and its DKIM record, and confirm it signs as `sean.cc`. Set DMARC to `p=none` while testing, then back to `p=reject`.

**If you leave iCloud:** replace the MX, SPF, and DKIM records with the new provider's and delete the `apple-domain` record. To go back to "no email," use a null MX (`MX @ 0 .`) and SPF `v=spf1 -all`.

## Credit

The base styles come from [Bear Blog](https://github.com/HermanMartinus/bearblog/) (MIT license), by way of the Astro blog starter.
