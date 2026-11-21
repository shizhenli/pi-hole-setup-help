---
title: "Pi-hole Allow List Entry Not Unblocking a Domain: Controlled Diagnosis"
description: "A domain you added to the Pi-hole allow list is still blocked. Follow these controlled checks to confirm whether the entry was saved, matched, or overridden before changing anything else."
seo_title: "Pi-hole Whitelist Domain Not Working: Fix an Allow Entry That Still Blocks"
seo_description: "A domain you added to the Pi-hole allow list is still blocked. Follow these controlled checks to confirm whether the entry was saved, matched, or overridden before changing anything else."
keywords:
  - "pi-hole whitelist domain"
date: 2026-11-21T15:36:09+08:00
publishDate: 2026-11-21T15:36:09+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a domain to the allow list, but Pi-hole still blocks it, and you want to diagnose the cause before making more changes.

## Confirm the entry was saved and matched

Open the Pi-hole admin page and go to Domains. Find your entry and check three things: the domain is spelled exactly as the blocked query (no `www.` mismatch), the type is Allow, and it is enabled. If the entry is missing, it was not saved. If it is present but disabled, enable it. Next, open Query Log and filter for that domain. Look at the status column. If the query still shows Blocked (gravity), the allow rule is not matching the exact name being requested. Add the exact hostname shown in the log, not a parent domain, unless you intend to allow the whole subtree.

## Check for a conflicting block rule

In Domains, search the same name. A deny or regex entry can override an allow entry depending on evaluation order. If you find a conflicting block, remove or disable it, then flush the cache with `pihole flush` from the Pi-hole console. Also confirm the client is actually using Pi-hole: on the affected device, run `nslookup example.com` and check the server address in the answer. If it returns your router, the device is bypassing Pi-hole and the allow entry cannot help.

## Verification test

From the affected device, query the domain again and check Query Log. A correct fix shows the query as Allowed or OK, and the page loads. If it still shows Blocked, note the exact status and the client IP.

## Rollback note

If a change makes things worse, disable the new entry and run `pihole restartdns` to return to the previous state. Keep a note of what you changed so you can reverse it cleanly. For a second opinion on your **pi-hole whitelist domain** entry,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
