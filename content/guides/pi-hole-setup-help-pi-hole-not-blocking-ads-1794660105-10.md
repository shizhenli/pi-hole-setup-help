---
title: "Pi-hole Not Blocking Ads: Diagnose the Query Path Before You Change Anything"
description: "Ads resolve again because queries are bypassing Pi-hole or gravity is empty. Learn the exact checks to confirm the query path and restore blocking."
seo_title: "Pi-hole Not Blocking Ads: Fix Ads That Resolve Again"
seo_description: "Ads resolve again because queries are bypassing Pi-hole or gravity is empty. Learn the exact checks to confirm the query path and restore blocking."
keywords:
  - "pi-hole not blocking ads"
date: 2026-11-17T10:04:55+08:00
publishDate: 2026-11-17T10:04:55+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

## Troubleshooting

You are on a home LAN with router-provided DNS, and known ad domains now resolve instead of being blocked. Before changing settings, confirm where the client's DNS queries actually go. On a client, run `nslookup doubleclick.net` and read the Server line. If it shows your router's LAN address rather than the Pi-hole address, the router is still handing out its own DNS and clients bypass Pi-hole, which explains why you see **pi-hole not blocking ads**. Next, open the Pi-hole admin Query Log and search for doubleclick.net. If the query is absent, the client never reached Pi-hole; if it appears as forwarded or allowed, the blocklist is not matching. Check Settings > Pi-hole DNS and confirm the upstream is not set to a public resolver that clients also use directly. Then run `pihole -g` and watch the output: a healthy update ends with a non-zero domains-being-blocked count. If the count is zero or the gravity database is empty, blocking cannot occur. For the verification test, flush the client DNS cache, query doubleclick.net again, and confirm the Server line now shows the Pi-hole address and the reply is 0.0.0.0 or the Pi-hole block page address. If the query still resolves, repeat the Server-line check on a second device to rule out a single stale client. If you changed the router DNS handout, note the original LAN DNS value first; to roll back, restore that value and renew DHCP leases so clients regain connectivity. If the query path is correct but the domain still resolves,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with the Query Log entry and gravity output.
