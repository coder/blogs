---
title: Run Coder in a self-hosted homelab
date: April 7, 2023
author: Marc Paquette
---

# Run Coder in a self-hosted homelab

I outgrew my modest homelab. To serve files and media I have a couple of [ODROID-HC2](https://ameridroid.com/products/odroid-hc2) devices from a closet. They work great but they’re running at full capacity.

My needs have expanded to more than just file storage. It was time to expand my homelab to handle my dev projects too.

## My problem: I can’t go anywhere

My personal and work projects are spread out on too many devices, environments, and even locations. Switching to a project switching to another laptop or even location. The environments for my projects are all over the place: OpenBSD, Windows, Linux, arm64, python3, browsers, and old-school command-line tools. I work from home, a co-work space, and sometimes other cities and countries. My  Raspberry Pi is deliciously small, but carrying one around is a pain.

Ideally, I’d like to work on my projects wherever and however I want. My daily driver, the machine I’m typing on right now, is an OpenBSD laptop. But I’d also like to use an iPad for the [ultimate sofa software development
rig](https://coder.com/blog/a-guide-to-writing-code-on-an-ipad).

You’d think it would be easy to consolidate all these things.

## I discovered Coder

Turns out that it is easy with [Coder](https://coder.com). It solves big problems for big enterprise dev teams. And I discovered that it can also solve my problems:

- An isolated workspace for each project, no matter the environment
- Remote, secure access
- Flexibility of handling cloud and on-prem projects
- Administering all these projects from a single place
- Bonus: Open source!

## First, the hardware

I found a used [Lenovo m92P Tiny](https://www.lenovo.com/il/en/desktops/thinkcentre/m-series-tiny/m92p) with 16 GB RAM and upgraded its spinning disk with a 1 TB SSD, all for $200. I named it "Marvin", a character from my favorite movie.

I installed Debian GNU/Linux and tucked Marvin into the homelab closet. Marvin’s only connection to the outside world is with an ethernet cable.

## Next, install Docker and Coder

I ssh'd into Marvin to get started.

First was Docker. I installed before Coder.

Instead of [Docker Desktop](https://www.docker.com/products/docker-desktop/), I installed [Docker Engine for Debian](https://docs.docker.com/engine/install/debian/) because Marvin is headless so I'll only use him remotely. Besides, Coder already does a lot of Docker management for me with its built-in web interface.

Next was Coder. It's surprisingly easy to set up. Its [install script](https://coder.com/docs/v2/latest/install/install.sh) does the right thing. In Marvin’s case it detects Debian to install a .deb package. It can also handle other Linux distros, too.

```bash
marc@marvin:~$ sudo curl -fsSL https://coder.com/install.sh | sh
[sudo] password for marc: 
Debian GNU/Linux 11 (bullseye)
Installing v0.18.1 of the amd64 deb package from GitHub.
# Installation progress...
deb package has been installed.
# Info for next steps, including setting up Coder as a systemd server...
marc@marvin:~$
```

Marvin is all about self-hosted projects, so I wanted Coder to run as a service.  I followed the installer's suggestion:

```bash
marc@marvin:~$ sudo systemctl enable --now coder
```

By default, Coder also set up a tunnel for me. I got the [tunnel’s access URL](https://coder.com/docs/v2/latest/admin/configure) (and checked that Coder is up and running):

```bash
marc@marvin:~$ sudo journalctl -u coder.service -b
# Log entries from Coder
Mar 09 14:22:21 marvin coder[617]: Opening tunnel so workspaces can connect to your deployment. For production scenarios, specify an external access URL
Mar 09 14:22:22 marvin coder[617]: View the Web UI: https://marvin-access-url.pit-1.try.coder.app
# More log entries from Coder
```

## Using Coder for the first time

At this point, I used my laptop's browser to log in to Coder on Marvin. I just opened a tab with the access URL where I was asked to set up an account. This account is for Marvin's instance of Coder only.

![Setting up an account](./static/account-setup.png)

Once logged in, I was ready to use a workspace. A Coder workspace is the runnable environment that a developer works in. Each developer has their own workspace. A workspace is defined by a template, which is the collection of settings that Coder uses to create a workspace. 

Before I could work in a workspace, I needed to [create a template](https://coder.com/docs/v2/latest/templates). For my first template, I wanted to keep things simple, I selected Templates, then so I chose the default settings in Coder's [Docker template](https://github.com/coder/coder/tree/main/examples/templates/docker). This template just uses the vanilla [Ubuntu container image](https://hub.docker.com/_/ubuntu/). 

Setting up a template is done on Marvin’s command line, starting with authentication:

```bash
marc@marvin:~$ coder login https://marvin-access-url.coder.app
Open the following in your browser:

        https://marvin-access-url.coder.app/cli-auth

> Paste your token here:
> Welcome to Coder, marc! You're authenticated.
```

Then 

## Where to go from here

I can finally go places because I can leave Marvin at home.

And I can finally work on my projects however I want.

[ pic of Coder on an iPad ]

The next projects I'll set up with Coder:

- Put my static web site generator in a Docker image, set it up as a Coder workspace.
- Connect my ARM single board computer to Docker
