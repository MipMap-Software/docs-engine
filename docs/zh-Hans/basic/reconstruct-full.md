---
title: 全流程重建
sidebar_position: 1
---

## 全流程重建

:::tip 开始之前
如果您还未安装 SDK，请先查看[安装指南Windows](../overview/installation)或[安装指南Linux](../overview/linux-deployment)。
:::

ReconstructFull 是最简单易用的全流程重建接口，适合快速启动项目和新手用户。它会自动处理图像元数据提取、空三计算和三维重建的完整流程。

### 接口特点

- ✅ **自动化程度高**：自动从图像 EXIF 读取 GPS 和相机信息
- ✅ **配置简单**：最少只需要提供图像路径
- ✅ **一步到位**：从图像直接生成最终成果
- ✅ **智能优化**：自动选择最佳处理参数

### 接口调用方式

MipMapEngine SDK 通过命令行的方式进行调用，不限制开发语言，通过指定`reconstruct_type`参数为`0`来执行全流程重建，命令如下：

```bash
reconstruct_full_engine(.exe) --reconstruct_type 0 --task_json /path/to/task.json
```

- `reconstruct_type`: 接口 Type，固定为 `0`（表示全流程重建）
- `task_json`: 参数文件路径，具体的参数信息在下面详细介绍

### 错误处理

所有接口返回整数错误码：
- `0`: 成功
- `非0`: 各种错误情况

详见错误码参考[错误码](./error-codes)。

*提示：大多数用户使用 全流程重建接口 就能满足需求。只有在需要精细控制时才使用其他接口。*

### 必需参数

