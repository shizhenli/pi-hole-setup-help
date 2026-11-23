---
title: "Pi-hole Device Not Using DNS: Fix a Single Client That Still Bypasses Pi-hole"
description: "One device still uses another DNS server and shows no Pi-hole queries? Follow these checks to fix a single client bypassing Pi-hole on your home LAN."
seo_title: "Pi-hole Device Not Using DNS: Fix One Client Bypassing Pi-hole"
seo_description: "One device still uses another DNS server and shows no Pi-hole queries? Follow these checks to fix a single client bypassing Pi-hole on your home LAN."
keywords:
  - "pi-hole device not using dns"
date: 2026-11-23T09:05:00+08:00
publishDate: 2026-11-23T09:05:00+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You have Pi-hole running on your home LAN with router-provided DNS, but one device still uses another DNS server and shows no Pi-hole queries. This is a common **pi-hole device not using dns** issue, and it usually comes down to a per-device setting or a stale lease rather than a broken Pi-hole.

## Check the device's DNS configuration

Start on the affected device, not the Pi-hole. Open its network settings and look for the DNS server field. If it shows your router's IP, a public resolver like 8.8.8.8, or an automatic/private DNS option, that device is bypassing Pi-hole. The fix is to set DNS manually to your Pi-hole's IP address, or to disable private DNS if the device offers it. If the field is greyed out, the device is likely receiving DNS from DHCP and ignoring your router's handout.

## Check the DHCP lease

On your router, find the DHCP client list and locate the device. If the lease still shows an old DNS server, release and renew the lease on the device, or reboot it. A renewed lease should show the Pi-hole IP as DNS. If it does not, the router is still handing out its own DNS; correct that before retesting.

## Verification test

On the device, visit a site you know is blocked by Pi-hole. If it is blocked, DNS is now flowing through Pi-hole. Confirm by opening the Pi-hole query log and filtering by the device's IP; you should see new queries appear within a minute.

## Rollback note

If the device loses internet after the change, set its DNS back to automatic, renew the lease, and confirm connectivity before trying again. For guided help,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult).
