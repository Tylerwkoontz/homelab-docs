# Proxmox Setup & ZFS RAID Configuration

## Overview

This document outlines the installation and configuration of `pve1`, the primary Proxmox node in my homelab. It includes the setup of Proxmox VE 8.4.1 using a macOS-flashed USB, configuration of the Dell H730 Mini controller in HBA (IT) mode, and the deployment of a ZFS mirror (RAID 1) across two 2TB SAS drives. ZFS automatic snapshots have been enabled to provide versioned backups and rollback points.

This setup prioritizes data integrity, service reliability, and future scalability. All steps are documented to ensure clarity.

---

## System Summary
- **Node:** pve1
- **Hardware:** Dell PowerEdge R430
- **Drives:** 2 × 2TB SAS
- **Proxmox Version:** 8.4.1
- **Storage Type:** ZFS Mirror (RAID 1)

---

## Step-by-Step Installation

### 1. Flash Proxmox ISO to USB
Used macOS terminal tools instead of GUI software:
```bash
# Convert the ISO to a raw disk image format (.dmg) suitable for writing to USB
hdiutil convert -format UDRW -o proxmox-ve_8.4-1.dmg ~/Downloads/proxmox-ve_8.4-1.iso

# List all disks to identify the USB drive (e.g., /dev/disk2)
diskutil list

# Unmount the USB drive so it can be written to directly
diskutil unmountDisk /dev/disk2

# Write the .dmg image to the USB drive using raw access (note the 'r' in rdisk2 for faster write)
sudo dd if=proxmox-ve_8.4-1.dmg of=/dev/rdisk2 bs=1m
```
**Why:** Using the macOS CLI gives more control over the flashing process, avoids GUI tools, and allows installing Proxmox directly from a clean bootable USB. Writing the ISO to USB ensures the OS lives on its own device, keeping the primary drives available for dedicated ZFS storage.

### 2. Configure BIOS and Storage Controller
- Set USB as the primary boot device to ensure the Proxmox installer loads first.
- Configured the H730 Mini RAID controller into **HBA (IT) mode** to allow ZFS direct access to raw disks.
- Verified both 2TB drives were exposed individually (non-RAID) to the Proxmox installer.

**Why:** ZFS requires direct access to physical disks for redundancy and integrity features. HBA mode bypasses the RAID logic, making the drives visible as raw block devices to the OS without removing the controller.

---

## ZFS RAID 1 Setup
### 1. Created ZFS Mirror Pool
Used the Proxmox web UI during installation:
- Selected **ZFS RAID 1 (mirror)** using `/dev/sda` and `/dev/sdb`
- Accepted the **default pool name**: `rpool`

**ZFS provides:**
- **Built-in redundancy** via mirroring (RAID 1)
- **Data integrity** through checksumming and self-healing
- **Efficient snapshots** for backups and rollback
- **SMART integration** to monitor drive health and predict failure (`zpool status` and `smartctl` both supported natively)

**Note to self (future plan):**  
Once the remaining 6 drives are installed, plan to:
- Expand to a **RAID-Z2 or multiple mirror vdevs**
- Create a **separate storage pool** for high-capacity or tiered storage use
- Consider setting up **ZFS alerts** and monitoring via Grafana or Prometheus

---

## ZFS Automatic Snapshots
### Tool Used
Installed `zfs-auto-snapshot`, which adds automated, scheduled snapshots using system cron jobs.
```bash
apt update
apt install zfs-auto-snapshot -y
```
### Default Retention Policy
Once installed, snapshots are created and rotated automatically:
- Hourly: 24 retained
- Daily: 7 retained
- Weekly: 4 retained
- Monthly: 12 retained

Snapshots are stored within the same dataset and do not consume extra space unless data changes (copy-on-write).

**Verify Snapshots**
To view existing snapshots:
```bash
zfs list -t snapshot
```
**Why It Matters**
- Provides automatic rollback points for VMs and datasets
- Enables rapid recovery from misconfiguration, corruption, or deletion
- Integrates with ZFS-native features like rollback and replication
- Supports SMART-based storage and future offsite backup plans

**Note to Self**
- Plan to integrate snapshot monitoring with Grafana or Prometheus
- Future: Automate off-node snapshot syncing for cold storage redundancy
  
**Optional:** Exclude specific datasets with:
```bash
zfs set com.sun:auto-snapshot=false rpool/data/iso-store
```

---

## Notes
- Proxmox 8.4.1 was installed via a bootable USB created using macOS CLI tools (`dd`, `hdiutil`) for maximum control and precision.
- The Dell H730 Mini controller was left installed but switched to **HBA (IT) mode**, allowing Proxmox/ZFS direct access to raw disks.
- No hardware RAID is used — **ZFS software RAID 1 (mirror)** provides redundancy, integrity checking, and snapshot capability.
- Automatic snapshots (`zfs-auto-snapshot`) are scheduled hourly, daily, weekly, and monthly using cron, enabling lightweight rollback and recovery.
- **SMART monitoring** is natively supported in ZFS; future plans include logging and alerting via Grafana/Prometheus stack.
- ZFS snapshots are copy-on-write and live within the same datasets, requiring minimal space unless data changes.
- All configurations were documented from the beginning to ensure traceability, reproducibility, and showcase-level professionalism.
- Plans to expand storage to **RAID-Z2 or multiple vdev mirrors** with six additional drives for higher capacity and resilience.
- Future snapshot syncing and backup strategy will include **off-node replication** or **cold storage**, improving disaster recovery posture.
