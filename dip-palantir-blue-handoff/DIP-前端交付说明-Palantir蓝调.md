# DIP 决策智能平台 - 前端交付说明（Palantir 蓝调版）

## 一、交付物清单

| 类型 | 说明 |
|------|------|
| **HTML** | 单页 `dip-standalone.html`（或重命名为 `index.html`），内嵌 Tailwind 配置、全部主题 CSS、交互脚本 |
| **资源目录 `assets/`** | 见下方「资源文件列表」，需与 HTML 同层级或按路径放置 |

---

## 二、目录结构建议

```
项目根目录/
├── index.html          # 主页面（即 dip-standalone.html 重命名）
├── assets/
│   ├── 中科闻歌-logo.svg
│   ├── logo-DIP.svg             # 顶部导航用 DIP logo
│   ├── DIP-LOGO.svg             # 视频封面居中「DIP」三字 logo
│   ├── 架构图.svg               # 系统架构图（SVG 矢量）
│   ├── DIP决策智能平台视频-V2.mp4
│   ├── workflow-slide-1.png     # 轮播图 1：数据管道
│   ├── workflow-slide-2.png     # 轮播图 2：本体建模
│   ├── workflow-slide-3.png     # 轮播图 3
│   └── workflow-slide-4.png     # 轮播图 4
└── DIP-前端交付说明-Palantir蓝调.md  # 本说明
```

---

## 三、默认显示「Palantir 蓝调」主题

当前 HTML 首次加载时默认是 **Palantir 经典**。若需要一打开就是 **Palantir 蓝调**，在 HTML 中找到：

```javascript
applyTheme('palantir2'); 
```

改为：

```javascript
applyTheme('palantir'); 
```

（约在 `DOMContentLoaded` 回调内，仅此一处。）

---

## 四、资源文件列表与引用路径

所有资源均以 **相对路径** 从页面所在目录引用，路径前缀为 `assets/`：

| 文件 | 用途 | HTML 中的引用示例 |
|------|------|-------------------|
| 中科闻歌-logo.svg | 顶部导航公司 logo | `assets/中科闻歌-logo.svg` |
| logo-DIP.svg | 顶部导航 DIP logo | `assets/logo-DIP.svg` |
| DIP-LOGO.svg | 视频未播放时封面居中 logo | `assets/DIP-LOGO.svg` |
| 架构图.svg | 核心架构区系统架构图 | `assets/架构图.svg` |
| DIP决策智能平台视频-V2.mp4 | 首屏产品宣传视频 | `assets/DIP决策智能平台视频-V2.mp4` |
| workflow-slide-1.png | 数据与本体建模轮播第 1 张 | `assets/workflow-slide-1.png` |
| workflow-slide-2.png | 第 2 张 | `assets/workflow-slide-2.png` |
| workflow-slide-3.png | 第 3 张 | `assets/workflow-slide-3.png` |
| workflow-slide-4.png | 第 4 张 | `assets/workflow-slide-4.png` |

请保证上述文件与 HTML 中路径一致，否则图片/视频会 404。

---

## 五、技术栈与依赖

- **Tailwind CSS**：通过 CDN 引入，无需本地构建  
  `https://cdn.tailwindcss.com`
- **字体**：Inter（Google Fonts）
- **图标**：Lucide Icons（UMD）  
  `https://cdn.jsdelivr.net/npm/lucide@latest/dist/umd/lucide.min.js`
- **主题实现**：通过给 `<body>` 增加 class 切换，例如 `theme-palantir`（蓝调）、`theme-palantir2`（经典）等；样式全部写在页面内 `<style>` 中，无需额外 CSS 文件。

---

## 六、Palantir 蓝调主题关键信息

- **body class**：`theme-palantir`
- **主色变量**（在 `.theme-palantir` 下生效）：
  - `--color-primary: #1D4ED8`
  - `--color-secondary: #2563EB`
  - `--color-accent: #3B82F6`
- **特点**：浅灰底 + 20px 方格暗纹背景、导航与卡片蓝调强调、企业区卡片顶部蓝条、TOC 圆点高亮为主色。

若前端需要单独抽离「仅蓝调」版本，可只保留 `body.theme-palantir` 及 `.theme-palantir` 相关规则，并去掉主题选择器相关 DOM 与脚本。

---

## 七、主要交互说明

| 功能 | 说明 |
|------|------|
| 主题切换 | 右上角调色板按钮打开面板，点击对应主题卡片即可切换；当前为多主题共用一个 HTML。 |
| 立即体验平台 | 跳转至 `https://dip.wenge.com`（新标签页）。 |
| 观看产品宣传片 | 点击后平滑滚动到视频区域并自动播放，封面（DIP logo）隐藏。 |
| 视频封面 | 未播放时显示居中 `DIP-LOGO.svg`，点击任意处播放。 |
| 数据与本体建模轮播 | 左侧 4 个步骤按钮对应 4 张图，点击切换 `#workflow-ui-image` 的 `src`。 |
| 右侧 TOC | 根据滚动高亮当前区块，点击可平滑滚动到对应 section。 |
| 核心架构 / 实践案例 | 当前「了解集成方案」「查看建模流程」「探索智能体市场」「查看完整解决方案」等链接已在 HTML 中加 `hidden`，暂不展示，后续可去 `hidden` 并改 `href`。 |

---

## 八、打包发送建议

1. 将 **dip-standalone.html** 与 **assets/** 整个目录放在同一文件夹内（可将 HTML 重命名为 `index.html`）。
2. 如需默认蓝调，按第三节修改一行 `applyTheme('palantir')`。
3. 将该文件夹打成 **zip** 发给前端同事；或推送到 Git 仓库，提供仓库地址与分支。
4. 本说明文档可一并放入该文件夹或随 zip 发送。

---

## 九、备注

- 页面为响应式，已兼顾桌面与移动端（含移动端导航折叠）。
- 所有文案与结构以当前 HTML 为准，如需仅保留「Palantir 蓝调」单主题版本，可在交付前由前端按需删除其他主题的 DOM 与 CSS，并固定 `body` 为 `theme-palantir`。
