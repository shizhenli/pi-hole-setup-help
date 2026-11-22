---
title: "Pi-hole Not Blocking Ads: Check the Gravity Database First"
description: "Queries resolve but known ad domains are no longer blocked? Follow this focused check of the Pi-hole gravity database to restore ad blocking."
seo_title: "Pi-hole Not Blocking Ads: Fix Gravity Database Checks"
seo_description: "Queries resolve but known ad domains are no longer blocked? Follow this focused check of the Pi-hole gravity database to restore ad blocking."
keywords:
  - "pi-hole not blocking ads"
date: 2026-11-22T12:05:26+08:00
publishDate: 2026-11-22T12:05:26+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

## Troubleshooting

You are on a home LAN with router-provided DNS, and Pi-hole resolves queries but no longer blocks known ad domains.  
Before changing DNS settings, confirm the blocking lists are actually loaded. Open the Pi-hole admin console and go to Tools > Update Gravity. Run the update and read the final status line. If it shows a completed update with a nonzero domain count, the list is loaded. If it shows a failure, a zero count, or a warning about an empty database, stop here and repair gravity before anything else.  
Next, go to Settings > DNS and confirm at least one upstream resolver is selected. Then use Query Logs to test a known ad domain. From a LAN client, run `nslookup doubleclick.net` against the Pi-hole IP. A blocked result should return 0.0.0.0 or the Pi-hole blocking address. If it returns a public IP, the query is bypassing the block list.  
Also check Settings > Blocking and confirm blocking is enabled, not paused. If it is paused, resume it and repeat the lookup.  
To verify the fix, run the same lookup again and confirm the blocked address is returned. Then open a page that normally shows ads and confirm the ad domains appear as blocked in Query Logs.  
If you changed gravity or blocking settings and the result is worse, restore the previous gravity list from a backup or re-enable the last known working list before retesting. If the lookup still returns a public IP after these checks, the issue is likely outside the gravity database, and this is where **pi-hole not blocking ads** becomes a routing or client-DNS question.  
For a controlled next step,

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) with your Query Log output and gravity status.
