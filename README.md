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
