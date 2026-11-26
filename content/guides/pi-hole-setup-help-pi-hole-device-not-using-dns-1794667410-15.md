---
title: "One Device Still Bypasses Pi-hole: Pin Down the DNS Leak"
description: "One device still uses another DNS server and shows no Pi-hole queries? Follow these checks to confirm and fix a single-device DNS bypass on your LAN."
seo_title: "Pi-hole Device Not Using DNS: Fix One Device Bypassing Pi-hole"
seo_description: "One device still uses another DNS server and shows no Pi-hole queries? Follow these checks to confirm and fix a single-device DNS bypass on your LAN."
keywords:
  - "pi-hole device not using dns"
date: 2026-11-26T08:04:16+08:00
publishDate: 2026-11-26T08:04:16+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

## Confirm the bypass before changing anything

You set up Pi-hole on your home LAN, but one device still resolves through another DNS server and never appears in the query log. That is the **pi-hole device not using dns** symptom, and it usually means the device is ignoring the DNS address your router hands out.

Start by identifying the device's current resolver. On the affected device, open its network settings and note the DNS server addresses. If they are not your Pi-hole IP, the device is bypassing Pi-hole. Next, check the Pi-hole query log and filter by that device's IP or hostname. No entries in the last few minutes confirms the bypass.

## Find where the override comes from

Check three places in order. First, the device's own static DNS setting: if it lists a public resolver such as 8.8.8.8, remove it and set the Pi-hole address, or switch the device to automatic DNS. Second, private DNS or encrypted DNS features: if enabled, disable them temporarily because they skip the LAN resolver. Third, the router's DHCP handout: confirm the advertised DNS server is the Pi-hole IP, not the router itself or an ISP resolver. If the router advertises itself, some devices will use the router as DNS and bypass Pi-hole.

## Verification test

Reconnect the device to Wi-Fi, then load a few new domains. Watch the Pi-hole query log for that device's IP. Queries appearing within a minute mean the fix worked. If the log stays empty, the device is still overriding DNS.

## Rollback note

If a change breaks connectivity, set the device back to automatic DNS and reconnect. Then revisit the router handout before retrying. For a guided review,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
