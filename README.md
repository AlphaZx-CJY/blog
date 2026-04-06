# 我的个人博客

这是我的个人博客仓库，使用 Markdown 写作，通过 GitHub Actions 自动部署到 GitHub Pages。

## 目录结构

```
my-blog/
├── .github/
│   └── workflows/
│       └── deploy.yml      # 部署配置
├── posts/                  # 博客文章目录
│   └── hello-world.md      # 示例文章
├── images/                 # 图片资源目录
├── blog.config.yml         # 博客配置文件
└── README.md               # 本文件
```

## 写作规范

### 文章格式

每篇文章需要包含 YAML 前言（Front Matter）：

```markdown
---
title: 文章标题
date: 2026-04-06
tags: ["标签1", "标签2"]
category: 分类
summary: 文章摘要（可选）
---

正文内容...
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

### 图片引用

将图片放在 `images/` 目录下，在 Markdown 中引用：

```markdown
![图片描述](/images/your-image.png)
```

## 发布流程

1. 编写 Markdown 文章并保存到 `posts/` 目录
2. 提交更改：`git add . && git commit -m "Add new post"`
3. 推送到 GitHub：`git push origin main`
4. GitHub Actions 会自动构建并部署

## 本地预览（可选）

如需本地预览，请参考 [blog-workflow 文档](https://github.com/AlphaZx-CJY/blog-workflow#本地预览)。

## 相关链接

- [blog-workflow](https://github.com/AlphaZx-CJY/blog-workflow) - 博客构建工具
- [blog-generator](https://github.com/AlphaZx-CJY/blog-generator) - 静态博客生成器
