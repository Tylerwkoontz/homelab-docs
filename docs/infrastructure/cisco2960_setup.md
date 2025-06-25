# Cisco 2960 Homelab Switch Setup + IOS Upgrade (LAN Base with SSH)

## Switch Model

* **Device**: Cisco Catalyst 2960 Plus Series (SI)
* **Original IOS**: `c2960-lanlitek9-mz.152-2.E5.bin` (LAN Lite — no SSH support)
* **Target IOS**: `c2960-lanbasek9-mz.152-7.E12.bin` (LAN Base — crypto/SSH support)

---

## Final Switch Configuration

### User + SSH Access

```plaintext
hostname HomelabSwitch
!
enable secret YourEnableSecret123
username admin privilege 15 secret YourSecurePass123
!
ip domain-name tkcloudstack
crypto key generate rsa
! (Choose 1024 bits)
ip ssh version 2
!
line vty 0 4
login local
transport input ssh
!
line con 0
login local
!
no ip http server
no ip http secure-server
!
interface vlan 1
 ip address 192.168.1.10 255.255.255.0
 no shutdown
!
ip default-gateway 192.168.1.1
!
interface range fa0/1 - 24
 no shutdown
!
write memory
```

---

## 🚀 IOS Upgrade Process Summary

### 1. ✅ **Download New IOS**

* File used: `c2960-lanbasek9-mz.152-7.E12.bin`
* Placed in: `/private/tftpboot/` on macOS

### 2. 🛠 **Start macOS TFTP Server**

```bash
sudo mkdir -p /private/tftpboot
sudo chmod 777 /private/tftpboot
sudo cp ~/Downloads/c2960-lanbasek9-mz.152-7.E12.bin /private/tftpboot/
sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist
```

### 3. 📡 **Assign IP to Switch + Enable Ports**

```bash
interface vlan 1
 ip address 192.168.1.10 255.255.255.0
 no shutdown
exit

interface range fa0/1 - 24
 no shutdown
exit

ip default-gateway 192.168.1.1
```

### 4. 📤 **Transfer IOS from Mac to Switch**

```bash
copy tftp: flash:
```

* Remote host: `192.168.1.51` (Mac IP)
* Source filename: `c2960-lanbasek9-mz.152-7.E12.bin`

### 5. 🧭 **Set Boot Image**

```bash
configure terminal
boot system flash:c2960-lanbasek9-mz.152-7.E12.bin
exit
write memory
```

### 6. 🔄 **Reload and Verify**

```bash
reload
show version
show ip ssh
```

---

## ✅ Post-Upgrade Status

* SSH v2 supported and enabled
* Console + VTY login secured
* Switch reachable at `192.168.1.10`
* VLAN 1 used for management
* Mac-based TFTP workflow confirmed functional

---

Let me know if you'd like a Markdown version or PDF export format for your repo/docs!
