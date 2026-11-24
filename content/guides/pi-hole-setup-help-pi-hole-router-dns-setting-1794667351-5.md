---
title: "Router DNS Override: When Your Router Hands Out Its Own DNS Instead of Pi-hole"
description: "Fix the router DNS settings override Pi-hole issue where your router hands out its own DNS instead of the Pi-hole address. Step-by-step checks and verification."
seo_title: "Router DNS Override Pi-hole: Fix DHCP Handout"
seo_description: "Fix the router DNS settings override Pi-hole issue where your router hands out its own DNS instead of the Pi-hole address. Step-by-step checks and verification."
keywords:
  - "pi-hole router dns setting"
date: 2026-11-24T10:05:03+08:00
publishDate: 2026-11-24T10:05:03+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You set up Pi-hole on your home LAN, but your router keeps handing out its own DNS address to clients instead of the Pi-hole IP. This means devices bypass Pi-hole entirely, and you're stuck at the router DNS settings override Pi-hole step before completing a controlled diagnosis. Here's how to fix it.

## Check the Router's DHCP DNS Handout

Log into your router's admin panel and find the DHCP settings. Look for a field labeled "DNS Server" or "Primary DNS." If it shows the router's own IP (e.g., 192.168.1.1) or an ISP address, that's the override. Change it to your Pi-hole's static IP (e.g., 192.168.1.10). Save and reboot the router. Result interpretation: after reboot, the router should hand out the Pi-hole IP as the DNS server to all DHCP clients.

## Verify the Handout

On a client device, release and renew its DHCP lease (or reconnect to Wi-Fi). Then check the DNS server: on Windows, run `ipconfig /all` and look for "DNS Servers"; on macOS, use `scutil --dns`; on Linux, `cat /etc/resolv.conf`. The result should show your Pi-hole IP, not the router's. If it still shows the router, the override persists.

## Confirm Pi-hole Is Receiving Queries

Open Pi-hole's admin dashboard and go to Query Log. Browse a few websites from the client. You should see new queries appear. If the log stays empty, the client is still using another DNS. This confirms the **pi-hole router dns setting** is not yet effective.

## Rollback and Recovery

If you lose internet after changing the router DNS, revert the DHCP DNS field to its previous value (often the router's IP) and save. Reboot the router and renew client leases. This restores connectivity while you diagnose further.

If the override persists after these steps, your router may have a separate DNS override or a firmware quirk. For guided help,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
