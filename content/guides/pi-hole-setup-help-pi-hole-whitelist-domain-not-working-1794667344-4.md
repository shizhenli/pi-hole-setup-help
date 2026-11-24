---
title: "Whitelist Entry Still Blocked: Check Domain Format and List Order"
description: "A domain on Pi-hole's allow list is still blocked. Check domain format, list precedence, and cache, then verify with a controlled query."
seo_title: "Pi-hole Whitelist Domain Not Working: Fix a Blocked Allow Entry"
seo_description: "A domain on Pi-hole's allow list is still blocked. Check domain format, list precedence, and cache, then verify with a controlled query."
keywords:
  - "pi-hole whitelist domain not working"
date: 2026-11-24T09:05:06+08:00
publishDate: 2026-11-24T09:05:06+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a domain to Pi-hole's allow list, but it still resolves as blocked on your home LAN.

## Check the entry format first
Open your Pi-hole admin page and go to Domains. Find your allow entry and confirm it matches the exact domain being blocked. An entry for example.com does not automatically cover www.example.com or sub.example.com. If the blocked query is a subdomain, add that exact name. Remove any leading http://, trailing slash, or path. A wildcard is not supported in the standard allow list, so do not rely on one.

## Confirm which list wins
If the same domain also appears on a blocklist, the block can take precedence depending on your Pi-hole version and list configuration. Search your blocklists for the domain and remove it from the block side. Then run a gravity update so the change is applied. This is the most common reason a **pi-hole whitelist domain not working** report turns out to be a list conflict rather than a broken allow entry.

## Clear the cache and test
Restart the Pi-hole DNS service or flush the cache from Settings. On a client, run nslookup example.com against the Pi-hole IP. A reply of 0.0.0.0 or the Pi-hole blocking address means the domain is still blocked. A normal public IP means the allow entry is now effective. If the client uses router-provided DNS, confirm the router hands out only the Pi-hole address.

## Rollback note
If your change makes things worse, remove the allow entry and restore the previous blocklist state before retesting. Keep a note of what you altered so you can reverse it cleanly.

If the query still returns a blocked address after these checks,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your query log output.
