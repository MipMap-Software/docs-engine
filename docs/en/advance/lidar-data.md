---
title: LiDAR Data Specification
sidebar_position: 2
---

## LiDAR Data

By organizing LiDAR data into a single JSON file, you can quickly import all data required for reconstruction into MipMap Desktop with one click, or merge it with reconstruction parameters and pass it to the MipMap SDK for fully automated textured model and Gaussian splatting reconstruction.

For easy identification, change the JSON file extension to **.mpl** before importing into MipMap Desktop (for example, `mipmap_lidar.mpl`).

The data includes:

- ✅ **Point cloud data**: Typically LAS-format point clouds. Points must record acquisition timestamps in the `gps_time` field.
- ✅ **Image data**: Image set, including path, timestamp, position, and attitude angles (optional) for each image.
- ✅ **Camera parameters**: Focal length, principal point intrinsics, and distortion parameters.

### LiDAR Data Fields

| Field | Type | Required | Description |
|--------|------|--------|------|
| `coordinate_systems` | JSON | Yes | Coordinate system definitions. See [coordinate_systems](#coordinate-systems). |
| `camera_meta_data` | JSON | Yes | Camera parameters. See [camera_meta_data](#camera-meta-data). |
| `image_meta_data` | JSON | Yes | Image collection. See [image_meta_data](#image-meta-data). |
| `lidar_data` | JSON | Yes | LiDAR data. See [lidar_data](#lidar-data). |

#### coordinate_systems

`coordinate_systems` is an array of coordinate systems, each with an ID.

| Field | Type | Required | Description |
|--------|------|--------|------|
| `coordinate_system` | JSON | Yes | Coordinate system object. See [coordinate_system](../basic/reconstruct-full#coordinate-system). |
| `id` | int | Yes | Coordinate system ID |

The following example includes one local coordinate system and one WGS 84 geographic coordinate system:

```json
"coordinate_systems": [
  {
    "id": 0,
    "coordinate_system": {
      "type": 1,
      "type_name": "Local"
    }
  },
  {
    "id": 1,
    "coordinate_system": {
      "type": 2,
      "type_name": "Geographic",
      "label": "WGS 84",
      "epsg_code": 4326
    }
  }
]
```

#### camera_meta_data

| Field | Type | Required | Description |
|----------------------|--------------|----------|--------------------------------------------------------------|
| `id` | int | Yes | Unique camera ID |
| `meta_data` | object | Yes | Camera metadata. See table below. |

**meta_data sub-fields:**

| Field | Type | Required | Description |
|----------------|--------------|----------|--------------------------------------------------------------|
| `projection_model` | int | Yes | Lens model: `0` — perspective (k1, k2, k3, p1, p2); `1` — fisheye (k1, k2, k3, k4) |
| `camera_name` | string | Yes | Camera name |
| `width` | int | Yes | Sensor width (pixels) |
| `height` | int | Yes | Sensor height (pixels) |
| `parameters` | array | Yes | Intrinsic parameter array (perspective order: fx, fy, cx, cy, k1, k2, p1, p2, k3; fisheye order: fx, fy, cx, cy, k1, k2, k3, k4) |
| `constant_parameters` | array | No | Index array of intrinsics to hold fixed during bundle adjustment, in `parameters` order (0-based). For example, `[0,1,2,3,4,5,6,7,8]` for a perspective camera fixes all parameters. |
| `mask_path` | string | No | Path to a binary mask image (PNG). Pixels with value `0` are ignored during reconstruction (applied to all images from this camera). |

:::tip
A typical use case for camera masks: when a fixed camera on a robot captures parts of the robot body, use a mask to exclude those pixels from reconstruction.
:::

Perspective lens

```json
"camera_meta_data": [
  {
    "id": 1,
    "meta_data": {
      "projection_model": 0,    // perspective camera
      "camera_name": "common",
      "width": 5280,
      "height": 3956,
      "parameters": [
        3713.29,                // fx
        3713.29,                // fy
        2647.02,                // cx
        1969.28,                // cy
        -0.11257524,            // k1
        0.01487443,             // k2
        -0.00008572,            // p1
        1e-7,                   // p2
        -0.02706411             // k3
      ]
    }
  }
]
```

Fisheye lens

```json
"camera_meta_data": [
  {
    "id": 1,
    "meta_data": {
      "camera_name": "fisheye",         // fisheye camera
      "projection_model": 1,
      "width": 3600,
      "height": 3600,
      "parameters": [
        982.7593599212141,              // fx
        982.7593599212141,              // fy
        1747.6373897301492,             // cx
        1806.4116030074354,             // cy
        0.03702410479839055,            // k1
        -0.016007338300982825,          // k2
        -1.0884582901480562e-05,        // k3
        -9.773097281093723e-05          // k4
      ]
    }
  }
]
```

#### image_meta_data

| Field | Type | Required | Description |
|----------------------|--------------|----------|--------------------------------------------------------------|
| `id` | int | Yes | Unique image ID |
| `path` | string | Yes | Absolute image path |
| `meta_data` | object | Yes | Image metadata. See table below. |
| `crs_id` | int | No | Coordinate system ID for the image. Default: `0`. |
| `source_id` | int | Yes | Acquisition source ID. Default: `0`. |

:::tip
Use `crs_id` to distinguish image groups whose POS uses different coordinate systems—for example, WGS 84 or CGCS2000 for aerial data and a local CRS for ground data.

Use `source_id` to distinguish images from different acquisition sources—for example, UAV imagery versus handheld captures. Assigning different `source_id` values is essential.

Data sources in different coordinate systems must use different source IDs.

Typical use case: air–ground fusion.
:::

**meta_data sub-fields:**

| Field | Type | Required | Description |
|----------------------|--------------|----------|--------------------------------------------------------------|
| `width` | int | Yes | Image width (pixels) |
| `height` | int | Yes | Image height (pixels) |
| `camera_id` | int | Yes | Corresponding camera ID |
| `timestamp` | double | Yes | Image acquisition timestamp. **Required for handheld LiDAR imagery; must match timestamps recorded in the LAS point cloud `gps_time` field.** Optional for other sources. |
| `pos` | array[3] | Yes | POS position [longitude/X, latitude/Y, elevation/Z]. **Required for handheld LiDAR imagery; must use the same CRS as the corresponding LAS point cloud.** |
| `pos_sigma` | array[3] | Yes | POS accuracy. Centimeter-level RTK: `[0.03, 0.03, 0.06]`; decimeter-to-meter GNSS: `[0.5, 0.5, 1]`; meter-level and above: `[2.0, 2.0, 5.0]`. LiDAR data is recommended to use centimeter-level values. |
| `orientation` | array[9] | No | 3×3 rotation matrix (9 values, row-major, unit vectors; world-to-camera). Strongly recommended for fisheye images. |
| `position_constant` | bool | No | Fix position (`true`/`false`). When fixed, bundle adjustment will not optimize image position. |
| `mask_path` | string | No | Path to a binary mask image (PNG). Pixels with value `0` are ignored during reconstruction. |

**Note:** Binary masks mark invalid regions. The SDK ignores them during reconstruction to improve efficiency and produce cleaner outputs.

#### lidar_data

| Field | Type | Description |
|--------|------|------|
| `laser_meta_data` | array | LiDAR point cloud file metadata list |

#### laser_meta_data

| Field | Type | Description |
|--------|------|------|
| `id` | int | LAS point cloud data ID |
| `path` | string | LAS point cloud file path (`.las` format) |
| `source_id` | int | `source_id` for the LAS data. Must match the paired images. Default: `0`. |
| `crs_id` | int | Coordinate system ID for the LAS data. Default: `0`. |

Example:

```json
{
  "camera_meta_data": [
  {
    "id": 1,
    "meta_data": {
      "camera_name": "Left",
      "projection_model": 1,
      "width": 3600,
      "height": 3600,
      "parameters": [
        982.7593599212141,
        982.7593599212141,
        1747.6373897301492,
        1806.4116030074354,
        0.03702410479839055,
        -0.016007338300982825,
        -1.0884582901480562e-05,
        -9.773097281093723e-05
      ]
    }
  },
  {
    "id": 2,
    "meta_data": {
      "camera_name": "Right",
      "projection_model": 1,
      "width": 3600,
      "height": 3600,
      "parameters": [
        982.7593599212141,
        982.7593599212141,
        1747.6373897301492,
        1806.4116030074354,
        0.03702410479839055,
        -0.016007338300982825,
        -1.0884582901480562e-05,
        -9.773097281093723e-05
      ]
    }
  }
  ],
  "image_meta_data":[
    {
        "id": 1,
        "path": "\\left\\2390-195042-472_00395-L.JPG",
        "crs_id": 0,
        "source_id": 0,
        "meta_data": {
        "width": 3600,
        "height": 3600,
        "camera_id": 1,
        "timestamp": 1.0000,
        "pos": [
            -2272303.077314121,
            5011055.355739306,
            3215292.943560472
        ],
        "pos_sigma": [0.03,0.03,0.06],
        "orientation": [
            0.8929275412022056,
            0.43070972182403255,
            0.13103259781004117,
            -0.4500241512548592,
            0.8457917052937085,
            0.2865565468519331,
            0.012596406230632032,
            -0.31484206641207435,
            0.9490604847782084
        ]
        }
    },
    {
        "id": 2,
        "path": "\\right\\2390-195042-472_00395-R.JPG",
        "crs_id": 0,
        "source_id": 0,
        "meta_data": {
        "width": 3600,
        "height": 3600,
        "camera_id": 2,
        "timestamp": 2.0000,
        "pos": [
            -2272303.117693735,
            5011055.368901664,
            3215292.967908075
        ],
        "pos_sigma": [0.03,0.03,0.06],
        "orientation": [
            0.8918935649712837,
            0.435497387453275,
            0.12193397510204676,
            -0.4504120798442928,
            0.8311302214603127,
            0.3261157973874772,
            0.040679566039387514,
            -0.34578111645476495,
            0.9374329802231962
        ]
        }
    },
    {
        "id": 2,
        "path": "\\DJI_0001.JPG",
        "crs_id": 1,
        "source_id": 1,
        "meta_data": {
        "width": 3600,
        "height": 3600,
        "camera_id": 2,
        "timestamp": 2.0000,
        "pos": [
            -2272303.117693735,
            5011055.368901664,
            3215292.967908075
        ],
        "pos_sigma": [0.03,0.03,0.06],
        "orientation": [
            0.8918935649712837,
            0.435497387453275,
            0.12193397510204676,
            -0.4504120798442928,
            0.8311302214603127,
            0.3261157973874772,
            0.040679566039387514,
            -0.34578111645476495,
            0.9374329802231962
        ]
        }
    }
  ],
  "lidar_data": {
    "laser_meta_data": [
      {
        "id": 1,
        "path": "C:/path/to/file1.las",
        "source_id": 0,
        "crs_id": 0
      },
      {
        "id": 2,
        "path": "C:/path/to/file2.las",
        "source_id": 0,
        "crs_id": 0
      }
    ]
  },
  "coordinate_systems": [
    {
      "id": 0,
      "coordinate_system": {
        "type": 1,
        "type_name": "Local"
      }
    },
    {
      "id": 1,
      "coordinate_system": {
        "type": 2,
        "type_name": "Geographic",
        "label": "WGS 84",
        "epsg_code": 4326
      }
    }
  ]
}
```
