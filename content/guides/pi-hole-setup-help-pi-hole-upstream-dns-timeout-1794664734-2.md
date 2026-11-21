---
title: "Pi-hole Upstream DNS Timeout: Restore Internet After Installation"
description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to diagnose an upstream DNS timeout and restore internet access."
seo_title: "Fix Pi-hole Upstream DNS Timeout After Installation"
seo_description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to diagnose an upstream DNS timeout and restore internet access."
keywords:
  - "pi-hole upstream dns timeout"
date: 2026-11-21T15:32:40+08:00
publishDate: 2026-11-21T15:32:40+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole, pointed your clients at it, and now the internet breaks before you can run a controlled diagnosis.

## Confirm the failure is upstream

On a client, run `nslookup example.com` against the Pi-hole IP. If it times out, the Pi-hole is not answering. Next, open a terminal on the Pi-hole host and run `dig example.com @127.0.0.1`. A timeout here means Pi-hole cannot reach its upstream resolver. If it returns an answer, the problem is between the client and Pi-hole, not upstream.

## Check the upstream setting

In the Pi-hole admin interface, go to Settings > DNS. If the only upstream is the router's own DNS address, and the router hands out Pi-hole as DNS, you can create a loop or a dead end. Set a reliable public resolver such as 1.1.1.1 or 8.8.8.8 as upstream, save, and restart DNS. This is the most common cause of a **pi-hole upstream dns timeout** on a home LAN.

## Verify resolution

From the Pi-hole host, run `dig example.com @1.1.1.1` to confirm the upstream itself responds. Then run `dig example.com @127.0.0.1` again. A successful answer with a low query time means Pi-hole is forwarding correctly. Finally, test from one client with `nslookup example.com <pi-hole-ip>`.

## Roll back if needed

If clients still cannot resolve, temporarily set one client's DNS back to the router address to restore its internet. Keep the Pi-hole installed but do not force all clients to it until the upstream test passes. If the checks above do not isolate the timeout,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your dig output and DNS settings.
