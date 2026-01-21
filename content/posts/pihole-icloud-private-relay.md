+++
date = '2026-01-21T19:01:20+01:00'
title = 'Why Your Apple Device Shows iCloud Private Relay Errors on Pi-hole and How to Fix It'
tags = ["Pi-hole","iCloud Private Relay","Apple devices","DNS filtering","Network troubleshooting"]
categories = ["Tutorials"]
+++

Apple's iCloud Private Relay is a privacy feature available to iCloud+ subscribers. When enabled, it encrypts and routes Safari traffic through
two separate relays :
1. The first relay (operated by Apple) sees your IP but **not the destination domain.**

2. The second relay (operated by a trusted third party) sees the destination domain but **not your original IP address.**

This way no single party can see both your identity and the websites you visit. It also hides DNS queries from the local network.

While this enhances privacy on untrusted networks, it also means that **Apple devices can bypass Pi-hole**, because DNS never reaches your Pi-hole resolver. As a result, Pi-hole can no longer filter ads or track DNS activity for those devices.

When Pi-hole allows Apple’s Private Relay domains to resolve normally, Apple devices assume that the network supports iCloud Private Relay and attempt to activate it. But if the network cannot fully handle the encrypted relay traffic, the connection fails partway through.
When that happens, iPhones and Macs often display warnings like **"Private Relay is unavailable on this network"** or **"This network is blocking encrypted DNS traffic."**

**If you rely on Pi-hole for network-wide DNS filtering, you will want Apple devices to stop using iCloud Private Relay while connected to your home Wi-Fi.** Fortunately, Apple officially provides a way for networks to signal that Private Relay should be disabled: by returning **NXDOMAIN** for two specific domains. See [Apple's recommendations for this.](https://developer.apple.com/icloud/prepare-your-network-for-icloud-private-relay/)

## The Pi-hole Setting: iCloudPrivateRelay

**Setting:**\
`dns.specialDomains.iCloudPrivateRelay`

**What it does:**\
When set to `true`, Pi-hole will always reply with **NXDOMAIN** when
Apple devices query:

-   `mask.icloud.com`
-   `mask-h2.icloud.com`

To activate :
```
sudo pihole-FTL --config dns.specialDomains.iCloudPrivateRelay true
```

**This tells Apple that Private Relay is not allowed on this network.** The device immediately disables iCloud Private Relay for that Wi-Fi and falls back to normal DNS which now **goes through Pi-hole again.** with no warning messages.