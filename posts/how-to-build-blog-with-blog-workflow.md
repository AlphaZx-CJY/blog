---
title: 我是怎么用 blog-workflow 搭这个博客的
date: 2026-04-06
tags: ["教程", "博客", "GitHub Pages"]
category: 技术
summary: 记录一下搭建这个博客的过程，用到的工具和踩过的坑。
---

折腾个人博客这件事，我试过 WordPress、Hexo、Hugo，最后发现还是这种「写 Markdown 就自动部署」的方式最省事。这篇记录一下我是怎么用 [blog-workflow](https://github.com/AlphaZx-CJY/blog-workflow) 把这个博客搭起来的。

## blog-workflow 是什么

简单来说，它是一个 GitHub Workflow。你把 Markdown 文件 push 到仓库，它自动帮你生成静态网站并部署到 GitHub Pages。不需要本地装 Node、不用手动构建，一切交给 GitHub Actions。

功能方面，我比较在意的几点它都有：Markdown 写作、代码高亮、标签分类、深色模式、移动端适配，还有全文搜索。评论系统用的 Giscus，基于 GitHub Discussions，不用额外注册。

## 具体怎么操作

**第一步：建仓库**

目录结构大概长这样：

```
my-blog/
├── .github/workflows/deploy.yml   # 部署配置
├── posts/                         # 放文章的地方
│   └── hello-world.md
├── images/                        # 图片
└── blog.config.yml                # 博客配置
```

文章我习惯放 `posts/` 目录，和配置分开比较清爽。

**第二步：写 deploy.yml**

在 `.github/workflows/deploy.yml` 里贴这段：

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
      posts-path: 'posts'
    secrets:
      GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

注意这里用的是 `GH_TOKEN`，不是 `GITHUB_TOKEN`。一开始我写的 `GITHUB_TOKEN`，结果报 reserved name 错误，查了好久才发现这个问题。

**第三步：写配置**

`blog.config.yml` 里填博客基本信息：

```yaml
title: "丛继晔的博客"

about:
  description: "软件工程师 · AI 工具开发者"

friends:
  - name: "GitHub"
    url: "https://github.com/AlphaZx-CJY"
  - name: "CSDN"
    url: "https://blog.csdn.net/weixin_40448140"
  - name: "个人主页"
    url: "https://www.congjiye.com/"

social:
  github: "https://github.com/AlphaZx-CJY"
  email: "congjiye@outlook.com"
```

**第四步：开 Pages**

进仓库 Settings → Pages，Source 选 GitHub Actions。这一步很容易忘，我就因为没开这个排查了半天。

**第五步：推送**

```bash
git add .
git commit -m "init blog"
git push origin main
```

push 完等一两分钟，GitHub Actions 跑完就能访问了。地址一般是 `https://yourusername.github.io/blog/`。

## 文章怎么写

每篇文章开头要加一段 YAML 前言：

```markdown
---
title: 文章标题
date: 2026-04-06
tags: ["技术", "随笔"]
category: 技术
summary: 一段话概括内容
---

正文开始...
```

只有 `title` 和 `date` 是必填的，其他可有可无。另外有个 `published` 字段，默认是 true，设为 false 就不会发布这篇文章。

## 本地预览

如果想本地看效果，可以这么搞：

```bash
git clone https://github.com/AlphaZx-CJY/blog-generator.git
cd blog-generator
npm install
cp /path/to/your/posts/*.md content/posts/
npm run dev
```

然后开 `http://localhost:3000` 看。不过我个人觉得没必要，直接 push 让 GitHub 构建也挺快的，出问题再改就是了。

## 遇到过的问题

**部署失败**
- 检查 Pages 设置里的 Source 是不是 GitHub Actions
- 检查 workflow 有没有读写权限
- 看 Actions 日志，一般会有具体报错

**文章没显示**
- 确认 YAML 前言格式正确，特别是 `---` 包裹
- 确认 date 格式是 `2026-04-06` 这种
- 检查有没有手误写成 `published: false`

**concurrency 死锁**
- 父 workflow 和 blog-workflow 都写了 `concurrency: group: "pages"` 会冲突
- 把父 workflow 里的 concurrency 删掉，只保留子 workflow 的

总的来说，这套方案适合不想折腾服务器、只想安静写东西的人。配置一次之后，后续就是写 Markdown → push，剩下的都自动搞定。
