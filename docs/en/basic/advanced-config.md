---
title: Advanced Parameters
sidebar_position: 3
---

## Advanced Parameters

You can set advanced parameters in the SDK JSON configuration file for more flexible control.

### Reconstruction Module

| Parameter | Type | Default | Description |
|--------|------|--------|------|
| `min_avali_memory_size` | double | 0 | Minimum available memory for the device; controls tile size and count for tiled reconstruction. `0` uses maximum available memory |
| `fill_water_area_with_AI` | bool | false | Use machine learning for water-area inpainting `[requires ML plugin]` |
| `build_overview` | bool | true | Whether to build image pyramids for 2D deliverables |
| `cut_frame_2d` | bool | false | Whether to output 2D deliverables in tiles |
| `cut_frame_width` | int | 5000 | Pixel width per 2D tile when `cut_frame_2d` is `true` |
| `dom_gsd` | double | 0 | Ground resolution of true orthophoto; `0` means auto-compute. SDK applies a minimum guard; values below the limit are ignored |
| `dom_gsd_min_scale_to_original` | double | 0.25 | Minimum guard: `dom_gsd` will not exceed original image GSD × this factor |
| `tile_extention` | string | ".webp" | Image format for true orthophoto tiles: `.png` or `.webp` |
| `use_gcj02` | bool | false | Convert orthophoto tiles to GCJ-02 coordinate system |
| `fast_mode` | bool | false | Faster processing at the cost of some mesh detail |
| `keep_undistort_images` | bool | false | Keep undistorted images after reconstruction (default: do not keep) |
| `remove_small_islands` | bool | false | Remove isolated floating components from the 3D model |
| `mesh_decimate_ratio` | double | 1.0 | Mesh simplification ratio; smaller values mean more simplification |
| `use_draco_compression` | bool | true | Enable Draco vertex compression for 3D Tiles |
| `tex_image_type_3dtiles` | int | 0 | 3D Tiles texture format: `0`=JPEG, `1`=WebP |
| `roi_for_2d` | [roi](#roi) | - | Region of interest for 2D reconstruction. See [ROI structure](#roi) |
| `roi_for_3d` | [roi](#roi) | - | Region of interest for 3D reconstruction. See [ROI structure](#roi) |
| `roi_coordinate_system` | JSON | WGS84 | Coordinate system for ROI. See [coordinate system](../basic/reconstruct-full#coordinate-system) |


### Parameter Structures

#### roi

An ROI defines a spatial extent with a 2D `boundary`, `min_z`, and `max_z`.

| Field | Type | Description |
|--------|------|------|
| `boundary` | array | 2D polygon vertices of the ROI boundary |
| `min_z` | double | Minimum elevation of the ROI |
| `max_z` | double | Maximum elevation of the ROI |

Example:

```json
{
  "boundary": [
    [x1, y1],
    [x2, y2],
    [x3, y3]
  ],
  "min_z": 100.0,
  "max_z": 500.0
}
```
