---
title: "Using VNC on Xorg with TigerVNC"
date: 2023-03-06T21:22:23+07:00
publishDate: 2023-03-06T21:22:23+07:00
image: "/images/2023/TigerVNC_logo.webp"
url: "/content/using-vnc-xorg-with-tigervnc"
tags: ["Linux", "X11"]
draft: false
---

{{< figure src="/images/2023/TigerVNC_logo.webp" title="TigerVNC Logo" >}}

The protocol in X Window differs in terms of client and server roles. When X Window is running on the server, you can't "remote" it like you do with Remote Desktop on Windows or Mac. For instance, if you connect to a server running X Window from another Linux computer using X Window via X11 forwarding over `ssh`, the applications you run on the server (e.g., `xeyes`) will be displayed on your client desktop instead of displaying the entire desktop on your client computer. :joy:

To remotely access your desktop on the server, you need to use VNC[^1] or RDP[^2]. In this article, I will explain how to use VNC.

There's a well-known VNC for X11, x11vnc[^3], but it is currently unmaintained. As an alternative, I will be using TigerVNC[^4], but not its VNC server. Instead, I'll use its `libvnc.so` module for Xorg. In Ubuntu Linux (currently using Ubuntu 22.04 LTS), you can easily install the `tigervnc-xorg-extension` package with `sudo apt install tigervnc-xorg-extension`.

Next, you need to configure `xorg.conf`.

If you have never configured `xorg.conf`, in the latest Ubuntu, it is mostly not defined, as seen by the empty `/etc/X11/xorg.conf.d/` directory. Let's create an `xorg.conf` configuration.

Switch to text mode or remotely access the computer. Stop the Desktop Manager (e.g., `sddm`) and generate the X configuration (by default, it will be located at `/root/xorg.conf.new`, then copy it to `/etc/X11/xorg.conf.d/vnc_xorg.conf`).

```
$ sudo systemctl stop sddm
$ sudo X -configure
$ sudo mv /root/xorg.conf.new /etc/X11/xorg.conf.d/vnc_xorg.conf
```

Edit and configure the xorg.conf:

* Load the vnc module, which will use the `libvnc.so` file.
* Configure TigerVNC with `$SecurityTypes = "None";` (detailed in `/etc/tigervnc/vncserver-config-defaults`). This is to simplify; you can choose the authentication that suits you.
* Configure TigerVNC with `$localhost = "yes"`; (detailed in `/etc/tigervnc/vncserver-config-defaults`). This ensures the VNC server listens only on `localhost:5900` instead of `0.0.0.0:5900`.

```
Section "Module"
        ...
        Load  "vnc"
EndSection
...
Section "Screen"
        ...
        Option     "SecurityTypes" "None"
        Option     "localhost" "yes"
EndSection
```

Almost done. To access the VNC, use ssh with port forwarding, e.g., `ssh user@host.domain -L59000:localhost:5900`. `port` 59000 is on our client computer, and `localhost:5900` is on the server's localhost where Xorg listens for VNC.

By the way, related to this article, if you need to install a minimal version of Linux with X Window (in Bahasa Indonesia):

{{< article link="/content/gui-linux-seringkas-mungkin-dengan-lxqt-sddm-openbox/" >}}

Nice! Now we can access the X :0 desktop remotely! :fire:

[^1]: https://en.wikipedia.org/wiki/Virtual_Network_Computing
[^2]: https://en.wikipedia.org/wiki/Remote_Desktop_Protocol
[^3]: https://github.com/LibVNC/x11vnc
[^4]: https://github.com/TigerVNC/tigervnc
