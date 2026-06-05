---
title: 故障排除
sidebar_position: 1
---

## 故障排除

本页面提供了 MipMapEngine SDK 使用过程中常见问题的诊断和解决方案。

### 诊断工具

在遇到问题时，请首先使用以下诊断工具：

#### 硬件检查

```bash
hardware_check.exe
```

#### 许可证状态

Windows系统可以打开“virbox用户工具”查看激活码的绑定情况，或者通过命令行检查绑定的许可：

```bash
license_engine.exe --enum /path/to/license.json
```

Linux系统执行以下命令查看绑定的许可：

```bash
ssclt -l
```

### 常见错误及解决方案

### 配置文件错误

##### 错误：JSON 解析失败

**错误信息**：
```
[ERROR] JSON parse failed: expected ',' or '}' at line 45
```

**原因**：JSON 格式错误，通常是缺少逗号或多余的逗号。

**解决方案**：
1. 使用 JSON 格式化工具检查语法
2. 确保最后一个元素后没有逗号
3. 检查是否有未闭合的括号或引号

**示例修正**：
```json
// 错误
{
  "image_meta_data": [
    {"id": 1, "path": "image1.jpg"},  // 最后一个元素不应有逗号
  ]
}

// 正确
{
  "image_meta_data": [
    {"id": 1, "path": "image1.jpg"}
  ]
}
```

#### 错误：图像文件不存在

**错误信息**：
```
[ERROR] Image file not found: C:/images/DJI_0001.JPG
```

**原因**：指定的图像路径错误或文件不存在。

**解决方案**：
1. 检查文件路径拼写
2. 确认文件确实存在
3. 使用正确的路径分隔符（推荐使用 `/`）
4. 尝试使用绝对路径

**路径示例**：
```json
// Windows - 推荐
"path": "C:/Projects/Images/DJI_0001.JPG"

// Windows - 也可以
"path": "C:\\Projects\\Images\\DJI_0001.JPG"

// 相对路径
"path": "../images/DJI_0001.JPG"
```

#### 错误：参数超出范围

**错误信息**：
```
[ERROR] Invalid resolution_level: 5 (should be 1-3)
```

**原因**：参数值不在允许范围内。

**解决方案**：
检查并修正参数值：
- `resolution_level`: 1（高）、2（中）、3（低）
- `input_image_type`: 1（RGB）、2（多光谱）、3（红外）
- `coordinate_system.type`: 1（投影）、2（地理）、3（地心）

### 图像处理错误

#### 错误：图像数量不足

**错误信息**：
```
[ERROR] Insufficient images for reconstruction. Minimum 5 images required, found 3.
```

**原因**：图像数量太少，无法进行三维重建。

**解决方案**：
- 至少提供 5 张图像
- 确保图像之间有足够的重叠（建议 >60%）
- 检查是否有图像被自动过滤（查看日志）

#### 错误：无 GPS 信息

**错误信息**：
```
[WARNING] No GPS information found in image EXIF
```

**原因**：图像 EXIF 中没有 GPS 信息。

**解决方案**：

1. **使用自定义 POS 数据**：
```json
{
  "id": 1,
  "path": "image.jpg",
  "meta_data": {
    "pos": [114.123, 22.456, 100.5],
    "pos_sigma": [0.5, 0.5, 1.0]
  }
}
```

2. **降低位置约束**：
```json
{
  "use_image_position_constraint": false
}
```

### 多相机重建问题

#### 错误：相机组图像数量不匹配

**错误信息**：
```
[ERROR] Camera group image count mismatch: nadir(50), oblique_f(48)
```

**原因**：不同相机组的图像数量不一致。

**解决方案**：
1. 检查是否有图像丢失
2. 确保多相机同步触发正常

**正确的分组示例**：
```json
{
  "image_meta_data": [
    // 每个位置都应该有所有相机的图像
    {"id": 1, "path": "pos1/nadir.jpg", "group": "nadir"},
    {"id": 2, "path": "pos1/forward.jpg", "group": "forward"},
    {"id": 3, "path": "pos1/backward.jpg", "group": "backward"},
    // ... 下一个位置
    {"id": 4, "path": "pos2/nadir.jpg", "group": "nadir"},
    {"id": 5, "path": "pos2/forward.jpg", "group": "forward"},
    {"id": 6, "path": "pos2/backward.jpg", "group": "backward"}
  ]
}
```

