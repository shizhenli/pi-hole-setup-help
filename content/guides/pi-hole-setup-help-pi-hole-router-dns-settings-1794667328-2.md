---
title: "Router DNS Override: Stop Your Router from Handing Out Its Own DNS Instead of Pi-hole"
description: "Your router is handing out its own DNS instead of Pi-hole's address. Learn how to check DHCP settings, interpret results, and verify the fix with a controlled test."
seo_title: "Router DNS Settings Override Pi-hole: Fix DHCP Handout"
seo_description: "Your router is handing out its own DNS instead of Pi-hole's address. Learn how to check DHCP settings, interpret results, and verify the fix with a controlled test."
keywords:
  - "pi-hole router dns settings"
date: 2026-11-23T12:05:13+08:00
publishDate: 2026-11-23T12:05:13+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You set Pi-hole as your DNS server, but your router keeps giving devices its own DNS address instead of Pi-hole's.

## Check the router's DHCP DNS setting

Log in to your router's admin page and find the DHCP server settings. Look for a field labeled "DNS Server," "Primary DNS," or "DHCP DNS." If it shows the router's LAN IP (e.g., 192.168.1.1) or is blank, the router is handing out its own DNS. Change it to your Pi-hole's static IP (e.g., 192.168.1.10). Save and reboot the router. This is the core **pi-hole router dns settings** step that many guides skip.

## Interpret the result

After reboot, connect a client device and check its DNS server. On Windows, run `ipconfig /all` and look for "DNS Servers." On macOS, use `scutil --dns`. If the DNS server matches Pi-hole's IP, the override is fixed. If it still shows the router's IP, your router may not support custom DHCP DNS, or another DHCP server is active.

## Verification test

From a client, run `nslookup pi-hole.net` and confirm the query is answered by Pi-hole's IP. Then open Pi-hole's admin dashboard and check the Query Log for a recent entry from that client. If both pass, DNS is flowing through Pi-hole.

## Rollback note

If clients lose internet after the change, revert the DHCP DNS field to its previous value and reboot the router. Then use

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) to diagnose further without leaving your network broken.

## Final check

Confirm no secondary DNS is set to a public resolver, as that can bypass Pi-hole. With the DHCP DNS correctly pointed to Pi-hole, your router will stop overriding it.
