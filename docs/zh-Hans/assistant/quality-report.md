---
title: 重建质量报告
sidebar_position: 3
---

## 重建质量报告

### 概述

MipMapEngine SDK 在完成重建任务后会生成详细的质量报告，包含设备信息、重建效率、参数设置和成果质量等信息。报告以 JSON 格式存储在 `report/report.json` 文件中，同时提供可视化缩略图用于快速预览。

### 报告结构

#### 1. 设备信息

记录执行重建任务的硬件配置：

| 字段 | 类型 | 说明 |
|------|------|------|
| `cpu_name` | string | CPU 名称 |
| `gpu_name` | string | GPU 名称 |

示例：
```json
{
  "cpu_name": "Intel(R) Core(TM) i9-10900K CPU @ 3.70GHz",
  "gpu_name": "NVIDIA GeForce RTX 3080"
}
```

#### 2. 重建效率

记录各阶段的处理时间（单位：分钟）：

| 字段 | 类型 | 说明 |
|------|------|------|
| `feature_extraction_time` | float | 特征提取时间 |
| `feature_match_time` | float | 特征匹配时间 |
| `sfm_time` | float | 光束法平差时间 |
| `at_time` | float | 空三总时间 |
| `reconstruction_time` | float | 重建总时间（不含空三） |

示例：
```json
{
  "feature_extraction_time": 12.5,
  "feature_match_time": 8.3,
  "sfm_time": 15.2,
  "at_time": 36.0,
  "reconstruction_time": 48.6
}
```

#### 3. 重建参数

记录任务的输入参数和配置：

**相机参数**

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

参数数组顺序：`[f, cx, cy, k1, k2, p1, p2, k3]`
- `f`: 焦距
- `cx, cy`: 主点坐标
- `k1, k2, k3`: 径向畸变系数
- `p1, p2`: 切向畸变系数

**其他参数**
| 字段 | 类型 | 说明 |
|------|------|------|
| `input_camera_count` | int | 输入相机数量 |
| `input_image_count` | int | 输入图像数量 |
| `reconstruction_level` | int | 重建精度 (1=超高, 2=高, 3=中) |
| `production_type` | string | 产品类型 |
| `max_ram` | float | 最大内存使用 (GB) |

**坐标系信息**
```json
"production_cs_3d": {
  "epsg_code": 4326,
  "origin_offset": [0, 0, 0],
  "type": 2
}
```

坐标系类型：
- 0: LocalENU（本地东北天）
- 1: Local（本地坐标系）
- 2: Geodetic（大地坐标系）
- 3: Projected（投影坐标系）
- 4: ECEF（地心地固坐标系）

#### 4. 重建结果

**空三后的相机参数**
记录优化后的相机内参：
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

**图像位置差异**
记录每张图像的位置优化量：
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

**质量指标**
| 字段 | 类型 | 说明 |
|------|------|------|
| `removed_image_count` | int | 空三后移除的图像数 |
| `residual_rmse` | float | 图像点残差均方根误差 |
| `tie_point_count` | int | 连接点数量 |
| `scene_area` | float | 场景面积 (平方米) |
| `scene_gsd` | float | 地面采样距离 (米) |
| `flight_height` | float | 飞行高度 (米) |
| `block_count` | int | 重建分块数 |

#### 5. 其他信息

| 字段 | 类型 | 说明 |
|------|------|------|
| `sdk_version` | string | SDK 版本号 |

### 可视化缩略图

报告目录下的 `thumbnail` 文件夹包含以下可视化文件：

#### 1. 相机残差图
`camera_{id}_residual.png` - 24位彩色图像

- **良好的标定结果**：残差在各位置大小相近，方向随机
- **较差的标定结果**：残差较大且有明显方向性

:::tip
大残差不一定表示整体精度差，这仅反映相机内部精度。最终精度需综合考虑检查点坐标精度和模型质量。
:::

#### 2. 重叠度图
`overlap_map.png` - 8位灰度图像

- 像素值范围：0-255
- 可渲染为彩色图显示重叠度分布
- 用于评估航线设计和图像覆盖质量

#### 3. 测区缩略图
`rgb_thumbnail.jpg` - 32位彩色图像

- 用于项目快速预览
- 显示测区范围和重建效果

### 报告解读示例

#### 完整报告示例
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

#### 质量评估指标

**优秀质量标准**
- `residual_rmse` < 1.0 像素
- `removed_image_count` / `input_image_count` < 5%
- `tie_point_count` > 10000
- 位置差异平均值 < 0.5米

**需要注意的情况**
- `residual_rmse` > 2.0 像素：可能存在系统误差
- `removed_image_count` > 10%：图像质量或重叠度问题
- `tie_point_count` < 5000：特征点不足，影响精度

### 报告解析工具

