---
title: "Pi-hole Local DNS Record Missing: Fix a Hostname That Resolves Wrong"
description: "A local hostname resolves to the wrong address or not at all in Pi-hole. Follow these checks to confirm a missing local DNS record and restore correct resolution."
seo_title: "Pi-hole Local DNS Record Missing: Fix Wrong Hostname Resolution"
seo_description: "A local hostname resolves to the wrong address or not at all in Pi-hole. Follow these checks to confirm a missing local DNS record and restore correct resolution."
keywords:
  - "pi-hole local dns record missing"
date: 2026-11-22T08:05:09+08:00
publishDate: 2026-11-22T08:05:09+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

A home user finds that a LAN hostname resolves to the wrong address or not at all after setting up Pi-hole.

## Confirm the record is actually missing

Open the Pi-hole admin page and go to Local DNS Records. Look for the exact hostname and the expected IP address. If the hostname is absent, or the IP shown differs from what the device currently uses, you have found the **pi-hole local dns record missing** condition. Note the exact spelling, because a typo creates a separate entry that never matches.

## Check the client and the router

On the affected device, run `nslookup hostname` and read the answer. If it returns the router's address or an old IP, the device may still be using cached or router-provided DNS. Run `ipconfig /flushdns` on Windows or `sudo resolvectl flush-caches` on Linux, then repeat the lookup. If the answer is now correct, the record was fine and only the cache was stale. If the answer is still wrong, continue.

Confirm the device's IP has not changed. A DHCP lease renewal can move a host to a new address while the local record keeps the old one. Compare the device's current IP with the entry in Local DNS Records.

## Restore the record

Add or correct the entry in Local DNS Records with the exact hostname and current IP, then save. Restart the DNS resolver with `pihole restartdns` so the change loads.

## Verification test

From a client, run `nslookup hostname` again and confirm it returns the expected address. Then open the hostname in a browser.

## Rollback note

If resolution breaks after your edit, delete the new entry and re-add the previous hostname and IP exactly as they were, then restart DNS again. If the hostname still will not resolve correctly,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
