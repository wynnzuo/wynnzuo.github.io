# Wynn's Blog 🚀

基于 [Hugo](https://gohugo.io/) + [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题的个人博客，评论系统使用 [Giscus](https://giscus.app/)。

已发布 23 篇 GoF 设计模式系列文章（含 Java 代码 + ASCII 结构图），以及行业投资分析文章，可通过标签 [设计模式](https://wynnzuo.github.io/tags/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/) 和 [投资](https://wynnzuo.github.io/tags/%E6%8A%95%E8%B5%84/) 查看。

## 目录结构

```
wynnzuo.github.io/
├── source/                     # Hugo 源码
│   ├── content/                # Markdown 文章
│   │   ├── posts/
│   │   │   ├── design-patterns/ # 23 篇设计模式
│   │   │   └── investment/      # 投资分析
│   │   ├── about/              # 关于页面
│   │   ├── archives/           # 归档
│   │   └── search/             # 搜索
│   ├── themes/PaperMod/        # PaperMod 主题（git submodule）
│   ├── layouts/partials/       # 自定义布局（Giscus 评论）
│   └── hugo.toml               # Hugo 配置文件
├── main/                       # 生成的静态文件（CI 自动构建）
├── .github/workflows/          # GitHub Actions 自动部署
└── README.md
```

## 本地开发

```bash
# 克隆仓库（含 submodule 主题）
git clone --recursive https://github.com/wynnzuo/wynnzuo.github.io.git
cd wynnzuo.github.io/source
hugo server -D
```

访问 `http://localhost:1313` 即可预览。

## 构建

```bash
cd source
hugo --minify
```

静态文件输出到 `main/` 目录。

## 创建新文章

```bash
cd source
hugo new content posts/my-new-post.md
```

编辑文章，将 `draft: true` 改为 `draft: false` 后推送即可发布。

## 部署

推送 `main` 分支 → GitHub Actions 自动构建 → 部署到 GitHub Pages。

如需手动触发：仓库 Actions 页面 → **Deploy Hugo Site** → **Run workflow**。

## 联系方式

- GitHub: [wynnzuo](https://github.com/wynnzuo)
- Email: wynnzuo@163.com
