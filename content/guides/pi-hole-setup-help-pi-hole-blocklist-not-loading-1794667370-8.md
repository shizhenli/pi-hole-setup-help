---
title: "Pi-hole Blocklist Not Loading: Restore Ad Blocking When Queries Still Resolve"
description: "Queries resolve but known ad domains are no longer blocked? Follow these practical checks to fix a pi-hole blocklist not loading and restore ad blocking."
seo_title: "Pi-hole Blocklist Not Loading: Fix Ad Blocking on a Home LAN"
seo_description: "Queries resolve but known ad domains are no longer blocked? Follow these practical checks to fix a pi-hole blocklist not loading and restore ad blocking."
keywords:
  - "pi-hole blocklist not loading"
date: 2026-11-24T12:06:44+08:00
publishDate: 2026-11-24T12:06:44+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

## Troubleshooting

You are on a home LAN with router-provided DNS, and after reaching the pi-hole stops blocking ads step, known ad domains still resolve because the blocklist is not loading. Start by confirming the blocklist state on the Pi-hole itself. Open a terminal and run `pihole -g` to force a gravity update. Watch the output: if it reports the list download failed, times out, or shows zero domains imported, the blocklist is not loading. If it reports domains imported, the list loaded and the issue is elsewhere. Next, check the blocklist file or URL under Settings > Blocklists. A single typo, a dead URL, or an expired subscription will silently prevent loading. Remove any suspect entry, save, and run `pihole -g` again. Then verify with a known ad domain: run `nslookup doubleclick.net 192.168.1.10` (use your Pi-hole IP). A blocked result returns 0.0.0.0 or the Pi-hole address; a normal public IP means blocking is still inactive. If the list loads but ads still resolve, flush the DNS cache with `pihole restartdns` and retest. For recovery, keep a copy of your working blocklist file before editing; if the update fails after changes, restore that copy and rerun `pihole -g` to return to the previous state. If the **pi-hole blocklist not loading** persists after these checks, the list source or local storage may need closer inspection.

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) if you want a second set of eyes on the gravity output.

Repeat the same check after a restart and confirm the result twice.
