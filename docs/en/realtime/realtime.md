---
title: Usage Guide
sidebar_position: 2
---

## Usage Guide

### Data requirements
Real-time reconstruction supports images captured on UAV **nadir flight** missions, with along-track and side overlap of at least 70% each (commonly stated as along-track 70 and side 70 or higher).

:::tip 
We recommend DJI surveying UAVs M3E/M4E. All images for real-time reconstruction must be captured from a nadir (straight-down) perspective.
:::

### Nadir flight path diagram

<svg viewBox="0 0 800 350" xmlns="http://www.w3.org/2000/svg">
  <!-- Background -->
  <rect width="800" height="350" fill="#f8f9fa" stroke="none"/>
  
  <!-- Title -->
  <text x="400" y="30" text-anchor="middle" font-size="20" font-weight="bold" fill="#333">UAV Nadir Flight Path Diagram</text>
  
  <!-- Flight line 1 - left to right -->
  <path d="M 100 200 L 700 200" stroke="#2196F3" stroke-width="4" fill="none" marker-end="url(#arrowhead1)"/>
  <text x="400" y="185" text-anchor="middle" font-size="12" fill="#1976D2" font-weight="bold">Flight line 1</text>
  
  <!-- Flight line 2 - right to left (reversed direction) -->
  <path d="M 700 250 L 100 250" stroke="#2196F3" stroke-width="4" fill="none" marker-end="url(#arrowhead2)"/>
  <text x="400" y="235" text-anchor="middle" font-size="12" fill="#1976D2" font-weight="bold">Flight line 2</text>
  
  <!-- Flight line 3 - left to right -->
  <path d="M 100 300 L 700 300" stroke="#2196F3" stroke-width="4" fill="none" marker-end="url(#arrowhead3)"/>
  <text x="400" y="285" text-anchor="middle" font-size="12" fill="#1976D2" font-weight="bold">Flight line 3</text>
  
  <!-- Waypoints on flight line 1 (9 total) -->
  <circle cx="100" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="175" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="250" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="325" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="400" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="475" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="550" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="625" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="700" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  
  <!-- Waypoints on flight line 2 (9 total, reversed direction) -->
  <circle cx="700" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="625" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="550" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="475" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="400" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="325" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="250" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="175" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="100" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  
  <!-- Waypoints on flight line 3 (9 total) -->
  <circle cx="100" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="175" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="250" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="325" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="400" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="475" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="550" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="625" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="700" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  
  <!-- Connecting segments between flight lines -->
  <!-- Flight line 1 to flight line 2 -->
  <path d="M 700 200 L 700 250" stroke="#666" stroke-width="2" fill="none" stroke-dasharray="5,5"/>
  
  <!-- Flight line 2 to flight line 3 -->
  <path d="M 100 250 L 100 300" stroke="#666" stroke-width="2" fill="none" stroke-dasharray="5,5"/>
  
  <!-- Footprint coverage -->
  <rect x="120" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="220" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="320" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="420" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="520" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="620" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  
  <!-- Footprint coverage on flight line 2 (reversed direction) -->
  <rect x="620" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="520" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="420" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="320" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="220" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="120" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  
  <!-- Footprint coverage on flight line 3 -->
  <rect x="120" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="220" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="320" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="420" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="520" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="620" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  
  <!-- Overlap annotations -->
  <path d="M 170 160 L 170 140" stroke="#333" stroke-width="1" stroke-dasharray="2,2"/>
  <path d="M 220 160 L 220 140" stroke="#333" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="195" y="130" text-anchor="middle" font-size="12" fill="#666">Along-track overlap 70-80%</text>
  
  <path d="M 80 200 L 60 200" stroke="#333" stroke-width="1" stroke-dasharray="2,2"/>
  <path d="M 80 250 L 60 250" stroke="#333" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="50" y="225" text-anchor="middle" font-size="12" fill="#666" transform="rotate(-90, 50, 225)">Side overlap 70-80%</text>
  
  <!-- Flight parameters -->
  <rect x="50" y="50" width="200" height="120" fill="#FFF3E0" stroke="#F57C00" stroke-width="1"/>
  <text x="150" y="70" text-anchor="middle" font-size="14" font-weight="bold" fill="#E65100">Flight parameters</text>
  <text x="60" y="90" font-size="12" fill="#333">• Flight altitude: 100-200m</text>
  <text x="60" y="110" font-size="12" fill="#333">• Along-track overlap: 70-80%</text>
  <text x="60" y="130" font-size="12" fill="#333">• Side overlap: 70-80%</text>
  <text x="60" y="150" font-size="12" fill="#333">• Capture interval: 1-3 s</text>
  
  <!-- Arrow definitions -->
  <defs>
    <marker id="arrowhead1" markerWidth="6" markerHeight="4" refX="5" refY="2" orient="auto">
      <polygon points="0 0, 6 2, 0 4" fill="#2196F3"/>
    </marker>
    <marker id="arrowhead2" markerWidth="6" markerHeight="4" refX="5" refY="2" orient="auto">
      <polygon points="0 0, 6 2, 0 4" fill="#2196F3"/>
    </marker>
    <marker id="arrowhead3" markerWidth="6" markerHeight="4" refX="5" refY="2" orient="auto">
      <polygon points="0 0, 6 2, 0 4" fill="#2196F3"/>
    </marker>
  </defs>
