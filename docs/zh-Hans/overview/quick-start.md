---
title: 快速开始
sidebar_position: 4
---

## 快速开始

本指南将帮助您快速上手 MipMapEngine SDK。

### 前置准备

在开始之前，请确保您已经：

1. ✅ 安装了 SDK（参见 [安装指南Windows](./installation) 或 [安装指南Linux](./linux-deployment)）
2. ✅ 激活了许可

### 快速启动重建
我们提供了一个非常直观的交互式的页面，用于生成启动重建所需的JSON参数文件，您只需要将图像拖入页面，设置您的工作目录、SDK的GDAL目录、以及简单的重建参数，选择需要的成果格式，即可生成JSON文件。

》[点击打开交互式页面](https://mipmap3d.com/tasks_generator/#/)

接下来，将JSON文件里的图像根目录统一修改为您的实际目录，即可通过命令行启动全流程重建：

```bash
reconstruct_full_engine(.exe) --reconstruct_type 0 --task_json config_task.json
```

:::tip 提示
Linux Docker部署模式，reconstruct_full_engine可执行程序存放在容器的mipmap_engine目录下。
:::

 参数说明

- `reconstruct_type`: `0`表示一键全流程重建
- `task_json`: JSON参数文件路径

[**重要**] 您启动的是SDK一键全流程重建接口，也是最常用最推荐的接口，推荐你查看页面 [Reconstruct Full手册](../basic/reconstruct-full) 查看详细的参数设置规则。


*需要帮助？查看故障排除指南或联系技术支持。*