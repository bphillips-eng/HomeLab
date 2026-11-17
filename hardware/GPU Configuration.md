 GPU Configuration

## Overview

This document covers the GPU hardware and configuration
used in my Proxmox homelab.

The primary GPU is used for hardware acceleration,
media transcoding, and planned local AI workloads.

## GPU Hardware

| Component | Specification |
|---|---|
| GPU | NVIDIA GeForce RTX 4070 Ti |
| VRAM | 12 GB |
| Host | Jumblenode3 |
| CPU | AMD Ryzen 5 5600X |
| Motherboard | ASUS TUF Gaming B550-PLUS WiFi II |
| RAM | 16 GB DDR4 |
| Storage | 6TB, 1tb, 3x 500gb |
| PSU | 850 W |
| Operating System | Proxmox VE |

## NVIDIA Driver Configuration

| Component | Version |
|---|---|
| NVIDIA Driver | 620.70.06 |
| CUDA Version Reported | 13.5 |
| Proxmox Kernel | 7.0.14-28-pve |

### Verify GPU Detection

Run the following command on the Proxmox host:

```bash
nvidia-smi
```

### Verify NVIDIA Kernel Modules

```bash
lsmod | grep nvidia
```

### Verify GPU Device Files

```bash
ls -l /dev/nvidia*
```

## NVIDIA UVM Configuration

During setup, the NVIDIA UVM device was initially
missing and required additional configuration.

The following command was used to initialize
the NVIDIA device files:

```bash
nvidia-modprobe -u -c=0
```

Verify the UVM device:

```bash
ls -l /dev/nvidia-uvm*
```

## Jellyfin Hardware Acceleration

Jellyfin runs in Proxmox LXC container 102.

### Verify GPU Access in Jellyfin

From the Proxmox host:

```bash
pct exec 102 -- nvidia-smi
```

### Verify GPU Access as Jellyfin User

```bash
pct exec 102 -- runuser -u jellyfin -- nvidia-smi
```

### Hardware Encoding

NVIDIA NVENC provides GPU-accelerated video encoding
for supported media formats.

FFmpeg was used to test NVIDIA hardware encoding
and validate GPU access.

## Troubleshooting

### Issue: NVIDIA UVM Device Missing

**Problem:**
The NVIDIA UVM device was not initially available. Then errors kept popping up after each possible fix.

**Solution:**
Initialized NVIDIA device files using nvidia-modprobe.

**Verification:**
Confirmed /dev/nvidia-uvm existed afterward.

### Issue: GPU Access Inside LXC

**Problem:**
Jellyfin required access to the host's GPU devices.

**Resolution:**
Configured NVIDIA device access and verified
that the Jellyfin service account could detect the GPU.

## Future Plans

- [ ] Deploy local AI inference workloads
- [ ] Monitor GPU usage and power consumption
- [ ] Document GPU configuration persistence after reboot
- [ ] Benchmark hardware transcoding performance

## Skills Demonstrated

- Linux GPU driver management
- Proxmox LXC configuration
- NVIDIA CUDA and NVENC
- GPU device troubleshooting
- Hardware-accelerated media transcoding
