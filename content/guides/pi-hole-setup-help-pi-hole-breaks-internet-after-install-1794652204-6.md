---
title: "Pi-hole Breaks Internet After Install: Fix DNS Resolution on Your LAN"
description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to restore internet access and verify DNS resolution."
seo_title: "Pi-hole Breaks Internet After Install? Fix DNS Resolution Now"
seo_description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to restore internet access and verify DNS resolution."
keywords:
  - "pi-hole breaks internet after install"
date: 2026-11-15T18:54:19+08:00
publishDate: 2026-11-15T18:54:19+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole, pointed your router's DNS at it, and now clients cannot resolve names—so you are stuck before you can run a controlled diagnosis.

## Check Pi-hole's Upstream DNS First

On the Pi-hole host, open a terminal and run:

`dig @127.0.0.1 google.com`

If this times out or returns SERVFAIL, Pi-hole itself cannot resolve. Go to Settings > DNS in the Pi-hole admin UI and confirm at least one upstream resolver is selected and enabled. If you set a custom upstream, verify it is reachable from the Pi-hole host with `ping <upstream-ip>`. Save and retry the dig command. A working result shows an A record and a short query time.

## Confirm Clients Are Actually Using Pi-hole

On a failing client, run `nslookup google.com`. The Server line should show your Pi-hole's LAN IP. If it shows your router or a public resolver, the client is bypassing Pi-hole—check the router's DHCP DNS handout or the client's manual DNS setting. If the Server line is correct but the query fails, return to the first check.

## Verification Test

From a client, run `nslookup pi-hole.net` and then open a website. Both should succeed. If the nslookup works but the browser does not, clear the browser's DNS cache or restart the client's network adapter.

## Rollback if You Need Internet Back Immediately

If clients still cannot resolve, temporarily set the router's DNS back to its previous value (often the ISP or router IP) so DHCP hands out working DNS again. Renew the client lease or reconnect Wi-Fi. This restores access while you diagnose—it does not fix the underlying **pi-hole breaks internet after install** condition, so revisit the upstream check once you are back online.

If the upstream test passes but clients still fail, the issue is likely DHCP or firewall related;

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your dig and nslookup outputs.
