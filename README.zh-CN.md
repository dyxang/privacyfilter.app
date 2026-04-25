# 在线隐私过滤器

[English](./README.md) · **简体中文** · [繁體中文](./README.zh-TW.md) · [日本語](./README.ja.md)

一个免费、运行在浏览器本地的**隐私移除工具**，可识别文本中的 PII（姓名、邮箱、电话、地址、账号、日期、URL、密钥），一键生成脱敏文本。底层使用 [`openai/privacy-filter`](https://huggingface.co/openai/privacy-filter) 模型，通过 [Transformers.js](https://github.com/huggingface/transformers.js) 在本地推理——**文本永不离开你的设备**。

> 线上地址：[privacyfilter.app](https://privacyfilter.app)

## 功能特性

- **100% 本地推理** —— 无服务器、无上传、无追踪
- **WebGPU 加速**，不支持时自动回退到 WebAssembly
- **8 种 PII 类别** 在 token 粒度上以 BIOES 标签输出：
  - `private_person` · `private_email` · `private_phone` · `private_address`
  - `account_number` · `private_date` · `private_url` · `secret`
- **图片 OCR + PII 识别** —— 通过本地 Tesseract OCR 识别截图中的文本并遮蔽敏感区域
- **多语言界面** —— English、简体中文、繁體中文、日本語
- **静态站点** —— 可部署到任意静态托管（Cloudflare Pages、Vercel、Netlify、GitHub Pages）

## 应用场景

- **AI 对话前的隐私清理** —— 在粘贴到 ChatGPT、Claude、Gemini 之前先去除个人信息
- **日志与工单脱敏** —— 在共享错误日志或 Bug 报告前清除客户敏感信息
- **安全分享文档** —— 截图或文本分享前打码姓名、地址、账号
- **密钥审计** —— 排查文档或聊天记录中误粘贴的 API Key 和访问令牌

## 技术栈

| 分层       | 选型                                   |
| ---------- | -------------------------------------- |
| 框架       | [Astro 5](https://astro.build)         |
| 样式       | [Tailwind CSS v4](https://tailwindcss.com) |
| ML 运行时  | [@huggingface/transformers](https://github.com/huggingface/transformers.js) (Transformers.js) |
| 模型       | [`openai/privacy-filter`](https://huggingface.co/openai/privacy-filter) |
| OCR        | [tesseract.js](https://github.com/naptha/tesseract.js) |
| 部署       | Cloudflare Workers / 静态托管          |

## 快速开始

### 环境要求

- Node.js 20+
- pnpm 9+

### 安装与运行

```bash
pnpm install
pnpm dev
```

首次运行会从 Hugging Face 下载 `openai/privacy-filter` 模型权重（约 50 MB）并缓存到浏览器。

### 构建

```bash
pnpm build       # 输出到 ./dist
pnpm preview     # 本地预览构建产物
```

### 部署（Cloudflare）

```bash
pnpm deploy      # pnpm build && wrangler deploy
```

## 配置

将 `.env.example` 复制为 `.env`，按需配置统计脚本：

```env
PUBLIC_ANALYTICS_DOMAIN=yourdomain.com
PUBLIC_ANALYTICS_SRC=https://analytics.example.com/script.js
```

留空即可完全禁用统计。

## 浏览器支持

| 浏览器        | 性能路径              |
| ------------- | --------------------- |
| Chrome / Edge | WebGPU（最快）        |
| Safari 17+    | WebAssembly           |
| Firefox       | WebAssembly           |

需要 `SharedArrayBuffer`。站点会对 HTML 响应设置 `Cross-Origin-Opener-Policy: same-origin` 与 `Cross-Origin-Embedder-Policy: require-corp`：开发环境由 Vite（`astro.config.mjs`）配置，生产环境由 Cloudflare Worker（`src/worker.ts`）注入。如果部署到其他托管，请自行保证 HTML 响应带有相同的 headers。

## 项目结构

```
src/
├── lib/
│   └── i18n.js               # 多语言数据、SEO 元信息、示例文本
├── pages/
│   └── [...locale].astro     # 单一模板，4 条语言路由共享
└── styles/
    └── global.css            # Tailwind v4 主题与自定义样式
```

路由使用 Astro 的剩余参数动态路由，`/`、`/ja`、`/zh-hans`、`/zh-hant` 共享同一模板。

## 常见问题

**我的文本会被上传到服务器吗？**
不会。推理通过 Transformers.js 完全在浏览器中完成，除了从 Hugging Face 下载模型文件之外，文本不会离开本地设备。

**为什么第一次检测比较慢？**
首次运行需要下载模型权重，后续访问会从浏览器缓存读取，速度明显更快。

**可以替代正则表达式脱敏吗？**
对自然语言文本，基于上下文的模型能识别正则难以匹配的多语言姓名和自由格式地址。两者结合可获得最佳覆盖率。

## 参与贡献

欢迎 Issue 与 PR。较大改动请先开 Issue 讨论方向。

## 开源协议

基于 [MIT License](./LICENSE) 发布。

打包依赖（Transformers.js、tesseract.js 等）的第三方归属信息见 [NOTICE](./NOTICE)。模型权重（`openai/privacy-filter`）遵循 Apache-2.0 协议，详见 [模型卡片](https://huggingface.co/openai/privacy-filter)。

## 致谢

- [openai/privacy-filter](https://huggingface.co/openai/privacy-filter) —— 底层的 token 分类模型
- [Transformers.js](https://github.com/huggingface/transformers.js) —— 将 Hugging Face 模型带入浏览器
- [Astro](https://astro.build) —— 静态优先、零 JS by default 的框架
