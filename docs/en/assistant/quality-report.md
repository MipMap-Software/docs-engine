---
title: Reconstruction Quality Report
sidebar_position: 3
---

## Reconstruction Quality Report

### Overview

After a reconstruction task completes, the MipMapEngine SDK generates a detailed quality report with hardware information, reconstruction efficiency, parameter settings, and output quality. The report is stored as JSON in `report/report.json`, with visualization thumbnails for quick preview.

### Report Structure

#### 1. Device Information

Records the hardware used for the reconstruction task:

| Field | Type | Description |
|------|------|------|
| `cpu_name` | string | CPU name |
| `gpu_name` | string | GPU name |

Example:
```json
{
  "cpu_name": "Intel(R) Core(TM) i9-10900K CPU @ 3.70GHz",
  "gpu_name": "NVIDIA GeForce RTX 3080"
}
```

#### 2. Reconstruction Efficiency

Records processing time per stage (unit: minutes):

| Field | Type | Description |
|------|------|------|
| `feature_extraction_time` | float | Feature extraction time |
| `feature_match_time` | float | Feature matching time |
| `sfm_time` | float | Bundle adjustment time |
| `at_time` | float | Total AT (aerial triangulation) time |
| `reconstruction_time` | float | Total reconstruction time (excluding AT) |

Example:
```json
{
  "feature_extraction_time": 12.5,
  "feature_match_time": 8.3,
  "sfm_time": 15.2,
  "at_time": 36.0,
  "reconstruction_time": 48.6
}
```

#### 3. Reconstruction Parameters

Records task input parameters and configuration:

**Camera parameters**

```json
"initial_camera_parameters": [
  {
    "camera_name": "DJI_FC6310",
    "width": 5472,
    "height": 3648,
    "id": 0,
    "parameters": [3850.5, 2736, 1824, -0.02, 0.05, 0.001, -0.001, 0.01]
  }
]
```

Parameter array order: `[f, cx, cy, k1, k2, p1, p2, k3]`
- `f`: focal length
- `cx, cy`: principal point coordinates
- `k1, k2, k3`: radial distortion coefficients
- `p1, p2`: tangential distortion coefficients

**Other parameters**
| Field | Type | Description |
|------|------|------|
| `input_camera_count` | int | Number of input cameras |
| `input_image_count` | int | Number of input images |
| `reconstruction_level` | int | Reconstruction precision (1=ultra-high, 2=high, 3=medium) |
| `production_type` | string | Product type |
| `max_ram` | float | Peak memory usage (GB) |

**Coordinate system**
```json
"production_cs_3d": {
  "epsg_code": 4326,
  "origin_offset": [0, 0, 0],
  "type": 2
}
```

Coordinate system types:
- 0: LocalENU (local east-north-up)
- 1: Local (local coordinate system)
- 2: Geodetic (geodetic coordinate system)
- 3: Projected (projected coordinate system)
- 4: ECEF (Earth-centered, Earth-fixed)

#### 4. Reconstruction Results

**Camera parameters after AT**
Records optimized camera intrinsics:
```json
"AT_camera_parameters": [
  {
    "camera_name": "DJI_FC6310",
    "width": 5472,
    "height": 3648,
    "id": 0,
    "parameters": [3852.1, 2735.8, 1823.6, -0.019, 0.048, 0.0008, -0.0009, 0.009]
  }
]
```

**Image position differences**
Records position adjustment per image:
```json
"image_pos_diff": [
  {
    "id": 0,
    "pos_diff": 0.125
  },
  {
    "id": 1,
    "pos_diff": 0.087
  }
]
```

**Quality metrics**
| Field | Type | Description |
|------|------|------|
| `removed_image_count` | int | Images removed after AT |
| `residual_rmse` | float | RMS error of image point residuals |
| `tie_point_count` | int | Number of tie points |
| `scene_area` | float | Scene area (square meters) |
| `scene_gsd` | float | Ground sample distance (meters) |
| `flight_height` | float | Flight height (meters) |
| `block_count` | int | Number of reconstruction blocks |

#### 5. Other Information

| Field | Type | Description |
|------|------|------|
| `sdk_version` | string | SDK version |

### Visualization Thumbnails

The `thumbnail` folder under the report directory contains:

#### 1. Camera Residual Plot
`camera_{id}_residual.png` — 24-bit color image

- **Good calibration**: residuals are similar in magnitude across the image with random direction
- **Poor calibration**: large residuals with clear directional bias

:::tip
Large residuals do not necessarily mean poor overall accuracy; they reflect internal camera precision only. Final accuracy should also consider checkpoint coordinate accuracy and model quality.
:::

#### 2. Overlap Map
`overlap_map.png` — 8-bit grayscale image

