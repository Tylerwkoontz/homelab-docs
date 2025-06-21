## Homelab Documentation

Welcome to my personal homelab documentation repository. This repo tracks the setup, configuration, and operational practices for my self-hosted infrastructure — designed with production-level standards in mind.

---

## Overview

This homelab is built on:

- **Proxmox VE 8.4.1** as the hypervisor
- **ZFS (RAID 1)** for resilient local storage
- **Automated snapshot management**
- Containerized services like **Pi-hole**, **Grafana**, and more
- Full documentation built with **MkDocs + Material Theme**

---

## Folder Structure

```
homelab-docs/
├── docs/
│ ├── index.md # Homepage for MkDocs
│ ├── guides/
│ │ └── mkdocs_deploy_guide.md # How to deploy this doc site
│ ├── infrastructure/
│ │ └── proxmox_setup.md # ZFS, RAID1, BIOS setup
│ ├── services/
│ │ └── pihole/
│ │ └── config_notes.md # Pi-hole LXC container setup
│ ├── monitoring/
│ │ └── grafana_setup.md # Grafana, uptime monitoring stack
├── mkdocs.yml # MkDocs site configuration
├── .gitignore # Git ignored files
└── README.md # GitHub landing page (this file)
```

---

## Live Docs

View the full, browsable documentation site:

🔗 [https://tylerwkoontz.github.io/homelab-docs/](https://tylerwkoontz.github.io/homelab-docs/)

---

## Goals

- ✅ Learn and document real-world infrastructure practices
- ✅ Host and monitor my own stack
- ✅ Build a project portfolio for future cloud/infrastructure roles
- ✅ Make my setup reproducible for others

---

## Highlights

- ZFS mirror with automatic snapshotting
- Clean documentation flow with MkDocs + Material
- Air-gapped service isolation via containers
- Everything reproducible, version-controlled, and platform-ready

---

## Future Plans

- Add Ansible playbooks for provisioning
- Set up off-site snapshot syncing
- Integrate Prometheus & Grafana alerting
- Deploy more services: Vaultwarden, Jellyfin, Home Assistant

---

## Contact

> Built and maintained by [Tyler Koontz](https://github.com/Tylerwkoontz)
