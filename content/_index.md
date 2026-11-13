---
title: Pi-hole Setup Help
seo_title: Pi-hole Setup Help | Test One Device and Restore DNS
seo_description: Remote Pi-hole help for DNS filtering, broken browsing, one-device testing, and clean DNS rollback.
keywords:
- pi-hole breaks internet
- pi-hole setup test
- restore dns after pi-hole
hero_kicker: NETWORK-WIDE DNS FILTERING
hero_h1: Pi-hole blocks too much or breaks browsing?
hero_lead: Test Pi-hole on one device, confirm normal browsing and blocking, then restore the original DNS settings cleanly.
hero_proof:
- One device, not the whole network
- Blocking and normal browsing both tested
- Original DNS restored and verified
hero_alt: Network dashboard on a monitor beside a small Pi-hole computer
hero_title: Pi-hole network dashboard and small computer
body_alt: A laptop showing Pi-hole query logs and DNS settings
body_title: Pi-hole query and DNS rollback check
body_caption: Keep the original DNS value written down before the first test query.
failures_kicker: THE THREE FAILURES WE SEE
failures_heading: What is going wrong?
test_kicker: HOW WE TEST
test_heading: Find the failing layer, then prove the fix.
scope_kicker: SERVICE SCOPE
scope_heading: What is included
scope_note: The session covers one agreed device and one verified path. Anything outside the written scope is quoted separately before work starts.
faq_kicker: FAQ
faq_heading: Before you book
consult_heading: Tell us what is happening.
consult_lead: Send the device, software, exact symptom, and what you have already tried. We will confirm scope and a fixed quote before booking.
form_placeholder: Device, software version, exact symptom, and what you have tried.
first_guide_url: /guides/pi-hole-setup-help/
symptoms:
- title: Pages will not load
  text: A device loses internet access or some domains time out after its DNS is pointed at Pi-hole.
- title: Ads still appear
  text: Pi-hole is reachable but the device bypasses it or uses an encrypted DNS resolver.
- title: The whole house goes offline
  text: A router-level DNS change affects every client before the Pi-hole path is proven.
checks:
- title: Record the original DNS
  text: Write down the device's current DNS servers and whether it uses DHCP or a manual setting. Take a screenshot before changing anything.
- title: Point one device at Pi-hole
  text: Set only the test device to the Pi-hole address, then renew its lease or reconnect Wi-Fi.
- title: Test block and allow
  text: Confirm a known ad domain is blocked and a normal site still resolves. Check the Pi-hole query log for the test device.
- title: Restore and verify
  text: Put the original DNS back, reconnect, and confirm normal browsing before removing the test configuration.
included:
- One Pi-hole instance and one test device
- DNS and query-log verification
- Blocking and normal-browsing checks
- Clean rollback to the original DNS
excluded:
- Router replacement or ISP repair
- Guaranteeing every ad is blocked
- Bypassing parental controls
- Managing a large multi-VLAN network
faqs:
- q: Will this change my whole network?
  a: No. The first test changes one device. A router-wide change is a separate decision after the device test passes.
- q: Why are ads still showing?
  a: The client may use DoH/DoT, a different DNS server, or a cached connection. The Pi-hole query log shows whether the device is actually asking Pi-hole.
- q: How do I undo it?
  a: Restore the DNS value you recorded, renew the connection, and confirm browsing. The handover note includes the exact original value.
---