</svg>

:::tip Nadir flight planning essentials
- **Flight line spacing**: Compute from camera field of view and flight altitude to ensure side overlap
- **Capture interval**: Set from ground speed and along-track overlap requirements
- **Flight altitude**: Affects ground resolution (GSD) and footprint size
- **Weather**: Prefer uniform lighting and calm or light wind
:::


### Basic real-time reconstruction workflow

Real-time reconstruction is provided by a gRPC server program: **realtime_service**. Starting it starts the real-time reconstruction service. We provide a `.proto` file for developers to build client applications. The client performs the following operations.

（1）Initialize — initialize the real-time reconstruction service

（2）AddFrame — add image frames

（3）EndTask — end the current real-time reconstruction task

:::tip
The real-time reconstruction service uses the gRPC protocol. Developers should be familiar with gRPC client development for smooth integration and API calls.
:::

Proto file download: [Proto file download](https://asset.mipmap3d.com/engine/realtime.proto)

The diagram below shows the basic workflow of the real-time reconstruction service:

<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Background -->
  <rect width="800" height="500" fill="#f8f9fa" stroke="none"/>
  
  <!-- Title -->
  <text x="400" y="30" text-anchor="middle" font-size="20" font-weight="bold" fill="#333">Real-Time Reconstruction Service Workflow</text>
  
  <!-- Workflow steps -->
  <!-- 1. Start service -->
  <rect x="50" y="60" width="120" height="60" fill="#E3F2FD" stroke="#2196F3" stroke-width="2" rx="5"/>
  <text x="110" y="85" text-anchor="middle" font-size="12" font-weight="bold" fill="#1976D2">1. Start service</text>
  <text x="110" y="100" text-anchor="middle" font-size="10" fill="#333">realtime_service</text>
  <text x="110" y="112" text-anchor="middle" font-size="10" fill="#333">Listen on port 50051</text>
  
  <!-- 2. Client connect -->
  <rect x="250" y="60" width="120" height="60" fill="#E8F5E8" stroke="#4CAF50" stroke-width="2" rx="5"/>
  <text x="310" y="85" text-anchor="middle" font-size="12" font-weight="bold" fill="#2E7D32">2. Client connect</text>
  <text x="310" y="100" text-anchor="middle" font-size="10" fill="#333">Establish gRPC connection</text>
  <text x="310" y="112" text-anchor="middle" font-size="10" fill="#333">Specify port number</text>
  
  <!-- 3. Initialize task -->
  <rect x="450" y="60" width="120" height="60" fill="#FFF3E0" stroke="#FF9800" stroke-width="2" rx="5"/>
  <text x="510" y="85" text-anchor="middle" font-size="12" font-weight="bold" fill="#F57C00">3. Initialize task</text>
  <text x="510" y="100" text-anchor="middle" font-size="10" fill="#333">Initialize API</text>
  <text x="510" y="112" text-anchor="middle" font-size="10" fill="#333">Pass config JSON</text>
  
  <!-- 4. Add image frames -->
  <rect x="650" y="60" width="120" height="60" fill="#F3E5F5" stroke="#9C27B0" stroke-width="2" rx="5"/>
  <text x="710" y="85" text-anchor="middle" font-size="12" font-weight="bold" fill="#7B1FA2">4. Add image frames</text>
  <text x="710" y="100" text-anchor="middle" font-size="10" fill="#333">AddFrame API</text>
  <text x="710" y="112" text-anchor="middle" font-size="10" fill="#333">Frame by frame</text>
  
  <!-- Arrow 1 -->
  <path d="M 170 90 L 250 90" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow1)"/>
  
  <!-- Arrow 2 -->
  <path d="M 370 90 L 450 90" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow2)"/>
  
  <!-- Arrow 3 -->
  <path d="M 570 90 L 650 90" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow3)"/>
  
  <!-- Real-time processing loop -->
  <rect x="50" y="180" width="700" height="200" fill="#FAFAFA" stroke="#BDBDBD" stroke-width="1" stroke-dasharray="5,5" rx="10"/>
  <text x="400" y="200" text-anchor="middle" font-size="16" font-weight="bold" fill="#424242">Real-time processing loop</text>
  
  <!-- Image input -->
  <rect x="80" y="220" width="100" height="50" fill="#E1F5FE" stroke="#03A9F4" stroke-width="2" rx="5"/>
  <text x="130" y="240" text-anchor="middle" font-size="11" font-weight="bold" fill="#0277BD">Image input</text>
  <text x="130" y="255" text-anchor="middle" font-size="9" fill="#333">Image path</text>
  <text x="130" y="265" text-anchor="middle" font-size="9" fill="#333">Metadata</text>
  
  <!-- SLAM localization -->
  <rect x="220" y="220" width="100" height="50" fill="#E8F5E8" stroke="#4CAF50" stroke-width="2" rx="5"/>
  <text x="270" y="240" text-anchor="middle" font-size="11" font-weight="bold" fill="#2E7D32">SLAM localization</text>
  <text x="270" y="255" text-anchor="middle" font-size="9" fill="#333">Camera pose</text>
  <text x="270" y="265" text-anchor="middle" font-size="9" fill="#333">Trajectory estimate</text>
  
  <!-- 2D map reconstruction -->
  <rect x="360" y="220" width="100" height="50" fill="#FFF3E0" stroke="#FF9800" stroke-width="2" rx="5"/>
  <text x="410" y="240" text-anchor="middle" font-size="11" font-weight="bold" fill="#F57C00">2D map</text>
  <text x="410" y="255" text-anchor="middle" font-size="9" fill="#333">Orthophoto tiles</text>
  <text x="410" y="265" text-anchor="middle" font-size="9" fill="#333">Web Mercator</text>
  
  <!-- 3D mesh reconstruction -->
  <rect x="500" y="220" width="100" height="50" fill="#F3E5F5" stroke="#9C27B0" stroke-width="2" rx="5"/>
  <text x="550" y="240" text-anchor="middle" font-size="11" font-weight="bold" fill="#7B1FA2">3D mesh</text>
  <text x="550" y="255" text-anchor="middle" font-size="9" fill="#333">PLY/3DTiles</text>
  <text x="550" y="265" text-anchor="middle" font-size="9" fill="#333">Tiled storage</text>
  
  <!-- AddFrame return value -->
  <rect x="640" y="220" width="100" height="50" fill="#FFEBEE" stroke="#F44336" stroke-width="2" rx="5"/>
  <text x="690" y="240" text-anchor="middle" font-size="11" font-weight="bold" fill="#C62828">AddFrame return</text>
  <text x="690" y="255" text-anchor="middle" font-size="9" fill="#333">Live update data</text>
  <text x="690" y="265" text-anchor="middle" font-size="9" fill="#333">Incremental info</text>
  
  <!-- Client handles return value -->
  <rect x="50" y="300" width="700" height="40" fill="#F3E5F5" stroke="#9C27B0" stroke-width="1" rx="5"/>
  <text x="400" y="320" text-anchor="middle" font-size="12" font-weight="bold" fill="#7B1FA2">Client handles AddFrame return</text>
  <text x="400" y="335" text-anchor="middle" font-size="10" fill="#333">Parse incremental data, update rendering</text>
  
  <!-- Loop arrows -->
  <path d="M 130 270 L 130 300" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow4)"/>
  <path d="M 400 340 L 400 380" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow6)"/>
  
  <!-- End task -->
  <rect x="300" y="400" width="200" height="60" fill="#FFCDD2" stroke="#F44336" stroke-width="2" rx="5"/>
  <text x="400" y="425" text-anchor="middle" font-size="12" font-weight="bold" fill="#C62828">5. End task</text>
  <text x="400" y="440" text-anchor="middle" font-size="10" fill="#333">EndTask API</text>
  <text x="400" y="452" text-anchor="middle" font-size="10" fill="#333">Post-process and save</text>
  
  <!-- Arrow from loop to end -->
  <path d="M 400 380 L 400 400" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow5)"/>
  
  <!-- Output note -->
  <rect x="50" y="480" width="700" height="15" fill="#E8F5E8" stroke="#4CAF50" stroke-width="1" rx="3"/>
  <text x="400" y="492" text-anchor="middle" font-size="10" fill="#2E7D32">Outputs: 2D/dom_tiles/ (2D tiles) | 3D/mesh (3D mesh PLY|3DTiles)</text>
  
  <!-- Arrow definitions -->
  <defs>
    <marker id="arrow1" markerWidth="6" markerHeight="4" refX="5" refY="2" orient="auto">
      <polygon points="0 0, 6 2, 0 4" fill="#666"/>
    </marker>
    <marker id="arrow2" markerWidth="6" markerHeight="4" refX="5" refY="2" orient="auto">
      <polygon points="0 0, 6 2, 0 4" fill="#666"/>
    </marker>
    <marker id="arrow3" markerWidth="6" markerHeight="4" refX="5" refY="2" orient="auto">
      <polygon points="0 0, 6 2, 0 4" fill="#666"/>
    </marker>
    <marker id="arrow4" markerWidth="6" markerHeight="4" refX="5" refY="2" orient="auto">
      <polygon points="0 0, 6 2, 0 4" fill="#666"/>
    </marker>
    <marker id="arrow5" markerWidth="6" markerHeight="4" refX="5" refY="2" orient="auto">
      <polygon points="0 0, 6 2, 0 4" fill="#666"/>
    </marker>
    <marker id="arrow6" markerWidth="6" markerHeight="4" refX="5" refY="2" orient="auto">
      <polygon points="0 0, 6 2, 0 4" fill="#666"/>
    </marker>
  </defs>
