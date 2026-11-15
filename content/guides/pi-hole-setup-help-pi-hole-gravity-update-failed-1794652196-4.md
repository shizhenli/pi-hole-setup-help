---
title: "Pi-hole Gravity Update Failed: Restore Ad Blocking After a Failed Update"
description: "Pi-hole gravity update failed and ads are no longer blocked? Follow these checks to diagnose the failure, restore blocking, and verify with a test."
seo_title: "Pi-hole Gravity Update Failed: Fix Ad Blocking | Pi-hole Setup Help"
seo_description: "Pi-hole gravity update failed and ads are no longer blocked? Follow these checks to diagnose the failure, restore blocking, and verify with a test."
keywords:
  - "pi-hole gravity update failed"
date: 2026-11-15T18:54:19+08:00
publishDate: 2026-11-15T18:54:19+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You're on a home LAN with router-provided DNS, and after a gravity update you notice queries still resolve but known ad domains are no longer blocked.

## Check the gravity update result

Run `pihole -g` in a terminal. Read the final lines. If you see `[✗] Gravity update failed` or a non-zero exit, the blocklist database didn't rebuild. A successful run ends with `[✓] Gravity update complete` and a count of domains. If it failed, note the error text: `database is locked`, `no space left on device`, or `unable to resolve` each point to a different cause.

## Check disk space and permissions

Run `df -h /etc/pihole`. If use is above 90%, free space first. Then check ownership with `ls -l /etc/pihole/gravity.db`. It should be owned by `pihole:pihole`. If not, correct it with `sudo chown pihole:pihole /etc/pihole/gravity.db`. A **pi-hole gravity update failed** message often traces to one of these two conditions.

## Rebuild and verify

Run `pihole -g` again. When it completes, confirm the domain count is non-zero. Then test blocking: `dig doubleclick.net @127.0.0.1` should return `0.0.0.0` or `::`. If it returns a real IP, blocking is still off.

## Rollback note

If the rebuild keeps failing, restore the previous database from backup: `sudo cp /etc/pihole/gravity.db.bak /etc/pihole/gravity.db` and restart with `pihole restartdns`. This returns you to the last working blocklist while you investigate.

If these checks don't restore blocking,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with the exact error line from `pihole -g`.
