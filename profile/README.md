# 👏 Coding, Creating and Exploring.

这里是 Minecraft 服务器[氢气工艺](https://hydcraft.cn)的代码仓库 Hydroline。主开发者为[柠檬（AurLemon）](https://github.com/AurLemon)。

组织内的大部分代码都是与服务器运维、运营紧密相连的项目，如服务器的基础设施网站、模组、客户端分发等项目。氢气工艺是一个以建设为主的创造模式服务器，大部分项目的目的主要围绕仿真和建筑。

欢迎交流，可来柠檬的 [GitHub 主页](https://github.com/AurLemon)或是[个人站](https://aurlemon.top)积极讨论。氢气工艺的所有项目介绍介绍请参见下方。

## 站点

| 项目 | 职责 |
| --- | --- |
| [hydcraft-portal](https://github.com/Hydroline/hydcraft-portal) | 服务器的门户站，基于 TypeScript 生态的全栈框架 Nuxt 开发，是目前项目体量较大的一个项目。内建了服务器统一账户系统 Hydroline ID、Minecraft 服务端实时连接（[Portal Bridge](https://github.com/Hydroline/portal-bridge)，基于 WebSocket 的通信）以及 OAuth 等服务 |
| [hydcraft-console](https://github.com/Hydroline/hydcraft-console) | 客户端更新与分发、玩家调整服务器账户，主要由管理员使用，基于 Nuxt 开发 |
| hydcraft-map | 计划中。服务器的整合地图站点，融合 Mod 地图和 Bluemap WebApp，计划上是实现和现实的地图 App 无异的产品 |
| hydcraft-urban | 计划中。服务器的信息汇总站，架空组织和公司申请、城市信息介绍与行政区划内容等，融合公共交通类模组（如 MTR、机械动力）的数据和游戏内外数据综合呈现信息 |

## 边缘服务

| 项目 | 职责 |
| --- | --- |
| [miencraft-skin](https://github.com/Hydroline/minecraft-skin) | 可自部署的 Minecraft 皮肤 API，基于 NMSR 封装了 Adater 用于兼容 mc-heads.net 路由 |
| [oauth-proxy](https://github.com/Hydroline/oauth-proxy) | 使用 Cloudflare Worker 作为跳板的 Google OAuth 服务中转站 |
| Cloudflare Turnstile | 使用 Cloudflare 的 Turnstile 作为验证服务 |
| EdgeOne 边缘函数 | 对 Assets 站的图片内容进行压缩，根据用户的设备自动转换 avif 或 webp |

## 客户端分发

服务器的客户端更新器通过在游戏启动器内注入 JVM 启动参数，通过 `javaagent` 参数调用服务器客户端更新器以实现文件修改和各类自定义功能。

Bootstrap 负责调起 Updater、控制游戏 JVM 启动时机；Updater 负责客户端文件调整。HydCraft Console 会给 Bootstrap 和 Updater 提供 Manifest。

客户端下载源根据费用开支分为公共源和限制源。前者基于 Cloudflare R2 实现，后者基于腾讯云 COS 桶 + EO 回源实现。

| 项目 | 职责 |
| --- | --- |
| [hydcraft-bootstrap](https://github.com/Hydroline/hydcraft-bootstrap) | 在 Minecraft 启动器启动游戏进程 JVM 后以 `javaagent` 参数方式注入客户端实现 Updater 调用的程序，基于 Java 开发 |
| [hydcraft-updater](https://github.com/Hydroline/hydcraft-updater) | 基于 Tauri 的客户端更新器，基于氢气工艺服务器自身需求调整，支持 Hydroline ID 登录和限制源下载、以及客户端、附属包下载，目前还在积极维护中 |
| [hydcraft-updater-workspace](https://github.com/Hydroline/hydcraft-updater-workspace) | 基于 Node + pnpm 维护的客户端管理程序，服务器管理员可使用此将打包后的客户端发布至 S3 桶 |
| [hydcraft-installer](https://github.com/Hydroline/hydcraft-installer) | 为没有安装 HydCraft Updater 的新客户端初始化的一次性程序，基于 C# Windows 框架开发 |

## 工作流

氢气工艺的大部分项目都已经接入了 CDN，目前采用的服务商主要为腾讯云 EdgeOne，部分站点针对境外配置了 Cloudflare。还有一些 Assets 站使用了对象存储桶 + CDN 回源的组合部署。CI/CD 使用了第三方平台，大部分项目为 GitHub Actions 搭配 CNB。

氢气工艺的代码主要存放在 GitHub，鉴于此，GitHub Actions 一般用于快速 Build 验证。但 GitHub Actions 的 Runner 都位于境外，网络延迟和丢包率较高。借助 CNB 的优势，我们使用 CNB 作为落地部署的最后一环，CNB 负责构建和部署到源站。

## 协议

参照各仓库内的开源协议，一般为氢气工艺的私有协议。
