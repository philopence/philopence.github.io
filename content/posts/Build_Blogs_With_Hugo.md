+++
title = "使用hugo搭建博客"
date = "2021-12-27T19:27:20+08:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["", ""]
keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
+++

## Quick Start

```shell
sudo pacman -Sy hugo # archlinux

hugo new site DIRNAME

cd DIRNAME

git init
git submodule add -f https://github.com/panr/hugo-theme-terminal.git themes/terminal
# 编辑config.toml文件，添加主题推荐的配置

hugo new posts/my-first-post.md # 创建一篇博客，在/content/posts/目录中

hugo server # 开启服务预览博客，访问：localhost:1313
```

## 自动部署

1. 创建`.github/workflows/gh-pages.yml`文件，添加如下内容：

```yaml
name: github pages

on:
  push:
    branches:
      - master  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

2. 修改Github pages分支为`gh-pages`，默认推送的分支名

3. 将`config.toml`的`baseURL`选项的值改为`https://<USERNAME>.github.io`

操作流程：创建博客 -> 提交更新 -> 推送到远程仓库

## 其他命令

```shell
## 新环境初始化
git clone git@github.com:USERNAME/USERNAME.github.io.git --recurse-submodules

## 更新子模块
git submodule update --remote
```
