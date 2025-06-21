# Pi-hole Configuration Notes

This document outlines the rationale, setup process, and configuration details of the **Pi-hole DNS filtering service** running in my homelab.

[Detailed installation steps in Proxmox](proxmox_pihole_setup.md)


---

## Overview

**Pi-hole** is a network-level ad blocker that intercepts DNS queries and blocks known ad/tracking domains before they reach client devices.

### Why I Use It
- Reduces ads and tracking across all devices
- Improves page load times
- Helps monitor network-level DNS activity
- Protects against malicious domains

---

## Deployment Details

| Component     | Value                       |
|---------------|-----------------------------|
| Host          | `pve1`                      |
| Container     | LXC (Unprivileged, Nesting) |
| OS Template   | Debian 12 (Turnkey)         |
| IP Address    | `192.168.1.4` (static)    |
| Pi-hole Version | Installed via official curl script |
| Web UI Port   | `80`                        |

---

## Install Process (Inside LXC)

```bash
apt update && apt upgrade -y
curl -sSL https://install.pi-hole.net | bash
```
**During install:**
- Selected upstream DNS provider (Cloudflare)
- Set static IP manually (192.168.79.10)
- Enabled web admin interface
- Default blocklists were applied (for now)

---

## Admin Interface
Access via: http://192.168.1.2/admin

Set password:
```bash
pihole -a -p
```

---

## DNS Configuration
**Router-Level:**
- DHCP DNS server points to Pi-hole's IP
- IPv6 filtering disabled (for now)

**Client-Level (fallback):**
- Some devices manually configured to use Pi-hole as DNS resolver

---

## Monitoring & Logs
- Query log available in web UI
- Block/allow domains via dashboard
- Consider future integration with Grafana/InfluxDB for visual DNS monitoring

---

## Why LXC Instead of Docker?
- Easier networking setup on Proxmox
- Lower overhead than full VM
- Better integration with snapshotting and backup via Proxmox GUI

---

## Future Enhancements
- Replace or test with AdGuard Home
- Add fail2ban for admin UI brute-force protection
- Schedule regular Pi-hole config backups
- Expose via Tailscale or reverse proxy with Auth if remote access needed