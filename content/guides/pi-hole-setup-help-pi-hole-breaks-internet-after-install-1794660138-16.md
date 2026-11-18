---
title: "Pi-hole Breaks Internet After Install: Restore DNS Resolution"
description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to isolate the upstream DNS failure and restore internet access."
seo_title: "Pi-hole Breaks Internet After Install: Fix DNS Resolution"
seo_description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to isolate the upstream DNS failure and restore internet access."
keywords:
  - "pi-hole breaks internet after install"
date: 2026-11-18T11:12:07+08:00
publishDate: 2026-11-18T11:12:07+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole, pointed your clients at it, and now name resolution fails across the LAN.

## Confirm the failure point

On a client, run `nslookup example.com` and note the exact response. A timeout or "server can't find" means the client reached Pi-hole but Pi-hole could not resolve upstream. If the query returns immediately with a refusal, the client is likely still using the router's DNS, not Pi-hole.

Next, log into the Pi-hole admin console and open Query Log. Query a domain from the client again. If no entry appears, the client is not using Pi-hole. If an entry appears with status "forwarded" but no answer, Pi-hole's upstream resolver is unreachable.

## Check upstream DNS on the Pi-hole host

SSH into the Pi-hole device and run `dig @127.0.0.1 example.com`. If this fails, Pi-hole itself has no working upstream. Open Settings > DNS in the admin console and confirm at least one upstream server is enabled and reachable from the Pi-hole host. Test it directly with `dig @<upstream-ip> example.com`. A timeout here means the Pi-hole host cannot reach that resolver, often because the router blocks it or the host has no route.

## Verification test

After correcting the upstream entry, run `dig @127.0.0.1 example.com` on the Pi-hole host, then `nslookup example.com` on the client. Both should return an answer. If the host resolves but the client does not, the client is still bypassing Pi-hole.

## Rollback and recovery

If you cannot restore resolution quickly, set the client's DNS back to the router address to regain internet access, then continue diagnosis. This is why **pi-hole breaks internet after install** is usually an upstream DNS problem, not a Pi-hole defect.

If these checks do not isolate the cause,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
