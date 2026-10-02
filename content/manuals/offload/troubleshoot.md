---
title: Troubleshoot Docker Offload
linktitle: Troubleshoot
weight: 800
description: Learn how to troubleshoot issues with Docker Offload.
keywords: cloud, troubleshooting, Docker Desktop, offload, entitlement
tags: [Troubleshooting]
---

{{< summary-bar feature_name="Docker Offload" >}}

{{% include "offload-version-note.md" %}}

Docker Offload requires:

- Docker Desktop 4.95 or later
- Authentication
- An active internet connection
- No restrictive proxy or firewall blocking traffic to Docker Cloud
- A Docker Agentic Platform subscription
- **Enable Docker Offload** turned off, if the setting appears in Docker
  Desktop settings

## Docker Offload commands report an error

Because there are two versions of Docker Offload and only one can be active at
a time, most access errors come from a mismatch between the **Enable Docker
Offload** setting and your subscription. Use the following table to map the
error to a fix.

| You see | Cause | Fix |
| --- | --- | --- |
| A message telling you to turn off **Enable Docker Offload** when you run `docker offload create` or `docker --cloud` | The setting is on, so your commands use Docker Offload v1 | Turn off **Enable Docker Offload** in **Settings** > **Docker Offload**, then confirm your account has a Docker Agentic Platform subscription |
| A message telling you to enable Docker Offload in settings when you run `docker offload start` | The setting is off, so your commands use the current version of Docker Offload | Turn on **Enable Docker Offload** if you have a Docker Offload v1 subscription, or use `docker offload create` instead |
| No Docker Agentic Platform subscription, with a link to purchase | Your account doesn't have a subscription that covers Docker Offload | [Activate a subscription](/manuals/agentic-platform/signup.md#activate-cloud-access) |
| No Offload subscription when you run `docker offload start` | Your organization's Docker Offload v1 subscription has ended, or you never had one | Turn off **Enable Docker Offload** and use the current version of Docker Offload, or <a href="https://www.docker.com/pricing/contact-sales/" id="dkr_docs_cs_offload_troubleshoot" class="link" rel="noopener">contact sales</a> about Docker Offload v1 |

If **Enable Docker Offload** is on but your Docker Offload v1 subscription has
ended, both versions report an error until you turn the setting off. Turning it
off doesn't grant access on its own, so you also need a Docker Agentic Platform
subscription. For more information, see [Configure Docker Offload
v1](v1/configuration.md#what-the-setting-controls).

## Builds or containers aren't running in the cloud

1. Check whether a sandbox is connected:

   ```console
   $ docker offload status
   ```

2. Check which Docker context is selected. Docker Offload uses the `cloud`
   context:

   ```console
   $ docker context ls
   ```

   If the selected context is `desktop-linux`, your commands run locally. Run
   `docker offload create` to connect, or pass `--cloud` on individual
   commands.

3. Get more information about your connection and account access:

   ```console
   $ docker offload diagnose
   ```

4. Verify authentication with `docker login`. If needed, sign out and sign in
   again:

   ```console
   $ docker logout
   $ docker login
   ```

5. Verify your usage. For more information, see [Docker Offload
   usage](usage.md).

## Your containers are missing

If `docker ps -a` doesn't show containers you expect, check which context is
selected. Containers in your cloud sandbox and containers on your local machine
are separate, and each context only shows its own.

To list your local containers while Docker Offload is connected:

```console
$ docker --context desktop-linux ps -a
```

For more information, see [Work with local and cloud
containers](sandboxes.md#work-with-local-and-cloud-containers).

## Resuming a sandbox is slow

Hibernation can take several minutes to complete. If you resume a sandbox while
it's still hibernating, Docker Offload reports that and connects once the
sandbox is ready. Resuming a sandbox that has finished hibernating is close to
immediate.
