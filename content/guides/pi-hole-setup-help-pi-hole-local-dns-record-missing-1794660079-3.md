---
title: "Pi-hole Local DNS Record Missing: Fix a Hostname That Resolves Wrong or Not at All"
description: "A local hostname resolves to the wrong address or not at all. Follow these checks to restore the missing Pi-hole local DNS record and verify resolution."
seo_title: "Pi-hole Local DNS Record Missing: Fix Wrong or Missing Hostname"
seo_description: "A local hostname resolves to the wrong address or not at all. Follow these checks to restore the missing Pi-hole local DNS record and verify resolution."
keywords:
  - "pi-hole local dns record missing"
date: 2026-11-16T08:04:53+08:00
publishDate: 2026-11-16T08:04:53+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a local hostname in Pi-hole, but it still resolves to the wrong address or not at all on your home LAN. Before changing anything else, confirm whether the record is actually saved and being served.

## Check the Local DNS Record

Open the Pi-hole admin interface and go to Local DNS > DNS Records. Find the hostname you expect. If it is absent, that is your **pi-hole local dns record missing** condition. Re-add the hostname and its intended IP address, then save. If it is present, compare the IP address character by character with the device's current address; a stale address produces the wrong result.

## Confirm Pi-hole Is Answering

From a client machine, run `nslookup yourhostname 192.168.1.10`, replacing the address with your Pi-hole IP. If the answer matches the record, the record is being served. If you get NXDOMAIN, the record is not loaded or the client is asking a different DNS server. Check the client's DNS settings and your router's DHCP DNS handout.

## Verification Test

Flush the client DNS cache, then resolve the hostname again. A correct reply should show the intended LAN address. Also test from a second device to rule out one stale cache.

## Rollback Note

If a change makes resolution worse, remove the record you just added and restore the previous entry. Keep a note of the original IP so you can revert cleanly.

If these checks do not restore the expected answer,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your record details and test output.
