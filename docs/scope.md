---
title: Scope
nav_order: 9
description: What gowebfilter is, and what it deliberately leaves to you.
---

# A proxy, not an enforcer
{: .no_toc }

gowebfilter is the proxy server and nothing more.
{: .fs-6 .fw-300 }

## What is not covered

There is currently no focus on enforcement. Every platform (Windows, macOS, Linux) and every mode (localhost,
LAN, bridge, transparent) is different, and each needs its own development and testing track. There are not
the resources for that at this time.

On Android there are no enforcement hacks either. Use an EMM/MDM to lock the app down.

## What you can build on

The proxy supports several listener modes, so it can be integrated with any firewall method:

| Mode | Use it for |
|---|---|
| Regular HTTP(S) proxy | Browsers and devices configured by hand or by a PAC/WPAD file |
| SOCKS4 / SOCKS5 | Clients and tools that speak SOCKS |
| Transparent (Linux) | Firewall redirect rules, with no client configuration |
| ICAP | Squid or another proxy that hands requests over for a verdict, see [ICAP]({{ '/docs/icap/' | relative_url }}) |
| TLS-wrapped variants | An HTTPS proxy, SOCKS5 over TLS, or ICAP over TLS |

Optional TUN capture (`tun2socks`) and a Linux gateway mode exist too, but treat them as starting points.

## Contributing

You are welcome to integrate it yourself for your platform and mode. Documentation is especially welcome.
Please send it to the [gowebfilter repository](https://github.com/Yjlion/gowebfilter).
