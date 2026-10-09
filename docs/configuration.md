---
title: Configuration
nav_order: 6
description: Settings, policies, hot reload and management HTTPS.
---

# Configuration
{: .no_toc }

<details open markdown="block">
  <summary>Table of contents</summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

Configuration lives entirely on disk:

| Path | Purpose |
|---|---|
| `config/settings.json` | Global settings (listeners, auth, ICAP, PAC, …) |
| `policies/*.json` | Per-client policies, hot-reloaded |
| `certs/` | Generated CA and leaf-certificate cache |
| `categories/` | Domain-list blocklists, refreshed by `webfilter categories update` |
| `logs/webfilter.db` | SQLite request/block log browsable from the UI |

## What reloads without a restart

Policies hot-reload wholesale. Settings read per request — interface language, proxy authentication, the
management pseudo-hostname, ICAP tuning, PAC settings and management auth — apply immediately. The API tells
you what still needs a restart:

```json
PUT /api/settings  ->  { ..., "restart_required": ["proxy_listen"] }
```

Restart-only: `proxy_listen`, `mgmt_host`/`mgmt_port`, `cert_dir`, `logs_dir` and the `log_*` options,
`policies_dir`, and the `tun2socks`/`gateway` capture modes.

## Management HTTPS

Set `mgmt_tls` to serve the management UI and API over TLS. Supply your own pair with
`mgmt_cert_file`/`mgmt_key_file` (the right choice for a publicly trusted certificate) or let the runtime CA
mint one.

{: .note }
With a CA-minted certificate the CA download itself is served by a CA the client does not yet trust, so the
first fetch warns, and WPAD clients will not fetch `/proxy.pac`. Install `certs/ca.crt` out of band, supply a
real certificate, or leave `mgmt_tls` off if you distribute PAC from this port.

## Models

- **Image** — [GantMan/nsfw_model](https://github.com/GantMan/nsfw_model) (MobileNetV2, MIT) embedded in the
  binary (~8.6 MB).
- **Text** — a compact embedded feature table scored with Naive Bayes; seed vocabulary curated from LDNOOBW
  concepts under CC-BY-4.0.

No setup or downloads are needed for either.
