---
title: Basic Concepts
sidebar_position: 5
---

## Basic Concepts

Before using MipMapEngine SDK, understanding a few core concepts will help you work more effectively. This chapter introduces 3D reconstruction fundamentals in plain language.

*If you are an experienced photogrammetry professional or want API details directly, you may skip this chapter and read [Full-pipeline reconstruction](../basic/reconstruct-full).*

### What Is 3D Reconstruction?

3D reconstruction is the process of creating a 3D model from 2D images. Imagine you take many photos around a building; photogrammetry and reconstruction technology can:

1. Analyze relationships between the photos
2. Compute the camera position for each exposure
3. Derive 3D information for corresponding pixels from camera pose and image texture
4. Represent object or scene geometry as a point cloud or mesh
5. Apply textures from the source images to build a realistic 3D model

:::tip Applications of 3D reconstruction

- **Surveying and mapping**: High-accuracy topographic maps and orthophotos
- **Urban planning**: City-scale 3D models for design and planning
- **Cultural heritage**: Digital preservation of historic buildings and artifacts
- **Emergency response**: Rapid 3D capture of disaster scenes
- **Construction monitoring**: Track progress and change on engineering projects
- **3D content production**: Assets for games, film, AR/VR

:::

### Photogrammetry Fundamentals

#### 3D Reconstruction Pipeline

```mermaid
flowchart TB
    %% Style definitions
    classDef inputStyle fill:#4CAF50,stroke:#4CAF50,stroke-width:3px,color:#fff,font-weight:bold
    classDef processStyle fill:#2196F3,stroke:#2196F3,stroke-width:3px,color:#fff,font-weight:bold
    classDef orangeStyle fill:#FF9800,stroke:#FF9800,stroke-width:3px,color:#fff,font-weight:bold
    classDef purpleStyle fill:#9C27B0,stroke:#9C27B0,stroke-width:3px,color:#fff,font-weight:bold
    classDef decisionStyle fill:#FFC107,stroke:#FFC107,stroke-width:3px,color:#333,font-weight:bold
    classDef outputStyle fill:#4CAF50,stroke:#4CAF50,stroke-width:2px,color:#fff
    classDef branch3DStyle fill:#E91E63,stroke:#E91E63,stroke-width:2px,color:#fff
    classDef branch2DStyle fill:#795548,stroke:#795548,stroke-width:2px,color:#fff
    classDef branchPCStyle fill:#00BCD4,stroke:#00BCD4,stroke-width:2px,color:#fff
    classDef subgraphStyle fill:#f9f9f9,stroke:#666,stroke-width:2px
    classDef noteStyle fill:#FFF3E0,stroke:#F57C00,stroke-width:1px,color:#333
    
    %% Main flow
    Input["📷 Input images<br/>Aerial photo set"]:::inputStyle
    Extract["🔍 Auto extraction<br/>EXIF/GPS/camera parameters"]:::processStyle
    AT["📐 Aerial triangulation<br/>Camera pose estimation"]:::orangeStyle
    Dense["🎯 Dense matching<br/>Dense point cloud"]:::purpleStyle
    Mode{"🎭 Reconstruction mode"}:::decisionStyle
    
    Input --> Extract
    Extract --> AT
    AT --> Dense
    Dense --> Mode
    
    %% 3D branch
    subgraph 3D[" "]
        direction TB
        3DTitle["🏗️ 3D reconstruction"]:::noteStyle
        Mesh["Mesh reconstruction<br/>Build 3D model"]:::branch3DStyle
        Texture["Texture mapping<br/>Apply imagery"]:::branch3DStyle
        LOD["LOD generation<br/>Multi-level detail"]:::branch3DStyle
        
        3DTitle -.-> Mesh
        Mesh --> Texture
        Texture --> LOD
        
        %% 3D outputs
        subgraph 3DFormats[" "]
            OBJ["📦 OBJ<br/>General format"]:::outputStyle
            OSGB["🏙️ OSGB<br/>Oblique photogrammetry"]:::outputStyle
            Tiles3D["🌐 3D Tiles<br/>Web delivery"]:::outputStyle
        end
        
        LOD --> 3DFormats
    end
    
    %% 2D branch
    subgraph 2D[" "]
        direction TB
        2DTitle["🗺️ 2D reconstruction"]:::noteStyle
        DSM["Digital surface model<br/>DSM generation"]:::branch2DStyle
        DOM["Orthophoto<br/>DOM generation"]:::branch2DStyle
        Tiles["Map tiles<br/>Tile pyramids"]:::branch2DStyle
        
        2DTitle -.-> DSM
        DSM --> DOM
        DOM --> Tiles
        
        %% 2D outputs
        subgraph 2DFormats[" "]
            GeoTIFF["🌍 GeoTIFF<br/>Georeferenced"]:::outputStyle
            DOMTiles["🗺️ DOM tiles<br/>Online maps"]:::outputStyle
            DSMTiles["📊 DSM tiles<br/>Elevation data"]:::outputStyle
        end
        
        Tiles --> 2DFormats
    end
    
    %% Point cloud branch
    subgraph PC[" "]
        direction TB
        PCTitle["☁️ Point cloud output"]:::noteStyle
        PointOpt["Point cloud optimization<br/>Classification & filtering"]:::branchPCStyle
        
        PCTitle -.-> PointOpt
        
        %% Point cloud outputs
        subgraph PCFormats[" "]
            LAS["📍 LAS<br/>Standard format"]:::outputStyle
            PLY["🔵 PLY<br/>General format"]:::outputStyle
            PNTS["🌐 PNTS<br/>3D Tiles point cloud"]:::outputStyle
        end
        
        PointOpt --> PCFormats
    end
    
    %% Mode connections
    Mode -->|"3D model"| 3D
    Mode -->|"Orthophoto products"| 2D
    Mode -->|"Point cloud"| PC
    
    %% Special mode
    Mode -.->|"2D+3D mode"| Extra["🎁 Additional outputs<br/>Orthographic view imagery<br/>Orthographic elevation render"]:::noteStyle
    
    %% Automation
    Auto["⚡ Fully automatic<br/>Smart parameter tuning"]:::noteStyle
    Auto -.-> Extract
    
    %% Subgraph styling
    style 3D fill:#fff0f0,stroke:#E91E63,stroke-width:2px,stroke-dasharray: 5 5
    style 2D fill:#f5f0ff,stroke:#795548,stroke-width:2px,stroke-dasharray: 5 5
    style PC fill:#f0f8ff,stroke:#00BCD4,stroke-width:2px,stroke-dasharray: 5 5
```

