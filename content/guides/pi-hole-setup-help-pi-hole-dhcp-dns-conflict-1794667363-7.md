---
title: "Pi-hole DHCP DNS Conflict: Fix One Device Still Bypassing Pi-hole"
description: "Troubleshoot a pi-hole dhcp dns conflict where one device still uses another DNS server and shows no Pi-hole queries, with exact checks and a verification test."
seo_title: "Pi-hole DHCP DNS Conflict: One Device Still Bypasses Pi-hole"
seo_description: "Troubleshoot a pi-hole dhcp dns conflict where one device still uses another DNS server and shows no Pi-hole queries, with exact checks and a verification test."
keywords:
  - "pi-hole dhcp dns conflict"
date: 2026-11-24T11:05:13+08:00
publishDate: 2026-11-24T11:05:13+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

## Troubleshooting

You're on a home LAN where the router hands out DNS, and one device still queries another server instead of Pi-hole. Start by confirming what that device actually received. On the device, check its DNS settings: Windows uses ipconfig /all, macOS uses scutil --dns, Linux uses resolvectl status, and phones show it under Wi-Fi details. If the listed DNS server is the router's LAN IP rather than Pi-hole's IP, the device is following the router's handout. Next, open Pi-hole's query log and filter by that device's IP. Zero queries means the device never asked Pi-hole, so this is a handout or bypass problem, not blocking. Then inspect the router's DHCP settings. If Pi-hole's DHCP server is also enabled, you have a **pi-hole dhcp dns conflict**: two DHCP servers may answer, and the device can keep the router's DNS. Disable one DHCP server, preferably the router's, or set the router's DHCP DNS option to Pi-hole's IP only. On the device, forget the Wi-Fi network and reconnect, or run ipconfig /release and ipconfig /renew, then ipconfig /flushdns. Verification test: from the device, run nslookup example.com and confirm the answer comes from Pi-hole's IP, then load a few sites and confirm new entries appear in Pi-hole's query log for that device. If the device still shows the old DNS server, it may have a static DNS entry; clear it and set it to automatic. Rollback note: if disabling router DHCP leaves devices without addresses, re-enable it and instead set its DNS handout to Pi-hole's IP. If the conflict persists,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
