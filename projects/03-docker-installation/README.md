# Project 03 — Docker Engine and Docker Compose Installation

## Overview

I installed Docker Engine and Docker Compose on Ubuntu Server 24.04 LTS to prepare my homelab for containerized applications.

The goal was to understand Linux package management, software repositories, systemd services, Docker images, and containers.

## Environment

| Component | Configuration |
|---|---|
| OS | Ubuntu Server 24.04 LTS |
| Hypervisor | VMware Workstation |
| Docker Engine | 29.10.1 |
| Docker Compose | v5.0.0 |
| Application Storage | /srv/apps |

## Step 1 — Update Package Information

```bash
sudo apt update
```

This refreshed Ubuntu's package lists.

## Step 2 — Install Prerequisites

```bash
sudo apt install ca-certificates curl
```

The packages provide trusted HTTPS certificates and a command-line tool for downloading data.

## Step 3 — Add Docker's Signing Key

```bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

These commands prepared the repository key directory, downloaded Docker's signing key, and configured its permissions.

## Step 4 — Configure Docker's Repository

I created `/etc/apt/sources.list.d/docker.sources` with the following configuration:

```text
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: noble
Components: stable
Architectures: amd64
Signed-By: /etc/apt/keyrings/docker.asc
```

This configured Docker's official stable APT repository for Ubuntu 24.04.

I then refreshed the package lists.

```bash
sudo apt update
```

## Step 5 — Install Docker

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

This installed Docker Engine, the Docker CLI, containerd, Buildx, and Docker Compose.

## Step 6 — Verify the Docker Service

```bash
sudo systemctl status docker
```

The service reported `active (running)` and was enabled at startup.

## Step 7 — Verify Installed Versions

```bash
docker --version
docker compose version
```

The commands confirmed that Docker Engine and Docker Compose were installed.

## Step 8 — Run a Test Container

```bash
sudo docker run --rm hello-world
```

Docker downloaded the `hello-world` image from Docker Hub, created a container, executed it, and displayed a success message.

## Results

Docker Engine and Docker Compose were installed successfully.

The test container executed without errors, confirming that Docker could retrieve images and run containers.

## Skills Practiced

- Linux APT package management
- Software repository configuration
- Repository signing keys
- systemd service management
- Docker Engine
- Docker Compose
- Container images and execution
- Linux command-line troubleshooting

## Next Steps

Deploy Uptime Kuma using Docker Compose and store its persistent application data on `/srv/apps`.