### 内存和性能问题

#### 错误：内存不足

**错误信息**：
```
[ERROR] Out of memory. Required: 24GB, Available: 16GB
```

**原因**：系统内存不足以处理当前任务。

**解决方案**：

1. **降低精度等级**：
```json
{
  "resolution_level": 3  // 使用低精度
}
```

2. **限制内存使用**：
```json
{
  "min_avali_memory_size": 12.0  // 限制为 12GB
}
```

3. **减少输出格式**：
```json
{
  "generate_obj": true,
  "generate_osgb": false,  // 关闭不必要的输出
  "generate_3d_tiles": false
}
```

#### 错误：GPU 内存不足

**错误信息**：
```
[ERROR] CUDA out of memory. Tried to allocate 2.00 GiB
```

**原因**：GPU 显存不足。

**解决方案**：
1. 关闭其他占用 GPU 的程序
2. 降低 `resolution_level`
3. 使用分块处理（大型项目）

### 许可证问题

#### 错误：许可证无效

**错误信息**：
```
[ERROR] Invalid license: License has expired
```

**解决方案**：
1. 检查许可证状态：`license_engine.exe --enum ./license.json`
2. 确认系统时间正确
3. 重新激活许可证
4. 联系技术支持更新许可证

#### 错误：Virbox 服务未运行

**错误信息**：
```
[ERROR] Virbox service not running
```

**解决方案**：

**Windows**：
```powershell
# 启动服务
net start "Virbox Service"

# 检查服务状态
sc query "Virbox Service"
```

**Linux**：
```bash
# 加载驱动
sudo modprobe virbox

# 检查驱动
lsmod | grep virbox
```

### 输出结果问题

#### 问题：模型有空洞

**可能原因**：
- 图像重叠度不足
- 纹理贫乏区域（水面、玻璃等）
- 图像质量差

**解决方案**：
1. 增加图像重叠度到 80%
2. 使用倾斜摄影补充侧面
3. 避免在强光或阴影条件下拍摄
4. 检查并剔除模糊图像

#### 问题：纹理模糊

**可能原因**：
- 原始图像模糊
- 飞行高度过高
- 相机设置不当

**解决方案**：
1. 使用更快的快门速度（≥1/1000s）
2. 降低飞行高度
3. 使用更高分辨率的相机
4. 确保相机对焦正确

#### 问题：坐标偏移

**可能原因**：
- 坐标系设置错误
- GPS 精度差

**解决方案**：

1. **检查坐标系配置**：
```json
{
  "coordinate_system": {
    "type": 2,
    "label": "WGS 84",
    "epsg_code": 4326
  }
}
```

### 高级诊断

#### 查看详细日志

日志文件位置：`working_dir/logs/log.txt`

查找关键信息：
```bash
# 查看错误
grep -i "error" log.txt

# 查看警告
grep -i "warning" log.txt

# 查看进度
grep "PROGRESS" log.txt
```

#### 性能分析

在日志中查找性能指标：
- `[TIMING]` - 各阶段耗时
- `[MEMORY]` - 内存使用情况
- `[GPU]` - GPU 使用率

#### 中间结果检查

检查中间结果以定位问题：
1. **空三结果**：`working_dir/AT/mvs.xml`
2. **ROI 范围**：`working_dir/milestones/roi.json`
3. **质量报告**：`working_dir/report/report.json`

### 获取帮助

如果以上方案无法解决您的问题：

1. **收集诊断信息**：
   - 完整的 `log.txt` 文件
   - 配置文件（脱敏处理）
   - 系统信息（硬件配置）

2. **提交问题**：
   - GitHub Issues：[项目地址]
   - 技术支持邮箱：support@mipmap.com
   - 包含上述诊断信息

3. **查看更新**：
   - 检查是否有新版本
   - 查看版本更新日志
   - 关注已知问题列表

---

*持续更新中。如遇到未列出的问题，请联系技术支持。*