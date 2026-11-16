---
title: "Pi-hole Blocklist Not Loading: Restore Ad Blocking When Queries Resolve but Ads Appear"
description: "Pi-hole resolves queries but known ad domains are no longer blocked. Follow these checks to restore ad blocking when the blocklist fails to load."
seo_title: "Pi-hole Blocklist Not Loading: Fix Ads Not Blocked"
seo_description: "Pi-hole resolves queries but known ad domains are no longer blocked. Follow these checks to restore ad blocking when the blocklist fails to load."
keywords:
  - "pi-hole blocklist not loading"
date: 2026-11-16T10:04:56+08:00
publishDate: 2026-11-16T10:04:56+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

Your Pi-hole is resolving DNS queries, but known ad domains are no longer being blocked, and you're stuck at the "pi-hole stops blocking ads" step before completing a controlled diagnosis.

## Check the Blocklist Status

Open the Pi-hole admin interface and go to Group Management > Adlists. Look at the status column for each list. If any list shows a red error or a timestamp that is days old, the **pi-hole blocklist not loading** is the likely cause. Click Update Gravity (Tools > Update Gravity) and watch the output. A successful run ends with "Done" and a nonzero domain count. If it fails, note the error message: a timeout or 404 means the list URL is unreachable; a permission error means the gravity database cannot be written.

## Verify DNS and Gravity Database

From a client, run `nslookup doubleclick.net` against the Pi-hole IP. If it returns a real IP instead of 0.0.0.0 or the Pi-hole's blocking address, blocking is inactive. On the Pi-hole host, run `pihole -g` in a terminal. If it reports "database is locked," stop the FTL service with `pihole restartdns stop`, then rerun `pihole -g`. If the list URL is unreachable, temporarily disable it in Adlists, update gravity, and confirm blocking resumes with the remaining lists.

## Verification Test

After a successful gravity update, run `nslookup doubleclick.net` again. You should see 0.0.0.0 or your configured blocking IP. Then load a page with known ads and confirm they no longer appear.

## Rollback Note

If disabling a list caused unexpected blocking gaps, re-enable it and instead fix the URL or increase the gravity timeout in Settings > DNS > Advanced. Keep a backup of `/etc/pihole/gravity.db` before major changes.

If these steps do not restore blocking,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your gravity output and list status.
