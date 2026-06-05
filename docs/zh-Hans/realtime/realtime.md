---
title: 使用说明
sidebar_position: 2
---

## 使用说明

### 数据说明
实时重建支持无人机**正射航线**采集的图像，重叠度满足航线70、旁向70以上。

:::tip 
推荐使用大疆测绘无人机 M3E/M4E，实时重建的所有照片必须是正射视角拍摄。
:::

### 正射航线示意图

<svg viewBox="0 0 800 350" xmlns="http://www.w3.org/2000/svg">
  <!-- 背景 -->
  <rect width="800" height="350" fill="#f8f9fa" stroke="none"/>
  
  <!-- 标题 -->
  <text x="400" y="30" text-anchor="middle" font-size="20" font-weight="bold" fill="#333">无人机正射航线示意图</text>
  
  <!-- 航线1 - 从左到右 -->
  <path d="M 100 200 L 700 200" stroke="#2196F3" stroke-width="4" fill="none" marker-end="url(#arrowhead1)"/>
  <text x="400" y="185" text-anchor="middle" font-size="12" fill="#1976D2" font-weight="bold">航线1</text>
  
  <!-- 航线2 - 从右到左（方向调转） -->
  <path d="M 700 250 L 100 250" stroke="#2196F3" stroke-width="4" fill="none" marker-end="url(#arrowhead2)"/>
  <text x="400" y="235" text-anchor="middle" font-size="12" fill="#1976D2" font-weight="bold">航线2</text>
  
  <!-- 航线3 - 从左到右 -->
  <path d="M 100 300 L 700 300" stroke="#2196F3" stroke-width="4" fill="none" marker-end="url(#arrowhead3)"/>
  <text x="400" y="285" text-anchor="middle" font-size="12" fill="#1976D2" font-weight="bold">航线3</text>
  
  <!-- 航线1的航点（再增加2个，共9个） -->
  <circle cx="100" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="175" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="250" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="325" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="400" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="475" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="550" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="625" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="700" cy="200" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  
  <!-- 航线2的航点（再增加2个，共9个，方向调转） -->
  <circle cx="700" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="625" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="550" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="475" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="400" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="325" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="250" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="175" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="100" cy="250" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  
  <!-- 航线3的航点（再增加2个，共9个） -->
  <circle cx="100" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="175" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="250" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="325" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="400" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="475" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="550" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="625" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  <circle cx="700" cy="300" r="8" fill="#FF5722" stroke="#D32F2F" stroke-width="2"/>
  
  <!-- 航线间连接线段 -->
  <!-- 航线1到航线2的连接 -->
  <path d="M 700 200 L 700 250" stroke="#666" stroke-width="2" fill="none" stroke-dasharray="5,5"/>
  
  <!-- 航线2到航线3的连接 -->
  <path d="M 100 250 L 100 300" stroke="#666" stroke-width="2" fill="none" stroke-dasharray="5,5"/>
  
  <!-- 拍摄范围示意 -->
  <rect x="120" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="220" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="320" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="420" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="520" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="620" y="180" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  
  <!-- 航线2的拍摄范围（方向调转） -->
  <rect x="620" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="520" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="420" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="320" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="220" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="120" y="230" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  
  <!-- 航线3的拍摄范围 -->
  <rect x="120" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="220" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="320" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="420" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="520" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  <rect x="620" y="280" width="100" height="40" fill="#4CAF50" opacity="0.2" stroke="#4CAF50" stroke-width="1" stroke-dasharray="3,3"/>
  
  <!-- 重叠度标注 -->
  <path d="M 170 160 L 170 140" stroke="#333" stroke-width="1" stroke-dasharray="2,2"/>
  <path d="M 220 160 L 220 140" stroke="#333" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="195" y="130" text-anchor="middle" font-size="12" fill="#666">航向重叠 70-80%</text>
  
  <path d="M 80 200 L 60 200" stroke="#333" stroke-width="1" stroke-dasharray="2,2"/>
  <path d="M 80 250 L 60 250" stroke="#333" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="50" y="225" text-anchor="middle" font-size="12" fill="#666" transform="rotate(-90, 50, 225)">旁向重叠 70-80%</text>
  
  <!-- 飞行参数说明 -->
  <rect x="50" y="50" width="200" height="120" fill="#FFF3E0" stroke="#F57C00" stroke-width="1"/>
  <text x="150" y="70" text-anchor="middle" font-size="14" font-weight="bold" fill="#E65100">飞行参数</text>
  <text x="60" y="90" font-size="12" fill="#333">• 飞行高度: 100-200m</text>
  <text x="60" y="110" font-size="12" fill="#333">• 航向重叠: 70-80%</text>
  <text x="60" y="130" font-size="12" fill="#333">• 旁向重叠: 70-80%</text>
  <text x="60" y="150" font-size="12" fill="#333">• 拍摄间隔: 1-3秒</text>
  
  <!-- 箭头定义 -->
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

