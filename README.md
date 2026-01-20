# Secure-wirelesss-NAS-DIY
I built a NAS from scratch by disconnecting it completely from the internet through the whole building process using OMV as my Operating Software without sacrificing wireless capabilities through using an enclosed WIFI router to connect devices to my NAS. 2 x 1TB, 2x 512GB HDD for storage and a 1TB nvme as cache storage

# Secure Wireless NAS (Offline-First) — DIY

![Status](https://img.shields.io/badge/status-active-success)
![Security](https://img.shields.io/badge/focus-security-blue)
![Offline](https://img.shields.io/badge/network-air--gapped%20(no%20WAN)-critical)
![Built%20With](https://img.shields.io/badge/built%20with-OpenMediaVault-9cf)

A **secure, offline-first NAS** built from scratch using **OpenMediaVault (OMV)** and a dedicated **enclosed Wi-Fi router** to provide **local wireless access** without exposing the NAS to the public internet.

> **Core idea:** Keep the NAS **disconnected from WAN** during the entire build + operation, while still allowing trusted devices to connect over a **private Wi-Fi LAN**.

---

## Table of Contents
- [Overview](#overview)
- [Motivation](#motivation)
- [Threat Model](#threat-model)
- [Hardware](#hardware)
- [Software Stack](#software-stack)
- [Network Architecture](#network-architecture)
- [Security Controls](#security-controls)
- [Storage Design](#storage-design)
- [Build Process](#build-process)
- [Validation](#validation)
- [Limitations](#limitations)
- [Future Improvements](#future-improvements)
- [Use Cases](#use-cases)
- [License](#license)

---

## Overview
This project documents the design and implementation of a NAS system focused on:

- **Zero WAN exposure**
- **Reduced attack surface**
- **Local-only wireless access**
- **Owner-controlled data storage**

---

## Motivation
Many NAS setups are always online, increasing exposure to:

- remote exploitation
- brute-force attempts
- ransomware targeting exposed services
- cloud dependency and telemetry

This project explores a **minimal-attack-surface** approach:

- no WAN link
- no cloud services
- no external DNS reliance
- no remote access by default

---

## Threat Model

### ✅ Threats mitigated
- Internet-based attacks (no WAN route)
- Remote brute-force attempts (no exposed ports)
- Attack surface from remote services
- Unsafe “set it and forget it” remote access

### ⚠️ Not mitigated
- Physical access attacks
- Malicious local network users
- Hardware/firmware backdoors

---

## Hardware

| Component | Description |
|---|---|
| Platform | x86-based system |
| Storage (HDD) | 2 × 1TB (primary storage) |
| Storage (HDD/SSD) | 2 × 512GB (secondary/extra) |
| Cache | 1 × 1TB NVMe |
| Network | Dedicated Wi-Fi router (WAN unused) |

> Adjust the table to match your exact specs.

---

## Software Stack
- **OS:** OpenMediaVault (OMV)
- **File Sharing:** SMB/CIFS
- **Filesystem:** EXT4 (stability + recovery)
- **Cache:** NVMe-based caching (depending on OMV plugin/config)

---

---

## Security Configuration

### Network Security (Router)
- **WAN port unused** (no uplink to ISP modem / internet source)
- Router admin panel secured with a strong password
- **UPnP disabled**
- **Port forwarding disabled**
- **WPS disabled**
- Optional: isolate admin interface to a single trusted client MAC/IP

### NAS Hardening (OMV)
- Disable root SSH login (if SSH is enabled at all)
- Use strong unique passwords for all accounts
- Enable only the services you need (avoid extra plugins/services)
- Keep file sharing limited to LAN-only interfaces

### Access Control
- No guest access
- Create per-user accounts (or per-role accounts)
- Use group permissions for shared folders
- Restrict SMB/NFS access to the local subnet only

---

## Storage Design

### Layout
- HDDs used for primary data storage
- NVMe SSD configured as a cache/performance layer (if enabled via OMV plugins/tools)

### Rationale
- HDDs provide affordable capacity
- NVMe cache improves responsiveness and reduces latency for frequently accessed files
- EXT4 chosen for stability and predictable recovery behavior

> Update this section to match your exact configuration (RAID level, pooling, filesystem, cache mode).

---

## Build Process

### 1) Hardware Assembly
- Installed drives (HDDs + NVMe)
- Checked power delivery and SATA/NVMe detection
- Verified airflow/cooling to handle sustained disk activity

### 2) BIOS/UEFI Configuration
- Enabled **AHCI**
- Confirmed NVMe is detected
- Disabled unused boot devices (optional)
- Verified boot order (USB first for install, then OS drive)

### 3) OMV Installation (Offline)
- Installed OMV using a bootable USB
- **No internet connection during setup**
- Completed initial admin setup locally

### 4) Network Setup
- Connected NAS to the isolated router LAN
- Assigned a **static IP** (example: `192.168.1.10`)
- Verified that there is **no default route** to the internet

### 5) Storage Configuration
- Wiped/initialized disks
- Created filesystem(s) and mount points
- Configured cache layer (if applicable)

### 6) Services and Shares
- Enabled **SMB/CIFS**
- Created users + groups
- Created shared folders with proper permissions
- Tested access from laptop/phone over Wi-Fi

---

## Validation

### Isolation Tests
Confirm the NAS is not able to reach the public internet:
- No WAN cable connected
- No port forwarding / UPnP
- Attempt external DNS resolution / ping (should fail unless you intentionally provide local DNS)

### Functionality Tests
- Connect a laptop/phone to the router Wi-Fi
- Access SMB share via LAN IP
- Transfer files (single + multiple) and confirm stability
- Reboot NAS/router and confirm it still works without internet

---

## Limitations
- No remote access (by design)
- Manual maintenance/updates
- Requires local access for administration and troubleshooting

---

## Future Improvements
- Optional VPN gateway (still offline-by-default; only enabled when needed)
- Full disk encryption (LUKS) for data-at-rest protection
- Monitoring + alerts (LAN-only dashboards)
- Automated local backups (offline rotation drive + checksum verification)

---

## Use Cases
- Personal secure storage
- Academic data archiving
- Offline media server
- Low-trust environments where WAN exposure is unacceptable

---

## Conclusion
This project shows that a NAS can be **secure and practical without being internet-connected**. By enforcing isolation at the network level and minimizing services, the system reduces attack surface while maintaining convenient wireless access locally.

---

## License
MIT

---

## Author
**Ali Amhaz**  
Computer Engineering Student
