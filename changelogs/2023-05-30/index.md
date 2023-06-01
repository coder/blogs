---
title: What is New in May 2023
date: May 30, 2023
author: Muhammad Atif Ali
version: "0.23.7"
---

![banner](.\static\banner.png)

In the last few months, we have been working on exciting new features for Coder. We are excited to share them with you today.

## Workspace Proxies (Experimental)

Workspace proxies provide low-latency experiences for geo-distributed teams.

Coder's networking makes the best effort to connect directly to a workspace. In situations where this is not possible, such as developing with a web IDE (e.g. Jupyter), workspace proxies reduce latency for developers.

A workspace proxy is a relay connection a developer can use when connecting with their workspace over SSH, a workspace app, port forwarding, etc. Coder automatically selects the best proxy for the developer based on their location and the location of the workspace.

For deployment instructions, see [Docs](https://coder.com/docs/v2/latest/admin/workspace-proxies).

![workspace proxies selection page](.\static\worksapce-proxies-2.png)

## Envbox

[Envbox](https://github.com/coder/envbox) is an image developed and maintained by Coder that bundles the sysbox runtime. It works by starting an outer container that manages the various sysbox daemons and spawns an unprivileged inner container that acts as the user's workspace. The inner container is able to run system-level software similar to a regular virtual machine (e.g., systemd, dockerd, etc.). Envbox offers the following benefits over running sysbox directly on the nodes:

- No custom runtime installation or management on your Kubernetes nodes.
- No limit to the number of pods that run Envbox.

Refer to the [docs](https://coder.com/docs/v2/latest/templates/docker-in-workspaces#envbox) for more information.

## Template Editor

Edit & duplicate templates directly from the Coder dashboard. This feature is available to all users with the `template-admin` or `owner` role.

![template editor screenshot](.\static\template-editor.gif)

## Fly.io Starter Template

We have added a new starter template for [Fly.io](https://fly.io/). Fly.io is a platform for running applications globally. It is a great fit for Coder workspaces because it allows you to run them close to your developers. See the blog post [here](https://coder.com/blog/remote-developer-environments-on-fly-io) for more information.
![Fly.io starter template screenshot](.\static\fly-io-starter-template.png)
![Fly.io regions map](https://fly.io/docs/images/fly-region-map.webp)

## Open in Coder

Now you can generate _Open In Coder_ buttons for your templates to provide a one-click experience for your developers. See the [docs](https://coder.com/docs/v2/latest/templates/open-in-coder) for more information.

### Other Exciting Features

- Ability to use non-localhost addresses with `coder port-forward` [docs](https://coder.com/docs/v2/latest/cli/port-forward)
- Add or remove licenses from the dashboard.
- Added support for X11 forwarding in the Coder CLI. [docs](https://coder.com/docs/v2/latest/cli/ssh)
