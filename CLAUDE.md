# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

个人技术博客，基于 **Hexo 3.9.0** 静态站点生成器，主题 `3-hexo`，语言 zh-CN，作者杨本泊。
源码仓库（本仓库）跟踪 `source/_posts/` 下的 Markdown 文章与配置；构建产物部署到**另一个独立仓库** `yangbenbo.github.io`（GitHub Pages，master 分支）。

## 常用命令

所有 hexo 命令通过 `npx` 调用（`node_modules/` 已安装，无需全局安装 hexo-cli）：

```bash
npx hexo clean          # 清空 public/、db.json、缓存（构建异常时先 clean）
npx hexo g              # generate，生成静态文件到 public/
npx hexo s              # server，本地预览 http://localhost:4000
npx hexo g && npx hexo d  # 生成并部署到 GitHub Pages
npx hexo new "标题"      # 用 scaffolds/post.md 新建文章
npx hexo new draft "标题" # 新建草稿到 source/_drafts/
npx hexo publish "标题"  # 草稿转正式文章
npx hexo new page "about" # 新建页面（如 about 页）
```

典型发布流程：`hexo clean` → `hexo g` → 本地 `hexo s` 校验 → `hexo d` 推送。

## 架构要点

### 两份 _config.yml
- **根 `_config.yml`**：站点信息、URL、永久链接（`:year/:month/:day/:title/`）、deploy 配置、search/feed 插件配置。
- **`themes/3-hexo/_config.yml`**：主题相关（头像、左下角菜单、友链、评论系统 gitalk、代码高亮主题、MathJax 开关、CDN 地址等）。改外观改这里，不要改主题的 layout/source 源码。
- **CDN 已本地化**：`CDN` 段的所有外部库（jquery、jquery.pjax、highlight.js、nprogress、animate.css、font-awesome、mathjax 等）已从 `cdn.bootcss.com` 改为本地路径 `/js/lib/...`、`/css/lib/...`，文件存放在 `themes/3-hexo/source/{js,css}/lib/`。原因：`cdn.bootcss.com` 在受限网络下被防火墙拦截，导致 jQuery 加载失败、`script.js` 抛 `$ is not defined`、分类点击等前端交互全部失效。**新增/更换前端库时**：把文件放到 `source/js/lib/` 或 `source/css/lib/`，再改 `CDN` 段指向本地路径，不要改回外部 CDN。`highlight.min.js` 是用 browserify 从 `highlight.js@9.12.0` 核心打包的浏览器版（含 28 种常用语言），不是官方预打包文件。

### 文章与资源目录（关键约定）
- `post_asset_folder: true`（根 _config.yml）：每篇文章 `Foo.md` 都有一个同名兄弟目录 `Foo/` 存放图片。
- 图片引用用**裸文件名**：`![eye-in-hand](eye-in-hand.png)`。`hexo-asset-image` 插件在构建时把裸文件名重写为正确的 URL。
- 不要用 `../` 或绝对路径引用文章图片；不要把图片放到统一的 `images/` 目录。
- 文章 front matter 字段：`title`、`date`、`categories`（列表）、`tags`（列表）、可选 `mathjax: true`。

### 数学公式（MathJax）
- 主题用 `hexo-renderer-kramed`（而非默认的 marked）渲染 Markdown，目的是不转义 `$...$` / `$$...$$` 的 LaTeX。
- 主题配置 `mathjax.per_page: false`：MathJax **仅**在 front matter 中带 `mathjax: true` 的文章里加载。含公式（矩阵、动力学、控制理论等）的文章必须加该字段，否则公式不渲染。

### 部署机制
- `hexo-deployer-git` 在 `hexo d` 时把 `public/` 复制到 `.deploy_git/`（自动生成的独立 git 工作树，已 gitignore），commit 后 push 到 `deploy.repo`（`git@github.com:yangbenbo/yangbenbo.github.io.git`，master 分支）。
- `deploy.repo` 走 SSH，需本机已配置好到 github.com 的 SSH key（详见文章 `Github-SSH-Key避免Hexo部署输入密码.md`）。
- `.deploy_git/` 是构建产物，**不要手动改**，也不要提交到本仓库。

### 搜索与 RSS
- `hexo-generator-searchdb` 生成 `search.xml` 供站内全文搜索（主题 `searchAll: true`）。
- `hexo-generator-feed` 生成 `atom.xml`（RSS，limit 20）。

## 编辑约定

- **文件命名**：文章文件名混用中文与英文横线命名（如 `机械臂动力学.md`、`c-函数指针.md`），新建文章时保持与同类文章一致的命名风格。
- **永久链接**含日期：`permalink: :year/:month/:day/:title/`，改文件名会改变 URL，对已发布文章慎重重命名。
- `db.json`（Hexo 缓存数据库，~2.8MB）和 `public/` 已 gitignore，不要提交。
- `venv/` 是遗留的 Python 虚拟环境，与 Hexo 构建无关。
