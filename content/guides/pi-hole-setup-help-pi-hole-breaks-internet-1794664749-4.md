---
title: "Pi-hole Breaks Internet: Router DNS and DHCP Conflict Fix"
description: "Clients lose name resolution after pointing DNS at Pi-hole. Learn to check router DNS settings, DHCP handout, and Pi-hole upstreams, then restore internet with a rollback."
seo_title: "Pi-hole Breaks Internet: Fix Router DNS & DHCP Conflict"
seo_description: "Clients lose name resolution after pointing DNS at Pi-hole. Learn to check router DNS settings, DHCP handout, and Pi-hole upstreams, then restore internet with a rollback."
keywords:
  - "pi-hole breaks internet"
date: 2026-11-21T15:36:09+08:00
publishDate: 2026-11-21T15:36:09+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole and pointed your clients' DNS at it, but now they cannot resolve names and the internet appears broken.

## Check the router's DNS and DHCP handoff

On your router's admin page, look at the DHCP settings. If the router still hands out its own address as DNS, clients may bypass Pi-hole or get conflicting answers. Set the DHCP DNS option to the Pi-hole IP only, then renew a client lease. On the Pi-hole device, confirm its own DNS is not set to itself. Run `cat /etc/resolv.conf`; if it lists 127.0.0.1, change the Pi-hole host to use your router or a public resolver. This is a common reason **pi-hole breaks internet** for every client at once.

## Verify Pi-hole upstream resolution

From the Pi-hole terminal, run `dig google.com @127.0.0.1`. A NOERROR answer with an A record means Pi-hole can resolve. If it times out, check Settings > DNS in the Pi-hole admin UI and enable at least one upstream server, then save. Next, from a client, run `nslookup google.com <Pi-hole-IP>`. If that works but normal browsing fails, the client is still using the wrong DNS server; release and renew its DHCP lease.

## Rollback if clients stay offline

If you need immediate recovery, set the router DHCP DNS back to the router's address and renew client leases. Internet returns while you diagnose. Do not leave Pi-hole as the sole DNS until the upstream test passes.

## Verification test

After fixing DHCP and upstreams, load a fresh site on two clients. Both should resolve and browse normally. If one fails, its lease is stale.

If the checks do not isolate the cause,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
