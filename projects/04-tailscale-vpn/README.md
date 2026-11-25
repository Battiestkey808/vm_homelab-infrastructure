# Tailscale VPN — Remote Access to Ubuntu Server

## Project Overview

For this project, I configured Tailscale VPN to remotely access an Ubuntu Server virtual machine running on VMware Workstation.

My goal was to establish a secure connection between my Windows desktop and an Ubuntu Server hosted on a separate laptop, without exposing SSH directly to the public internet or configuring port forwarding.

This project is part of my personal homelab, where I am practicing Linux administration, networking, virtualization, and infrastructure management.

## Lab Environment

| Component | Configuration |
|---|---|
| Server OS | Ubuntu Server 24.04 LTS |
| Hypervisor | VMware Workstation |
| Server Host | Windows laptop |
| Remote Client | Windows desktop |
| Virtual Networking | VMware NAT (VMnet8) |
| VPN Solution | Tailscale |
| Remote Administration | OpenSSH |
| Authentication | Tailscale device authentication and Ubuntu SSH login |

## Network Architecture

The Ubuntu Server runs as a virtual machine on my laptop using VMware NAT networking.

Initially, I attempted to connect from my desktop using the Ubuntu VM's private VMware address. The connection timed out because the VMware NAT network was local to the laptop and wasn't directly reachable from my separate desktop.

To solve this, I installed Tailscale on Ubuntu and connected my Windows desktop to the same private tailnet.

The resulting connection was:

**Windows Desktop → Tailscale VPN → Ubuntu Server VM**

The laptop hosting the VM must remain powered on, and the Ubuntu VM must remain running for remote access to work.

## Step 1 — Install Tailscale

I installed Tailscale on Ubuntu using its official installation script.

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

After installation, I verified that Tailscale was available and its background service was running.

```bash
tailscale version
systemctl status tailscaled
```

## Step 2 — Authenticate Ubuntu with Tailscale

I connected Ubuntu to my Tailscale network using:

```bash
sudo tailscale up
```

Tailscale provided an authentication URL, which I opened in my browser to authorize the device.

After authentication, Ubuntu appeared in my tailnet.

## Step 3 — Verify the VPN Connection

I used:

```bash
tailscale status
tailscale ip -4
```

These commands confirmed that Ubuntu had received a Tailscale IPv4 address and could see my Windows device on the same tailnet.

## Step 4 — Connect the Windows Desktop

I installed Tailscale on my Windows desktop and authenticated using the same Tailscale account.

This allowed the desktop and Ubuntu Server to communicate through the private VPN, even though the Ubuntu VM was behind VMware NAT.

## Step 5 — Configure SSH Access

I used Ubuntu's OpenSSH server with systemd socket activation.

```bash
sudo systemctl start ssh.socket
```

I then verified the socket was active:

```bash
systemctl is-active ssh.socket
```

The socket was configured to accept incoming SSH connections.

## Step 6 — Connect Remotely

From Windows PowerShell, I connected to Ubuntu using its Tailscale IPv4 address:

```powershell
ssh <ubuntu-username>@<tailscale-server-ip>
```

After authentication, I was able to access the Ubuntu terminal remotely from my desktop.

This confirmed that the VPN connection and SSH remote administration were working.

## Troubleshooting

### Issue: SSH Connection Timed Out

My initial SSH attempts from the desktop to the Ubuntu VM's VMware NAT IP address failed.

**Cause:** The VMware NAT network existed on the laptop hosting the VM. The desktop was not directly connected to that virtual subnet.

**Resolution:** I configured Tailscale on both devices and connected using the Ubuntu Server's Tailscale IP address.

### Issue: Tailscale Command Not Recognized in PowerShell

Windows PowerShell initially could not locate the Tailscale executable by its command name.

I used the executable's full installation path to test connectivity:

```powershell
& "C:\Program Files\Tailscale\tailscale.exe" ping <tailscale-server-ip>
```

This allowed me to run the Tailscale connectivity test without changing the Windows PATH configuration.

## Results

I successfully established remote access from my Windows desktop to an Ubuntu Server virtual machine hosted on a separate laptop.

The connection works through Tailscale without requiring public SSH port forwarding.

## Skills Practiced

- Linux administration
- OpenSSH remote access
- VPN networking
- VMware NAT networking
- IPv4 addressing and routing
- Network connectivity troubleshooting
- systemd service and socket management
- Technical documentation

## Future Improvements

- Restrict SSH access to the Tailscale interface using appropriate firewall rules.
- Configure SSH key authentication for remote administration.
- Document Tailscale connectivity and routing diagnostics.
- Deploy monitoring services and access them remotely.
- Migrate selected services to a dedicated physical homelab server.