:::tip Pipeline characteristics
- **🚀 Fully automatic**: End-to-end processing from input to output without manual steps
- **🎯 Smart decisions**: Automatically selects optimal parameters from data characteristics
- **📦 Multiple formats**: Generate several formats in one run for different applications
- **⚡ Parallel optimization**: Output branches can be processed in parallel for higher throughput
:::

:::tip Output selection guide
- **Web visualization**: 3D Tiles + DOM tiles
- **Professional analysis**: OSGB + GeoTIFF + LAS
- **General interchange**: OBJ + PLY
- **Surveying / mapping**: GeoTIFF + DSM + ground control optimization
:::

#### Aerial Triangulation

**Aerial triangulation (AT)** is the first step in 3D reconstruction. It:

- Computes the precise position and orientation of the camera for each image
- Establishes geometric relationships between images
- Builds a sparse point cloud structure of the scene

#### Dense Reconstruction

Once camera poses are known:

- Depth is computed for each pixel
- A dense 3D point cloud is generated

#### 3D Model Reconstruction

- Build a 3D mesh from the point cloud
- Create model textures from the original images
- Generate LOD models for efficient rendering of large scenes

#### Deliverable Formats

Finally, produce the outputs you need:

- **3D models**: OSGB, 3D Tiles, PLY, OBJ, FBX, and more
- **Point clouds**: LAS, PLY
- **Gaussian splats**: PLY, Splats
- **Orthophotos**: Georeferenced GeoTIFF imagery
- **Digital surface model (DSM)**: Terrain / surface elevation data

#### Standard Output Directory Layout

Every reconstruction task produces this standard layout:

