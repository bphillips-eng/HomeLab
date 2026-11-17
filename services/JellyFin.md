# Jellyfin Media Server

## Project Overview
Deployed a Jellyfin media server inside a
Proxmox LXC container with NVIDIA GPU
hardware acceleration and shared NAS storage.

## Technologies
- Proxmox VE
- Linux LXC
- Jellyfin
- NVIDIA GPU
- TrueNAS
- SMB/CIFS

## Implementation
1. Created and configured the LXC container.
2. Mounted the shared NAS media directory.
3. Configured access to NVIDIA GPU devices.
4. Enabled hardware transcoding in Jellyfin.
5. Tested GPU encoding functionality.
6. Verified media playback.

## Validation
# On the Proxmox host: test GPU visibility inside CT 102
pct exec 102 -- runuser -u jellyfin -- nvidia-smi

# Inspect the host's media mount
findmnt /mnt/truenas-media

# Verify the container media path exists
pct exec 102 -- ls -la /mnt/media

## Troubleshooting
Encountered playback errors with incomplete
media downloads.

Investigated:
- Media file availability
- NAS mount status
- GPU device permissions
- Transcoding functionality

## Skills Demonstrated
- Linux administration
- Container configuration
- GPU acceleration
- Network storage
- Troubleshooting

## Lessons Learned
Document the root cause of each failure,
the commands used to investigate it,
and the final solution.
