# 線上隱私過濾器

[English](./README.md) · [简体中文](./README.zh-CN.md) · **繁體中文** · [日本語](./README.ja.md)

一款免費、在瀏覽器本機運行的**隱私移除工具**，可辨識文字中的 PII（姓名、電子郵件、電話、地址、帳號、日期、URL、金鑰），一鍵產生去識別化文字。底層使用 [`openai/privacy-filter`](https://huggingface.co/openai/privacy-filter) 模型，透過 [Transformers.js](https://github.com/huggingface/transformers.js) 在本機推論——**文字不會離開你的裝置**。

> 線上網址：[privacyfilter.app](https://privacyfilter.app)

## 功能特色

- **100% 本機推論** —— 無伺服器、不上傳、不追蹤
- **WebGPU 加速**，不支援時自動回退到 WebAssembly
- **8 種 PII 類別** 在 token 粒度上以 BIOES 標籤輸出：
  - `private_person` · `private_email` · `private_phone` · `private_address`
  - `account_number` · `private_date` · `private_url` · `secret`
- **圖片 OCR + PII 辨識** —— 透過本機 Tesseract OCR 辨識截圖文字並遮蔽敏感區域
- **多語言介面** —— English、简体中文、繁體中文、日本語
- **靜態站台** —— 可部署至任何靜態主機（Cloudflare Pages、Vercel、Netlify、GitHub Pages）

## 應用情境

- **AI 對話前的隱私清理** —— 在貼到 ChatGPT、Claude、Gemini 之前先移除個人資料
- **日誌與工單去識別化** —— 分享錯誤日誌或 Bug 報告前清除客戶敏感資訊
- **安全分享文件** —— 截圖或文字分享前遮蔽姓名、地址、帳號
- **金鑰稽核** —— 排查文件或聊天紀錄中誤貼的 API 金鑰與權杖

## 技術堆疊

| 分層       | 選型                                   |
| ---------- | -------------------------------------- |
| 框架       | [Astro 5](https://astro.build)         |
| 樣式       | [Tailwind CSS v4](https://tailwindcss.com) |
| ML 執行時期 | [@huggingface/transformers](https://github.com/huggingface/transformers.js) (Transformers.js) |
| 模型       | [`openai/privacy-filter`](https://huggingface.co/openai/privacy-filter) |
| OCR        | [tesseract.js](https://github.com/naptha/tesseract.js) |
| 部署       | Cloudflare Workers / 靜態主機          |

## 快速開始

### 環境需求

- Node.js 20+
- pnpm 9+

### 安裝與執行

```bash
pnpm install
pnpm dev
```

首次執行會從 Hugging Face 下載 `openai/privacy-filter` 模型權重（約 50 MB）並由瀏覽器快取。

### 建置

```bash
pnpm build       # 輸出至 ./dist
pnpm preview     # 於本機預覽建置產物
```

### 部署（Cloudflare）

```bash
pnpm deploy      # pnpm build && wrangler deploy
```

## 設定

將 `.env.example` 複製為 `.env`，依需求設定統計腳本：

```env
PUBLIC_ANALYTICS_DOMAIN=yourdomain.com
PUBLIC_ANALYTICS_SRC=https://analytics.example.com/script.js
```

留空即可停用統計。

## 瀏覽器支援

| 瀏覽器         | 效能路徑               |
| -------------- | ---------------------- |
| Chrome / Edge  | WebGPU（最快）         |
| Safari 17+     | WebAssembly            |
| Firefox        | WebAssembly            |

需要 `SharedArrayBuffer`。站台會對 HTML 回應設定 `Cross-Origin-Opener-Policy: same-origin` 與 `Cross-Origin-Embedder-Policy: require-corp`：開發環境由 Vite（`astro.config.mjs`）設定，正式環境由 Cloudflare Worker（`src/worker.ts`）注入。若部署到其他主機，請自行確保 HTML 回應帶有相同的 headers。

## 專案結構

```
src/
├── lib/
│   └── i18n.js               # 多語系資料、SEO 中繼資訊、示例文字
├── pages/
│   └── [...locale].astro     # 單一模板，4 條語系路由共用
└── styles/
    └── global.css            # Tailwind v4 主題與自訂樣式
```

路由使用 Astro 的剩餘參數動態路由，`/`、`/ja`、`/zh-hans`、`/zh-hant` 共用同一模板。

## 常見問題

**我的文字會被上傳到伺服器嗎？**
不會。推論透過 Transformers.js 完全在瀏覽器內完成，除了從 Hugging Face 下載模型檔案外，文字不會離開本機。

**為什麼第一次偵測較慢？**
首次執行需要下載模型權重，後續存取會從瀏覽器快取讀取，速度明顯更快。

**可以取代正規表達式去識別化嗎？**
對自然語言文字，基於上下文的模型能辨識正規表達式難以匹配的多語言姓名與自由格式地址。兩者搭配可獲得最佳覆蓋率。

## 參與貢獻

歡迎 Issue 與 PR。較大的變更請先開 Issue 討論方向。

## 開源授權

以 [MIT License](./LICENSE) 發佈。

打包相依（Transformers.js、tesseract.js 等）的第三方歸屬資訊請見 [NOTICE](./NOTICE)。模型權重（`openai/privacy-filter`）遵循 Apache-2.0 授權，詳見 [模型卡](https://huggingface.co/openai/privacy-filter)。

## 致謝

- [openai/privacy-filter](https://huggingface.co/openai/privacy-filter) —— 底層的 token 分類模型
- [Transformers.js](https://github.com/huggingface/transformers.js) —— 將 Hugging Face 模型帶入瀏覽器
- [Astro](https://astro.build) —— 靜態優先、zero-JS by default 的框架
