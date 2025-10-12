---
title: 'Mikrotik: Setup WireGuard VPN'
date: 2025-08-18 08:18:00 +0700
draft: true
categories: ["Homelab", "Networking"]
tags: ["VPN", "Mikrotik", "Self-Hosted"]
pin: false
slug: "wireguard-vpn-tunnel"
cover: 'https://cdn.anh.moe/f/JvxhZNV.jpg-webp'
lightbox:
  enabled: true
---

You want to securely access your NAS at home from the office without exposing port 22 to avoid brute-force attacks. With a *Mikrotik hAP ac²* running RouterOS v7 that supports WireGuard, you can easily set up a secure VPN connection. This allows you to SSH into your NAS safely from anywhere, enhancing your home network security.

---
### Requirements
- *Mikrotik*: Router OS Ver 7. Static IP or DDNS
- *Client*: [WireGuard Client](https://www.wireguard.com/install/) installed

Issues related to Mikrotik’s PPPoE / DHCP setup are not covered in this post.

---
### Setting Up WireGuard on Mikrotik and Client
#### Step 1: Create WireGuard Interface on Mikrotik
Open Winbox and log into your Mikrotik.

- WireGuard → Add new Interface
    - Name: *wireguard1*
    - Listen Port: *13231*

> [!TIP]
> You can change the Name/Port as needed.

![create_wg_interface](https://cdn.anh.moe/f/T70ed0.jpg-webp)

---
#### Step 2: Assign IP Address to WireGuard
After creating the interface, create a separate Address List for easier management.

Go to IP → Address → Add new Address and enter:

```yaml
Address: 25.25.25.1/24
Network: 25.25.25.0
Interface: wireguard1
```
![ipaddress](https://cdn.anh.moe/f/c3j2jLj.jpg-webp)

---
#### Step 3: Retrieve Public Key on Client
On your PC, open WireGuard → Add Tunnel → Add empty tunnel.

Then name the tunnel and copy the Public Key.

![pc_client_get_public_key](https://cdn.anh.moe/f/IUebozWg.jpg-webp)

---
#### Step 4: Create Peer to Access WireGuard Server on Client
Once you have the Public Key, return to *WinBox*.

WireGuard → Peer → Add new Peer and paste the copied Public Key. Also assign an IP to this connection.

![add_peer](https://cdn.anh.moe/f/jiSEAS.jpg-webp)

---
#### Step 5: Configure WireGuard on Client
After creating the Peer, go back to the WireGuard interface tab and copy its Public Key.

Then return to the PC, copy the sample config below and adjust it to match your settings.

```yaml
[Interface]
PrivateKey = kPnywjIUK8xysYCZWYPg7OZXfJLO/1qjUBR6qs8sVkk=
Address = 25.25.25.2/32 # IP assigned in Step 4
DNS = 8.8.8.8 

[Peer]
PublicKey = P4KPCop/66Qtw+EShxVyoefuUYC0zxfNx7h4y+AanBo=
AllowedIPs = 0.0.0.0/0
Endpoint = hiepchau.net:13231 
PersistentKeepalive = 10
```
![wireguard_client_config](https://cdn.anh.moe/f/ZQ99n7ND.jpg-webp)

---
#### Step 6: NAT Port 13231 in Winbox
Back to Winbox → IP → Firewall → Filter Rules → Add new Rule:

```yaml
Chain: input
Protocol: udp
Dst. Port: 13231
In. Interface: all ppp
Action: accept
```
![nat_port](https://cdn.anh.moe/f/SLt51T.jpg-webp)

> [!CAUTION]  
> Move this rule on top but under PPPoE rule

---
#### Step 7: NAT Masquerade to Access Internal LAN
From Step 6, switch to:
IP → Firewall → NAT → Create new NAT Rule:

```yaml
Chain: srcnat
Src. Address: 25.25.25.1/24
Action: masquerade
```
![nat_masquerade](https://cdn.anh.moe/f/aDTaBY.jpg-webp)

---
### Result

![wg_result](https://cdn.anh.moe/f/uoB8Ce.jpg-webp)

And that’s it!

Good luck setting up your WireGuard VPN for secure remote access to your LAN!