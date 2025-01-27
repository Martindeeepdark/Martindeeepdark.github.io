# Hugo 使用指南
本指南记录了 Hugo 的基本使用方法和常用命令，方便快速查阅。

1. 安装 Hugo
1.1 安装 Hugo 扩展版
Hugo 扩展版支持 Sass/SCSS 等高级功能。推荐使用扩展版。

Linux/macOS：
```bash
wget https://github.com/gohugoio/hugo/releases/download/v0.125.7/hugo_extended_0.125.7_linux-amd64.tar.gz
tar -xzf hugo_extended_*.tar.gz
sudo mv hugo /usr/local/bin/
```

验证安装：
```bash
复制
hugo version
# 应输出：hugo v0.125.7+extended...
```


2. 创建新文章
2.1 使用模板创建文章
Hugo 会根据 archetypes 目录下的模板文件生成新文章。

```bash
hugo new post/my-first-post.md
```

默认元数据：
新创建的文章会包含以下默认元数据：

```markdown
---
title: "My First Post"
date: 2023-10-01T15:00:00+08:00
draft: true
---
```
草稿状态：
默认情况下，新文章处于草稿状态（draft: true）。构建网站时不会包含草稿文章。

3. 本地开发与预览
3.1 启动本地开发服务器
使用以下命令启动本地服务器，实时预览网站效果：

bash
复制
hugo server -D -w
参数说明：

-D：包含草稿文章。

-w：监听文件变化，自动重新构建。

访问地址：
打开浏览器，访问 http://localhost:1313。

4. 构建静态网站
4.1 构建网站
完成文章编写和网站配置后，使用以下命令构建静态网站：

bash
复制
hugo
输出目录：
构建结果会生成到 public/ 目录。

5. 部署到 GitHub Pages
5.1 使用 GitHub Actions 自动部署
创建 GitHub Actions 配置文件：
在 .github/workflows/ 目录下创建 hugo.yml，内容如下：

yaml
复制
name: Build and Deploy Hugo Site

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          submodules: true

      - name: Set up Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.125.7'
          extended: true

      - name: Build site
        run: hugo --minify

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
手动创建 gh-pages 分支（如果不存在）：

bash
复制
git checkout --orphan gh-pages
git rm -rf .
git commit --allow-empty -m "Initial commit for gh-pages branch"
git push origin gh-pages
推送代码以触发部署：

bash
复制
git add .
git commit -m "Trigger GitHub Actions"
git push origin main
6. 常用命令速查
命令	说明
hugo new post/my-post.md	创建新文章
hugo server -D -w	启动本地服务器（包含草稿）
hugo	构建静态网站
hugo --minify	构建并压缩静态网站
hugo mod clean	清理 Hugo 模块缓存
hugo mod get -u	更新 Hugo 模块
7. 注意事项
草稿文章：

默认情况下，草稿文章不会在构建时发布。

使用 hugo server -D 或 hugo -D 包含草稿。

主题更新：

如果使用 Git 子模块安装主题，确保子模块已初始化：

bash
复制
git submodule update --init --recursive
GitHub Pages 部署：

确保 .gitignore 中没有 public/，否则构建结果无法推送。

8. 参考链接
Hugo 官方文档

GitHub Actions for Hugo

GitHub Pages Action