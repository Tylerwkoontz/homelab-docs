# Proxmox Pi-hole Setup (LXC) – With Explanations
[Back to Pi-hole overview and rationale](config_notes.md)

---

## 1. Download a Base LXC Template

**Proxmox Web UI →** `pve1` → `local` → **CT Templates** → **Templates** → Download **Debian 12** or **Ubuntu 22.04**

**Why:**  
- Minimal, stable, widely supported.
- Debian is Pi-hole recommended.
- Needed to create containers.

---

## 2. Create an LXC Container

In the Proxmox Web UI:

- **Unprivileged Container**: ✅ Checked  
  _Why:_ Improves security by isolating container root from host root.
- **Features > Nesting**: ✅ Checked  
  _Why:_ Needed for systemd and services like dnsmasq.
- **Disk Size**: 4–8GB (ZFS-backed)
- **CPU / RAM**: 1 vCPU, 512MB–1GB RAM
- **Network**: DHCP (we'll set static IP later)
- **Container Name**: `pihole`

---

## 3. Set Static IP (Recommended)

Access console:

```bash
pct console 100  # Replace 100 with your CT ID
```

Edit interfaces (Debian example):

```bash
nano /etc/network/interfaces
```

```text
auto eth0
iface eth0 inet static
  address 192.168.79.10
  netmask 255.255.255.0
  gateway 192.168.79.1
  dns-nameservers 1.1.1.1 8.8.8.8
```

```bash
reboot
```

**Why:**  
Avoids DNS failure if Pi-hole’s IP changes.

---

## 4. Install Pi-hole

SSH in or use `pct exec`:

```bash
apt update && apt upgrade -y
apt install curl -y
curl -sSL https://install.pi-hole.net | bash
```

Follow prompts:
- Choose **static IP**
- Choose **DNS provider**
- Enable blocklists
- Enable web interface and lighttpd

---

## 5. Set Web UI Password

```bash
pihole -a -p
```

---

## 6. Access Web Interface

Visit:
```
http://<container-ip>/admin
```

Login with the password you set.

---

## 7. Configure Router to Use Pi-hole

### Option A – DHCP DNS:

- Set LAN DNS to `192.168.79.10`

### Option B – Static DNS per device

**Why:**  
This routes all network DNS through Pi-hole.

---

## 8. Secure and Harden

```bash
apt install unattended-upgrades -y
```

Add blocklists, optionally:
- Back up `/etc/pihole/`
- Consider `PiVPN` if remote access is needed

---

## 9. Add Fail2Ban (Optional)

```bash
apt install fail2ban -y
```

---

## 10. Test Pi-hole

```bash
nslookup google.com 192.168.79.10
```

Should reply from Pi-hole.

---

## 📋 Summary Table

| Step | What You Do | Why You Do It |
|------|-------------|----------------|
| 1 | Download Template | Base image needed |
| 2 | Create LXC | Lightweight, secure |
| 3 | Assign Static IP | DNS reliability |
| 4 | Install Pi-hole | Setup DNS blocker |
| 5 | Set Password | Secures web UI |
| 6 | Configure Router | Routes traffic |
| 7 | Harden System | Keeps it updated |
| 8 | Enable Fail2Ban | Adds login protection |
| 9 | Test | Verifies it works |