</svg>

`realtime_service` is a gRPC real-time reconstruction server. Clients can use it to:

**（1）Initialize the service and start a real-time task.**

**（2）Pass image paths frame by frame for real-time SLAM localization and 2D map | 3D mesh computation.**

**（3）Run post-processing and end the task automatically.**

**（4）End the task explicitly.**

The following sections describe how clients use this server.

The server listens on port **50051** by default. You can set another port via startup arguments.

### API reference

The service exposes these gRPC APIs:

#### Initialize

**Purpose:** Initialize the server and start a real-time task. Call this before any other API, passing task configuration.

Request format (InitRequest)
| Field name                | Type         | Description                                                         |
|----------------------|--------------|--------------------------------------------------------------|
| `task_json`                 | string          |  Task JSON content as a string                                                   |

Response format (InitResponse)
| Field name                | Type         | Description                                                         |
|----------------------|--------------|--------------------------------------------------------------|
| `state`                 | int32          |  Initialization status (0 = success, other values = failure)                                                   |

**`task_json` parameters:**

| Field name                | Type         | Required | Description                                                         |
|----------------------|--------------|----------|--------------------------------------------------------------|
| `license_id`                 | int          | Yes       |  License ID; must be **9200**                                                  |
| `working_dir`                 | string          | Yes       |  Working directory for intermediate and final output files                                                  |
| `gdal_folder`               | string       | Yes       | GDAL directory path; in containers use `/mipmap_engine/data`                                                 |
| `do_realtime_2d_map`          | bool       | No       | Default `false`. Whether to compute a real-time 2D map (orthophoto). At least one of `do_realtime_2d_map` and `do_realtime_3d_map` must be `true`.                                         |
| `do_realtime_3d_map`          | bool       | No       | Default `false`. Whether to compute a real-time 3D map (mesh; Windows only for now). At least one of `do_realtime_2d_map` and `do_realtime_3d_map` must be `true`.                                          |
| `target_max_size`          | int       | No       | 	Default `0`. Longest-edge pixel count after frame downscaling. Real-time tasks should downscale frames; larger values improve quality but increase per-frame time. **When `0`, the caller must downscale before `AddFrame` and pass metadata for the scaled image (see below).** Suggested values: 2048 / 1920 / 1280 |
| `tile_2d_save_type`          | int       | No       | Default `0`. Real-time 2D output format: `0` = webp, `1` = png                                         |
| `mesh_3d_save_type`          | int       | No       | Default `0`. Real-time 3D output format: `0` = ply, `1` = 3dtiles                                         |
| `rt_mesh_reso_scale_to_gsd`          | int       | No       | Default `6`. Real-time 3D mesh resolution = `rt_mesh_reso_scale_to_gsd` × image GSD. Smaller values yield higher mesh resolution and sharper color but lower performance.                                        |
| `save_dom_tiff`          | bool       | No       | Default `false`. Whether each frame writes a GeoTIFF DOM mosaic (adds per-frame time). Output path: `working_dir/2D/geotiffs/dom.tif`                                        |
| `coordinate_descriptor_2D`          | object       | No       | Default `null`. Coordinate system for GeoTIFF DOM; see [coordinate system](../basic/basic-config#coordinate-system)  

#### AddFrame

**Purpose:** Pass image paths frame by frame for real-time processing. The client keeps adding frames; the server processes them in real time.

Request format (AddFrameRequest)

| Field name                | Type         | Description                                                         |
|----------------------|--------------|--------------------------------------------------------------|
| `image_path`                 | string          | Image file path (UTF-8)                                                   |
| `image_meta_data`               | string       | Image metadata as a JSON string. **If empty, the service parses metadata internally (caller must ensure EXIF includes 35 mm equivalent focal length and GPS position).**                                                 |

**`image_meta_data` parameters:**

| Field name                | Type         | Required | Description                                                         |
|----------------------|--------------|----------|--------------------------------------------------------------|
| `width`              | int          | Yes       | Image width (pixels)                                             |
| `height`             | int          | Yes       | Image height (pixels)                                             |
| `camera_id`          | int          | No       | Camera ID                                                   |
| `focal_length_in_35mm`| double      | No       | 35 mm equivalent focal length (mm). **Required when `pre_calib_param` is not set.**
| `pos`                | array[3]     | Yes       | Position [longitude/X, latitude/Y, elevation/Z]                                      |
| `pos_sigma`          | array[3]     | No       | Position accuracy (m): RTK: [0.03, 0.03, 0.06], standard GPS: [2.0, 2.0, 5.0]                          |
| `orientation`        | array[9]     | No       | Rotation matrix (9 values, row-major, unit vectors). Optional extrinsics: world to camera, Cartesian frame.           |
| `position_constant`  | bool         | No       | Fix position (`true` / `false`)                                   |
| `relative_altitude`  | double       | No       | Relative flight height (m)                                           |
| `pre_calib_param`    | array        | No       | Pre-calibration parameters (camera intrinsics if available)                             |
| `dewarp_flag`        | bool         | No       | Dewarp flag (e.g. from DJI XMP: `true` if already dewarped)           |

Example:
```json
{
    "width": 5280,
    "height": 3956,
    "camera_id": 1,
    "pos": [
        121.11150580555555,
        31.788278305555558,
        21.226
    ],
    "pos_sigma": [
        0.03,
        0.03,
        0.06
    ],
    "orientation": [
        -0.3583679497241974,
        0.9335803985595703,
        0,
        0.7533622980117798,
        0.2891887128353119,
        -0.5906056761741638,
        -0.5513778924942017,
        -0.21165414154529572,
        -0.8069602847099304
    ],
    "relative_altitude": 6.012,
    "focal_length_in_35mm": 24,
    "pre_calib_param": [
        3713.29,
        3713.29,
        2647.02,
        1969.28,
        -0.11257524,
        0.01487443,
        -0.00008572,
        1e-7,
        -0.02706411
    ],
    "dewarp_flag": false
}
```
Response format (AddFrameResponse)
| Field name                | Type         | Description                                                         |
|----------------------|--------------|--------------------------------------------------------------|
| `update_json`                 | string          |  JSON string describing updates for the current frame (see below)                                                   |

`update_json` fields:
| Field name                | Type         | Description                                                         |
|----------------------|--------------|--------------------------------------------------------------|
| `tile2d_folder`                 | string          |  Root folder for 2D orthophoto quick-mosaic tiles                                                   |
| `updated_2d_tiles`                 | array          |  Tile paths updated this frame (relative to `tile2d_folder`)                                                   |
| `tile3d_folder`                 | string          |  Root folder for 3D mesh tiles                                                   |
| `updated_3d_tiles`                 | array          |  Mesh paths updated this frame (relative to `tile3d_folder`)                                                   |

**Real-time 2D output:**
The `2D/dom_tiles` directory under the working directory stores orthophoto tiles in **Web Mercator** (EPSG:3857). Use a third-party GIS library to read and display tiles. The client can incrementally refresh rendering from the returned `update_2d_tiles` set for better performance.

**Real-time 3D output:**
`3D/mesh` under the working directory stores the 3D mesh in binary form (PLY or 3DTiles), tiled by geographic extent. The client can incrementally refresh blocks from `update_3d_tiles`.

:::tip Stability note
For stability, the first N frames (N ≈ 10) usually produce no localization or mesh output. After localization stabilizes, poses and products are written to disk.
:::

#### EndTask

**Purpose:** End the current real-time task.

Request format (EndTaskRequest)

| Field name | Type | Description |
|--------|------|------|
| (none) | — | End-task request |

Response format (EndTaskResponse)

| Field name | Type | Description |
|--------|------|------|
| `state` | int32 | Task end status (`0` = success, other values = failure) |
