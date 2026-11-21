---
title: "Pi-hole Local DNS Record Missing: Restore a Hostname That Resolves Wrong"
description: "A local hostname resolves to the wrong address or not at all. Follow these checks to confirm whether the pi-hole local dns record missing is the cause and restore correct resolution."
seo_title: "Pi-hole Local DNS Record Missing: Fix Wrong Hostname Resolution"
seo_description: "A local hostname resolves to the wrong address or not at all. Follow these checks to confirm whether the pi-hole local dns record missing is the cause and restore correct resolution."
keywords:
  - "pi-hole local dns record missing"
date: 2026-11-21T15:32:40+08:00
publishDate: 2026-11-21T15:32:40+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a local hostname in Pi-hole, but a device on your LAN now resolves it to the wrong address or not at all. This usually means the **pi-hole local dns record missing** from the active configuration, or another DNS source is answering first.

## Check the Local DNS Record

Open the Pi-hole admin page and go to Local DNS > DNS Records. Look for the exact hostname and IP pair you expect. If the entry is absent, that is your first confirmation. If it is present, note the IP: a typo here explains a wrong-address result immediately. Also check Local DNS > CNAME Records if the name is an alias; a missing target record makes the alias fail.

## Confirm Which Server Answers

From the affected client, run `nslookup yourhostname` (or `dig yourhostname`). Read the Server line. If it shows your router instead of the Pi-hole address, the client is bypassing Pi-hole and the record cannot apply. If it shows Pi-hole but returns the wrong IP, re-check the record value. If it returns NXDOMAIN, the record is genuinely missing or disabled.

## Re-add and Verify

Recreate the record with the exact hostname and intended IP, save, and flush the Pi-hole cache with `pihole restartdns`. Then run the same lookup again. Success means the answer comes from Pi-hole and matches the intended address. Test from a second device to rule out a stale local cache.

## Rollback Note

If the new record causes conflicts, delete it and restart DNS again; the previous behavior returns. Keep a note of the old value before editing.

If the lookup still bypasses Pi-hole after these checks,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your lookup output and record screenshot.
