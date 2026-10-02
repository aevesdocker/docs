---
title: About Docker Offload
linktitle: About
weight: 15
description: Learn about Docker Offload, its features, and how it works.
keywords: cloud, offload, vdi, cloud sandbox
---

{{< summary-bar feature_name="Docker Offload" >}}

{{% include "offload-version-note.md" %}}

Docker Offload is a fully managed service for building and running containers
in the cloud using the Docker tools you already know, including Docker Desktop,
the Docker CLI, and Docker Compose. It extends your local development workflow
into a cloud environment, so you can work on hardware that can't run your
workload, in virtual desktop infrastructure (VDI) environments, or on systems
that don't support nested virtualization.

## Key features

- Cloud sandboxes that persist: Your containers, images, and volumes survive
  stopping and resuming, so you can pick up a long-running workload where you
  left off.
- Familiar commands: Run the Docker CLI and Docker Compose commands you already
  use. Your context determines where they run.
- Secure communication: Use encrypted tunnels between Docker Desktop and the
  cloud sandbox, with support for secure secrets and image pulling.
- Usage-based billing: Pay for the time your sandbox runs, not for the time it
  spends hibernating.
- VDI-friendly: [Use Docker Desktop](/manuals/desktop/setup/vm-vdi.md) in
  virtual desktop environments or systems that don't support nested
  virtualization.

For more information, see the [Docker Offload product
page](https://www.docker.com/products/docker-offload/).

## How Docker Offload works

Docker Offload replaces the need to build or run containers locally by
connecting Docker Desktop to a cloud sandbox running its own Docker daemon.

When you run `docker offload create`:

1. Docker Offload provisions a cloud sandbox for your account.
2. Docker Desktop opens a secure tunnel to the Docker daemon in that sandbox.
3. Your Docker context switches to `cloud`, so Docker commands run in the
   sandbox.
4. Builds and containers run on cloud hardware, and their output streams to
   your terminal.

Your local Docker daemon keeps running throughout. Local containers aren't
stopped, but Docker commands no longer apply to them unless you pass
`--context desktop-linux`. For more information, see [Manage
sandboxes](sandboxes.md#work-with-local-and-cloud-containers).

### Relationship to Docker Sandboxes

Docker Offload runs on the same cloud infrastructure as [cloud
sandboxes](/manuals/ai/sandboxes/cloud/_index.md) in Docker Agentic Platform,
and both are covered by a Docker Agentic Platform subscription.

<!-- TODO: Document whether a sandbox created by `docker offload create`
appears in `sbx --cloud ls`, and whether `docker offload ls` shows sandboxes
created by `sbx --cloud create`. The two CLIs are documented as having separate
state, so readers need to know whether Offload sandboxes are a separate pool
or the same one viewed through a different command. -->

### Cloud resources

<!-- TODO: Confirm the vCPU and memory of a Docker Offload sandbox, whether
the size is configurable, and which CPU architectures are available. Docker
Offload v1 used a fixed 4 vCPU / 8 GiB host. -->

### Session lifecycle and hibernation

A Docker Offload sandbox moves between a running state and a hibernated state.

- Running: The sandbox is connected and billable. Builds and containers execute
  on cloud hardware.
- Hibernated: The sandbox is suspended and isn't billed. Its containers,
  images, and volumes are preserved.

Your sandbox hibernates when you run `docker offload stop`, or automatically
after a period without activity. Hibernation can take several minutes to
complete.

Resuming a hibernated sandbox restores its exact previous state. Containers
that were running resume, and data written to volumes and container
filesystems is still there. Build logs from an earlier session remain available
in the sandbox.

<!-- TODO: Document the idle period that triggers automatic hibernation. -->

<!-- TODO: Document how long a hibernated sandbox is retained before it's
deleted. This determines how long earlier build logs and container state stay
available, and it's the most common question from readers planning to walk away
from a workload. -->

<!-- TODO: Document the command that resumes a hibernated sandbox. The v2
command set replaces `docker offload start`, so confirm whether resuming uses
`docker offload create`, a dedicated resume command, or happens implicitly on
the next Docker command. -->

<!-- TODO: Document what happens when `docker offload create` runs for the same
account on two machines. -->

### Comparison with Docker Offload v1

| | Docker Offload | [Docker Offload v1](v1/_index.md) |
| --- | --- | --- |
| Subscription | Docker Agentic Platform, purchased by individual developers | Docker Offload, purchased by an organization |
| Start with | `docker offload create` | `docker offload start`, or the Dashboard toggle |
| Run one command remotely | `docker --cloud build`, `docker --cloud run` | Not available |
| Docker context | `cloud` | `docker-cloud` |
| On stop | Sandbox hibernates and keeps its state | Environment is terminated and its contents are deleted |
| Billing | Metered for the time the sandbox runs | Covered by the organization's subscription, subject to fair use |

> [!IMPORTANT]
>
> The Docker context is named `cloud` rather than `docker-cloud`. If you have
> scripts or tooling that reference the `docker-cloud` context, update them
> before moving from Docker Offload v1.

## What's next

Get hands-on with Docker Offload by following the [Docker Offload
quickstart](quickstart.md).
