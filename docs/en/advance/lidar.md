---
title: LiDAR Reconstruction
sidebar_position: 1
---

## LiDAR Reconstruction

LiDAR reconstruction uses the same invocation method as basic reconstruction, but requires specific data inputs. In addition to the parameters required for basic reconstruction, set or override the following parameters for LiDAR:

### Required Parameters

| Field | Type | Required | Description |
|--------|------|--------|------|
| `pipeline_mode` | int | Yes | Must be set to `1` for pure LiDAR point cloud reconstruction |
| `coordinate_systems` | Array | Yes | Coordinate system definitions |
| `camera_meta_data` | JSON | Yes | Camera parameters |
| `image_meta_data` | JSON | Yes | Image collection |
| `lidar_data` | JSON | Yes | LiDAR data |

For detailed parameter descriptions, see [LiDAR Data Specification](./lidar-data).

### JSON Example

```json
{
  "working_dir": "C:/Projects/AT_Task",
  "gdal_folder": "C:/MipMap/SDK/data",
  "pipeline_mode": 1,
  "generate_osgb": true,
  "generate_gs_ply": true,
  "coordinate_systems": [
    {
      "id": 1,
      "coordinate_system": {
        "type": 2,
        "epsg_code": 4326
      }
    }
  ],
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
  "image_meta_data": [
    {
      "id": 1,
      "path": "\\left\\2390-195042-472_00395-L.JPG",
      "crs_id": 1,
      "meta_data": {
        "width": 3600,
        "height": 3600,
        "camera_id": 1,
        "pos": [
          -2272303.077314121,
          5011055.355739306,
          3215292.943560472
        ],
        "pos_sigma": [
          0.03,
          0.03,
          0.06
        ],
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
      "crs_id": 1,
      "meta_data": {
        "width": 3600,
        "height": 3600,
        "camera_id": 2,
        "pos": [
          -2272303.117693735,
          5011055.368901664,
          3215292.967908075
        ],
        "pos_sigma": [
          0.03,
          0.03,
          0.06
        ],
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
        "crs_id": 1,
        "path": "C:/path/to/file.las"
      }
    ]
  }
}
```
