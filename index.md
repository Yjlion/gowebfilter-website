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

<p class="tagline">A single static binary that filters the web for your household or small office — per-client policies, TLS interception, and a browser-based management UI.</p>

[Get started]({{ '/docs/quick-start/' | relative_url }}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[View on GitHub](https://github.com/Yjlion/gowebfilter){: .btn .fs-5 .mb-4 .mb-md-0 }

</div>

![gowebfilter dashboard]({{ '/assets/images/dashboard.png' | relative_url }}){: .shot }

## Why gowebfilter?

gowebfilter is a from-scratch Go port of
[mitmproxy-web-filter](https://github.com/Yjlion/mitmproxy-web-filter), replacing a Python + mitmproxy
runtime with **one static executable** — no Python, no virtualenv, no native ML runtime, no CGO.

<div class="feature-grid">
  <div class="feature"><h3>TLS-intercepting proxy</h3><p>Generates its own CA and issues per-host leaf certificates on the fly, filtering decrypted HTTP/HTTPS through an ordered addon pipeline.</p></div>
  <div class="feature"><h3>Per-client policies</h3><p>Match by MAC, exact IP, CIDR range, or a catch-all default. Policies hot-reload without a restart.</p></div>
  <div class="feature"><h3>Rich filtering</h3><p>URL allow/blacklists, category blocklists, SafeSearch, YouTube channel filtering, DoH and QUIC blocking.</p></div>
  <div class="feature"><h3>Embedded classifiers</h3><p>Pure-Go Bayesian adult-text classifier and an embedded NSFW image classifier — no model downloads.</p></div>
  <div class="feature"><h3>ICAP service</h3><p>Plug filtering into the Squid you already run — caching, ACLs and auth stay where they are.</p></div>
  <div class="feature"><h3>Management UI</h3><p>Policy editor, live logs and analytics, PAC generation, ARP scanning, and a native desktop window.</p></div>
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

## A note on provenance

{: .warning }
> **Vibe-coded disclaimer.** gowebfilter was built almost entirely through AI-assisted sessions, with a human
> reviewing direction and testing rather than writing most code by hand. It has real test coverage and has been
> exercised against live traffic, but it has **not** had an independent human security audit. Treat it as a
> personal/homelab project, not audited security software.
