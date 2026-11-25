---
title: "Pi-hole Blocklist Not Loading: When a Whitelist Entry Does Not Unblock a Domain"
description: "A whitelist entry still blocks a domain? Follow these checks to diagnose a Pi-hole blocklist not loading issue and restore expected allow-list behavior."
seo_title: "Pi-hole Blocklist Not Loading: Whitelist Entry Not Unblocking Domain"
seo_description: "A whitelist entry still blocks a domain? Follow these checks to diagnose a Pi-hole blocklist not loading issue and restore expected allow-list behavior."
keywords:
  - "pi-hole blocklist not loading"
date: 2026-11-25T08:05:06+08:00
publishDate: 2026-11-25T08:05:06+08:00
draft: false
categories:
  - "DNS Filtering"
category_slug: "dns-filtering"
---

You added a domain to the allow list, but it is still blocked, and you are stuck at the whitelist entry does not unblock a domain step before completing a controlled diagnosis.

## Check the Blocklist and Gravity Database

First, confirm the blocklist is actually loading. Run `pihole -g` in a terminal. A successful update ends with a summary showing domains processed and no errors. If you see errors like "database is locked" or "unable to update gravity," the blocklist is not loading correctly, which can cause stale blocking rules to persist. Check `/etc/pihole/gravity.db` exists and is readable. If it is missing or corrupt, Pi-hole may fall back to an older list, so your whitelist entry might not be applied against the current blocklist.

## Verify the Whitelist Entry Is Active

Open the Pi-hole admin interface and go to Domains. Confirm your domain appears under the allow list and is enabled. Then run `pihole -q yourdomain.com` to see which blocklist contains it. If the output shows the domain is still on a blocklist, the allow entry is not overriding it. This often happens when the blocklist is not loading properly, so the allow list cannot take effect. Restart the DNS resolver with `pihole restartdns` and test again.

## Verification Test

From a client device, run `nslookup yourdomain.com` against the Pi-hole IP. A successful unblock returns the correct IP address, not 0.0.0.0 or a blocking page. If it still resolves to a block page, the issue is not resolved.

## Rollback and Recovery

If changes make things worse, restore the previous `gravity.db` from backup or run `pihole -g` again to rebuild. If the **pi-hole blocklist not loading** problem persists, do not keep editing lists blindly.

![A laptop showing Pi-hole query logs and DNS settings](/images/service-detail-960.webp "Pi-hole query and DNS rollback check")

[Ask for help](/#consult) to review your configuration safely.
