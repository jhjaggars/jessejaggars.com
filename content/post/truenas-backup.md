---
title: "Truenas Backup"
date: 2024-04-29T08:42:24-04:00
draft: true
---

I use [Truenas]() at home for storage.  I use it for backups and volumes for my
local kubernetes setup.  While, it's really neat to have a backup target at
home, that's insufficient to ensure that I can recover from a serious failure
or disaster so I also have an offsite backup configured.

My first pass at accomplishing this was to synchronize my data to amazon s3
glacier.  It was pretty easy to configure, but I am clearly not smart enough to
read the fine print and ended up running up a large bill.. somehow.  The best
part is how smart felt before I realized my error, so awesome.  I decided to
abandon that plan and try out storj, which also has an integration built
directly into truenas.  Easy! 
