# 服务器优选工具

一个轻量、高效的 Cloudflare Workers 订阅优选与生成工具。内置现代化 iOS 风格 Web 交互界面，支持一键解析节点并生成多客户端订阅。

GitHub 项目: [https://github.com/Gkun233/yx-auto](https://github.com/Gkun233/yx-auto)

---

## ✨ 主要功能

- **🚀 节点一键智能解析**：支持粘贴 `vless://`、`vmess://`、`trojan://` 节点链接，自动识别提取 UUID/密码、域名、Path 路径，并自动联动切换协议与传输方式。
- **🌐 多协议支持**：
  - **VLESS**
  - **Trojan**
  - **VMess**（已修复节点中文名 Base64 编码引发的 1101 报错）
- **⚡ 多传输协议**：
  - **WebSocket (WS)**：通用性强，兼容全协议。
  - **XHTTP (SplitHTTP)**：支持 VLESS 协议，内置 `auto` / `packet-up` / `stream-up` / `stream-one` 四种模式，并支持自定义 Extra JSON 参数。
- **🔒 ECH (Encrypted Client Hello)**：支持开启 ECH 增强抗干扰，可自定义 DoH 解析服务器及 ECH 域名（开启时自动启用纯 TLS 节点）。
- **🎯 纯净优选（不含原生地址）**：已去除 Worker 默认原生域名节点的生成，仅保留经过优选的高质量节点。
- **📡 丰富的优选数据源**：
  - **内置优选域名**：内置精选的常用高质量 Cloudflare 优选域名。
  - **动态优选 IP**：按电信/联通/移动/多线/IPv6 动态获取最新测速 IP。
  - **自定义 GitHub / API**：支持自定义 GitHub 仓库或第三方测速 API（自动兼容 TXT / CSV 格式及多种编码）。
- **🔍 精细化节点筛选**：
  - **仅 TLS 节点**：支持过滤非 TLS 端口（如 80 端口），仅保留加密端口。
  - **IP 版本**：支持分别开启/关闭 IPv4 与 IPv6。
  - **运营商线路**：支持单独筛选电信、联通、移动线路。
- **📱 广泛的客户端支持**：
  - 支持直接生成并唤醒客户端：**Clash**、**Stash**、**Surge**、**Sing-Box**、**Loon**、**Quantumult X**、**V2Ray**、**V2RayNG**、**NekoRay**、**Shadowrocket**。

---

## 🛠️ 部署指南

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)，进入 **Workers & Pages**。
2. 点击 **Create Application** -> **Create Worker**，输入名称并点击 **Deploy**。
3. 点击 **Edit Code**，将本项目的 `_worker.js` 代码完整复制粘贴进去。
4. 点击右上角 **Deploy**（保存并部署）即可。

*(可选) 如需自定义订阅转换后端，可在 Worker 的 **Settings -> Variables** 中添加环境变量 `scu`，填入自定义的 Subconverter 地址。*

---

## 📖 使用方法

直接在浏览器中访问你部署好的 Worker 域名：

1. **自动解析（推荐）**：在顶部输入框粘贴现有的节点链接，点击 **“解析”**，页面会自动填入域名、UUID、路径并切换匹配的协议与传输方式。
2. **手动配置**：按需勾选优选域名、优选 IP、协议（VLESS/Trojan/VMess）、传输方式（WS/XHTTP）、运营商与客户端。
3. **一键生成/导入**：在客户端列表中点击对应客户端按钮，即可直接一键唤醒客户端导入配置，或自动复制订阅链接到剪贴板。

---

## 🔗 订阅链接格式

订阅链接基础格式如下：
```text
https://your-worker.workers.dev/{UUID}/sub?domain=your-domain.com&epd=yes&epi=yes&egi=yes
```

可以通过 `&target=` 参数直接指定客户端配置格式：
- `base64` - 默认格式（通用节点链接列表，Base64 编码）
- `clash` / `clashr` - 生成 Clash YAML 配置
- `surge` - 生成 Surge 托管配置
- `quantumult` / `quanx` - Quantumult X 格式

