# SmartReverser 部署指南

SmartReverser 是一个独立的媒体服务器反向代理工具，用于接管媒体播放流量。

## 1. 目录结构

建议创建如下目录结构：

```bash
mkdir -p smartreverser/config smartreverser/logs
```

## 2. Docker Compose 部署

创建 `docker-compose.yml` 文件：

```yaml
version: '2.4'

services:
  smart-reverser:
    image: smarteam/smartreverser:latest
    container_name: smart-reverser
    ports:
      - 9000:9000
    environment:
      - REVERSER_DEBUG=1
      - TZ=Asia/Shanghai
    volumes:
      - ./config:/config
      - ./logs:/logs
    restart: unless-stopped
```

## 3. 配置文件 (config.yaml)

在 `./config` 目录下创建 `config.yaml`。

**重要**：请务必根据您的实际环境修改 `MediaServer` 和 `AlistStrm` 中的地址和认证信息。

```yaml
Port: 9000                                  # 监听端口

MediaServer:                                # 媒体服务器配置
  Type: Emby                                # 类型：Emby 或 Jellyfin
  ADDR: http://192.168.1.10:8096            # Emby/Jellyfin 地址
  AUTH: your_api_key_here                   # Emby/Jellyfin API Key

Logger:
  AccessLogger:
    Console: True
    File: False
  ServiceLogger:
    Console: True
    File: False

ClientFilter:                               # 客户端屏蔽配置
  Enable: False
  Mode: BlackList                           # BlackList (黑名单) 或 WhiteList (白名单)
  ClientList:
    - Fileball
    - Infuse

HTTPStrm:                                   # HTTPStrm 模式配置
  Enable: False
  TransCode: False                          # 是否允许转码 (False 为强制直连)
  FinalURL: True                            # 是否解析最终重定向地址
  PrefixList:                               # 匹配路径前缀
    - /data/symlink
  ReplaceMap:                               # URL 替换规则
    "http://nas.local:5244": "http://nas.public:5244"

AlistStrm:                                  # AlistStrm 模式配置
  Enable: True
  TransCode: False
  RawURL: False                             # True: 使用 Alist 上游直链; False: 使用 Alist 自身链接
  List:
    - ADDR: http://alist.local:5244         # Alist 地址
      Username: admin                       # Alist 用户名
      Password: password                    # Alist 密码
      PrefixList:                           # 匹配路径前缀
        - /data/symlink
  ReplaceMap:
    "http://nas.local:5244/d": ""
```

## 4. 启动服务

```bash
docker-compose up -d
```

## 5. 配置 Emby/Jellyfin

启动成功后，您需要在 Emby/Jellyfin 的网络设置中，将**局域网网络地址**或**外部域**修改为 SmartReverser 的地址（例如 `http://docker-host-ip:9000`），以便客户端流量经过 SmartReverser。
