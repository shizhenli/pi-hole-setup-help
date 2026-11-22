---
title: "Pi-hole Blocklist Not Loading: Restore Ad Blocking After a Failed List Update"
description: "Queries resolve but known ad domains are no longer blocked? Learn how to diagnose a Pi-hole blocklist not loading and restore ad blocking on your home LAN."
seo_title: "Fix Pi-hole Blocklist Not Loading: Restore Ad Blocking"
seo_description: "Queries resolve but known ad domains are no longer blocked? Learn how to diagnose a Pi-hole blocklist not loading and restore ad blocking on your home LAN."
keywords:
  - "pi-hole blocklist not loading"
date: 2026-11-22T09:05:04+08:00
publishDate: 2026-11-22T09:05:04+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

Your Pi-hole is answering DNS queries, but known ad domains now resolve normally instead of being blocked. This points to a blocklist that isn't loading, so start by checking the list status before changing anything else.

## Check whether gravity loaded the lists

Open the Pi-hole admin interface and look at the dashboard or Tools > Update Gravity. If the last gravity run shows a failure or zero domains, the blocklist did not load. Run the gravity update once and watch the output. A successful run reports domains imported and no errors. If it fails, note the exact error line, such as a timeout, DNS failure, or a malformed list URL.

## Verify the list source and DNS on the Pi-hole host

Go to Settings > Adlists and confirm the list URL is enabled and spelled correctly. Then test name resolution from the Pi-hole host itself, for example with `dig @127.0.0.1 doubleclick.net`. If that lookup fails or times out, the Pi-hole cannot reach the list server even though client queries resolve. Check the host's own DNS settings and default gateway; a broken upstream path will cause a **pi-hole blocklist not loading** while clients still get answers from cache or another resolver.

## Confirm blocking is actually active

After a successful gravity run, query a known ad domain from a LAN client with `nslookup doubleclick.net` or `dig doubleclick.net`. The expected result is 0.0.0.0 or the Pi-hole address, not a public IP. If you still get a public IP, confirm the client uses Pi-hole as its only DNS server and that no router-provided DNS is overriding it.

## Rollback and recovery

If a recent adlist change caused the failure, disable that list, rerun gravity, and confirm blocking returns before re-enabling it. If gravity still fails, restore your last working list configuration and test again.

If these checks don't restore blocking,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with the gravity output and your dig results.
