---
title: Home
layout: home
nav_order: 1
description: A single-binary, policy-based web-filtering proxy for households and small offices.
permalink: /
---

<div class="hero" markdown="1">

# gowebfilter
{: .fs-9 }

<p class="tagline">A single-binary parental-control web filter, written in Go. Download it, run it, and manage it from your browser.</p>

[Get started]({{ '/docs/quick-start/' | relative_url }}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[View on GitHub](https://github.com/Yjlion/gowebfilter){: .btn .fs-5 .mb-4 .mb-md-0 }

</div>

![gowebfilter dashboard]({{ '/assets/images/dashboard.png' | relative_url }}){: .shot }

## Why gowebfilter?

gowebfilter is a from-scratch Go port of
[mitmproxy-web-filter](https://github.com/Yjlion/mitmproxy-web-filter), replacing a Python + mitmproxy
runtime with **one static executable**: no Python, no virtualenv, no native ML runtime, no CGO.

It is built as a **parental-control proxy, not a general proxy server**. There are no complicated ACLs and no
policy ordering. A client's IP or MAC is matched to a policy, and a single IP wins over an IP range.

<div class="feature-grid">
  <div class="feature"><h3>TLS-intercepting proxy</h3><p>Generates its own CA and issues per-host leaf certificates on the fly, filtering decrypted HTTP/HTTPS through an ordered addon pipeline.</p></div>
  <div class="feature"><h3>Per-client policies</h3><p>Match by MAC, exact IP, CIDR range, or a catch-all default. The most specific match wins, so there is no rule ordering to manage. Policies hot-reload without a restart.</p></div>
  <div class="feature"><h3>Poor man's classification</h3><p>Every policy can query a DoH (DNS-over-HTTPS) endpoint, so another service can do the classification. You do not have to download and maintain large blocklists, although the standard blocklists are supported too.</p></div>
  <div class="feature"><h3>Rich filtering</h3><p>URL allow/blacklists, category blocklists, SafeSearch, YouTube channel filtering, DoH and QUIC blocking.</p></div>
  <div class="feature"><h3>Embedded classifiers</h3><p>Pure-Go Bayesian adult-text classifier and an embedded NSFW image classifier — no model downloads.</p></div>
  <div class="feature"><h3>ICAP service</h3><p>Plug filtering into the Squid you already run — caching, ACLs and auth stay where they are.</p></div>
  <div class="feature"><h3>Management UI</h3><p>A CLI app with a web interface: policy editor, live logs and analytics, PAC generation and ARP scanning.</p></div>
  <div class="feature"><h3>Lightweight</h3><p>About 30&nbsp;MB binary, about 50&nbsp;MB of memory idle and 150-220&nbsp;MB with the image classifier busy. Runs on a Raspberry Pi, a small VM, or your desktop's localhost.</p></div>
  <div class="feature"><h3>Android, no hacks</h3><p>The Android app does no tricks to enforce itself. Lock it down with an EMM/MDM.</p></div>
  <div class="feature"><h3>Runs anywhere</h3><p>Windows and Linux (x86_64/arm64), Docker, systemd, and an Android app.</p></div>
  <div class="feature"><h3>Observable</h3><p>Built-in <code>/health</code> and Prometheus <code>/metrics</code> with no extra exporter.</p></div>
</div>

## Quick start

```bash
go build -o webfilter ./cmd/webfilter

cp config/settings.example.json config/settings.json
cp policies/default.json.example policies/default.json

./webfilter run --settings config/settings.json
```

Open `http://127.0.0.1:8000` for the management UI and point clients at `127.0.0.1:8080` as their
HTTP(S) proxy. Prefer containers? Run `docker compose up -d` — see [Docker]({{ '/docs/docker/' | relative_url }}).

## A proxy, not an enforcer

gowebfilter is the proxy server and nothing more, and it does not currently focus on enforcement. It supports
several listener modes, so it can be integrated with any firewall method. Read [the details]({{ '/docs/scope/' | relative_url }}).

## Image blurring

Flagged images are blurred (or blocked, or replaced with a checkerboard) before they reach the browser.

| As published | Through the proxy |
|---|---|
| ![Images as published]({{ '/assets/images/image-blur-before.png' | relative_url }}){: .shot } | ![Images blurred by the proxy]({{ '/assets/images/image-blur-after.png' | relative_url }}){: .shot } |

{: .note }
> These captures use harmless generated artwork, with the policy's image threshold set to 0 so every image is
> flagged. They show what the blur looks like, not how the classifier scores real images.

## A note on provenance

{: .warning }
> **Vibe-coded disclaimer.** gowebfilter was built almost entirely through AI-assisted sessions, with a human
> reviewing direction and testing rather than writing most code by hand. It has real test coverage and has been
> exercised against live traffic, but it has **not** had an independent human security audit. Treat it as a
> personal/homelab project, not audited security software.
