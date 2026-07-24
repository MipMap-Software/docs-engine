---
title: 安装
sidebar_position: 1
---

## 安装
实时重建的SDK与后处理SDK是两套独立的SDK，实时重建需要单独安装，支持windows系统和Linux系统，安装环境与后处理SDK相同。

### 许可驱动安装并激活许可证
#### windows
（1）下载[virbox用户中心](https://lm.virbox.com/tools.html)

（2）使用license_engie可执行程序完成绑定、解绑和枚举操作
```bash
license_engine --bind xxxx-xxxx-xxxx-xxxx
license_engine --unbind xxxx-xxxx-xxxx-xxxx
license_engine --enum lics.json
```
执行license_engine返回值为0时表示操作成功，否则操作失败，失败错误码详见 [错误码](../basic/error-codes)
#### linux
:::tip **注意**
许可操作均在宿主机运行
:::
##### 安装许可证工具
```bash
# 安装SenseShield许可证客户端
# 在网站 https://lm.virbox.com/tools.html下载 Virbox 用户工具（Linux版）（.deb(64位)）
sudo dpkg -i senseshield-lcc-2.7.2.68616-amd64.deb
```
##### 激活许可证
```bash
# 使用ssclt激活许可证（请将JJKB-NBFS-KTM6-206T替换为您的实际激活码）
ssclt --online_bind_license_key --license_key JJKB-NBFS-KTM6-206T
```

### Windows系统下SDK安装
Windows系统下面提供本地开发包，请找MipMap索要本地开发包。

### Linux系统下SDK安装
Linux系统下只支持Docker环境部署，部署前的环境准备工作请参考：[Linux部署环境准备](../overview/linux-deployment#环境准备) 。

#### 下载镜像
```bash
docker pull registry.mipmap3d.com/docker/mipmap/realtime:v1.0-ubuntu22.04
```
#### 从镜像启动容器
```bash
# 启动 Docker 容器
docker run -it --rm \
  -v /tmp:/tmp \             # 挂载宿主机目录 /tmp 到容器[许可访问需要，必须执行且按此命令挂载]
  -v /mnt:/mnt \             # 挂载宿主机目录到容器[示例，按实际情况调整]
  --cpus 8 \                 # 限制容器使用 8 个 CPU
  --gpus all \               # 使用所有 GPU（需要 NVIDIA Container Toolkit）
   --name mipmap_rt \            # 容器名称为 mipmap_rt
  registry.mipmap3d.com/docker/mipmap/realtime:v1.0-ubuntu22.04 \
  /bin/bash                  # 进入容器后启动 bash
```

**重要说明**：
- `/mnt` 目录映射：mnt是示例，实际调用者想挂载那个目录灵活执行
- `/tmp` 目录映射：许可访问需要，必须挂载
- `--cpus 8`：限制容器使用8个CPU核心
- `--gpus all`：启用所有GPU支持

### 运行实时重建服务
通过在命令行启动`realtime_service`来启动实时重建服务，启动参数解释如下：
| 字段名                | 类型         | 是否必需 | 说明                                                         |
|----------------------|--------------|----------|--------------------------------------------------------------|
| `port_number`                 | number          | 否       |  服务端端口号，默认50051                                                  |
| `task_type`               | string       | 否       | 重建类型，2D-通过GPS和云台角拼接，只执行二维实时快拼；3D-通过SLAM拼接，支持二维实时快拼和三维实时网格，**默认3D**                                                 |
```bash
# 在容器内运行实时重建服务
./mipmap_engine/realtime_service -port_number 50051(可选) -task_type 3D(可选)
```

---