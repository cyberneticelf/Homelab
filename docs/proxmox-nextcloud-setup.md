# Proxmox + Nextcloud Homelab

## Overview
A self-hosted Nextcloud instance running in a Proxmox VE LXC container 
(TurnKey Debian 12 template), with a 4TB external hard drive providing 
the actual file storage, bind-mounted into the container.

## Hardware
- **Host**: Repurposed laptop running Proxmox VE, 16GB RAM, 250GB NVMe
- **Storage**: 4TB external HDD, bind-mounted into the container
- **Container**: LXC, TurnKey Debian 12

## Software stack
- **Hypervisor**: Proxmox VE 9.1.9, kernel 7.0.0-3-pve
- **Container template**: TurnKey Linux Debian 12 (Nextcloud appliance)
- **Filesystem**: ext4

## Setup steps
1. Installed and updated Proxmox VE — disabled the enterprise repo, 
   switched to the no-subscription repo for Debian trixie, fixed DNS
2. Set Datacenter-level firewall rules: allow SSH and Web UI access from 
   the local subnet only, drop everything else
3. Tried a popular community helper script for a quick Nextcloud LXC — 
   discovered the script repo had been discontinued, so built the 
   container manually instead using the TurnKey Debian 12 template
4. Connected the 4TB external drive and mounted it on the host
5. Bind-mounted that path into the Nextcloud container
6. Set the Nextcloud data directory and trusted domain accordingly
7. Connected the Nextcloud desktop sync client

## Lessons learned / notes

**The drive started as NTFS — had to reformat to ext4**
The 4TB drive was originally NTFS (carried over from a previous Windows 
use). NTFS doesn't play well with Linux's permission model for this kind 
of setup, so it had to be unmounted from the container, unmounted on the 
host, and reformatted to ext4 before continuing.

**The real troubleshooting story: unprivileged container UID mapping**
This was the trickiest part of the whole build, and the most useful thing 
to understand for anyone doing something similar:

- After setting up the bind mount and pointing Nextcloud at the new data 
  directory, file uploads failed with "you don't have permission to 
  create files here"
- Setting ownership to `www-data:www-data` on the host looked correct — 
  but inside the container, the same files showed up as `nobody:nogroup`
- The root cause: **Proxmox unprivileged LXC containers remap UIDs**. 
  The host's `www-data` (uid 33) does *not* map to `www-data` inside an 
  unprivileged container — it maps to a shifted UID instead, as part of 
  Proxmox's UID-shifting security model that keeps container users from 
  matching host users 1:1
- The fix: `chown` the mount point on the **host** side to the shifted 
  UID, which then correctly appeared as `www-data:www-data` from inside 
  the container
- Even after that, uploads still failed — because Nextcloud's user data 
  folder structure didn't exist yet on the fresh mount. Had to manually 
  create it and run `occ files:scan` to register it
- Once both the UID mapping and the folder structure were correct, 
  uploads worked and the desktop sync client connected successfully

**Why this matters beyond just "it works now"**
This is a genuinely common trap with Proxmox unprivileged containers — 
permissions that look completely correct on the host can still fail 
inside the container because of UID remapping. Worth understanding the 
*why* here, not just copy-pasting a `chown` command.

## Access
Currently local network only. No external/internet access configured yet.

## Future improvements
- [ ] Remote access (Tailscale, or port forwarding + Let's Encrypt with a domain)
- [ ] Automated backups for the Nextcloud data directory
- [ ] Monitoring/alerting
