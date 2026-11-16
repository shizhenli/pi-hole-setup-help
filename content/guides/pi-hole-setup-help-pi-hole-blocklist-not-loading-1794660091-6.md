---
title: "Pi-hole Blocklist Not Loading: When a Whitelist Entry Still Blocks a Domain"
description: "Troubleshoot a Pi-hole whitelist entry that fails to unblock a domain. Check list loading, exact match, regex, and client cache before changing settings."
seo_title: "Pi-hole Blocklist Not Loading: Whitelist Entry Still Blocks Domain"
seo_description: "Troubleshoot a Pi-hole whitelist entry that fails to unblock a domain. Check list loading, exact match, regex, and client cache before changing settings."
keywords:
  - "pi-hole blocklist not loading"
date: 2026-11-16T11:04:56+08:00
publishDate: 2026-11-16T11:04:56+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a domain to the allow list, but Pi-hole still blocks it, and you want to fix that before changing anything else.

## Confirm the blocklist actually loaded

Open the Pi-hole admin page and check the gravity database status. If the last update failed or lists show zero domains, the blocklist is not loading and the allow entry may be applied against incomplete data. Run `pihole -g` from the terminal and read the summary. A successful run reports domains imported and no failed list downloads. If lists fail, fix that first; otherwise the whitelist test is unreliable.

## Check exact match and regex

In the allow list, confirm the entry matches the blocked domain exactly, including subdomain form. If the block comes from a regex deny rule, a plain allow entry will not override it. Check the query log for the blocked domain and note the list or regex that matched. Add a regex allow rule only after you identify that exact pattern.

## Flush the client cache

On the client device, flush DNS and retry. On Windows use `ipconfig /flushdns`; on macOS use `sudo dscacheutil -flushcache`. Then load the domain again. If it now resolves, the issue was stale client cache, not the allow entry.

## Verification test

Run `nslookup example.com 192.168.1.10` using your Pi-hole address. A reply with the Pi-hole IP or a valid answer means the allow entry is working. A blocked response means the entry is still not applied or a regex rule is winning.

## Rollback note

If you added a regex allow rule during testing, remove it and restart the DNS resolver with `pihole restartdns` to return to the previous state. If the whitelist entry does not unblock a domain after these checks, and the **pi-hole blocklist not loading** symptom persists,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your query log excerpt.
