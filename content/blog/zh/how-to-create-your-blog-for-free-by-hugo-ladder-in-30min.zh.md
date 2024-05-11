---
title: 如何 30 分钟搭建一套完整独立博客
date: 2022-08-14T16:47:12+08:00
tags: ["hugo", "ladder主题", "教程", "GitHub Pages"]
series: ["how to create your blog"]
featured: true
---

本文是关于如何搭建免费的独立博客系列的第二篇，包括如何使用本博客主题 `hugo-ladder`，如何集成评论和统计功能等。如果后续打算使用该主题来搭建和运营博客，建议跟随本篇文章来搭建。

<!--more-->

## Hugo

### 技术选型

今天的大多数网站分为两类--动态网站和静态网站。动态网站是基于服务器的，它的内容是变化的，可以根据用户返回不同的内容。例如根据不同用户的喜好进行推荐，根据地区进行内容语言区分。页面依靠内容管理系统（CMS）或数据库进行渲染，并可以随着需求的增长而不断增加复杂性。而缺点的话也很明显，首先你需要一笔不菲的服务器费用来支持带宽和并发，如果服务是面向全球的，还需要考虑多地部署来提升访问速度。

还有一种就是我们选择的静态网站，使用 static site generators (SSG) 技术，会向所有用户显示相同的内容。使用服务器端的渲染来提供HTML、CSS和Javascript文件。静态网站的优势包括速度、安全和SEO。还易于维护，并具有高度可扩展性。由于静态网站生成器（SSG）在CDN上存储一个已经编译好的页面，它们的加载速度要快得多。

