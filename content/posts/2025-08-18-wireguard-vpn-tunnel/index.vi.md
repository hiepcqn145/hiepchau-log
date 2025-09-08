---
title: 'Hướng dẫn thiết lập Wireguard trên Mikrotik'
date: 2025-08-18 08:18:00 +0700
draft: false
categories: ["Homelab", "Networking"]
tags: ["VPN", "Mikrotik", "Self-Hosted"]
pin: false
slug: "wireguard-vpn-tunnel"
cover: 'https://cdn.anh.moe/f/JvxhZNV.jpg-webp'
lightbox:
  enabled: true
---

Bạn muốn truy cập NAS ở nhà từ công ty một cách an toàn mà không cần mở port 22, tránh nguy cơ tấn công brute-force. Với *Mikrotik hAP ac²* chạy RouterOS v7 có tích hợp WireGuard, bạn có thể dễ dàng thiết lập một kết nối VPN bảo mật. Điều này cho phép bạn SSH vào NAS an toàn mọi lúc mọi nơi, tăng cường bảo mật cho mạng gia đình của bạn.

---
### Yêu cầu
- *Mikrotik*: Router OS Ver 7. IP Tĩnh hoặc DDNS
- *Client*: đã cài [WireGuard Client](https://www.wireguard.com/install/)


Các vấn đề liên quan đến cấu hình PPPoE / DHCP của Mikrotik không nằm trong bài viết này. 

---
### Thiết lập WireGuard trên Mikrotik và Client
#### Bước 1: Tạo Interface WireGuard trên Mikrotik
Mở winbox và login vào Mikrotik. 

- WireGuard → Add new Interface
    - Name: *wireguard1*
    - Listen Port: *13231*

> [!TIP]
> Các bạn có thể đổi Name/Port nếu muốn

![create_wg_interface](https://cdn.anh.moe/f/T70ed0.jpg-webp)

---
#### Bước 2: Tạo IP Address cho WireGuard
Sau khi tạo xong interface, mình tiến hành tạo 1 Address List riêng cho interface đó để tiện quản lý. 

Vào tab IP → Address → Add new Adress và điền các thông số. 

```yaml
Address: 25.25.25.1/24
Network: 25.25.25.0
Interface: wireguard1
```
![ipaddress](https://cdn.anh.moe/f/c3j2jLj.jpg-webp)

---
#### Bước 3: Lấy Public Key trên Client
WireGuard trên PC → Add Tunnel → Add empty tunnel.

Sau đó đặt tên cho tunnel và copy Public Key.

![pc_client_get_public_key](https://cdn.anh.moe/f/IUebozWg.jpg-webp)

---
#### Bước 4: Tạo Peer để truy cập vào WireGuard Server trên Client
Lấy được Public Key rồi ta quay trở lại *WinBox*. 

WireGuard → Peer → Add new Peer và dán Public Key đã copy ở trên vào. Gán luôn IP cho kết nối này

![add_peer](https://cdn.anh.moe/f/jiSEAS.jpg-webp)

---
#### Bước 5: Cấu hình WireGuard trên Client
Sau khi tạo Peer xong, quay lại tab WireGuard interface và copy Public Key của Interface. 

Tiếp đó quay lại PC, copy sample config bên dưới và hiệu chỉnh lại cho phù hợp với cấu hình của mình.

```yaml {filename="wireguard.conf"}
[Interface]
PrivateKey = kPnywjIUK8xysYCZWYPg7OZXfJLO/1qjUBR6qs8sVkk=
Address = 25.25.25.2/32 # Allowed Address đã cấu hình ở Bước 4
DNS = 8.8.8.8 

[Peer]
PublicKey = P4KPCop/66Qtw+EShxVyoefuUYC0zxfNx7h4y+AanBo= #Public Key mình nói copy ở vừa nãy
AllowedIPs = 0.0.0.0/0 # Để chấp nhận tất cả IP kết nối ()
Endpoint = hiepchau.net:13231 
PersistentKeepalive = 10
```
![wireguard_client_config](https://cdn.anh.moe/f/ZQ99n7ND.jpg-webp)

---
#### Bước 6: NAT Port 13231 trên Winbox
Trở lại Winbox → IP → Firewall → Filter Rules → Add new Rule
```yaml
Chain: input
Protocol: udp
Dst. Port: 13231
In. Interface: all ppp
Action: accept
```
![nat_port](https://cdn.anh.moe/f/SLt51T.jpg-webp)

> [!CAUTION]
> Kéo rule vừa tạo lên đầu danh sách NAT (nằm dưới rule pppoe)

---
#### Bước 7: NAT masquerade để truy cập mạng LAN nội bộ 
Ở bước 6, chuyển qua Tab 
IP → Firewall → NAT → Create new NAT Rule
```yaml
Chain: srcnat
Src. Address: 25.25.25.1/24 # IP Address List ở Bước 2
Action: masquerade
```
![nat_masquerade](https://cdn.anh.moe/f/aDTaBY.jpg-webp)

---
### Kết quả

![wg_result](https://cdn.anh.moe/f/uoB8Ce.jpg-webp)

Vậy là xong rồi đó ! 

Chúc bạn thiết lập Wireguard VPN để kết nối về mạng LAN nội bộ thành công !

---