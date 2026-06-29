---
title: 安装指南（Linux）
sidebar_position: 3
---

## 安装指南（Linux）

本指南详细介绍如何在 Linux 系统上部署和运行 MipMapEngine SDK，Linux系统仅提供Docker部署方式。

### 系统要求
- **操作系统**: Ubuntu（不支持WSL环境）
- **Docker版本**: 19.03 或更高版本（支持GPU）
- **GPU**: 支持CUDA的NVIDIA显卡，计算力5.0及以上（高斯泼溅重建需要7.0以上）
- **内存**: 建议32GB以上
- **存储**: 至少500GB可用空间

---

### 环境准备

#### Docker环境配置

Docker已安装可跳过此步骤，Ubuntu系统安装步骤如下：

```bash
# 更新包管理器
sudo apt update

# 安装Docker
sudo apt install -y docker.io

# 启动Docker服务
sudo systemctl start docker
sudo systemctl enable docker

# 安装NVIDIA Container Toolkit（Ubuntu, Debian）
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
  sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
  sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit

# 配置Docker使用NVIDIA运行时
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

#### 验证GPU支持
```bash
# 验证Docker GPU支持
# [中国大陆]
docker run --gpus all registry.mipmap3d.com/docker/nvidia/cuda:12.8.0-base-ubuntu22.04 nvidia-smi
# [其他]
docker run --gpus all nvidia/cuda:12.8.0-base-ubuntu22.04 nvidia-smi
```

---

### 许可证管理

:::tip **注意**
许可操作均在宿主机运行
:::

#### 安装许可证工具
```bash
# 安装SenseShield许可证客户端
# 在网站 https://lm.virbox.com/tools.html下载 Virbox 用户工具（Linux版）（.deb(64位)）
sudo dpkg -i senseshield-lcc-2.7.2.68616-amd64.deb
```

#### 激活许可证
```bash
# 方式一：使用ssclt激活许可证（请将JJKB-NBFS-KTM6-206T替换为您的实际激活码）
ssclt --online_bind_license_key --license_key JJKB-NBFS-KTM6-206T
```
```bash
# 方式二：使用license_engie可执行程序完成绑定、解绑和枚举操作
license_engine --bind xxxx-xxxx-xxxx-xxxx
license_engine --unbind xxxx-xxxx-xxxx-xxxx
license_engine --enum lics.json
```
执行license_engine返回值为0时表示操作成功，否则操作失败，失败错误码详见 [错误码](../basic/error-codes)

#### 解绑许可证（机器不再使用时）
```bash
# 解绑许可证
ssclt --online_unbind_license_key --license_key JJKB-NBFS-KTM6-206T
```
---

### 运行重建容器

#### 下载镜像

```bash
docker pull registry.mipmap3d.com/mipmap/runtime:v5.1.0.8-ubuntu22.04
```

#### 从镜像启动容器
```bash
# 启动 Docker 容器
docker run -it --rm \
  -v /tmp:/tmp \             # 挂载宿主机目录 /tmp 到容器[许可访问需要，必须执行且按此命令挂载]
  -v /mnt:/mnt \             # 挂载宿主机目录到容器[示例，按实际情况调整]
  --cpus 8 \                 # 限制容器使用 8 个 CPU
  --gpus all \               # 使用所有 GPU（需要 NVIDIA Container Toolkit）
  --name mipmap \            # 容器名称为 mipmap
  registry.mipmap3d.com/mipmap/runtime:v5.1.0.8-ubuntu22.04 \
  /bin/bash                  # 进入容器后启动 bash
```

**重要说明**：
- `/mnt` 目录映射：mnt是示例，实际调用者想挂载那个目录灵活执行
- `/tmp` 目录映射：许可访问需要，必须挂载
- `--cpus 8`：限制容器使用8个CPU核心
- `--gpus all`：启用所有GPU支持

#### 运行重建程序

```bash
# 在容器内运行重建引擎
./mipmap_engine/reconstruct_full_engine --reconstruct_type 0 --task_json /mnt/task.json
```

**参数说明**：
- `--reconstruct_type`：重建类型（0=默认类型）
- `--task_json`：任务配置文件路径


### 故障排除

#### 常见问题

**问题1**: Docker无法启动GPU支持
```bash
# 检查NVIDIA驱动
nvidia-smi

# 检查Docker GPU支持
# [中国大陆]
docker run --gpus all registry.mipmap3d.com/docker/nvidia/cuda:12.8.0-base-ubuntu22.04 nvidia-smi
# [其他]
docker run --gpus all nvidia/cuda:12.8.0-base-ubuntu22.04 nvidia-smi
```

### 文件结构说明

```
mipmap_engine/
├── gdal_data/                       # gdal_folder目录
├── reconstruct_full_engine     # 主处理程序
```