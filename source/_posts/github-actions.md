---
title: github actions进行github仓库和gitee仓库同步
abbrlink: a41696b
date: 2020-11-18 19:06:12
tags:
  - github
  - 部署
subtitle: hexo在github pages的部署
description: hexo使用actions自动化部署
keywords: hexo部署 github hexo部署 nodejs的GitHubactions方案
author: 墨墨
---

## 前言

博客是很多技术人的梦想，但是如何最低成本运营一个博客，搭建一个博客。百度一搜，千奇百样，`php`、`java`、`python`、`nodejs` 等等一大堆。但是却有一个很致命的问题。如何让博客一直运营和保持在线上。
但是如果部署在服务器上面的话，成本太大了。就算最低配置的服务器，每年也要至少一百多的成本，再加上域名，不算少了。
今天在这里给大家安利的是，怎么在 github 和 gitee 上部署博客。 - 在这里今天使用的是`hexo` 推送`github`，使用`actions`自动部署到`github pages`和`gitee pages`上。

<!--more-->

### 搭建一个 hexo 博客

> 搭建一个`hexo`博客，18 年的时候就已经提及到了[传送门](https://xuanbi.gitee.io/m-blog/archives/d92ac943.html#more),但是那时候因为忙，也忘记了更新博客。导致了后面`coding`更新之后没重新部署，才有了自动部署博客的想法。
> 后面看到了 github actions 的用法之后，就搜索了一堆资料集合成了这篇博客的使用了。

### GitHub Actions

> 在 GitHub Actions 的仓库中自动化、自定义和执行软件开发工作流程。 您可以发现、创建和共享操作以执行您喜欢的任何作业（包括 CI/CD），并将操作合并到完全自定义的工作流程中。[传送门](https://docs.github.com/cn/free-pro-team@latest/actions)
> 话不多少，直接上代码。

- 在根目录下新建文件`.github/workflows/deploy.yml`,对代码部分进行修改

```yml
name: Blog CI/CD

# 触发条件：在 push 到 main 分支后触发,然后推送同步到gitee
on:
  push:
    branches:
      - main
# 设置系统时间
env:
  TZ: Asia/Shanghai

# 执行自动构建工作流
jobs:
  blog-cicd:
    name: Hexo blog build & deploy # 名字可以随便起，但是不可与下面的重复
    runs-on: ubuntu-latest # 使用最新的 Ubuntu 系统作为编译部署的环境
    # 开始步骤
    steps:
      - name: Checkout codes
        uses: actions/checkout@v2
      - name: Setup node
        # 设置 node.js 环境
        uses: actions/setup-node@v1
        with:
          node-version: '12.x' # 如果要设置成多个的，此处可以是一个数组['10.x','12.x','13.x']

      - name: Cache node modules
        # 设置包缓存目录，避免每次下载.缓存
        uses: actions/cache@v1
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}

      - name: Install hexo dependencies
        # 下载 hexo-cli 脚手架及相关安装包
        run: |
          npm install hexo-cli gulp -g
          npm install

      - name: Generate files
        # 清除打包文件 重新编译 markdown 文件
        run: |
          hexo clean
          hexo generate

      - name: Deploy hexo blog
        env:
          # Github 仓库
          GITHUB_REPO: github.com/xcbt9527/m-blog
          # Coding 仓库
          # CODING_REPO: e.coding.net/yifanzheng/blogs.git
          # Gitee 仓库
          GITEE_REPO: gitee.com/xuanbi/m-blog
        # 将编译后的博客文件推送到指定仓库
        # 因为我这边的github和gitee的仓库名字都不一样，所以需要对git name 重新设置过
        run: |
          cd ./public && git init && git add .
          git config user.name "xcbt9527"
          git config user.email "764567192@qq.com"
          git add .
          git commit -m "GitHub Actions Auto Builder at $(date +'%Y-%m-%d %H:%M:%S')"
          git branch -M gh-pages
          git push --force --quiet "https://${{ secrets.ACCESS_TOKEN }}@$GITHUB_REPO" gh-pages:gh-pages
          git config user.name "玄笔"
          git push --force --quiet "https://xuanbi:${{ secrets.GITEE_ACCOUNT_TOKEN }}@$GITEE_REPO" gh-pages:gh-pages
```

> 在提交代码之前，你需要先去你的 github 的仓库创建`Secrets`。
>
> > 在仓库的`Settings`中找到`Secrets`,并点击`New repository secret`添加以下几个参数

```json
{
  "ACCESS_TOKEN": "github的个人令牌",
  "GITEE_PRIVATE_KEY": "gitee和github共同的ssh密钥。也可以是你本地的密钥，必须存放到github和gitee上"
}
```

- 然后你需要分别在`github`和`gitee`的仓库当中新建一个存放 pages 的分支。
- 接着就是把代码推送上去。然后在`actions`上你就可以看到自动部署了。

> end
