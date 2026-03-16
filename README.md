# cf-net

基于 Cloudflare Worker 的代理服务，支持 VLESS / Trojan / XHTTP 协议，内置 Web 管理面板和多客户端订阅。

## 功能

- **多协议** — VLESS (WebSocket)、Trojan (SHA224)、XHTTP (HTTP POST 伪装，需绑定自定义域名 + gRPC)
- **多客户端订阅** — Clash、Surge、V2Ray、Quantumult X、Shadowrocket、Loon、SingBox，自动识别 User-Agent
- **Web 管理面板** — 协议开关、ProxyIP 管理、优选 IP、延迟测试、SOCKS5 配置、高级控制
- **KV 持久化** — 配置通过 Cloudflare KV 存储，支持在线修改
- **智能优选** — 内置优选 IP/域名、GitHub 优选、自定义 URL 来源、地区智能匹配
- **ECH 支持** — Encrypted Client Hello，自动从 DoH 获取 ECH 配置
- **自定义首页** — 可设置 URL 伪装首页，默认显示终端风格页面

## 变量说明

| 变量 | 类型 | 说明 |
|------|------|------|
| `u` | Secret | UUID，用户标识 |
| `d` | KV/面板 | 自定义路径 |
| `p` | KV/面板 | 自定义 ProxyIP |
| `s` | KV/面板 | SOCKS5 配置 |
| `yx` | KV/面板 | 优选 IP 列表 |
| `yxURL` | KV/面板 | 优选 IP 来源 URL |
| `homepage` | KV/面板 | 自定义首页 URL（伪装） |
| `ae` | KV/面板 | 允许 API 管理（默认关闭） |
| `rm` | KV/面板 | 地区匹配开关 |
| `qj` | KV/面板 | 降级控制 |
| `dkby` | KV/面板 | TLS 控制 |
| `yxby` | KV/面板 | 优选控制 |
| `wk` | KV/面板 | 指定 Worker 地区 |

## 订阅格式

访问 `https://your-worker.dev/{UUID}/sub?target={client}` 获取订阅，支持的 `target`：

| 值 | 客户端 |
|----|--------|
| `clash` / `clashr` | Clash |
| `surge` / `surge2` / `surge3` / `surge4` | Surge |
| `v2ray` | V2Ray |
| `quantumult` / `quanx` | Quantumult X |
| `loon` | Loon |
| `ss` / `ssr` | Shadowsocket |
| `base64` | 通用 Base64 |

直接访问 `https://your-worker.dev/{UUID}` 会根据 User-Agent 自动返回对应格式。

## 部署（Wrangler CLI）

### 1. 安装

```bash
npm i -g wrangler
wrangler login
```

### 2. 配置环境

```bash
export CLOUDFLARE_ACCOUNT_ID="your-account-id"
```

### 3. 创建 KV

```bash
wrangler kv namespace create "C"
# 将返回的 id 填入 wrangler.toml
```

### 4. 设置 UUID

```bash
wrangler secret put u
# 输入你的 UUID
```

### 5. 部署

```bash
wrangler deploy
```

### 6. 验证

```bash
wrangler tail
```

## 本地开发

```bash
cp .dev.vars.example .dev.vars  # 填入 u=your-uuid
wrangler dev
```

## 管理面板

部署后访问 Worker 根路径，通过终端界面输入 UUID 进入管理面板，可在线配置：

- 协议开关（VLESS / Trojan / XHTTP）
- Trojan 自定义密码
- ProxyIP / SOCKS5
- 优选 IP 及延迟测试
- 订阅转换地址
- 首页伪装 URL
- ECH / DNS 配置

## 脱敏说明

`wrangler.toml` 不包含任何账户标识或密钥，所有敏感值通过环境变量或 `wrangler secret` 注入。
