---
title: "Pi-hole Not Blocking Ads: Fix Router-Provided DNS Override"
description: "Queries resolve but ads load? Fix Pi-hole not blocking ads when your router still hands out its own DNS. Step-by-step checks, verification, and rollback."
seo_title: "Pi-hole Not Blocking Ads: Fix Router DNS Override"
seo_description: "Queries resolve but ads load? Fix Pi-hole not blocking ads when your router still hands out its own DNS. Step-by-step checks, verification, and rollback."
keywords:
  - "pi-hole not blocking ads"
date: 2026-11-25T11:05:03+08:00
publishDate: 2026-11-25T11:05:03+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You set up Pi-hole on your home LAN, but now queries resolve and known ad domains are no longer blocked, so you're stuck at the pi-hole stops blocking ads step before a controlled diagnosis.

## Confirm the client is using Pi-hole
On a device that shows ads, check its DNS server. On Windows, run `ipconfig /all` and read the DNS Servers line; on macOS, run `scutil --dns | grep nameserver`; on Linux, run `resolvectl status`. If the listed DNS server is your router's LAN IP, the client is bypassing Pi-hole. That explains why resolution works but filtering does not.

## Check the router's DHCP DNS handout
Log in to the router and open the DHCP settings. If the DNS server field is blank or set to the router itself, the router is advertising its own address. Set the primary DNS to the Pi-hole's static LAN IP, save, and reboot the router or renew the client lease. After renewal, repeat the DNS check above; the client should now list the Pi-hole IP.

## Verify blocking directly
From the Pi-hole host, run `dig @127.0.0.1 doubleclick.net` and confirm the answer is 0.0.0.0 or the Pi-hole's blocking address. Then run the same query from the affected client without specifying a server; it should return the same blocked answer. If it returns a real IP, the client is still using another resolver.

## Rollback if needed
If changing the router DNS breaks connectivity, restore the previous DNS value and reboot the router. Keep a note of the original setting so you can revert quickly.

If the client still bypasses Pi-hole after these checks, that is the remaining **pi-hole not blocking ads** cause to isolate.

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your router model and the DNS output.
