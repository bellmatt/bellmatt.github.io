
I discovered a couple of Raspberry Pis in one of my "lockdown clearouts" recently, left untouched in a box for 4-5 years. Would they still work?

Here's what I found:
{% include image.html url="/images/piB.jpg" description="Raspberry Pi Model B - Revision 2 512MB RAM (2012)" %}
{% include image.html url="/images/pi2B.jpg" description="Raspberry Pi 2 Model B Desktop (Quad Core CPU 900 MHz, 1 GB RAM, Linux) (2015). I got a case for this one!" %}

I thought it would be interesting to find out what had changed in the world of Raspberry Pi in the last few years. I remember being slightly annoyed that lots of things did not run on ARM without re-compiling from source (how things have changed since then!) and spending an unreasonable amount of time configuring WiFi manually. Fast forward a few years and I have more desk space, more time and some Ethernet cables!

## Re-imaging

After checking out the official site, I discovered the official OS had a rebrand from Raspbian to Raspberry Pi OS. This supports all historic versions of Pis - part of me was wondering whether my 8 year old Model B might have been end-of-lifed. When it comes to installing there is an [Imager](<https://www.raspberrypi.org/software/>) available now which makes things very easy - so much so that I decided to forget about whatever might have been on the old microSD card and start afresh in a few clicks:
{% include image.html url="/images/imager.png" description="Nice and simple imaging, and lots of other OS options too" %}

## Running

As it turns out, only one microSD card survived being loose in a box for a few years, so I decided to pick the more powerful of the two Pis to test out first. The 2B ran without any issues, although everything does feel (even) slower than I remember. SSH was also [disabled by default in 2016](<https://www.raspberrypi.org/documentation/remote-access/ssh/>) in the OS, which did catch me out! Luckily, this is enabled by checking one box in the preferences menu.

Even Docker setup is now a familiar [one-liner](<https://www.raspberrypi.org/blog/docker-comes-to-raspberry-pi/>) and Docker Compose is easy to install with `pip`, so I'll try this out for hosting a few things I am working on that I'd like to host on my local network.
