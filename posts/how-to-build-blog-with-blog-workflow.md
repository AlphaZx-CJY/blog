---
title: 如何使用 blog-workflow 搭建个人博客
date: 2026-04-06
tags: ["教程", "博客", "GitHub Pages"]
category: 技术
summary: 详细介绍如何使用 blog-generator 和 blog-workflow 快速搭建并部署个人博客到 GitHub Pages。
---

本文将手把手教你如何使用 [blog-generator](https://github.com/AlphaZx-CJY/blog-generator) 和 [blog-workflow](https://github.com/AlphaZx-CJY/blog-workflow) 搭建一个功能完善的个人博客，并自动部署到 GitHub Pages。

## 什么是 blog-workflow？

**blog-workflow** 是一个可复用的 GitHub Workflow，它让你只需要写 Markdown，就能自动构建并部署一个漂亮的博客站点。

基于 **blog-generator** 构建，支持：

- ✅ Markdown + YAML 前言写作
- ✅ 代码高亮、标签、分类、归档
- ✅ 目录导航、阅读进度条
- ✅ 深色/浅色主题切换
- ✅ 本地搜索功能
- ✅ Giscus 评论系统
- ✅ 移动端适配

## 快速开始

### 1. 创建博客仓库

新建一个 GitHub 仓库，目录结构如下：

```
my-blog/
├── .github/
│   └── workflows/
│       └── deploy.yml      # 部署配置
├── posts/                  # 文章目录
│   └── hello-world.md
├── images/                 # 图片资源
└── blog.config.yml         # 博客配置
```

### 2. 配置部署 Workflow

创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy Blog

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    uses: AlphaZx-CJY/blog-workflow/.github/workflows/publish-blog.yml@main
    with:
      posts-path: 'posts'    # 文章存放目录
    secrets:
      GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

> ⚠️ **注意**：使用 `GH_TOKEN` 而不是 `GITHUB_TOKEN`，后者是系统保留名称会导致错误。

### 3. 配置博客信息

创建 `blog.config.yml`：

```yaml
# 博客标题
title: "丛继晔的博客"

# 关于我
about:
  description: "软件工程师 · AI 工具开发者"

# 友链
friends:
  - name: "GitHub"
    url: "https://github.com/AlphaZx-CJY"
  - name: "CSDN"
    url: "https://blog.csdn.net/weixin_40448140"
  - name: "个人主页"
    url: "https://www.congjiye.com/"

# 社交链接
social:
  github: "https://github.com/AlphaZx-CJY"
  email: "congjiye@outlook.com"
```

### 4. 开启 GitHub Pages

进入仓库 **Settings → Pages**：
- Source 选择 **GitHub Actions**

### 5. 提交并推送

```bash
git add .
git commit -m "Initial blog setup"
git push origin main
```

稍等片刻，博客就会自动部署到 `https://yourusername.github.io/blog/`

## 写作指南

### Markdown 文章格式

```markdown
---
title: 文章标题
date: 2026-04-06
tags: ["技术", "教程"]
category: 技术
summary: 文章摘要
---

正文内容，支持 **Markdown** 语法。

```javascript
console.log('Hello World');
```
```

### 支持的 YAML 字段

| 字段 | 说明 | 必填 |
|------|------|------|
| `title` | 文章标题 | ✅ |
| `date` | 发布日期 | ✅ |
| `tags` | 标签数组 | ❌ |
| `category` | 分类 | ❌ |
| `summary` | 摘要 | ❌ |
| `published` | 是否发布（默认 true）| ❌ |

## 本地预览（可选）

如果你想在本地预览博客效果：

```bash
# 克隆 blog-generator
git clone https://github.com/AlphaZx-CJY/blog-generator.git
cd blog-generator

# 安装依赖
npm install

# 复制你的文章
cp /path/to/your/posts/*.md content/posts/

# 开发模式
npm run dev
```

访问 http://localhost:3000 查看效果。

## 常见问题

### 部署失败

1. 检查 GitHub Pages 是否已开启（Settings → Pages → Source: GitHub Actions）
2. 检查 workflow 权限设置
3. 查看 Actions 日志获取详细错误

### 文章未显示

1. 确认 Markdown 文件有正确的 YAML 前言
2. 确认 `date` 格式正确（如 `2026-04-06`）
3. 检查是否设置了 `published: false`

## 总结

使用 blog-workflow 搭建博客的优势：

- **零配置**：几行 YAML 即可完成部署配置
- **自动化**：Push 即部署，无需手动操作
- **纯 Markdown**：专注写作，无需关心样式
- **版本控制**：文章使用 Git 管理，可追溯历史

现在就开始你的博客之旅吧！