```
output/
├── 2D/
│   ├── dom_tiles/      # Orthophoto tiles
│   ├── dsm_tiles/      # DSM tiles
│   └── geotiffs/       # GeoTIFF deliverables
├── 3D/
│   ├── model-b3dm/     # 3D Tiles model format
│   ├── model-osgb/     # OSGB model format
│   ├── model-ply/      # PLY model format
│   ├── model-obj/      # OBJ model format
│   ├── model-fbx/      # FBX model format
│   ├── point-ply/      # PLY point cloud
│   ├── point-las/      # LAS point cloud
│   ├── point-pnts/     # PNTS point cloud
│   ├── point-gs-ply/   # PLY Gaussian splat format
│   └── point-gs-splats/# SPLATS Gaussian splat format
├── AT/
│   ├── mvs.xml         # AT results
│   └── mvs_undistort.xml # Undistorted AT results
├── report/
│   └── report.json     # Quality report
└── log.txt             # Processing log
```

#### Output Format Reference

| Format | Use case | Characteristics |
|------|------|------|
| **3D Tiles** | Web visualization | LOD support; suited for Cesium and similar platforms |
| **OSGB** | Professional software | OpenSceneGraph format; widely supported |
| **OBJ** | General 3D models | Simple, universal, easy to edit |
| **LAS** | Point cloud workflows | Standard point cloud format with classification |
| **GeoTIFF** | GIS analysis | Georeferenced; suitable for measurement |
| **Tiles** | Online maps | Multi-resolution pyramids for fast loading |


### Key Parameters

#### Resolution Level

Controls reconstruction detail:

| Level | Description | Use case | Processing time |
|------|------|----------|----------|
| 1 | Ultra-high: maximum geometric detail and texture sharpness | Professional surveying, fine modeling | Longer |
| 2 | High: some geometric simplification; maximum texture sharpness | General production, faster delivery | Medium |
| 3 | Low | Preview, quick validation | Shorter |

#### Image Overlap

<svg viewBox="0 0 800 300" xmlns="http://www.w3.org/2000/svg">
  <!-- Background -->
  <rect width="800" height="300" fill="#f8f9fa" stroke="none"/>
  
  <!-- Title -->
  <text x="400" y="30" text-anchor="middle" font-size="18" font-weight="bold" fill="#333">Ideal image overlap</text>
  
  <!-- Image 1 -->
  <rect x="100" y="80" width="150" height="100" fill="#2196F3" opacity="0.6" stroke="#1976D2" stroke-width="2"/>
  <text x="175" y="130" text-anchor="middle" fill="white" font-size="14" font-weight="bold">Image 1</text>
  
  <!-- Image 2 -->
  <rect x="200" y="80" width="150" height="100" fill="#4CAF50" opacity="0.6" stroke="#388E3C" stroke-width="2"/>
  <text x="275" y="130" text-anchor="middle" fill="white" font-size="14" font-weight="bold">Image 2</text>
  
  <!-- Image 3 -->
  <rect x="300" y="80" width="150" height="100" fill="#FF9800" opacity="0.6" stroke="#F57C00" stroke-width="2"/>
  <text x="375" y="130" text-anchor="middle" fill="white" font-size="14" font-weight="bold">Image 3</text>
  
  <!-- Overlap annotation -->
  <path d="M 200 200 L 200 180" stroke="#333" stroke-width="1" stroke-dasharray="2,2"/>
  <path d="M 250 200 L 250 180" stroke="#333" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="225" y="220" text-anchor="middle" font-size="12" fill="#666">60–80% overlap</text>
  
  <!-- Caption -->
  <text x="400" y="260" text-anchor="middle" font-size="14" fill="#333">Recommended: 60–80% forward overlap, 40–60% side overlap</text>
</svg>

### Quality Control

#### Factors That Affect Reconstruction Quality

1. **Image quality**
   - Sharpness (avoid motion blur)
   - Lighting (even illumination is best)

2. **Acquisition parameters**
   - Overlap (>70%)
   - Flight altitude (affects ground sample distance, GSD)
   - Viewing angles (nadir + oblique combinations work best)

3. **Typical accuracy without ground control**
   - RTK/PPK: centimeter-level (1–2 cm + 1–2× GSD)
   - Standard GPS: meter-level

### Best Practices for Reconstruction Accuracy

**Reliable accuracy**: RTK and PPK workflows without ground control often achieve good results, but ground control points (GCPs) and check points remain the most reliable way to guarantee and verify accuracy. If your application must meet a strict accuracy target, or project delivery requires documented proof of accuracy, deploy GCPs and check points. Otherwise you risk costly re-flight and re-collection in the field.

