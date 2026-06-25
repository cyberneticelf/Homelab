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
1. Installed Pi-hole on the Raspberry Pi 5
2. Configured DNS sinkhole rules / blocklists
3. [Any additional steps — e.g. setting Pi-hole as the network's DNS server via DHCP]

## Lessons learned / notes
- [Why it fell out of active use — interesting context for a writeup: was it 
  reliability, convenience, replaced by something else?]

## Future improvements
- [ ] Bring back into active use as primary network DNS
- [ ] Add Unbound for upstream DNS privacy
- [ ] Pair with the existing Nginx reverse proxy / Gitea setup, if still relevant
