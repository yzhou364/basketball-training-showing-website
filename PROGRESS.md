# 项目进度 Log

> 按时间倒序记录，每条包含：日期、动作、产出/决策、下一步

---

## 2026-04-19 · Session 2 — 骨架搭建完成

- **用户决策确认**：
  - 内容：全部占位符，后续替换
  - 部署：GitHub Pages
  - 风格：简约高端（minimal & premium）
  - 语言：英文
- **技术选型敲定**：单 `index.html` + Tailwind Play CDN + 原生 JS，零构建
- **产出文件**：
  - `index.html` — 7 个模块全部就位：Nav / Hero / About / Programs / Results / Testimonials / FAQ / Contact / Footer
  - `styles.css` — 滚动揭示动画、nav 滚动背景模糊、占位图斜纹底、reduced-motion 兼容
  - `main.js` — 移动端菜单、滚动样式切换、IntersectionObserver 滚动动画、年份自动填充
  - `.nojekyll` — 防止 GitHub Pages 走 Jekyll 构建
- **设计决策**：
  - 配色：`#fafaf7` / `#0a0a0a` / 深橙点缀 `#c2410c`
  - 字体：Fraunces（serif 标题）+ Inter（sans 正文），Google Fonts CDN
  - Hero 大字排版 + 斜体强调词，呼应高端杂志感
  - 课程卡片采用三栏共边框设计（bg-line + gap-px 技巧）
  - Testimonials 反色深底，形成视觉节奏断点
  - FAQ 用原生 `<details>` + 旋转 `+` 号，零 JS
- **本地预览**：在 Finder 双击 `index.html` 即可看效果，或用 VSCode Live Server
- **里程碑状态**：M1 / M2 完成 → 等用户看骨架反馈
- **下一步（等用户反馈）**：
  1. 用户预览，反馈视觉/布局调整点
  2. 用户开始提供真实素材（文案、照片、课程价格、联系方式）
  3. M4 SEO 优化：favicon、OG 图、meta tags、sitemap
  4. M5 部署：推到 GitHub 远程仓库 → 开启 Pages → 验证上线

## 2026-04-19 · Session 1 — 项目初始化

- **动作**：项目初始化，仓库 review
- **现状**：仓库为空，仅有 `.gitattributes` / `.gitignore` / `LICENSE`
- **产出**：创建 `PLAN.md` 与 `PROGRESS.md`，输出初步方案
- **决策**：暂未最终确定技术栈与视觉风格，等待用户回复 7 个待确认问题
