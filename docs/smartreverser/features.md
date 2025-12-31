# SmartReverser 功能特性

SmartReverser 是 SmartMedia 套件的一部分，主要用于优化媒体播放流量，实现 STRM 文件的直链播放。

## 核心功能

- **Strm 文件 302 直链播放**
    - 流量不经过 EmbyServer/Jellyfin，直接由客户端请求存储源，减轻服务器带宽压力。
    - **推荐配合SuperSTRM使用**。
    - **兼容性**：已通过测试客户端（Web、iOS Emby、Infuse、Conflux、Fileball、Vidhub）。

## 支持的 Strm 类型

### 1. HTTPStrm
- **描述**：Strm 文件内容是标准的 HTTP 链接。
- **原理**：浏览器或客户端访问该链接可以直接下载或播放视频文件。
- **网络要求**：**客户端需要可以访问到该链接**，SmartReverser 本身不需要访问到该地址。

### 2. OpenlistStrm
- **描述**：Strm 文件内容是 Openlist 上的文件路径。
- **原理**：需要拼接 Openlist 的地址以访问到文件。
- **网络要求**：
    - **客户端无需访问到 Openlist 服务器**。
    - **仅需要 SmartReverser 可以访问到 Openlist 服务器**。
- **注意事项**：
    - 需要可以访问到 Openlist 服务器上文件的 `raw_url` 属性。
    - 如果使用网盘存储则通常无需在意这一点。
    - 目前兼容性相对较差且不支持转码，通过挂载真实目录可以缓解这一问题。

## 安全与访问控制

- **客户端屏蔽**：支持屏蔽特定客户端的访问请求。
