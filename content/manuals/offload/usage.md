---
title: Docker Offload usage
linktitle: Usage
weight: 30
description: Learn how Docker Offload is billed and how to monitor your usage.
keywords: cloud, usage, offload, billing
---

{{< summary-bar feature_name="Docker Offload" >}}

{{% include "offload-version-note.md" %}}

Docker Offload is billed for the time your cloud sandbox runs, as part of your
Docker Agentic Platform subscription. For subscription and payment information,
see [Signup and billing](/manuals/agentic-platform/signup.md).

## What you're billed for

Billing starts when your sandbox finishes being created and stops when
hibernation is initiated, whether you run `docker offload stop` or the sandbox
hibernates on its own after a period without activity.

You aren't billed while a sandbox is hibernated. A hibernated sandbox keeps its
containers, images, and volumes, so you can leave a workload in place between
sessions without paying for idle time.

<!-- TODO: Confirm the billing unit and the hourly rate, and whether there's a
minimum billable duration. The v2 plan mentions exploring more granular
metering in a later milestone, so note the unit that ships with Docker Desktop
4.95. -->

<!-- TODO: Document whether volumes retained by a hibernated sandbox incur a
storage charge separate from compute. -->

## Monitor your usage

To see your running sandbox and its hourly price, open the [Docker Agentic
Platform Console](https://agentic-platform.docker.com/).

To see your sandboxes from the CLI:

```console
$ docker offload ls
```

While a sandbox is running, Docker Desktop shows a session duration estimate in
the Dashboard footer next to the hourglass icon.

<!-- TODO: Confirm which Docker Desktop and Docker Home surfaces report Offload
spend for v2, and whether the Offload activity page in Docker Home covers v2
sandboxes or remains specific to Docker Offload v1. -->

## Reduce your usage

Because you're billed for the time your sandbox runs, the main ways to reduce
your spend are to stop your sandbox when you're not using it and to make your
builds faster. For build and transfer optimizations, see [Optimize Docker
Offload usage](optimize.md).
