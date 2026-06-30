---
title: Installation Guide (Linux)
sidebar_position: 3
---

## Installation Guide (Linux)

This guide explains how to deploy and run MipMapEngine SDK on Linux. Linux is supported **only** via Docker deployment.

### System Requirements
- **Operating system**: Ubuntu (WSL is not supported)
- **Docker**: Version 19.03 or later (with GPU support)
- **GPU**: NVIDIA GPU with CUDA support, compute capability 5.0 or higher (Gaussian splatting requires 7.0+)
- **Memory**: 32 GB or more recommended
- **Storage**: At least 500 GB free space

---

### Environment Setup

#### Docker Configuration

Skip this section if Docker is already installed. On Ubuntu:

```bash
# Update package lists
sudo apt update

# Install Docker
sudo apt install -y docker.io

# Start Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Install NVIDIA Container Toolkit (Ubuntu, Debian)
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
  sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
  sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit

# Configure Docker to use the NVIDIA runtime
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

#### Verify GPU Support
```bash
# Verify Docker GPU support
# [Mainland China]
docker run --gpus all registry.mipmap3d.com/docker/nvidia/cuda:12.8.0-base-ubuntu22.04 nvidia-smi
# [Other regions]
docker run --gpus all nvidia/cuda:12.8.0-base-ubuntu22.04 nvidia-smi
```

---

### Run the Reconstruction Container

#### Pull the Image

```bash
docker pull registry.mipmap3d.com/mipmap/runtime:v5.1.0.8-ubuntu22.04
```

#### Start a Container from the Image
```bash
# Start Docker container
docker run -it --rm \
  -v $HOME/.config:/root/.config \ # Required for license verification
  -v /mnt:/mnt \                   # Mount host directory into container [example; adjust as needed]
  --cpus 8 \                      # Limit container to 8 CPUs
  --gpus all \                    # Use all GPUs (requires NVIDIA Container Toolkit)
  --name mipmap \                 # Container name: mipmap
  registry.mipmap3d.com/mipmap/runtime:v5.1.0.8-ubuntu22.04 \
  /bin/bash                       # Start bash after entering the container
```

**Important notes**:
- `/mnt` volume mapping: `mnt` is an example; mount whichever host paths your workflow needs
- `$HOME/.config` volume mapping: required for license access; must be mounted
- `--cpus 8`: limits the container to 8 CPU cores
- `--gpus all`: enables all GPUs

#### Run the Reconstruction Program

```bash
# Run the reconstruction engine inside the container
./mipmap_engine/reconstruct_full_engine --reconstruct_type 0 --task_json /mnt/task.json --license_key xxxx-xxxx-xxxx-xxxxxx
```

**Parameter description**:
- `--reconstruct_type`: Reconstruction type (`0` = default full pipeline)
- `--task_json`: Path to the task configuration JSON file
- `license_key`: License key

### Troubleshooting

#### Common Issues

**Issue 1**: Docker cannot start with GPU support
```bash
# Check NVIDIA driver
nvidia-smi

# Check Docker GPU support
# [Mainland China]
docker run --gpus all registry.mipmap3d.com/docker/nvidia/cuda:12.8.0-base-ubuntu22.04 nvidia-smi
# [Other regions]
docker run --gpus all nvidia/cuda:12.8.0-base-ubuntu22.04 nvidia-smi
```

### File Structure

```
mipmap_engine/
├── gdal_data/                       # gdal_folder directory
├── reconstruct_full_engine     # Main processing executable
```
