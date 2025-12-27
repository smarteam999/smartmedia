# SuperSTRM Docker 部署指南

本文档介绍如何使用 Docker 和 Docker Compose 部署 SuperSTRM 应用。

## 1. 快速开始 (Docker CLI)

```bash
# 创建必要的目录
mkdir -p superstrm/config superstrm/data superstrm/logs

# 启动容器
docker run -d \
  --name superstrm \
  --restart unless-stopped \
  -p 8080:8080 \
  -v $(pwd)/superstrm/config:/config \
  -v $(pwd)/superstrm/data:/data \
  -v $(pwd)/superstrm/logs:/logs \
  -e LOG_LEVEL=info \
  -e TZ=Asia/Shanghai \
  -e PUID=1000 \
  -e PGID=1000 \
  -e UMASK=022 \
  smarteam/superstrm:latest
```

## 2. 使用 Docker Compose 部署 (推荐)

创建 `docker-compose.yml` 文件：

```yaml
version: '3.8'

services:
  superstrm:
    image: smarteam/superstrm:latest
    container_name: superstrm
    restart: unless-stopped
    ports:
      - "8080:8080"
    volumes:
      - ./config:/config
      - ./data:/data
      - ./logs:/logs
    environment:
      - LOG_LEVEL=info
      - TZ=Asia/Shanghai
      - PUID=1000
      - PGID=1000
      - UMASK=022
      # 如果需要连接 Redis，取消注释以下行
      # - REDIS_ADDR=redis:6379
    # 默认命令启动 web 服务，如果需要自动迁移，请参考下一节
    # command: ["/superstrm-web", "serve", "--migrate"]

  # (可选) Redis 服务
  # redis:
  #   image: redis:7-alpine
  #   container_name: smart-redis
  #   restart: unless-stopped
  #   volumes:
  #     - redis-data:/data

# volumes:
#   redis-data:
```

启动服务：

```bash
docker-compose up -d
```

## 3. 数据库迁移 (Migrate)

当您更新 SuperSTRM 版本或初次安装时，可能需要执行数据库迁移以更新表结构。

### 方法一：启动时自动迁移 (推荐)

在 `docker-compose.yml` 中修改 `command`：

```yaml
services:
  superstrm:
    # ...
    # 注意：必须指定二进制文件的完整路径
    command: ["/superstrm-web", "serve", "--migrate"]
```

### 方法二：手动执行一次性迁移

```bash
# 执行 migrate 子命令
docker-compose run --rm superstrm /superstrm-web migrate

# 强制重建表结构 (警告：会丢失数据)
# docker-compose run --rm superstrm /superstrm-web migrate --force
```

## 4. 目录挂载说明

| 容器内路径 | 说明 |
| :--- | :--- |
| `/config` | 配置文件存放目录 (config.yaml) |
| `/data` | 持久化数据存储 (数据库文件等) |
| `/logs` | 运行日志 |

建议将配置文件挂载到宿主机以便持久化保存和修改。
参考配置文件示例: [config.yaml.example](https://github.com/smarteam999/smart-media/blob/main/superstrm/example/config.yaml.example)

## 5. 参数与环境变量详解

### 通用环境变量

| 变量名 | 默认值 | 说明 |
| :--- | :--- | :--- |
| `TZ` | `Asia/Shanghai` | 容器运行时区设置 |
| `PUID` | `1000` | 运行进程的用户 ID |
| `PGID` | `1000` | 运行进程的组 ID |
| `UMASK` | `022` | 文件创建权限掩码 |

### 应用特定环境变量

| 变量名 | 默认值 | 对应配置项 | 说明 |
| :--- | :--- | :--- | :--- |
| `LOG_LEVEL` | `info` | `Logging.Level` | 日志级别 (debug, info, warn, error) |
| `WEB_PORT` | `8080` | `Server.Port` | Web 服务监听端口 |
| `DB_DSN` | `./data/superstrm.db` | `Database.DSN` | 数据库连接字符串或文件路径 |
| `REDIS_ADDR` | - | `Cache.Redis.Addr` | Redis 服务器地址 (host:port) |
| `REDIS_PASSWORD`| - | `Cache.Redis.Password`| Redis 密码 |
| `JWT_SECRET` | - | `Security.JWTSecret` | JWT 签名密钥 (建议生产环境修改) |
