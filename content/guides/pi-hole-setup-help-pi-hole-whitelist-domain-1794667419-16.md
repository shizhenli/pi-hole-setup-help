---
title: "Pi-hole Whitelist Domain Still Blocked: Verify the Entry Before You Change Anything"
description: "A domain on your Pi-hole allow list is still blocked. Run these checks to confirm the entry, the query path, and the blocklist before changing settings."
seo_title: "Pi-hole Whitelist Domain Not Working: Fix an Allow Entry That Still Blocks"
seo_description: "A domain on your Pi-hole allow list is still blocked. Run these checks to confirm the entry, the query path, and the blocklist before changing settings."
keywords:
  - "pi-hole whitelist domain"
date: 2026-11-26T09:04:11+08:00
publishDate: 2026-11-26T09:04:11+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a domain to the allow list, but the site is still blocked, and you want to confirm the entry is actually the problem before touching anything else.

## Confirm the Entry Is Saved and Exact

Open your Pi-hole admin page and go to Domains under the allow/deny lists. Find your entry and check three things: it sits in the allow column, it is saved, and it matches the queried name exactly. A whitelist entry for example.com does not cover www.example.com or a subdomain. If the blocked query is a subdomain, add that exact name. Then check the Query Log for the blocked request and note the exact domain shown there.

## Check Which List Is Blocking It

In the Query Log, click the blocked query and read the status and the list that matched. If the block comes from a regex deny rule or a subscribed blocklist, a plain allow entry may not override it. Note the list name and the rule text. Do not disable lists yet; just record what matched so you know whether the allow entry can win.

## Run a Verification Test

From a client on your LAN, flush its DNS cache, then request the domain again. Watch the Query Log as you do. Expected result: the query appears as allowed and the client loads the site. If it still shows blocked, the allow entry is not matching the queried name or a higher-priority rule is winning.

## Roll Back Safely

If your changes made things worse, remove only the entries you added, save, and restart the DNS resolver from Settings. Your earlier blocklists remain untouched.

One clean way to confirm a **pi-hole whitelist domain** entry is working is to compare the Query Log before and after the test request. If the log still disagrees with your allow entry,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with the exact domain and the matched list name.
