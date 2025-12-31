# SuperSTRM Docker 部署指南

本文档介绍如何使用 Docker 和 Docker Compose 部署 SuperSTRM 应用。

## 1. 快速开始 (Docker CLI)

```bash
# 创建必要的目录
mkdir -p superstrm/data superstrm/logs

# 启动容器
docker run -d \
  --name superstrm \
  --restart unless-stopped \
  -p 8080:8080 \
  -v $(pwd)/superstrm/data:/data \
  -v $(pwd)/superstrm/logs:/logs \
  -e LOG_LEVEL=info \
  -e TZ=Asia/Shanghai \
  -e PUID=1000 \
  -e PGID=1000 \
  -e UMASK=022 \
  smarteam/superstrm:latest \
  /superstrm-web serve --migrate
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
    volumes:
      - ./data:/data #数据目录
      - ./logs:/logs # 挂载日志目录，可选
      - /share/CACHEDEV1_DATA/datas/strm-media:/strm # 挂载 STRM 生成目录
    environment:
      - LOG_LEVEL=debug
      - TZ=Asia/Shanghai
      - PUID=1000
      - PGID=1000
      - UMASK=022
      - SERVER_PORT=8080 # 管理端口，可选
    ports:
      - "8166:8080"
    command: ["/superstrm-web", "serve", "--migrate"]
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
| `/data` | 持久化数据存储 (数据库文件等) |
| `/logs` | 运行日志 |

## 5. 参数与环境变量详解

### 通用环境变量

| 变量名 | 默认值 | 说明 |
| :--- | :--- | :--- |
| `TZ` | `Asia/Shanghai` | 容器运行时区设置 |
| `PUID` | `1000` | 运行进程的用户 ID |
| `PGID` | `1000` | 运行进程的组 ID |
| `UMASK` | `022` | 文件创建权限掩码 |

### 应用特定环境变量

以下是主要配置的环境变量映射。完整配置结构请参考代码。

| 变量名 | 默认值 | 说明 |
| :--- | :--- | :--- |
| **基础设置** | | |
| `DEBUG` | `false` | 是否开启调试模式 |
| `TZ` | `Asia/Shanghai` | 时区设置 |
| **服务器** | | |
| `SERVER_HOST` | `0.0.0.0` | 监听地址 |
| `SERVER_PORT` | `8080` | 监听端口 |
| **数据库** | | |
| `DATABASE_TYPE` | `sqlite` | 数据库类型 (sqlite, postgres, mysql) |
| `DATABASE_DSN` | `./data/superstrm.db` | 数据库连接字符串 |
| **存储** | | |
| `STORAGE_DATA_DIR` | `./data` | 数据目录 |
| `STORAGE_LOGS_DIR` | `./logs` | 日志目录 |
| `STORAGE_TEMP_DIR` | `./temp` | 临时目录 |
| **日志** | | |
| `LOG_LEVEL` | `info` | 日志级别 (debug, info, warn, error) |
| `LOG_FORMAT` | `json` | 日志格式 |
| **安全** | | |
| `SECURITY_SESSION_TIMEOUT` | `3600` | 会话超时 (秒) |
| `SECURITY_RATE_LIMIT_ENABLED`| `true` | 是否开启限流 |
| **缓存** | | |
| `CACHE_TYPE` | `local` | 缓存类型 (local, redis) |
| `REDIS_ADDR` | - | Redis 地址 |
| `REDIS_PASSWORD` | - | Redis 密码 |
| `REDIS_DB` | `0` | Redis DB 索引 |
| **GitHub** | | |
| `GITHUB_PROXY` | - | GitHub API 代理地址 (例如 `https://mirror.ghproxy.com/`,国内环境建议配置) |
| `GITHUB_TOKEN` | - | GitHub 访问令牌 (用于提高 API 速率限制) |
