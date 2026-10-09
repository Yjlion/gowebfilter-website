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
traffic through an ordered addon pipeline. It also supports SOCKS5, ICAP, TUN capture (`tun2socks`) and
transparent gateway modes.

## Per-client policy routing

Each policy matches clients by **MAC address**, **exact IP**, **CIDR range**, or acts as the **catch-all
default**, evaluated in that tiered order. Policies are JSON files that hot-reload — edit them in the UI or on
disk.

## Filtering addons

| Addon | What it does |
|---|---|
| URL allow/blacklist | Per-policy rules plus category blocklists (ads, gambling, malware, phishing, porn, social, …) |
| SafeSearch | Enforces safe-search modes on major search engines |
| YouTube filtering | Channel-level allow/block |
| DoH / DoT blocking | Stops clients bypassing the filter with encrypted DNS |
| QUIC blocking | Forces HTTP/3 traffic back onto the inspected path |
| Text classifier | Embedded pure-Go Naive Bayes adult-text scorer |
| Image classifier | Embedded MobileNetV2 NSFW model run by a from-scratch pure-Go inference engine |

Both classifiers are opt-in per policy, because NSFW false positives have real cost.

## Management UI

A browser-based interface for policies, live logs, analytics, PAC file generation, neighbour/ARP scanning and
category list management. See the [screenshots]({{ '/docs/screenshots/' | relative_url }}).

## Single static binary

No Python runtime, no native ML runtime, no sidecar DLLs. Cross-compiles for Windows and Linux
(x86_64/arm64) with `CGO_ENABLED=0`. An Android app is also in the repository.
