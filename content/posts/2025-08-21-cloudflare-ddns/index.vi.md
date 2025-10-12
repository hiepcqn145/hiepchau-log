---
title: 'unRAID Server: Cập nhật IP động bằng Cloudflare DDNS'
date: 2025-08-21 07:00:00 +0700
draft: false
categories: ["Homelab"]
tags: ["unRAID", "Cloudflare", "DDNS", "Self-Hosted"]
pin: false
slug: "cloudflare-ddns"
cover: 'https://cdn.anh.moe/f/t0xzJQ.png-webp'
lightbox:
  enabled: true
---

IP động, hầu hết gói cước Internet gia đình đều thuộc dạng này. Có nghĩa là, IP thay đổi bất cứ lúc nào, thời điểm nào, như cái cách mà vợ bạn giận bạn vậy. 😱

IP thay đổi một cách chóng mặt như vậy thì việc kết nối tới unRAID server trở nên rất khó khăn. Mà cho dù bạn có IP tĩnh thì việc nhớ dãy IP của mình theo dạng dãy số `101.101.97.255` cũng là điều không thể. Quỹ đen mình còn không nhớ có bao nhiêu nữa nói chi cái dãy IP này. 

Thế nên chúng ta cần tới giải pháp DDNS (Dynamic DNS). Ở trong nội dung bài viết này, mình sẽ hướng dẫn các bạn chạy Cloudflare-DDNS trên docker để tự động cập nhật IP động của mình thành DNS Tĩnh 

## Chuẩn bị
1. Tài khoản [Cloudflare](https://cloudflare.com)
2. Domain đã được chuyển về Cloudflare quản lý

## Tạo Cloudflare API Token
Container cloudflare-ddns sẽ sử dụng Token API tùy chỉnh để gửi IP đã cập nhật đến tài khoản Cloudflare của bạn. Vậy nên chúng ta cần tạo Cloudflare API Token trước tiên
1. Truy cập [Cloudflare API](https://dash.cloudflare.com/profile/api-tokens)
2. Create Token → Custom token (End of Page) → Get started
3. Fill in the information
    1. Token name: cloudflare-ddns 
    2. Permission: Zone | DNS | Edit
    3. Zone Resource: Include | All zones from an account | Chọn domain của bạn
    4. Continue to summary
    5. Create Token
4. Copy token và lưu tạm vào đâu đó. Lưu ý là API Token này sẽ không hiện lại LẦN THỨ 2.
    ![create_api_tokens](https://cdn.anh.moe/f/Zt3L2X.jpg-webp)

## Cài đặt cloudflare-ddns
Mình sử dụng docker image [onzu/cloudflare-ddns](https://github.com/oznu/docker-cloudflare-ddns) mặc dù đã ngưng phát triển nhưng vẫn hoạt động tốt. 

### unRAID UI
1. Truy cập unRAID Dashboard → APPS → cloudflare ddns
2. Chọn App → Install

    ![unraid_install](https://cdn.anh.moe/f/Y7uxpl.jpg-webp)

3. Thiết lập các variable của container → Apply để deploy
![app_config](https://cdn.anh.moe/f/XFxILZo.jpg-webp)

Sau khi container hoạt động, hãy kiểm tra tài khoản Cloudflare để xem Bản ghi A đã được thêm vào tên miền bạn chỉ định với IP bên ngoài hay chưa.

### Docker compose
Tạo thư mục để lưu file docker-compose.yaml
```bash
mkdir ~/cloudflare-ddns
cd cloudflare-ddns
nano docker-compose.yaml
```

Nội dung trong file docker-compose như bên dưới
```yaml {filename="docker-compose.yaml"}
version: '2'
services:
  cloudflare-ddns:
    image: oznu/cloudflare-ddns:latest
    restart: always
    environment:
      - API_KEY=xxxxxxx #các bạn điền API Token vào đây
      - ZONE=example.com #domain của bạn
      - SUBDOMAIN=subdomain #subdomain nếu bạn muốn 
      - PROXIED=true
```

Chạy lệnh để khởi tạo container 

```bash
docker-compose up -d
```

## Kết luận
Vậy là xong, cứ mỗi 15 phút ứng dụng tự động kiểm tra IP Động của bạn và cập nhật IP mới nhất lên Cloudflare. Bây giờ thay vì phải nhớ địa chỉ IPv4 dài ngoằn hoặc phải kiểm tra lại IP nếu nó thay đổi thì mình chỉ cần nhớ mỗi domain `test.hiepchau.net` là xong. 

Chúc bạn thành công ! 

