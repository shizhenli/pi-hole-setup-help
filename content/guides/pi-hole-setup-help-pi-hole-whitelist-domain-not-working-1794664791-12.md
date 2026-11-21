---
title: "Whitelist Entry Not Unblocking a Domain: Step-by-Step Fix"
description: "A domain added to Pi-hole's allow list is still blocked. Follow these practical checks to diagnose why the whitelist entry is not unblocking the domain."
seo_title: "Pi-hole Whitelist Domain Not Working: Diagnose and Fix"
seo_description: "A domain added to Pi-hole's allow list is still blocked. Follow these practical checks to diagnose why the whitelist entry is not unblocking the domain."
keywords:
  - "pi-hole whitelist domain not working"
date: 2026-11-21T15:39:16+08:00
publishDate: 2026-11-21T15:39:16+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a domain to Pi-hole's allow list, but it is still being blocked on your home LAN with router-provided DNS.

## Check the exact domain format
Open Pi-hole admin, go to Domains, and confirm the entry matches the blocked domain exactly. If the block is for a subdomain such as ads.example.com, adding example.com may not override it. Add the specific subdomain as a separate allow entry. Also check for typos, trailing spaces, or a wildcard character that Pi-hole does not interpret as expected.

## Confirm the block source
Go to Query Log, filter for the domain, and look at the status column. If it shows Blocked (gravity), the domain is still on a blocklist. If it shows Blocked (regex), a regex rule is overriding your allow entry. Allow entries do not automatically override regex blocks. In that case, disable the matching regex rule temporarily or add a more specific allow rule that matches the regex pattern.

## Verify client and DNS path
On the affected client, run nslookup example.com and confirm the server address is your Pi-hole IP, not the router. If the router is handing out its own DNS, the client may bypass Pi-hole entirely. Check that Pi-hole is the only DNS server in your router's DHCP settings.

## Verification test
After making changes, flush the client DNS cache and reload the domain. In Query Log, the domain should now show Allowed or OK, not Blocked. If it still shows Blocked, note the exact status and list name.

## Rollback note
If a change makes things worse, remove the new allow entry and re-enable any regex rule you disabled. Restore the previous state before trying another approach.

If the status remains unclear,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your Query Log screenshot and domain details. This **pi-hole whitelist domain not working** issue is usually resolved by matching the exact blocked domain and checking regex overrides.
