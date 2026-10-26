<p align="center">
  <img src="project/native/windows/ui/src/assets/BrandMark.png" width="112" alt="ZCode Antigravity 黄色袋鼠与四芒星图标">
</p>

<h1 align="center">ZCode Antigravity</h1>

<p align="center">
  <strong>把 Gemini、Google 账号内的 Claude 与可选 Grok 接入 ZCode 和更多 Agent / CLI。</strong>
</p>

<p align="center">
  <img alt="Stable" src="https://img.shields.io/badge/stable-V1.2.0-2f6bff">
  <img alt="Windows" src="https://img.shields.io/badge/Windows-10%20%7C%2011-0078d4?logo=windows11">
  <img alt="Local only" src="https://img.shields.io/badge/API-loopback%20only-20a464">
</p>

<p align="center">
  <a href="https://github.com/YShiLMu/ZCode-Antigravity/releases/tag/v1.2.0"><strong>下载 V1.2.0 正式版</strong></a>
  · <a href="#快速开始">快速开始</a>
  · <a href="#一键接入-agent--cli">Agent 接入</a>
  · <a href="#隐私与安全">安全边界</a>
</p>

> **本仓库是 [Hhz0823/ZCode-Antigravity](https://github.com/Hhz0823/ZCode-Antigravity) 的社区 fork**，在其 V1.1.0 基础上新增 Gemini 4 Argon 支持并修复了新版 ZCode 的接入兼容性，感谢原作者。

ZCode Antigravity 是面向 Windows 与 macOS 的本地 AI 控制中心。它负责账号授权、模型路由、
ZCode / Agent 配置、额度监控与网络出口选择；默认只启用 Gemini，需要时可在设置中开启
Google Antigravity 账号内的 Claude、Grok 和其他文本模型。

> 当前源码与最新正式版均为 `V1.2.0`：在保留 Gemini 3.8 / 3.7 默认配置的基础上，账号可用时自动接入 **Gemini 4 Argon**（2026-10-13 正式发布的最新旗舰模型），并完整支持只用 `provider_config.json` 的新版 ZCode。

## 核心功能

| 功能 | 能做什么 |
| --- | --- |
| **Gemini 默认，Claude / Grok 可选** | 默认只向客户端暴露 Gemini；Google Claude、Grok / xAI 与其他文本模型按需开启。 |
| **384K + 三级思考配置** | 同步新版与旧版 ZCode 配置；完整支持 Low / Medium / High 三级思考调节，默认 High。 |
| **多模态输入** | Gemini Flash 支持文本、图片、音频和视频；Google Claude 支持文本与图片输入，当前均为文本输出。 |
| **一键接入更多 Agent** | 自动备份并合并 ZCode、DeepSeek Harness、Grok Build、Codex、Claude Code、Gemini CLI、Qwen Code、Kimi Code 与 OpenCode 配置。 |
| **无需强制开启 TUN** | Windows 自动使用系统代理或运行中的 v2rayN mixed / HTTP 代理，均不可用时才直连；手动代理始终优先。 |
| **额度与 Token 小组件** | macOS 菜单栏和 Windows 系统托盘直接查看 5 小时 / 本周额度、重置时间、最近输出 Token、推理 Token 与 Token/s。 |
| **自动检测与安全更新** | 启动后及每 6 小时检查 GitHub 正式版；可手动更新或开启自动安装，下载后核对平台、大小与 GitHub SHA-256。 |
| **本地安全边界** | API、OAuth 回调和管理接口只监听 `127.0.0.1`；配置写入前自动备份，凭据使用系统安全能力加密。 |
| **双平台精美界面** | macOS 使用 SwiftUI + AppKit 原生界面，Windows 使用简洁浅色控制中心，支持一键打开 Antigravity 和账号验证指引。 |

## 模型与联网能力

默认的 Antigravity 模型：

- `gemini-4-argon`：Gemini 4 Argon，2026 年 10 月 13 日发布的最新旗舰推理模型；仅当当前
  登录账号的模型目录中实际提供时自动注入（需要相应订阅权限），并作为默认首选模型。
- `gemini-3.8-flash`：所有账号默认可用的稳定模型，支持多模态输入。
- `gemini-3.7-flash`：通用对话、编程与多模态理解。

账号没有 Gemini 4 Argon 权限时，同步不会失败，自动回退到 3.8 / 3.7 默认组合。
这些模型默认上下文为 **384K（393,216 tokens）**，并在 ZCode 中提供 **Low / Medium / High** 三级思考深度选项，
默认选择 High 思考。无论是新版还是旧版 ZCode 均可自由切换三级思考，重新同步时会自动保持已有选择。
同步会移除本程序管理的 Gemini 3.6 及更早模型，包括基于 3.1 的旧 `gemini-web-search` 别名。
普通模型不会因为提示词写了“搜索”就自动联网；需要网页信息时请使用客户端的搜索工具。
Google Claude、Grok 和其他 AI 文本模型默认关闭，可在“设置 → 模型”中开启并重新同步。
图片 / 视频生成模型不会混入文本模型列表。

开启“Google Claude / 其他模型”后，当前已验证的 Google Antigravity 模型为
`claude-sonnet-4-6` 和 `claude-opus-4-6-thinking`。它们复用已登录的 Google 账号，
不需要另外填写 Anthropic API Key；模型权限和额度以当前 Google 账号实时返回为准。

## 一键接入 Agent / CLI

| 客户端 | 接入方式 | 说明 |
| --- | --- | --- |
| ZCode | 一键写入 Provider | Provider 显示为简洁的 `Google`，内部兼容 ID 保持稳定。 |
| DeepSeek Harness | 一键合并配置 | 保留已有 Provider，并在修改前创建备份。 |
| Grok Build | 一键合并配置 | 可连接 Antigravity 或已启用的 Grok。 |
| OpenAI Codex | 一键写入自定义 Provider | 使用本机 OpenAI 兼容网关。 |
| Claude Code | 一键写入 LLM Gateway | 使用本机 Anthropic 兼容网关。 |
| Gemini CLI | 一键写入原生 Gemini 端点 | 仅在 Antigravity 提供商下启用。 |
| Qwen Code / Kimi Code | 一键合并配置 | 不删除用户已有配置。 |
| OpenCode | 一键合并 Provider | 可随当前提供商同步模型。 |

程序只管理自己创建的配置项，并会在覆盖前创建有上限的备份。通用 OpenAI / Anthropic 客户端也可从
“Agent 接入”页复制当前本地配置。

## 下载

前往 [V1.2.0 正式版 Release](https://github.com/YShiLMu/ZCode-Antigravity/releases/tag/v1.2.0) 下载：

| 文件 | 用途 | 推荐场景 |
| --- | --- | --- |
| `ZCode-Antigravity-Setup-v1.2.0.exe` | Windows 图形化安装器（内嵌完整载荷） | Windows 用户首选 |
| `SHA256SUMS-v1.2.0.txt` | 包内全部文件的 SHA-256 | 下载后完整性校验 |
| Source code (zip / tar.gz) | 与发布标签对应的源码快照（GitHub 自动附带） | 审计与二次开发 |

V1.2.0 暂未构建 macOS 安装包；macOS 用户可使用上游 [V1.1.0 的 macOS 包](https://github.com/Hhz0823/ZCode-Antigravity/releases/tag/v1.1.0)（不含本 fork 的新模型与兼容性修复），或从源码构建。

## 快速开始

### macOS

macOS 安装包未随 V1.2.0 发布，请先从上游 Release 获取 V1.1.0 的 macOS 包（步骤如下），
或参考文末「从源码构建」。

1. 完整解压 Universal ZIP，在 `Terminal Tools` 中运行 `Verify-Package.command` 校验安装包。
2. 首次运行时按住 Control 点击 `ZCode Antigravity.app` 并选择“打开”。当前 App 使用临时签名，尚未公证。
3. 登录 Antigravity，点击“一键接入 ZCode”，然后完全退出并重新打开 ZCode。
4. 在 ZCode 中选择 Provider `Google` 和目标 Gemini 模型；需要联网时选择 `Gemini Web Search (Google)`。

关闭主窗口后，菜单栏仍会保留一个小图标。单击即可查看额度与 Token 小组件，再点窗口外部会自动收起。

### Windows

1. 从系统托盘完全退出 ZCode；仅关闭主窗口可能仍会留下 `ZCode.exe`。
2. 校验 SHA-256 后运行 `ZCode-Antigravity-Setup-v1.2.0.exe`。当前安装器未使用商业代码签名证书。
3. 登录 Antigravity 并点击“一键接入 ZCode”。程序会自动探测 Windows 系统代理与 v2rayN，无需强制开启 TUN；新版 ZCode 只使用 `provider_config.json` 也已完整支持。
4. 重新打开 ZCode，选择 Provider `Google` 和目标模型；系统托盘图标可随时打开额度小组件。

如需 Grok，在设置中启用 Grok 后完成 xAI 设备授权。Windows 会在软件内显示临时验证码；把它输入官方
授权页后，程序会自动轮询授权结果，无需粘贴回调内容。

两端均会在启动后自动检查 GitHub 正式版，之后每 6 小时检查一次。可在“设置 → 软件更新”手动检查，
或开启自动安装。为避免 Provider 和网关文件被占用，安装更新前请先完全退出 ZCode；程序会验证下载包
的文件名、平台、大小和 GitHub SHA-256，更新后自动重启并重新同步网关。

## 网络出口与本地端口

| 服务 | 默认地址 | 端口占用时 |
| --- | --- | --- |
| 本地模型网关 | `127.0.0.1:18080` | 自动扫描 `18081–18180` |
| OAuth callback | `127.0.0.1:51121` | 自动扫描 `51122–51221` |
| 控制中心 | `127.0.0.1:18200–18250` | 在范围内选择可用端口 |

Windows 自动网络顺序为：手动 `proxyURL` → 当前用户系统代理 → v2rayN mixed / SOCKS5
`127.0.0.1:10808` → 旧版 HTTP `10809` → 直连。macOS 可使用系统 TUN、`HTTP_PROXY` /
`HTTPS_PROXY`，或 App 资源中的 `settings.json`。所有代理均应指向可信的本机服务。

## 额度、Token 与状态栏小组件

- Antigravity 展示 5 小时和每周剩余额度；Grok 展示上游 Grok Build billing 返回的共享周期额度。
- 额度每 5 分钟自动刷新，手动刷新、切换提供商和完成接入会立即刷新；失败时保留上次成功数据。
- Token 统计来自本地网关真实 usage，显示最近输出、推理 Token、生成速度和本地累计输出。
- 不保存提示词或模型回复。上游未提供某类额度时会明确显示“未提供”，不会用本地 Token 估算账号余量。

## 隐私与安全

- API、OAuth callback 与管理路由只监听 loopback，不开放远程管理。
- Windows 使用当前用户 DPAPI 保护 OAuth token；macOS 使用登录钥匙串中的随机主密钥进行 AES-256-GCM 加密。
- ZCode 配置只保存随机本地网关密钥，不写入 Google / xAI token。
- 修改 ZCode 或 Agent 配置前自动备份；停止 Bridge 不会删除账号或聊天记录。
- 发布包不包含 OAuth token、账号 JSON、本地 API key、日志或用户的 ZCode 配置。
- 背景模糊由系统 / Electron 合成器完成，程序不会读取、缓存或上传窗口后方像素。

## 已验证范围

V1.2.0（本 fork）的验证范围：

- Go Core（管理器与固定 CLIProxyAPI 源码）的 `go build`、`go vet` 与全量单元测试通过，含新增的
  无旧版 `config.json` 同步 / 重复同步 / Provider 删除 / 状态读取回归测试。
- Windows x64 安装器由源码本机构建，校验和见 Release 资产 `SHA256SUMS-v1.2.0.txt`。
- 控制中心复用上游 V1.1.0 已验证的 Electron 包（内嵌 OAuth 桌面凭据无法从公开源码重建），与
  V1.2.0 Core 的接口已验证兼容；前端版本号显示仍为 1.1.0，属预期。
- 已在一台只使用 `provider_config.json` 的新版 ZCode 环境完成真实接入验证。
- Gemini 4 Argon 的真实账号调用尚未实测（需要具备相应订阅权限的账号）；注入与回退逻辑由单元测试覆盖。

上游 V1.1.0 的完整发布验证记录（双平台构建、OAuth、加密存储等）见
[`VERIFICATION.md`](VERIFICATION.md)。

## 从源码构建

源码包含 SwiftUI / AppKit macOS 客户端、Electron / React Windows 客户端、Go 本地 Core、测试、
固定的 CLIProxyAPI v7.2.132 源码与可重放补丁。GitHub 源码不内置 Google OAuth 桌面应用凭据；
开发者需要使用自己有权使用的配置：

```bash
export ANTIGRAVITY_OAUTH_CLIENT_ID='<your-client-id>'
export ANTIGRAVITY_OAUTH_CLIENT_SECRET='<your-client-secret>'
```

macOS Universal 构建：

```bash
cd project/packaging/macos
./Build-Universal.sh
```

Windows Electron UI 构建：

```bash
cd project/native/windows/ui
npm ci
npm run test:electron
npm run package:windows
```

构建脚本默认拒绝生成缺少 OAuth 配置的可登录发布包。开发规范和提交要求见
[`CONTRIBUTING.md`](CONTRIBUTING.md)。

## 常见问题

### 为什么 Gemini 没有联网搜索？

新版保留 Gemini 4 Argon / 3.8 / 3.7，旧的 Google Search 别名使用 Gemini 3.1，已一并移除。
需要最新网页信息时使用客户端提供的搜索工具。

### 为什么我的账号里没有 Gemini 4 Argon？

Gemini 4 Argon 需要相应的 Google 订阅或 API 权限。未开通的账号不会注入该模型，
同步自动回退到 Gemini 3.8 / 3.7；开通后在控制中心重新同步即可使用。

### 新添加的 Google 账号无法使用模型？

打开 Antigravity 并登录同一账号，尝试发送一条消息。若 Google 要求身份或年龄验证，
在官方页面由本人完成，再返回控制中心刷新额度。普通网络、资格或额度错误不会一律判定为验证要求。

### 为什么显示 401、403、429 或模型不可用？

先确认当前提供商、账号授权和网络出口。403 / 429 通常属于上游资格、风控或额度边界；本项目不会绕过验证。

### 为什么系统提示未知发布者？

当前 Windows 安装器未使用商业代码签名，macOS App 使用临时签名且未公证。请先核对 Release 中的
`SHA256SUMS-v1.2.0.txt`，哈希完全一致后再运行；不要全局关闭系统安全机制。

## 商标与免责声明

黄色袋鼠与渐变四芒星为本项目原创组合标识，并非美团、Google 或 xAI 官方联合标识。相关名称和商标
归各自权利人所有。本项目仅用于研究、开发与测试，使用者应自行确认服务条款、账号政策、网络规则和
当地法律要求，并承担使用未公开接口可能带来的兼容性与账号风险。
