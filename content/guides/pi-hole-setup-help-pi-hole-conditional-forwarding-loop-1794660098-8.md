---
title: "Pi-hole Conditional Forwarding Loop: Fix Missing Local DNS Records"
description: "Troubleshoot a Pi-hole conditional forwarding loop that leaves local hostnames missing or wrong. Step-by-step checks, exact result interpretation, and a verification test."
seo_title: "Pi-hole Conditional Forwarding Loop: Fix Missing Local DNS Records"
seo_description: "Troubleshoot a Pi-hole conditional forwarding loop that leaves local hostnames missing or wrong. Step-by-step checks, exact result interpretation, and a verification test."
keywords:
  - "pi-hole conditional forwarding loop"
date: 2026-11-17T08:09:21+08:00
publishDate: 2026-11-17T08:09:21+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

## Understand the symptom

You set up Pi-hole on your home LAN with router-provided DNS, and now a local hostname like `nas.lan` resolves to the wrong address or not at all, and you suspect a **pi-hole conditional forwarding loop** is involved.

## Check conditional forwarding settings

Open Pi-hole admin, go to Settings > DNS, and look at Conditional Forwarding. If it is enabled, note the router IP and local domain. If the router IP points to Pi-hole itself, or the domain is blank, that is a loop or misconfiguration. Result interpretation: a router IP equal to Pi-hole's address means queries bounce back; a blank domain means local names are never forwarded correctly. Disable Conditional Forwarding temporarily and save.

## Check local DNS records

Go to Local DNS > DNS Records. Look for the hostname in question. If it is absent, that is your missing record. Add the correct IP and hostname, then save. If it exists but shows the wrong IP, edit it. Result interpretation: an absent entry explains a name that does not resolve; a wrong entry explains a name resolving to the wrong address.

## Check router DHCP/DNS handout

Confirm your router hands out Pi-hole as DNS. If the router also advertises itself as DNS, local names may resolve through the router instead. Result interpretation: if clients show the router as DNS, local records on Pi-hole are bypassed.

## Verification test

From a client, run `nslookup nas.lan` (or `dig nas.lan`). Expected result: the IP you entered in Local DNS. If it still fails, flush the client DNS cache and retry.

## Rollback and recovery

If changes make things worse, re-enable Conditional Forwarding with the correct router IP and domain, or remove the new local record. Keep a note of original settings before editing.

If the loop persists after these checks,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
