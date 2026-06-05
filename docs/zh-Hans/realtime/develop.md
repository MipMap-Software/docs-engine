---
title: 开发指南
sidebar_position: 3
---

## 客户端开发指南

#### 前期准备

1. **安装 gRPC 与 Protocol Buffers**

   请确保已在开发环境中正确安装 gRPC 及 Protocol Buffers（protoc 编译器）。推荐参考官方文档完成安装，确保版本兼容。

2. **生成客户端代码**

   使用MipMap提供的 `.proto` 文件 [Proto文件下载](https://asset.mipmap3d.com/engine/realtime.proto)，通过 `protoc` 工具生成对应语言的客户端代码。具体命令及参数请参考 gRPC 官方文档或项目内说明。

3. **准备任务配置 JSON 文件**

   实时重建服务初始化时需提供任务配置 JSON 文件。请确保该文件内容完整、路径正确，且服务端具备访问权限。

:::tip 路径访问
请确保客户端配置文件中涉及的图像路径为服务端可直接访问的本地或网络路径，避免因路径不可达导致任务失败。
:::

#### 实时重建服务标准使用流程

1. **启动服务端（realtime_service）**

启动服务端程序，保持监听状态。可通过 `-port_number` 参数指定监听端口，例如：

```bash
realtime_service -port_number 50051
```

支持同时启动多个服务端实例，通过不同端口号区分。

2. **启动客户端并建立连接**

启动客户端程序，指定服务端端口号，建立 gRPC 通信连接。按照以下顺序调用接口：

- `Initialize`：初始化实时重建任务
- `AddFrame`：逐帧添加图像数据
- `EndTask`：结束当前实时任务

3. **任务管理与多次调用**

服务端进程可持续运行，支持多次任务。客户端每次调用 `EndTask` 结束任务后自动退出。若需发起新任务，可重新启动客户端并与服务端建立新连接，重复上述流程。

#### 客户端程序realtime_client

客户端程序realtime\_client提供给对grpc调用不熟悉且使用方式单一的用户（下面称为调用者），不需要编写客户端代码，启动realtime_client可执行程序和服务端程序即可。

步骤：

（1）初始化的json参数需要增加图像目录和实时计算的目标图像大小，请确保目录有效

| 字段名                | 类型         |说明                                                         |
|----------------------|--------------|--------------------------------------------------------------|
| `image_dir`                 | string          | 图像文件夹目录，utf-8编码                                                   |
| `target_max_size`          | int         | 图像帧缩放后的最长边像素数，实时任务推荐将图像帧缩放，该值控制缩放后的图像大小，值越大实时重建效果越好，单帧耗时越长。为0时，调用者在外部缩放并传入图像metadata字符串（详情见下文） |

（2）控制台命令启动realtime_service，启动的可选参数是端口号，不同的端口号代表不同的服务端口，不传参数则默认使用端口50051。

```bash
realtime_service -port_number 50051
```
（3）控制台命令启动realtime_client，启动的可选参数是端口号，不同的端口号代表不同的服务端口，不传参数则默认使用端口50051。

```bash
realtime_client -port_number 50051
```
client启动后，控制台要求输入参数json的路径来初始化服务，请调用者往控制台输入参数json文件的路径（不带双引号），输入后，服务端开始运行实时重建并打印日志

（4）调用者定时访问二维实时成果和三维实时成果文件，刷新渲染

（5）实时任务启动后客户端将等待调用者输入end指令，输入end后服务端会结束帧添加并结束本次实时任务。

### 妙算版开发指南
妙算版SDK有些特殊之处需要注意：

1 执行realtime_service服务端程序的task_type参数指定为：2D

```bash
./realtime_service --port_number 50051(可选) --task_type 2D
```

2 `license_id`字段必须为`9202`

3 AddFrame的image_meta_data参数必须指定有效值，意味着调用方需要自行对图像进行缩放，并获取缩放后的image_meta_data值。我们提供一个可执行程序extract_image_info完成这个工作，程序的使用方式如下：

```bash
./extract_image_info --src_image_path path_to_src_image --dst_image_path path_to_dst_image --info_json path_to_extracted_image_meta_data --max_side_width target_pixels_of_longest_side_of_dst_iamge
```

4 服务端初始化需要的JSON参数内，有些参数不会生效，包括`do_realtime_3d_map`、`target_max_size`、`mesh_3d_save_type`、`rt_mesh_reso_scale_to_gsd`、`save_dom_tiff`、`coordinate_descriptor_2D`