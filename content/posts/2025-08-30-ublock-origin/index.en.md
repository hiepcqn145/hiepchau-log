---
title: 'Re-enable uBlock Origin on New Google Chrome'
date: 2025-08-30 16:50:00 +0700
draft: true 
categories: ["Web Browser"]
tags: ["Chrome", "MV2", "MV3", "uBlock Origin"]
pin: false
slug: "turn-on-ublock-origin"
cover: 'https://cdn.anh.moe/f/wOZy1K.png-webp'
---

> [!IMPORTANT]
> 2025/09/09
>
> Chrome version `140.0.7339.81` can not turn on MV2 anymore.

This morning at the office, I noticed that Google Chrome had automatically updated to version `138.0.7204.169`. Several websites I visit daily were suddenly flooded with ads. When I checked the Extensions Manager, I saw that both of my ad-blocking extensions — `uBlock Origin` and `Violent Monkey` — were disabled (since they use MV2), and I couldn’t re-enable them. This marks the shift toward Manifest V3 (MV3).

>[!CAUTION] WARNING  
>Re-enabling MV2 on Google Chrome is not recommended.  
>
>By following the instructions below, you acknowledge the potential risks involved.  
>
>DO WITH YOUR OWN RISK.

## Re-enable MV2 on Google Chrome
Browsing through [r/uBlockOrigin](https://www.reddit.com/r/uBlockOrigin), I found updated instructions using `chrome://flags` to temporarily bring back MV2 support. This can help maintain uBlock Origin’s ad-blocking effectiveness on Chrome — at least until Chrome completely drops support.

1. Open Chrome and go to `chrome://flags`
2. Search for flag → M137  
    1. Enable: Temporarily unexpire M137 flags
3. Restart Chrome
4. Go back to `chrome://flags`
5. Search for → MV2  
    1. Disable: Extension Manifest V2 Deprecation Warning Stage  
    2. Disable: Extension Manifest V2 Deprecation Disabled Stage  
    3. Disable: Extension Manifest V2 Deprecation Unsupported Stage  
    4. Enable: Allow legacy extension manifest versions  
    ![Chrome_uBlock_Origin](https://cdn.anh.moe/f/6Oj2m8bm.jpg-webp)
6. Restart Chrome
7. Reinstall `uBlock Origin` and `Violent Monkey` extensions  
![chrome_extensions](https://cdn.anh.moe/f/7BLzq6.jpg-webp)

## Conclusion
Re-enabling MV2 support as shown above is only a temporary workaround until Chrome officially removes MV2. When that happens, users will have to either switch to a browser that still supports MV2 or use more limited extensions — such as uBlock Origin LITE.

## Reference
[r/uBlockOrigin - hide warnings/prevent disabling with chrome flags (until June)](https://www.reddit.com/r/uBlockOrigin/comments/1itw1bz/end_of_support_for_ubo_on_chrome_chromium/)