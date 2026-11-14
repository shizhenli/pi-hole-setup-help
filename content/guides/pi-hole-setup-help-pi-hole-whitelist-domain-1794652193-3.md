---
title: "Pi-hole Whitelist Domain Not Working: Why an Allow Entry Still Blocks a Domain"
description: "A domain added to the Pi-hole allow list is still blocked. Follow these checks to confirm the entry, clear cache, and verify the fix."
seo_title: "Pi-hole Whitelist Domain Not Unblocking: Fix Allow List Entry"
seo_description: "A domain added to the Pi-hole allow list is still blocked. Follow these checks to confirm the entry, clear cache, and verify the fix."
keywords:
  - "pi-hole whitelist domain"
date: 2026-11-14T19:35:14+08:00
publishDate: 2026-11-14T19:35:14+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a domain to the allow list, but it is still blocked on a home LAN where your router provides DNS.

## Confirm the entry is actually saved
Open the Pi-hole admin console and go to Domains. Under the allow list, search for the exact domain. Result interpretation: if it is missing, the save did not complete; re-add it and click Add. If it appears with a different spelling or a subdomain prefix, edit it to match the blocked request exactly.

## Check the blocking list still wins
A domain can be allowed and still blocked if a broader block list entry or regex covers it. In Query Log, find the blocked query and note the blocking list shown. If a regex or wildcard entry matches, the allow entry for the exact domain may not override it. Temporarily disable that one block list, then retest. Do not disable all lists at once.

## Clear the local cache and retest
On the Pi-hole host, run pihole restartdns to reload settings and clear the DNS cache. On the client, flush its DNS cache or reconnect to Wi-Fi. Then run a verification test: open a terminal and run nslookup the-domain.com. Result interpretation: a Pi-hole address in the answer means the domain is no longer blocked; a block page or refused response means the entry still is not taking effect.

## Rollback note
If you disabled a block list during testing, re-enable it after the test. If the allow entry caused unexpected behavior, remove it from Domains and restart DNS again.

If the **pi-hole whitelist domain** entry is correct and the query still fails, stop changing settings and gather the Query Log line.

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with that detail for a controlled diagnosis.
