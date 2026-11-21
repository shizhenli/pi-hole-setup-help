---
title: "Pi-hole Not Blocking Ads: Verify Clients Use Pi-hole Before Changing Lists"
description: "Queries resolve but ads return? Check which DNS server your clients actually use, confirm Pi-hole blocking, and restore ad blocking without breaking the LAN."
seo_title: "Pi-hole Not Blocking Ads: Fix Client DNS and Restore Blocking"
seo_description: "Queries resolve but ads return? Check which DNS server your clients actually use, confirm Pi-hole blocking, and restore ad blocking without breaking the LAN."
keywords:
  - "pi-hole not blocking ads"
date: 2026-11-21T15:36:09+08:00
publishDate: 2026-11-21T15:36:09+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You are on a home LAN with router-provided DNS, and Pi-hole resolves queries but known ad domains are no longer blocked.

## Confirm the client is asking Pi-hole
On the affected device, check the DNS server it uses. On Windows, run `ipconfig /all` and read the DNS Servers line; on macOS, use `scutil --dns`; on Linux, `resolvectl status`. The address must be the Pi-hole host IP. If it shows the router, the router is still handing out its own DNS and the client bypasses Pi-hole entirely.

## Check Pi-hole's own blocking
From the Pi-hole host, run `pihole status`. You want blocking enabled and FTL running. Then query a known ad domain directly against Pi-hole: `dig @<pi-hole-ip> doubleclick.net`. A reply of 0.0.0.0 or the Pi-hole address means blocking works; a normal public IP means the domain is not on a subscribed list or blocking is off. If blocking is off, run `pihole enable`.

## Fix the DNS handout
If clients point at the router, set the router's DHCP DNS option to the Pi-hole IP only, or disable the router's DNS relay. Reconnect the client so it renews its lease, then repeat the DNS server check. This is the usual cause of **pi-hole not blocking ads** when resolution still works.

## Verification test
After renewal, load a page with obvious ad slots and confirm the Pi-hole query log shows the client's requests and blocked entries. Then run the direct dig test again; both should agree.

## Rollback note
If the LAN loses DNS after the change, restore the router's previous DNS setting and reconnect clients before retrying.

If the client still bypasses Pi-hole after these checks,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
