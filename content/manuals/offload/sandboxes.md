---
title: Manage Docker Offload sandboxes
linktitle: Manage sandboxes
weight: 20
description: Create, list, stop, resume, and delete the cloud sandbox that Docker Offload uses to run your containers.
keywords: offload, cloud sandbox, context, hibernate, resume
---

{{< summary-bar feature_name="Docker Offload" >}}

Docker Offload runs your builds and containers in a cloud sandbox. This page
covers the sandbox lifecycle and how to work with local and cloud containers at
the same time.

You can run one Docker Offload sandbox at a time.

## Create a sandbox

```console
$ docker offload create
```

This creates a sandbox, connects Docker Desktop to it, and switches your Docker
context to `cloud`.

If a sandbox already exists for your account, Docker Offload shows a warning
instead of creating a second one.

## List your sandboxes

```console
$ docker offload ls
```

This lists only the sandboxes that Docker Offload created. You can also view
your sandbox and its hourly price in the [Docker Agentic Platform
Console](https://agentic-platform.docker.com/).

To check the state of your connection:

```console
$ docker offload status
```

## Stop a sandbox

```console
$ docker offload stop
```

Your Docker context switches back to `desktop-linux` immediately, and the
sandbox begins hibernating in the background. Hibernation can take several
minutes to finish.

A hibernated sandbox keeps its containers, images, and volumes. Billing stops
when hibernation is initiated. For more information, see [Docker Offload
usage](usage.md).

## Resume a sandbox

Resuming a hibernated sandbox restores its previous state. Containers that were
running resume, and data written to volumes and container filesystems is
preserved.

If hibernation is still in progress when you resume, Docker Offload reports
that and connects once the sandbox is ready. If the sandbox has finished
hibernating, resuming is close to immediate.

<!-- TODO: Document the command that resumes a hibernated sandbox, then replace
this placeholder with the command and its output. The v2 command set replaces
`docker offload start`, so confirm whether resuming uses `docker offload
create`, a dedicated resume command, or happens implicitly on the next Docker
command. -->

<!-- TODO: Document how long a hibernated sandbox is retained before it's
deleted, and what happens to its contents. -->

## Delete a sandbox

<!-- TODO: Confirm whether `docker offload delete` exists. If it does, document
it here, including whether it needs the sandbox to be hibernated first and what
it does to volumes. If it doesn't, document how a reader removes a sandbox
they no longer want to keep. -->

## Work with local and cloud containers

Creating a sandbox doesn't stop your local containers. It changes which Docker
daemon your commands reach.

While your context is `cloud`:

- `docker ps -a` lists containers in the sandbox. Your local containers don't
  appear.
- Images you built locally aren't present in the sandbox, and images you build
  in the sandbox aren't present locally.

To run a command against your local daemon without stopping Docker Offload,
pass the context explicitly:

```console
$ docker --context desktop-linux ps -a
$ docker --context desktop-linux compose up
```

To send a single command to the cloud while your context is local, use the
`--cloud` flag:

```console
$ docker --cloud build .
```

<!-- TODO: Document how ports published by containers in the sandbox are
reached, and whether local published ports stay reachable while the context is
`cloud`. The Offload v2 user journey describes sandbox ports as available on
localhost, but cloud sandboxes are documented as exposing a port through a
public HTTPS URL with no host port binding. See
/manuals/ai/sandboxes/cloud/local-vs-cloud.md. -->

## Use Docker Compose in a sandbox

Compose commands behave the same way in a sandbox as they do locally, and
Compose state survives hibernation:

- `docker compose stop` keeps the containers and their data. `docker compose
  up` resumes them.
- `docker compose down` removes the containers. `docker compose up` then
  creates containers without the previous data.

Because the sandbox persists through hibernation, a stack you bring up in one
session is still there when you resume, including data written to its volumes.
