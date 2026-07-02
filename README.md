# Wynn's Blog 🚀

基于 [Hugo](https://gohugo.io/) + [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题的个人博客，评论系统使用 [Giscus](https://giscus.app/)。

## 目录结构

```
wynnzuo.github.io/
├── source/           # Hugo 源码（博客内容、主题配置）
│   ├── content/      # Markdown 文章
│   ├── themes/       # PaperMod 主题（git submodule）
│   ├── layouts/      # 自定义布局（Giscus 评论）
│   └── hugo.toml     # Hugo 配置文件
├── main/             # 生成的静态文件（CI 自动构建，不手动修改）
├── .github/          # GitHub Actions 工作流
└── README.md         # 本文件
```

## 快速开始

### 前置条件

- [Hugo (extended)](https://gohugo.io/installation/) v0.163.3+

### 本地开发

```bash
# 克隆仓库（包含 submodule）
git clone --recursive https://github.com/wynnzuo/wynnzuo.github.io.git
cd wynnzuo.github.io

# 进入 source 目录启动开发服务器
cd source
hugo server -D
```

访问 `http://localhost:1313` 即可预览。

### 构建

```bash
cd source
hugo --minify
```

静态文件会生成到 `main/` 目录。

## Giscus 评论配置

1. 在 [Giscus 官网](https://giscus.app/) 配置你的仓库

2. 在 `source/hugo.toml` 中替换以下参数：

   - `giscus.repoId` — 仓库的 `repo-id`
   - `giscus.categoryId` — 讨论分类的 `category-id`

3. 确保你的 GitHub 仓库已启用 Discussions 功能

## 部署

推送代码到 `main` 分支后，GitHub Actions 会自动构建并部署到 GitHub Pages。

### 手动触发

在 GitHub 仓库的 Actions 页面，选择 **Deploy Hugo Site** 工作流，点击 **Run workflow**。

## 创建新文章

```bash
cd source
hugo new content posts/my-new-post.md
```

然后编辑生成的 Markdown 文件，将 `draft: true` 改为 `draft: false` 即可发布。
