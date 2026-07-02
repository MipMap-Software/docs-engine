---
title: 高级参数
sidebar_position: 3
---


## 高级参数

在SDK输入的JSON参数文件内，可以设置一些高阶参数，更灵活的控制。

### 重建模块
| 参数名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `min_avali_memory_size` | double | 0 | 设备最小可支持内存，可以控制分块重建的大小和数量，为0则最大化使用设备可用内存 |
| `fill_water_area_with_AI` | bool | false | 是否使用机器学习进行水域识别填补`[需机器学习插件]` |
| `build_overview` | bool | true | 二维成果是否构建图像金字塔 |
| `cut_frame_2d` | bool | false | 二维成果是否分幅输出 |
| `cut_frame_width` | int | 5000 | 二维成果分幅输出的单幅像素尺寸， `cut_frame_2d` 为`true`时有效 |
| `dom_gsd` | double | 0 | 二维真正射影像的地面分辨率，默认为0，表示自动计算，为了防止图像过大，SDK做了小值保护，设置过小的值不生效 |
| `tile_extention` | string | ".webp" | 真正射二维瓦片成果的图像格式，支持格式： ".png"、".webp" |
| `use_gcj02` | bool | false | 是否将正射影像瓦片转换为 GCJ02 坐标系 |
| `keep_undistort_images` | bool | false | 是否在重建完成后保留去畸变图像，默认不保留 |
| `remove_small_islands` | bool | false | 是否删除三维模型的孤立悬浮物 |
| `mesh_decimate_ratio` | double | 1.0 | 三维模型网格的简化率，值越小简化力度越大 |
| `use_draco_compression` | bool | true | 3D Tiles 成果是否启用 Draco 顶点压缩 |
| `tex_image_type_3dtiles` | int | 0 | 3D Tiles 纹理格式：0=JPEG，1=WebP |
| `roi_for_2d` | [roi](#roi) | - | 二维重建的感兴趣区域，格式查看[ROI结构释义](#roi)  |
| `roi_for_3d` | [roi](#roi) | - | 三维重建的感兴趣区域，格式查看[ROI结构释义](#roi)  |
| `roi_coordinate_system` | JSON | WGS84 | ROI 的坐标系，格式查看[坐标系](../basic/reconstruct-full#coordinate-system)  |

:::tip **重要提示**
Windwos系统开启`fill_water_area_with_AI`机器学习水域填补需要下载机器学习插件，下载地址：[机器学习插件](https://asset.mipmap3d.com/plugins/ml_dlls_v10.8.0.43.zip)，下载后解压并将所有 dll 格式文件存放在 reconstruct_full_engine 同级目录
:::

### 参数结构详解

#### roi

ROI描述了一个空间范围，包括平面范围`boundary`、最小高程`min_z`和最大高程`max_z`。

| 字段名 | 类型 | 说明 |
|--------|------|------|
| `boundary` | array | ROI多边形的2D边界顶点集合 |
| `min_z` | double | ROI最小高程 |
| `max_z` | double | ROI最大高程 |
|

示例：
```json
{
  "boundary": [              // 2D 多边形边界（逆时针）
    [x1, y1],
    [x2, y2],
    [x3, y3]
    // ...
  ],
  "min_z": 100.0,           // 最小高程
  "max_z": 500.0            // 最大高程
}
```