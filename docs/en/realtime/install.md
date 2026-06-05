---
title: Installation
sidebar_position: 1
---

## Installation

The real-time reconstruction SDK is separate from the post-processing SDK and must be installed independently. It supports Windows and Linux with the same environment requirements as the post-processing SDK.

### Windows Installation

A local development package is provided for Windows. Contact MipMap to obtain the local development package.

### Linux Installation

On Linux, deployment is supported only in Docker. For environment preparation before deployment, see [Linux deployment environment preparation](../overview/linux-deployment#environment-setup).

### Pull the Image

```bash
docker pull registry.mipmap3d.com/docker/mipmap/realtime:v1.0-ubuntu22.04
```

### Start a Container from the Image

```bash
# Start Docker container
docker run -it --rm \
  -v /tmp:/tmp \             # Mount host /tmp into the container [required for license access; must use this mount]
  -v /mnt:/mnt \             # Mount host directory into the container [example; adjust as needed]
  --cpus 8 \                 # Limit container to 8 CPUs
  --gpus all \               # Use all GPUs (requires NVIDIA Container Toolkit)
   --name mipmap_rt \            # Container name: mipmap_rt
  registry.mipmap3d.com/docker/mipmap/realtime:v1.0-ubuntu22.04 \
  /bin/bash                  # Start bash after entering the container
```

**Important notes:**

- `/mnt` volume mapping: `mnt` is an example; mount whichever host directory your application needs.
- `/tmp` volume mapping: Required for license access; must be mounted.
- `--cpus 8`: Limits the container to 8 CPU cores.
- `--gpus all`: Enables all GPUs.

### Run the Real-time Reconstruction Service

Start the real-time reconstruction service from the command line by running `realtime_service`. Startup parameters:

| Field | Type | Required | Description |
|----------------------|--------------|----------|--------------------------------------------------------------|
| `port_number` | number | No | Server port number. Default: `50051`. |
| `task_type` | string | No | Reconstruction type. `2D` — stitch using GPS and gimbal angles; 2D real-time fast mosaic only. `3D` — stitch via SLAM; supports 2D real-time fast mosaic and 3D real-time mesh. **Default: `3D`.** |

```bash
# Run real-time reconstruction service inside the container
./mipmap_engine/realtime_service -port_number 50051(optional) -task_type 3D(optional)
```

---
