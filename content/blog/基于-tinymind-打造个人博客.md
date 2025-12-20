---
title: 基于 tinymind 打造个人博客
date: 2025-12-20T08:59:19.390Z
---


## 介绍

Tinymind 是一个生成博客的工具，将该项目部署到 Vercel 之后，访问对应的域名，授权你的 Github 账号，就可以用它来写你的个人博客和短想法，每条新日记都会立刻同步到你 Github 一个名为"tinymind-blog"的 repo 里。

这是一个开源产品、没有服务器，只授权公开 repo 的写权限，不会读取 private repo ，只要 Github 不倒闭，你的日记数据就不会丢失。

## 原理

1. 用 Github API 在你的目录下创建一个"tinymind-blog" repo
2. 你的每次提交(blog/thoughts)，都会进行一次 commit ，数据被提交到这个 repo 。
3. 读取最新的 blog 和 thoughts 数据，然后渲染在网页上。

## 部署
首先 fork [Tinymind](https://github.com/mazzzystar/tinymind) 到你的帐号，
- 准备仓库：先 fork 到你账号；确认分支（默认 main）包含最新代码。
- 创建 GitHub OAuth App：Homepage 填你的站点（先用临时 Vercel 预览域名也行），Callback 填 https://<你的域名>/api/auth/callback/github。拿到 Client ID/Secret。
- 在 Vercel 创建项目：Import Git 仓库（选择你的 fork），Root 目录保持 `./`，Build Command 默认 `npm run build`，Install Command 默认 `npm install`，Output `/.next`（这几个理论上都不用修改，默认即可）。
- 配置环境变量（Project Settings → Environment Variables，Production/Preview/Development 都加）：
- GITHUB_ID / GITHUB_SECRET：刚才的 OAuth App 值
- NEXTAUTH_URL：生产环境填 https://<你的域名>，预览自动域名可用 https://<vercel-preview-url>（或者留空也行但最好明确）
- NEXTAUTH_SECRET：openssl rand -hex 32 生成的随机字符串
- NEXT_PUBLIC_BASE_URL：https://<你的域名>（供前端 canonical/分享使用）
- 绑定自定义域名：在 Vercel Domain 设置添加你的域名，DNS 做 CNAME/A 指向 Vercel；如果需要 www→根域跳转，可在 next.config.mjs 里更新 redirect 规则为你的域名；app/sitemap.ts 和 public/robots.txt 的 baseUrl/
 - Sitemap 也改成你的域名。