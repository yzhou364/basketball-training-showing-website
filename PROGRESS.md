# 项目进度 Log

> 按时间倒序记录，每条包含：日期、动作、产出/决策、下一步

---

## 2026-05-06 · Session 7 — 邮箱 + 兜底文案

- 邮箱 `jtabb25@gmail.com` 替换全站三处 `[...@example.com]`：Contact 区、Foundation 按钮（带 mailto subject 预填 "JTElite Foundation Support"）、Footer
- Testimonial 名字写成中性占位（"M. J. · Player · Class of 2027" / "T. W. · Parent of a 14U athlete"）——不指向真人，等 Joshua 拿到真实授权再换
- FAQ 5 条答案写完整：年龄段（8–18）、地点（St. Charles 周边合作场地）、首次评估流程、24h 取消政策、套餐/兄弟姐妹/团体折扣
- 重新跑 `npm run build`

## 2026-05-06 · Session 6 — JTElite 品牌化 + 真实素材接入

- **用户提供**：
  - 设计稿 `b87dacd4-...png`（揭示完整品牌：站点叫 JTElite，含 Foundation 慈善线）
  - 4 张教练训练实拍（IMG_0289 / 0724 / 1381 / 9697，4032×3024 EXIF orientation=6 横置）
  - 1 张更高清的 Frontier Eagle 队伍合影（4760a797）
  - 2 个 logo（`logo.png` 全标 + `logo2.png` 紧凑标）

- **图片处理（PIL）**：
  - 用 `ImageOps.exif_transpose()` 把 4 张 IMG 的 EXIF 旋转烤进字节，保证所有浏览器一致
  - 长边压到 1600px，JPEG q=88，单张约 330–390KB
  - 命名 → `coaching-04-shooting-form` / `coaching-05-dual-training` / `coaching-06-mentorship-champions` / `coaching-07-mentorship`
  - `coaching-01-team-championship` 替换为 4760a797 的高清版本
  - 设计稿移到 `pic/_originals/design-mockup.png` 留作参考

- **HTML 大改**（按设计稿提取的真实文案）：
  - **品牌**：`Joshua Tabb` → `JTElite`，nav 改用 `logo2.png` PNG logo
  - **Nav 链接**：Training / Foundation / About / Gallery / FAQ / Contact
  - **Nav CTA**：保留 `Book Training`（outline）+ 新增 `Donate Now`（橙色 solid，跳到 Foundation）
  - **Hero 文案**：替换为 "Built from Division I experience and real player development..."
  - **新增 Stats 横条**：10+ Years / 300+ Athletes / 20+ Programs（独立 section，白底）
  - **About 重写**：标题改 "From Southern Illinois to Division I."，bio 双段落写明 FIU → Tennessee → Lindenwood、D-I 履历，加 pull quote "JTElite was built to give athletes..."；左侧 4 图错位 collage（含 Tennessee + 教练实拍）
  - **Programs 卡片**：更新到 mockup 文案（Film + breakdown / Live decision-making / Conditioning + skill integration），价格栏换成 `Learn More →` 链接
  - **Gallery**：用新教练实拍替换部分老图，竖图放在 row-span-2 大格
  - **Testimonials**：从 3 条占位简化为 2 条 mockup 真实引言（人名仍占位待补）
  - **新增 Foundation section**：用 `logo.png` 全标，标题 "Talent isn't enough. Opportunity is."，CTA `Support the Foundation`
  - **Footer**：彻底重做——深底 ink 色、4 列（Logo + 3 列链接 + 联系方式 + 社交图标）、版权改 JTElite Training & Foundation

- **构建**：`npm run build` → `tailwind.css` 21KB（新增 `bg-accent` / `border-l-2` / `lg:h-11` 等都已生成）

- **仍占位（待 Joshua 补）**：
  - 邮箱
  - 2 条 testimonial 的人名 + 学校
  - FAQ 5 个问题的答案
  - Foundation 邮箱（目前 `mailto:[foundation@example.com]`）
  - 社交链接 URL
  - About 的 bio 是基于设计稿文字 + 图片信息整合写的，建议 Joshua 通读校对

- **构图待用户验证**：Foundation headline 我用了"Talent isn't enough. Opportunity is."——设计稿原句被前景元素遮挡，这是我对其语义的合理改写；如果 Joshua 有原句可替换

## 2026-04-19 · Session 5 — Tailwind 迁移到预编译（修 Chrome 渲染差）

