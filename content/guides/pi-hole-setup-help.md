---
title: Test Pi-hole on One Device and Restore the Original DNS
description: A controlled Pi-hole test that proves DNS filtering on one client and restores the original settings.
seo_title: Pi-hole Setup Test on One Device | Safe DNS Rollback
seo_description: A controlled Pi-hole test that proves DNS filtering on one client and restores the original settings.
keywords:
- pi-hole breaks internet
- pi-hole setup test
- restore dns after pi-hole
date: '2026-11-13'
draft: false
categories:
- DNS Filtering
category_slug: dns-filtering
---

<p>Pi-hole is safest when one client is tested first. Record the original DNS so a failed test is a two-minute rollback, not a household outage.</p>
<h2>Before you start</h2><ul>
<li>Pi-hole IP address and admin access</li>
<li>One test computer or phone</li>
<li>The current DNS setting and router access</li>
<li>A known ad domain and a normal website</li>
</ul>
<h2>Step-by-step checks</h2>
<h3>Step 1: Capture the current DNS</h3>
<p>Open the test device's network settings and record whether DNS is automatic or manual. If manual, copy every address; if automatic, note the router/DHCP source.</p>
<p><strong>Check:</strong> A screenshot or written copy of the original DNS exists.</p>
<h3>Step 2: Set only the test device</h3>
<p>Set the device's DNS to the Pi-hole address, then reconnect Wi-Fi or renew the lease. Do not change the router yet.</p>
<p><strong>Check:</strong> The device's active DNS shows the Pi-hole address.</p>
<h3>Step 3: Confirm the query path</h3>
<p>Visit a normal site and a known ad-test domain. Open Pi-hole's query log and find the test device's queries.</p>
<p><strong>Check:</strong> The Pi-hole log shows queries from the test device.</p>
<h3>Step 4: Test a normal browsing session</h3>
<p>Load several sites, check email or a video call, and note any false positives. Disable blocking for one domain only if needed and document why.</p>
<p><strong>Check:</strong> Normal browsing works and the intended block is visible.</p>
<h3>Step 5: Restore the original DNS</h3>
<p>Put back the recorded DNS or automatic setting, reconnect, and load the same sites. Confirm the device no longer depends on Pi-hole.</p>
<p><strong>Check:</strong> The original DNS is active and browsing works after rollback.</p>
<h2>Verify the result</h2>
<p>The device used Pi-hole for DNS, a known block appeared in the log, normal browsing worked, and the original DNS restored cleanly.</p>
<h2>Rollback</h2>
<p>Use the recorded DNS screenshot. If the router was changed, restore its previous DNS entries and renew client leases.</p>
<p><a href="/#consult">Ask for help with this exact symptom</a> · <a href="/guides/">All troubleshooting guides</a></p>
