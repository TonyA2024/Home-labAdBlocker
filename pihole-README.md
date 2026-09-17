# Network-Wide Ad & Tracker Blocking (Pi-hole)

A self-hosted DNS sinkhole running on a Raspberry Pi, providing network-wide ad, tracker, and malicious-domain blocking for every device on the network — and, via Tailscale, for devices off the network too.

## Overview

[Pi-hole](https://pi-hole.net/) intercepts DNS queries and blocks known ad, tracking, and malware domains before a connection is ever made — filtering happens at the DNS layer, so it covers every device on the network automatically, with no per-device browser extensions or apps required.

This project extends an existing Raspberry Pi home-lab setup (alongside a self-hosted photo server and password vault) into network-level privacy and security tooling.

## Architecture

- **Host:** Raspberry Pi, native install (official install script, no containerization)
- **Role:** DNS-only resolver — the router's DHCP settings are configured to hand out the Pi's address as the DNS server, so all client devices route DNS queries through it without individual configuration
- **Remote coverage:** Devices connected via [Tailscale](https://tailscale.com/) continue to resolve DNS through the Pi-hole instance even when off the home network, extending ad/tracker blocking and DNS-level filtering to mobile and remote use

```
Client device DNS query
        │
        ▼
Pi-hole (Raspberry Pi)
   ├── Blocklist match → blocked (sinkholed)
   └── No match → forwarded to upstream DNS resolver
```

## Features

- Network-wide ad and tracker blocking with no per-device configuration
- Query logging and a web dashboard for visibility into DNS activity and block rates
- Blocklists updated automatically to stay current against new ad/tracker domains
- Extends protection to remote devices via Tailscale, rather than only covering the home network

## Setup Summary

1. **Install Pi-hole** on the Raspberry Pi using the official install script:
   ```bash
   curl -sSL https://install.pi-hole.net | bash
   ```
2. **Configure the router's DHCP settings** to distribute the Pi's local IP as the primary DNS server for all connected devices.
3. **Join the Pi to a Tailscale network** and configure client devices to use it as their DNS resolver while connected to the tailnet, extending filtering beyond the home network.
4. **Access the dashboard** (`http://<pi-ip>/admin`) to monitor query logs, block rates, and manage allow/deny lists.

## Stack

`Pi-hole` · `DNS` · `Tailscale` · `Raspberry Pi OS`

## Future Improvements

- Add Unbound as a local recursive resolver, removing reliance on third-party upstream DNS providers entirely
- Set up a secondary Pi-hole instance for DNS redundancy
- Custom blocklist curation based on observed query logs
