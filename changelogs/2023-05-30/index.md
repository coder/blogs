---
title: What is New in May 2023
date: May 30, 2023
authors:
  - { name: Muhammad Atif Ali, avatar: https://github.com/matifali.png }
  - { name: Ben Potter, avatar: https://github.com/bpmct.png }
version: "0.23.7"
---

![banner](.\static\banner.png)

In the last few months, we have been working on exciting new features for Coder. We are excited to share them with you today.

## Workspace Proxies

Workspace proxies provide low-latency experiences for geo-distributed teams.

Coder's networking makes the best effort to connect directly to a workspace. In situations where this is not possible, such as dashboard connections, workspace proxies can reduce the distance the network traffic needs to travel.

A workspace proxy is a relay connection a developer can use when connecting with their workspace over ssh, a workspace app, port forwarding, etc. Coder automatically selects the best proxy for the developer based on their location and the location of the workspace.

For deployment instructions, see [Docs](https://coder.com/docs/v2/latest/admin/workspace-proxies).

![workspace proxies](.\static\worksapce-proxies.png)

## Envbox

[Envbox](https://github.com/coder/envbox) is an image developed and maintained by Coder that bundles the sysbox runtime. It works by starting an outer container that manages the various sysbox daemons and spawns an unprivileged inner container that acts as the user's workspace. The inner container is able to run system-level software similar to a regular virtual machine (e.g., systemd, dockerd, etc.). Envbox offers the following benefits over running sysbox directly on the nodes:

- No custom runtime installation or management on your Kubernetes nodes.
- No limit to the number of pods that run Envbox.

Refer to the [docs](https://coder.com/docs/v2/latest/templates/docker-in-workspaces#envbox) for more information.

## Template Editor

The template editor is a new feature that allows you to edit and duplicate templates directly from the Coder dashboard. This feature is available to all users with the `template-admin` or `owner` role.

![template editor](.\static\template-editor.gif)

## Fly.io Starter Template

We have added a new starter template for [Fly.io](https://fly.io/). Fly.io is a platform for running applications globally. It is a great fit for Coder workspaces because it allows you to run them close to your developers. See the blog post [here](https://coder.com/blog/remote-developer-environments-on-fly-io) for more information.
![Fly.io Starter Template](.\static\fly-io-starter-template.png)

### Other Exciting Features

- Ability to use non-localhost addresses with `coder port-forward` [docs](https://coder.com/docs/v2/latest/cli/port-forward)
- Generate _Open In Coder_ buttons for your templates to provide a one-click experience for your developers. [#7501](https://github.com/coder/coder/pull/7501)
- Add or remove licenses from the dashboard.
- Added support for X11 forwarding in the Coder CLI. [docs](https://coder.com/docs/v2/latest/cli/ssh)
