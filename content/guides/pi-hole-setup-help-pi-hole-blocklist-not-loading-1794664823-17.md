---
title: "Pi-hole Blocklist Not Loading: When an Allow Entry Still Blocks a Domain"
description: "A whitelist entry isn't unblocking a domain on your Pi-hole home LAN. Follow these checks to diagnose the blocklist loading issue and restore expected allow behavior."
seo_title: "Pi-hole Blocklist Not Loading: Fix Allow Entry Still Blocking"
seo_description: "A whitelist entry isn't unblocking a domain on your Pi-hole home LAN. Follow these checks to diagnose the blocklist loading issue and restore expected allow behavior."
keywords:
  - "pi-hole blocklist not loading"
date: 2026-11-22T10:05:10+08:00
publishDate: 2026-11-22T10:05:10+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a domain to the allow list, but it is still blocked, and you want to fix this before running a full diagnosis.

## Confirm the allow entry is active

Open the Pi-hole admin console and go to Domains. Find your entry and verify it is enabled and listed under the exact domain you need. If it shows as disabled, enable it and click Save. Then run `pihole restartdns` from the terminal. If the domain still resolves to 0.0.0.0, the allow entry is not being applied.

## Check for a conflicting blocklist entry

A domain can be both allowed and blocked. In Domains, search the same domain under the block lists. If it appears there, the block may take precedence depending on list order. Remove the block entry or move the allow entry higher, then restart DNS. This is often the reason a **pi-hole blocklist not loading** correctly into the active rules, leaving stale blocks in place.

## Verify list loading and gravity

Run `pihole -g` to update gravity. Watch the output for errors such as failed downloads or permission denied. If gravity reports zero domains or skips your list, the blocklist is not loading, and allow entries may not behave as expected. Check that the list URL is reachable and the file permissions are correct.

## Verification test

From a client, run `nslookup yourdomain.com` against the Pi-hole IP. A correct allow result returns the real IP, not 0.0.0.0. If it still returns 0.0.0.0, flush the client DNS cache and retry.

## Rollback note

If changes make things worse, disable the new allow entry, restart DNS, and re-enable your previous working configuration. For guided help,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
