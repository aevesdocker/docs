---
title: Docker Offload
weight: 15
description: Find documentation on Docker Offload to help you offload containers to the cloud.
keywords: build, cloud, cloud build, remote builder, offload
params:
  sidebar:
    group: Application development

grid:

- title: Quickstart
  description: Get up and running with Docker Offload in just a few steps.
  icon: rocket-launch
  link: /offload/quickstart/

- title: About
  description: Learn about Docker Offload and how it works.
  icon: information-circle
  link: /offload/about/

- title: Manage sandboxes
  description: Start, stop, inspect, and resume the cloud sandbox that runs your containers.
  icon: command-line
  link: /offload/sandboxes/

- title: Usage
  description: Learn how Docker Offload is billed and how to monitor your spend.
  icon: heart
  link: /offload/usage/

- title: Optimize
  description: Improve performance and cost efficiency in Docker Offload.
  icon: bolt
  link: /offload/optimize/

- title: Troubleshoot
  description: Learn how to troubleshoot issues with Docker Offload.
  icon: bug-ant
  link: /offload/troubleshoot/

- title: Docker Offload v1
  description: Find documentation for the subscription-based version started with docker offload start.
  icon: history
  link: /offload/v1/

- title: Feedback
  description: Provide feedback on Docker Offload.
  icon: chat-bubble-left
  link: /offload/feedback/

aliases:
- /harmonia/
---

{{< summary-bar feature_name="Docker Offload" >}}

Docker Offload is a fully managed service that lets you offload building and
running containers to the cloud using the Docker tools you already know. It
runs your containers in a cloud sandbox instead of on your local machine, which
lets you work on hardware that can't run your workload, in virtual desktop
infrastructure (VDI) environments, or on systems that don't support nested
virtualization.

You start Docker Offload with `docker offload create` and send individual
commands to the cloud with `docker --cloud`.

## Which version of Docker Offload you have

There are two versions of Docker Offload, and the commands differ between them.

| You have | If | Start with |
| --- | --- | --- |
| Docker Offload | You have a Docker Agentic Platform subscription, and **Enable Docker Offload** is off or doesn't appear in Docker Desktop settings | `docker offload create` |
| [Docker Offload v1](v1/_index.md) | Your organization has a Docker Offload subscription, and **Enable Docker Offload** is on | `docker offload start` |

These pages describe the first version. If you use `docker offload start` or the
toggle in the Docker Desktop Dashboard header, see [Docker Offload
v1](v1/_index.md) instead.

If you have a Docker Offload v1 subscription and want to use `docker offload
create`, see [Move to the current version of Docker
Offload](v1/configuration.md#move-to-the-current-version-of-docker-offload).

In the following topics, learn about Docker Offload, how to set it up, use it
for your workflows, and troubleshoot common issues.

{{< grid >}}
