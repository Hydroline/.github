# 👏 Coding, Creating and Exploring.

这里是 Minecraft 服务器[氢气工艺 HydCraft](https://hydcraft.cn) 的代码组织 **Hydroline**，主要开发者为[柠檬（AurLemon）](https://github.com/AurLemon)。

组织内的大部分项目都与氢气工艺的运行和长期建设密切相关，包括服务器基础设施、网站与账户系统、Minecraft 模组与插件、客户端更新和分发工具，以及地图、城市和公共交通数据服务。

氢气工艺是一个以创造模式建设为核心的 Minecraft 服务器。项目除了服务于服务器运维，也围绕城市管理与虚拟社会模拟。欢迎通过柠檬的 [GitHub 主页](https://github.com/AurLemon)或[他的个人网站](https://aurlemon.top)交流。

## 站点与平台

| 项目                                                              | 职责                                                                                                                                                                                                                                                                                 |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [hydcraft-portal](https://github.com/Hydroline/hydcraft-portal)   | 氢气工艺的统一门户与主要公共站点，基于 Nuxt 和 TypeScript 生态开发。项目内包含统一身份系统 Hydroline ID、Minecraft 账户绑定、服务器数据展示、OAuth 服务，以及通过 [Portal Bridge](https://github.com/Hydroline/portal-bridge) 实现的 Minecraft 服务端实时通信（基于 WebSocket 协议） |
| [hydcraft-console](https://github.com/Hydroline/hydcraft-console) | 面向服务器管理和玩家自助服务的控制台，负责客户端版本管理、更新文件分发、权限组申请、账户调整等功能，基于 Nuxt 开发                                                                                                                                                                   |
| `hydcraft-map`                                                    | 计划中的服务器整合地图平台。用于统一展示不同周目的地图，并融合 BlueMap、模组地图、公共交通和建筑数据，目标是提供接近现实地图应用的浏览体验                                                                                                                                           |
| `hydcraft-urban`                                                  | 计划中的服务器城市信息平台。用于展示架空组织、公司、城市、行政区划和公共交通数据，并整合 MTR、机械动力等模组产生的游戏内外信息                                                                                                                                                       |

## Minecraft 集成

| 项目                                                                    | 职责                                                                                                                |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| [portal-bridge](https://github.com/Hydroline/portal-bridge)             | 连接 Minecraft 服务端与 HydCraft Portal 的桥接项目，通过 WebSocket 上报在线玩家、服务器状态、访问记录和其他实时数据 |
| [cancel-block-update](https://github.com/Hydroline/cancel-block-update) | 用于控制特定方块更新行为的 Minecraft 模组，基于 CBU 二改                                                            |
| [create-track-map](https://github.com/Hydroline/create-track-map) | 基于 [AyOhEe/create-track-map](https://github.com/AyOhEe/create-track-map) 二次修改的机械动力铁路地图 |

## 边缘服务与基础设施

| 项目或服务                                                    | 职责                                                                                                             |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| [minecraft-skin](https://github.com/Hydroline/minecraft-skin) | 可自行部署的 Minecraft 皮肤 API。基于 NMSR 进行封装，并提供 Adapter 以兼容 `mc-heads.net` 的部分路由格式         |
| [oauth-proxy](https://github.com/Hydroline/oauth-proxy)       | 部署于 Cloudflare Workers 的 OAuth 中转服务，用于改善内地访问 Google OAuth 服务时的稳定性                |
| Cloudflare Turnstile                                          | 氢气工艺所有站点的人机验证                                                                  |
| EdgeOne 边缘函数                                              | 对 Assets 站点的图片资源进行边缘处理，并根据客户端支持情况自动转换为 AVIF 或 WebP 格式                           |
| 对象存储与 CDN                                                | 静态资源和客户端文件主要存储于 Cloudflare R2、腾讯云 COS 等对象存储服务，并通过 Cloudflare 或腾讯云 EdgeOne 分发 |

## 客户端分发

HydCraft 客户端分发系统由 Installer、Bootstrap、Updater、Workspace 和 Console 共同组成。

Minecraft 启动器启动游戏 JVM 时，会通过 `-javaagent` 参数加载 HydCraft Bootstrap。Bootstrap 负责控制 Updater 的启动和游戏 JVM 的继续执行；Updater 负责检查并调整客户端文件；HydCraft Console 则负责生成和下发 Bootstrap、Updater 所需的 Manifest。

客户端文件源按照计费成本分为公共源与限制源：

- 公共源主要基于 Cloudflare R2，用于分发普通客户端资源；
- 限制源主要基于腾讯云 COS 和 EdgeOne，为避免流量成本过高仅限部分服务器玩家使用。

| 项目                                                                                  | 职责                                                                                                                                              |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| [hydcraft-bootstrap](https://github.com/Hydroline/hydcraft-bootstrap)                 | 基于 Java 开发的客户端引导程序。通过 `-javaagent` 注入 Minecraft 游戏 JVM，负责启动 Updater、控制更新流程，并协调游戏进程继续运行                 |
| [hydcraft-updater](https://github.com/Hydroline/hydcraft-updater)                     | 基于 Tauri 开发的 HydCraft 客户端更新器。支持 Hydroline ID 登录、公共源与限制源下载、客户端更新、附属包安装和其他服务器定制功能，目前仍在积极维护 |
| [hydcraft-updater-workspace](https://github.com/Hydroline/hydcraft-updater-workspace) | 基于 Node.js 和 pnpm 的客户端发布工具。服务器管理员通过该项目整理客户端文件、生成 Manifest，并将构建产物发布至兼容 S3 的对象存储服务            |
| [hydcraft-installer](https://github.com/Hydroline/hydcraft-installer)                 | 面向首次安装用户的一次性客户端初始化程序，基于 C# 和 Windows 桌面框架开发，用于给新客户端部署 HydCraft Updater 及其基础运行环境                             |

## 开发与部署工作流

氢气工艺的大部分站点和静态资源已经接入 CDN。目前主要使用腾讯云 EdgeOne，并针对部分境外访问场景使用 Cloudflare。部分 Assets 站点采用对象存储与 CDN 回源结合的部署方式。

氢气工艺的所有项目代码托管于 GitHub。持续集成与部署流程通常由 GitHub Actions 和 CNB 共同完成：

```text
GitHub Push / Pull Request
          │
          ▼
    GitHub Actions
代码检查、测试与快速构建验证
          │
          ▼
         CNB
正式构建、产物分发与源站部署
```

GitHub Actions 主要负责 Pull Request 和代码提交后的快速验证。由于其公共 Runner 通常位于境外，在连接国内源站时可能受到网络延迟和丢包影响，因此正式部署流程由 CNB 完成。

## 项目状态

一般来说，能长期维护的基本只有 Portal 站。具体状态以各仓库的 README、Release 和 GitHub Repository 状态为准。

氢气工艺无法保证项目能完美运行在其他环境上，我们没有针对服务器以外的情况做兼容和测试。但我们也欢迎 PR 和 Issue，项目的推进需要长期维护和使用，若我们的项目刚好命中了你（您）的需求，欢迎和我们交流。

## 许可证

各项目的授权方式以对应仓库中的 `LICENSE` 文件和 README 说明为准。

部分项目采用标准开源许可证，部分与氢气工艺服务器运行、客户端分发或内部基础设施密切相关的项目可能采用 HydCraft 自定义许可协议。在使用、修改或重新分发代码前，请先阅读对应仓库的授权条款。
