# sean.cc

Source for [sean.cc](https://sean.cc), my personal site. Built with [Astro](https://docs.astro.build) and deployed to Cloudflare Workers as a static site.

## Run it locally

Requires Node.js 22.12 or newer.

```sh
npm install
npm run dev
```

The dev server runs at http://localhost:4321. To test the production build the way Cloudflare serves it: `npx astro build && npx wrangler dev`.

## Deploying

A push to `main` builds and deploys automatically. A failed build doesn't replace the live site.

## Credit

The base styles come from [Bear Blog](https://github.com/HermanMartinus/bearblog/) (MIT license), by way of the Astro blog starter.
