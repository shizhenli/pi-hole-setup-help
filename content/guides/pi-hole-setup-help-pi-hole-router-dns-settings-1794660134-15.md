---
title: "Router DNS Settings Override Pi-hole: Stop the Router From Handing Out Its Own DNS"
description: "Router still handing out its own DNS instead of Pi-hole? Follow these checks to fix pi-hole router dns settings override and verify the DHCP handout."
seo_title: "Fix Router DNS Override of Pi-hole: DHCP Handout Troubleshooting"
seo_description: "Router still handing out its own DNS instead of Pi-hole? Follow these checks to fix pi-hole router dns settings override and verify the DHCP handout."
keywords:
  - "pi-hole router dns settings"
date: 2026-11-18T10:20:39+08:00
publishDate: 2026-11-18T10:20:39+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

Your router keeps advertising its own DNS address instead of Pi-hole's, so clients never query the blocklist. Before changing anything else, confirm what the router actually hands out. On a client, run `ipconfig /all` (Windows) or `cat /etc/resolv.conf` (Linux/macOS). If the DNS server listed is the router's LAN IP (for example 192.168.1.1) rather than Pi-hole's address, the router is overriding Pi-hole at the DHCP level.

## Check the DHCP DNS option

Log in to the router and find the DHCP server settings, not the WAN DNS fields. Look for a LAN DNS or DHCP DNS option. Set it to Pi-hole's static LAN IP only, then save and reboot the router. If the router has no such field, disable its DHCP server and let Pi-hole handle DHCP; otherwise the router will keep advertising itself.

## Confirm the lease changed

On the client, release and renew the lease (`ipconfig /release` then `ipconfig /renew`, or reconnect Wi-Fi). Re-run the DNS check. The listed server must now be Pi-hole's IP. If it is still the router, the client kept an old lease or the router ignored the setting.

## Verification test

Query Pi-hole directly: `nslookup example.com <Pi-hole-IP>`. A reply from Pi-hole confirms it answers. Then query without specifying a server; if the answer comes from the router, the handout is still wrong. Also check Pi-hole's query log for that client's IP.

## Rollback note

If clients lose internet after the change, set the router's DHCP DNS back to automatic and reboot, restoring connectivity while you review the **pi-hole router dns settings**.

Still stuck after these checks?

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
