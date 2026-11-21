---
title: "Pi-hole Breaks Internet After Install: Restore DNS with a Controlled Test"
description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to isolate the failure, verify DNS, and roll back safely if needed."
seo_title: "Pi-hole Breaks Internet After Install: Fix DNS Resolution"
seo_description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to isolate the failure, verify DNS, and roll back safely if needed."
keywords:
  - "pi-hole breaks internet after install"
date: 2026-11-21T15:39:16+08:00
publishDate: 2026-11-21T15:39:16+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

## Troubleshooting

You installed Pi-hole, pointed your clients at it, and now name resolution fails across the LAN. Before changing more settings, run a controlled check from one affected client. Open a terminal and run `nslookup example.com 192.168.1.10`, replacing the IP with your Pi-hole address. If it times out, Pi-hole is not answering DNS; if it returns an address, the problem is upstream or in the client's DNS assignment. Next, on the Pi-hole host, run `pihole status`. A healthy result shows DNS listening on port 53 and blocking enabled. If it reports port 53 unavailable, another resolver is bound to that port. Then run `dig @127.0.0.1 example.com` on the Pi-hole itself. A timeout here means Pi-hole cannot reach its upstream DNS servers. Check `/etc/resolv.conf` and confirm the upstream entry is a reachable resolver, not the router's own DNS if the router is also handing out Pi-hole. If Pi-hole answers locally but clients still fail, verify the client received the Pi-hole IP as its only DNS server; a secondary public DNS server can cause intermittent failures. This is the common pattern where **pi-hole breaks internet after install** appears to be a Pi-hole fault but is actually a DNS handoff mismatch. To verify the fix, flush the client DNS cache, then run `nslookup example.com` without specifying a server and confirm it returns an address. If resolution still fails, roll back by restoring the previous DNS setting on the router or client, confirm internet access returns, then retry with one client only. If you need a second set of eyes,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
