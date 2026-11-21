---
title: "Pi-hole Blocklist Not Loading: Fix a Whitelist Entry That Still Blocks a Domain"
description: "A whitelisted domain is still blocked on your Pi-hole home LAN. Follow these checks to confirm the allow entry, clear stale cache, and verify the fix."
seo_title: "Pi-hole Blocklist Not Loading: Whitelist Entry Still Blocks Domain"
seo_description: "A whitelisted domain is still blocked on your Pi-hole home LAN. Follow these checks to confirm the allow entry, clear stale cache, and verify the fix."
keywords:
  - "pi-hole blocklist not loading"
date: 2026-11-21T15:32:40+08:00
publishDate: 2026-11-21T15:32:40+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a domain to the allow list, but Pi-hole still blocks it, and you are stuck before completing a controlled diagnosis.

## Confirm the Allow Entry Is Active

Open the Pi-hole admin console and go to Domains. Under the allow list, find your exact domain. Check that it is enabled and matches the blocked domain character for character, including any subdomain. If you allowed example.com but the blocked query is ads.example.com, add the subdomain or use a wildcard if your version supports it. A mismatch here is the most common reason a whitelist entry appears not to work.

## Check Which List Is Blocking It

Use Query Log and filter for the domain. The status column will show whether the block came from a blocklist, a regex, or a local blacklist entry. If a regex matches the domain, the allow entry may not override it. Disable the matching regex temporarily and retest. Also run a gravity update from Tools > Update Gravity. If the update fails, the blocklist may be stale, which is a separate **pi-hole blocklist not loading** symptom worth fixing before further whitelist testing.

## Clear Cache and Retest

Flush the Pi-hole DNS cache with pihole flush from the command line, then flush the client browser cache or use a private window. Run nslookup yourdomain.com on a client and confirm it returns the expected address rather than 0.0.0.0. If it still returns the block address, recheck the Query Log for the exact blocking source.

## Rollback Note

If you disabled a regex or edited lists during testing, re-enable them one at a time and retest so you do not leave a gap in blocking. If the allow entry still fails after these checks,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your Query Log output and list configuration.
