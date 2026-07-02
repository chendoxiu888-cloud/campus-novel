# 我的网站 — Personal Website

一个清新、简洁的个人网站，采用原生三件套（HTML + CSS + JavaScript）构建，支持深色/浅色主题切换，适合个人简介、技能展示及作品集用途。

---

## 目录

- [技术架构](#技术架构)时代的
- [项目结构](#项目结构)
- [核心功能](#核心功能)
- [技术细节](#技术细节)
- [如何运行](#如何运行)
- [自定义指南](#自定义指南)
- [部署建议](#部署建议)
- [浏览器兼容性](#浏览器兼容性)

---

## 技术架构阿巴巴

```
┌─────────────────────────────────────────────┐
│                用户浏览器                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ index.html│  │styles.css│  │script.js │   │
│  │ (语义结构)│  │(视觉表现)│  │(交互逻辑)│   │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘   │
│       └──────────────┼─────────────┘          │
│                      │                        │
│             纯静态网站 · 无构建步骤             │
│             无框架 · 无运行时依赖               │
└─────────────────────────────────────────────┘
```

### 层次划分

| 层次 | 文件 | 职责 |
|------|------|------|
| **结构层** | `index.html` | 页面语义化标记、内容结构、SEO 基础元信息 |
| **样式层** | `styles.css` | 视觉语言、布局系统、响应式设计、主题变量 |
| **行为层** | `script.js` | DOM 交互、本地存储读写、主题切换逻辑 |

### 设计决策

- **零依赖策略** — 不使用任何前端框架（React/Vue）、CSS 框架（Tailwind/Bootstrap）或构建工具（Webpack/Vite）。源文件即部署产物，打开即可运行，学习成本极低。
- **CSS 自定义属性** — 利用 `:root` 和 `html[data-theme]` 实现主题管理，无需预处理器。
- **语义化 HTML** — 使用 `<header>`、`<main>`、`<section>`、`<footer>` 等标签增强可访问性与 SEO。
- **CSS Grid / Flexbox** — 全页面采用现代布局方案，无浮动、无表格。

---

## 项目结构

```
my web/
├── index.html        # 主页面 — 个人简介、技能、联系信息
├── styles.css        # 全局样式 — 主题变量、布局、组件样式
├── script.js         # 客户端脚本 — 主题切换与本地存储
└── README.md         # 项目文档（本文件）
```

文件总数：**4**，所有文件均为纯文本格式。

---

## 核心功能

### 1. 深色/浅色主题切换奋斗奋斗

- 点击右上角按钮在 **深色模式** 和 **浅色模式** 间切换。
- 用户偏好通过 `localStorage` 持久化，刷新页面后保持上次选择。
- 首次访问时自动跟随操作系统主题设置（`prefers-color-scheme`）。

### 2. 响应式布局

- 在移动端（宽度 ≤ 760px）自动切换为单列布局。
- 头像区域、技能卡片网格均采用自适应 `grid-template-columns`。
- 文字字号使用 `clamp()` 函数在不同屏幕尺寸下平滑缩放。

### 3. 视觉风格

- 柔和渐变背景、大圆角卡片、精致阴影，营造干净现代的视觉语言。
- 主色调使用品牌蓝色（`#3b82f6`），暗色模式下切换为更柔和的浅蓝（`#60a5fa`）。
- 全页面使用 `Inter` 字体（系统回退 `system-ui`），兼顾美观与加载速度。

---

## 技术细节

### CSS 变量体系

```css
:root {                        /* 浅色主题（默认） */
  --bg: #f5f7fb;
  --surface: #ffffff;
  --text: #1f2937;
  --primary: #3b82f6;
}

html[data-theme='dark'] {     /* 深色主题 */
  --bg: #0f172a;
  --surface: #111827;
  --text: #e2e8f0;
  --primary: #60a5fa;
}
```

所有组件均引用这些变量，切换主题只需更改变量值，无需重写样式。

### 主题切换逻辑（script.js）

```
用户点击按钮
  → 读取当前 data-theme 属性值
  → 切换 light / dark
  → 更新 data-theme 属性
  → 保存到 localStorage
  → 更新按钮文字
```

初始化流程：

```
页面加载
  → localStorage 中有保存的主题？  → 使用保存的主题
  → 否                            → 检测 prefers-color-scheme
                                    → 跟随系统设置或默认浅色
```

### 响应式断点

| 断点 | 目标设备 | 变化 |
|------|---------|------|
| ≤ 760px | 手机/小平板 | Hero 区域切换为单列、主题按钮全宽 |

### 性能特征

- **零外部请求**（未引用外部字体 CDN 等，仅有 3 个本地文件）。
- 文件大小总计约 **6 KB**（未压缩），加载时间可忽略。
- 无 JavaScript 框架运行时开销。

---

## 如何运行

由于项目完全由静态文件组成，**无需安装任何依赖**，直接通过浏览器打开即可。

### 方式一：直接打开（推荐）

在 VS Code 中右键 `index.html` → 选择 **"Open with Live Server"**（如果安装了 Live Server 扩展），或直接双击文件在浏览器中打开。

### 方式二：本地预览

```bash
# 使用 Python 内置 HTTP 服务器
cd "my web"
python3 -m http.server 8080
# 然后访问 http://localhost:8080
```

### 方式三：VS Code Live Server

```bash
# 安装 Live Server 扩展后
# 右键 index.html → Open with Live Server
```

---

## 自定义指南

### 替换个人信息

编辑 `index.html` 中的以下占位内容：

| 占位内容 | 位置 |
|---------|------|
| `你的名字` | `h1`（品牌）、`h2`（Hero）、footer |
| `youremail@example.com` | 联系方式区 |
| `github.com/yourname` | 联系方式区 |
| 个人描述文字 | Hero 区、关于我区 |

### 调整主题色

修改 `styles.css` 中 `:root` 和 `html[data-theme='dark']` 下的 `--primary` 变量值即可。

### 增删页面区块

- **新增卡片**：在 `styles.css` 中使用 `.card` 类包裹任意 HTML 结构。
- **修改布局**：调整 `.grid` 的 `grid-template-columns` 值控制列数。
- **修改头像**：替换 `.avatar` 中的 emoji 或改为 `<img>` 标签。

---

## 部署建议

### 静态托管平台（免费）

| 平台 | 部署方式 |
|------|---------|
| **Vercel** | 连接 GitHub 仓库，自动部署 |
| **Netlify** | 拖拽 `my web` 文件夹或连接 Git |
| **GitHub Pages** | 推送至仓库的 `main` 分支即可 |
| **Cloudflare Pages** | 连接 Git 或通过 CLI 上传 |

### 部署步骤（以 GitHub Pages 为例）

```bash
cd "my web"
git init
git add .
git commit -m "Initial commit"
# 在 GitHub 创建仓库后
git remote add origin https://github.com/你的用户名/你的仓库.git
git push -u origin main
```

然后在仓库 Settings → Pages 中选择 `main` 分支部署。

---

## 浏览器兼容性

| 特性 | 支持范围 |
|------|---------|
| CSS Custom Properties | Chrome 49+, Firefox 31+, Safari 9.1+ |
| CSS Grid | Chrome 57+, Firefox 52+, Safari 10.1+ |
| `clamp()` | Chrome 79+, Firefox 75+, Safari 13.1+ |
| `localStorage` | Chrome 4+, Firefox 3.5+, Safari 4+ |
| `prefers-color-scheme` | Chrome 76+, Firefox 67+, Safari 12.1+ |
| ES6+ (箭头函数、模板字符串) | Chrome 49+, Firefox 45+, Safari 10+ |

所有主流现代浏览器均可正常使用。IE 11 不在此支持范围内。

---

## 许可

该项目采用 MIT 许可证。你可以自由使用、修改和分发。
