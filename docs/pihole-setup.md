# Pi-hole Setup (Raspberry Pi 5)

## Overview
A Pi-hole DNS sinkhole running on a Raspberry Pi 5, set up as a network-wide 
ad/tracker blocker. 

## Hardware
- **Host**: Raspberry Pi 5

## Software stack
- **OS**: Debian
- **DNS sinkhole**: Pi-hole

## Status
Built and functional, but not currently in active use as the primary DNS 
resolver for the network.

## Setup steps
1. Installed Pi-hole on the Raspberry Pi 5 using the [official one-step automated install](https://github.com/pi-hole/pi-hole/#one-step-automated-install). I used the curl command directly in the terminal.
2. Configured DNS sinkhole rules / blocklists through the `http://pi.hole/admin` interface. I used:
   - StevenBlack/hosts
   - AdAway host list
   - Firebog Easylist

All 3 lists have their own merit and history, run by passionate volunteers.

## Lessons learned / notes
- For new devices on the network need to configure router to push Pi-Hole as the DNS, otherwise you have to do it manually.

## Future improvements
- [ ] Add Unbound for upstream DNS privacy
- [ ] Pair with the existing Nginx reverse proxy / Gitea setup, if still relevant
