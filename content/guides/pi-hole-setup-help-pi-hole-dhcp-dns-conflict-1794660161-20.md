---
title: "Resolve a Pi-hole DHCP DNS Conflict When a Device Still Bypasses Pi-hole"
description: "Troubleshoot a Pi-hole DHCP DNS conflict where one device still uses another DNS server and shows no Pi-hole queries. Step-by-step checks and a verification test."
seo_title: "Pi-hole DHCP DNS Conflict: Fix One Device Bypassing Pi-hole"
seo_description: "Troubleshoot a Pi-hole DHCP DNS conflict where one device still uses another DNS server and shows no Pi-hole queries. Step-by-step checks and a verification test."
keywords:
  - "pi-hole dhcp dns conflict"
date: 2026-11-21T15:32:40+08:00
publishDate: 2026-11-21T15:32:40+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

Your device still resolves through your router or another DNS server, and Pi-hole logs show no queries from it, even though other devices work fine. This is a classic **pi-hole dhcp dns conflict** symptom, and you can isolate it without changing your whole network.

## Check the device's actual DNS server

On the affected device, open its network settings and note the DNS server it reports. Then compare that address to your Pi-hole IP. If it shows your router's LAN IP or a public resolver (for example, 8.8.8.8), the device is not using Pi-hole. That is your first concrete clue.

## Check DHCP assignment

Log into your router and look at the DHCP client list for that device. If the router is still handing out its own DNS address, the device will keep bypassing Pi-hole. Confirm whether Pi-hole is acting as DHCP server or whether the router is. Only one should be authoritative for DNS handouts.

## Check for a static DNS entry

On the device itself, inspect manual or static DNS settings. A manually entered DNS server overrides DHCP. Remove it and set DNS to automatic, then reconnect to Wi-Fi or renew the lease.

## Verification test

After the change, run a DNS lookup from the device (for example, open a new website or use nslookup). Then check Pi-hole's query log for that device's IP. A successful result is a fresh query appearing within a minute. If no query appears, the device is still bypassing Pi-hole.

## Rollback note

If the device loses connectivity after changing DNS, set it back to automatic and reboot it before making further changes.

If the query log stays empty after these checks,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your router model and the device's DNS settings.
