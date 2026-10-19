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

## Image blurring

Flagged images are blurred before they reach the browser. First as published, then through the proxy.

![Images as published]({{ '/assets/images/image-blur-before.png' | relative_url }}){: .shot }

![Images blurred by the proxy]({{ '/assets/images/image-blur-after.png' | relative_url }}){: .shot }

{: .note }
These captures use harmless generated artwork, with the policy's image threshold set to 0 so every image is
flagged. They show what the blur looks like, not how the classifier scores real images.
