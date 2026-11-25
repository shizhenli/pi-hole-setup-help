---
title: "Pi-hole Breaks Internet: Router DNS Override After Installation"
description: "Clients lose name resolution after pointing DNS at Pi-hole because the router still hands out its own DNS. Learn how to check and fix this override."
seo_title: "Pi-hole Breaks Internet: Fix Router DNS Override"
seo_description: "Clients lose name resolution after pointing DNS at Pi-hole because the router still hands out its own DNS. Learn how to check and fix this override."
keywords:
  - "pi-hole breaks internet"
date: 2026-11-25T10:05:00+08:00
publishDate: 2026-11-25T10:05:00+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You installed Pi-hole and pointed your clients' DNS to it, but now every device on the LAN has lost name resolution. The most common cause is that your router is still advertising its own DNS server via DHCP, so clients keep using the router instead of Pi-hole. Here is how to confirm and fix that specific override.

## Check what DNS your clients actually receive

On a client that lost internet, open a terminal and run `nslookup example.com`. If it returns the router's IP as the server, the client is not using Pi-hole. Next, check the router's DHCP settings. Look for a DNS server field that is set to the router's own address or to an ISP resolver. If that field is populated with anything other than your Pi-hole IP, the router is overriding your manual DNS change.

## Fix the DHCP DNS handout

In the router's DHCP configuration, set the primary DNS server to your Pi-hole's static IP and leave the secondary blank if the router allows it. Save and renew the DHCP lease on a test client. Then run `nslookup example.com` again. A correct result shows the Pi-hole IP as the server and returns a valid address. If the lookup still shows the router, the router may be forcing its own DNS regardless of DHCP settings; in that case, disable the router's DNS relay or set Pi-hole as the upstream DNS in the router itself.

## Verify and recover

After the change, load a website on the test client. If it resolves, the override is fixed. If not, temporarily set the client's DNS back to automatic to restore internet, then recheck the router's DHCP page. This is also your rollback step if the fix disrupts other devices. When the cause is unclear,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your router model and the nslookup output. The key lesson is that **pi-hole breaks internet** only when the router keeps handing out its own DNS, so always verify the DHCP handout before changing Pi-hole settings.
