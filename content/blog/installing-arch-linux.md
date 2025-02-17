+++
date = '2025-02-16T20:02:23+03:00'
title = 'My experience installing Arch Linux'
+++

In this post I'll describe how the process of installing, configuring and using
Arch Linux went for me.

My current setup is an Acer Predator Helios 300 laptop with
- an i7-11800H CPU
- an RTX 3050ti Mobile GPU
- 16GB of RAM
- a 1TB NVME SSD that houses my Windows installation
- a 2TB NVME SSD that I installed recently, half of which is allocated
just for storage space in my Windows installation, leaving the other half
for Arch.

The laptop is also connected to an external 144hz monitor through an
HDMI cable.

Surprisingly enough, partitioning the drive to work how I wanted it to
wasn't difficult to figure out at all. I made the 1TB storage
partition with Windows' disk manager and had the free space to deal with
however I wanted to.

This wasn't my first time installing Linux, I have an old laptop that
runs Manjaro, so I was familliar with the general way OSs are installed.

The general impression that I got from ~~researching~~ watching videos about
Arch's install process was this: **VERY DIFFICULT AND FRAGILE**. Arch gives
the user total control over every aspect of the system, which means that
editing an important file incorrectly can cause your system to break.

It took two attempts for me to get Arch to the state that it is in now.

For the first attempt I tried to follow the Arch Wiki's install guide,
failed and decided to use the archinstall script instead. I chose i3
as my window manager (as that's what I use on my old laptop) and everything
seemed to work out well *until* I needed to configure my dual monitor setup.

For whatever reason, I had an issue where if I had both of my monitors running
at 144hz, every single thing on my system slowed down to a crawl (for example
opening alacritty took an entire 10 minutes). After hours of googling and
editing many a config file, nothing worked.

Having gotten sufficiently frustrated by the absence forum posts and nothing
working, I decided to start everything over from scratch (as I often do).

So, I tried again. This time not using archinstall and doing everything myself.
That way if something went wrong, I would only have myself to blame.

I read through the entire installation guide again, taking great care not to skip
a single word even if the section I was reading didn't have anything to do with my
system (the more information I have about what I'm doing, the better!).

This time I chose swaywm as my window manager. It's a drop-in replacement for i3,
except for the fact that it uses Wayland instead of Xorg. I thought that since
Wayland is newer, it would fare better with my RTX 3050 and not have any issues.

To my surprise, getting everything to a usable state only took about 2 hours
of effort! I had to replace the nvidia drivers swaywm usually uses (Noveau)
with the nvidia-open-dkms package (because of this I have to start sway with the
`--unsupported-gpu` flag), but other than that everything went fine.

Adding the boot entry for Windows into the grub bootloader menu was a task
I already had some knowledge about, having configured that with a friend for
his machine about a month prior.

Further configuration of swaywm and various utilities was extremely easy,
so now I have a system which I'm happy to daily-drive! I still log into
Windows for gaming needs, obviously.

Now I'll briefly talk about my Arch setup:
- nano is my text editor of choice for most things I write. Yes, I know,
it's a weird decision, but I've been using it for quite a while and I
adore its simplicity. For languages that I pretty much can't write anything
in without IntelliSense (currently that's Zig and rarely Rust) I use VSCode.
I've also been trying out Emacs to *maybe* replace nano in the future.
- I switched from Yandex.Browser to ZenBrowser due to the fact that Yandex
doesn't(?) have a package Wayland-based WMs. Overall I enjoy using it but
there are some things it definitely lacks compared to Yandex. It is quite
new, though, so I expect more in the future.

I do still have a few, albeit minor, problems. For example: whenever I have
a video visibly playing on either of my monitors everything else seemingly
slows down to 60hz. This is not particularly important to me, though, so
unless I randomly stumble upon a fix I'll probably just ignore it.

I've configured swaywm to look minimalistic, using mostly black and white,
I quite like how it turned out.

![screenshot of me writing this post in nano](/writing-arch-linux-post-in-nano.jpg "what it looked like when I was writing this post")

Overall, installing, configuring and using Arch has been a blast.
I'd recommend it to anyone who is even remotely interested.
Installing it will teach you a lot about how everything works under the hood
which is always useful. The freedom of configuration lends itself to infinite
possibilities for customizing every aspect of your system, which I think most
people can enjoy.