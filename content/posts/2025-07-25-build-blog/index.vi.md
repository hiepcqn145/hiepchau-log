---
title: 'Xây dựng blog với Hugo và Cloudflare Pages'
date: 2025-07-25 14:00:00 +0700
draft: true 
categories: ["Blog"]
tags: ["Hugo", "Self-Hosted"]
pin: false
slug: "deploy-hugo-to-cloudflare-pages"
# cover: 'https://cdn.anh.moe/f/GluuKpyx.jpg-webp'
---

Bài viết này sẽ hướng dẫn bạn cách tạo một blog cá nhân sử dụng Hugo với giao diện Hugo Narrow và triển khai dễ dàng lên Cloudflare Pages hoàn toàn miễn phí.

## Chuẩn bị
1. *Tài khoản Github*: để lưu mã nguồn.
2. *Tài khoản Cloudflare*: để triển khai, lưu trữ và phân tích.
3. *Trình soạn thảo*: VSCode.
4. [*Cài đặt Git*](https://git-scm.com/downloads)  
5. [*Cài đặt Hugo-Extended*](https://gohugo.io/installation/)

## Tạo site Hugo mới
Hiện tại, blog này đang sử dụng giao diện Hugo Narrow – một giao diện sạch và hiện đại dành cho blog cá nhân, xây dựng bằng Tailwind CSS 4.0, hỗ trợ nhiều bảng màu và tùy chỉnh người dùng.

Để cài đặt và cấu hình giao diện này, vui lòng tham khảo tài liệu chính thức:
- [Hugo Narrow - Docs](https://hugo-narrow-docs.vercel.app/)
- [Github - tom2almighty/hugo-narrow](https://github.com/tom2almighty/hugo-narrow)

Sau đó, đẩy mã nguồn lên GitHub:
1. Tạo một repository trống trên GitHub, ví dụ `myblog`
2. Đẩy mã nguồn lên GitHub
    ```bash
    git add .
    git commit -m "Initial commit"
    git remote add origin https://github.com/your-username/myblog.git
    git branch -M main
    git push -u origin main
    ```
    > [!TIP] Mẹo nhỏ  
    > Nếu bạn đang dùng Windows và không quen với dòng lệnh, bạn có thể dùng [GitHub Desktop](https://desktop.github.com/download/) với giao diện thân thiện.

## Cấu hình Cloudflare Pages
Giờ chúng ta sẽ chuyển sang Cloudflare Pages để triển khai blog:

1. Đăng nhập vào [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. Truy cập mục Compute (Workers) → Workers & Pages
3. Bấm Get Started hoặc Create → Chọn Pages → nhấn Get Started trong phần Import a Repository.
4. Kết nối với Git → Chọn repository bạn đã commit ở bước trên → Bắt đầu thiết lập  
   ![cloudflare_pages_import_repository](https://cdn.anh.moe/f/GRuqT5Z.png-webp)
5. Cấu hình build:

    1. Project name: `myblog` / sẽ được gán vào subdomain sau khi build
    2. Product branch: `main` 
    3. Framework preset: `Hugo` 
    4. Build command: `hugo --gc --minify`
    5. Build output directory: `public`
    7. Save and Deploy
    7. Sau khi triển khai xong, bạn có thể truy cập blog tại `myblog.pages.dev`.

6. Gắn domain riêng:
    1. Đảm bảo domain của bạn đang được quản lý bởi Cloudflare.
    2. Truy cập dự án Cloudflare Pages → mục Custom domains
    3. Bấm “Set up a custom domain”.
    4. Chờ một chút, Cloudflare sẽ tự động xử lý phần còn lại, sau đó bạn có thể truy cập blog qua domain của chính mình.

## Kết luận
Vậy là xong!

Với Hugo và Cloudflare Pages, bạn đã có đầy đủ công cụ để tạo một blog cá nhân hiệu năng cao, miễn phí và dễ dàng triển khai. Hugo cung cấp khả năng quản lý nội dung linh hoạt và hỗ trợ giao diện đa dạng, trong khi Cloudflare Pages đảm bảo việc xuất bản và truy cập luôn nhanh chóng, tiện lợi nhờ vào hệ thống CDN toàn cầu.
