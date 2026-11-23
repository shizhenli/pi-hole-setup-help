---
title: "Pi-hole Gravity Update Failed: Restore Ad Blocking Without Breaking DNS"
description: "Queries resolve but ads return after a failed gravity update. Follow these checks to restore blocking and verify Pi-hole is filtering again."
seo_title: "Fix pi-hole gravity update failed: Restore Ad Blocking"
seo_description: "Queries resolve but ads return after a failed gravity update. Follow these checks to restore blocking and verify Pi-hole is filtering again."
keywords:
  - "pi-hole gravity update failed"
date: 2026-11-23T11:05:11+08:00
publishDate: 2026-11-23T11:05:11+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You ran a gravity update, it failed partway, and now queries resolve but known ad domains are no longer blocked on your home LAN.

## Check the gravity database state

SSH into your Pi-hole and run `pihole -g` in the foreground. Read the last lines. If it ends with a database error or a download timeout, the blocklists were not rebuilt. A successful run ends with a summary of domains on the adlists. If you see zero domains, the gravity database is empty and blocking is effectively off.

## Confirm DNS is still served

Run `pihole status`. DNS should show as enabled and listening. If DNS is down, fix that first; this article assumes resolution works. Then run `dig doubleclick.net @127.0.0.1` from the Pi. If it returns a real address instead of 0.0.0.0 or the Pi-hole IP, gravity is not loaded.

## Repair the gravity database

Run `pihole -g -f` to force a fresh rebuild. If it still fails, check disk space with `df -h`; a full partition is a common cause. Also confirm the Pi can reach the list URLs with `curl -I https://raw.githubusercontent.com`. When the rebuild completes, the summary should list a nonzero domain count. This resolves most cases of **pi-hole gravity update failed** where DNS itself stayed healthy.

## Verify blocking

From a LAN client, run `nslookup doubleclick.net` against the Pi-hole address. It should return 0.0.0.0 or the Pi-hole IP. Then load a page you know carries ads and confirm they are blocked.

## Rollback note

If the forced rebuild leaves you with an empty database, restore your last known-good backup before changing lists: `pihole -a -r` and select the archive. If you are unsure which state is safe,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) before making further changes.
