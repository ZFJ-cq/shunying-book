# 瞬影簿 · 轻量科普图卡

纯前端静态页面（HTML + CSS + 原生 JS，无框架、无后端），单文件 `index.html`，体积仅几十 KB，可零成本部署到 GitHub Pages / Vercel。

## 功能
- 小红书风格瀑布流卡片（CSS 多列实现，零 JS 布局计算）
- 点击卡片弹出大图预览，支持 × / 点击背景 / Esc 关闭
- 顶部搜索框，按标题或标签实时前端筛选（示例：牛奶、土豆、青菜、香蕉）
- 响应式：电脑 4 列 / 平板 3 列 / 手机 2 列
- 图片放在 `images/` 目录，HTML 中使用相对地址引用，无 base64 内嵌，部署后完全自包含

---

## 一、本地预览
直接双击 `index.html` 用浏览器打开即可（图片随仓库走，无需联网）。

---

## 二、部署上线

### 方式 A：GitHub Pages（推荐）
1. 在 GitHub 新建一个仓库（如 `shunyingbu`）。
2. 把 `index.html` 和 `images/`/` 整个目录推送到仓库主分支：
   ```bash
   git init
   git add index.html images
   git commit -m "init"
   git branch -M main
   git remote add origin https://github.com/<你的用户名>/<仓库名>.git
   git push -u origin main
   ```
3. 仓库 → **Settings → Pages → Source** 选择 `main` 分支、`/ (root)` 目录，保存。
4. 等待约 1 分钟，访问 `https://<你的用户名>.github.io/<仓库名>/` 即可公开访问。
   图片的 `images/xxx.jpg` 相对路径会自动解析为 `https://<你的用户名>.github.io/<仓库名>/images/xxx.jpg`。

### 方式 B：Vercel
1. 打开 [vercel.com](https://vercel.com)，用 GitHub 登录。
2. **Add New → Project**，导入上面那个仓库，Framework 选 **Other**，直接 Deploy。
3. 或更快：进入 [Vercel 控制台](https://vercel.com/new)，把含 `index.html` 和 `images/`/` 的文件夹直接**拖拽**上传，秒级上线，自动分配 `*.vercel.app` 域名。
4. 可选：在 Project Settings → Domains 绑定自己的域名。

---

## 三、图片接入方式（本地化）
页面所有图片放在项目根目录的 `images/`/` 文件夹中，`index.html` 用相对地址引用，部署后完全自包含。

```
瞬影簿/
├── index.html
├── images/              ← 图片统一放这里
│   ├── hotpot-life-cycle.jpg
│   ├── egg-life-cycle.jpg
│   └── ...（其他 60+ 张）
└── README.md
```

### 新增一张卡片（4 步）
1. 把图片文件保存到 `images/`/`，文件名只用英文/数字/连字符，例如 `hotpot-life-cycle.jpg`。
2. 打开 `index.html`，找到 `const DATA = [ ... ]` 数组。
3. 在数组里追加一条记录：
   ```js
   {title:"标题文字", desc:"补充说明", tag:"搜索标签", img:"images/你的文件名.jpg"}
   ```
4. 保存后刷新浏览器即可看到新卡片。

### 文件名建议用语义化英文短名
- ✅ `hotpot-life-cycle.jpg` `apple-12-types.jpg` `chicken-savings-map.jpg`
- ❌ `微信图片_20260903.jpg`（不兼容部分服务器）`IMG_1234.jpg`（不便识别）

### 注意事项
- **不要**把图片转 base64 写进 HTML，会令单文件体积暴涨。
- 图片建议提前压缩（宽度 400–800px、WebP 格式），保证移动端加载流畅。
- `images/` 当前 61 张图共约 34MB，GitHub 单仓库建议 <1GB，留足余量。

---

## 四、二次开发建议
- 想换列数：改 `.masonry` 的 `column-count` 及对应媒体查询。
- 想换主题色：改 `:root` 里的 `--accent` 等变量。
- 卡片数据多时，可把 `DATA` 抽成独立 `data.json` 用 `fetch` 加载（需以 http 方式访问，本地 `file://` 直接打开会被浏览器 CORS 拦截）。
