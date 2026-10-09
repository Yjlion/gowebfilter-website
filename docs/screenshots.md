---
title: Screenshots
nav_order: 8
description: A tour of the management UI.
---

# Screenshots
{: .no_toc }

The management UI, rendered against generated sample data.
{: .fs-6 .fw-300 }

{% assign shots = "dashboard:Dashboard,policies:Policies,policy-editor:Policy editor,logs:Logs,analytics:Analytics,tools:Tools,settings:Settings" | split: "," %}
{% for s in shots %}
{% assign parts = s | split: ":" %}
## {{ parts[1] }}

![{{ parts[1] }}]({{ '/assets/images/' | append: parts[0] | append: '.png' | relative_url }}){: .shot }
{% endfor %}
