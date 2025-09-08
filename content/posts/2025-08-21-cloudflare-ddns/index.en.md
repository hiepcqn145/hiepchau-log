---
title: 'Update Dynamic IP with Cloudflare DDNS'
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

Dynamic IP — this is what most home internet packages offer. That means your IP address can change anytime, unpredictably, like when your wife suddenly gets mad at you. 😱

With constantly changing IPs, connecting to your unRAID server becomes difficult. And even if you have a static IP, remembering something like `101.101.97.255` is not realistic. I can't even remember how much is in my secret stash, let alone a string of numbers like that.

So we need a DDNS (Dynamic DNS) solution. In this guide, I’ll walk you through running Cloudflare-DDNS in Docker to automatically update your dynamic IP to a static DNS record.

## Requirements
1. A [Cloudflare](https://cloudflare.com) account
2. A domain managed under your Cloudflare account

## Create a Cloudflare API Token
The cloudflare-ddns container will use a custom API token to send your current IP address to your Cloudflare account. So we need to create the API Token first.

1. Go to [Cloudflare API](https://dash.cloudflare.com/profile/api-tokens)
2. Click Create Token → Custom token (End of Page) → Get started
3. Fill in the details:
    1. Token name: cloudflare-ddns  
    2. Permission: Zone | DNS | Edit  
    3. Zone Resource: Include | All zones from an account | Select your domain  
    4. Continue to summary  
    5. Create Token  
4. Copy and save the token somewhere safe. Note: You won’t be able to view this token again.  
   ![create_api_tokens](https://cdn.anh.moe/f/Zt3L2X.jpg-webp)

## Install cloudflare-ddns
I’m using the [onzu/cloudflare-ddns](https://github.com/oznu/docker-cloudflare-ddns) Docker image. Although it’s no longer maintained, it still works great.

### Docker Compose
Create a folder for your docker-compose.yaml file:

```bash
mkdir ~/cloudflare-ddns
cd cloudflare-ddns
nano docker-compose.yaml
```

The content of the `docker-compose.yaml` file is as follows:

```yaml
version: '2'
services:
  cloudflare-ddns:
    image: oznu/cloudflare-ddns:latest
    restart: always
    environment:
      - API_KEY=xxxxxxx # paste your API Token here
      - ZONE=example.com # your domain
      - SUBDOMAIN=subdomain # subdomain if you want
      - PROXIED=true
```

Run the following command to initialize the container:

```bash
docker-compose up -d
```

### unRAID UI

1. Go to **unRAID Dashboard** → **APPS** → search for `cloudflare ddns`
2. Select the app → Click **Install**

   ![unraid_install](https://cdn.anh.moe/f/Y7uxpl.jpg-webp)

3. Configure variable of container → Click **Apply** to deploy  
   ![app_config](https://cdn.anh.moe/f/XFxILZo.jpg-webp)

Once the container is up and running, check your Cloudflare account to see if an **A record** has been added to your specified domain with your current public IP.

## Conclusion

And that’s it! Every 15 minutes, the application will automatically check your dynamic IP and update the latest one to Cloudflare. Now, instead of remembering a long IPv4 address or checking it every time it changes, you just need to remember your domain like `test.hiepchau.net`.

Good luck!

