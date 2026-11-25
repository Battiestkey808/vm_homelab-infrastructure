# Virtualized Homelab Infrastructure

A personal homelab project focused on Linux system administration, networking, virtualization, containerization, VPN connectivity, and infrastructure troubleshooting.

## About This Project

I started this homelab to develop hands-on experience with technologies used in network engineering, systems administration, and IT infrastructure.

Instead of immediately purchasing a dedicated physical server, I decided to build a virtualized environment using VMware Workstation and Ubuntu Server.

This allows me to practice deploying services, configuring Linux, troubleshooting network connectivity, and documenting the process.

The repository contains my configurations, technical documentation, troubleshooting experiences, and project results.

## Homelab Environment

| Component | Configuration |
|---|---|
| Host System | Windows laptop |
| Hypervisor | VMware Workstation |
| Server OS | Ubuntu Server 24.04 LTS |
| Virtual CPU | 4 vCPUs |
| Memory | 8 GB |
| Virtual Disk | 80 GB |
| Root Logical Volume | 39 GB |
| Application Logical Volume | 25 GB |
| Network Mode | VMware NAT |
| Remote Access | Tailscale VPN and OpenSSH |
| Container Platform | Docker Engine and Docker Compose |
| Version Control | Git and GitHub |

## Completed Projects

| Project | Description |
|---|---|
| [01 — Ubuntu Server & VMware](projects/01-ubuntu-server/README.md) | Virtual machine configuration, Linux administration, and network verification |
| [02 — Linux Storage & LVM](projects/02-linux-storage-lvm/README.md) | Logical volumes, ext4, persistent mounts, and filesystem troubleshooting |
| [03 — Docker Installation](projects/03-docker-installation/README.md) | Docker Engine, Docker Compose, repositories, and container verification |
| [04 — Tailscale VPN](projects/04-tailscale-vpn/README.md) | Secure remote access between a Windows desktop and Ubuntu VM |
| [05 — Git & GitHub](projects/05-git-github/README.md) | Git configuration, SSH authentication, commits, and repository management |

## Network Architecture

The Ubuntu Server runs inside VMware Workstation using NAT networking.

Tailscale provides private connectivity between the Ubuntu virtual machine and a separate Windows desktop.

```text
Windows Desktop
      |
      | Tailscale VPN
      |
Ubuntu Server VM
      |
VMware Workstation
      |
Windows Laptop
      |
Internet Connection
```

This is a simplified logical diagram. The Ubuntu VM uses the laptop's network connection through VMware NAT, while Tailscale provides secure connectivity between devices.

## Skills and Technologies

**Linux Administration:** Ubuntu Server, systemd, SSH, APT, Linux filesystems, and terminal utilities.

**Networking:** IPv4, NAT, DNS, routing, VPNs, network troubleshooting, and virtual networking.

**Virtualization:** VMware Workstation, virtual CPU and memory allocation, virtual disks, and network adapters.

**Storage:** LVM, ext4, mount points, UUIDs, and `/etc/fstab`.

**Containers:** Docker Engine, Docker Compose, images, containers, and persistent storage planning.

**Version Control:** Git, GitHub, SSH authentication, commits, and technical documentation.

## Future Projects

- Deploy Uptime Kuma for service monitoring.
- Configure secure remote access and SSH authentication improvements.
- Deploy Pi-hole for DNS-based ad blocking.
- Set up Jellyfin for media streaming.
- Experiment with Linux web hosting.
- Explore cloud VPS hosting and hybrid infrastructure.
- Migrate selected services to a dedicated mini PC server.

## Documentation Approach

Each project includes its objectives, environment, implementation steps, verification commands, troubleshooting, and lessons learned.

The goal is to build a reproducible record of my work and demonstrate practical infrastructure skills.

## Project Status

This homelab is an ongoing learning environment. New projects and improvements will be added as I develop my Linux, networking, and systems administration skills.

