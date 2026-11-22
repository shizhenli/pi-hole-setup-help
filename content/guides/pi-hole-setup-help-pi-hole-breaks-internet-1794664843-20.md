---
title: "Pi-hole Breaks Internet: A Controlled Diagnosis Before You Change Anything"
description: "Clients lose name resolution after pointing DNS at Pi-hole. Run these checks to confirm whether Pi-hole is the cause, test resolution, and roll back safely."
seo_title: "Pi-hole Breaks Internet: Diagnose DNS Loss Step by Step"
seo_description: "Clients lose name resolution after pointing DNS at Pi-hole. Run these checks to confirm whether Pi-hole is the cause, test resolution, and roll back safely."
keywords:
  - "pi-hole breaks internet"
date: 2026-11-22T11:05:14+08:00
publishDate: 2026-11-22T11:05:14+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole, pointed a client's DNS at it, and now that client cannot resolve names—so you need a controlled diagnosis before changing more settings.

## Confirm where resolution actually fails

On the affected client, open a terminal and run `nslookup example.com`. If it times out or returns no address, the client is not getting answers from Pi-hole. Next, run `nslookup example.com <Pi-hole-IP>` using the Pi-hole address explicitly. If that also fails, the problem is on the Pi-hole side; if it succeeds, the client's DNS setting or router handout is wrong.

On the Pi-hole host, run `pihole status`. You want to see DNS service enabled and listening on port 53. Then run `dig @127.0.0.1 example.com`. A NOERROR answer with an A record means Pi-hole can resolve locally. SERVFAIL or timeout means Pi-hole's upstream DNS is unreachable or misconfigured—check that upstream resolver in Settings > DNS and confirm the Pi-hole host itself has internet access.

## Verification test

From the client, run `nslookup example.com <Pi-hole-IP>` again after fixing upstream DNS. A returned address confirms resolution works through Pi-hole. Then test a blocked domain to confirm filtering still functions.

## Rollback and recovery

If Pi-hole still cannot resolve, restore the client's previous DNS (often the router) so the household regains internet immediately. Keep Pi-hole installed but stop handing out its address until the upstream issue is fixed. This is the fastest way to recover when **pi-hole breaks internet** for the whole LAN.

If these checks do not isolate the fault,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your `pihole status` and `dig` output.
