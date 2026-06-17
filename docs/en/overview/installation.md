---
title: Installation Guide (Windows)
sidebar_position: 2
---

## Installation Guide (Windows)

This guide helps you install and configure MipMapEngine on Windows.

### System Requirements

#### Hardware Requirements

| Component | Minimum | Recommended |
|------|----------|----------|
| **CPU** | Intel i5 or equivalent | Intel i7/i9 or AMD Ryzen 7/9 |
| **Memory** | 16 GB | 32 GB or more |
| **GPU** | NVIDIA GTX 1050 Ti | NVIDIA RTX 3070 or higher (Gaussian splatting requires compute capability 7.0+) |
| **Storage** | 500 GB free space | 2 TB SSD |

#### Supported GPUs

:::info MipMapEngine currently supports NVIDIA GPUs only 
- **GPU architecture**: Compute Capability ≥ 5.0 (Gaussian splatting ≥ 7.0)
- **VRAM**: GPU VRAM ≥ 4 GB

:::

#### Driver Version Requirements

- **Windows**: NVIDIA driver ≥ 528.33
- **Linux**: NVIDIA driver ≥ 525.60.33

### Windows Installation

#### Step 1: Check the System Environment

```bash
# Check NVIDIA GPU model and driver version
nvidia-smi
```

#### Step 2: Install the SDK

Download the latest SDK package from the [MipMap Engine SDK download page](https://na.mipmap3d.com/download/engine).

**Extract the SDK package.** The directory layout is as follows:

📁 Directory structure

After installation, the SDK directory layout is:

```
├── bin/                             
│   ├── reconstruct_full_engine.exe  # Reconstruction executable; all reconstruction steps use this program
├── gdal_data/                       # GDAL data directory
```

### Installation Checklist

- NVIDIA GPU driver updated to the required version
- SDK files extracted correctly

### Next Steps

Installation complete. You can now:

- 📚 Read [Quick Start](./quick-start) to run your first reconstruction task
- 📚 Read [Full-pipeline reconstruction](../basic/reconstruct-full) to learn how to use the API

---

*Tip: Problems during installation? See [troubleshooting](../assistant/troubleshooting) or contact technical support.*
