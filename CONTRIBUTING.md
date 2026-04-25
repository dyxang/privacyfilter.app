# Contributing

Thanks for your interest in contributing to Privacy Filter Online! This document covers the workflow and conventions.

## Ground Rules

- **Be kind.** See our [Code of Conduct](./CODE_OF_CONDUCT.md).
- **Open an issue first** for non-trivial changes so we can align on direction before you invest time.
- **Keep PRs focused.** One logical change per PR is easier to review than a grab-bag.
- **All text stays local.** Any change that would cause user text or images to leave the browser — even for telemetry — is out of scope unless explicitly discussed.

## Development Setup

### Prerequisites

- Node.js 20+ (see `.nvmrc`)
- pnpm 9+

### Install & run

```bash
pnpm install
pnpm dev
```

Open the address Astro prints (usually http://localhost:4321). The first detection downloads the model weights from Hugging Face; subsequent runs are cached.

### Useful scripts

```bash
pnpm dev        # Start Astro dev server
pnpm check      # Type-check with @astrojs/check (astro check)
pnpm build      # Production build to ./dist
pnpm preview    # Preview the built site locally
pnpm deploy     # Build + deploy to Cloudflare (requires wrangler login)
```

CI runs `pnpm check` and `pnpm build` on every PR — please make sure both pass locally before pushing.

## Project Structure

```
src/
├── lib/i18n.js               # Locale data, SEO metadata, demo texts
├── pages/[...locale].astro   # Single template, 4 locale routes (en/ja/zh-CN/zh-TW)
├── styles/global.css         # Tailwind v4 theme + custom utility classes
└── worker.ts                 # Cloudflare Worker (HF proxy + COOP/COEP headers)
```

## Adding or Changing Translations

All UI strings live in `src/lib/i18n.js`. The four locale dictionaries (`en`, `ja`, `zh-CN`, `zh-TW`) must be kept in sync — if you add a key to one, add it to all four.

Please keep the existing tone per locale:

- **English**: direct, minimal
- **Japanese**: polite form (です/ます) for UI text
- **Simplified / Traditional Chinese**: concise, no redundant particles

## Coding Style

- Prefer `textContent` / `createElement` over `innerHTML` for any DOM mutation driven by data.
- Keep inline comments to the essential "why" — avoid restating what the code does.
- Use Tailwind utility classes; if you need custom CSS, add it to `src/styles/global.css`.
- TypeScript for worker code, vanilla JS in browser scripts (to keep bundle simple).

## Commit Messages

Follow imperative, present-tense summaries. Example:

```
Add Japanese translations for image buttons

- imageUploadButton, imageAddMaskButton, imageDownloadButton
- Keeps Japanese UI consistent with the rest of the locale.
```

## License

By submitting a pull request, you agree that your contribution will be licensed under the [MIT License](./LICENSE).