:::tip 正射航线规划要点
- **航线间距**：根据相机视场角和飞行高度计算，确保旁向重叠度
- **拍摄间隔**：根据飞行速度和航向重叠度要求设置
- **飞行高度**：影响地面分辨率和拍摄范围
- **天气条件**：选择光照均匀、无风或微风天气
:::


### 实时重建基本流程

实时重建以一个grpc服务端程序提供：**realtime_service**，启动它即启动了实时重建服务，我们将提供一个proto文件来供开发者开发客户端程序，客户端程序完成以下操作。

（1）Initialize，初始化实时重建服务

（2）AddFrame，添加图像帧

（3）EndTask，结束本次实时重建任务

:::tip
实时重建服务基于 gRPC 通信协议实现，建议开发者具备 gRPC 客户端开发经验，以便顺利集成和调用相关接口。
:::

实时重建服务的proto文件下载页面：[Proto文件下载](https://asset.mipmap3d.com/engine/realtime.proto)

下面是实时重建服务的基本流程示意图：

<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- 背景 -->
  <rect width="800" height="500" fill="#f8f9fa" stroke="none"/>
  
  <!-- 标题 -->
  <text x="400" y="30" text-anchor="middle" font-size="20" font-weight="bold" fill="#333">实时重建服务基本流程</text>
  
  <!-- 流程步骤 -->
  <!-- 1. 启动服务 -->
  <rect x="50" y="60" width="120" height="60" fill="#E3F2FD" stroke="#2196F3" stroke-width="2" rx="5"/>
  <text x="110" y="85" text-anchor="middle" font-size="12" font-weight="bold" fill="#1976D2">1. 启动服务</text>
  <text x="110" y="100" text-anchor="middle" font-size="10" fill="#333">realtime_service</text>
  <text x="110" y="112" text-anchor="middle" font-size="10" fill="#333">监听端口50051</text>
  
  <!-- 2. 客户端连接 -->
  <rect x="250" y="60" width="120" height="60" fill="#E8F5E8" stroke="#4CAF50" stroke-width="2" rx="5"/>
  <text x="310" y="85" text-anchor="middle" font-size="12" font-weight="bold" fill="#2E7D32">2. 客户端连接</text>
  <text x="310" y="100" text-anchor="middle" font-size="10" fill="#333">建立gRPC连接</text>
  <text x="310" y="112" text-anchor="middle" font-size="10" fill="#333">指定端口号</text>
  
  <!-- 3. 初始化任务 -->
  <rect x="450" y="60" width="120" height="60" fill="#FFF3E0" stroke="#FF9800" stroke-width="2" rx="5"/>
  <text x="510" y="85" text-anchor="middle" font-size="12" font-weight="bold" fill="#F57C00">3. 初始化任务</text>
  <text x="510" y="100" text-anchor="middle" font-size="10" fill="#333">Initialize接口</text>
  <text x="510" y="112" text-anchor="middle" font-size="10" fill="#333">传递配置JSON</text>
  
  <!-- 4. 添加图像帧 -->
  <rect x="650" y="60" width="120" height="60" fill="#F3E5F5" stroke="#9C27B0" stroke-width="2" rx="5"/>
  <text x="710" y="85" text-anchor="middle" font-size="12" font-weight="bold" fill="#7B1FA2">4. 添加图像帧</text>
  <text x="710" y="100" text-anchor="middle" font-size="10" fill="#333">AddFrame接口</text>
  <text x="710" y="112" text-anchor="middle" font-size="10" fill="#333">逐帧处理</text>
  
  <!-- 箭头1 -->
  <path d="M 170 90 L 250 90" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow1)"/>
  
  <!-- 箭头2 -->
  <path d="M 370 90 L 450 90" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow2)"/>
  
  <!-- 箭头3 -->
  <path d="M 570 90 L 650 90" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow3)"/>
  
  <!-- 循环处理区域 -->
  <rect x="50" y="180" width="700" height="200" fill="#FAFAFA" stroke="#BDBDBD" stroke-width="1" stroke-dasharray="5,5" rx="10"/>
  <text x="400" y="200" text-anchor="middle" font-size="16" font-weight="bold" fill="#424242">实时处理循环</text>
  
  <!-- 图像输入 -->
  <rect x="80" y="220" width="100" height="50" fill="#E1F5FE" stroke="#03A9F4" stroke-width="2" rx="5"/>
  <text x="130" y="240" text-anchor="middle" font-size="11" font-weight="bold" fill="#0277BD">图像输入</text>
  <text x="130" y="255" text-anchor="middle" font-size="9" fill="#333">图像路径</text>
  <text x="130" y="265" text-anchor="middle" font-size="9" fill="#333">元数据</text>
  
  <!-- SLAM定位 -->
  <rect x="220" y="220" width="100" height="50" fill="#E8F5E8" stroke="#4CAF50" stroke-width="2" rx="5"/>
  <text x="270" y="240" text-anchor="middle" font-size="11" font-weight="bold" fill="#2E7D32">SLAM定位</text>
  <text x="270" y="255" text-anchor="middle" font-size="9" fill="#333">相机位姿</text>
  <text x="270" y="265" text-anchor="middle" font-size="9" fill="#333">轨迹估计</text>
  
  <!-- 二维地图重建 -->
  <rect x="360" y="220" width="100" height="50" fill="#FFF3E0" stroke="#FF9800" stroke-width="2" rx="5"/>
  <text x="410" y="240" text-anchor="middle" font-size="11" font-weight="bold" fill="#F57C00">二维地图</text>
  <text x="410" y="255" text-anchor="middle" font-size="9" fill="#333">正射图瓦片</text>
  <text x="410" y="265" text-anchor="middle" font-size="9" fill="#333">Web Mercator</text>
  
  <!-- 三维网格重建 -->
  <rect x="500" y="220" width="100" height="50" fill="#F3E5F5" stroke="#9C27B0" stroke-width="2" rx="5"/>
  <text x="550" y="240" text-anchor="middle" font-size="11" font-weight="bold" fill="#7B1FA2">三维网格</text>
  <text x="550" y="255" text-anchor="middle" font-size="9" fill="#333">PLY/3DTiles</text>
  <text x="550" y="265" text-anchor="middle" font-size="9" fill="#333">分块存储</text>
  
  <!-- AddFrame返回值 -->
  <rect x="640" y="220" width="100" height="50" fill="#FFEBEE" stroke="#F44336" stroke-width="2" rx="5"/>
  <text x="690" y="240" text-anchor="middle" font-size="11" font-weight="bold" fill="#C62828">AddFrame返回值</text>
  <text x="690" y="255" text-anchor="middle" font-size="9" fill="#333">实时更新数据</text>
  <text x="690" y="265" text-anchor="middle" font-size="9" fill="#333">增量信息</text>
  
  <!-- 客户端处理返回值 -->
  <rect x="50" y="300" width="700" height="40" fill="#F3E5F5" stroke="#9C27B0" stroke-width="1" rx="5"/>
  <text x="400" y="320" text-anchor="middle" font-size="12" font-weight="bold" fill="#7B1FA2">客户端处理AddFrame返回值</text>
  <text x="400" y="335" text-anchor="middle" font-size="10" fill="#333">解析增量数据，更新渲染</text>
  
  <!-- 循环箭头 -->
  <path d="M 130 270 L 130 300" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow4)"/>
  <path d="M 400 340 L 400 380" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow6)"/>
  
  <!-- 结束任务 -->
  <rect x="300" y="400" width="200" height="60" fill="#FFCDD2" stroke="#F44336" stroke-width="2" rx="5"/>
  <text x="400" y="425" text-anchor="middle" font-size="12" font-weight="bold" fill="#C62828">5. 结束任务</text>
  <text x="400" y="440" text-anchor="middle" font-size="10" fill="#333">EndTask接口</text>
  <text x="400" y="452" text-anchor="middle" font-size="10" fill="#333">后处理并保存</text>
  
  <!-- 从循环到结束的箭头 -->
  <path d="M 400 380 L 400 400" stroke="#666" stroke-width="2" fill="none" marker-end="url(#arrow5)"/>
  
  <!-- 更新说明 -->
  <rect x="50" y="480" width="700" height="15" fill="#E8F5E8" stroke="#4CAF50" stroke-width="1" rx="3"/>
  <text x="400" y="492" text-anchor="middle" font-size="10" fill="#2E7D32">成果文件：2D/dom_tiles/ (二维瓦片) | 3D/mesh (三维网格 PLY|3DTiles)</text>
  
  <!-- 箭头定义 -->
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

