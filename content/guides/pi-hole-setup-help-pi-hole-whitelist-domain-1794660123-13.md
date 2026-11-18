---
title: "Pi-hole Whitelist Domain Entry Not Unblocking: Check Order and Lists"
description: "A whitelisted domain still blocked in Pi-hole? Learn how to check list precedence, exact domain matching, and verify with a controlled query."
seo_title: "Pi-hole Whitelist Domain Not Working: Why an Allow Entry Still Blocks"
seo_description: "A whitelisted domain still blocked in Pi-hole? Learn how to check list precedence, exact domain matching, and verify with a controlled query."
keywords:
  - "pi-hole whitelist domain"
date: 2026-11-18T08:05:10+08:00
publishDate: 2026-11-18T08:05:10+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a domain to the allow list, but Pi-hole still blocks it on your home LAN with router-provided DNS.

## Check the exact domain string
Open the Pi-hole admin interface and go to Domains. Compare the blocked entry and your allow entry character by character. A whitelist entry for example.com does not unblock ads.example.com or www.example.com unless those exact names are also allowed. If the blocked query is a subdomain, add that subdomain exactly as it appears in the query log. Result interpretation: if the strings differ, the whitelist is not matching and the block remains expected.

## Confirm list precedence
Still in Domains, check whether the domain appears on a blocklist that has higher precedence in your configuration. In Pi-hole, exact allow entries normally override block entries, but regex blocks and wildcard blocks can still win depending on order. Review the regex filter list for patterns like (^|\.)example\.com$. If a regex block matches, your plain **pi-hole whitelist domain** entry may be ignored. Result interpretation: a matching regex block explains the continued block even with an allow entry present.

## Run a verification test
From a client on the LAN, run nslookup example.com or dig example.com against the Pi-hole IP. Then check Query Log for that client and domain. If the status shows blocked by regex or gravity, the allow entry did not take effect. If it shows allowed, the client may be caching the old answer; flush the client DNS cache and retry.

## Rollback and recovery
If you changed regex or list order while testing, revert those edits and reload the lists before retesting. Remove any temporary allow entries you added so the original state is restored. If the domain still blocks after exact matching and precedence checks,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your query log excerpt and list order.
