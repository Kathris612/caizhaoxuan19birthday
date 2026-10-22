# 蔡兆轩 · 十九岁生日快乐

一个纯静态、零外部依赖的生日祝福网站。深墨蓝夜色、香槟金与灰粉玫瑰，核心是一只手绘质感的三层生日蛋糕。页面收件人为「蔡兆轩」，年龄为「19」。

## 当前公网地址

<https://kathris612.github.io/caizhaoxuan19birthday/>

站点发布在 GitHub Pages，其他电脑可直接访问，不依赖本地电脑保持开机。源码仓库：

<https://github.com/Kathris612/caizhaoxuan19birthday>

## 这一版的特别设计

- **云朵肖像蛋糕顶饰**：用户提供的粉紫云朵图片被裁成拱形，嵌入手绘金色相框，立在蛋糕最上层
- **3D 翻转效果**：右下角「翻转」按钮让整个蛋糕连同相框完成一次 360° 翻转，翻到侧面时会扫过一层金色光晕
- **轻微 3D 视差**：桌面端移动鼠标时，蛋糕与相框会随视角轻轻倾斜
- 点击「许个愿 · 吹蜡烛」：烛火熄灭、轻烟升起、烟花与彩纸落下
- 点击蛋糕：撒下一小簇金色星光
- 右上角「音乐」：浏览器实时合成的音乐盒旋律，不加载任何音频文件
- 响应式布局，支持手机、平板与桌面
- 尊重系统「减少动态效果」设置

## 本地预览

```bash
cd caizhaoxuan19birthday
python -m http.server 8089
```

打开 <http://127.0.0.1:8089/>。

## 文件结构

```text
caizhaoxuan19birthday/
├─ index.html              页面结构 + 手绘 SVG 蛋糕 + 云朵相框
├─ 404.html                同风格 404 页面
├─ css/
│  └─ style.css            视觉、翻转动画与响应式布局
├─ js/
│  └─ main.js              星光、烟花、吹蜡烛、翻转、音乐与视差
├─ assets/
│  ├─ cloud-portrait.png   蛋糕顶部的云朵肖像
│  ├─ favicon.svg
│  └─ grain.svg
├─ og-image.png            1200 × 630 社交分享图
├─ robots.txt
├─ sitemap.xml
├─ _headers                Cloudflare Pages / Netlify 安全响应头
└─ README.md
```

## 修改名字和年龄

全局搜索 `蔡兆轩`，替换成新的名字。年龄出现在：

- `index.html` 中的 `19`、`NINETEEN`、`01 / 19`
- `404.html` 的标题与装饰数字
- `README.md` 与 `robots.txt` / `sitemap.xml` 中的地址

## 替换顶部的云朵图片

1. 将新图片保存为 `assets/cloud-portrait.png`，建议使用正方形图片。
2. 在 `index.html` 中搜索 `cloud-portrait.png`，图片会通过 `clip-path="url(#portraitClip)"` 自动裁成拱形。
3. 如需调整相框大小，修改 `portraitClip` 与相框 `path` 的坐标；其余蜡烛位置在 `transform="translate(290 327)"` 等四组烛台坐标中。

## 调整翻转动画

- 翻转时长：`css/style.css` 中 `.cake-3d.is-flipping .cake-svg` 的 `1.75s`，以及 `js/main.js` 中 `flipCake()` 的 `1820` 毫秒收尾时间。
- 翻转角度：修改 `@keyframes cakeFlip` 中的 `rotateY(...)`。
- 关闭翻转按钮：删除 `index.html` 中的 `#flip-btn`，或保留按钮但移除 `js/main.js` 中的 `flipButton.addEventListener("click", flipCake);`。

## 改祝福语与配色

在 `index.html` 中搜索：

- `十九岁，是风开始有了方向，`
- `愿你此后的每一天，`
- `愿望已被星光收好。十九岁的你，请继续发光。`

配色在 `css/style.css` 顶部的 `:root`：

```css
--ink: #090b11;          /* 背景 */
--gold: #d6b37a;         /* 金色 */
--ivory: #f7ede1;        /* 象牙白 */
--rose: #d6a39d;         /* 灰粉 */
```

## 绑定自定义域名（可选）

目前公网地址是 GitHub Pages 子路径。如之后注册了独立域名：

1. 在域名服务商处添加 CNAME：`www` → `kathris612.github.io`。
2. 在 GitHub 仓库的 **Settings → Pages → Custom domain** 填入域名并保存。
3. 开启 **Enforce HTTPS**。
4. 如需让分享图和 canonical 使用新域名，同步修改 `index.html`、`robots.txt`、`sitemap.xml` 中的地址。

## 技术说明

- 无构建步骤、无第三方 CDN、无外部字体请求；断网也能完整打开。
- 音乐通过 Web Audio API 实时合成，首次点击「音乐」后才创建音频上下文，符合浏览器自动播放限制。
- 所有动画使用 CSS / Canvas，并为 `prefers-reduced-motion: reduce` 提供静态版本。
- 没有 Cookie、统计脚本或表单，不收集访问者数据。
