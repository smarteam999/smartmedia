# SmartMedia

SmartMedia 是一个媒体工具套件，包含多个专注于提升家庭影音体验的应用组件。

> 本项目非开源，当前仓库作为 SmartMedia 套件的**官方发布窗口**，主要用于发布使用文档、版本更新信息以及 Feature 规划。
> 无收费计划，仅为个人兴趣与家庭使用开发。

## 🧩 组件应用

SmartMedia 目前包含以下两个核心应用：

### 1. [SmartSTRM](docs/superstrm/features.md)
网盘影视资源 STRM 扩展工具。
- **功能**：一站式完成家庭影库的搭建，支持多数据源接入 (Openlist, 123-API, 115open) 及批量生成 STRM。
- **文档**：
    - [功能特性](docs/superstrm/features.md)
    - [Docker 部署指南](docs/superstrm/deployment.md)

### 2. [SmartReverser](docs/smartreverser/features.md)
媒体服务器流量优化与直链播放工具。
- **功能**：实现 Strm 文件的 302 直链播放，流量不经过媒体服务器 (Emby/Jellyfin)。
- **特点**：
    - 推荐配合SmartSTRM使用。
    - 支持 HTTPStrm (直接 HTTP 链接) 及 AlistStrm (Alist 路径)。
    - 支持屏蔽特定客户端访问。
- **文档**：
    - [功能特性](docs/smartreverser/features.md)
    - [Docker 部署指南](docs/smartreverser/deployment.md)

## 📦 镜像版本说明

SmartMedia 的各个组件共享相同的版本发布策略，但可能独立发版。

| 标签 (Tag) | 说明 | 适用场景 |
| :--- | :--- | :--- |
| `latest` | **稳定版** (推荐)。基于最新的正式发布版本构建。 | 生产环境，追求稳定性的用户。 |
| `beta` | **预览版**。包含即将发布的特性，已经过初步测试但可能存在少量 Bug。 | 尝鲜新功能，愿意协助测试反馈的用户。 |
| `test` | **功能验证版**。包含最新的实验性功能或特定的 Bug 修复，用于快速验证。 | 需要验证特定问题修复或体验实验性功能的用户。 |
| `dev` | **开发版**。基于最新代码提交构建，更新频繁，可能极不稳定。 | 开发者，需要验证最新修复或特性的用户。 |

> **注意**: 生产环境建议固定使用具体的版本号 Tag (如 `v1.0.0`) 以避免非预期的自动升级。

## � 发布信息

SmartMedia 采用按需发布模式，发布版本可能仅包含其中一个或全部应用的更新。

- **[更新日志 (Changelog)](docs/changelog.md)**：查看版本更新历史及发布计划。

## 🤝 交流与反馈

- Bug 反馈：直接提交 Issue
- 讨论组：[SmartMedia TG Group](https://t.me/superstrm)
