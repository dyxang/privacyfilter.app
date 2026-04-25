# Privacy Filter Online

**English** · [简体中文](./README.zh-CN.md) · [繁體中文](./README.zh-TW.md) · [日本語](./README.ja.md)

A free, in-browser **privacy remover** that detects PII (names, emails, phone numbers, addresses, account numbers, dates, URLs, and secrets) and redacts it in one click. Powered by [`openai/privacy-filter`](https://huggingface.co/openai/privacy-filter) running locally via [Transformers.js](https://github.com/huggingface/transformers.js) — **your text never leaves the device**.

> Live site: [privacyfilter.app](https://privacyfilter.app)

## Features

- **100% local inference** — no servers, no uploads, no tracking
- **WebGPU accelerated**, with WebAssembly fallback for older browsers
- **8 PII categories** detected at token level via BIOES tagging:
  - `private_person` · `private_email` · `private_phone` · `private_address`
  - `account_number` · `private_date` · `private_url` · `secret`
- **Image OCR + PII** — scan screenshots with on-device Tesseract OCR, then mask sensitive regions
- **Multilingual UI** — English, 简体中文, 繁體中文, 日本語
- **Static site** — deploys anywhere (Cloudflare Pages, Vercel, Netlify, GitHub Pages)

## Use Cases

- **AI prompt sanitization** — privacy clean text before pasting into ChatGPT, Claude, or Gemini
- **Log & ticket redaction** — strip customer data from bug reports before sharing
- **Safe document sharing** — mask names, addresses, and account numbers in screenshots
- **Secret auditing** — catch API keys accidentally committed to docs or chat threads

## Tech Stack

| Layer            | Choice                                 |
| ---------------- | -------------------------------------- |
| Framework        | [Astro 5](https://astro.build)         |
| Styling          | [Tailwind CSS v4](https://tailwindcss.com) |
| ML runtime       | [@huggingface/transformers](https://github.com/huggingface/transformers.js) (Transformers.js) |
| Model            | [`openai/privacy-filter`](https://huggingface.co/openai/privacy-filter) |
| OCR              | [tesseract.js](https://github.com/naptha/tesseract.js) |
| Deployment       | Cloudflare Workers / static hosting    |

## Getting Started

### Prerequisites

- Node.js 20+
- pnpm 9+

### Install & run

```bash
pnpm install
pnpm dev
```

The first run downloads `openai/privacy-filter` weights (~50 MB) and caches them in the browser.

### Build

```bash
pnpm build       # outputs to ./dist
pnpm preview     # preview the built site locally
```

### Deploy (Cloudflare)

```bash
pnpm deploy      # pnpm build && wrangler deploy
```

## Configuration

Copy `.env.example` to `.env` and set optional analytics:

```env
PUBLIC_ANALYTICS_DOMAIN=yourdomain.com
PUBLIC_ANALYTICS_SRC=https://analytics.example.com/script.js
```

Leave these blank to disable analytics entirely.

## Browser Requirements

| Browser         | Performance path        |
| --------------- | ----------------------- |
| Chrome / Edge   | WebGPU (fastest)        |
| Safari 17+      | WebAssembly             |
| Firefox         | WebAssembly             |

Requires `SharedArrayBuffer`. The site is served with `Cross-Origin-Opener-Policy: same-origin` and `Cross-Origin-Embedder-Policy: require-corp`. These headers are configured by the Vite dev server (`astro.config.mjs`) and the Cloudflare Worker (`src/worker.ts`). If you deploy to a different host, make sure the same headers are applied to HTML responses.

## Project Structure

```
src/
├── lib/
│   └── i18n.js               # Locale data, SEO metadata, demo texts
├── pages/
│   └── [...locale].astro     # Single template, 4 locale routes
└── styles/
    └── global.css            # Tailwind v4 theme + custom utility classes
```

Routing uses Astro's rest-parameter dynamic route so `/`, `/ja`, `/zh-hans`, `/zh-hant` share one template.

## FAQ

**Is my text uploaded to a server?**
No. Inference runs entirely in your browser via Transformers.js. After the initial model download from Hugging Face, your text never leaves the device.

**Why is the first detection slow?**
The first run downloads the model weights. Subsequent runs use the browser cache and are noticeably faster.

**Can it replace a regex-based redactor?**
For natural-language text, a contextual model catches entities regex misses (multilingual names, free-form addresses). Combining both gives the best coverage.

## Contributing

Issues and PRs are welcome. Please open an issue first for larger changes so we can discuss direction.

## License

Released under the [MIT License](./LICENSE).

Third-party attributions for bundled dependencies (Transformers.js, tesseract.js, etc.) are listed in [NOTICE](./NOTICE). Model weights (`openai/privacy-filter`) are distributed under Apache-2.0 — see the [model card](https://huggingface.co/openai/privacy-filter) for details.

## Acknowledgments

- [openai/privacy-filter](https://huggingface.co/openai/privacy-filter) — the underlying token classification model
- [Transformers.js](https://github.com/huggingface/transformers.js) — bringing Hugging Face models to the browser
- [Astro](https://astro.build) — the shipped-static, zero-JS-by-default framework
