# Tailscale with PiHole Adblocking
## Overview

This guide details how to install Tailscale on a PiHole LXC and route your DNS via your PiHole when connected via Tailscale. The guide from https://tailscale.com/kb/1114/pi-hole/ has been used but there are additional actions for it to work in an LXC.

## Modify PiHole LXC

You need to make modifications to your PiHole LXC to get it to run. Without this, Tailscale will not run. Within your Shell Console for your VE, you need to edit the PiHole LXC Configuration file (change the number to your relevant LXC conf):

```
$ nano /etc/pve/lxc/116.conf
```

Then add the following lines to the LXC Configuration file:

```
$ lxc.cgroup.devices.allow: c 10:200 rwm
$ lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
```

Then restart your PiHole LXC

