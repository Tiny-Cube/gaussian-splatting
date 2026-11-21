# 3D Gaussian Splatting 作品集

在 GitHub Pages 上展示用 **3D Gaussian Splatting（3DGS）** 生成的 3D 模型。
点击下面的示例即可在浏览器中实时查看、旋转、缩放模型（无需安装任何软件，手机也能看）。

> 在线入口：`https://<你的用户名>.github.io/gaussian-splatting/`
> （对仓库 `Tiny-Cube/gaussian-splatting`，就是 `https://tiny-cube.github.io/gaussian-splatting/`）

## ✨ 支持的格式（.ply / .sog / .spz）

本仓库的查看器由开源的 [SuperSplat Viewer](https://github.com/playcanvas/supersplat-viewer) 驱动，
原生支持你手上的三种格式：

| 格式 | 说明 | 展示建议 |
| ---- | ---- | -------- |
| **.ply** | 3DGS 标配输出（含 SH 球谐系数，画质最高） | 体积大，一般只在本地/训练查看 |
| **.sog** ✅ | 单文件打包 + 压缩，体积小 | 👑 **你现在用的就是它，足够小，直接放仓库即可** |
| **.spz** ✅ | 官方高压缩展示格式（体积最小、加载最快，开源标准 KHR_gaussian_splatting） | 也很适合展示，可按需转换 |

### ✅ 结论：用 .sog 展示就行

你的 `.sog` 文件已经压缩过、体积小，可直接提交进仓库并由查看器加载，**不需要再做格式转换**。
如果某个模型想进一步减小体积（比如 .ply 很大），可以再转 `.spz`：

> 转换工具（免费开源）：
> - PlayCanvas 出品的一体化工具 [SuperSplat Editor](https://superspl.at/editor) —— 直接拖入 `.ply` / `.sog`，
>   顶部菜单 **File → Save / Export** 即可导出 `.spz`。
> - 命令行批量转换 [splat-transform](https://github.com/playcanvas/splat-transform)：`splat-transform model.ply model.spz`。

## 📁 目录结构

```
gaussian-splatting/
├── index.html              # 作品集首页（模型卡片列表，点卡片进查看器）
├── model-viewer/           # SuperSplat Viewer 静态文件（已替你放好，勿改内部文件）
│   ├── index.html
│   ├── index.js
│   ├── index.css
│   ├── settings.json        # 查看器观感设置（背景色 / 初始相机等，可自调）
│   ├── gsaf-decoder.wasm
│   └── zstd-decoder.wasm
└── assets/models/          # ← 把你的模型文件都放这里
    └── _list.json          # ← 在这里登记要展示的模型
```

## 🚀 使用方法（三步）

1. **把你的 `.sog` 模型文件放进 `assets/models/`**（`.ply` / `.spz` 也支持，直接放即可）。

2. **在 `assets/models/_list.json` 里登记模型**（支持多个）：
   ```json
   {
     "models": [
       { "file": "my-scene.sog", "name": "我的 3D 场景", "note": "一句话描述（可选）" },
       { "file": "another.sog",  "name": "另一个模型",  "note": "" }
     ]
   }
   ```

3. **推送到 GitHub 并开启 GitHub Pages**：
   ```bash
   git add .
   git commit -m "add 3D Gaussian splatting showcase"
   git push origin main
   ```
   然后在仓库 **Settings → Pages → Source / Build and deployment** 里选择
   **Deploy from a branch → 分支 `main` → 目录 `/ (root)`**，保存。
   等一两分钟，访问 `https://tiny-cube.github.io/gaussian-splatting/` 即可看到效果。

### 部署分支建议

- 想全部放 `main` 根目录：把上面这些文件直接放在仓库根目录即可。
- 想独立成页面、不动代码分支：把本目录内容放到 `docs/` 子目录，然后 Pages 源选**分支 `main` → 目录 `/docs`**。

## 🧠 加一个查看指定文件直达链接

查看器支持 URL 参数，可用来单独分享某个模型：
```
./model-viewer/index.html?content=../assets/models/my-scene.sog&settings=./settings.json
```
`content` 指向模型文件（相对于查看器页面，所以是 `../assets/models/...`），`settings` 指向观感设置。
直接把该地址发给别人即可。

## ⚠️ 常见问题 / 文件过大怎么办

- **单文件超过 100MB**：GitHub 仓库对普通 Git 单文件有 100MB 硬上限（>50MB 会告警）。
  方案：① 优先转成 `.spz` 大幅减小体积；② 用 Git LFS 跟踪模型（Pages 对 LFS 支持有限，需配
  `.nojekyll` 与较新的部署配置，优先推荐方案③）；③ 把模型上传到 Hugging Face / 对象存储等，
  然后用上面的 `?content=<外链地址>` 直接加载，仓库只留查看器和登记列表。

- **停用 Jekyll 干扰**：在仓库根放一个空的 `.nojekyll` 文件可避免 Jekyll 吞掉带下划线的目录
  （`assets/models/_list.json` 里的 `_` 开头文件）。本结构默认建议加上。

## 📜 致谢

- 查看器：[SuperSplat Viewer](https://github.com/playcanvas/supersplat-viewer)（PlayCanvas 开源项目）
- 3DGS 原始算法：[3D Gaussian Splatting for Real-Time Radiance Field Rendering](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/)