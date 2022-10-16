# Tailscale with PiHole Adblocking
## Overview

This guide details how to install Tailscale on a PiHole LXC and route your DNS via your PiHole when connected via Tailscale. The guide from https://tailscale.com/kb/1114/pi-hole/ has been used but there are additional actions for it to work in an LXC.

## Modify PiHole LXC

**You need to make modifications to your PiHole LXC to get it to run. Without this, Tailscale will not run; this is not in the Tailscale guide linked above!** Within your Shell Console for your VE, you need to edit the PiHole LXC Configuration file (change the number to your relevant LXC conf):

```
$ nano /etc/pve/lxc/116.conf
```

Then add the following lines to the LXC Configuration file:

```
$ lxc.cgroup.devices.allow: c 10:200 rwm
$ lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
```

Then restart your PiHole LXC.

## Install Tailscale on PiHole LXC

In the PiHole LXC console, run the following command:

```
$ curl -fsSL https://tailscale.com/install.sh | sh
```

Once successfully installed, then run the following, noting the addition of *--accept-dns=false*.

```
$ tailscale up --accept-dns=false
```

Follow the onscreen instructions and copy the URL into a browser to authorise the Tailscale connection. Once confirmed, your Tailscale PiHole device shoudl be visible in your *Machines* overview of the Tailscale web portal: https://login.tailscale.com/admin/machines

## Modify Tailscale & PiHole for DNS Ad Blocking
#### DNS Override

You can configure DNS for your entire Tailscale network from Tailscale’s admin console. Go to the **DNS** page and enter your PiHole Tailscale IP address as a **Global Nameserver**.

Since we want our network-wide DNS to override any local DNS settings that devices have, make sure you enable the Override local DNS toggle after adding your PiHole's Tailscale IP address.

#### PiHole Permit All Origins
In the Pi-hole Admin page in **Settings** > **DNS**, make sure that within **Interface Settings**, **'Permit all origins'** is selected.

Tailscale traffic comes in on the tailscale0 network interface, so this option is needed to allow your Pi-Hole to respond to Tailscale-based DNS traffic. When using this option, make sure your Pi-Hole is properly firewalled.