- Pixel values: 0–255
- Can be rendered as a color map for overlap distribution
- Used to evaluate flight planning and image coverage

#### 3. Survey Area Thumbnail
`rgb_thumbnail.jpg` — 32-bit color image

- Quick project preview
- Shows survey extent and reconstruction appearance

### Report Interpretation Example

#### Full Report Example
```json
{
  "cpu_name": "Intel(R) Core(TM) i9-10900K CPU @ 3.70GHz",
  "gpu_name": "NVIDIA GeForce RTX 3080",
  "feature_extraction_time": 12.5,
  "feature_match_time": 8.3,
  "sfm_time": 15.2,
  "at_time": 36.0,
  "reconstruction_time": 48.6,
  "initial_camera_parameters": [{
    "camera_name": "DJI_FC6310",
    "width": 5472,
    "height": 3648,
    "id": 0,
    "parameters": [3850.5, 2736, 1824, -0.02, 0.05, 0.001, -0.001, 0.01]
  }],
  "input_camera_count": 1,
  "input_image_count": 156,
  "reconstruction_level": 2,
  "production_type": "all",
  "production_cs_3d": {
    "epsg_code": 4326,
    "origin_offset": [0, 0, 0],
    "type": 2
  },
  "production_cs_2d": {
    "epsg_code": 3857,
    "origin_offset": [0, 0, 0],
    "type": 3
  },
  "max_ram": 28.5,
  "AT_camera_parameters": [{
    "camera_name": "DJI_FC6310",
    "width": 5472,
    "height": 3648,
    "id": 0,
    "parameters": [3852.1, 2735.8, 1823.6, -0.019, 0.048, 0.0008, -0.0009, 0.009]
  }],
  "removed_image_count": 2,
  "residual_rmse": 0.68,
  "tie_point_count": 125840,
  "scene_area": 850000.0,
  "scene_gsd": 0.025,
  "flight_height": 120.5,
  "block_count": 1,
  "sdk_version": "3.0.1"
}
```

#### Quality Assessment Criteria

**Excellent quality**
- `residual_rmse` < 1.0 pixels
- `removed_image_count` / `input_image_count` < 5%
- `tie_point_count` > 10000
- Average position difference < 0.5 m

**Situations requiring attention**
- `residual_rmse` > 2.0 pixels: possible systematic error
- `removed_image_count` > 10%: image quality or overlap issues
- `tie_point_count` < 5000: insufficient tie points, affecting accuracy

### Report Parsing Tools

#### Python Parsing Example
```python
import json
import numpy as np

def analyze_quality_report(report_path):
    with open(report_path, 'r', encoding='utf-8') as f:
        report = json.load(f)
    
    # Compute efficiency metrics
    total_time = report['at_time'] + report['reconstruction_time']
    images_per_minute = report['input_image_count'] / total_time
    
    # Compute quality metrics
    removal_rate = report['removed_image_count'] / report['input_image_count']
    avg_pos_diff = np.mean([item['pos_diff'] for item in report['image_pos_diff']])
    
    # Build analysis report
    analysis = {
        'efficiency': {
            'total_time_minutes': total_time,
            'images_per_minute': images_per_minute,
            'area_per_hour': report['scene_area'] / (total_time / 60)
        },
        'quality': {
            'residual_rmse': report['residual_rmse'],
            'removal_rate_percent': removal_rate * 100,
            'avg_position_diff_meters': avg_pos_diff,
            'tie_points_per_image': report['tie_point_count'] / report['input_image_count']
        },
        'scale': {
            'area_sqm': report['scene_area'],
            'gsd_cm': report['scene_gsd'] * 100,
            'flight_height_m': report['flight_height']
        }
    }
    
    return analysis

# Example usage
analysis = analyze_quality_report('report/report.json')
print(f"Throughput: {analysis['efficiency']['images_per_minute']:.1f} images/min")
print(f"Mean residual: {analysis['quality']['residual_rmse']:.2f} px")
print(f"Ground resolution: {analysis['scale']['gsd_cm']:.1f} cm")
```

