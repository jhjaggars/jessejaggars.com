---
title: "Btrfs Is Awesome"
date: 2021-04-15T18:46:22-04:00
draft: false
description: "I resized my btrfs partition and it was fun!"
---

I just had a really enjoyable software experience.  Without explaining _how_ I
got into this situation, let me just explain the starting conditions.

I have a 1TB nvme disk in my system and had two btrfs partitions, `/`
and `/home`.  On this same system I have a handful of other partitions
to enable dual-booting.  The layout was something like this:

```
[[ntfs      512GB].....................[/home 64GB][/    64GB]]
```

And I wanted a layout more like this:

```
[[ntfs      512GB].....................[/home 128GB][/    64GB]]
```

So I create a new 128GB partition to land the contents of `/home` on and
the magic happened:

```
btrfs replace start /dev/$old-home-device /dev/$new-home-device /home
```

And in an instant my `/home` filesystem was moved and the old filesystem
was disabled.  I involuntarily yelped with joy when I had realized that
the process compeleted so quickly.

The final bit is to resize the filesystem to fit the entire partition:

```
btrfs filesystem resize max /home
```
