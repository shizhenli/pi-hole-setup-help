---
title: "Pi-hole Upstream DNS Timeout: Fix Internet After Installation"
description: "Clients lose name resolution after pointing DNS to Pi-hole. Diagnose a pi-hole upstream dns timeout and restore internet access with a controlled test and rollback."
seo_title: "Pi-hole Upstream DNS Timeout: Restore Internet After Installation"
seo_description: "Clients lose name resolution after pointing DNS to Pi-hole. Diagnose a pi-hole upstream dns timeout and restore internet access with a controlled test and rollback."
keywords:
  - "pi-hole upstream dns timeout"
date: 2026-11-16T12:17:47+08:00
publishDate: 2026-11-16T12:17:47+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole, pointed your router's DNS at it, and now clients on your home LAN cannot resolve names, so the internet appears broken.

## Confirm the failure is upstream, not client-side

On a client that lost resolution, run `nslookup example.com` or `dig example.com`. If it times out or returns SERVFAIL, the client is reaching Pi-hole but Pi-hole cannot get an answer. Next, open the Pi-hole admin page and check Settings > DNS. Confirm at least one upstream resolver is enabled. Then run `dig @127.0.0.1 example.com` from the Pi-hole host. A timeout here points to a **pi-hole upstream dns timeout**: Pi-hole is receiving queries but its upstream servers are not responding.

## Check upstream reachability

From the Pi-hole host, ping the upstream resolver's IP and test port 53 with `dig @<upstream-ip> example.com`. If that times out, the router or ISP may be blocking outbound DNS, or the upstream address may be wrong. Temporarily set a known public resolver as upstream and retest. If queries succeed, the original upstream was the problem.

## Verification test

After changing upstream DNS, flush the client cache (`ipconfig /flushdns` on Windows, `sudo resolvectl flush-caches` on Linux) and run `nslookup example.com` again. A valid answer confirms resolution is restored. Then load a website to confirm the expected outcome: internet works after Pi-hole installation.

## Rollback and recovery

If you cannot restore resolution quickly, point the router's DNS back to its previous setting or disable Pi-hole DHCP DNS handout. Clients will regain internet while you continue diagnosis. For guided help,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
