---
layout: post
title: Rediscovering Pi
date: 2021-04-05 10:30:00 +0000
description: 
img: piB.jpg
fig-caption:
tags: [Tech, RaspberryPi]
---
I discovered a couple of Raspberry Pis in a "lockdown clearout" recently, left untouched in a box for 4-5 years. Would they still work?

Here's what I found:
{% include image.html url="/assets/img/piB.jpg" description="Raspberry Pi Model B - Revision 2 512MB RAM (2012)" %}
{% include image.html url="/assets/img/pi2B.jpg" description="Raspberry Pi 2 Model B Desktop (Quad Core CPU 900 MHz, 1 GB RAM, Linux) (2015). I got a case for this one!" %}

I thought it would be interesting to find out what had changed in the world of Raspberry Pi in the last few years. I remember being slightly annoyed that lots of things did not run on ARM without re-compiling from source (how things have changed since then!) and spending an unreasonable amount of time configuring WiFi manually. Fast forward a few years and I have more desk space, more time and some Ethernet cables!

## Re-imaging

After checking out the official site, I discovered the official OS had a rebrand from Raspbian to Raspberry Pi OS. This supports all historic versions of Pis - part of me was wondering whether my 8 year old Model B might have been end-of-lifed. When it comes to installing there is an [Imager](<https://www.raspberrypi.org/software/>) available now which makes things very easy - so much so that I decided to forget about whatever might have been on the old microSD card and start afresh in a few clicks:
{% include image.html url="/assets/img/imager.png" description="Nice and simple imaging, and lots of other OS options too" %}

## Running

As it turns out, only one microSD card survived being loose in a box for a few years, so I decided to pick the more powerful of the two Pis to test out first. The 2B ran without any issues, although everything does feel (even) slower than I remember. SSH was also [disabled by default in 2016](<https://www.raspberrypi.org/documentation/remote-access/ssh/>) in the OS, which did catch me out! Luckily, this is enabled by checking one box in the preferences menu.

## Issues

I'll keep this section of the post updated with issues I find and any useful workarounds.

### Docker Compose

Although Docker install is simple with a familiar [one-liner](<https://www.raspberrypi.org/blog/docker-comes-to-raspberry-pi/>), Docker Compose is not released for ARM yet. Saving this for reference: <https://github.com/docker/compose/issues/6831>. Most suggested workarounds I found for this are to install an older version with `sudo apt install docker-compose` or to install it with `pip`. But if you want the latest version without much effort, I found this option the best: <https://hub.docker.com/r/linuxserver/docker-compose>

```bash
sudo curl -L --fail https://raw.githubusercontent.com/linuxserver/docker-docker-compose/master/run.sh -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

It runs docker-compose as a Docker container based on an image from linuxserver.io - so when running for the first time, it will `docker pull` and run before running the command

```bash
$ sudo docker-compose --version
Unable to find image 'ghcr.io/linuxserver/docker-compose:latest' locally
latest: Pulling from linuxserver/docker-compose
bb6a3913091f: Pull complete
[...]
Digest: sha256:4c4ccb8250b249091a8d264050e3cd624756fd6faa7ae88c271054d9600b9e81
Status: Downloaded newer image for ghcr.io/linuxserver/docker-compose:latest
docker-compose version 1.29.1, build c34c88b2
```

### 32/64 bit

Raspberry Pi 3 and above are 64 bit, however Raspberry Pi OS is 32 bit by default - the 64 bit OS has been in beta for some time. If you're running the 32-bit OS, pulling many Docker images will fail with an error like:

```bash
latest: Pulling from library/influxdb
ERROR: no matching manifest for linux/arm/v7 in the manifest list entries
```

Installing the beta 64-bit version is fairly straightforward though (assuming you have a Pi 3 or above)

* Download and extract the latest version: <https://downloads.raspberrypi.org/raspios_arm64/images/>
* Install to your SD card with Raspberry Pi Imager - it lets you select a custom .iso file instead of one of the pre-populated options
