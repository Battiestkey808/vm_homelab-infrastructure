# Project 01 — Ubuntu Server & VMware Workstation

## Overview

I set up an Ubuntu Server virtual machine using VMware Workstation on my Windows laptop to create a personal homelab for learning Linux administration, networking, virtualization, and self-hosted services.

I decided to begin with a virtual machine rather than purchasing a dedicated server so I could practice configuring Linux and deploying applications without additional hardware expenses.

## Objectives

- Build a Linux server environment using VMware.
- Configure virtual CPU, memory, and storage resources.
- Understand VMware NAT networking.
- Verify network connectivity and DNS resolution.
- Prepare the server for Docker, VPNs, and future applications.

## Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | VMware Workstation |
| Host | Windows laptop |
| Operating System | Ubuntu Server 24.04 LTS |
| CPU | 4 virtual CPUs |
| RAM | Approximately 8 GB |
| Virtual Disk | 80 GB |
| Network | VMware NAT (VMnet8) |
| Linux Interface | ens33 |

## System Verification

### Operating System

```bash
cat /etc/os-release
```

This command displays the Linux distribution and version information.

### CPU

```bash
nproc
```

The output confirmed that four virtual CPUs were available.

### Memory

```bash
free -h
```

This displays total, used, free, and available system memory.

### Storage

```bash
lsblk
```

This lists block devices, partitions, and logical volumes.

### Network Interfaces

```bash
ip -br addr
```

This displays a brief summary of the server's network interfaces and IP addresses.

### Routing

```bash
ip route
```

This displays the routing table, including the default gateway.

## Network Connectivity

I tested outbound internet connectivity using:

```bash
ping -c 4 1.1.1.1
```

The test completed with zero packet loss.

I also verified DNS resolution using:

```bash
getent hosts ubuntu.com
```

The system successfully resolved the domain name.

## Troubleshooting

During remote SSH configuration, I discovered that the VMware VMnet8 host adapter was disabled on Windows.

This prevented the Windows laptop from reaching the Ubuntu VM over the VMware NAT network.

After enabling VMnet8, local SSH connectivity was restored.

I also learned that VMware NAT addresses are local to the host computer and cannot normally be reached directly from a separate desktop computer.

I later used Tailscale VPN to provide secure remote access between the devices.

## Results

The Ubuntu Server virtual machine was successfully configured and verified.

The VM now serves as the foundation for my homelab projects involving LVM, Docker, GitHub, Tailscale, and future monitoring services.

## Skills Practiced

- Linux system administration
- VMware virtualization
- Network interface configuration
- IPv4 addressing and routing
- NAT networking
- DNS troubleshooting
- Linux command-line utilities

## Related Project

[Project 02 — Linux Storage and LVM](../02-linux-storage-lvm/README.md)