我们可以选择免费托管到  CDN 或者平台 [Netlify](https://netlify.com/)、[Heroku](https://www.heroku.com/)、[GoDaddy](https://www.godaddy.com/)、[DreamHost](https://www.dreamhost.com/)、[GitHub Pages](https://pages.github.com/)、[GitLab Pages](https://about.gitlab.com/features/pages/)、[Surge](https://surge.sh/)、[Firebase](https://firebase.google.com/docs/hosting/)、[Google Cloud Storage](https://cloud.google.com/storage/)、[Amazon S3](https://aws.amazon.com/s3/)、[Rackspace](https://www.rackspace.com/cloud/files)、[Azure](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website)和 [CloudFront](https://aws.amazon.com/cloudfront/) 上。

对于开发者来讲，托管到 [GitHub Pages](https://pages.github.com/) 是最熟悉的方案，我们本篇文章也是基于此方案展开的教程。不过 [GitHub Pages](https://pages.github.com/) 对于部分地区的访问速度来讲并不友好，可以考虑使用其它自带 CDN 的免费平台来部署，例如本站使用的 [Cloudflare Pages](https://pages.cloudflare.com/)。

在 static site generators (SSG) 方案中，有几种常见的方案，[Hexo](https://hexo.io/), [Jekyll](https://jekyllrb.com/),  [Hugo](https://gohugo.io/) 和 [Next.js](https://nextjs.org/)。

其中 [Hexo](https://hexo.io/) 的生态是最丰富和完善的，在 GitHub 中能找到最多的主题适配、最完善的第三方支持。但是考虑到我后续会自己开发定制化的主题和功能，去翻阅了下 [Hexo](https://hexo.io/) 的官方文档，结果 [Hexo](https://hexo.io/) 的文档不能说是事无巨细吧，只能说是等于没有。[Next.js](https://nextjs.org/) 是非常符合未来的一门技术，也是我比较心动的方案，但是后续发现  [Next.js](https://nextjs.org/) 在开发博客的生态上目前还不够完善。

所以在对比了几项技术的优缺点后，我最终采用了Hugo。[Hugo](https://gohugo.io/) 是基于 `golang` 编写的快速、现代的静态网站生成器。在 benchmark 测试中可以一秒渲染 5000 篇文章，也具备完善的生态。

### 安装

安装 `Hugo` 可以按照官方的文档一步一步安装 [hugo install](https://gohugo.io/getting-started/installing)。也可以按照下面命令快速安装。

{{< tabgroup >}}

{{< tab name="MacOS / Linux" >}}

```shell
brew install hugo
```


{{< /tab >}}

{{< tab name="Window" >}}

```shell
choco install hugo -confirm
```

{{< /tab >}}

{{< /tabgroup >}}

可以通过 `hugo version` 来确保安装成功。

## Ladder

本博客的主题名为 `ladder`，中文即阶梯的含义，读书为输入，写作为输出，都是获取知识的阶梯。

Ladder 主题的仓库地址是：[https://github.com/guangzhengli/hugo-theme-ladder](https://github.com/guangzhengli/hugo-theme-ladder)

为大家准备好的一个已经配置好的仓库地址是： https://github.com/guangzhengli/hugo-ladder-exampleSite

如果你打算熟悉  `hugo` 命令或者后续持续开发的话  ，可以从 [#配置初始化](#配置初始化)一步一步来。

如果你之前没有使用过 `hugo` 或者是打算迁移博客的话，可以直接输入克隆已经初始化好的，在浏览器打开 `http://localhost:1313/`。**并且可以直接跳转到 [#自定义配置](#自定义配置)。无需做初始化操作**。

```shell
git clone https://github.com/guangzhengli/hugo-ladder-exampleSite.git
hugo server -D
```

### 配置初始化

Hugo 提供了一个 `new` 命令来创建一个新的网站:

```shell
hugo new site my_website
cd my_website
```

初始化你的项目目录为 git 仓库, 并且把主题仓库作为你的网站目录的子模块。

```shell
git init
git submodule add https://github.com/guangzhengli/hugo-theme-ladder themes/ladder
```

因为个人偏爱 `yml` 的阅读习惯，所以本文使用 `yml` 而非默认的 `toml` 文件类型来配置，大家可以根据偏好到 [transform.tool](https://transform.tools/yaml-to-toml) 进行转换。

下面是本主题完整的 `config.yml` 文件，大家可以删除根目录下默认的 `config.toml` 文件，新建一个 `config.yml` 文件代替。

{{< toggle summary="点击查看完整的 `config.yml` 文件" >}}

```yml
baseURL: 'https://hugo-ladder.pages.dev'
title: LADDER
theme: hugo-theme-ladder
license: MIT
licenselink: 'https://github.com/guangzhengli/hugo-theme-ladder/blob/master/LICENSE'
description: 'A fast, clean Hugo theme'
homepage: 'https://hugo-ladder.pages.dev'
defaultContentLanguage: 'en'
googleAnalytics: G-xxx
paginate: 10
params:
  brand: HOME
  avatarURL: /images/avatar.png
  author: Hugo Ladder
  authorDescription: A clean, fast hugo theme focused on Reading
  info: Build a free and beautiful blog site to record your thoughts and increase your influence
  favicon: /images/avatar.png
  options:
    showDarkMode: true
    enableImgZooming: true
    enableMultiLang: true
  darkModeTheme: data-dark-mode
  #darkModeTheme: icy-dark-mode
  comments:
    giscus:
      enable: true
      repo: username/repo
      repo_id: xxx
      category: Announcements
      category_id: xxx
      mapping: pathname
      position: top
      lang: en # pick a language from https://github.com/giscus/giscus/tree/main/locales
    utteranc:
      enable: false
      repo: username/xxx
      issueTerm: pathname
  analytics:
    google:
      SiteVerificationTag: xxx
    umami:
      enable: true
      website_id: xxx
      url: https://xxx
  guestbook:
    title: Guestbook
    description: Leave a comment below. It could be anything –- question, appreciation, information, or even humor.
  social:
    - name: GitHub
      pre: >-
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-github"><path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"></path></svg>
      url: 'https://github.com/username/xxx'
    - name: Dashboard
      pre: >-
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-box"><path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"></path><polyline points="3.27 6.96 12 12.01 20.73 6.96"></polyline><line x1="12" y1="22.08" x2="12" y2="12"></line></svg>
      url: 'https://xxx'
languages:
  en:
    languageName: EN
    menu:
      main:
        - name: Blog
          url: /blog
          weight: 1
        - name: Tags
          url: /tags
          weight: 2
        - name: Archive
          url: /archives
          weight: 3
        - name: Guestbook
          url: /guestbook
          weight: 4
        - name: Dashboard
          url: https://xxx
          weight: 5
  zh:
    languageName: 中
    author: Ladder 主题
    authorDescription: 一个美观，快速并且专注于阅读的主题
    info: 帮助开发者构建一个免费并且漂亮的博客网站，记录自己的思考并且提高自己的影响力
    guestbook:
      title: 留言板
      description: 您的评论，会让该网站更精彩！
    menu:
      main:
        - name: 文章
          url: /blog
          weight: 1
        - name: 分类
          url: /tags
          weight: 2
        - name: 历史文章
          url: /archives
          weight: 3
        - name: 留言板
          url: /guestbook
          weight: 4
        - name: 网站统计
          url: https://xxx
          weight: 5
taxonomies:
  series: series
  tag: tags
```

{{</toggle>}}

### 启动博客

首先我们找到根目录下 `archetypes` 文件夹中的 `default.md` 文件，用下面的文件替代其中内容。

```markdown
---
title: 
date: {{ .Date }}
tags: []
series: []
featured: true
---
Here is summary.

<!--more-->

here is your content.

```

复制 `themes/ladder/exampleSite/content` 里面的内容到自己的根目录的 `content` 下面。

输入命令创建一篇名为 `my-first-blog` 新的博客。

```shell
hugo new blog/my-first-blog.md
```

创建后可以使用 `hugo server -D` 来启动博客，并且在浏览器打开 `http://localhost:1313/`，即可看到你的博客网站。

```shell
hugo server -D
```

## 自定义配置

### 多语言支持

首先我们需要确定我们博客默认使用的语言是哪种？是否需要多语言的支持？例如我们默认语言使用英文，那么 `defaultContentLanguage` 即使用默认的 `en`，假如默认使用中文就是 `zh`。

除此之外，如果不需要多语言的支持，先将 `params.options.enableMultiLang` 设置为 `false`, 删除配置文件 `languages` 配置下其它语言的配置即可。

上面的文件展示的是本博客使用的配置，多语言配置中默认语言是英文，可以切换成中文。

### 修改基本信息

```yml
baseURL: 'https://hugo-ladder.pages.dev' #修改为你的 https://username.github.io
homepage: 'https://hugo-ladder.pages.dev' #修改为你的 https://username.github.io
params:
  brand: HOME # 修改默认的为自己网站的标志
  avatarURL: /images/avatar.png #网站主页的照片信息，你可以在根目录 /static/images/ 里面替换成自己的照片
  author: Hugo Ladder # 修改你自己的名字
  authorDescription: # 修改对你自己的描述
  info:  # 修改对你自己网站描述
  favicon: /images/avatar.png #网站的 icon，你可以在根目录 /static/images/ 里面替换成自己的照片
  options:
    showDarkMode: true # 是否支持黑暗模式
 languages: # 如果你启用多语言，下面是中文展示
  zh:
    languageName: 中
    author: Ladder 主题
    authorDescription: 一个美观，快速并且专注于阅读的主题
    info: 帮助开发者构建一个免费并且漂亮的博客网站，记录自己的思考并且提高自己的影响力
```

你可以一边修改一边通过浏览器打开 `http://localhost:1313/` 实时观看效果。

### 导航栏

目前本博客支持 4 种默认的页面，即博客列表，标签分类，历史文章和留言板，

可以通过修改 `menu.main` 来调整 `名称` ，如果要添加新的页面或者修改  `URL`  的话，记得在根目录的 `content` 下面添加对应的 `markdown` 文件。

### 黑暗模式

本主题目前提供两种黑暗模式，一种是默认的 `data-dark-mode` 模式，还有一种是 `icy-dark-mode` 模式，可以通过修改 `params.darkModeTheme` 的参数进行替换。大家可以自行探索，也欢迎贡献新的模式。

### 社交图标

可以通过修改配置文件添加新的图标， `params.social`  字段标明 `名称`，`图标`，`自定义地址`，图标可以在这个网站找到 [feathericons.com](https://feathericons.com/)。

## 内容管理

### 文章管理

每一次写文章建议填写的配置（也可以不填），文章开头有这个几个字段，分别代表着

```markdown
title: 
date: {{ .Date }}
tags: []
series: []
featured: true
```

* `title` 文章题目
* `date` 发布日期
* `tags` 标签分类
* `series` 系列文章，会在下方推荐阅读中推荐同系列文章
* `featured` 是否在主页面中展示，`true` or `false`

### 富文本功能

目前博客默认集成 `Toggle`和 `Tab group` 两种富文本， `Toggle` 的效果例如 [#配置初始化](#配置初始化) 中可以隐藏代码或者文本， `Tab group` 的效果例如在 [#安装Hugo](#安装) 中可以切换标签来写教程或文本。

除此之外，还有集成 `youtube`  `twitter ` 和 `站点页面跳转` 等富文本功能。详细可以查看 [rich content](https://hugo-ladder.pages.dev/blog/tag-plugins/)。

我还新建了一个仓库 https://github.com/guangzhengli/awesome-hugo-shortcodes ，用来收集 `hugo shortcodes` ，大家可以去看看有没有自己需要的，可以自行添加。

## 留言板和评论功能集成

本主题留言板和评论功能支持两种集成，一种是基于 [GitHub Issues](https://docs.github.com/en/issues) 的 [utteranc](https://utteranc.es/)，还有一种是基于[GitHub Discussions](https://docs.github.com/en/discussions) 的 [giscus](https://giscus.app/)。

这里推荐大家使用基于[GitHub Discussions](https://docs.github.com/en/discussions) 的 [giscus](https://giscus.app/)，毕竟 `Issues` 是比较严肃的功能， [giscus](https://giscus.app/) 的页面也比较美观，还可以在同一话题下持续回复。

集成这两者的步骤都十分简单，只需要跟随  [utteranc](https://utteranc.es/) 和 [giscus](https://giscus.app/) 官方教程走到最后，生成对应的配置值，填入 `params.comments` 里面对应的 `giscus` `utteranc` 即可，需要将另外的一个的 `enable` 设置成 `false`。

## 网站数据统计功能

主题可以集成 [Google analytics](https://analytics.google.com/analytics/web/) 和 [umami](https://umami.is/) 两种网站数据统计能力。

 [Google analytics](https://analytics.google.com/analytics/web/) 可以通过官方文档得到对应 `G-xxx` 开头的  `MEASUREMENT ID` ，填入配置文件中的  `googleAnalytics`  即可。

 [Google analytics](https://analytics.google.com/analytics/web/) 的集成简单和免费，不过缺点是无法做到隐私保护（对应数据会提供给 Google），页面也是比较笨重和缓慢。

所以大家如果有需求可以自己搭建 [umami](https://umami.is/) 来做数据统计，相关过程和步骤我放到另外的一篇博客 [如何免费搭建的 umami]({{< ref "blog/zh/how-to-integrate-umami-for-free-to-blog-site.zh.md" >}})。

## SEO

`SEO` 目前主题已经做了对应的优化，包括完善的  `meta` 标签信息等。 除此之外，默认支持 [Google Search Console](https://search.google.com/search-console)，可以根据官方相关文档生成对应的  `google-site-verification` ，填入对应的 `params.analytics.google.SiteVerificationTag`。

## 部署

关于部署也有多种方式，可以选择托管到 CDN 或者平台 [Netlify](https://netlify.com/)、[Heroku](https://www.heroku.com/)、[GoDaddy](https://www.godaddy.com/)、[DreamHost](https://www.dreamhost.com/)、[GitHub Pages](https://pages.github.com/)、[GitLab Pages](https://about.gitlab.com/features/pages/)、[Surge](https://surge.sh/)、[Firebase](https://firebase.google.com/docs/hosting/)、[Google Cloud Storage](https://cloud.google.com/storage/)、[Amazon S3](https://aws.amazon.com/s3/)、[Rackspace](https://www.rackspace.com/cloud/files)、[Azure](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website)和 [CloudFront](https://aws.amazon.com/cloudfront/) 。

本文选择大家比较通用的 [GitHub Pages](https://pages.github.com/) 来作为教程。

### 创建 [GitHub](https://github.com/) 账号

在搭建自己的博客前，我们需要先注册一个 [GitHub](https://github.com/) 账号，这个账号的账户名非常重要，它是后面我们博客的域名地址。

例如我们创建的账号名称是 guangzhengli，那么我们最终的博客地址就是 https://guangzhengli.github.io。

### 创建 GitHub Pages 仓库

完整的教程可以查看 [官方教程](https://pages.github.com/)，简单来讲就是 [创建一个新的 repository](https://github.com/new)，名字为 `username.github.io`。

![j0eHDj](https://cdn.jsdelivr.net/gh/guangzhengli/PicURL@master/uPic/j0eHDj.png)

### Actions

`Actions` 方式用两种，一种是直接推送到 [GitHub Pages](https://pages.github.com/) ，还有一种是在 `gh-pages` 分支上生成静态页面代码的方式。代码都在 https://github.com/guangzhengli/hugo-ladder-exampleSite/tree/master/.github/workflows 这里。

如果你不是选择的直接克隆已经配置好的仓库： https://github.com/guangzhengli/hugo-ladder-exampleSite。需要创建新的文件夹 `.github/workflows` ，添加新的文件 `gh-pages.yml` 文件。

{{ <toggle summary="点击此处查看代码">}}

``` yml
name: generate github pages to gh-pages branch

on:
  push:
    branches:
      - main ## Set a branch name to trigger deployment

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
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        # If you're changing the branch from main, 
        # also change the `main` in `refs/heads/main` 
        # below accordingly.
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

{{ </toggle> }}

### 部署 GitHub Pages

在你项目的根目录下输入以下命令：

``` shell
git remote add origin https://github.com/username/username.github.io.git

git add .

git commit -m "feat: init blog site"

git push origin main
```

确保我们推送成功后，我们会在 `Actions` 中自动的将代码 build 成 GitHub Pages 需要的文件，可以到这个地址检查 `workflow` 是否成功运行。注意将地址中 `username` 替换成自己的账号名称。`https://github.com/username/username.github.io/actions`。

如果 `Actions` 运行成功的话，我们会有一个新的分支叫做 `gh-pages`。这时候我们去仓库的设置中心 `Settings` ，选择 `Pages` ，之后将 `Build and deployment` -> `Source` 选择 `Deploy from a branch`。后