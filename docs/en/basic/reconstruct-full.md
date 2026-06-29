---
title: Full Pipeline Reconstruction
sidebar_position: 1
---

## Full Pipeline Reconstruction

:::tip Before you start
If you have not installed the SDK yet, see the [Windows installation guide](../overview/installation) or [Linux installation guide](../overview/linux-deployment).
:::

ReconstructFull is the simplest full-pipeline reconstruction API, ideal for quick project startup and new users. It automatically handles image metadata extraction, aerial triangulation (AT), and 3D reconstruction in one workflow.

### API Characteristics

- ✅ **Highly automated**: Reads GPS and camera information from image EXIF automatically
- ✅ **Simple configuration**: Minimum input is the image path
- ✅ **End-to-end**: Produces final deliverables directly from images
- ✅ **Smart optimization**: Automatically selects optimal processing parameters

### Invocation

The MipMapEngine SDK is invoked from the command line and is not tied to any programming language. Set `reconstruct_type` to `0` for full-pipeline reconstruction:

```bash
reconstruct_full_engine(.exe) --reconstruct_type 0 --task_json /path/to/task.json --license_key xxxx-xxxx-xxxx-xxxxxx
```

- `reconstruct_type`: API type, fixed to `0` (full-pipeline reconstruction)
- `task_json`: Path to the JSON parameter file
- `license_key`: License key

### Error Handling

All APIs return an integer error code:

- `0`: Success
- Non-zero: Various error conditions

See [Error Codes](./error-codes) for details.

*Tip: Most users only need the full-pipeline reconstruction API. Use other APIs only when you need fine-grained control.*

### Required Parameters

