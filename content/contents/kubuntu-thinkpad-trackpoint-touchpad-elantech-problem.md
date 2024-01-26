---
title: "Kubuntu on ThinkPad with TrackPoint and TouchPad Elantech Problem"
date: 2024-01-25 17:18:00+07:00
publishDate: 2024-01-25 17:18:00+07:00
image: "/images/2024/kubuntu-jammy-jellyfish.webp"
url: "/content/kubuntu-thinkpad-trackpoint-touchpad-elantech-problem"
tags: ["Linux"]
draft: false

---

{{< figure src="/images/2024/kubuntu-jammy-jellyfish.webp" title="Kubuntu Jammy Jellyfish 22.04 LTS" alt="Kubuntu Jammy Jellyfish 22.04 LTS" position="center">}}

Since I work at Dicoding, I use a ThinkPad (yeah, kind of bored of using Mac).

{{< article link="/content/re-starting-career-educator-dicoding/" >}}

And, of course, I use Linux on that ThinkPad, as I am a fan of Kubuntu. :joy:

{{< article link="/content/add-new-resolution-xorg-kubuntu/" >}}

After years of no problems at all, today I found that the touchpad was not moving while I was working. At some point, I used a USB mouse.

Okay, let's get technical.

First, I thought that it was the device driver, so probably a new kernel caused the problem.
I found that my Kubuntu has several kernels with versions 5.15.xx and 6.5.xx.

```bash
$ apt list --installed | grep linux-image
```

So, I tried to reboot several times with different kernel versions, manually selected via `grub`. But, the problem still exists. :cry:

Let's try a different perspective, check what happened on `dmesg`.

```bash
$ sudo dmesg | grep mouse
```

Now, we can see some kernel messages, "probably" related to the kernel module psmouse, or there's a clue on the hardware name "probably" Elantech.

And then, of course, we move to ask Google. :joy:

The quick fix that I got was to remove the kernel module psmouse and add it again. [^1]

```bash
sudo modprobe -r psmouse && sudo modprobe psmouse proto=imps
```

But, um... yeah, still not perfect. I can't use the scroll. :confused:

On the other side, I found something with Elantech, information officially from Lenovo, the producer of ThinkPad. [^2] So there's a trick regarding `psmouse` for `Elantech` by setting the kernel with the parameter `psmouse.elantech_smbus=0`.

Digging deeper regarding `psmouse` and `Elantech`, I found that it's a bug in the Linux kernel [^3], which comes from years ago. It made a wrong protocol detection in the kernel configuration: `/sys/bus/serio/devices/serio1/protocol`.

So, as mentioned in [^2], adjusting the kernel parameter fixed my problem. Then, I made it permanent by adding the parameter to `GRUB_CMDLINE_LINUX_DEFAULT` in `/etc/default/grub` [^4].

Hope it helps.


[^1]: https://askubuntu.com/questions/1235067/touchpad-stopped-working-20-04 
[^2]: https://download.lenovo.com/pccbbs/mobiles_pdf/lenovo_thinkpad_p53_p73_debian10_installation_v1.0.pdf
[^3]: https://askubuntu.com/questions/1091635/elantech-touchpad-not-working-after-upgrading-to-18-10-from-18-04-thinkpad-l4
[^4]: https://askubuntu.com/questions/19486/how-do-i-add-a-kernel-boot-parameter
