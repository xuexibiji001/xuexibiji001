---
title: 如何零成本给博客集成 umami 数据统计分析功能
date: 2022-08-14T21:34:36+08:00
tags: ["hugo", "ladder", "教程", "analytics", "umami"]
series: ["how to create your blog"]
featured: true
---
本篇文章介绍如何零成本给博客或者网站集成 [umami](https://umami.is/) 统计功能。数据库用的是 [supabase](https://app.supabase.com/) 提供的有限额的 [postgres](https://supabase.com/docs/guides/database)，不过免费提供的 500M 对于  [umami](https://umami.is/) 来讲已经足够了。

托管 [umami](https://umami.is/) 服务用的是 [vercel](https://vercel.com/)。得益于现在的云厂商优秀的服务能力，你可以在 10 分钟内集成好 *umami*。可以点击 [数据统计看板](https://umami-ochre-nu.vercel.app/share/o3zAba1V/guangzhengli) 查看最终效果。

<!--more-->

## 创建数据库
