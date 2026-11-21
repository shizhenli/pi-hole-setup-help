---
title: "Pi-hole DNS Resolution Failed: Diagnose Before You Panic"
description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to confirm whether Pi-hole is forwarding queries or the router is still interfering."
seo_title: "Pi-hole DNS Resolution Failed: Step-by-Step Diagnosis"
seo_description: "Clients lose name resolution after pointing DNS at Pi-hole. Follow these checks to confirm whether Pi-hole is forwarding queries or the router is still interfering."
keywords:
  - "pi-hole dns resolution failed"
date: 2026-11-21T15:36:09+08:00
publishDate: 2026-11-21T15:36:09+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole, pointed your router's DNS at it, and now every device on the LAN has lost name resolution.

## Confirm Pi-hole is answering locally

From a client, run `nslookup pi.hole` and then `nslookup google.com`. If the first resolves but the second times out, Pi-hole is reachable but not forwarding upstream. If both fail, the client is not reaching Pi-hole at all.

## Check upstream DNS inside Pi-hole

Open the Pi-hole admin interface and go to Settings > DNS. Confirm at least one upstream resolver is selected and enabled. If the list is empty, Pi-hole has nowhere to send queries, which produces a **pi-hole dns resolution failed** condition for every client. Select a public resolver, save, and restart the DNS service with `pihole restartdns`.

## Verify the router is not still handing out its own DNS

On a client, run `nslookup pi.hole` again and check the server address it reports. If it shows the router's IP instead of Pi-hole's, DHCP is still advertising the router as DNS. Either set the router's DHCP DNS option to Pi-hole's IP only, or disable router DNS and let Pi-hole handle DHCP.

## Run the verification test

From two different clients, run `nslookup example.com` and confirm both return an address. Then open a browser and load a site. If both clients resolve, the fix is holding.

## Rollback if you need internet back now

Point the router's DNS back to its previous setting and renew DHCP leases on clients. Internet returns immediately; you can retry Pi-hole later with a controlled test.

If the checks above do not isolate the failure,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your Pi-hole DNS settings and the nslookup output.