#### Python 解析示例
```python
import json
import numpy as np

def analyze_quality_report(report_path):
    with open(report_path, 'r', encoding='utf-8') as f:
        report = json.load(f)
    
    # 计算效率指标
    total_time = report['at_time'] + report['reconstruction_time']
    images_per_minute = report['input_image_count'] / total_time
    
    # 计算质量指标
    removal_rate = report['removed_image_count'] / report['input_image_count']
    avg_pos_diff = np.mean([item['pos_diff'] for item in report['image_pos_diff']])
    
    # 生成分析报告
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

# 使用示例
analysis = analyze_quality_report('report/report.json')
print(f"处理效率: {analysis['efficiency']['images_per_minute']:.1f} 张/分钟")
print(f"平均残差: {analysis['quality']['residual_rmse']:.2f} 像素")
print(f"地面分辨率: {analysis['scale']['gsd_cm']:.1f} 厘米")
```

#### 质量报告可视化
```python
import matplotlib.pyplot as plt
from PIL import Image

def visualize_quality_report(report_dir):
    # 读取报告数据
    with open(f'{report_dir}/report.json', 'r') as f:
        report = json.load(f)
    
    # 创建图表
    fig, axes = plt.subplots(2, 2, figsize=(12, 10))
    
    # 1. 时间分布饼图
    times = [
        report['feature_extraction_time'],
        report['feature_match_time'],
        report['sfm_time'],
        report['reconstruction_time']
    ]
    labels = ['特征提取', '特征匹配', '光束法平差', '3D重建']
    axes[0, 0].pie(times, labels=labels, autopct='%1.1f%%')
    axes[0, 0].set_title('处理时间分布')
    
    # 2. 位置差异直方图
    pos_diffs = [item['pos_diff'] for item in report['image_pos_diff']]
    axes[0, 1].hist(pos_diffs, bins=20, edgecolor='black')
    axes[0, 1].set_xlabel('位置差异 (米)')
    axes[0, 1].set_ylabel('图像数量')
    axes[0, 1].set_title('图像位置优化量分布')
    
    # 3. 重叠度图
    overlap_img = Image.open(f'{report_dir}/thumbnail/overlap_map.png')
    axes[1, 0].imshow(overlap_img, cmap='jet')
    axes[1, 0].set_title('图像重叠度分布')
    axes[1, 0].axis('off')
    
    # 4. 关键指标文本
    metrics_text = f"""
    输入图像: {report['input_image_count']}
    移除图像: {report['removed_image_count']}
    残差RMSE: {report['residual_rmse']:.2f} px
    连接点数: {report['tie_point_count']:,}
    场景面积: {report['scene_area']/10000:.1f} 公顷
    地面分辨率: {report['scene_gsd']*100:.1f} cm
    """
    axes[1, 1].text(0.1, 0.5, metrics_text, fontsize=12, 
                    verticalalignment='center', family='monospace')
    axes[1, 1].set_title('关键质量指标')
    axes[1, 1].axis('off')
    
    plt.tight_layout()
    plt.savefig('quality_report_summary.png', dpi=150)
    plt.show()
```

### 自动化质量检查

#### 质量阈值配置
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
    """自动评估重建质量等级"""
    
    # 计算指标
    removal_rate = report['removed_image_count'] / report['input_image_count']
    tie_points_per_image = report['tie_point_count'] / report['input_image_count']
    pos_diff_avg = np.mean([item['pos_diff'] for item in report['image_pos_diff']])
    
    # 评估等级
    for level, thresholds in QUALITY_THRESHOLDS.items():
        if (report['residual_rmse'] <= thresholds['residual_rmse'] and
            removal_rate <= thresholds['removal_rate'] and
            tie_points_per_image >= thresholds['tie_points_per_image'] and
            pos_diff_avg <= thresholds['pos_diff_avg']):
            return level
    
    return 'poor'
```

### 报告集成应用

#### 批处理质量监控
```python
def batch_quality_monitor(project_dirs):
    """批量项目质量监控"""
    
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
    
    # 生成汇总报告
    df = pd.DataFrame(results)
    df.to_csv('batch_quality_report.csv', index=False)
    
    # 统计信息
    print(f"Total projects: {len(results)}")
    print(f"Excellent: {len(df[df['quality'] == 'excellent'])}")
    print(f"Good: {len(df[df['quality'] == 'good'])}")
    print(f"Acceptable: {len(df[df['quality'] == 'acceptable'])}")
    print(f"Poor: {len(df[df['quality'] == 'poor'])}")
    
    return df
```

### 最佳实践

1. **定期检查报告**：每次重建完成后查看质量指标
2. **建立基准**：记录典型项目的质量指标作为参考
3. **异常预警**：设置自动化脚本检测异常指标
4. **趋势分析**：长期跟踪质量指标变化趋势
5. **优化建议**：根据报告指标调整拍摄和处理参数

---

:::tip 提示
质量报告是评估和优化重建流程的重要工具，建议将其集成到自动化工作流程中。
:::