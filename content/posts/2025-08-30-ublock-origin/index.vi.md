---
title: 'Bật lại uBlock Origin trên Google Chrome mới'
date: 2025-08-30 16:50:00 +0700
draft: false 
categories: ["Web Browser"]
tags: ["Chrome", "MV2", "MV3", "uBlock Origin"]
pin: false
slug: "turn-on-ublock-origin"
cover: 'https://cdn.anh.moe/f/wOZy1K.png-webp'
---

> [!IMPORTANT]
> 2025/09/09
>
> Chrome version `140.0.7339.81` hiện không còn mở lại MV2 được nữa.

Sáng nay vào văn phòng công ty thấy Google Chrome của mình tự động update lên version `138.0.7204.169`. Hàng loạt website mình truy cập hằng ngày tràn ngập trong quảng cáo. Truy cập vô Extensions Manager thì thấy 2 extensions mình dùng để chặn quảng cáo là `uBlock Origin` và `Violent Monkey` đã tự động disable (do chạy ở MV2) và không bật lên lại được nữa - đánh dấu bước chuyển đổi sang Manifest V3 (MV3).

>[!CAUTION] LƯU Ý
>Việc mở lại MV2 trên Google Chrome là không khuyến khích
>
>Việc bạn làm theo hướng dẫn của mình dưới đây là đã hiểu rõ các nguy cơ rủi ro có thể xảy ra.
>
>DO WITH YOUR OWN RISK.

## Mở lại MV2 trên Google Chrome
Ngồi lướt [r/uBlockOrigin](https://www.reddit.com/r/uBlockOrigin) thì thấy có hướng dẫn cập nhật trong `chrome://flags` để bật lại tính năng MV2. Điều này có thể giúp duy trì hiệu quả chặn quảng cáo của uBlock Origin trên Chrome, ít nhất là trong thời gian cho đến khi Chrome ngừng hổ trợ hẳn.

1. Mở trình duyệt Chrome vào truy cập `chrome://flags`
2. Search flags → M137 
    1. Enable: Temporarily unexpire M137 flags
3. Khởi động lại Chrome
4. Truy cập lại `chrome://flags`
5. Search flags → MV2
    1. Disable: Extension Manifest V2 Deprecation Warning Stag
    2. Disable: Extension Manifest V2 Deprecation Disabled Stage
    3. Disable: Extension Manifest V2 Deprecation Unsupported Stage
    4. Enabled: Allow legacy extension manifest versions
    ![Chrome_uBlock_Origin](https://cdn.anh.moe/f/6Oj2m8bm.jpg-webp)
6. Khởi động lại Chrome
7. Cài đặt lại Extensions `uBlock Origin` và `Violent Monkey`
![chrome_extensions](https://cdn.anh.moe/f/7BLzq6.jpg-webp)

## Kết luận
Việc bật lại hỗ trợ MV2 như trên chỉ là một giải pháp tạm thời cho đến khi Chrome chính thức loại bỏ hoàn toàn MV2 khỏi hệ thống và phải chấp nhận đổi sang web browser khác có hổ trợ MV2 hoặc sử dụng các extension hạn chế hơn. uBlock Origin LITE chẳn hạn. 

## Tham khảo
[r/uBlockOrigin - hide warnings/prevent disabling with chrome flags (until June)](https://www.reddit.com/r/uBlockOrigin/comments/1itw1bz/end_of_support_for_ubo_on_chrome_chromium/)