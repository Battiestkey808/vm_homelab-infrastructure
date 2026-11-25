# Project 02 — Linux Storage Management with LVM

## Overview

For this project, I configured Logical Volume Management (LVM) on my Ubuntu Server virtual machine.

The goal was to create a separate filesystem for application data while keeping the Ubuntu operating system on its existing root logical volume.

This project also gave me hands-on experience with ext4 formatting, filesystem mounting, persistent mount configuration, and troubleshooting UUID errors.

## Lab Environment

| Component | Configuration |
|---|---|
| Operating System | Ubuntu Server 24.04 LTS |
| Virtual Disk | 80 GB |
| Volume Group | ubuntu-vg |
| Root Logical Volume | ubuntu-lv |
| Application Logical Volume | apps-lv |
| Application Filesystem | ext4 |
| Application Mount Point | /srv/apps |

## Storage Layout

| Logical Volume | Size | Purpose |
|---|---|---|
| ubuntu-lv | 39 GB | Ubuntu root filesystem |
| apps-lv | 25 GB | Persistent application data |
| Unallocated LVM Space | Approximately 14 GB | Future expansion |

## Step 1 — Inspect LVM

```bash
sudo pvs
sudo vgs
sudo lvs
```

These commands display physical volumes, volume groups, and logical volumes.

I discovered that my Ubuntu installation had approximately 39 GB of unallocated space in its volume group.

## Step 2 — Create an Application Logical Volume

```bash
sudo lvcreate -L 25G -n apps-lv ubuntu-vg
```

This created a 25 GB logical volume named `apps-lv` within the `ubuntu-vg` volume group.

## Step 3 — Format the Volume

```bash
sudo mkfs.ext4 /dev/ubuntu-vg/apps-lv
```

This created an ext4 filesystem on the new logical volume.

**Warning:** This is a formatting command and should only be used on the intended empty volume.

## Step 4 — Create the Mount Point

```bash
sudo mkdir -p /srv/apps
```

This created the directory that would provide access to the new filesystem.

## Step 5 — Mount the Filesystem

```bash
sudo mount /dev/ubuntu-vg/apps-lv /srv/apps
```

I then verified the filesystem using:

```bash
df -h /srv/apps
lsblk -f
```

The output confirmed that the 25 GB application filesystem was mounted at `/srv/apps`.

## Step 6 — Configure Persistent Mounting

I retrieved the filesystem UUID using:

```bash
sudo blkid /dev/ubuntu-vg/apps-lv
```

I then edited `/etc/fstab` to configure automatic mounting during startup.

The entry followed this format:

```text
UUID=<filesystem-uuid> /srv/apps ext4 defaults 0 2
```

The actual UUID is intentionally omitted from this public documentation.

## Step 7 — Verify the Configuration

```bash
sudo systemctl daemon-reload
sudo findmnt --verify
sudo mount -a
findmnt /srv/apps
```

These commands reloaded systemd configuration, verified the filesystem table, processed mount entries, and displayed the mounted filesystem.

## Troubleshooting — UUID Mount Failure

During verification, Ubuntu reported that the configured filesystem UUID could not be found.

I investigated using:

```bash
sudo blkid /dev/ubuntu-vg/apps-lv
sudo lvs
findmnt /srv/apps
ls -l /dev/disk/by-uuid/
```

The logical volume and ext4 filesystem were present, but the `/etc/fstab` entry contained an incorrect UUID.

I confirmed that the filesystem could be mounted directly through its LVM device path, corrected the UUID entry, and repeated the verification process.

The final verification reported zero errors.

## Results

I successfully configured a separate 25 GB ext4 filesystem for application data.

After rebooting Ubuntu, I confirmed that `/srv/apps` mounted automatically.

Approximately 14 GB of unallocated LVM capacity remains available for future projects.

## Skills Practiced

- Linux Logical Volume Management
- Physical volumes, volume groups, and logical volumes
- ext4 filesystems
- Mount points
- Persistent filesystem mounting
- UUID identification
- `/etc/fstab` configuration
- Linux storage troubleshooting

## Related Project

[Project 03 — Docker Installation](../03-docker-installation/README.md)
