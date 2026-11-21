---
title: "Router DNS Override: Why Your Router Ignores Pi-hole and How to Fix It"
description: "Router still hands out its own DNS instead of Pi-hole? Follow these checks to fix the pi-hole router dns setting override and restore expected DNS behavior."
seo_title: "Pi-hole Router DNS Setting: Fix Router Handing Out Its Own DNS"
seo_description: "Router still hands out its own DNS instead of Pi-hole? Follow these checks to fix the pi-hole router dns setting override and restore expected DNS behavior."
keywords:
  - "pi-hole router dns setting"
date: 2026-11-21T15:39:16+08:00
publishDate: 2026-11-21T15:39:16+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You set Pi-hole as your DNS server, but your router keeps handing out its own DNS address to clients.

## Check the router's DHCP DNS setting

Log in to your router's admin page and find the DHCP settings. Look for a field labeled DNS Server, DNS Primary, or DNS Override. It should contain only the Pi-hole's LAN IP address (for example, 192.168.1.10). If it shows the router's own IP, the ISP's DNS, or a public resolver like 8.8.8.8, that is your problem. Remove those entries and enter the Pi-hole address. Save and apply.

## Confirm the change took effect

On a client device, disconnect and reconnect to Wi-Fi, or run `ipconfig /renew` (Windows) or `sudo dhclient -r && sudo dhclient` (Linux). Then check the DNS server with `ipconfig /all` or `cat /etc/resolv.conf`. The result should list only the Pi-hole IP. If it still shows the router, the router is overriding your **pi-hole router dns setting**, often because a secondary DNS field is populated or a "DNS Rebind" protection is active.

## Verification test

From a client, run `nslookup pi-hole.net`. The server line should show your Pi-hole IP. Then open a known ad-heavy site and confirm blocking works. If queries still go elsewhere, the router is not honoring the DHCP DNS entry.

## Rollback

If clients lose internet after the change, temporarily set the router's DNS back to automatic, renew DHCP on one device, and confirm connectivity before retrying.

If the router still refuses to hand out the Pi-hole address,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your router model and DHCP settings.
