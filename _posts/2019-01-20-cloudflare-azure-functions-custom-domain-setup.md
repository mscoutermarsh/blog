---
layout: post
title: CloudFlare + Azure Functions Custom Domain Setup
---

### How to do it

In CloudFlare, setup your DNS entry to look like this:

![Cloudflare & Azure]({% asset_path azure-cloudflare-setup.png %})

The key part is, leaving the "cloud" set to DNS only (only for setup, you can enable CDN after setup).

Once you do that, click "Validate" in Azure and you'll get that "Domain Ownership" box to turn green.
