---
title: "Pi-hole Gravity Update Failed: Restore Ad Blocking on Your Home LAN"
description: "Queries resolve but ads slip through after a failed gravity update? Follow these checks to restore Pi-hole ad blocking on your home LAN."
seo_title: "Pi-hole Gravity Update Failed: Fix Ad Blocking | Pi-hole Setup Help"
seo_description: "Queries resolve but ads slip through after a failed gravity update? Follow these checks to restore Pi-hole ad blocking on your home LAN."
keywords:
  - "pi-hole gravity update failed"
date: 2026-11-26T10:04:13+08:00
publishDate: 2026-11-26T10:04:13+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You're on a home LAN with router-provided DNS, and after a **pi-hole gravity update failed**, pages still load but known ad domains are no longer blocked.

## Check the gravity database and update output

SSH into your Pi-hole and run `pihole -g`. Read the last 20 lines. If you see `database is locked` or `unable to update gravity`, the blocklist database didn't refresh. Confirm the file exists: `ls -lh /etc/pihole/gravity.db`. A zero-byte or missing file means the update aborted. Next, check free space with `df -h /`. If usage is above 90%, the update cannot write the new database. Also run `pihole -q doubleclick.net`. If it returns no results, the domain isn't in the active blocklist, which confirms the gravity data is stale or empty.

## Repair the database, then verify blocking

If space is fine, rebuild the database: `pihole -g -r` (or delete `/etc/pihole/gravity.db` and run `pihole -g` again). Watch for permission errors on `/etc/pihole`. After the update completes, run `pihole -q doubleclick.net` again; you should see it listed under a blocklist. Then test from a client: `nslookup doubleclick.net` should return `0.0.0.0` or your Pi-hole's blocking address, not a public IP. Finally, open a known ad-heavy page and confirm the Pi-hole query log shows blocked entries.

## Rollback note

If the rebuild fails repeatedly, restore the previous `gravity.db` from your backup (or reinstall the blocklists with `pihole -g` after clearing the list cache). Keep a copy before retrying.

If these steps don't restore blocking,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your `pihole -g` output and `df -h` result.
