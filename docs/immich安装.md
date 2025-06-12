安装步骤

### 1.安装WSL

```powershell
wsl --install
```

在应用商店安装debian，打开虚拟化

用户文件夹下新建.wslconfig，打开网络映射

```toml
[wsl2]

networkingMode=mirrored
```

**修改软件源**

Debian Buster 以上版本默认支持 HTTPS 源。如果遇到无法拉取 HTTPS 源的情况，请先使用 HTTP 源并安装：

```bash
apt update 
apt install apt-transport-https ca-certificates
```

```bash
# /etc/apt/sources.list
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware

# 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
deb https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
# deb-src https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
```

```bash
apt-get install tsocks
apt-get install wget
```

### 2.安装

编辑`/etc/tsocks.conf`文件。找到`server =`这一行，将其修改为代理服务器的地址和端口，例如`server = 192.168.1.102 1080`（假设 SOCKS 代理服务器地址是`192.168.1.102`，端口是`1080`）

```
server = 192.168.31.151 7898
```

```bash
mkdir ./immich-app
cd ./immich-app
tsocks wget -O docker-compose.yml https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml
tsocks wget -O .env https://github.com/immich-app/immich/releases/latest/download/example.env
```

服务端docker-compose

```yaml
name: immich

services:
  immich-machine-learning:
    container_name: immich_machine_learning
    image: ghcr.io/immich-app/immich-machine-learning:${IMMICH_VERSION:-release}
    volumes:
      - model-cache:/cache
    ports:
      - 3003:3003
    env_file:
      - .env
    restart: always
    healthcheck:
      disable: false

volumes:
  model-cache:
```

安装下载大模型

到存储卷目录

```bash
apt-get install git-lfs
# https://huggingface.co/immich-app
# https://hf-mirror.com/ 镜像站
```

./clip 放大模型 

./Facial Recognition放人脸识别

设置服务端

http://192.168.31.151:3003

默认服务端

http://immich-machine-learning:3003







