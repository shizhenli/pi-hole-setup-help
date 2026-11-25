---
title: "Pi-hole Upstream DNS Timeout: Why Internet Breaks After Installation"
description: "Clients lose name resolution after pointing DNS at Pi-hole. Diagnose a Pi-hole upstream DNS timeout and restore internet with a controlled test."
seo_title: "Pi-hole Upstream DNS Timeout: Fix Internet After Install"
seo_description: "Clients lose name resolution after pointing DNS at Pi-hole. Diagnose a Pi-hole upstream DNS timeout and restore internet with a controlled test."
keywords:
  - "pi-hole upstream dns timeout"
date: 2026-11-25T09:05:02+08:00
publishDate: 2026-11-25T09:05:02+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole, pointed your clients at it, and now every name lookup fails while the LAN still works. That pattern usually means Pi-hole cannot reach its upstream resolver, so it has nothing to forward queries to. This is a **pi-hole upstream dns timeout** symptom, not a client problem.

## Check Pi-hole's upstream reachability

From the Pi-hole host, run `dig @127.0.0.1 google.com`. A timeout or SERVFAIL here confirms the forwarder path is broken. Next, check the upstream configured under Settings > DNS. If it is your router's LAN address, test it directly with `dig @192.168.1.1 google.com` (use your router IP). If that times out, the router is not answering DNS for the Pi-hole host, often because the router hands out itself as DNS and blocks LAN clients from querying it.

## Confirm the client path

On a client, run `nslookup google.com`. If it returns the Pi-hole address but no answer, the client is reaching Pi-hole and Pi-hole is failing upstream. If it returns a public resolver, DHCP is still overriding Pi-hole.

## Verification test

Set Pi-hole's upstream to a public resolver such as 1.1.1.1, save, then rerun `dig @127.0.0.1 google.com`. A fast NOERROR with an A record means upstream forwarding works. Then test one client with `nslookup google.com`; a valid answer confirms resolution is restored.

## Rollback

If clients still fail, point one device's DNS back to the router temporarily to restore internet while you finish diagnosis. Do not leave the whole LAN on a broken forwarder. For a guided review of your exact setup,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
