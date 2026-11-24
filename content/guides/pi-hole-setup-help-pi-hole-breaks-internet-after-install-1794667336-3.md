---
title: "Clients Lose DNS After Pi-hole Install: Restore Name Resolution"
description: "Clients lose name resolution after pointing DNS to Pi-hole. Learn how to diagnose upstream DNS, router settings, and restore internet access."
seo_title: "pi-hole breaks internet after install: Fix DNS Loss"
seo_description: "Clients lose name resolution after pointing DNS to Pi-hole. Learn how to diagnose upstream DNS, router settings, and restore internet access."
keywords:
  - "pi-hole breaks internet after install"
date: 2026-11-24T08:05:06+08:00
publishDate: 2026-11-24T08:05:06+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole, pointed your clients to it, and now they cannot resolve domain names, leaving you without internet access.

## Check Pi-hole's Upstream DNS

First, verify Pi-hole can reach the internet. SSH into your Pi-hole or open a terminal and run:

`dig google.com @127.0.0.1`

If this times out or returns SERVFAIL, Pi-hole's upstream DNS is misconfigured. Go to Settings > DNS in the Pi-hole admin interface. Ensure at least one upstream DNS server is selected and reachable. If your router provides DNS, enter its LAN IP (e.g., 192.168.1.1) as the only upstream. Then restart the DNS resolver with `pihole restartdns`.

## Check Router DNS Handout

If Pi-hole resolves locally but clients still fail, your router may still be handing out its own DNS via DHCP. Log into your router and set the DHCP DNS server to Pi-hole's IP address only. Do not list a secondary DNS; clients may bypass Pi-hole and fail. Save and renew a client's DHCP lease.

## Verification Test

On a client, run `nslookup example.com`. A successful response with Pi-hole's IP as the server confirms resolution works. Then open a browser and load a site. If both succeed, the issue is resolved.

## Rollback if Needed

If clients remain offline, temporarily set the client's DNS back to the router's IP to restore internet. Then revisit Pi-hole's upstream settings. For guided help,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).

Remember: **pi-hole breaks internet after install** is usually an upstream DNS or DHCP handout problem, not a Pi-hole defect.