realtime_service是一个gRPC实时重建服务端程序，客户端可以通过该服务完成以下任务：

**（1）初始化服务，启动实时任务。**

**（2）逐帧传递图像路径，进行实时SLAM定位和二维地图|三维网格计算。**

**（3）执行后处理并自动结束任务。**

**（4）主动结束任务。**

以下是客户端使用该服务端程序的详细说明：

服务端监听端口号默认50051，也可通过启动参数自由配置端口号

### API 接口说明

服务定义了以下 gRPC 接口：

#### Initialize

功能：初始化服务端并启动实时任务，客户端需在调用其他接口之前调用此接口，传递任务配置。

请求格式 (InitRequest)
| 字段名                | 类型         |说明                                                         |
|----------------------|--------------|--------------------------------------------------------------|
| `task_json`                 | string          |  task json内容字符串                                                   |

响应格式 (InitResponse)
| 字段名                | 类型         |说明                                                         |
|----------------------|--------------|--------------------------------------------------------------|
| `state`                 | int32          |  初始化状态（0表示成功，其他值表示失败）码                                                   |

**task_json参数说明**：

| 字段名                | 类型         | 是否必需 | 说明                                                         |
|----------------------|--------------|----------|--------------------------------------------------------------|
| `license_id`                 | int          | 是       |  许可ID,必须设置为9200                                                  |
| `working_dir`                 | string          | 是       |  工作目录，保存中间文件及最终成果文件                                                  |
| `gdal_folder`               | string       | 是       | gdal目录路径，容器中/mipmap_engine/data目录                                                 |
| `do_realtime_2d_map`          | bool       | 否       | 默认false，是否计算实时二维地图，即二维正射地图，和`do_realtime_3d_map`必须至少设置其中一个为true                                         |
| `do_realtime_3d_map`          | bool       | 否       | 默认false，是否计算实时三维地图，即三维网格（暂windows可用），和`do_realtime_2d_map`必须至少设置其中一个为true                                          |
| `target_max_size`          | int       | 否       | 	默认0, 图像帧缩放后的最长边像素数，实时任务推荐将图像帧缩放，该值控制缩放后的图像大小，越大实时重建效果越好，单帧耗时越长。**为0时，调用者需在AddFrame前自行缩放图像并传入缩放后的图像metadata字符串（详情见下文）**，建议值：2048/1920/1280 |
| `tile_2d_save_type`          | int       | 否       | 默认0, 实时二维成果格式，0：webp  1：png                                         |
| `mesh_3d_save_type`          | int       | 否       | 默认0, 实时三维成果格式，0：ply  1：3dtiles                                         |
| `rt_mesh_reso_scale_to_gsd`          | int       | 否       | 默认6，实时三维生成的网格分辨率 = rt\_mesh\_reso\_scale\_to\_gsd\*图像gsd，  该值越小，网格分辨率越高，颜色越清晰，但效率也会相应降低                                        |
| `save_dom_tiff`          | bool       | 否       | 默认false，实时帧是否生成tif格式的DOM大图，生成大图会增加每帧耗时，大图固定存放在`working_dir/2D/geotiffs/dom.tif`                                        |
| `coordinate_descriptor_2D`          | object       | 否       | 默认null，tif格式DOM大图的坐标系，格式参考[坐标系](../basic/basic-config#coordinate-system)  

#### AddFrame

功能：逐帧传递图像路径进行实时处理。客户端通过该接口持续添加图片帧，服务端实时运算。

请求格式 (AddFrameRequest)

| 字段名                | 类型         |说明                                                         |
|----------------------|--------------|--------------------------------------------------------------|
| `image_path`                 | string          | 图像的文件路径，utf-8编码                                                   |
| `image_meta_data`               | string       | 图像的元数据metadata json内容字符串，**设置为空字符串时，实时服务将自动内部解析图像元数据（调用方需确保图像文件头的EXIF信息写入了相机35mm等效焦距值以及GPS位置信息）**                                                 |

**image_meta_data参数说明**：

| 字段名                | 类型         | 是否必需 | 说明                                                         |
|----------------------|--------------|----------|--------------------------------------------------------------|
| `width`              | int          | 是       | 图像宽度（像素）                                             |
| `height`             | int          | 是       | 图像高度（像素）                                             |
| `camera_id`          | int          | 否       | 对应相机ID                                                   |
| `focal_length_in_35mm`| double      | 否       | 35mm等效焦距（毫米），**pre_calib_param未设置时必需设置此值**
| `pos`                | array[3]     | 是       | 位置 [经度/X, 纬度/Y, 高程/Z]                                      |
| `pos_sigma`          | array[3]     | 否       | 位置精度（米），RTK:[0.03,0.03,0.06], 普通GPS: [2.0,2.0,5.0]                          |
| `orientation`        | array[9]     | 否       | 旋转矩阵（9个值，行优先，单位向量，若有外参可填写，世界坐标系到相机坐标系的旋转矩阵，笛卡尔坐标系）           |
| `position_constant`  | bool         | 否       | 是否固定位置（true/false）                                   |
| `relative_altitude`  | double       | 否       | 相对飞行高度（米）                                           |
| `pre_calib_param`    | array        | 否       | 预标定参数（如有，填写相机内参）                             |
| `dewarp_flag`        | bool         | 否       | 畸变校正标志（如来自DJI XMP数据，true表示已去畸变）           |

示例：
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
响应格式 (AddFrameResponse)
| 字段名                | 类型         |说明                                                         |
|----------------------|--------------|--------------------------------------------------------------|
| `update_json`                 | string          |  存储当前帧更新量的json字符串，含义见下                                                   |

update_json的字段含义：
| 字段名                | 类型         |说明                                                         |
|----------------------|--------------|--------------------------------------------------------------|
| `tile2d_folder`                 | string          |  二维正射快拼图的瓦片成果文件夹路径                                                   |
| `updated_2d_tiles`                 | array          |  当前帧更新的瓦片路径列表，相对路径，以`tile2d_folder`为根目录                                                   |
| `tile3d_folder`                 | string          |  三维网格成果的瓦片成果文件夹路径                                                   |
| `updated_3d_tiles`                 | array          |  当前帧更新的网格模型路径列表，相对路径，以`tile3d_folder`为根目录                                                   |

**二维实时成果**：
工作目录的`2D/dom_tiles`目录存储Web Mercator（EPSG:3857）坐标系的正射图瓦片格式，用第三方GIS库来读取瓦片显示。客户端可以根据返回的`update_2d_tiles`集合增量更新渲染瓦片数据，提高渲染效率。

**三维实时成果**：
工作目录的`3D/mesh`存储三维网格模型，三维模型存储为二进制格式（ply或3dtiles），且会根据地理范围分块，客户端可以根据返回的`update_3d_tiles`集合增量更新渲染块数据，提高渲染效率。

:::tip 稳定性说明
为了保证稳定性，前N帧（N≈10）一般不会出定位和网格成果，到定位达到稳定状态后，定位和成果开始更新到磁盘。
:::

#### EndTask

功能：结束当前实时任务。

请求格式 (EndTaskRequest)

| 字段名 | 类型 | 描述 |
|--------|------|------|
| 无字段 | 无 | 结束任务请求 |

响应格式 (EndTaskResponse)

| 字段名 | 类型 | 描述 |
|--------|------|------|
| `state` | int32 | 任务结束状态（0 表示成功，其他值表示失败） |
