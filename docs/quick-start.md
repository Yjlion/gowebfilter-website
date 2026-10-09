---
title: Quick start
nav_order: 2
description: Build, run and connect your first client.
---

# Quick start
{: .no_toc }

Building needs only the Go toolchain — no CGO and no C compiler.
{: .fs-6 .fw-300 }

## Build and run

```bash
git clone https://github.com/Yjlion/gowebfilter
cd gowebfilter

go build -o webfilter ./cmd/webfilter        # webfilter.exe on Windows

cp config/settings.example.json config/settings.json
cp policies/default.json.example policies/default.json

./webfilter run --settings config/settings.json
```

| Address | What |
|---|---|
| `http://127.0.0.1:8000` | Management UI and REST API |
| `127.0.0.1:8080` | HTTP(S) forward proxy — point clients here |

`run` starts the proxy engine and the management server in one process. `webfilter proxy` and
`webfilter mgmt` run them standalone if you want process isolation.

## Trust the CA

gowebfilter generates its own certificate authority on first start. Import `certs/ca.crt` (also downloadable
from `http://127.0.0.1:8000/api/ca-cert`) into the trust store of every client, otherwise HTTPS interception
fails closed and browsers show certificate errors.

## Point a client at the proxy

Set the client's HTTP(S) proxy to `<host>:8080`, or use the generated PAC file at
`http://<host>:8000/proxy.pac`. Then watch the **Logs** page in the UI to confirm traffic is flowing.

## Category blocklists

```bash
./webfilter categories update --settings config/settings.json
# or only a subset:
./webfilter categories update --settings config/settings.json --keep porn,gambling,malware
```

This downloads the IPFire squidGuard category lists, which then show up in the policy editor.

## Native desktop window

`webfilter gui` opens a native, GPU-rendered window (pure Go) covering the dashboard, policies, logs and
settings. If a server is already running it attaches to it; otherwise it hosts one itself.

## Other ways to run it

- [Docker]({{ '/docs/docker/' | relative_url }}) — `docker compose up -d`
- Windows service / Linux systemd unit — see
  [packaging](https://github.com/Yjlion/gowebfilter/blob/main/packaging/README.md)
- [ICAP for Squid]({{ '/docs/icap/' | relative_url }}) — keep your existing proxy
