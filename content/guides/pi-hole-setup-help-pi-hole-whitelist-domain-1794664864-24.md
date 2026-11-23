---
title: "Pi-hole Whitelist Domain Still Blocked: One Controlled Diagnosis"
description: "A domain added to the Pi-hole allow list is still blocked. Run one controlled diagnosis to confirm whether the whitelist entry, DNS path, or cache is responsible, then restore the expected outcome."
seo_title: "Pi-hole Whitelist Domain Not Working: Fix an Allow Entry That Still Blocks"
seo_description: "A domain added to the Pi-hole allow list is still blocked. Run one controlled diagnosis to confirm whether the whitelist entry, DNS path, or cache is responsible, then restore the expected outcome."
keywords:
  - "pi-hole whitelist domain"
date: 2026-11-23T10:05:04+08:00
publishDate: 2026-11-23T10:05:04+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a domain to the allow list, but it is still blocked, and you want to fix that before changing anything else. Here is a controlled way to diagnose the **pi-hole whitelist domain** entry without guessing.

## Confirm the entry is exact and active

Open your Pi-hole admin page and go to Domains. Check the allow entry character by character: no leading www unless the blocked query includes it, no trailing slash, no path. A path like example.com/page will never match a DNS query. Confirm the entry is enabled and saved. Then run this from a client on the same LAN:

`nslookup example.com 192.168.1.10`

Replace the IP with your Pi-hole address. If the reply shows 0.0.0.0 or your Pi-hole address, the block is still active. If it resolves normally, the entry is working and the browser or app cache is the remaining suspect.

## Check the actual blocked query

In Query Log, filter for the domain and note the exact name and status. If the blocked name differs from your entry, such as a subdomain, add that exact name. If the status is already allowed but the client still fails, the client is using a different resolver. On a router-provided DNS setup, confirm the router hands out only the Pi-hole address, not a secondary public DNS server.

## Verification test

Flush the client DNS cache, reopen the site in a private window, and query again. A normal answer with no block status confirms the fix. If it still fails, compare the Query Log timestamp with your test to see whether the request reached Pi-hole at all.

## Rollback and recovery

If your changes make resolution worse, disable the new allow entry, restart the DNS resolver from Settings, and retest a known-good domain. Keep a note of the exact entry you added so you can remove it cleanly.

If the query never appears in the log, stop changing allow entries and

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with the domain, client IP, and timestamps.
