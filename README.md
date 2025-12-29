# SuperSTRM

SuperSTRM 是一个网盘影视资源扩展工具。

- 一站式完成家庭影库的搭建,额外依赖NAS(如群晖、QNAP等)、Emby、应用播放器
- 本项目非开源，当前仓库只作为**信息发布窗口**，主要用于发布使用文档、版本更新信息以及 Feature 规划。 
- 无收费计划、无开源计划，小伙伴们一心为家人提供情绪价值😂
- Bug直接提Issue
- TG讨论组： [SuperSTRM](https://t.me/superstrm)

## 📚 文档(不及时)

- **[Docker 部署指南](docs/deployment.md)**：详细的 Docker 及 Docker Compose 部署说明。
- **[更新日志](docs/changelog.md)**：查看版本更新历史及发布计划。

## 📦 镜像版本说明

SuperSTRM 提供以下三种类型的 Docker 镜像标签 (Tag)，请根据您的需求选择：

| 标签 (Tag) | 说明 | 适用场景 |
| :--- | :--- | :--- |
| `latest` | **稳定版** (推荐)。基于最新的正式发布版本构建。 | 生产环境，追求稳定性的用户。 |
| `beta` | **测试版**。包含即将发布的特性，已经过初步测试但可能存在少量 Bug。 | 尝鲜新功能，愿意协助测试反馈的用户。 |
| `dev` | **开发版**。基于最新代码提交构建，更新频繁，可能极不稳定。 | 开发者，需要验证最新修复或特性的用户。 |

> **注意**: 生产环境建议固定使用具体的版本号 Tag (如 `v1.0.0`) 以避免非预期的自动升级。

## 🚀 快速开始 (Docker)

确保已安装 Docker，运行以下命令即可快速启动：

```bash
docker run -d \
  --name superstrm \
  --restart unless-stopped \
  -p 8080:8080 \
  -v $(pwd)/superstrm/data:/data \
  -v $(pwd)/superstrm/logs:/logs \
  -e LOG_LEVEL=info \
  smarteam/superstrm:latest
```

更多配置选项（环境变量、Compose 部署、数据库迁移等）请查阅 [部署指南](docs/deployment.md)。

## 🔧 正在开发 (In Development)

> 当前版本正在开发中的特性：

- [x] Openlist 数据源接入
- [x] 123API 数据源接入
- [x] 支持数据源内容浏览
- [x] 定时自动生成/更新 STRM 文件
- [x] 可视化配置管理界面

查看完整功能列表请访问 **[功能特性 (Features)](docs/features.md)**。

## 📢 最近发布信息

### v1.0.0 (Planned)

- **预计发布时间**: 2025-12-31
- **版本类型**: Initial Release
- **主要内容**:
    - 核心功能上线：支持 Openlist、123API 等多种数据源接入
    - 自动化能力：支持批量生成 STRM 及定时任务调度
    - 部署支持：提供标准 Docker 镜像及 Compose 编排文件
    - 用户体验：提供 Web 可视化管理界面
