---
title: "Pi-hole Gravity Update Failed: Fix Ad Blocking When Queries Still Resolve"
description: "Pi-hole gravity update failed? Queries still resolve but ads are no longer blocked. Follow these steps to diagnose and restore ad blocking."
seo_title: "Pi-hole Gravity Update Failed: Restore Ad Blocking"
seo_description: "Pi-hole gravity update failed? Queries still resolve but ads are no longer blocked. Follow these steps to diagnose and restore ad blocking."
keywords:
  - "pi-hole gravity update failed"
date: 2026-11-18T09:05:03+08:00
publishDate: 2026-11-18T09:05:03+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You're on a home LAN with router-provided DNS, queries still resolve, but known ad domains are no longer blocked—and you're stuck before a controlled diagnosis.

## Check the gravity database state

SSH into your Pi-hole and run `pihole -g` to see the exact error. A common result is a permission or disk-space failure during list download. Interpret it precisely: if the output ends with "database is locked" or "no space left on device," the update never completed, so the blocklist is stale or empty. If it says "all lists downloaded successfully," the failure is elsewhere—check `/etc/pihole/gravity.db` exists and is readable by the `pihole` user.

## Confirm disk and permissions

Run `df -h /` and `df -h /etc/pihole`. If either is above 90% used, free space first. Then run `ls -l /etc/pihole/gravity.db` and confirm ownership is `pihole:pihole`. If not, correct it with `sudo chown pihole:pihole /etc/pihole/gravity.db`. A **pi-hole gravity update failed** message often traces to these two conditions.

## Rebuild and verify

After fixing space or ownership, run `pihole -g` again. Then test blocking directly: `dig +short doubleclick.net @127.0.0.1` should return `0.0.0.0` or `::`. If it returns a real IP, blocking is still off—check that your router hands out only the Pi-hole IP as DNS.

## Rollback note

If the rebuild corrupts the database, restore the previous copy from `/etc/pihole/gravity.db.bak` (created automatically) and rerun `pihole -g`. If the failure persists after these checks,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with the exact `pihole -g` output.
