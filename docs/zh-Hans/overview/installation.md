---
title: 安装指南（Windows）
sidebar_position: 2
---

## 安装指南（Windows）

本指南将帮助您在 Windows 系统上安装和配置 MipMapEngine。

### 系统要求

#### 硬件要求

| 组件 | 最低要求 | 推荐配置 |
|------|----------|----------|
| **CPU** | Intel i5 或同等 | Intel i7/i9 或 AMD Ryzen 7/9 |
| **内存** | 16 GB | 32 GB 或更高 |
| **显卡** | NVIDIA GTX 1050 Ti | NVIDIA RTX 3070 或更高（高斯泼溅需要计算力7.0以上显卡） |
| **存储** | 500 GB 可用空间 | 2 TB SSD |

#### 支持的显卡

:::info MipMapEngine 目前仅支持英伟达显卡 
- **显卡架构要求：**: Compute Capability ≥ 5.0（高斯泼溅 ≥ 7.0）
- **显存要求**: 显卡VRAM ≥ 4GB

:::

#### 驱动版本要求

- **Windows**: NVIDIA 驱动 ≥ 528.33
- **Linux**: NVIDIA 驱动 ≥ 525.60.33

### Windows 安装

#### 步骤 1：检查系统环境

```bash
# 检查 NVIDIA 型号和版本
nvidia-smi
```

#### 步骤 2：安装 Virbox 驱动 

1. 下载 Virbox 用户工具：[https://lm.virbox.com/tools.html](https://lm.virbox.com/tools.html)
2. 运行安装程序，按照向导完成安装

*注意：Virbox 是 SDK 的加密与授权工具，必须安装后才能够使用。*

#### 步骤 3：安装 SDK

在页面 [MipMap Engine SDK下载页](https://www.mipmap3d.com/download/engine) 下载最新版本的SDK包

**解压 SDK 包**，目录结构如下：

📁 目录结构

安装完成后，SDK 目录结构如下：

```
├── bin/                             
│   ├── reconstruct_full_engine.exe  # 重建程序，所有重建步骤都执行此程序
│   ├── license_engine.exe           # 许可证管理，激活、绑定、枚举许可
├── gdal_data/                       # GDAL 目录
```

#### 步骤 4：激活许可证

方式一：在`virbox用户中心`软件在线绑定许可码完成激活

方式二：使用license_engie可执行程序完成绑定、解绑和枚举操作

```bash
license_engine --bind xxxx-xxxx-xxxx-xxxx
license_engine --unbind xxxx-xxxx-xxxx-xxxx
license_engine --enum lics.json
```
执行license_engine返回值为0时表示操作成功，否则操作失败，失败错误码详见 [错误码](../basic/error-codes)

### 安装检查清单

- NVIDIA 显卡驱动已更新到要求版本
- SDK 文件已正确解压
- 许可证已成功激活

### 下一步

安装完成！现在您可以：

- 📚 阅读 [快速开始](./quick-start) 快速启动您的第一个重建任务
- 📚 阅读 [全流程重建](../basic/reconstruct-full) 开始了解API接口使用方法

---

*提示：安装过程中遇到问题？查看[故障排除](../assistant/troubleshooting)或联系技术支持。*