---
title: 'Build a Blog with Hugo and Cloudflare Pages'
date: 2025-07-25 14:00:00 +0700
draft: true 
categories: ["Blog"]
tags: ["Hugo", "Self-Hosted"]
pin: false
slug: "deploy-hugo-to-cloudflare-pages"
# cover: 'https://cdn.anh.moe/f/GluuKpyx.jpg-webp'
---

This guide will show you how to create a personal blog using Hugo with the Hugo Narrow theme and deploy it easily on Cloudflare Pages completely free.

## Prerequisites
1. *GitHub account*: to store the source code.
2. *Cloudflare account*: for deployment, hosting, and analytics.
3. *Text editor*: VSCode.
4. [*Install Git*](https://git-scm.com/downloads)  
5. [*Install Hugo-Extended*](https://gohugo.io/installation/)

## Create a New Hugo Site
This blog uses the Hugo Narrow theme – a clean and modern theme for personal blogs, built with Tailwind CSS 4.0, supporting multiple color palettes and user customization.

To install and configure this theme, refer to the official documentation:
- [Hugo Narrow - Docs](https://hugo-narrow-docs.vercel.app/)
- [Github - tom2almighty/hugo-narrow](https://github.com/tom2almighty/hugo-narrow)

Then push your source code to GitHub:
1. Create an empty repository on GitHub, e.g., `myblog`
2. Push your source code:
    ```bash
    git add .
    git commit -m "Initial commit"
    git remote add origin https://github.com/your-username/myblog.git
    git branch -M main
    git push -u origin main
    ```
    > [!TIP] Pro Tip  
    > If you’re using Windows and not familiar with command line, try [GitHub Desktop](https://desktop.github.com/download/) with its user-friendly interface.

## Configure Cloudflare Pages
Now let’s move on to Cloudflare Pages for deployment:

1. Log in to [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. Go to Compute (Workers) → Workers & Pages
3. Click Get Started or Create → Choose Pages → then Get Started under Import a Repository.
4. Connect your GitHub → Choose the repository from the previous step → Begin configuration  
   ![cloudflare_pages_import_repository](https://cdn.anh.moe/f/GRuqT5Z.png-webp)
5. Build settings:

    1. Project name: `myblog` / will be assigned to the subdomain after build
    2. Product branch: `main` 
    3. Framework preset: `Hugo` 
    4. Build command: `hugo --gc --minify`
    5. Build output directory: `public`
    7. Save and Deploy
    7. After deployment, access your blog at `myblog.pages.dev`.

6. Add custom domain:
    1. Make sure your domain is managed by Cloudflare.
    2. Go to your Cloudflare Pages project → Custom domains
    3. Click “Set up a custom domain”.
    4. Wait a moment while Cloudflare handles the rest. Then you can access your blog via your own domain.

## Conclusion
And that’s it!

With Hugo and Cloudflare Pages, you have all the tools to create a fast, free, and easy-to-deploy personal blog. Hugo offers flexible content management and theme support, while Cloudflare Pages ensures fast and reliable publishing via its global CDN network.