#### Quality Report Visualization
```python
import matplotlib.pyplot as plt
from PIL import Image

def visualize_quality_report(report_dir):
    # Load report data
    with open(f'{report_dir}/report.json', 'r') as f:
        report = json.load(f)
    
    # Create charts
    fig, axes = plt.subplots(2, 2, figsize=(12, 10))
    
    # 1. Processing time pie chart
    times = [
        report['feature_extraction_time'],
        report['feature_match_time'],
        report['sfm_time'],
        report['reconstruction_time']
    ]
    labels = ['Feature extraction', 'Feature matching', 'Bundle adjustment', '3D reconstruction']
    axes[0, 0].pie(times, labels=labels, autopct='%1.1f%%')
    axes[0, 0].set_title('Processing time distribution')
    
    # 2. Position difference histogram
    pos_diffs = [item['pos_diff'] for item in report['image_pos_diff']]
    axes[0, 1].hist(pos_diffs, bins=20, edgecolor='black')
    axes[0, 1].set_xlabel('Position difference (m)')
    axes[0, 1].set_ylabel('Image count')
    axes[0, 1].set_title('Image position adjustment distribution')
    
    # 3. Overlap map
    overlap_img = Image.open(f'{report_dir}/thumbnail/overlap_map.png')
    axes[1, 0].imshow(overlap_img, cmap='jet')
    axes[1, 0].set_title('Image overlap distribution')
    axes[1, 0].axis('off')
    
    # 4. Key metrics text
    metrics_text = f"""
    Input images: {report['input_image_count']}
    Removed images: {report['removed_image_count']}
    Residual RMSE: {report['residual_rmse']:.2f} px
    Tie points: {report['tie_point_count']:,}
    Scene area: {report['scene_area']/10000:.1f} ha
    Ground resolution: {report['scene_gsd']*100:.1f} cm
    """
    axes[1, 1].text(0.1, 0.5, metrics_text, fontsize=12, 
                    verticalalignment='center', family='monospace')
    axes[1, 1].set_title('Key quality metrics')
    axes[1, 1].axis('off')
    
    plt.tight_layout()
    plt.savefig('quality_report_summary.png', dpi=150)
    plt.show()
```

### Automated Quality Checks

#### Quality Threshold Configuration
```python
QUALITY_THRESHOLDS = {
    'excellent': {
        'residual_rmse': 0.5,
        'removal_rate': 0.02,
        'tie_points_per_image': 1000,
        'pos_diff_avg': 0.1
    },
    'good': {
        'residual_rmse': 1.0,
        'removal_rate': 0.05,
        'tie_points_per_image': 500,
        'pos_diff_avg': 0.5
    },
    'acceptable': {
        'residual_rmse': 2.0,
        'removal_rate': 0.10,
        'tie_points_per_image': 200,
        'pos_diff_avg': 1.0
    }
}

def assess_quality(report):
    """Automatically assess reconstruction quality level"""
    
    # Compute metrics
    removal_rate = report['removed_image_count'] / report['input_image_count']
    tie_points_per_image = report['tie_point_count'] / report['input_image_count']
    pos_diff_avg = np.mean([item['pos_diff'] for item in report['image_pos_diff']])
    
    # Evaluate level
    for level, thresholds in QUALITY_THRESHOLDS.items():
        if (report['residual_rmse'] <= thresholds['residual_rmse'] and
            removal_rate <= thresholds['removal_rate'] and
            tie_points_per_image >= thresholds['tie_points_per_image'] and
            pos_diff_avg <= thresholds['pos_diff_avg']):
            return level
    
    return 'poor'
```

### Report Integration

#### Batch Quality Monitoring
```python
def batch_quality_monitor(project_dirs):
    """Batch project quality monitoring"""
    
    results = []
    
    for project_dir in project_dirs:
        report_path = os.path.join(project_dir, 'report/report.json')
        
        if os.path.exists(report_path):
            with open(report_path, 'r') as f:
                report = json.load(f)
            
            quality_level = assess_quality(report)
            
            results.append({
                'project': project_dir,
                'images': report['input_image_count'],
                'area': report['scene_area'],
                'gsd': report['scene_gsd'],
                'rmse': report['residual_rmse'],
                'quality': quality_level,
                'time': report['at_time'] + report['reconstruction_time']
            })
    
    # Generate summary report
    df = pd.DataFrame(results)
    df.to_csv('batch_quality_report.csv', index=False)
    
    # Statistics
    print(f"Total projects: {len(results)}")
    print(f"Excellent: {len(df[df['quality'] == 'excellent'])}")
    print(f"Good: {len(df[df['quality'] == 'good'])}")
    print(f"Acceptable: {len(df[df['quality'] == 'acceptable'])}")
    print(f"Poor: {len(df[df['quality'] == 'poor'])}")
    
    return df
```

### Best Practices

1. **Review reports regularly**: Check quality metrics after each reconstruction
2. **Establish baselines**: Record typical project metrics as reference
3. **Alert on anomalies**: Use scripts to flag out-of-range metrics
4. **Trend analysis**: Track quality metrics over time
5. **Optimize from metrics**: Adjust capture and processing parameters based on the report

---

:::tip
The quality report is an important tool for evaluating and optimizing reconstruction workflows; consider integrating it into automated pipelines.
:::
