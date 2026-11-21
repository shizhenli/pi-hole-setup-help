---
title: "Router DNS Settings Override Pi-hole: Fix the Handout"
description: "Router still handing out its own DNS instead of Pi-hole? Follow these checks to fix the pi-hole router dns settings override and verify the handout."
seo_title: "Router DNS Settings Override Pi-hole: Fix DHCP Handout"
seo_description: "Router still handing out its own DNS instead of Pi-hole? Follow these checks to fix the pi-hole router dns settings override and verify the handout."
keywords:
  - "pi-hole router dns settings"
date: 2026-11-21T15:39:16+08:00
publishDate: 2026-11-21T15:39:16+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

Your router keeps advertising its own DNS address to clients even though you set Pi-hole as the upstream server, so devices never use Pi-hole for filtering.

## Confirm what the router is actually handing out

On a client, check the DNS server it received. On Windows, run `ipconfig /all` and read the DNS Servers line. On macOS or Linux, run `cat /etc/resolv.conf` or `scutil --dns`. If the listed address is the router's LAN IP (for example 192.168.1.1) rather than the Pi-hole's static IP, the DHCP handout is still the problem. If it already shows the Pi-hole address, stop here and diagnose the client instead.

## Check the DHCP DNS field, not the WAN field

Log in to the router and open the LAN or DHCP server settings. Find the DNS server field that is sent to clients. Many routers have two separate places: a WAN DNS setting (used by the router itself) and a LAN DHCP DNS setting (sent to clients). Changing only the WAN field leaves the handout unchanged. Set the LAN DHCP DNS to the Pi-hole's static IP, save, and reboot the router if the interface requires it. This is the **pi-hole router dns settings** step most often missed.

## Verify the handout

Reconnect one client to Wi-Fi or renew its lease (`ipconfig /release` then `ipconfig /renew` on Windows). Re-run the DNS check above. The DNS Servers line should now show only the Pi-hole address. Then open a browser and confirm ads are blocked on that client.

## Rollback if clients lose connectivity

If clients cannot resolve names after the change, restore the previous LAN DNS value, save, and renew the lease again. Internet access returns immediately. If the router will not accept a custom LAN DNS, use Pi-hole as the DHCP server instead, or

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) before making further changes.