---

## ⚙️ URL 参数详解

所有配置均可通过 URL 参数灵活控制，无需更改 Worker 源码：

| 参数 | 说明 | 默认值 / 可选值 | 示例 |
| :--- | :--- | :--- | :--- |
| `domain` | 你的节点域名 / 反代 SNI（**必填**） | - | `domain=example.com` |
| `path` | 自定义节点 Path 路径 | `/` | `path=%2Fcustom-path` |
| `transport` | 传输方式 | `ws` / `xhttp` | `transport=xhttp` |
| `xhttpMode` | XHTTP 模式（仅 XHTTP 生效） | `auto` / `packet-up` / `stream-up` / `stream-one` | `xhttpMode=auto` |
| `xhttpExtra`| XHTTP Extra 参数（JSON 字符串） | - | `xhttpExtra=%7B%22mode%22%3A%22auto%22%7D` |
| `ev` | 启用 VLESS 协议 | `yes` / `no`（默认 `yes`） | `ev=yes` |
| `et` | 启用 Trojan 协议（仅限 WS 传输） | `yes` / `no`（默认 `no`） | `et=yes` |
| `mess` | 启用 VMess 协议（仅限 WS 传输） | `yes` / `no`（默认 `no`） | `mess=yes` |
| `epd` | 启用内置优选域名 | `yes` / `no`（默认 `yes`） | `epd=yes` |
| `epi` | 启用动态优选 IP | `yes` / `no`（默认 `yes`） | `epi=yes` |
| `egi` | 启用 GitHub 优选 / API 优选 | `yes` / `no`（默认 `yes`） | `egi=yes` |
| `piu` | 自定义优选 IP 来源 URL 或 API 接口 | 默认内置仓库 | `piu=https://raw.github.com/...` |
| `dkby` | 仅保留 TLS 节点（过滤 80 端口等非 TLS） | `yes` / `no`（默认 `no`） | `dkby=yes` |
| `ech` | 启用 ECH（开启时自动强制仅 TLS） | `yes` / `no`（默认 `no`） | `ech=yes` |
| `customDNS` | ECH 专用的 DoH DNS 解析地址 | 默认阿里 DNS | `customDNS=https%3A%2F%2Fdns.alidns.com%2Fdns-query` |
| `customECHDomain` | ECH 目标域名 | `cloudflare-ech.com` | `customECHDomain=cloudflare-ech.com` |
| `ipv4` | 启用 IPv4 节点 | `yes` / `no`（默认 `yes`） | `ipv4=yes` |
| `ipv6` | 启用 IPv6 节点 | `yes` / `no`（默认 `yes`） | `ipv6=yes` |
| `ispTelecom` | 启用电信优选节点 | `yes` / `no`（默认 `yes`） | `ispTelecom=yes` |
| `ispUnicom` | 启用联通优选节点 | `yes` / `no`（默认 `yes`） | `ispUnicom=yes` |
| `ispMobile` | 启用移动优选节点 | `yes` / `no`（默认 `yes`） | `ispMobile=yes` |
| `target` | 输出订阅格式 | `base64` / `clash` / `surge` / `quanx` | `target=clash` |

---

## ⚠️ 注意事项

1. **用途声明**：本工具仅为优选测速 IP 节点合并及订阅格式生成工具，**不提供任何代理服务与服务器流量转发**。
2. **服务器配合**：生成的节点需配合您自己拥有的服务器与正确的反代域名使用。
3. **原生地址已移除**：生成的节点列表中不再包含 Worker 自带的原生域名，防止原生域名受阻影响订阅节点质量。
4. **XHTTP 协议限制**：XHTTP 传输方式目前仅支持 VLESS 协议，启用 XHTTP 时会自动禁用 Trojan / VMess。
5. **VMess 参数说明**：VMess 参数采用 `mess` 而非 `vm`，以避免在部分网络环境下被敏感关键词拦截。
