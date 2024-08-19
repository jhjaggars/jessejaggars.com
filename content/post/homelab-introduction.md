---
title: "Homelab Introduction"
date: 2024-08-02T13:27:45-04:00
draft: true
---

# So, I guess I have a homelab now

Over the past couple of years I have collected some computing devices and put them to use doing various self-hosting things and some educational stuff as well.  The goal of this post is to give a broad overview of what I've been up to and some of my motivations that led me to where I am now.

### Home Automation

I think it's fair to say the first 'full-time' self-hosted thing I launched was a server to run [Home Assistant](https://www.home-assistant.io).  I used a raspberry pi 4 and the [diy image](https://www.home-assistant.io/installation/#diy-with-raspberry-pi) running on a small thumb drive.  I remember reading somewhere that the speed differences between usb3 and the sdcard interface would be noticible so that's why I went that route.

I built up a small collection of iot devices and managed them all via Home Assistant.  I also utilized the homekit bridge integration to present a subset of devices to the Home app on my wife and I's iPhones.  We are still making good use of Home Assistant today.

### Network Attached Storage

At some point around 2016 or so I picked up a [Dell T30]() with the intention of running some VM stuff for work.  It had been sitting mostly unused for years when I decided to try out [TrueNAS]().  I added a couple 4 TB disks and installed TrueNAS and have been running it ever since.  We used this system as a backup target for our personal laptops initially, and now I use it for all sorts of stuff which I'll get to later.  At one point I decided to upgrade from Core to Scale and I thought the upgrade process was magical.  I've made a [couple of upgrades]() to make the system even more useful for remote clients.  There are a couple of downsides to this system though, the form factor and the lack of disk capacity are eventually going to limit the usefulness of this system.

### Compute

I got a good deal on an 11th generation Intel NUC on ebay and installed [Proxmox]() on it.  I quickly decided I wanted to try managing a few more machines and built a system around a [free ryzen 3800xt] one of my friends gave me.  I ran a 2 node Proxmox cluster for a while, until I found another NUC at a great price on ebay, which allowed me to expand to 3 nodes.

Along the way, I set up [K3S]() on top of VMs and have been on that journey ever since.

I got very excited by the meteor lake mobile processors from Intel and grabbed a new ASROCK 155H system and included it in my setup later.  Notable for this system is that I added it as a bare metal worker node for my k8s cluster, and I did not install ProxMox on it.

### Racking

I have a 15U rack that I have been putting most of my equipment into.  I decided on this because it fits nicely under my workbench in my office and it is open and easy to work with.  I've added an [8-port KVM switch]() and a very useful [rackmount KVM]() from Ebay.  Prior to this I was using a [4 port EzCoo KVM switch]() and a portable monitor as a sort of crash cart.  Once I moved beyond 4 physical machines I decided to go with the current setup.

### Power

I have a total of 3 UPS devices.  One for my workstation, one for the rack, and one for the network equipment.  I acquired the workstation UPS many years ago and I [replaced the battery]() recently to keep it working well.  I [monitor]() all of the devices with Network UPS tools.

### Networking

I started with a mesh wifi setup from [Eero](), then expanded to a small desktop switch in my office, then to a [Unifi UDM Pro]() and [more switches]().  I've recently consolidated down to two switches, and enabled 2.5GbE networking across the house.


