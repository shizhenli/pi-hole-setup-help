---
title: "Pi-hole DNS Resolution Failed: Fix Internet After Installation"
description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to restore internet access and verify DNS resolution."
seo_title: "Pi-hole DNS Resolution Failed: Fix Internet After Installation"
seo_description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to restore internet access and verify DNS resolution."
keywords:
  - "pi-hole dns resolution failed"
date: 2026-11-23T08:05:04+08:00
publishDate: 2026-11-23T08:05:04+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole, pointed your router's DNS at it, and now every device on your home LAN has lost name resolution.

## Confirm the failure
On a client, run `nslookup example.com`. If it times out or returns "server can't find," DNS is failing. Next, check whether Pi-hole itself can resolve: SSH into the Pi-hole host and run `nslookup example.com 127.0.0.1`. If that works, the problem is between clients and Pi-hole. If it fails, Pi-hole's upstream DNS is the issue.

## Check upstream DNS settings
Open the Pi-hole admin interface and go to Settings > DNS. Confirm at least one upstream resolver is selected and enabled. If you previously relied on your router for DNS, the router's address may not be reachable from the Pi-hole host. Try a public resolver such as 1.1.1.1, save, and retest with `nslookup example.com 127.0.0.1` on the Pi-hole host.

## Check client reachability
From a client, ping the Pi-hole IP address. If pings fail, clients cannot reach Pi-hole. Verify the Pi-hole host has a static IP and that your router's DHCP hands out that same address as DNS. A common cause of **pi-hole dns resolution failed** is a router still advertising its own DNS alongside Pi-hole, so clients query the wrong server.

## Verification test
After correcting upstream DNS or the DHCP handout, renew a client's lease and run `nslookup example.com` again. A successful answer with an IP address confirms resolution is restored. Then load a website to confirm browsing works.

## Rollback and recovery
If you need internet back immediately, set your router's DNS back to its previous value or disable Pi-hole's DHCP integration. This restores the prior state while you diagnose. For guided help,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
