# SuperSTRM

SuperSTRM 是一个网盘影视资源 STRM 扩展工具。

> 本仓库是 SuperSTRM 私有工程的**发布窗口**，主要用于发布使用文档、版本更新信息以及 Feature 规划。

## 📚 文档

- **[Docker 部署指南](docs/deployment.md)**：详细的 Docker 及 Docker Compose 部署说明。

## 🚀 快速开始 (Docker)

确保已安装 Docker，运行以下命令即可快速启动：

```bash
docker run -d \
  --name superstrm \
  --restart unless-stopped \
  -p 8080:8080 \
  -v $(pwd)/superstrm/config:/config \
  -v $(pwd)/superstrm/data:/data \
  -v $(pwd)/superstrm/logs:/logs \
  -e LOG_LEVEL=info \
  smarteam/superstrm:latest
```

更多配置选项（环境变量、Compose 部署、数据库迁移等）请查阅 [部署指南](docs/deployment.md)。

## 🗺️ Feature 规划

*暂无规划信息，敬请期待。*

## 📢 发布信息

*暂无发布信息。*
