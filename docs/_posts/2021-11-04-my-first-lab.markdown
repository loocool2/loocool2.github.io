---
layout: post
title:  "My First Lab"
date:   2021-11-05
categories: homelab documentation
image: /assets/myfirstlab.png
read_time: true
---

I only have two computers right now to use as servers that were given to me by friends. I'm using one as a OPNSense router, and the other as a Proxmox host. They're not great, but I can't complain about free hardware.

# OPNSense

My firewall and router. It's a fairly straightforward setup with a A10-7860k overclocked to 4.4ghz and 16GB of 2133 MHz DDR3 RAM, and two 2x1 gigabit NICs in it. It's running a couple services on it to take advantage of the power it's got.

## AdGuard Home
Blocks ads on my network, pretty self-explanatory. I chose it over PiHole since it has a couple more features I like and I can install it directly on my router rather than use another machine for it, since native FreeBSD support. No ugly error boxes that you'd get from a hosts based blocker or wasted CPU cycles from a browser extension.

## Caddy v2

Makes running a bunch of subdomains on one port is way easier than poking 20 holes in my firewall. Running it directly on my router means it's more streamlined as well. Prefer Caddy over `nginx` for reasons of simplicity, I just need redirects for subdomains.

## WireGuard

So I can access stuff on my home network from anywhere in the world from my laptop and phone.

## Prometheus Exporter

Makes tracking stats in one place easier by exporting to my Raspberry Pi Grafana dashboard.

# Squirtle

My main current server I have running is a AMD Kaveri based machine. Unfortunately, it's in a NZXT H210, so that means I have to run a ITX board and not have much space for drives, but I'm too poor for too many more drives than I already have anyway. I'm currently running two 10TB drives and one 6TB drive without RAID or anything, so a full nonredundant 26TB of space. I'm going to need to get something going soon, but I'm too poor for that right now. The machine is running a 860k overclocked to 4.5Ghz, and 16GB of 2133 MHz DDR3 RAM, neither of which is enough for my purposes, but I have to make do. I also have a 1050 Ti in it for Plex encoding.

Right now, the server's running Proxmox 7. I don't have another two Proxmox hosts so I turned off corosync and HA completely off. Don't think I'd even want to, with the reputation that Proxmox HA has.

Going to go from up to down here.

## Komga
My Manga server, has multiple libraries to seperate types of content. Really nice built in webreader, but I mostly use the Tachiyomi to access my server through the extension. I wish there were any good OPDS clients on Android for it otherwise but sadly there are not.

## Calibre-web
Hosts all my books, primarily light novels. Sadly can't read online through it, but there really aren't any good light novel softwares out there that you can self host. I primarily use Moon+ Reader to read things on my tablet by accessing it through the OPDS protocol that nothing has proper support for.

## Watchtower
I'm lazy and forget to update my docker containers a lot. This checks once a day and updates them for me.

## Portainer
This is an essential tool in many a docker user's toolbox. Really makes deployments much easier.

## Plex
Not much to say about this one, I encode things with my 1050 Ti and it works pretty well, then Tautulli detects whenever a Anime episode is finished, and triggers Plex-AniSync to scrobble whatever I just watched to AniList.

### Tautulli
Monitors my Plex server, I also access it with Tautulli Remote. I still use the Plex Dashboard for performance monitoring but the specific stream stats stuff is pretty nice.

## LXCs
Everything next is hosted through LXCs to reduce server overhead and conserve resources for my poor server.

### Minecraft server
Spun up a little server for my friends, runs 24/7 but doesn't see much action sadly. I still haven't been able to see how much performance this would consume at peak but I hope to see it soon. I use Purpur instead of Paper for even further performance optimizations.

### Nextcloud
Running this with a premade Turnkey Linux template since I'm too lazy to figure out how to do with Docker. Not too much customization to it aside from mounting my hard drives.

### SMB
Runs SMB shares from my hard drives to computers on my network. Pretty simple.

### MangaDex@Home/Hentai@Home
Nodes for content sharing to MangaDex/E-Hentai users.

# Raspberry Pi
I have literally no idea what to do with this thing, my server and my router have already enscapulated all the common uses for it.

## Grafana
Gets data from my Proxmox server and my router for now, hopefully I'll be able to add more datasources later.

# Seedbox
This remote server I rent downloads everything I need and keeps it seeded with a 1Gbps upload connection. It also runs *arrs and Syncthing to autodownload to my server, where Filebot watches and renames upon entering.

# What's next?
There's so much to do with hardware right now, I'm already feeling heavily constrained by the servers I have. I'm hoping to get another tower to run more and better game servers and other things. Heck, my servers keep my room so warm I need to keep my windows open always or I'd be way too warm. I still need to get more comfortable with Grafana, and maybe I'll move onto ansible next, but who knows!
