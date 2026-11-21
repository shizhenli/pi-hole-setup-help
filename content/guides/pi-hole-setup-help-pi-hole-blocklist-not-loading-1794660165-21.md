---
title: "Pi-hole Blocklist Not Loading: Restore Ad Blocking After a Failed Gravity Update"
description: "Troubleshoot Pi-hole blocklist not loading when queries resolve but ads slip through. Check gravity status, list health, and DNS cache to restore ad blocking."
seo_title: "Fix Pi-hole Blocklist Not Loading: Restore Ad Blocking"
seo_description: "Troubleshoot Pi-hole blocklist not loading when queries resolve but ads slip through. Check gravity status, list health, and DNS cache to restore ad blocking."
keywords:
  - "pi-hole blocklist not loading"
date: 2026-11-21T15:32:40+08:00
publishDate: 2026-11-21T15:32:40+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

Your Pi-hole resolves DNS but known ad domains are no longer blocked, and you are stuck before completing a controlled diagnosis. This points to a blocklist that is not loading into gravity, not a broken upstream DNS path.

## Check Gravity Status and List Health

SSH into your Pi-hole and run `pihole -g` to force a gravity update. Read the output carefully. A healthy run ends with a summary showing domains processed and no fatal errors. If you see a failed download, an empty list, or a parse error, note which list URL failed. A common cause is a **pi-hole blocklist not loading** because the source returns a 404, times out, or serves HTML instead of a hosts file. Check the list URL in a browser or with `curl -I`; a 200 with text/plain is good, a 404 or redirect is not. Then run `pihole -q doubleclick.net` to see whether a known ad domain is present in gravity. If it returns no match, the list is not loaded.

## Clear Cache and Verify

After fixing or removing the bad list, run `pihole -g` again and confirm the domain count increases. Flush the DNS cache with `pihole restartdns` so clients do not reuse stale answers. On a client, run `nslookup doubleclick.net` and confirm it returns 0.0.0.0 or your Pi-hole address, not a public IP. If it still resolves publicly, the client may be caching; reboot the client or wait for its TTL.

## Rollback Note

If a list change breaks resolution, restore your previous list configuration from the web admin or a backup before retesting. If the failure persists after these checks,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your `pihole -g` output and the failing list URL.