- **问题**：用户反馈 Chrome 体验明显差于其他浏览器
- **根因**：Tailwind **Play CDN**（`<script src="cdn.tailwindcss.com">`）在浏览器里运行时 JIT 编译 CSS——所有浏览器都会 FOUC，Chrome 因字体/CORS 策略更严格所以更明显
- **解决**：迁移到 **Tailwind CLI v4 预编译**
  - 新增 `package.json`（script: `build` / `dev` / `serve`）
  - 新增 `src/input.css`（用 `@theme` 把原来 inline 的配色/字体 token 搬过来）
  - 新增 `tailwind.css`（19KB 构建产物，由 CLI 生成，直接 link）
  - `index.html` 移除 CDN script + inline config，改为 `<link href="tailwind.css">`
  - `.gitignore` 追加 `node_modules/` 和 `package-lock.json`
- **效果**：
  - 零运行时编译 → 首屏即样式齐全，无 FOUC
  - Chrome / Safari / Firefox / Edge / 手机浏览器表现一致
  - 总 CSS 体积从 "CDN 脚本 + 运行时生成" 变成一个 19KB 静态文件
- **开发流程更新**：
  - 改样式 → 跑 `npm run build`（或开着 `npm run dev` 实时构建）
  - 本地预览 → `npm run serve`（= `python3 -m http.server 8000`）
- **仍待优化（非阻塞）**：图片尺寸 attr、favicon、OG 图、GA

## 2026-04-19 · Session 4 — Hero 图构图修正

- **问题**：用户反馈 Hero 图位置不对，截图显示顶部一大条黑，主体被推到下方
- **根因**：两张 Tennessee 照片是 iPhone 截图（1170x2532），上下都有 iOS letterbox 黑边；`object-top` 把黑边顶到了视口上方
- **解决**：
  - 写 Python/PIL 脚本检测黑边并裁掉，原图备份在 `pic/_originals/`
  - 朴素阈值法失败（底部 iOS home indicator 白条干扰检测）→ 改用**窗口平滑 dark% 法**（窗口 40 行，均值 ≥80% 暗判定为 letterbox），两张都干净裁出
  - `career-01`: 2532 → 1734（裁掉顶 405 / 底 393）
  - `career-02`: 2532 → 2095（裁掉顶 273 / 底 164）
  - Hero `<img>` `object-top` 改回 `object-center`，现在主体居中
- **经验沉淀**：未来如果用户再丢 iPhone 截图进来，直接跑 `pic/_originals/` 里备份的那套脚本即可复用

## 2026-04-19 · Session 3 — 素材整理 + 真实信息注入

- **用户新增**：`info/josh.md`（姓名/电话/地点）+ 6 张 jpg 散落在根目录
- **图片归档到 `pic/`**（重命名以分组管理，`career-*` 是教练球员时期，`coaching-*` 是现在青训）：
  - `career-01-tennessee-celebration.jpg` — Tennessee #25 赛后庆祝（用作 Hero 主视觉）
  - `career-02-tennessee-vs-kansas.jpg` — Tennessee vs Kansas 对位
  - `career-03-nike-allamerica-2004.jpg` — Nike All-America Camp 2004 合影
  - `coaching-01-team-championship.jpg` — 青训队伍领奖合影（Frontier Eagle 场馆）
  - `coaching-02-dribbling-drill.jpg` — 学员户外运球训练
  - `coaching-03-summer-camp.jpg` — 夏令营合影
- **index.html 更新**：
  - `[Coach Name]` → Joshua Tabb（全局替换）
  - `[City]` → St. Charles, MO
  - 电话 → +1 (305) 923-7319
  - Hero 占位图 → `career-01-tennessee-celebration.jpg`（4:5 竖构图适配）
  - Results gallery 5 张全部接入真实照片（含一张竖图放在 row-span-2 高栏位）
  - 所有 `<img>` 加了 `alt` 和 `loading="lazy"`
  - `<title>` 加入 "St. Charles, MO" 利好本地 SEO
- **仍为占位符（待用户补充）**：
  - 邮箱 `[coach@example.com]`
  - Hero / About 的一段式文案（建议突出 Tennessee D1 + Nike All-America 背景 → 建立权威）
  - 3 门课程的价格
  - 3 条学员/家长评价
  - FAQ 5 条答案
  - 社交媒体链接（Instagram / YouTube / WeChat）
- **下一步建议**：
  1. 让 Joshua 把邮箱和 Hero/About 两段英文 bio 发来，我来润色嵌入
  2. 表单接入真实邮件收件（推荐 Formspree，免费额度够用）
  3. 加 favicon（篮球小图标 SVG 内嵌即可）
  4. 推到 GitHub 开 Pages 先跑起来

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
