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

## Network Architecture

### Diagram (Mermaid)
```mermaid
flowchart TB
  I[(Internet)]:::no -->|NO CONNECT| X{{WAN Disabled}}:::no
  R[Wi-Fi Router\n(LAN only)] --> L[Laptop]
  R --> P[Phone]
  R --> N[NAS Server\nStatic IP: 192.168.1.10]

classDef no fill:#ffdddd,stroke:#ff0000,color:#000;