| Parameter | Type | Description | Example |
|--------|------|------|--------|
| `working_dir` | string | Working directory path | `"C:/Projects/MyProject"` |
| `gdal_folder` | string | GDAL data directory(**Only needed in Windows system**) | `"C:/MipMap/SDK/data"` |
| `image_meta_data` | array[[image_meta_data](#image-meta-data)] | Image list. See [image_meta_data](#image-meta-data) | |
| `input_image_type` | int | Image type | `1`=RGB, `2`=multispectral, `3`=infrared |
| `resolution_level` | int | `1`=high, `2`=medium, `3`=low | 1 |
| `coordinate_system` | [coordinate_system](#coordinate-system) | Coordinate system of POS (GPS) embedded in image EXIF. UAV images often embed WGS84; images without embedded POS (phones, panoramic cameras) use Local. See [coordinate_system](#coordinate-system) | [Common coordinate systems](#common-coordinate-systems) |

:::tip
`gdal_folder` points to the SDK's bundled `data` or `gdal_data` directory. You can move this directory as needed, as long as `gdal_folder` points to its new location. On Linux, this parameter is not required. **The path must contain ASCII characters only.**
:::

### Output Types Parameters

Output parameters select which reconstruction deliverables to generate. **At least one** output type must be enabled.

| Parameter | Type | Default | Description |
|--------|------|--------|------|
| `generate_obj` | bool | false | Generate OBJ model |
| `generate_ply` | bool | false | Generate PLY model |
| `generate_osgb` | bool | false | Generate OSGB (oblique photogrammetry) |
| `generate_3d_tiles` | bool | false | Generate 3D Tiles (Cesium) |
| `generate_las` | bool | false | Generate LAS point cloud |
| `generate_pc_ply` | bool | false | Generate point cloud PLY |
| `generate_pc_osgb` | bool | false | Generate point cloud OSGB (LOD) |
| `generate_pc_pnts` | bool | false | Generate point cloud PNTS (3D Tiles) |
| `generate_gs_ply` | bool | false | Generate PLY Gaussian splatting output|
| `generate_gs_sog` | bool | false | Generate compressed SOG Gaussian splatting output|
| `generate_gs_splat_sog_tiles` | bool | false | Generate SOG Tiles for large-scale Gaussian splatting rendering|
| `generate_geotiff` | bool | false | Generate orthophoto |
| `generate_tile_2D` | bool | false | Generate 2D tiles |
| `generate_2D_from_3D_model` | bool | false | Generate orthophoto from 3D model |


### Output Coordinate Systems Parameters [Optional]

| Parameter | Type | Description |
|--------|------|------|
| `coordinate_system_3d` | [coordinate_system](#coordinate-system) | Output 3D coordinate system; if unset, Local or LocalENU is used |
| `coordinate_system_2d` | [coordinate_system](#coordinate-system) | Output 2D coordinate system; if unset, Local or LocalENU is used |

### Advanced Parameters

The above parameters meet the needs of most users. For more advanced options, refer to the [Advanced Parameters](./advanced-config).

### Parameter Structures

#### coordinate_system

| Field | Type | Description | Example |
|--------|------------|--------------------------------------------------------------|----------------|
| `type` | int | `0`=LocalENU, `1`=Local, `2`=Geographic, `3`=Projected, `4`=ECEF | 2 |
| `epsg_code` | int | EPSG code (required for geographic/projected/ECEF) | 4326 |
| `wkt` | string | WKT string (optional, alternative to `epsg_code`) | "..." |
| `origin_point` | array[3] | WGS84 origin [lon, lat, elevation] (required only for `type=0` LocalENU) | [114.12, 22.12, 0] |
| `offset` | array[3] | Origin offset (optional); for projected CRS, avoids precision loss and model flicker in viewers | [0, 0, 0] |

#### image_meta_data

| Field | Type | Required | Description |
|----------------------|--------------|----------|--------------------------------------------------------------|
| `id` | int | Yes | Unique image ID |
| `path` | string | Yes | Absolute image path |
| `group` | string | No | Camera group name (multi-camera systems) |
| `mask_path` | string | No | Path to binary mask PNG; pixels with value 0 are ignored during reconstruction |

- **`group` parameter**

Groups images from different cameras in multi-camera systems:

Example:

```json
"image_meta_data": [
  {"id": 1, "path": "nadir/IMG_001.jpg", "group": "nadir"},
  {"id": 2, "path": "nadir/IMG_002.jpg", "group": "nadir"},
  {"id": 3, "path": "oblique_f/IMG_001.jpg", "group": "oblique_forward"},
  {"id": 4, "path": "oblique_b/IMG_001.jpg", "group": "oblique_backward"},
  {"id": 5, "path": "oblique_l/IMG_001.jpg", "group": "oblique_left"},
  {"id": 6, "path": "oblique_r/IMG_001.jpg", "group": "oblique_right"}
]
```

Common grouping scenarios:

- **Five-camera oblique system**: nadir, oblique_forward, oblique_backward, oblique_left, oblique_right
- **Dual-camera system**: wide_angle, telephoto
- **Multi-UAV**: drone_1, drone_2, drone_3
- **Multi-temporal**: morning, afternoon

- **`mask_path` parameter**

Binary masks mark invalid regions. The SDK ignores masked areas for cleaner results and better efficiency.

Example:

```json
{
  "id": 1,
  "path": "C:/Images/DJI_0001.JPG",
  "group": "camera_1",
  "mask_path": "C:/Images/DJI_0001.PNG"
}
```

### Configuration Examples

#### Common coordinate systems {#common-coordinate-systems}

```json
{
  "type": 2,
  "epsg_code": 4326
}
{
  "type": 3,
  "epsg_code": 32650,
  "offset": [
      269431.5374728,
      3364907.7329131,
      90.337
  ]
}
{
  "type": 2,
  "epsg_code": 4490
}
{
  "type": 2,
  "epsg_code": 4547,
  "offset": [
      269431.5374728,
      3364907.7329131,
      90.337
  ]
}
```

#### Minimal task configuration

```json
{
  "working_dir": "C:/Projects/QuickStart",
  "gdal_folder": "C:/MipMap/SDK/data",
  "input_image_type": 1,
  "resolution_level": 2,
  "coordinate_system": {
    "type": 2,
    "epsg_code": 4326
  },
  "image_meta_data": [
    {"id": 1, "path": "C:/Images/DJI_0001.JPG"},
    {"id": 2, "path": "C:/Images/DJI_0002.JPG"},
    {"id": 3, "path": "C:/Images/DJI_0003.JPG"},
    {"id": 4, "path": "C:/Images/DJI_0004.JPG"},
    {"id": 5, "path": "C:/Images/DJI_0005.JPG"}
  ],
  "generate_osgb": true,
  "generate_3d_tiles": true,
  "generate_geotiff": true
}
```

#### Multi-camera group configuration

```json
{
  "working_dir": "D:/Projects/MultiCamera",
  "gdal_folder": "D:/MipMap/SDK/data",
  "input_image_type": 1,
  "resolution_level": 1,
  "coordinate_system": {
    "type": 2,
    "epsg_code": 4326
  },
  "image_meta_data": [
    {"id": 1, "path": "nadir/IMG_001.jpg", "group": "nadir"},
    {"id": 2, "path": "nadir/IMG_002.jpg", "group": "nadir"},
    {"id": 3, "path": "forward/IMG_001.jpg", "group": "oblique_f"},
    {"id": 4, "path": "forward/IMG_002.jpg", "group": "oblique_f"}
  ],
  "generate_osgb": true,
  "generate_3d_tiles": true,
  "generate_geotiff": true
}
```

