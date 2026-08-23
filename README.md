# Zhaoyang Liu — Hugo academic website

这是一个完整、独立的 Hugo 学术个人主页项目。项目使用内置的 `academic-clean` 主题，不依赖远程主题仓库或 Git 子模块；全部页面内容均由 Markdown 管理。

## 1. 本地预览

先从 [Hugo 官方安装页](https://gohugo.io/installation/) 安装 Hugo（建议使用 **0.128.0 或更新版本**），然后在本项目根目录运行：

```bash
hugo server -D
```

浏览器访问终端中显示的地址，通常是 `http://localhost:1313/`。正式构建使用：

```bash
hugo --gc --minify
```

生成的网站位于 `public/`，无需手工编辑该目录。

## 2. 添加新论文

论文位于 `content/research/`，一篇论文对应一个 Markdown 文件。最省事的方法是复制 `archetypes/research.md`，或运行：

```bash
hugo new research/my-new-paper.md
```

然后编辑生成文件顶部的信息：

```yaml
---
title: "Paper title"
date: 2026-01-01
draft: false
authors:
  - "First Author"
  - "Zhaoyang Liu"
publication: "Journal Name"
publication_date: "1 January 2026"
doi: "https://doi.org/10.xxxx/xxxxx"
---
```

arXiv 预印本使用下面两个字段替代 `doi`：

```yaml
link: "https://arxiv.org/abs/xxxx.xxxxx"
link_label: "arXiv"
```

注意：

- `date` 必须写成 `YYYY-MM-DD`，网站用它自动按时间倒序排列。
- `publication_date` 是页面显示的日期，可使用自然语言。
- 所有作者都放在 `authors` 列表中。
- 通过 `hugo new` 创建的文件，应将 `draft` 设为 `false` 才会进入正式网站。
- Research 页面按要求不显示摘要，因此正文可以留空。

## 3. 修改个人信息

- 首页介绍、身份、导师、研究兴趣：`content/_index.md`
- 研究页面导语：`content/research/_index.md`
- 报告与会议：`content/activities/_index.md`
- 教学经历：`content/teaching/_index.md`
- 生活页面：`content/life/_index.md`
- CV 页面：`content/cv/_index.md`
- 联系方式：`content/contact/_index.md`
- 网站标题、导航、邮箱、照片与 MathJax 开关：`hugo.yaml`

### 添加个人照片

当前个人照片位于 `static/images/profile.jpg`。如需更换，将新照片命名为 `profile.jpg` 并覆盖该文件即可。

建议使用至少 800 × 800 px 的正方形或轻微竖幅照片。照片的裁切位置由主题 CSS 中的 `.portrait` 设置控制。

Life 页面的两张照片位于 `static/images/life-portrait.jpg` 和 `static/images/life-evening.jpg`，图片顺序及替代文字在 `content/life/_index.md` 的 `gallery` 列表中设置。

### 添加 PDF 简历

把文件放到 `static/files/cv.pdf`，然后按照 `content/cv/_index.md` 末尾的说明加入下载链接。

### 将来启用数学公式

打开 `hugo.yaml`，将 `params.mathjax.enabled` 改为 `true`。主题已经预留 MathJax 加载和行内公式配置，当前默认关闭。

## 4. 部署到 GitHub Pages

项目已包含 `.github/workflows/hugo.yaml`，推送到 `main` 分支后可自动构建和发布。
完整背景说明可参考 [Hugo 官方 GitHub Pages 指南](https://gohugo.io/host-and-deploy/host-on-github-pages/)。

1. 在 GitHub 创建一个新仓库并把本项目推送到该仓库。
2. 打开仓库的 **Settings → Pages**。
3. 将 **Source** 设为 **GitHub Actions**。
4. 再次推送到 `main`，或在 **Actions** 页面手动运行工作流。
5. 构建完成后，Actions 的 deploy 步骤会显示网站地址。

工作流会自动使用 GitHub Pages 提供的正确 `baseURL`，适用于用户主页仓库和项目仓库。

## 5. 部署到 Netlify

项目已包含 `netlify.toml`。
构建设置也遵循 [Netlify 官方 Hugo 指南](https://docs.netlify.com/build/frameworks/framework-setup-guides/hugo/)。

1. 将项目推送到 GitHub、GitLab 或 Bitbucket。
2. 在 Netlify 选择 **Add new site → Import an existing project**。
3. 选择仓库；Netlify 会读取配置并使用 `hugo --gc --minify` 构建，发布目录为 `public`。
4. 部署后，把 `hugo.yaml` 中的 `baseURL` 改成最终域名，例如 `https://your-name.netlify.app/`。

## 6. 目录速览

```text
.
├── content/                  # 所有页面与论文 Markdown
│   ├── research/             # 一篇论文一个文件
│   ├── life/
│   ├── cv/
│   └── contact/
├── static/
│   ├── images/               # 个人与生活照片
│   └── files/                # PDF CV 等下载文件
├── themes/academic-clean/    # 项目内置主题
├── archetypes/               # 新内容模板
├── .github/workflows/        # GitHub Pages 自动部署
├── hugo.yaml                 # 站点总配置
└── netlify.toml              # Netlify 构建配置
```

## 7. 上线前检查

- 将 `hugo.yaml` 中的 `baseURL` 从 `https://example.org/` 改为最终域名（GitHub Actions 构建时会自动覆盖）。
- 确认首页个人照片的裁切位置合适。
- 如需公开其他学术主页，在 Contact 页面添加 Google Scholar、ORCID 等链接。
- 运行 `hugo --gc --minify`，确认无报错后再推送。
