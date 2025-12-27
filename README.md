# SuperSTRM

SuperSTRM 是一个网盘影视资源 STRM 扩展工具。

> 非开源项目，本仓库只作为**信息发布窗口**，主要用于发布使用文档、版本更新信息以及 Feature 规划。
> 无收费计划，希望可以简化大家使用网盘影视资源的流程。

## 📚 文档

- **[Docker 部署指南](docs/deployment.md)**：详细的 Docker 及 Docker Compose 部署说明。
- **[更新日志](docs/changelog.md)**：查看版本更新历史及发布计划。

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

- [X] 数据源Openlist接入
- [X] 数据源123API接入
- [X] 支持数据源浏览
- [X] 批量生成 STRM 文件
- [X] 定时任务自动生成 STRM 文件

## 📢 最近发布信息

### v1.0.0 (Planned)

- **预计发布时间**: 2025-12-31
- **版本类型**: Initial Release
- **主要内容**:
    - 核心功能上线：支持 Openlist、123-API 等多种数据源接入
    - 自动化能力：支持批量生成 STRM 及定时任务调度
    - 部署支持：提供标准 Docker 镜像及 Compose 编排文件
    - 用户体验：提供 Web 可视化管理界面
