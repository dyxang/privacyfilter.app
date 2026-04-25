# プライバシーフィルター オンライン版

[English](./README.md) · [简体中文](./README.zh-CN.md) · [繁體中文](./README.zh-TW.md) · **日本語**

ブラウザ内で完結する無料の**プライバシー除去ツール**。テキスト中の PII（氏名、メール、電話番号、住所、口座番号、日付、URL、secret）を検出し、ワンクリックでマスクできます。[`openai/privacy-filter`](https://huggingface.co/openai/privacy-filter) を [Transformers.js](https://github.com/huggingface/transformers.js) で実行し、**テキストがデバイスから外に出ることはありません**。

> 公式サイト：[privacyfilter.app](https://privacyfilter.app)

## 主な機能

- **100% ローカル推論** —— サーバーなし、アップロードなし、トラッキングなし
- **WebGPU 高速化**、未対応環境では WebAssembly に自動フォールバック
- **8 種類の PII カテゴリ** をトークン単位で BIOES タグ付け：
  - `private_person` · `private_email` · `private_phone` · `private_address`
  - `account_number` · `private_date` · `private_url` · `secret`
- **画像 OCR + PII 検出** —— ローカル Tesseract OCR でスクリーンショットを読み取り、機微領域をマスク
- **多言語 UI** —— English、简体中文、繁體中文、日本語
- **静的サイト** —— Cloudflare Pages、Vercel、Netlify、GitHub Pages など任意のホスティングに配置可能

## ユースケース

- **LLM プロンプトの事前整形** —— ChatGPT、Claude、Gemini に貼り付ける前に個人情報を除去
- **ログ・チケットの匿名化** —— サポートチケットやエラーログを共有する前に顧客情報をマスク
- **ドキュメントの安全共有** —— スクリーンショットや本文の共有前に氏名・住所・口座番号を伏せ字化
- **Secret の検出** —— ドキュメントやチャットに混入した API キー・トークンを発見

## 技術スタック

| レイヤー     | 選定                                     |
| ------------ | ---------------------------------------- |
| フレームワーク | [Astro 5](https://astro.build)           |
| スタイル     | [Tailwind CSS v4](https://tailwindcss.com) |
| ML ランタイム | [@huggingface/transformers](https://github.com/huggingface/transformers.js) (Transformers.js) |
| モデル       | [`openai/privacy-filter`](https://huggingface.co/openai/privacy-filter) |
| OCR          | [tesseract.js](https://github.com/naptha/tesseract.js) |
| デプロイ     | Cloudflare Workers / 静的ホスティング    |

## セットアップ

### 要件

- Node.js 20+
- pnpm 9+

### インストールと起動

```bash
pnpm install
pnpm dev
```

初回実行時に Hugging Face から `openai/privacy-filter` のモデル重み（約 50 MB）がダウンロードされ、ブラウザにキャッシュされます。

### ビルド

```bash
pnpm build       # ./dist に出力
pnpm preview     # ビルド成果物をローカルでプレビュー
```

### デプロイ（Cloudflare）

```bash
pnpm deploy      # pnpm build && wrangler deploy
```

## 設定

`.env.example` を `.env` にコピーし、必要に応じてアナリティクスを設定してください：

```env
PUBLIC_ANALYTICS_DOMAIN=yourdomain.com
PUBLIC_ANALYTICS_SRC=https://analytics.example.com/script.js
```

空のままにするとアナリティクスは無効化されます。

## 対応ブラウザ

| ブラウザ      | 実行経路               |
| ------------- | ---------------------- |
| Chrome / Edge | WebGPU（最速）         |
| Safari 17+    | WebAssembly            |
| Firefox       | WebAssembly            |

`SharedArrayBuffer` が必要なため、HTML レスポンスに `Cross-Origin-Opener-Policy: same-origin` と `Cross-Origin-Embedder-Policy: require-corp` を付与します。開発は Vite（`astro.config.mjs`）、本番は Cloudflare Worker（`src/worker.ts`）が設定します。別のホスティングにデプロイする場合は、同じヘッダーを HTML レスポンスに付与してください。

## プロジェクト構成

```
src/
├── lib/
│   └── i18n.js               # ロケールデータ、SEO メタデータ、デモテキスト
├── pages/
│   └── [...locale].astro     # 単一テンプレート、4 ロケールで共有
└── styles/
    └── global.css            # Tailwind v4 テーマとカスタムユーティリティ
```

Astro の rest パラメータ動的ルートを利用し、`/`、`/ja`、`/zh-hans`、`/zh-hant` が同一テンプレートを共有します。

## FAQ

**入力したテキストはサーバーに送信されますか？**
いいえ。推論は Transformers.js によりブラウザ内で完結します。Hugging Face からの初回モデルダウンロード以外、テキストはデバイスから外に出ません。

**初回の検出が遅いのはなぜですか？**
初回のみモデル重みをダウンロードします。2 回目以降はブラウザキャッシュから読み込まれ、大幅に高速化します。

**正規表現ベースのマスク処理の代わりになりますか？**
自然言語のテキストでは、文脈を理解するモデルが正規表現では拾えない多言語の氏名や自由形式の住所も検出できます。両者の併用が最も高精度です。

## コントリビュート

Issue と PR を歓迎します。大きな変更の場合は、まず Issue で方向性を相談してください。

## ライセンス

[MIT License](./LICENSE) で公開しています。

バンドルされる依存（Transformers.js、tesseract.js など）のサードパーティ帰属情報は [NOTICE](./NOTICE) を参照してください。モデル重み（`openai/privacy-filter`）は Apache-2.0 ライセンスです。詳細は [モデルカード](https://huggingface.co/openai/privacy-filter) を参照してください。

## 謝辞

- [openai/privacy-filter](https://huggingface.co/openai/privacy-filter) —— ベースとなるトークン分類モデル
- [Transformers.js](https://github.com/huggingface/transformers.js) —— Hugging Face モデルをブラウザへ
- [Astro](https://astro.build) —— 静的優先・zero-JS by default のフレームワーク
