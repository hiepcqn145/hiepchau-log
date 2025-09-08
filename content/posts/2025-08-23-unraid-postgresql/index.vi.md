---
title: 'unRAID Server: Cài đặt PostgreSQL'
date: 2025-08-23 14:00:00 +0700
draft: false 
categories: ["Homelab"]
tags: ["unRAID", "Self-Hosted", "Database", "PostgreSQL"]
pin: false
slug: "unraid-install-postgresql"
cover: 'https://cdn.anh.moe/f/sVh5rnX4.png-webp'
---

PostgreSQL là nền tảng quản trị cơ sở dữ liệu trên unRAID Server của mình. Chúng không chỉ đảm bảo sự ổn định cho các dự án cá nhân như NPM, Vaultwarden, Umami, v.v., mà còn là xương sống cho mọi hoạt động dữ liệu của mình.

## Tại sao lại chọn PostgreSQL ? 
Thật ra, bản thân mình không hiểu rõ lắm về SQL (như PostgreSQL hay MariaDB). Tuy nhiên, nhiều phần mềm mình tự host lại yêu cầu những hệ quản trị cơ sở dữ liệu này, dù một số có thể chạy trên SQLite. Ban đầu, việc dùng SQLite có vẻ tiện lợi vì mỗi phần mềm tự tạo ra một file cơ sở dữ liệu riêng. Nhưng mình nhận ra vấn đề là cứ mỗi 1 app nó lại tạo ra 1 SQLite riêng. Mặc dù chả ảnh hưởng gì nhưng thôi gom lại 1 rổ cho tiện. 

Ah, còn lý do tại sao lại chọn PostgreSQL thì đơn giản lắm. Vì hiện tại Immich (thay thế choGoogle Photos) chỉ hổ trợ PostgreSQL 😅.

## Cài đặt PostgreSQL trên unRAID
Trong nội dung bài viết này, mình sẽ hướng dẫn các bạn cài đặt PostgreSQL16 đã có sẵn template trên unRAID Community Applications (CA). 

1. Đăng nhập vào unRAID Dashboard
2. Truy cập vào Apps
3. Tìm `PostgreSQL16` trên thanh tìm kiếm
4. Install → Ok 
5. Chỉnh cấu hình Container
    1. Variable: POSTGRES_USER: ""
    2. Variable: POSTGRES_DB: ""

    2 mục này mình để trống để app tự khởi tạo user / database mặc định.

    Sau này khi chạy app có yêu cầu database. Mình sẽ khởi tạo cụ thể user / database sau.
    ![docker_postgresql](https://cdn.anh.moe/f/MyXA1wf.jpg-webp)
6. Apply → Hệ thống tự động pull data và deploy app cho bạn
![docker_postgresql_successfully](https://cdn.anh.moe/f/cQsq8eCr.jpg-webp)

## Login và khởi tạo database
Trước tiên, mình truy cập vào `console` của docker postgresql vừa tạo. 
1. unRAID Dashboard
2. Truy cập vào Docker
3. Chuột phải vào icon postgresql16
4. Console
5. Login vào database
```bash
# psql -d postgres -U postgres -W
Password: 
psql (16.9 (Debian 16.9-1.pgdg120+1))
Type "help" for help.

postgres=#
```
6. Khởi tạo database
```bash
postgres=# create database test;
CREATE DATABASE
postgres=# create user testid with encrypted password 'testpd';
CREATE ROLE
postgres=# grant connect on database test to testid;
GRANT
postgres=# alter database test owner to testid;
ALTER DATABASE
```

![postgresql_command](https://cdn.anh.moe/f/19e5Og.jpg-webp)

## Kết luận
Cài đặt PostgreSQL trên unRAID khá dễ dàng nhờ có sẵn template. Chỉ việc điền vài thông tin cơ bản là có ngay một Docker container chạy PostgreSQL ổn định. 

Từ cơ bản này, mình đã tạo ra các database để chạy các app như Vaultwardent (quản lý mật khẩu), Nginx Proxy Manager (quản lý reverse proxy), Umami Analytics (Phân tích blog này của mình), ... Toàn bộ database đều gom về 1 mối thay vì mỗi file sqlite riêng biệt trong từng folder `/appdata/app_services`

Mình sẽ lên bài hướng dẫn cụ thể từng app sau. Hẹn các bạn trong bài viết tới. 

Happy building !