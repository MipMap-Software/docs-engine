---
title: Quick Start
sidebar_position: 4
---

## Quick Start

This guide helps you get started with MipMapEngine SDK quickly.

### Prerequisites

Before you begin, make sure you have:

1. ✅ Installed the SDK (see [Windows installation guide](./installation) or [Linux installation guide](./linux-deployment))
2. ✅ Activated your license

### Start a Reconstruction Quickly

We provide an intuitive interactive page to generate the JSON parameter file required to start reconstruction. Drag your images into the page, set your working directory, the SDK GDAL directory, and basic reconstruction parameters, then choose the output formats you need to produce the JSON file.

**[Open the interactive page](https://mipmap3d.com/tasks_generator/#/)**

Next, update the image root directory in the JSON file to your actual path, then launch full-pipeline reconstruction from the command line:

```bash
reconstruct_full_engine(.exe) --reconstruct_type 0 --task_json config_task.json
```

:::tip Tip
For Linux Docker deployment, the `reconstruct_full_engine` executable is located in the container’s `mipmap_engine` directory.
:::

Parameter description

- `reconstruct_type`: `0` means one-click full-pipeline reconstruction
- `task_json`: Path to the JSON parameter file

[**Important**] You are starting the SDK’s one-click full-pipeline reconstruction interface—the most common and recommended entry point. See [Reconstruct Full](../basic/reconstruct-full) for detailed parameter rules.


*Need help? See the troubleshooting guide or contact technical support.*
