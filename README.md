# 瞬影簿 · 轻量科普图卡

纯前端静态页面（HTML + CSS + 原生 JS，无框架、无后端），单文件 `index.html`，体积仅几十 KB，可零成本部署到 GitHub Pages / Vercel。

## 功能
- 小红书风格瀑布流卡片（CSS 多列实现，零 JS 布局计算）
- 点击卡片弹出大图预览，支持 × / 点击背景 / Esc 关闭
- 顶部搜索框，按标题或标签实时前端筛选（示例：牛奶、土豆、青菜、香蕉）
- 响应式：电脑 4 列 / 平板 3 列 / 手机 2 列
- 图片全部外部 URL 引用，无 base64 内嵌，文件体积可控

---

## 一、本地预览
直接双击 `index.html` 用浏览器打开即可（演示图来自 picsum.photos，需联网）。

---

## 二、部署上线

### 方式 A：GitHub Pages
1. 在 GitHub 新建一个仓库（如 `shunyingbu`）。
2. 把 `index.html` 推送到仓库主分支：
   ```bash
   git init
   git add index.html
   git commit -m "init"
   git branch -M main
   git remote add origin https://github.com/<你的用户名>/<仓库名>.git
   git push -u origin main
   ```
3. 仓库 → **Settings → Pages → Source** 选择 `main` 分支、`/ (root)` 目录，保存。
4. 等待约 1 分钟，访问 `https://<你的用户名>.github.io/<仓库名>/` 即可公开访问。

### 方式 B：Vercel
1. 打开 [vercel.com](https://vercel.com)，用 GitHub 登录。
2. **Add New → Project**，导入上面那个仓库，Framework 选 **Other**，直接 Deploy。
3. 或更快：进入 [Vercel 控制台](https://vercel.com/new)，把含 `index.html` 的文件夹直接**拖拽**上传，秒级上线，自动分配 `*.vercel.app` 域名。
4. 可选：在 Project Settings → Domains 绑定自己的域名。

---

## 三、图片接入方式（重要）
页面**不内置任何图片**，所有图片通过外部图床 URL 引用，避免 HTML 体积膨胀。

1. 把图片上传到任意图床 / 对象存储，拿到**可直接在浏览器打开的 https 直链**，例如：
   - 对象存储：阿里云 OSS、腾讯云 COS、七牛云、AWS S3
   - 图床：SM.MS、Imgur、又拍云、Cloudflare R2
   - 也可以直接引用公开图片服务（如演示用的 `picsum.photos`）
2. 打开 `index.html`，修改 `DATA` 数组里每个对象的 `img` 字段为你自己的直链：
   ```js
   {title:"牛奶的营养价值", desc:"...", tag:"牛奶",
    img:"https://你的图床域名/牛奶.jpg"}   // ← 换成你的链接
   ```
3. 在 `DATA` 中可自由增删卡片，或调整 `title / desc / tag` 文案。

### 注意事项
- **防盗链**：图床需允许外链（Referer 白名单放通你的域名，或关闭防盗链），否则页面加载不出图。
- **不要**把图片转成 base64 写进 HTML——这会令单文件体积暴涨到数 MB，违背轻量化初衷。
- 图片建议提前压缩（宽度 400–800px、WebP 格式），保证移动端加载流畅。

---

## 四、二次开发建议
- 想换列数：改 `.masonry` 的 `column-count` 及对应媒体查询。
- 想换主题色：改 `:root` 里的 `--accent` 等变量。
- 卡片数据多时，可把 `DATA` 抽成独立 `data.json` 用 `fetch` 加载（需以 http 方式访问，本地 `file://` 直接打开会被浏览器 CORS 拦截）。
