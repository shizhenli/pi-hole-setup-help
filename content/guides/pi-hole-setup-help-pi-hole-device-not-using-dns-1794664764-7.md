---
title: "Pi-hole Device Not Using DNS: Fix a Single Device That Still Bypasses Pi-hole"
description: "One device still uses another DNS server and shows no Pi-hole queries? Follow these checks to restore the expected outcome: a device bypasses pi-hole."
seo_title: "Pi-hole Device Not Using DNS: Fix One Device Bypassing Pi-hole"
seo_description: "One device still uses another DNS server and shows no Pi-hole queries? Follow these checks to restore the expected outcome: a device bypasses pi-hole."
keywords:
  - "pi-hole device not using dns"
date: 2026-11-21T15:36:09+08:00
publishDate: 2026-11-21T15:36:09+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

One device on your home LAN still uses another DNS server and shows no Pi-hole queries, even though the rest of the network works.

## Confirm the device's actual DNS server

On the affected device, open its network settings and note the DNS server it is using. If it shows your router's IP, the device is not using Pi-hole directly. If it shows a public resolver such as 8.8.8.8 or 1.1.1.1, the device is bypassing Pi-hole entirely. This is the common **pi-hole device not using dns** symptom: the device resolves names, but Pi-hole's query log stays empty for that client.

## Check router DHCP and device overrides

On the router, confirm the DHCP DNS handout points to the Pi-hole address, not the router itself. Then check for a per-device DNS override or static DNS entry for the affected device. If one exists, remove it and renew the device's lease. On the device, set DNS to automatic and reconnect to Wi-Fi or Ethernet. If the device still shows a manual DNS entry after reconnecting, the override is stored on the device, not the router.

## Verification test

From the device, visit several sites, then open Pi-hole's query log and filter by the device's IP address. You should see queries from that client within a minute. If the log remains empty, the device is still using another resolver.

## Rollback note

If you changed router DHCP settings, restore the previous DNS value before finishing, then renew leases on other devices so they are not left with a stale configuration.

If the device still bypasses Pi-hole after these checks,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with the device model and its current DNS setting.
