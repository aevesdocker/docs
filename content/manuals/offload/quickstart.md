---
title: Docker Offload quickstart
linktitle: Quickstart
weight: 10
description: Learn how to use Docker Offload to build and run your container images in the cloud.
keywords: cloud, quickstart, Docker Desktop, offload
---

{{< summary-bar feature_name="Docker Offload" >}}

{{% include "offload-version-note.md" %}}

[Docker Offload](about.md) lets you build and run containers in a cloud sandbox
while using your local Docker tools and workflow. Your containers run on cloud
hardware, but you work with them the same way you work with local containers.

This quickstart covers the steps developers need to get started with Docker
Offload.

## Prerequisites

- [Docker Desktop](/desktop/) 4.95 or later. Docker recommends using the latest
  version to access the newest features and improvements in Docker Offload.
- A Docker Agentic Platform subscription. To subscribe, see [Signup and
  billing](/manuals/agentic-platform/signup.md).
- **Enable Docker Offload** turned off in **Settings** > **Docker Offload**, if
  the setting appears. The setting appears only if your account has been
  activated for [Docker Offload v1](v1/_index.md).

## Step 1: Sign in

1. Start Docker Desktop and sign in with the Docker account you used to
   subscribe.
2. Open a terminal and verify that your account has access:

   ```console
   $ docker offload diagnose
   ```

   If the check reports that your account doesn't have access, see
   [Troubleshoot Docker Offload](troubleshoot.md).

## Step 2: Create a cloud sandbox

Run the following command to create a sandbox and connect Docker to it:

```console
$ docker offload create
```

Docker Offload creates a cloud sandbox and switches your Docker context to
`cloud`. From this point, Docker commands run in the sandbox instead of on your
local machine.

> [!TIP]
>
> To learn more about the Docker Offload CLI commands, see the [Docker Offload
> CLI reference](/reference/cli/docker/offload/).

To confirm which context is selected:

```console
$ docker context ls
```

## Step 3: Run a container

Run a container to verify that Docker Offload is working:

```console
$ docker run --rm hello-world
```

The container runs in the cloud sandbox, and its output is streamed to your
terminal. If Docker Offload is working, you see `Hello from Docker!` in the
output.

You can also send a single command to the sandbox without creating one first.
The `--cloud` flag runs that command in the cloud and leaves your context
unchanged:

```console
$ docker --cloud build .
$ docker --cloud run --rm hello-world
```

<!-- TODO: Clarify the relationship between `docker offload create` and `docker
--cloud`. If `create` switches the context to `cloud`, document when a reader
should reach for `--cloud` instead, and whether `--cloud` creates a sandbox
on demand. -->

## Step 4: Reach your local containers

Creating a sandbox doesn't stop your local containers, but it does change which
containers Docker commands apply to. While your context is `cloud`:

- `docker ps -a` lists containers in the sandbox, not your local containers.
- Ports published by your local containers aren't reachable through the `cloud`
  context.

To work with your local containers without leaving Docker Offload, pass the
local context explicitly:

```console
$ docker --context desktop-linux ps -a
```

<!-- TODO: Document how ports published by containers in the sandbox are
reached. The Offload v2 user journey describes them as available on
localhost, but cloud sandboxes are documented as exposing a port through a
public HTTPS URL with no host port binding. See
/manuals/ai/sandboxes/cloud/local-vs-cloud.md. These need to agree before this
step can describe port access. -->

## Step 5: Check your sandbox

List the sandboxes that Docker Offload created for you:

```console
$ docker offload ls
```

You can also view your sandbox, including its hourly price, in the [Docker
Agentic Platform Console](https://agentic-platform.docker.com/).

You can run one Docker Offload sandbox at a time. If you run `docker offload
create` while a sandbox exists, Docker Offload shows a warning instead of
creating a second one.

To check the state of your connection at any time:

```console
$ docker offload status
```

## Step 6: Stop Docker Offload

When you finish, stop Docker Offload:

```console
$ docker offload stop
```

Your Docker context switches back to `desktop-linux` immediately, and the
sandbox begins hibernating in the background. Your local containers and their
published ports are available again.

Unlike Docker Offload v1, stopping doesn't discard your work. The sandbox keeps
its containers, images, and volumes, so you can resume where you left off. For
what's preserved and how billing stops, see [Manage
sandboxes](sandboxes.md).

## What's next

- [Learn how Docker Offload works](about.md)
- [Manage your sandbox](sandboxes.md), including resuming and deleting it
- [Optimize your Docker Offload usage](optimize.md)
