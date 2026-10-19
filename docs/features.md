---
title: Features
nav_order: 3
description: What gowebfilter filters and how.
---

# Features
{: .no_toc }

<details open markdown="block">
  <summary>Table of contents</summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## TLS-intercepting forward proxy

gowebfilter runs its own CA, issues per-host leaf certificates on demand, and passes decrypted HTTP/HTTPS
traffic through an ordered addon pipeline. It supports several listener modes (regular HTTP proxy, SOCKS4/5,
transparent on Linux, and ICAP, each optionally TLS-wrapped), plus optional TUN capture (`tun2socks`) and a
Linux gateway mode. See [Scope]({{ '/docs/scope/' | relative_url }}).

## Per-client policy routing

Each policy matches clients by **MAC address**, **exact IP**, **CIDR range**, or acts as the **catch-all
default**. The most specific match wins: MAC, then a single IP, then an IP range, then the default. There are no
ACLs and no rule ordering to manage. Policies are JSON files that hot-reload, so you can edit them in the UI or
on disk.

## Filtering addons

| Addon | What it does |
|---|---|
| URL allow/blacklist | Per-policy rules plus category blocklists (ads, gambling, malware, phishing, porn, social, …) |
| SafeSearch | Enforces safe-search modes on major search engines |
| YouTube filtering | Channel-level allow/block |
| DoH classification | Each policy can query a DoH (DNS-over-HTTPS) resolver, such as a family-filtering DNS service, and block what it blocks. No large blocklists to download and maintain |
| DoH / DoT blocking | Stops clients bypassing the filter with encrypted DNS |
| QUIC blocking | Forces HTTP/3 traffic back onto the inspected path |
| Text classifier | Embedded pure-Go Naive Bayes adult-text scorer |
| Image classifier | Embedded MobileNetV2 NSFW model run by a from-scratch pure-Go inference engine. Flagged images are blurred, blocked or replaced with a checkerboard |

Both classifiers are opt-in per policy, because NSFW false positives have real cost.

## Management UI

A browser-based interface for policies, live logs, analytics, PAC file generation, neighbour/ARP scanning and
category list management. This is the only UI: gowebfilter is a CLI app with a web interface. See the [screenshots]({{ '/docs/screenshots/' | relative_url }}).

## Single static binary

No Python runtime, no native ML runtime, no sidecar DLLs. Cross-compiles for Windows and Linux
(x86_64/arm64) with `CGO_ENABLED=0`. The binary is about 30 MB, and memory use is about 50 MB idle and
150-220 MB with the image classifier busy. An Android app is also in the repository; it does no enforcement
tricks, so lock it down with an EMM/MDM.
