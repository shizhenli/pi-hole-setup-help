---
title: "Pi-hole Breaks Internet: Isolate the DNS Failure Before Changing Settings"
description: "Clients lose name resolution after pointing DNS at Pi-hole. Learn to isolate the failure, run a verification test, and roll back safely."
seo_title: "Pi-hole Breaks Internet: Isolate DNS Failure First"
seo_description: "Clients lose name resolution after pointing DNS at Pi-hole. Learn to isolate the failure, run a verification test, and roll back safely."
keywords:
  - "pi-hole breaks internet"
date: 2026-11-17T09:15:11+08:00
publishDate: 2026-11-17T09:15:11+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole, pointed your clients at it, and now name resolution fails—so you need to isolate the failure before changing more settings.

## Confirm the failure is DNS, not routing

On a client that lost internet, open a terminal and run `ping 1.1.1.1`. If that succeeds but `ping example.com` fails, routing is fine and DNS is the problem. Next, run `nslookup example.com <Pi-hole-IP>`. If it times out, Pi-hole is not answering queries. If it returns an answer, the client is using a different resolver.

## Check Pi-hole's upstream path

On the Pi-hole host, run `dig example.com @127.0.0.1`. A timeout or SERVFAIL means Pi-hole cannot reach its upstream DNS. Confirm the upstream servers in Settings > DNS are reachable from the host with `dig example.com @<upstream-IP>`. If that also fails, the host's own network or firewall is blocking outbound DNS—fix that before touching client settings. This is the most common reason **pi-hole breaks internet** for every client at once.

## Verify resolution end to end

From the client, run `nslookup example.com <Pi-hole-IP>` again. A valid answer with the Pi-hole address as server confirms the path works. Then load a known site in a browser. If it resolves but pages stall, the issue is no longer DNS.

## Roll back if clients remain offline

If clients still cannot resolve, temporarily set their DNS back to the router or a public resolver so the household has internet. Keep Pi-hole running for diagnosis, then compare its query log against a working resolver before re-pointing clients. For a guided review of your exact setup,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