| 参数名 | 类型 | 说明 | 示例值 |
|--------|------|------|--------|
| `working_dir` | string | 工作目录路径 | `"C:/Projects/MyProject"` |
| `gdal_folder` | string | GDAL数据目录，只有Windows系统需要设置，**需要设置为全英文路径** | `"C:/MipMap/SDK/data"` |
| `image_meta_data` | array[[image_meta_data](#image-meta-data)] | 图像列表 | 格式说明查看[image_meta_data](#image-meta-data) |
| `input_image_type` | int | 图像类型 | `1`=RGB, `2`=多光谱, `3`=红外 |
| `resolution_level` | int | `1`=高, `2`=中, `3`=低 | 1 |
| `coordinate_system` | [coordinate_system](#coordinate-system) | 输入图像EXIF内嵌的POS(GPS)所属坐标系，无人机图像常内嵌WGS84坐标系，若无内嵌POS如手机、全景相机图像则为Local坐标系，详见[coordinate_system](#coordinate-system) | [常见坐标系示例](#常见的坐标系)|

:::tip 提示
`gdal_folder`是SDK包里自带的`data`或`gdal_data`目录，目录下有众多数据库文件，目录可以移动位置，指定到对应的路径即可，Linux不需要设置该字段；**必须为全英文路径**。
:::
 
### 成果类型参数

成果参数用来确定最终输出哪种类型的重建成果，成果参数必须**至少选择一种**，否则无法完成重建。

| 参数名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `generate_obj` | bool | false | 生成 OBJ 格式模型 |
| `generate_ply` | bool | false | 生成 PLY 格式模型 |
| `generate_osgb` | bool | false | 生成 OSGB 格式（倾斜摄影） |
| `generate_3d_tiles` | bool | false | 生成 3D Tiles（Cesium） |
| `generate_las` | bool | false | 生成 LAS 点云 |
| `generate_pc_ply` | bool | false | 生成点云 PLY |
| `generate_pc_osgb` | bool | false | 生成点云 OSGB (LOD) |
| `generate_pc_pnts` | bool | false | 生成点云 PNTS (3D Tiles) |
| `generate_gs_ply` | bool | false | 生成 PLY 格式高斯泼溅成果[需高斯插件和机器学习插件] |
| `generate_gs_sog` | bool | false | 生成 SOG 格式，体积压缩后的高斯泼溅成果[需高斯插件和机器学习插件] |
| `generate_gs_splat_sog_tiles` | bool | false | 生成 SOG Tiles 格式，适用于大规模渲染的高斯泼溅成果[需高斯插件和机器学习插件] |
| `generate_geotiff` | bool | false | 生成正射影像 |
| `generate_tile_2D` | bool | false | 生成 2D 瓦片 |
| `generate_2D_from_3D_model` | bool | false | 从 3D 模型生成正射 |

:::tip **重要提示**
Windwos系统生成高斯泼溅成果需要下载高斯插件，下载地址：[高斯插件](https://asset.mipmap3d.com/plugins/gs_dlls_v2.7.1.0.zip)，以及机器学习插件，下载地址：[机器学习插件](https://asset.mipmap3d.com/plugins/ml_dlls_v10.8.0.43.zip)，下载后解压并将所有 dll 格式文件存放在 reconstruct_full_engine 同级目录。
:::

### 成果坐标系参数【可选】

| 参数名 | 类型 | 说明 |
|--------|------|------|
| `coordinate_system_3d` | [coordinate_system](#coordinate_system) | 输出 3D 成果坐标系，如果不设置，则输出Local或LocalENU坐标系 |
| `coordinate_system_2d` | [coordinate_system](#coordinate_system) | 输出 2D 成果坐标系，如果不设置，则输出Local或LocalENU坐标系 |

### 高级参数

以上参数基本能满足绝大部分需求，更多高级参数可参考[高级参数](./advanced-config)。

### 参数结构详解

#### coordinate_system
| 字段名         | 类型        | 说明                                                         | 示例值         |
|----------------|------------|--------------------------------------------------------------|----------------|
| `type`         | int        | 坐标系类型：0=LocalENU，1=Local，2=Geographic，3=Projected，4=ECEF | 2              |
| `type_name`    | string     | 坐标系类型名称（可选，辅助说明）                             | "Geographic"   |
| `label`        | string     | 坐标系名称（可选）                                           | "WGS 84"       |
| `epsg_code`    | int        | EPSG 代码（地理/投影/地心坐标系需填写）                      | 4326           |
| `wkt`          | string     | WKT 字符串（可选，替代 epsg_code）                           | "..."          |
| `origin_point` | array[3]   | 原点WGS84坐标 [经度, 纬度, 高程]（仅 type=0 LocalENU 时需要）              | [114.12, 22.12, 0] |
| `offset`       | array[3]   | 坐标系原点偏移（可选），投影系设置偏移值可避免大数精度损失导致的模型浏览闪烁                                       | [0, 0, 0]      |


#### image_meta_data
| 字段名                | 类型         | 是否必需 | 说明                                                         |
|----------------------|--------------|----------|--------------------------------------------------------------|
| `id`                 | int          | 是       | 图像唯一ID                                                   |
| `path`               | string       | 是       | 图像绝对路径                                                 |
| `group`              | string       | 否       | 相机组名称（多相机系统时使用） 
| `mask_path`              | string       | 否       | 二值掩膜图的路径，PNG格式，值为0的像素区域将在重建处理中被忽略 

- **`group` 参数**

用于多相机系统，将不同相机的图像分组处理：

示例：
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

常见分组场景：
- **五目倾斜相机**：nadir, oblique_forward, oblique_backward, oblique_left, oblique_right
- **双相机系统**：wide_angle, telephoto
- **多机协同**：drone_1, drone_2, drone_3
- **多时相**：morning, afternoon

- **`mask_path` 参数**

二值掩膜图用来标记无效区域，SDK将在重建时忽略无效区域，提升效率并获取更干净的成果

示例：
```json
{
  "id": 1,                           // 唯一标识符
  "path": "C:/Images/DJI_0001.JPG",  // 图像路径
  "group": "camera_1"                // 相机分组【可选】
  "mask_path": "C:/Images/DJI_0001.PNG" // 二值掩膜图【可选】
}
```

### 配置示例

#### 常见的坐标系
```json
{
  "type": 2,              // 0=LocalENU，1=Local，2=Geographic，3=Projected
  "type_name": "Geographic",
  "label": "WGS 84",     // 坐标系名称
  "epsg_code": 4326      // EPSG 代码
}
{
  "type": 3,              // 0=LocalENU，1=Local，2=Geographic，3=Projected
  "type_name": "Projected",
  "label": "WGS 84 / UTM zone 50N",     // 坐标系名称
  "epsg_code": 32650,      // EPSG 代码
  "offset": [
      269431.5374728,
      3364907.7329131,
      90.337
  ]
}
{
  "type": 2,              // 0=LocalENU，1=Local，2=Geographic，3=Projected
  "type_name": "Geographic",
  "label": "CGCS2000",     // 坐标系名称
  "epsg_code": 4490      // EPSG 代码
}
{
  "type": 2,              // 0=LocalENU，1=Local，2=Geographic，3=Projected
  "type_name": "Projected",
  "label": "CGCS2000 / 3-degree Gauss-Kruger CM 114E",     // 坐标系名称
  "epsg_code": 4547,      // EPSG 代码
  "offset": [
      269431.5374728,
      3364907.7329131,
      90.337
  ]
}
```

#### 最简任务配置

```json
{
  "working_dir": "C:/Projects/QuickStart",
  "gdal_folder": "C:/MipMap/SDK/data",
  "input_image_type": 1,
  "resolution_level": 2,
  "coordinate_system": {
    "type": 2,
    "label": "WGS 84",
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

#### 多相机组任务配置

```json
{
  "working_dir": "D:/Projects/MultiCamera",
  "gdal_folder": "D:/MipMap/SDK/data",
  "input_image_type": 1,
  "resolution_level": 1,
  "coordinate_system": {
    "type": 2,
    "label": "WGS 84",
    "epsg_code": 4326
  },
  
  // 多相机图像分组
  "image_meta_data": [
    // 下视相机
    {"id": 1, "path": "nadir/IMG_001.jpg", "group": "nadir"},
    {"id": 2, "path": "nadir/IMG_002.jpg", "group": "nadir"},
    
    // 前视倾斜相机
    {"id": 3, "path": "forward/IMG_001.jpg", "group": "oblique_f"},
    {"id": 4, "path": "forward/IMG_002.jpg", "group": "oblique_f"},
    
    // 更多相机组...
  ],
  
  "generate_osgb": true,
  "generate_3d_tiles": true,
  "generate_geotiff": true
}
```