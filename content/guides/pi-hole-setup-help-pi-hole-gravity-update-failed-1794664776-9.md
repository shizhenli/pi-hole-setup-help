---
title: "Pi-hole Gravity Update Failed: Restore Ad Blocking When Queries Still Resolve"
description: "Queries resolve but ads return after a pi-hole gravity update failed. Follow these checks to restore blocking without breaking DNS on your home LAN."
seo_title: "Pi-hole Gravity Update Failed: Fix Ad Blocking on Home LAN"
seo_description: "Queries resolve but ads return after a pi-hole gravity update failed. Follow these checks to restore blocking without breaking DNS on your home LAN."
keywords:
  - "pi-hole gravity update failed"
date: 2026-11-21T15:39:16+08:00
publishDate: 2026-11-21T15:39:16+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You are on a home LAN with router-provided DNS, and after a **pi-hole gravity update failed**, known ad domains resolve again while the internet still works.

## Confirm the gravity list state
Run `pihole -g` in a terminal and read the final lines. A successful run ends with a summary showing domains on the adlists; a failure shows an error, a timeout, or zero domains processed. If you see zero, the blocklist database is empty and blocking will not occur even though DNS answers.

## Check disk space and permissions
Run `df -h /etc/pihole` and confirm the filesystem is not 100 percent full. Then run `ls -l /etc/pihole/gravity.db` and confirm the file is owned by pihole and readable. A full disk or wrong ownership causes the update to abort silently. If space is low, remove old backups under /etc/pihole before retrying.

## Test one known ad domain
From a LAN client, run `nslookup doubleclick.net` against the Pi-hole address. If it returns a public address instead of 0.0.0.0 or the Pi-hole address, blocking is inactive. Then run `pihole -q doubleclick.net` on the Pi-hole host. No match means the domain is not in gravity, confirming the failed update.

## Retry and verify
Run `pihole -g` again after freeing space or fixing ownership. When it completes, repeat the nslookup test; the ad domain should now return the blocking address. If it still resolves publicly, flush the client DNS cache and retry once.

## Rollback note
Before changing ownership or deleting files, copy gravity.db to a safe location. If a retry worsens resolution, restore that copy and restart the DNS service. For guided diagnosis,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
