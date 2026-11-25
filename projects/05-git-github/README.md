# Project 05 — Git, GitHub, and SSH Authentication

## Overview

For this project, I configured Git on my Ubuntu Server and established SSH authentication with GitHub.

My goal was to create a version-controlled repository for documenting my Linux, networking, and infrastructure projects.

## Environment

| Component | Configuration |
|---|---|
| Operating System | Ubuntu Server 24.04 LTS |
| Git | 2.43.0 |
| Authentication | SSH Ed25519 key pair |
| Repository | vm_homelab-infrastructure |
| Hosting Platform | GitHub |

## Step 1 — Verify Git

```bash
git --version
```

This confirmed that Git was installed.

## Step 2 — Configure Commit Identity

```bash
git config --global user.name "Your Author Name"
git config --global user.email "YOUR-GITHUB-NOREPLY-EMAIL"
```

I configured my Git author identity and used GitHub's noreply email feature to avoid exposing my personal email in future commits.

## Step 3 — Generate an SSH Key

```bash
ssh-keygen -t ed25519 -C "ubuntu-homelab-github"
```

This generated an Ed25519 public/private key pair.

I protected the private key with a passphrase.

## Step 4 — Register the Public Key

I displayed the public key using:

```bash
cat ~/.ssh/id_ed25519.pub
```

I registered the public key in GitHub's SSH authentication settings.

The private key remained on Ubuntu and was not uploaded.

## Step 5 — Test GitHub Authentication

```bash
ssh -T git@github.com
```

GitHub confirmed that SSH authentication was successful.

## Step 6 — Clone the Repository

```bash
git clone git@github.com:Battiestkey808/vm_homelab-infrastructure.git
```

This downloaded the repository onto my Ubuntu Server.

## Step 7 — Verify the Remote

```bash
git remote -v
```

The output confirmed that the repository's `origin` remote used GitHub's SSH URL.

## Step 8 — Commit and Push Documentation

```bash
git add projects/04-tailscale-vpn/README.md
git commit -m "Document Tailscale VPN Remote Access Setup"
git push
```

These commands staged the documentation, created a commit, and uploaded it to GitHub.

## Results

I successfully connected Ubuntu Server to GitHub using SSH authentication and published my first homelab project documentation.

## Skills Practiced

- Git version control
- SSH key authentication
- Ed25519 cryptography
- Public and private key management
- GitHub repository management
- Git staging, commits, and pushes
- Technical documentation
